# Inner Circle

> *An intimate salon for ancient traditions in a contemporary space.*

The website for **Inner Circle**, a house concert series of Hindustani classical
music at 5800 Deal Place, Chevy Chase, MD.

**Stack:** [Hugo](https://gohugo.io) (static site) · [Sveltia CMS](https://github.com/sveltia/sveltia-cms) (editing) · GitHub (storage + history) · [Netlify](https://netlify.com) (build + hosting).

---

## How concerts work (the important part)

Each concert is **one Markdown file** in [`content/concerts/`](content/concerts/).
The site reads the **event date** and rotates everything automatically:

| State | Rule | Where it appears |
|-------|------|------------------|
| **Featured** | The soonest upcoming concert (or an explicit override) | Home page hero |
| **Upcoming** | Event date/`endDate` is in the future | "Upcoming Gatherings" + Concerts page |
| **Archived** | Event date/`endDate` has passed | "The Archive" |

A **scheduled rebuild runs daily** (see `.github/workflows/scheduled-rebuild.yml`),
so the morning after a concert it moves itself into the archive. **You never have
to manually retire a concert.**

To override the hero (e.g. spotlight concert #3), set `featuredConcert: "slug"` in
[`content/_index.md`](content/_index.md).

## Two ways to edit

1. **Through Claude (primary).** Just say what you want — "add the December sitar
   concert," "change Rimpa's doors time to 5:30," "swap the home hero photo." Claude
   edits the file, rebuilds, and (when asked) pushes. Concert details are pulled from
   Ticket Tailor.
2. **Through the web UI (backup).** Visit `/admin` on the live site, log in with
   GitHub, and edit concerts/pages in a form. Saves commit to GitHub and Netlify
   redeploys automatically.

### Anatomy of a concert file

```yaml
---
title: "An Evening with Rimpa Siva — Tabla"
date: 2026-10-25T17:00:00-04:00      # drives upcoming/archive rotation
endDate: 2026-10-25T22:30:00-04:00   # optional; for multi-day events
artist: "Rimpa Siva"
instrument: "Tabla"
accompanists: ["Shreyes Ravi — Harmonium"]
summary: "One sentence for cards and the home page."
image: "concerts/rimpa-siva.jpg"     # banner, lives in assets/images/concerts/
ticketUrl: "https://www.tickettailor.com/events/ibeam/2185033"
doors: "5:00 PM"
starts: "5:30 PM"
program:                              # optional "The Evening" schedule
  - { time: "Peshkar", desc: "A long, meditative introduction." }
---

Markdown body — the full description, artist bio, etc.
```

## Design system — the rules

The visual language lives in **one file**: [`assets/css/design-system.css`](assets/css/design-system.css).
It is the **single source of truth**. Every page and every future change must use its
tokens (CSS variables) — never hard-code a color, font, or spacing value elsewhere.

- **Colors:** bone white `--bone`, charcoal `--charcoal`, madder maroon `--maroon`, turmeric `--turmeric`.
- **Type:** EB Garamond (headlines) + Hanken Grotesk (body); uppercase tracked labels.
- **Shape:** sharp 0px corners; thin 1px charcoal rules; generous whitespace.
- **Direction:** light & airy — honest to the real house (light oak, white, warm wood).

## Running locally

```bash
hugo server --buildFuture     # http://localhost:1313
hugo --gc --minify            # production build into ./public
```

`--buildFuture` is required locally because upcoming concerts are future-dated.
(On Netlify it is set in `hugo.toml` via `buildFuture = true`.)

## Deployment

Pushes to `main` auto-build on Netlify (`netlify.toml`). See
[`DEPLOY.md`](DEPLOY.md) for the one-time GitHub + Netlify + newsletter setup.
