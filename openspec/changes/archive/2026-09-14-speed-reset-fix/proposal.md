## Why

The game starts at inconsistent speeds depending on the path the player takes to begin a new run. Starting from the Game Over overlay (via Space, R, or clicking "ГРАТИ ЗНОВУ") resets speed to `INITIAL_SPEED` (6.5), but going back to the start menu and pressing "ПОЧАТИ ГРУ" resumes at the speed carried over from the previous run — often `MAX_SPEED` (14.5). This makes the game unpredictably fast on some starts and normal on others.

## What Changes

- The `Game.start()` method will reset all per-run game state (`score`, `distance`, `speed`, `lastMilestone`) and reinitialize the panda, obstacles, and particles — the same state that `restart()` already resets — so every run begins from a clean baseline regardless of how the player reached the start screen.
- `Game.goToStart()` will be left as-is (it only manages overlay visibility); the reset responsibility moves into `start()` so it is guaranteed to run before the loop begins.
- No new UI, keys, or capabilities are introduced. No existing gameplay rules change.

## Capabilities

### New Capabilities
- `game-speed-management`: the game's speed must always begin at `INITIAL_SPEED` when a new run starts, regardless of the entry path (fresh load, restart from Game Over, or start from the START menu).

### Modified Capabilities
- (none — no existing spec-level requirement changes; this is a bug fix on an undocumented behavior)

## Impact

- `game.js`: `Game.start()` (around line 2299) — add reset of `score`, `distance`, `speed`, `lastMilestone` and calls to `panda.reset()`, `obstacles.reset()`, `particles.reset()`.
- `game.js`: `Game.restart()` (around line 2310) — can be simplified to call `start()` after resetting score/distance, since `start()` will now do the full reset; kept as a thin wrapper to preserve the existing restart flow.
- No `localStorage` keys, CSS, or HTML changes. No dependency changes.