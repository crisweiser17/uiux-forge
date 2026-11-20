# uiux-forge — design spec

**Date:** 2026-11-20
**Status:** approved design, pre-plan

## What it is

A Claude Code skill that rethinks a screen (panel, table, form, report, public page)
until it tells its story at a glance, **then** makes it beautiful. Evolves the existing
`uiux-improve` skill (structure-first method) by adding an aesthetic "unslop" layer and a
mandatory showcase gate where the human sees the ranked mockups and the reasoning before
anything is beautified.

Two existing ideas merged:
- **uiux-improve** (the user's skill): picks the clearest layout by simulating 4-6 real
  variants with real data and judging by "least doubt" criteria. Structure/information layer.
- **unslop** (github.com/heydamir/unslop): removes AI-slop aesthetics via named ordered
  refinement passes. Skin layer.

Order is deliberate: **logic chooses, human confirms, unslop beautifies.** Polishing a
badly-structured screen is painting a crooked wall.

## Core principle

- Structure before skin. A pretty screen that still confuses is not done.
- The logic picks the winner; the user always sees *why* the losers lost before the pretty pass.
- Real data + domain rules drive grouping, not DB column order.
- Verify on the real screen: 1280px and 360px, light and dark.

## The flow (9 phases)

1. **Complaint → questions** — write the 2-4 questions the person asks the screen, in their words. A concrete example becomes the acceptance test.
2. **Study code + domain + real data** — read the whole screen, understand business rules (a shipment expense is *one* payment split across orders — can't be sliced per order), look at sibling screens, pull a real DB copy.
3. **Simulate 4-6 structural variants** — genuinely different grouping/hierarchy, not recolors. Static HTML, real data in a JS object, screenshot each at 1280px and 360px. Always include the current screen as baseline.
4. **Judge by criteria → winner by logic** — look at the screenshots (not the code), run the "least doubt" ruler, decide. Record a short table: variant → why it was cut.
5. **★ Showcase gate (always runs)** — build a page served on `127.0.0.1`: refined winner on top, discarded variants below, each with its "why it lost" caption. Ask the user: *"quer ver os mockups/HTML?"* and give the URL. Wait for "pode seguir" before phase 6. This is phase 5 = **the why**.
6. **★ Unslop the winner** — phase 6 = **the pretty**. First set a one-paragraph visual direction (character + 1-2 moments that carry the color + one signature detail, taken from the product). Then run ordered passes adapted to the user's stack (Filament/Tailwind/Blade), not generic React: `spacing → color → icons → copy → type → fine layout → detail → visuals → motion`. Run the anti-slop checklist: kill purple gradients, glows, emoji-as-icons, inconsistent spacing, over-rounded corners, everything-centered.
7. **Implement test-first** — a test that reproduces the user's example numbers and the grouping rule: red, implement, green. Load raw ids/numbers to the view, build context in one query.
8. **Verify on the real screen** — local server with a DB copy on `127.0.0.1`, screenshot at 1280px and 360px, light and dark, collapsibles open. On mobile: `scrollWidth === innerWidth`.
9. **Deliver + before/after** — final message: how it turned out, the variant table and why each lost, side fixes, tests/checks done, where to see it. Include before/after screenshots side by side. Optional square video < 10s (Playwright + ffmpeg) for posting.

## Shortcut: "screen already good"

If the complaint is cosmetic, not structural, skip the 4-6 variant simulation (phases 3-5) and
do 2-3 targeted unslop refinements. Avoids running the full method to fix a spacing issue.

## What is preserved from uiux-improve (unslop does not do these)

Structural variant simulation · real DB data · business rules in grouping · acceptance test ·
360px no horizontal scroll · light/dark check · sibling screens share a spine · the Portuguese
screen-organization rules in `~/.claude/rules/ecc/common/coding-style.md`.

## Language

Public skill + README in **English**, skill `description` has **bilingual triggers (PT + EN)** so
the user's Portuguese prompts still fire it. Body and `references/` in English; worked examples
keep the user's real cases (cash-flow, receivables/payables) with a short PT→EN glossary.

## Repo structure (Claude Code plugin + marketplace)

```
uiux-forge/
├─ README.md                     # MiroFish-style landing page
├─ .claude-plugin/
│  └─ marketplace.json           # installable like unslop
├─ skills/uiux-forge/
│  ├─ SKILL.md
│  ├─ references/
│  │  ├─ variants.md             # ported from uiux-improve variantes.md
│  │  ├─ criteria.md             # ported from criterios.md
│  │  ├─ verification.md         # ported from verificacao.md
│  │  ├─ unslop-passes.md        # new: ordered passes + anti-slop checklist
│  │  ├─ showcase.md             # new: how to build the 127.0.0.1 comparison page
│  │  └─ example-cash-flow.md    # ported exemplo-fluxo-caixa.md (+ EN glossary)
│  └─ assets/
│     ├─ mockup-template.html    # ported
│     └─ showcase-template.html  # new: winner-on-top + losers-with-reasons layout
├─ LICENSE                       # CC BY 4.0 (matches unslop)
└─ docs/…                        # this spec
```

## README blueprint (replicating MiroFish layout)

Top-to-bottom, adapted:
1. Centered logo/banner (`<div align="center">`, ~75% width).
2. Badge row: stars, forks, license CC BY 4.0, "Claude Code plugin".
3. Language toggle: `English | Português` (pipe-separated links to README.md / README-PT.md).
4. **⚡ Overview** — blockquote: the "logic chooses, you confirm, unslop beautifies" promise.
5. **🧠 The method** — why structure-first beats pretty-first.
6. **🎬 Demo** — GIF of the full flow: complaint → variants → showcase → unslopped winner.
7. **📸 Screenshots** — 2x3 HTML table grid: complaint screen, 2-3 variants, showcase page, unslopped result.
8. **🔄 Workflow** — the 9 numbered phases, condensed.
9. **🚀 Quick start** — plugin marketplace install (one-liner like unslop), manual install fallback.
10. **📄 License / credits** — CC BY 4.0; credit unslop (Damir Sokolovsky) for the aesthetic-pass idea.

## Demo assets

The GIF/video and screenshot grid are generated from a **real case**: the cash-flow screen of
`prestige_pedidos` (the first screen built with the uiux-improve method). Needs the app running
locally with a DB copy + Playwright + ffmpeg. This is the last build phase and can lag the skill
itself; the repo ships with placeholder images if the live capture is not ready.

## Out of scope (YAGNI)

- No settings/config file for the skill.
- No support for stacks the user doesn't use (keep examples Filament/Blade/Livewire).
- No auto-publish to production — delivery always waits for the user's approval.
- Discord/Twitter/community badges (MiroFish has them; not needed for a solo skill).
