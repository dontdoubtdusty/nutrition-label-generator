# Session log

Session counter: 2

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
