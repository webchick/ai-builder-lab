---
title: Dependency Scout
description: A Temporal-powered agent that triages Dependabot and Renovate PRs with supply-chain-aware security checks.
date: 2026-05-15
tags: [security, temporal, dependabot, supply-chain]
featured: true
---

## What it is

[Dependency Scout](https://github.com/temporal-community/dependency-scout) is a durable, supply-chain-aware agent that gives every dependency PR a **data-backed second opinion** before it merges.

You have 47 unreviewed Dependabot PRs. It's midnight, CI is green, and you've merged dozens of these before. Maintainers aren't careless — they're exhausted. Modern supply-chain attacks are designed to slip past smart humans under impossible workloads.

This tool exists for that moment.

## What it checks

Before classifying a PR as 🟢 GREEN, 🟡 YELLOW, or 🔴 RED, the Scout runs checks across:

- **Known vulnerabilities** — OSV database (including OpenSSF malicious-packages)
- **Supply chain score** — Socket.dev for obfuscated code, install-time scripts, typosquatting
- **What code actually changed** — diffs package archives; flags new binaries, install hooks, network calls, git-URL deps
- **Release freshness** — very fresh releases (< 24h) and recent ones (< 7 days) get extra scrutiny
- **Maintainer changes** — new accounts publishing popular packages
- **Build provenance** — SLSA attestations, dropped tag signing, re-release patterns
- **Repo health** — OpenSSF Scorecard signals
- **Zombie packages** — deprecated packages and patches to abandoned major lines
- **Suspicious PR files** — CI scripts or Dockerfiles hiding in a "routine dep bump"

It posts a comment explaining its reasoning and can auto-merge GREEN PRs or close RED ones — depending on your config. **15 ecosystems** covered: pip, npm, RubyGems, Maven, NuGet, Cargo, Go, Composer, and more.

## How it works

Built on **[Temporal](https://temporal.io)** for durable workflows — checks survive restarts, retry cleanly, and scale across a PR queue. A rule-based classifier runs entirely locally with zero API keys; add Claude, OpenAI, or Ollama for smarter classification.

Quick start:

```bash
git clone https://github.com/temporal-community/dependency-scout
cd dependency-scout
uv run python setup.py

# Terminal 1
temporal server start-dev

# Terminal 2
uv run python -m worker

# Terminal 3 — triage a PR
uv run dependency-scout triage https://github.com/your-org/your-repo/pull/123
```

You can also vet a package *before* installing it (`dependency-scout check requests 2.32.0`) or wire it up as an MCP tool so Claude Code checks deps before running `pip install`.

## What's next

- Webhook mode for continuous triage on every new Dependabot/Renovate PR
- Broader registry signal coverage per ecosystem
- Tighter defaults for auto-merge thresholds and prompt-injection hardening
