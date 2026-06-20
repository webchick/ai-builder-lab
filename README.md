# ai-builder-lab

A place to stick all my fun AI projects.

Built with [Astro](https://astro.build) and Content Collections.

## Development

```bash
npm install
npm run dev
```

Open [http://localhost:4321](http://localhost:4321).

## Add a project

Create a markdown file in `src/content/projects/` with frontmatter. The collection is defined in `src/content.config.ts`.

```yaml
---
title: My Project
description: One-line summary
date: 2026-06-19
tags: [tag-one, tag-two]
featured: false
draft: false
---
```

## Deploy to Vercel

Push to GitHub and import the repo in Vercel. It auto-detects Astro — no adapter needed for this static site.
