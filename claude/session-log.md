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
- User confirmed live USDA searches work.
- Built v0.04: recipe mode (free-text lines, USDA portions for unit conversion, review cards).
  Decisions: flag-and-ask-for-grams on bad units; recipes not saved; nutrient null if any
  counted ingredient lacks it.
- Next: user tries recipe mode live (portion matching is untested against real USDA data);
  then v0.05 packaged foods from Open Food Facts (needs a scope discussion); verify rounding rules.
