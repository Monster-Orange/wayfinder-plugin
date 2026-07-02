# Wayfinder — Claude plugin

Use [Wayfinder](https://mo-wayfinder.fly.dev) — a retirement & life‑scenario planner — from Claude. This plugin bundles Wayfinder's Agent Skills **and** its hosted MCP connector, so one install gives Claude the skills **and** the live connection to your account (build, stress‑test, and advise on plans — life‑paths + Monte Carlo).

> Thin, public distribution repo. The engine, app, and infrastructure stay in a separate private repo; only the safe playbooks (skills) and the public connector config are published here.

## Install in the Claude app (desktop or web)

Plugins need a paid Claude plan (Pro, Max, Team, or Enterprise).

1. Open **Customize → Plugins**.
2. Under **Personal plugins**, click **+ → Add marketplace → Add from a repository**.
3. Enter this repo: **`Monster-Orange/wayfinder-plugin`** (or `https://github.com/Monster-Orange/wayfinder-plugin`).
4. Install **Wayfinder**. On first use Claude walks you through a one‑time sign‑in to your Wayfinder account (OAuth); every tool is scoped to you.

## Install in Claude Code

```
/plugin marketplace add Monster-Orange/wayfinder-plugin
/plugin install wayfinder
```

## What you get

- **Skills** — guided playbooks:
  - `wayfinder-create-plan` — build a plan from a household's situation.
  - `wayfinder-create-scenarios` — add what‑if scenarios (higher spend, weaker returns, a crash…).
  - `wayfinder-retirement-advising` — holistic advice: stay‑put baseline vs. downsize / relocate / nomad, Monte Carlo, income floor.
- **MCP connector** — the hosted Wayfinder server (`wayfinder`), so the skills work on your **real** plans.

Then just ask, e.g. *"Build me a Wayfinder plan for a couple retiring at 60"* or *"What should I do — downsize or relocate?"*

## Links

- App: https://mo-wayfinder.fly.dev
- MCP endpoint: `https://mo-wayfinder.fly.dev/mcp`
- Maintainer: Ward Vuillemot · ward@wardvuillemot.com

*Wayfinder is a planning sandbox — not financial advice.*
