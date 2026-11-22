# AGENTS.md — CherokeeLessons

Static site (Zola) for CherokeeLessons.com — content and media, no application
code. Agent-behavior governance lives in the `.opencode/` deck (floor.md);
this file holds repo operational facts only.

## Build & verification

- `./build.sh` is the only sanctioned build path: it fetches the zola release
  pinned in `ZOLA_VERSION` on first use (needs network once; cached as
  `./zola`, gitignored) and builds into `public/` (gitignored). Do not use a
  system zola — the pin was chosen for vendored-Zulma compatibility (README).
- There is no test suite and no CI. A clean `./build.sh` (exit 0) is the
  verification gate for content and config changes.
- **Iframe-embed CSP gate (evergreen — every new page).** `static/_headers`
  carries the site CSP, and `build.sh` cannot catch its runtime effect: an
  iframe whose origin is absent from the `_headers` `frame-src` list is
  silently blocked at runtime — it renders as a blank box in the served page
  (seen on the #22 itch widget; the console reports "violates the following
  Content-Security-Policy directive: frame-src"). When adding any new blog or
  content entry that embeds an iframe — itch.io widgets, video/comment
  embeds, anything frame-based: (1) check the embed origin against
  `static/_headers`' `frame-src` and add the origin there if absent; (2)
  verify the served page by browser-rendering a preview (deploy or local
  serve) and checking the console for CSP frame violations — a clean
  `./build.sh` proves nothing about the frame. The #22 itch-widget embed
  establishes the pattern (widget iframe `itch.io/embed/…`, origin
  `https://itch.io` in frame-src).
- Media tooling: media (ffmpeg/ffprobe) work uses the static 7.0.2 builds
  vendored at `.tools/` (fetch source: johnvansickle.com
  `ffmpeg-release-amd64-static.tar.xz`). The snap-packaged `/snap/bin/ffmpeg`
  is broken in this environment (snap-confine capability failure) and must
  not be used. Always pass `-nostdin` to ffmpeg — non-interactive shells can
  trigger a stdin-consuming encode hang (observed 2026-11-21, #67: one encode
  spun CPU-bound for hours without `-nostdin`). Example transcode form used
  by the repo's audio conversions:

  ```
  .tools/ffmpeg -y -nostdin -loglevel error -i IN.mp3 -ac 1 -ar 48000 \
    -c:a aac -b:a 128k -movflags +faststart OUT.m4a
  .tools/ffprobe -v error -select_streams a:0 \
    -show_entries stream=codec_name,channels,sample_rate,bit_rate \
    -show_entries format=format_name -of default=noprint_wrappers=1 OUT.m4a
  ```

- Deployment is Cloudflare Workers Static Assets: a merge to `master` triggers
  a Workers Builds build and `npx wrangler deploy` automatically; PR branches
  get preview builds. The one-time dashboard setup (GitHub App connection,
  Worker name, build command, production branch, custom domains, R2 bucket)
  is documented in README's Deployment section.

## Content facts

- `templates/shortcodes/` ships `{{ video(video=…) }}`, `{{ audio(file=…) }}`,
  and `{{ youtube(id=…) }}`. Legacy `video` embeds reference colocated mp4s
  that are untracked, so those embeds are dead in any fresh build; `audio`
  embeds are mixed — pages whose referenced mp3s are tracked stay live (122
  tracked mp3s across four page dirs), while several lesson-audio and TTS
  pages embed untracked mp3s and are dead in fresh builds.
- Lesson media dispositions of record (developer decisions 2026-11-15/16,
  landing via CherokeeLessons#54):
  the Syllabary-Dictation videos embed via `{{ youtube(id=…) }}` (channel
  mirror verified); the two lesson-audio pages keep their media local —
  tracked AAC `.m4a` files re-encoded per track through the bitrate ladder
  (owning definition: audio-lessons-generator-python#16); the TTS demo audio
  ships as tracked mp3s per CherokeeLessons#30's R-2. Lesson media remains
  mirrored on the YouTube channel (mirror provenance, not the delivery method
  for the audio pages); a local-media embed ships only if its media file is
  tracked (see the ignore rules below — `*.m4a` is not ignored;
  `*.mp3`/`*.mp4` are).
- Content filenames use Cherokee syllabary and Osage script (e.g.
  `content/books/ꮳꮃꭹ-…-1.md`). Use `git -c core.quotepath=off` for readable
  git output and quote paths in shell commands.
- `.gitignore` globally ignores `*.mp3`, `*.mp4`, and `/static/HTML-Games/`.
  Media is nevertheless tracked where it matters (122 tracked mp3s — tracked
  files override ignore rules), and
  `content/content/tacotron2-and-cherokee-tts/.gitignore` re-includes new
  mp3s with `!*.mp3`. A plain `git add` of ignored media is a silent no-op —
  confirm with `git status` / `git check-ignore` after adding media.
- Scaffold new content pages with `content/new_page.py --title "…"
  --folder <section-dir>`. It is a bash/python polyglot that activates the
  `cherokee-lessons` conda env; `environment.yml` is that env's export
  (refreshed by `export-env.sh`).
- Audio-exercise display form of record (CherokeeLessons#68 SC-10, developer
  ruling 2026-11-21): audio-exercise listings on TTS-demo and lesson-audio
  pages present as the 3-column `SESSION|M4A FILE|DOWNLOAD` table (the
  `content/lesson-audio/animal-names/index.md` form), never a bulleted
  list; each row's third cell carries the `⤓ download` anchor.

## SoP — content and distribution decisions are the developer's

- Distribution and content-disposition decisions (where downloadable
  artifacts live, which channel hosts what, whether a download link
  exists) are the developer's product decisions. The agent surfaces the
  choice points for discussion BEFORE any spec carries the disposition;
  an agent-derived disposition entered into a spec without prior
  developer discussion is invalid work and is rejected on discovery —
  regardless of how thoroughly the spec was validated after the fact.
- Ruling of record (2026-11-12): the R2 delivery plan for the oversized
  book PDFs (spec #31 R-8) was never discussed with the developer and is
  rejected — R2 is not a valid delivery path for this repo's work and
  must not be re-proposed. Platform constraints (the Workers 25 MiB/file
  asset limit) narrow the option space; they never license a silent
  default.
- Ruling of record (2026-11-16): lesson-media channel dispositions are
  per-page decisions, not a blanket channel assignment. The recorded method
  of record (2026-11-15/16, via CherokeeLessons#54 and
  audio-lessons-generator-python#16): dictation videos → YouTube embeds; the
  two lesson-audio pages → local tracked `.m4a` re-encoded per track through
  the bitrate ladder; TTS demo audio → tracked mp3s. An agent
  spec/prose that assumes blanket "lesson media → YouTube" for this repo is
  wrong on discovery — record the per-page split.
- Ruling of record (2026-11-21, via CherokeeLessons#68): audio-exercise
  listings on TTS-demo and lesson-audio pages display as the 3-column
  `SESSION|M4A FILE|DOWNLOAD` table (the
  `content/lesson-audio/animal-names/index.md` form of record), never as a
  bulleted list; each row's third cell carries the `⤓ download` anchor.

## Workspace artifacts — never commit

- `.opencode/` is a gitignored, read-only deck checkout.
- `.issues/` is a linked git worktree on the orphan `issues-data` branch (the
  local issue store) and `.tools/` holds vendored agent tooling — both are
  untracked machinery. Never `git add -A` / commit them, and `issues-data` is
  never a branch-management or cleanup candidate.
