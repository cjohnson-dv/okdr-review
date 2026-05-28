# OKDR Review Site — Cursor Handoff (v2)

A 15-minute workflow to get the OKDR review mockups deployed via GitHub → DO App Platform.

**v2 changes:** three pages now (was two). Added the Kobudo page with the real Matayoshi lineage. Member dojos populated from okdr.org/affiliates — eight Shibu Cho leaders plus select affiliates.

## What you're building

A three-page static site for stakeholder review of the proposed unified OKDR + Authentic Ancient Arts website. No backend, no build step, no framework. Three self-contained HTML files.

This is a **review mockup**, not the production site. The Flask production build comes later.

## Files

In `/mnt/user-data/outputs/`:

- `OKDR-mockup.html` → rename to `index.html`
- `OKDR-karate.html` → rename to `karate.html`
- `OKDR-kobudo.html` → rename to `kobudo.html`

All three are large (~200–280KB) because the OKDR crest is embedded as base64. Intentional — keeps them portable.

## Setup workflow

### 1. Create folder, copy files, rename

```bash
mkdir okdr-review
cd okdr-review
# Copy the three HTML files into this folder first
mv OKDR-mockup.html index.html
mv OKDR-karate.html karate.html
mv OKDR-kobudo.html kobudo.html
```

### 2. Open in Cursor

```bash
cursor .
```

### 3. Add `.cursorrules`

Create `.cursorrules` in the repo root:

```
This is a static review site for the OKDR (Okinawa Kobudo Doushi Rensei-Kai)
website redesign. Three self-contained HTML files deployed to DO App Platform.

CONSTRAINTS:
- Static HTML only. No build step. No package.json. No framework.
- Do not extract CSS into separate files. Each HTML page is intentionally
  self-contained including embedded base64 crest image.
- Do not restructure or "improve" the design. It has been iteratively
  approved by the user and stakeholders.
- Python only for any tooling (per user's stack preference).

CONTEXT:
- index.html = OKDR home page (kobudo HQ framing, AAA as honbu dojo)
- karate.html = Shorin-Ryu Shorinkan karate detail page
- kobudo.html = Matayoshi kobudo detail page (the organization's primary art)
- All three pages cross-link via nav menu, breadcrumb, and footer

KEY FACTS (verify before editing related content):
- OKDR founded January 2002 by Gakiya Yoshiaki Sensei
- Kobudo lineage: Matayoshi Shinpo (d. 1997) -> Gakiya Yoshiaki -> Neil Stolsmark (President since 2011)
- Hanshi Stolsmark: 9th Dan Kobudo, 8th Dan Karate
- Karate lineage: Chibana -> Shugoro Nakazato -> Minoru Nakazato -> Stolsmark
- Honbu = Authentic Ancient Arts, 369 W Main St, Waukesha, WI 53186

WHEN MAKING CONTENT EDITS:
- Verify cross-page links still work
- Preserve gold (kobudo) / red (karate) color coding
- Honor Gakiya Sensei in any kobudo content
```

### 4. Fix internal links (terminal, 3 commands)

```bash
sed -i '' 's/OKDR-karate\.html/karate.html/g' index.html karate.html kobudo.html
sed -i '' 's/OKDR-mockup\.html/index.html/g' index.html karate.html kobudo.html
sed -i '' 's/OKDR-kobudo\.html/kobudo.html/g' index.html karate.html kobudo.html
```

### 5. Verify rename

```bash
grep -c "OKDR-mockup.html\|OKDR-karate.html\|OKDR-kobudo.html" index.html karate.html kobudo.html
```

All three lines should report `0`.

### 6. Test locally

```bash
open index.html
```

Click through all three pages via the nav. Confirm:

- Kobudo card on home (gold accent) → loads `kobudo.html`
- Karate card on home (red accent) → loads `karate.html`
- Breadcrumb / nav links between all pages work
- Crest renders in header on every page

### 7. README

Create `README.md`:

```markdown
# OKDR Review Site

Static mockups of the proposed unified OKDR + AAA website. Hosted on DO App Platform for stakeholder review.

## Pages

- `/` — OKDR home (kobudo HQ framing, AAA as honbu dojo)
- `/karate.html` — Shorin-Ryu Shorinkan karate page (Chibana / Nakazato lineage)
- `/kobudo.html` — Matayoshi kobudo page (Gakiya / OKDR lineage)

## Updating

Edit HTML, commit, push to main. DO auto-deploys in ~30 seconds.

## Stack

- Static HTML (intentional)
- Crest embedded as base64 (intentional)
- No build step

Future production build will be Flask + Jinja2 on DO App Platform — separate effort.
```

### 8. Git init and push

Create empty GitHub repo first (`okdr-review`, no README/license/gitignore).

```bash
git init
git add .
git commit -m "Initial OKDR review mockups — 3 pages"
git branch -M main
git remote add origin https://github.com/YOUR-USERNAME/okdr-review.git
git push -u origin main
```

### 9. Deploy on DO App Platform

In DO dashboard:

1. Apps → Create App
2. Source: GitHub → authorize → select `okdr-review` repo, `main` branch
3. Resource type: **Static Site**
4. Build command: leave blank
5. Output directory: `/`
6. Index document: `index.html`
7. Plan: **Starter (free)**
8. App name: `okdr-review`
9. Create

First deploy ~2 minutes. URL: `okdr-review-xxxxx.ondigitalocean.app`.

## Verification after deploy

- Home loads with crest in header and hero watermark
- Stats bar shows: 25+ dojos, 7 countries, 3 continents, 40+ years
- Two path cards on home (Kobudo gold, Karate red) — both clickable to their detail pages
- Top nav works between all three pages
- Member dojos section shows 8 Shibu Cho cards in a 4-column grid
- Below those, 4 affiliated dojos
- Funakoshi quote on karate page in serif italic
- Lineage tree on karate page: Chibana → Nakazato → M. Nakazato → Stolsmark
- Lineage tree on kobudo page: Matayoshi → Gakiya → Stolsmark
- Gakiya Sensei honored on kobudo page in dedicated "Founder · In Memoriam" card

## Content sources

All content is sourced from public OKDR materials:

- `okdr.org/home` — organization background, founding (Jan 2002), Gakiya lineage, crest meaning, philosophy (Doushi, Rensei)
- `okdr.org/affiliates` — full member dojo list with instructor names, ranks, locations
- `authenticancientarts.com` — local dojo content, schedule, history
- Wikipedia (Shorin-Ryu Shorinkan), news interviews — for biographical context

## Member dojos on home page

**Branch Chiefs (Shibu Cho) — 8 cards in 4-column grid:**

1. **Honbu** — Authentic Ancient Arts, Waukesha WI (Hanshi Neil Stolsmark, Kokusai Honbu Cho)
2. **Boston Shibu** — Kodokan Boston, Waltham MA (Frederick W. Lohse III, Kyoshi Nanadan)
3. **Pennsylvania Shibu** — Allegheny County Budo-kai, Venetia PA (Susan & Rick Sbuscio, Rokudan)
4. **Canada Shibu** — Eastwind Martial Arts, Ottawa & Napanee ON (Mike Sywyk, Rokudan)
5. **New York Shibu** — Mountain Stream Budo, Putnam Valley NY (Noah Mitchell, Rokudan)
6. **Switzerland Shibu** — Shindokan Chur, Switzerland (Markus Ullius, Sandan)
7. **India Shibu** — OKDR India, Wayanad Kerala (Anand A, Yondan)
8. **Cyprus Shibu** — Faitazu Martial Arts, Larnaca Cyprus (Charalambos Charalambous, Shodan)

**Select affiliated dojos — 4 cards:**

- Authentic Ancient Arts MN / Nintai Dojo, Hudson WI
- Roninkan, Athens Greece
- Karatemantova A.S.D., Mantova Italy
- Okinawan Karate-Do Academy, St. Louis MO

OKDR has 25+ total dojos. The home page shows leadership tier + representative affiliates. A future "View All 25+ Dojos" page could be built when the production site goes up.

## Pending content questions for Sensei

- **Karate kata curriculum** on karate.html — verify the kata list matches Shorinkan progression
- **Class schedule** placeholders need real times
- **Madison Shibu** (Don & Theresa Wideman) is not currently shown on the home page — should be added?
- **Should the home page member dojos section also include the worldwide stat band** (3 continents, 7 countries) more prominently?
- **Affiliated dojos** — the 4 shown are a curated selection. Sensei may want a different mix featured.

## What this is NOT

- Not the production Flask app
- Don't add a build step, dependencies, or tooling
- Don't refactor the design or restructure the HTML
- Don't extract the CSS into separate files

Keep it boring. Boring deploys reliably.
