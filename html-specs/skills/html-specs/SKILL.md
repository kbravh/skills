---
name: html-specs
description: Create engaging, self-contained HTML slide decks for specs, implementation plans, ADRs, brainstorms, and decision docs — keyboard-navigable single-file decks with a dark slate + emerald design system and ready-made components (callouts, option grids, timelines, terminal blocks, diagrams). Use whenever the user asks for a spec, plan, design doc, ADR, decision record, proposal, RFC, or brainstorm writeup, even if they don't say "HTML" or "slides" — writing these as markdown is the failure mode this skill exists to prevent.
---

# HTML Spec Decks

Specs written as markdown walls of text don't get read. This skill produces
single-file HTML slide decks: one idea per screen, arrow-key navigation, an
overview grid for jumping around, and components that *show* things
(terminals, diagrams, option grids) instead of describing them. A deck is
easier to grok than a long page — each slide is a bounded thought.

## Core rules

1. **One file, zero dependencies.** The deck must open correctly from a
   `file://` URL on any machine with no network. No CDN scripts, no webfonts,
   no external images. Everything inline. Non-negotiable — these files get
   emailed and dropped in chats.
2. **Start from the template.** Copy `assets/template.html` and replace the
   slides. The `<style>` and `<script>` blocks are the design system and the
   deck engine (nav, overview, deep-links, strap) — keep them intact. Add
   small per-document CSS only for a component that genuinely doesn't exist.
3. **One idea per slide.** If a slide needs a scrollbar at 1280×720, it's two
   slides. Never shrink type or cram — split. Long code (>18 lines), long
   tables (>6 rows): continue on a second slide or move to an appendix slide.
4. **Show, don't describe.** When a slide discusses a terminal command,
   render a terminal (`pre.term`). An API shape, a code block with a filename
   label. A pipeline, a diagram. A choice, an option grid with the
   recommendation flagged. The reader should *see* the artifact.
5. **Vary the shape.** Consecutive slides shouldn't repeat the same layout.
   Split → diagram → option grid → checklist. Bullet lists are the component
   of last resort — most are secretly a timeline (`.steps`), checklist
   (`.check`), ledger (`.ledger`), or card grid.

## Workflow

1. Copy `assets/template.html` to the destination. Durable docs go in the
   repo (e.g. `docs/specs/`); throwaway brainstorms go in the scratchpad.
2. Update `<title>`, the title slide (eyebrow, h1, lede, doc-meta), and the
   strap brand. The eyebrow's first segment names the doc type: `SPEC`,
   `PLAN`, `ADR`, `BRAINSTORM`.
3. Outline the deck as slide titles first — they become `data-title`
   attributes, which build the overview grid (press `O`). A good deck outline
   reads like a table of contents.
4. Write slides using `references/components.md` — read it before writing
   your first slide. For decks with 8+ content slides, add `.slide.divider`
   part breaks; put reference material on `.slide.appendix` slides (counter
   shows A1/A2, signalling "not the pitch").
5. Set the status tag honestly: `warn` Draft → `info` In review → `pass`
   Approved (`fail` Rejected/Superseded).
6. Open it for the user: local file directly, or if they're remote (SSH),
   serve bound to the LAN so Tailscale reaches it:
   `python3 -m http.server 8080 --bind 0.0.0.0 --directory <dir>`

## What the template gives you

- **Deck engine** (~110 lines inline JS): arrow/space/PgUp/PgDn/Home/End
  nav, `O` overview grid, `F` fullscreen, prev/next buttons, touch swipe,
  scrubbable progress strap, `#n` hash deep-links (share a link to slide 7),
  appendix numbering. Resist adding libraries — there is no build step.
- **Palette**: dark slate (`#0f172b`) + emerald accent (`#00bc7d`),
  dark-only on screen; printing flips to a light theme with one slide per
  page automatically.
- **Type**: fluid — body font-size clamps with viewport width and every
  component is sized in `em`, so slides scale like slides, not like a web
  page. System fonts only. Mono for all labels/eyebrows/tags — mono labels
  are what make it read as engineering rather than corporate.
- **Signatures**: pixel-art link underlines, zigzag dividers, pixel corner
  frames, notched progress strap. These are the personality — don't strip
  them.

## Judgment calls

- **Deck length**: 6–15 slides covers most specs. A 25-slide spec deck is
  usually a spec that hasn't decided anything yet.
- **Diagrams**: HTML boxes + unicode arrows by default (they reflow at every
  width). Hand-written inline SVG only for genuinely 2D topologies. Never
  mermaid or any external renderer.
- **Syntax highlighting**: hand-wrapped `tok-*` spans, only for short,
  load-bearing snippets. Long code stays unhighlighted.
- **Status colors** are semantic, not decorative: green
  shipped/accepted/safe, amber draft/caution, red blocked/rejected/dangerous,
  blue informational/in-flight. Never pick them for looks.
- **ADHD-reader defaults** (the audience includes readers who bounce off
  dense docs): the title slide's lede answers "what and why" in one sentence;
  every slide leads with its conclusion in the h2, not a topic label
  ("Two new tables, one column change" beats "Database changes");
  acceptance criteria and next actions are checklists, not prose; the final
  main slide states the ask or the next concrete action.

## References

- `references/components.md` — slide anatomy (title, divider, appendix) and
  copy-paste markup for every component (layouts, callouts, tags, option
  grids, timelines, checklists, tables, ledgers, code blocks, terminals,
  diagrams, decision records, pixel frame). Read before writing slides.
