# Wayfinder — Claude plugin

Use [Wayfinder](https://mo-wayfinder.fly.dev) — a retirement & life‑scenario planner — from **Claude Code**. This plugin bundles Wayfinder's Agent Skills **and** wires up its hosted MCP server, so Claude can build, stress‑test, and advise on your plans (life‑paths + Monte Carlo) with live data from your account.

> This is a thin, public distribution repo. The engine, app, and infrastructure live in a separate private repo; only the safe playbooks (skills) and the public connector config are published here.

## Install (Claude Code)

```
/plugin marketplace add Monster-Orange/wayfinder-plugin
/plugin install wayfinder
```

That gives you:

- **Skills** — guided playbooks:
  - `wayfinder-create-plan` — build a plan from a household's situation.
  - `wayfinder-create-scenarios` — add what‑if scenarios (higher spend, weaker returns, a crash…).
  - `wayfinder-retirement-advising` — holistic advice: stay‑put baseline vs. downsize / relocate / nomad, Monte Carlo, income floor.
- **MCP server** — the hosted Wayfinder server (`wayfinder`), so the skills work on your **real** plans. On first use, Claude walks you through a one‑time sign‑in to your Wayfinder account (OAuth); every tool is scoped to you.

Then just ask, e.g. *"Build me a Wayfinder plan for a couple retiring at 60"* or *"What should I do — downsize or relocate?"*

## Using Claude.app (Desktop / claude.ai) instead?

Plugins are a Claude Code feature. In the Claude chat apps, connect Wayfinder as a **custom connector** instead:

**Settings → Connectors → Add custom connector →** paste the connector URL from your Wayfinder app's **Integrations** page **→ sign in → Allow.** The same skills come along automatically.

## Links

- App: https://mo-wayfinder.fly.dev
- MCP endpoint: `https://mo-wayfinder.fly.dev/mcp`
- Maintainer: Ward Vuillemot · ward@wardvuillemot.com

*Wayfinder is a planning sandbox — not financial advice.*
