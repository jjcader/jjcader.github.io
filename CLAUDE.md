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

**Above 900px — i.e. whenever the rail is on screen — the dividers stop
before they reach the rail's column.** They are full-bleed and the rail is
fixed on top of them, so at full strength they cut straight across the gauge
and the planets. This used to be a round `86%` fade-out; it is now
**`--rule-inset`, the exact pixel distance from the viewport's right edge to
the rail's own vertical separator**, so the horizontal rule ends precisely
where the vertical one begins instead of guessing at it. Two values:

| Mode | `--rule-inset` | Derivation |
|---|---|---|
| full (≥1800px) | `257px` | 24 `right` + 150 pill reserve + 36 planet column + 20 body gap + 11 gauge + 16 separator margin |
| compact (900–1799px) | `calc(123px + clamp(6px,.8vw,10px))` | 16 `right` + 88 label column + 11 gauge + 8 separator margin, plus the one `.rail-body` gap that scales |
| <900px | `0px` | no rail, so the line runs the full width |

**Re-derive both the same way if the rail's width, its `right` offset or
`.rail::before`'s margin change** — measure `.rail`'s box, since its width is
content-driven rather than declared.

**Both ends of a divider fade over the same fraction of the line's real
span.** Every stop is written as a fraction of `100% - var(--rule-inset)`
rather than of the viewport, so the left end fades in over exactly the
distance the right end fades out over. The previous version had a 6% fade-in
on the left and a 26% dissolve on the right, which read as a line that gave up
before it got to the rail. **If you touch the gradient, keep the stops
expressed against the span, not against `100%`.**

**The divider ornament is a slowly turning gear, centred ON the line, with the
line cut away underneath it.**

**Every block draws the divider BELOW itself — line and gear both.** This is
the third architecture for this and the only one that actually puts the gear
on the line, so it is worth understanding before moving anything. The gear has
to straddle the rule, half above and half below. Every top-level block is
`overflow:hidden` (sections so decor can bleed, the strip for its marquee), so
*nothing can be painted across the boundary between two blocks*: the upper one
clips at that y and so does the lower one. The two earlier versions both
dodged that by keeping the gear wholly on one side, and both read wrong — the
first sat below the line (clipped in half, read as a notch chewed out of the
rule), the second rested on top of it (which is what prompted "I want them
physically on the line, overlayed").

The fix is to stop drawing on the boundary at all. A block now owns both
halves of its own bottom divider: **`::after` is the line, floated half a gear
*above* the block's bottom edge, and `::before` is the gear at `bottom:0`.**
The gear's centre lands exactly on the line with 12px of it either side, all
of it comfortably inside that one block's clip. The block's `padding-bottom`
is grown by the same half-gear (`calc(var(--sec-pad) + var(--gear-size)/2)`)
so the rule ends up at exactly the y it always sat at and nothing else on the
page moves.

So: about→selected, selected→projects, projects→strip, strip→education,
education→experience, experience→contact, contact→footer. Seven dividers,
seven gears, one each — and a new section gets both for free. **The rule to
preserve: never anchor either pseudo-element to a block's TOP.** `footer` has
no divider of its own (`#contact` draws it) and `section:first-of-type` needs
no exception any more, because the hero→about boundary is simply nobody's
bottom edge.

**The line is cut away where the gear sits, rather than drawn behind it** —
`--rule-notch`, a radial-gradient `mask` on the `::after`, punches a 30px hole
at the centre. A filled disc behind the gear would *not* work: the gear is a
mask with a hollow hub and open tooth gaps, so a rule behind it still shows
through in half a dozen places. The notch radius is 3px larger than the gear's
so a little paper shows between the teeth and the cut.

`footer` also carries `margin-top:calc(var(--gear-size)/-2)` with the same
amount added back as `padding-top`, so its tinted background starts *at* the
rule rather than leaving a paper-coloured sliver above it. The gear and the
rule both have a positive `z-index`, so they still paint over that background.

The gear is drawn as a CSS `mask` over a `background-color` (`--gear` in
`:root`) rather than an inline SVG or a coloured data URI, so its colour still
comes from a token: a data URI cannot read `currentColor`, and hard-coding a
hex would break "recolour by editing `:root` only". Its `translateX(-50%)`
centring is declared **both** in the base rule and inside the keyframes:
animating `transform` replaces the whole property while the animation runs,
and the reduced-motion block kills the animation entirely — so either one
alone leaves the gear half a gear off-centre in one of the two states.

**A divider will also read as "missing" if something the same colour sits just
under it, and this has now cost three passes on the same divider.**
`#projects` is the only section with a full-bleed wave at its top. At
`top:-16px` the wave's first crest landed ~24px below the rule; moved to a
`clamp()`ed offset it was still only ~27px below on a narrow window. A
full-width blue hairline that close to a full-width blue rule merges into one
fuzzy band — the eye reads a smudge, not two lines, and concludes the rule
isn't there. It was reported missing three times while the other dividers read
fine, most recently as "missing on narrow mode", where the gap is smallest.

The root cause is that the gap the wave has to fit in **is `#projects`' top
padding**, and below ~1360px that padding is `7vw` — not tall enough to hold
44px of clearance from the rule, 42px of wave, and clearance from the heading
as well. So `.deco-waves-proj` now uses **fixed** offsets (`top:46px`,
`height:42px` → ~58px clear of the rule) and is **shown only at ≥1360px**,
where `7vw` finally reaches its 96px maximum. Narrower than that, the divider
gets the empty band to itself.

**Before assuming a divider isn't rendering, check what else is drawn within
~40px of it** — and if you move that wave again, measure the gap first rather
than eyeballing it.

**The stops are `6% / 30% / 70% / 94%` of the span, not an even fade from the
ends.** An
even `transparent → accent → transparent` gradient looked fine in the middle
of the screen and was effectively invisible across the outer third, so on a
wide monitor the section dividers read as *missing* rather than as subtle —
reported as "we lost the divider line between Selected and Browse by area."
Keeping ~64% of the width at full strength with short fades at the very edges
gives a real line that still doesn't terminate in a hard edge. **If a divider
is ever reported as invisible again, check where the reader's viewport sits
relative to those stops before assuming the element isn't rendering.**

**The section head is inverted from the usual arrangement**: the section NAME
(About / Selected / Projects / Experience / Education / Contact) is the big
element — mono, 600, `clamp(26px,3.4vw,44px)`, `--accent`, bracketed by an
orange dash on each side — and the serif line beneath it is a small subtitle.
Those six words are the same six the progress rail uses, so making them
dominant means the rail and the page state the same thing at the same weight.

**The `<h2>` is the name, not the serif line.** When the visual hierarchy was
inverted the markup was inverted with it, so the heading a screen reader
announces is the one that actually labels the section. If these are ever
re-ranked again, move the `<h2>` too.

### Typography

- **Instrument Serif** — display only. Hero name, section titles, group titles,
  category names, timeline roles, contact line, tile titles. Chosen *instead of*
  Playfair Display, which is the default serif of every generated portfolio.
- **IBM Plex Sans** — all body copy and entry titles.
- **IBM Plex Mono** — every piece of metadata: years, tags, section eyebrows,
  nav links, status chips, the rail's mission countdown, captions.

The mono is not decoration. It is the vernacular of engineering documents —
part numbers, revisions, spec sheets — and it is what makes the page read as
belonging to this person rather than to a template. Keep metadata in mono.

**Type sizes come from tokens, not from the component.** Five of them:

| Token | Used by |
|---|---|
| `--fs-small` | Card descriptions, entry summaries, timeline blurbs, category blurbs |
| `--fs-meta` | Small mono metadata: hero chips, table keys, timeline years *and* orgs, card meta, entry datelines, form labels |
| `--fs-nav` | Header nav, brand, status chip, rail labels, rail readout |
| `--fs-action` | Every pill-shaped action: Jump, Open, Full CV, Send, Close, Top, social pills |
| `--fs-card-title` | Mosaic, Life and timeline card titles |

Each replaced a spread of near-identical values (the metadata alone ran from
10px to 12.5px across six components), which reads as drift rather than
hierarchy. **Change the token, never the component** — and if a new component
needs a size, use the token whose job it does rather than inventing a sixth.

**There is ONE size for small explanatory prose: `--fs-small`.** Card
descriptions, entry summaries, timeline blurbs and category blurbs were five
slightly different clamps, which reads as sloppiness rather than hierarchy.
Change the token, never the component.

**A section head is two BLOCKS, not three, and two typefaces, not three.**
Mono name, then one serif line. It has been walked down twice. First the note
was sans at a third size, so three consecutive lines each had their own
typeface; that was fixed by giving `.section-note` `.section-sub`'s exact
family and size, leaving colour as the only separation. Then the note stopped
being its own line at all: **it is now a `<span>` INSIDE `.section-sub`**, so
the head reads as one sentence whose tail fades rather than as a subtitle with
a caption stapled underneath. Only three heads have one — Highlights,
Everything and Life — and their subtitles gained a full stop to join the two
clauses. Heads without a note (`#about`, Education, Experience) take no
terminal punctuation, which is why the site is inconsistent about it on
purpose.

**Every outbound link on the page carries `target="_blank" rel="noopener"`,
and the `.links-label` reads "Link" or "Links" depending on how many are in
that block.** The singular/plural is per entry and hand-maintained — adding a
second link to an entry means changing its label too. The CV PDF opens in a new
tab for the same reason; internal `#section` anchors and `mailto:` never do.

⚠ `.section-note` still exists as a **block** rule, because `#contact`'s line
under the email box is a real standalone `<p>` using it. The inline behaviour
lives in `.section-sub .section-note`, which strips the block properties and
keeps the colour. **Don't collapse the two rules into one** — that would turn
the contact line into a run-on with the button above it.

`.section-sub` was given `max-width:64ch` at the same time. It had no measure
because it used to be a short line; now that the aside sets inside it, without
one the merged sentence would run the full 1228px of the wrap.

**`--fs-meta` is 13px, raised from 11.5px, and that was done at the token
rather than on one component on purpose.** The ask was "make the dates and
company names bigger", and dates and company names are not in one place — they
are entry datelines, timeline years and orgs, mosaic card meta, hero
credential chips and the facts table's keys, all of which read from this one
token. Editing `.entry-meta` alone would have made the same information
smaller in the timeline than in the list. There is a `≤520px` override holding
the hero chips at 10.5px; nothing else needed one.

**The entry dateline column is FIXED at 348px and left-aligned, and that is
the only way to get the years flush.** Every `.entry-head` is its own grid, so
an `auto` column sizes to that entry's own string and the twelve datelines
each begin at a different x — which is what "the years aren't aligned" meant.
The value is the widest dateline, 40 monospace characters at 13px plus .06em
tracking, rounded up; the upper bound is `max-content`, so a missing webfont
grows the column instead of running it under the toggle. ⚠ **A dateline longer
than 40 characters silently breaks the alignment for the whole list** — widen
the column with it. The trade is that short datelines ("2026 · HACKATHON")
now leave a visible gap before the toggle, which is inherent: you cannot have
both a flush left edge and a flush right one.

**Restacking moved from 820px to 1150px** for the same reason. Below ~1150px
the fixed column takes enough of the title's `1fr` to wrap titles to four
lines, so the dateline drops under the title instead — where it sits at the
row's own left edge and the years stay flush against that. The `≤820px` block
still exists for the padding and `.entry-content`.

**The three things in an entry head are centre-aligned, not baseline-aligned.**
A 27px serif title, a 13px mono dateline and a 30px bordered disc share that
row; baseline alignment sat the disc visibly above the words, because a disc's
baseline is its glyph's baseline and the glyph is centred inside it.

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

**The eyebrow above each section title is a section NAME, not fine print.**
About / Selected / Projects / Education / Experience / Contact — the same six
words as the progress rail's labels, one for one, so the rail and the page
agree on what each section is called. It is therefore sized as a title
(`clamp(15px,1.25vw,19px)`, `--accent`), not at `.label`'s 11.5px caption
size. **That sizing is scoped to `.section-head .label` plus the one-off
`.section-label`, never applied to `.label` globally** — `.label` is also the
image placeholders, the footer lines and the CV strip's "The formal version"
sub-label, none of which should grow. `#contact`'s eyebrow isn't inside a
`.section-head`, which is what `.section-label` exists for.

**The eyebrow is bracketed by a matching orange dash on BOTH sides**
(`::before` *and* `::after`, identical `width` and `border-radius`), not led
by one on the left. With a single dash the name hung off the end of a stub;
the pair reads as a deliberate mark, and it puts the whole colour system —
blue name between two orange rules — into one small object. **Keep them
identical; the symmetry is the point.**

**A section head is two clear steps**: the name (mono, blue, the big element)
then the serif subtitle, whose fainter tail is the note. It was three steps
until the note was folded into the subtitle — and before that the note was
full body size and competed with the title above it, while the title topped
out at 48px and did not clearly outrank the group titles at 38px. The
resulting ladder is hero 104 → section title 56 → group title 38 → strip title
34. **If you resize one of these, check it against that ladder.**

The two timelines' years (`.path-when`) get the strongest treatment on the
page — 15px/600 in `--accent`, centred in their 170px column. They are the
spine of those sections, they sit alone in a wide column, and short values
("2025") looked like an accident when left-aligned in it. The centring is
reverted to left in the ≤700px single-column stack, where there is no column
for it to be centred in.

### Layout

**Above 1500px the content column is nudged LEFT, not re-centred.** Once the
fixed rail owns the right margin, a dead-centred column reads as sitting too
far right, because the eye measures the gap to the visible rail rather than to
the viewport edge. `.wrap` gets an explicit `margin-left` and `margin-right:auto`
so the box **shifts without widening** — the right edge moves left too, which
*improves* clearance from the rail rather than eating into it. Verified clear
from 1500px to 3440px. If `--wrap`, the gutter or the rail's footprint change,
re-check that the content's right edge still lands left of the rail.

- `--wrap: 1400px`, gutters `clamp(20px, 4.5vw, 86px)`. Widened from 1140px on
  request; the reference sites Jędrzej liked all use wide layouts.
- Body prose inside entries and timeline cards is capped at ~54–70 characters
  per line regardless of container width, because line length governs
  readability, not container width. **If you widen anything further, do not let
  prose measure grow with it.**

---

## 4. Page structure

1. **Sticky header** — brand, one "Now at" status chip listing both current
   institutions, vertical divider, nav, mobile
   menu button.
2. **Hero** — ~68vh full-bleed photo with parallax, gradient veil, serif name,
   a small mono pronunciation line, one-line positioning statement, a row of
   credential chips and a **Learn more** button. There is no eyebrow above the
   name any more — it read "Mechanical engineer · Zürich → Cambridge" and the
   chips below say the same thing with more force.
   The name is plain text, no `<em>` — an earlier version
   italicised the surname only, which read as if it were being emphasised or
   were a different name; first and last name now share identical styling.
   The pronunciation line (`.hero-pronounce`, small mono, dim white,
   between the name and the positioning statement) exists specifically
   because "Jędrzej" trips up an English-speaking reader — keep it dry
   ("(yes, really)"), not apologetic, matching the rest of the voice.
   The **Learn more** button carries `z-index:3` — `.hero-copy` is `z-index:2`
   and stretches across the bottom of the hero, so without it the button
   rendered correctly and silently swallowed every click. It scrolls `#about`'s
   top to just under the header; `#about` is sized to fit one viewport from
   there so the top of Highlights comes into view at the bottom. **If `#about`
   grows, that pairing breaks** — trim it rather than changing the offset.
   A `.hero-creds` row of four chips (NASA JPL · MIT · ETH Zürich · ARIS) sits
   under the positioning statement. **These are load-bearing, not decoration**:
   the page deliberately puts Education and Experience near the bottom, and
   this row is what stops that being a problem for someone scanning for
   credentials. The hero is ~68vh rather than a full screen so the chips and
   the top of `#about` are both visible on landing.
3. **`#about`** — the section head sits inside the grid's left column above
   the facts table; prose on the right spans all three of the left column's
   rows, so its first line starts level with **the section name itself**, not
   with the subtitle one row lower. `align-self:start`, not `baseline`:
   `.section-name` is a flex row whose first flex item is a 3px dash, so the
   baseline it hands the grid is that bar's, not the word's. Aligning the two
   line-box tops is predictable and lands the cap heights within a pixel
   anyway, because the mono's half-leading is larger than the serif's.
   The prose runs lede → lead-in → **quotation** → hardware half → people half.
   `.about-quote` is the only quotation on the page and is a **left rule, not a
   tinted panel** — `--paper-2` boxes are already the CV strip and the
   text-only Life card, and a third would read as another piece of UI rather
   than as someone else's voice. It is serif, because the change of face is
   what marks the switch from Jędrzej's words to Zurbuchen's, and it is
   deliberately set *below* the serif lede at the top of the column so the two
   don't read as a tie. **There is no attribution line under it on purpose**:
   the sentence directly above already names the speaker and his two jobs, so
   a `<cite>` would say it twice. ⚠ The quote costs roughly 150px of column
   height over the version without it, which eats most of the slack behind the
   "`#about` fits one viewport" pairing above — if that pairing needs winning
   back, the cheapest ~66px is folding the lead-in sentence into a `<cite>`
   under the quote, not shortening either of the two paragraphs beneath.
4. **`#selected`** — "Highlights": mosaic of four featured cards (image + caption).
5. **`#projects`** — "Everything": three category cards, then three grouped lists of entries.
6. **`#life`** — "Life": fun-stuff cards (image + caption, some text-only),
   with the photo strip as a band at its foot.
7. **`#education`** — owns the whole timeline: one spine, education left,
   experience right, reverse-chronological, with `#experience` as an anchor
   partway down it. Then the CV strip.
8. *(no separate experience section — see the timeline in §5)*
9. **`#contact`** — a click-to-reveal email box and the message form side by
   side, plus socials.
10. **Footer** — location, centred icon links (under the divider gear),
    copyright plus a "no template" credit.
11. Floating: right-hand progress rail, back-to-top button.

Sections carry `EDIT ME #n` comments marking where content goes.

---

## 5. Components

### The featured mosaic (`#selected`)

Four `<button class="tile">` cards on a twelve-column grid, alternating
`.t-wide` (7 cols) and `.t-narrow` (5 cols). The uneven widths are what stop it
reading as a product listing — **do not regularise them into a flat 4-up.**

**Each card is image on top, caption underneath** — not a title floating over a
photograph, which is what it was for six rounds. That version asked the reader
to click a picture to find out what it was, and a reader who is scanning does
not click, so they learned nothing. The caption carries meta, title, a
two-line description and an "Open" affordance; roughly half the card is now
text. The description is the one place a project gets summarised without
opening anything.

Each card carries `data-target="eN"` pointing at an entry's `id` below. Clicking
scrolls to that entry, opens it, and flashes it. The mosaic is a *visual layer
over the same content*, never a duplicate — there is exactly one copy of every
project's text.

The four featured are **JPL, MIT, POLANA and EYP** — chosen on the grounds
that they are the ones with usable photographs. A card with no real image looks
worse than no mosaic at all, so **feature what you can actually illustrate**
and cut the count rather than shipping grey placeholders. Note the trade this
makes: none of the four is a rocket, so the mosaic no longer shows the thing
the rest of the site argues is central. HERMES or HELIOS moving into the four
(displacing one of the two leadership cards) would fix that if the photos
exist.

**`#selected` is named "Highlights" and `#projects` is named "Everything".**
They previously read "Selected" and "Projects", which does not tell anyone how
the two differ — both sound like "some of his work". The pair now states the
relationship: one is a curated four, the other is the complete list. Rail and
nav labels follow, as always, one for one. The photo strip took the name
"Snapshots" so it wasn't competing for "Highlights".

### Category cards + groups (`#projects`)

Three `<button class="cat">` cards with `data-goto="g-xxx"` scrolling to the
matching `<div class="group" id="g-xxx">`:

| id | Title | Icon |
|---|---|---|
| `g-space` | Rocketry & space | `#i-rocket` |
| `g-robotics` | Robotics & simulation | `#i-cpu` |
| `g-people` | Leadership & teaching | `#i-users` |

Order encodes priority. The MIT thesis sits in Robotics & simulation, not
Rocketry — it is simulation-led, and Robotics was the thin group.
**A `.cat` card is a filled pill saying "Jump"; a mosaic card is an outlined
pill saying "Open".** Same shape, different weight, because they are different
verbs: one moves you down the page, the other expands something in place. Keep
that distinction if either is restyled. When adding or
removing entries, **update three hand-maintained things, not two**: the card's
leading number and `data-n` in the `.cat-n` line, and the `.group-count` span.
`data-n` is what the count-up animation counts to, so a stale one animates to
the wrong number and then sits there.

There was a fifth group, **`g-making`** (Making, `#i-tool`, entries `e10` and
`e15`), removed on request — "I didn't make enough stuff and I don't want it."
Removing a group is not just deleting the block; the full checklist is: the
`.group`, its `.cat` card, **renumber every remaining card's leading number**,
any photo-strip or mosaic card whose `data-target` pointed into it (a
strip card pointed at `e10` and had to go), and any copy that counts the
groups (the `#projects` section note said "Five areas"). Re-check the
`#projects` decoration spacing too — they are placed by `top:%` against a
section that just got ~10% shorter.

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

**The collapsed summary is the literal opening of the entry's own first
paragraph, cut off with an ellipsis** — so clicking continues the sentence
rather than restating it. `.entry-summary` is therefore hidden once the entry
is open (`.entry[data-open="true"] .entry-summary{display:none}`): the body
says the same words at full length immediately below, and only one copy is ever
on screen. **When editing an entry's prose, re-cut its summary from the new
first sentence**, or the two will disagree.

The tag chips that used to sit beside it are gone. Across fifteen rows they
were noise — the summary already says what the work involved, and the tags
repeated it in a second typeface.

```html
<article class="entry" data-open="false">
  <button class="entry-head" aria-expanded="false" aria-controls="eN">
    <span class="entry-title">…</span>
    <span class="entry-meta">Year · Place</span>
    <span class="entry-toggle" aria-hidden="true">+</span>
    <span class="entry-brief">
      <span class="entry-summary">One line on what the work was.</span>
      <span class="tags"><span>Tag</span><span>Tag</span></span>
    </span>
  </button>
  <div class="entry-body" id="eN"><div><div class="entry-content">
    <div>
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

**An entry body's prose is budgeted against its own photo column, not written
to length.** `.entry-content` is two equal columns, so an entry looks balanced
only while the text is no taller than the shots beside it. For the three-photo
layout that ceiling is `wide + 8px + square`, i.e. about `1.06 × columnWidth`,
against which the prose gets whatever is left once the caption lines (~74px)
and the links block (~88px) are subtracted. **In practice that is 135–150 words
in 3–4 paragraphs** — the JPL, micropump, swarm and valve entries are all
written to it. Model it rather than eyeballing it: the terms are `54ch` (the
`.entry-content p` cap, which bites before the column does), `17px × 1.65`
line-height, and a 14px margin per paragraph. Two-photo entries have roughly
half the ceiling and should run correspondingly shorter.

⚠ It fits from **1520px up**. Between 900 and 1519px the `.wrap` gains 100px of
`padding-right` to keep the rail alive, which narrows the photo column faster
than it narrows the text — so the prose runs ~40px past the shots there. That
is the cost of the "shrink the page, not the rail" decision, not a bug in the
copy; don't cut good sentences to fix it.

**An entry is not obliged to fill a screen, and the photo count is the dial.**
Three shots (`wide` spanning the top, then two `small` side by side) is the
full-size entry; two `small` side by side, or one `wide` on its own, are the
short ones. Use the short forms wherever there genuinely isn't the material —
a thin entry padded out to match a rich one reads worse than a short entry
that knows it is short. Currently: three on JPL, HERMES, HELIOS, valves,
swarm, POLANA and EYP; two on micropump, the MIT thesis, the hackathon and
the snack robot; one on teaching. **Dropping to two photos leaves the `-3.jpg`
of that set unreferenced** — no error, just a file you no longer need.

**Every entry with photos ends its prose with caption lines named for grid
position**, `<p class="shot-cap"><span>Top:</span> …</p>` and so on — Top /
Bottom left / Bottom right for three, Left / Right for two, Photo for one.
They are mono at `--fs-meta` like every other caption on the site, and the
`<span>` carries the accent colour so the label reads as a label. They sit in
the TEXT column, not under `.shots`, so they run down the left of the photo
grid rather than beneath it.

⚠ `.shot-cap` and `.links-label` are both scoped as `.entry-content .shot-cap`
rather than as bare classes. `.entry-content p` is a class **plus** an element
selector, so it out-specifies a single class and silently takes back `color`
and `margin` — which it had already quietly done to `.links-label`. **Anything
styling a `<p>` inside an open entry has to clear that same bar.**

**The summary and the tags live in the HEAD, not the body, and are visible
while the entry is collapsed.** They used to sit inside `.entry-content`, which
meant a reader scanning `#projects` saw fifteen bare titles and had to click
each one to learn anything — the exact wrong bargain for someone who is
scanning, because they don't click. They were **moved, not copied**: there is
still one copy of every entry's text.

Two consequences to preserve. The tags are `<span>`, not `<ul>`/`<li>` — a list
is not valid inside a `<button>`, and the whole row has to stay one hit target.
And `.entry-head` is a two-row `grid-template-areas` layout
(`"title meta toggle" / "brief brief brief"`), which is what keeps the meta
right-aligned on row one while the brief spans the full width on row two; the
≤820px query restacks the same areas into one column.

The triple-nested `div` inside `.entry-body` is load-bearing: the expand
animation uses `grid-template-rows: 0fr → 1fr` with `overflow:hidden` on the
inner wrapper, which animates to auto height without JavaScript measurement.
**Do not flatten those divs.** Entry ids run `e1`–`e17` **with `e10` and
`e15` retired** (they belonged to the deleted Making group — see below); new
ones continue past `e17` rather than reusing a retired number, and must be
unique, because tiles and photo cards target them.

**Group heads are `position:sticky` under the site header** (`top:78px`, the
header's own height — change one and change the other), so you always know
which of the four areas you are reading. This is why `section` is
`overflow:clip` rather than `overflow:hidden`: both stop decoration bleeding
into a horizontal scrollbar, but `hidden` creates a scroll container and
`position:sticky` inside one resolves against *that box* instead of the
viewport, which silently kills the stickiness. `clip` clips identically without
becoming a scrollport. The `hidden` declaration is kept first as a fallback —
anything that doesn't understand `clip` loses stickiness and nothing else.
**Don't put `overflow:hidden` back on `section`.**

**Every entry has an explicit `.entry-close` button at the foot of its body.**
The only way to close an open entry used to be the toggle at the top of the
head — which, after two paragraphs and three photographs, is off screen, and
clicking a *title* to close something doesn't look like a control anyway. The
toggle is now also a bordered disc rather than a bare `+` so it reads as
pressable. The close button returns focus to the entry head, so a keyboard
reader isn't dumped at the top of the next entry.

### Photo strip

A CSS marquee: `.strip-track` animates `translateX(0 → -50%)`. **The card set
must be duplicated exactly once** for the loop to be seamless. Pauses on hover
and on keyboard focus-within. Each card is a button with `data-target` and opens
an entry, same as the tiles.

This actually shipped broken for a while — the track held **one** set of seven,
so `-50%` scrolled the cards off and then showed a card's width of empty track
before snapping. If the strip ever shows a gap, count the cards: it is almost
always that the two halves are no longer identical. Removing a card means
removing **both** copies.

`.strip-viewport` carries `padding-block` so cards have room for their
`translateY(-4px)` hover lift. Without it the lift pushed each card's top edge
past the clip boundary and the orange hover outline lost its top side — which
looks like a broken border, not a clipping problem. This works *because*
`overflow:hidden` clips at the padding edge (see the `margin-right` note below,
which exploits the same fact in the opposite direction).

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

Right-hand side, screens ≥900px. Structure: countdown readout, then a flex row
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

**The rail carries SEVEN items** (About / Highlights / Everything / Life /
Experience / Education / Contact). Adding `#life` broke the previous budget and
this is the thing to re-check whenever a section is added: at
`min(66vh,560px)` with 20px/16px gaps, seven items needed 470px and overflowed
at every viewport under ~780px tall. The shipped values —
`height:min(70vh,580px)`, `gap:10px` full / `8px` compact, and a 34px dot in
compact — were picked by modelling it, not by eye, and verified to fit from
600px viewport height upward in both modes.

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

**The readout is a mission clock, not a percentage** — `T+100` at the top of
the page down to `T+000` at the bottom, zero-padded to three digits and set in
`tabular-nums` so the column never changes width as it ticks. The rail is a
rocket riding a gauge, so a clock says what the element is doing better than
"41%" did. **The prefix is `T+`, not `T-`, and that is deliberate** — Jędrzej
changed it himself on the grounds that the rocket has already left the pad by
the time anyone is reading, so the page measures time *since* launch. Don't
"correct" it back.

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
   robotics group. (A Rubik's cube one-off sat by the old hobbies group and
   went with it.) Build a one-off when the content calls for something
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

### The warm/cool split in the decorative layer (settled)

`.warm` switches a deco's `color` from `--accent` to `--accent-warm`, and it is
applied to roughly half the deco elements, **interleaved within sections rather
than assigned by whole section**. This began as a flagged experiment and is now
settled: assigning it by whole section read as blocky, and interleaving is what
made it read as one system. **If asked to mix it further, keep editing
individual elements' `warm` class — don't go back to toggling it by section.**

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

| Section | Decorations, in document order |
|---|---|
| `#about` | `deco-orbit-about` *(warm)* |
| `#selected` | `deco-orbit`, `deco-orbit-sm` *(warm)*, `deco-telescope` *(warm)*, `deco-drone` |
| `#projects` | `deco-traj` *(warm)*, `deco-orbit-2` *(warm)*, `deco-orbit-left`, `deco-rocket-b`, `deco-orbit-mid`, `deco-robot` *(warm)*, `deco-waves-proj` |
| `#life` | `deco-orbit-life`, `deco-waves-top` |
| `#education` | `deco-orbit-4`, `deco-rocket`, `deco-orbit-3` *(warm)*, `deco-traj-2` *(warm)* |
| `#contact` | `deco-waves`, `deco-orbit-contact` *(warm)* |

*(warm)* = carries the `warm` class. The table drifts as decorations move;
treat it as a guide, not a contract, and trust the markup.

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

**Nothing on this page is hidden waiting to be scrolled into view.** `.reveal`
used to be `opacity:0` until an `IntersectionObserver` added `.in`, and that
was removed on purpose: a reader who lands on the page and simply *looks* saw
`#about` blank until they nudged the wheel. The class survives only as a
"this became visible" hook for the entry-count roll-up. The same reasoning
killed the staggered `.facts` row animation. **Don't reintroduce a fade-in.**
If something must animate on entry, animate it from a visible resting state —
never from nothing.


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
the flame scales about the point where it meets the engine bell, given as
**explicit view-box units (`transform-origin:12px 17.3px`)** rather than a
percentage under `transform-box:fill-box`. `fill-box` resolves against *each
path's own* bounding box, and `.flame` and `.flame-core` have different
bounding boxes — so the same `50% 0` meant two different points and the core
crept out of the flame as it stretched. **When two sibling SVG paths must
scale together, give them a shared origin in view-box units, not a
percentage.**

Both this and the deco parallax below were first shipped tuned so
conservatively that they were reported as not working at all: the flame
normalised speed against 60px/frame when a normal wheel notch moves 15–25px,
so ordinary scrolling only reached a third of the range. **The failure mode
for velocity-driven motion is being invisible, not being wrong** — pick the
constants against a *normal* scroll, not a fast fling, and verify the effect
is legible at the slow end before trusting it.

**The rAF loop re-schedules itself while the flame is still easing back to
rest.** Scroll events have stopped by then, so nothing else would keep it
running and the flame would freeze at whatever length it had when you
stopped. Any future velocity-decay effect needs the same self-scheduling.

### Decorative parallax

`.deco` elements drift against the scroll at 0.16× (orbit rings) or 0.24×
(one-off glyphs) so the background sits at a real depth, with the resulting
offset **clamped to ±120px**. The rates started at 0.07/0.12 and were reported
as doing nothing — on a 400px ring at 24% opacity that is about a 3% shift
across a whole section, which is technically working and impossible to see.
The clamp exists because the higher rates would otherwise carry a deco a few
hundred pixels from where it was placed and break the "never let two
decorations overlap" spacing; it engages just under one viewport from centre,
by which point the deco is barely on screen anyway.

Two families are **excluded and must stay excluded**:

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
- **Year scramble** — hovering a timeline card scrambles the year's digits for
  ~380ms before they settle, each digit locking in at a position-dependent
  threshold so the number resolves left to right. Only characters `0-9` are
  scrambled, so separators and words ("present") survive and the string keeps
  its exact length; `.path-when` is `tabular-nums` so the column can't jitter
  as digits swap. It re-arms only after the pointer leaves, or running the
  cursor down the list turns into a slot machine.
- **Fact rows** — `#about`'s facts table deals its rows in with staggered
  `nth-child` delays. `.facts` carries `.reveal` purely as a visibility
  trigger and explicitly opts out of the generic fade-and-rise
  (`.facts.reveal{opacity:1;transform:none}`), or the container would slide
  while its own rows were individually sliding.
- **Reverse ignition** — closing an entry plays `ignite-rev`, the mirror of
  the opening ring: it falls in from outside and collapses into the toggle
  instead of blowing out of it, so the two actions read as opposites rather
  than as the same puff twice. The JS reads `open` (the state *before* the
  click), so `open === true` means the click is a close.
- **Hero cursor orbit** — an orbit ring trails the pointer across the hero,
  easing toward it rather than tracking exactly, because the lag is what makes
  it read as an object being towed instead of a cursor graphic. It runs on its
  **own** rAF, not `onFrame()`: that loop is scroll-driven and idle whenever
  the page isn't moving, which is precisely when this needs to run. The loop
  starts on pointer entry and stops once the ring has caught up, so nothing
  spins in the background. Gated on `(pointer: fine)` — on a touch screen
  there is no cursor to follow and the ring would just strand itself wherever
  you last tapped. It sits at `z-index:1` with `.hero-copy` at `2`, so it
  passes *behind* the name.
- **Envelope flap** — the mail icon's flap folds open on hover. CSS still
  cannot reach inside a `<use>` shadow tree to transform one path of a symbol,
  but it *can* transform the `<use>` element itself, so the envelope is split
  into two `<use>`s (`#i-mail-box` + `#i-mail-flap`). Those two are `<g>`
  inside `<defs>`, **not `<symbol>`**: a `<use>` of a `<symbol>` with its own
  `viewBox` re-establishes a viewport and remaps coordinates, which makes the
  flap's `transform-origin` unpredictable; a `<use>` of a `<g>` is copied
  straight into the current coordinate system. **`<g>` in `<defs>` + two
  `<use>`s is the pattern to reuse whenever one part of an icon needs to
  animate.**

  Two things about it were wrong and are worth not repeating. It was hinged
  with `transform-box:fill-box` + `transform-origin:center top`, which
  resolves against the `<use>`'s own object bounding box — something engines
  compute inconsistently for a `<use>`, so the flap silently never moved in
  some browsers while working in others, and it was reported as simply not
  working. It is now `transform-box:view-box` with the hinge named outright in
  view-box units (`transform-origin:12px 7px`), which is exact and portable.
  **Do not go back to `fill-box` on a `<use>`.** And the open state is
  `translateY(-3px) scaleY(-.5)`, not a full `scaleY(-1)`: a full mirror puts
  the apex at y=1 with the flap's legs still crossing the box's own top edge
  at y=4, which reads as a line floating over the envelope. Halving and
  lifting it seats the flap's base exactly on that top edge — a folded-back
  flap, foreshortened the way a real one is. It also has to stay inside the
  0–24 viewBox, and the box top at y=4 is all the headroom there is.

  The animation lives in three places: the two 17px icons in `#about`'s
  socials and the footer link row, and — added because those two are far too
  small for anyone to notice a flap moving — **a `.62em` envelope in front of
  the contact section's email address**, which is the one place it is actually
  discoverable. There, the underline sits on an inner `<span>` rather than on
  the `<a>`, so the rule runs under the address only and not under the glyph.

---

### Fun stuff & life (`#life`)

**The divider + gear pair belongs to `<section>` only.** When the strip was a
top-level band it shared those selectors (`section::after,.strip::after`);
once it moved inside `#life` that produced TWO rules before Experience — one
across the middle of the snapshots strip and one at the section's real bottom.
If another block is ever nested inside a section, check it isn't inheriting
the divider rules.

Sits between `#projects` and `#experience`. It exists because everything else
on the page is artifacts — nothing showed the person. Hobbies used to be a
fourth group inside `#projects`; it was pulled out because a list row is the
wrong shape for "I swam for ten years", and because burying it at the bottom of
the longest section meant nobody reached it.

Cards work like the Highlights mosaic — image on top, caption under — on a
plain auto-fit grid rather than an uneven one, because these are peers rather
than a ranked four. **`.life-card.no-photo` drops the image block entirely**
and gets a tinted panel instead: not everything here has a photograph, and a
card that admits that reads better than a grey box.

**The photo strip now lives at the foot of this section**, and its cards are
`<figure>`, not buttons. It used to open project entries, which made it a
third pass over work the mosaic and the list already covered. As photographs
only it stops competing — and that also removed the reason it was hidden below
700px, since a marquee nobody taps doesn't need to pause on touch.

### The contact form (`#contact`)

**Email and form sit side by side in `.contact-grid`, not stacked.** There was
an empty column next to the address, and stacking pushed the form below the
fold on the one screen where the reader is most likely to act.

`<form class="msg" id="msgForm">` — a bordered panel, deliberately the only
thing on the page that asks the reader to *do* something rather than read.

**It has two modes and picks between them from the `action` attribute**, which
is the whole point of the design: the site is static, has no backend, and must
keep working with zero setup.

- `action` still contains `PASTE-YOUR-FORM-ENDPOINT-HERE` → the JS composes a
  pre-filled `mailto:` and hands off to the reader's mail client. Nothing is
  sent by the page, nothing leaves the browser.
- `action` set to a real endpoint (Formspree, Web3Forms — anything that takes a
  POST and answers JSON) → the JS `fetch`es it in the background and confirms
  in place, so the reader never leaves the page.

**Swapping modes is a one-line change and requires no code edits** — paste the
endpoint over the placeholder. Don't "simplify" this by deleting the mailto
branch: it is what makes the form work on a fresh clone with no account
anywhere.

The browser's own `required` / `type=email` validation runs first via
`reportValidity()`, so there is no hand-rolled validation to keep in sync. The
send button's rocket animation is CSS keyed off `.is-sent`, which the JS
removes again after 800ms so a second message re-arms it — same
remove/reflow/re-add pattern as the entry ignition ring.

---

**Every image needs `loading="lazy"` except the hero and the first mosaic
card.** There are roughly sixty images on the finished page, most of them
inside collapsed entries a reader may never open, and by default the browser
fetches all of them at load. The two exceptions are on screen immediately —
deferring those makes them appear *later*, not sooner. Give every `<img>` real
`width`/`height` attributes too, so the page doesn't jump as they arrive. The
replacement recipe is in a comment above the hero in `index.html`.

**`og:image` must be an absolute URL.** The scraper fetching it is not on this
domain, so a relative `images/og-card.jpg` resolves against *its* host and
fails. 1200×630 is the size to target.

### The timeline (`#experience` + `#education`)

**One spine, two threads.** Study hangs off the RIGHT, work off the left, and
the whole thing runs in reverse-chronological order — which is why **Education
leads**: the MIT thesis is the most recent thing on the page. That ordering is the whole
point: it shows ARIS and ETH overlapping, which two separate lists could never
say. They used to be two sections; merging them removed a divider, a heading
block and a screenful of height.

**`#experience` is an `<li>` inside `#education`, not a section.** Its heading
sits in the left-hand column partway down the spine, and the rail's Experience
item scrolls to it — two headings pointing into one object. Whichever thread
leads owns the `<section>`; the other is an anchor inside it.

⚠ **That change required a fix in the rail's `frac` maths.** A section now runs
until the NEXT one starts rather than for its own `offsetHeight`. Those are the
same thing for real sections, but the inner anchor's own height is a single row —
measured by `offsetHeight` the rocket hit `frac=1` the instant you reached that
heading and jumped straight to the next dot. **Any future anchor that isn't
a full-height section depends on this.**

**The inline heading's `::after` dash and the CV pill fight over flex order.**
`.section-name` brackets its text with a dash on each side via `::before` and
`::after`. Put a `.cv-mini` pill inside the heading and the `::after` dash lands
after the *pill*; "fix" that with `order:-1` and it jumps to the front, giving
`— — EXPERIENCE`. The correct fix is `.cv-mini{order:1}` — order the pill last
and the dashes stay either side of the name where they belong.

Layout: `.tl` is a flex column and each `.tl-row` is its own internal 3-column
grid (`card | node | card`). Document order therefore *is* vertical order, and
adding an entry is just adding an `<li>` — there are no per-row `grid-row`
assignments to keep in sync. **Spacing is not to scale**: rows are evenly
spaced regardless of date range, because a true scale leaves a hole at
2016–2020 and squashes everything recent together.

Below 860px the spine moves to the left edge and both threads stack in one
column — the left/right split has no meaning when there is only one side.


### `404.html`

GitHub Pages serves it for any URL that doesn't exist; without it you get
GitHub's own generic page with no connection to this site. It is **deliberately
self-contained** rather than sharing `index.html`'s stylesheet — it needs about
thirty lines of CSS, and duplicating those is cheaper than the alternative.

⚠ **Its `:root` tokens are copied from `index.html`. If the palette changes
there, change it here too.** That is the one place on the site where a value is
duplicated, and it is a deliberate trade, not an oversight.

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

**GitHub is deliberately not linked anywhere.** The `#i-github` symbol is kept
in the sprite, but every link was removed: a GitHub link is a claim that there
is code worth reading, and a thin commit history makes that claim fail on the
click. None of the work this site is about — valve design, a test campaign,
chairing a hundred people — lives in a repo. Put the links back when two or
three repos would survive being read.

## 12. Common tasks

**Add a project** → copy an `<article class="entry">` into the right `.group`,
give it a fresh unique `id`, update that group's `.group-count` and the matching
`.cat-n` count.

**Add a category** → copy a whole `.group` block with a new `id`, add a matching
`.cat` card with `data-goto` pointing at it, pick an icon from the sprite.

**Feature something in the mosaic** → change a card's `data-target`, title,
meta, description and image. Keep four cards, and keep the alternating
`.t-wide` / `.t-narrow` widths.

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

**Favicon** → an inline `data:image/svg+xml` URI in `<head>`, not a file: the
repo has a no-extra-files habit and an SVG favicon scales to every size on its
own. It is drawn **filled**, unlike every sprite icon on the page, because at
16px a hairline stroke disappears completely. Its colours are hard-coded
(`%230d55bd` / `%23f07a1a`, where `%23` is an escaped `#`) because a data URI
cannot read CSS variables — **if `--accent` or `--accent-warm` change, change
them there too.** That is the one place on the site where "recolour by editing
`:root` only" does not hold.

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
