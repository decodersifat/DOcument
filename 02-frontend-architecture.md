# Sections 2, 8–10, and 13–18 — Frontend Architecture

## Section 2 — Technology Stack

### Runtime and framework

| Technology | Why/where it is used | Alternatives | Why this choice |
|---|---|---|---|
| Next.js 15 | App Router, static prerendering, layouts, metadata, `next/image`, `next/font`, navigation, and build tooling. | Vite SPA, Remix, React Router, Astro. | The route/layout model and image/font pipeline fit the app. Historical decision rationale is not present. |
| React 19 | Components, hooks, context, Suspense, and hydration. | Vue, Svelte, Angular. | Required by the Next.js implementation; historical rationale is unknown. |
| React DOM 19 | Browser rendering and hydration through Next.js. | Framework-managed equivalents. | Paired runtime for React. |
| TypeScript 5.9 | Strict type checking for most source files. `allowJs` keeps one JavaScript route valid. | JavaScript, Flow. | Reduces contract errors, although extensive `any` weakens it. |

### UI, styling, and interaction dependencies

| Dependency | Use in this project | Plausible alternatives | Source-grounded rationale |
|---|---|---|---|
| Tailwind CSS 4 | Utility styling and CSS-variable token mapping in `globals.css`. | CSS Modules, Sass, styled-components. | Enables one tokenized utility system; no separate Tailwind config is required in v4. |
| `@tailwindcss/postcss`, PostCSS, Autoprefixer | Build Tailwind and add compatible CSS prefixes. | Lightning CSS-only pipeline. | Required by the selected CSS pipeline. |
| `tw-animate-css` | Animation utilities imported globally. | Tailwind plugin, Framer Motion, CSS keyframes. | Lightweight CSS animations; exact selection history is unknown. |
| shadcn/ui conventions | `components.json` defines New York style, stone base, CSS variables, RSC mode, and aliases. Components are source-owned under `components/ui`. | MUI, Chakra, Ant Design. | Full local control over component code and styling. |
| Radix UI packages | Accessible primitives for accordion, avatar, checkbox, collapsible, dialog, dropdown, label, popover, progress, radio, select, separator, slider, slot, switch, tabs, toggle, and tooltip. | Headless UI, React Aria. | shadcn components wrap these primitives. |
| Lucide React | Consistent SVG icon set throughout navigation and features. | Heroicons, custom SVGs. | Configured as the shadcn icon library. |
| class-variance-authority | Typed variants, notably buttons/badges/toggles. | Handwritten class maps. | Standard shadcn variant pattern. |
| `clsx` + `tailwind-merge` | `cn()` composes conditional classes and resolves Tailwind conflicts. | `classnames`, custom merging. | Standard shadcn utility. |
| `next-themes` | Class-based light/dark theme provider and Sonner theme. | Custom context, CSS media query only. | Avoids manual hydration-safe theme persistence. |
| Sonner | Success/error/info/warning toasts. | React Toastify, Radix Toast. | Used in nearly every mutation/upload workflow. |
| React Spinners | `ClipLoader`-style loading in shared loaders. | Lucide animated icon, CSS loader. | Existing loader implementation; rationale is not documented. |
| `input-otp` | Segmented OTP input during signup. | Normal text input, custom slots. | Improves OTP entry UX. |
| `react-phone-number-input` | International phone input in profile/onboarding. | `libphonenumber-js` + custom UI. | Provides country-aware entry; final validation is still a local regex. |

### State, data, tables, charts, and forms

| Dependency | Use | Alternatives | Why this choice |
|---|---|---|---|
| Redux Toolkit | Store configuration and one small user slice. | Zustand, Context, Jotai. | Primarily required for RTK Query. |
| React Redux | Provides the store and typed hooks. | Framework-specific stores. | React binding for Redux Toolkit. |
| RTK Query | All declared REST endpoints, cache tags, loading state, generated hooks, and deduplication. | TanStack Query, SWR, Apollo. | Co-locates server state with the Redux store. |
| Axios | RTK base query, signed uploads, R2 management, telemetry, and direct calls. | `fetch`, ky. | Upload progress/cancellation and a shared interceptor layer are useful, though the current error interceptor is defective. |
| React Hook Form | Streaming-config form context and field binding. | Formik, controlled state, TanStack Form. | Efficiently shares a very large configuration schema. Other forms do not use it. |
| TanStack React Table | Generic, streaming-link, analytics, notification, transaction, and server tables. | AG Grid, MUI DataGrid. | Headless sorting/filtering/pagination fits the custom UI kit. |
| Recharts | Bar, pie, and shadcn chart helpers for analytics. | Chart.js, ECharts, Visx. | React-native declarative charts; selection history unknown. |
| date-fns | Date formatting, date-range initialization, and analytics inputs. | Luxon, Day.js, Intl only. | Small function imports and immutable date helpers. |

### Platform and utility dependencies

| Dependency | Use | Alternatives | Why this choice |
|---|---|---|---|
| Firebase | Google auth plus four named apps and three Firestore workloads. | Auth0/Clerk; WebSocket/SSE; custom database APIs. | Existing Eagle identity and real-time processing/monitoring contracts use Firebase. |
| `js-cookie` | Non-auth `userConsent` cookie. | `document.cookie`, cookie library. | Simple client cookie API. |
| `unzipit` | Browser-side ZIP structure inspection before upload. | JSZip, fflate. | Reads entries without server upload; rationale unknown. |
| `uuid` | IDs for stream and meeting links. | `crypto.randomUUID`. | Established package; modern browsers could remove this dependency. |
| `react-hook-form` | Large config forms only. | Controlled fields. | Avoids rerendering a very large settings form. |

### Development dependencies

| Dependency/group | Responsibility | Notes |
|---|---|---|
| ESLint 9, `@eslint/eslintrc`, Next ESLint packages | Flat ESLint configuration, Core Web Vitals, Next and TypeScript rules. | The build succeeds with many warnings; accessibility rules are partly disabled. |
| TypeScript ESLint parser/plugin | Type-aware syntax and recommended rules. | `no-explicit-any` and unused variables are warnings, not build blockers. |
| `eslint-plugin-import` + TS resolver | Import ordering, resolution, and diagnostics. | Produces several Axios named/default warnings. |
| `eslint-config-prettier` | Disables formatting conflicts. | Formatting is delegated to Prettier. |
| Prettier 3 + classnames/Tailwind plugins | Formats source, long class strings, and Tailwind class order. | Only `src/**/*.{js,jsx,ts,tsx}` is included in the format script. |
| `cross-env` | Sets `NEXT_PUBLIC_ENV` portably in scripts. | Shell-specific environment syntax. | Declared but missing from the current `.bin` installation used for verification. |
| Node/React/js-cookie type packages | TypeScript declarations. | Required for libraries/runtime globals. |

### Technologies explicitly not used

There is no Zustand, TanStack Query, SWR, Zod, Framer Motion, Stripe client SDK, Clerk,
NextAuth/Auth.js, Sentry SDK, Jest, React Testing Library, Cypress, Playwright, Storybook, or
bundle analyzer. Stripe is inferred behind payment APIs, not imported in the browser.

### Section 2 closeout

**Key takeaways**

- Next.js + React + TypeScript form the shell; Tailwind/shadcn/Radix form the UI.
- RTK Query/Axios own REST state; Firebase owns authentication popup and live state.
- React Hook Form is specialized to app configuration, not the repository-wide form standard.
- Several SOP-recommended tools, especially Zod and tests, are absent.

**Common pitfalls**

- Looking for a Tailwind config file in this Tailwind v4 setup.
- Assuming Firebase API keys are server secrets; they are public client configuration, while
  Firebase rules remain security-critical and are outside this repository.
- Adding TanStack Query alongside RTK Query without a deliberate ownership boundary.
- Assuming Stripe behavior can be debugged in a nonexistent client SDK.

**Questions to ask a mentor**

- Is RTK Query the mandated long-term data layer?
- Is React Hook Form expected for new forms?
- Are Firebase projects temporary integration points or permanent architecture?
- What browser support targets justify Autoprefixer and current polyfill choices?

**Related files to read next**

- `package.json`, `pnpm-lock.yaml`
- `components.json`
- `src/app/globals.css`
- `src/store/store.ts`
- `src/config/firebase.ts`

**Practical exercises**

- Build a small shadcn field using existing tokens.
- Add one typed RTK Query endpoint and cache tag.
- Replace `uuid` in a local experiment with `crypto.randomUUID`.

---

## Section 8 — Components

The exhaustive per-file component catalog is in
[Section 22](./04-file-catalog.md#section-22--file-by-file-documentation). This section
explains how to reason about every component family, its rerender behavior, and reuse rules.

### Route and layout components

- `RootLayout` is a Server Component with no state. It rerenders during server generation,
  not on client state changes.
- `SidebarLayout` is the main client gate. Its state is `loading` and `redirecting`; auth/API
  query changes and its effects determine whether it renders a loader or the application
  shell.
- Page components under `src/app` are mostly stateless adapters that select one feature.
  Inline payment/reset pages are exceptions.

Do not place business workflows in new `page.tsx` files. Keep URL composition and route
metadata there; place feature behavior in `components/features`.

### Provider components

| Provider | State/side effects | Consumers and rerenders | Main concern |
|---|---|---|---|
| `Providers` | No local state; mounts Redux/theme. | Entire app. | Module-level store and no server hydration. |
| `ThemeProvider` | Delegates to `next-themes`. | All themed components. | Theme toggle is currently unreferenced. |
| `ConfigProvider` | Breadcrumb, screen, selected config/field. | Streaming app modal/config/asset screens. | Context value is recreated on every render; `any[]` breadcrumbs. |
| `UploadSequenceProvider` | 25+ state values, refs, timers, API mutations, Firestore listener. | App cards, upload modal/details/sequence/error. | 1,200+ lines and broad context rerenders; highest-risk component. |
| `AdditionalUploadProvider` | File, modal, step, progress, cancellation. | Additional upload page/details. | Misspelled directive; Azure-specific headers. |
| `DedicatedServerUploadProvider` | ZIP, app name, progress, cancellation. | Dedicated upload dialog/details. | Duplicates additional-upload structure. |

### Feature component hierarchy

```text
StreamingAppPage
├─ SubscriptionSummarySection
└─ StreamingAppSection
   ├─ Application
   │  ├─ StreamingAppCard / TemporaryUploadCard
   │  ├─ StreamingLinkTab / MeetingLinkTab
   │  ├─ ConfigSettingsForm
   │  │  ├─ CommonTab
   │  │  ├─ UITab
   │  │  ├─ CustomizationTab
   │  │  └─ DeveloperTab
   │  ├─ ImageAssetList / VideoAssetList
   │  └─ ImageUpload / VideoUpload
   ├─ DemoApplication
   └─ StreamingAppMonitor
```

Most feature roots use local tab/modal/search/sort state plus custom data hooks. They rerender
when local state, RTK Query subscriptions, Firebase snapshots, context values, or parent props
change. Only expensive column definitions and derived datasets are selectively memoized.

### Shared components

- `CommonTable` is the primary generic TanStack table with optional toolbar, search, column
  visibility, CSV export, and pagination.
- `StreamingAppTable` is a near-duplicate specialized table.
- `DataTable` is a third, currently unreferenced, hand-paginated table.
- `FileDropzone`, dialog wrappers, tabs, loaders, page titles, documentation links, and
  tooltip helpers are reusable application-level pieces.
- Logo components are presentational SVG/JSX indicators for upload sequence stages.

Reusable components should remain business-neutral. A component that imports plan/API hooks
belongs under its feature, even if its visual form looks reusable.

### UI primitives

Files under `components/ui` wrap Radix primitives or render low-level HTML. They accept normal
component props/ref behavior, style through `cn()`, and rerender when props or their Radix
context changes. Avoid business APIs, account logic, or feature-specific copy in this folder.

The large `ui/sidebar.tsx` is a component system of its own: provider, state, cookie
persistence, responsive sheet, groups, menus, rails, triggers, badges, and skeletons. Read it
before altering sidebar behavior.

### Component performance rules

- Context providers should memoize values or split state when unrelated consumers rerender.
- Table column callbacks must include live handler dependencies; several current `useMemo`
  arrays omit them and risk stale closures.
- Large dialogs/config/upload flows should be dynamically imported.
- Avoid calling the same custom aggregation hook in a parent and many children when it
  performs several subscriptions. RTK Query deduplicates identical requests, but React work
  and selector subscriptions still multiply.
- Object URLs created for previews must be revoked. The upload provider does this; some other
  preview components do not consistently clean up.

### Section 8 closeout

**Key takeaways**

- Components are divided into route adapters, feature components, providers, shared pieces,
  and low-level UI primitives.
- Provider and feature state dominate rerenders.
- `UploadSequenceProvider` is the architectural hotspot.
- Section 22 records every component's props, hooks, state, dependencies, and concerns.

**Common pitfalls**

- Putting API calls in `components/ui`.
- Adding state to page wrappers instead of feature modules.
- Treating a memoized table column array as correct while omitting handler dependencies.
- Reusing a feature component whose hidden hooks create unrelated network requests.

**Questions to ask a mentor**

- Is the upload provider scheduled for state-machine extraction?
- Which of the three table implementations is canonical?
- Are zero-incoming shadcn components intentionally kept as a palette?
- What accessibility acceptance criteria apply to custom dialogs/tables?

**Related files to read next**

- `src/components/providers/upload/upload-sequence-provider.tsx`
- `src/components/features/streaming-app/application.tsx`
- `src/components/shared/common-table.tsx`
- `src/components/ui/sidebar.tsx`
- Section 22's component entries

**Practical exercises**

- Use React DevTools to inspect context-driven rerenders during upload.
- Extract one shared upload-progress presentation component.
- Add an accessibility test checklist to one dialog.

---

## Section 9 — Custom Hooks

| Hook | Inputs → outputs | Side effects/dependencies | Used by / mistakes to avoid |
|---|---|---|---|
| `useCardDetailsV2` | none → `{cardInfo,isLoading}` | Payment-method/default-card queries, `useSubscriptionV2`, `useUserInfo`. | New billing. It fetches a default method but returns only subscription payment method. |
| `useCardDetails` | none → legacy card and loading | Chooses payment query by legacy plan; default card for PPM. | Legacy billing. Do not call before plan identity settles. |
| `useCCUData` | date range → empty compatibility result | No current I/O; dates are discarded. | Effectively dead stub. |
| `useCCUDataFromRecords` | stream rows/loading/error → frequency intervals/stats | Pure `useMemo` sweep-line calculation. | `CcuTab`. Average is interval-count weighted, not duration weighted. |
| `useCommonUpload` | `{assetType,appName?}` → file/progress/handler | Gets signed URL, direct PUT, upload-complete telemetry, toasts. | Profile/images/videos/thumbnails. It supports only mapped asset types and logs generic errors. |
| `useCustomerId` | none → customer ID/loading | Session, API-key, customer-ID queries. | Billing/auth layout. It does not understand SuperAdmin skip state. |
| `useDebounce` | callback, delay → debounced callback | Timer and unmount cleanup. | Signup availability checks. Callback identity changes reset the returned callback. |
| `useOldSubPause` | none → legacy pause flags/IDs | Composes `useSubscription`. | Legacy subscription banners/details. |
| `useHasPauseSubscription` | none → V2 pause flags/IDs | Composes Core, minute, and storage hooks. | New billing/summary. This can fan out into many requests. |
| `useMinutesV2` | none → minute subscription, quantity, price, limit | Core and min status queries, `useSubscriptionV2`. | New plans. Imported minute-stream query is unused. |
| `useMinute` | none → streamed minutes/loading | Chooses date range from legacy plan/trial and queries usage. | Legacy summaries. Invalid/missing dates can produce `Invalid Date`. |
| `useIsMobile` | none → boolean | `matchMedia` listener at 640px. | Sidebar. Initial `undefined` is coerced to false. |
| `useStorage` | none → legacy storage quantities/cost/usage/pause | Storage status and utilization queries. | Legacy plans and upload gating. Assumes 10 GB free storage. |
| `useStorageV2` | none → V2 storage subscription/usage | GB status + storage utilization + Core hook. | New plans. `hasCoreStorageExp` is hardcoded `false`; remaining calculations are inconsistent. |
| `useStreamed` | none → stream time and remaining percentage | Core/minute status and usage queries. | No incoming imports; candidate dead code. Loading values are discarded. |
| `useStreamingApp` | none → apps merged with thumbnails/loading | App list and thumbnail queries. | Home/app-name validation. Merges by exact asset name and first thumbnail version. |
| `useSubscriptionV2` | none → Core subscription metadata/resources/usage | User info, Core status, minute usage. | New subscription UI. Defaults products to 5 GB/60 min and combines yearly shadow dates. |
| `useSubscription` | none → selected legacy plan/data/loading | Four legacy status queries. | Legacy UI. Priority is PPCCU → PPL → prepaid → PPM. |
| `useTrialV2` | none → free resource usage/limit flags | User info, storage usage, minute usage. | Trial banners/gating. Trial ends only when both resources finish in `hasTrialFinish`, while other callers use either flag. |
| `useTrial` | none → date progress/expiry | API key + user info; date calculation. | Older trial UI. Falls back to “now,” which can mask missing backend dates. |
| `useUserInfo` | none → identity, role, API key, customer ID | Session/API-key/customer queries, `localStorage`, storage listener. | Nearly every feature. Same-tab storage writes do not fire the `storage` event; owner/onwer spelling is inconsistent. |
| `useCheckExactAssetNameMatch` | none → checker function | Composes `useStreamingApp`. | Upload modal. Returns false while data loads, so callers must not treat that as authoritative uniqueness. |
| `useConfigContext` | none → config context | Throws outside provider. | Streaming config modal only. |
| `useUploadSequence`, `useAdditionalUpload`, `useDedicatedServerUpload` | none → workflow context | Context subscriptions. | Must stay under matching provider; any provider value change rerenders consumers. |
| `useBeforeUnload` | message, enabled → void | Adds/removes `beforeunload`. | Streaming upload. Browsers may ignore custom text. |

### Section 9 closeout

**Key takeaways**

- Hooks are mostly aggregation layers over RTK Query rather than isolated computations.
- One hook call can subscribe to many backend resources.
- Legacy and V2 hooks must not be mixed casually.
- Several returned flags contain known placeholder/inconsistent logic.

**Common pitfalls**

- Calling `useUserInfo` repeatedly without understanding its query fan-out.
- Assuming `isLoading` includes every nested request.
- Using V2 storage-limit flags for enforcement while the flag is hardcoded false.
- Treating `useCCUData` as implemented.

**Questions to ask a mentor**

- Which defaults (5 GB/60 min/10 GB) are product contracts?
- Should customer and identity data become one selector/hook?
- What is the correct Core storage limit formula?
- Can legacy hooks be removed on a timeline?

**Related files to read next**

- all files in `src/hooks`
- `src/store/api/*`
- `src/components/features/subscription/subscription.tsx`

**Practical exercises**

- Create a dependency graph for `useHasPauseSubscription`.
- Unit-test CCU overlapping intervals.
- Correctly type one deeply nested API response and simplify its hook.

---

## Section 10 — State Management

### Data ownership

| State kind | Owner | Examples | Lifetime |
|---|---|---|---|
| REST server state | RTK Query `baseApi` | session, apps, subscriptions, notifications, analytics | In-memory store/cache; discarded on reload. |
| Small global client state | Redux `user` slice | `selectedUserId` | In-memory; currently no consumer. |
| Cross-component workflow state | React Context | upload sequence, additional upload, dedicated upload, config navigation | Provider lifetime under sidebar layout. |
| Live server state | Firestore SDK | upload processing, stream monitor, DS monitor | Listener lifetime; Firebase client cache semantics. |
| Local UI/form state | `useState` / React Hook Form | tabs, dialogs, inputs, search, settings form | Component/modal lifetime. |
| Browser-persisted state | `localStorage`, cookie, sidebar cookie | selected team/SuperAdmin user, access logs, consent, sidebar state | Across reloads according to browser storage. |
| Module cache | `cachedProvider` | selected storage provider | Until page reload/module reset. |

### Redux and RTK Query

`store.ts` registers one API reducer and `userReducer`. Serializable checks are disabled,
likely because API/Firebase/date payloads are not normalized, but the source does not state
the reason. Disabling the check also hides accidental non-serializable state.

All API modules inject endpoints into the same `baseApi`. This gives one normalized cache key
space and one tag list. Tags cover plan types, storage, notifications, uploads, app list,
config, login, and Core subscription status.

There is no optimistic update logic, normalized entity adapter, persisted store, server
preload, or cross-tab cache synchronization.

### React Context

Context is used where state must survive switching between nested screens/dialogs. The upload
provider additionally owns imperative resources: cancel tokens, timeouts, object URLs, and a
Firestore unsubscribe callback.

Provider values are object literals without `useMemo`. Every state change notifies every
consumer, even if a consumer reads one field. Splitting command/status/file contexts or using
a reducer/state machine would reduce coupling.

### Local state and forms

Most forms are controlled `useState` forms. React Hook Form is used only for the streaming
config, where `FormProvider` lets tab fields share one form instance. This means validation,
dirty state, and submission conventions differ by feature.

### Cache synchronization

- RTK tags invalidate a subset of queries.
- Many feature flows call `refetch()` manually after mutation/upload.
- Firestore processing success triggers a cluster of manual refetches.
- Team selection uses `localStorage`; only cross-document/tab storage events are observed.
- Full-page navigations reset all in-memory state and naturally force fresh queries.

### Section 10 closeout

**Key takeaways**

- RTK Query owns REST server state; contexts own workflows; Firebase owns real-time streams.
- The plain Redux slice is effectively unused.
- Cache synchronization is a mix of tags, manual refetch, listeners, and reloads.
- No state is persisted except selected user/access logs/consent/sidebar.

**Common pitfalls**

- Storing REST responses in new Redux slices.
- Duplicating Firestore data into RTK Query without an ownership plan.
- Expecting same-tab `localStorage` writes to trigger `storage`.
- Adding more unrelated fields to the upload context.

**Questions to ask a mentor**

- Is selected team state meant to be application-global and same-tab reactive?
- Can the unused user slice be removed?
- Which refetches can be replaced by tags?
- Should upload state survive route changes or reloads?

**Related files to read next**

- `src/store/store.ts`
- `src/store/tag-types.ts`
- `src/components/providers/*`
- `src/hooks/use-user-info.ts`

**Practical exercises**

- Inventory duplicate RTK requests in Redux DevTools.
- Replace one manual refetch with precise tag invalidation.
- Prototype an upload reducer with explicit states.

---

## Section 13 — Forms

### Form inventory

| Form/workflow | State and validation | Submission, loading, error, success |
|---|---|---|
| Sign in | Controlled email/password; browser `required`; backend decides credentials. | Sign-in mutation → cookie mutation; combined loading; inline generic error; full navigation. |
| Sign up | Four controlled steps. Regex email/password/username, availability APIs, OTP API. | Per-step loading plus global loading; inline flags/toasts; creates account/session/username then `/username`. |
| Google sign in/up | Firebase popup, no local fields. | Firebase errors become inline/toast; token is exchanged for cookie. |
| Reset request (`/repass`) | Controlled email + `isValidEmail`. | Reset mutation; button spinner; toast success/backend error. `unwrap()` is not caught. |
| Reset confirmation (`/fb-mail`) | Two password fields, match + strong-password regex, `oobCode`. | Mutation; alerts/toasts; redirects to sign-in. |
| Username onboarding | Username, international phone, lead source, newsletter; regex/API checks. | Username/profile mutations; field errors/toasts; logout on certain failures; home on success. |
| Profile settings | Controlled username/email/phone plus many confirm dialogs. | Availability checks then mutations; reset/delete/logout flows; “DELETE” confirmation for account removal. |
| Profile logo | File picker/drop, image MIME accept, max 1 MB. | Signed upload and upload-complete call; dialog pending and toasts. |
| Team invite/remove | Email regex; selected member for removal. | Auth mutations; dialog loading; toast success/failure. |
| API-key rotation | Confirmation only. | Mutation/refetch/toast. |
| Token generation | Raw JSON text and expiry. No guarded JSON parser. | `JSON.parse` then mutation; unhandled invalid JSON can throw; generated token is copyable. |
| Version-control toggle | Proposed boolean + confirmation. | Mutation/refetch/toast. |
| Legacy PPCCU/PPL | Stepper bounded 1–15 (PPCCU starts at 3); current-subscription check. | Checkout mutation; opens hosted session. |
| Legacy prepaid minutes | 100-minute stepper, starts 500; auto-recharge choice required. | Checkout mutation; success URL carries plan. |
| Legacy PPM | Optional promo text is collected but never sent. | Card setup mutation; opens hosted session. |
| Core plan | Monthly/yearly is supplied by pricing tabs. | Checkout mutation with hardcoded 29/290 and product code. |
| Resource modifications | Modal-specific numeric/select inputs and quote queries. | Upgrade/downgrade/non-checkout/unpause mutations; toasts/dialog close. |
| Cancellation reason | Controlled reason, required by parent workflow. | Reason precedes cancel confirmation and notification. |
| Streaming link/meeting link | Generated names; inline config/version selects; config-name input rejects special characters. | `saveAppUrl`, config create/delete, version/link delete; toasts/refetches. |
| Streaming config | React Hook Form shared across Common/UI/Customization/Developer tabs. Defaults from `defaultConfig`; conditional fields. | Loads single config into form, submits full config through edit mutation, toasts. No Zod/schema resolver. |
| Image/video assets | File type/size checks in upload components; asset selection state. | Signed URL PUT, progress, then return to asset list. |
| Streaming app upload | App name, ZIP, optional thumbnail, switches/warnings; ZIP inspection and storage limits. | Multi-provider upload state machine with progress/cancel/retry/Firestore outcome. |
| Additional upload | Any file through dropzone. | Signed direct PUT, progress/cancel, refetch list. |
| Dedicated-server upload | App name and ZIP. | Dedicated signed URL PUT, progress/cancel, refetch list. |
| SuperAdmin generator | Email/username/API key required by HTML/manual trim. | Produces Base64 credential URL. Component currently has no incoming import. |

No form uses Zod. Several forms call `.unwrap()` without `try/catch`; combined with the Axios
interceptor behavior, error handling is inconsistent.

### Section 13 closeout

**Key takeaways**

- Form architecture is mixed: mostly controlled state, one large React Hook Form workflow.
- Validation is manual regex/API validation, not schema-based.
- Hosted payment forms live outside this app.
- Upload “forms” are multi-stage workflows rather than simple submissions.

**Common pitfalls**

- Parsing token JSON without guarding errors.
- Assuming an HTML `accept` attribute is security validation.
- Forgetting duplicate availability checks during signup.
- Adding fields to `defaultConfig` without updating its type and relevant tab.

**Questions to ask a mentor**

- Should new forms standardize on React Hook Form + schema validation?
- Which validation rules are mirrored on the backend?
- Is PPM promo-code input intentionally unused?
- What are exact file-size/type limits for each upload kind?

**Related files to read next**

- auth/account feature folders
- streaming config folder
- upload providers and modal
- plan/subscription dialog folders

**Practical exercises**

- Add safe JSON parsing and an inline error to token generation.
- Write a schema for username onboarding without changing behavior.
- Trace the config form from fetched JSON to submitted payload.

---

## Section 14 — UI Architecture

### Design system

The design system is a local shadcn/ui implementation over Radix. `globals.css` defines
semantic color, radius, font, chart, sidebar, and shadow tokens for light/dark modes. Feature
components should consume semantic utilities such as `bg-background`, `text-foreground`, and
`text-destructive`.

### Layers

1. `components/ui`: low-level primitives; no business logic.
2. `components/shared`: application-neutral compositions such as tables, loaders, dialogs,
   tabs, dropzones, and doc links.
3. `components/navbar` and `components/sidebar`: shell navigation.
4. `components/features`: product-specific components and copy.
5. `src/app`: routing/layout composition.

### Responsive strategy

Tailwind mobile-first breakpoints (`sm`, `md`, `lg`, `xl`) are used. The sidebar switches to
a sheet on mobile. Several feature containers force `md:min-w-[1250px]`; on smaller
viewports this can cause horizontal overflow instead of a responsive reflow.

### Theme and dark mode

Both token sets exist. `next-themes` sets a class and defaults to light. `ModeToggle` exists
but has zero incoming imports, so users have no visible toggle in this snapshot. Some
features hardcode light-only hex colors/backgrounds and may fail dark-mode contrast.

### Accessibility

Positive foundations include Radix primitives, semantic labels in many forms, focus rings,
and some `aria-label`/`sr-only` text. Risks include disabled `jsx-a11y/alt-text`, raw clickable
`div`/icons, icon-only controls without labels, color-only statuses, custom inline consent UI,
and hardcoded fixed/minimum widths. Accessibility behavior has no automated tests.

### Section 14 closeout

**Key takeaways**

- The intended boundary is UI primitives → shared compositions → feature components.
- Semantic CSS variables are the theme contract.
- Responsive support is uneven because several screens require 1,250px.
- Dark-mode tokens exist but the toggle is unreachable and hardcoded colors remain.

**Common pitfalls**

- Importing feature hooks into `components/ui`.
- Adding raw hex values when a semantic token exists.
- Making a `div` clickable without keyboard semantics.
- Assuming Radix makes surrounding custom composition automatically accessible.

**Questions to ask a mentor**

- Is dark mode a supported product feature?
- What is the minimum supported viewport?
- Is WCAG 2.1 AA required?
- Which shared table is approved for future work?

**Related files to read next**

- `components.json`
- `src/app/globals.css`
- `src/components/ui/*`
- `src/components/shared/*`

**Practical exercises**

- Keyboard-test one streaming-link dialog.
- Audit hardcoded colors in the pricing cards.
- Remove a forced minimum width in a local experiment.

---

## Section 15 — Styling

Tailwind v4 is imported directly in `globals.css`; there is no `tailwind.config.*`. The file:

- defines pointer behavior for enabled buttons/roles;
- creates a class-scoped `dark` variant;
- declares light and dark OKLCH tokens;
- maps those tokens into Tailwind theme variables;
- defines radii and shadows;
- applies global border/focus and body colors.

Typography:

- Root layout loads Google Asap and applies its class to `<html>`.
- `fonts.ts` loads local `Spantaran.woff2`.
- Spantaran is used for brand/pricing headings, sometimes through inline
  `fontFamily`.
- Global tokens mention Inter/Merriweather/JetBrains Mono, but those font files/imports are
  not provided. Browser fallbacks apply unless supplied externally.

Spacing and layout use Tailwind's scale plus many arbitrary sizes, negative margins, fixed
heights, and `md:min-w-[1250px]`. Animations come from Tailwind utilities, `tw-animate-css`,
and custom transition classes; there is no motion orchestration library.

Image strategy is mixed: `next/image`, raw `<img>`, CSS backgrounds, imported static assets,
and remote URLs. `next.config.ts` permits all HTTPS hosts.

### Section 15 closeout

**Key takeaways**

- `globals.css` is the Tailwind and token source of truth.
- Theme colors are semantic OKLCH variables.
- Typography combines Asap, Spantaran, and unresolved fallback token names.
- Arbitrary values and hardcoded colors are common outside primitives.

**Common pitfalls**

- Creating a Tailwind v3 config for a v4 project.
- Using `var(--color-primary` without a closing parenthesis, as the current pie palette does.
- Assuming `font-sans` loads Inter.
- Adding fixed widths that worsen existing overflow.

**Questions to ask a mentor**

- Are Inter/Merriweather/JetBrains Mono expected to be loaded?
- Is Spantaran licensed and limited to headings?
- Are design tokens maintained elsewhere?
- Can remote image hosts be allowlisted?

**Related files to read next**

- `src/app/globals.css`
- `src/app/fonts.ts`
- `src/styles/custom-font.ts`
- `next.config.ts`

**Practical exercises**

- Verify every semantic color in both themes.
- Replace one hardcoded color block with tokens.
- Measure layout at 320, 768, and 1,280 pixels.

---

## Section 16 — Business Logic

### Identity and permission rules

- Protected routes require backend session message exactly equal to `Signed`.
- Signed users without a nonblank username go to `/username`.
- Developer navigation and page require current email to equal owner email.
- Team management requires owner equality.
- Many account/plan buttons reject team members with a toast.
- Non-admin stream monitors hide sessions marked `e3ds_employee`.
- SuperAdmin URL sessions set `isSuperAdmin=false` for normal admin checks and impersonate via
  selected-member API key.

### Account rules

- Email/password signup requires unused email, strong password, username of 3–30 allowed
  characters, and successful OTP.
- Password requires uppercase, lowercase, number, special character, and 8+ characters.
- Account deletion requires exact uppercase `DELETE`.
- Username change warns that existing apps/config may be lost; source cannot verify backend
  migration behavior.

### App and upload rules

- App names cannot contain spaces or non-alphanumeric/underscore characters.
- Linux root `.sh` name must equal app name.
- ZIP must be extractable and contain a Windows executable or recognized Linux structure
  unless checking is skipped.
- Pixel Streaming/Pixel Streaming 2 plugin discovery controls warning/error UI.
- App upload progress maps upload to 0–50%, download to 60%, extract to 70%, test to 90%, and
  tested to 100%.
- Slow non-R2 upload below 0.5 MB/s for 90 seconds triggers one alternate-URL retry.
- R2 uploads six parts concurrently with three attempts per part.
- Existing apps can receive versions; version-control setting determines backend behavior but
  details are unknown.
- A latest app version uploaded within 30 minutes displays “Ready to Launch.”
- Upload availability depends on mixed legacy/Core/trial limit flags; V2 storage enforcement
  is currently ineffective because `hasCoreStorageExp` is false.

### Links/configuration

- Each new link begins on config `default`.
- Link IDs use UUIDs; names use four random alphanumeric characters.
- “Latest” version is represented by UI value/string conventions and backend values including
  `-1`.
- An anonymized V6 URL encodes owner/app/config/version JSON in Base64.
- Demo apps and their default links/config/version are effectively read-only in the UI.
- Configuration names reject special characters.

### Monitoring/analytics

- Stream sessions with continuation older than 5 minutes or start older than 5 hours are
  hidden.
- Dedicated-server monitor entries older than 10 minutes are hidden.
- CCU uses inclusive interval endpoints and only emits positive-concurrency ranges.
- Analytics chart grouping sums `videoStreamTime`, sorts descending, and keeps seven groups.
- City/country containing “ahsan” is replaced with `Unknown2`; business justification is not
  present.

### Subscription and trial rules

- Legacy active statuses include `active`, `trialing`, and `past_due`.
- Legacy plan priority is PPCCU, PPL, prepaid minute, then PPM.
- PPCCU costs $100 per unit/month and UI permits 1–15.
- PPL costs $50 per license/month and UI permits 1–15.
- Prepaid minutes cost $0.10, step by 100, minimum 100, and require an auto-recharge choice.
- PPM usage is computed at $0.10/minute.
- Legacy storage assumes 10 GB free and prices additional storage from plan metadata.
- Core is hardcoded at $29 monthly or $290 yearly in checkout UI.
- Trial defaults to backend products or 60 minutes/5 GB.
- Different components define trial exhaustion differently: `hasTrialFinish` requires both
  minute and storage exhaustion, while banners/gating often use either.
- Owner-only restrictions guard purchases/management in several UI paths.
- Paused subscriptions display payment update/reactivation actions.

### Section 16 closeout

**Key takeaways**

- Business rules are distributed across hooks and components, not centralized.
- Two billing models and multiple definitions of limits coexist.
- Upload is a frontend-orchestrated state machine with many operational rules.
- Some rules are obvious placeholders or unexplained hardcoding.

**Common pitfalls**

- Updating display copy without updating checkout payload math.
- Fixing a limit in one hook while other components calculate it independently.
- Treating frontend owner checks as authorization.
- Changing upload progress percentages without synchronizing sequence UI.

**Questions to ask a mentor**

- What is the canonical trial-expiry rule?
- What characters are officially valid in app/config names?
- Is “past_due” intentionally treated as active?
- Why is `ahsan` anonymized?
- What plan values must move to backend configuration?

**Related files to read next**

- `src/constant/plan.ts`
- all subscription/trial/storage/minute hooks
- upload provider and ZIP utility
- streaming link/config modules

**Practical exercises**

- Build a single decision table for all subscription summaries.
- Write tests for trial-limit combinations.
- Extract app-name validation into one typed function shared by UI and upload.

---

## Section 17 — Performance

### Existing techniques

- Static prerendering for every route.
- Next image optimization for many imported images.
- `next/font` self-hosts the fetched Google font after build.
- RTK Query request deduplication and cache reuse.
- `useMemo` for table columns, chart totals, monitor columns, CCU calculations, and merged
  processing state.
- `useCallback` in debounce and some upload inputs.
- Firestore listener cleanup.
- R2 multipart parallelism and upload retry.
- Client-side pagination/filtering in TanStack Table.

There is no `React.memo`, dynamic import, `React.lazy`, route-level streaming loader, explicit
prefetch policy, bundle analysis, virtualized table, service worker, or image-host allowlist.

### Verified bottlenecks

- `/` first-load JS: 577 kB.
- `/multiplayer-server`: 455 kB.
- `/utilities`: 402 kB.
- `/signup`: 359 kB.
- `/signin`: 347 kB.
- `/analytics`: 343 kB.
- Upload provider is eagerly available across every sidebar route.
- Firebase initializes all four apps from a shared module even when a route needs one.
- Feature contexts cause broad rerenders.
- Large tables/charts operate entirely in browser memory.
- Many hooks repeat aggregation subscriptions.
- Missing/stale hook dependencies can cause outdated closures or extra listener churn.

### Improvement order

1. Fix correctness before memoization: Axios error semantics and effect dependencies.
2. Dynamically load upload/config/account/pricing dialogs.
3. Split Firebase service initialization by feature.
4. Split upload provider state/commands or move to an external state machine.
5. Consolidate duplicate hooks and table implementations.
6. Add server pagination/aggregation for large analytics datasets.
7. Establish route bundle budgets and CI reporting.

### Section 17 closeout

**Key takeaways**

- Static rendering is offset by large hydrated client bundles.
- RTK Query helps network duplication but not component/render duplication.
- No deliberate code-splitting strategy exists.
- Upload/Firebase/config code are prime lazy-loading targets.

**Common pitfalls**

- Adding `useMemo` around cheap values while retaining huge eager imports.
- Memoizing callbacks with incomplete dependency arrays.
- Assuming client pagination reduces downloaded data.
- Loading all Firebase services to use one auth popup.

**Questions to ask a mentor**

- What are acceptable first-load budgets?
- How large can analytics responses become?
- Can upload context mount only on routes that use it?
- Is Firebase modular splitting supported by current deployment?

**Related files to read next**

- verified route table in Section 5
- upload provider
- Firebase config
- table/chart components

**Practical exercises**

- Produce a bundle treemap.
- Lazy-load the account modal and compare `/` output.
- Profile context rerenders during upload.

---

## Section 18 — Error Handling

### Current layers

1. **Transport wrapper:** `axiosBaseQuery` catches rejected Axios promises.
2. **RTK Query state:** query/mutation loading/error values, unevenly consumed.
3. **Feature handling:** toasts, inline error text, alert dialogs, and loaders.
4. **Upload state machine:** structured `{message,step,color}` errors, retries,
   cancellation, stuck detection, and Firestore-derived failures.
5. **Global browser catcher:** `UnCaughtErrorCatcher` registers `window.onerror` and sends
   device/user/error information to the notification service.
6. **Next route handling:** only `not-found.tsx`; no `error.tsx` or `global-error.tsx`.

### Critical transport flaw

The Axios response error interceptor returns `error`. Axios therefore resolves the
interceptor chain instead of rejecting, and `axiosBaseQuery` returns `{data: error}`. Fixing
this requires `return Promise.reject(error)`. All current response-shape checks must be
regression-tested because some UI may accidentally depend on the broken behavior.

### Upload failures

Upload handles user cancellation, low-speed restart, HTTP/storage errors, invalid ZIPs,
plugin/Linux naming issues, processing failures, viruses, app crashes, alternate-upload
errors, and stalls. It also sends extensive telemetry. The flow has no persisted recovery
after a browser reload.

### Missing boundaries and fallbacks

- A render-time exception can blank a route because no App Router error boundary exists.
- Invalid JSON in token generation can become an uncaught event error.
- Several `.unwrap()` calls lack `try/catch`.
- Firestore listeners often omit an error callback or only log.
- Failed profile/additional/dedicated uploads use generic messages that discard server detail.
- Success/failure payment pages do not automatically redirect despite copy implying they may.

### Section 18 closeout

**Key takeaways**

- Error UX exists primarily at feature level, especially uploads.
- Global reporting is not a React error boundary.
- The shared Axios interceptor can misclassify every HTTP error.
- Route-level error recovery is absent.

**Common pitfalls**

- Checking only `data` after a mutation.
- Throwing inside an event handler without a local catch.
- Assuming `window.onerror` catches React render errors with usable recovery.
- Displaying raw backend/credential data in telemetry.

**Questions to ask a mentor**

- What data may be sent to the notification service?
- Is there a formal API error envelope?
- Which errors should be retryable?
- Should Sentry or another audited error platform replace chat notifications?

**Related files to read next**

- `src/helpers/axios/*`
- `src/components/error/*`
- upload provider and error component
- all mutation handlers using `.unwrap()`

**Practical exercises**

- Add a local error boundary to one feature.
- Write a transport test for a 400 response.
- Trigger every upload error state using mocked Firestore data.

