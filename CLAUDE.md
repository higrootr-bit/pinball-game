# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Browser-based pinball game built as a single-file HTML5 application using Canvas 2D rendering and vanilla JavaScript. No build tools, frameworks, or dependencies.

## Development

Open `pinball.html` directly in a browser — no server required:
```
open pinball.html
```

## Git Workflow

After completing any meaningful unit of work, commit the changes with a clean, descriptive commit message and push to GitHub (`origin main`). This ensures we never lose progress and can revert if needed. Do not batch up multiple unrelated changes — commit as you go.

## Architecture

Everything lives in `pinball.html` — HTML structure, CSS styles, and JS game logic are all inline in one file.

### Game Loop

`loop()` runs via `requestAnimationFrame`, calling `update()` then `draw()` each frame.

### Key Systems

- **Physics**: Gravity + friction applied per-frame. Ball-to-wall/bumper/flipper collisions use `distToSegment()` for segment proximity and `reflectBall()` for velocity reflection.
- **Game objects**: Defined as plain object arrays (`walls`, `bumpers`, `slingshots`, `rollovers`, `flippers`). Each has position data and collision parameters. To add a new bumper or wall, push to the relevant array.
- **Flippers**: Angle interpolates toward `activeAngle` or `restAngle` at 0.3 rate. Collision adds extra impulse when the flipper is actively swinging.
- **Scoring**: Combo multiplier (max 8x) decays after 2s without a bumper hit. Rollover lanes give 500 pts each + 5000 bonus for collecting all three.
- **Rendering**: Each object type has its own draw block in `draw()`. Bumpers use radial gradients and glow on hit. Particles are spawned on collisions and decay over time.
- **State persistence**: High score stored in `localStorage` under key `pinballHigh`.

### Playfield Coordinate System

Canvas is 400x700. Playfield is bounded by walls at x=20 (left), x=380 (right), y=20 (top). Launch lane occupies x=350-380 on the right side. Drain gap is between x=70 and x=330 at the bottom.
