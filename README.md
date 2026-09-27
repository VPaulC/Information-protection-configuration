<!DOCTYPE html>
<html lang="en-GB" dir="ltr">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<meta name="color-scheme" content="light dark">
<title>Microsoft Purview Information Protection — Common Scenarios &amp; Label Configuration</title>
<style>
:root {
  color-scheme: light dark;
  --bg: #f5f6f8;
  --surface: #ffffff;
  --surface-alt: #eef1f5;
  --ink: #1b1d21;
  --ink-soft: #52575e;
  --line: #d9dde3;
  --line-strong: #b9c0c9;
  --accent: #1f4e79;
  --accent-soft: #e6eef6;
  --public: #1e7a45;
  --public-bg: #e4f4ea;
  --general: #1f5da8;
  --general-bg: #e5edf8;
  --conf: #9a5b00;
  --conf-bg: #fbeed8;
  --high: #a32020;
  --high-bg: #fae5e5;
  --code-bg: #eef1f5;
  --shadow: 0 1px 2px rgba(20,25,35,.08), 0 6px 18px rgba(20,25,35,.06);
  --radius: 10px;
  --maxw: 1100px;
  --nav-h: 76px;        /* sticky nav height + breathing room, for scroll-margin */
  --sticky-top: 56px;   /* where the quiz scorebar parks, under the nav */
  --tap: 44px;          /* minimum comfortable tap target */
  --gutter: 20px;
}
@media (prefers-color-scheme: dark) {
  :root {
    --bg: #15171b;
    --surface: #1e2126;
    --surface-alt: #24282e;
    --ink: #e9ebee;
    --ink-soft: #a9b0b9;
    --line: #333941;
    --line-strong: #454c56;
    --accent: #7fb2e5;
    --accent-soft: #1f2a35;
    --public: #63c98d;
    --public-bg: #16301f;
    --general: #7cb0ef;
    --general-bg: #172436;
    --conf: #e0a44a;
    --conf-bg: #33260f;
    --high: #f08b8b;
    --high-bg: #361a1a;
    --code-bg: #262a30;
    --shadow: 0 1px 2px rgba(0,0,0,.4), 0 6px 18px rgba(0,0,0,.3);
  }
}
* { box-sizing: border-box; }
html { -webkit-text-size-adjust: 100%; }
body {
  margin: 0;
  background: var(--bg);
  color: var(--ink);
  font-family: "Segoe UI", -apple-system, BlinkMacSystemFont, Roboto, "Helvetica Neue", Arial, sans-serif;
  font-size: 16px;
  line-height: 1.6;
  overflow-wrap: break-word;
}
img, svg, video, table, pre { max-inline-size: 100%; }
/* `anywhere` only on small screens: it changes intrinsic min-content
   widths, which would reflow the wide desktop table. */
.wrap { max-width: var(--maxw); margin-inline: auto; padding-inline: var(--gutter); }

/* ── Header ─────────────────────────────────────────────── */
header.masthead {
  background: var(--surface);
  border-block-end: 3px solid var(--accent);
  padding-block: 28px 22px;
}
header.masthead h1 { margin: 0 0 6px; font-size: 1.75rem; line-height: 1.25; }
header.masthead p.sub { margin: 0; color: var(--ink-soft); font-size: 1rem; }

/* ── Nav ────────────────────────────────────────────────── */
nav.toc {
  position: sticky;
  inset-block-start: 0;
  z-index: 10;
  background: var(--surface-alt);
  border-block-end: 1px solid var(--line);
  padding-block: 10px;
}
nav.toc ul {
  list-style: none;
  margin: 0; padding: 0;
  display: flex; flex-wrap: wrap; gap: 6px;
}
nav.toc a {
  display: inline-block;
  padding: 4px 10px;
  font-size: .8rem;
  text-decoration: none;
  color: var(--ink);
  background: var(--surface);
  border: 1px solid var(--line);
  border-radius: 999px;
}
nav.toc a:hover, nav.toc a:focus-visible { background: var(--accent-soft); border-color: var(--accent); }
nav.toc a.active { background: var(--accent-soft); border-color: var(--accent); color: var(--accent); font-weight: 600; }
a { color: var(--accent); }
a:focus-visible, button:focus-visible { outline: 3px solid var(--accent); outline-offset: 2px; }

main { padding-block: 28px 48px; }
section { scroll-margin-block-start: calc(var(--nav-h) + 8px); }
h2 { font-size: 1.3rem; margin-block: 36px 12px; padding-block-end: 6px; border-block-end: 2px solid var(--line-strong); }
h3 { font-size: 1.05rem; margin-block: 20px 8px; }
p { margin-block: 0 12px; }

.intro {
  background: var(--surface);
  border: 1px solid var(--line);
  border-radius: var(--radius);
  padding: 20px 22px;
  box-shadow: var(--shadow);
}
.taxonomy {
  list-style: none; margin: 14px 0 0; padding: 0;
  display: grid; gap: 8px;
}
.taxonomy li {
  padding: 8px 12px;
  background: var(--surface-alt);
  border-inline-start: 4px solid var(--line-strong);
  border-radius: 4px;
  font-size: .92rem;
}

/* ── Badges ─────────────────────────────────────────────── */
.badge {
  display: inline-block;
  padding: 2px 10px;
  border-radius: 999px;
  font-size: .75rem;
  font-weight: 600;
  letter-spacing: .02em;
  white-space: nowrap;
  border: 1px solid currentColor;
}
.badge.public  { color: var(--public); background: var(--public-bg); }
.badge.general { color: var(--general); background: var(--general-bg); }
.badge.conf    { color: var(--conf); background: var(--conf-bg); }
.badge.high    { color: var(--high); background: var(--high-bg); }
.badge.neutral { color: var(--ink-soft); background: var(--surface-alt); }

/* ── Scenario cards ─────────────────────────────────────── */
.card {
  background: var(--surface);
  border: 1px solid var(--line);
  border-radius: var(--radius);
  box-shadow: var(--shadow);
  padding: 20px 22px;
  margin-block-end: 20px;
  border-inline-start: 6px solid var(--line-strong);
}
.card.t-public  { border-inline-start-color: var(--public); }
.card.t-general { border-inline-start-color: var(--general); }
.card.t-conf    { border-inline-start-color: var(--conf); }
.card.t-high    { border-inline-start-color: var(--high); }
.card.t-mixed   { border-inline-start-color: var(--accent); }
.card > h3 { margin-block-start: 0; display: flex; flex-wrap: wrap; gap: 10px; align-items: center; }
.card .num {
  font-size: .78rem; font-weight: 700; color: var(--ink-soft);
  background: var(--surface-alt); border-radius: 4px; padding: 1px 7px;
}
dl.meta { margin: 0 0 4px; }
dl.meta > div {
  display: grid;
  grid-template-columns: 190px 1fr;
  gap: 10px;
  padding-block: 7px;
  border-block-start: 1px solid var(--line);
}
dl.meta dt { font-weight: 600; font-size: .86rem; color: var(--ink-soft); text-transform: uppercase; letter-spacing: .03em; }
dl.meta dd { margin: 0; }
dl.meta ul { margin: 0; padding-inline-start: 20px; }
dl.meta li { margin-block-end: 4px; }
@media (max-width: 680px) {
  dl.meta > div { grid-template-columns: 1fr; gap: 2px; }
}
code, .mono {
  font-family: Consolas, "SF Mono", Menlo, monospace;
  font-size: .87em;
  background: var(--code-bg);
  padding: 1px 5px;
  border-radius: 4px;
}

/* ── Table ──────────────────────────────────────────────── */
.table-scroll { overflow-x: auto; border: 1px solid var(--line); border-radius: var(--radius); background: var(--surface); -webkit-overflow-scrolling: touch; }
.table-scroll:focus-visible { outline: 3px solid var(--accent); outline-offset: 2px; }
.scroll-hint { display: none; font-size: .82rem; color: var(--ink-soft); margin-block: 0 6px; }
@media screen and (min-width: 821px) and (max-width: 1000px) { .scroll-hint { display: block; } }
table { border-collapse: collapse; width: 100%; min-width: 900px; font-size: .84rem; }
caption { text-align: start; padding: 12px 14px; color: var(--ink-soft); font-size: .85rem; }
th, td { padding: 9px 12px; text-align: start; vertical-align: top; border-block-start: 1px solid var(--line); }
thead th { background: var(--surface-alt); border-block-end: 2px solid var(--line-strong); position: sticky; inset-block-start: 0; }
tbody tr:nth-child(even) { background: var(--surface-alt); }

/* ── Callouts ───────────────────────────────────────────── */
.callout {
  border: 1px solid var(--line);
  border-inline-start: 5px solid var(--accent);
  background: var(--accent-soft);
  border-radius: var(--radius);
  padding: 14px 18px;
  margin-block: 18px;
}
.callout.warn { border-inline-start-color: var(--conf); background: var(--conf-bg); }
.callout h3 { margin-block-start: 0; }
.pitfalls { list-style: none; margin: 0; padding: 0; display: grid; gap: 12px; }
.pitfalls li {
  background: var(--surface); border: 1px solid var(--line);
  border-radius: var(--radius); padding: 12px 16px; box-shadow: var(--shadow);
}
.pitfalls strong { display: block; margin-block-end: 3px; }

footer.page-foot {
  border-block-start: 1px solid var(--line);
  padding-block: 18px 40px;
  color: var(--ink-soft);
  font-size: .85rem;
}
.toplink { font-size: .78rem; margin-inline-start: auto; }

/* ── Print ──────────────────────────────────────────────── */
@media print {
  :root { --bg: #fff; --surface: #fff; --surface-alt: #fff; --shadow: none; }
  body { font-size: 10.5pt; background: #fff; color: #000; }
  nav.toc, .toplink { display: none; }
  .card, .intro, .pitfalls li, .callout { break-inside: avoid; page-break-inside: avoid; box-shadow: none; }
  h2 { break-after: avoid; }
  table { min-width: 0; font-size: 8.5pt; }
  thead th { position: static; }
  a { color: #000; text-decoration: none; }
}

/* ══════════════════════════════════════════════════════════
   Interactive layer — everything below is progressive
   enhancement. Without JavaScript nothing is hidden.
   ══════════════════════════════════════════════════════════ */

html { scroll-behavior: smooth; }
@media (prefers-reduced-motion: reduce) { html { scroll-behavior: auto; } }

/* Controls that only make sense when script runs */
html:not(.js) .js-only { display: none !important; }
html.js .nojs-only { display: none; }

/* ── Buttons ─────────────────────────────────────────────── */
.btn {
  font: inherit;
  font-size: .82rem;
  font-weight: 600;
  color: var(--ink);
  background: var(--surface);
  border: 1px solid var(--line-strong);
  border-radius: 999px;
  padding: 5px 14px;
  cursor: pointer;
  line-height: 1.4;
}
.btn:hover { background: var(--accent-soft); border-color: var(--accent); }
.btn[aria-pressed="true"] { background: var(--accent-soft); border-color: var(--accent); color: var(--accent); }
.btn.primary { background: var(--accent-soft); border-color: var(--accent); color: var(--accent); }

/* ── Toolbar (filter + expand/collapse), injected by script ─ */
.toolbar {
  background: var(--surface);
  border: 1px solid var(--line);
  border-radius: var(--radius);
  box-shadow: var(--shadow);
  padding: 14px 16px;
  margin-block-end: 20px;
  display: grid;
  gap: 12px;
}
.toolbar-row { display: flex; flex-wrap: wrap; gap: 8px; align-items: center; }
.toolbar label.field { display: flex; align-items: center; gap: 8px; flex: 1 1 260px; }
.toolbar input[type="search"] {
  font: inherit;
  font-size: .9rem;
  flex: 1 1 auto;
  min-width: 0;
  padding: 7px 12px;
  color: var(--ink);
  background: var(--surface-alt);
  border: 1px solid var(--line-strong);
  border-radius: 999px;
}
.toolbar .group-label {
  font-size: .74rem;
  font-weight: 700;
  text-transform: uppercase;
  letter-spacing: .04em;
  color: var(--ink-soft);
  inline-size: 84px;
  flex: 0 0 auto;
}
.chip { font-size: .78rem; padding: 4px 12px; }
.chip.t-public[aria-pressed="true"]  { color: var(--public);  background: var(--public-bg);  border-color: currentColor; }
.chip.t-general[aria-pressed="true"] { color: var(--general); background: var(--general-bg); border-color: currentColor; }
.chip.t-conf[aria-pressed="true"]    { color: var(--conf);    background: var(--conf-bg);    border-color: currentColor; }
.chip.t-high[aria-pressed="true"]    { color: var(--high);    background: var(--high-bg);    border-color: currentColor; }
.filter-status { font-size: .82rem; color: var(--ink-soft); }
.card.is-filtered-out { display: none; }

/* ── Accordion ───────────────────────────────────────────── */
.card > h3 > button.acc-toggle {
  font: inherit;
  font-size: 1.05rem;
  font-weight: 600;
  color: inherit;
  background: none;
  border: 0;
  padding: 0;
  margin: 0;
  cursor: pointer;
  text-align: start;
  display: flex;
  flex-wrap: wrap;
  gap: 10px;
  align-items: center;
  flex: 1 1 auto;
}
button.acc-toggle::before {
  content: "▸";
  display: inline-block;
  color: var(--ink-soft);
  font-size: .8em;
  transition: transform .15s ease-in-out;
}
button.acc-toggle[aria-expanded="true"]::before { transform: rotate(90deg); }
@media (prefers-reduced-motion: reduce) { button.acc-toggle::before { transition: none; } }
button.acc-toggle:hover { color: var(--accent); }
.card-body.is-collapsed,
.config-region.is-collapsed,
.reveal-region.is-collapsed,
.quiz-answer.is-collapsed { display: none; }

/* ── Progressive reveal of the configuration ─────────────── */
.config-controls { margin-block: 10px 0; }
.config-region { margin-block-start: 6px; }
.card .hint {
  font-size: .82rem;
  color: var(--ink-soft);
  margin-block: 8px 0;
}

/* ── Quiz ────────────────────────────────────────────────── */
.quiz-q {
  background: var(--surface);
  border: 1px solid var(--line);
  border-inline-start: 6px solid var(--accent);
  border-radius: var(--radius);
  box-shadow: var(--shadow);
  padding: 16px 20px;
  margin-block-end: 18px;
}
.quiz-q > p.stem { font-weight: 600; margin-block-end: 10px; }
.quiz-q fieldset { border: 0; margin: 0; padding: 0; min-inline-size: 0; }
.quiz-q legend { font-weight: 600; padding: 0; margin-block-end: 10px; }
.opt {
  display: flex;
  gap: 10px;
  align-items: flex-start;
  padding: 8px 12px;
  margin-block-end: 6px;
  background: var(--surface-alt);
  border: 1px solid var(--line);
  border-radius: 6px;
  font-size: .92rem;
  cursor: pointer;
}
.opt:hover { border-color: var(--line-strong); }
.opt input { margin-block-start: 4px; flex: 0 0 auto; }
.opt > span { min-inline-size: 0; }
.opt.is-correct { color: var(--public); background: var(--public-bg); border-color: currentColor; }
.opt.is-wrong   { color: var(--high);   background: var(--high-bg);   border-color: currentColor; }
.quiz-answer {
  margin-block-start: 10px;
  padding: 12px 14px;
  background: var(--surface-alt);
  border: 1px solid var(--line);
  border-inline-start: 4px solid var(--line-strong);
  border-radius: 6px;
  font-size: .9rem;
}
.quiz-answer p { margin-block-end: 8px; }
.quiz-answer p:last-child { margin-block-end: 0; }
.quiz-answer .verdict { font-weight: 700; }
.quiz-answer .verdict.right { color: var(--public); }
.quiz-answer .verdict.wrong { color: var(--high); }
.scorebar {
  position: sticky;
  inset-block-start: var(--sticky-top);
  z-index: 5;
  display: flex;
  flex-wrap: wrap;
  gap: 10px;
  align-items: center;
  background: var(--accent-soft);
  border: 1px solid var(--accent);
  border-radius: var(--radius);
  padding: 10px 16px;
  margin-block-end: 16px;
  font-size: .9rem;
  font-weight: 600;
}
.scorebar .spacer { margin-inline-start: auto; }

/* ── Worksheet ───────────────────────────────────────────── */
.worksheet {
  background: var(--surface);
  border: 1px solid var(--line);
  border-radius: var(--radius);
  box-shadow: var(--shadow);
  padding: 18px 20px;
  margin-block: 16px;
}
.ws-field { display: grid; gap: 4px; margin-block-end: 14px; }
.ws-field > label { font-weight: 600; font-size: .86rem; color: var(--ink-soft); text-transform: uppercase; letter-spacing: .03em; }
.ws-field .help { font-size: .84rem; color: var(--ink-soft); margin: 0; }
.ws-field input[type="text"],
.ws-field select,
.ws-field textarea {
  font: inherit;
  font-size: .92rem;
  color: var(--ink);
  background: var(--surface-alt);
  border: 1px solid var(--line-strong);
  border-radius: 6px;
  padding: 7px 10px;
  inline-size: 100%;
  max-inline-size: 100%;
}
.ws-field textarea { min-block-size: 66px; resize: vertical; }
.ws-grid { display: grid; gap: 0 20px; grid-template-columns: repeat(2, minmax(0, 1fr)); }
@media (max-width: 680px) { .ws-grid { grid-template-columns: 1fr; } }
.checkline { display: flex; flex-wrap: wrap; gap: 6px 16px; }
.checkline label { display: flex; gap: 6px; align-items: center; font-size: .9rem; }
.model-answer {
  border: 1px solid var(--line);
  border-inline-start: 5px solid var(--public);
  background: var(--surface);
  border-radius: var(--radius);
  padding: 16px 20px;
  margin-block: 14px;
}
.model-answer h4 { margin-block: 16px 6px; font-size: .95rem; }
.model-answer h4:first-child { margin-block-start: 0; }

/* ── Print: expand everything, show typed answers ────────── */
@media print {
  .toolbar, .scorebar, .config-controls, .js-only, .btn, .filter-status { display: none !important; }
  .card-body.is-collapsed,
  .config-region.is-collapsed,
  .reveal-region.is-collapsed,
  .quiz-answer.is-collapsed,
  .card.is-filtered-out { display: block !important; }
  button.acc-toggle::before { display: none; }
  button.acc-toggle { font-size: 1.05rem; }
  .quiz-q, .worksheet, .model-answer, .ws-field { break-inside: avoid; page-break-inside: avoid; }
  .ws-field input[type="text"], .ws-field select, .ws-field textarea {
    background: #fff;
    border: 1px solid #888;
    color: #000;
    overflow: visible;
  }
  .opt.is-correct, .opt.is-wrong { border-width: 2px; }
}

/* ══════════════════════════════════════════════════════════
   Mobile & touch layer — phones from 320px up, and tablets.
   Desktop rules above are untouched; everything here is
   screen-only or pointer-conditional so print keeps working.
   ══════════════════════════════════════════════════════════ */

/* ── Touch targets: ≥44px effective, on small screens and on
      any coarse pointer (tablets, touch laptops). ─────────── */
@media screen and (max-width: 900px), screen and (pointer: coarse) {
  nav.toc a, .btn, .chip {
    min-block-size: var(--tap);
    display: inline-flex;
    align-items: center;
    justify-content: center;
    padding-inline: 14px;
  }
  nav.toc a { font-size: .85rem; padding-block: 6px; }
  .btn, .chip { font-size: .88rem; padding-block: 6px; }

  /* 16px minimum on form controls: anything smaller makes iOS
     zoom the page on focus. */
  .toolbar input[type="search"],
  .ws-field input[type="text"],
  .ws-field select,
  .ws-field textarea {
    font-size: 16px;
    min-block-size: var(--tap);
    padding: 10px 14px;
  }
  .ws-field textarea { min-block-size: 120px; padding-block: 10px; }

  .card > h3 > button.acc-toggle {
    min-block-size: var(--tap);
    padding-block: 6px;
    padding-inline-end: 4px;
  }
  .card > h3 { gap: 8px; }

  /* Quiz options: full-width tappable rows. */
  .opt {
    inline-size: 100%;
    min-block-size: var(--tap);
    align-items: center;
    gap: 12px;
    padding: 12px 14px;
    margin-block-end: 8px;
  }
  .opt input[type="radio"] { inline-size: 22px; block-size: 22px; margin-block-start: 0; }

  .checkline { gap: 4px 14px; }
  .checkline label { min-block-size: var(--tap); gap: 10px; font-size: .95rem; }
  .checkline input[type="checkbox"] { inline-size: 22px; block-size: 22px; }

  .toolbar-row { gap: 10px; }
  .config-controls .btn, .worksheet .btn { margin-block-end: 2px; }
}

/* ── No hover-only affordances: give touch an :active echo and
      stop sticky hover states on touch devices. ───────────── */
.btn:active { background: var(--accent-soft); border-color: var(--accent); }
nav.toc a:active { background: var(--accent-soft); border-color: var(--accent); }
.opt:not(.is-correct):not(.is-wrong):active { border-color: var(--line-strong); }
@media screen and (hover: none) {
  .btn:not([aria-pressed="true"]):not(.primary):hover { background: var(--surface); border-color: var(--line-strong); }
  nav.toc a:not(.active):hover { background: var(--surface); border-color: var(--line); }
  .opt:not(.is-correct):not(.is-wrong):hover { border-color: var(--line); }
  .card > h3 > button.acc-toggle:hover { color: inherit; }
}

/* ── Tablet: the wide summary table still scrolls sideways,
      and says so. ─────────────────────────────────────────── */
@media screen and (max-width: 1000px) {
  .table-scroll { position: relative; }
}

/* ── Tablets and phones: the pill nav collapses into a single
      swipeable strip instead of wrapping into a wall of rows. ── */
@media screen and (max-width: 900px) {
  :root {
    --nav-h: 70px;      /* strip is ~61px tall — well under 20% of any viewport */
    --sticky-top: 62px; /* quiz scorebar parks just under the strip */
  }

  /* Fluid type: headings scale down with the viewport instead of
     dominating a 320px screen. Screen-only, so print is unchanged. */
  header.masthead h1 { font-size: clamp(1.3rem, 1.02rem + 1.5vw, 1.75rem); }
  header.masthead p.sub { font-size: clamp(.92rem, .88rem + .2vw, 1rem); }
  h2 { font-size: clamp(1.12rem, 1.02rem + .5vw, 1.3rem); }
  h3, .card > h3 > button.acc-toggle { font-size: clamp(1rem, .96rem + .22vw, 1.05rem); }
  .card .hint, .filter-status { font-size: .85rem; }

  /* Nav becomes a single-row swipeable strip: ~60px tall, well
     under a fifth of a 480px-tall viewport, with no scrollbar
     chrome and momentum scrolling. */
  nav.toc {
    padding-block: 8px;
    padding-block-start: calc(8px + env(safe-area-inset-top, 0px));
  }
  nav.toc .wrap { padding-inline: 0; max-width: none; }
  nav.toc ul {
    flex-wrap: nowrap;
    overflow-x: auto;
    overscroll-behavior-inline: contain;
    -webkit-overflow-scrolling: touch;
    scroll-snap-type: x proximity;
    scroll-padding-inline: var(--gutter);
    scrollbar-width: none;
    -ms-overflow-style: none;
    gap: 8px;
    padding-inline:
      max(var(--gutter), env(safe-area-inset-left, 0px))
      max(var(--gutter), env(safe-area-inset-right, 0px));
  }
  nav.toc ul::-webkit-scrollbar { inline-size: 0; block-size: 0; display: none; }
  nav.toc li { flex: 0 0 auto; scroll-snap-align: start; }
  nav.toc a { white-space: nowrap; }
}

/* ── Phones and small tablets ──────────────────────────────── */
@media screen and (max-width: 820px) {
  :root { --gutter: 16px; }
  body { padding-block-end: env(safe-area-inset-bottom, 0px); }
  .wrap {
    padding-inline:
      max(var(--gutter), env(safe-area-inset-left, 0px))
      max(var(--gutter), env(safe-area-inset-right, 0px));
  }

  header.masthead { padding-block: 18px 14px; }
  main { padding-block: 20px 36px; }
  h2 { margin-block: 26px 10px; }

  /* Cards, callouts and panels: tighter, fluid padding. */
  .intro, .card, .quiz-q, .worksheet, .model-answer { padding: 16px 14px; }
  .callout { padding: 14px; }
  /* Long label names must wrap rather than push the page wide. */
  .badge { white-space: normal; }
  code, .mono, .badge, a[href], td, dd, li { overflow-wrap: anywhere; }
  .pitfalls li { padding: 12px 14px; }
  .card { border-inline-start-width: 5px; margin-block-end: 16px; }
  .taxonomy li { font-size: .95rem; }

  /* Definition lists stack label over value. */
  dl.meta > div { grid-template-columns: 1fr; gap: 2px; }

  /* Toolbar: each group gets its own line, chips wrap. */
  .toolbar { padding: 14px; }
  .toolbar .group-label { inline-size: auto; flex: 1 0 100%; }
  .toolbar label.field { flex: 1 1 100%; flex-wrap: wrap; gap: 6px; }
  .toolbar input[type="search"] { flex: 1 1 100%; }
  .toolbar-row { align-items: stretch; }

  /* Scorebar sits directly under the nav strip and wraps. */
  .scorebar { padding: 10px 14px; font-size: .88rem; }
  .scorebar .spacer { margin-inline-start: 0; }

  /* Worksheet: one column, labels above fields. */
  .ws-grid { grid-template-columns: 1fr; }
  .ws-field { margin-block-end: 16px; }

  /* Summary table becomes stacked cards, one per label, using
     the header text carried on each cell as its own label. */
  .scroll-hint { display: none; }
  .table-scroll {
    overflow-x: visible;
    border: 0;
    border-radius: 0;
    background: transparent;
  }
  table { display: block; min-width: 0; inline-size: 100%; font-size: .9rem; }
  table caption { display: block; inline-size: 100%; padding-inline: 0; }
  thead {
    position: absolute;
    inline-size: 1px; block-size: 1px;
    overflow: hidden;
    clip-path: inset(50%);
    white-space: nowrap;
  }
  tbody { display: block; }
  tbody tr,
  tbody tr:nth-child(even) {
    display: block;
    background: var(--surface);
    border: 1px solid var(--line);
    border-radius: var(--radius);
    box-shadow: var(--shadow);
    padding: 6px 4px;
    margin-block-end: 12px;
  }
  tbody td {
    display: block;
    border: 0;
    border-block-start: 1px solid var(--line);
    padding: 8px 12px;
  }
  tbody td:first-child { border-block-start: 0; }
  tbody td::before {
    content: attr(data-label);
    display: block;
    font-size: .7rem;
    font-weight: 700;
    text-transform: uppercase;
    letter-spacing: .04em;
    color: var(--ink-soft);
    margin-block-end: 2px;
  }

  footer.page-foot {
    padding-block: 18px calc(32px + env(safe-area-inset-bottom, 0px));
  }
}

/* ── Very narrow phones (320px) ────────────────────────────── */
@media screen and (max-width: 360px) {
  :root { --gutter: 12px; }
  .intro, .card, .quiz-q, .worksheet, .model-answer { padding: 14px 12px; }
  .toolbar { padding: 12px; }
  .btn, .chip { padding-inline: 12px; }
}

/* Print is deliberately untouched by this layer: every rule above
   is screen-scoped, and the scroll hint is hidden by default. */
</style>
<script>document.documentElement.classList.add("js");</script>
</head>
<body>

<header class="masthead">
  <div class="wrap">
    <h1>Microsoft Purview Information Protection — Common Scenarios</h1>
    <p class="sub">A practical reference for designing sensitivity labels: what each scenario looks like, and how the label should be configured.</p>
  </div>
</header>

<nav class="toc" aria-label="Scenario navigation">
  <div class="wrap">
    <ul>
      <li><a href="#taxonomy">Taxonomy</a></li>
      <li><a href="#s1">1 · Public</a></li>
      <li><a href="#s2">2 · General</a></li>
      <li><a href="#s3">3 · Confidential internal</a></li>
      <li><a href="#s4">4 · Confidential external</a></li>
      <li><a href="#s5">5 · Highly Confidential</a></li>
      <li><a href="#s6">6 · PII &amp; payment data</a></li>
      <li><a href="#s7">7 · Email-only</a></li>
      <li><a href="#s8">8 · Containers</a></li>
      <li><a href="#s9">9 · Meetings &amp; chat</a></li>
      <li><a href="#s10">10 · Files at rest</a></li>
      <li><a href="#s11">11 · External collaboration</a></li>
      <li><a href="#s12">12 · Policy settings</a></li>
      <li><a href="#summary">Summary table</a></li>
      <li><a href="#pitfalls">Pitfalls</a></li>
      <li><a href="#quiz">Self-check</a></li>
      <li><a href="#yourturn">Your turn</a></li>
    </ul>
  </div>
</nav>

<main class="wrap">

<!-- ══ INTRO ══ -->
<section id="taxonomy" aria-labelledby="taxonomy-h">
  <h2 id="taxonomy-h">The taxonomy model</h2>
  <div class="intro">
    <p>A sensitivity label is a single piece of metadata that travels with the content and can carry three kinds of behaviour: <strong>visual marking</strong> (header, footer, watermark), <strong>encryption with usage rights</strong> (Azure Rights Management), and <strong>downstream signals</strong> that DLP, Defender for Cloud Apps, auto-labelling and container policies can key on. A label is applied to a file, an email, a meeting, a Teams/SharePoint container, or a Power BI/Fabric item, and it persists with the content wherever it travels.</p>
    <p>Design the taxonomy around <em>business language</em>, not technical controls. Users should be able to choose a label from the name and tooltip alone, without knowing what RMS, a usage right or a sensitive information type is. “Confidential — External Sharing” is a good name; “AES-256 + Co-Author + Expiry 30d” is not. The control configuration lives behind the label, not in its name.</p>
    <p>The proven shape is <strong>four or five top-level labels in increasing order of sensitivity, with a small number of sublabels</strong> where a single tier needs different audiences. Label order matters: it defines the sensitivity ranking used for downgrade detection and for the “highest label wins” logic in containers and attachments.</p>
    <ul class="taxonomy">
      <li><span class="badge public">Public</span> &nbsp;Approved for release outside the organisation. No protection.</li>
      <li><span class="badge general">General</span> &nbsp;Ordinary internal business content. The default label. Marking only.</li>
      <li><span class="badge conf">Confidential</span> &nbsp;Damage if disclosed. Encrypted. Sublabels: <em>Internal Only</em>, <em>External Sharing</em>, <em>Anyone (unencrypted)</em>.</li>
      <li><span class="badge high">Highly Confidential</span> &nbsp;Severe damage if disclosed. Encrypted, tightly scoped. Sublabels per project / function.</li>
    </ul>
    <p style="margin-block-start:14px;margin-block-end:0;"><strong>Parent labels with sublabels cannot be applied directly</strong> — only the sublabel is selectable. Keep the sublabel count low: every extra choice is a decision the user has to make correctly, every time.</p>
  </div>
</section>

<h2 id="scenarios">Scenarios</h2>

<!-- ══ 1 ══ -->
<section id="s1" data-tier="public" data-workload="files email" class="card t-public" aria-labelledby="s1-h">
  <h3 id="s1-h"><span class="num">01</span> Public / marketing material <span class="badge public">Public</span> <a class="toplink" href="#taxonomy">↑ top</a></h3>
  <p>Press releases, published datasheets, the public website copy, brochures, job adverts, published annual reports — content that has been cleared for release and where the risk is <em>inappropriate internal handling</em>, not disclosure.</p>
  <dl class="meta">
    <div><dt>Applies to</dt><dd>Marketing, Communications, HR recruitment, Investor Relations; available to all users.</dd></div>
    <div><dt>Recommended label</dt><dd><span class="badge public">Public</span> — top-level, no sublabels.</dd></div>
    <div><dt>Scope</dt><dd>Files &amp; emails; Meetings; Groups &amp; sites (optional — rarely useful on containers).</dd></div>
    <div><dt>Encryption / rights</dt><dd><strong>None.</strong> Do not encrypt. Encryption here breaks the entire purpose of the content and blocks publication pipelines, CMS ingestion and PDF conversion.</dd></div>
    <div><dt>Content marking</dt><dd>Normally none. Some organisations apply a discreet footer such as <code>Public</code> or <code>Approved for external release</code> to signal that the review gate has been passed. Keep it off documents that go to print/design.</dd></div>
    <div><dt>Auto-labelling</dt><dd>None. Public is an act of human judgement — never infer it. Auto-labelling <em>to</em> Public would systematically downgrade content.</dd></div>
    <div><dt>Policy settings</dt><dd>Published to all users. Not the default label. Selecting Public from a higher label is a <strong>downgrade</strong> and should therefore be caught by the justification prompt.</dd></div>
    <div><dt>Watch out</dt><dd>Because Public is the least restrictive label, it is the label users reach for to escape protection. Monitor Public label activity in Activity Explorer, and consider restricting it to a publisher group if abuse appears.</dd></div>
  </dl>
</section>

<!-- ══ 2 ══ -->
<section id="s2" data-tier="general" data-workload="files email collab" class="card t-general" aria-labelledby="s2-h">
  <h3 id="s2-h"><span class="num">02</span> General internal business content <span class="badge general">General</span> <a class="toplink" href="#taxonomy">↑ top</a></h3>
  <p>The bulk of day-to-day work: project plans, meeting notes, internal presentations, process documents, most email. Not sensitive enough to justify encryption, but not intended for the outside world.</p>
  <dl class="meta">
    <div><dt>Applies to</dt><dd>Every user in the tenant. This is the label that most content should end up with.</dd></div>
    <div><dt>Recommended label</dt><dd><span class="badge general">General</span> — the <strong>default label</strong> for documents and email.</dd></div>
    <div><dt>Scope</dt><dd>Files &amp; emails; Meetings; Groups &amp; sites.</dd></div>
    <div><dt>Encryption / rights</dt><dd><strong>None.</strong> Encrypting the default label encrypts everything, and is the single most common cause of a failed rollout — it breaks search indexing behaviour expectations, third-party tooling, PDF/print workflows and external sharing of harmless content.</dd></div>
    <div><dt>Content marking</dt><dd>Footer on documents and email: <code>General — for internal use</code>, or with dynamic text such as <code>${Item.Label} — ${Item.Name}</code>. Small point size, muted colour. No watermark (it makes ordinary documents look alarming and is distracting when printed).</dd></div>
    <div><dt>Auto-labelling</dt><dd>Not condition-based. General is reached via the <em>default label</em> policy setting rather than by detection.</dd></div>
    <div><dt>Policy settings</dt><dd>Default label for documents, email, meetings and new containers. Combine with mandatory labelling so untouched content still lands here. Users must be able to move up (to Confidential) without friction and down (to Public) only with justification.</dd></div>
  </dl>
</section>

<!-- ══ 3 ══ -->
<section id="s3" data-tier="confidential" data-workload="files email collab auto" class="card t-conf" aria-labelledby="s3-h">
  <h3 id="s3-h"><span class="num">03</span> Confidential — Internal Only <span class="badge conf">Confidential ▸ Internal Only</span> <a class="toplink" href="#taxonomy">↑ top</a></h3>
  <p>Content whose disclosure would cause real harm but which the whole workforce may legitimately read or contribute to: internal financials before release, org plans, pricing models, architecture and security documentation, commercial contracts under negotiation.</p>
  <dl class="meta">
    <div><dt>Applies to</dt><dd>All employees; typically authored by Finance, Legal, Strategy, IT and Engineering.</dd></div>
    <div><dt>Recommended label</dt><dd>Sublabel <strong>Confidential ▸ Internal Only</strong>.</dd></div>
    <div><dt>Scope</dt><dd>Files &amp; emails; Meetings; Groups &amp; sites.</dd></div>
    <div><dt>Encryption / rights</dt><dd>
      <ul>
        <li><strong>Encryption: On</strong>, permissions assigned <em>now</em> by the administrator (not user-defined).</li>
        <li>Assign to a single <strong>mail-enabled security group containing all employees</strong>. Never use “everyone in the organisation” loosely — use one deliberate group so the boundary is auditable and guests are excluded.</li>
        <li>Rights: <strong>Co-Author</strong> (view, edit, copy, print, reply, reply all, forward — but not Full Control / change permissions). Co-Author preserves Office co-authoring and AutoSave.</li>
        <li>Content expiry: <strong>Never</strong>. Offline access: default (typically 7–30 days) — internal content should not fail on a plane.</li>
      </ul>
    </dd></div>
    <div><dt>Content marking</dt><dd>Footer <code>Confidential — Internal Only</code> on documents and email; <strong>diagonal watermark</strong> <code>CONFIDENTIAL</code> on documents so printed and screenshotted copies remain identifiable. Watermarks apply to Word, Excel and PowerPoint, not to email bodies.</dd></div>
    <div><dt>Auto-labelling</dt><dd>Client-side <em>recommended</em> (not automatic) on internal-sensitive keywords, trainable classifiers or a custom SIT — for example project codenames or “Board pack”. Recommend rather than enforce so the user stays in control of encryption.</dd></div>
    <div><dt>Policy settings</dt><dd>Published to all users. Users may apply it directly. Moving <em>down</em> from it triggers justification.</dd></div>
    <div><dt>Watch out</dt><dd>Encrypted files are still indexed and discoverable for the service, but third-party and non-Microsoft tooling (some PDF viewers, legacy apps, non-Office renderers, printing appliances) may not open them. Test the actual application estate before publishing.</dd></div>
  </dl>
</section>

<!-- ══ 4 ══ -->
<section id="s4" data-tier="confidential" data-workload="files email auto" class="card t-conf" aria-labelledby="s4-h">
  <h3 id="s4-h"><span class="num">04</span> Confidential — shared with a named partner or customer <span class="badge conf">Confidential ▸ External Sharing</span> <a class="toplink" href="#taxonomy">↑ top</a></h3>
  <p>A proposal, a design review, an audit response, a tender submission or a data pack that must reach a specific external party — and must remain unreadable to anyone else, including onward recipients.</p>
  <dl class="meta">
    <div><dt>Applies to</dt><dd>Sales, Procurement, Legal, Partner/Alliance and project teams working with a defined counterparty.</dd></div>
    <div><dt>Recommended label</dt><dd>Sublabel <strong>Confidential ▸ External Sharing</strong>. Where the external party changes per deal, use a label with <strong>user-defined permissions</strong> so the author names the recipients; where a small number of long-standing partners exist, an admin-defined label per partner is cleaner and less error-prone.</dd></div>
    <div><dt>Scope</dt><dd>Files &amp; emails.</dd></div>
    <div><dt>Encryption / rights</dt><dd>
      <ul>
        <li><strong>Encryption: On.</strong> Permissions assigned now, or user-defined in Outlook/Office at apply time.</li>
        <li>Grant to <strong>specific external domains or named external users</strong> — e.g. <code>contoso.com</code> or <code>legal@contoso.com</code> — plus the internal project group. External recipients authenticate with their Entra ID, a Microsoft account, or a one-time passcode.</li>
        <li>External rights: a custom set equivalent to <strong>Viewer / Reviewer</strong> — <em>View</em> and <em>Reply</em> only. Explicitly do <strong>not</strong> grant Print, Copy (Extract), Save As, Macros or Forward.</li>
        <li><strong>Content expiry</strong>: set an absolute date or a number of days aligned with the engagement (for example 30–90 days). After expiry the external party loses access while internal owners retain it.</li>
        <li><strong>Offline access</strong>: restrict to a small number of days (e.g. 3–7) or “never” for the highest-risk packs, so access can be genuinely revoked.</li>
      </ul>
    </dd></div>
    <div><dt>Content marking</dt><dd>Footer <code>Confidential — provided to ${Item.Label} recipients only; not for redistribution</code> and a watermark carrying the recipient organisation where the label is partner-specific.</dd></div>
    <div><dt>Auto-labelling</dt><dd>Generally none — this is a deliberate human act. Pair instead with a DLP policy that warns or blocks when unlabelled attachments go to that partner domain.</dd></div>
    <div><dt>Policy settings</dt><dd>Publish to the teams that need it rather than everyone; that alone prevents most misuse. Note that the <em>encryption boundary is independent of SharePoint sharing settings</em> — the file stays protected even if the sharing link is forwarded.</dd></div>
    <div><dt>Watch out</dt><dd>View-only encryption prevents external co-authoring. If the partner must edit, grant Co-Author and accept Copy/Print, or move the collaboration into a labelled, guest-enabled Teams site instead of exchanging protected files.</dd></div>
  </dl>
</section>

<!-- ══ 5 ══ -->
<section id="s5" data-tier="high" data-workload="files email collab" class="card t-high" aria-labelledby="s5-h">
  <h3 id="s5-h"><span class="num">05</span> Highly Confidential — restricted project / M&amp;A <span class="badge high">Highly Confidential ▸ Project</span> <a class="toplink" href="#taxonomy">↑ top</a></h3>
  <p>Deal rooms, pre-announcement M&amp;A, restructuring plans, unreleased results, litigation strategy, security incident material. A named, small, often time-boxed group; disclosure is materially or legally damaging.</p>
  <dl class="meta">
    <div><dt>Applies to</dt><dd>A named deal team only — typically Executive, Corporate Development, Legal, a handful of Finance staff and named advisers.</dd></div>
    <div><dt>Recommended label</dt><dd>Sublabel under <strong>Highly Confidential</strong>, one per project (for example <em>Highly Confidential ▸ Project Northwind</em>), retired when the project closes. Also provide <strong>Highly Confidential ▸ Do Not Forward</strong> and <strong>▸ Specific People</strong> (user-defined permissions) for ad-hoc cases.</dd></div>
    <div><dt>Scope</dt><dd>Files &amp; emails; Meetings; Groups &amp; sites (to label the deal-room site/team itself).</dd></div>
    <div><dt>Encryption / rights</dt><dd>
      <ul>
        <li><strong>Encryption: On</strong>, permissions assigned now to <strong>one dedicated mail-enabled security group</strong> whose membership is governed and reviewed (ideally access-reviewed or PIM-style time-boxed).</li>
        <li>Rights: <strong>Co-Author</strong> for the core team (they must edit together); <strong>Viewer</strong> for advisers/observers. Withhold <em>Export</em>, <em>Full Control</em> and <em>Copy</em> from everyone except the two or three owners who need <strong>Co-Owner</strong> to manage the material.</li>
        <li>Alternatively, the <strong>Do Not Forward</strong> variant: permissions are computed from the recipients of the email, with no forward, print, copy or save-as. Good for ad-hoc messages, poor for documents that need a stable audience.</li>
        <li><strong>Offline access: never</strong> (or 1 day). This forces a licence check on every open, so removing a person from the group actually removes access.</li>
        <li><strong>Content expiry</strong>: set to the expected announcement/close date where one exists.</li>
      </ul>
    </dd></div>
    <div><dt>Content marking</dt><dd>Header and footer <code>Highly Confidential — Project Northwind</code>, plus a strong diagonal watermark. Consider including the user name dynamically (<code>${User.Name}</code>) in the watermark so a leaked screenshot points at a person.</dd></div>
    <div><dt>Auto-labelling</dt><dd>Only as a safety net: service-side auto-labelling on the deal-room site so anything landing there inherits the label. Never auto-apply a restrictive sublabel tenant-wide on a keyword — a false positive locks the wrong people out of their own document.</dd></div>
    <div><dt>Policy settings</dt><dd>Publish the label policy to the deal-team group only, so the label is invisible to everyone else. Its existence in a label picker is itself a disclosure.</dd></div>
    <div><dt>Watch out</dt><dd>Members must be able to open the content from mobile and from the web. Test Outlook mobile, Office for the web and any e-signature or virtual data-room tooling in the flow before go-live. Plan the retirement: after the deal, either relabel the corpus or keep the group alive for the retention period — deleting the group orphans the content.</dd></div>
  </dl>
</section>

<!-- ══ 6 ══ -->
<section id="s6" data-tier="confidential" data-workload="files email auto" class="card t-conf" aria-labelledby="s6-h">
  <h3 id="s6-h"><span class="num">06</span> Personal data (PII), health and payment data <span class="badge conf">Confidential ▸ Internal Only</span> <a class="toplink" href="#taxonomy">↑ top</a></h3>
  <p>HR records, customer databases and extracts, payroll, health and occupational data, and anything containing payment card numbers. The distinguishing feature is that the sensitivity is <em>detectable in the content</em>, so this is the classic auto-labelling scenario.</p>
  <dl class="meta">
    <div><dt>Applies to</dt><dd>HR, Finance, Customer Operations, Support — and, because data leaks sideways into spreadsheets, effectively everyone.</dd></div>
    <div><dt>Recommended label</dt><dd><strong>Confidential ▸ Internal Only</strong> for ordinary PII; <strong>Highly Confidential ▸ Restricted Personal Data</strong> for special-category data (health, biometric) and large cardholder-data extracts.</dd></div>
    <div><dt>Scope</dt><dd>Files &amp; emails (client and service-side auto-labelling).</dd></div>
    <div><dt>Encryption / rights</dt><dd>As per the parent scenario (3 or 5). For auto-applied labels, prefer a label whose encryption grants <strong>Co-Author to all employees</strong>, so an automatic decision cannot lock a business team out of its own file.</dd></div>
    <div><dt>Content marking</dt><dd>Footer only. Avoid watermarking spreadsheets that are exported or printed as reports.</dd></div>
    <div><dt>Auto-labelling conditions</dt><dd>
      <ul>
        <li>Built-in <strong>sensitive information types</strong>: <em>Credit Card Number</em>, <em>national identifiers</em> (e.g. UK NI Number, US SSN), passport numbers, IBAN/SWIFT, health record and medical terms SITs.</li>
        <li>Prefer the packaged <strong>policy templates</strong> (GDPR, PCI-DSS, HIPAA/HITECH) as a starting point, and tune the <strong>instance count</strong> and <strong>confidence level</strong> — a common baseline is “10 or more instances at high confidence” for bulk data, with a separate low-count rule for high-risk types.</li>
        <li>Use <strong>Exact Data Match (EDM)</strong> where you can supply the real customer/employee identifier set — it collapses false positives dramatically.</li>
        <li><strong>Trainable classifiers</strong> (e.g. HR, Finance, Source Code) supplement pattern matching for content with no fixed format.</li>
        <li>Choose <em>recommended</em> labelling in the client while tuning, then promote to <em>automatic</em> once the false-positive rate is acceptable.</li>
      </ul>
    </dd></div>
    <div><dt>DLP interaction</dt><dd>
      <ul>
        <li>DLP policies can use the <strong>label itself</strong> as a condition — the cleanest pattern: detect once (auto-labelling), then enforce everywhere on the label rather than re-detecting the SITs in every policy.</li>
        <li>Typical rules: block or require justification for sending Confidential PII outside the organisation; block upload to unmanaged cloud apps via Defender for Cloud Apps; restrict copy to removable media and print via <strong>Endpoint DLP</strong>.</li>
        <li>Cardholder data: block external send outright rather than warn, and keep the policy tip text short and actionable.</li>
        <li>Remember DLP can act on content the label did not catch, and on labelled-but-unencrypted content; the two features are complementary, not alternatives.</li>
      </ul>
    </dd></div>
    <div><dt>Policy settings</dt><dd>Run every new auto-labelling policy in <strong>simulation mode</strong> first and review matches in the policy's simulation results and Activity Explorer before enabling.</dd></div>
  </dl>
</section>

<!-- ══ 7 ══ -->
<section id="s7" data-tier="confidential high" data-workload="email auto" class="card t-mixed" aria-labelledby="s7-h">
  <h3 id="s7-h"><span class="num">07</span> Email-only scenarios <span class="badge conf">Encrypt-Only</span> <span class="badge high">Do Not Forward</span> <a class="toplink" href="#taxonomy">↑ top</a></h3>
  <p>Mail is where most external disclosure happens, and where protection has to be decided in seconds. Two built-in protection templates cover most of it.</p>
  <dl class="meta">
    <div><dt>Applies to</dt><dd>All users; heavily used by HR, Legal, Finance and anyone sending to consumers.</dd></div>
    <div><dt>Recommended labels</dt><dd>
      <ul>
        <li><strong>Confidential ▸ Encrypt-Only</strong> — recipients (internal or external, any email provider) can read, reply and forward, but the message stays encrypted and cannot be stripped of protection. No expiry, no offline restriction. The right default for “this must not sit in plain text in someone's inbox”.</li>
        <li><strong>Highly Confidential ▸ Do Not Forward</strong> — recipients can read and reply but cannot forward, print, copy or extract content. Permissions are derived from the recipient list at send time.</li>
      </ul>
    </dd></div>
    <div><dt>Scope</dt><dd>Files &amp; emails, with the label configured so it can be applied to <strong>email only</strong> where the protection template is email-shaped (Do Not Forward).</dd></div>
    <div><dt>Attachments</dt><dd>A label applied to an email can be configured to <strong>apply the same protection to unlabelled Office attachments</strong>, so a plain attachment inherits the message's protection. Note that an attachment that already carries a <em>higher</em> label keeps its own protection, and that non-Office attachments (e.g. ZIP, CAD) are protected only as generic/PFILE-style content or not at all — check behaviour for your file types.</dd></div>
    <div><dt>Content marking</dt><dd>Footer text in the message body. Headers and watermarks are not applied to email.</dd></div>
    <div><dt>Auto-labelling / outbound mail</dt><dd>
      <ul>
        <li><strong>Service-side auto-labelling policies</strong> can label mail in transit in Exchange Online, including messages from mobile and non-Office clients that the label client never sees. This is the only way to guarantee outbound coverage.</li>
        <li><strong>Client-side</strong> recommendation prompts the sender in Outlook — better user experience, but bypassable.</li>
        <li>Exchange <strong>mail flow rules</strong> can still apply encryption independently; where both exist, document which wins and avoid double-protecting.</li>
      </ul>
    </dd></div>
    <div><dt>Policy settings</dt><dd>Set a default label for email, and consider different defaults for email versus documents. Where mandatory labelling is on, Outlook blocks sending until a label is chosen — pilot this carefully, it is the most visible change users experience.</dd></div>
    <div><dt>Watch out</dt><dd>Encrypted mail cannot be scanned by some third-party gateways and archiving products, and recipients on consumer providers get a portal/OTP experience. Test the actual recipient mix before turning Do Not Forward into a default.</dd></div>
  </dl>
</section>

<!-- ══ 8 ══ -->
<section id="s8" data-tier="general confidential high" data-workload="collab files" class="card t-mixed" aria-labelledby="s8-h">
  <h3 id="s8-h"><span class="num">08</span> Teams, SharePoint sites and Microsoft 365 Groups (container labelling) <a class="toplink" href="#taxonomy">↑ top</a></h3>
  <p>Container labels do <strong>not</strong> encrypt anything and do not apply to the files inside. They configure the <em>workspace</em>: who may join, whether guests are allowed, and how the content may be reached. They are the most under-used and highest-value part of the feature set.</p>
  <dl class="meta">
    <div><dt>Applies to</dt><dd>Every team, group and site owner; enforced at creation time in Teams, SharePoint and Outlook.</dd></div>
    <div><dt>Recommended labels</dt><dd>The same taxonomy, scoped to containers — e.g. <span class="badge general">General</span> (default for new teams), <span class="badge conf">Confidential ▸ Internal Only</span>, <span class="badge high">Highly Confidential ▸ Project</span>.</dd></div>
    <div><dt>Scope</dt><dd><strong>Groups &amp; sites</strong>. Requires the tenant to have sensitivity labels enabled for containers (an Entra ID / Microsoft 365 Groups setting).</dd></div>
    <div><dt>Container settings controlled by the label</dt><dd>
      <ul>
        <li><strong>Privacy</strong>: Public, Private, or “let users decide”. Confidential and above should force <em>Private</em>.</li>
        <li><strong>External users / guest access</strong>: allow or block Microsoft 365 Group owners adding guests. Block for Highly Confidential.</li>
        <li><strong>External sharing from the SharePoint site</strong>: Anyone / New and existing guests / Existing guests only / Only people in your organisation. Map directly to the tier (Public→Anyone if you genuinely publish, General→Existing guests, Confidential→Existing guests or org-only, Highly Confidential→org-only).</li>
        <li><strong>Access from unmanaged devices</strong> (Conditional Access integration): full access / web-only, limited (no download, print, sync) / block. Web-only is the sweet spot for Confidential.</li>
        <li><strong>Private channel</strong> behaviour and <strong>Authentication context</strong>, where you want a step-up MFA or compliant-device requirement on the site.</li>
        <li><strong>Default sensitivity label for the document library</strong> — new and edited files in the library are labelled automatically. This is how you get file-level protection to follow a container label.</li>
      </ul>
    </dd></div>
    <div><dt>Encryption / marking</dt><dd>Not applicable to the container itself; file-level protection comes from the library default label or from auto-labelling policies.</dd></div>
    <div><dt>Policy settings</dt><dd>Set a default container label and consider making container labelling mandatory, so no ungoverned team can be created. Changing a container label later updates the site settings but does not retroactively relabel existing files.</dd></div>
    <div><dt>Watch out</dt><dd>A container label and the file labels inside it are independent. A Highly Confidential team full of unlabelled files is still a Highly Confidential <em>container</em> with unprotected <em>content</em> once a file leaves it — use the library default label to close the gap.</dd></div>
  </dl>
</section>

<!-- ══ 9 ══ -->
<section id="s9" data-tier="confidential high" data-workload="collab email" class="card t-conf" aria-labelledby="s9-h">
  <h3 id="s9-h"><span class="num">09</span> Meetings and Teams chat <span class="badge conf">Confidential</span> <a class="toplink" href="#taxonomy">↑ top</a></h3>
  <p>Board meetings, deal calls, HR hearings and incident bridges: the meeting <em>invitation</em> is a document, and the meeting itself has a security posture that should follow the same taxonomy.</p>
  <dl class="meta">
    <div><dt>Applies to</dt><dd>Meeting organisers — especially Executive Assistants, HR and Legal.</dd></div>
    <div><dt>Recommended label</dt><dd><strong>Confidential ▸ Internal Only</strong> for routine sensitive meetings; a <strong>Highly Confidential</strong> sublabel for restricted project calls.</dd></div>
    <div><dt>Scope</dt><dd><strong>Meetings</strong> (scope must be enabled on the label, and labels for meetings require the appropriate licensing and tenant configuration).</dd></div>
    <div><dt>What the label controls</dt><dd>
      <ul>
        <li><strong>The invitation and its attachments</strong> — encrypted and marked like any email, so the agenda and pre-read are protected.</li>
        <li><strong>Meeting options enforced by the label</strong>: who can bypass the lobby; who can present; whether chat is on, off, or in-meeting-only; whether recording and transcription are permitted; whether copying chat to the clipboard is allowed; camera and microphone permissions for attendees.</li>
        <li>These options are <em>locked</em> when the label enforces them, so an organiser cannot quietly open a restricted meeting up.</li>
      </ul>
    </dd></div>
    <div><dt>Teams chat</dt><dd>Labels can be applied to Teams chats and channel conversations in supported configurations, and chat content is protected in line with the label. Treat chat as the weakest link in practice: pair labelling with a DLP policy for Teams chat and channel messages, which can block or redact messages containing sensitive information.</dd></div>
    <div><dt>Content marking</dt><dd>Footer text on the invitation body; no watermark on the meeting itself.</dd></div>
    <div><dt>Policy settings</dt><dd>Set a default meeting label (usually General) and let organisers raise it. Apply the same downgrade justification rules.</dd></div>
    <div><dt>Watch out</dt><dd>A meeting label affects the meeting and its invite — it does not retroactively protect a recording that has already been shared, nor does it substitute for Teams recording/retention governance.</dd></div>
  </dl>
</section>

<!-- ══ 10 ══ -->
<section id="s10" data-tier="general confidential high" data-workload="files email auto" class="card t-mixed" aria-labelledby="s10-h">
  <h3 id="s10-h"><span class="num">10</span> Files at rest: service-side auto-labelling vs client-side labelling <a class="toplink" href="#taxonomy">↑ top</a></h3>
  <p>Most tenants have years of unlabelled content already in SharePoint and OneDrive. Getting a label onto that backlog is a different mechanism from labelling new work.</p>
  <dl class="meta">
    <div><dt>Applies to</dt><dd>The whole existing estate: SharePoint document libraries, OneDrive accounts, and mail in transit.</dd></div>
    <div><dt>Service-side auto-labelling policy</dt><dd>
      <ul>
        <li>Runs in the service, with no user and no client involved; applies to files at rest in SharePoint/OneDrive and to email in transit in Exchange.</li>
        <li>Labels content regardless of who created it, what device it came from, or whether the labelling client is installed — the only way to reach the backlog and non-Office clients.</li>
        <li>Always start in <strong>simulation mode</strong>; review the matched items, tune the SIT confidence and instance counts, then turn the policy on.</li>
        <li>Cannot be overridden by the user at the moment of application, so choose a label whose encryption is inclusive (see scenario 6). Applies to supported Office file types; it will not label every binary format.</li>
        <li>Respects existing labels: by default it will not overwrite a manually applied label, and should never be allowed to downgrade one.</li>
      </ul>
    </dd></div>
    <div><dt>Client-side auto / recommended labelling</dt><dd>
      <ul>
        <li>Runs in the Office apps (built-in labelling in Word/Excel/PowerPoint/Outlook) as the user types or saves.</li>
        <li><strong>Automatic</strong>: applies the label silently with a notification; <strong>Recommended</strong>: prompts the user with a policy tip they may accept or dismiss with a reason.</li>
        <li>Immediate feedback and teaching value, but only covers users with the client, only on supported apps, and is bypassable.</li>
      </ul>
    </dd></div>
    <div><dt>Default label for a document library</dt><dd>A third mechanism: set on the SharePoint library (directly or via a container label), so new and updated files inherit a label without any detection at all. The lowest-effort way to protect a known-sensitive site.</dd></div>
    <div><dt>Recommended combination</dt><dd>Library default labels for known-sensitive sites &nbsp;+&nbsp; service-side auto-labelling for detectable content everywhere &nbsp;+&nbsp; client-side <em>recommended</em> labelling to train users &nbsp;+&nbsp; a default label and mandatory labelling to catch the rest.</dd></div>
    <div><dt>Watch out</dt><dd>Re-labelling millions of files changes modified metadata and can generate very large volumes of audit and sync activity. Roll out site by site, and communicate before the encryption-bearing labels start landing.</dd></div>
  </dl>
</section>

<!-- ══ 11 ══ -->
<section id="s11" data-tier="confidential high" data-workload="files collab" class="card t-mixed" aria-labelledby="s11-h">
  <h3 id="s11-h"><span class="num">11</span> Guest and external collaboration — labels persist with the file <a class="toplink" href="#taxonomy">↑ top</a></h3>
  <p>The core principle: the label and its encryption are written <em>into the file itself</em>, not into SharePoint. Protection therefore survives download, email, USB copy, upload to a third-party cloud, and rename.</p>
  <dl class="meta">
    <div><dt>Applies to</dt><dd>Any scenario where content crosses the tenant boundary: guests in Teams, partner extranets, supplier portals, personal devices.</dd></div>
    <div><dt>What persists</dt><dd>
      <ul>
        <li>The label metadata and, where encryption is configured, the protection itself travel with the file. An unencrypted labelled file keeps its label and markings, but nothing stops it being read — labelling alone is classification, not control.</li>
        <li>Because the encryption policy is enforced at open time by the rights service, <strong>access can be revoked after the fact</strong> by removing the user from the group or revoking the document — subject to the offline-access period you configured.</li>
        <li>Usage of protected documents is <strong>tracked and auditable</strong>, including unauthorised access attempts, which is how you detect misdirected sharing.</li>
      </ul>
    </dd></div>
    <div><dt>Guest access design</dt><dd>
      <ul>
        <li>Guests must be able to authenticate: Entra ID guest account, Microsoft account, or one-time passcode. Confirm which applies to each partner before relying on it.</li>
        <li>Guests <strong>cannot see your label picker</strong> — labels are published by policy to your users only. A guest editing in a labelled library will have the library default applied, but cannot choose labels.</li>
        <li>Use container labels (scenario 8) to control whether guests can be added at all, and unmanaged-device policy to keep external editing in the browser.</li>
        <li>For partners on the recipient side, prefer a <em>domain</em>-based grant over a list of individuals — people change roles; domains don't.</li>
      </ul>
    </dd></div>
    <div><dt>Watch out</dt><dd>Persistence is not a licence to ignore governance. Encrypted content that outlives its owning group becomes unopenable; encrypted content indexed before protection may behave differently in search and eDiscovery; and a super-user/admin recovery path must exist and be tested <em>before</em> you need it.</dd></div>
  </dl>
</section>

<!-- ══ 12 ══ -->
<section id="s12" data-tier="public general confidential high" data-workload="files email collab auto" class="card t-general" aria-labelledby="s12-h">
  <h3 id="s12-h"><span class="num">12</span> Policy settings: downgrade justification, mandatory labelling, defaults <a class="toplink" href="#taxonomy">↑ top</a></h3>
  <p>These four settings live on the <em>label policy</em>, not the label, and they determine how the taxonomy actually behaves for users. They can differ per published policy, so a pilot group can run stricter settings than the tenant.</p>
  <dl class="meta">
    <div><dt>Default label</dt><dd>Applies a label to new documents, emails, meetings and containers when the user chooses nothing. Set separately for documents, email, meetings and containers. Use <span class="badge general">General</span> — never a label that encrypts.</dd></div>
    <div><dt>Mandatory labelling</dt><dd>“Users must apply a label to their email or documents.” Office prompts before save or send and will not proceed without a choice. Powerful for coverage, and the single biggest source of user friction — only enable after training, and after a default label is set so the path of least resistance is a correct one.</dd></div>
    <div><dt>Justification for downgrade</dt><dd>“Users must provide justification to remove a label or lower its classification.” Prompts for a reason, records it in the audit log, and surfaces it in <strong>Activity Explorer</strong>. It is a detective control, not a preventive one — it does not block the downgrade.</dd></div>
    <div><dt>Custom help link</dt><dd>Point “Learn more about sensitivity labels” at your own one-page internal guidance. Users will click it at the exact moment they are confused; this is the highest-value 30 minutes in the whole deployment.</dd></div>
    <div><dt>Other settings worth setting deliberately</dt><dd>
      <ul>
        <li>Label <strong>priority/order</strong> defines what counts as a downgrade and which label wins when content is combined.</li>
        <li>Per-policy <strong>scoping</strong>: publish restrictive labels to the groups that need them; publish the base taxonomy to everyone.</li>
        <li><strong>Label inheritance</strong> from attachments to email, so a message carrying a Confidential attachment is itself labelled.</li>
        <li>Multiple policies apply in order; where a user is in scope for several, the higher-priority policy's settings win — keep the number of policies small and document the order.</li>
      </ul>
    </dd></div>
  </dl>
</section>

<!-- ══ SUMMARY ══ -->
<section id="summary" aria-labelledby="summary-h">
  <h2 id="summary-h">Summary — the complete label set</h2>
  <p class="scroll-hint">Scroll the table sideways to see every column. <span aria-hidden="true">→</span></p>
  <div class="table-scroll" role="region" tabindex="0" aria-label="Summary of the complete label set — scrollable table">
    <table>
      <caption>An illustrative taxonomy. Encryption rights shown are Azure Rights Management usage rights; “All employees” means a single governed mail-enabled security group.</caption>
      <thead>
        <tr>
          <th scope="col">Label</th><th scope="col">Sublabel</th><th scope="col">Encryption</th>
          <th scope="col">Who can access</th><th scope="col">Marking</th>
          <th scope="col">Auto-label triggers</th><th scope="col">Typical use</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <td data-label="Label"><span class="badge public">Public</span></td><td data-label="Sublabel">—</td><td data-label="Encryption">None</td>
          <td data-label="Who can access">Anyone</td><td data-label="Marking">Optional footer</td><td data-label="Auto-label triggers">None (never auto-applied)</td>
          <td data-label="Typical use">Press releases, website copy, brochures</td>
        </tr>
        <tr>
          <td data-label="Label"><span class="badge general">General</span></td><td data-label="Sublabel">—</td><td data-label="Encryption">None</td>
          <td data-label="Who can access">Anyone the file is shared with</td><td data-label="Marking">Footer: <em>General — internal use</em></td>
          <td data-label="Auto-label triggers">Default label (not detection-based)</td>
          <td data-label="Typical use">Everyday internal work, most email</td>
        </tr>
        <tr>
          <td data-label="Label"><span class="badge conf">Confidential</span></td><td data-label="Sublabel">Internal Only</td>
          <td data-label="Encryption">Yes — admin-defined</td><td data-label="Who can access">All-employees group · Co-Author</td>
          <td data-label="Marking">Footer + <em>CONFIDENTIAL</em> watermark</td>
          <td data-label="Auto-label triggers">Recommended on internal keywords / trainable classifiers; auto on PII SITs</td>
          <td data-label="Typical use">Financials, pricing, HR &amp; customer PII, architecture docs</td>
        </tr>
        <tr>
          <td data-label="Label"><span class="badge conf">Confidential</span></td><td data-label="Sublabel">External Sharing</td>
          <td data-label="Encryption">Yes — named domains/users, expiry, limited offline</td>
          <td data-label="Who can access">Internal team + named partner · View/Reply only (no print, copy, forward)</td>
          <td data-label="Marking">Footer + recipient watermark</td><td data-label="Auto-label triggers">None — deliberate manual act</td>
          <td data-label="Typical use">Proposals, tenders, audit responses, partner data packs</td>
        </tr>
        <tr>
          <td data-label="Label"><span class="badge conf">Confidential</span></td><td data-label="Sublabel">Encrypt-Only (email)</td>
          <td data-label="Encryption">Yes — Encrypt-Only template</td>
          <td data-label="Who can access">The recipients; may reply and forward, protection persists</td>
          <td data-label="Marking">Footer in message body</td>
          <td data-label="Auto-label triggers">Service-side auto-labelling of outbound mail containing SITs</td>
          <td data-label="Typical use">Sensitive mail to any provider, incl. consumers</td>
        </tr>
        <tr>
          <td data-label="Label"><span class="badge high">Highly Confidential</span></td><td data-label="Sublabel">Project &lt;name&gt;</td>
          <td data-label="Encryption">Yes — dedicated group, offline access never, expiry at close</td>
          <td data-label="Who can access">Named deal team · Co-Author; advisers · Viewer</td>
          <td data-label="Marking">Header + footer + user-name watermark</td>
          <td data-label="Auto-label triggers">Service-side auto-label on the deal-room site only</td>
          <td data-label="Typical use">M&amp;A, restructuring, litigation, unreleased results</td>
        </tr>
        <tr>
          <td data-label="Label"><span class="badge high">Highly Confidential</span></td><td data-label="Sublabel">Do Not Forward (email)</td>
          <td data-label="Encryption">Yes — Do Not Forward template</td>
          <td data-label="Who can access">Recipients only · read &amp; reply; no forward, print, copy</td>
          <td data-label="Marking">Footer in message body</td><td data-label="Auto-label triggers">None</td>
          <td data-label="Typical use">Ad-hoc restricted mail, HR and legal correspondence</td>
        </tr>
        <tr>
          <td data-label="Label"><span class="badge high">Highly Confidential</span></td><td data-label="Sublabel">Specific People</td>
          <td data-label="Encryption">Yes — user-defined permissions</td>
          <td data-label="Who can access">Whoever the author names, at apply time</td>
          <td data-label="Marking">Footer + watermark</td><td data-label="Auto-label triggers">None</td>
          <td data-label="Typical use">One-off documents with an unpredictable audience</td>
        </tr>
        <tr>
          <td data-label="Label"><span class="badge high">Highly Confidential</span></td><td data-label="Sublabel">Restricted Personal Data</td>
          <td data-label="Encryption">Yes — restricted group, limited offline</td>
          <td data-label="Who can access">HR/Compliance group · Co-Author</td>
          <td data-label="Marking">Footer + watermark</td>
          <td data-label="Auto-label triggers">Health, biometric and bulk cardholder SITs; EDM where available</td>
          <td data-label="Typical use">Special-category personal data, large PII extracts</td>
        </tr>
        <tr>
          <td data-label="Label"><span class="badge neutral">Container labels</span></td><td data-label="Sublabel">Reuse the same names</td>
          <td data-label="Encryption">No (containers are never encrypted)</td>
          <td data-label="Who can access">Controls privacy, guests, external sharing, unmanaged-device access</td>
          <td data-label="Marking">n/a</td>
          <td data-label="Auto-label triggers">Default label for the document library</td>
          <td data-label="Typical use">Teams, Microsoft 365 Groups, SharePoint sites</td>
        </tr>
      </tbody>
    </table>
  </div>
</section>

<!-- ══ PITFALLS ══ -->
<section id="pitfalls" aria-labelledby="pitfalls-h">
  <h2 id="pitfalls-h">Common pitfalls</h2>
  <ul class="pitfalls">
    <li><strong>Too many labels.</strong> Every label and sublabel is a decision a user must make correctly under time pressure. Beyond roughly five top-level labels the accuracy of user classification collapses and the taxonomy becomes decorative. Start smaller than feels right; adding a label later is easy, removing one is not.</li>
    <li><strong>Over-encrypting.</strong> Encryption is the only label behaviour that can actually stop someone working. Applying it to the default label, or to a broadly auto-applied label, generates support volume, breaks conversion, print, third-party and line-of-business tooling, and trains users to avoid labels entirely. Encrypt where the harm justifies the friction — nowhere else.</li>
    <li><strong>Breaking co-authoring, search and eDiscovery.</strong> Restrictive usage rights (view-only, no copy, no export) prevent simultaneous editing and AutoSave. Labels applied by desktop clients versus the service behave differently for older content, and encrypted content held outside the service, or whose protecting group has been deleted, can be effectively unrecoverable. Confirm co-authoring, search, eDiscovery export and your super-user recovery path for <em>every</em> encrypting label before you publish it.</li>
    <li><strong>Labelling before piloting.</strong> Publishing labels tenant-wide, or running an auto-labelling policy outside simulation mode, converts a configuration mistake into thousands of wrongly protected files. Pilot with a representative group across the real device and application mix — including mobile, Mac, web and any non-Microsoft viewer — and run every auto-labelling policy in simulation until its match quality is proven.</li>
    <li><strong>Mandatory labelling without training.</strong> Turning on “users must apply a label” overnight guarantees that the fastest label wins, which is usually the least restrictive one. Publish the guidance, set a sensible default, add a custom help link, brief the service desk, then enable mandatory labelling — in that order.</li>
    <li><strong>Labels that describe controls, not the business.</strong> Names like “Internal AIP Protected T2” mean nothing to a user in a hurry. Name labels for the audience and the intent, and explain the rest in the tooltip.</li>
    <li><strong>No owner and no review cycle.</strong> Project sublabels accumulate, groups drift, partner domains change. Give the taxonomy an owner, review it on a fixed cadence, and have a documented retirement process for project labels.</li>
  </ul>
</section>

<section id="validate" aria-labelledby="validate-h">
  <h2 id="validate-h">Before you deploy</h2>
  <div class="callout warn">
    <h3>Validate against current documentation and your licensing</h3>
    <p>Microsoft Purview evolves continuously: setting names, scopes, supported clients, and which capabilities are available in which application move between releases. <strong>Every specific setting in this document must be confirmed against the current Microsoft Purview documentation and tested in your own tenant before deployment.</strong></p>
    <p>Licensing is the other gate. Manual labelling and basic protection are broadly available with Microsoft 365 E3/Office 365 E3 and equivalent plans, while the advanced capabilities used in several of the scenarios above — automatic labelling (client and service-side), auto-labelling policies for SharePoint/OneDrive and Exchange, advanced classifiers such as Exact Data Match and trainable classifiers, Endpoint DLP, and labels for meetings — generally require <strong>Microsoft 365 E5</strong>, the <strong>E5 Compliance / Information Protection and Governance</strong> add-on, or the equivalent standalone Purview SKU. Confirm entitlement per user, not per tenant: a capability licensed for the pilot group may not be licensed for everyone you later bring into scope.</p>
    <p style="margin-block-end:0;">This document is a design reference, not a configuration export. Treat the values in it as a starting point for a design workshop.</p>
  </div>
</section>


<!-- ══ QUIZ ══ -->
<section id="quiz" aria-labelledby="quiz-h">
  <h2 id="quiz-h">Self-check</h2>
  <p>Five questions drawn from the scenarios above. Pick an answer to see whether it holds up and why the alternatives do not. <span class="nojs-only">(Answers and explanations are shown below each question.)</span></p>

  <div class="quiz-q" data-correct="b">
    <fieldset>
      <legend>1. A pricing model must be co-authored for six months with the project team at a named contract partner. They need to read and edit it; they must not print it, copy from it or save it out to another format. Which configuration fits?</legend>
      <label class="opt" data-opt="a"><input type="radio" name="q1" value="a"><span><strong>A.</strong> <span class="badge conf">Confidential ▸ Internal Only</span> — the partner is working on our project, so the internal label is close enough.</span></label>
      <label class="opt" data-opt="b"><input type="radio" name="q1" value="b"><span><strong>B.</strong> <span class="badge conf">Confidential ▸ External Sharing</span> with encryption on and permissions assigned now: the all-employees group as <em>Co-Author</em>, and the partner's domain (or a guest security group) with custom rights limited to view, edit and reply — no print, no copy, no save-as/export.</span></label>
      <label class="opt" data-opt="c"><input type="radio" name="q1" value="c"><span><strong>C.</strong> <span class="badge high">Highly Confidential ▸ Do Not Forward</span> applied to the document.</span></label>
      <label class="opt" data-opt="d"><input type="radio" name="q1" value="d"><span><strong>D.</strong> No label; share the SharePoint file with the partner using a guest link with edit permission.</span></label>
    </fieldset>
    <div class="quiz-answer">
      <p class="verdict">Correct answer: B.</p>
      <p><strong>Why B is right.</strong> Permissions assigned <em>now</em> by the administrator give a predictable, auditable grant that travels inside the file, so the restriction survives download, forwarding and re-upload. A domain-based grant is the right shape for a partner: individuals change roles, domains do not. Custom usage rights let you keep edit (and therefore co-authoring) while withholding print, copy and export.</p>
      <p><strong>Why the others are wrong.</strong> <strong>A</strong> — the Internal Only grant is scoped to the all-employees group, so the partner simply cannot open the file. <strong>C</strong> — Do Not Forward is an email-shaped protection template whose permissions are derived from the recipient list at send time; it is not a way to configure standing access to a co-authored document, and it removes edit rights. <strong>D</strong> — a sharing link is a SharePoint permission, not protection: once the file is downloaded there is nothing left to stop printing, copying or onward sharing.</p>
    </div>
  </div>

  <div class="quiz-q" data-correct="c">
    <fieldset>
      <legend>2. Ten years of documents already sit in SharePoint and OneDrive. You need the ones containing payment card data to end up labelled, including files created by people who no longer work here. What actually achieves that?</legend>
      <label class="opt" data-opt="a"><input type="radio" name="q2" value="a"><span><strong>A.</strong> Turn on client-side automatic labelling in the Office apps for the credit-card sensitive information type.</span></label>
      <label class="opt" data-opt="b"><input type="radio" name="q2" value="b"><span><strong>B.</strong> Turn on mandatory labelling in the label policy so users must label their content.</span></label>
      <label class="opt" data-opt="c"><input type="radio" name="q2" value="c"><span><strong>C.</strong> A service-side auto-labelling policy for SharePoint and OneDrive, run in simulation mode first, applying a label whose encryption grant is inclusive enough for everyone who legitimately needs the content.</span></label>
      <label class="opt" data-opt="d"><input type="radio" name="q2" value="d"><span><strong>D.</strong> A DLP policy that blocks sharing of files containing credit card numbers.</span></label>
    </fieldset>
    <div class="quiz-answer">
      <p class="verdict">Correct answer: C.</p>
      <p><strong>Why C is right.</strong> Service-side auto-labelling runs in the service with no user and no client involved, so it reaches files at rest regardless of who created them or whether anyone ever opens them again. Simulation mode lets you review the matches and tune confidence levels and instance counts before anything is written. Because the user is not present to override it, the label it applies must not lock people out — that is why the encryption grant has to be inclusive (or absent).</p>
      <p><strong>Why the others are wrong.</strong> <strong>A</strong> — client-side labelling only acts on content a licensed user opens or saves in a supported Office app; a dormant file in a departed user's OneDrive is never touched. <strong>B</strong> — mandatory labelling prompts users going forward; it does nothing to a ten-year backlog. <strong>D</strong> — DLP controls the movement of the content, but never applies a label; classification and protection are not the same control.</p>
    </div>
  </div>

  <div class="quiz-q" data-correct="a">
    <fieldset>
      <legend>3. Your design proposes making the default label for documents and email one that encrypts to the all-employees group, on the grounds that “everything internal should be protected by default”. What is the right response?</legend>
      <label class="opt" data-opt="a"><input type="radio" name="q3" value="a"><span><strong>A.</strong> Reject it. Make <span class="badge general">General</span> — marking only, no encryption — the default, and let users raise the label when they need protection.</span></label>
      <label class="opt" data-opt="b"><input type="radio" name="q3" value="b"><span><strong>B.</strong> Accept it, but only after turning on downgrade justification so users cannot remove the protection.</span></label>
      <label class="opt" data-opt="c"><input type="radio" name="q3" value="c"><span><strong>C.</strong> Accept it — encryption is transparent to users once the label client is deployed.</span></label>
      <label class="opt" data-opt="d"><input type="radio" name="q3" value="d"><span><strong>D.</strong> Accept it for email only, where nothing can break.</span></label>
    </fieldset>
    <div class="quiz-answer">
      <p class="verdict">Correct answer: A.</p>
      <p><strong>Why A is right.</strong> The default label lands on everything nobody thought about, which is most content. Encryption is the one label behaviour that can stop a person working, so applying it by default guarantees breakage across PDF and print pipelines, third-party and line-of-business tooling, and ordinary external sharing of harmless material — and it trains users to avoid labels. General as the default gives you coverage and marking with no blast radius, and users can still move up without friction.</p>
      <p><strong>Why the others are wrong.</strong> <strong>B</strong> — justification is a detective control that records a reason in the audit log; it does not prevent the downgrade, and it does not make the default safe. <strong>C</strong> — encryption is transparent only inside supported Microsoft clients for users inside the grant; it is highly visible to everyone else. <strong>D</strong> — email is where it hurts most: encrypted mail can be unreadable to some third-party gateways and archiving products, and external recipients drop into a portal or one-time-passcode experience.</p>
    </div>
  </div>

  <div class="quiz-q" data-correct="d">
    <fieldset>
      <legend>4. A restricted acquisition project gets its own private team. The team is labelled <span class="badge high">Highly Confidential ▸ Project</span>. What has that container label actually done?</legend>
      <label class="opt" data-opt="a"><input type="radio" name="q4" value="a"><span><strong>A.</strong> Encrypted every file in the team's document library to the project group.</span></label>
      <label class="opt" data-opt="b"><input type="radio" name="q4" value="b"><span><strong>B.</strong> Applied the label to the files inside, so anything downloaded from the team stays protected.</span></label>
      <label class="opt" data-opt="c"><input type="radio" name="q4" value="c"><span><strong>C.</strong> Nothing enforceable — container labels are only a visual marker for the team.</span></label>
      <label class="opt" data-opt="d"><input type="radio" name="q4" value="d"><span><strong>D.</strong> Configured the workspace: privacy forced to Private, guest access blocked, external sharing restricted to the organisation only, and access from unmanaged devices limited or blocked. The files inside are untouched unless you also set a default label on the library.</span></label>
    </fieldset>
    <div class="quiz-answer">
      <p class="verdict">Correct answer: D.</p>
      <p><strong>Why D is right.</strong> A container label governs the container — privacy, guest access, external sharing scope, unmanaged-device access through Conditional Access integration, and optionally an authentication context. It never encrypts, and it does not reach inside to the content. The bridge to file-level protection is the default sensitivity label on the document library, which labels new and edited files there.</p>
      <p><strong>Why the others are wrong.</strong> <strong>A</strong> and <strong>B</strong> — both assume the container label flows down to files; it does not, which is exactly the gap that leaves a Highly Confidential team full of unprotected documents. <strong>C</strong> — the opposite error: the settings a container label applies are enforced at creation and on the site, not cosmetic.</p>
    </div>
  </div>

  <div class="quiz-q" data-correct="b">
    <fieldset>
      <legend>5. HR must email an individual outcome letter to an employee's personal Gmail address. The recipient must be able to read and reply, but must not be able to forward, print or copy it. Which label?</legend>
      <label class="opt" data-opt="a"><input type="radio" name="q5" value="a"><span><strong>A.</strong> <span class="badge conf">Confidential ▸ Encrypt-Only</span>.</span></label>
      <label class="opt" data-opt="b"><input type="radio" name="q5" value="b"><span><strong>B.</strong> <span class="badge high">Highly Confidential ▸ Do Not Forward</span>.</span></label>
      <label class="opt" data-opt="c"><input type="radio" name="q5" value="c"><span><strong>C.</strong> <span class="badge general">General</span>, with the letter sent as a password-protected archive.</span></label>
      <label class="opt" data-opt="d"><input type="radio" name="q5" value="d"><span><strong>D.</strong> <span class="badge conf">Confidential ▸ Internal Only</span> — the strongest internal label available.</span></label>
    </fieldset>
    <div class="quiz-answer">
      <p class="verdict">Correct answer: B.</p>
      <p><strong>Why B is right.</strong> Do Not Forward derives permissions from the recipient list at send time and allows read and reply while removing forward, print, copy and extract. It works to any email address, including consumer providers, where the recipient authenticates with a Microsoft account or a one-time passcode.</p>
      <p><strong>Why the others are wrong.</strong> <strong>A</strong> — Encrypt-Only keeps the message encrypted but deliberately permits forwarding, which is the one thing this scenario forbids. <strong>C</strong> — a password-protected archive is unauditable, unrevocable, and the password almost always travels in the next message; it also defeats mail hygiene scanning. <strong>D</strong> — Internal Only grants rights to the all-employees group, so the external recipient cannot open it at all. Do check the recipient experience before making Do Not Forward routine: encrypted mail is opaque to some third-party gateways and archiving products.</p>
    </div>
  </div>
</section>

<!-- ══ YOUR TURN ══ -->
<section id="yourturn" aria-labelledby="yourturn-h">
  <h2 id="yourturn-h">Your turn — design a label for a new scenario</h2>

  <div class="intro">
    <h3 style="margin-block-start:0;">The scenario: engineering drawings released to a contract manufacturer</h3>
    <p><strong>Who.</strong> You are designing for <em>Halden Aerostructures</em>, a UK-based tier-two aerospace supplier of about 4,000 people. The requesting business is the Engineering Release function; the stakeholders are the Chief Engineer, Export Control, Legal, and the IT security team that runs Purview.</p>
    <p><strong>What content.</strong> A released drawing pack for a new actuator housing: 2D drawings and manufacturing data exported to PDF, a specification and tolerance document in Word, a costed bill of materials in Excel, and the native CAD model files (STEP and a vendor CAD format). The pack is the company's core intellectual property and took four years to develop.</p>
    <p><strong>Who needs access.</strong> Roughly 25 named engineers and quality staff at <em>Meridian Precision</em>, a contract manufacturer in another country, who must read the pack, mark it up and return supplier deviation requests for the 18-month build programme. Internally, the Engineering, Quality and Programme teams author and edit it. Meridian's own sub-tier suppliers must <em>not</em> receive it, and the pack must not reach Halden's other customers or competitors.</p>
    <p><strong>Constraints.</strong> Meridian's engineers are already Entra ID guests in a shared Halden project team. They work on managed laptops but also use shop-floor terminals that Halden does not manage. The programme ends on a known date, after which access must stop. Printing to the shop floor is a genuine business need — but only at Meridian, and only with the copy traceable. The CAD files are not Office formats. An internal design-review workshop has asked you to produce a label design that Export Control can sign off.</p>
    <p style="margin-block-end:0;"><strong>Regulatory driver.</strong> Parts of the pack are export-controlled technical data (UK export control / ITAR-equivalent regimes). Unauthorised release to a foreign national or a third country is a criminal offence, not just a commercial loss, and the company must be able to evidence <em>who accessed what</em> to its export control auditors.</p>
  </div>

  <h3>Your design</h3>
  <p class="hint">Fill this in before you look at the model answer. Answers stay in this page only — nothing is sent anywhere and nothing is stored when you close the tab. Use <strong>Print / save my answers</strong> to keep a copy.</p>

  <form class="worksheet" id="ws-form" novalidate>
    <div class="ws-grid">
      <div class="ws-field">
        <label for="ws-label">Label and sublabel name</label>
        <input type="text" id="ws-label" name="ws-label" placeholder="e.g. Highly Confidential ▸ …">
        <p class="help">Business language, not control language.</p>
      </div>
      <div class="ws-field">
        <label for="ws-tooltip">Label tooltip / description</label>
        <input type="text" id="ws-tooltip" name="ws-tooltip" placeholder="What the user sees when hovering the label">
        <p class="help">One sentence the author can act on.</p>
      </div>
    </div>

    <div class="ws-field">
      <label id="ws-scope-l" for="ws-scope-files">Scope</label>
      <div class="checkline" role="group" aria-labelledby="ws-scope-l">
        <label><input type="checkbox" id="ws-scope-files" name="ws-scope" value="Files &amp; emails"> Files &amp; emails</label>
        <label><input type="checkbox" name="ws-scope" value="Meetings"> Meetings</label>
        <label><input type="checkbox" name="ws-scope" value="Groups &amp; sites"> Groups &amp; sites</label>
        <label><input type="checkbox" name="ws-scope" value="Schematised data assets"> Schematised data assets</label>
      </div>
    </div>

    <div class="ws-grid">
      <div class="ws-field">
        <label for="ws-encrypt">Encryption</label>
        <select id="ws-encrypt" name="ws-encrypt">
          <option value="">— choose —</option>
          <option>No encryption — marking and classification only</option>
          <option>Encrypt, permissions assigned now by the administrator</option>
          <option>Encrypt, permissions chosen by the user (user-defined)</option>
          <option>Encrypt, Do Not Forward (email)</option>
          <option>Encrypt, Encrypt-Only (email)</option>
        </select>
      </div>
      <div class="ws-field">
        <label for="ws-expiry">Content expiry &amp; offline access</label>
        <input type="text" id="ws-expiry" name="ws-expiry" placeholder="e.g. access expires on …; offline access … days">
      </div>
    </div>

    <div class="ws-field">
      <label for="ws-who">Who gets access (the grant)</label>
      <textarea id="ws-who" name="ws-who" placeholder="Name the groups, domains or individuals — and say which mechanism you would use for each."></textarea>
    </div>

    <div class="ws-field">
      <label for="ws-rights">Usage rights per audience</label>
      <textarea id="ws-rights" name="ws-rights" placeholder="e.g. internal authors: Co-Author. Partner: view / edit / print? copy? export? reply?"></textarea>
    </div>

    <div class="ws-grid">
      <div class="ws-field">
        <label for="ws-marking">Content marking</label>
        <textarea id="ws-marking" name="ws-marking" placeholder="Header / footer / watermark, and the exact text — including any dynamic variables."></textarea>
      </div>
      <div class="ws-field">
        <label for="ws-auto">Auto-labelling conditions</label>
        <textarea id="ws-auto" name="ws-auto" placeholder="Service-side, client-side automatic, client-side recommended, library default — and on what condition?"></textarea>
      </div>
    </div>

    <div class="ws-grid">
      <div class="ws-field">
        <label for="ws-policy">Policy settings</label>
        <textarea id="ws-policy" name="ws-policy" placeholder="Who is it published to? Default label? Mandatory labelling? Downgrade justification? Priority/order?"></textarea>
      </div>
      <div class="ws-field">
        <label for="ws-controls">DLP and container controls</label>
        <textarea id="ws-controls" name="ws-controls" placeholder="Container label settings, guest access, unmanaged-device access, DLP rules, endpoint DLP."></textarea>
      </div>
    </div>

    <div class="ws-field">
      <label for="ws-risk">The one thing most likely to go wrong with your design</label>
      <textarea id="ws-risk" name="ws-risk" placeholder="Name it, and say how you would test for it before publishing."></textarea>
    </div>

    <div class="toolbar-row js-only">
      <button type="button" class="btn primary" id="print-answers">Print / save my answers</button>
      <button type="button" class="btn" id="clear-answers">Clear the worksheet</button>
    </div>
  </form>

  <div class="config-controls js-only">
    <button type="button" class="btn" id="model-toggle" aria-expanded="false" aria-controls="model-answer">Reveal a model answer</button>
  </div>

  <div class="model-answer" id="model-answer">
    <h3 style="margin-block-start:0;">A model answer — and why</h3>
    <p>There is no single right design, but a defensible one for this scenario looks like the following. What matters is the reasoning, not matching it word for word.</p>

    <h4>Label: <span class="badge high">Highly Confidential ▸ Export Controlled — Manufacturing Partner</span></h4>
    <p>A sublabel of the existing Highly Confidential parent rather than a new top-level tier: the harm is severe, the taxonomy already expresses that, and adding a fifth top-level label to serve one programme is how taxonomies rot. Naming it for the audience and the driver (export controlled, manufacturing partner) tells the author when to use it without teaching them anything about rights management. Tooltip: <em>“Released engineering data shared with an approved manufacturing partner. Export-controlled — do not share outside the named partner.”</em></p>

    <h4>Scope: Files &amp; emails, plus a matching container label</h4>
    <p>Files &amp; emails, so the drawing pack and the covering mail both carry it. The project team itself gets a container label (see below) — the same taxonomy, different mechanism.</p>

    <h4>Encryption: on, permissions assigned now, two audiences</h4>
    <ul>
      <li><strong>Internal:</strong> a dedicated mail-enabled security group — <code>SG-Actuator-Programme-Internal</code>, not all employees — with <strong>Co-Author</strong>, so co-authoring and AutoSave keep working for the people who write the pack.</li>
      <li><strong>Partner:</strong> a guest security group holding the 25 named Meridian accounts, with <strong>custom rights: view, edit, print, reply</strong> — and explicitly <em>not</em> copy, <em>not</em> save-as/export, <em>not</em> full control. Print is granted because the shop floor genuinely needs it and the watermark makes the printed copy traceable; copy and export are withheld because they are how content leaves the protected envelope silently.</li>
    </ul>
    <p>Use a <em>named group</em> rather than the partner's whole domain here — the opposite of the usual partner advice. A domain grant would reach every employee at Meridian including their sub-tier liaison staff, and export control requires the grant to match an approved person list. Accept the administrative cost of maintaining the group; it is the control the auditor will ask to see.</p>

    <h4>Expiry and offline access</h4>
    <p>Set content expiry to the programme end date, and shorten offline access to something like one day. Expiry gives you a hard stop that survives the file having been downloaded; short offline access means revoking a leaver at Meridian takes effect in hours rather than weeks. Both are a deliberate trade against usability on the shop floor, and both should be agreed with the Chief Engineer before publishing.</p>

    <h4>Content marking</h4>
    <p>Footer on documents and email: <code>Export controlled — Halden Aerostructures / Meridian Precision programme use only</code>. Diagonal watermark on documents: <code>EXPORT CONTROLLED</code> combined with a dynamic variable such as <code>${User.Name}</code> so a printed or photographed page identifies the person who opened it. Watermarks apply to Word, Excel and PowerPoint only — they are not applied to email bodies and not to PDFs produced outside Office, which is one reason the PDF export path needs its own check.</p>

    <h4>Auto-labelling</h4>
    <p>Do not try to detect this with content conditions — drawings and CAD have little reliable text to classify on, and a false positive here encrypts something to a group that excludes its own author. Instead:</p>
    <ul>
      <li><strong>Default sensitivity label on the document library</strong> in the engineering release site, so everything filed there is labelled on arrival without any detection at all. This is the primary mechanism.</li>
      <li><strong>Client-side recommended</strong> labelling on a custom sensitive information type matching the programme's part-number pattern and the export control marking string, so authors working outside the release library get prompted rather than silently overridden.</li>
      <li>A <strong>service-side auto-labelling policy in simulation mode</strong> over the engineering sites, used as a discovery tool: it tells you where else the drawing data already lives before you decide whether to enforce anything.</li>
    </ul>

    <h4>Policy settings</h4>
    <p>Publish the sublabel to a scoped policy containing only the programme's internal group — a restrictive label visible to 4,000 people will be applied by someone who should not have applied it. Never a default label. Keep mandatory labelling and downgrade justification as they are set tenant-wide; place the sublabel above the general Highly Confidential sublabels in priority order so that combining content resolves upward. Point the custom help link at the export control team's internal page.</p>

    <h4>DLP and container controls</h4>
    <ul>
      <li><strong>Container label</strong> on the project team: privacy forced to Private, guest access allowed (the partner needs it) but external sharing from the SharePoint site restricted to <em>existing guests only</em>, and access from unmanaged devices limited to <em>web-only, no download, print or sync</em> — which is exactly how you let the shop-floor terminals read without leaving copies behind. Consider an authentication context requiring MFA or a compliant device for the site.</li>
      <li><strong>DLP</strong> keyed on the label: block or warn on sending labelled content to any recipient outside the approved domains, and cover Teams chat and channel messages, which is where drawings actually get re-shared.</li>
      <li><strong>Endpoint DLP</strong> for the internal estate: block copy of labelled content to removable media and to unapproved cloud upload paths.</li>
      <li><strong>Insider risk / Activity Explorer review</strong> on the label, monthly, with the export control team as the audience — the access record is part of the regulatory evidence, not just an IT metric.</li>
    </ul>

    <h4>Trade-offs to state out loud</h4>
    <ul>
      <li><strong>CAD is the hard part.</strong> STEP and vendor CAD formats are not Office formats. Labelling clients can apply generic protection to non-Office files, but the recipient then needs a compatible viewer to open them, and rights such as “no copy” are not enforced by the CAD application itself. Expect to pair the label with a controlled exchange process for native models, or to release PDFs and keep native CAD inside the tenant.</li>
      <li><strong>Granting print weakens the control</strong> to whatever the watermark and the audit trail can deter. That is a decision for the Chief Engineer and Export Control, recorded as such — not a decision for whoever configures the label.</li>
      <li><strong>Short offline access and hard expiry create support calls</strong> at exactly the wrong moment (a build stoppage). Agree the escalation path and the super-user recovery route before the programme starts, and test recovering a protected file whose grant group has been emptied.</li>
      <li><strong>A named-person grant has an owner cost.</strong> If nobody maintains the guest group when Meridian's staff change, the control silently turns into either a lockout or a stale grant. Name the owner and the review cadence in the design.</li>
      <li><strong>Encryption will break something</strong> — a viewer, a CAM import, a print appliance, a supplier portal. Pilot across the real application estate on both sides before publishing.</li>
    </ul>

    <h4>What a reviewer should look for</h4>
    <ul>
      <li>Is the grant a <em>deliberate, auditable group</em> rather than “everyone in the organisation” or a whole partner domain?</li>
      <li>Do the internal rights still permit co-authoring, and has that been tested rather than assumed?</li>
      <li>Is there an answer for the non-Office file types, or has the design quietly assumed everything is a Word document?</li>
      <li>Does anything downgrade? Check that no auto-labelling policy can overwrite this label with a lower one, and that the label sits correctly in priority order.</li>
      <li>Is there a defined end state — expiry, group removal, site closure — or does the access simply persist after the programme finishes?</li>
      <li>Is the super-user recovery path documented <em>and exercised</em>, and does eDiscovery export still work for the protected content?</li>
      <li>Is the licensing confirmed per user, on both sides of the collaboration, for every capability the design relies on?</li>
    </ul>
  </div>
</section>

</main>

<footer class="page-foot">
  <div class="wrap">
    <p style="margin-block-end:0;">Microsoft Purview Information Protection — common scenarios and label configuration reference. Product and feature names are trademarks of their respective owners. Verify all settings against current vendor documentation before implementation.</p>
  </div>
</footer>

<script>
/* ══════════════════════════════════════════════════════════
   Progressive enhancement. Nothing here hides content unless
   it also provides the control that brings it back, and every
   collapsed region is forced open again for printing.
   ══════════════════════════════════════════════════════════ */
(function () {
  "use strict";

  var doc = document;
  var cards = Array.prototype.slice.call(doc.querySelectorAll("section.card"));

  function el(tag, cls, text) {
    var n = doc.createElement(tag);
    if (cls) { n.className = cls; }
    if (text != null) { n.textContent = text; }
    return n;
  }
  function setCollapsed(node, collapsed) {
    node.classList.toggle("is-collapsed", collapsed);
  }

  /* ── 1. Accordion + progressive configuration reveal ────── */

  cards.forEach(function (card) {
    var h3 = card.querySelector("h3");
    if (!h3) { return; }

    /* Move everything after the heading into a body region. */
    var body = el("div", "card-body");
    body.id = card.id + "-body";
    var node = h3.nextSibling;
    while (node) {
      var next = node.nextSibling;
      body.appendChild(node);
      node = next;
    }
    card.appendChild(body);

    /* Turn the heading into a button, keeping the ↑ top link outside it. */
    var toplink = h3.querySelector(".toplink");
    if (toplink) { toplink.parentNode.removeChild(toplink); }
    var btn = el("button", "acc-toggle");
    btn.type = "button";
    btn.setAttribute("aria-expanded", "true");
    btn.setAttribute("aria-controls", body.id);
    while (h3.firstChild) { btn.appendChild(h3.firstChild); }
    h3.appendChild(btn);
    if (toplink) { h3.appendChild(toplink); }

    btn.addEventListener("click", function () {
      var open = btn.getAttribute("aria-expanded") === "true";
      btn.setAttribute("aria-expanded", open ? "false" : "true");
      setCollapsed(body, open);
    });
    card.accToggle = btn;
    card.accBody = body;

    /* Split the definition list: context stays, configuration hides. */
    var dl = body.querySelector("dl.meta");
    if (!dl) { return; }
    var rows = Array.prototype.filter.call(dl.children, function (c) {
      return c.tagName === "DIV";
    });
    if (rows.length < 2) { return; }
    var first = rows[0];
    var keepFirst = /^applies to/i.test((first.querySelector("dt") || {}).textContent || "");
    var moving = keepFirst ? rows.slice(1) : rows;
    if (!moving.length) { return; }

    var region = el("div", "config-region is-collapsed");
    region.id = card.id + "-cfg";
    var dl2 = el("dl", "meta");
    moving.forEach(function (r) { dl2.appendChild(r); });
    region.appendChild(dl2);

    var controls = el("div", "config-controls");
    var cfgBtn = el("button", "btn", "Show recommended configuration");
    cfgBtn.type = "button";
    cfgBtn.setAttribute("aria-expanded", "false");
    cfgBtn.setAttribute("aria-controls", region.id);
    controls.appendChild(cfgBtn);
    var hint = el("p", "hint", "Decide how you would configure this scenario before you reveal it.");
    controls.appendChild(hint);

    if (!keepFirst && dl.parentNode) { dl.parentNode.removeChild(dl); }
    body.appendChild(controls);
    body.appendChild(region);

    cfgBtn.addEventListener("click", function () {
      var open = cfgBtn.getAttribute("aria-expanded") === "true";
      cfgBtn.setAttribute("aria-expanded", open ? "false" : "true");
      setCollapsed(region, open);
      cfgBtn.textContent = open ? "Show recommended configuration" : "Hide recommended configuration";
    });
    card.cfgToggle = cfgBtn;
    card.cfgRegion = region;
  });

  function setCard(card, open) {
    if (!card.accToggle) { return; }
    card.accToggle.setAttribute("aria-expanded", open ? "true" : "false");
    setCollapsed(card.accBody, !open);
  }
  function setConfig(card, open) {
    if (!card.cfgToggle) { return; }
    card.cfgToggle.setAttribute("aria-expanded", open ? "true" : "false");
    setCollapsed(card.cfgRegion, !open);
    card.cfgToggle.textContent = open ? "Hide recommended configuration" : "Show recommended configuration";
  }

  /* ── 2. Toolbar: search, tier and workload chips ─────────── */

  var TIERS = [
    ["public", "Public", "t-public"],
    ["general", "General", "t-general"],
    ["confidential", "Confidential", "t-conf"],
    ["high", "Highly Confidential", "t-high"]
  ];
  var WORKLOADS = [
    ["email", "Email"],
    ["collab", "Teams / Sites"],
    ["files", "Files at rest"],
    ["auto", "Auto-labelling"]
  ];

  var scenariosH2 = doc.getElementById("scenarios");
  var status = el("p", "filter-status");
  status.setAttribute("role", "status");
  status.setAttribute("aria-live", "polite");
  var searchInput = null;
  var tierBtns = [];
  var workBtns = [];

  if (scenariosH2) {
    var bar = el("div", "toolbar js-only");
    bar.setAttribute("aria-label", "Scenario filters");

    var row1 = el("div", "toolbar-row");
    var fieldLabel = el("label", "field");
    fieldLabel.htmlFor = "scenario-search";
    fieldLabel.appendChild(el("span", "group-label", "Search"));
    searchInput = doc.createElement("input");
    searchInput.type = "search";
    searchInput.id = "scenario-search";
    searchInput.placeholder = "Filter by keyword — e.g. watermark, guest, Do Not Forward";
    fieldLabel.appendChild(searchInput);
    row1.appendChild(fieldLabel);

    var expandAll = el("button", "btn", "Expand all");
    expandAll.type = "button";
    var collapseAll = el("button", "btn", "Collapse all");
    collapseAll.type = "button";
    var revealAll = el("button", "btn", "Show all configurations");
    revealAll.type = "button";
    row1.appendChild(expandAll);
    row1.appendChild(collapseAll);
    row1.appendChild(revealAll);
    bar.appendChild(row1);

    var row2 = el("div", "toolbar-row");
    row2.setAttribute("role", "group");
    row2.setAttribute("aria-label", "Filter by label tier");
    row2.appendChild(el("span", "group-label", "Tier"));
    TIERS.forEach(function (t) {
      var b = el("button", "btn chip " + t[2], t[1]);
      b.type = "button";
      b.setAttribute("aria-pressed", "false");
      b.dataset.value = t[0];
      row2.appendChild(b);
      tierBtns.push(b);
    });
    bar.appendChild(row2);

    var row3 = el("div", "toolbar-row");
    row3.setAttribute("role", "group");
    row3.setAttribute("aria-label", "Filter by workload");
    row3.appendChild(el("span", "group-label", "Workload"));
    WORKLOADS.forEach(function (w) {
      var b = el("button", "btn chip", w[1]);
      b.type = "button";
      b.setAttribute("aria-pressed", "false");
      b.dataset.value = w[0];
      row3.appendChild(b);
      workBtns.push(b);
    });
    bar.appendChild(row3);

    var row4 = el("div", "toolbar-row");
    var clearBtn = el("button", "btn", "Clear filters");
    clearBtn.type = "button";
    row4.appendChild(status);
    var spacer = el("span", "spacer");
    spacer.style.marginInlineStart = "auto";
    row4.appendChild(spacer);
    row4.appendChild(clearBtn);
    bar.appendChild(row4);

    scenariosH2.parentNode.insertBefore(bar, scenariosH2.nextSibling);

    /* Cache the searchable text of each card once. */
    cards.forEach(function (c) {
      c.searchText = (c.textContent || "").toLowerCase().replace(/\s+/g, " ");
    });

    var applyFilters = function () {
      var q = (searchInput.value || "").toLowerCase().trim();
      var terms = q ? q.split(/\s+/) : [];
      var tiers = tierBtns.filter(function (b) { return b.getAttribute("aria-pressed") === "true"; })
        .map(function (b) { return b.dataset.value; });
      var works = workBtns.filter(function (b) { return b.getAttribute("aria-pressed") === "true"; })
        .map(function (b) { return b.dataset.value; });
      var shown = 0;

      cards.forEach(function (c) {
        var cardTiers = (c.dataset.tier || "").split(/\s+/);
        var cardWork = (c.dataset.workload || "").split(/\s+/);
        var okTier = !tiers.length || tiers.some(function (t) { return cardTiers.indexOf(t) > -1; });
        var okWork = !works.length || works.some(function (w) { return cardWork.indexOf(w) > -1; });
        var okText = !terms.length || terms.every(function (t) { return c.searchText.indexOf(t) > -1; });
        var match = okTier && okWork && okText;
        c.classList.toggle("is-filtered-out", !match);
        if (match) {
          shown++;
          if (terms.length) { setCard(c, true); }
        }
      });

      var filtering = terms.length || tiers.length || works.length;
      status.textContent = filtering
        ? "Showing " + shown + " of " + cards.length + " scenarios."
        : "Showing all " + cards.length + " scenarios.";
    };

    searchInput.addEventListener("input", applyFilters);
    tierBtns.concat(workBtns).forEach(function (b) {
      b.addEventListener("click", function () {
        b.setAttribute("aria-pressed", b.getAttribute("aria-pressed") === "true" ? "false" : "true");
        applyFilters();
      });
    });
    clearBtn.addEventListener("click", function () {
      searchInput.value = "";
      tierBtns.concat(workBtns).forEach(function (b) { b.setAttribute("aria-pressed", "false"); });
      applyFilters();
      searchInput.focus();
    });
    expandAll.addEventListener("click", function () {
      cards.forEach(function (c) { setCard(c, true); });
    });
    collapseAll.addEventListener("click", function () {
      cards.forEach(function (c) { setCard(c, false); });
    });
    revealAll.addEventListener("click", function () {
      var anyClosed = cards.some(function (c) {
        return c.cfgToggle && c.cfgToggle.getAttribute("aria-expanded") === "false";
      });
      cards.forEach(function (c) {
        if (anyClosed) { setCard(c, true); }
        setConfig(c, anyClosed);
      });
      revealAll.textContent = anyClosed ? "Hide all configurations" : "Show all configurations";
    });

    applyFilters();
  }

  /* ── 3. Self-check quiz ──────────────────────────────────── */

  var quiz = doc.getElementById("quiz");
  var questions = quiz ? Array.prototype.slice.call(quiz.querySelectorAll(".quiz-q")) : [];
  if (questions.length) {
    var answered = 0;
    var correct = 0;
    var scoreText = el("span", "", "");
    var scorebar = el("div", "scorebar");
    scorebar.setAttribute("role", "status");
    scorebar.setAttribute("aria-live", "polite");
    scorebar.appendChild(scoreText);
    var resetQuiz = el("button", "btn", "Reset quiz");
    resetQuiz.type = "button";
    resetQuiz.className = "btn spacer";
    resetQuiz.style.marginInlineStart = "auto";
    scorebar.appendChild(resetQuiz);

    var paintScore = function () {
      scoreText.textContent = "Score: " + correct + " correct out of " + answered +
        " answered (" + questions.length + " questions)";
    };

    questions.forEach(function (q) {
      var answer = q.querySelector(".quiz-answer");
      var verdict = el("p", "verdict-live");
      answer.insertBefore(verdict, answer.firstChild);
      setCollapsed(answer, true);
      var opts = Array.prototype.slice.call(q.querySelectorAll(".opt"));
      q.addEventListener("change", function (ev) {
        var input = ev.target;
        if (!input || input.type !== "radio") { return; }
        var right = input.value === q.dataset.correct;
        if (!q.dataset.answered) {
          q.dataset.answered = "1";
          answered++;
          if (right) { correct++; }
          paintScore();
        }
        opts.forEach(function (o) {
          o.classList.remove("is-correct", "is-wrong");
          if (o.dataset.opt === q.dataset.correct) { o.classList.add("is-correct"); }
          if (o.dataset.opt === input.value && !right) { o.classList.add("is-wrong"); }
        });
        verdict.className = "verdict-live verdict " + (right ? "right" : "wrong");
        verdict.textContent = right
          ? "Correct."
          : "Not quite — the correct answer is " + q.dataset.correct.toUpperCase() + ".";
        setCollapsed(answer, false);
      });
    });

    resetQuiz.addEventListener("click", function () {
      answered = 0;
      correct = 0;
      questions.forEach(function (q) {
        delete q.dataset.answered;
        Array.prototype.forEach.call(q.querySelectorAll("input[type=radio]"), function (i) { i.checked = false; });
        Array.prototype.forEach.call(q.querySelectorAll(".opt"), function (o) {
          o.classList.remove("is-correct", "is-wrong");
        });
        var a = q.querySelector(".quiz-answer");
        var v = a.querySelector(".verdict-live");
        if (v) { v.textContent = ""; v.className = "verdict-live"; }
        setCollapsed(a, true);
      });
      paintScore();
    });

    paintScore();
    var quizH2 = quiz.querySelector("h2");
    quiz.insertBefore(scorebar, quizH2.nextSibling);
  }

  /* ── 4. Worksheet: model answer, print, clear ────────────── */

  var modelBtn = doc.getElementById("model-toggle");
  var modelBox = doc.getElementById("model-answer");
  if (modelBtn && modelBox) {
    setCollapsed(modelBox, true);
    modelBtn.addEventListener("click", function () {
      var open = modelBtn.getAttribute("aria-expanded") === "true";
      modelBtn.setAttribute("aria-expanded", open ? "false" : "true");
      setCollapsed(modelBox, open);
      modelBtn.textContent = open ? "Reveal a model answer" : "Hide the model answer";
      if (!open) { modelBox.scrollIntoView({ block: "nearest" }); }
    });
  }

  var form = doc.getElementById("ws-form");
  var printBtn = doc.getElementById("print-answers");
  var clearBtn2 = doc.getElementById("clear-answers");
  if (form) {
    form.addEventListener("submit", function (ev) { ev.preventDefault(); });
  }
  if (printBtn) {
    printBtn.addEventListener("click", function () { window.print(); });
  }
  if (clearBtn2 && form) {
    clearBtn2.addEventListener("click", function () {
      if (window.confirm("Clear everything you have typed into the worksheet?")) {
        form.reset();
        Array.prototype.forEach.call(form.querySelectorAll("textarea"), function (t) {
          t.style.height = "";
        });
      }
    });
  }

  /* ── 5. Sticky nav: active section highlighting ──────────── */

  var navLinks = Array.prototype.slice.call(doc.querySelectorAll("nav.toc a[href^='#']"));
  var targets = navLinks.map(function (a) {
    return doc.getElementById(a.getAttribute("href").slice(1));
  });
  var navList = doc.querySelector("nav.toc ul");

  /* On small screens the nav is a horizontal strip; keep the
     active pill inside the visible part of it. */
  function revealNavLink(a) {
    if (!navList || navList.scrollWidth <= navList.clientWidth + 4) { return; }
    var ar = a.getBoundingClientRect();
    var nr = navList.getBoundingClientRect();
    if (ar.left < nr.left + 8) {
      navList.scrollLeft += ar.left - nr.left - 12;
    } else if (ar.right > nr.right - 8) {
      navList.scrollLeft += ar.right - nr.right + 12;
    }
  }

  function setActive(id) {
    navLinks.forEach(function (a) {
      var on = a.getAttribute("href") === "#" + id;
      a.classList.toggle("active", on);
      if (on) {
        a.setAttribute("aria-current", "true");
        revealNavLink(a);
      } else {
        a.removeAttribute("aria-current");
      }
    });
  }
  if ("IntersectionObserver" in window) {
    var visible = {};
    var io = new IntersectionObserver(function (entries) {
      entries.forEach(function (e) {
        visible[e.target.id] = e.isIntersecting;
      });
      for (var i = 0; i < targets.length; i++) {
        if (targets[i] && visible[targets[i].id]) { setActive(targets[i].id); return; }
      }
    }, { rootMargin: "-80px 0px -65% 0px", threshold: 0 });
    targets.forEach(function (t) { if (t) { io.observe(t); } });
  }

  /* ── 6. Printing: open everything, keep typed answers ────── */

  var printState = null;
  function expandForPrint() {
    printState = {
      cards: cards.map(function (c) {
        return {
          card: c,
          open: !c.accToggle || c.accToggle.getAttribute("aria-expanded") === "true",
          cfg: !!(c.cfgToggle && c.cfgToggle.getAttribute("aria-expanded") === "true"),
          hidden: c.classList.contains("is-filtered-out")
        };
      }),
      model: modelBox ? modelBox.classList.contains("is-collapsed") : null
    };
    printState.cards.forEach(function (s) {
      s.card.classList.remove("is-filtered-out");
      setCard(s.card, true);
      setConfig(s.card, true);
    });
    if (modelBox) { setCollapsed(modelBox, false); }
    if (form) {
      Array.prototype.forEach.call(form.querySelectorAll("textarea"), function (t) {
        t.style.height = "auto";
        t.style.height = (t.scrollHeight + 4) + "px";
      });
    }
  }
  function restoreAfterPrint() {
    if (!printState) { return; }
    printState.cards.forEach(function (s) {
      setCard(s.card, s.open);
      setConfig(s.card, s.cfg);
      s.card.classList.toggle("is-filtered-out", s.hidden);
    });
    if (modelBox && printState.model !== null) { setCollapsed(modelBox, printState.model); }
    printState = null;
  }
  window.addEventListener("beforeprint", expandForPrint);
  window.addEventListener("afterprint", restoreAfterPrint);
}());
</script>

</body>
</html>
