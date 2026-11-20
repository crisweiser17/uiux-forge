# Example: Cash flow (prestige_pedidos, Nov 2026)

The first screen built with this method. Use it as a model of depth and of delivery. The real
case keeps Brazilian money (R$) and the user's domain (orders, shipments).

## Complaint

Screenshot of the screen + "it's not helping me understand what's going on. Many orders have
items to pay and a balance to receive. When I open the week and group by order: the order is
R$ 30,000, I already received 20, 10 is left, expenses are 14,000 summing the items, and it shows
the items."

## Questions extracted

1. Why is cash dropping this week?
2. How much is left to receive and how much is left to pay on each order?
3. What is real and what is an estimate?

Acceptance test: the 30,000 / 20,000 / 10,000 / 14,000 case became a Pest test.

## What the study found

- The open week was a flat list with items from 10 orders mixed, sorted by value.
- A shipment expense (freight from China) is **one payment** split across several orders — the
  "to pay" screen already showed it that way ("once, with the split orders alongside").
- Problems outside the complaint: the table scrolled sideways on mobile; "Personal · Personal"
  repeated; an "Estimated:" prefix on half the items; items with no date; nothing signaled the
  row expanded; the period selector sat inside the table title but also drove the numbers above.

## Variants simulated (same data, 1280px and 360px)

| Variant | Verdict |
|---|---|
| V1 flat list (current) | orders mixed, can't sum how much is each one's |
| **V2 stacked card per order** | **winner**: title with how much the order moves cash this week, "whole order" strip, items with date |
| V3 card with Receive \| Pay side by side | half the cards with "—" (a personal order only pays), mismatched heights |
| V4 summary table per order + click | extra click, no picture of the order; 4 columns don't fit at 360px |
| V5 by money type | loses the per-order story |
| V6 matrix order × week | almost all empty, scrolls sideways |

## Refinements of the winner

- Orders sorted by what moves the most cash (in + out), not by net balance — by net, an order
  receiving 10k and paying 11k vanished at the bottom.
- Inside the card: what comes in before what goes out, then by date.
- Context strip with explicit scope: "Whole order: worth R$ 30,000 · already received R$ 20,000 ·
  left to receive R$ 10,000 · left to pay R$ 14,000". "Left to pay" includes the order's share of
  the shipment expenses (by the split quotas).
- Personal order: "Personal order: money only goes out · left to pay …".
- Shipment in its own card: "Shipment R-026 · split across P-0038, P-0040".
- Item: date ("Tue 17/11", "due 04/11", blank when it's today), description, gray "estimated" tag,
  value with sign.
- Week row: chevron, "13 items", "Up to today" with "overdue or already due" below.
- The table becomes a card when it itself gets narrow (`@container`), with a label on each number.

## Additions requested later (same method in miniature)

- **Chart** ("revenue and expense lines"): simulated per-week vs summed-from-today, swapping the
  data in the screen's own Chart.js. Per-week = a misleading sawtooth; kept the running total
  (shows when inflows catch up with outflows). Tooltip with the whole week and a conclusion:
  "Cash dropped R$ 18,448.04 since today". Blue/orange validated for colorblindness on both
  backgrounds (light orange failed in dark → a pair that passes both).
- **30/60/90 days**: the selector moved above the numbers, the chart and the table.

## What verifying on the real screen caught

Week label wrapping (first column squeezed), overflow with the sidebar open (swapped `@media` for
`@container`), empty rows misaligned (invisible chevron), "split across" in bold, chart squashed on
mobile, Chart.js legend and tooltip with no color and a dark legend in dark theme. None of this
showed in the tests.

## Implementation

- `MovimentoCaixa` now loads `pedidoId`, `remessaId`, `estimado`, `rateio` (ids and numbers, not
  ready text); `origem`/`url`/`pessoal` deleted.
- `GruposCaixa::fotos()` builds the context of all orders in one query; `GruposCaixa::de()` groups
  each period's items.
- `GraficoCaixa` (ChartWidget) receives the points already computed by the screen, embedded with
  `@livewire(..., key(md5 of the points))` to remount when the period changes.
- Tests first: group per order with the context, shipment with the split summing into left-to-pay,
  cumulative chart, 30/60/90 period.

## Delivery

Message with: how it turned out, the variant table, fixes shipped along, tests and verified shots,
local URL, and two small choices (blank "today" date? order by value or by date?). The user
replied in one line, asked for the chart and 30 days, approved, and it went to production.

---

*Glossary (PT→EN): pedido = order · remessa = shipment · rateio = split/proration · falta receber =
left to receive · falta pagar = left to pay · caixa = cash · estimado = estimated.*
