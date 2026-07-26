# Sections 1 and 4–7 — Product and Runtime Architecture

## Section 1 — Product Overview

### What the product does

`cp-next` is the customer control panel for Eagle 3D Streaming. It lets a customer take an
Unreal Engine application from a local ZIP archive to a managed pixel-streaming deployment,
then configure links, monitor live sessions, inspect usage, manage supporting files and
dedicated multiplayer servers, and administer billing and account access.

The source supports these product areas:

1. **Identity and account setup.** Email/password and Google authentication, email and
   username availability checks, one-time-password (OTP) verification, username/profile
   completion, password reset, team invitations, and profile management.
2. **Streaming application lifecycle.** Upload a Windows or Linux Unreal build, validate the
   ZIP, upload it to CoreWeave/Google Cloud Storage/Cloudflare R2, enqueue backend processing,
   watch processing progress through Firestore, upload a thumbnail, keep versions, and delete
   apps or versions.
3. **Streaming configuration and links.** Create normal or meeting links, select app
   versions and configurations, edit runtime/user-interface/developer settings, and manage
   2D-image and video assets.
4. **Dedicated multiplayer servers.** Upload dedicated-server apps, start and stop server
   instances, inspect IP/port/location data, and monitor recent server activity.
5. **Operations and analytics.** Display live streaming sessions from Firestore, kick a
   player, display active dedicated servers, inspect stream records, calculate concurrent
   users (CCU), chart usage, and export table data.
6. **Commercial management.** Show trial resources, legacy PPCCU/PPL/PPM/prepaid-minute
   plans, newer Core/GB/minute subscriptions, create Stripe checkout sessions through backend
   APIs, revise resources, cancel/reactivate plans, change payment methods, and list invoices.
7. **Developer controls.** View/rotate account and streaming API keys, mint custom tokens,
   and enable or disable version control.

### Business purpose

The control panel is a self-service layer over Eagle 3D Streaming's authentication, storage,
payment, upload-processing, and streaming infrastructure. Its business value is reducing
manual support work: customers can deploy and operate Unreal workloads, buy capacity, and
diagnose running sessions without an Eagle employee performing each operation.

The repository does not contain revenue reporting, internal sales workflows, backend
provisioning algorithms, Stripe products/prices, Firebase security rules, or infrastructure
code. Those concerns must not be inferred as frontend responsibilities.

### Target users and roles

| Role | How the source identifies it | Capabilities visible in this repository |
|---|---|---|
| Anonymous visitor | No valid session cookie; `/signin`, `/signup`, `/repass`, `/fb-mail` | Create/sign in to an account and reset a password. |
| Authenticated account owner | `email === ownerEmail` in `useUserInfo()` | Full navigation, developer section, team management, plan/account changes, uploads, configuration, and monitoring. |
| Team member/guest | `selectedTeamMember` in `localStorage`, or current email differs from owner email | Work against the selected member's API key; owner-only account/developer/team actions are hidden or rejected in several components. |
| Account admin | `loginStatus.data.userinfo.isAdmin` | Live-stream monitor does not hide sessions flagged `e3ds_employee`; no broader admin permission system is visible. |
| “SuperAdmin” URL user | Base64 `userInfo` query parameter creates a local `selectedTeamMember` with `isSuperAdminAccess` | Impersonates an account through an API key stored in `localStorage`. This is a client-side credential shortcut, not a backend-validated role in this repository. |

Authorization is inconsistent at the presentation layer. The sidebar hides the developer
section from non-owners, `DeveloperSectionPage` redirects non-owners, `TeamPage` blocks
non-owners, and many plan/account buttons compare `email` with `ownerEmail`. The backend must
still enforce every permission because client checks are bypassable.

### Main user journeys

#### New account

`/signup` → email availability → password validation → username availability → OTP send and
verify → account creation → password sign-in → session-cookie generation → username update →
`/username` for phone/lead-source completion → `/`.

Google sign-up uses Firebase Authentication, sends the Firebase ID token to the auth service,
creates a cookie, and navigates directly to `/`.

#### Returning user

`/signin` → AGW email/password sign-in or Firebase Google popup → receive ID token → send
Bearer token to the auth service's session endpoint → backend sets a cookie →
`window.location.href` to `/` or a requested redirect.

#### Upload a streaming app

`/` → Add Application → name and ZIP selection → optional thumbnail → local ZIP inspection →
storage-provider lookup → signed URL or R2 multipart initialization → upload → app executable
metadata API → Firestore waiting-queue document → real-time Firestore processing document →
download/extract/test/rename completion → cache refetch, notification, and success UI.

#### Configure and share an app

Open an app card → create streaming or meeting link → select a configuration and version →
copy/open link or iframe → edit configuration tabs → attach image/video assets → save config
through AGW.

#### Operate a dedicated server

`/multiplayer-server` → upload app or open an existing card → start server → wait for list
refetch → copy `IP:port` or stop the server. The monitor tab subscribes to Firestore and only
shows documents updated during the last ten minutes.

#### Inspect usage

`/analytics` → choose a date range → POST stream-record query → view session rows, totals,
bar/pie groupings, or a CCU interval calculation → optionally export CSV.

#### Buy or change a plan

`/plans` → monthly/yearly Core selection or legacy plan dialog → backend creates a checkout
session → browser leaves for hosted payment → returns to success/failure route → success route
performs plan-specific follow-up → home/account displays the new subscription.

### Core frontend architecture

```text
App Router route entry
  → feature component
    → custom aggregation hooks
      → generated RTK Query hooks
        → one shared RTK Query API cache
          → Axios base query
            → external Eagle services

Client feature
  ↔ local React state / feature context
  ↔ Firebase real-time listener
  ↔ browser storage and cookies
```

The architecture is feature-oriented at the component layer but service-oriented at the data
layer:

- `src/app` owns URL routing and layouts.
- `src/components/features` owns product screens and workflows.
- `src/components/providers` owns cross-component workflow state.
- `src/hooks` composes API responses into product concepts.
- `src/store/api` owns HTTP request definitions and RTK Query cache tags.
- `src/config` owns endpoints and environment switching.
- `src/services` owns direct storage-provider integrations.
- `src/components/ui` is the shadcn/Radix design-system base.

Nearly all meaningful interaction runs in the browser. Route pages are statically rendered,
but client components hydrate and fetch data after mount. There is no frontend Backend for
Frontend (BFF), Server Action, route handler, or server-side API proxy.

### Frontend architecture philosophy

The code suggests, but does not explicitly document, these intentions:

- Thin route files and feature-heavy component modules.
- A shared UI kit built from shadcn/ui and Radix primitives.
- RTK Query as the default REST cache and request abstraction.
- React Context for long-running, multi-screen upload/config workflows.
- Firestore for event-driven/live operational state.
- CSS variables plus Tailwind utilities for themes and visual consistency.
- Backward compatibility while migrating from legacy subscription models to Core/GB/minute
  products.

The source does not establish why these choices were made historically. The coexistence of
legacy and V2 hooks, commented code, duplicated flows, and placeholder modules shows an
ongoing migration rather than a fully consolidated target architecture.

### Backend communication and backend assumptions

Most requests target one of three configured systems:

- **AGW** (`AGW_BASE_URL`): account metadata, payments, subscriptions, storage, assets,
  uploads, analytics, notifications, and dedicated-server operations.
- **Auth REST service** (`AUTH_BASE_URL`): cookie/session state, API keys, teams, username,
  account deletion, token generation, and profile flags.
- **Direct storage/Firebase/external services:** signed Azure/GCS URLs, R2 multipart URLs,
  four Firebase apps, geolocation, Telegram-style notification service, documentation, and
  download links.

Inferred backend contracts:

- AGW commonly returns nested envelopes shaped like
  `{ data: { status, data } }`, which becomes even more nested after the Axios/RTK wrapper.
- The auth service sets and reads a cross-origin session cookie; calls that need it use
  `withCredentials: true`.
- Many AGW calls accept `apiKey` in the JSON body; R2 management calls put it in an `apiKey`
  header.
- Payment endpoints return hosted checkout/setup URLs. No Stripe JavaScript SDK is used.
- Upload processing workers consume Firestore collection `0_waitingQueue` and write status to
  a per-username collection.
- The client assumes API authorization is enforced by the services. That enforcement cannot
  be verified here.

### Section 1 closeout

**Key takeaways**

- This is an operations and commerce control panel for Unreal pixel-streaming customers.
- The browser coordinates several independent services; it is not a server-rendered BFF.
- “Legacy” and “V2/Core” subscription models both remain active.
- Role enforcement in this repository is UI-level and must be backed by server checks.

**Common pitfalls**

- Treating “owner”, `isAdmin`, team selection, and SuperAdmin access as the same concept.
- Editing only the visible component without tracing its aggregation hooks and RTK endpoint.
- Assuming payment, Firebase, or upload semantics can be proven without their backend code.
- Assuming existing `docs/SUPER_ADMIN_LOGIN.md` matches the current implementation.

**Questions to ask a mentor**

- Which subscription model is the long-term source of truth?
- Is SuperAdmin URL access approved for production, and where is it audited server-side?
- Which service owns authorization for each `apiKey`-based request?
- What are the supported storage providers and the migration plan from CoreWeave/GCS to R2?
- Which Firebase collections and schemas are contractual?

**Related files to read next**

- `src/app/layout.tsx`
- `src/app/(withSidebarLayout)/layout.tsx`
- `src/components/providers/providers.tsx`
- `src/store/api/base-api.ts`
- `src/config/api.ts`
- `src/components/providers/upload/upload-sequence-provider.tsx`

**Practical exercises**

- Trace one `/analytics` request from the route to its configured URL.
- Draw the two subscription models and list the hooks used by each.
- Follow a successful upload from file selection to the Firestore success event.

---

## Section 4 — Application Startup

### From `pnpm dev:staging` to the first page

There is no plain `dev` script. The supported commands are:

```bash
pnpm dev:staging
pnpm dev:prod
```

Both run Next.js with Turbopack on port `6400`; `cross-env` sets `NEXT_PUBLIC_ENV` to
`staging` or `production`.

#### 1. Package-manager and CLI startup

`pnpm` resolves the script in `package.json`, `cross-env` injects the public environment
selector, and `next dev --turbopack -p 6400` boots Next.js. `pnpm-workspace.yaml` is not a
multi-package workspace list; it only allowlists install-time builds for Firebase utilities,
protobuf, Sharp, and resolver binaries.

#### 2. Configuration loading

Next reads:

- `.env` automatically. Values are statically inlined when referenced through
  `process.env.NEXT_PUBLIC_*`.
- `next.config.ts`, which allows `next/image` to optimize an HTTPS image from any hostname.
- `tsconfig.json`, including strict TypeScript, JSX preservation, bundler module resolution,
  and `@/* → ./src/*`.
- `postcss.config.mjs`, which activates Tailwind CSS v4 and Autoprefixer.
- `eslint.config.mjs` during production builds.

`src/config/config-loader.ts` imports both JSON configuration files at build time and selects
staging only when `NEXT_PUBLIC_ENV === 'staging'`; every other value, including missing or
misspelled values, selects production.

#### 3. Route resolution

The App Router maps the requested pathname into `src/app`. Parenthesized folders are route
groups and do not appear in the URL. There is no `middleware.ts`, so no application
middleware executes before the route.

#### 4. Root layout

`src/app/layout.tsx` is a Server Component. It:

1. Loads global Tailwind/theme CSS.
2. configures the Google-hosted Asap font through `next/font/google`;
3. emits metadata (`Control Panel - Eagle`);
4. inserts Google Tag Manager before interactivity;
5. emits the no-script GTM iframe;
6. schedules Hotjar after interactivity;
7. renders the client `Providers` boundary.

#### 5. Root providers

`Providers` mounts:

```text
React Redux Provider (module-level store)
  → next-themes ThemeProvider
    → SuperAdminHandler
    → matched route
    → ConsentBanner
    → Sonner Toaster
    → UnCaughtErrorCatcher
```

RTK Query starts with an empty cache. There is no server cache hydration or persisted Redux
rehydration. Theme defaults to `light`, uses a class on `<html>`, and suppresses hydration
warnings at the root.

Firebase configuration modules initialize four named Firebase apps when imported:
`accounts`, `streamMonitor`, `dsMonitor`, and `appExeData`.

#### 6. Public route startup

Public route pages render directly under the root providers. `/signin`, `/signup`, and
`/username` are thin Server Component wrappers around client feature components.
`/repass` and `/fb-mail` are client page modules. Public pages still run the global
SuperAdmin handler, analytics scripts, consent banner, toaster, and uncaught-error catcher.

#### 7. Protected route startup

All protected-looking routes share
`src/app/(withSidebarLayout)/layout.tsx`, a Client Component. It:

1. checks whether the current URL uses an old control-panel/home domain and performs a full
   navigation to `CURRENT_CP_URL`;
2. redirects the legacy `EAGLE_DOMAIN` host to `/analytics`;
3. runs login-status and API-key RTK queries;
4. requests a customer ID after those requests finish;
5. redirects an unsigned user to `/signin`;
6. redirects a signed user with no username to `/username`;
7. shows a full loader until all checks settle;
8. mounts the sidebar/header and workflow providers;
9. finally renders the route's feature component.

This is client-side gating. Static HTML/JavaScript for a route can still be served before the
browser runs the check.

#### 8. Feature and data initialization for `/`

For the home route:

```text
page.tsx
  → StreamingAppPage
    → legacy subscription queries
    → trial/user-info query
    → streaming app and thumbnail queries
    → loader until those requests settle
    → SubscriptionSummarySection
    → StreamingAppSection
      → Application
```

The surrounding sidebar layout already mounted upload/config providers. Those providers also
start queries, so identical RTK Query cache keys are deduplicated while different payload
shapes create distinct entries.

### Server Components, Client Components, hydration, and caches

- Route wrappers and the root layout are Server Components unless marked otherwise.
- The sidebar layout is a Client Component, making its imported subtree part of the client
  graph even when some descendants omit a correct directive.
- Next.js prerenders the route shell. React then hydrates client components.
- RTK Query starts browser requests during component execution/effects after the provider is
  available; data is not prefetched on the server.
- Firebase listeners attach in `useEffect`.
- `localStorage`, `sessionStorage`, `window`, and browser cookies are read only on the client.
- RTK Query provides the only general request cache. Firestore maintains its own real-time
  client cache. `getStorageProvider()` adds a process/module-level one-value cache.

### Section 4 closeout

**Key takeaways**

- Use `dev:staging` or `dev:prod`; there is no `npm run dev`.
- Environment selection is build-time and defaults to production.
- No middleware, server data preload, or Redux hydration exists.
- The protected shell waits on three client-side auth/account requests.

**Common pitfalls**

- Expecting route authorization to occur before static content is served.
- Changing `.env` or JSON config without restarting the dev server/rebuilding.
- Forgetting that global analytics run on public and authenticated routes.
- Adding a provider outside the existing client boundary without considering hydration.

**Questions to ask a mentor**

- Should development default to staging rather than production when the selector is absent?
- Is the three-request protected-layout gate intentionally sequential?
- Should GTM/Hotjar wait for consent?
- Should Firebase initialize lazily by feature?

**Related files to read next**

- `package.json`
- `src/config/config-loader.ts`
- `src/app/layout.tsx`
- `src/components/providers/providers.tsx`
- `src/app/(withSidebarLayout)/layout.tsx`

**Practical exercises**

- Add timestamps locally to the layout queries and measure time-to-content.
- Disable JavaScript and observe what a protected static route exposes.
- Change `NEXT_PUBLIC_ENV` and verify the selected endpoint JSON in a local build.

---

## Section 5 — Rendering Strategy

### Verified route table

The staging production build completed successfully with Next.js 15.5.21. Every route was
reported as `○ (Static) — prerendered as static content`.

| Route | Entry | Build strategy | Route JS | First-load JS | Client behavior after hydration |
|---|---|---:|---:|---:|---|
| `/` | `(withSidebarLayout)/page.tsx` | Static shell | 52.6 kB | 577 kB | Auth gate, subscription/trial/assets, Firebase/config/upload UI. |
| `/_not-found` | generated from `not-found.tsx` | Static | 133 B | 103 kB | Redirects to `/`. |
| `/additional-uploads` | `additional-uploads/page.tsx` | Static shell | 9.23 kB | 252 kB | Lists and uploads supplementary files. |
| `/analytics` | `analytics/page.tsx` | Static shell | 114 kB | 343 kB | Fetches stream records and renders tables/charts. |
| `/credit-card-failed` | `credit-card-failed/page.js` | Static shell | 843 B | 125 kB | Client home button. |
| `/credit-card-successful` | `credit-card-successful/page.tsx` | Static shell | 811 B | 125 kB | Client home button. |
| `/credit-card-successful-core` | corresponding `page.tsx` | Static shell | 3.94 kB | 184 kB | Reads `sessionId`; confirms payment-method change. |
| `/developer-section` | `developer-section/page.tsx` | Static shell | 9.14 kB | 199 kB | Owner check and developer APIs. |
| `/fb-mail` | `fb-mail/page.tsx` | Static shell | 4.21 kB | 187 kB | Suspended search-param password confirmation. |
| `/multiplayer-server` | `multiplayer-server/page.tsx` | Static shell | 8.07 kB | 455 kB | Dedicated-server data and Firebase monitoring. |
| `/payment-failed` | `payment-failed/page.tsx` | Static shell | 839 B | 125 kB | Client home button. |
| `/payment-successful` | `payment-successful/page.tsx` | Static shell | 3.98 kB | 178 kB | Reads payment parameters and performs follow-up mutations. |
| `/plans` | `plans/page.tsx` | Static shell | 21.9 kB | 243 kB | Subscription/pricing queries and checkout actions. |
| `/repass` | `repass/page.tsx` | Static shell | 3.14 kB | 186 kB | Password-reset request form. |
| `/signin` | `signin/page.tsx` | Static shell | 4.88 kB | 347 kB | Firebase + AGW sign-in and cookie creation. |
| `/signup` | `signup/page.tsx` | Static shell | 10.9 kB | 359 kB | Multi-step signup/OTP/Firebase workflow. |
| `/team` | `team/page.tsx` | Static shell | 6.73 kB | 239 kB | Owner-gated team APIs. |
| `/username` | `username/page.tsx` | Static shell | 7.82 kB | 242 kB | Profile-completion form. |
| `/utilities` | `utilities/page.tsx` | Static shell | 5.5 kB | 402 kB | Two Firebase monitors and download tools. |

Shared first-load JavaScript is 103 kB. No route uses ISR, request-time SSR, a Server Action,
or server-side dynamic rendering in this snapshot.

### Why routes are static

This is a consequence of the source structure:

- Route Server Components do not call request-bound APIs such as `cookies()` or `headers()`.
- There are no server-side `fetch` calls, dynamic route segments, or revalidation exports.
- All user-specific data is requested by client hooks.
- Search parameters are read by Client Components.

The repository does not state whether static rendering was an explicit product decision or an
emergent result of moving all data access to the client.

### Server Components and Client Components

Server Components are used mainly as route/layout wrappers. Their benefits here are limited
to emitting static HTML/metadata and excluding simple wrapper code from the client.

Client Components own:

- Redux and RTK Query;
- authentication redirects;
- React state/forms;
- Firebase auth and Firestore;
- browser storage/cookies;
- charts/tables/dialogs;
- uploads and progress;
- almost every feature screen.

Several modules begin with `'use-client'`, which is only a harmless string expression, not the
valid `'use client'` directive. They currently enter the browser bundle through the valid
client sidebar layout/provider boundary. Importing one from a pure Server Component would
surface build errors or invalid hook usage.

### Suspense, streaming, hydration, and lazy loading

- Explicit React `Suspense` appears only on `/fb-mail`, around the component that calls
  `useSearchParams`.
- There are no route `loading.tsx` files, so App Router has no route-level streaming
  fallback.
- No `next/dynamic`, `React.lazy`, or deliberate feature-level code splitting is present.
- Loading states are component loaders, table loaders, or mutation button states after
  hydration.
- Static HTML is followed by hydration of a large client tree; then user-specific data
  arrives and replaces loader/empty states.

### Performance interpretation

The home route's 577 kB first load reflects a broad client boundary and eager imports of
upload, Firebase, React Hook Form, subscription, and configuration code. Dedicated servers
and utilities similarly pull in monitoring/upload dependencies. Route-level static
generation does not by itself make these routes lightweight.

### Section 5 closeout

**Key takeaways**

- All routes are verified static shells with client data fetching.
- There is no ISR or request-time SSR.
- Only `/fb-mail` uses explicit Suspense; no route-level streaming fallbacks exist.
- Home, multiplayer, and utilities are the heaviest initial bundles.

**Common pitfalls**

- Calling a route “SSR” merely because Next prerenders Client Component HTML.
- Assuming static output means data is cached at build time.
- Adding request-specific data to a Server Component without revisiting deployment behavior.
- Importing an invalidly marked hook/provider outside the current client graph.

**Questions to ask a mentor**

- Are bundle budgets defined?
- Should authenticated data move to Server Components or remain browser-only?
- Which large dialogs can be loaded dynamically?
- Is static export a supported deployment target? The current Next config does not declare it.

**Related files to read next**

- Build scripts in `package.json`
- All route files under `src/app`
- `src/app/(withSidebarLayout)/layout.tsx`
- `src/components/features/streaming-app/application.tsx`

**Practical exercises**

- Run both environment builds and compare route tables.
- Add a bundle analyzer locally and identify the largest dependencies in `/`.
- Convert one modal to `next/dynamic` and measure its route bundle.

---

## Section 6 — App Router

### Layouts and execution order

There are two layouts:

1. `src/app/layout.tsx` applies to every route.
2. `src/app/(withSidebarLayout)/layout.tsx` applies only to routes inside that route group.

There is no layout under `(with-public-layout)`; the folder only groups URLs without changing
the URL or rendering tree.

For `/analytics`, execution/render order is:

```text
RootLayout (Server Component)
  → Providers (Client boundary)
    → SidebarLayout (Client auth/layout)
      → Analytics page wrapper (Server Component reference in route tree)
        → AnalyticsPage (Client Component)
```

For `/signin`, order is:

```text
RootLayout
  → Providers
    → signin/page.tsx
      → SignInPage
```

### Every route

| Path | Page responsibility | Shared sidebar layout? | Primary feature |
|---|---|---:|---|
| `/` | Streaming applications dashboard | Yes | `StreamingAppPage` |
| `/additional-uploads` | Supplementary-file list/upload | Yes | `AdditionalUploadsPage` |
| `/analytics` | Stream usage and CCU analytics | Yes | `AnalyticsPage` |
| `/credit-card-failed` | Card-setup failure feedback | Yes | Inline page |
| `/credit-card-successful` | Card-setup success feedback | Yes | Inline page |
| `/credit-card-successful-core` | Core card-change confirmation | Yes | Inline page + payment mutation |
| `/developer-section` | Keys, tokens, version control | Yes | `DeveloperSectionPage` |
| `/multiplayer-server` | Dedicated-server apps/monitor | Yes | `DedicatedServerPage` |
| `/payment-failed` | Checkout failure feedback | Yes | Inline page |
| `/payment-successful` | Checkout success and follow-up | Yes | `PaymentSuccessfulPage` |
| `/plans` | Current pricing experience | Yes | `AllPan` |
| `/team` | Team membership | Yes | `TeamPage` |
| `/utilities` | Live monitors and downloads | Yes | `UtilitiesPage` |
| `/fb-mail` | Complete reset with `oobCode` | No | Inline reset form |
| `/repass` | Request reset email | No | Inline reset form |
| `/signin` | Sign in | No | `SignInPage` |
| `/signup` | Sign up | No | `SignupPage` |
| `/username` | Required profile completion | No | `UsernamePage` |

`src/app/not-found.tsx` immediately invokes `redirect('/')`. There are no custom templates,
route error boundaries, or route loading files. `UploadError.tsx` is a feature component
despite its name; it is not App Router's reserved `error.tsx`.

### Route-group consequences

- Parenthesized names never appear in public URLs.
- The sidebar layout wraps payment-result routes as well as main app routes. A payment result
  therefore waits for auth/API/customer-ID checks.
- `/username` is outside the sidebar group so a signed-in user missing a username can complete
  onboarding without being redirected by the same guard.
- Public routes still inherit global Redux, theme, analytics, consent, SuperAdmin, toast, and
  uncaught-error components.

### Navigation behavior

- Internal menu entries use `next/link`.
- The guard uses `router.push`; developer access uses `router.replace`.
- Several flows deliberately force full document navigation with `window.location.href` or
  `window.open(..., '_parent'/'_self')`, especially auth, domain migration, and payment.
- Documentation and support links open new tabs.
- Not-found performs a server redirect to `/`.

### Section 6 closeout

**Key takeaways**

- Route files are intentionally thin; feature code is elsewhere.
- One root layout and one client sidebar layout define the entire route hierarchy.
- Reserved loading/error/template files do not exist.
- Payment callback pages are inside the authenticated sidebar group.

**Common pitfalls**

- Creating a URL containing a route-group folder name.
- Mistaking `UploadError.tsx` for an App Router error boundary.
- Placing profile completion inside the sidebar group and creating a redirect loop.
- Forgetting that a full navigation resets in-memory Redux state.

**Questions to ask a mentor**

- Should payment callbacks require the full sidebar/auth bootstrap?
- Should public routes have a dedicated visual layout?
- What should a real 404 show instead of redirecting home?
- Which routes need route-level loading and error boundaries first?

**Related files to read next**

- `src/app/layout.tsx`
- `src/app/(withSidebarLayout)/layout.tsx`
- every `page.tsx` listed in the route table
- `src/config/page.ts`

**Practical exercises**

- Add a temporary route and predict its layout tree before running it.
- Trace every full-page navigation and decide whether state loss is intentional.
- Prototype an `error.tsx` for `/analytics`.

---

## Section 7 — Request Flow

### Protected page visit

```text
Browser requests /analytics
  ↓
Next.js serves statically prerendered route HTML and JS
  ↓
No middleware executes
  ↓
RootLayout emits metadata, GTM, Hotjar, and Providers
  ↓
React hydrates Redux/theme/SuperAdmin/global utilities
  ↓
SidebarLayout starts:
  GET auth session state (credentials included)
  GET API keys (credentials included)
  POST customer ID after identity/key data
  ↓
Loader remains visible
  ↓
Unsigned → client navigation to /signin
Missing username → client navigation to /username
Valid account → sidebar/header/workflow providers render
  ↓
AnalyticsPage starts POST stream-record request
  ↓
Axios response interceptor unwraps response.data
  ↓
RTK Query stores response in its cache
  ↓
React rerenders table, statistics, CCU data, and charts
```

Files involved in the analytics example:

- `src/app/layout.tsx`
- `src/components/providers/providers.tsx`
- `src/store/store.ts`
- `src/store/api/base-api.ts`
- `src/helpers/axios/axiosBaseQuery.ts`
- `src/helpers/axios/axiosInstance.ts`
- `src/app/(withSidebarLayout)/layout.tsx`
- `src/store/api/auth.ts`
- `src/store/api/api-key.ts`
- `src/store/api/user-info.ts`
- `src/app/(withSidebarLayout)/analytics/page.tsx`
- `src/components/features/analytics/analytics-page.tsx`
- `src/store/api/analytics.ts`
- `src/config/api.ts`
- analytics tabs/charts/table files.

### REST request flow

Generated hook → endpoint definition → `baseApi` → `axiosBaseQuery` → `axiosInstance` →
configured external URL.

`axiosBaseQuery` defaults to JSON, optionally applies headers and `withCredentials`, and is
supposed to convert thrown Axios errors into `{ error }`. However, the response interceptor
returns an Axios error object instead of rejecting it. Non-2xx responses can therefore be
classified as successful RTK data. Callers frequently inspect nested status fields because
transport failure handling is unreliable.

RTK Query has no global retry policy, focus/reconnect refetch configuration, persisted cache,
or server hydration. It uses normal endpoint subscription lifetimes and tag invalidation.

### Authentication request flow

Email/password:

```text
SignInPage
  → POST AGW sign-in {mail, pass}
  → obtain Firebase-style idToken
  → POST auth session/token with Authorization: Bearer <idToken>
     and credentials enabled
  → backend sets cookie (attributes unknown)
  → full navigation to destination
  → SidebarLayout GET session/state with credentials
```

Google:

```text
Firebase signInWithPopup(accounts app)
  → user.getIdToken()
  → same auth session/token endpoint
```

### Upload request/event flow

Uploads combine REST, direct object storage, and Firestore:

1. Inspect ZIP locally with `unzipit`.
2. GET storage provider once and cache it.
3. For R2: initiate multipart, fetch six signed part URLs per batch, upload XHR parts with
   three exponential-backoff attempts, and complete multipart.
4. For other providers: request primary and alternate signed URLs; upload with Axios PUT.
5. If speed stays below 0.5 MB/s for 90 seconds, cancel once and retry the alternate URL.
6. Notify/upload-log/upload-complete calls execute.
7. Upload executable metadata.
8. Create `0_waitingQueue/<username_app_version>`.
9. Subscribe to `<username>/<app_version>` in Firestore.
10. Convert Firestore fields into Downloading/Extracting/Testing/Tested and error states.
11. On `uploadSystemFile`, refetch relevant caches and show success/failure.
12. Every three minutes without progress, send a stuck notification; after twenty minutes of
    accumulated stall time, mark the UI failed.

### Firebase live-monitor flow

`StreamingAppMonitor` subscribes to a per-username collection in the stream-monitor Firebase
project. It filters employee sessions for non-admins and drops stale continuation/start
timestamps. `MultiplayerAppMonitor` uses the dedicated-server Firebase project and keeps
documents logged within ten minutes. Unsubscribe functions run when dependencies change or
components unmount.

### Browser-to-browser response and rerender

API responses enter the RTK Query cache. Any component subscribed to the identical
endpoint/serialized arguments rerenders. Mutations invalidate selected cache tags; many
mutations without tags rely on manual `refetch()`. Firestore snapshot callbacks set local
state directly. Context providers cause all consumers of a changed context value to rerender
because context values are not memoized.

### Section 7 closeout

**Key takeaways**

- A protected page performs authentication entirely after static delivery/hydration.
- REST state, Firestore state, browser state, and local workflow state are separate channels.
- The upload flow is the most cross-system request path.
- Axios error semantics are currently broken at the interceptor layer.

**Common pitfalls**

- Reading the `API` constant without checking environment selection.
- Assuming a mutation invalidates everything it affects.
- Forgetting direct storage PUTs and Firestore events when debugging an “API upload.”
- Treating an RTK `data` value as proof of HTTP success.

**Questions to ask a mentor**

- What is the canonical API response envelope?
- Which calls are idempotent and safe to retry?
- What backend job transitions the upload Firestore document?
- Which cache tags are contractual and which manual refetches are accidental?

**Related files to read next**

- `src/helpers/axios/axiosInstance.ts`
- `src/helpers/axios/axiosBaseQuery.ts`
- `src/store/api/*`
- `src/components/providers/upload/upload-sequence-provider.tsx`
- `src/services/r2/*`

**Practical exercises**

- Force a 4xx response and inspect whether RTK reports `data` or `error`.
- Trace one cache invalidation from mutation to rerender.
- Diagram an upload failure before and after the storage PUT completes.

