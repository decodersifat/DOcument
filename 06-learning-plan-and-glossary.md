# Sections 24–25 and 28 — Learning Roadmap, 14-Day Plan, and Glossary

## Section 24 — Recommended Learning Roadmap

### Phase 0 — Establish a safe local baseline

**Goal:** know that failures are yours rather than setup drift.

1. Read the onboarding index, product overview, environment section, and code-review
   executive summary.
2. Install with the team-approved Node and pnpm versions. The repository does not
   pin them, so ask before standardizing.
3. Create `.env.local` from `.env.example` using approved staging Firebase values.
4. Confirm `NEXT_PUBLIC_ENV=staging`.
5. Start on port 6400, sign in with a non-production test account, and capture the
   network hosts being called.
6. Run a production-style staging build. The audit verified that the Next build
   succeeds, while reporting a large lint-warning backlog.

Do not begin by changing Super Admin, billing, uploads, or production environment
configuration.

### Phase 1 — Learn the framework shell

Read in this order:

1. `src/app/layout.tsx`
2. `src/components/providers.tsx`
3. `src/app/(withSidebarLayout)/layout.tsx`
4. every `src/app/**/page.*`
5. `src/components/sidebar/app-sidebar.tsx`
6. `src/components/navbar/site-header.tsx`

Be able to explain static prerendering, client hydration, route groups, layout
inheritance, the session gate, provider order, global scripts, and why no
middleware means no server-side route guard in this repository.

Suggested first change: a copy-only or spacing fix in a leaf component with no API
or business-rule impact.

### Phase 2 — Learn identity, data, and state

Read:

1. `store/index.ts`, `store/api/base-api.ts`, `helpers/axios/*`
2. `config/config-loader.ts`, `config/api.ts`, `config/firebase.ts`
3. `store/api/auth.ts`, `user-info.ts`, `asset.ts`, and one billing module
4. `hooks/use-user-info.ts` and `hooks/use-subscription.ts`
5. sign-in and signup feature trees
6. Super Admin implementation and its legacy documentation

Trace an email sign-in, Google sign-in, query cache hit, mutation invalidation,
selected-team-member switch, 401, and logout. Treat the Axios error interceptor,
unvalidated redirect, and Super Admin credentials as known hazards.

Suggested next change: type a small query/mutation and normalize its error display.

### Phase 3 — Learn the main product domains

Study one vertical slice at a time:

| Domain | Route | Start files | External systems |
|---|---|---|---|
| Streaming apps | `/` | streaming-app `application.tsx`, app list/cards, config, upload context | AGW, Firebase app-exe data, signed storage |
| Analytics | `/analytics` | analytics application, chart helpers, table | AGW stream record |
| Dedicated servers | `/multiplayer-server` | dedicated-server application/cards/context | AGW, DS monitor Firebase |
| Utilities/monitoring | `/utilities` | utility application and monitor tables | Two monitor Firebase apps, geolocation |
| Billing | `/plans` | plan application, subscription hooks, dialogs | AGW payment/Stripe-backed APIs |
| Team/account | `/team` plus navbar account modal | team application, account components, auth/user APIs | Auth and AGW |
| Developer access | `/developer-section` | API-key/token components | Auth API |

For each slice, draw page → component → hook/context → endpoint/service → cache or
listener → UI. Use `04-file-catalog.md` to find direct dependencies and consumers.

### Phase 4 — Master uploads and billing

Uploads and billing are the two most consequential areas.

For uploads, learn ZIP inspection rules, plan/storage gating, provider selection,
primary/fallback transport, R2 multipart, progress mapping, Firestore queues,
processing statuses, cancellation, stuck detection, and notification side effects.
Then split one non-UI concern behind tests; avoid a large rewrite.

For billing, learn legacy plan priority, Core/GB/minute products, trial derivation,
quote/create/upgrade/downgrade/cancel/unpause transitions, owner restrictions,
payment result routes, and tag invalidation. Verify all money/entitlement decisions
server-side.

### Phase 5 — Improve engineering safety

Recommended order:

1. Fix Axios rejection semantics with transport tests.
2. Add same-origin redirect validation.
3. Retire or redesign Super Admin credential links.
4. Add route-level error boundaries and audited error reporting.
5. Establish Vitest/React Testing Library or the team-selected equivalent.
6. Type API contracts and remove high-risk `any`s.
7. Resolve hook dependency warnings based on behavior, not blanket suppression.
8. Break the dedicated-server import cycle.
9. Decompose upload workflow.
10. Reconcile package manager, PM2 port, semi-prod output, and CI secret logging.

### What a new engineer should not touch first

- Production/staging origins or Firebase projects.
- Super Admin URL generation/use.
- Subscription pricing, status priority, or storage entitlement calculations.
- Upload queue paths, retry thresholds, or processing status strings.
- Account deletion, ownership transfer, API-key rotation, kick-player, or server
  start/stop calls.
- Axios response behavior without a regression test.
- Global provider order or payment callback route grouping.
- “Unused” assets/modules before checking dynamic string references and roadmap
  intent.

### Section 24 closeout

**Key takeaways**

- Learn the shell, then identity/data, then one vertical slice.
- Uploads and billing require deliberate study before modification.
- The safest early work reduces ambiguity and adds tests.
- The catalogue and diagrams are reading maps, not substitutes for running flows.

**Common pitfalls**

- Starting with the largest file.
- Using a production account because production is the fallback.
- Refactoring across several state systems at once.
- Fixing warnings mechanically without understanding stale-closure behavior.

**Questions to ask a mentor**

- Which staging accounts cover owner, member, trial, and paid states?
- Which production incidents are most common?
- Which review owners are required for upload, auth, and billing changes?
- What automated test stack should be standardized?

**Related files to read next**

- `docs/onboarding/README.md`
- `01-product-and-runtime.md`
- `03-data-auth-and-configuration.md`
- `07-code-review.md`

**Practical exercises**

- Trace one route end to end and present it in ten minutes.
- Pair with a mentor to reproduce one safe staging mutation.
- Add one test that would have caught a documented issue.

---

## Section 25 — Detailed 14-Day Onboarding Plan

Assume focused engineering days. Each day ends with a small written artifact or
demonstration so a mentor can correct misunderstandings early.

### Day 1 — Product, setup, and safety

- **Concepts:** product users, streaming applications, dedicated servers, plans,
  teams, environment boundaries, frontend versus backend assumptions.
- **Read:** onboarding index; Sections 1, 19, 20, and 26; `README.md`,
  `.env.example`, `package.json`, config JSON files.
- **Hands-on:** install reproducibly, configure staging, start on port 6400, visit
  all public routes, record selected origins without recording credentials.
- **Debugging:** distinguish missing Firebase variables, missing `cross-env`, a
  rejected cookie, and a production-fallback mistake.
- **Interview prompts:** What problem does the product solve? What is not proven by
  this frontend? Why is `NEXT_PUBLIC_` not secret?
- **Exercise/deliverable:** a one-page system/context map and a verified setup
  checklist.

### Day 2 — App Router and rendering

- **Concepts:** root layout, route groups, page conventions, static prerender,
  hydration, client boundary, metadata, `next/font`, `next/script`.
- **Read:** all `src/app` files; Sections 4–6; route and layout diagrams.
- **Hands-on:** map every URL to page and first feature component; inspect server
  HTML before hydration.
- **Debugging:** explain why a route can be static although it shows personalized
  client data; test the 404.
- **Interview prompts:** Do parentheses affect the URL? Where should
  `loading.tsx`/`error.tsx` go? Why is there no SSR user data?
- **Exercise/deliverable:** annotated route table with public/protected and provider
  inheritance.

### Day 3 — Providers, navigation, and reusable UI

- **Concepts:** provider ordering, Context reach, sidebar composition, shadcn/Radix
  ownership, design tokens, theme hydration.
- **Read:** `components/providers.tsx`, protected layout, sidebar/navbar trees,
  `components/ui`, Sections 8, 14, and 15.
- **Hands-on:** use React DevTools to observe provider/component rerenders; change a
  non-semantic spacing token locally.
- **Debugging:** identify a missing-provider error and a hardcoded dark-mode color.
- **Interview prompts:** Why is provider order an API? When should a component be
  shared versus UI? Is the theme toggle reachable?
- **Exercise/deliverable:** provider tree plus a proposal to make one provider
  route-specific.

### Day 4 — REST transport and RTK Query

- **Concepts:** endpoint injection, query serialization, mutations, `.unwrap()`,
  tags, invalidation, refetch, Axios interceptors.
- **Read:** store root, every `store/api` module, Axios helpers, API config,
  Section 11.
- **Hands-on:** trace `useStreamingAppQuery` and one plan mutation in browser
  DevTools; inspect cache state in Redux DevTools.
- **Debugging:** mock/reproduce a non-2xx response and explain why it can appear as
  data.
- **Interview prompts:** Why RTK Query? How do tags work? What does the Axios
  instance return?
- **Exercise/deliverable:** typed request/response proposal for one small endpoint
  and a transport test case.

### Day 5 — Authentication, teams, and Super Admin threat model

- **Concepts:** identity token versus application session, cookie credentials,
  client route gate, owner/member selection, impersonation risk, redirect safety.
- **Read:** auth API, sign-in/signup, protected layout, `useUserInfo`,
  `SuperAdminHandler`, Section 12 and old Super Admin doc.
- **Hands-on:** trace email and Google flows using staging; switch a permitted team
  member; inspect storage cleanup on logout.
- **Debugging:** expired session, missing username, invalid continue URL, stale
  selected member.
- **Interview prompts:** What is the security boundary? Why is the current Super
  Admin link unsafe? How would you prevent an open redirect?
- **Exercise/deliverable:** concise threat model and a same-origin redirect
  validator design.

### Day 6 — Streaming-app screen and configuration

- **Concepts:** app asset normalization, app/version cards, thumbnails, generated
  launch links, configuration defaults, UUID/anonymized links, React Hook Form.
- **Read:** streaming-app feature tree, config components/context, app-link and
  asset APIs, default config constants.
- **Hands-on:** list an app, open config, compare create/edit data, generate a safe
  test link.
- **Debugging:** missing thumbnail, stale config query, invalid config JSON, deleted
  app version.
- **Interview prompts:** Why use React Hook Form only here? How are configs cached?
  What does V6 anonymization do?
- **Exercise/deliverable:** data-flow diagram from application card to saved URL.

### Day 7 — Upload preparation and transport

- **Concepts:** ZIP traversal, Windows/Linux validation, plan/storage gates, signed
  URLs, progress, abort, throughput detection, R2 multipart concurrency/retry.
- **Read:** upload provider first half, R2 services, upload hooks/helpers, Sections
  7 and 16.
- **Hands-on:** inspect sample ZIP structures without uploading production data;
  calculate progress and multipart behavior.
- **Debugging:** invalid root executable, Linux shell mismatch, low-speed fallback,
  cancelled part, exhausted retry.
- **Interview prompts:** Why upload directly to storage? Why six parts concurrently?
  What makes a retry safe?
- **Exercise/deliverable:** unit-test matrix for ZIP inspection and transport
  failures.

### Day 8 — Upload processing and realtime state

- **Concepts:** Firestore paths/listeners, queue identity, processing statuses,
  notification/refetch, stuck timing, page-refresh recovery.
- **Read:** upload provider second half, Firebase config, upload error/progress UI,
  app-exe API.
- **Hands-on:** observe a mentor-approved staging upload and correlate UI,
  Firestore, REST, and network events.
- **Debugging:** listener never terminates, 3-minute warning, 20-minute failure,
  browser refresh mid-process.
- **Interview prompts:** Why are Context and Firestore both needed? What cleanup is
  mandatory? How would resumability work?
- **Exercise/deliverable:** explicit upload state machine with terminal/retryable
  states.

### Day 9 — Analytics and live monitors

- **Concepts:** dated stream records, TanStack Table, Recharts, CCU sweep-line
  calculation, listener freshness filters, admin visibility.
- **Read:** analytics feature/charts, utilities feature, monitor hooks/components,
  analytics and location endpoints.
- **Hands-on:** verify table/chart filters against the same data; inspect a live
  listener cleanup.
- **Debugging:** malformed chart CSS variable, hardcoded user-name substitution,
  stale session threshold, average CCU bias.
- **Interview prompts:** How is concurrency computed? Why is an omitted zero interval
  important? What is client filtering versus authorization?
- **Exercise/deliverable:** corrected duration-weighted average CCU algorithm with
  tests.

### Day 10 — Dedicated servers and additional uploads

- **Concepts:** DS application package lifecycle, server instance start/stop,
  location/player actions, DS realtime monitor, simpler signed uploads.
- **Read:** dedicated-server and additional-upload feature/provider trees, upload
  API endpoints, confirmed circular dependency.
- **Hands-on:** trace list/upload/delete without changing live resources; map the
  instance monitor.
- **Debugging:** stale DS snapshot, failed completion callback, cycle-induced
  coupling, duplicate filename.
- **Interview prompts:** How do DS and streaming-app workflows differ? Why break the
  component cycle? Where must authorization be enforced?
- **Exercise/deliverable:** extract a proposed shared type/action module that breaks
  the cycle.

### Day 11 — Plans, subscriptions, and payments

- **Concepts:** legacy plan priority, Core/GB/min plans, trial, quotes, checkout,
  non-checkout changes, cancel/reactivate/unpause, payment methods.
- **Read:** plan/subscription trees, all payment/storage/subscription hooks and APIs,
  business-logic tables.
- **Hands-on:** use fixtures or read-only staging accounts to enumerate UI states;
  never create charges without explicit approval.
- **Debugging:** plan-name ternary bug, hardcoded storage entitlement, combined trial
  completion, stale tags, callback under auth gate.
- **Interview prompts:** Which plan wins? Which decisions belong on the server? What
  is mutation invalidation?
- **Exercise/deliverable:** transition table covering owner/member, trial, active,
  past-due, cancel, and upgrade cases.

### Day 12 — Forms, accessibility, errors, and performance

- **Concepts:** controlled forms, validation boundaries, semantic labels, route
  boundaries, error normalization, bundle splitting, memoization.
- **Read:** Sections 13, 17, and 18; all forms/dialogs; global error catcher;
  build-output route sizes.
- **Hands-on:** keyboard-test two dialogs and one form; profile a large route; force
  a safe render error locally.
- **Debugging:** stale effect closure, uncaught JSON parse, missing focus behavior,
  heavy initial bundle.
- **Interview prompts:** What does `window.onerror` miss? When does memoization help?
  Where should validation live?
- **Exercise/deliverable:** prioritized accessibility/performance/error-boundary
  mini-audit.

### Day 13 — Testing, CI/CD, and maintainability

- **Concepts:** test pyramid, contract tests, deterministic fixtures, lockfiles,
  build artifacts, PM2, secret hygiene, dead-code proof.
- **Read:** workflows, root verifier, package/process config, dependency graph, code-review
  findings.
- **Hands-on:** run the verified build path; design tests for Axios, auth redirect,
  and one business-rule function.
- **Debugging:** npm/pnpm drift, missing `cross-env`, semi-prod `build/` mismatch,
  PM2 port conflict, env log leakage.
- **Interview prompts:** What blocks safe refactoring? What belongs in CI? How do you
  prove a file is dead?
- **Exercise/deliverable:** staged CI proposal: install, typecheck/lint, unit,
  component, build, secret-safe verification.

### Day 14 — Capstone and readiness review

- **Concepts:** end-to-end reasoning, blast radius, migration sequencing, incident
  ownership, communicating uncertainty.
- **Read:** all diagrams, glossary, file entries for the chosen vertical slice, all
  review findings.
- **Hands-on:** implement one mentor-approved low-risk improvement with a test,
  build it, and manually exercise affected states.
- **Debugging:** narrate diagnosis from symptom to component, state source,
  transport, backend boundary, and observable evidence.
- **Interview prompts:** explain the app at beginner/intermediate/senior depth; defend
  a priority decision; identify facts versus assumptions.
- **Exercise/deliverable:** 20-minute architecture walkthrough, reviewed PR, and a
  personal 30/60/90-day learning plan.

### Section 25 closeout

**Key takeaways**

- Each day couples reading with observation, debugging, and a concrete artifact.
- The order minimizes production risk while building end-to-end understanding.
- Mentor checkpoints are most important before auth, upload, and billing work.
- Day 14 proves reasoning, not mere file familiarity.

**Common pitfalls**

- Treating the plan as a checklist without running the app.
- Using real charges, deletions, or server actions as exercises.
- Skipping written artifacts that expose misunderstanding.
- Waiting until Day 14 to ask domain questions.

**Questions to ask a mentor**

- Which exercises need a sandbox account or fixture?
- What should the capstone PR target?
- Which incidents or postmortems can supplement the code?
- What defines readiness for independent on-call work?

**Related files to read next**

- The “Read” list under each day.
- `04-file-catalog.md` for any unfamiliar path.
- `08-interview-bank.md` for daily knowledge checks.

**Practical exercises**

- Schedule the 14 mentor checkpoints.
- Track unknowns in an evidence/assumption/owner table.
- Repeat the plan at reduced scope for a future new domain.

---

## Section 28 — Project Glossary

### Product and account terms

| Term | Meaning in this repository |
|---|---|
| Eagle 3D Streaming / E3DS | The product/company represented by the control panel. |
| CP | Control Panel; this Next.js application. |
| Previous CP / old panel | Legacy control-panel origin used for redirects or links. |
| Current CP | The configured origin for this application. |
| Streaming application | A packaged Unreal Engine application uploaded, processed, configured, and launched through the platform. |
| App | Context-dependent shorthand for a streaming application, dedicated-server package, Firebase app, or the web application; qualify it in new code. |
| Application version | A named uploaded build beneath a streaming application. |
| Executable / EXE | The Windows launch binary discovered inside an uploaded ZIP. |
| Engine / Linux engine | The Linux executable candidate paired with a root shell script. |
| Shell / `.sh` | Linux launch script; upload validation expects a naming relationship to the app. |
| Plugin | Detected packaged integration/content in an uploaded application; not a Codex/browser plugin. |
| PS2 | A project-specific marker detected during ZIP inspection; its business expansion is not defined in source. Ask the upload/backend owner. |
| Dedicated server / DS | Server-side application package and runtime instance managed from `/multiplayer-server`. |
| Multiplayer server | User-facing route name for dedicated-server functionality. |
| Instance | A running dedicated-server allocation identified by backend/network fields. |
| Additional upload | A user file uploaded outside the primary streaming-app package flow. |
| Asset 2D | User-uploaded image-style asset managed through signed URLs. |
| Asset video | User-uploaded video asset. |
| Thumbnail | Image associated with a streaming app/version. |
| Profile logo | Account/profile image stored through the logo asset endpoints. |
| Configuration / config | Saved runtime options used to construct a streaming launch URL. |
| App link / launch URL | URL assembled from application, version, configuration, and generated identity fields. |
| V6 anonymized link | Link format built with base64-encoded/anonymized information by the current URL logic. |
| UUID | Universally Unique Identifier; generated for launch-link/session-style identity. |
| Team owner | Session user whose email matches the team/account owner email. |
| Team member | Invited user who may operate within an owner’s account according to backend permissions. |
| Selected team member | Browser-stored target identity/API key used by `useUserInfo` to scope product operations. |
| Super Admin | Client impersonation mechanism that decodes credentials from a link; not a safe authorization design in its current form. |
| Username | Product-facing unique account identifier used heavily in payloads and Firestore paths. |
| Customer ID | Payment/customer identifier obtained from AGW. |
| API key | Credential returned by Auth and sent with many product requests. |
| Streaming API key | Separate key returned alongside the normal API key. |
| Developer token | Token created in the developer section through the external token-creation endpoint. |
| Version control | Developer metadata updated through the Auth user-info endpoint; not Git itself in this context. |
| Notification | Control-panel message fetched from AGW; also sometimes means an operational chat notification sent by helpers. |

### Billing and entitlement terms

| Term | Meaning in this repository |
|---|---|
| Subscription | Recurring or usage-backed product entitlement returned by billing APIs. |
| Legacy plan | PPCCU, PPL, prepaid minute, or PPM product family. |
| Core plan | Newer coded base product, displayed as monthly or yearly. |
| GB plan | Newer storage/data subscription measured in gigabytes. |
| Min plan | Newer streaming-minute subscription. |
| PPCCU | Legacy plan acronym used throughout code; pricing UI treats it as a concurrent-user style product. The full official expansion is not present—confirm with product. |
| PPL | Legacy plan acronym; the official expansion is not defined in this repo. |
| PPM | Pay Per Minute legacy product. |
| Prepaid minute | Legacy minute balance bought in configured increments with auto-recharge behavior. |
| CCU | Concurrent users/connections; analytics derives simultaneous stream count over time. |
| Plan priority | UI conflict resolution: PPCCU before PPL before prepaid minute before PPM. |
| Trial | Temporary Core/minute/storage entitlement derived from backend product/status data. |
| `trialing` | Stripe-style subscription status considered active by UI logic. |
| `active` | Paid/usable subscription status. |
| `past_due` | Payment-delinquent status that still requires explicit product handling. |
| Quote | Backend-calculated price/credit effect before upgrade or downgrade. |
| Checkout | Hosted payment/setup flow initiated by a backend-generated session or URL. |
| Non-checkout subscription | Create/change request that uses an existing payment method without the initial hosted checkout. |
| Upgrade | Increase a plan quantity/tier through a plan-specific endpoint. |
| Downgrade | Decrease plan quantity/tier, often through the shared downgrade endpoint. |
| Cancel | End or schedule termination of one/all subscriptions. |
| Reactivate | Reverse a legacy cancellation using the reactivate-all endpoint. |
| Unpause | Resume newer subscription products through the unpause endpoint. |
| Auto-renew / auto-recharge | Billing metadata controlling automatic prepaid-minute replenishment. |
| Watcher | Backend PPM operation added through `/add-watcher`; exact backend semantics are not documented locally. |
| Payment method | Stored card/payment instrument referenced through AGW; raw card handling is not implemented by a browser Stripe SDK here. |
| Default payment method | Backend-designated primary instrument. |
| Setup card | Backend operation that begins payment-method setup. |
| Invoice | Billing record listed through the invoice endpoint. |
| Budget | Optional usage cap controlled through add/increase/decrease/remove operations. |
| Storage utilized | Backend-reported R2 storage usage. |
| Storage entitlement | Purchased/free GB allowance used to gate uploads; current frontend derivation contains known inconsistencies. |

### Framework and rendering terms

| Term | Meaning in this repository |
|---|---|
| Next.js | React application framework, version 15 in the audited package. |
| App Router | `src/app` routing system based on layouts, pages, route groups, and special files. |
| Route group | Parenthesized folder organizing layout inheritance without adding a URL segment. |
| Root layout | `src/app/layout.tsx`, the required document/provider/script shell. |
| Page | `page.tsx`/`page.js` route entry. |
| 404 / not-found | Global unmatched-route UI from `not-found.tsx`. |
| Server Component | Default App Router component evaluated outside the browser unless it is under/imported across a client boundary. |
| Client Component | Module rooted by an exact `"use client"` directive and executed/hydrated in the browser. |
| Client-bound module | Module without its own directive that becomes part of a client graph because a Client Component imports it. |
| Hydration | React attaching client behavior to prerendered HTML. |
| Static prerender | Build-time route-shell generation; all audited routes were marked static. |
| SSR | Server-Side Rendering per request; this repo does not fetch personalized page data on the server. |
| CSR | Client-Side Rendering/fetching after hydration; the dominant personalized-data model here. |
| RSC | React Server Components; `components.json` enables the shadcn convention, while most meaningful feature behavior is client-side. |
| Metadata | Next title/description/icon configuration exported by root layout. |
| `next/font` | Next-managed font loading; root uses Google Asap and a separate unused helper loads local Spantaran. |
| `next/script` | Script lifecycle API used for GTM and Hotjar. |
| `beforeInteractive` | Script strategy that runs early, used by GTM. |
| `afterInteractive` | Post-hydration script strategy, used by Hotjar. |
| Turbopack | Next development bundler enabled by dev scripts. |
| Route boundary | `loading.tsx`, `error.tsx`, `global-error.tsx`, or template convention; only not-found exists currently. |

### React, state, and form terms

| Term | Meaning in this repository |
|---|---|
| Hook | React function beginning `use` that consumes state/lifecycle/context or packages reusable logic. |
| `useState` | Component-local state; widely used for forms, dialogs, selections, and progress. |
| `useEffect` | Synchronizes with external state/listeners; build warnings identify many incomplete dependencies. |
| Stale closure | Callback/effect reading old props/state because dependencies are incomplete. |
| Context | React tree-scoped value used for upload and configuration workflows. |
| Provider | Component making Context/store/theme services available to descendants. |
| Redux | Central client state container; local state usage is small here. |
| Redux Toolkit / RTK | Redux's standard store and query tooling. |
| RTK Query | REST request/cache layer built into the store. |
| Query | Cached read operation keyed by endpoint and serialized argument. |
| Mutation | Imperative write operation; often invalidates cache tags. |
| `.unwrap()` | Converts an RTK mutation trigger result into a promise that resolves with data or rejects with the RTK error. |
| Tag | RTK Query cache label used to connect mutations with queries needing refetch. |
| Invalidation | Marking tagged cache entries stale and refetching active consumers. |
| Refetch | Explicitly repeat a query independent of normal cache reuse. |
| Server state | Remote API/Firestore data; should not be needlessly duplicated in local stores. |
| Local state | Ephemeral component-owned UI data. |
| Workflow state | Multi-step progress/cancellation/error state owned by upload Contexts. |
| React Hook Form / RHF | Form library used for the large streaming-config form. |
| Controlled input | Input whose value is managed by React state. |
| Derived state | Value computed from queries/props instead of independently stored. |
| Memoization | `useMemo`, `useCallback`, or `memo` reuse; helps only when dependencies and consumer identity make it valuable. |
| Strict Mode | React development checks that can expose unsafe effects; ensure listener/request cleanup is idempotent. |

### Data, transport, storage, and realtime terms

| Term | Meaning in this repository |
|---|---|
| API | Application Programming Interface; primarily AGW and Auth HTTP services. |
| AGW | API gateway/base origin for product, upload, profile, analytics, and billing endpoints. The official acronym expansion is not in source. |
| Auth service | Separate base origin for sessions, teams, account changes, and developer credentials. |
| REST | HTTP endpoint style used by the two backend services. |
| Axios | HTTP client used by RTK base query and direct upload/service calls. |
| Interceptor | Shared Axios request/response transformation. Current response-error interceptor wrongly resolves errors. |
| Base query | RTK adapter that converts endpoint descriptors into Axios calls and `{data|error}`. |
| Payload/body | JSON argument sent to most POST/PUT/DELETE endpoints. |
| `withCredentials` | Browser request setting that includes/accepts cross-origin cookies. |
| Bearer token | Identity token in `Authorization` used to create the application cookie. |
| Session cookie | Backend-issued browser credential; its security attributes are not visible in frontend code. |
| CORS | Browser cross-origin request policy; must permit configured origins/credentials on the backend. |
| CSRF | Cross-Site Request Forgery; cookie-authenticated write protection is backend-owned and undocumented here. |
| Signed URL | Time-limited storage URL permitting a direct file `PUT` without proxying bytes through the app server. |
| CoreWeave | One configured storage/backend provider name returned as `coreweave`. |
| R2 | Cloudflare object storage path used by multipart upload and provider-specific list/delete APIs. |
| Multipart upload | R2 process: initiate, obtain part URLs, upload pieces, collect ETags, complete. |
| Part | Byte range uploaded independently in a multipart session. |
| ETag | Storage response identifier required to complete multipart upload. |
| Concurrency | Number of simultaneous part uploads; current R2 flow uses six. |
| Exponential backoff | Increasing delay between retry attempts; parts retry three times. |
| AbortController | Browser cancellation primitive used for uploads. |
| Upload throughput | Measured bytes/time; primary signed upload falls back below 0.5 MB/s for 90 seconds. |
| Storage provider cache | Module-level promise/value preventing repeated provider lookups for the page lifetime. |
| Firebase | Client SDK used for Google Auth and four named Firestore projects. |
| Firebase app | Named initialized SDK instance: accounts, streamMonitor, dsMonitor, or appExeData. |
| Firestore | Realtime document database supplying upload and monitor snapshots. |
| `onSnapshot` | Firestore subscription callback; must be unsubscribed on cleanup. |
| Snapshot | Realtime representation of a document/collection at an update point. |
| Waiting queue | Firestore upload-processing queue location keyed by username/app/version. |
| Realtime monitor | UI fed by Firestore snapshots rather than REST polling. |
| Stale session | Monitor record older than configured freshness thresholds. |
| Normalization | Conversion of raw asset blobs into UI-friendly grouped data. |

### UI, styling, analytics, and tooling terms

| Term | Meaning in this repository |
|---|---|
| Tailwind CSS | Utility CSS framework, version 4, imported/configured primarily in `globals.css`. |
| shadcn/ui | Source-owned component convention configured by `components.json`. |
| Radix UI | Accessible unstyled primitives wrapped by many UI components. |
| Design token | CSS variable representing color/radius/font semantics. |
| OKLCH | Perceptual CSS color format used for light/dark tokens. |
| Dark mode | Class-driven token variant provided by `next-themes`; toggle component appears unreferenced. |
| CVA | `class-variance-authority`, used to define typed visual variants. |
| `cn()` | Local helper combining `clsx` and `tailwind-merge`. |
| Sonner | Toast notification library. |
| Lucide | SVG icon library configured for shadcn and feature icons. |
| TanStack Table | Headless data-table state/rendering library used by analytics and tables. |
| Recharts | React chart library used for analytics visualization. |
| Sweep line | Event-sorting algorithm used to derive concurrent stream count over time. |
| GTM | Google Tag Manager; inserted before interactive. |
| Hotjar | Behavior/session analytics script inserted after interactive. |
| Consent banner | UI preference prompt that currently does not gate GTM/Hotjar loading. |
| PII | Personally Identifiable Information; email, username, IP/geolocation, and device details require careful telemetry handling. |
| CSP | Content Security Policy; no CSP/security headers are configured in `next.config.ts`. |
| XSS | Cross-Site Scripting; especially damaging because keys/impersonation data live in browser-readable storage. |
| Open redirect | Redirect to an attacker-controlled origin; current post-login target is not visibly allowlisted. |
| App Check | Firebase abuse-control mechanism; configuration/enforcement cannot be verified here. |
| ESLint | Static quality/lint tool configured with a warning-heavy current build. |
| Prettier | Code formatter with Tailwind/class plugins. |
| TypeScript | Strict typed language used throughout, with many local `any` escapes. |
| `any` | Type-system opt-out; high-risk around backend responses and complex workflows. |
| PostCSS | CSS transformation pipeline hosting Tailwind and Autoprefixer. |
| Autoprefixer | Adds browser vendor prefixes to CSS. |
| pnpm | Package manager represented by the committed lockfile. |
| npm | Package runner/manager used in scripts and CI despite pnpm lock presence. |
| Lockfile | Exact dependency resolution record (`pnpm-lock.yaml`). |
| PM2 | Node process manager configured by `ecosystem.config.json`. |
| CI/CD | Continuous Integration/Deployment workflows in `.github/workflows`. |
| Static export | Next output mode producing deployable static files; current config does not enable it despite semi-prod expecting `build/`. |
| Bundle | JavaScript/CSS shipped for a route; build output reports route and shared sizes. |
| Code splitting | Loading feature code in separate chunks rather than initial bundles. |
| Dead code | Module not used at runtime; zero static imports are evidence, not proof. |
| Circular dependency | Import loop; confirmed between two dedicated-server components. |
| Telemetry | Operational/error/activity data sent to analytics or notification services. |
| Sentry | Common audited error platform not currently installed; mentioned as a possible replacement for chat-based error reporting. |
| SOP | Standard Operating Procedure; `docs/sop.md` contains existing project guidance. |

### Upload status vocabulary

| Status/term | Meaning |
|---|---|
| Selecting | User chooses a ZIP and the client inspects its directory structure. |
| Uploading | Browser sends file bytes to signed storage; mapped roughly to 0–50% in the combined UI. |
| Waiting | Metadata has been registered and backend processing is queued. |
| Downloading | Processing system is retrieving the uploaded archive; displayed around 60%. |
| Extracting | Backend is unpacking/inspecting the archive; displayed around 70%. |
| Testing | Backend stream/app test is running; displayed around 90%. |
| Tested | Successful terminal processing status; displayed as 100%. |
| Alternate upload | One fallback attempt through an alternate signed URL after sustained low speed. |
| Stuck | No adequate progress; the UI emits periodic warnings and eventually fails after roughly 20 minutes. |
| Virus/crash/invalid executable | Backend-derived terminal failure classes interpreted by upload UI. |
| Completion callback | REST notification that a direct storage upload finished. |

### Section 28 closeout

**Key takeaways**

- Several acronyms are project labels whose official expansion is absent; do not
  invent one.
- “Auth,” “app,” “notification,” and “config” are overloaded and should be
  qualified in design discussions.
- Client terms such as role, gate, or hidden control do not imply backend
  authorization.
- Upload and billing vocabulary encodes meaningful state machines.

**Common pitfalls**

- Expanding PPCCU, PPL, AGW, or PS2 without product confirmation.
- Confusing Firebase identity with the backend session.
- Calling every remote value Redux state.
- Treating signed URLs as permanent public URLs.

**Questions to ask a mentor**

- What are the official expansions of AGW, PPCCU, PPL, and PS2?
- Which terms have customer-facing names different from code names?
- Which upload statuses are a stable backend contract?
- Which legacy-plan terms can be retired?

**Related files to read next**

- `src/config/api.ts`
- `src/constant/*`
- `src/types/*`
- upload and subscription feature folders

**Practical exercises**

- Explain ten terms without using another undefined acronym.
- Add a missing verified domain term when encountered.
- Replace one ambiguous new identifier with a qualified name.
