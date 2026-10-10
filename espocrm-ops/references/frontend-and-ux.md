# Custom Client Views, CSS and UX Debugging

## AMD/HTML contract

Check the exact route/view used in browser, `define()` module ID, selectors, `data-role`, `data-action`, `href`, `data-href`, CSS stylesheet and cache versions. Verify which file the browser actually loaded before inferring a CSS regression.

## Computed style beats declarations

CSS can override inline template style through selector specificity and `!important`. Use browser `getComputedStyle`, stylesheet/rule origins, computed `display`, `grid-template-columns`, `align-items`, `align-self`, `height`, `min-height`, `flex`, `overflow`, `position` and `white-space`.

## Visual trap checklist

- `display: grid` with auto rows may leave vertical whitespace across unequal card heights; `align-items: start` removes stretching but not implicit row gaps.
- CSS multi-column layout can reduce vertical gaps but may **reorder visual/reading sequence** (down each column rather than across rows). Check tab order, screen-reader ordering and semantics. Don't use columns solely to improve a screenshot if it harms accessibility.
- KPI `repeat(auto-fill, ...)` may create a lone item on a final row or unused tracks; inspect actual container width.
- An aligned X-coordinate does not mean related values are visually grouped. Check reading path and whitespace at the actual zoom/viewport.
- `white-space: nowrap`, `text-overflow: ellipsis` and shrinking fonts to fit can hide necessary labels. Prefer sensible mobile wrapping.
- Distinguish a full-page screenshot artifact for a fixed/sidebar SPA from a real overlap in normal viewport scrolling.
- Don't accept an attractive static mockup as validation of authenticated layout or JS behavior.

## Evidence to collect

At 1920 / 1440 / 1024 / 768 / 390 / 375 record DOM bounding boxes for each card, computed style origin, screenshots at actual zoom, horizontal scroll width, console/network errors, and link click results. Compare before and after. If the user points to a region in an image, zoom/crop *that exact region* and restate the defect before proposing code.
