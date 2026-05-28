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
