---
name: wayfinder-create-plan
description: Create a new Wayfinder retirement/financial plan from a household's situation (people, accounts, home, income, spending). Use when the user wants to set up, start, or build a new Wayfinder plan. Requires the Wayfinder MCP server (tools named mcp__wayfinder__*).
version: 1.0.0
---

# Create a Wayfinder plan

Build a new, saved plan from what the user tells you about their household, using the Wayfinder MCP tools. A plan is the baseline ledger everything else (life-paths, scenarios, Monte Carlo) branches from — so capture today's reality accurately.

## 1. Gather the inputs
**Three things are the user's to state and are never yours to invent: `people` (with birth years),
`accounts`, and `spend_base`.** If you do not have them, ASK. Do not fill them from a "sample
household" — a plan built on invented numbers looks exactly like a real one, and the user will read
its net worth, its depletion year and its shortfall as facts about their life.

This is also the difference between the guard firing and not: `create_plan` returns
`{"status":"needs_input", …}` only when the spec it RECEIVES is short. If you invent the values
before calling, the tool has nothing to ask about and the safeguard never runs.

Sensible defaults are fine for the genuinely optional modelling knobs — return, inflation, horizon —
and say plainly which ones you assumed.
- **People** (0+): for each, a `label` (name) and `birth` year. Optionally working income (`semi_income`) and the age range they earn it (`semi_start`/`semi_end`), Social Security (`ss_annual`, `ss_claim_age`).
- **Accounts**: `cash`, `k401` (401k/IRA/pre-tax), `roth`.
- **Home** (optional): `value`, `basis`, annual `appr` (as a fraction, e.g. 0.025), `mortgage`, `mortgage_rate`, `mortgage_years_left`.
- **Spending**: `spend_base` (total annual), `essential_spend` (the floor you must always cover).
- **Horizon** (optional): `base_year` (default current year), `end_age` (plan-to age).

## 2. Create it
Call **`create_plan`** with a unique `name` and a `spec` built from the above:

```
create_plan(name="Alex & Jordan", spec={
  "people": [{"label": "Alex", "birth": 1970, "ss_annual": 36000, "ss_claim_age": 70},
             {"label": "Jordan", "birth": 1972}],
  "accounts": {"cash": 300000, "k401": 800000, "roth": 120000},
  "home": {"value": 900000, "mortgage": 150000, "mortgage_rate": 0.035},
  "spend_base": 85000, "essential_spend": 55000, "end_age": 95
})
```

- Percentages are **fractions** (3.5% → 0.035).
- If `create_plan` comes back with `{"status": "needs_input", "missing": [...]}`, **nothing was created**. It is telling you the plan cannot be built without those facts — ask the user for exactly what `missing` names, then call `create_plan` again with the same name and the completed spec. Do not fill them in yourself: a plan built on assumed people, balances or spending looks exactly like a real one, and every answer it gives afterwards is about somebody else.
- **`missing: ["confirm"]` is different, and it is not a gap.** The spec is complete; nothing has been written because the user has not seen it yet. Show them **every value in `review`** — the people and their birth years, the balances, the yearly spending — and get a yes in their own words. Then call `create_plan` again with the same arguments plus `confirm=true`. Never send `confirm=true` on a spec you read back to nobody: that turns the one moment a person can catch an invented household into a formality. (Some clients ask the user directly instead, and you will simply get the headline back — that is the same check, done for you.)
- If `create_plan` says the name already exists, pick another name or use `update_plan`.
- New plans are **stay-put** by default (no life-paths or scenarios applied yet) — that's intentional; the baseline is "what happens if nothing changes."

## 3. Confirm and hand off
Read back the returned headline (net worth now, money left at the end, earliest depletion). Then offer the natural next steps:
- "Want me to **analyze** it?" → the **wayfinder-retirement-advising** skill.
- "Want to explore **what-ifs**?" → the **wayfinder-create-scenarios** skill.

Never invent financial facts — if you assumed a default (return, inflation, SS), say so plainly.
