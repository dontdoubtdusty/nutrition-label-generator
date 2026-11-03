# File map

- `index.html` - the whole app: inline CSS, inline JS. Open it in a browser to run. Sections in order: CSS (theme tokens, header, label, layout, cards and buttons, tabs, wide-screen rules), a second small style block holding the base64 Bricolage Grotesque font (one very long line; do not edit by hand), page markup, reference data (daily values, row layout, sample label), label logic (formatting, rounding, percent DV, row building), rendering (DOM), USDA lookup, recipe logic (parser, unit conversion, totals), Open Food Facts lookup, page state, page wiring (single food), recipe wiring, barcode wiring, start.
- `README.md` - what the app is, how to run it, feature list.
- `claude/vision.md` - design doc: decisions, build order, open decisions, out of scope.
- `claude/file-map.md` - this file.
- `claude/session-log.md` - session counter and per-session notes.
