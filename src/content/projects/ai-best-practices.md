---
title: AI Best Practices for Drupal
description: Community-maintained context engineering for Drupal — agent skills, AGENTS.md sync, and eval-driven development.
date: 2026-03-26
tags: [drupal, skills, evals, context-engineering]
featured: true
---

## What it is

[AI Best Practices for Drupal](https://www.drupal.org/project/ai_best_practices) is the canonical, community-maintained initiative for helping AI coding agents behave like experienced Drupal developers — not generic LLMs guessing at APIs from 2023 training data.

The project ships **authoritative agent skills** (following the [agentskills.io](https://agentskills.io) standard), a **Composer plugin** that syncs them into your project, and an **eval harness** so you can measure whether your agent setup actually works before you trust it in CI.

Source: [git.drupalcode.org/project/ai_best_practices](https://git.drupalcode.org/project/ai_best_practices)

## What I built

The core idea is **context engineering**: instead of one giant system prompt, you give agents small, focused skills they can load on demand — Views, JSON:API, SDC, testing patterns, contrib management, and more.

Key pieces:

- **`skills/`** — one directory per skill, validated against the agentskills.io spec
- **`evals/`** — paired eval assets per skill for eval-driven development
- **Composer plugin** — `composer drupal-ai skills-sync` merges discovered skills into your project and keeps root `AGENTS.md` in sync via managed regions (so your own notes outside the markers stay intact)
- **`ai_best_practices.yaml`** — controls skill discovery and output directory (default `.agents/skills`)

Install with:

```bash
composer require drupal/ai_best_practices
composer drupal-ai skills-sync
```

## What I learned

- **Skills beat monolithic prompts.** Agents perform better when they pull in 2–3 relevant skills for a task instead of drowning in everything at once.
- **Evals are the missing half.** Shipping skills without measuring agent performance is vibes-based development. The 3-layer test harness (deterministic checks + semantic scoring) catches regressions when you tweak prompts or swap models.
- **Distribution matters as much as content.** A great skill nobody can install is useless. The Composer plugin + `AGENTS.md` sync means any Drupal project gets consistent context automatically on `composer update`.

## What's next

- Expand skill coverage across more contrib modules and common site-builder workflows
- Tighten eval gates for CI pipelines (hard fail when agent quality drops below threshold)
- Continue consolidating overlapping community efforts — several repos were solving similar problems; this is meant to be the shared home
