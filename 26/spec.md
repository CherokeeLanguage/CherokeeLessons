# Spec — standing app-announcement workflow: itch.io promotion format for Mastodon announcements (all apps going forward)

## 1. Intent and Executive Summary

**Problem Statement.** The itch migration made itch.io the app download channel, but the workflow carries no announcement step: nothing records how app announcements are posted on Mastodon. itch.io's own guidance defines the format that lets the @itchio account find and boost promotion posts about itch projects — a toot about an itch.io project includes a direct itch.io link, some image/video, and a mention of @itchio ([itchio@mastodon.gamedev.place, 2022-11-07](https://mastodon.gamedev.place/@itchio/109304068236703627), text fetched and verified 2026-11-10: "Please make toots for your itch.io projects: include a direct itch.io link, some image/video, and mention @itchio so we can find your posts to boost!").

**Root Cause / Motivation.** The workflow has no announcement step anywhere — no announcement CI step in the app repos, no social links in `config.toml`, no announcement content in the issue store (observed 2026-11-10); the itch migration's phase-1 umbrella (#21) and per-app specs (#20, #22–#25) govern page creation and deployment only. Announcements going forward would not follow the itchio format, and as of 2026-11-24 the hashtag element of an announcement carries no bound selection basis at all.

**Approach Chosen.** One new documentation file, `docs/app-announcement-workflow.md` — repo content, versioned, PR-governed, readable by any future session implementing an announcement — defining the announcement step and its format for all apps going forward, including the 2026-11-24 hashtag-selection basis (live tag-usage trends on the Native American–specific Mastodon server, `turtleisland.social`).

**Alternatives Considered & Why Discarded.** Amending umbrella #21's shared-conventions seam — rejected: that umbrella is phase-1-specific and closes when its parts close. A store-level convention record — rejected: issue-store data is not repo content that future implementation work executes against (weighed 2026-11-10).

**Key Design Decisions.** The doc records the announcement content format only; toot-posting mechanics (poster account, instance, trigger, cadence) stay out of scope until the developer binds them (2026-11-10, "not applicable right now"). The hashtag-selection basis is bound to the Native American–specific Mastodon server identified live on 2026-11-24 (the formerly-used `indigenous-mastodon.social` domain does not resolve); per-tag usage stats come from that server's own Mastodon API at announcement-drafting time.

**User Intent / Original Prompt.** Verbatim, dated: "add this in as a spec to add this into the workflow for all apps going forwards" (2026-11-10, chat) — with the itchio toot link above as the format source; "not applicable right now" (2026-11-10) declining to bind the poster account, trigger, and retroactivity; "file a draft spec" (2026-11-10) authorizing the filing. 2026-11-24, chat: "don't forget all the needed hash tags based on live tag usage trends"; "focus on the indigenous mastodon site for the hash tag analysis"; "search for the native american specific mastodon server"; "make a note of it in AGENTS.md as an added SC in the spec"; "revise spec and implement". Live-verified 2026-11-24: `turtleisland.social` is the Native American–specific Mastodon server (JoinMastodon directory, bipoc category: "Native owned and operated for Native/Indigenous people, BIPOC people and true Allies only"; instance API: 110 users, 102,252 posts), with 7-day local tag usage led by #gamedev (364 uses/190 accounts), #indiedev (154/85), #indigenous (95/64), #languages (35/34), #languagelearning (12/12), #nativeamerican (11/11), #itchio (10/9).

## 2. Not Included

- The poster account and its Mastodon instance — declined 2026-11-10 ("not applicable right now"); the doc records the format only.
- Announcement trigger and cadence (initial launch, per-release push, or both) — not yet bound.
- Retroactive announcements for already-live apps (Bound Pronouns, Animals — the two pages with verified live state at filing) — awaiting the developer's binding.
- The site-page specs (CherokeeLessons#30 — the four itch-backed app pages' distribution policy; CherokeeLessons#32 — the CLL-1 page's itch link) and the app repos' pipelines — owned by their own issues.
- The itchio account's actual boost behavior — the doc cites the toot's guidance; it does not assert outcomes.

## 3. Requirements

- **R-1.** The repo SHALL gain `docs/app-announcement-workflow.md`, defining the standing announcement step for app announcements and stating that it applies to all apps going forward.
- **R-2.** Every app announcement post SHALL include all three mandatory elements: (1) a direct itch.io page link — `https://cherokee-language.itch.io/<slug>`; (2) at least one image or video; (3) a mention of @itchio — canonical handle `https://mastodon.gamedev.place/@itchio` (full form `@itchio@mastodon.gamedev.place` when the post originates from another instance).
- **R-3.** The doc SHALL state only what the linked itchio toot (2022-11-07), the developer's 2026-11-10 direction, and the developer's 2026-11-24 hashtag directive carry: no claims about boost guarantees or likelihood, no account identities beyond the canonical handle, no invented cadence, trigger, or posting ownership.
- **R-4.** Every announcement post SHALL carry hashtags, selected from live tag-usage trends on the Native American–specific Mastodon server `turtleisland.social` (the formerly-used `indigenous-mastodon.social` domain does not resolve).

## 4. Success criteria

| ID | Criterion | Class | Instrument |
|---|---|---|---|
| SC-1 | `docs/app-announcement-workflow.md` exists, states its applicability to all apps going forward, and names all three mandatory elements | structural | Read the file; check the three elements and the applicability statement are present |
| SC-2 | The doc binds the canonical @itchio handle URL and the itch.io page URL pattern, matching R-2's bound values exactly | structural | Read the file against R-2's bound values |
| SC-3 | The doc carries no factual claim beyond the linked itchio toot and the developer's directions (2026-11-10; 2026-11-24 hashtag directive) | structural | Side-by-side read against the toot text recorded in §1 and against R-4's bound values |
| SC-4 | The doc mandates hashtags on every announcement post and binds the hashtag-selection basis — live tag-usage trends on the Native American–specific Mastodon server `turtleisland.social` (the former `indigenous-mastodon.social` domain is dead) — matching R-4's bound values exactly | structural | Read the file against R-4's bound values |

## 5. Dependencies

| Reference | Relationship | Status |
|---|---|---|
| @itchio boost-guidance toot | The doc's format mandate derives from and cites its text | Satisfied (fetched and verified 2026-11-10; recorded in §1) |
| Five live itch.io app pages (`cherokee-language.itch.io/<slug>`) | Announced app pages must exist and be live for posts to carry valid links | Satisfied (all five HTTP 200, verified 2026-11-24) |
| `turtleisland.social` tag-usage statistics (`/api/v1/tags/<name>`) | R-4's selection basis needs per-tag usage stats from the bound server | Satisfied (live calls 2026-11-24; §1 records the figures) |

## 6. Traceability

| Requirement | Success criteria |
|---|---|
| R-1 | SC-1 |
| R-2 | SC-1, SC-2 |
| R-3 | SC-3 |
| R-4 | SC-4 |

## 7. Documentation Sources

| Source | Type | Location | Verification |
|---|---|---|---|
| @itchio boost-guidance toot | social post | https://mastodon.gamedev.place/@itchio/109304068236703627 | Text fetched and recorded verbatim 2026-11-10 (§1) |
| `turtleisland.social` instance stats | live API | https://turtleisland.social/api/v1/instance | Live call 2026-11-24: 110 users, 102,252 posts |
| JoinMastodon server directory | API directory | https://api.joinmastodon.org/servers | Live call 2026-11-24: turtleisland.social entry, bipoc category |
| `turtleisland.social` per-tag usage | live API | https://turtleisland.social/api/v1/tags/<name> | Live calls 2026-11-24; 7-day figures in §1 |
| `indigenous-mastodon.social` (predecessor domain) | domain | https://indigenous-mastodon.social | Live check 2026-11-24: does not resolve |
| The five itch.io app pages | live pages | https://cherokee-language.itch.io/<slug> | Live fetch 2026-11-24: all HTTP 200 |
| ᏣᎳᎩ tag records (both instances) | live API | https://mastodon.social/api/v1/tags/ᏣᎳᎩ | Verified 2026-11-24: persisted records exist on both mastodon.social and turtleisland.social (the latter, id 154025, resolves in the case-normalized small-letter form ꮳꮃꭹ) |

## 8. Cost Frame

Every SC verifies as a single file read (SC-1–SC-4) — minutes each; skipping the reads risks binding a wrong handle or URL (SC-2, SC-4) or admitting invented claims (SC-3) into a standing workflow that future announcements inherit silently. The live API calls backing §1 were one-time at write/revision time (minutes, 2026-11-24). No numeric threshold is a pass condition.

## 9. Edge Cases

| Condition | Expected behavior |
|---|---|
| Post originates from `mastodon.gamedev.place` itself | The canonical handle link form suffices there; the full cross-instance form applies elsewhere — the doc states both forms and when each applies |
| Hashtag analysis queries the small server's trends endpoint | The instance carries no trending-tags data at its volume (observed 2026-11-24); the selection basis works from per-tag API statistics, not trends lists |
| Tag names resolve in case-normalized small-letter form (ᏣᎳᎩ → ꮳꮃꭹ) | Stats lookups verify the resolved record, never the requested spelling (the 2026-11-24 check initially misreported ᏣᎳᎩ as absent on the server) |
| The predecessor domain `indigenous-mastodon.social` | Does not resolve (verified 2026-11-24); the doc binds only the live server |

## 10. Completion gate

Every success criterion (SC-1 through SC-4) passes before the deliverable is complete; partial implementation is not permitted.

🤖 Co-authored with AI: OpenCode (huggingface/zai-org/GLM-5.3-Flash)
