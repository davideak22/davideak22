# 🚀 High-End GitHub Profile README — Complete Technical Masterguide

> _For developers at the intersection of Software Engineering & Social Media Marketing_

---

## ⚠️ GitHub Markdown Rendering Constraints (Read First)

Before anything else, understand what GitHub **strips from Markdown**:

| Feature                                       | Status                                                                           |
| --------------------------------------------- | -------------------------------------------------------------------------------- |
| `<style>` tags                                | ❌ Stripped entirely                                                             |
| `class=` attributes                           | ❌ Stripped                                                                      |
| CSS properties inline (`style="color: red"`)  | ⚠️ Partially — only a safe subset survives (alignment, width on `<img>`, `<td>`) |
| `<script>` tags                               | ❌ Stripped                                                                      |
| `<iframe>`                                    | ❌ Stripped                                                                      |
| `<div>`, `<table>`, `<td>`, `<img>`, `<a>`    | ✅ Allowed (with sanitized attributes)                                           |
| SVG `<img src="*.svg">` (external URL)        | ✅ Allowed and rendered                                                          |
| SVG `<img>` with animated CSS inside SVG file | ✅ Animations work inside SVGs                                                   |
| Tailwind/Bootstrap classes                    | ❌ No external CSS, zero effect                                                  |
| `align="center"` on `<p>` or `<div>`          | ✅ Works as a legacy HTML attribute                                              |

**Key insight:** You cannot use React/Tailwind _directly_ in the README. Your React/Tailwind skills are best deployed to **build the SVG generator services** (Cloudflare Workers, Vercel functions) that _serve_ the visual output as `.svg` URLs. The README just `<img>`s them in.

---

## 1. THE HIGH-END TECH STACK (5–7 Tools)

### Tier 1 — Core Stats & Infographics

**① `lowlighter/metrics`** — The Swiss Army knife. 30+ plugins, renders as SVG/PDF/Markdown.

- Repo: https://github.com/lowlighter/metrics
- Free instance: https://metrics.lecoq.io
- Plugins of note: `plugin_activity`, `plugin_habits`, `plugin_isocalendar`, `plugin_languages`, `plugin_posts` (RSS), `plugin_topics`, `plugin_reactions`
- Run it as a GitHub Action that commits SVGs directly to your repo daily.

```yaml
# .github/workflows/metrics.yml
name: Metrics
on:
  schedule: [{ cron: "0 0 * * *" }]
  workflow_dispatch:
jobs:
  github-metrics:
    runs-on: ubuntu-latest
    permissions:
      contents: write
    steps:
      - uses: lowlighter/metrics@latest
        with:
          token: ${{ secrets.METRICS_TOKEN }}
          filename: assets/metrics.svg
          plugin_languages: yes
          plugin_habits: yes
          plugin_habits_charts: yes
          plugin_isocalendar: yes
          plugin_isocalendar_duration: half-year
          config_timezone: America/New_York
```

**② `anuraghazra/github-readme-stats`** — The classic. Cards for stats, top langs, WakaTime.

- Repo: https://github.com/anuraghazra/github-readme-stats
- Self-deploy on Vercel (free) to avoid rate limit issues on the public endpoint.
- Supports 30+ themes, custom hex colors, border radius, `hide_rank`, `show_icons`, gradient `bg_color`.

```
<!-- Example with full customization -->
<img src="https://github-readme-stats.vercel.app/api?username=YOUR_USER
  &show_icons=true
  &theme=tokyonight
  &hide_border=true
  &bg_color=0D1117
  &title_color=58A6FF
  &icon_color=1F6FEB
  &text_color=8B949E"
/>
```

**③ `anmol098/waka-readme-stats`** — Deep coding activity with WakaTime integration.

- Repo: https://github.com/anmol098/waka-readme-stats
- Shows time-coded bar charts: languages, editors, OS, projects, commit times.
- Requires: WakaTime account (free) + IDE plugin + GitHub PAT.

**④ `DenverCoder1/github-readme-streak-stats`** — Contribution streak, total commits, longest streak.

- URL: https://streak-stats.demolab.com
- Highly brandable: custom `background`, `border`, `stroke`, `ring`, `fire`, `dates` colors.

```
[![GitHub Streak](https://streak-stats.demolab.com?user=YOUR_USER
  &theme=dark&hide_border=true&date_format=M%20j%5B%2C%20Y%5D)](https://git.io/streak-stats)
```

**⑤ `antonkomarev/github-profile-views-counter`** — Visitor counter badge.

- Repo: https://github.com/antonkomarev/github-profile-views-counter
- `![Views](https://komarev.com/ghpvc/?username=YOUR_USER&color=58A6FF&style=flat-square&label=Profile+Views)`

**⑥ `tthn0/Spotify-Readme`** OR `kittinan/spotify-github-profile`\*\* — Live Spotify card.

- Repo: https://github.com/tthn0/Spotify-Readme
- Deploys to Vercel or PythonAnywhere (both free tier). Shows currently playing / last played track with album art.

**⑦ `readme-typing-svg` (DenverCoder1)** — Animated typing effect for headers/CTAs.

- URL: https://readme-typing-svg.demolab.com
- `![Typing SVG](https://readme-typing-svg.demolab.com?font=Fira+Code&pause=1000&color=58A6FF&width=435&lines=Software+Engineer+%7C+Marketing+Tech;Building+at+the+intersection+of+...)`

---

## 2. SOCIAL MEDIA BRIDGE — Step-by-Step

### Architecture Overview

```
┌─────────────────────────────────────────────────────────────┐
│                  GitHub Actions (cron: daily)               │
└─────────────────────────────────────────────────────────────┘
         │                        │                   │
         ▼                        ▼                   ▼
  ┌─────────────┐       ┌──────────────────┐  ┌────────────────┐
  │  Twitter/X  │       │  Instagram Graph │  │  LinkedIn RSS  │
  │  API v2     │       │  API (free tier) │  │  (scrape/RSS)  │
  └─────────────┘       └──────────────────┘  └────────────────┘
         │                        │                   │
         ▼                        ▼                   ▼
  ┌─────────────────────────────────────────────────────────────┐
  │              Python script: update_social.py                │
  │  - Fetches data → formats → injects into README.md          │
  │    between <!--START_SECTION:social--> markers              │
  └─────────────────────────────────────────────────────────────┘
         │
         ▼
  ┌─────────────────┐
  │  git commit     │
  │  & push README  │
  └─────────────────┘
```

### Platform Reality Check (API Restrictions as of 2025)

| Platform                       | Free API Access                                                                                                                      | What You Can Get                                                   |
| ------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------ |
| **Twitter/X**                  | Basic tier ($0 — read-only for your own data)                                                                                        | Your last 5 tweets, follower count                                 |
| **Instagram**                  | Instagram Graph API via Meta for Developers (free)                                                                                   | Post captions, media count, follower count (your own account only) |
| **LinkedIn**                   | Official API is heavily restricted; use your own profile RSS via `https://www.linkedin.com/in/YOUR_SLUG/recent-activity/shares/rss/` | Post titles/links                                                  |
| **YouTube**                    | Data API v3, free 10,000 units/day                                                                                                   | Latest video title, view count, subscriber count                   |
| **Dev.to / Medium / Hashnode** | RSS feed, completely free                                                                                                            | Latest post titles, URLs, publish dates                            |

> ⚡ **Pro tip:** RSS feeds from Dev.to/Medium/Hashnode are the most friction-free social integration for developers. Use the `lowlighter/metrics` `plugin_posts` to render them as a styled SVG card automatically — zero custom code needed.

### Step-by-Step: Updating README with Latest Posts (Python + GitHub Actions)

**Step 1 — Add section markers to your README.md:**

```markdown
## 📝 Latest Content

<!-- START_SECTION:latest-posts -->
<!-- END_SECTION:latest-posts -->
```

**Step 2 — Create the Python update script (`scripts/update_posts.py`):**

```python
import feedparser
import re
import os

FEEDS = {
    "dev.to": "https://dev.to/feed/YOUR_DEV_TO_USERNAME",
    "medium": "https://medium.com/feed/@YOUR_MEDIUM_USERNAME",
    # Add your YouTube RSS:
    # "youtube": "https://www.youtube.com/feeds/videos.xml?channel_id=YOUR_CHANNEL_ID",
}

MAX_POSTS = 5
README_PATH = "README.md"

def fetch_latest_posts():
    all_posts = []
    for source, url in FEEDS.items():
        feed = feedparser.parse(url)
        for entry in feed.entries[:MAX_POSTS]:
            all_posts.append({
                "title": entry.title,
                "url": entry.link,
                "date": entry.get("published", "")[:10],
                "source": source,
            })
    # Sort by date, newest first
    all_posts.sort(key=lambda x: x["date"], reverse=True)
    return all_posts[:MAX_POSTS]

def build_markdown_table(posts):
    lines = [
        "| 📌 Title | 🗓 Date | 🔗 Platform |",
        "|---|---|---|",
    ]
    for p in posts:
        lines.append(f"| [{p['title']}]({p['url']}) | {p['date']} | {p['source'].title()} |")
    return "\n".join(lines)

def inject_into_readme(content: str):
    readme = open(README_PATH).read()
    pattern = r"<!-- START_SECTION:latest-posts -->.*?<!-- END_SECTION:latest-posts -->"
    replacement = (
        "<!-- START_SECTION:latest-posts -->\n"
        + content
        + "\n<!-- END_SECTION:latest-posts -->"
    )
    return re.sub(pattern, replacement, readme, flags=re.DOTALL)

if __name__ == "__main__":
    posts = fetch_latest_posts()
    md = build_markdown_table(posts)
    new_readme = inject_into_readme(md)
    with open(README_PATH, "w") as f:
        f.write(new_readme)
    print(f"✅ Injected {len(posts)} posts into README")
```

**Step 3 — Create `requirements.txt`:**

```
feedparser==6.0.11
```

**Step 4 — Create the GitHub Actions workflow (`.github/workflows/update-social.yml`):**

```yaml
name: Update Social Content
on:
  schedule:
    - cron: "0 6 * * *" # Daily at 6 AM UTC
  workflow_dispatch: # Allow manual trigger

jobs:
  update-readme:
    runs-on: ubuntu-latest
    permissions:
      contents: write
    steps:
      - name: Checkout repo
        uses: actions/checkout@v4

      - name: Set up Python
        uses: actions/setup-python@v5
        with:
          python-version: "3.11"

      - name: Install dependencies
        run: pip install -r scripts/requirements.txt

      - name: Run update script
        run: python scripts/update_posts.py

      - name: Commit changes
        run: |
          git config user.name "readme-bot"
          git config user.email "41898282+github-actions[bot]@users.noreply.github.com"
          git add README.md
          git diff --staged --quiet || git commit -m "🤖 Auto-update: latest posts [skip ci]"
          git push
```

### Twitter/X Integration (Optional — Requires Basic API Tier)

```python
import tweepy

client = tweepy.Client(bearer_token=os.environ["TWITTER_BEARER_TOKEN"])

def get_latest_tweets(username: str, max_results: int = 3):
    user = client.get_user(username=username)
    tweets = client.get_users_tweets(
        id=user.data.id,
        max_results=max_results,
        exclude=["retweets", "replies"],
        tweet_fields=["created_at", "public_metrics"],
    )
    return [
        {
            "text": t.text[:80] + "...",
            "url": f"https://x.com/{username}/status/{t.id}",
            "likes": t.public_metrics["like_count"],
            "date": str(t.created_at)[:10],
        }
        for t in tweets.data
    ]
```

Add `TWITTER_BEARER_TOKEN` to your repo's **Settings → Secrets and Variables → Actions**.

### Instagram Integration (via Meta Graph API)

The Instagram Basic Display API was deprecated in 2024. Use the **Instagram Graph API** via a Meta Developer App on a Business/Creator account:

```python
import requests

def get_instagram_posts(access_token: str, limit: int = 3):
    url = f"https://graph.instagram.com/me/media"
    params = {
        "fields": "id,caption,media_url,permalink,timestamp",
        "limit": limit,
        "access_token": access_token,
    }
    data = requests.get(url, params=params).json()
    return data.get("data", [])
```

> ⚠️ Instagram access tokens expire every 60 days. Automate refresh using the long-lived token endpoint in a separate cron workflow.

---

## 3. LAYOUT BLUEPRINT — Dashboard Feel in Markdown

GitHub Markdown allows `<table>` with `<td>` and `align` attributes. This is your layout primitive.

```html
<!-- 2-column dashboard layout -->
<div align="center">
  <!-- HERO HEADER -->
  <img
    src="https://readme-typing-svg.demolab.com?font=JetBrains+Mono&weight=600&size=28&pause=1000&color=58A6FF&center=true&vCenter=true&width=600&lines=Hi+%F0%9F%91%8B+I'm+YOUR_NAME;Software+Engineer+%26+Marketing+Tech;Building+digital+products+that+scale."
  />

  <!-- VISITOR COUNTER + BADGES ROW -->
  <p>
    <img
      src="https://komarev.com/ghpvc/?username=YOUR_USER&color=58A6FF&style=flat-square&label=Profile+Views"
    />
    <a href="https://twitter.com/YOUR_HANDLE">
      <img
        src="https://img.shields.io/twitter/follow/YOUR_HANDLE?style=flat-square&logo=x&color=58A6FF&labelColor=0D1117"
      />
    </a>
    <a href="https://linkedin.com/in/YOUR_SLUG">
      <img
        src="https://img.shields.io/badge/LinkedIn-Connect-0A66C2?style=flat-square&logo=linkedin"
      />
    </a>
  </p>
</div>

---

<!-- TWO COLUMN GRID using HTML table -->
<table>
  <tr>
    <td width="50%" valign="top">
      ### 🧠 GitHub Stats
      <img
        src="https://github-readme-stats.vercel.app/api?username=YOUR_USER&show_icons=true&theme=tokyonight&hide_border=true&bg_color=0D1117"
      />

      <img
        src="https://streak-stats.demolab.com?user=YOUR_USER&theme=dark&hide_border=true"
      />
    </td>
    <td width="50%" valign="top">
      ### ⏱ Weekly Coding Activity
      <!-- Waka README stats section start -->
      <!--START_SECTION:waka-->
      <!--END_SECTION:waka-->

      ### 🎵 Now Playing
      <img
        src="https://spotify-readme.YOUR_VERCEL_DOMAIN.vercel.app/api/now-playing"
      />
    </td>
  </tr>
</table>

---

<!-- THREE COLUMN for skills / metrics / CTA -->
<table>
  <tr>
    <td width="33%" align="center">
      **🛠 Stack**
      <br />
      ![React](https://img.shields.io/badge/React-20232A?style=for-the-badge&logo=react&logoColor=61DAFB)
      ![TypeScript](https://img.shields.io/badge/TypeScript-007ACC?style=for-the-badge&logo=typescript&logoColor=white)
      ![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)
    </td>
    <td width="33%" align="center">
      **📈 Metrics**
      <br />
      <img src="assets/metrics.svg" />
    </td>
    <td width="33%" align="center">
      **📣 CTA**
      <br />

      > 🚀 **Available for freelance & collabs** [![Hire
      Me](https://img.shields.io/badge/💼_Hire_Me-2ea44f?style=for-the-badge)](mailto:you@domain.com)
      [![Portfolio](https://img.shields.io/badge/🌐_Portfolio-58A6FF?style=for-the-badge)](https://your-site.com)
    </td>
  </tr>
</table>
```

> 💡 **Layout Hack:** The `<table>` trick is the only true multi-column layout in GitHub Markdown. Nested `<img>` tags inside `<td>` are fine. Width percentages (`width="50%"`) work reliably.

---

## 4. SVG Header Generators & Dynamic SVG Techniques

### Top 3 SVG Header Tools with Branding/CTA Support

**① readme-typing-svg** (DenverCoder1)

- URL: https://readme-typing-svg.demolab.com
- Customizable: font, size, color, speed, multi-line cycling text, center/vCenter alignment.
- Perfect for cycling through your value props: _"React Developer | Marketing Tech | Open to Hire"_

**② GitHub Profile Header Generator**

- URL: https://github.com/leviarista/github-profile-header-generator (static but exportable)
- Creates a custom-branded PNG/SVG header with waves, gradient, name, tagline, icons.

**③ Custom Vercel SVG Function (DIY — Maximum Control)**
Build your own dynamic SVG using a Cloudflare Worker or Vercel Edge Function:

```typescript
// api/header.ts  — Deploy to Vercel
import { ImageResponse } from "@vercel/og";

export const config = { runtime: "edge" };

export default function handler() {
  return new ImageResponse(
    (
      <div
        style={{
          background: "linear-gradient(135deg, #0D1117 0%, #161B22 100%)",
          width: "100%",
          height: "100%",
          display: "flex",
          flexDirection: "column",
          alignItems: "center",
          justifyContent: "center",
          fontFamily: "JetBrains Mono",
          color: "#58A6FF",
        }}
      >
        <h1 style={{ fontSize: 64, margin: 0 }}>YOUR NAME</h1>
        <p style={{ fontSize: 24, color: "#8B949E" }}>
          Software Engineer × Marketing Tech
        </p>
        <div style={{ display: "flex", gap: 16, marginTop: 20 }}>
          <span style={{ background: "#1F6FEB22", padding: "8px 16px", borderRadius: 8, border: "1px solid #1F6FEB" }}>
            🚀 Available for hire
          </span>
        </div>
      </div>
    ),
    { width: 1200, height: 400 }
  );
}
```

This uses **`@vercel/og`** (JSX → PNG/SVG on the edge, free on Vercel hobby plan). Embed as:

```markdown
![Header](https://YOUR_VERCEL_APP.vercel.app/api/header)
```

---

## 5. VISITOR COUNTER & LIVE ACTIVITY

### Visitor Counter

```markdown
![Profile Views](https://komarev.com/ghpvc/?username=YOUR_USER
&color=58A6FF
&style=flat-square
&label=Visitors
&abbreviated=true)
```

- Repo: https://github.com/antonkomarev/github-profile-views-counter
- Free, no setup required, persistent counter.

### Spotify "Now Playing" Live Card

```markdown
[![Spotify](https://spotify-readme.YOUR_APP.vercel.app/api/now-playing)](https://open.spotify.com/user/YOUR_SPOTIFY_ID)
```

- Repo (deploy your own): https://github.com/tthn0/Spotify-Readme
- Alternative (public instance): https://github.com/kittinan/spotify-github-profile
  - `![Spotify](https://spotify-github-profile.kittinan.dev/api/view?uid=YOUR_SPOTIFY_UID&cover_image=true&theme=natemoo-re)`

### Discord Rich Presence (Live Coding Status)

```markdown
![Discord](https://discord-readme-badge.vercel.app/api?id=YOUR_DISCORD_USER_ID)
```

- Repo: https://github.com/Zyplos/discord-readme-badge
- Shows IDE, file being edited, time elapsed via Discord Rich Presence.
- Requires joining the project's Discord server (for the Lanyard API).

---

## 6. AUTOMATION WORKFLOW DIAGRAM

```
GitHub Actions Scheduler (cron)
        │
        ├──[0 0 * * *]──► lowlighter/metrics Action
        │                   └─► commits assets/metrics.svg
        │
        ├──[0 6 * * *]──► update-social.yml
        │                   ├─► Python fetches RSS/APIs
        │                   └─► Injects into README between markers
        │
        └──[*/5 * * * *]─► (if using self-hosted Spotify)
                            └─► Generates spotify.svg, commits

External Services (always-on, no Action needed):
  - github-readme-stats.vercel.app  → Stats SVG (per-request)
  - streak-stats.demolab.com        → Streak SVG (per-request)
  - komarev.com/ghpvc               → View counter (per-request)
  - spotify-readme.vercel.app       → Now playing SVG (per-request)

README.md embeds all of the above via <img src="URL" />
```

> **Data flow for Action-generated SVGs:** Action runs → API called → SVG rendered in container → `git commit` pushes SVG to repo → README `<img src="assets/metrics.svg">` loads the committed file via GitHub's raw CDN.

> **Data flow for per-request SVGs:** Visitor loads profile → GitHub proxies all image URLs through Camo → Camo requests the Vercel/external service → Service calls the relevant API → Returns fresh SVG.

---

## 7. THE 7-DAY IMPLEMENTATION PLAN

| Day       | Goal                  | Tasks                                                                                                                                                              |
| --------- | --------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **Day 1** | Foundation            | Create `USERNAME/USERNAME` repo. Set basic structure with `<table>` layout. Add typing SVG hero. Add Komarev visitor counter.                                      |
| **Day 2** | GitHub Stats Layer    | Deploy `github-readme-stats` to your own Vercel app. Add stats card + top langs card. Configure custom colors to match your brand. Add streak stats.               |
| **Day 3** | Metrics Action        | Set up `lowlighter/metrics` workflow. Generate `assets/metrics.svg` with `plugin_languages`, `plugin_habits`, `plugin_isocalendar`. Commit and verify.             |
| **Day 4** | WakaTime Layer        | Sign up at wakatime.com. Install IDE plugin. Wait 24h for data. Set up `anmol098/waka-readme-stats` Action.                                                        |
| **Day 5** | Social/Content Bridge | Pick your content platform (Dev.to / Medium / YouTube). Deploy `update_posts.py` with GitHub Action. Verify markdown table updates at 6AM UTC.                     |
| **Day 6** | Live Activity         | Deploy Spotify Readme to Vercel (30 min). Add Spotify card. Optionally add Discord badge. Set up `@vercel/og` header endpoint.                                     |
| **Day 7** | Polish & CTA          | Finalize color palette (pick 2–3 hex colors, use them everywhere). Add CTA badges (Hire Me, Portfolio, Email). Review mobile rendering. Share on LinkedIn/Twitter. |

---

## 8. METRICS-AS-A-SERVICE & SOCIAL BADGES

### Shields.io Social Badges (follower counts in real-time)

```markdown
<!-- Twitter/X followers -->

![X Follow](https://img.shields.io/twitter/follow/YOUR_HANDLE?style=social)

<!-- YouTube subscribers -->

![YouTube Channel Subscribers](https://img.shields.io/youtube/channel/subscribers/YOUR_CHANNEL_ID?style=social)

<!-- Twitch status -->

![Twitch Status](https://img.shields.io/twitch/status/YOUR_TWITCH_USERNAME)
```

These are all free, served by https://shields.io — no API key needed. They pull public data.

> ⚠️ `shields.io` does **not** support Instagram or LinkedIn follower counts because those APIs require auth and don't expose public endpoints.

### For Instagram/LinkedIn Follower Counts

Build a Cloudflare Worker (free 100k req/day) that:

1. Uses your own authenticated session token (stored as an env var/secret)
2. Returns a shields.io-compatible JSON endpoint
3. Embed as: `![Instagram](https://img.shields.io/endpoint?url=https://your-worker.workers.dev/instagram-followers)`

The shields.io custom endpoint format:

```json
{
  "schemaVersion": 1,
  "label": "Instagram",
  "message": "12.4K followers",
  "color": "E4405F",
  "style": "flat-square"
}
```

---

## 9. COMPLETE FILE STRUCTURE

```
USERNAME/
├── README.md                         # Your profile page
├── assets/
│   ├── metrics.svg                   # Generated by lowlighter/metrics
│   ├── header.png                    # Optional static header
│   └── spotify.svg                   # If self-generating Spotify card
├── scripts/
│   ├── update_posts.py               # Social content updater
│   └── requirements.txt
└── .github/
    └── workflows/
        ├── metrics.yml               # lowlighter/metrics Action
        ├── waka-readme.yml           # anmol098/waka-readme-stats Action
        └── update-social.yml         # Your custom social updater
```

---

## Resources

- `lowlighter/metrics`: https://github.com/lowlighter/metrics (accessed Feb 2025)
- `anuraghazra/github-readme-stats`: https://github.com/anuraghazra/github-readme-stats
- `anmol098/waka-readme-stats`: https://github.com/anmol098/waka-readme-stats
- `DenverCoder1/github-readme-streak-stats`: https://github.com/DenverCoder1/github-readme-streak-stats
- `readme-typing-svg`: https://github.com/DenverCoder1/readme-typing-svg
- `antonkomarev/github-profile-views-counter`: https://github.com/antonkomarev/github-profile-views-counter
- `tthn0/Spotify-Readme`: https://github.com/tthn0/Spotify-Readme
- `Zyplos/discord-readme-badge`: https://github.com/Zyplos/discord-readme-badge
- Shields.io: https://shields.io
- `@vercel/og` docs: https://vercel.com/docs/functions/og-image-generation
- Awesome GitHub Profile READMEs list: https://github.com/abhisheknaiidu/awesome-github-profile-readme
