# Vision: Nutrition Facts Label Creator

## Summary
The user gives a list of ingredients (with amounts) or a single food item. The app
looks up nutrition data from predefined sources and prints the result on an HTML
recreation of the standard FDA Nutrition Facts label.

## Decisions made (session 1)
- Single self-contained `index.html`: inline CSS and JS, no build step, no framework.
- Hosting: GitHub Pages. No backend, so all lookups run from the browser.
- Data source: USDA FoodData Central (browser-callable). Open Food Facts is not used for now.
- Input: both modes eventually. Recipe mode (ingredients with amounts plus a servings
  count) is built first; single-item mode is a shortcut over it (a recipe with one line).
- Label: FDA vertical format, rendered as HTML/CSS, light and dark mode, 375px to desktop.
- Version shown in the HTML title and header as "v0.01", bumped by 0.01 per finished feature.

## Build order
1. v0.01 Label renderer from hardcoded sample data (no lookups, no saved data).
2. Lookup: USDA search and nutrient fetch.
3. Recipe mode: parse lines, convert units to grams, sum, divide by servings.
4. Single-item mode shortcut.

## Shared data shape
The label renderer takes one plain object (see `SAMPLE_LABEL` in index.html). This is
the shared shape that lookups and recipe math will produce. Any change to it must be
flagged before it is made.

## Open decisions (ask before building anything that touches these)
- Unit conversion: turning tbsp, cups and pieces into grams needs USDA portion data,
  which is inconsistent. Riskiest part of recipe mode.
- API key handling: the key is visible in page source on a static site. The shared
  DEMO_KEY is heavily rate-limited; a personal free key is better but is public once deployed.
- FDA rounding rules: whether to apply the official rounding for displayed values.
- Daily Values: which set to use (current FDA 2016 set is the assumption).
- Whether lookups are cached, and where (nothing is saved anywhere today).

## Out of scope (for now)
- Accounts, backend, server-side storage.
- Saved recipes or any persisted data. If this is added, it is a data-shape change and must be flagged.
- Non-US label formats.
