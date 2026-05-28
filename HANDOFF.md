# OKDR Review Site — Claude Code Handoff

## Goal

Stand up a static review site on Digital Ocean App Platform so non-technical stakeholders (Sensei Stolsmark, OKDR members) can preview the proposed redesign of `authenticancientarts.com` + `okdr.org` as a unified site.

This is a **mockup for review only** — not the production build. Two static HTML pages with embedded crest images. No backend, no database, no build step.

## Background

- **Current state:** Two aging WordPress sites — `authenticancientarts.com` (local Waukesha dojo) and `okdr.org` (worldwide kobudo organization HQ'd at AAA).
- **Proposed direction:** Unify under `okdr.org` with the dojo (AAA) framed as the "honbu" (worldwide headquarters). The owner runs OKDR globally for kobudo, and is a member dojo of Shorinkan for karate.
- **Stack preference:** Python only for all backend/script work. Flask + Jinja2 for the eventual production site. No Node, no TypeScript, no JS frameworks.
- **Execution environment:** Digital Ocean App Platform.

This handoff covers **only the static review site**, not the production rebuild. Production build comes later.

## Repo structure to create

```
okdr-review/
├── index.html              # the OKDR home mockup (was OKDR-mockup.html)
├── karate.html             # the karate detail page (was OKDR-karate.html)
└── README.md               # short notes for future me
```

Both HTML files are completely self-contained. Crest image is embedded as base64 data URI. Google Fonts loaded via CDN link. No assets to manage, no external dependencies.

## Step 1 — Get the HTML source files

The two source files live at:

- `/mnt/user-data/outputs/OKDR-mockup.html` → rename to `index.html`
- `/mnt/user-data/outputs/OKDR-karate.html` → rename to `karate.html`

These are large (200KB+ each) because the OKDR crest is embedded as base64. That's intentional — keeps the files portable.

## Step 2 — Fix internal links after rename

Because the files are being renamed, internal cross-links must be updated.

### In `index.html`, find-and-replace:

| Find                  | Replace        | Count |
|-----------------------|----------------|-------|
| `OKDR-karate.html`    | `karate.html`  | 2     |

Locations: nav menu link, homepage karate path card link.

### In `karate.html`, find-and-replace:

| Find                  | Replace        | Count |
|-----------------------|----------------|-------|
| `OKDR-mockup.html`    | `index.html`   | 6     |
| `OKDR-karate.html`    | `karate.html`  | 1     |

Locations: header logo link, all nav menu items pointing home (Kobudo, Honbu Dojo, Member Dojos, Events anchors), breadcrumb home link, footer "Home" link, footer cross-references, and the karate self-reference in the footer.

Verify with grep after replacing — both `OKDR-mockup.html` and `OKDR-karate.html` strings should not appear anywhere in either file.

## Step 3 — Create README.md

```markdown
# OKDR Review Site

Static mockups of the proposed unified OKDR + AAA site, hosted on DO App Platform for stakeholder review.

## Pages

- `/` — OKDR home page (kobudo HQ framing, AAA as honbu dojo)
- `/karate.html` — Shorin-Ryu Shorinkan karate detail page

## Updating

Edit HTML locally, commit, push to main. DO App Platform auto-deploys in ~30s.

## Notes

- Crest image is embedded as base64 in each HTML file (intentional — keeps files portable)
- No build step
- No backend
- This is a review mockup, not the production site
```

## Step 4 — Git init and push

```bash
cd okdr-review
git init
git add .
git commit -m "Initial OKDR review mockups"
git branch -M main
git remote add origin <github-repo-url>
git push -u origin main
```

The user will create the GitHub repo manually before this step and provide the URL.

## Step 5 — DO App Platform setup

User handles this in the DO dashboard. No app.yaml needed — DO auto-detects static HTML sites.

Reference config for the user:

- **Source:** GitHub repo, `main` branch, auto-deploy on push
- **Resource type:** Static Site
- **Build command:** (leave blank — no build needed)
- **Output directory:** `/` (root)
- **Index document:** `index.html`
- **Plan:** Starter (free tier, 3 static sites included)

URL will be something like `okdr-review-xxxxx.ondigitalocean.app`.

## Verification checklist

After deploy, confirm:

- [ ] Home page (`/`) loads, crest renders in header and as hero watermark
- [ ] Stats bar shows gold numbers (40+, 4, 24+, 1000+)
- [ ] Two-paths section shows Kobudo (gold) and Karate (red) cards
- [ ] Karate card on home page is clickable, navigates to `/karate.html`
- [ ] Nav "Karate" link also navigates to `/karate.html`
- [ ] On karate page, breadcrumb "Home" link returns to `/`
- [ ] On karate page, nav links work for Kobudo, Honbu Dojo, Member Dojos, Events (all anchor jumps back on home page)
- [ ] Member dojos grid on home shows Honbu (Waukesha) and Canada Shibu (Eastwind, Ottawa & Napanee) prominently
- [ ] Funakoshi quote renders in Noto Serif JP italic
- [ ] Lineage tree on karate page shows 4-step Chibana → Nakazato → M. Nakazato → Stolsmark progression
- [ ] Both leadership cards (Hanshi + Rocky) render with credentials grids

## What this is NOT

- This is **not** the production Flask app. That's a separate phase.
- Do **not** add a build step, package.json, requirements.txt, or any tooling. It's just static HTML.
- Do **not** restructure the HTML or extract the CSS into separate files. Keeping each page as a single portable file is intentional — stakeholders may want to download and view offline.
- Do **not** "improve" the design, change colors, or refactor. The mockup has been reviewed and approved iteratively with the user. Cosmetic changes will be made by the user directly.

## Stakeholder review feedback expected

After deploy, the user will share the URL with Sensei Stolsmark and other OKDR stakeholders. Expect feedback on:

- Member dojo list (placeholders need real names — Midwest Karate Academy, Naha Dojo Affiliate are placeholders)
- Kata curriculum list on karate page (verify with Sensei)
- Class schedule placeholders
- Whether to add a dedicated Kobudo detail page (parallel to karate.html)

When feedback comes in, the user will edit the HTML directly and push. No tooling needed for content updates.
