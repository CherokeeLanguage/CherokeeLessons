# Spec — itch.io page for Cherokee Numbers Maze: create the page and deploy the published artifacts (itch migration, phase 1)

> **Ownership correction (2026-11-14, developer direction):** the itch page creation, the artifact deployment, and the push tooling are **app-repo work** under the developer's cross-repo filing rule (2026-11-11) — owned by the paired spec CherokeeLanguage/NumbersMaze.godot#3, whose build outputs are the correct deploy sources (the site's untracked copies of the Linux/Windows/mac builds and the `static/HTML-Games/NumbersMaze/` web build that this spec's original channel table anchored are not this repo's deliverable to deploy). This spec's channel table, R-4/R-5/R-6/R-7, and SC-5/SC-6/SC-7 are superseded to that app-repo ownership. This repo's ticket owns only the site surface — the Numbers Maze page's conversion (CherokeeLessons#30) — and its SC-1–SC-4 remain as acceptance criteria for the app-repo-delivered page.
Paired spec: **CherokeeLanguage/NumbersMaze.godot#3** — the NumbersMaze.godot repo's side of this app's migration (its deploy-target conversion). This spec owns the cherokeelessons.com side: the itch page and the initial deployment of the currently-published artifacts.


## Problem

The Cherokee Numbers Maze app is downloadable only through site-hosted binaries and a store link of unknown validity; its playable web build lives only on the site. The migration makes itch.io the app download channel; this app has no itch presence, so its site page cannot point at an itch URL yet. (Disposition of record, remediated 2026-11-14: the site page's conversion is owned by the filed distribution-policy spec CherokeeLessons#30, which rewrites this app's page to the itch targets and removes all store links.)

## Scope

Phase 1 of the two-phase migration, for this app: create its itch.io project page and deploy this repo's currently-published Numbers Maze artifacts — desktop builds and the playable browser build — to it. Site-repo changes are limited to the repeatable push tooling; the site's app page is not modified by this spec (the site-page conversion is owned by CherokeeLessons#30 — see Out of scope).

## Requirements

- **R-1 — itch project page.** A itch.io project is created under the `cherokee-language` account, titled "Cherokee Numbers Maze - ᏣᎳᎩ ᏗᏎᏍᏗ ᏚᎴᎾᎯᎯ" (slug `cherokee-numbers-maze`, page `https://cherokee-language.itch.io/cherokee-numbers-maze`), published, classified Educational, with the site page's featured image as cover art — the page-bundle asset `android-store-tv-banner.png` inside the page's own content directory (no `static/images/` copy exists for it). Project creation is a developer dashboard step — butler has no project-creation path (it pushes to existing projects), matching the Bound Pronouns precedent.
- **R-2 — Pricing mirrors Bound Pronouns.** The page's payment setting is "Name your own price" (free download, donations enabled) — the developer's "download/donate page" direction and the live Bound Pronouns page's configuration.
- **R-3 — Description derives from the site page.** The page's description text is adapted from `content/apps/cherokee-numbers-maze---ᏣᎳᎩ-ᏗᏎᏍᏗ-ᏚᎴᎾᎯᎯ/index.md`'s existing prose, with no factual claim absent from that prose (BoundPronouns#59 is the form precedent). Cover image comes from the page-bundle asset the site page's front matter references.
- **R-4 — Butler deployment of existing artifacts.** The page's download channels carry this repo's currently-published Numbers Maze artifacts per the channel table, pushed with butler. Pushes carry no version claims (no `--userversion`; butler's auto-numbering stands). Artifacts deploy as opaque files under their existing filenames — metadata (tags, filename versions, page dates) is not trusted, so no version label is verified or asserted as truth.
- **R-5 — Repeatable push tooling.** The repo-local push script plus gitignored `itch.conf` (mirroring the BoundPronouns pattern: butler resolution PATH→vendored `.tools/butler/`, `REPLACE_ME` config guard, per-channel pushes) encode this app's channel-table rows. No secrets in the repo — butler credentials live in butler's own store.
- **R-6 — Playable browser build.** The page offers the playable browser build from `static/HTML-Games/NumbersMaze/` (the directory zipped as `NumbersMaze-html.zip`, pushed to an `html` channel, then flagged playable dashboard-side per the butler manual) in addition to the desktop downloads. This is the site's currently-published web version migrating with its app.
- **R-7 — Ordering.** The dashboard project creation precedes the butler pushes; platform tags follow butler's channel-naming rules (`linux`/`windows`/`mac` auto-tag their platforms); the HTML5 playability flag is set from the Edit-game page after the first push, with the page kind set to HTML.

### Channel table

| App | itch project (slug) | Channel | Source artifact (this repo) |
|---|---|---|---|
| Numbers Maze | `cherokee-numbers-maze` | `linux` | `content/apps/cherokee-numbers-maze---ᏣᎳᎩ-ᏗᏎᏍᏗ-ᏚᎴᎾᎯᎯ/NumbersMaze.x86_64` |
| Numbers Maze | `cherokee-numbers-maze` | `windows` | `content/apps/cherokee-numbers-maze---ᏣᎳᎩ-ᏗᏎᏍᏗ-ᏚᎴᎾᎯᎯ/NumbersMaze-x86_64.exe` |
| Numbers Maze | `cherokee-numbers-maze` | `mac` | `content/apps/cherokee-numbers-maze---ᏣᎳᎩ-ᏗᏎᏍᏗ-ᏚᎴᎾᎯᎯ/NumbersMaze-mac.zip` |
| Numbers Maze | `cherokee-numbers-maze` | `html` | `static/HTML-Games/NumbersMaze/`, zipped as `NumbersMaze-html.zip` |

Platform-specific Godot binaries need per-platform channels for correct butler auto-tagging — a single bundled desktop channel was weighed and rejected.

## Success criteria

| ID | Criterion | Class | Instrument | Traces to |
|---|---|---|---|---|
| SC-1 | The itch URL serves the published game page | behavioral | HTTP GET of the URL returns the game page (HTTP 200 + expected game title) | R-1 |
| SC-2 | The page's download section lists its channels' files with filenames matching the channel table's source artifacts, and each displayed file size consistent with its source artifact's size at the page's displayed granularity | behavioral | Page fetch; compare listed filenames against the source files' names, and each displayed size against the source file's byte size rounded the same way the page displays it | R-4 |
| SC-3 | The page's payment setting reads "Name your own price" | behavioral | Page fetch text | R-2 |
| SC-4 | The page's description derives from the site page's prose with no factual claim absent from that prose | content | Side-by-side read of the itch description against the site page source | R-3 |
| SC-5 | The repo-local push script + gitignored `itch.conf` exist, carry the BoundPronouns-pattern guards (REPLACE_ME, butler resolution), and encode this app's channel-table rows | structural | Read the script and config | R-5 |
| SC-6 | The pushes carry no version claims | structural | Script read (`--userversion` absent); butler push output shows auto-numbered builds only | R-4 |
| SC-7 | The page offers the playable browser build | behavioral | Page fetch shows the HTML5 playable embed | R-6 |

Traceability note: R-7 is a procedural requirement whose satisfaction is verified by the work's own record (ordering of steps taken); it carries no standalone SC. The site-pages boundary is carried by this spec's Scope section.

## Approach

Mirror the proven BoundPronouns pattern: the developer creates and configures the itch project dashboard-side (one-time prerequisites: `butler` install + `butler login`, project creation with the R-1/R-2 settings), the agent supplies description text adapted from the site page and executes the butler pushes through the repo-local tooling, then verifies the live page against the SCs.

Alternatives weighed (2026-11-09): CI-driven pushes — rejected, this is a one-time deployment of existing artifacts, and CI enters later at the binary-repo deploy-pipeline conversion; a single bundled desktop channel — rejected, platform-specific Godot binaries need per-platform channels for correct auto-tagging; API-driven project creation — rejected, no documented butler/API path creates projects.

## Site page update — executable site scope (2026-11-14, developer direction)

- **R-S1 — Numbers Maze page update.** `content/apps/cherokee-numbers-maze---ᏣᎳᎩ-ᏗᏎᏍᏗ-ᏚᎴᎾᎯᎯ/index.md` SHALL reference `https://cherokee-language.itch.io/cherokee-numbers-maze` as its distribution target, replacing the self-hosted platform binary links (`NumbersMaze.x86_64`, `NumbersMaze-x86_64.exe`, `NumbersMaze-mac.zip`) and the `/HTML-Games/NumbersMaze/` web-build link; the commented-out wrong store links (Apple id `966667496`, Amazon ASIN `B00TCP955U` — wrong app's identifiers, present in an HTML comment) SHALL be deleted; per the itch-only policy (2026-11-11), ALL store links on the page SHALL be removed.

### Site-update criteria

| ID | Criterion | Class | Instrument | Traces to |
|---|---|---|---|---|
| SC-S1 | The page references the itch project URL | structural | `grep -c "cherokee-language.itch.io/cherokee-numbers-maze" "content/apps/cherokee-numbers-maze---ᏣᎳᎩ-ᏗᏎᏍᏗ-ᏚᎴᎾᎯᎯ/index.md"` ≥ 1 | R-S1 |
| SC-S2 | The page retains no self-hosted binary link, no web-build link, no store links (incl. the live Play link), and no wrong store-link residue | structural | `grep -nE "\.(x86_64|exe|zip)\)|HTML-Games" "content/apps/cherokee-numbers-maze---ᏣᎳᎩ-ᏗᏎᏍᏗ-ᏚᎴᎾᎯᎯ/index.md"` is empty; `grep -cE "play\.google\.com|apps\.apple\.com|amazon\.com" "content/apps/cherokee-numbers-maze---ᏣᎳᎩ-ᏗᏎᏍᏗ-ᏚᎴᎾᎯᎯ/index.md"` = 0; `grep -c "id966667496\|B00TCP955U" "content/apps/cherokee-numbers-maze---ᏣᎳᎩ-ᏗᏎᏍᏗ-ᏚᎴᎾᎯᎯ/index.md"` = 0 (the BoundPronouns page's own correct links keep these strings) | R-S1 |
| SC-S3 | The page edit does not break the build | behavioral | executed `./build.sh` over the edited working tree; exit 0 | R-S1 |

## Out of scope

- The site-page landing conversion of the Numbers Maze page — owned by CherokeeLessons#30 (R-1: itch targets replace the self-hosted binary links, the `/HTML-Games/NumbersMaze/` link, and all store links). Originally filed as "phase 2 — separate spec" with no spec named; remediated 2026-11-14 to point at the filed owner so the deferred work is not dangling. [Superseded same day: the page's executable edit is now this ticket's R-S1 above per the developer's site-update direction; #30 retains only the working-tree binary cleanup for this app.]
- The `CherokeeLanguage/NumbersMaze.godot` repo's deploy-target conversion ticket — filed in that repo.
- Any artifact rebuild — only the existing site-repo copies deploy.

## Impact

One public itch.io page appears under the `cherokee-language` account. No site page content changes; the NumbersMaze.godot repo is untouched; other apps unaffected. Risk is low: new external state only, no destructive operation (a mis-configured page can be edited or deleted dashboard-side; butler pushes to a channel are idempotent and re-pushable).

🤖 Co-authored with AI: OpenCode (huggingface/zai-org/GLM-5.3-Flash)

> Full spec and plan artifacts: https://github.com/CherokeeLanguage/CherokeeLessons.com/tree/issues-data/25/
>
> AI-facing artifacts pointer: `.issues/25/`

