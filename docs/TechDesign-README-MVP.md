# Technical Design Document: README MVP

## How We'll Build It

### Recommended Approach: GitHub Actions-Native Dashboard

Based on your requirement for a "set it and forget it" minimalist dashboard that lives entirely in one repo, the optimal path is using GitHub Actions to fetch data and commit SVGs directly to your repository.

**Primary Recommendation: `lowlighter/metrics` + `github-readme-stats**`

- **Why it's perfect for you:** It leverages GitHub's infrastructure for free, requires no external servers, and supports highly customizable, minimalist SVG outputs.
- **What it costs:** $0 (GitHub Actions free tier).
- **Time to learn:** ~2 hours to configure YAML workflows.
- **Limitations to know:** GitHub’s "Camo" image proxy might cache images for a few minutes, but your daily cron job will ensure consistency over 24-hour cycles.

### Alternative Options Compared

| Option                 | Pros                          | Cons                             | Cost       | Time to MVP |
| ---------------------- | ----------------------------- | -------------------------------- | ---------- | ----------- |
| **Actions + Commits**  | 100% Reliable; No rate limits | Slightly more complex YAML       | $0         | 2 days      |
| **Live External URLs** | Instant updates               | High risk of rate limiting       | $0         | 1 day       |
| **Vercel Functions**   | Total design control          | Requires managing external infra | $0 (Hobby) | 4 days      |

---

## Building Your Features

### Feature 1: Core Stats Layer

**Complexity:** Easy

**Implementation:**

1. **Metric Selection:** Use `lowlighter/metrics` to generate `assets/metrics.svg`.
2. **Stat Cards:** Embed `github-readme-stats` for specific language and rank breakdowns.
3. **Automation:** Configure a workflow to commit the generated SVG back to the `/assets` folder daily.

### Feature 2: Minimalist Dashboard Layout

**Complexity:** Medium (HTML-in-Markdown)

**Implementation:**

1. **Grid System:** Use a `<table>` with `width="50%"` to create two distinct columns.
2. **Constraints:** Ensure all attributes are sanitized (no `class`, no external `<style>`).
3. **Alignment:** Use `valign="top"` to keep content aligned correctly as cards vary in height.

---

## AI Assistance Strategy

| Task               | Best AI Tool | Example Prompt                                                                       |
| ------------------ | ------------ | ------------------------------------------------------------------------------------ |
| **Workflow Logic** | Claude       | "Write a GitHub Action that fetches stats and commits an SVG to the assets/ folder." |
| **HTML Layout**    | v0 / ChatGPT | "Create a 2-column HTML table layout for a GitHub README that works on mobile."      |
| **Custom Styling** | Cursor       | "Adjust the hex colors in this SVG generator URL to match `#58A6FF`."                |

---

## Deployment & Maintenance

### Recommended Platform: GitHub Actions

- **Workflow:** `.github/workflows/metrics.yml` will handle the daily heavy lifting.
- **Persistence:** By committing SVGs to the repo, the README always has a "fallback" image even if external APIs go down temporarily.

### Cost Breakdown

| Service        | Free Tier     | You Need             |
| -------------- | ------------- | -------------------- |
| GitHub Actions | 2,000 mins/mo | ~30 mins/mo (Free)   |
| GitHub Storage | 500MB         | ~5MB for SVGs (Free) |
| **Total**      |               | **$0/mo**            |

---
