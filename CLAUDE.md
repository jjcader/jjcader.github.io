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
| `--ink-body` | `#38342f` | Prose — darker than `--ink-2`, softer than `--ink` |
| `--ink-2` | `#4c4842` | Secondary text |
| `--ink-3` | `#736d65` | Metadata, labels, placeholder text |
| `--rule` | `#e6e0d6` | Warm hairline — small interior borders (tags, chips, cards) |
| `--rule-blue` | `mix(--accent 26%, --rule)` | Structural hairlines that belong to the blue system |
| `--rule-grad` | gradient | The one divider recipe, horizontal |
| `--rule-grad-v` | gradient | The same stops, vertical — the rail separator |
| `--accent` | `#0a5fd0` | Bright royal blue — links, active states, icons, metadata |
| `--accent-2` | `#16b8c8` | Turquoise — gradient partner, water motif, graphics |
| `--accent-warm` | `#f07a1a` | Bright orange — flames, fills, bars, icons on dark |
| `--accent-warm-ink` | `#b85410` | Darker orange — anywhere orange has to be *read* |
| `--accent-soft` | `#e4efff` | Tinted background for active/hover states |
| `--warm-soft` | `#fdeede` | Orange's equivalent of `--accent-soft` |

**There are two oranges and the split is an accessibility constraint, not a
style choice.** `--accent-warm` (`#f07a1a`) clears only ~3:1 against paper —
fine for a flame, a filled bar, an icon on a dark photo veil, or a border,
all of which are graphics. It is *not* enough for text. Any orange that has
to be read — `.path-org`, hover text on links and pills, the contact line, a
stroked UI icon sitting on paper — uses `--accent-warm-ink` (`#b85410`,
~4.8:1). **When adding a new orange thing, ask whether a person has to read
it**; if yes it takes the ink variant, and if you are unsure, the ink variant
is always the safe answer.

The scheme is **warm neutral surfaces with a vivid two-colour accent system**.
Do not neutralise the warmth back out of `--paper` / `--paper-2` / `--rule` —
the surfaces stay warm and quiet precisely so the accents can be loud. Do not
introduce a third accent hue.

The blue is a water reference (Jędrzej swims and surfs) and doubles as a
technical/blueprint tone. That double meaning is intentional.

**The accents were deliberately moved off "subtle".** The palette began as a
muted deep teal (`#10617d`) with a muted copper (`#c47a45`), and both were
pushed to a bright royal blue and a real orange on request — "I know that this
site is supposed to be subtle, but I like a bit less mellow of a feel." The
brief is now **quiet surfaces, vivid accents**, so the answer to "this looks
flat" is to strengthen an accent, not to tint a surface. A five-way
side-by-side comparison on the category cards picked the azure candidate,
which was then pushed brighter and more royal into the current `--accent`.

**Blue and orange have distinct jobs — keep them apart.** Blue is structure
and state: rules, metadata, icons at rest, the active nav item, links.
Orange is heat, motion and feedback: anything that appears, slides, lifts,
ignites or turns over *because the reader did something*.

**Every hoverable panel on the page runs the same blue-at-rest →
orange-on-interaction move**, and that consistency is the point — it is what
stopped orange reading as "a random second colour". The full set: category
cards (icon + left bar), project entries (left bar + `+` toggle + ignition
ring), mosaic tiles (border + the arrow that slides in), photo-strip cards
(border + arrow), timeline cards (border + the warm end of the wipe bar),
social pills, footer icons, entry links, the contact line, and the
back-to-top rocket. **When adding a new interactive element, give it this
same move** rather than inventing a different feedback colour — and when
adding a new *resting* element, it is blue.

This supersedes the earlier "copper on a tight leash, used sparingly"
framing: the leash is now *semantic* rather than quantitative. The theme is
explicit — blue for the space/water side, orange for flame and heat.

**Structural rules are blue, not warm grey.** There is exactly **one divider
recipe**, in two orientations that share identical colour stops:
`--rule-grad` (horizontal) and `--rule-grad-v` (vertical). Between them they
draw the site header's bottom edge, every `section::after`, the photo strip,
the footer, and the progress rail's vertical separator. They must stay in
sync — the whole point is that these read as the same object turned different
ways. `--rule-blue` is the flat blue-tinted hairline for everything
structural but smaller: the `#about` facts table, social pills, the CV strip,
the back-to-top button.

**A divider must be a pseudo-element, not a `border-top`** — a border can't
fade out at its ends, and the fade is the recipe. This bit `.g-water
.group-head` once: it set `border-bottom-color`, which silently became a
no-op the moment `.group-head`'s rule turned into a gradient `::after`. If
you convert a border to a gradient, grep for anything overriding that
border's colour.

**The stops are `6% / 30% / 70% / 94%`, not an even fade from the ends.** An
even `transparent → accent → transparent` gradient looked fine in the middle
of the screen and was effectively invisible across the outer third, so on a
wide monitor the section dividers read as *missing* rather than as subtle —
reported as "we lost the divider line between Selected and Browse by area."
Keeping ~64% of the width at full strength with short fades at the very edges
gives a real line that still doesn't terminate in a hard edge. **If a divider
is ever reported as invisible again, check where the reader's viewport sits
relative to those stops before assuming the element isn't rendering.**

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

**Mono metadata is set at 500–600 weight, 11px and up, in `--accent` or
`--ink-2` — never 400 weight at 10px in `--ink-3`.** That was the original
setting throughout and it consistently read as faint to the point of
disappearing. Small mono loses contrast twice over: it is small *and* it is
letter-spaced, which thins the apparent stroke further. The fix that worked
was three dials moved together — weight up (IBM Plex Mono now loads 600 for
exactly this), size up by 1–1.5px, and colour moved off `--ink-3` — plus
darkening the `--ink-2`/`--ink-3` tokens themselves. **If metadata still
reads faint after a future change, raise weight and colour before size**;
making it bigger alone starts to compete with the titles it sits beside.

The two timelines' years (`.path-when`) get the strongest treatment on the
page — 15px/600 in `--accent`, centred in their 170px column. They are the
spine of those sections, they sit alone in a wide column, and short values
("2025") looked like an accident when left-aligned in it. The centring is
reverted to left in the ≤700px single-column stack, where there is no column
for it to be centred in.

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
2. **Hero** — full-bleed photo with parallax, gradient veil, serif name, a
   small mono pronunciation line, one-line positioning statement, bobbing
   scroll cue. The name is plain text, no `<em>` — an earlier version
   italicised the surname only, which read as if it were being emphasised or
   were a different name; first and last name now share identical styling.
   The pronunciation line (`.hero-pronounce`, small mono, dim white,
   between the name and the positioning statement) exists specifically
   because "Jędrzej" trips up an English-speaking reader — keep it dry
   ("(yes, really)"), not apologetic, matching the rest of the voice.
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

#### The blue-hue experiment (resolved)

The five cards each briefly carried a different candidate blue via `--cat-hue`,
as a side-by-side comparison on the real page. **Card 03's azure won and was
pushed brighter and more royal into `--accent` (`#0a5fd0`).** The
`.cat:nth-child(n)` overrides are gone; every card now shares that one hue.

`--cat-hue` itself is kept, defaulting to `var(--accent)`, because everything
blue inside a card reads from it — that is what made swapping five hues a
one-line change, and it is worth keeping for the next time something similar
is asked for.

The thing people actually liked about the five-card state was not five blues:
it was **the colour changing under the cursor**. So the variety now comes from
the blue → orange transition on interaction, applied consistently (below),
rather than from per-component hues. Do not give other components their own
resting hues.

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

The strip is deliberately full-bleed (no `.wrap`), which means it sits under
the fixed progress rail's screen column whenever the rail is visible.
`.strip-viewport` gets `margin-right` (270px full mode / 135px compact mode,
matching the rail's footprint above with a safety margin) to keep cards from
sliding under it. **This has to be `margin-right`, not `padding-right`** —
`overflow:hidden` clips at the padding edge, so content in the padding area is
still visible; only shrinking the box itself (which `margin-right` does, on a
`width:auto` block) actually narrows what's visible.

Hidden below 700px (`.strip{display:none}`) — the marquee only pauses on
`:hover`/`:focus-within`, neither of which fires on touch, so cards drift
while you're trying to tap one. That's a real broken interaction, not just an
unpolished one, so it's cut for now rather than left live pending a full
mobile pass.

### Progress rail

Right-hand side, screens ≥900px. Structure: percentage readout, then a flex row
containing the nav list and a separate `.rail-gauge` column. The gauge being its
own column is the fix for an earlier bug where the rocket overlapped the section
dots — **keep the rocket inside `.rail-gauge`, never in the list column.**

#### Sizing — why the breakpoints are what they are

The rail is `position:fixed`, so it overlays the page rather than displacing
it. It therefore has to fit inside the empty margin to the right of the
content, which is:

```
freeRight = max(0, (100vw − var(--wrap)) / 2) + var(--gutter)
```

Two modes, chosen so the rail never lands on text:

| Width | Mode | Rail occupies | Layout |
|---|---|---|---|
| ≥1800px | full | 259px | label pill beside each planet |
| 900–1799px | compact | ≤119px | tiny label *under* each planet |
| <900px | hidden | — | — |

Compact mode's footprint is a ceiling, not a fixed number — `.rail-body` and
`.rail ol`'s gaps are `clamp()`s that shrink as the viewport narrows within
that range (planets pull in toward the rocket, and the whole list compresses)
rather than holding one size until the 900px cliff. 119px is the clamp's
*maximum*, at the top of the range; it only gets smaller from there, so it
never threatens the clearance math below.

**These numbers are hand-derived and must be re-derived if `--wrap`,
`--gutter`, the pill's `min-width`, the planet size, or the rail's `right`
offset change.** The old rail showed from 1400px in one fixed layout, and
between 1400 and 1500px it sat directly on top of the text — that's the bug
these breakpoints fix, so don't lower a threshold without redoing the
arithmetic (there's a Python snippet worth reusing for this: model
`freeRight(vw)` and each mode's exact box-model footprint, then check the
floor of its range).

Compact mode's floor was pushed from 1520px down to 900px on request ("keep
the rail alive longer as the screen narrows, shrink the page instead of
losing the rail") — but the natural gutter alone isn't wide enough for
compact's 123px footprint below ~1520px (at 900px it's only ~40px). The fix
is a **separate** `900–1519px` query that adds `100px` of extra
`padding-right` to `.wrap` — i.e. the page's content column itself gets
narrower in that range, which is "shrink the rest, not the rail" quite
literally. That padding rule is intentionally flat (not `vw`-scaled) because
CSS can't subtract a `clamp()` from a fixed target in one declaration; it only
needs to be safe at the 900px floor, so it leaves unused slack near 1519px —
that's fine.

`.rail` is `pointer-events:none` with `pointer-events:auto` restored on the
links. Without that, the 150px of `padding-left` that reserves room for the
pills would swallow clicks on the page underneath it.

In full mode, `.rail-label` being `position:absolute` means it doesn't
contribute to its parent `<a>`'s own box — so the `<a>`'s clickable area
stopped at the planet, and the gap between the visible pill and the planet
wasn't clickable. **The fix is an invisible `.rail a::before`** sized to the
same 150px reserve, not `padding-left` on the `<a>` — padding-left was tried
first and broke the pill's own position: `.rail-label`'s `right:100%` is
measured against its containing block's *padding box*, so widening `<a>`'s
padding box with `padding-left` pushed the visible pill another 150px further
left than intended, opening a large visible gap between the pill and its
planet. **If extending an element's click area and it also contains an
absolutely-positioned, percentage-offset child, use a separate pseudo-element
or sibling for the hit area — never grow that element's own padding/box**,
since that's exactly the box any percentage offset inside it is measured
against.

`.rail::before` is a hairline that fences the rail off from the page —
deliberately more visible than a standard `--rule` divider: 2px,
`min(100vh,980px)` tall in full mode / `min(94vh,760px)` in compact
(stretching nearly the full viewport height on request), and coloured with
the **same blue gradient as the gauge fill** (`--accent-2` → `--accent` →
`--accent-2`, not the neutral `--ink-3`/`--rule` mix it started as) so the
rail visually reads as one blue system rather than a grey divider next to a
blue bar. The fade at each end is deliberately gradual — stops at `22%`/`78%`,
not a tight `8%`/`92%` — so the line reads as emerging from/receding into the
page rather than switching on abruptly. This is a purposeful section
boundary, not incidental chrome. It's positioned at `right:100%`, i.e. the
rail's own left edge — which is only in the right place because
`padding-left` on `.rail` (not on the `<a>`s — see above) makes the rail's
box actually enclose its absolutely-positioned pills. Drop the padding and
the rule lands in the middle of them.

**The label pills are the default at every width the rail appears at.** They
were once scoped to a `max-width:1799px` query, so on a large monitor the
boxes disappeared and the labels read as loose floating text.

**Unselected items must stay clearly visible, not just the active one.**
Planets were `--ink-3 @ .55` and close to invisible; they're now `--ink-2 @
.8` at 36px (up from 30px). Stroke-width went `1.5→1.7` then back down to
`1.4` — 1.7 overshot into looking heavy-handed once the size and opacity were
already fixed, so don't stack all three dials up at once next time; opacity
and size carry most of the "can I see this" job, stroke is a fine-tuner.
Labels went from `--ink-2 @ .85` to `--ink @ .95`. If a future pass wants the
active state to stand out *more*, push the active item's treatment further
rather than dimming the rest — dimming the rest is what broke this the first
time. `.rail ol`'s `gap` also went `38px→46px`, making the whole rail taller
so there's more air between markers ("more separation between sections") —
purely vertical, doesn't touch the horizontal clearance math above.

**`.rail-body` carries an explicit `height:min(66vh,560px)`** — without it,
the track's physical length was whatever `ol`'s content-plus-gaps happened to
add up to, which meant compact mode's smaller gaps also made the *track*
shorter, not just the spacing between planets. `align-items:stretch` stretches
both `ol` and `.rail-gauge` to fill that height regardless of mode, and
`justify-content:space-between` spreads the planets across it.
**If the rail ever needs to look taller or shorter, change this one height** —
not the gaps. The vertical gaps are now only a *floor*; space-between supplies
the real spacing, so changing them no longer changes the track's length at all.

⚠ **That height must be ≥ the `ol`'s natural content height, and getting this
wrong breaks three things at once in a way that looks like three separate
bugs.** It was first set to `min(58vh,446px)` while full mode's six planets
plus their then-46px gaps needed 530px. The consequences, all from that one
number:

1. The `ol` overflowed its box downward, so **the planets stopped being
   vertically centred** even though the rail around them still was.
2. `.rail-gauge` stretches to the *box*, not to the overflowing content, so
   **the track ended up shorter than the span of dots it measures**.
3. The rocket's position is computed against dot positions and then clamped
   to the track, so **it stopped ~60px short of Contact** and the fill maxed
   out early.

The defence is to keep the vertical `gap` values *small* — a floor, not the
real spacing — so that even when the `vh` term shrinks the height on a short
viewport, content still fits and space-between just distributes less. There
is a Python snippet worth reusing here: model `H = min(0.66*vh, 560)`, each
mode's per-item height and gap floor, then assert `content + 5*gap <= H` and
that the gauge's `14px` inset margins still enclose the first and last dot
centres, across a range of viewport heights.

Compact mode deliberately gets the **same** height as full mode rather than a
smaller one — "keep the rail alive longer as the screen narrows, shrink the
page instead" applies to width only. Narrowing the window costs horizontal
room (`.rail-body`'s gap is still a `clamp()` that pulls the planets in
toward the rocket); it must never cost vertical room, because the vertical
extent is what makes the gauge legible as a progress indicator.

**`.rail-pct` is `position:absolute` (`bottom:100%`), not a block in flow.**
`.rail` centres itself with `top:50%` + `translateY(-50%)`, which centres its
whole box — so while the readout was in flow, the thing being centred was
"readout + planets", and the planet column sat ~18px below true screen
centre. Taking the readout out of flow makes `.rail`'s box exactly
`.rail-body`, so what centres is what the reader actually looks at. It also
means `.rail::before` (the separator, `top:50%`) centres on the planets too.

Fill height and rocket `top` are **not** driven by raw page-scroll percentage —
that was tried first and drifted out of sync with the dots, because sections
have very different heights (`#projects` alone holds all seventeen entries) so
a linear scroll fraction doesn't land on the right dot. Instead, each frame:
find which section contains a fixed anchor point 45% down the viewport,
compute how far through that section the reader is, then interpolate between
that section's dot and the next dot's *actual measured screen position*
(`getBoundingClientRect`). This guarantees the rocket always sits on (or
between) the correct dots regardless of section length. **Do not go back to a
page-scroll-percentage formula for the rocket/fill** — reading real dot
positions is what keeps it correct.

**That same computed `idx` also drives which nav link gets `is-active`** —
for both `.site-nav` and `.rail`, in the same `onFrame()` pass, not via a
separate `IntersectionObserver`. An observer-based scrollspy was the original
implementation and could disagree with the rocket: it fires once per section
as it crosses a rootMargin band, using its OWN, slightly different reference
geometry, and during a fast scroll can process a batch of entries out of
visual order, leaving a stale section marked active — reproducible by
scrolling from `#experience` up into `#education` and landing fully in
`#education` while `#experience` stayed highlighted. **Don't reintroduce a
separate observer/mechanism for nav highlighting** — there must be exactly
one calculation for "which section is current," or the rocket and the
highlighted label can drift apart again.

Having one calculation wasn't quite enough on its own, though: a second,
subtler version of the same class of bug showed up as clicking a nav link
sometimes leaving the *previous* section highlighted even once the page had
visibly scrolled to the right place — reproducible occasionally, not always,
and self-correcting the moment you scrolled manually afterward. The cause was
a units mismatch, not a logic mismatch: the click handler computes its scroll
target from `getBoundingClientRect().top + scrollY` (sub-pixel), while the
`idx` loop was comparing that against `s.offsetTop` (layout-tree-relative,
rounded to an integer). The two numbers describe the same point but can
differ by a fraction of a pixel, and right at a section boundary that's
enough to miss the `readerY >= offsetTop` threshold. Normally a near-miss like
that self-heals on the next scroll event — but a `behavior:'smooth'`
`scrollTo()` fires its *last* scroll event exactly at its resting position,
so if that final position is the one that misses, nothing scrolls again to
correct it, and the wrong section stays lit until the reader moves the page
by hand. The fix: the `idx` loop now reads each section's position with
`getBoundingClientRect()` too (plus a 1px epsilon for general safety), so the
click target and the highlight threshold are always measured with the same
API at the same precision. **Any code comparing a scroll-derived position
against a section's position must get both numbers from the same
measurement API** — mixing `getBoundingClientRect()` with `offsetTop`/
`offsetHeight` reintroduces exactly this class of rare, self-healing-until-
it-isn't bug.

**While a nav-click's smooth scroll is in flight, `onFrame()` reads the
anchor from the DESTINATION (`navJumpY`) instead of the live scroll
position.** Without that, the highlight only changed at the very end of the
animation — the reader anchor has to physically cross the target section's
top edge before that section becomes current, and for the whole ~500ms of the
scroll it is still inside the *previous* section. Clicking "Contact" lit
"Experience" for half a second first, which read as lag.

**This is not a second mechanism for "which section is current"** — the rule
above still holds, there is one `idx` calculation driving both the rocket and
the highlight, and all that changes during a jump is *which scroll position
is fed into it*. That distinction is the whole reason this fix is safe: feed
the one calculation a different input, never add a second calculation. The
jump ends when the scroll arrives (within 2px), on a timeout in case the
browser stops short, or immediately if the reader grabs the page — `wheel`
and `touchstart` cancel it, so a manual scroll always beats an animation in
progress. Those are input listeners, not `scroll` listeners; the rAF loop is
still the only thing bound to `scroll`.

Because the rocket is driven from the same `idx`, it crosses the whole rail
at once during a jump — so `.rail.is-jumping` lengthens its `top` transition
to roughly the length of the smooth scroll, or the 0.12s tracking transition
would read as a teleport.

**At the very bottom of the document, `idx`/`frac` are forced to the last
section and `1`.** The reader anchor sits 45% down the viewport, so at max
scroll it is still short of `#contact`'s own bottom edge by the footer's
height plus the remaining 55% of the viewport — meaning `frac` never
organically reached `1`, the end-of-track overshoot below never fired, and
the fill stalled a few percent short while the readout beside it already
said 100%. Any future "have we finished the page" check needs the same
treatment: the anchor point is not the bottom of the viewport, so it never
reaches the bottom of the last element on its own.

The rocket also **deliberately overshoots past the track's ends** at the very
top and bottom of the whole scrollable range (before `#about`, past
`#contact`) — forced to `-OVER_TOP`/`trackHeight+OVER_BOTTOM` rather than
merely clamped to `0`/`trackHeight`, so the nose/flame visibly clears the
track with no unfilled rule showing past it. This only triggers when
`idx===0/last && frac===0/1` — i.e. truly at an end, not just near a dot —
so normal interpolation between dots is unaffected. **The two overshoot
amounts are intentionally different (top `0px`, bottom `13px`), not a
typo** — the nose is a narrow pointed shape and the flame is a wide rounded
one, so the same pixel overshoot reads as "way too much rocket" at the
pointed end and looks right at the rounded one. Top went through `5px` before
settling at `0`: even with zero *forced* overshoot, the nose still visibly
clears the track on its own, because the rocket icon isn't vertically
symmetric around its own bounding-box centre — the nose sits well above the
icon's geometric middle (window circle) while the flame sits well below, so
centring the 44px box on the track position already puts the nose several
pixels above the line before any forcing is added on top. Forcing on top of
that inherent offset was doubling up and reading as "way too much rocket" at
the top specifically; the bottom doesn't have the same problem because the
flame is much closer to the icon's true visual weight, so its `13px` forced
overshoot is additive on a nearly-zero inherent offset instead. If the top
ever needs to poke out *more* again, prefer nudging the icon's own vertical
centring before reaching back for a positive `OVER_TOP`. If either end starts
looking off again, adjust that end's constant, not both together.

**Nav-link clicks (`.site-nav a`, `.rail a`) don't use native anchor
scrolling** — they're intercepted (`preventDefault` + `window.scrollTo`) to
land at the same 45%-down-viewport anchor point `onFrame()` reads for
`idx`/`frac`, not at the section's raw top edge. Native anchor scroll (via
`scroll-padding-top`) lands the target's top just under the header, which is
a *different* reference point than the rocket uses — arriving that way means
the section is already a few hundred pixels into its own "current" range on
arrival, so the rocket sits visibly past its own dot right after you click
it, which read as unnatural. Scrolling to the 45% anchor instead makes a nav
click and organic scrolling agree on frac=0 for the same landing spot, the
same way §"single source of truth" above makes the rocket and the highlight
agree. `scroll-padding-top:96px` on `html` is left in place as a fallback for
real anchor navigation this doesn't intercept (a direct link to `#about`,
browser back/forward) — it's fine that this is a different offset from the
JS one, since it's a secondary path, not the primary interaction.

**The labels must stay visible at all times** in every mode, never hover-only
— people need to read the section names to navigate by them. **Don't go back
to a blanket `display:none`, or to hover-only visibility, on `.rail-label`**
— both were tried and made the rail unusable as navigation. Every planet/label
pair is a single `<a href="#section">`, i.e. already a real link — no separate
click handler needed when extending this.

The rocket's flame (`.rail-rocket .flame` / `.flame-core`) is two nested
teardrop `<path>`s, not a stroked line — a straight line has no fill area, so
if it's flattened back to one path it will disappear rather than shrink. Outer
flame is solid `--accent-warm`; inner core is that same token blended toward
`--paper` via `color-mix()`, not a new colour — keep any future flame tweaks
to variations on those two tokens, not a new hue.

The section markers are **tiny planets and moons**, not plain dots — six
distinct `#p-*` sprites (ringed planet, crescent moon, banded planet, cratered
moon, second ringed planet, planet-with-companion), one per section, all at the
same visual weight so the column still reads as one set of markers. The active
state is colour + `scale()` + a `drop-shadow` glow. **It must not go back to a
`box-shadow` ring** — a spread ring grows outward into the label pill next to
it and collides with it; `drop-shadow` blurs outward without claiming layout
space. The labels themselves carry `min-width` + `text-align:center` so every
pill is identical in size regardless of how long the section name is.

### Icon sprite

An inline `<svg>` of `<symbol>` elements at the top of `<body>`, used as
`<svg class="ic"><use href="#i-mail"/></svg>`. All stroke-based, all inheriting
`currentColor` so they recolour with context. Add more by copying a `<symbol>`
with `viewBox="0 0 24 24"` and stroke paths — **no `fill` attributes**, the
`.ic` class handles that.

Three families, distinguished by id prefix:

- `i-*` — real UI icons used through `.ic`.
- `p-*` — the progress rail's planet/moon markers.
- `d-*` — decoration-only. These carry `vector-effect="non-scaling-stroke"`
  **on their own paths**, because CSS can't reach inside a `<use>` shadow tree
  to set it (see §6). Any new decoration symbol needs that attribute or it
  will draw a hugely thick stroke when scaled up.

---

## 6. The decorative layer

`.deco` elements are absolutely-positioned SVGs behind content. Three ways to
build one, in order of preference:

1. **Orbit rings** (`deco-orbit*`) — concentric circles + a tilted ellipse +
   a filled core and one or two satellite dots, always spinning at
   200s/rotation. **This is the house motif and the thing to reach for first.**
   The one at the top-right of `#selected` is the reference example. Vary the
   ring count, ellipse tilt and satellite placement between instances so they
   aren't clones, and feel free to hang extra things on one (a planet at the
   centre, a rocket in orbit) — that's the intended way to get variety, rather
   than inventing an unrelated new shape.
2. **Sprite reuse** — `<use href="#d-rocket"/>` or `<use href="#d-satellite"/>`,
   scaled way up. Use the `d-*` decoration variants, never `i-*` directly: they
   carry `vector-effect="non-scaling-stroke"` so they stay hairline at any size
   (see the stroke-width note below). `#d-rocket` is reused because it already
   means something elsewhere (the to-top button, the progress rail);
   `#d-satellite` — a central square body, a rod out of two opposite sides,
   a rectangle (panel) on each rod end — exists purely for this layer, since
   nothing else in the UI needed a satellite. Both are drawn simply on
   purpose: at decoration scale, fine detail disappears anyway.
3. **Hand-drawn one-offs** — trajectory arcs (dashed `stroke-dasharray:1.5 11`
   + `stroke-linecap:round` paths — small round dashes read as a dotted flight
   path; the earlier flat-cap `4 12` read as broken tick marks), or bespoke
   content-specific glyphs, each parked beside the thing it refers to rather
   than floating generically: the telescope-aimed-at-a-moon and the quadcopter
   sit behind the JPL and swarm tiles in `#selected`, the robot behind the
   robotics group, the Rubik's cube by the hobbies group where the puzzles
   entry lives. Build a one-off when the content calls for something
   *specific*, not as a default.

`deco-traj`'s arc has a filled dot at the launch end and a **hollow** ring at
the other — give a trajectory two different markers, not two identical dots,
so it visibly reads as "from here, to there" rather than just being a curved
line with decoration at both ends.

**Any hand-drawn glyph built from several rigidly-connected parts (a tube +
its mount, a body + its limbs) should rotate as ONE `<g transform="rotate(...)">`
wrapping all of them, with every part's coordinates written in that group's
local space — not each part given its own independent `transform="rotate(...)"`.**
`deco-telescope` originally did the latter (the tube, the finder scope and the
eyepiece each had their own separate rotation, around three slightly different
pivot points), and even with the same angle on all three, independent pivots
meant they didn't stay correctly attached to each other. One shared group with
one shared pivot makes that whole class of misalignment impossible — parts
drawn touching in local coordinates stay touching after rotation, always.

A part that must stay unrotated regardless of the assembly's angle
(`deco-telescope`'s tripod mount) sits outside the group, anchored to a point
computed by applying the same rotation to the assembly's actual attachment
coordinate — **that point should be the assembly's true geometric centre, not
an eyeballed guess.** The mount was originally attached at a hand-picked point
that turned out to read as biased toward the objective end; it's now anchored
to the tube's actual midpoint (local x=39, the middle of the 12–66 span),
rotated through the exact same transform as the tube. If a similarly-composed
glyph's mount/leg/stand looks off-centre, check whether its attachment point
was computed from the assembly's real geometry or just guessed.

The same tube also carries a centreline marking where its wide and narrow
portions meet (`M39 48.5V60`, inside the rotated group). It has to be
anchored at that same `x=39` the mount uses, not just "wherever it looks
about right" — the two are meant to visually meet at the tripod, and any
offset between them (it was at `x=40` before, one unit off) puts the
centreline's rotated position several units away from the mount's rotated
position once the `-34°` transform is applied to both, since only the
rotation's exact pivot point stays fixed under rotation — everything else
moves, by an amount that grows with its distance from the pivot. Its two
endpoints are also computed from the tube's actual slanted top/bottom edges
at `x=39` (`48.5` and `60`) rather than a rounder-looking guess, the same
edge-interpolation fix already applied to the finder-scope strut below — a
flat guess is what let the earlier version poke a bit past the tube's own
top surface.

A part that's meant to *touch* another rigidly-rotated part but isn't itself
inside the rotation group (`deco-telescope`'s finder-scope support strut,
connecting to the tube's slanted top edge) needs its endpoint computed
against that edge's actual position at that x, not a flat guess — the tube's
top edge is a slanted line (`(12,51)→(66,46)`), so "where does the strut end"
is a linear interpolation, not a constant. Using a flat guess there is what
originally left the finder scope reading as disconnected from the tube.

The moon reads as "further away" through distance and scale, not size alone —
it's deliberately small (`r=14`/`10`, down from an earlier oversized `r=22`)
and pushed almost entirely off the corner of the viewBox so most of it bleeds
past the edge, on a canvas shrunk back down (`300px`/`28vw`, down from an
earlier `420px`/`40vw` — that width was wide enough to overflow and break
layout in narrow/compact viewports) so it partially tucks behind the JPL
mosaic tile sitting in front of this deco — "further away, partially behind
the image" was the brief; making the moon itself bigger had actually pushed
the opposite reading (closer, more prominent), so shrinking it while keeping
it corner-bled is what sells "distant." Its stars are `<use href="#i-star"/>`
(the sprite's proper 4-point
sparkle, filled via `.deco-telescope svg use{fill:currentColor;stroke:none}`),
not hand-drawn diamond `<path>`s — the diamonds were a workaround for a
vector-effect concern that turns out not to apply here: that concern is only
about *stroked* shapes scaling their stroke-width, and these stars are
rendered with `stroke:none`, so there's no stroke to scale in the first
place. **`<use href="#i-star"/>` with `fill:currentColor;stroke:none` is safe
to reuse anywhere a small filled star accent is wanted** — reach for that
before hand-drawing another star shape.

`#d-rocket`'s flame starts and ends exactly on the body's bottom edge
(`9.6,17` → `14.4,17`) so the outline is continuous. It was originally a
closed teardrop floating just below the body, which left a visible hole at the
engine end — **if you redraw it, keep both endpoints on that edge.**

The fins are **wide closed triangles whose root edge lies along the body's own
outline**: `M8.4 14.4 9.6 17 7 20.8Z` and its mirror. The top vertex
`(8.4,14.4)` is a point evaluated **on the body's left bezier at t=0.5**, the
bottom vertex `(9.6,17)` is the body's bottom-left corner, and the tip sweeps
down and outboard from there.

This took three attempts, and the two failures are worth keeping because they
are the two *different* ways this geometry can go wrong:

1. **An open zigzag** (`M9.6 17 7.7 20.2l2.6-.9`) whose return leg came back
   in to `x=10.3`. The flame's left curve bulges out to about `x=9.4`, so
   that vertex was **inside the flame**.
2. **A triangle with a flat third vertex at `(9.6,15.3)`.** Correct on the
   flame, wrong on the hull: the body tapers, and by `y=15.3` its edge has
   already pulled in to about `x=8.8` — so `x=9.6` at that height is
   **inside the body**.

A single stroked flare line avoids both hazards but reads as a wire rather
than a fin, so it isn't an option either.

**The rule that satisfies everything: put the root's vertices on the hull's
real curve (evaluate the bezier, don't guess a round number), and keep the
tip strictly outboard of the body's widest point.** A fin that only ever
moves outward and downward from a root on the hull cannot re-enter either the
hull or the flame, because both are confined to the body-width column it is
moving away from. When checking a change, check the diagonal *edges*, not
just the vertices — an edge between two legal vertices can still cross the
flame if it cuts the corner.

The rail's own inline rocket (`#railRocket`'s `<svg>`, a separate hand-drawn
shape with a slightly different body bezier, not a `<use>` of `#d-rocket`)
has had every one of these bugs and fixes in parallel — its root vertex is
re-derived against *its own* curve (`8.3,14.4`), not copied. **Keep the two
in sync if either's fins change, but re-derive rather than copy the numbers.**

### Stroke width — the thing that makes or breaks these

`.deco svg *{vector-effect:non-scaling-stroke}` makes every deco shape draw at
the same on-screen hairline no matter how far its viewBox is scaled up.
Without it `stroke-width` is multiplied by the scale factor, so a 24-unit icon
blown up to 200px drew an ~8px stroke while a 400-unit orbit ring at the same
size drew ~1px — which is exactly why the background rockets once looked far
heavier than everything around them. **Don't remove that rule, and don't go
back to hand-tuning tiny per-class `stroke-width` values to compensate.** The
rule can't reach inside a `<use>` shadow tree, which is why the `d-*` symbols
carry the attribute on their own paths instead.

### Orbiters — a rocket or satellite actually riding a ring

Almost every orbit ring on the page now carries at least one `<g
class="orbiter">` wrapping a `<use>` of `#d-rocket` or `#d-satellite`,
positioned at the top of one of the ring's circles and spinning around the
ring's centre (`.orbiter{animation:spin-slow 80s...;transform-origin:200px
200px}` — a different rate from the ring's own 200s spin, so the two never
fall back into sync). Two rings (the education ring, and the `#selected`
favourite ring) carry **two** orbiters each, riding different circles: the
second one adds class `rev` (`.orbiter.rev{animation-direction:reverse}` —
plays the same `spin-slow` keyframe backwards, no separate keyframe needed)
so the pair visibly counter-rotate rather than reading as one shape trailing
another. **There's no cap on how many orbiters one ring can carry** — the
request that produced the second ones was explicitly "no problem having two
on one symbol, orbiting in different directions," so treat that as
permission, not a special case.

**`.orbiter` carries `fill:var(--paper)`, and that's load-bearing.** Every
other deco shape is a bare stroke outline (`fill:none`), which is fine when
nothing crosses behind it — but an orbiting body visually crosses the ring
it's riding, and document order alone (later paint = on top) doesn't stop an
*unfilled* shape from looking pierced by a stroke that's technically behind
it, because there's nothing opaque to occlude it with. Giving the orbiter a
real fill is what makes it read as flying in front of the ring rather than
being skewered by it. **Don't drop that fill to "match" the rest of the
hairline system** — the reason every other deco *can* stay unfilled is that
nothing else in this layer overlaps another deco shape by design.

A rocket orbiter also needs an inner `rotate(90 cx cy)` on top of the outer
`.orbiter` rotation — `#d-rocket`'s nose points "up" in its own coordinates,
and the 90° turn is what makes it tangential to the ring (flying alongside
the orbit) instead of pointing straight out of the planet. `#d-satellite` is
symmetric enough along its rod axis that it doesn't need the equivalent turn.

### Motifs and placement

Two families deliberately — **space** (rockets, satellites, orbits, planets,
trajectories) around the engineering sections, **water** (full-bleed wave
paths) around the photo strip, hobbies and contact.

### ⚠ Ongoing colour experiment — expect this to possibly get reverted

`.warm` (switches a deco's `color` from `--accent` to `--accent-warm`) is
currently applied to roughly half the deco elements, **interleaved within
sections rather than assigned by whole section**, as an explicit,
flagged-as-temporary experiment ("test having half the decorations a
different colour... we might revert later", later escalated to "mix and match
the colours more... switch them up more"). It is **not** a settled design
decision the way the rest of this file's rules are. If a future session is
asked to revert it: remove `warm` from the `class` list on each deco div
listed *(warm)* in the table below — `deco-traj` and `deco-traj-2` keep it
regardless, they were warm before this experiment and aren't part of it.
Section 3's "copper, used sparingly, on a tight leash" rule for
`--accent-warm` is exactly the constraint this experiment is testing against;
don't treat this section as having quietly overridden that rule until the
experiment is confirmed to stay.

**No scattered star fields.** Two versions were tried — plain `<circle>` dots
and `#i-star` sparkles as a standalone field — and both read as dirt on the
screen rather than as sky. Where one used to sit, an orbit ring goes instead.
The realised version of "stars as a deliberate accent inside another deco" is
`deco-telescope`'s eight `<use href="#i-star"/>` stars around the moon (see
the one-offs section below for why `<use>` is safe here despite the general
`vector-effect`-can't-reach-`<use>` caveat — short version: these are filled,
`stroke:none`, so there's no stroke to scale in the first place).

Every content section carries at least one `.deco`, and long sections carry
several so the motif recurs as you scroll rather than appearing once at the
top and vanishing. Essentially every orbit ring on the page now carries at
least one orbiter (see above) — treat "plain, orbiter-less ring" as the
exception going forward, not the default.

| Section | Decorations, roughly top to bottom |
|---|---|
| `#about` | `deco-orbit-about` *(warm)* — bottom-**left**, bled off the corner, satellite orbiter |
| `#selected` | `deco-orbit` — top-right, the reference, rocket + counter-rotating satellite; `deco-telescope` *(warm)* (left); `deco-drone` (left, low, level with the swarm tile); `deco-orbit-sm` *(warm)* — bottom-right, satellite orbiter |
| `#projects` | `deco-waves-proj` (a wavy set across the top, above "Browse by area"); `deco-traj` *(warm)* (right); `deco-orbit-left` (left, behind space & rocketry, satellite orbiter); `deco-rocket-b` (left, 33%); `deco-robot` *(warm)* (left, 46%, behind robotics & simulation); `deco-orbit-mid` (right, rocket orbiter); `deco-cube` *(warm)* (left, 76%, by the hobbies group where the puzzles entry lives); `deco-orbit-2` *(warm)* (bottom-left, satellite orbiter) |
| photo strip | `deco-waves-top` |
| `#education` | `deco-orbit-edu-2` (top-left, rocket + counter-rotating satellite); `deco-orbit-edu` *(warm)* (bottom-right, satellite orbiter) |
| `#experience` | `deco-orbit-4` (top-left, satellite orbiter); `deco-orbit-3` *(warm)* (right, bled almost fully off-canvas, satellite orbiter); `deco-rocket` (left, mid); `deco-traj-2` *(warm)* (bottom-left) |
| `#contact` | `deco-orbit-contact` *(warm)* (top-left, satellite orbiter); `deco-waves` (bottom) |

*(warm)* = part of the ongoing colour experiment, see below — expect these
tags to go stale if it's reverted. The assignment was deliberately reshuffled
once already (originally applied by whole section — every deco in
about/projects/experience warm, everything else blue — which read as blocky;
now interleaved within each section too) on the explicit request to "mix and
match the colours more." **If asked to mix further, keep editing
individual elements' `warm` class, don't go back to toggling it by section.**

Suffixes (`-2`/`-3`/`-4`/`-b`/`-mid`/`-left`/`-edu`/`-about`/`-sm`/`-contact`)
exist so a repeated motif never sits at an identical offset twice; follow that
pattern rather than reusing one class's exact position in two places.

`#projects` is the crowded one — five decorations down its left edge. Because
they're placed by `top:%` against a section whose height depends on how many
entries exist, **adding or removing entries shifts them all**. The current
spacing assumes the entries are collapsed (~2150px tall) and leaves 45–80px
between neighbours; if entries are added, re-check the gaps.

**Never let two decorations overlap each other.** This has been flagged
repeatedly and is the single easiest way to make the page look cluttered: a
rocket landed on top of an orbit ring in `#projects`, and another sat on the
trajectory arc in `#experience`. When adding one, work out the vertical band
it occupies (`top`/`bottom` % × the section's real height, plus its own
height) and check it against every other deco in that section. Opposite sides
of the page don't count as overlapping.

**Keep decor — especially rocket-shaped decor — off the right edge in the
vertical band the fixed progress rail occupies (roughly the middle third of
the viewport, at any scroll position, ≥1400px).** A background rocket sitting
near the rail's own rocket reads as the two arguing with each other, not as
atmosphere; a background orbit ring there is a smaller problem but still
competes with real navigation UI. Where a right-side element is kept anyway
(`deco-orbit-3` in `#experience`), bleed nearly all of it off-canvas so only a
sliver shows. Prefer the left edge for anything new.

**Don't put two full-width wave paths (`deco-waves`/`deco-waves-top`-style)
close together on the page** — that was tried in `#projects` right above the
photo strip's own wave and read as a mistake, not a motif, once you scrolled
past both in the same screenful. The three that exist (`deco-waves-proj` at
the top of `#projects`, `deco-waves-top` on the photo strip, `deco-waves` at
contact) are each separated by most of a section's height.

A small contained wave *glyph* was tried as a middle ground near the hobbies
group and cut — it read as a leftover next to the orbit rings rather than as
part of the set. The water family is now full-bleed wave paths only; the
`#d-wave` symbol that backed it has been deleted. **Don't reintroduce a small
wave icon** — if a section needs more presence, add an orbit ring.

Full-bleed wave paths are drawn from `x=-200` out to `x=1400` inside a
`0 0 1200 …` viewBox, and wrapped in `<g class="wave-g">`. Both details are
load-bearing for the `wave-drift` animation: the overhang means the sideways
drift never exposes a bare edge, and animating the inner `<g>` (rather than
the `<svg>`) keeps the SVG's own clip still so the overhang scrolls into view.
**If you add a wave, copy that structure** — a path drawn 0→1200 directly on
the `<svg>` will visibly tear at the right edge as it drifts.

**Don't place decor directly behind dense text blocks** (a facts list, a
tight paragraph) — even at low opacity it reads as clutter once you consciously
notice it, which is exactly what happened with the original stars behind the
`#about` facts list. Bleed decor off a corner or edge so it sits mostly in the
page's outer margin instead, `deco-orbit-about` being the fix for that
specific case.

Rules for anything added here:

- `pointer-events: none`, `z-index: 0`, opacity roughly `.19`–`.28`, base
  stroke-width `1.6` (raised in three rounds from an original `.07`–`.16` @
  `1.2` — every round of feedback said the previous range still read as too
  faint. Treat `.19`–`.28` as the current working range, not a ceiling: if a
  future pass says "still can't see it," the answer is to raise the range
  again, not to assume this one is final). Now that everything strokes at a
  uniform hairline, the rockets no longer need to be held at a lower opacity
  than the rings to stop them shouting.
- Content sits above via `section > .wrap { position: relative; z-index: 1 }`.
- Sections are `overflow: hidden` so decor can bleed past edges without causing
  horizontal scroll. Decor placed outside a section must be clipped manually.
- Motion is very slow and frozen entirely under `prefers-reduced-motion`.
  Three keyframes are in play, and new decoration should reuse them rather
  than inventing a fourth: `spin-slow` (200s, every orbit ring; also 80s on
  `.orbiter`, so a rocket in orbit compounds the two rates into something
  that never quite repeats), `wave-drift` (16s sideways, on `.wave-g`), and
  `drift` (15s vertical bob, on the drone). Rotations that need to coexist
  with a keyframe go on the inner `<svg>` (see `.deco-drone svg`), because an
  animation on the wrapper would otherwise overwrite its `transform`.

**The brief moved in stages: "subtle — don't overdo it" → "more pronounced,
more of it" → "still not enough, be more creative, more flair."** Read that
trajectory as the standing direction, not any single snapshot of it — when in
doubt, add another orbit ring rather than holding back. The one constraint
that hasn't moved: don't reach for a third accent colour, and don't let a
motif actually overlap content people are trying to read or click.

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
  **Any new animation must be covered by it.** That block kills all
  `animation` and `transition` globally, which covers anything CSS-driven for
  free — but **it cannot stop JavaScript from writing inline styles**, so
  every JS-driven effect below also carries its own `if(reduced) return`
  guard. A new JS effect needs both.

### The rocket's flame and lean

The flame's length (`--flame`) and the rocket's lean (`--tilt`) are driven by
scroll **velocity**, not position: `onFrame()` diffs `y` against `lastY`,
normalises it, and eases each value toward its target so a jerky wheel
doesn't make the rocket twitch. Both are written as custom properties and
consumed entirely in CSS, so no layout is touched.

Two details are load-bearing. The rotation goes on the **inner `<svg>`**,
because `.rail-rocket`'s own `transform` is already spoken for by its
`translate(-50%,-50%)` centring — the same rule as the decorative layer. And
the flame paths use `transform-box:fill-box` with `transform-origin:50% 0`,
so scaling happens from the flame's own top edge where it meets the engine;
without `fill-box` the origin resolves against the whole SVG viewport and the
flame inflates around its middle, detaching from the body.

**The rAF loop re-schedules itself while the flame is still easing back to
rest.** Scroll events have stopped by then, so nothing else would keep it
running and the flame would freeze at whatever length it had when you
stopped. Any future velocity-decay effect needs the same self-scheduling.

### Decorative parallax

`.deco` elements drift against the scroll at 0.07× (orbit rings) or 0.12×
(one-off glyphs) so the background sits at a real depth. Two families are
**excluded and must stay excluded**:

- **the full-bleed waves**, which are anchored to a section's top or bottom
  edge — moving them vertically exposes a bare strip;
- **the drone**, whose *wrapper* carries the `drift` keyframe. Writing
  `style.transform` on it would fight the animation for the same property.
  Inner-`<svg>` animations are fine, which is why every spinning orbit ring is
  safe — so the test for a new deco is *"is the animation on the wrapper?"*,
  not *"is it animated?"*

Positions are measured **once** (and on `load` and `resize`) by walking the
`offsetTop`/`offsetParent` chain, which is layout geometry and therefore
unaffected by the transform being written. Measuring with
`getBoundingClientRect()` here would feed each frame's own offset back into
the next frame's measurement and drift steadily off.

### Ignition ring, count-up, envelope

- **Ignition ring** — a ring expands out of an entry's `+` each time it opens
  *or closes*, orange for the engineering groups and turquoise under
  `.g-water` so the accent matches the group's motif. The class is removed,
  a reflow forced, then re-added: re-adding a class that is already present
  is not a state change, so without the reflow the animation would not re-run
  on a quick open/close.
- **Count-up** — the category entry counts roll up when scrolled into view,
  driven from **the existing `revealer` observer**, not a second one. Same
  reasoning as the single-`idx` rule: one "this became visible" mechanism.
  The literal number stays in the markup as the no-JS fallback, and the count
  is zero-padded back to its original width. Note this means each `.cat-n`
  now holds a `<span class="count" data-n="N">` — **when updating a category's
  entry count, update `data-n` as well as the visible text.**
- **Envelope flap** — the mail icon's flap lifts on hover. CSS still cannot
  reach inside a `<use>` shadow tree to transform one path of a symbol, but it
  *can* transform the `<use>` element itself, so the envelope is split into
  two `<use>`s (`#i-mail-box` + `#i-mail-flap`). Those two are `<g>` inside
  `<defs>`, **not `<symbol>`**: a `<use>` of a `<symbol>` with its own
  `viewBox` re-establishes a viewport and remaps coordinates, which makes the
  flap's `transform-origin` unpredictable; a `<use>` of a `<g>` is copied
  straight into the current coordinate system. Flipping the flap about its own
  top edge turns the V into a Λ standing above the box — an open flap for
  free, with no second drawing of the icon. **`<g>` in `<defs>` + two `<use>`s
  is the pattern to reuse whenever one part of an icon needs to animate.**

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

**Outbound links are deliberately not repeated everywhere.** The POLANA link
lives in the `#contact` socials and on the POLANA entry itself, and was
removed from the `#about` socials and the footer icon row — it is one of
Jędrzej's projects rather than one of his profiles, so repeating it in every
link cluster over-weighted it. GitHub is still in all three clusters, kept
for now despite a thin commit history; if that changes, remove it from
`#about` and the footer first and leave `#contact`, same shape as POLANA.

---

## 13. Things not to do

- Do not split the file, add a framework, or introduce a build step.
- Do not restore a single flat list of entries in place of the groups.
- Do not make the mosaic tiles uniform.
- Do not promote the CV strip or the timelines up the page — this is not a CV.
- Do not add a third accent colour (the per-card `--cat-hue` values are a
  flagged, temporary experiment, not a precedent — see §5).
- Do not set mono metadata back to 400 weight / 10px / `--ink-3`.
- Do not give `.rail-body` a height smaller than the planet column needs.
- Do not let decorative elements compete with content.
- Do not add scroll listeners outside the existing `rAF` loop.
- Do not put personal contact details beyond email and LinkedIn into the repo.
