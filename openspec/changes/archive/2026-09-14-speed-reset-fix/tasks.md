## 1. Implementation

- [x] 1.1 Edit `Game.start()` in `game.js` (line ~2299) to reset `score`, `distance`, `speed`, `lastMilestone`, and call `panda.reset()`, `obstacles.reset()`, `particles.reset()` plus `updateScoreDisplay()` before starting the loop — verify by running the app and starting from the START screen that the HUD shows `00000` and speed begins at `INITIAL_SPEED` (6.5)
- [x] 1.2 Edit `Game.restart()` in `game.js` (line ~2310) to remove the now-redundant reset assignments and delegate to `start()` after clearing score-flash and new-record alert — verify by pressing Space/R or clicking "ГРАТИ ЗНОВУ" from GAMEOVER and confirming the game starts at `INITIAL_SPEED`
- [x] 1.3 Clear stale HUD badges in `restart()` before calling `start()` — verify that after a run that ended with an active shield or eating mode, the next run starts with both badges hidden

## 2. Verification

- [x] 2.1 Verify the inconsistent-start bug is fixed: start a game to build speed, die, click "ГОЛОВНЕ МЕНЮ", then press Space/click "ПОЧАТИ ГРУ" — the new run must start at `INITIAL_SPEED` (6.5), not the previous run's speed
- [x] 2.2 Verify normal restart path: from GAMEOVER press Space/R — speed resets to `INITIAL_SPEED`
- [x] 2.3 Verify fresh-load path: reload the page and start — speed starts at `INITIAL_SPEED`
- [x] 2.4 Verify no regressions: jump/duck/pause/mute still work, no console errors in the browser console