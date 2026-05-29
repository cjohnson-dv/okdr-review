# OKDR Review Site

Static mockups of the proposed unified OKDR + AAA website. Hosted on DO App Platform for stakeholder review.

## Pages

- `/` — OKDR home (kobudo HQ framing, AAA as honbu dojo)
- `/karate.html` — Shorin-Ryu Shorinkan karate page (Chibana / Nakazato lineage)
- `/kobudo.html` — Matayoshi kobudo page (Gakiya / OKDR lineage)
- `/dojos.html` — full worldwide member dojo directory

## Updating

Edit HTML, commit, push to main. DO auto-deploys in ~30 seconds.

## Stack

- Static HTML (intentional)
- Crest: `okdr-black-bg.png` (shared asset)
- No build step

Future production build will be Flask + Jinja2 on DO App Platform — separate effort.
