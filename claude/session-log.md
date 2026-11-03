# Session log

Session counter: 1

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
- Next: user's live test results; verify the unverified rounding rules; then recipe mode.
