## Why

On screens ≥940px wide, the start overlay card (controls legend + start button) is clipped by the canvas container — the "ПОЧАТИ ГРУ" button is cut off and the card shows a scrollbar. Players cannot see or reach the start button without manually scrolling the overlay.

## What Changes

- `.game-wrapper` max-width is increased from `940px` to `1200px`.
- This raises the canvas container height from `334px` (940×320/900) to `426px` (1200×320/900), giving the overlay card (~374px) comfortable room without scrolling.
- No JS, HTML, localStorage, or keybind changes. No gameplay rule changes.

## Capabilities

### New Capabilities
- `overlay-display`: the start overlay card SHALL be fully visible within the canvas container without scrolling at the game's maximum width.

### Modified Capabilities
- (none)

## Impact

- `style.css`: `.game-wrapper` rule (around line 35) — increase `max-width` from `940px` to `1200px`.
- `index.html`: no changes.
- `game.js`: no changes.
- No `localStorage` keys, fonts, or dependency changes.