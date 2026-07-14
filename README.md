# TEP Web Prototype

A UX/UI **prototype** for The Education People website redesign, built with
[Astro](https://astro.build). This is a front-end concept for agreeing design and
user journeys — **not** the production build (which will be Umbraco). Its job is to:

1. Lock the visual design and page layouts
2. Let stakeholders click through real journeys
3. Act as a component reference for the production build

## Run it locally

```bash
npm install      # first time only
npm run dev      # start the dev server → http://localhost:4321
```

Other commands: `npm run build` (static output to `dist/`), `npm run preview`.

## Pages

| Route | Page |
|---|---|
| `/` | Homepage |
| `/training-product` | Training / course detail page |
| `/generic-product` | Generic product detail page (downloadable resource) |
| `/search-results` | Search results page |
| `/global-search` | Global search dropdown experience |

## How it's structured

```
src/
  styles/tep-system.css   ← the shared design system (tokens, buttons, cards,
                             badges, header/footer/nav styles). Change here = change everywhere.
  components/
    Header.astro          ← site header + primary nav (prop: `active`)
    Footer.astro          ← site footer + newsletter
  layouts/
    Base.astro            ← <head>, header + footer slots (props: title, active, header, footer)
  pages/                  ← one .astro file per route (uses Base + page-specific styles)
public/                   ← static assets (logo, hero image)
```

**Editing tips**
- Shared header/footer/nav → edit the components in `src/components/`.
- Colours, buttons, cards, spacing, fonts → `src/styles/tep-system.css`
  (fonts are CSS variables, so the final brand typeface swaps in one place).
- A specific page's layout → that page's `.astro` file in `src/pages/`.

## Deploy (Vercel)

Push this repo to GitHub, then in Vercel choose **Add New → Project → Import** the
repo. Vercel auto-detects Astro — no config needed. Every push to `main`
redeploys automatically.

## Notes / still placeholder

- Photography is Unsplash; stats, testimonials and copy are placeholder.
- Typography is Figtree as a neutral stand-in — final type TBC with TEP brand guidance.
- Interactions (accordion, tabs, search dropdown, date select) are front-end mockups.
- Desktop-first; a mobile pass is still to do.

— Built by Loop Design for The Education People.
