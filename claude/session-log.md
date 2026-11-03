# Session log

Session counter: 4

## Session 1 (2026-11-03)
- Repo created and attached: dontdoubtdusty/nutrition-label-generator.
- Decisions: USDA FoodData Central; recipe mode first then single-item; renderer first.
- Built: docs scaffold, then v0.01 label renderer.
- Decided: free personal USDA key in a localStorage settings field; single food lookup next.
- Built v0.02: USDA single food lookup with mocked-API smoke tests (live USDA call not yet
  confirmed with a real key).
- User loaded a real key; live results not reported back yet.
- Built v0.03: FDA rounding at display, null amounts shown as dashes, vitamin percent steps.
  Approved shape change: amount and calories may be null.
- User confirmed live USDA searches work.
- Built v0.04: recipe mode (free-text lines, USDA portions for unit conversion, review cards).
  Decisions: flag-and-ask-for-grams on bad units; recipes not saved; nutrient null if any
  counted ingredient lacks it.
- Built v0.05: Barcode tab (Open Food Facts, barcode only). Decisions: third tab; package serving
  size as default grams; no camera scanning; no shape change.
- Next: user tries a real barcode on the deployed page (CORS unconfirmed); then (earlier note)
  user tries recipe mode live (portion matching is untested against real USDA data);
  then v0.05 packaged foods from Open Food Facts (needs a scope discussion); verify rounding rules.

## Session 2 (2026-11-03)
- Built v0.06: UI/UX overhaul from a pastel recipe-app reference. CSS and markup only; no JS,
  saved-data or label-shape changes. Decisions: embed Bricolage Grotesque as base64; dark floating
  bottom nav on phones; label first, review cards below; deep olive-charcoal dark mode.
- Tested with mocked USDA and Open Food Facts at 375px and 1280px, light and dark: no console errors,
  no horizontal overflow. Not tested on a real phone.
- Built v0.07: barcode scanning (live camera and photo), requested mid-session. Reverses the v0.05
  "no camera scanning" decision. Decision: inline the UPC/EAN part of @zxing/library (about 60 KB)
  instead of native BarcodeDetector (not on iPhone Safari) or a hand-written decoder. No shape change.
- Tested with generated barcode images and Chrome's fake camera feed (32 checks), plus the earlier
  smoke test. Not tested on a real phone.
- Next: user tries scanning on a real phone on the deployed page; then the earlier open items
  (real barcode lookup CORS, recipe mode on real USDA data, rounding rules).

## Session 3 (2026-11-03)
- Trigger: a real recipe (cream scones, 11 lines) failed in recipe mode. Asked for: remove an
  ingredient, a way out of bad USDA matches, and oz or household serving sizes.
- Built v0.08: "1 Tablespoon + 2 teaspoons" sums added amounts of the same kind; state words (hot,
  melted, sifted, packed...) are dropped from the USDA search text only.
- Built v0.09: remove button per ingredient card; also deletes the line from the text box.
- Built v0.10: per-card "Search USDA again" box and "Show more matches" (same two USDA datasets).
- Built v0.11: serving size is amount plus unit (g, oz, USDA household measures on Single food; g and oz on
  Barcode); label shows e.g. "2 oz (57g)". Recipe gets an optional serving name and a g/oz readout.
  Label serving grams now rounded (whole from 5 g up). Decisions: Branded Foods not added; household
  measures fetched in the background on selection (one extra USDA call per selected food).
- Project instructions changed mid-session: one file is no longer required. Nothing was split yet.
- No saved-data or label-shape change in any of the four versions.
- Tested with mocked USDA and Open Food Facts in headless Chrome at 375px and 1280px, light and dark:
  no console errors, no horizontal overflow. Not tested against live USDA (sandbox cannot reach it) or on a phone.
- Next: user tries the scone recipe live (portion text such as "cup, packed" for raisins and powdered
  sugar is the likely weak spot); then the earlier open items (barcode CORS, phone scanning, rounding rules).

## Session 4 (2026-11-03)
- Built v0.12: Copy image and Copy text buttons under the label. Decisions: text uses the label's rounded
  values (not unrounded 2 decimals); every label row in the text; image drawn on a canvas, not a DOM snapshot.
  Added `servingText(label)` so the label, text and image print the serving size identically.
- No saved-data or label-shape change.
- Tested in headless Chrome over HTTP at 375px and 1280px, light and dark: text and PNG on the clipboard,
  image size matches the on-screen label, download fallback, long serving name wraps, no app console errors
  (the only 404 was the test server's missing favicon). Not tested on a real phone or in real Safari.
- Next: user tries both buttons on the deployed page, especially pasting the image on iPhone Safari;
  then the earlier open items (barcode CORS, phone scanning, recipe mode on real USDA data, rounding rules).
- Built v0.13: Copy text puts name and value on one line ("Calories 190") instead of two. Tests updated and rerun.
