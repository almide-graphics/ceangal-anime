<h1 align="center">ceangal-anime</h1>

<p align="center">
  <strong>Animation engine for Almide</strong> — anime.js-inspired tween, easing, timeline, and stagger.<br>
  Pure WASM. No DOM. No JS runtime dependency.
</p>

---

## Modules

| Module | Description |
|--------|-------------|
| `easing` | 31 easing functions (Robert Penner's) + ID-based dispatch |
| `tween` | Value interpolation: from/to, duration, delay, loop, alternate, easing |
| `timeline` | Sequence / parallel orchestration with offset timing |
| `stagger` | Per-element delay: linear, range, from-center, from-edges, eased |

## Usage

```almide
import ceangal_anime.easing as ease
import ceangal_anime.tween as tw
import ceangal_anime.timeline as tl
import ceangal_anime.stagger as stag

// Single tween
let t0 = tw.create(0.0, 100.0, 800.0)
let t1 = tw.with_easing(t0, ease.OUT_ELASTIC)
let t2 = tw.with_delay(t1, 200.0)

// Tick each frame
let t3 = tw.tick(t2, 16.0)
let value = tw.value(t3)  // → current interpolated value

// Timeline
let timeline = tl.create()
  |> tl.add(tw.create(0.0, 1.0, 500.0), 0.0)
  |> tl.sequence(tw.create(0.0, 1.0, 500.0))

// Stagger delays
let delay = stag.from_center(index, total, 60.0)
```

## Easing Functions

All 31 easings, 4 variants each (in/out/inOut + base):

| Curve | In | Out | InOut |
|-------|-----|------|-------|
| Quad | `ease_in_quad` | `ease_out_quad` | `ease_in_out_quad` |
| Cubic | `ease_in_cubic` | `ease_out_cubic` | `ease_in_out_cubic` |
| Quart | `ease_in_quart` | `ease_out_quart` | `ease_in_out_quart` |
| Quint | `ease_in_quint` | `ease_out_quint` | `ease_in_out_quint` |
| Sine | `ease_in_sine` | `ease_out_sine` | `ease_in_out_sine` |
| Circ | `ease_in_circ` | `ease_out_circ` | `ease_in_out_circ` |
| Expo | `ease_in_expo` | `ease_out_expo` | `ease_in_out_expo` |
| Back | `ease_in_back` | `ease_out_back` | `ease_in_out_back` |
| Elastic | `ease_in_elastic` | `ease_out_elastic` | `ease_in_out_elastic` |
| Bounce | `ease_in_bounce` | `ease_out_bounce` | `ease_in_out_bounce` |

Plus `linear` and ID-based dispatch via `ease.apply(id, t)`.

## Tween Features

- **Loop**: `tw.looping(t)` for infinite, `tw.with_loop(t, n)` for n times
- **Direction**: `NORMAL`, `REVERSE`, `ALTERNATE`
- **Delay**: `tw.with_delay(t, ms)`
- **Control**: `tw.pause`, `tw.resume`, `tw.reset`
- **Query**: `tw.value`, `tw.progress`, `tw.is_running`, `tw.is_finished`

## Stagger Patterns

```almide
stag.linear(index, total, step)        // 0, 100, 200, 300...
stag.range(index, total, 0.0, 400.0)   // distribute evenly
stag.from_center(index, total, step)   // center=0, edges=max
stag.from_edges(index, total, step)    // edges=0, center=max
stag.eased(index, total, 0.0, 1.0, ease.OUT_CUBIC)
```

## Demo

5 interactive demos — all animation computed in WASM, JS only reads values and draws:

- **Easing Curves** — 10 easing functions visualized
- **Stagger Grid** — 12×12 elastic ripple from center
- **Fireworks** — 144 particles with radial elastic burst
- **Wave** — responsive bar waveform with sequential stagger
- **Camera** — 6×6×3 3D grid with orbiting perspective projection

## Design

- **Pure math** — no DOM, no CSS, no browser APIs
- **Immutable tweens** — `tick` returns a new Tween, no mutation
- **ID-based easing** — avoids closures (WASM-friendly), `apply(id, t)` dispatch
- **Composable** — tween → timeline → stagger, all pipe-chainable

## License

MIT
