# Blog-posting workflow — Mastodon toots + inline embed back-fill

Workflow of record for publishing new blog entries on CherokeeLessons
(entries under `content/content/`) with a Mastodon toot and back-filling the
toot link into the article body.

**Applicability:** this workflow applies to new `content/content/` entries
going forward. Existing entries carry no matching Mastodon toots and are not
retrofitted (developer statement, 2026-11-21) — no retroactive toot or
back-fill work is in scope for them, ever.

The workflow is **credentialless and agent-assisted**: the agent prepares the
toot text and performs all verification through signed-out public Mastodon
reads; the developer posts the toot by hand and performs every git merge. No
Mastodon credentials, tokens, or write-API calls exist anywhere in this
workflow; no wrapper script, pairing ledger, CI step, or hook encodes it.

Sources of record: the brothertown-language-com workflow of record
(`docs/sop-site-setup.md` SOP rule 5, 2026-11-15), adapted here with embeds
allowed and the modern inline embed form; the michael-conrad.com back-fill
implementation (`local_util/backfill_mastodon_embeds.py`) was compared and
its ledger+script machinery deliberately not carried — the agent-editor
mechanics below need no repo machinery, and no cargo culting of either
sibling's dated forms is permitted.

## Account of record

Toots are posted by the developer from **`@michaelconrad@mastodon.social`**
(mastodon.social, home base). That account's profile URL,
`https://mastodon.social/@michaelconrad`, is the threadless stand-in target
(see the GAP mechanism below). No other account is in scope for blog-entry
toots.

## Step 1 — Toot-text preparation (agent)

When a new entry is ready to publish, the agent prepares paste-ready toot
info carrying:

- the article's public URL;
- a short lede for the entry;
- hashtags derived from the entry's frontmatter `tags` in Mastodon
  concatenated form (`#TagText`), **each tag verified before it enters the
  toot** via a signed-out public read:
  `GET https://mastodon.social/api/v1/tags/<tag>` — a 200 response with a
  populated record confirms the tag surface; an empty or failed result is
  recorded (noted in the posting info) but is not fatal — the frontmatter
  tags remain the hashtag source of record either way;
- the posting visibility: **public**, of record.

The developer then posts the toot **by hand** from the account of record.
No script and no API write anywhere in this step.

## Step 2 — Threadless publication (GAP mechanism)

An entry whose discussion thread does not yet exist may publish threadless
**only on the developer's explicit flag-release instruction** — never as a
default and never silently. Its threadless state of record:

- an HTML comment above the section naming the blocker, in the form
  `<!-- GAP: … -->` (e.g. the toot does not exist yet);
- the section shell (Step 3's heading and intro line) with, inside the
  sentinels, a plain placeholder anchor:

  ```markdown
  [View on Mastodon](https://mastodon.social/@michaelconrad)
  ```

  A profile URL cannot produce an OEmbed embed — the placeholder is never
  presented as a generated embed, and no substitute embed markup is
  hand-rolled for it.

The **completion sequence of record** for a GAP entry:

1. toot posted by the developer;
2. probe: agent discovers and dual-verifies the toot URL (Step 4);
3. the placeholder is replaced by fresh verified OEmbed output (Step 4's
   insertion, generated fresh per entry), and the GAP comment is stripped;
4. `./build.sh` verifies the tree;
5. a follow-up PR carries the completed back-fill;
6. the developer merges; cleanup follows.

The section's wording never changes across this sequence — only the
placeholder's replacement, the GAP comment's removal, and the embed markup
inside the sentinels.

## Step 3 — Back-fill block form of record

The back-fill block is **body-authored**: it ships inside the entry's
`index.md` markdown body, appended at the end of the entry (not frontmatter,
not a template-rendered slot). The exact form:

```markdown
### Comment on Mastodon

You can also comment on this post via Mastodon:

<!-- mastodon-embed-start -->

<output of GET https://mastodon.social/api/oembed?url=<verified toot url>,
inserted verbatim, generated fresh per entry>

<!-- mastodon-embed-end -->
```

- The heading is `### Comment on Mastodon`; the intro line is
  `You can also comment on this post via Mastodon:` — the word "**also**"
  is load-bearing: giscus is the site's discussions system of record
  (`templates/page.html` loads it in the page footer), and the Mastodon
  block is an additional venue pointer rendered above the giscus footer
  widget (body content vs page-footer template). Nothing in this workflow
  repositions, replaces, or out-frames giscus.
- The `<!-- mastodon-embed-start -->` / `<!-- mastodon-embed-end -->`
  sentinels wrap the embed and make completion/replacement of a placeholder
  idempotent.
- Inside the sentinels goes the **verbatim output of the OEmbed endpoint**,
  `GET https://mastodon.social/api/oembed?url=<verified toot url>`, fetched
  fresh per entry. The generator family of record (verified live
  2026-11-21, mastodon.social) is: a `blockquote.mastodon-embed` element
  carrying `data-embed-url`, with a fallback anchor to the verified toot
  URL inside it, plus
  `<script data-allowed-prefixes="https://mastodon.social/" async src="https://mastodon.social/embed.js"></script>`.
- Generator evolution is not a defect: the same `embed.js` hydrates both
  this of-record family and the older `iframe.mastodon-embed` family shown
  in the joinmastodon.org OEmbed docs example — embed origins stay
  `mastodon.social` either way. The fallback styling is Mastodon's fixed
  light theme, accepted as the generator's own surface.
- **Never authored as the form of record:** legacy `iframe.mastodon-embed`
  markup, any hand-rolled substitute, or any reworked variant of the
  endpoint output. If the OEmbed endpoint is unreachable or returns unusable
  output at back-fill time, the completion step is blocked and reported —
  the GAP/placeholder state persists until the endpoint answers; no
  fallback markup is invented.
- If the verbatim output is later replaced, the replacement is again fresh
  verbatim OEmbed output, not a hand edit.

Tradeoff of record: a body-authored block appears in feeds and no-JS
contexts; the generator's fallback anchor serves those.

## Step 4 — Probe discipline (URL discovery and dual verification)

After the developer's hand-post, the agent — never supplied the thread URL
by the developer — discovers and verifies it entirely via **signed-out
public reads**: the account statuses timeline
(`GET https://mastodon.social/api/v1/accounts/<id>/statuses`) and/or the
account RSS feed (`https://mastodon.social/@michaelconrad.rss`) and/or
single-status reads, as best fits the moment.

Before any repo change lands from the probe, the agent confirms **both**:

1. the toot's own canonical `url`; and
2. its link back to the article — a link in the status body, or the
   preview-card `card.url`.

If no public toot links back to the article (missing, private, or deleted
toot), the back-fill does not proceed; the GAP state stands and the blocker
is reported. Multiple candidates (reposts, quotes) constrain to canonical
`url`s published by the bound handle that link back to the article. An entry
whose folder path changed so that the link-back no longer matches correctly
fails dual verification: halt and report, never back-fill.

## CSP step (first embed-bearing change)

The Mastodon embed loads `https://mastodon.social/embed.js` and renders
`mastodon.social` embed content. `https://mastodon.social` enters
`static/_headers`'s `script-src` and `frame-src` **in the same change as
the first embed-bearing back-fill** — never earlier (the repo's evergreen
iframe-embed gate defers the origins edit to the first embed-bearing
change), and never silently skipped. The CSP edit alone proves nothing: the
served page is verified by browser render with a console check for CSP
frame/script violations, per the repo's evergreen iframe-embed gate
(`./build.sh` cannot catch a runtime frame block). Current CSP of record
(`static/_headers`): `script-src` carries `'self' 'unsafe-inline'
https://giscus.app`; `frame-src` carries `https://giscus.app
https://www.youtube.com https://itch.io` — `mastodon.social` is in neither
at the time of writing.

## Boundaries of record

- **No automation, no credentials:** no Mastodon tokens or write-API calls
  anywhere; posting is the developer's manual act; every Mastodon API touch
  by the agent is a signed-out public read.
- **No machinery:** no wrapper script, pairing ledger, CI step, or hook
  encodes this workflow. Each back-fill is manual agent-editor work executed
  inside the entry's follow-up PR cycle.
- **No ending-block import:** new entries keep the site's current article
  shape — no sign-off, no hot-linked in-body hashtag line, no AI byline are
  imported from the sibling workflows. Toot hashtags derive from frontmatter
  tags at toot time.
- **No invented claims:** this doc states only what the sources of record
  above establish — no boost or reach claims, no repair obligations beyond
  the completion sequence, no account identities beyond the bound handle.
- The CherokeeLessons app-announcement workflow (CherokeeLessons#26) is a
  separate standing surface, unbound by this doc.
