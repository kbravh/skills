# Component & slide markup reference

Copy-paste markup for everything in `assets/template.html`. All classes already
exist in the template's CSS — no new CSS needed.

## Contents

- [Slide anatomy](#slide-anatomy)
- [Title slide](#title-slide)
- [Divider slides](#divider-slides)
- [Appendix slides](#appendix-slides)
- [Layout helpers](#layout-helpers)
- [Eyebrow kickers](#eyebrow-kickers)
- [Tags / status pills](#tags--status-pills)
- [Callouts](#callouts)
- [Panels and card grids](#panels-and-card-grids)
- [Option comparison](#option-comparison)
- [Timeline steps](#timeline-steps)
- [Checklist (acceptance criteria)](#checklist-acceptance-criteria)
- [Tables](#tables)
- [Ledger rows](#ledger-rows)
- [Code blocks](#code-blocks)
- [Terminal sessions](#terminal-sessions)
- [Diagrams](#diagrams)
- [Decision records (ADR)](#decision-records-adr)
- [Pixel frame](#pixel-frame)

## Slide anatomy

Every slide is a `<section class="slide">` with a `data-title` — that title
feeds the overview grid (press `O`), so make it short and scannable. Number
slides with a comment for easy authoring:

```html
<!-- ================= SLIDE 4 · DB SCHEMA ================= -->
<section class="slide" data-title="DB schema">
  <div class="eyebrow">PART 2<span class="sep">/</span>DATA MODEL</div>
  <h2>Two new tables, one column change</h2>
  <!-- one idea, one or two components -->
</section>
```

One idea per slide. If a slide needs a scrollbar at 1280×720, it's two slides.
Never shrink content to fit — split it.

## Title slide

First slide, `class="slide title active"` (the `active` starts the deck there).
Status tag: `warn` Draft, `info` In review, `pass` Approved, `fail`
Rejected/Superseded. Eyebrow's first segment names the doc type: `SPEC`,
`PLAN`, `ADR`, `BRAINSTORM`.

```html
<section class="slide title active" data-title="Title">
  <div class="eyebrow">SPEC<span class="sep">/</span>PODLING<span class="sep">/</span>2026</div>
  <h1>Notification Digest Pipeline</h1>
  <p class="lede">Batch per-user notifications into a daily digest email instead of sending each one individually.</p>
  <div class="doc-meta">
    <span><b>Status</b> <span class="tag warn">Draft</span></span>
    <span><b>Author</b> Karey Higuera</span>
    <span><b>Date</b> 2026-08-04</span>
  </div>
</section>
```

Also update the strap brand (`<span class="brand">SPEC / PODLING</span>`) and
the `<title>`.

## Divider slides

Structural rhythm for decks with 8+ content slides — a breath between major
parts. Skip them in short decks.

```html
<section class="slide divider" data-title="Part 2 · Data model">
  <div class="part">Part 02 / 04</div>
  <h2>The data model</h2>
  <p class="lede">Two tables, one enum, zero migrations that lock.</p>
</section>
```

## Appendix slides

`class="slide appendix"` after the last main slide. Counter shows `A1 / A3`
instead of `09 / 12` — signals "reference material, not the pitch". Use for
rejected alternatives, raw data, config dumps.

```html
<section class="slide appendix" data-title="Rejected: BullMQ">
  <h2>Appendix: why not BullMQ</h2>
  <p>Redis dependency we don't otherwise have…</p>
</section>
```

## Layout helpers

`.cols` (2-up), `.cols-3` (3-up), `.split` (narrow narrative left, wide
artifact right — the workhorse for "explain + show"):

```html
<div class="split">
  <div>
    <h3>Flush loop</h3>
    <p>Consumer drains per-user buffers every 5 minutes. Idempotency key prevents double-send.</p>
  </div>
  <pre><code>...</code></pre>
</div>
```

All collapse to one column under 760px.

## Eyebrow kickers

Mono uppercase kicker at the top of content slides. Use it to carry the
current part name — it's the cheap version of a divider.

```html
<div class="eyebrow">PART 2<span class="sep">/</span>DATA MODEL</div>
```

## Tags / status pills

```html
<span class="tag">default</span>
<span class="tag pass">shipped</span>
<span class="tag warn">draft</span>
<span class="tag fail">blocked</span>
<span class="tag info">in review</span>
```

Inline in headings, table cells, doc-meta, option cards. Colors are semantic —
green shipped/accepted, amber draft/caution, red blocked/rejected, blue
informational. Never decorative.

## Callouts

Four types: `tip` (emerald), `note` (blue), `warning` (amber), `danger` (red).

```html
<div class="callout warning">
  <div class="callout-title">Warning</div>
  <p>Running the backfill twice double-counts events. The job is not idempotent yet.</p>
</div>
```

## Panels and card grids

```html
<div class="cardgrid">
  <div class="panel"><h4>Ingest</h4><p>Webhook receiver, validates and enqueues.</p></div>
  <div class="panel"><h4>Batch</h4><p>Hourly job groups by user.</p></div>
  <div class="panel"><h4>Send</h4><p>Renders digest, hands to Postmark.</p></div>
</div>
```

## Option comparison

The decision workhorse. Flag the recommended option with `.reco` — emerald
border + tint. Every option gets a `.verdict` line.

```html
<div class="optgrid">
  <div class="opt">
    <h4>Cron + SQL <span class="tag">option a</span></h4>
    <p>Hourly cron queries unsent notifications, groups in app code.</p>
    <div class="verdict">Simple, but polling wastes cycles at low volume.</div>
  </div>
  <div class="opt reco">
    <h4>Queue consumer <span class="tag pass">recommended</span></h4>
    <p>Notifications land in a queue; consumer flushes per-user buffers on a timer.</p>
    <div class="verdict">Scales with volume, no polling, reuses existing infra.</div>
  </div>
</div>
```

Give one option slide to the comparison, then optionally one slide per option
for depth. Don't cram three deep analyses onto one slide.

## Timeline steps

Vertical timeline for phases, rollout plans, sequences. Bold first element is
the step title.

```html
<ol class="steps">
  <li><b>Expand</b> Add nullable <code>digest_id</code> column, deploy writers.</li>
  <li><b>Migrate</b> Backfill historical rows in 10k batches.</li>
  <li><b>Contract</b> Make column non-null, drop old index.</li>
</ol>
```

## Checklist (acceptance criteria)

`done` fills the box with a checkmark.

```html
<ul class="check">
  <li class="done">Digest renders with 1, 10, and 500 notifications</li>
  <li>Unsubscribe link resolves per-user</li>
  <li>Duplicate send prevented by idempotency key</li>
</ul>
```

## Tables

Plain semantic tables are styled automatically. Keep them to ~6 rows per
slide; longer tables continue on a second slide or move to an appendix.

```html
<table>
  <thead><tr><th>Endpoint</th><th>Method</th><th>Auth</th><th>Notes</th></tr></thead>
  <tbody>
    <tr><td><code>/digests</code></td><td>GET</td><td>session</td><td>Paginated, 50/page</td></tr>
    <tr><td><code>/digests/:id/resend</code></td><td>POST</td><td>admin</td><td>Idempotent</td></tr>
  </tbody>
</table>
```

## Ledger rows

Two-column ruled definition rows — glossaries, config keys, API fields.
Lighter than a table.

```html
<div class="ledger">
  <div class="row"><b>DIGEST_WINDOW</b><span>Hours bundled per digest. Default 24.</span></div>
  <div class="row"><b>FLUSH_INTERVAL</b><span>Seconds between buffer flushes. Default 300.</span></div>
</div>
```

## Code blocks

Night Owl styling on any `pre > code`. For syntax color, hand-wrap tokens —
only for short, load-bearing snippets (`tok-k` keyword, `tok-s` string,
`tok-f` function, `tok-n` number, `tok-c` comment):

```html
<div class="codeblock">
  <span class="code-label">digest/consumer.ts</span>
  <pre><code><span class="tok-k">export async function</span> <span class="tok-f">flush</span>(userId: <span class="tok-k">string</span>) {
  <span class="tok-c">// must stay idempotent</span>
  <span class="tok-k">const</span> batch = <span class="tok-k">await</span> <span class="tok-f">collect</span>(userId, <span class="tok-s">"pending"</span>);
}</code></pre>
</div>
```

Keep code ≤ 18 lines per slide. A 40-line schema is two slides (`schema 1/2`)
or an appendix slide. Never shrink the font to fit.

## Terminal sessions

Use `pre.term` so whitespace is preserved. `ps1` = prompt (non-selectable so
copy-paste grabs only the command), `out` = output, `cmt` = comment.

```html
<pre class="term"><span class="ps1">$</span> pnpm db:migrate
<span class="out">Applied 002_add_digest_id.sql (14ms)</span>
<span class="cmt"># safe to re-run; migrations are tracked</span></pre>
```

## Diagrams

HTML boxes + unicode arrows (→ ↓ ⇄), never absolute-positioned text — that's
why they survive every viewport. `hot` highlights the box under discussion.

```html
<div class="diagram">
  <div class="drow">
    <div class="box">Webhook<small>receiver</small></div>
    <span class="arrow">→</span>
    <div class="box hot">Queue<small>per-user buffer</small></div>
    <span class="arrow">→</span>
    <div class="box">Renderer<small>MJML → HTML</small></div>
  </div>
  <div class="drow"><span class="arrow">↓</span></div>
  <div class="drow"><div class="box">Postmark</div></div>
  <div class="dcaption">Digest pipeline — buffer flush every 5 min</div>
</div>
```

For genuinely 2D topologies, hand-written inline `<svg>` is fine — keep text in
the page fonts so it inherits the theme.

## Decision records (ADR)

One `.decision` per decision — standalone ADR deck or a "Decisions" slide in a
spec.

```html
<div class="decision">
  <div class="decision-head">
    <b>D3: Store digests as rendered HTML, not template + data</b>
    <span class="tag pass">accepted</span>
    <span class="tag">2026-08-04</span>
  </div>
  <p>Digest content is rendered at send time and stored verbatim.</p>
  <p class="drivers"><b>Because:</b> resend must reproduce exactly what the user saw. <b>Traded away:</b> storage (~40KB/digest), re-render flexibility.</p>
</div>
```

## Pixel frame

Corner-bracket frame for a hero element — the title slide's h1 block, a
closing "the ask" panel. One per deck, maybe two; it's an accent, not a border
style.

```html
<div class="pixel-frame">
  <h2>The ask</h2>
  <p>Green-light phase 1 — two evenings of work, reversible.</p>
</div>
```
