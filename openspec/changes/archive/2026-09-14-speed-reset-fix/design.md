## Context

See `proposal.md` for motivation. The `Game` class in `game.js` has two entry paths to a new run: `start()` (used from the START screen and fresh load) and `restart()` (used from GAMEOVER). Currently `restart()` resets `score`, `distance`, `speed`, and calls `panda.reset()` / `obstacles.reset()` / `particles.reset()`, but `start()` does not — it only flips the state and kicks off the loop. As a result, starting from the START menu after a prior run carries over `MAX_SPEED`.

## Goals / Non-Goals

**Goals:**
- Ensure every new run begins at `INITIAL_SPEED` (6.5) with a fully reset state, regardless of entry path.
- Keep `restart()` working unchanged for the Game Over flow (it should still fully reset and start cleanly).

**Non-Goals:**
- No new UI, hotkeys, or power-ups.
- No changes to scoring math or speed progression curve (speed still scales by `INITIAL_SPEED + (score / 100) * 0.45`).
- No `localStorage`, HTML, or CSS changes.

## Decisions

### Decision: Move reset logic into `start()`, keep `restart()` as a thin wrapper

`start()` will perform the full per-run reset that `restart()` currently does. `restart()` will then only reset the small subset that is conceptually "game over → new game" (it already calls `start()`, so the work is consolidated).

```js
start() {
  this.state = 'RUNNING';
  this.score = 0;           // ADDED
  this.distance = 0;        // ADDED
  this.speed = INITIAL_SPEED;  // ADDED
  this.lastMilestone = 0;   // ADDED
  this.panda.reset();       // ADDED
  this.obstacles.reset();   // ADDED
  this.particles.reset();   // ADDED
  this.startOverlay.classList.remove('active');
  this.gameOverOverlay.classList.remove('active');
  this.pauseOverlay.classList.remove('active');
  this.updateScoreDisplay();  // ADDED - refresh HUD to 0
  this.panda.jump(this.sound);
  this.sound.startBgm();
  this.lastTime = performance.now();
  requestAnimationFrame(this.loop.bind(this));
}

restart() {
  // start() now does the full reset; restart() just adds hi-score / milestone UI cleanup
  this.scoreDisplay.classList.remove('score-flash');
  this.newRecordAlert.style.display = 'none';
  this.updateShieldBadge();     // ADDED - clear badge from prior run
  this.updateEatingBadge();     // ADDED - clear eating badge from prior run
  this.start();
}
```

**Why:** `start()` is the single choke point that the loop depends on. Putting the reset here guarantees the invariant for both the START-screen path and `restart()`. `restart()` stays as the Game Over entry but delegates fully to `start()`.

### Decision: Reset badges in `restart()` before `start()`

The shield and eating HUD badges (`eatingBadge`, `shieldBadge`) are not reset anywhere currently. A run that ended with an active shield or eating mode would carry stale HUD state into the next run.

**Why:** Keeps the HUD consistent with the reset game state.

### Alternative considered: Reset in `goToStart()` only

We could reset state inside `goToStart()` (the "ГОЛОВНЕ МЕНЮ" path). However, this would leave `start()` relying on a precondition that a reset already happened — fragile if the code is refactored — and wouldn't protect the fresh-load path or any future entry path. Resetting in `start()` is the single source of truth.

## Risks / Trade-offs

- **[Risk] Double reset** → If `restart()` is later refactored to also reset, the panda/obstacles would be reset twice (harmless, just redundant). **Mitigation:** `restart()` delegates entirely to `start()` — no duplicate reset calls.
- **[Risk] `start()` is also called from `handleJumpPress` while `RUNNING`** → In the current code, `handleJumpPress` only calls `start()` when `state === 'START'`. No risk of calling `start()` mid-run. **Mitigation:** The reset guard (`state === 'START'` check in `handleJumpPress`) is unchanged.
- **[Risk] `updateScoreDisplay()` and badge resets add a tiny perf cost** → Negligible; these are single DOM calls.

## Migration Plan

1. Edit `Game.start()` in `game.js` (around line 2299) to reset `score`, `distance`, `speed`, `lastMilestone`, and call `panda.reset()` / `obstacles.reset()` / `particles.reset()` plus `updateScoreDisplay()`.
2. Edit `Game.restart()` (around line 2310) to remove the now-redundant `score/distance/speed/lastMilestone` resets and badge-clearing, delegating to `start()`.
3. Verify in browser: start a game from the START screen, die (high speed), go to START menu, start again — speed should begin at 6.5.
4. Verify restart (Space/R) from Game Over also starts at 6.5.

No rollback needed — single-file JS change, zero side effects on persisted state.
