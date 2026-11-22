# SC-6 download click-through evidence — CherokeeLessons#67 (2026-11-21)

Local serve of `public/` (python http.server; the built tree of SC-8's executed `./build.sh` run). chromium (playwright-cli vendored tooling). One anchor clicked per page; per-page console inspected — no CSP frame-src violations; the only console noise is the pre-existing giscus "discussion not found" 404 on pages with no comment thread (present before this change).

| page | anchor clicked | download event | sha256 served == downloaded |
|---|---|---|---|
| espeak-ng-and-ced-phonetics | first ⤓ download (ee3gaa_) | `ee3gaa_.m4a` | match |
| tacotron2-and-cherokee-tts | first ⤓ download | `bragging-hunter-cno-spk_0-01.m4a` | match |
| here-is-why-… | first ⤓ download (ạ²ma_) | `ạ²ma_.m4a` | match |
| cherokee-tts-update--2--ims-toucan- | first ⤓ download | `_bragging-hunter-ref-de-41.m4a` | match |

All four were save-to-disk download events (browser download pipeline, not navigation), filenames matching the clicked entries' `.m4a` files.
