# Nutrition Facts Label Creator

Give it a list of ingredients or a food item and it prints the nutrition information
on an HTML recreation of the standard FDA Nutrition Facts label.

Status: early development. Version is shown in the page title and header.

## Run
Open `index.html` in a browser. No build step, no dependencies.

## Hosting
GitHub Pages, served straight from `main`.

## Features
- v0.01: FDA-style vertical Nutrition Facts label rendered from hardcoded sample data.
  Percent Daily Values are computed from amounts (2,000 calorie diet). Follows the
  system light/dark setting; the label itself stays black on white. Works from 375px up.

- v0.02: single food lookup from USDA FoodData Central (Foundation and SR Legacy
  datasets). Search a food, pick a match, set a serving size in grams, and the label
  rescales. Needs a free USDA API key (https://fdc.nal.usda.gov/api-key-signup), pasted
  once into the key box and saved in this browser's localStorage under `nlc.usdaKey`.
  The key is never written to the repo. USDA nutrients that are missing show as 0.

## Docs
- `claude/vision.md` - design doc
- `claude/file-map.md` - file map
- `claude/session-log.md` - session log
