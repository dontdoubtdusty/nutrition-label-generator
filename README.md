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

- v0.08: Recipe parser fixes. Added amounts of the same kind are summed (`1 Tablespoon + 2 teaspoons
  baking powder`). State words such as hot, melted, sifted, packed and heaping are dropped from the
  USDA search text (`1 cup hot water` searches for "water"); your line is shown as typed.

- v0.09: Remove an ingredient. Each recipe review card has an X button that drops the ingredient from
  the label and also deletes its line from the ingredients box, so pressing Calculate again does not
  bring it back. Works on lines that could not be read, too.

- v0.10: Get unstuck on a bad USDA match. Each recipe review card now has a "Search USDA again" box
  (type your own words, such as `wheat flour`) and a "Show more matches" button that adds the next
  page of results. A search with no results keeps the earlier matches. Lines with no match at all
  get the search box too. Still limited to USDA Foundation and SR Legacy foods.

- v0.11: Serving size units. On the Single food and Barcode tabs the serving size is now an amount box
  plus a unit dropdown: grams, ounces, and (Single food only) the food's own household measures from
  USDA, such as cup, tbsp or slice. The label prints it the FDA way, with grams alongside: `2 oz (57g)`,
  `1/2 cup (120g)`. Switching unit keeps the same weight; switching foods keeps a chosen measure and
  amount when the new food has it. Serving weights on the label are now rounded to whole grams from 5 g
  up. On the Recipe tab an optional serving name gives `1 scone (95g)` instead of `1 serving (95g)`,
  and the note under the label gives each serving's weight in grams and ounces.

- v0.12: Copy image and Copy text buttons under the label. Copy image puts a PNG of the label on the
  clipboard (drawn from the label data, so it works in Safari too); where the browser blocks image
  copying it saves the PNG as a download instead. Copy text puts every value on the clipboard, one name
  line and one value line each (Calories, Serving Size, Carbohydrates, Protein, Fat, Sodium, Fiber, then
  the rest of the label rows). Values are the same rounded ones the label prints; missing data is a
  dash. No saved data or label shape change.

## Not for regulatory use
This is a hobby tool. USDA averages and a simplified rounding table are not a substitute
for lab analysis or a compliant label. Some rounding rules were taken from a secondary
source and are listed as unverified in `claude/vision.md`.

## Docs
- `claude/vision.md` - design doc
- `claude/file-map.md` - file map
- `claude/session-log.md` - session log
