# Section 26 — Full Code Review

## Executive assessment

The application has broad, working product coverage and a coherent high-level
stack: Next App Router, source-owned shadcn UI, RTK Query for REST, Firestore for
realtime data, and direct signed uploads. A staging-selected Next 15 production
build completed successfully during this audit.

The main engineering risk is not framework choice. It is the combination of
client-only security decisions, a transport interceptor that can turn HTTP failures
into data, untyped backend contracts, very large effect-heavy upload code, no
automated tests, and deployment/configuration drift. Auth/impersonation, billing,
and upload changes currently have a high regression blast radius.

Priority labels:

- **P0:** credible credential/session or authorization exposure; stop expanding the
  behavior and remediate urgently.
- **P1:** can produce incorrect security, billing, entitlement, or core-workflow
  behavior.
- **P2:** material reliability, maintainability, accessibility, or performance
  issue.
- **P3:** cleanup, consistency, or developer-experience improvement.

## P0 findings

### 26.1 Super Admin puts operational credentials in URLs and browser storage

- **Evidence:** `src/components/auth/super-admin-handler.tsx`,
  `src/utils/super-admin-login.ts`, and `docs/SUPER_ADMIN_LOGIN.md`.
- **Observed behavior:** a base64 query value is decoded to email, username, and API
  key; all local/session storage is cleared; credentials are written to
  `selectedTeamMember`; local audit data is written; the query remains in the URL.
  Base64 is encoding, not encryption.
- **Impact:** browser history, copied links, screenshots, referrers, analytics,
  session replay, extensions, or XSS may disclose a reusable API key. The visible
  payload has no signature, expiry, nonce, scope, or single-use exchange. Local
  audit logs are mutable.
- **Documentation defect:** the old doc says the parameter is removed, while the
  implementation retains it.
- **Recommendation:** stop issuing this format. Use an authenticated backend to
  mint a short-lived signed one-time code, exchange it for a scoped impersonation
  session, audit centrally, remove the query immediately, display impersonation
  state, require explicit exit, and revoke/rotate exposed keys.
- **Verification:** threat-model review; replay/expiry/scope tests; confirm no key
  appears in URL/history/referrer/GTM/Hotjar/logs; centralized audit check.

### 26.2 Post-login redirect accepts an untrusted target

- **Evidence:** `src/components/features/signin/signin-page.tsx` parses `redirect`
  and decoded `continueUrl`, then assigns `window.location.href`.
- **Impact:** crafted login links can send a user to an attacker-controlled site
  after successful authentication, enabling phishing and credential-context abuse.
- **Recommendation:** accept relative application paths only, or validate protocol
  and origin against an explicit allowlist. Normalize before comparison and reject
  protocol-relative, encoded, or nested redirect variants.
- **Verification:** tests for internal paths, external HTTPS, `//host`, encoded
  origins, malformed base64/JSON, and nested redirects.

### 26.3 Client route/role checks are being asked to carry security meaning

- **Evidence:** no `middleware.ts` or server authorization; protected layout and
  scattered owner/member/admin JSX conditions; selected-member username/API key is
  browser-controlled.
- **Impact:** hiding a route/control cannot authorize account deletion, plan
  changes, key rotation, player kicks, or dedicated-server operations. A modified
  client can call endpoints directly.
- **Recommendation:** inventory every sensitive endpoint and verify server-side
  session, target-account membership, owner/admin permission, input validation, and
  audit. Treat frontend checks as UX only.
- **Verification:** negative integration tests for anonymous, unrelated member,
  permitted member, owner, and expired impersonation sessions.

## P1 findings

### 26.4 Axios errors can be classified as successful RTK data

- **Evidence:** `src/helpers/axios/axiosInstance.ts` response rejection handler
  `return error`; request rejection handler has the same pattern.
- **Mechanism:** returning resolves the interceptor promise. `axiosBaseQuery` then
  returns `{data: result}`, so hooks may not enter `isError` and `.unwrap()` may not
  reject.
- **Impact:** false success toasts, unsafe state transitions, missing session-expiry
  handling, and inconsistent response checks across all REST features.
- **Recommendation:** `return Promise.reject(error)`, define one normalized error
  shape, and migrate consumers that accidentally inspect error-shaped `data`.
- **Verification:** transport tests for network failure, timeout, 400, 401, 403,
  404, 409, 422, 500, and valid 2xx bodies before releasing the fix.

### 26.5 Core storage expiration is hardcoded off

- **Evidence:** `src/hooks/use-storageV2.ts` comments out the real comparison and
  sets `hasCoreStorageExp = false`.
- **Impact:** storage-limit warnings/gates cannot activate through this hook; users
  may begin an upload that the backend later rejects or bills unexpectedly.
- **Recommendation:** derive the flag from authoritative backend entitlements and
  normalized units; do not trust it as the backend enforcement layer.
- **Verification:** boundary tests below, equal to, and above allowance, including
  free/trial/paid and missing-data states.

### 26.6 Trial completion semantics conflict with consumers

- **Evidence:** `src/hooks/use-trial-v2.ts` defines `hasTrialFinish` as
  `hasMinFinish && hasStorageFinish`; streaming-app gating separately combines the
  overall flag with individual flags.
- **Impact:** a user with one exhausted dimension may be shown inconsistent upload
  availability, messaging, or plan prompts.
- **Recommendation:** name distinct facts (`isMinuteTrialExhausted`,
  `isStorageTrialExhausted`, `isAnyTrialLimitReached`,
  `areAllTrialBenefitsExhausted`) and encode one reviewed product policy.
- **Verification:** truth-table tests for all four boolean combinations and missing
  subscription data.

### 26.7 `BillSummary` contains an always-truthy plan-name branch

- **Evidence:** `src/components/features/subscription/bill-summary.tsx` uses
  `: PLAN_NAME.PREPAID_MINUTE ? ...`, testing a constant string rather than
  comparing `planName`.
- **Impact:** later plan labels fall into the prepaid branch and can show an
  incorrect billing summary.
- **Recommendation:** replace nested ternaries with an exhaustive map or switch over
  the plan enum; assert unknown plans.
- **Verification:** snapshot/unit cases for PPCCU, PPL, prepaid, PPM, Core, GB,
  minute, and unknown.

### 26.8 No automated test suite protects auth, billing, uploads, or analytics

- **Evidence:** no test files, test dependencies, or test scripts were found.
- **Impact:** the most stateful and financially sensitive paths rely on manual
  regression. Fixing the Axios interceptor or hook dependencies becomes dangerous.
- **Recommendation:** establish the team’s test stack and first cover pure business
  functions, transport behavior, auth redirect validation, plan-state matrices,
  ZIP inspection, upload state transitions, and critical components. Add a small
  Playwright-style smoke layer if approved.
- **Verification:** CI-enforced tests with deterministic fixtures and meaningful
  failure cases, not coverage percentage alone.

### 26.9 Upload orchestration is a non-persisted 1,200+ line client provider

- **Evidence:** `src/components/providers/upload/upload-sequence-provider.tsx`.
- **Responsibilities combined:** ZIP traversal/validation, subscription gating,
  provider resolution, primary/fallback upload, R2 coordination, progress,
  cancellation, REST metadata, Firestore listeners, timeout logic, notifications,
  refetching, and UI Context.
- **Impact:** stale closures, partial cleanup, duplicate side effects, difficult
  tests, and lost local state on refresh during long processing.
- **Recommendation:** extract pure ZIP validator; transport adapters; explicit
  reducer/state machine; processing repository; telemetry policy; persisted
  resumable job identity. Keep the provider as orchestration/composition.
- **Verification:** state-transition and cancellation tests plus reload/resume and
  duplicate-submission scenarios.

### 26.10 Client telemetry can disclose sensitive context

- **Evidence:** upload notification helpers and
  `UnCaughtErrorCatcher` include current URLs, usernames/emails, device/browser,
  error detail, IP/location-style context, application identifiers, and speed.
- **Impact:** secrets in Super Admin URLs and user operational data can reach a chat
  notification system. Retention, access, redaction, consent, and data residency are
  not encoded.
- **Recommendation:** create a reviewed telemetry schema, redact query strings and
  credentials, minimize PII, move privileged notification to backend, apply rate
  limits, and use an audited error platform.
- **Verification:** automated redaction tests and security/privacy owner approval.

### 26.11 Consent UI does not gate GTM or Hotjar

- **Evidence:** root layout inserts GTM `beforeInteractive` and Hotjar
  `afterInteractive`; `ConsentBanner` is rendered later inside client providers.
- **Impact:** tracking executes before a user choice, which may violate the intended
  consent policy.
- **Recommendation:** make script inclusion conditional on stored/managed consent,
  and define behavior for unknown, accepted, rejected, and revoked states.
- **Verification:** clean-browser network test proves no tracking request before
  opt-in where policy requires it.

### 26.12 Deployment definitions conflict with the application build

- **Evidence:** `.github/workflows/semi-prod.yml` runs `npm run build:prod` then
  syncs `build/`; `next.config.ts` does not enable static export, so audited builds
  create `.next/`. The workflow also uses npm while the exact lock is pnpm.
  `ecosystem.config.json` sets `PORT=3448`, while `start:prod` fixes `-p 6400`.
- **Impact:** failed or stale deployments, non-reproducible dependencies, and
  confusing runtime ports.
- **Recommendation:** choose server versus static-export deployment explicitly;
  use one package manager/frozen lock; align artifact path and port; pin Node; test
  workflow in an isolated environment.
- **Verification:** workflow artifact assertion, smoke URL, immutable version, and
  rollback drill.

### 26.13 Environment selection silently falls back to production

- **Evidence:** `config-loader.ts` selects staging only for the exact string
  `staging`.
- **Impact:** missing/typoed local or CI environment contacts production systems.
- **Recommendation:** fail builds outside an explicit allowed enum; consider a safe
  local/staging default for development policy.
- **Verification:** absent, `stage`, `staging`, and `production` configuration tests.

### 26.14 Configuration verifier prints environment-file contents

- **Evidence:** root `verify-build-config.ps1` reads and prints full
  `.env.production` and `.env.staging` contents.
- **Impact:** secrets can enter terminal capture, CI logs, support transcripts, or
  recordings if those files later contain non-public values.
- **Recommendation:** report file existence and required-name presence only; never
  echo values.
- **Verification:** seed recognizable canary values and assert they do not appear in
  output.

## P2 findings

### 26.15 Invalid client directives obscure the real boundary

- **Evidence:** upload providers and `use-common-upload.ts` use `'use-client'`
  instead of exact `'use client'`.
- **Current reason it works:** these modules are imported beneath valid client
  boundaries.
- **Impact:** moving/importing them from a server graph can fail unexpectedly; code
  review gives a false signal.
- **Recommendation:** use the exact directive when a module independently requires
  the client, or omit it deliberately and document that it is client-bound.
- **Verification:** Next build and boundary-specific import test.

### 26.16 Hook dependency warnings indicate stale-closure risk

- **Evidence:** the successful build reported many `react-hooks/exhaustive-deps`
  warnings across credit-card callbacks, streaming application, upload providers,
  monitor/table memoization, config form, asset uploads, and notifications.
- **Impact:** effects/callbacks may use stale API keys, handlers, form values, or
  mutation functions and may fail to resubscribe/cleanup correctly.
- **Recommendation:** address one behavior at a time: stabilize callbacks, include
  dependencies, move event-only work out of effects, and use reducers/refs only
  where semantics justify them. Do not blanket-disable the rule.
- **Verification:** Strict Mode, rerender with changed identity/props, and listener
  cleanup tests.

### 26.17 API contracts are predominantly `any`

- **Evidence:** endpoint arguments/responses and many feature records lack generics;
  the build prints extensive explicit-any warnings.
- **Impact:** response drift becomes runtime failure, transformations are
  speculative, and business logic accepts malformed values.
- **Recommendation:** obtain OpenAPI/schema ownership, generate types where
  possible, validate untrusted boundaries, and migrate endpoints feature by
  feature.
- **Verification:** typecheck plus contract fixtures representing success and every
  supported error envelope.

### 26.18 No React/App Router error recovery boundary

- **Evidence:** only `not-found.tsx`; no route `error.tsx`, `global-error.tsx`, or
  React error-boundary component. `window.onerror` reports but does not recover a
  render tree.
- **Impact:** render exceptions can blank a route and leave no localized retry.
- **Recommendation:** add route-group error UI and narrowly scoped boundaries around
  complex features; keep reporting sanitized.
- **Verification:** deliberately throwing render errors in route and widget scopes.

### 26.19 Unsafe JSON parsing remains in runtime paths

- **Evidence:** token/redirect/member/Super Admin helpers call `JSON.parse`; some
  paths are guarded while others rely on surrounding behavior.
- **Impact:** malformed URL or persisted values can crash an interaction or route.
- **Recommendation:** one safe parse/validation helper per external schema; clear
  invalid storage and provide recoverable UI.
- **Verification:** empty, malformed, oversized, wrong-shape, and version-old data.

### 26.20 Profile upload may make duplicate completion calls

- **Evidence:** common upload logic performs an upload-complete call, while the
  profile path also exposes/calls the user-info upload-complete mutation.
- **Impact:** duplicate backend processing, duplicate notification/cache
  invalidation, or race conditions depending on idempotency.
- **Recommendation:** define one owner for signed upload completion and require an
  idempotency key if repetition is possible.
- **Verification:** network assertion of exactly one completion request and safe
  retry behavior.

### 26.21 Analytics contains correctness and presentation defects

- **Evidence:** a chart color string is missing the closing `)` in
  `var(--color-primary`; username filtering special-cases `ahsan` to `Unknown2`;
  average CCU averages reported intervals rather than duration-weighting the entire
  selected window and can omit zero-concurrency time.
- **Impact:** broken colors, unexplained data mutation, and misleading averages.
- **Recommendation:** correct token syntax, remove or formally document server-side
  anonymization, and calculate time-weighted metrics over a defined window.
- **Verification:** fixed event fixtures with overlaps, gaps, zero streams, equal
  timestamps, and range boundaries.

### 26.22 Realtime monitor filtering is client-side policy

- **Evidence:** non-admin UI omits documents marked `e3ds_employee`; staleness is
  computed locally (stream records use start/continuation thresholds, DS uses a
  separate threshold).
- **Impact:** hidden records may still be readable from Firestore, and clock skew or
  inconsistent timestamps can change visibility.
- **Recommendation:** enforce Firestore read permissions and sensitive filtering in
  rules/backend; centralize time units and inject a clock for tests.
- **Verification:** Firebase Emulator rules tests and boundary-time fixtures.

### 26.23 Storage-provider selection is cached for the page lifetime

- **Evidence:** `services/storage-provider.ts` uses module-level cached state.
- **Impact:** backend provider migration or account switch in the same tab can leave
  list/delete/upload paths inconsistent.
- **Recommendation:** key the cache by account/session with defined expiry, or treat
  it as immutable session bootstrap and reset explicitly on identity change.
- **Verification:** owner/member switch and provider-change test.

### 26.24 Cache tags and response handling are inconsistent

- **Evidence:** several create mutations do not invalidate status/list queries;
  2D upload invalidates `logoList`; direct Axios completion relies on manual
  refetches; endpoint response shapes are read with different nesting assumptions.
- **Impact:** stale cards/status, redundant refetches, and UI coupled to accidental
  backend nesting.
- **Recommendation:** define tag matrix and adapters per domain; mutations should
  document exactly which active queries become stale.
- **Verification:** Redux cache-state tests after each mutation.

### 26.25 Dedicated-server components have a circular import

- **Evidence:** `features/dedicated-server/application.tsx` and
  `dedicated-server-card-details.tsx` import each other.
- **Impact:** fragile initialization, difficult isolated tests, and ownership
  ambiguity.
- **Recommendation:** extract shared types/actions/presentation into a third module
  with one-directional dependencies.
- **Verification:** static cycle check and isolated component imports.

### 26.26 Initial client bundles are heavy

- **Evidence:** verified build reported first-load JS up to roughly 577 kB for `/`,
  455 kB for `/multiplayer-server`, 402 kB for `/utilities`, and 359 kB for
  `/signup`; every protected route mounts all upload/config providers.
- **Impact:** slower parse/hydration, especially on lower-end devices, even when a
  feature does not use the providers.
- **Recommendation:** measure with a bundle analyzer; route-scope heavy providers;
  dynamically load charts, account dialogs, and rarely used panels; avoid importing
  entire feature graphs from shell components.
- **Verification:** before/after bundle output and Web Vitals on representative
  hardware.

### 26.27 Responsive and theme behavior is inconsistent

- **Evidence:** many layouts enforce `md:min-w-[1250px]`, colors are frequently
  hardcoded, dark tokens exist but the mode toggle has no internal importer, and
  typography token names reference fonts not all loaded.
- **Impact:** horizontal overflow, incomplete dark mode, and inconsistent design.
- **Recommendation:** define supported viewport matrix, replace hardcoded semantic
  colors, decide whether dark mode is a product feature, and align font tokens with
  loaded fonts.
- **Verification:** visual regression at phone/tablet/desktop and light/dark,
  keyboard, zoom 200%, and reduced-motion modes.

### 26.28 Accessibility coverage is uneven

- **Evidence:** source-owned Radix primitives provide a good base, but custom
  clickable containers, icon-only controls, tables/charts, status colors, and
  warning-heavy lint configuration require manual review.
- **Impact:** keyboard, screen-reader, focus, and color-dependent interactions may
  fail.
- **Recommendation:** restore meaningful lint rules, use semantic buttons/labels,
  provide chart/table equivalents, test dialogs/focus, and verify contrast.
- **Verification:** axe plus keyboard/screen-reader smoke tests on each main route.

### 26.29 Remote images and browser security headers are overly permissive/missing

- **Evidence:** `next.config.ts` permits HTTPS images from hostname `**` and defines
  no CSP or other security headers.
- **Impact:** unnecessary content origins, weaker containment of XSS/third-party
  scripts, and harder privacy review.
- **Recommendation:** inventory required asset domains and allowlist them; deploy a
  tested CSP plus HSTS, referrer, permissions, framing, and MIME policies appropriate
  to the hosting architecture.
- **Verification:** CSP report-only rollout, image regression, and security scan.

### 26.30 Payment result routes are inside the protected group

- **Evidence:** success/failure pages inherit the sidebar session gate.
- **Impact:** users returning without a valid cookie can be redirected away from
  the result; copy implies redirect behavior not consistently implemented.
- **Recommendation:** explicitly decide whether callbacks are public status pages,
  authenticated verification pages, or both. Never trust query parameters as proof
  of payment.
- **Verification:** return flows with valid, expired, blocked, and third-party-cookie
  scenarios.

## P3 findings

### 26.31 Dead, empty, or unreachable candidates need ownership

Static analysis found no internal importer for multiple components/hooks and found
empty API/slice placeholders. Strong candidates are listed in Section 21. Some UI
files may be shadcn inventory or framework roots, so deletion requires string,
dynamic-import, external-package, and roadmap checks.

### 26.32 Legacy and new subscription UI duplicate policy

Plan status, trial, storage, summary, dialog, and card rules are distributed across
many components and hooks. Introduce a typed domain model and pure selectors before
trying to merge UI trees.

### 26.33 Naming and file consistency reduce discoverability

Examples include `singup`, `use-storageV2`, `apiCall.ts`, mixed kebab/camel/Pascal
filenames, `use-client`, and overloaded “app/config/auth.” Rename incrementally
with import-safe tooling and avoid combining it with behavior changes.

### 26.34 Comments and old documentation can contradict source

`SUPER_ADMIN_LOGIN.md` is the strongest example. API comments label some endpoints
used/unused inaccurately. Treat comments as hypotheses and update them alongside
tests when behavior is confirmed.

### 26.35 Build/lint scripts need modernization

The verified `next build` works, but normal environment scripts depended on a
currently missing `cross-env` executable in the local install. `next lint` is a
fragile/obsolete path for newer Next versions; run ESLint directly after aligning
the team toolchain. Pin Node/package manager versions and use a frozen install.

## Refactor roadmap

### Stage 1 — Contain security and correctness risk

1. Disable/redesign Super Admin links and rotate exposed keys.
2. Validate login redirects.
3. Add backend authorization tests for sensitive endpoints.
4. Lock down/redact telemetry and gate analytics consent.
5. Write transport tests, then fix Axios rejection.
6. Fix the storage, trial, bill-summary, and analytics correctness bugs.

### Stage 2 — Create a safety net

1. Add deterministic unit/component test infrastructure.
2. Type and validate auth, asset, upload, and billing boundaries.
3. Add route error boundaries and standardized error envelopes.
4. Add Firestore Emulator rules/listener tests.
5. Make CI use a pinned runtime and frozen lockfile.

### Stage 3 — Decompose high-coupling domains

1. Extract upload pure validation, transports, state machine, processing listener,
   and telemetry.
2. Model subscription states/transitions in pure typed selectors.
3. Break the dedicated-server cycle.
4. Route-scope feature providers and split large bundles.
5. Centralize owner/member capability presentation while retaining backend policy.

### Stage 4 — Improve product quality and operations

1. Resolve effect/lint warnings feature by feature.
2. Establish viewport/theme/accessibility visual regression.
3. Remove proven-dead placeholders/constants/assets.
4. Reconcile deployment outputs, ports, package manager, and rollback.
5. Add observability with redaction, correlation IDs, and service ownership.

## Missing-test blueprint

| Layer | First tests |
|---|---|
| Pure unit | asset normalization, config serialization, URL validation, plan priority, trial/storage selectors, CCU sweep/duration average, ZIP structure validation. |
| Transport | Axios 2xx/non-2xx/timeout, RTK error shape, tag invalidation, provider-selected endpoint. |
| Component | sign-in redirect, username gate, owner/member controls, bill summary labels, dialogs/focus, upload error states. |
| Realtime | Firestore snapshot mapping, stale thresholds, employee filtering, unsubscribe, processing terminal states. |
| Integration | email/Google cookie exchange with mocked services, selected-member switch, signed-upload completion, subscription mutations. |
| End-to-end | public auth smoke, protected redirect, app list/config, safe fixture upload, analytics filters, read-only plan state; destructive/payment tests only in isolated sandboxes. |
| CI/deploy | frozen install, lint/typecheck/test/build, correct artifact path, no secret output, post-deploy smoke and rollback. |

## Build evidence

The audit ran Next 15.5.21 with `NEXT_PUBLIC_ENV=staging`. Google-font retrieval
required network access, after which the production build completed and all 18
listed routes were statically prerendered. The build did not prove backend
integration correctness; it did prove compilation and route generation. The
warning backlog included unused code, explicit `any`, and numerous hook dependency
issues and should be recorded as a baseline rather than silently accepted.

### Section 26 closeout

**Key takeaways**

- Credential-bearing Super Admin links, redirect validation, client-only
  authorization assumptions, and Axios error semantics are the highest priorities.
- Core entitlement/billing analytics logic has specific reproducible defects.
- No tests makes even correct refactoring hazardous.
- Upload decomposition should follow, not precede, behavior characterization.

**Common pitfalls**

- Fixing the Axios line without testing consumers of accidental error-shaped data.
- calling a UI permission check a security fix.
- rewriting upload and subscription domains simultaneously.
- deleting static-analysis candidates without dynamic-use proof.

**Questions to ask a mentor**

- Can current Super Admin links be revoked immediately?
- Where are backend authorization and API schemas owned?
- Which billing/upload states have reliable staging fixtures?
- Is semi-production deployment still an active requirement?

**Related files to read next**

- `src/components/auth/super-admin-handler.tsx`
- `src/helpers/axios/*`
- `src/hooks/use-storageV2.ts` and `use-trial-v2.ts`
- `src/components/providers/upload/*`
- `.github/workflows/*`

**Practical exercises**

- Turn the P0/P1 findings into owned tickets with acceptance tests.
- Write the Axios regression suite without changing production behavior.
- Demonstrate one entitlement bug with a pure fixture.
