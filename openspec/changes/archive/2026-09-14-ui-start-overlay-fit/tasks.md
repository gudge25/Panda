## 1. Implementation

- [x] 1.1 Edit `style.css` (`.game-wrapper` rule, around line 35) to change `max-width` from `940px` to `1200px` — verify the canvas container grows from 334px to 426px tall at maximum width

## 2. Verification

- [x] 2.1 Verify the start overlay at 1200px+ width: open the page, confirm the "ПОЧАТИ ГРУ" button and full controls legend are visible without scrolling or a scrollbar
- [x] 2.2 Verify the start overlay at ≤600px width: confirm the game scales to the viewport, the overlay remains readable, and touch controls are available
- [x] 2.3 Verify no regressions: start a game, confirm the canvas renders correctly and the header/footer do not overlap the canvas