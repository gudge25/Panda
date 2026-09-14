# game-speed-management Specification

## Purpose
The game's running speed must always begin at `INITIAL_SPEED` (6.5) at the start of every new run, regardless of how the player reached the start screen (fresh page load, restart from Game Over, or returning from the START menu).

## Requirements

### Requirement: Speed resets on every new run
The game SHALL set `speed = INITIAL_SPEED` when a new run begins, regardless of the previous run's state.

#### Scenario: Starting from START menu
- **WHEN** the player is at the START screen (entered via "ПОЧАТИ ГРУ" or Space while state is `START`)
- **THEN** the game initializes `speed` to `INITIAL_SPEED`, `score` to `0`, `distance` to `0`, and `lastMilestone` to `0` before the game loop begins

#### Scenario: Restarting from Game Over
- **WHEN** the player presses Space/R or clicks "ГРАТИ ЗНОВУ" while in `GAMEOVER` state
- **THEN** the game initializes `speed` to `INITIAL_SPEED`, `score` to `0`, `distance` to `0`, and `lastMilestone` to `0` before the game loop begins

#### Scenario: Fresh page load
- **WHEN** the player opens the page for the first time (state is `START`)
- **THEN** the game initializes `speed` to `INITIAL_SPEED`, `score` to `0`, `distance` to `0`, and `lastMilestone` to `0`

### Requirement: All per-run state resets on start
The game SHALL reset all per-run state — including panda position, velocity, hit state, animation tick, obstacle list, collectible list, particle list, and distance-since-last — at the beginning of every new run.

#### Scenario: State isolation between runs
- **WHEN** a new run begins (from either START or GAMEOVER entry path)
- **THEN** the panda is at its default position on the ground, not ducking, not hit; the obstacle and collectible lists are empty; all particles and floating texts are cleared; and `distSinceLast` is `0`

### Requirement: Game Over overlay and start menu do not carry run state
The game SHALL not display leftover score, distance, or speed values on the START screen after a previous run ended.

#### Scenario: Clean start screen
- **WHEN** the player reaches the START screen after a game over (via "ГОЛОВНЕ МЕНЮ")
- **THEN** no run-specific values are shown (score displays `00000`, HUD reflects zero/initial state) and pressing Space or clicking "ПОЧАТИ ГРУ" starts at `INITIAL_SPEED`
