# Click-through evidence — CherokeeLessons#76 (re-encoded media, browser download)

Executed 2026-11-22 (Playwright/Firefox — the program's recorded evidence class for this test, .issues/59/research/download-column.md; the msedge channel is not installed on this machine). Served `public/` via `python3 -m http.server 8323`; page `content/cherokee-animal-names-tts-demo-audio-1/`.

- Rendered table: `columnheader "SESSION" / "M4A FILE" / "DOWNLOAD"` + `link "⤓ download"` per session row.
- Clicked session 0001's anchor (ref e67). Playwright events:
  - `Downloading file Animals_2021-05-31_0001.m4a ...`
  - `Downloaded file Animals_2021-05-31_0001.m4a to ".playwright-cli/Animals-2021-05-31-0001-1.m4a"` (browser save, not navigation; page URL unchanged).
- md5 of the downloaded file: `5ebe02da8a98e4fbbdae47cafe98ab94` — identical to the working-tree re-encoded deliverable `Animals_2021-05-31_0001.m4a`: `5ebe02da8a98e4fbbdae47cafe98ab94` (byte-identical).

Browser session closed (`-s=s76 close`). The download is the 192k re-encode (source: git-history-recovered mp3 `Animals_2021-05-31_0001.mp3`).
