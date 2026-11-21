# SC-6 executed evidence (2026-11-21)

Local serve of the `./build.sh` output (`python3 -m http.server 18923` on `public/`); playwright-cli chromium session `dl68`; one anchor clicked per page via `page.click('a[download]')` with a `waitForEvent('download')` gate:

- `cherokee-tts-update-3--ims-toucan-/index.html` → download event, filename `wolf-and-crawdad-en-360-m.m4a`
- `cherokee-animal-names-tts-demo-audio-1/index.html` → download event, filename `Animals_2021-05-31_0001.m4a`
- `cherokee-animal-names--ims-toucan-tts-/index.html` → download event, filename `animals-0001.m4a`

All three served as browser downloads (save event, not navigation), matching the entry filenames. (Ephemeral scratch evidence class — recorded here, no test files authored.)

## Display-form amendment (developer review feedback, 2026-11-21)

Developer reviewed the preview deploy and named the two list pages' Exercises display broken relative to the styled 3-column table form on `lesson-audio/animal-names`. The list pages originally rendered the exercises as a bulleted list (`<li><p>` with the audio widget inline). The Exercises blocks on `cherokee-animal-names--ims-toucan-tts-/index.md` and `cherokee-animal-names-tts-demo-audio-1/index.md` were converted to the reference table form (`SESSION|M4A FILE|DOWNLOAD`, the lesson-audio template), preserving the anchor counts (9 and 12) and the `.m4a` embeds. Verified: served HTML shows one styled table per page, 0 `.mp3` refs; browser-rendered screenshot matches the reference table's display. tts-update-3's four tables were already in the same form and unchanged.
