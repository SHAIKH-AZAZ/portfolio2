# Blueprint SaaS style guide

> Copy of the `blueprint-saas-style` Claude Code skill (`~/.claude/skills/blueprint-saas-style/`). The starter page, `drawing-kit.js` and `references/components.md` mentioned below live in that skill folder. Edit the skill first, then refresh this copy.

The user's house style for showcase pages. A clean, white SaaS page in the Material family, matching the look of the product app. On top of that sit engineering-drawing touches that make the page feel technical and built rather than templated. Reference build: `index.html` in this folder.

The user signed off on this look ("that's nice", "mast ek dam mast") after rejecting these:

- **Heavy:** dark backgrounds, condensed display fonts, a moss and concrete palette, full-width coloured bands. They called it "heavy, not professional".
- **Emoji:** emoji used as icons.
- **Busy drawings:** dense bar grids and cross-section-only drawings.

Stay inside the lines below unless they ask otherwise.

## Start here

1. Copy `assets/starter.html`. It is a complete, working one-file page with every token, component and script below already wired.
2. Replace the content. Keep the tokens, then delete the sections you don't need.
3. Swap the accent if the project has its own brand. Change only `--accent`, `--accent-tint` and `--accent-ink`, and keep everything else.
4. For technical diagrams, pull helpers from `assets/drawing-kit.js`. `references/components.md` has the CSS and JS for every component and the reason behind each.

## Page structure

**Product showcase.** This is the order the BBSteel page uses:

```
nav → hero (blueprint bg, 2-line h1, lede with 2–3 mark.good phrases, 2 buttons, icon pills)
    → vision ("a week of work in a day": week strip + 3 pillars)
    → problem (before/after comparison: topic column, ✕/✓, mark.bad / mark.good)
    → how it works (numbered steps + one worked example card)
    → interactive demo (type values → live drawing, shape cards, result table)
    → deep-dive sections (AI, optimiser…) → coverage list → deliverable cards → closing CTA → footer
```

**Portfolio:**

```
hero (who you are, 2–3 highlighted skills, pills) → selected work cards → one deep case study
(interactive or drawing-led, linked highlight) → how I work (steps) → before/after → skills → contact
```

The starter has this skeleton. Cut what the content doesn't fill, because an empty section is worse than no section.

## Tokens (light only, no dark mode)

```css
:root {
  --bg: #FFFFFF;  --surface: #FFFFFF;  --fill: #F2F6FA;     /* page, cards, drawing fill */
  --ink: #2A3547; --ink-soft: #5A6A85; --steel: #7C8FAC;    /* text, secondary, drawing lines */
  --rule: #DFE5EF;                                          /* every border and divider */
  --accent: #008C1F; --accent-tint: #E8FBDA; --accent-ink: #006B18;
  --accent-2: #49BEFF;   /* secondary accent: AI and "detected" things */
  --dim: #2563EB;        /* dimension arrows and labels */
  --bad: #C2410C; --bad-tint: #FDECE7;                      /* the "before / manual" side */
  --shadow: 0 1px 2px rgba(42,53,71,.06), 0 8px 24px rgba(42,53,71,.06);
  color-scheme: light;
}
```

- The page is white. Separate sections with a 1px `--rule` line, never with coloured bands. The closing CTA section is white too.
- The accent is for actions, highlights and the "after" state only. Never use it as a large fill.

## Typography

- One family: **Plus Jakarta Sans** at 400, 500, 600 and 700 (Google Fonts). Never go above 700, and never use condensed or display fonts.
- Hero h1: `clamp(2.9rem, 7.6vw, 7rem)`, line-height 1.02, letter-spacing −.035em. A second line in the accent can be indented.
- h2: `clamp(2rem, 4.2vw, 3.4rem)`, line-height 1.12, letter-spacing −.03em, max-width 20ch.
- Body is 1rem at line-height 1.6. Lede is 1.1875rem in `--ink-soft`, max-width about 38rem. Fine print is .875rem.
- Set `font-variant-numeric: tabular-nums`, so numbers line up in tables.
- Use sentence case. No ALL-CAPS eyebrow labels, and no single accented word inside a headline.

## Surfaces and shape

- **Cards:** white, 1px `--rule` border, 12px radius, `--shadow`, 22px padding.
- **Buttons:** 8px radius, 44px minimum height. The solid style is accent on white text, the outline style is white with a `--rule` border.
- **Pills:** 999px radius, white, `--rule` border. A Tabler icon in the accent, then a bold label in the accent, then plain text.
- **Icon tiles:** 52px square, 12px radius, `--accent-tint` background, the icon in the accent at 60% of the tile.
- **Widths:** `.wrap { width: min(1240px, 100% - 32px) }`. Sections get `padding-block: clamp(64px, 10vw, 128px)`.

## Interaction (all motion respects `prefers-reduced-motion`)

- **Buttons and cards lift on hover:** `translateY(-2px)`, a deeper shadow and an accent ring. Buttons also get a Material-style click **ripple**. This matches the project cards in the app.
- **Tabs:** a 2px accent underline. Tab strips scroll sideways with **no scrollbar** and **no vertical overflow**. Draw the baseline with an inset shadow, never with negative margins, which create a phantom vertical scrollbar. Keep the active tab scrolled into view.
- **Wide tables:** wrap them in `.table-scroll`. This gives a thin rounded scrollbar, mouse drag with a short glide, and an edge fade on whichever side has hidden content.
- **Linked highlight:** when one thing appears in several views, such as a drawing, a card and a table row, give each copy the same `data-mark`. Hovering, focusing or tapping any copy lights up all of them (accent and glow) and dims the rest.
- **One orchestrated moment** per page, such as the hero animation or a typing demo of a form. Don't fade in every section.

## Components beyond the basics

- **Comparison table:** a `10rem` topic column, then before and after. The before cell is `--ink-soft` with a ✕ in a `--bad-tint` circle. The after cell has a ✓ in an `--accent-tint` circle. Key phrases use `mark.bad` and `mark.good`. On mobile the topic sits on top in the accent and the cells stack.
- **Week strip:** two rows of day cells (Mon–Sat). The "before" row has every day filled in `--steel`. The "after" row has one cell filled in the accent and the rest outlined. Use it to show time saved, but only with the user's own claim.
- **Numbered steps:** only when the content really is a sequence. The number is in the accent at 700, with a rule between steps.
- **Worked example card:** "On the drawing", then a thin connector, then a result table. Every number on it must be reproducible from the formula shown.
- **Interactive demo:**
  - **Layout:** the form sits on the left (inputs right-aligned, units as suffix, grouped as Size, Bars or Inputs, then Rules). Tabs pick the variant. On the right sit the live drawing, the shape or detail cards and the result table, with a formula column in KaTeX and a total row.
  - **Behaviour:** validate inline with a plain message ("Cover is too large for this section."). Use the one-time typing demo. Use linked highlight across drawing, card and row.
  - **Wide drawings:** a long drawing such as a beam elevation gets a full-width row (`.wide`), with the cards below it.
- **Deliverable cards:** a 52px icon tile, an h3, two lines of text, and optional tags.

## Icons

- Use **Tabler Icons** as inline SVG, from `https://unpkg.com/@tabler/icons@2.47.0/icons/<name>.svg`.
- Strip the class and size attributes, add `class="ti" aria-hidden="true"`, and let `currentColor` set the colour. Stroke is 1.6–1.8.
- **Never use emoji as icons.** If Tabler has nothing that fits, draw one in the same line style: 24 viewBox, round caps and joins, about 1.5 stroke.
- Checkmarks and crosses in comparisons are CSS-mask icons inside a tinted 20px circle.

## Technical-drawing touches (what makes it not generic)

- **Hero background:** a faint blueprint, such as a grid with axis bubbles A–E and 1–3, dimension chains and a title block. It sits at 20–30% opacity in `--steel` and is masked lighter behind the text.
- **Dimensions:** blue (`--dim`) arrows with end ticks, a bold label and the value in `--ink`, for example `L2 4,610`. Use the helpers in `drawing-kit.js`.
- **Callouts:** a leader line with a dot to a short label, CAD style (`2-T16`, `2L-T8 @ 150 c/c`).
- **Views:** show **front or side views plus a small A–A section inset** rather than only a cross-section.
- **Plans:** draw a few faint lines and **one typical element** with its call-out, never a dense grid.
- **Match the user's own drawing conventions.** When they send a CAD screenshot, a DWG view or a shape-library image, copy its notation:
  - bar marks such as `3-T25` and `2L-T8-150 C/C`
  - L1/L2/L3 dimension labels
  - a top bar's legs going down and a bottom bar's legs going up
  - an L-shaped column starter with its foot in the footing and the lap above the floor

  Draw them as SVG so the values stay live, rather than pasting the image.
- Drawings are schematic. Keep proportions believable, but clamp minimum sizes, for example a beam depth of at least 56px and leg lengths between 10 and 70px, so nothing turns into a hairline. The real numbers live in the labels and the table.
- **Formulas:** use **KaTeX** (pinned `katex@0.16.11` with auto-render, `$…$`). Re-render whenever a script rewrites content. Inside JS strings, escape backslashes (`"\\text"`, `"\\times"`), because `"\t"` silently becomes a tab.

## Copy

- Plain, specific and in active voice. A CTA names what happens: "Create account", "Upload a drawing".
- **No invented numbers.** Don't add speed claims, percentages, client logos, testimonials or prices unless the user supplies them or they are measured. Label demos as examples. If an existing page carries unverified claims, point them out rather than amplifying them.
- **Highlighting key phrases:** use the solid green box `mark.good` everywhere, the hero lede included. It is accent-tint background, `--accent-ink` text, 600 weight, 4px radius and `box-decoration-break: clone`. The user picked it over a lighter highlighter underline. On the before or manual side, use `mark.bad`. Highlight two or three capabilities, never whole sentences, and never an unverified claim.
- **Never reveal the internal stack on a product or company page.** This was a security request from the user ("ye share karna nahi chahiye").
  - Leave out frameworks, databases, queues, cloud services, AI vendors and model names, CAD libraries, file paths and hostnames.
  - Describe capabilities instead: "Vision AI reads drawings", "DWG and DXF are read natively", "Revit models are supported".
  - On a personal portfolio the user's own skills are fine, such as "TypeScript, React, Node". Still leave out deployment details and the stack behind a specific company product.
- Every worked example must be arithmetically correct, and must agree with every other place the same example appears on the page.

## Working with this user

- They iterate by screenshot ("isme ye karo", "make it less messy", "esa highlight karna hai").
  1. Find the exact element in the file first, with `grep` on its visible text.
  2. Change only that element.
  3. Screenshot the result.
- Reply in plain Hinglish with one small ASCII sketch of what changed and the real numbers. Dense English gets "samja nahi".
- Another session may be editing the same file, and the page folder may have its own git repo. Before an edit:
  - check `git log --oneline -3`
  - re-grep anything you are about to replace
  - don't rewrite sections you didn't write unless asked
  - do fix clear bugs in them (raw LaTeX, broken escapes, overflow) and say so
- When they ask "what could be better?", give one recommendation and build it. Don't hand them a menu.

## Before calling it done

- Screenshot at **1440 and 390** with headless Chromium. Commands are in `references/components.md`.
- Look for:
  - horizontal page overflow
  - clipped tables
  - text sitting on top of drawings
  - phantom scrollbars
- **Check for script errors:** load the page with `--enable-logging=stderr` and grep for `Uncaught`.
- **Test anything that only appears on hover or after an event** by dispatching the event in a throwaway copy, then screenshotting.
- **Read the copy back:** no emoji, no raw `$…$`, and no "lorem".
