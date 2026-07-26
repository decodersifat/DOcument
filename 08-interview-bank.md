# Section 27 — 150 Project-Specific Interview Questions and Answers

Use these for self-review, mentoring, or interviews. Answers describe the audited
repository, not an undocumented ideal backend.

## Beginner — 50 questions

### B1. What does this application do?

It is the Eagle 3D Streaming customer control panel. Users manage streaming
application builds and links, dedicated servers, uploads, analytics, plans,
payments, teams, profiles, and developer credentials.

### B2. Which framework and router does it use?

Next.js 15 with the App Router under `src/app`.

### B3. Which React version is declared?

React and React DOM 19.

### B4. What language is most application code written in?

TypeScript/TSX. `allowJs` permits at least one JavaScript route entry.

### B5. What is the development port?

The package dev scripts explicitly use port 6400.

### B6. Where is the root layout?

`src/app/layout.tsx`.

### B7. What does a parenthesized App Router folder mean?

It is a route group: it organizes layouts without adding that folder name to the
URL.

### B8. What are the two main route groups?

`(with-public-layout)` for auth/onboarding-style public pages and
`(withSidebarLayout)` for the authenticated navigation shell.

### B9. Is there a public-layout file inside the public group?

No. Those pages inherit only the root layout.

### B10. Is there Next middleware?

No `middleware.ts` was found.

### B11. How are protected pages gated?

The sidebar route-group layout checks session, username, API key, and customer ID
on the client, then redirects or renders a loader/shell.

### B12. Does that client gate replace backend authorization?

No. It is UI navigation behavior; sensitive APIs must authorize independently.

### B13. What is the main route `/`?

The streaming-application management page, implemented through a JavaScript page
entry that composes the streaming-app feature.

### B14. Name three public routes.

Examples are `/signin`, `/signup`, and `/repass`; `/username` and `/fb-mail` are
also in the public group.

### B15. Name three protected feature routes.

Examples are `/analytics`, `/plans`, and `/multiplayer-server`.

### B16. What global providers wrap the route?

Redux, `next-themes`, Super Admin handling, consent banner, Sonner toaster, and a
global browser-error catcher.

### B17. What additional providers wrap protected page content?

Streaming-app upload sequence, dedicated-server upload, additional upload, and
streaming configuration providers.

### B18. What library supplies server-state caching?

RTK Query through Redux Toolkit.

### B19. What library sends shared REST requests?

Axios, wrapped by `axiosBaseQuery`.

### B20. What is `baseApi`?

The single RTK Query API created with the shared Axios base query; domain endpoint
files inject operations into it.

### B21. What is a query tag?

A cache label. Queries provide tags and mutations invalidate them so active stale
queries can refetch.

### B22. What does `.unwrap()` do on an RTK mutation result?

It returns a promise that resolves with mutation data or rejects with the RTK
error, enabling normal `try/catch`.

### B23. What are the two REST base-service categories?

AGW for most product/billing/upload operations and Auth for sessions, teams,
account changes, and developer credentials.

### B24. How is environment selection performed?

`config-loader.ts` selects staging only when
`NEXT_PUBLIC_ENV === "staging"`; otherwise it uses production.

### B25. Why is that default risky for local work?

A missing or misspelled environment value can direct a developer to production
services.

### B26. How many public environment variables are documented?

Twenty-five: `NEXT_PUBLIC_ENV` plus six Firebase values for each of four named
Firebase apps.

### B27. Are `NEXT_PUBLIC_*` variables secret?

No. They can be embedded in browser JavaScript.

### B28. What are the four named Firebase app groups?

Accounts, stream monitor, dedicated-server monitor, and app executable data.

### B29. What does Firebase do here?

It supplies Google identity and Firestore realtime data for monitors and upload
processing.

### B30. How does email sign-in establish a session?

AGW returns an identity token; the UI sends it as a Bearer token to the Auth
session-token endpoint, which sets a browser cookie.

### B31. How does Google sign-in differ?

Firebase Auth popup supplies the ID token; session-cookie creation then follows the
same Auth endpoint path.

### B32. What is `useUserInfo` important for?

It resolves the effective email, username, API key, and owner/member context,
including a browser-selected team member.

### B33. What styling approach is used?

Tailwind CSS v4 utilities backed by CSS-variable design tokens in `globals.css`.

### B34. What is shadcn/ui in this repository?

A convention for source-owned components under `components/ui`, configured for
New York style, stone base, CSS variables, and Lucide icons.

### B35. What does Radix UI provide?

Accessible headless primitives used beneath dialogs, selects, tabs, tooltips, and
other shadcn components.

### B36. What does `cn()` do?

It combines conditional class names with `clsx` and resolves Tailwind conflicts
with `tailwind-merge`.

### B37. How are toasts shown?

With Sonner.

### B38. Which form library is used?

React Hook Form, primarily for the large streaming configuration form. Many other
forms use local controlled state.

### B39. Which table library is used?

TanStack React Table.

### B40. Which chart library is used?

Recharts.

### B41. Why are uploads sent to signed URLs?

The browser can send large bytes directly to storage without proxying the payload
through this Next frontend/server.

### B42. Which upload storage modes exist?

A CoreWeave/GCS-style signed upload path and a Cloudflare R2 multipart path selected
by a storage-provider API.

### B43. What happens when a primary upload stays too slow?

Below 0.5 MB/s for about 90 seconds, it is cancelled and one alternate signed-URL
attempt is made.

### B44. How many R2 parts upload concurrently?

Six, with per-part retries.

### B45. How does upload processing progress arrive?

Through Firestore snapshots after upload metadata is registered in the waiting
queue.

### B46. Is upload workflow state restored after page refresh?

No complete recovery/rehydration path was found; Context state is ephemeral.

### B47. What are PPCCU, PPL, prepaid minute, and PPM?

They are legacy billing products. PPM means Pay Per Minute; the official expansions
of PPCCU and PPL are not defined in source.

### B48. What legacy-plan priority does the UI use?

PPCCU, then PPL, then prepaid minute, then PPM.

### B49. Are there automated tests?

No project test files, test script, or test framework dependency was found.

### B50. Did the audited production build pass?

Yes, with staging selected and network access for the Google font. It completed
static route generation but emitted a large warning backlog.

---

## Senior — 50 questions

### S1. What is the dominant architectural boundary problem?

The frontend has recognizable route/feature/data layers, but critical policy and
workflow state are distributed through large client components, contexts, hooks,
browser storage, and untyped responses. Boundaries exist structurally more than
contractually.

### S2. What would you remediate before a broad refactor?

Contain Super Admin credentials/open redirect, verify backend authorization, add
transport regression tests and fix Axios errors, then test the known entitlement
and billing bugs.

### S3. Why not start by rewriting the upload provider?

Its behavior spans several external systems and has no tests. A rewrite before
characterization can silently change retry, processing, notification, or billing
semantics.

### S4. How would you decompose upload architecture?

Separate pure archive inspection, entitlement decision, provider-neutral transport
interface, R2/signed-URL adapters, reducer/state machine, Firestore processing
repository, persistence/resume, and sanitized telemetry. Keep React as the adapter
that renders state and dispatches events.

### S5. What state-machine properties should upload enforce?

Explicit allowed transitions, terminal-state idempotency, one active job identity,
cancellation ownership, late-event rejection, retry budgets, resumable persisted
metadata, and exactly-once-or-idempotent completion side effects.

### S6. How would you safely fix the Axios interceptor?

Freeze existing behavior with 2xx/non-2xx/network/timeout tests, inventory consumers
checking error-shaped `data`, introduce a normalized error type, reject errors, and
migrate feature handling in the same controlled release.

### S7. What should the normalized error contract include?

Stable kind/status/code/message, safe field errors, retryability, request
correlation ID, and an original cause available to internal logging but not blindly
rendered.

### S8. How would you redesign Super Admin?

An authenticated administrator requests a backend-minted, short-lived, signed,
single-use code scoped to target and capability. The target browser exchanges it
for a distinct audited impersonation session; the URL is immediately cleaned, the
UI shows/ends impersonation, and backend authorization/audit is mandatory.

### S9. How would you handle already exposed Super Admin keys?

Stop link issuance, search approved telemetry/history stores, rotate/revoke keys,
invalidate sessions, notify security owners, preserve incident evidence, and track
completion centrally.

### S10. What is the trust boundary for selected team member?

The browser selection is an untrusted requested target. The authenticated backend
session plus server-side team/role policy must decide whether each operation is
allowed.

### S11. Would Next middleware solve authorization here?

It can improve early navigation/session checks, but it cannot replace endpoint
authorization. Cross-origin backend services still need to validate every action.

### S12. How would you choose server versus client session gating?

If the cookie and Auth service are usable server-side, validate in a server layout
or middleware to avoid shell flashes and redirect earlier. Preserve client queries
for interactive state, and avoid exposing backend-only credentials to the client.

### S13. What CSRF questions must be answered?

Cookie `SameSite`/domain/path, CORS credential policy, CSRF token/origin validation
for writes, method/content-type defenses, and whether cross-site payment returns
need exceptions.

### S14. How would you define a role/capability model?

Create named capabilities such as billing-write, team-admin, key-rotate,
app-delete, server-control, and monitor-internal. Backend derives them from session
and target; frontend consumes capability results for presentation.

### S15. How should API types be introduced without blocking delivery?

Start at high-risk boundary modules, add request/response generics and runtime
validation/adapters, migrate one vertical slice, and generate from an owned schema
when available. Do not type accidental raw nesting throughout components.

### S16. Where should response normalization live?

At the endpoint/repository boundary through typed `transformResponse` or adapter
functions, so features consume stable domain models.

### S17. How would you model subscription state?

Normalize products into a discriminated union with status, quantity, entitlement,
billing cadence, transition capabilities, and source. Pure selectors derive the
current presentation; backend decisions remain authoritative.

### S18. How would you resolve legacy-plan priority?

Document it as an explicit pure selector with exhaustive fixtures. If multiple
simultaneous plans are invalid, surface and monitor the anomaly rather than silently
hiding all but one.

### S19. How would you clarify trial exhaustion?

Model minute and storage entitlements separately, then name product decisions such
as `blocksNewUpload` or `requiresUpgrade`; avoid one ambiguous `hasTrialFinish`.

### S20. What should be tested around monetary UI?

All plan/status/owner combinations, quote display, quantity boundaries, currency and
rounding, checkout/non-checkout selection, retries/idempotency, stale cache, and
server rejection. Never infer payment success from query parameters alone.

### S21. How would you calculate average CCU correctly?

Sort start/end events with defined tie behavior, sweep active count, integrate
`activeCount * duration` over the complete selected window including zero gaps,
then divide by total window duration.

### S22. How would you anonymize analytics properly?

Define policy outside presentation, preferably apply it server-side before
delivery, use stable pseudonyms only when required, document reidentification risk,
and test that raw identifiers are not exposed.

### S23. How would you secure Firestore monitor data?

Use Firebase Security Rules/claims to restrict reads by account and capability,
minimize document fields, test rules with the Emulator, and treat UI filtering as
presentation only.

### S24. How would you make realtime hooks testable?

Inject a repository/listen function and clock, map snapshots through pure
functions, expose unsubscribe, and test initial/error/update/stale/unmount sequences
with deterministic timestamps.

### S25. What is a safe telemetry design?

Define allowed structured fields, redact URLs/query/body/keys, minimize PII, sample
and rate-limit, attach correlation IDs, enforce retention/access, and send privileged
events from trusted backend services.

### S26. How should consent affect global scripts?

Start in unknown state with disallowed scripts absent where required, load them only
after consent, persist/version the decision, support rejection/revocation, and
verify via network tests.

### S27. How would you add CSP with GTM, Hotjar, Firebase, and signed uploads?

Inventory exact script/connect/image/frame origins and required inline behavior,
introduce report-only CSP with nonces/hashes where possible, inspect violations,
then enforce gradually. Avoid a wildcard policy that provides little value.

### S28. How would you reduce first-load JavaScript?

Measure a bundle graph, route-scope upload/config providers, lazy-load charts and
rare dialogs, remove proven-dead code, isolate Firebase SDK consumers, and compare
route output plus real device metrics after each change.

### S29. When is memoization appropriate in this codebase?

When profiling shows expensive recomputation or prop identity prevents a stable
child from skipping renders. Correct dependency lists and state ownership matter
more than adding `useMemo` mechanically.

### S30. How would you address exhaustive-deps warnings?

Classify each effect as synchronization, subscription, or event response; stabilize
inputs or restructure logic, include real dependencies, test identity changes and
cleanup, and suppress only with a documented invariant.

### S31. What error-boundary hierarchy would you add?

A root `global-error` for catastrophic failures, route-group `error.tsx` for
recoverable screens, and local boundaries around charts/upload/account panels,
each with sanitized reporting and retry/reset behavior.

### S32. What should remain outside a React error boundary?

Async/event/network failures should be caught and represented in workflow/query
state. Boundaries primarily catch descendant render/lifecycle errors, not all
promise rejections.

### S33. How would you make the package toolchain reproducible?

Pin Node and pnpm, use Corepack or the approved mechanism, install from
`pnpm-lock.yaml` with frozen resolution, cache by lock hash, and run the same
commands locally and in CI.

### S34. What deployment decision must be made for semi-production?

Choose a Next server/standalone deployment or enable and validate static export if
all features support it. Then align the artifact path instead of assuming `build/`.

### S35. What is unauditable about production deployment?

The workflow only SSHes to a VM and runs `~/redeploy_cp_next.sh`; that script and
its rollback/security behavior are outside this repository.

### S36. How would you reconcile PM2 port configuration?

Choose one source of truth: allow Next to consume `PORT` or set the same explicit
port in both process and script. Add a startup health assertion.

### S37. How would you introduce testing in stages?

First pure and transport units, then critical components/realtime adapters, then a
small sandboxed end-to-end suite. Make CI fast and deterministic before expanding
coverage.

### S38. Which pure functions should be extracted first?

Redirect validation, plan priority/labels, trial/storage entitlement, CCU
calculation, ZIP executable detection, upload progress mapping, and stale-record
classification.

### S39. How would you verify a file is truly dead?

Check static and dynamic imports, framework conventions, CSS/HTML string paths,
public URL references, package exports, tests/stories/tooling, git history, and
roadmap owners; then remove it in an isolated build-tested change.

### S40. What should replace the confirmed component cycle?

A dependency-neutral dedicated-server domain module or controller hook that both
components import, with container-to-presentational direction kept one way.

### S41. How would you organize feature boundaries going forward?

Co-locate a feature’s components, domain model/selectors, endpoint adapters, tests,
and route-facing entry while keeping stable primitives and cross-feature services
in shared layers. Enforce import direction.

### S42. Would you replace Redux?

Not without evidence. RTK Query already handles the dominant REST state well; the
priority is typed contracts and consistent caching. The tiny local slice could be
removed or retained independently.

### S43. When should Context be replaced?

When it carries a large frequently changing state across many unrelated consumers
or combines business engine and rendering. For upload, a tested external reducer/
state machine plus a thin Context adapter is more valuable than merely swapping
state libraries.

### S44. How should direct Axios uploads synchronize RTK cache?

Give one orchestration layer ownership of completion, then invalidate/refetch
specific tags after confirmed success. Avoid components independently issuing
duplicate completion and refetch calls.

### S45. What resilience should signed uploads support?

Expiry-aware URL refresh, cancellable/retryable chunks, network/offline handling,
checksums where supported, idempotent completion, durable job identity, and explicit
terminal/recovery UX.

### S46. How would you handle account changes with module caches/listeners?

Key caches by effective account or reset them on session/member changes; unsubscribe
old listeners, cancel old requests, invalidate RTK data, and prevent late events
from populating the new account view.

### S47. How would you improve responsive/theme quality?

Define supported breakpoints, remove forced 1250px minimum layouts, map hardcoded
colors to semantic tokens, decide whether dark mode is supported, and add visual,
keyboard, zoom, and contrast regression checks.

### S48. What observability would help diagnose cross-system uploads?

A client-generated non-secret correlation/job ID propagated through initiate,
parts, completion, queue, processing, and sanitized telemetry, with stage latency,
retry, cancellation, and terminal-state metrics.

### S49. How would you sequence a 90-day modernization?

Days 1–30 contain security/correctness and establish tests; 31–60 type boundary
contracts and decompose upload/subscription models; 61–90 optimize provider/bundle
architecture, accessibility, deployment, and observability using measured results.

### S50. What distinguishes a fact from an assumption in this audit?

A fact is directly visible or build-verified in this repository. Cookie attributes,
backend authorization, API schemas, Firestore Rules, official acronym expansions,
and infrastructure behavior outside checked-in workflows are assumptions or
questions until their owning systems provide evidence.

### Section 27 closeout

**Key takeaways**

- The bank contains exactly 50 beginner, 50 intermediate, and 50 senior questions.
- Strong answers trace evidence and state what the repository cannot prove.
- Senior reasoning prioritizes risk containment and characterization before
  rewrites.
- Interview knowledge should translate into safe staging demonstrations.

**Common pitfalls**

- Memorizing package names without explaining request/state flow.
- Claiming backend guarantees from UI conditions.
- Inventing acronym expansions or historical rationale.
- Recommending a rewrite without migration and verification.

**Questions to ask a mentor**

- Which answers differ from current backend reality?
- Which questions should be required for on-call readiness?
- Which domain needs an additional specialist bank?

**Related files to read next**

- Every answer points to topics mapped in Sections 1–26 and 28.
- Use `04-file-catalog.md` to locate the direct implementation.

**Practical exercises**

- Answer five random questions at each level without notes.
- Demonstrate one answer in staging or a test.
- Rewrite any answer when new backend evidence becomes available.

## Intermediate — 50 questions

### I1. Why can every route be statically prerendered while showing user data?

The build emits static page shells. Client components query session and product
data after hydration, so personalized content is not required at build time.

### I2. What is the consequence of putting payment result routes inside the protected group?

A returning user without a valid session may be redirected to sign-in before seeing
the result. The backend must remain the payment truth regardless of the page shown.

### I3. Why is provider order significant?

Descendants can consume only providers above them. Reordering Redux, theme,
identity, or workflow contexts can remove dependencies or change initialization
side effects.

### I4. Why do invalid `'use-client'` strings not currently break uploads?

They are not valid directives, but the files are imported beneath real client
boundaries. They are therefore bundled into the client graph indirectly.

### I5. What could happen if one of those modules were imported from a server component?

Next could reject browser-only hooks/globals because the module does not declare a
valid independent client boundary.

### I6. How does RTK Query reach Axios?

A generated hook calls an injected endpoint; `baseApi` invokes
`axiosBaseQuery`; that calls `axiosInstance`.

### I7. What does the Axios success interceptor return?

`response.data`, so endpoint consumers see the response body rather than the full
Axios response.

### I8. What is wrong with the Axios error interceptor?

It returns the error instead of `Promise.reject(error)`, resolving the request chain
and potentially classifying an HTTP failure as successful `data`.

### I9. Why can fixing that single line cause regressions?

Some components may have adapted to error-shaped success data. Tests must identify
and migrate those accidental response assumptions.

### I10. How does app asset listing adapt to storage provider?

`getStreamingAppListEndpoint()` awaits `getStorageProvider()` and selects the
normal or R2 list endpoint before calling the base query.

### I11. What else uses provider-selected endpoints?

Deleting a streaming app and deleting one app version.

### I12. What is risky about the provider cache?

It lasts for the module/page lifetime and is not visibly keyed by account. An
account/provider change in the same tab can leave stale routing.

### I13. How does selected team-member state alter requests?

`useUserInfo` prefers `selectedTeamMember` from local storage, so username and API
key payloads can target that member instead of the owner.

### I14. Why must the backend validate the selected member?

Local storage is user-controlled and cannot prove the signed-in session has
permission to act on that target.

### I15. Describe the Super Admin payload.

It is base64-decoded JSON with mail, user, and API key, then stored in browser
storage for effective-member resolution.

### I16. Why is base64 unsuitable for protecting it?

Base64 is reversible encoding with no confidentiality, integrity, expiry, or sender
authentication.

### I17. What contradicts the old Super Admin documentation?

The old doc says the query parameter is removed; the handler retains it in the URL.

### I18. How should login redirect targets be validated?

Prefer relative internal paths. If absolute destinations are necessary, parse and
allowlist exact protocols and origins while rejecting protocol-relative and
encoded bypasses.

### I19. Does the consent banner prevent GTM loading?

No. GTM is inserted `beforeInteractive`; the banner renders later.

### I20. What plan statuses are treated as active?

The hooks commonly recognize `active`, `trialing`, and sometimes product-specific
handling of `past_due`.

### I21. What is wrong with `hasCoreStorageExp`?

It is hardcoded `false`, so the UI cannot report/gate an expired Core storage
allowance through that hook.

### I22. What ambiguity exists in `hasTrialFinish`?

It requires both minute and storage trial exhaustion, while consumers also test
individual flags. “Finish” therefore has no single clear product meaning.

### I23. Explain the `BillSummary` ternary defect.

A branch tests the constant `PLAN_NAME.PREPAID_MINUTE` rather than comparing the
current plan, so the truthy string captures later cases.

### I24. What should replace that nested plan ternary?

An exhaustive typed map or switch with a deliberate unknown-plan case.

### I25. What is the app-upload ZIP validation looking for?

A Windows executable, or a Linux engine plus appropriately named root shell script,
with app-name and plugin/PS2-related structural checks.

### I26. How is combined upload progress mapped?

The byte upload occupies roughly the first half; backend waiting/downloading,
extracting, testing, and tested states map to later milestone percentages.

### I27. What triggers upload stuck reporting?

The provider emits periodic notifications around every three minutes and enters an
error path after roughly twenty minutes without a successful terminal state.

### I28. What cleanup is essential for Firestore listeners?

Retain and invoke every unsubscribe during effect cleanup and identity/job changes;
also prevent late snapshots from mutating a cancelled/unmounted workflow.

### I29. How does R2 multipart completion work?

Initiate upload, request signed part URLs in batches, upload byte parts, collect
part numbers/ETags, then send them to the complete endpoint.

### I30. Why does multipart retry need idempotency awareness?

The client must know whether repeating part upload or completion is safe and avoid
creating duplicate sessions or completing with inconsistent ETags.

### I31. How are analytics stream records fetched?

`useStreamRecordQuery` posts the date/filter body to the AGW stream-record endpoint.

### I32. How is CCU derived?

Stream starts and ends become ordered events; a sweep adjusts the active count over
time.

### I33. Why can the displayed average CCU be misleading?

It averages event/interval values without necessarily weighting the entire selected
duration and can omit zero-concurrency gaps.

### I34. What unexplained analytics transformation exists?

A hardcoded filter maps a username containing `ahsan` to `Unknown2`, embedding
undocumented data policy in the UI.

### I35. How are employee monitor records hidden?

Non-admin clients filter Firestore documents marked `e3ds_employee`.

### I36. Why is that not access control?

If Firestore rules allow the read, the browser already received the data; client
filtering only changes presentation.

### I37. What confirmed circular dependency exists?

Dedicated-server `application.tsx` and
`dedicated-server-card-details.tsx` import one another.

### I38. How should that cycle be broken?

Move shared types/actions/state or common presentation into a third module, leaving
one-way imports.

### I39. Why are zero-incoming files not automatically dead?

Framework entries, configuration roots, dynamic imports, CSS/string asset
references, or external consumers may not appear as TypeScript static importers.

### I40. Give examples of likely placeholders.

The empty additional-upload and dedicated-server API/slice files are strong
candidates, pending roadmap confirmation.

### I41. Why can the protected provider stack hurt performance?

Every protected route mounts all upload/config contexts even if the page does not
consume them, increasing bundle, initialization, effect, and rerender cost.

### I42. Which audited route had the largest first-load JavaScript?

The `/` streaming-app route, at roughly 577 kB in the verified build output.

### I43. What is the issue with `images.remotePatterns`?

It accepts HTTPS images from any hostname, much broader than an inventory-based
allowlist.

### I44. What security headers are configured by Next?

No custom CSP or related browser security headers are declared in `next.config.ts`.

### I45. Why is CI dependency installation inconsistent?

The repo commits `pnpm-lock.yaml`, while the semi-production workflow runs
`npm install`.

### I46. Why is semi-production output suspicious?

It syncs a `build/` directory, but normal Next config produces `.next/` because
static export is not enabled.

### I47. What port conflict exists?

PM2 sets `PORT=3448`, while `start:prod` explicitly invokes Next with `-p 6400`.

### I48. What is unsafe in the config verification script?

It prints environment-file contents when the files exist, which can leak values to
logs or recordings.

### I49. What should be the first tests added?

Axios success/error behavior, redirect validation, plan/trial/storage selectors,
ZIP validation, and analytics concurrency are high-value deterministic beginnings.

### I50. How should an engineer investigate a feature end to end?

Trace route → component → hook/context → RTK endpoint or service → config URL →
cache/listener → render states, and clearly mark backend assumptions.

---
