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
  The key is never written to the repo.
- v0.03: label values follow FDA rounding (calories, fats, cholesterol, sodium, carbs,
  fiber, sugars, protein, and the vitamin and mineral amounts). Percent Daily Values are
  computed from the unrounded amounts. Nutrients USDA has no data for show a dash with no
  percent instead of a false 0.

- v0.04: Recipe tab. Paste ingredient lines such as `200 g cheddar`, `1 1/2 cups flour`,
  `2 large eggs` or `3 cloves garlic`, set the number of servings, and press Calculate. Each
  line is matched to a USDA food and converted to grams (weights directly; cups, spoons and
  counts through USDA portion data). Every line gets a review card where you can swap the
  USDA match or type grams yourself. Lines that cannot be converted are left out of the
  totals and the label is marked incomplete. A nutrient shows a dash if any counted
  ingredient lacks it. Cooking losses are ignored, and recipes are not saved.

- v0.05: Barcode tab. Type a product's 8 to 14 digit barcode number to get its nutrition from
  Open Food Facts (no key needed). The label uses the package's own serving size and gram
  weight when listed, otherwise 100 g, and you can rescale with the grams box. Open Food Facts
  stores minerals and vitamins in grams; they are converted to mg and mcg. Missing fields show
  a dash. This is community-edited data: check it against the package.

- v0.06: UI/UX overhaul. Pastel gradient page (lime to sky in light mode, deep olive-charcoal in
  dark mode), cream rounded cards, pill buttons and chips, numbered ingredient cards, and the
  embedded Bricolage Grotesque font (Latin subset, base64, no network needed). On phones the
  modes are a dark floating bar at the bottom and the label sits right under the input form,
  with results or review cards below it. On wide screens the tabs are a pill bar and the label
  stays in view on the right. The label itself is unchanged (black on white, square corners).
  No change to behavior, saved data or the label data shape.

- v0.07: Barcode scanning. On the Barcode tab, Camera opens the live camera and reads the barcode
  automatically; Photo reads a barcode from a photo you take or choose. Either one fills the number
  box and runs the lookup. Typing the number still works. Reads EAN-13, EAN-8, UPC-A and UPC-E (UPC-E
  is converted to UPC-A). The decoder is the UPC/EAN part of @zxing/library (MIT), embedded in the
  file, so it works in any browser with no network. Live camera needs HTTPS (GitHub Pages) and the
  camera permission; the camera is turned off when you leave the tab or the page. No saved data.

## Not for regulatory use
This is a hobby tool. USDA averages and a simplified rounding table are not a substitute
for lab analysis or a compliant label. Some rounding rules were taken from a secondary
source and are listed as unverified in `claude/vision.md`.

## Docs
- `claude/vision.md` - design doc
- `claude/file-map.md` - file map
- `claude/session-log.md` - session log
