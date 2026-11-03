# Session log

Session counter: 1

## Session 1 (2026-11-03)
- Repo created and attached: dontdoubtdusty/nutrition-label-generator.
- Decisions: USDA FoodData Central; recipe mode first then single-item; renderer first.
- Built: docs scaffold, then v0.01 label renderer.
- Decided: free personal USDA key in a localStorage settings field; single food lookup next.
- Built v0.02: USDA single food lookup with mocked-API smoke tests (live USDA call not yet
  confirmed with a real key).
- Next: confirm one live search with a real key; then decide FDA rounding and missing-nutrient
  display; then recipe mode.
