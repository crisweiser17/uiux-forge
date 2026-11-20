# Verify on the real screen

The automated test proves the rule; only the screenshot proves the screen is clear. Do both.

## Bring the screen up with real data without dirtying the DB

- Copy the DB to the scratch folder and point the server at the copy.
- Laravel/Herd: `cd public && DB_DATABASE=<copy> APP_URL=http://127.0.0.1:<port> php -S 127.0.0.1:<port> ../vendor/laravel/framework/src/Illuminate/Foundation/resources/server.php`
  (`artisan serve` without `--no-reload` ignores the environment `DB_DATABASE`).
- Server always on `127.0.0.1`, never `0.0.0.0` (exposing to the network is blocked and unneeded).
- Check project memory for the local login and busy ports.
- Production: read data only with the user's explicit authorization.

## What to shoot

| Width | Theme | State |
|---|---|---|
| 1280px | light | collapsibles **open** (the detail is what changed) |
| 1280px | dark | one group open |
| 360px | light | top + one part open |

Playwright MCP: writes screenshots only inside the project folder (`.playwright-mcp/`); `file://`
is refused — serve the HTML on `127.0.0.1`. To open collapsibles, click via `browser_evaluate` on
the row buttons. For chart hover, `page.mouse.move` inside a `browser_run_code_unsafe`. Dark theme
in Filament: `localStorage.setItem('theme','dark')` and reload (set back to `light` after).

## Automated browser checks

```js
// horizontal scroll: these must be equal
[document.documentElement.scrollWidth, innerWidth]
// who is overflowing
[...document.querySelectorAll('body *')].filter(e => e.scrollWidth > e.clientWidth + 1 && getComputedStyle(e).overflowX !== 'visible').map(e => e.className)
```

## What usually shows up in the shot (and the fix)

| Symptom | Fix |
|---|---|
| first-column label wraps to 2 lines | `minmax(14rem, 1fr)` on the first column + `white-space: nowrap` on the label |
| table overflows between 640 and 1100px (sidebar open) | `container-type: inline-size` on the table + `@container (max-width: …)` instead of window `@media` |
| rows with no chevron misaligned | invisible chevron (`visibility: hidden`) as a spacer |
| secondary text bold inside a title | `font-weight: 400` on the secondary |
| chart squashed on mobile | larger `aspectRatio` on narrow screens |
| Chart.js legend with no color (with `usePointStyle`) | `generateLabels` with `strokeStyle` from the line and `fontColor: chart.options.color` |
| tooltip with a white key | `labelColor` returning the dataset color (default grabs the point ring) |
| "·" left hanging at line end before a text button | a `<button>` doesn't flow with text: use `<span role="button" tabindex="0">` with click and Enter |
| modifier color vanishes in dark (`.pz-now` went gray) | the `.dark .x` rule comes later and wins: repeat `.dark .x.modifier` |
| color right in light, wrong in dark | validate the palette on both backgrounds (`dataviz` skill script); use a pair that passes both |

## Smoke test in production (after deploy)

No need to log into the production panel: run the builder and the page through the server, with
the Laravel bootstrap and an admin (`ssh ... 'cd public_html && php' <<'EOF'` +
`auth()->setUser(admin)` + `Livewire::test(Page::class)->html()`), and look at the day's log.
Tinker may not run on the server (psysh folder without write permission).

## Before delivering

- Screen suite passing; formatter (Pint) run.
- Sibling screens: suggest the same treatment (one line), do not do it unasked.
- Mockup server stopped; the preview one can stay for the user to look at (give the URL).
