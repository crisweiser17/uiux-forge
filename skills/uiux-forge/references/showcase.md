# The showcase gate — show the human why

Phase 5. The point: the logic picked a winner, but the **user sees the ranked options and the
reason each loser lost before anything is beautified or written to real code.** Logic chooses,
human confirms, unslop beautifies.

## What the showcase page shows

Serve one HTML page on `127.0.0.1` (start from `assets/showcase-template.html`):

1. **Top: the winner**, labeled as the pick, with a one-line reason it won (the criterion it
   nails, plus what it borrowed from the losers).
2. **Below: each discarded variant**, in the order they ranked, each with a **"why it lost"**
   caption naming the exact criterion it failed (from `references/criteria.md`). Not vague
   ("looked worse") — specific ("totals mixed two deadlines", "empty column on mobile").
3. The **same real data** in every variant, so the comparison is fair.
4. Rendered at desktop width; each variant also shots at 360px so the mobile failure is visible.

## How to present it

Send the user a short message:

- One line per variant: name → why it lost.
- The winner in 2-3 lines.
- The URL (they open it in their own browser; do not screenshot-dump all variants into chat).
- The question: *"Quer ver os mockups antes de eu deixar bonito? / Want to see the mockups
  before I make it pretty?"*

Then **wait for "go ahead"** before phase 6 (unslop) and phase 7 (real code).

Exception: if the user said up front "just do it" / "pode seguir direto", still build the
showcase and give the URL (so they can look later), but proceed without blocking.

## Why this gate exists

- The user asked for a **decision, not a menu** — but they also want to see the reasoning, not be
  handed a result they cannot question. The showcase does both: you decided, and you show why.
- It catches a wrong read of the domain cheaply, before any real code or any beautifying work.
- The "why it lost" captions are where the method earns trust: the losers are not hidden, they
  are explained.

## Keep it cheap

The showcase reuses the variant mockups from phase 3 — do not rebuild them. Add the winner's
refined version on top and the "why it lost" captions. One extra HTML file, same data object.
