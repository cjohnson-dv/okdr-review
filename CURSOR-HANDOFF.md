# OKDR Review Site — Cursor Setup Guide

A 15-minute workflow to get the OKDR review mockups deployed via GitHub → DO App Platform, using Cursor as your editor.

## What you're building

A two-page static site for stakeholder review of the proposed OKDR + Authentic Ancient Arts website redesign. No backend, no build step, no framework. Just two self-contained HTML files.

This is a **review mockup**, not the production site. The Flask production build comes later.

## What you have

In your `/mnt/user-data/outputs/` (or wherever you downloaded them):

- `OKDR-mockup.html` — the home page (will become `index.html`)
- `OKDR-karate.html` — the Karate detail page (will become `karate.html`)

Both files are large (~200KB each) because the OKDR crest is embedded as base64. Intentional — keeps them portable.

## Setup workflow

### 1. Create the project folder

```bash
mkdir okdr-review
cd okdr-review
```

Copy both HTML files into this folder. Rename them:
- `OKDR-mockup.html` → `index.html`
- `OKDR-karate.html` → `karate.html`

### 2. Open in Cursor

```bash
cursor .
```

### 3. Add a `.cursorrules` file

Create `.cursorrules` in the repo root with this content:

```
This is a static review site for the OKDR (Okinawa Kobudo Doushi Rensei-Kai) 
website redesign. Two self-contained HTML files deployed to DO App Platform.

CONSTRAINTS:
- Static HTML only. No build step. No package.json. No framework.
- Do not extract CSS into separate files. Each HTML page is intentionally 
  self-contained including embedded base64 crest image.
- Do not restructure or "improve" the design. It has been iteratively 
  approved by the user and stakeholders.
- Python only for any tooling (per user's stack preference).

CONTEXT:
- index.html = OKDR home page (kobudo HQ framing, AAA dojo as honbu)
- karate.html = Shorin-Ryu Shorinkan karate detail page
- Both pages cross-link via nav menu, breadcrumb, and footer
- The crest is embedded as base64 data URI in each file

WHEN MAKING CONTENT EDITS:
- Update placeholder member dojo data when user provides real info
- Verify cross-page links still work after any rename
- Preserve the gold (kobudo) / red (karate) color coding throughout
```

This gives Cursor persistent context across every chat session.

### 4. Fix internal links (one-time)

The HTML files reference each other by their old filenames. Use Cursor to fix.

Open `index.html`, then in Cursor chat (Cmd+L), paste:

> Find and replace all `OKDR-karate.html` with `karate.html` in this file. There should be 2 occurrences — the Karate nav link and the homepage Karate path card.

Then open `karate.html` and paste:

> Find and replace all `OKDR-mockup.html` with `index.html` in this file. Then replace any remaining `OKDR-karate.html` with `karate.html`. Verify with grep that neither old filename appears anywhere in the file when done.

### 5. Add a README

Create `README.md`:

```markdown
# OKDR Review Site

Static review mockups for the proposed unified OKDR + Authentic Ancient Arts website.
Deployed via DO App Platform for stakeholder feedback.

## Pages

- `/` — OKDR home (kobudo HQ framing, AAA as honbu dojo)
- `/karate.html` — Shorin-Ryu Shorinkan karate detail page

## Updating content

Edit HTML directly. Commit and push to `main`. DO App Platform auto-deploys in ~30s.

## Stack

- Plain static HTML (intentional)
- Crest embedded as base64 (intentional — keeps files portable)
- No build step

For the future production build, see HANDOFF docs elsewhere — Flask + Jinja2 on DO App Platform.
```

### 6. Verify before pushing

In your terminal:

```bash
grep -c "OKDR-mockup.html" index.html karate.html
grep -c "OKDR-karate.html" index.html karate.html
```

Both should return zero for both files. If anything's left, ask Cursor to clean it up.

Open both HTML files locally (double-click) and click between them to confirm navigation works.

### 7. Git init & push to GitHub

Create the empty GitHub repo first (in browser): `okdr-review`. Don't add README/license/gitignore — keep it empty.

Then:

```bash
git init
git add .
git commit -m "Initial OKDR review mockups"
git branch -M main
git remote add origin https://github.com/YOUR-USERNAME/okdr-review.git
git push -u origin main
```

### 8. Deploy on DO App Platform

In the DO dashboard:

1. Apps → Create App
2. Source: GitHub → authorize → select `okdr-review` repo → `main` branch
3. Resource type: **Static Site**
4. Build command: leave blank
5. Output directory: `/`
6. Index document: `index.html`
7. Plan: **Starter (free)** — DO includes 3 free static sites
8. Name the app: `okdr-review`
9. Create

First deploy takes ~2 minutes. URL will be something like `okdr-review-xxxxx.ondigitalocean.app`.

## Verification after deploy

Check these in the live URL:

- Home page (`/`) loads with crest in header and as hero watermark
- Stats bar shows gold numbers (40+, 4, 24+, 1000+)
- Kobudo card (gold) and Karate card (red) both render
- Click the Karate card → navigates to `/karate.html`
- Nav "Karate" link → also goes to `/karate.html`
- On karate page: breadcrumb "Home" → returns to `/`
- On karate page: nav links work for anchor jumps back home (Kobudo, Honbu Dojo, Member Dojos, Events)
- Member dojos grid shows Honbu (Waukesha) and Canada Shibu (Eastwind, Ottawa & Napanee) with gold accent borders
- Funakoshi quote renders in serif italic on karate page
- Lineage tree on karate page shows 4-step Chibana → Nakazato → M. Nakazato → Stolsmark

## Future updates

When stakeholders provide feedback:

1. Edit HTML directly in Cursor (use Cmd+K for inline edits, Cmd+L for chat)
2. Commit and push to main
3. DO auto-deploys in ~30 seconds
4. Same URL stays live throughout

## Pending content placeholders

These need real data from Sensei before going public:

- **Member dojos** — Chicago "Midwest Karate Academy" and Naha "Naha Dojo Affiliate" are placeholders. AAA (Waukesha) and Eastwind (Canada) are real.
- **Karate page curriculum** — kata names listed are the canonical Shorinkan progression. Verify with Sensei.
- **Class schedule** — placeholder times. Get the real schedule.
- **Open question:** Should a parallel Kobudo detail page be built next (matching the karate.html depth)?

## What this is NOT

- This is not the production Flask app. Don't extend it that way.
- Don't add a build step, dependencies, or tooling.
- Don't refactor the design or restructure the HTML.
- Don't extract the CSS into separate files.

Keep it boring. Boring deploys reliably.
