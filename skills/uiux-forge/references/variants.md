# Variant catalog to simulate

Pick 4-6 that make sense for the screen's data. Always include the **current** one as baseline.
Each archetype below says when it tends to win and where it tends to lose — use it as a starting
point, but decide from the screenshots with real data.

## Grouping archetypes (what goes together)

| Archetype | Wins when | Loses when |
|---|---|---|
| **Flat list** (everything mixed, sorted by value or date) | few items, a single entity | items from several entities: nobody can sum "how much is this one" |
| **By main entity** (order, client, project) | the question is "how is each X doing?" | the question is "how much of each type?" |
| **By type** (receipt, real expense, estimate, commission) | the question is "how much is real vs estimated?" | the story is per entity — type becomes a label |
| **By time** (day, week, month) | flow, schedule, deadlines | comparing entities against each other |
| **By status/stage** (to do, overdue, done) | the day's work list | a report of numbers |
| **Two layers** (time → entity, entity → time) | the screen needs both questions | the second level has 1 item per group (becomes noise) |

## Form archetypes (how to show it)

| Form | Wins when | Loses when |
|---|---|---|
| **Stacked card per group** (title with total, context strip, items) | each group has its own context to show | groups of 1 line with no context (card becomes an empty frame) |
| **Two columns side by side** (in vs out, before vs after) | both sides almost always have items | one side lives empty: half the cards with "—", mismatched heights |
| **Summary table + click to expand** | many groups, scanning is the main job | the detail is what answers the question (extra click), and 4 columns don't fit on mobile |
| **Matrix/pivot** (entity × period) | spreadsheet mind, few periods, full cells | cells almost all empty; scrolls sideways on mobile |
| **Timeline** (events in sequence) | the order of events is the story | many events on the same day |
| **Tabs** | different subjects on the same screen (numbers vs work list) | the tabs hide what the person needs to compare |
| **Number/stat** instead of a chart | the answer is a single value | the answer is a shape over time |

## Chart: forms to simulate

| Question | Likely form | Trap |
|---|---|---|
| When do inflows catch up with outflows? | **running totals from today** (cumulative) | value-per-period as a line becomes a sawtooth and suggests a trend |
| How much comes in and out each period? | grouped bars | a line connecting discrete periods |
| Balance over time | one line (or a light area) | two Y axes |
| One item against the rest | highlight one series, rest gray | a rainbow of categories |

Before any chart, load the `dataviz` skill (form, validated color, legend, tooltip).

## How to build the variants fast

`assets/mockup-template.html`: data in an object, one function per variant, base CSS with card,
item row with date/description/value, right-aligned numbers, labels, and a 360px breakpoint. Same
data in every variant — otherwise the comparison is unfair.

Include in the dataset the **real worst case** (most items, longest name, a one-line group, a
group with no context) and the **example the user gave**.

### Rearranging an existing screen: in the browser itself

When the question is "tabs, side column, or stack?" on a finished screen (e.g. an order screen
with 10 stacked sections), do not redraw it in HTML. Open the real one and move the nodes:

```js
// Playwright (browser_run_code_unsafe): one variant per page load
await page.goto(url); await page.waitForLoadState('networkidle');
await page.addStyleTag({ content: '.sim-side{display:grid;grid-template-columns:minmax(0,1fr) 300px;gap:16px}' });
await page.evaluate(() => {
  const stack = document.querySelector('.pp-stack');
  const section = t => [...stack.querySelectorAll(':scope > .fi-section')]
    .filter(s => t.includes(s.querySelector('.fi-section-header-heading')?.innerText.trim()));
  const grid = Object.assign(document.createElement('div'), { className: 'sim-side' });
  const main = document.createElement('div'), side = document.createElement('div');
  grid.append(main, side); stack.append(grid);
  main.append(...section(['Parts', 'Costs'])); side.append(...section(['Notes', 'Attachments']));
});
await page.screenshot({ path: '.playwright-mcp/v-side.png', fullPage: true });
```

- **Tabs:** a fake bar with title + summary line, and `display: none` on the other tabs' sections.
- **Sticky index:** a `position: sticky` bar with links to the sections.
- Reload the page between variants (the mutated DOM does not reset itself).
- Works for Filament and any locally-served screen. For a new screen, or one that changes what
  each row shows (columns, data grouping), the HTML mockup is still the way.
