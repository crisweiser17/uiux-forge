# The "least doubt" ruler

Run each variant through these questions looking at the **screenshot**, not the code. A variant
that fails a high-weight item is out, however pretty it is.

## High weight (these decide)

1. **Answers the main question at a glance?** Count clicks and scrolling to the answer. An extra
   click is only acceptable if the top layer already answers most of the time.
2. **Is the context to interpret right there, with scope?** A number with no "of what" creates
   doubt. State scope in the label: "Whole order: worth…", "this week", "running from today".
3. **Numbers in one right-aligned column**, with sign (+/−), color and `tabular-nums` — so you
   can add them in your head. Values scattered across different columns per row = doubt.
4. **No repetition** ("Personal · Personal", "today" on every line, same prefix on every item).
5. **No empty half** (a card with "—" on one side) and **nothing side by side at different
   heights** (the eye doesn't know where to go). Prefer a main column + side column.
6. **Survives the real worst case**: 13+ items, a huge client name, one group dominating.
7. **360px with no horizontal scroll**; table becomes a card with a label on each number.
8. **Group total has a single scope**: group first by time (deadline, week), then by person. A
   client/creditor total summing "today" with "next month" is not actionable.
9. **Header totals aligned with the row values** (same grid). Subtotal only when it sums 2+ items.

## Medium weight (tie-breakers)

10. **Order the person thinks in**: what → how much → when → detail. Heaviest first.
11. **Consistency with sibling screens** (links between entities, labels, how the other screen
    shows the same data).
12. **Signal of interaction**: what expands has a chevron; what is a link looks like a link; what
    does not expand has no click cursor.
13. **Filter inside/above what it filters**, never loose. If it affects several parts, it sits
    above all of them.
14. **Natural language**, no internal names ("Estimated:" becomes a tag; "batch" never shows).

## Red flags (symptoms of the wrong layout)

- You need a paragraph of description to make the screen make sense.
- The same value shows in two places under different names.
- A whole column of "—".
- Color as the only way to tell things apart (no signal, no text, no icon).
- Red/green as the only two series of a chart (worst pair for colorblind users).
- A period label wrapping to two lines because the first column got squeezed.
- Tabs that on mobile become a crooked stack (the 3rd alone at the bottom): if the volume fits
  one screen, stacked blocks beat tabs.
- A "default" filter showing as an active filter ("Which: All open ×").
- A column mixing two subjects (a logistics stage inside the payment-status badge).

## Record the decision

A short table for the user:

| Variant | Why it was cut |
|---|---|
| … | one sentence, naming the criterion it failed |

Plus one line for the winner: the criterion it wins on and what it borrowed from the losers.
