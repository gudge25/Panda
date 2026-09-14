# overlay-display Specification

## Purpose
The start overlay card SHALL remain fully visible within the canvas container without scrolling at the game's maximum desktop width, so players can always see and activate the start button.

## Requirements

### Requirement: Start overlay fits without scrolling
The start overlay card SHALL be fully visible within the canvas container at the game's maximum width, with no vertical scrollbar required to reach the "ПОЧАТИ ГРУ" button.

#### Scenario: Start overlay at maximum width
- **WHEN** the game is displayed at a viewport width of 1200px or greater
- **THEN** the start overlay card, including its full controls legend and the "ПОЧАТИ ГРУ" button, is visible without vertical scrolling

#### Scenario: Start overlay at smaller widths
- **WHEN** the game is displayed at a viewport width below 1200px
- **THEN** the start overlay card remains centered and readable, with the start button reachable by scrolling only if the viewport height is too short to fit the full card

### Requirement: Game layout remains responsive
The game layout SHALL preserve its responsive behavior across desktop, tablet, and mobile viewports after the overlay sizing change.

#### Scenario: Mobile viewport
- **WHEN** the game is displayed at a viewport width of 600px or less
- **THEN** the game scales to the viewport width, the overlay card remains readable, and touch controls are available

#### Scenario: Desktop viewport
- **WHEN** the game is displayed at a viewport width of 1200px or greater
- **THEN** the game uses the larger canvas size, the overlay card fits within it, and the header and footer remain visible without overlap
