# Product Requirements Document: README MVP

## Overview

**Product Name:** README
**Problem Statement:** Standard GitHub profiles are static and fail to showcase a developer's real-time activity and professional brand effectively.
**MVP Goal:** Create a visually stunning, automated, and minimalist GitHub profile dashboard that updates daily.
**Target Launch:** 7 Days.

## Target Users

### Primary User Profile

**Who:** Myself (Developer/Engineer).
**Problem:** Maintaining a high-end profile requires manual updates and deep knowledge of GitHub’s Markdown rendering constraints.
**Current Solution:** Basic Markdown files with static text.
**Why They'll Switch:** Automation allows for a "set it and forget it" approach to personal branding.

### User Persona: Developer

- **Demographics:** Software Engineer focused on brand growth.
- **Tech Level:** Intermediate (Path C) — comfortable with APIs and basic scripting but prefers streamlined solutions.
- **Goals:** Showcase coding habits, language stats, and project activity dynamically.
- **Frustrations:** GitHub's removal of `<style>` tags and external CSS makes high-end design difficult.

## User Journey

### The Story

The user starts with a blank repository named after their username. They integrate external SVG generators via URLs and set up GitHub Actions to handle data fetching. Every 24 hours, the profile refreshes itself with new stats, maintaining a "live" feel for any visitor.

### Key Touchpoints

1. **Repository Setup:** Creating the `USERNAME/USERNAME` profile repo.
2. **Dashboard Layout:** Building the core structure using HTML tables for a multi-column look.
3. **Stat Integration:** Connecting `github-readme-stats` and `metrics`.
4. **Automation:** Configuring `.github/workflows/` to run cron jobs.
5. **Success Moment:** Seeing a visually stunning dashboard that accurately reflects real coding activity.

## MVP Features

### Core Features (Must Have)

#### 1. Core Stats Layer

- **Description:** Integration of dynamic cards showing GitHub stats, top languages, and contribution streaks.
- **User Value:** Provides instant proof of technical activity and expertise.
- **Success Criteria:** Cards render correctly as SVGs and update daily.
- **Priority:** Critical.

#### 2. Minimalist Dashboard Layout

- **Description:** A 2-column or 3-column grid built using sanitized HTML `<table>` tags to circumvent Markdown limitations.
- **User Value:** Organizes dense information into a scannable, professional interface.
- **Success Criteria:** Layout remains stable on both desktop and mobile views.
- **Priority:** High.

#### 3. Animated Typing Header

- **Description:** A `readme-typing-svg` implementation to cycle through professional roles and value propositions.
- **User Value:** Grabs visitor attention immediately with motion.
- **Success Criteria:** Text cycles smoothly without broken URLs.
- **Priority:** Medium.

### Future Features (Not in MVP)

| Feature                         | Why Wait                                                      | Planned For |
| ------------------------------- | ------------------------------------------------------------- | ----------- |
| **Spotify Live Card**           | Requires additional Vercel deployment and auth                | Version 2   |
| **Social Media Bridge**         | Requires custom Python script for RSS/API parsing             | Version 2   |
| **Instagram/LinkedIn Scrapers** | Complexity involving session tokens and potential rate limits | Version 2   |

## Success Metrics

### Primary Metrics

1. **Visual Aesthetic:** Subjective "visually stunning" result that adheres to a minimalist theme.
2. **Daily Updates:** 100% success rate for GitHub Actions cron jobs (0 0 \* \* \*).

### Secondary Metrics

- **Visitor Count:** Incremental growth as tracked by the `github-profile-views-counter`.
- **Performance:** SVG load times under 2 seconds via GitHub Camo proxy.

## UI/UX Direction

**Design Feel:** Minimalist.
**Inspiration:** Dashboard-style layouts and data visualization.

### Key Sections

1. **Hero Section:** Typing SVG and visitor counter centered at the top.
2. **Stats Grid:** 2-column table with GitHub Stats on the left and Weekly Activity on the right.
3. **Footer/CTA:** Minimalist badges for social links and portfolio.

### Design Principles

- **Clarity:** Use bold headings and horizontal rules to separate data.
- **Consistency:** Pick 2–3 hex colors (e.g., `#58A6FF`) and apply them across all SVG generators.
- **Scannability:** Prioritize visual infographics over long walls of text.

## Technical Considerations

**Platform:** GitHub Profile (Special Repository).
**Responsive:** Yes, using width percentages in HTML tables (e.g., `width="50%"`).
**Rendering Constraints:**

- No `<style>` or `class` attributes allowed.
- No external CSS (Tailwind/Bootstrap).
- Only a safe subset of inline CSS (alignment, width) survives.

**Key Tools:**

- `lowlighter/metrics` for advanced activity infographics.
- `anuraghazra/github-readme-stats` for language and rank cards.
- GitHub Actions for automated SVG commits.

## Constraints & Requirements

### Budget

- **Development tools:** $0 (GitHub, Vercel Free Tier).
- **Hosting/Infrastructure:** $0 (GitHub Actions, Vercel Hobby).

### Timeline

- **Foundation:** Day 1.
- **Stats Integration:** Days 2-3.
- **Automation Setup:** Day 4.
- **Polish & Launch:** Day 7.

## Open Questions & Assumptions

- **Assumption:** GitHub’s "Camo" image proxy will not cause excessive caching issues for dynamic SVGs.
- **Question:** Will the minimalist design be compromised if too many "must-have" metrics are added later?.

## Quality Standards

**Code Quality:**

- YAML workflows must be linted to avoid silent failures in GitHub Actions.
- Use persistent storage for metrics (committing SVGs to the repo) rather than relying solely on external live-calls to avoid rate limits.

**Design Quality:**

- Adhere strictly to the "Minimalist" vibe—avoid neon gradients or cluttered layouts.
- Verify all links and badges lead to active destinations.

## MVP Completion Checklist

### Development Complete

- [ ] Profile repository created.
- [ ] Core HTML table layout implemented.
- [ ] GitHub Stats and Language cards integrated.
- [ ] `lowlighter/metrics` Action running on a schedule.

### Quality Checks

- [ ] Profile renders correctly on mobile devices.
- [ ] Daily automation successfully commits `assets/metrics.svg`.
- [ ] "Visually stunning" and "Minimalist" vibe verified.

---
