# HTML Spec Decks

Specs written as markdown walls of text don't get read. This skill turns specs,
implementation plans, ADRs, and brainstorm docs into single-file HTML slide
decks: one idea per screen, arrow-key navigation, an overview grid for jumping
around, and components that show things (terminals, diagrams, option grids)
instead of describing them.

## What you get

- **Single self-contained file** — opens from `file://` on any machine, no
  network, no build step, no CDN. Email it, drop it in a chat.
- **Deck engine** — arrow keys, `O` overview grid, `F` fullscreen, touch
  swipe, segmented click-to-jump progress strap, `#n` deep links to slides.
- **Design system** — dark slate + emerald with pixel-art trim, fluid type
  that scales with the viewport, system fonts only.
- **Components** — callouts, status tags, option grids with a recommended
  flag, timelines, checklists, terminal blocks, Night Owl code blocks,
  HTML-box diagrams, decision records.
- **Print support** — flips to a light theme, one slide per page.

## Usage

Ask Claude for a spec, plan, ADR, decision record, or brainstorm writeup.
The skill triggers on intent — no need to say "HTML" or "slides".

> Write me a spec for adding TOTP two-factor auth to my app

Navigation inside a generated deck: `←`/`→` arrows, `O` for the overview
grid, `F` for fullscreen, click segments in the bottom strap to jump.
