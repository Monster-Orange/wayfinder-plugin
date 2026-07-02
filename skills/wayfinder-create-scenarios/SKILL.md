---
name: wayfinder-create-scenarios
description: Add what-if scenarios (e.g. higher spending, weaker returns, retiring later, a market crash) to an existing Wayfinder plan so they're compared against the baseline across every life-path and Monte Carlo. Use when the user wants to explore alternatives or stress a Wayfinder plan. Requires the Wayfinder MCP server.
---

# Add scenarios to a Wayfinder plan

A **scenario** is a named override on the baseline's assumptions — `{name, overrides: {param: value}}`. Wayfinder then runs every scenario against every life-path (and Monte Carlo), so the user can see how each what-if changes the odds. Use the Wayfinder MCP tools.

## 1. Find the plan
`list_plans` to get names; confirm which plan. If unsure of its current numbers, `get_plan(name)`.

## 2. Translate the what-if into overrides
Each scenario overrides one or more engine params. Common ones (percentages are **fractions**):
- `spend_base` — total annual spending (e.g. "spend $15k more" → baseline + 15000)
- `essential_spend` — the must-cover floor
- `nominal_return` — market return (e.g. a bear market → 0.04)
- `inflation` — e.g. high-inflation regime → 0.05
- `sigma` — market volatility
- `semi_end_age` — work longer / stop earlier
- `end_age` — plan to a different age (longevity)
- `new_home` — downsize target price

Name scenarios for humans: "Higher spend", "Bear market", "Retire at 62", "Live to 100".

## 3. Save them (replace the whole list)
Scenarios live as `params.scenarios`. `update_plan` **merges keys but replaces the `scenarios` array wholesale**, so include every scenario you want kept:

```
update_plan(name="Jane & John Doe", changes={"scenarios": [
  {"name": "Higher spend", "overrides": {"spend_base": 100000}},
  {"name": "Bear market",  "overrides": {"nominal_return": 0.04}},
  {"name": "Work to 70",   "overrides": {"semi_end_age": 70}}
]})
```

To add one, fetch the plan's existing scenarios first (or ask the user) and re-send the full list. To clear them, send `{"scenarios": []}`.

## 4. Show the effect
After saving, run the **wayfinder-retirement-advising** skill (or at least `plan_monte_carlo` and `compare_lifepaths`) so each scenario's money-left and success rate are visible. Call out which what-ifs break the plan (run dry) and which it survives.
