# Download-link column (SC-7/SC-8) evidence — CherokeeLessons#58

Executed 2026-11-22, per the plan's Item 4 (the #59 download-column evidence pattern;
developer disposition on CherokeeLessons#59, 2026-11-21 — direct `.m4a` anchors,
arrow glyph + "download" word, 3rd table column).

## SC-7 structural checks (executed on the built page)

- `public/lesson-audio/cherokee-language-lessons-1---audio-exercises/index.html`
  contains 23 `download>` anchor occurrences; `M4A FILE` header present.
- Playwright page snapshot: 23 `link "⤓ download"` nodes under the session table.

## SC-8 behavioral test (executed, Playwright/Chromium)

- Served `public/` via `python3 -m http.server 8323 --directory public`;
  page rendered at `http://127.0.0.1:8323/lesson-audio/cherokee-language-lessons-1---audio-exercises/`.
- Clicked session 0001's anchor (ref e72) — Playwright events:
  `Downloading file cll1-v3-0001.m4a ...` → `Downloaded file cll1-v3-0001.m4a to
  ".playwright-cli/cll1-v3-0001.m4a"` (browser save, not navigation; page URL
  unchanged after the click).
- Downloaded file 6,003,030 B, md5 `bb6fb513a030644cd63829abbc019321` — byte-identical
  to the tracked `content/lesson-audio/cherokee-language-lessons-1---audio-exercises/cll1-v3-0001.m4a`.
