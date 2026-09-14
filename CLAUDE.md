# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Panda Runner (Панда Раннер) — a Ukrainian-localized, Chrome-Dino-style endless runner. It is a fully static, zero-dependency, zero-build web app: `index.html` + `style.css` + `game.js`. There is no package.json, bundler, linter, or test suite in this repo.

## Running the Project

There are no compiler/build steps. Either open `index.html` directly in a browser, or serve it locally (recommended so `localStorage` and fonts behave consistently):

```
python3 -m http.server 8000
# or
npx serve .
```

There are no lint or test commands to run — verify changes by loading the page in a browser and playing the game (keyboard: Space/↑/W jump, ↓/S duck/roll, P pause, M mute, R restart; touch buttons on mobile).

## Architecture

All game logic lives in `game.js`, wrapped in a single `'use strict'` IIFE (no modules, no globals leaked, no external JS libraries). It is organized as ES6 classes:

- **`SoundController`** — procedurally synthesizes all sound effects via Web Audio API oscillators (`playJump`, `playDuck`, `playMilestone`, `playBonus`, `playHit`, `playClick`). No audio files are used. Mute state persists in `localStorage` (`panda_runner_muted`).
- **`Panda`** — the player: physics (gravity/jump velocity), hitbox, and state-specific canvas drawing methods (`drawRunning`, `drawJumping`, `drawDucking`, `drawHit`, `drawHead`).
- **`ObstacleManager`** — spawns, moves, and draws obstacles (small/tall bamboo, bamboo clusters, mossy rocks, cranes flying at 3 heights with different avoidance requirements) and golden-bamboo bonus pickups (+100 points).
- **`Environment`** — parallax scrolling (ground, forest, mountains, clouds, stars, falling leaves) and sky-color interpolation driven by score, producing the day → sunset → night → sunrise cycle every 700 points.
- **`ParticleSystem`** — dust trails, bonus sparkles, and floating score text (e.g. `+100`).
- **`Game`** — orchestrator: state machine (`START`, `RUNNING`, `PAUSED`, `GAMEOVER`), keyboard/touch input bindings, delta-time `requestAnimationFrame` loop, high-score persistence (`panda_runner_hi` in `localStorage`), and the skin shop (SkinPoints earned per 1000 score, stored in `panda_runner_skin_points`, spent on unlocking/selecting panda skins).

Canvas size constants: `CANVAS_WIDTH = 900`, `CANVAS_HEIGHT = 320` — the canvas is scaled responsively via CSS, so drawing code should stay proportional to these.

`style.css` centralizes theme values (colors, fonts, borders) as CSS custom properties on `:root` (e.g. `--accent-green`, `--font-retro`) — use these variables rather than hardcoding values when styling.

## Conventions

- **No image/audio assets.** All visuals are drawn with Canvas 2D paths (`arc`, `ellipse`, `lineTo`, `quadraticCurveTo`, etc.); all sound is synthesized via Web Audio API oscillators. Don't introduce `.png`/`.jpg`/`.svg`/`.mp3`/`.wav` files unless explicitly requested.
- **No frameworks or third-party JS libraries** — vanilla JS only, all inside the existing IIFE.
- **Localization is Ukrainian (uk).** All in-game text, HUD, overlays, and hints are Ukrainian — keep new UI text consistent with this.
- **New gameplay elements** (power-ups, enemies, indicators) should be added as new classes or extensions of the existing manager classes, keeping the `START`/`RUNNING`/`PAUSED`/`GAMEOVER` state handling in `Game` clean and explicit.

## Roadmap

`GEMINI.md` contains a detailed backlog of planned features (power-ups, new obstacle types, weather effects, a coin/skin shop, procedural BGM) with specific implementation notes — check it before designing new features, since it may already describe the intended approach.
