# Section 23 — Architecture Diagrams

These diagrams describe the repository as audited. Solid arrows mean direct
composition or invocation; dotted arrows mean a contextual, cached, or external
relationship. Route-group names in parentheses do not appear in URLs.

## 23.1 Overall project structure

```mermaid
flowchart TD
  Browser[Browser]
  Next[Next.js App Router]
  Public[Public auth routes]
  Protected[Sidebar route group]
  Features[Feature components]
  Shared[Shared and UI primitives]
  Hooks[Custom hooks and contexts]
  RTK[Redux Toolkit and RTK Query]
  Axios[Axios base query]
  Direct[Direct Axios and upload services]
  Fire[Four named Firebase apps]
  Config[Environment JSON and API config]
  AGW[AGW API]
  Auth[Auth API]
  Storage[CoreWeave or R2 signed storage]
  Monitor[Firestore realtime data]
  Third[GTM, Hotjar, notification service]

  Browser --> Next
  Next --> Public
  Next --> Protected
  Public --> Features
  Protected --> Features
  Features --> Shared
  Features --> Hooks
  Hooks --> RTK
  Hooks --> Direct
  Hooks --> Fire
  RTK --> Axios
  Axios --> AGW
  Axios --> Auth
  Direct --> AGW
  Direct --> Storage
  Fire --> Monitor
  Config -. selects .-> Axios
  Config -. initializes .-> Fire
  Next --> Third
```

## 23.2 Complete layout and provider hierarchy

```mermaid
flowchart TD
  Root[RootLayout: server]
  Html[html lang=en]
  Head[metadata and Asap font]
  GTM[GTM beforeInteractive]
  Body[body]
  NoScript[GTM noscript iframe]
  Providers[Providers: client]
  Redux[Redux Provider]
  Theme[ThemeProvider]
  Admin[SuperAdminHandler]
  Route[Current route tree]
  Consent[ConsentBanner]
  Toast[Sonner Toaster]
  Catcher[UnCaughtErrorCatcher]
  Hotjar[Hotjar afterInteractive]

  Root --> Html
  Root --> Head
  Html --> GTM
  Html --> Body
  Body --> NoScript
  Body --> Providers
  Providers --> Redux
  Redux --> Theme
  Theme --> Admin
  Admin --> Route
  Admin --> Consent
  Admin --> Toast
  Admin --> Catcher
  Body --> Hotjar
```

For an authenticated route, `Current route tree` expands as follows:

```mermaid
flowchart TD
  SL[SidebarLayout: client auth gate]
  SP[SidebarProvider]
  AS[AppSidebar]
  SI[SidebarInset]
  SH[SiteHeader]
  Main[main]
  US[UploadSequenceProvider]
  DS[DedicatedServerUploadProvider]
  AU[AdditionalUploadProvider]
  CP[ConfigProvider]
  Page[Protected page]

  SL --> SP
  SP --> AS
  SP --> SI
  SI --> SH
  SI --> Main
  Main --> US
  US --> DS
  DS --> AU
  AU --> CP
  CP --> Page
```

## 23.3 Navigation and route map

```mermaid
flowchart LR
  Start[/Request/]
  Root[/ /]
  Signin[/signin/]
  Signup[/signup/]
  Username[/username/]
  Reset[/repass/]
  Mail[/fb-mail/]
  Apps[Streaming applications]
  Additional[/additional-uploads/]
  Analytics[/analytics/]
  Dev[/developer-section/]
  DS[/multiplayer-server/]
  Plans[/plans/]
  Team[/team/]
  Utilities[/utilities/]
  PayOK[/payment-successful/]
  PayFail[/payment-failed/]
  CardOK[/credit-card-successful/]
  CardCore[/credit-card-successful-core/]
  CardFail[/credit-card-failed/]
  Missing[not-found]

  Start --> Root
  Start --> Signin
  Start --> Signup
  Start --> Username
  Start --> Reset
  Start --> Mail
  Start --> Additional
  Start --> Analytics
  Start --> Dev
  Start --> DS
  Start --> Plans
  Start --> Team
  Start --> Utilities
  Start --> PayOK
  Start --> PayFail
  Start --> CardOK
  Start --> CardCore
  Start --> CardFail
  Start --> Missing

  Root --> Apps
  Signin -->|new user| Signup
  Signup -->|cookie created| Username
  Username -->|complete| Root
  Signin -->|authenticated redirect| Root
  Apps -->|navigation| Additional
  Apps --> Analytics
  Apps --> Dev
  Apps --> DS
  Apps --> Plans
  Apps --> Team
  Apps --> Utilities
```

Every audited route is statically prerendered as a shell. The protected group
performs its redirect decisions on the client after hydration.

## 23.4 Route-to-core-component tree

```mermaid
flowchart TD
  R0["/ page.js"]
  RSignin["/signin"]
  RSignup["/signup"]
  RUsername["/username"]
  RReset["/repass"]
  RMail["/fb-mail"]
  RAdditional["/additional-uploads"]
  RAnalytics["/analytics"]
  RDev["/developer-section"]
  RDS["/multiplayer-server"]
  RPlans["/plans"]
  RTeam["/team"]
  RUtilities["/utilities"]
  RPay["payment result routes"]

  Stream[StreamingApp Application]
  StreamList[ApplicationList and ApplicationCard]
  Config[Config form and dialogs]
  Upload[Upload dialog and sequence provider]
  Auth[Signin form and Google signin]
  Signup[Signup stages and OTP]
  User[Username and profile completion]
  Add[AdditionalUpload Application and table]
  Analytic[Analytics Application]
  Charts[Chart tabs: CCU, duration, location, app]
  Table[Analytics table]
  Developer[DeveloperSection Application]
  Key[API key, token, version-control panels]
  Dedicated[DedicatedServer Application]
  DSCards[App cards, instance details, monitor]
  Plan[Plan Application]
  Legacy[Legacy plan cards and dialogs]
  Core[Core/minute/GB subscription UI]
  Teams[Team Application and member table]
  Utils[Utilities Application]
  Monitors[Streaming and server monitor tables]
  Billing[Post-payment status component]

  R0 --> Stream
  Stream --> StreamList
  Stream --> Config
  Stream --> Upload
  RSignin --> Auth
  RSignup --> Signup
  RUsername --> User
  RReset --> Auth
  RMail --> Auth
  RAdditional --> Add
  RAnalytics --> Analytic
  Analytic --> Charts
  Analytic --> Table
  RDev --> Developer
  Developer --> Key
  RDS --> Dedicated
  Dedicated --> DSCards
  RPlans --> Plan
  Plan --> Legacy
  Plan --> Core
  RTeam --> Teams
  RUtilities --> Utils
  Utils --> Monitors
  RPay --> Billing
```

The exhaustive leaf-level component, hook, import, props, state, and consumer
inventory is in `04-file-catalog.md`; this diagram intentionally keeps the full
screen tree readable.

## 23.5 Authentication lifecycle

```mermaid
sequenceDiagram
  actor User
  participant UI as Signin UI
  participant AGW as AGW Auth
  participant FB as Accounts Firebase
  participant AUTH as Auth Session API
  participant Layout as SidebarLayout
  participant API as Product APIs

  alt Email and password
    User->>UI: Submit credentials
    UI->>AGW: POST sign-in
    AGW-->>UI: Identity token
  else Google
    User->>UI: Choose Google
    UI->>FB: signInWithPopup
    FB-->>UI: Firebase ID token
  end
  UI->>AUTH: POST session/token with Bearer token
  AUTH-->>UI: Set browser session cookie
  UI->>Layout: Navigate
  Layout->>AUTH: GET session/state with cookie
  AUTH-->>Layout: Session, email, username, owner/team fields
  alt no session
    Layout-->>User: Replace with /signin
  else username missing
    Layout-->>User: Replace with /username
  else valid session
    Layout->>AUTH: GET user/api-key
    Layout->>API: POST get-customer-id
    Layout-->>User: Render protected shell
  end
```

## 23.6 Request lifecycle and cache invalidation

```mermaid
sequenceDiagram
  participant C as Component
  participant H as Generated RTK hook
  participant Q as baseApi cache
  participant B as axiosBaseQuery
  participant X as axiosInstance
  participant S as AGW/Auth service

  C->>H: query(arg) or trigger(body)
  H->>Q: lookup serialized endpoint + argument
  alt fresh cached query
    Q-->>H: cached data
  else request required
    Q->>B: request descriptor
    B->>X: Axios request
    X->>S: HTTP
    S-->>X: JSON response
    X-->>B: response.data
    B-->>Q: { data }
    Q-->>H: cache state
  end
  H-->>C: data/loading/error/refetch
  opt mutation invalidates a tag
    H->>Q: invalidate tag
    Q->>B: refetch active matching queries
  end
```

Important defect: the current Axios error interceptor returns an error object,
which can follow the success branch rather than reject into `axiosBaseQuery`'s
catch.

## 23.7 Streaming-app upload state machine

```mermaid
stateDiagram-v2
  [*] --> SelectZip
  SelectZip --> InvalidZip: no supported executable or bad structure
  SelectZip --> ValidatePlan: Windows exe or Linux engine and shell found
  ValidatePlan --> Blocked: storage or plan gate fails
  ValidatePlan --> ResolveProvider: allowed
  ResolveProvider --> R2Multipart: provider is R2
  ResolveProvider --> PrimarySignedUpload: other provider
  PrimarySignedUpload --> AlternateSignedUpload: speed below 0.5 MB/s for 90 sec
  PrimarySignedUpload --> RegisterMetadata: success
  AlternateSignedUpload --> RegisterMetadata: success
  AlternateSignedUpload --> Failed: fallback exhausted
  R2Multipart --> RegisterMetadata: all parts complete
  R2Multipart --> Failed: retries exhausted
  RegisterMetadata --> WaitingQueue
  WaitingQueue --> Downloading
  Downloading --> Extracting
  Extracting --> Testing
  Testing --> Tested: processing succeeds
  Testing --> Failed: crash, virus, invalid executable, or backend failure
  WaitingQueue --> Stuck: no terminal progress
  Downloading --> Stuck
  Extracting --> Stuck
  Testing --> Stuck
  Stuck --> Failed: over 20 minutes
  Tested --> [*]
  InvalidZip --> [*]
  Blocked --> [*]
  Failed --> [*]
```

The Context state is not persisted, so refresh does not resume the local workflow.
Firestore may still contain processing state, but no rehydration path reconstructs
the complete upload machine.

## 23.8 State-management map

```mermaid
flowchart TD
  Component[React component]
  Local[useState/useReducer]
  RHF[React Hook Form ConfigProvider]
  Context[Upload and config contexts]
  Redux[Redux user slice]
  RTK[RTK Query server cache]
  Fire[Firestore onSnapshot state]
  LS[localStorage/sessionStorage]
  Module[Module-lifetime storage-provider cache]
  Server[REST services]
  Realtime[Firebase projects]

  Component --> Local
  Component --> RHF
  Component --> Context
  Component --> Redux
  Component --> RTK
  Component --> Fire
  Component --> LS
  Context --> RTK
  Context --> Fire
  RTK --> Server
  Fire --> Realtime
  Context --> Module
  Module --> Server
```

Use local state for leaf UI, React Hook Form for the large config schema, Context
for tightly scoped workflows, RTK Query for REST server state, and Firestore
listeners for realtime server state. Do not duplicate one server value into several
stores unless the synchronization rule is explicit.

## 23.9 Subscription decision flow

```mermaid
flowchart TD
  Load[Load legacy and Core statuses]
  Owner{Account owner?}
  View[Read-only/member presentation]
  Core{Active Core/GB/min plan?}
  Legacy{Active legacy plan?}
  Priority[Resolve PPCCU > PPL > prepaid > PPM]
  Trial{Trial available/finished?}
  Select[Select plan and quantity]
  Card{Payment method present?}
  Checkout[Create checkout/session]
  NonCheckout[Create or change subscription without checkout]
  Result[Payment result route and status refetch]

  Load --> Owner
  Owner -->|no| View
  Owner -->|yes| Core
  Core -->|yes| Select
  Core -->|no| Legacy
  Legacy -->|yes| Priority
  Legacy -->|no| Trial
  Priority --> Select
  Trial --> Select
  Select --> Card
  Card -->|no| Checkout
  Card -->|yes| NonCheckout
  Checkout --> Result
  NonCheckout --> Result
```

Business-policy enforcement must remain backend-side even though this UI guides the
path.

## 23.10 Folder dependency direction

```mermaid
flowchart LR
  App[src/app]
  Feature[components/features]
  Nav[navbar/sidebar]
  Shared[components/shared]
  UI[components/ui]
  Hooks[src/hooks]
  Providers[components/providers]
  Store[src/store]
  Services[src/services]
  Helpers[src/helpers]
  Config[src/config and constants]
  Types[src/types and utils]

  App --> Feature
  App --> Nav
  App --> Providers
  Feature --> Shared
  Feature --> UI
  Feature --> Hooks
  Feature --> Store
  Nav --> Shared
  Nav --> UI
  Nav --> Hooks
  Providers --> Hooks
  Providers --> Store
  Providers --> Services
  Hooks --> Store
  Hooks --> Services
  Store --> Helpers
  Store --> Config
  Services --> Config
  Helpers --> Config
  Feature --> Types
  Hooks --> Types
```

The desired dependency direction is left/top toward right/bottom. A feature import
from a UI primitive, or a page import from a utility, is an architectural warning.

### Section 23 closeout

**Key takeaways**

- The entire app is one root provider tree plus a heavier authenticated provider
  tree.
- REST, upload storage, and realtime Firebase have separate lifecycles.
- Route protection happens after hydration.
- Upload is a multi-system state machine, not a single HTTP request.

**Common pitfalls**

- Reading route-group names as URL segments.
- Assuming the consent banner gates scripts.
- Treating query invalidation as universal when tags are inconsistent.
- Moving a component without tracing context dependencies.

**Questions to ask a mentor**

- Which provider can be made route-specific first?
- What is the authoritative upload-state transition contract?
- Are payment callback routes intentionally protected?
- Which external system owns each production incident class?

**Related files to read next**

- `src/app/layout.tsx`
- `src/app/(withSidebarLayout)/layout.tsx`
- `src/components/providers.tsx`
- `src/store/api/*`
- `src/components/providers/upload/*`

**Practical exercises**

- Redraw the provider tree from memory.
- Add one endpoint and show its request/cache path.
- Mark every security boundary on the auth diagram.
