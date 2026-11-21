# Download-link column (R-3/SC-7/SC-8) evidence — CherokeeLessons#59

Executed 2026-11-21, on the scope extension recorded in the spec (developer disposition
2026-11-21: direct .m4a download links, arrow glyph + "download" word, 3rd table column).

## SC-7 structural checks (executed)

- Built table header: `<thead>SESSION | M4A FILE | DOWNLOAD</thead>` — stale "MP3 FILE" gone.
- `grep -c 'MP3 FILE'` → page 0, process_end_notes.py 0; `.mp3` count → 0 both.
- 9 rows, each third cell rendered in the built page as
  `<a href="animals-NNNN.m4a" download>⤓ download</a>` (page source and generator
  emission carry the same form).

## SC-8 behavioral test (executed, Playwright/Firefox 159)

- Served `public/` via `python3 -m http.server 8322`; page rendered with
  `columnheader "DOWNLOAD"` and 9 `link "⤓ download"` nodes.
- Clicked session 0007's anchor (ref e103) — Playwright events:
  `Downloading file animals-0007.m4a ...` → `Downloaded file animals-0007.m4a to
  ".playwright-cli/animals-0007.m4a"` (browser save, not navigation; page URL unchanged).
- Downloaded file md5 `41eb1c0cdd73910d12dcff83097e6156`, 11,096,477 B — byte-identical
  to the tracked `content/lesson-audio/animal-names/animals-0007.m4a`.

Console note (not this change's defect): giscus comment widget logs
"not installed on this repository" when the page is served from 127.0.0.1 —
the widget resolves its repo from the page origin; it does not fire this way on
the deployed domain's build and predates this change.
