# CLAUDE.md

Working notes for Claude Code on this repository. Read this before changing
anything — it records not just *what* the site does but *why*, because several
decisions look arbitrary until you know the reasoning behind them.

---

## 1. What this is

Jędrzej Cader's personal website. Live at <https://jjcader.github.io>, served by
GitHub Pages from the `main` branch, root folder. Every push to `main`
redeploys automatically in about a minute.

**Who he is.** Mechanical engineer. MSc at ETH Zürich (robotics, design,
aerospace), currently writing his master's thesis at MIT. Previously a Visiting
Student Researcher at NASA JPL working on the Lunar Crater Radio Telescope, and
before that Head Coach and then propellant supply engineer at ARIS, the Swiss
Academic Spaceflight Initiative, on bi-liquid rocket projects HERMES and HELIOS.
Alongside the engineering: co-founder of POLANA, chair for the European Youth
Parliament, nine years of teaching and tutoring, and a decade swimming for the
Polish junior national team.

**What it is for.** A professional portfolio. That framing was arrived at
deliberately after an earlier version leaned too personal — the private archive
lives on Instagram, not here. But it is emphatically **not a CV in HTML**. The
distinction drives most of the layout:

- A CV lists roles. This site shows the *interesting thing the role was*.
- A CV is ordered by employability. This site leads with what a reader would
  actually want to open.
- A CV separates "experience" from "extracurriculars". This site puts POLANA and
  the European Youth Parliament in the same visual register as NASA JPL, because
  they demonstrate the same capabilities and Jędrzej regards them as
  professional work.
- Hobbies exist, at the end, in one clearly-labelled group. They are real but
  they are not the argument.

The reader should finish with a sense of a **person who builds things**, not a
candidate profile.

---

## 2. Hard constraints — do not break these

- **Single file.** Everything is in `index.html`: markup, CSS in one `<style>`
  block, JS in one `<script>` block. Do not split into separate files, do not
  add a bundler, framework, `package.json`, or any npm dependency. This is
  deliberate: Jędrzej is not a web developer, and one file he can read top to
  bottom is worth more than an architecturally correct project he can't
  maintain.
- **No build step.** Opening `index.html` in a browser must always just work.
- **No dependencies** except two Google Fonts loaded via `<link>`.
- **No `localStorage` / `sessionStorage` / cookies.** Fully static, no state.
- **No phone number, home address, or date of birth anywhere on the page or in
  committed files.** This is a public repository and gets scraped. Email and
  LinkedIn only. The CV PDF in `assets/` has already had the phone number
  removed — if it is ever replaced, verify the new one the same way: open it,
  select all, copy, paste into a text editor, confirm the number is absent.
  Visually covering text in a PDF does not remove it.
- **Filenames are always lowercase-with-hyphens.** `helios-1.jpg`, never
  `HELIOS 1.JPG`. The repo is edited from macOS (case-insensitive filesystem)
  and Zorin Linux (case-sensitive), and served from a case-sensitive host. A
  mismatched-case `src` works on the Mac and 404s in production — a genuinely
  confusing bug to chase.

---

## 3. Design system

All design tokens live in the `:root` block at the top of the `<style>` element.
**Change colours there, never inline.**

### Palette

| Token | Value | Role |
|---|---|---|
| `--paper` | `#fffdfa` | Page background — warm white, not pure white |
| `--paper-2` | `#f6f2ec` | Secondary surface: CV strip, hovers, placeholders |
| `--ink` | `#1b1a18` | Body text |
| `--ink-2` | `#5f5b55` | Secondary text |
| `--ink-3` | `#918c85` | Metadata, labels, placeholder text |
| `--rule` | `#e6e0d6` | Hairlines and borders |
| `--accent` | `#10617d` | Deep water blue — links, active states, icons |
| `--accent-2` | `#3a9fb8` | Shallow water — gradient partners, hover borders |
| `--accent-warm` | `#c47a45` | Copper. Used **sparingly**: rocket flame, trajectory arc |
| `--accent-soft` | `#e7f0f4` | Tinted background for active/hover states |

The scheme is **warm neutrals with a cool accent**. An earlier version used
cold blue-tinted greys for surfaces and read clinical; paper and rule tones were
warmed toward sand while the accent stayed blue. Do not neutralise the warmth
back out of `--paper` / `--paper-2` / `--rule`, and do not introduce a third
accent hue — copper is already the second and it is on a tight leash.

The blue is a water reference (Jędrzej swims and surfs) and doubles as a
technical/blueprint tone. That double meaning is intentional.

### Typography

- **Instrument Serif** — display only. Hero name, section titles, group titles,
  category names, timeline roles, contact line, tile titles. Chosen *instead of*
  Playfair Display, which is the default serif of every generated portfolio.
- **IBM Plex Sans** — all body copy and entry titles.
- **IBM Plex Mono** — every piece of metadata: years, tags, section eyebrows,
  nav links, status chips, the progress percentage, captions.

The mono is not decoration. It is the vernacular of engineering documents —
part numbers, revisions, spec sheets — and it is what makes the page read as
belonging to this person rather than to a template. Keep metadata in mono.

### Layout

- `--wrap: 1400px`, gutters `clamp(20px, 4.5vw, 86px)`. Widened from 1140px on
  request; the reference sites Jędrzej liked all use wide layouts.
- Body prose inside entries and timeline cards is capped at ~54–70 characters
  per line regardless of container width, because line length governs
  readability, not container width. **If you widen anything further, do not let
  prose measure grow with it.**

---

## 4. Page structure

1. **Sticky header** — brand, two status chips, vertical divider, nav, mobile
   menu button.
2. **Hero** — full-bleed photo with parallax, gradient veil, serif name,
   one-line positioning statement, bobbing scroll cue.
3. **`#about`** — facts sidebar (left), prose (right), social pills.
4. **`#selected`** — mosaic of six featured tiles.
5. **`#projects`** — five category cards, then five grouped lists of entries.
6. **Photo strip** — auto-scrolling clickable photo cards (not a `<section>`).
7. **`#education`** — three boxed timeline cards.
8. **`#experience`** — five boxed timeline cards, then the compact CV strip.
9. **`#contact`** — email as a large serif line, socials.
10. **Footer** — location, icon links, copyright.
11. Floating: right-hand progress rail, back-to-top button.

Sections carry `EDIT ME #n` comments marking where content goes.

---

## 5. Components

### The featured mosaic (`#selected`)

Six `<button class="tile">` elements in a six-column grid with deliberately
uneven spans: `.t-lg` (4 cols, 16:10), `.t-sm` (2 cols, 3:4), `.t-md` (3 cols,
3:2). The irregularity is what stops it reading as a product grid — **do not
regularise the tile sizes.**

Each tile carries `data-target="eN"` pointing at an entry's `id` below. Clicking
scrolls to that entry, opens it, and flashes it. The mosaic is a *visual layer
over the same content*, never a duplicate — there is exactly one copy of every
project's text.

Tiles are ~60% photograph. A tile with no real image looks worse than no mosaic
at all. If fewer than six good images exist, reduce the tile count rather than
shipping grey placeholders.

### Category cards + groups (`#projects`)

Five `<button class="cat">` cards with `data-goto="g-xxx"` scrolling to the
matching `<div class="group" id="g-xxx">`:

| id | Title | Icon |
|---|---|---|
| `g-space` | Space & rocketry | `#i-rocket` |
| `g-robotics` | Robotics & simulation | `#i-cpu` |
| `g-making` | Making | `#i-tool` |
| `g-people` | Leadership & teaching | `#i-users` |
| `g-hobbies` | Hobbies | `#i-waves` |

Order encodes priority. Hobbies is last and that is correct. When adding or
removing entries, **update the count in both the `.cat-n` line and the
`.group-count` span** — they are hand-maintained, not computed.

An earlier version used filter chips over one flat list. It read as a shopping
list: sixteen rows of identical weight with nothing for the eye to hold onto.
Chunking into named groups with large serif headings fixed it. **Do not
reintroduce a single flat list.**

### Entries

```html
<article class="entry" data-open="false">
  <button class="entry-head" aria-expanded="false" aria-controls="eN">
    <span class="entry-title">…</span>
    <span class="entry-meta">Year · Place</span>
    <span class="entry-toggle" aria-hidden="true">+</span>
  </button>
  <div class="entry-body" id="eN"><div><div class="entry-content">
    <div>
      <ul class="tags"><li>…</li></ul>
      <p>…</p>
      <ul class="entry-links"><li><a href="#">Label <svg class="ic"><use href="#i-external"/></svg></a></li></ul>
    </div>
    <div class="shots">
      <div class="shot wide">…</div>
      <div class="shot small">…</div>
      <div class="shot small">…</div>
    </div>
  </div></div></div>
</article>
```

The triple-nested `div` inside `.entry-body` is load-bearing: the expand
animation uses `grid-template-rows: 0fr → 1fr` with `overflow:hidden` on the
inner wrapper, which animates to auto height without JavaScript measurement.
**Do not flatten those divs.** Entry ids run `e1`–`e17`; new ones continue the
sequence and must be unique, because tiles and photo cards target them.

### Photo strip

A CSS marquee: `.strip-track` animates `translateX(0 → -50%)`. **The card set
must be duplicated exactly once** for the loop to be seamless. Pauses on hover
and on keyboard focus-within. Each card is a button with `data-target` and opens
an entry, same as the tiles.

### Progress rail

Right-hand side, screens ≥1400px. Structure: percentage readout, then a flex row
containing the nav list and a separate `.rail-gauge` column. The gauge being its
own column is the fix for an earlier bug where the rocket overlapped the section
dots — **keep the rocket inside `.rail-gauge`, never in the list column.**

Fill height and rocket `top` are both driven by page scroll percentage in the
shared `requestAnimationFrame` loop. Labels hide below 1800px so the wider
content area has room; dots and gauge remain.

### Icon sprite

An inline `<svg>` of `<symbol>` elements at the top of `<body>`, used as
`<svg class="ic"><use href="#i-mail"/></svg>`. All stroke-based, all inheriting
`currentColor` so they recolour with context. Eighteen symbols currently. Add
more by copying a `<symbol>` with `viewBox="0 0 24 24"` and stroke paths —
**no `fill` attributes**, the `.ic` class handles that.

---

## 6. The decorative layer

`.deco` elements are absolutely-positioned SVGs behind content: orbit rings,
scattered stars, a dashed trajectory arc, wave crests. They give the page
personality without noise, and they mix two motif families deliberately —
**space** (rockets, orbits, stars, trajectories) around the engineering
sections, **water** (waves) around the photo strip, hobbies and contact.

Rules for anything added here:

- `pointer-events: none`, `z-index: 0`, opacity between `.07` and `.16`.
- Content sits above via `section > .wrap { position: relative; z-index: 1 }`.
- Sections are `overflow: hidden` so decor can bleed past edges without causing
  horizontal scroll. Decor placed outside a section must be clipped manually.
- Motion is very slow (200s orbit rotation, 15s star drift) and frozen entirely
  under `prefers-reduced-motion`.

**The brief was "subtle — don't overdo it."** If a decoration is noticeable
before the content is, it is too strong. Reduce opacity rather than deleting it.

---

## 7. Motion

- Scroll reveal: `IntersectionObserver` adds `.in` to `.reveal` elements.
- Hero parallax: the image translates at 0.28× scroll speed. It lives inside
  `<div class="hero-media" id="heroMedia">` — **swap the inner placeholder for
  the `<img>`, not the wrapper**, or parallax breaks.
- Progress bar, rocket position, parallax and back-to-top visibility share
  **one** `requestAnimationFrame` loop guarded by a `ticking` flag, with
  `{passive:true}` on the scroll listener. New scroll-driven behaviour goes into
  `onFrame()` — do not attach another scroll listener.
- Everything is disabled by the `@media (prefers-reduced-motion: reduce)` block.
  **Any new animation must be covered by it.**

---

## 8. Images

Live in `images/`, referenced relatively. Expected names include `hero.jpg`,
`jpl-1..3`, `hermes-1..3`, `helios-1..3`, `mit-1..3`, `swarm-1..3`,
`valves-1..3`, `micropump-1..3`, `snackbot-1..3`, `cnc-1..3`, `polana-1..3`,
`eyp-1..3`, `teaching-1..3`, `swimming-1..3`, `balkans-1..3`, `puzzles-1..3`,
and `strip-1..8`.

**Resize before committing.** Max ~1600px on the long side, under ~400KB:

```bash
mogrify -resize 1600x1600\> -quality 82 images/*.jpg
```

Git stores every version of every binary forever, so an oversized image
committed once is permanent repository weight. Full-resolution originals stay
outside the repo, in OneDrive.

Placeholders look like
`<div class="shot wide"><span class="label">images/x.jpg</span></div>` —
replace the inner span with `<img src="images/x.jpg" alt="">`, keeping the
wrapper for its aspect ratio.

---

## 9. Voice

Entry titles name the interesting thing, not the job title:

- "A radio telescope on the far side of the Moon" — not "Researcher, NASA JPL"
- "Cryogenic valves and a rocket engine test stand" — not "Mechanical Engineer"
- "Chairing 100 people arguing about Europe" — not "Project Manager, EYP"

Prose is first person, dry, specific, occasionally funny, never earnest about
itself. Numbers and technical detail belong *inside* the expanded entry where
they carry weight, not in the title.

Banned: "passionate about", "results-oriented", "leveraged", "cutting-edge",
exclamation marks, and any sentence that would sit unaltered on a LinkedIn
profile. The CV already contains that register; the site exists to be the
opposite.

---

## 10. Accessibility

Maintain what's there: semantic landmarks, `aria-expanded` kept in sync on every
`.entry-head`, `aria-label` on icon-only links, `aria-hidden` on decorative SVG,
visible `:focus-visible` outlines, buttons for actions and links for navigation.
Colour never carries meaning alone. New interactive elements use real
`<button>` / `<a>`, never click handlers on divs.

---

## 11. Workflow

Two machines: macOS (repo inside a OneDrive-synced folder, `gc.auto` disabled
there, folder pinned to "Always Keep on This Device") and Zorin Linux
(`~/Projects/`, outside any sync folder). GitHub is the sync mechanism, not
OneDrive.

```bash
git pull                    # start of every session
# … edit …
git status
git add .
git commit -m "describe the change"
git push                    # end of every session
```

Preview locally with the Live Server VS Code extension or
`python3 -m http.server 8000`. **Never commit merely to see a change** — the
file opens in a browser directly.

---

## 12. Common tasks

**Add a project** → copy an `<article class="entry">` into the right `.group`,
give it a fresh unique `id`, update that group's `.group-count` and the matching
`.cat-n` count.

**Add a category** → copy a whole `.group` block with a new `id`, add a matching
`.cat` card with `data-goto` pointing at it, pick an icon from the sprite.

**Feature something in the mosaic** → change a tile's `data-target`, title, meta
and image. Keep six tiles and keep the size classes as they are.

**Recolour** → edit `:root` only.

**Add a social link** → copy a `.socials` anchor, swap the `<use href="#i-…">`.
Instagram, globe, mail, GitHub and LinkedIn symbols already exist.

---

## 13. Things not to do

- Do not split the file, add a framework, or introduce a build step.
- Do not restore a single flat list of entries in place of the groups.
- Do not make the mosaic tiles uniform.
- Do not promote the CV strip or the timelines up the page — this is not a CV.
- Do not add a third accent colour.
- Do not let decorative elements compete with content.
- Do not add scroll listeners outside the existing `rAF` loop.
- Do not put personal contact details beyond email and LinkedIn into the repo.
