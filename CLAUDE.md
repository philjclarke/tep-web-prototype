# CLAUDE.md — TEP Web Prototype

Context for Claude Code. Read this before making changes.

## What this is (and isn't)

A **UX/UI prototype** for The Education People (TEP) website redesign, built with
**Astro** (static, zero-JS-by-default). Its only jobs are to (1) lock the visual
design, (2) let stakeholders click through real journeys, and (3) serve as a
**component reference** for the production build.

**Golden rule — this is NOT the production site.** Production will be built in
**Umbraco** by a separate dev team. So:
- ❌ Do **not** build real search, auth, booking, a CMS, or any backend.
- ❌ Do **not** add heavy tooling or frameworks beyond Astro.
- ✅ Keep it front-end, static, and on-brand. Interactions are mockups.

When unsure whether something is "prototype-appropriate," it probably isn't —
ask the user.

## Run / build

```bash
npm install
npm run dev       # http://localhost:4321  (live reload)
npm run build     # static output → dist/
npm run preview
```

Deploys to **Vercel** from GitHub (`main` branch, auto-detected as Astro). Push = redeploy.

## Architecture — where things live

```
src/
  styles/tep-system.css   ← THE design system. Tokens, buttons, cards, badges,
                             header/nav/footer styles. Edit here → changes everywhere.
  components/
    Header.astro           ← header + primary nav.  Prop: active =
                             '' | 'consultancy' | 'products' | 'training' | 'knowledge' | 'employment'
    Footer.astro           ← footer + newsletter (no props)
  layouts/Base.astro       ← <head>, Header, Footer. Props: title, active, marker,
                             header (bool), footer (bool)
  pages/*.astro            ← one file per route; each uses <Base> + its own styles/scripts
public/                    ← static assets (referenced as /tep-logo.png etc.)
```

**Pages:** `/` (home), `/training-product` (typical density), `/training-product-rich`
(high-density template demo — same template, optional sections populated), `/generic-product`,
`/search-results`, `/search-results-empty` (no-results state), `/global-search` (legacy static
mock of the open-search state — the live search overlay now lives in `Header.astro`).

**16.07 feedback round (applied):** homepage Featured section sits above the audience
accordion; compact hero; rotating testimonial; header/nav divider removed. Training pages
show a single event instance (no date picker — TEP rule: one event per page), topical tags,
expanded trainer, reduced yellow. Search results filters mirror the live Training & Events
listing pattern (search + Content Type + Delivery Method + Dates + Filters dropdowns,
popular tags row) — Content Type added prominently per Loop's decision.

### Conventions (follow these — the migration relies on them)
- Page-specific CSS goes in **`<style is:global>`** (NOT scoped) so it can target the
  shared component classes. The system CSS is imported once in `Base.astro`.
- Page-specific JS goes in **`<script is:inline>`** at the end of the page body, so it
  runs as written (no Astro bundling/scoping surprises).
- Static assets live in **`public/`** and are referenced with a leading slash.
- Shared chrome (header/footer/nav) → edit the **component**, never copy markup into a page.
- `global-search.astro` uses `header={false} footer={false}` because it renders its own
  custom "active search" header — that's intentional.

## Brand & design rules (keep everything consistent with these)

Direction: **light, clean, contemporary, spacious.** These come from TEP's brand
direction doc (see `../Feedback/13062026/TEP Brand & Visual Direction...pdf`).

- **Purple (`--purple` #41008C) is an ACCENT, not a background.** Use it for CTA buttons,
  headings, links, icons, and active/hover states. **Avoid** large solid-purple
  backgrounds, full-width purple bands, and big purple panels. Primary CTA buttons stay
  dark purple (the strongest use of purple).
- **New accent `--sand` #FCECAC** — use **sparingly** (badges, small highlights, accents).
  Never a dominant background.
- **Pastels** (`--pink --cyan --mint --lavender` + `-soft` variants): use **soft
  gradients / ombre** to differentiate sections instead of heavy colour blocks.
  Backgrounds should be white / off-white (`--paper`) with generous whitespace.
- **Typography = Figtree everywhere**, as a **neutral placeholder**. Final typeface is
  TBC with TEP's brand guidance. Fonts are CSS variables (`--sans`) so the final font
  swaps in ONE place. **Do not design layouts that depend on a specific typeface.**
- **Shapes** (the decorative circles/blobs) are **subtle and secondary** — never
  load-bearing. The shape language may change when final brand assets arrive; keep them
  easy to swap.
- **Cards** are light (white + 1px border, `--purple` titles, light/sand pills), 14px
  radius. Reuse the `.card` classes in `tep-system.css`.

## Current state / recently done

- 5 pages migrated from standalone HTML mockups into this component-driven Astro project.
- Homepage highlights: hero (circular photo + slow-drifting brand circles), an
  **expanding image accordion** for the audience/life-stage section, a combined Discover
  section (featured tile + tab-switch), light stats+testimonial, compact "Why TEP".
- Header/nav rebuilt per TEP's own mock-up: white, un-boxed, active underline, dropdown
  flyouts, purple only on Register + active/hover.

## Likely next tasks

- **Mobile / responsive pass** — currently desktop-first (the accordion has a stacked
  fallback; most pages need proper breakpoints).
- **More page templates** as briefed (briefs live in `../Loop Briefs/`).
- **When TEP brand guidance lands:** swap the final typeface (one CSS variable),
  finalise the shape language, confirm colours.
- Optional: extract a `Card.astro` component (cards are currently inline HTML sharing the
  `.card` CSS — consistent, but could be DRYer).
- Replace placeholders (Unsplash photography, placeholder stats/testimonials/copy) if
  raising fidelity.

## Source-of-truth documents (in the parent project folder, outside this repo)

- `../Feedback/13062026/TEP Brand & Visual Direction – Apply Across All Designs.pdf` — brand rules
- `../Feedback/13062026/Homepage Feedback 12.07.2026.pdf` — homepage feedback
- `../Loop Briefs/*.pdf` — the page briefs (product, training, search, results)
- `../POC/Homepage/` — the original standalone HTML mockups these pages came from

## Environment notes (this machine)

- **Node works**; the system **python3, Homebrew and xcrun are broken** — don't rely on
  them. Use Node for any scripting.
- To capture a screenshot of a page, render with headless Chrome against the dev server, e.g.
  `"/Applications/Google Chrome.app/Contents/MacOS/Google Chrome" --headless=new --screenshot=out.png --window-size=1440,2000 http://localhost:4321/<route>`.
- The project lives in iCloud Drive; `node_modules` is gitignored. If sync feels slow,
  the folder can be moved out of iCloud.

---
Prototype by Loop Design for The Education People. Keep it lean, on-brand, and front-end.
