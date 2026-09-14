## Context

See `proposal.md` for motivation. The game's `.game-wrapper` is capped at `940px`, which makes the canvas container (aspect-ratio `900/320`) only `334px` tall. The start overlay card (~374px) overflows this height, clipping the "ПОЧАТИ ГРУ" button and forcing a scrollbar.

## Goals / Non-Goals

**Goals:**
- Make the start overlay card fully visible without scrolling at the game's maximum desktop width.
- Keep the existing responsive behavior intact on tablet and mobile.

**Non-Goals:**
- No gameplay, input, or state-machine changes.
- No HTML or JavaScript changes.
- No changes to the overlay card's internal spacing or content.

## Decisions

### Decision: Increase `.game-wrapper` max-width from 940px to 1200px

The canvas container uses `aspect-ratio: 900 / 320`. Raising the wrapper's max-width from `940px` to `1200px` increases the canvas height from `334px` to `426px`, comfortably exceeding the overlay card's ~374px content height.

**Why:** This is a single CSS property change that preserves the existing aspect ratio and responsive scaling. It addresses the root cause (insufficient canvas height) rather than masking it with smaller text or tighter padding.

**Alternative considered:** Reduce the overlay card's padding or content spacing. This would make the card fit at 940px but would also reduce readability and would not address the underlying layout constraint. Rejected.

**Alternative considered:** Increase the canvas container's height directly. This would distort the game's fixed aspect ratio and could misalign drawing coordinates. Rejected.

### Decision: Keep the existing responsive breakpoints

The existing `@media (max-width: 600px)` rules continue to handle small screens. The `1200px` max-width is a desktop-only ceiling, so tablet and mobile behavior is unchanged.

**Why:** The bug only appears at desktop widths. Touch controls, font scaling, and overlay behavior on smaller screens are already covered by the current media queries.

## Risks / Trade-offs

- **[Risk] On very wide screens, the game scales up to 1200px and may feel large** → This is the intended behavior; the canvas remains within typical desktop viewport widths and the aspect ratio is preserved.
- **[Risk] On narrow screens, the card may still need scrolling if the viewport is very short** → The spec explicitly allows scrolling only when viewport height is insufficient; this is a device limitation, not a layout defect.
- **[Risk] The larger canvas may slightly change perceived game speed** → None. The canvas is scaled via CSS; the internal 900×320 drawing surface and game loop are unchanged.

## Migration Plan

1. Edit `style.css` (`.game-wrapper` rule, around line 35): change `max-width: 940px;` to `max-width: 1200px;`.
2. Verify in browser at 1200px+ width: the start overlay card fits fully, the "ПОЧАТИ ГРУ" button is visible, and no scrollbar appears.
3. Verify at ≤600px width: the game still scales correctly and touch controls remain available.
4. No rollback needed — a single CSS property change with no persisted state or API impact.
