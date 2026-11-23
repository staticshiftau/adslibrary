# Static Shift Ads Library

The site at https://staticshiftau.github.io/adslibrary/. GitHub Pages serves `main`, so a push to `main` is a release.

## Files

- `index.html` - the library. Reads the "Static Shift Ads Library" Google Sheet (tab `Ads Library`, columns A to AB).
- `frameworks.html` - the Ad Frameworks page. Reads `frameworks.json` for the structures and the same sheet for the ads.
- `organic.html` - the Organic Posts page. Reads tab `Organic Posts` (columns A to Z) of the same sheet. Read-only: no tags or winners. Local preview reads `dev-fixture-organic.json` (gitignored).
- `frameworks.json` - generated. Do not edit by hand. It is built from the video ad bank cards by `scripts/build-frameworks-page.ps1` in the project-management repo, which refuses to write anything that is not meant to be public.

## Things to know

- **Two pages, copied helpers.** `frameworks.html` copies a few helpers from `index.html` (escaping, safe links, Drive file ids, the media markup). A change to how creatives render has to be made in both files.
- **Sheet text is always escaped.** Notes and tags can be written by anyone with the link. Never put a sheet value into the page without `escapeHtml` or `safeUrl`, and never put one inside an `onclick`. Buttons pass the ad's sheet row number and look the ad up.
- **Tags** save through the n8n workflow "Ads Library - update tags", which also finds the ad by sheet row number and writes only the `Interface Tags` cell. The page treats a save as done only when the workflow answers `ok: true`.
- **Confirmed winner** saves through the n8n workflow "Ads Library - update winner". It finds the ad by sheet row number (some ads have no Ad Link and some share one), writes only the `Confirmed Winner` and `Winner Note` cells, and logs every change to the `Winner Log` tab.
- **Structure column.** An ad appears under a framework when its `Structure` cell starts with that framework's two-digit number. Two frameworks are separated by a semicolon.
- **Local preview.** The Sheets API key only works from `staticshiftau.github.io`. On `localhost` both pages read `dev-fixture.json` instead: a dump of the sheet in the Sheets API's `{ "values": [...] }` shape. It is gitignored.
