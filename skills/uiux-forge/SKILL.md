---
name: uiux-forge
description: "Rethinks a screen (panel, table, form, report, public page) until it tells its story at a glance, THEN makes it beautiful. Logic picks the clearest layout by simulating 4-6 real variants with real data, shows you the ranked mockups with the reason each one lost, and only after you confirm does it run an aesthetic unslop pass on the winner. Use when a screen 'doesn't help me understand', is 'confusing' or 'messy', or when asked to improve UX/UI, reorganize, regroup, simulate layouts, rethink a screen, apply 'the same treatment' to another screen, or make an interface prettier / remove the generic AI look. Gatilhos PT: 'não ajuda a entender', 'tá confusa', 'tá bagunçada', 'melhorar a UX/UI', 'reorganizar', 'repensar', 'agrupar diferente', 'simular layouts', 'deixar bonito', 'mesmo tratamento', print de tela pedindo opinião. Also for a new data-heavy screen (flow, statement, work list, dashboard)."
license: CC-BY-4.0
---

# uiux-forge

A good screen answers the person's question **in the order they think it**, with no extra click
and no leftover doubt — **and then** it looks sharp. A bad screen shows all the right data and
still fails to tell what is going on. Or it is clear but wears the generic AI look that makes it
feel cheap.

uiux-forge fixes both, **in this order**: structure first, skin second.

1. The **logic** simulates real layouts with real data and picks the clearest one by criteria.
2. You **see the ranked mockups** — the winner on top, each loser with the reason it lost — and confirm.
3. Only then does the **unslop pass** make the winner beautiful.

Polishing a badly-structured screen is painting a crooked wall. Clear earns pretty.

The mistake this method kills: grabbing the first layout that comes to mind, polishing it, and
shipping. The first idea is almost never the clearest; you only know by comparing side by side
with the same data.

## The phases

Do them in order. Send the user a one-line status per phase (they follow along).

### 1. Translate the complaint into questions

The complaint ("it doesn't help me understand") is a symptom. Write the **2-4 questions** the
person asks the screen, in their words. Real example: "why is cash dropping this week?", "how
much is left to receive and pay on each order?".

If the user gave a concrete example ("order of 30k, received 20, 10 left, 14 to pay"), it
becomes the **acceptance test**: the final screen must show exactly that, and the automated
test reproduces that case.

### 2. Study the screen and the domain

- Read the whole screen code: page, the classes that build the data, views/partials, tests.
- Understand the domain behind it: what a real payment is, what an estimate is, what is split
  across several entities. Business rules decide grouping (e.g. a shipment expense is **one**
  payment split across orders — you cannot slice it per order).
- Look at the **sibling screens**: decisions already made (links between entities, labels, how
  the other screen shows the same thing) apply here too.
- Pull **real data** (a local DB copy; production only with authorization). Volume and shape of
  the data change the answer: 13 items in a week, long names, lots of zeros, one category that
  dominates. If a case is missing, build a scenario from the user's example.
- Note the **other problems** you find on the way (horizontal scroll on mobile, repeated text,
  information with no owner, filter in the wrong place, no signal that something expands). They
  ship with the delivery as "fixes along the way".

### 3. Simulate 4-6 real variants

Variants must be **structurally different** (different grouping, different hierarchy, different
form), not the same thing recolored. Always include the current screen as the baseline.
Archetype catalog and when each one wins: `references/variants.md`.

Cheap way to build them: one static HTML file, with real data in a JS object and one render
function per variant — start from `assets/mockup-template.html`. Serve on `127.0.0.1` (never
`0.0.0.0`) and screenshot each variant at **1280px and 360px**. Playwright only writes
screenshots inside the project folder and refuses `file://`.

For a chart, simulate the form by swapping the data in the screen's own chart (e.g. Filament
Chart.js: `Alpine.$data(el).getChart()`, change `data`, `update()`), without touching code.

For a screen that already exists where only the **arrangement of sections** changes (tabs, side
column, index, order): simulate by actually moving the sections in the browser, no mockup.
Playwright opens the real screen, a `page.evaluate` moves the nodes, and an `addStyleTag` gives
the variant's CSS. Cheaper and more faithful than redrawing. Details in `references/variants.md`.

### 4. Judge by criteria → pick the winner by logic

Look at the screenshots (not the code) and run each variant through the ruler in
`references/criteria.md`. The ones that decide: answers the main question at a glance with no
extra click; the context to interpret is there and says **what scope** it is ("across the whole
order" vs "this week"); numbers in **one right-aligned column** with sign and color; nothing
repeated, no empty half, nothing side by side at different heights; survives the real worst-case
volume; 360px with no horizontal scroll.

**You** decide (the user asked for a decision, not a menu). Record a short table: variant → why
it was cut. Keep what the losers had good (e.g. the scannability of the summary table becomes
the title of each card in the winner).

### 5. ★ Showcase gate — always runs

Before touching the real code, show the human **why**. Build a page served on `127.0.0.1`:

- The refined winner on top, labeled as the pick.
- The discarded variants below, each with its **"why it lost"** caption (the criterion it failed).
- Start from `assets/showcase-template.html`.

Tell the user: *"Quer ver os mockups? / Want to see the mockups?"* and give the URL (they open
it in their own browser). **Wait for "go ahead" before phase 6.** This is phase 5 = **the why**.

If the user said up front "just do it" / "pode seguir direto", still build the showcase and link
it, but proceed without blocking.

### 6. ★ Unslop the winner — the pretty pass

Phase 6 = **the pretty**. Only runs on the confirmed winner. Full method in
`references/unslop-passes.md`.

First set a one-paragraph **visual direction**: character + the one or two moments that carry the
color + one signature detail, all taken from the product (not invented). Steerable — show it to
the user before editing if the change is big.

Then run the ordered passes, adapted to the user's stack (Filament/Tailwind/Blade), not generic
React:

`spacing → color → icons → copy → type → fine layout → detail → visuals → motion`

Run the **anti-slop checklist**: kill purple-on-purple gradients, glows/neon haze, emoji used as
icons, inconsistent spacing, over-rounded corners, everything-centered, and the other tells of a
vibecoded screen. If the screen is already good, suggest 2-3 small refinements instead of
redesigning it.

### 7. Implement test-first

- A test that reproduces the user's example (their numbers) and the grouping rule — run it, see
  it fail, implement, see it pass.
- Data: load **raw ids and numbers** to the view (not pre-formatted strings), so the view can
  group; build the context in one query, not per row.
- Delete fields and helpers left unused.
- Chart: load the `dataviz` skill first; validate the palette with its script.

### 8. Verify on the real screen

Details in `references/verification.md`. Summary:

- Local server with a **copy** of the DB, on `127.0.0.1`.
- Screenshot at 1280px **and** 360px, light **and** dark, with collapsibles **open**.
- On mobile: `document.documentElement.scrollWidth === innerWidth`.
- Look at the screenshot the way the person would: label wrapping, narrow column, color that
  vanishes, legend with no key. Fix and re-shoot until there is nothing left to point at.

### 9. Deliver + before/after

Final message, in this order:

1. How it turned out (the winner in 3-5 lines, about the screen, not the code).
2. The table of simulated variants and why each lost.
3. Fixes that shipped along the way.
4. Tests and checks done (light/dark/360).
5. Where to see it (local URL, login) and whether it is in production or waiting for approval.
6. **Before/after**: winner screenshot next to the original. Optional square video < 10s
   (Playwright + ffmpeg) for posting — see `references/unslop-passes.md`.
7. **Small choices for the user to decide** (1-3, each with your recommendation).

Do not push to production before the user sees and approves, unless they already asked.

## Shortcut: "the screen is already good"

If the complaint is cosmetic, not structural, skip the 4-6 variant simulation (phases 3-5) and
do 2-3 targeted unslop refinements (phase 6). Do not run the full method to fix a spacing issue.

## When the user asks to "see the proposal" first

"I want to see what you propose", "show me first", "simulate and show me": stop after phase 5.
The showcase page already is the proposal. Give the URL. Code only after "go ahead"; pushing to
production may need a separate explicit request.

## Mid-task addition (chart, filter, column)

Same method in miniature: 2-3 forms simulated with real data, decide, verify. E.g. "chart with
revenue and expense lines" → simulated value-per-week vs running-total-from-today; per-week
became a sawtooth and a line connecting weeks suggested a trend that does not exist; the running
total showed when inflows catch up with outflows. Kept the running total and explained the
trade-off in one sentence.

## Examples

`references/example-cash-flow.md` — the first screen built with this method: complaint,
questions, the 6 variants, the verdict, the refinements, the chart, and what verifying on the
real screen caught.

## The user's own screen rules

This user keeps global screen-organization rules at
`~/.claude/rules/ecc/common/coding-style.md` ("Organização de tela"): masks on phone/CPF/CNPJ
fields, contact becomes a chat shortcut (`wa.me`, `mailto:`), toggles grouped and blue when on,
mobile-first at 360px with no sideways scroll. Follow them — they are part of delivering a screen,
not polish for later.
