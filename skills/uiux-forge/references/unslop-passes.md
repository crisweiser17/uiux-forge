# The unslop pass — making the winner beautiful

Runs in **phase 6**, only on the layout the logic already picked and the user confirmed. Goal:
take a clear screen and remove the generic, vibecoded, "an AI made this" look — without
redesigning what already works.

Idea adapted from unslop (github.com/heydamir/unslop), ported to this user's stack
(Filament / Tailwind / Blade / Livewire) instead of generic React.

## First: a visual direction (one paragraph)

Before editing anything, decide the direction and, if the change is big, show it to the user so
they can steer early. Pull it **from the product**, do not invent:

- **Character** — what this product feels like (a serious finance panel is not a playful
  consumer app).
- **The one or two moments that carry the color** — where a single accent earns its place (the
  primary action, the number that matters, an alert). Everything else stays quiet.
- **One signature detail** — a small consistent choice that becomes the product's tell (a corner
  radius, a divider style, a mono number treatment).

## Then: the ordered passes

Run them in this order. Each pass is one focused sweep; do not jump ahead.

1. **Spacing** — consistent scale (4 / 8 / 12 / 16 / 24). Remove random one-off margins. Group
   related things closer, separate unrelated things more. Most "AI look" is bad spacing.
2. **Color** — one neutral ramp + one accent. Kill the second and third accent. Semantic colors
   (success/danger) earn their spot; decoration does not. Must pass on light **and** dark.
3. **Icons** — real icon set (Heroicons in Filament), consistent weight and size. **No emoji as
   UI icons.** An icon only where it adds meaning, not on every label.
4. **Copy** — natural language, sentence case, no internal names leaking ("batch", "Estimado:").
   Labels say what the thing is to the person, not to the database.
5. **Type** — one family, a real scale (not 7 random sizes), tabular-nums on numbers, weight for
   hierarchy instead of color. Secondary text is lighter, not bold.
6. **Fine layout** — align edges, equal column widths where parallel, no element stretched to
   full width when its content is short, no two sections side by side at different heights.
7. **Detail** — borders, dividers, radius, shadow. One subtle shadow, not a glow. One radius
   scale. Remove the heavy drop-shadows and the neon ring.
8. **Visuals** — real imagery/illustration only if the product needs it; otherwise none. If a
   custom visual is needed, generate it to match the direction, never a stock-slop gradient blob.
9. **Motion** — a few short, purposeful transitions (hover, expand, toast). No bouncing, no
   endless pulsing glow.

## Anti-slop checklist (what to actively kill)

- Purple / purple gradient backgrounds (the #1 AI tell).
- Glows, neon haze, heavy multi-layer drop shadows.
- Emoji used as icons or bullets.
- Everything centered when it should be left-aligned (forms, tables, lists).
- Over-rounded corners (pill-shaped cards, 24px radius on everything).
- Inconsistent spacing (every gap a different number).
- Three+ accent colors competing.
- Gradient text, rainbow borders, glassmorphism for no reason.
- Fake depth: shadows on flat elements that do not move.

## "The screen is already good"

If the clear winner already looks clean, do **not** redesign it. Propose 2-3 small refinements
(a spacing fix, one color pulled back, numbers aligned) and stop. Over-editing a good screen is
its own kind of slop.

## Before/after artifact (phase 9)

- Screenshot the original and the unslopped winner at the same width; put them side by side in
  the delivery message.
- Optional square video < 10s of the screen "unslopping itself": capture the before and after
  states with Playwright (screenshots inside the project folder), then stitch with ffmpeg:
  `ffmpeg -framerate 2 -i frame_%03d.png -vf "scale=1080:1080:force_original_aspect_ratio=decrease,pad=1080:1080:(ow-iw)/2:(oh-ih)/2:white" -pix_fmt yuv420p out.mp4`.
  This is the shareable asset that gives the work visibility.
