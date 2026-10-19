<img src="assets/icon.svg" width="64" height="64" alt="">

# Crossy Road Clone

A complete, playable voxel-style Crossy Road clone in a single `index.html` — Three.js from a CDN, zero external assets (every prop is a procedural primitive).

Created in [T3 Code](https://t3.codes).

## Play

Open `index.html` in a browser, or serve the folder:

```bash
npx serve .          # or: python3 -m http.server 5173
```

Serving over HTTP is recommended — some browsers block ES-module imports (needed to
fetch Three.js from the CDN) on `file://` pages.

## Controls

| Action | Keys | Touch |
| --- | --- | --- |
| Forward | `↑` / `W` / `Space` | tap, or swipe up |
| Left / right | `←` `→` / `A` `D` | swipe left / right |
| Back (limited) | `↓` / `S` | swipe down |
| Restart | `Space` / `Enter` / `R` | tap |
| Mute | `M` | speaker button |
| Camera style | `C` | cube button |

## Rules

- Score = furthest row reached. Coins add to a separate counter (and are saved with your best).
- **Cars, trucks, buses and trains** flatten you. Trees and rocks block your path.
- **Water** drowns you — ride the logs, or hop across the lily pads.
- The flashing crossing lights warn you a train is coming. Listen for the bell.
- Ride a log too far towards the edge and you get **swept away**.
- Wander more than 7 rows behind your best and the **hawk** takes you.

## How it is built

Everything lives in `index.html`: inline CSS for the HUD/overlays, and one commented
script that is organised into sections — bootstrap (CDN loader with fallbacks),
shared geometry/material library, prop factories, chunk-based procedural generation,
the player controller, death sequences, particles/audio, and the game loop.

Notes on the implementation:

- **Terrain** is generated per *chunk* (a run of grass, 2–4 road lanes, 1–3 water rows,
  or a rail crossing) that streams in ~24 rows ahead and is deleted ~12 rows behind the player.
- **Traffic** is a rigid, evenly spaced convoy on a looping coordinate, so lane timing is
  deterministic and gaps are always survivable. Lane speeds and gaps ramp with distance.
- **Water** uses the same looping maths for logs; lily-pad rows reuse their column layout
  when they stack, so you can't be stranded in front of an unreachable row.
- **Movement** is grid-snapped 135 ms hops with a parabolic arc, squash & stretch, buffered
  input, and a slight yaw turn towards the direction of travel.
- **Audio** is entirely synthesised with WebAudio (oscillators + filtered noise) — no samples.
