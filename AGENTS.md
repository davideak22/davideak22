# AGENTS.md — Master Plan for README MVP

## Project Overview

**App:** README
**Goal:** Create a visually stunning, automated, and minimalist GitHub profile dashboard that updates daily.
**Stack:** GitHub Actions, lowlighter/metrics, github-readme-stats, sanitized HTML/Markdown.
**Current Phase:** Phase 2 — Core Features ✅

## How I Should Think

1. **Understand Intent First**: Before answering, identify what the user actually needs (e.g., a "set it and forget it" brand presence).
2. **Ask If Unsure**: If critical info like specific hex colors or GitHub handles is missing, ask before proceeding.
3. **Plan Before Coding**: Propose a plan (Antigravity Plan Artifact), ask for approval, then implement.
4. **Verify After Changes**: Run tests/linters or manual checks after each change.
5. **Explain Trade-offs**: When recommending a plugin, mention the impact on SVG size or rate limits.

## Plan → Execute → Verify

1. **Plan:** Outline a brief approach (e.g., which Metrics plugins to enable) and ask for approval.
2. **Plan Mode:** In Antigravity/Cursor, use Plan Mode to generate detailed Implementation Plans.
3. **Execute:** Implement one feature at a time (e.g., set up the `metrics.yml` before building the table).
4. **Verify:** Use the Antigravity Browser Preview or manual Markdown rendering checks after each feature.

## Current State

**Last Updated:** 2026-02-17
**Working On:** Polish & Launch
**Recently Completed:** Phase 2 — Core Features (metrics plugins, 2-column layout, typing header, stat cards)
**Blocked By:** `METRICS_TOKEN` secret must be added to GitHub repo settings

## Roadmap

### Phase 1: Foundation

- [x] Create repository `USERNAME/USERNAME`
- [x] Initialize `AGENTS.md` and `agent_docs/`
- [x] Set up `.github/workflows/` directory

### Phase 2: Core Features

- [x] **Feature 1:** `lowlighter/metrics` SVG integration (commit-to-repo strategy).
- [x] **Feature 2:** 2-column minimalist HTML table layout.
- [x] **Feature 3:** Animated Typing Header (readme-typing-svg).

## What NOT To Do

- Do NOT use `<style>` tags or Tailwind (GitHub strips them).
- Do NOT skip YAML linting for GitHub Actions.
- Do NOT add features like Spotify cards until Phase 2 is stable.
