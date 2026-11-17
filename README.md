"# red-fork

## Dashboard status

Production remains on the R5.16 baseline from the last verified read-only
preflight on 2026-10-16. This R5.17.5 phase made no production changes, and
did not recheck live services. The global command-access barrier, the
separate `kurumi_control` PostgreSQL cluster, the Red-native PostgreSQL
store, and unrelated services were not touched.

R5.17.5 is local preparation only. All 16 catalogued uncleared legacy photo
derivatives have been moved outside the dashboard's public static tree;
current landing, login, and authenticated-page artwork is CSS-only. The
530 generated R5.17 derivatives remain excluded, and the runtime artwork
manifest contains zero assets. File hashes verify technical integrity only,
not publication rights. The previously verified production rain-image URL
was not changed in this local-only phase and still requires a separate
owner-authorized disposition.

The Apache proposal is dashboard-scoped, but its routing behavior was
rehearsed with a local HTTP fixture rather than a running Apache server.
Syntax was checked against an isolated configuration; a faithful runtime
vhost rehearsal and rollback validation remain pending. Nothing was added
to or reloaded in production Apache.

Cutover remains blocked by the owner-operated sign-in/logout/re-sign-in
test, approval of the exact migration principal and maintenance window,
protected production backup/restore verification, and Apache integration
gates. See the new [R5.17.5 candidate status
record](./DEPLOYMENT-MANIFEST-R5.17.5.json); it is not deployable. The
[R5.17.4 status record](./DEPLOYMENT-MANIFEST-R5.17.4.json) remains
unchanged historical evidence.

The R5.17.4 diagnostic-output cookie was not inspected, copied, reused, or
invalidated in this phase. If it may have been exposed to an unauthorized
party, the associated session should be invalidated; authentication status
must be assessed through the authorized owner workflow.

## R5.17.6 release-gate review

The R5.17.4 and R5.17.5 status-record SHA-256 values still match their
recorded values. A read-only HTTPS `HEAD` request on 2026-10-16 returned
HTTP 200 (`image/jpeg`) for the previously reported `not_cleared` production
rain-image route; no image body was downloaded. This establishes that the
route is still publicly reachable, not that the full production release or
services were reverified. Production was not mutated.

The local candidate still excludes all 16 uncleared legacy derivatives and
all 530 R5.17 generated assets; the rotation manifest is empty. Candidate
checks pass, including 37/37 mocked command-access gate tests after stale
assertions requiring excluded WebPs were corrected. The production artwork
gate is **FAIL** until the rain asset receives documented clearance or is
removed through a separately authorized action. Owner authorization
lifecycle, migration approvals, protected backup/restore, and faithful
Apache runtime rehearsal remain **PENDING**. The new
[R5.17.6 blocked status record](./DEPLOYMENT-MANIFEST-R5.17.6.json) is not a
deployable manifest. Do not cut over.

See [DASHBOARD_WORKLOG.md](./DASHBOARD_WORKLOG.md) for the detailed
preparation, tests, and remaining gates, and
[kurumi_control/README.md](./kurumi_control/README.md) for control-store and
artwork behavior.

## R5.17.7 artwork planning and environment readiness

A read-only HTTPS `HEAD` check at 2026-10-16 21:42:09 UTC again found the
production `not_cleared` rain-image URL returning 200 `image/jpeg`. No image
body was downloaded and no production change was made. The proposed minimum
fix is limited to removing that exact deployed dashboard static derivative;
if the live R5.16 pages still reference it, the approved CSS-only page/style
replacements must be included atomically. Do not alter the shared static
route, other assets, source artwork, or rights labels. Expected direct
post-change behavior is 404, never a successful image response. The
operator must verify origin and public cache/CDN behavior and keep the
uncleared file absent during rollback. This is a plan only and needs explicit
owner approval and an authorized production operator/window.

Local candidate and route tests pass with an empty rotation manifest and
uncleared images excluded. No PostgreSQL 16 runtime/CLI or Apache 2.4 runtime
is available locally; WSL is unavailable and no container runtime was found.
Both rehearsal environments remain pending, as do owner authorization,
migration, backup/restore, and cookie-exposure actions. See the
[R5.17.7 non-deployable status record](./DEPLOYMENT-MANIFEST-R5.17.7.json)
for the exact remediation checklist and disposable-environment procedure.

## R5.17.8 artwork approval gate

R5.17.8 is blocked and non-deployable. Read-only production checks confirmed
the `not_cleared` rain derivative still returns 200 `image/jpeg` at both the
dashboard origin and public URL. The reviewable proposal removes only that
derivative and its two verified landing/login references, replacing them with
CSS-only motifs; no production change was made. Owner approval, named
operator, and an expiring UTC window are still required. PostgreSQL 16 and
Apache 2.4 runtime rehearsals also remain blocked pending isolated
administrator-provisioned environments.

See the [R5.17.8 non-deployable status record](./DEPLOYMENT-MANIFEST-R5.17.8.json),
[template patch](./R5.17.8-ARTWORK-TEMPLATES.patch), and
[CRLF-preserving CSS patch](./R5.17.8-ARTWORK-CSS.patch). Both patches passed
zero-fuzz dry-runs against the actual production text files but were not
applied. R5.16 remains the last fully verified production baseline. Do not
apply either patch without separately recorded approvals and passing release
gates.

## R5.17.9 artwork remediation approval gate

R5.17.9 is **BLOCKED — APPROVAL REQUIRED** and non-deployable. R5.17.8
records the artwork approval as pending, with no named operator or approved
UTC window/expiry. No authoritative record with the exact scope, target,
verification, and no-image-restore rollback approval was found. No production
access or change was attempted in this phase.

See the [R5.17.9 blocked status record](./DEPLOYMENT-MANIFEST-R5.17.9.json)
for missing approvals and the historical R5.17.8 observations. The proposed
patches from R5.17.8 remain unapplied. Production status is not freshly
verified; R5.16 remains the last fully verified baseline. The next step is
owner-recorded approval with the named human operator and valid UTC window;
do not proceed to pre-change production checks before that record is
validated.

The owner has since asserted full approval and supplied operator identifier
`ARCH`, but described the UTC authorization as open-ended (“no expiry”).
That does not meet the required finite UTC-window gate, and no durable
authoritative approval record tying the operator, window, scope, and
no-image-restore rollback together is recorded. R5.17.9 therefore remains
blocked; no production connection or change occurred.

### R5.17.9 execution follow-up - partial

The owner later supplied finite approval for operator identifier `ARCH`,
effective `2026-10-17T08:40:35Z` through `2028-01-01T00:00:00Z`. The approved
template and CSS patches were applied, and only the exact uncleared
derivative was deleted. Origin and public-edge requests now return 404 with
`text/html`; however, both origin- and edge-delivered landing HTML still
contains the removed filename, including cache-busted requests. No service
restart, Apache reload, or cache purge was authorized or performed. Keep the
asset absent and obtain separate approval before any runtime/cache
intervention. R5.17.9 remains partial and non-deployable; R5.16 remains the
last fully verified release baseline. See the
[R5.17.9 status record](./DEPLOYMENT-MANIFEST-R5.17.9.json) for exact hashes,
checks, test results, and remaining blockers.

Read-only follow-up found both landing responses identify `waitress`, with
no cache validators or intermediary cache headers. The dashboard service
started before the template edits; retained in-process template state is one
possible explanation for the stale markup, but the cause is unconfirmed. No
restart or cache purge was performed.

### R5.17.10 targeted live-template correction

The stale landing response was traced to the long-running dashboard process
serving pre-patch in-process template state: on-disk overrides had no image
reference, but the cache-busted origin still returned the old markup until
the dashboard process was refreshed. The owner authorized and the operator
restarted only `reddash-requiem.service`; no source/configuration change,
Apache reload, bot restart, database action, or CDN purge was needed.
Cache-busted origin and edge landing/login pages now return 200 with the CSS
motifs and no removed filename; the asset route remains 404 with a non-image
content type, and the anonymous protected route still redirects to login.
R5.17.10 is targeted remediation only, not full R5.17 deployment. See the
[R5.17.10 non-deployable status record](./DEPLOYMENT-MANIFEST-R5.17.10.json)
for the evidence and remaining independent release gates.

### R5.17.11 isolated release-gate rehearsals

The isolated PostgreSQL 16 migration/privilege rehearsal and protected
synthetic backup/restore passed. A private Apache 2.4.58 TLS virtual host
with Waitress verified proxying, static assets, authorization redirects,
application errors, and local gateway error documents; production Apache
and services were not changed. Focused access, artwork, Owner Portal,
error-page, and landing/login regressions passed. The R5.17.10 artwork fix
remains intact.

R5.17.11 is a **non-deployable, blocked status record**, not a release
manifest. The real owner-operated OAuth lifecycle and diagnostic-cookie
assessment, approved production migration principal/window, interactive
production command-gate check, and complete deployable inventory remain
open. Keep R5.16 as the last fully verified production baseline; do not
perform the R5.17 cutover. See
[DEPLOYMENT-MANIFEST-R5.17.11.json](./DEPLOYMENT-MANIFEST-R5.17.11.json)
and the chronological [worklog](./DASHBOARD_WORKLOG.md).

### R5.17.12 inventory and production prerequisites

R5.17.12 is **BLOCKED** and non-deployable. The new
[inventory/status record](./DEPLOYMENT-MANIFEST-R5.17.12.json) hashes 75
current candidate/support files and records deployment destinations,
sequence, pre/post checks, rollback boundaries, artwork exclusions, and
read-only production database observations. The inventory is blocked:
three local landing/login/CSS files differ from the verified R5.17.10 live
hashes, and additional runtime files in the dirty worktree need explicit
scope reconciliation. Do not deploy those local files or use them as
rollback inputs until reviewed.

Read-only checks confirmed the separate PostgreSQL 16.15 control cluster
on port 55439 remains at schema version 1; Red-native PostgreSQL on 5432
was not modified. Owner-role migration access and a finite UTC window are
only proposed, not approved or created. The diagnostic-cookie scope,
owner-operated OAuth lifecycle, and interactive production command-gate
check remain unresolved. The R5.17.11 isolated PostgreSQL/restore and
Apache/Waitress rehearsals remain valid PASS evidence. No production
service, database, or authentication state was changed; do not perform the
full R5.17 cutover.

### R5.17.13 source reconciliation and exact candidate inventory

R5.17.13 resolves the technical inventory blocker and records the exact
non-deployable candidate in
[DEPLOYMENT-MANIFEST-R5.17.13.json](./DEPLOYMENT-MANIFEST-R5.17.13.json)
(SHA-256
`207868c2ff1927513336ea82f969f4e4018c47caeb04696eff7023d2bcfa573f`).
It contains 26 hash-verified deployable files, a deterministic path/hash
list, 28 file-by-file runtime reviews (27 unchanged production files and
one owner-layout change), exact current destination states, supporting
prerequisites, exclusions, ordering, and a rollback boundary. No source
file was overwritten.

The local landing template, login template, and `kurumi-r58.css` hashes
match the broader R5.17.5 artwork-preparation versions, not the narrower
R5.17.10 live fix. Their extra CSS-dial redesigns are excluded; the exact
R5.17.10 production files remain the selected baseline. Six further local
image-to-motif page/style changes are excluded for the same scope reason.
`kurumi-r58-archive-rain.jpg` remains absent and may not be restored or
referenced. The R5.17.3-to-R5.17.5 manifest/error-page hash changes were
reconciled to the R5.17.5 record. Backup operator files differ from their
live copies only in CRLF versus LF line endings and are verify-only.

**Candidate inventory is complete for owner validation, not authorized for
cutover.** The real OAuth lifecycle, live Discord command-gate check,
diagnostic-cookie assessment, migration identity/window approvals, and
separate production cutover authorization remain outstanding. No migration,
grant change, service restart, Apache reload, authentication change, or
deployment occurred. See the manifest and chronological
[worklog](./DASHBOARD_WORKLOG.md) for gate evidence and the compact owner
checkpoint.

## Web mini-games and public pages

The dashboard has four features that talk to the bot only over the local RPC socket, with the bot enforcing every rule:
public **leaderboards** (`angelpublic`), **achievements** (`angelbadges`), a per-server **fun page** (`angelfun`) and
**browser mini-games** (the daily word, trivia and pet care, inside `angelgames` and `angelpets`). Since 14 November 2026
they are public by default: Angel shows members' Discord display names, avatars, profiles, levels, stats, achievements,
leaderboard positions, ratings and reviews, and starred messages, polls and welcome tiles on its website and in Discord cards.
One click hides a person from every public page and card (the switch on **My settings** at `/me/settings`, or
`angelweb hide`; `angelweb show` undoes it), and one click deletes everything Angel stores about them (**Delete everything**
on My settings, or `angelweb delete`). A server admin can hide a server from the website entirely, and the server fun page
stays off until an admin switches it on. Playing in the browser writes the same stats as playing in Discord, rewards go through the existing capped economy
path, and answers never leave the bot before a game is over. See the privacy page on the website for the full list of what
is stored. Support: https://discord.gg/8Kt9uAMBqe

## Angel's heart (R5.25.0)

- Angel has a bratty, self-adoring, flirty personality in chat and in staff orders; flirting is playful and never sexual, and never with anyone under 18. Punishment texts stay neutral.
- Members on Arch's chat list (owner page `/owner-portal/angel-chat`) can chat with her where AI is granted and switched on; chat has no tools and uses free providers by default.
- Now and then she playfully refuses a non-urgent staff order; asking again always works, and she never refuses the bot owner, urgent or safety orders.
- Member memory: Angel saves short notes about members who talk to her on our servers (never passwords, contact details, location, health, religion, politics, sexuality, ethnicity, age, crime or anything about other people), plus an under-18 flag that only switches flirting off and stays after an opt-out; she deletes them 180 days after the last talk, and `guard ai memory` (while signed in on the website) or the support server shows, deletes or stops them.
- Support: https://discord.gg/8Kt9uAMBqe (no e-mail).
