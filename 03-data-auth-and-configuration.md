# Sections 11–12 and 19–21 — Data, Authentication, and Configuration

## Section 11 — Data Fetching and API Integration

### The shared REST path

All RTK Query endpoints are injected into one `baseApi`. Its custom
`axiosBaseQuery` passes URL, method, body, params, headers, content type, and
`withCredentials` to a shared Axios instance. The instance unwraps successful
responses to `response.data`; consequently, an RTK Query hook's `data` is the
backend response body, not an Axios response object.

The response error interceptor currently returns the Axios error instead of
rejecting it. That resolves the promise and can turn an HTTP failure into RTK
Query `data`. The intended implementation is `return Promise.reject(error)`;
fixing it needs response-shape regression tests.

Endpoint arguments are mostly untyped `data`. The UI commonly sends
`userName`, `apiKey`, `appName`, `versionName`, plan identifiers, or form
objects. The exact backend schemas are not available in this repository, so
payload details below are based on call sites rather than an external contract.

### RTK Query endpoint inventory

The path suffixes below are appended to the environment-selected AGW or Auth
base URL. Unless stated otherwise, the request body is the hook argument.

#### Authentication and team (`store/api/auth.ts`)

| Generated hook operation | Method and path | Credentials/cache | Consumer intent |
|---|---|---|---|
| `loginStatus` | `GET AUTH /api/v2/session/state` | Cookie; provides `loginStatus` | Establish the current account session in protected layouts and auth screens. |
| `signIn` | `POST AGW /api/v3/arf/sign-in` | Body | Exchange email/password or provider credentials for an identity token. |
| `logout` | `POST AUTH /api/v2/auth/sign-out` | Cookie | End the backend browser session. |
| `signUp` | `POST AGW /api/v3/arf/sign-up` | Body | Create an account after OTP and availability checks. |
| `verifyEmail` | `POST AGW /api/v3/oc/check-mail` | Body | Check whether an email can be used. |
| `verifyUserName` | `POST AGW /api/v3/oc/check-username` | Body | Check username availability. |
| `changeUsername` | `PUT AUTH /api/v2/user/username` | Cookie; invalidates `loginStatus` | Set or replace the account username. |
| `changeEmail` | `PUT AUTH /api/v2/user/ownership-transfer` | Cookie | Transfer account ownership/email. |
| `sendOtp` | `POST AGW /api/v3/oc/send-otp` | Body | Send signup verification code. |
| `verifyOtp` | `POST AGW /api/v3/oc/verify-otp` | Body | Validate the signup code. |
| `generateCookie` | `POST AUTH /api/v2/session/token` | `Authorization: Bearer <token>`; cookie response | Convert an identity token into the application session. |
| `teamInfo` | `GET AUTH /api/v2/team/infos` | Cookie | Resolve owner/team context. |
| `invitedTeamMember` | `GET AUTH /api/v2/team/members` | Cookie; provides `teamMembers` | List invited members. |
| `removeTeamMember` | `DELETE AUTH /api/v2/team/member` | Cookie; invalidates `teamMembers` | Remove a member/invitation. |
| `inviteNewMember` | `POST AUTH /api/v2/team/member` | Cookie; invalidates `teamMembers` | Invite a member. |
| `updatePhoneNumber` | `POST AUTH /api/v2/user/upgrade-profile` | Cookie | Store profile phone information. |
| `removeUsername` | `DELETE AUTH /api/v2/user/unsubscribe` | Cookie | Remove/unsubscribe the username. |
| `deleteAccount` | `DELETE AUTH /api/v2/user/delete-account` | Cookie | Permanently request account deletion. |
| `resetPassword` | `POST AGW /api/v3/arf/reset-password` | Body | Send/start password reset. |
| `resetPassConfirm` | `POST AGW /api/v3/arf/set-password` | Body | Complete password reset. |

#### User, notifications, and developer credentials

| Module / operation | Method and path | Cache behavior | Consumer intent |
|---|---|---|---|
| user-info `customerId` | `POST AGW /api/v3/pay/get-customer-id` | None | Resolve payment customer identifier. |
| user-info `userAllInfo` | `POST AGW /api/v3/fb/getUserInfo` | None | Load account metadata used by billing/features. |
| user-info `profileLogo` | `POST AGW /api/v3/us/logo-list` | Provides `logoList` | Fetch normalized profile-logo assets. |
| user-info `notification` | `POST AGW /api/v3/fb/fetch-cp-notifications` | Provides `notification` | Populate notification menu. |
| user-info `removeSingleNotification` | `POST AGW /api/v3/fb/remove-cp-notification` | Invalidates `notification` | Delete one notification. |
| user-info `clearAllNotification` | `POST AGW /api/v3/fb/clear-cp-notifications` | Invalidates `notification` | Delete all notifications. |
| user-info `readAllNotification` | `POST AGW /api/v3/fb/read-cp-notifications` | Invalidates `notification` | Mark notifications read. |
| user-info `uploadLogo` | `POST AGW /api/v3/us/logo-signed-url` | Invalidates `logoList` | Obtain profile-logo upload URL. |
| user-info `uploadComplete` | `POST AGW /api/v3/us/on-upload-complete` | Invalidates `logoList` | Notify backend after upload. |
| api-key `apiKey` | `GET AUTH /api/v2/user/api-key` | Cookie; transforms nested keys | Load control-panel and streaming keys. |
| api-key `generateToken` | `POST AUTH /api/v2/ext/token-creation` | Cookie | Generate a developer token. |
| api-key `generateApiKey` | `PUT AUTH /api/v2/user/info` | Cookie | Rotate/generate API-key information. |
| api-key `updateVersionControl` | `PUT AUTH /api/v2/user/info` | Cookie | Update developer version-control metadata. |

#### Application assets, configurations, and analytics

| Module / operation | Method and path | Cache behavior | Consumer intent |
|---|---|---|---|
| asset `streamingApp` | `POST AGW /streamingapp-list[-r2]` | Provider-selected path; provides streaming `LIST` | List app ZIPs grouped by app/version. |
| asset `streamingAppThumbnail` | `POST AGW /api/v3/us/thumbnail-list` | Provides streaming `THUMBNAIL` | List thumbnails. |
| asset `dsApp` | `POST AGW /api/v3/us/fetchAllUserAssetInfo` | Provides `dsUpload` | List dedicated-server packages. |
| asset `asset2d` | `POST AGW /api/v3/us/asset2d-list` | Provides `asset2d` | List 2D assets. |
| asset `assetVideo` | `POST AGW /api/v3/us/assetvideo-list` | Provides `assetVideo` | List video assets. |
| appLink `appUrl` | `POST AGW /api/v3/fb/info-to-construct-url-list` | Provides `streamingApp` | Fetch saved launch-link definitions. |
| appLink `deleteStreamingApp` | `POST AGW /streamingapp-remove[-r2]` | Invalidates app list and thumbnails | Delete an app through provider-selected endpoint. |
| appLink `saveAppUrl` | `POST AGW /api/v3/fb/info-to-construct-url-save` | None | Save generated launch URL metadata. |
| appLink `configList` | `POST AGW /api/v3/fb/config-list` | None | List saved streaming configurations. |
| appLink `createConfig` | `POST AGW /api/v3/fb/create-config` | None | Create configuration. |
| appLink `deleteConfig` | `POST AGW /api/v3/fb/delete-config` | None | Delete configuration. |
| appLink `deleteAppVersion` | `POST AGW /streamingapp-version-remove[-r2]` | Invalidates app list and thumbnails | Delete one version through provider-selected endpoint. |
| appLink `getConfig` | `POST AGW /api/v3/fb/get-config` | Provides `getConfig` | Fetch one config for the form. |
| appLink `editConfig` | `POST AGW /api/v3/fb/update-config` | Invalidates `getConfig` | Update one config. |
| streaming-app `exeInfoUpload` | `POST AGW /api/v3/upload-sequence/exe-info-upload` | None | Register extracted executable metadata. |
| streaming-app `deleteAppExeData` | `POST AGW /api/v3/upload-sequence/force-delete-exe-data` | None | Force-remove executable metadata. |
| analytics `streamRecord` | `POST AGW /api/v3/fb/stream-record` | None | Fetch dated stream sessions for charts/table. |
| minute `minuteStreamed` | `POST AGW /api/v3/fb/singular-stream` | None | Fetch streamed-minute usage. |

The provider-selected paths expand to
`/api/v3/us/streamingapp-list` or `streamingapp-list-r2`,
`streamingapp-remove` or `streamingapp-remove-r2`, and
`streamingapp-version-remove` or `streamingapp-version-remove-r2`.

#### Uploads and dedicated servers

| Operation | Method and path | Invalidation | Consumer intent |
|---|---|---|---|
| `uploadStreamingAppSignedUrl` | `POST /api/v3/us/streamingapp-uv-signed-url-with-times` | `streamingApp` | Primary timestamped app upload URL. |
| `altUploadStreamingAppSignedUrl` | `POST /api/v3/us/streamingapp-alt-signed-url-filename-wise` | `streamingApp` | Alternate low-speed fallback URL. |
| `appThumbnailSignUrl` | `POST /api/v3/us/thumbnail-signed-url` | None | Thumbnail upload URL. |
| `asset2dUploadSignedUrl` | `POST /api/v3/us/asset2d-signed-url` | `logoList` (likely wrong tag) | 2D asset upload URL. |
| `assetVideoUploadSignedUrl` | `POST /api/v3/us/assetvideo-signed-url` | None | Video upload URL. |
| `profileLogoUploadSignedUrl` | `POST /api/v3/us/logo-signed-url` | None | Profile-logo upload URL. |
| `additionalUploadAppSignedUrl` | `POST /api/v3/us/additional-files-signed-url` | `additionalUpload` | Additional-file upload URL. |
| `additionalUploadAppList` | `POST /api/v3/us/additional-files-list` | Provides `additionalUpload` | List additional files. |
| `dedicatedServerAppUploadLink` | `POST /api/v3/us/getAppUploadLink` | `dsUpload` | Dedicated-server package URL. |
| `dedicatedServerInstanceList` | `POST /api/v3/ss/dedicated-servers` | Provides `dsList` | List running/server instances. |
| `dsInstanceStop` | `POST /api/v3/ss/stop-server-by-ip-port` | `dsList` | Stop an instance. |
| `dsInstanceStart` | `POST /api/v3/ss/start-server-app` | `dsList` | Start an instance. |
| `getLocationDetails` | `POST /api/v3/geo/get-location-from-ip-p` | None | Resolve IP region for the monitor. |
| `kickPlayer` | `POST /api/v3/mmlinker/KickPlayersForCS` | None | Disconnect a streaming player. |
| `dsAppDelete` | `POST /api/v3/us/deleteApp` | `dsUpload` | Delete a dedicated-server package. |

#### Legacy subscription, storage, card, budget, and Core billing

| Module | Query operations | Mutation operations |
|---|---|---|
| legacy subscription | `ppccuStatus`, `pplStatus`, `prepaidMinuteStatus`, `ppmStatus`; PPCCU/PPL/prepaid quote queries | Create PPCCU/PPL/prepaid/PPM; cancel all or PPM; reactivate all; upgrade/downgrade PPCCU, PPL, prepaid; change auto-renew; add watcher. All are `POST` AGW calls. |
| storage | Status `/check-subscription-status-storage`, utilized `/size-r2`, quote `/get-subscription-quote-storage` | Create `/create-subscription-storage`, upgrade `/upgrade-subscription-storage`, downgrade shared `/downgrade-subscription`; `POST`. |
| credit card | Payment methods `/get-payment-method`, default `/get-default-payment-method` | Save default `/set-default-payment-method`, set up `/setup-payment-method`; `POST`. |
| budget | Info `/budget-change-get` | Enable/add, increase, decrease, remove using the corresponding `/api/v3/pms/budget-change-*` paths; `POST`. |
| Core payment | Invoice list; Core, GB, and minute status; coded quote | Change card, subwise change and confirmation; checkout Core; upgrade/downgrade; non-checkout minute/GB; unpause; all `POST`. |

For exact operation-level coverage, every billing endpoint injected into RTK Query
is listed below. Every one uses `POST` and the endpoint argument as its body.

| Legacy subscription operation | AGW path |
|---|---|
| `ppccuStatus` | `/api/v3/pay/check-subscription-status-ppccu` |
| `pplStatus` | `/api/v3/pay/check-subscription-status-ppl` |
| `prepaidMinuteStatus` | `/api/v3/pay/check-subscription-status-pre-paid-minute` |
| `ppmStatus` | `/api/v3/pms/status` |
| `createPpmSub` | `/api/v3/pms/create-subscription-ppm` |
| `createPpccuSub` | `/api/v3/pay/create-subscription-ppccu` |
| `createPplSub` | `/api/v3/pay/create-subscription-ppl` |
| `createPrepaidMinuteSub` | `/api/v3/pay/create-subscription-pre-paid-minute` |
| `cancelAllSub` | `/api/v3/pay/pro-cancel-all-subscriptions` |
| `cancelPpmSub` | `/api/v3/pms/cancel` |
| `reactiveAllSub` | `/api/v3/pay/reactivate-all-subscriptions` |
| `ppccuQuote` | `/api/v3/pay/get-subscription-quote-ppcu` |
| `pplQuote` | `/api/v3/pay/get-subscription-quote-ppl` |
| `prepaidMinuteQuote` | `/api/v3/pay/get-subscription-quote-pre-paid-minute` |
| `upgradePpccu` | `/api/v3/pay/upgrade-subscription-ppccu` |
| `downgradePpccu` | `/api/v3/pay/downgrade-subscription` |
| `upgradePpl` | `/api/v3/pay/upgrade-subscription-ppl` |
| `downgradePpl` | `/api/v3/pay/downgrade-subscription` |
| `upgradePrepaidMinute` | `/api/v3/pay/upgrade-subscription-pre-paid-minute` |
| `downgradePrepaidMinute` | `/api/v3/pay/downgrade-subscription` |
| `addWatcher` | `/api/v3/pms/add-watcher` |
| `autoRenewSub` | `/api/v3/pay/manipulate-metadata` |

| Storage/card/budget operation | AGW path |
|---|---|
| `storageStatus` | `/api/v3/pay/check-subscription-status-storage` |
| `storageUtilized` | `/api/v3/us/size-r2` |
| `storageQuote` | `/api/v3/pay/get-subscription-quote-storage` |
| `createStorageSub` | `/api/v3/pms/create-subscription-storage` |
| `upgradeStorage` | `/api/v3/pay/upgrade-subscription-storage` |
| `downgradeStorage` | `/api/v3/pay/downgrade-subscription` |
| `paymentMethod` | `/api/v3/pay/get-payment-method` |
| `defaultPaymentMethod` | `/api/v3/pay/get-default-payment-method` |
| `saveCard` | `/api/v3/pay/set-default-payment-method` |
| `setupCard` | `/api/v3/pay/setup-payment-method` |
| `budgetInfo` | `/api/v3/pms/budget-change-get` |
| `budgetEnable` | `/api/v3/pms/budget-change-add` |
| `budgetIncrease` | `/api/v3/pms/budget-change-increase` |
| `budgetDecrease` | `/api/v3/pms/budget-change-decrease` |
| `budgetRemove` | `/api/v3/pms/budget-change-remove` |

| Core/new payment operation | AGW path |
|---|---|
| `editPayment` | `/api/v3/pay/payment-method` |
| `editPaymentForCore` | `/api/v3/pay/payment-method-subwise` |
| `paymentMethodConfirmationCore` | `/api/v3/pay/confirm-payment-method-change` |
| `invoiceList` | `/api/v3/pay/list-invoices` |
| `createSubscriptionPlan` | `/api/v3/pay/create-core-subscription-checkout-session` |
| `checkStatus` | `/api/v3/pay/check-core-subscription-status` |
| `checkStatusGb` | `/api/v3/pay/check-gb-subscription-status` |
| `checkStatusMin` | `/api/v3/pay/check-min-subscription-status` |
| `dataQuote` | `/api/v3/pay/get-coded-subscription-quote` |
| `upgradeSubscription` | `/api/v3/pay/upgrade-coded-subscription` |
| `downgradeSubscription` | `/api/v3/pay/downgrade-coded-subscription` |
| `nonCheckoutCreateMinSubscription` | `/api/v3/pay/create-min-subscription-non-checkout` |
| `nonCheckoutCreateGbSubscription` | `/api/v3/pay/create-gb-subscription-non-checkout` |
| `unPauseSubscription` | `/api/v3/pay/unpause-subscription` |

The full symbolic-to-literal mapping lives in `src/config/api.ts`; endpoint wrappers
live in `src/store/api/*.ts`. Status queries provide plan-specific tags. Most plan
mutations invalidate those tags, but create legacy/storage/card operations are
inconsistent, so some screens manually refetch or risk stale data.

### Direct Axios, storage, and Firebase calls

Not all network traffic passes through RTK Query:

- `services/storage-provider.ts` calls `GET /api/storage-provider` and caches
  `coreweave | r2` for the lifetime of the page.
- `services/r2/*` call initiate, batched part-URL, and complete endpoints. Upload
  parts go directly to signed R2 URLs with six concurrent uploads and three
  exponential-backoff retries.
- Upload providers send `PUT` requests to signed primary/alternate storage URLs
  with progress and `AbortController` cancellation.
- Profile, 2D, video, additional, and dedicated-server upload hooks PUT files to
  their signed URL, then make completion/refetch calls.
- Upload-sequence helpers call notification, upload-log, first-upload, and
  app-upload-mail endpoints directly.
- `UnCaughtErrorCatcher` and feature helpers POST error/activity data to the
  notification service.
- Firestore `onSnapshot` listeners supply app-processing progress, live streaming
  sessions, dedicated-server sessions, and executable metadata.
- Firebase Auth supplies Google popup sign-in against the named `accounts` app.

| Direct symbolic call | Method/path or destination | Owner |
|---|---|---|
| `Storage_Provider` | `GET AGW /api/storage-provider` | Storage-provider service. |
| `R2_Initiate_Upload` | `POST AGW /api/v3/us/r2-initiate-upload` | R2 service. |
| `R2_Batch_Part_Urls` | `POST AGW /api/v3/us/r2-batch-part-urls` | R2 service. |
| `R2_Complete_Upload` | `POST AGW /api/v3/us/r2-complete-upload` | R2 service. |
| `Upload_Complete_Call` | `POST AGW /api/v3/us/on-upload-complete` | Common/profile upload completion. |
| `Notify_First_Upload` | `POST AGW /api/v3/fb/notify-on-upload` | Upload notification helper. |
| `Create_Notification` | `POST AGW /api/v3/fb/create-cp-notification` | Upload/control-panel notification helper. |
| `Create_Upload_Log` | `POST AGW /api/v3/fb/create-upload-system-log` | Upload operational log. |
| `App_Upload_Mail` | `POST AGW /api/v3/upload-sequence/app-upload-mail` | Upload email helper. |
| `Telegram_Notify` | `POST https://notifications.eagle3dstreaming.com/message_sent` | Error/upload operational notifications. |
| `Geo_Location` | `GET https://geolocation-db.com/json/` | Client geolocation helper. |
| `Get_Location_Details` | `POST AGW /api/v3/geo/get-location-from-ip-p` | Location helper and RTK endpoint. |
| `Binary_EL_Download` | `AUTH /api/v2/ext/download-binary-el` | Developer binary download. |
| provider list/delete constants | The normal/R2 paths documented above | Endpoint selector used by RTK query functions. |

Fourteen constants are declared in `config/api.ts` but have no source consumer in
this snapshot: `Create_PPM_Invoice`, `Streaming_App_Signed_Url`,
`Streaming_App_Alt_Signed_Url`, `Budget_Api`,
`Set_Default_Payment_Method`, `Check_Promo`, `Shared_Drive_Download`,
`App_Test_Queue_Info`, `Delete_Temp_App`, `Stream_Test_Health`,
`Redeem_Promo`, `Old_Upload_Signed_Url`, `Stream_Test`, and
`GCS_Upload_Hook`. Confirm backend/roadmap ownership before deleting them.

### Loading, errors, caching, and invalidation

- Queries expose `isLoading`, `isFetching`, `isError`, `error`, `refetch`; usage is
  inconsistent by feature.
- Mutations usually call `.unwrap()`, show Sonner toasts, and sometimes explicitly
  refetch.
- The RTK cache has default lifetimes; there is no polling or global retry policy.
- Tag names are centralized in `store/tag-types.ts`.
- Firestore listeners unsubscribe in effect cleanup in the major monitor/upload
  flows, but listener error callbacks are sparse.
- Upload workflow state is in Context and disappears on refresh. REST query cache
  also resets because the Redux store is not persisted.

### Section 11 closeout

**Key takeaways**

- REST server state is centralized in one RTK Query API, while uploads and realtime
  state use direct Axios and Firebase.
- Most payloads and responses are untyped.
- Provider selection changes app list/delete endpoints at runtime.
- A shared Axios interceptor currently undermines reliable error classification.

**Common pitfalls**

- Treating RTK `data` as an Axios response.
- Expecting a create mutation to invalidate a corresponding query automatically.
- Forgetting that selected-member API keys affect most AGW payloads.
- Adding a direct Axios call without cancellation, telemetry review, or cache sync.

**Questions to ask a mentor**

- Where are authoritative OpenAPI schemas and error envelopes?
- Which endpoints require cookie auth, API-key body auth, or both?
- What are the intended retry/idempotency rules?
- Which API constants are deprecated?

**Related files to read next**

- `src/store/api/base-api.ts`
- `src/store/api/*.ts`
- `src/helpers/axios/*`
- `src/config/api.ts`
- `src/services/*`

**Practical exercises**

- Trace `useStreamingAppQuery` from component to provider-selected URL.
- Simulate a 401 and inspect current RTK state.
- Add types to one endpoint without using `any`.

---

## Section 12 — Authentication and Authorization

### Identity and session model

This frontend combines two identity systems:

1. The AGW authentication endpoints validate email/password and return a token.
   Google sign-in obtains the equivalent identity through Firebase Auth.
2. `generateCookie` sends that token to the Auth service with a Bearer header and
   `withCredentials: true`. The backend creates the browser session cookie.
3. Subsequent session/team/API-key calls use the cookie. Most domain requests also
   include `userName` and an API key in their body.

Cookie name, `HttpOnly`, `Secure`, `SameSite`, expiry, refresh, revocation, and CSRF
behavior are backend-owned and cannot be verified from this repository.

### Login flow

```text
signin page
  -> validate local email/password fields
  -> signIn mutation (AGW)
  -> extract returned identity token
  -> generateCookie mutation (Auth, Bearer token, credentials included)
  -> loginStatus refetch
  -> redirect to explicit redirect / decoded continue URL / "/"
```

Google sign-in starts with `signInWithPopup(firebaseServices.accounts.auth,
GoogleAuthProvider)`, obtains the Firebase ID token, then joins the same cookie
generation path.

The redirect target is accepted from URL state and assigned to
`window.location.href`. No same-origin allowlist is visible, so an attacker can
potentially construct an open redirect. Restrict targets to internal paths or an
explicit trusted-origin list.

### Signup and password reset

Signup is a client-driven sequence: validate fields, check email availability,
send OTP, verify OTP, check username availability, call signup, sign in, generate
the cookie, and continue to username/profile completion. The form keeps its own
state rather than using React Hook Form.

`/repass` starts or completes password reset through `resetPassword` and
`resetPassConfirm`. `/fb-mail` is a public support/mail route. Exact token lifetime
and reset-link validation are backend responsibilities and are not shown here.

### Protected-layout gate

There is no `middleware.ts` and no server-side authorization check in the repo.
`src/app/(withSidebarLayout)/layout.tsx` protects routes after hydration:

1. Redirect an old-panel hostname to `CURRENT_CP_URL`.
2. Query login status.
3. If not signed in, replace the route with `/signin`.
4. If signed in without a username, replace with `/username`.
5. Load API key and customer ID.
6. Until required context is ready, render a full-page loader.
7. Render sidebar/header plus the upload and configuration providers.

This improves UI behavior but is not a security boundary. A static page shell can
still be requested, and all APIs must independently authenticate and authorize.
Payment result pages are also under this gate, which may prevent a signed-out user
from viewing a return page.

### Roles and permission sources

| Effective role | How the frontend identifies it | Visible effect |
|---|---|---|
| Anonymous | `loginStatus` absent/false | Public auth routes; sidebar routes redirect. |
| Account owner | Session email equals `ownerEmail` | Owner-only billing/account/team controls in scattered conditions. |
| Team member | Team/session metadata; optional selected member | Uses selected member identity/API key for operational views. |
| Monitor admin | `isAdmin` flag | Can see employee/test sessions hidden from normal users. |
| Super Admin link session | Decoded URL payload stored in browser storage | Impersonates a supplied email, username, and API key. |

There is no central route-to-role matrix or authorization component. UI checks are
distributed through hooks and JSX. Never rely on those checks for billing, account
deletion, key rotation, player kicks, or server operations; backend authorization
must be the source of truth.

### Team-member selection

`selectedTeamMember` in `localStorage` carries a selected user's email, username,
and API key. `useUserInfo` prefers it over the signed-in owner's values, making
feature hooks operate on that member. This is a browser convenience, not proof of
permission. Logout/session changes should clear it, and backend calls must verify
that the session can act on the target account.

### Super Admin mechanism — actual behavior

`SuperAdminHandler` reads a base64-encoded query payload containing `mail`, `user`,
and `apiKey`. It clears **all** local and session storage, writes those credentials
to `selectedTeamMember`, stores Super Admin metadata, and records a local access
log. Contrary to `docs/SUPER_ADMIN_LOGIN.md`, the implementation intentionally
retains the credential-bearing query parameter.

Current risks:

- credentials appear in browser history, copied links, screenshots, referrers, and
  monitoring data;
- credentials live in readable `localStorage`, so any XSS can obtain them;
- the payload has no visible signature, expiry, single-use nonce, or backend
  exchange;
- clearing all web storage can remove unrelated preferences/workflow state;
- the audit log is local and therefore mutable and non-central;
- GTM and Hotjar load globally, increasing the need for strict URL redaction.

A safer design is a short-lived, signed, single-use server token exchanged via a
backend endpoint for an audited, scoped impersonation session, followed immediately
by URL replacement. Do not create more links using this client-only format.

### Logout and session expiry

The navbar/account UI calls the logout mutation, clears relevant local state, and
redirects. There is no global 401 handler, refresh-token queue, or broadcast-channel
coordination across tabs. Session expiry is detected when queries refetch or the
protected layout remounts. Because the Axios interceptor can hide HTTP failures,
expiry handling may be unreliable until that transport bug is fixed.

### Consent and third-party scripts

Google Tag Manager is inserted `beforeInteractive` and Hotjar `afterInteractive`
from the root layout. `ConsentBanner` renders later inside providers. Therefore the
banner does not prevent those scripts from loading before consent; it records a
choice after the fact. If legal policy requires prior consent, script insertion
must be gated.

### Section 12 closeout

**Key takeaways**

- Backend cookies establish the session; Firebase is an identity-token source, not
  the app's complete authorization system.
- Route protection is client-side and must be backed by API authorization.
- Owner/member/admin behavior is distributed rather than policy-driven.
- The current Super Admin and redirect mechanisms are high-priority security work.

**Common pitfalls**

- Calling UI hiding “authorization.”
- Assuming a Firebase login alone establishes the backend session.
- Putting secrets or long-lived API keys in URLs or `localStorage`.
- Redirecting to an unvalidated URL after authentication.

**Questions to ask a mentor**

- What are the session-cookie and CSRF policies?
- Which owner/member permissions are enforced per endpoint?
- Is Super Admin still in production use, and who can issue links?
- What third-party telemetry is legally approved before consent?

**Related files to read next**

- `src/app/(withSidebarLayout)/layout.tsx`
- `src/components/features/signin/*`
- `src/components/features/singup/*`
- `src/components/admin/*`
- `src/hooks/use-user-info.ts`

**Practical exercises**

- Draw the email and Google login flows.
- Build a same-origin redirect validator.
- Write a backend-oriented threat model for Super Admin.

---

## Section 19 — Environment Variables

### Complete public variable inventory

Every declared variable is prefixed `NEXT_PUBLIC_`, so Next.js can embed it into
browser bundles. Firebase web configuration is normally not a server secret, but
Firebase Security Rules, allowed domains, App Check, and backend authorization must
still prevent unauthorized access.

| Variable family | Six suffixes | Consumer |
|---|---|---|
| `NEXT_PUBLIC_ACCOUNT_FIREBASE_*` | `API_KEY`, `AUTH_DOMAIN`, `PROJECT_ID`, `STORAGE_BUCKET`, `MESSAGING_SENDER_ID`, `APP_ID` | Accounts Firebase Auth/Firestore app. |
| `NEXT_PUBLIC_APP_STREAM_MONITOR_FIREBASE_*` | Same six | Live application-stream monitor. |
| `NEXT_PUBLIC_DEDICATED_SERVER_MONITOR_FIREBASE_*` | Same six | Dedicated-server monitor. |
| `NEXT_PUBLIC_APP_EXE_DATA_FIREBASE_*` | Same six | Upload executable-processing data. |

`NEXT_PUBLIC_ENV` is the twenty-fifth variable. Only the exact value `staging`
selects staging configuration; absent, misspelled, or any other value selects
production.

### Files and environment precedence

- `.env.example` documents all 25 names with blank Firebase values.
- `.env.staging` sets only `NEXT_PUBLIC_ENV=staging`.
- `.env` and `.env.local` may exist locally but are ignored; never copy their values
  into documentation or logs.
- `config-loader.ts` imports both JSON configs at build time and selects one.
- Package scripts use `cross-env` to force `NEXT_PUBLIC_ENV`.

The current working installation declares `cross-env` but its executable was
missing from `node_modules/.bin`; the verified build therefore ran Next directly
with the environment variable set in PowerShell. A clean lockfile install should
restore the scripted path.

### Local setup

1. Use the repository's Node/pnpm version policy if the team supplies one; none is
   committed.
2. Install from `pnpm-lock.yaml` with the team-approved package manager.
3. Copy `.env.example` to `.env.local`.
4. Fill the four Firebase web-app configurations from an approved secret/config
   source.
5. Select `production` or `staging`; prefer staging for development.
6. Run `npm run dev:staging` (port 6400) after verifying `cross-env` is installed.

Do not use a production Firebase project merely because production is the default.
Fail-fast runtime validation should replace the current non-null assertions.

### Security rules

- Do not commit environment files containing real values.
- Do not print environment file contents in automation. The current root
  `verify-build-config.ps1` prints `.env.production` and `.env.staging` contents
  when present; change it to print variable names/presence only.
- Rotate a value if it has entered history, a ticket, chat, or screenshot.
- Assume all `NEXT_PUBLIC_*` values are public.
- Keep service-account keys and backend secrets out of this frontend entirely.

### Section 19 closeout

**Key takeaways**

- The app has 25 public environment variables.
- Four named Firebase projects are initialized in every client runtime.
- Environment selection defaults to production.
- There is no schema validation for missing or malformed variables.

**Common pitfalls**

- Treating `NEXT_PUBLIC_*` as secret.
- Starting without `NEXT_PUBLIC_ENV` and unintentionally calling production.
- Sharing `.env.local` to debug setup.
- Logging full env files in CI.

**Questions to ask a mentor**

- Which Firebase projects are safe for local development?
- What Node and pnpm versions are canonical?
- Where are values distributed and rotated?
- Are App Check and Firestore Security Rules enabled?

**Related files to read next**

- `.env.example`
- `.env.staging`
- `src/config/firebase.ts`
- `src/config/config-loader.ts`
- `verify-build-config.ps1`

**Practical exercises**

- Add non-secret presence validation for one Firebase group.
- Confirm a staging build contains staging API origins.
- Redact the CI verification script output.

---

## Section 20 — Configuration Files

### Runtime and application configuration

| File | Responsibility | Important behavior |
|---|---|---|
| `config.prod.json` | Production AGW, Auth, payment, legacy panel, home, Eagle domain, current panel, and R2 upload origins. | Imported into client-reachable config; values are origins, not credentials. |
| `config.staging.json` | Staging equivalents. | Selected only when `NEXT_PUBLIC_ENV === "staging"`. |
| `src/config/config-loader.ts` | Typed environment selector. | Defaults to production and imports both JSON files. |
| `src/config/api.ts` | Symbolic API catalogue and public origin exports. | Builds literal endpoints; includes both live and apparently unused legacy constants. |
| `src/config/endpoint-selector.ts` | Chooses CoreWeave versus R2 list/delete endpoints. | Awaits the page-lifetime cached provider. |
| `src/config/firebase.ts` | Creates four named Firebase apps/services. | Reuses existing named apps during hot reload. |
| `src/config/page.ts` | Page labels/navigation-oriented constants. | UI copy/config only. |
| `src/config/url.ts` | URL/config constants. | Centralizes link construction helpers/values. |
| `src/config/doc.ts` | Documentation-link constants. | Opens external product documentation from the UI. |

Do not add an endpoint literal inside a component. Put stable origins in the
environment JSON, symbolic paths in `api.ts`, and provider routing in
`endpoint-selector.ts`.

### Next.js and TypeScript

| File | Key settings | Consequences |
|---|---|---|
| `next.config.ts` | `images.remotePatterns` accepts HTTPS from hostname `**`. | Convenient for user assets, but broader than necessary; there are no headers, redirects, CSP, standalone/export output, or bundle settings. |
| `tsconfig.json` | Strict TypeScript, `noEmit`, bundler resolution, JSX preserve, `@/* -> ./src/*`, `allowJs`. | Next owns emit; one JS route is accepted; strictness is weakened locally by many `any`s. |
| `next-env.d.ts` | Next-generated type references. | Should not be manually edited. |

The semi-production workflow expects a `build/` directory for deployment, but
`next.config.ts` does not set `output: "export"` and normal Next builds produce
`.next/`. Treat that workflow as unverified/broken until the deployment target is
clarified.

### CSS, PostCSS, shadcn, and formatting

| File | Key settings | Consequences |
|---|---|---|
| `postcss.config.mjs` | Tailwind v4 PostCSS plugin plus Autoprefixer. | Drives `globals.css`; no `tailwind.config.*` exists. |
| `components.json` | shadcn New York style, stone base, CSS variables, RSC enabled, Lucide, import aliases. | UI primitives are copied source, not a black-box package. |
| `.prettierrc` | Formatting policy plus class-name and Tailwind plugins. | Reorders/normalizes long class strings. |
| `.prettierignore` | Formatting exclusions. | Check before expecting formatter changes. |
| `eslint.config.mjs` | Flat ESLint config, TypeScript/import/Next/Prettier integration, relaxed rules. | Build emits a large warning backlog; some accessibility and hook problems are not blocking. |
| `src/app/globals.css` | Tailwind imports, light/dark OKLCH tokens, base rules. | Main design-token source. |
| `src/styles/custom-font.ts` | Local Spantaran font loader. | Separate from root Asap Google font. |

### Package and process configuration

| File | Role | Notes |
|---|---|---|
| `package.json` | Scripts and dependency ranges. | All dev/start scripts use port 6400; build uses environment-specific `cross-env`. `next lint` is obsolete in newer Next flows and was not the verified build path. |
| `pnpm-lock.yaml` | Exact pnpm dependency graph. | Prefer frozen pnpm install for reproducibility. |
| `ecosystem.config.json` | PM2 process definition. | Sets `PORT=3448`, but `start:prod` hardcodes 6400, so the script wins. |
| `.npmrc` | Package-manager behavior. | Review alongside pnpm policy. |
| `.gitignore` | Excludes generated output, dependencies, and real env files. | `.env.example` and selected environment markers remain trackable. |
| `.vscode/settings.json` | Workspace editor defaults. | Developer convenience, not runtime behavior. |

### CI/CD workflows

| Workflow/script | Trigger and action | Review note |
|---|---|---|
| `.github/workflows/redeploy-prod.yml` | On pushes to `prod`, installs an SSH key, scans the configured host, and invokes `~/redeploy_cp_next.sh` on the VM. | The actual install/build/restart logic is external to this repository and cannot be audited here. |
| `.github/workflows/semi-prod.yml` | Semi-production deployment. | Expects static `build/`; incompatible with current Next config unless an omitted step provides it. |
| `verify-build-config.ps1` | Local verification of config files, env files, source wiring, and package scripts. | Prints env-file contents and checks for a missing/untracked `.env.production`; use presence-only validation. |

### Asset and metadata configuration

- `public/manifest.json` supplies PWA-style metadata but there is no service worker.
- Root `metadata` in `src/app/layout.tsx` supplies title/description/icons.
- `public/robots.txt` controls crawler hints.
- `public/fonts/*` hosts local font files.
- `public/*.svg|png|gif|ico` and `src/assets/images/*` are UI assets, not
  configuration, but filenames are referenced directly in components.

### Recommended change protocol

1. Decide whether a value is secret, environment-specific, endpoint-specific, or
   presentational.
2. Update the narrowest authoritative file.
3. Search all symbolic references with `rg`.
4. Verify staging first and inspect the selected network origin.
5. Run a production build.
6. Update this onboarding set and `.env.example` when the contract changes.

### Section 20 closeout

**Key takeaways**

- Configuration is split across environment JSON, symbolic API constants, Next,
  TypeScript, CSS/shadcn, package/process, and CI files.
- Production is the configuration fallback.
- Current PM2 and semi-prod expectations conflict with package/Next settings.
- Remote images and third-party scripts lack a restrictive security-header policy.

**Common pitfalls**

- Editing a generated or copied value instead of its authoritative source.
- Adding another environment switch inside a component.
- Assuming the lockfile used by CI matches the package manager.
- Changing deployment output without validating the remote workflow.

**Questions to ask a mentor**

- Is pnpm or npm canonical in CI?
- Is semi-production still active?
- Which remote image hosts can be allowlisted?
- Who owns deployment configuration and rollback?

**Related files to read next**

- All root `*.config.*` and config JSON files
- `src/config/*`
- `.github/workflows/*`
- `verify-build-config.ps1`

**Practical exercises**

- Explain which origin a misspelled environment selects.
- Reconcile PM2's port with the start script.
- Propose a restrictive image-host allowlist.

---

## Section 21 — Dependency Graph

### Provider and runtime dependency graph

```text
RootLayout (server)
└─ Providers (client)
   ├─ Redux Provider
   │  └─ store
   │     ├─ baseApi reducer + middleware
   │     └─ user slice
   ├─ ThemeProvider
   ├─ SuperAdminHandler
   ├─ route content
   ├─ ConsentBanner
   ├─ Sonner Toaster
   └─ UnCaughtErrorCatcher

SidebarLayout (client)
├─ loginStatus -> apiKey -> customerId gate
├─ AppSidebar + SiteHeader
└─ SidebarInset
   └─ UploadSequenceProvider
      └─ DedicatedServerUploadProvider
         └─ AdditionalUploadProvider
            └─ ConfigProvider
               └─ protected route page
```

An outer provider may be used by everything beneath it. Moving a route outside the
sidebar group removes all four feature contexts as well as the navigation shell.
Conversely, every protected route currently pays the initialization/render cost of
all four contexts, even when it does not use uploads or configuration.

### Module layers

```text
app pages/layouts
  -> feature components
     -> shared/UI components
     -> feature hooks + contexts
        -> RTK Query hooks
        -> services / Firebase / direct Axios
           -> config/api + environment config
     -> constants, utils, types

store
  -> baseApi
     -> axiosBaseQuery
        -> axiosInstance
  -> injected endpoint modules
     -> API constants
     -> endpoint selector -> storage-provider service
```

The intended direction is downward. UI primitives should not import a feature;
utilities should not import pages; API definitions should not import components.
Most code follows this, but feature components and hooks are tightly coupled to
response shapes and browser concerns.

### External system graph

| External system | Entry point | Used for |
|---|---|---|
| AGW API | `config/api.ts`, RTK/direct Axios | Product, upload, billing, analytics, profile, app operations. |
| Auth API | `config/api.ts`, cookie-enabled RTK | Sessions, team, ownership, credentials. |
| Accounts Firebase | `config/firebase.ts` | Google identity and account-oriented Firebase access. |
| Stream monitor Firebase | same | Live stream session listeners. |
| Dedicated-server Firebase | same | Live server listeners. |
| App-exe-data Firebase | same | Processing metadata/status. |
| CoreWeave/GCS-style signed storage | upload providers | Direct binary upload. |
| Cloudflare R2 signed multipart | `services/r2/*` | Large app uploads. |
| Stripe-backed billing APIs | AGW payment paths | Checkout/subscription/card operations; no Stripe SDK in browser. |
| Notification service | direct Axios | Operational/error chat notifications. |
| GTM / Hotjar | root layout scripts | Analytics/session behavior monitoring. |

### Circular and unusual dependencies

Static import analysis found one source cycle:

```text
features/dedicated-server/application.tsx
  -> dedicated-server-card-details.tsx
  -> application.tsx
```

Break it by extracting the shared type/state/action into a third module. Cycles make
initialization order, refactors, tests, and bundling harder to reason about.

Files with no internal importer are not automatically dead: App Router entry files,
config entry points, and tooling files are roots. After excluding obvious roots,
the strongest dead/unfinished candidates include:

- `components/admin/super-admin-url-generator.tsx`
- `features/new-subscription/terminated-new-subscription.tsx`
- `features/subscription/subscription-details/trial.tsx`
- `navbar/mode-toggle.tsx` and `navbar/ppccu-subscription.tsx`
- `shared/app-display-card.tsx`, `shared/data-table.tsx`,
  `shared/full-page-loader.tsx`, and `shared/logo/LaunchApp.tsx`
- sidebar `nav-projects.tsx` and `nav-user.tsx`
- UI breadcrumb, collapsible, popover, search-bar, and toggle-group wrappers
- `hooks/use-streamed.ts`
- empty `store/api/additional-uploads.ts`, `store/api/ds-app.ts`,
  `store/slices/additional-uploads.ts`, and `store/slices/ds-app.ts`
- `store/hooks.ts` and `styles/custom-font.ts`

Confirm dynamic/external use before deletion. The generated catalogue marks entry
points and zero-export placeholders so future reviewers can investigate safely.

### High-coupling areas

- `providers/upload/upload-sequence-provider.tsx` combines ZIP inspection, upload
  transport, provider routing, Firestore processing, retries, notifications, and
  UI state in more than 1,200 lines.
- The protected layout owns authentication plus the entire feature-provider stack.
- Subscription presentation components encode legacy and new-plan business rules.
- `useUserInfo` is a cross-cutting identity switch for owner/member/Super Admin.
- `config/api.ts` is the single endpoint registry but also retains many unused
  constants.

### Dependency upgrade strategy

1. Use `pnpm-lock.yaml` as the reproducible baseline.
2. Upgrade one coupled family at a time: Next/React, Redux, Firebase, Radix/shadcn,
   Tailwind/PostCSS, then leaf utilities.
3. Run typecheck/build and targeted manual journeys.
4. Pay special attention to Next 15/React 19 client boundaries and ESLint changes.
5. Avoid blind major upgrades while there are no tests.
6. Record breaking response/runtime changes in these docs.

### Section 21 closeout

**Key takeaways**

- The provider order is part of the application contract.
- REST, Firebase, storage, and third-party monitoring form four distinct external
  dependency paths.
- One confirmed source cycle and multiple dead/empty candidates need cleanup.
- Upload and subscription areas have the greatest coupling.

**Common pitfalls**

- Moving a page between route groups without restoring required providers.
- Deleting a zero-incoming file that is actually a framework/tool root.
- Importing feature logic into shared UI.
- Upgrading React/Next without exercising client-only flows.

**Questions to ask a mentor**

- Which unused candidates are planned work versus legacy residue?
- Why must every protected route mount every upload provider?
- Can the dedicated-server cycle be removed now?
- What is the supported upgrade cadence?

**Related files to read next**

- `src/components/providers.tsx`
- protected `layout.tsx`
- `src/store/*`
- `src/services/*`
- `src/components/providers/upload/*`

**Practical exercises**

- Move a context consumer on paper and list the providers it needs.
- Break the dedicated-server import cycle.
- Use a bundle analyzer to quantify provider cost.
