# Vision: Nutrition Facts Label Creator

## Summary
The user gives a list of ingredients (with amounts) or a single food item. The app
looks up nutrition data from predefined sources and prints the result on an HTML
recreation of the standard FDA Nutrition Facts label.

## Decisions made (session 1)
- Single self-contained `index.html`: inline CSS and JS, no build step, no framework.
- Hosting: GitHub Pages. No backend, so all lookups run from the browser.
- Data sources: USDA FoodData Central for generic foods and recipes; Open Food Facts for single
  packaged products by barcode only (no name search, not usable as a recipe ingredient).
- Input: both modes eventually. Recipe mode (ingredients with amounts plus a servings
  count) is built first; single-item mode is a shortcut over it (a recipe with one line).
- Label: FDA vertical format, rendered as HTML/CSS, light and dark mode, 375px to desktop.
- Version shown in the HTML title and header as "v0.01", bumped by 0.01 per finished feature.

- API key: each user pastes a free personal USDA key into a settings field; it is stored in
  localStorage as `nlc.usdaKey` and never committed to the repo (the repo is public).
- Single-item lookup was built before recipe mode: it is the smallest slice that proves search,
  nutrient mapping and the label data shape end to end.

## Build order
1. v0.01 Label renderer from hardcoded sample data (no lookups, no saved data). Done.
2. v0.02 Single food lookup: USDA search, pick a match, serving size in grams. Done.
3. v0.04 Recipe mode: parse lines, convert units to grams, sum, divide by servings. Done.
4. v0.05 Packaged foods by barcode from Open Food Facts. Done.
5. v0.06 UI/UX overhaul (also fixes the phone layout: the label now sits under the input form,
   review cards below it). Done.
6. v0.07 Barcode scanning (camera and photo). Done.

## Look and feel (v0.06)
- Reference mood: playful pastel recipe app. Taken: gradient page, big rounded cards, pill chips
  and buttons, chunky type, dark floating bottom nav. Not taken: food photos, ratings, favorites,
  tilted phone mockups (this is a calculator, and there are no image assets).
- The label is never restyled: black on white, square corners, in both themes. The page wraps
  around it (a lilac "stage" card).
- Light: lime-to-sky gradient, cream cards, dark pill primary buttons. Dark: deep olive-charcoal
  gradient with the same lime, lilac and peach accents; lime primary buttons.
- Font: Bricolage Grotesque (SIL OFL), Latin subset of the variable weight font, embedded as
  base64 in its own style block at the end of the head (about 32 KB). Chosen over system fonts
  so the look is identical everywhere and the file still needs no network.
- Phones (under 860px): one column ordered key box, input form, status line, label, results or
  review cards. Modes are a fixed dark bar at the bottom. Wide screens: two columns, tabs as a
  pill bar above the form, label sticky on the right.
- The USDA key box stays a collapsible card in the panel (it must open itself when a search
  needs a key), instead of a header gear icon.
- Icons are inline SVG. No emojis anywhere.

## Rounding and unknown data (v0.03)
- Label data holds actual unrounded values; rounding happens only when drawing.
- A nutrient `amount` (or `calories`) of null means "no data": shown as a dash, no percent.
- Percent Daily Value is computed from the unrounded amount and rounded to a whole percent;
  vitamin D, calcium, iron and potassium use 2% / 5% / 10% steps (the 2018 CFR text).

## Recipe mode (v0.04)
- Input: free-text lines. Parser reads amount (2, 0.5, 1/2, 1 1/2, fraction glyphs), then a unit
  (weight, volume, fl oz, or a counted word such as slice or clove) or a size word (small, medium,
  large), then the name (text before a comma, parentheses removed).
- Conversion: weights are direct. Volumes use the food's USDA portions: same unit first (prefer
  the plain "cup"), otherwise derived from any other volume portion by volume ratio. Counts use
  a size match, else "medium", else "large", else the first non-weight, non-volume portion.
  Anything unconvertible is flagged and left out; the person can type grams.
- Totals: nutrients are summed over counted lines and divided by servings. If any counted line
  lacks a nutrient, that nutrient is null (dash), never a partial sum.
- Each recipe label is a normal label: servingsPerContainer = servings, servingSize "1 serving".
- Recipes are not saved. Reloading the page loses them.

## Barcode mode (v0.05)
- Endpoint: https://world.openfoodfacts.org/api/v2/product/{barcode} with a fields list. No key.
- Mapping: per-100g fields (mass nutrients in grams, energy in kcal) converted to label units:
  cholesterol, sodium, calcium, iron and potassium times 1000 (mg); vitamin D times 1,000,000
  (mcg). Energy falls back to kJ / 4.184. Missing, empty or negative values are null (dash).
- Serving: the package serving_quantity (grams, at least 1) is the default grams; otherwise 100.
  The package serving text (parentheses removed, hidden if it is only grams) is shown only
  while the grams still equal the package serving.
- Barcode input: 8 to 14 digits after removing spaces and hyphens. Typing is always available.
- The label data shape is unchanged. Nothing new is saved.
- The USDA key box is hidden on the barcode tab.

## Barcode scanning (v0.07)
- Reverses the v0.05 "no camera scanning" decision, at the user's request.
- Decoder: the UPC/EAN readers from @zxing/library 0.21.3 (MIT), bundled with esbuild once outside the
  repo and pasted in as a third script block (about 60 KB). Chosen over the native BarcodeDetector
  (missing in iPhone Safari, Firefox, and Windows/Linux Chrome) and over a hand-written decoder.
  There is no build step in the project; to update the decoder, rebuild the bundle and replace the block.
- Formats: EAN-13, EAN-8, UPC-A, UPC-E (converted to UPC-A for lookup). Other types are ignored.
- Live camera: rear camera preferred, one frame checked every 150 ms at native video size. A hit stops
  the camera, fills the number box and submits the existing barcode form (so lookup logic is unchanged).
- Photo: any image; scaled so its long side is 1400 px (small photos are enlarged, which helps), then decoded.
- The camera is released on Stop, on leaving the Barcode tab, when the page is hidden and on page hide.
- Errors (blocked, no camera, busy, no HTTPS) show a short message pointing to Photo or typing.
- Nothing is saved; no change to the label data shape.

## Saved data
- `nlc.usdaKey` in localStorage: the user's USDA API key (string). Nothing else is saved.

## Shared data shape
The label renderer takes one plain object (see `SAMPLE_LABEL` in index.html). This is
the shared shape that lookups and recipe math produce. Any change to it must be flagged
before it is made.
- v0.02 change (approved): `servingsPerContainer` is optional (null hides the line).
- v0.02 convention on an existing field: `servingSize` as an empty string means "no
  household measure", and the label shows grams only.
- v0.03 change (approved): a nutrient `amount` may be null (no data). Also, `calories` may be
  null, and label data now holds unrounded values (rounding moved to display).

## Open decisions (ask before building anything that touches these)
- Barcode scanning on real devices: tested only with Chrome's fake camera, generated barcode images
  and mocked lookups. Real phones (focus, glare, curved packaging, iPhone Safari camera behavior) are
  untested. Torch and zoom controls are not built.
- Unit conversion: turning tbsp, cups and pieces into grams needs USDA portion data,
  which is inconsistent. Riskiest part of recipe mode.
- API key handling: the key is visible in page source on a static site. The shared
  DEMO_KEY is heavily rate-limited; a personal free key is better but is public once deployed.
- Unverified rounding rules: the amount increments for vitamin D (0.1mcg), calcium (10mg), iron
  (0.1mg) and potassium (10mg), and the cholesterol "less than 5mg" display, come from a secondary
  source ( https://www.recipal.com/blog/labeling-rules-and-guidelines/fda-us-nutrition-label-rounding-rules ).
  The 2% / 5% / 10% vitamin and mineral percent steps come from the 2018 CFR text and may have been
  changed by the 2016 labeling rule. Confirm against the current 21 CFR 101.9 before trusting them.
- Cooking losses and yield: recipe totals describe raw ingredients; cooked weight, water loss and
  fat loss are ignored. A cooked-weight serving size would need a yield input.
- Portion matching was built from the USDA data dictionary and mocked data, not from live
  responses; real portion text (for example "cup, chopped" versus Foundation unit names) may
  need tuning once tried on real recipes.
- Open Food Facts from a browser: the documentation I could read did not mention CORS or
  User-Agent rules, and field names and units come from its docs and mocked data, not a live
  response. Confirm with one real barcode on the deployed page.
- Open Food Facts serving_quantity is treated as grams even for liquids (it may be millilitres).
- Ranges ("2-3 cups"), "to taste" lines and multi-ingredient lines are not parsed.
- Missing nutrients show a dash. Whether to estimate or hide those rows instead is undecided.
- Daily Values: which set to use (current FDA 2016 set is the assumption).
- Whether lookups are cached, and where (nothing is saved anywhere today).

## Out of scope (for now)
- Accounts, backend, server-side storage.
- Saved recipes or any persisted data. If this is added, it is a data-shape change and must be flagged.
- Non-US label formats.
