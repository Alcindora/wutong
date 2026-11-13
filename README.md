# wutong-cluster.js

An interactive automotive instrument cluster that runs entirely in the browser.
Both gauges are drawn on `<canvas>` every animation frame — ticks, arcs, numerals
and needles are computed in JavaScript. No images, no frameworks, no build step.

**Live demo:** https://alcindora.github.io/wutong/ (also at https://wutong.js.org once DNS is approved)

## Features

- Spring-physics needles (stiffness + damping per gauge) that overshoot and
  settle like real stepper-motor instruments
- A tiny vehicle model: throttle -> power -> speed, with road load and
  regenerative braking in the negative power region
- Drive simulation: launch, cruise, lift-off regen, crawl, stop
- Eco / Comfort / Sport modes that re-tune gauge ranges, redline and accent color
- Dark and light cluster themes, live clock, tell-tale indicators
  (turn signals, high beam, seat-belt reminder)
- Retina-aware canvas sizing via `devicePixelRatio`

## How it works

One `requestAnimationFrame` loop integrates the vehicle model, then feeds speed
and power through independent spring integrators:

```
accel  = (target - value) * stiffness
vel   += accel * dt
vel   *= exp(-damping * dt)
value += vel * dt
```

All drawing is plain Canvas 2D: arcs for the track / value / red-zone, computed
tick positions, and a rotated needle path. The DOM around the canvases only
carries the center stack (gear, battery, range) and the top/bottom status bars.

## Run locally

Open `index.html` in any modern browser, or serve the folder:

```
npx serve .
```

## Reuse

The whole demo is a single `index.html`. Copy the gauge renderer, change the
scales, and it becomes a tachometer, a pressure gauge, or an industrial
dashboard. MIT licensed.

## Author

Wu Tong (Alcindora) — automotive HMI designer & developer, Nanjing.
Homepage: https://wutong.qd.je
