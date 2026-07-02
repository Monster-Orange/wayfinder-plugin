---
name: wayfinder-retirement-advising
description: Give holistic retirement advice from an existing Wayfinder plan — read the stay-put baseline, weigh life-paths (downsize / relocate / nomad), run Monte Carlo, factor scenarios and the income floor, and recommend the single best path honestly. Use when the user wants analysis, advice, or "what should I do?" on a Wayfinder plan. Requires the Wayfinder MCP server.
---

# Retirement advising with Wayfinder

Help the user get the most out of a plan: not just numbers, but a clear, honest recommendation. Use the Wayfinder MCP tools in this order, then synthesize.

## 1. Read the baseline
`plan_insights(name)` — the core read. It returns the stay-put outlook (net worth now, money left, earliest depletion), **earliest retirement age**, the biggest **assumption levers** (what moves the outcome most), **life-path opportunities**, the **income-floor gap** (how much of essential spending guaranteed income covers), and starter recommendations. Ground everything else in this.

## 2. Weigh the life-paths
`compare_lifepaths(name)` — every coherent combination of **downsize / relocation / nomad** vs stay-put, best-first by money left, with `vs_baseline` and whether it `runs_dry`. These are *options the user could choose*, not predictions. (Downsize and nomad are mutually exclusive — nomad already implies selling the home.)

## 3. Test robustness with Monte Carlo
`plan_monte_carlo(name, n_sims=500)` — success rate for the baseline AND each life-path, simulated separately, most-robust first. **A higher deterministic "money left" with a lower success rate is worse** — prefer the path that survives the most randomized markets. Report success as a percentage.

## 4. Factor scenarios & the floor
- If the plan has **scenarios** (what-ifs), the Monte Carlo / grid already cover them — note which scenarios the recommended path still survives.
- The **income floor**: if guaranteed income (SS, pensions, annuities) doesn't cover essential spending, that gap is the real risk — call it out and quantify it.
- The biggest **levers** from step 1 are the cheapest knobs (e.g. spend less, work two more years, claim SS later) — surface the one or two with the most leverage.

## 5. Recommend — clearly and honestly
Give the user **one** primary recommendation: the single best path forward and the top lever to pull, with its success rate and money-left. Then:
- Name the main risk (what makes it run dry) and the early-warning sign.
- Offer one cheaper fallback if they don't want the primary move.
- Be honest about paths that fail — don't oversell. If nothing clears a comfortable success rate, say so and show what it would take.

Keep it plain-language and decision-oriented. If exploring a new what-if would help, use the **wayfinder-create-scenarios** skill; to spin up a fresh plan, **wayfinder-create-plan**.
