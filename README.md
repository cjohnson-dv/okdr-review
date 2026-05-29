# OKDR Review Site

Static mockups of the proposed unified OKDR + AAA website. Hosted on DO App Platform for stakeholder review.

## Pages

- `/` — OKDR home (kobudo HQ framing, AAA as honbu dojo)
- `/karate.html` — Shorin-Ryu Shorinkan karate page (Chibana / Nakazato lineage)
- `/kobudo.html` — Matayoshi kobudo page (Gakiya / OKDR lineage)
- `/dojos.html` — full worldwide member dojo directory
- `/videos.html` — featured YouTube content and OKDR subscription library

## Assets

- `okdr-black-bg.png` — OKDR crest (nav, footer, home/kobudo hero watermarks)
- `Shorinkan.png` — Shorinkan seal (karate hero watermark)

## Updating

Edit the deploy HTML files directly, commit, push to `main`. DO auto-deploys in ~30 seconds.

## Stack

- Static HTML (intentional, self-contained per page)
- No build step

Future production build will be Flask + Jinja2 on DO App Platform — separate effort.
