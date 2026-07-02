---
name: wayfinder-create-plan
description: Create a new Wayfinder retirement/financial plan from a household's situation (people, accounts, home, income, spending). Use when the user wants to set up, start, or build a new Wayfinder plan. Requires the Wayfinder MCP server (tools named mcp__wayfinder__*).
---

# Create a Wayfinder plan

Build a new, saved plan from what the user tells you about their household, using the Wayfinder MCP tools. A plan is the baseline ledger everything else (life-paths, scenarios, Monte Carlo) branches from — so capture today's reality accurately.

## 1. Gather the inputs
Ask for whatever you don't already have. Don't over-ask — sensible defaults are fine; note any you assume.
- **People** (0+): for each, a `label` (name) and `birth` year. Optionally working income (`semi_income`) and the age range they earn it (`semi_start`/`semi_end`), Social Security (`ss_annual`, `ss_claim_age`).
- **Accounts**: `cash`, `k401` (401k/IRA/pre-tax), `roth`.
- **Home** (optional): `value`, `basis`, annual `appr` (as a fraction, e.g. 0.025), `mortgage`, `mortgage_rate`, `mortgage_years_left`.
- **Spending**: `spend_base` (total annual), `essential_spend` (the floor you must always cover).
- **Horizon** (optional): `base_year` (default current year), `end_age` (plan-to age).

## 2. Create it
Call **`create_plan`** with a unique `name` and a `spec` built from the above:

```
create_plan(name="Jane & John Doe", spec={
  "people": [{"label": "Jane", "birth": 1970, "ss_annual": 36000, "ss_claim_age": 70},
             {"label": "John", "birth": 1972}],
  "accounts": {"cash": 300000, "k401": 800000, "roth": 120000},
  "home": {"value": 900000, "mortgage": 150000, "mortgage_rate": 0.035},
  "spend_base": 85000, "essential_spend": 55000, "end_age": 95
})
```

- Percentages are **fractions** (3.5% → 0.035).
- If `create_plan` says the name already exists, pick another name or use `update_plan`.
- New plans are **stay-put** by default (no life-paths or scenarios applied yet) — that's intentional; the baseline is "what happens if nothing changes."

## 3. Confirm and hand off
Read back the returned headline (net worth now, money left at the end, earliest depletion). Then offer the natural next steps:
- "Want me to **analyze** it?" → the **wayfinder-retirement-advising** skill.
- "Want to explore **what-ifs**?" → the **wayfinder-create-scenarios** skill.

Never invent financial facts — if you assumed a default (return, inflation, SS), say so plainly.
