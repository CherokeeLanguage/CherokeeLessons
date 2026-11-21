# SC-6 executed evidence (2026-11-21)

Local serve of the `./build.sh` output (`python3 -m http.server 18923` on `public/`); playwright-cli chromium session `dl68`; one anchor clicked per page via `page.click('a[download]')` with a `waitForEvent('download')` gate:

- `cherokee-tts-update-3--ims-toucan-/index.html` → download event, filename `wolf-and-crawdad-en-360-m.m4a`
- `cherokee-animal-names-tts-demo-audio-1/index.html` → download event, filename `Animals_2021-05-31_0001.m4a`
- `cherokee-animal-names--ims-toucan-tts-/index.html` → download event, filename `animals-0001.m4a`

All three served as browser downloads (save event, not navigation), matching the entry filenames. (Ephemeral scratch evidence class — recorded here, no test files authored.)
