# `src/config/api.ts` reference

This document explains the endpoint registry in
[`src/config/api.ts`](../src/config/api.ts), how it selects production or
staging hosts, how the frontend sends requests, and where each endpoint is
used.

It documents the frontend contract that can be verified from this repository.
It is not a substitute for the backend's OpenAPI specification. Most RTK Query
operations accept an untyped `data` value, so this frontend alone cannot prove
every required request field or every possible response and error schema.

## Executive summary

- `api.ts` is a URL registry. It does not send requests itself.
- Environment-specific values come from
  [`config-loader.ts`](../src/config/config-loader.ts).
- `NEXT_PUBLIC_ENV=staging` selects `config.staging.json`. Every other value,
  including a missing value, selects `config.prod.json`.
- The environment choice happens when Next.js builds the browser bundle.
  Changing it requires a new build.
- Most requests are declared in the RTK Query slices under `src/store/api`.
- RTK Query uses a custom Axios adapter with a 60-second timeout and JSON
  headers.
- A small group of upload, telemetry, geolocation, and R2 requests use Axios
  directly.
- Storage-provider-aware operations first query `API.Storage_Provider`, then
  select either the CoreWeave/default endpoint or its R2 equivalent.
- There are 140 active properties in the `API` object. At the time of this
  audit, 126 are referenced in `src` and 14 are not referenced.

## Architecture

```text
NEXT_PUBLIC_ENV
      |
      v
config-loader.ts
      |
      +-- production (default) --> config.prod.json
      |
      `-- staging -------------> config.staging.json
                                      |
                                      v
                                  api.ts
                                      |
              +-----------------------+----------------------+
              |                       |                      |
              v                       v                      v
       RTK Query slices         Direct Axios calls    Exported page/upload URLs
       src/store/api/*          services/components   layout/upload provider
              |
              v
       axiosBaseQuery.ts
              |
              v
       axiosInstance.ts
```

## Environment selection

The loader starts with production configuration:

```ts
let config = prodConfig;
```

It switches only for this exact value:

```ts
if (process.env.NEXT_PUBLIC_ENV === 'staging') {
  config = stagingConfig;
}
```

The comparison is case-sensitive. For example, `Staging`, `STAGING`, an empty
string, and an undefined variable all use production.

### Project commands

| Purpose                             | Command              | Effective selector           |
| ----------------------------------- | -------------------- | ---------------------------- |
| Local production-mode configuration | `pnpm dev:prod`      | `NEXT_PUBLIC_ENV=production` |
| Local staging configuration         | `pnpm dev:staging`   | `NEXT_PUBLIC_ENV=staging`    |
| Production-host build               | `pnpm build:prod`    | `NEXT_PUBLIC_ENV=production` |
| Staging-host build                  | `pnpm build:staging` | `NEXT_PUBLIC_ENV=staging`    |
| Serve an existing production build  | `pnpm start:prod`    | `NEXT_PUBLIC_ENV=production` |
| Serve an existing staging build     | `pnpm start:staging` | `NEXT_PUBLIC_ENV=staging`    |

`NEXT_PUBLIC_ENV` is a public build-time value. Next.js can inline it in client
JavaScript. It must never contain a secret.

For the Vercel production domain to serve the application with staging hosts,
use `pnpm build:staging` as Vercel's Build Command. `pnpm dev:staging` is a
local development server and is not a deployment build.

### Environment host matrix

| Configuration property | Production                                     | Staging                                           |
| ---------------------- | ---------------------------------------------- | ------------------------------------------------- |
| `AGW_BASE_URL`         | `https://agw.eagle3dstreaming.com`             | `https://agw.eaglepixelstreaming.com`             |
| `AUTH_BASE_URL`        | `https://auth-restful.eagle3dstreaming.com`    | `https://auth-restful.eaglepixelstreaming.com`    |
| `STAGING_PAYMENT_URL`  | `https://stripe-api.eagle3dstreaming.com`      | `https://payment-tin-api.eaglepixelstreaming.com` |
| `PREVIOUS_CP_URL`      | `https://newcontrolpanel.eagle3dstreaming.com` | `https://newcontrolpanel.eaglepixelstreaming.com` |
| `HOME_URL`             | `https://account.eagle3dstreaming.com`         | `https://account.eaglepixelstreaming.com`         |
| `EAGLE_DOMAIN`         | `data.eagle3dstreaming.com`                    | `data.eaglepixelstreaming.com`                    |
| `CURRENT_CP_URL`       | `https://controlpanel.eagle3dstreaming.com`    | `https://controlpanel.eaglepixelstreaming.com`    |
| `R2_UPLOAD_DOMAIN`     | `upload-api.eagle3dstreaming.com`              | `upload-api.eaglepixelstreaming.com`              |

`STAGING_PAYMENT_URL` is loaded and assigned inside `api.ts`, but no active API
property currently uses it. The old Stripe routes that referenced it are
commented out. The active core-plan routes use `AGW_BASE_URL`.

## Exports outside the `API` object

`api.ts` also exports five environment-specific values:

| Export             | Current frontend use                                              |
| ------------------ | ----------------------------------------------------------------- |
| `PREVIOUS_CP_URL`  | Detects requests arriving through the previous control-panel URL. |
| `HOME_URL`         | Detects requests arriving through the account/home URL.           |
| `EAGLE_DOMAIN`     | Detects the data-domain host and redirects it to Analytics.       |
| `CURRENT_CP_URL`   | Target for the control-panel redirects above.                     |
| `R2_UPLOAD_DOMAIN` | Passed to R2 multipart-upload initialization.                     |

The redirect behavior is implemented in
[`src/app/(withSidebarLayout)/layout.tsx`](<../src/app/(withSidebarLayout)/layout.tsx>).
`R2_UPLOAD_DOMAIN` is consumed by
[`upload-sequence-provider.tsx`](../src/components/providers/upload/upload-sequence-provider.tsx).

## HTTP client behavior

### RTK Query path

[`base-api.ts`](../src/store/api/base-api.ts) creates one RTK Query API with an
empty base URL. This is intentional: every `API.*` value is already an absolute
URL.

[`axiosBaseQuery.ts`](../src/helpers/axios/axiosBaseQuery.ts) maps an RTK Query
request as follows:

| RTK Query field   | Axios field                                      |
| ----------------- | ------------------------------------------------ |
| `url`             | `url`                                            |
| `method`          | `method`                                         |
| `body`            | `data`                                           |
| `params`          | `params`                                         |
| `headers`         | merged into request headers                      |
| `contentType`     | `Content-Type`, defaulting to `application/json` |
| `withCredentials` | `withCredentials`                                |

[`axiosInstance.ts`](../src/helpers/axios/axiosInstance.ts) applies:

- `Accept: application/json`
- `Content-Type: application/json` for POST defaults
- a 60,000 ms request timeout
- a response interceptor that returns `response.data`

Because the response interceptor already unwraps Axios's response object, RTK
Query hooks normally receive the decoded response body rather than the full
Axios response.

### Credential behavior

Credentials are opt-in, not global. The verified credentialed operations are
primarily AUTH-service calls, including session state, sign-out, team
operations, profile changes, account deletion, API-key operations, and token
generation.

`Generate_Cookie` is special:

```ts
{
  method: 'POST',
  body: {},
  headers: {
    Authorization: `Bearer ${data.token}`
  },
  withCredentials: true
}
```

R2 AGW requests are also special. They send the application's API key in an
`apiKey` request header rather than through RTK Query:

```ts
{
  'Content-Type': 'application/json',
  apiKey
}
```

### Important error-handling caveat

The Axios response error interceptor currently does this:

```ts
error => {
  return error;
};
```

Returning an error resolves the Axios promise with the error object instead of
rejecting it. Consequently, `axiosBaseQuery` may report some HTTP failures as
successful `data` values instead of entering its `catch` block. If consumers
observe fulfilled RTK Query requests containing Axios error-shaped data, this
interceptor is the first place to inspect. A conventional Axios interceptor
normally uses `return Promise.reject(error)`.

This document records the current behavior; it does not change it.

## Dynamic storage-provider routing

[`storage-provider.ts`](../src/services/storage-provider.ts) sends:

```http
GET API.Storage_Provider
```

It expects:

```ts
{
  storageProvider: string;
}
```

The value is lowercased and cached in memory. If the request fails or the field
is missing, the frontend falls back to `coreweave`.

[`endpoint-selector.ts`](../src/config/endpoint-selector.ts) uses that provider
for three operation pairs:

| Operation                     | Default/CoreWeave              | R2                                |
| ----------------------------- | ------------------------------ | --------------------------------- |
| List streaming applications   | `Streaming_App_List`           | `Streaming_App_List_R2`           |
| Delete an application         | `Delete_Streaming_App`         | `Delete_Streaming_App_R2`         |
| Delete an application version | `Delete_Streaming_App_Version` | `Delete_Streaming_App_Version_R2` |

All three selected endpoints are called with `POST`. Provider detection occurs
before the operation. Once cached, the provider is not refreshed until the
page/application runtime is restarted.

## Endpoint inventory conventions

The tables below use these labels:

- **Used** means an active reference exists under `src`.
- **Unused** means the constant is active in `api.ts`, but no active `API.Name`
  reference exists under `src`.
- **Dynamic** means the endpoint is selected through `endpoint-selector.ts`.
- A method marked **Unknown** is not called by the current frontend, so the
  frontend cannot establish its HTTP method.
- Descriptions are based on the constant name, route, and verified call site.
  They are frontend descriptions, not promises about undocumented backend
  behavior.

## General external endpoints

These two endpoints do not use an environment-specific base URL.

| Constant          | Method | Absolute URL                                              | Frontend role                                                                                           |
| ----------------- | ------ | --------------------------------------------------------- | ------------------------------------------------------------------------------------------------------- |
| `Geo_Location`    | GET    | `https://geolocation-db.com/json/`                        | Retrieves public IP/location information before upload telemetry. Used.                                 |
| `Telegram_Notify` | POST   | `https://notifications.eagle3dstreaming.com/message_sent` | Sends operational, upload, and error notifications. Used. This host remains the same in staging builds. |

`Telegram_Notify` bodies observed in this repository contain
`input_chat_id` and `message`. Chat IDs are currently embedded in frontend
source, so they are visible in the browser bundle.

## AGW authentication and account-recovery endpoints

Base: `AGW_BASE_URL`

| Constant          | Method | Path                         | Frontend role                                                     |
| ----------------- | ------ | ---------------------------- | ----------------------------------------------------------------- |
| `Sign_In`         | POST   | `/api/v3/arf/sign-in`        | Sign in with the submitted credentials. Used by `authApi.signIn`. |
| `Sign_Up`         | POST   | `/api/v3/arf/sign-up`        | Register an account. Used by `authApi.signUp`.                    |
| `Email_Verify`    | POST   | `/api/v3/oc/check-mail`      | Check an email during registration/account flow. Used.            |
| `Username_Verify` | POST   | `/api/v3/oc/check-username`  | Check username availability/validity. Used.                       |
| `Send_OTP`        | POST   | `/api/v3/oc/send-otp`        | Request an OTP. Used.                                             |
| `Verify_OTP`      | POST   | `/api/v3/oc/verify-otp`      | Validate an OTP. Used.                                            |
| `Reset_Password`  | POST   | `/api/v3/arf/reset-password` | Start password reset. Used.                                       |
| `Set_Password`    | POST   | `/api/v3/arf/set-password`   | Confirm/set the replacement password. Used.                       |

The request bodies for these operations are passed through as untyped `data`
objects by [`auth.ts`](../src/store/api/auth.ts). Consult the form call sites
when changing their frontend payloads; the backend specification remains the
authority.

## AGW payment, subscription, and budget endpoints

Base: `AGW_BASE_URL`

### Existing PPCCU, PPL, PPM, prepaid-minute, and storage plans

| Constant                     | Method  | Path                                                    | Frontend role                                                                            |
| ---------------------------- | ------- | ------------------------------------------------------- | ---------------------------------------------------------------------------------------- |
| `Create_PPCCU_Sub`           | POST    | `/api/v3/pay/create-subscription-ppccu`                 | Creates a PPCCU subscription. Used.                                                      |
| `Create_PPM_Invoice`         | Unknown | `/api/v3/pay/basic-create-invoice`                      | Legacy/basic PPM invoice creation. Unused.                                               |
| `Upgrade_PPCCU_Sub`          | POST    | `/api/v3/pay/upgrade-subscription-ppccu`                | Upgrades PPCCU. Used.                                                                    |
| `Check_Sub_PPCCU`            | POST    | `/api/v3/pay/check-subscription-status-ppccu`           | Gets PPCCU status. Used.                                                                 |
| `Cancel_PPM`                 | POST    | `/api/v3/pms/cancel`                                    | Cancels a PPM subscription. Used.                                                        |
| `Cancel_All_Sub`             | POST    | `/api/v3/pay/pro-cancel-all-subscriptions`              | Cancels all applicable subscriptions and invalidates subscription caches. Used.          |
| `Check_Sub_PPM`              | POST    | `/api/v3/pms/status`                                    | Gets PPM status. Used.                                                                   |
| `Check_PPCCU_Quote`          | POST    | `/api/v3/pay/get-subscription-quote-ppcu`               | Gets a PPCCU quote. Used. The route spells `ppcu`, while the constant spells `PPCCU`.    |
| `Check_Storage_Status`       | POST    | `/api/v3/pay/check-subscription-status-storage`         | Gets storage-subscription status. Used.                                                  |
| `Create_Storage_Sub`         | POST    | `/api/v3/pms/create-subscription-storage`               | Creates a storage subscription. Used.                                                    |
| `Upgrade_Storage_Sub`        | POST    | `/api/v3/pay/upgrade-subscription-storage`              | Upgrades storage. Used.                                                                  |
| `Reactivate_PPCCU_Sub`       | POST    | `/api/v3/pay/reactivate-all-subscriptions`              | Reactivates all applicable subscriptions despite the PPCCU-specific constant name. Used. |
| `Downgrade_Sub`              | POST    | `/api/v3/pay/downgrade-subscription`                    | Shared downgrade route used for PPCCU, PPL, prepaid minute, and storage.                 |
| `Check_Storage_Quote`        | POST    | `/api/v3/pay/get-subscription-quote-storage`            | Gets a storage quote. Used.                                                              |
| `Create_PPM_Sub`             | POST    | `/api/v3/pms/create-subscription-ppm`                   | Creates a PPM subscription. Used.                                                        |
| `Check_Sub_PPL`              | POST    | `/api/v3/pay/check-subscription-status-ppl`             | Gets PPL status. Used.                                                                   |
| `Create_Sub_PPL`             | POST    | `/api/v3/pay/create-subscription-ppl`                   | Creates a PPL subscription. Used.                                                        |
| `Upgrade_Sub_PPL`            | POST    | `/api/v3/pay/upgrade-subscription-ppl`                  | Upgrades PPL. Used.                                                                      |
| `Check_PPL_Quote`            | POST    | `/api/v3/pay/get-subscription-quote-ppl`                | Gets a PPL quote. Used.                                                                  |
| `Create_Sub_Prepaid_Minute`  | POST    | `/api/v3/pay/create-subscription-pre-paid-minute`       | Creates a prepaid-minute subscription. Used.                                             |
| `Check_Sub_Prepaid_Minute`   | POST    | `/api/v3/pay/check-subscription-status-pre-paid-minute` | Gets prepaid-minute status. Used.                                                        |
| `Check_Quote_Prepaid_Minute` | POST    | `/api/v3/pay/get-subscription-quote-pre-paid-minute`    | Gets a prepaid-minute quote. Used.                                                       |
| `Upgrade_Prepaid_Minute`     | POST    | `/api/v3/pay/upgrade-subscription-pre-paid-minute`      | Upgrades prepaid minutes. Used.                                                          |
| `Auto_Renew_Sub`             | POST    | `/api/v3/pay/manipulate-metadata`                       | Changes subscription metadata used by the auto-renew flow. Used.                         |
| `Add_Watcher`                | POST    | `/api/v3/pms/add-watcher`                               | Adds a PPM/subscription watcher. Used.                                                   |
| `Budget_Api`                 | Unknown | `/api/v3/pms`                                           | Base PMS budget route retained as a constant. Unused.                                    |
| `Redeem_Promo`               | Unknown | `/api/v3/pms/promo`                                     | Redeems a PMS promotion. Unused.                                                         |

### Payment methods, invoices, and promotions

| Constant                           | Method  | Path                                             | Frontend role                                                    |
| ---------------------------------- | ------- | ------------------------------------------------ | ---------------------------------------------------------------- |
| `Invoice_List`                     | POST    | `/api/v3/pay/list-invoices`                      | Fetches invoices. Used.                                          |
| `Default_Payment_Method`           | POST    | `/api/v3/pay/get-default-payment-method`         | Fetches the default payment method. Used.                        |
| `Payment_Method`                   | POST    | `/api/v3/pay/get-payment-method`                 | Fetches payment-method information. Used.                        |
| `Setup_Card`                       | POST    | `/api/v3/pay/setup-payment-method`               | Starts payment-method setup. Used.                               |
| `Save_Card`                        | POST    | `/api/v3/pay/set-default-payment-method`         | Saves/selects a default payment method. Used.                    |
| `Change_Credit_Card`               | POST    | `/api/v3/pay/payment-method`                     | Changes the general payment method. Used.                        |
| `Change_Credit_Card_For_Core`      | POST    | `/api/v3/pay/payment-method-subwise`             | Changes a payment method for a specific core subscription. Used. |
| `Payment_Method_Confirmation_Core` | POST    | `/api/v3/pay/confirm-payment-method-change`      | Confirms the core payment-method change. Used.                   |
| `Set_Default_Payment_Method`       | Unknown | `/api/v3/pay/set-default-payment-method-for-all` | Sets one payment method for all subscriptions. Unused.           |
| `Check_Promo`                      | Unknown | `/api/v3/pay/check-promo-code`                   | Validates a promotion code. Unused.                              |
| `Customer_Id`                      | POST    | `/api/v3/pay/get-customer-id`                    | Fetches the payment customer ID. Used.                           |

### Current core-plan endpoints

| Constant                                 | Method | Path                                                    | Frontend role                                                                                                         |
| ---------------------------------------- | ------ | ------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------- |
| `Core_Plan`                              | POST   | `/api/v3/pay/create-core-subscription-checkout-session` | Creates a core-plan checkout session. Used.                                                                           |
| `Check_Status`                           | POST   | `/api/v3/pay/check-core-subscription-status`            | Gets general core-plan status. Used.                                                                                  |
| `Quote`                                  | POST   | `/api/v3/pay/get-coded-subscription-quote`              | Gets a coded/core subscription quote. Used.                                                                           |
| `Upgrade_Quote`                          | POST   | `/api/v3/pay/upgrade-coded-subscription`                | Upgrades a coded/core subscription. Used.                                                                             |
| `Downgrade_Quote`                        | POST   | `/api/v3/pay/downgrade-coded-subscription`              | Downgrades a coded/core subscription. Used.                                                                           |
| `Unpause_Subscription`                   | POST   | `/api/v3/pay/unpause-subscription`                      | Unpauses a subscription and invalidates all related subscription caches. Used.                                        |
| `Core_Pan_Non_Checkout_Min_Subscription` | POST   | `/api/v3/pay/create-min-subscription-non-checkout`      | Creates a minute-based core subscription without checkout. Used. `Pan` in the constant appears to be a retained typo. |
| `Core_Pan_Non_Checkout_Gb_Subscription`  | POST   | `/api/v3/pay/create-gb-subscription-non-checkout`       | Creates a GB-based core subscription without checkout. Used.                                                          |
| `Check_Status_Gb`                        | POST   | `/api/v3/pay/check-gb-subscription-status`              | Gets GB-based core status. Used.                                                                                      |
| `Check_Status_Min`                       | POST   | `/api/v3/pay/check-min-subscription-status`             | Gets minute-based core status. Used.                                                                                  |

The previously planned `Core_Pan_Non_Checkout_Subscription` route is commented
out in both `api.ts` and the payment slice. It is not part of the active
140-property inventory.

### Budget-management endpoints

| Constant          | Method | Path                                 | Frontend role                     |
| ----------------- | ------ | ------------------------------------ | --------------------------------- |
| `Budget_Info`     | POST   | `/api/v3/pms/budget-change-get`      | Reads budget configuration. Used. |
| `Budget_Enable`   | POST   | `/api/v3/pms/budget-change-add`      | Enables/adds a budget. Used.      |
| `Budget_Increase` | POST   | `/api/v3/pms/budget-change-increase` | Increases the budget. Used.       |
| `Budget_Decrease` | POST   | `/api/v3/pms/budget-change-decrease` | Decreases the budget. Used.       |
| `Budget_Remove`   | POST   | `/api/v3/pms/budget-change-remove`   | Removes the budget. Used.         |

The corresponding RTK Query declarations are in
[`subscription.ts`](../src/store/api/subscription.ts),
[`payment.ts`](../src/store/api/payment.ts),
[`credit-card.ts`](../src/store/api/credit-card.ts),
[`storage.ts`](../src/store/api/storage.ts), and
[`budget-api.ts`](../src/store/api/budget-api.ts).

## AGW user, storage, asset, and application endpoints

Base: `AGW_BASE_URL`

### User information and usage

| Constant           | Method | Path                         | Frontend role                                                                                                |
| ------------------ | ------ | ---------------------------- | ------------------------------------------------------------------------------------------------------------ |
| `User_Info`        | POST   | `/api/v3/fb/getUserInfo`     | Fetches aggregated user information. Used.                                                                   |
| `Utilized_Storage` | POST   | `/api/v3/us/size-r2`         | Fetches utilized storage. Used. The route name is R2-specific even though the UI treats it as general usage. |
| `Minute_Streamed`  | POST   | `/api/v3/fb/singular-stream` | Fetches streamed-minute information. Used.                                                                   |
| `Stream_Record`    | POST   | `/api/v3/fb/stream-record`   | Fetches stream records for Analytics. Used.                                                                  |

### Lists and signed upload URLs

| Constant                                 | Method        | Path                                                   | Frontend role                                                  |
| ---------------------------------------- | ------------- | ------------------------------------------------------ | -------------------------------------------------------------- |
| `Streaming_App_List`                     | POST, dynamic | `/api/v3/us/streamingapp-list`                         | Lists default/CoreWeave streaming apps.                        |
| `Streaming_App_List_R2`                  | POST, dynamic | `/api/v3/us/streamingapp-list-r2`                      | Lists R2 streaming apps.                                       |
| `Asset_2D_List`                          | POST          | `/api/v3/us/asset2d-list`                              | Lists 2D assets. Used.                                         |
| `Asset_Video_List`                       | POST          | `/api/v3/us/assetvideo-list`                           | Lists video assets. Used.                                      |
| `Streaming_App_Signed_Url`               | Unknown       | `/api/v3/us/streamingapp-uv-signed-url`                | Older/non-timestamp streaming-app signed URL. Unused.          |
| `Streaming_App_Signed_Url_Timestamp`     | POST          | `/api/v3/us/streamingapp-uv-signed-url-with-times`     | Gets a timestamp-aware primary streaming-app upload URL. Used. |
| `Alt_Streaming_App_Signed_Url_Timestamp` | POST          | `/api/v3/us/streamingapp-alt-signed-url-filename-wise` | Gets a filename-aware alternative upload URL. Used.            |
| `Streaming_App_Alt_Signed_Url`           | Unknown       | `/api/v3/us/streamingapp-alt-signed-url`               | Older alternative signed URL. Unused.                          |
| `Thumbnail_Signed_Url`                   | POST          | `/api/v3/us/thumbnail-signed-url`                      | Gets a thumbnail upload URL. Used.                             |
| `Asset_2D_Signed_Url`                    | POST          | `/api/v3/us/asset2d-signed-url`                        | Gets a 2D-asset upload URL. Used.                              |
| `Asset_Video_Signed_Url`                 | POST          | `/api/v3/us/assetvideo-signed-url`                     | Gets a video-asset upload URL. Used.                           |
| `Logo_Signed_Url`                        | POST          | `/api/v3/us/logo-signed-url`                           | Gets a profile/logo upload URL. Used by two RTK operations.    |
| `Logo_List`                              | POST          | `/api/v3/us/logo-list`                                 | Lists profile logos. Used.                                     |
| `Thumbnail_List`                         | POST          | `/api/v3/us/thumbnail-list`                            | Lists application thumbnails. Used.                            |
| `Additional_Upload_Signed_Url`           | POST          | `/api/v3/us/additional-files-signed-url`               | Gets an additional-file upload URL. Used.                      |
| `Additional_File_List`                   | POST          | `/api/v3/us/additional-files-list`                     | Lists additional uploaded files. Used.                         |
| `Old_Upload_Signed_Url`                  | Unknown       | `/api/v3/us/streamingapp-signed-url`                   | Legacy streaming-app signed URL. Unused.                       |

List responses for streaming apps, thumbnails, dedicated-server apps, 2D
assets, and video assets are normalized from the observed nested location:

```ts
response?.data?.data?.blobs;
```

See [`asset.ts`](../src/store/api/asset.ts) and
[`normalizeAsset`](../src/utils/asset.ts).

### Application deletion and upload completion

| Constant                          | Method        | Path                                        | Frontend role                                                                   |
| --------------------------------- | ------------- | ------------------------------------------- | ------------------------------------------------------------------------------- |
| `Delete_Streaming_App`            | POST, dynamic | `/api/v3/us/streamingapp-remove`            | Deletes a default/CoreWeave application.                                        |
| `Delete_Streaming_App_R2`         | POST, dynamic | `/api/v3/us/streamingapp-remove-r2`         | Deletes an R2 application.                                                      |
| `Delete_Streaming_App_Version`    | POST, dynamic | `/api/v3/us/streamingapp-version-remove`    | Deletes a default/CoreWeave application version.                                |
| `Delete_Streaming_App_Version_R2` | POST, dynamic | `/api/v3/us/streamingapp-version-remove-r2` | Deletes an R2 application version.                                              |
| `Upload_Complete_Call`            | POST          | `/api/v3/us/on-upload-complete`             | Notifies AGW after an upload completes. Used by RTK Query and upload telemetry. |

### Dedicated-server endpoints

| Constant             | Method | Path                                | Frontend role                                                            |
| -------------------- | ------ | ----------------------------------- | ------------------------------------------------------------------------ |
| `DS_Instance_List`   | POST   | `/api/v3/ss/dedicated-servers`      | Lists dedicated-server instances. Used.                                  |
| `DS_Instance_Start`  | POST   | `/api/v3/ss/start-server-app`       | Starts an application/server instance. Used.                             |
| `DS_Instance_Stop`   | POST   | `/api/v3/ss/stop-server-by-ip-port` | Stops an instance identified by request data. Used.                      |
| `DS_App_Upload_Link` | POST   | `/api/v3/us/getAppUploadLink`       | Gets a dedicated-server application upload link. Used.                   |
| `DS_App_Delete`      | POST   | `/api/v3/us/deleteApp`              | Deletes a dedicated-server application. Used.                            |
| `DS_App_List`        | POST   | `/api/v3/us/fetchAllUserAssetInfo`  | Lists dedicated-server application assets. Used.                         |
| `Kick_Player`        | POST   | `/api/v3/mmlinker/KickPlayersForCS` | Requests player disconnection for a dedicated/multiplayer session. Used. |

## AGW configuration, notification, upload-sequence, and utility endpoints

Base: `AGW_BASE_URL`

### Application configuration and generated URLs

| Constant               | Method | Path                                    | Frontend role                                                              |
| ---------------------- | ------ | --------------------------------------- | -------------------------------------------------------------------------- |
| `Config_List`          | POST   | `/api/v3/fb/config-list`                | Lists application configurations. Used.                                    |
| `Get_Single_Config`    | POST   | `/api/v3/fb/get-config`                 | Fetches one configuration. Used.                                           |
| `Update_Single_Config` | POST   | `/api/v3/fb/update-config`              | Updates one configuration. Used.                                           |
| `Create_Config`        | POST   | `/api/v3/fb/create-config`              | Creates a configuration. Used.                                             |
| `Delete_Config`        | POST   | `/api/v3/fb/delete-config`              | Deletes a configuration. Used. The `//unsed` comment in `api.ts` is stale. |
| `Get_App_Url`          | POST   | `/api/v3/fb/info-to-construct-url-list` | Retrieves saved inputs used to construct app URLs. Used.                   |
| `Save_App_Url`         | POST   | `/api/v3/fb/info-to-construct-url-save` | Saves app-URL construction data. Used.                                     |

### Control-panel notifications and operational logging

| Constant              | Method | Path                                  | Frontend role                                   |
| --------------------- | ------ | ------------------------------------- | ----------------------------------------------- |
| `Notify_First_Upload` | POST   | `/api/v3/fb/notify-on-upload`         | Notifies AGW about a user's first upload. Used. |
| `Create_Notification` | POST   | `/api/v3/fb/create-cp-notification`   | Creates a control-panel notification. Used.     |
| `Fetch_Notification`  | POST   | `/api/v3/fb/fetch-cp-notifications`   | Fetches control-panel notifications. Used.      |
| `Remove_Notification` | POST   | `/api/v3/fb/remove-cp-notification`   | Removes one notification. Used.                 |
| `Read_Notification`   | POST   | `/api/v3/fb/read-cp-notifications`    | Marks notifications as read. Used.              |
| `Clear_Notification`  | POST   | `/api/v3/fb/clear-cp-notifications`   | Clears notifications. Used.                     |
| `Create_Upload_Log`   | POST   | `/api/v3/fb/create-upload-system-log` | Writes upload-system telemetry. Used.           |

The observed `Create_Notification` payload is:

```ts
{
  apiKey,
  uuid,
  message,
  link?: {
    url,
    title
  }
}
```

The observed `Notify_First_Upload` payload is:

```ts
{
  (apiKey, mail, appName);
}
```

### Upload sequence, shared drive, testing, and location

| Constant                | Method  | Path                                            | Frontend role                                                                         |
| ----------------------- | ------- | ----------------------------------------------- | ------------------------------------------------------------------------------------- |
| `Shared_Drive_Download` | Unknown | `/api/v3/sd/download`                           | Shared-drive download route. Unused.                                                  |
| `Get_Location_Details`  | POST    | `/api/v3/geo/get-location-from-ip-p`            | Resolves a supplied IP into location details. Used by RTK Query and upload telemetry. |
| `Exe_Info_Upload`       | POST    | `/api/v3/upload-sequence/exe-info-upload`       | Uploads executable metadata for the upload sequence. Used.                            |
| `Delete_Exe_Data`       | POST    | `/api/v3/upload-sequence/force-delete-exe-data` | Force-deletes executable/upload-sequence data. Used.                                  |
| `App_Test_Queue_Info`   | Unknown | `/api/v3/st/get-stream-test-queue-details`      | Gets stream-test queue details. Unused.                                               |
| `App_Upload_Mail`       | POST    | `/api/v3/upload-sequence/app-upload-mail`       | Sends upload-result email data. Used.                                                 |
| `Delete_Temp_App`       | Unknown | `/api/v3/sd/remove-temp-app-version`            | Removes a temporary app version. Unused.                                              |
| `Stream_Test_Health`    | Unknown | `/api/v3/st/stream-test-health`                 | Stream-test health endpoint. Unused.                                                  |
| `Stream_Test`           | Unknown | `/api/v3/st/stream-test`                        | Starts/runs a stream test. Unused.                                                    |
| `GCS_Upload_Hook`       | Unknown | `/api/v3/aus/upload-hook`                       | GCS upload hook. Unused.                                                              |

The directly observed location request is:

```ts
axios.post(API.Get_Location_Details, { ip: ip.trim() });
```

The directly observed upload-mail request is:

```ts
{
  apiKey,
  mail,
  prop: 'uploadStatus',
  val: 'success' | 'failure',
  infos: [
    { prop: 'username', val: username },
    { prop: 'appname', val: appName }
  ]
}
```

## R2 multipart-upload endpoints

Base: `AGW_BASE_URL`

| Constant             | Method | Path                            | Frontend role                                                   |
| -------------------- | ------ | ------------------------------- | --------------------------------------------------------------- |
| `Storage_Provider`   | GET    | `/api/storage-provider`         | Detects `r2` versus the default `coreweave`. Used.              |
| `R2_Initiate_Upload` | POST   | `/api/v3/us/r2-initiate-upload` | Initializes a multipart upload. Used.                           |
| `R2_Batch_Part_Urls` | POST   | `/api/v3/us/r2-batch-part-urls` | Gets signed URLs for a batch of part numbers. Used.             |
| `R2_Complete_Upload` | POST   | `/api/v3/us/r2-complete-upload` | Completes the multipart upload using uploaded part ETags. Used. |

These operations are implemented in
[`r2-upload-api.ts`](../src/services/r2/r2-upload-api.ts).

### Initiate request

```ts
{
  childFolder: string,
  fileSize: number,
  domain: string
}
```

Verified response data:

```ts
interface R2InitiateData {
  uploadId: string;
  key: string;
  chunkSize: number;
  totalParts: number;
  region?: string;
  endpoint?: string;
  willOverwrite?: boolean;
}
```

### Batch signed-URL request

```ts
{
  key: string,
  uploadId: string,
  partNumbers: number[]
}
```

Verified response data:

```ts
{
  urls: Array<{
    partNumber: number;
    url: string;
  }>;
}
```

### Complete request

```ts
{
  key: string,
  uploadId: string,
  parts: Array<{
    PartNumber: number;
    ETag: string;
  }>
}
```

Verified response data:

```ts
{
  location?: string;
  key?: string;
  bucket?: string;
}
```

All three AGW responses are expected to have this envelope:

```ts
{
  data: {
    status: 'success',
    data: unknown
  }
}
```

After signed part URLs are returned, the browser sends each file chunk directly
to the signed URL with `PUT application/octet-stream`. That object-storage PUT
does not go through `API` or AGW. The uploader:

- uploads at most six parts concurrently;
- retries each part up to three times;
- waits 1 second before the second attempt and 2 seconds before the third;
- requires an `ETag` response header for every successful part;
- supports cancellation through an Axios cancel token.

See [`optimized-r2-upload.ts`](../src/services/r2/optimized-r2-upload.ts) and
[`r2-upload.ts`](../src/types/r2-upload.ts).

## AUTH service endpoints

Base: `AUTH_BASE_URL`

| Constant                 | Method         | Path                              | Credentials and frontend role                                          |
| ------------------------ | -------------- | --------------------------------- | ---------------------------------------------------------------------- |
| `Generate_Cookie`        | POST           | `/api/v2/session/token`           | Exchanges a bearer token for a credentialed session cookie.            |
| `New_Api_Key`            | GET            | `/api/v2/user/api-key`            | Fetches API and streaming API keys with credentials.                   |
| `Sign_Out`               | POST           | `/api/v2/auth/sign-out`           | Signs out with credentials.                                            |
| `Delete_Account`         | DELETE         | `/api/v2/user/delete-account`     | Deletes the current credentialed account.                              |
| `Check_Login_Status`     | GET            | `/api/v2/session/state`           | Reads credentialed session state.                                      |
| `Team_Info`              | GET            | `/api/v2/team/infos`              | Fetches credentialed team information.                                 |
| `Change_Username`        | PUT            | `/api/v2/user/username`           | Changes username with credentials.                                     |
| `Remove_Username`        | DELETE         | `/api/v2/user/unsubscribe`        | Removes/unsubscribes the current username with credentials.            |
| `Change_Email`           | PUT            | `/api/v2/user/ownership-transfer` | Transfers account ownership/email with credentials.                    |
| `Update_Version_Control` | PUT            | `/api/v2/user/info`               | Updates version-control user information with credentials.             |
| `Team_Invite_Remove`     | POST, DELETE   | `/api/v2/team/member`             | POST invites a member; DELETE removes a member. Both use credentials.  |
| `Get_Invited_Team`       | GET            | `/api/v2/team/members`            | Lists invited/team members with credentials.                           |
| `Generate_Api_Key`       | PUT            | `/api/v2/user/info`               | Updates/regenerates API-key-related user information with credentials. |
| `Generate_Token`         | POST           | `/api/v2/ext/token-creation`      | Generates an external token with credentials.                          |
| `Binary_EL_Download`     | GET/navigation | `/api/v2/ext/download-binary-el`  | Opened in a new browser window to download the EL binary.              |
| `Upgrade_Profile`        | POST           | `/api/v2/user/upgrade-profile`    | Updates phone/profile information with credentials.                    |

`New_Api_Key` has the only strongly typed RTK Query response transformation in
this group. It converts:

```ts
{
  apiKey: { apiKey: string },
  streamingApiKey: { apiKey: string }
}
```

into:

```ts
{
  apiKey: string,
  streamingApiKey: string
}
```

See [`api-key.ts`](../src/store/api/api-key.ts) and
[`auth.ts`](../src/store/api/auth.ts).

## Frontend API modules

| Module                                                  | Responsibility                                                                                         |
| ------------------------------------------------------- | ------------------------------------------------------------------------------------------------------ |
| [`auth.ts`](../src/store/api/auth.ts)                   | Sign-in/up, OTP, password recovery, credentialed sessions, team/account/profile operations.            |
| [`api-key.ts`](../src/store/api/api-key.ts)             | API keys, external token generation, and version-control profile updates.                              |
| [`user-info.ts`](../src/store/api/user-info.ts)         | User/customer information, logos, upload completion, and control-panel notifications.                  |
| [`subscription.ts`](../src/store/api/subscription.ts)   | Legacy plan status, creation, cancellation, quotes, upgrades/downgrades, watchers, and auto-renew.     |
| [`payment.ts`](../src/store/api/payment.ts)             | Core plans, invoices, core statuses, checkout/non-checkout creation, and payment changes.              |
| [`credit-card.ts`](../src/store/api/credit-card.ts)     | Payment-method retrieval, setup, and default-card selection.                                           |
| [`storage.ts`](../src/store/api/storage.ts)             | Storage status, usage, quote, creation, upgrade, and downgrade.                                        |
| [`minute.ts`](../src/store/api/minute.ts)               | Streamed-minute usage.                                                                                 |
| [`analytics.ts`](../src/store/api/analytics.ts)         | Stream records.                                                                                        |
| [`budget-api.ts`](../src/store/api/budget-api.ts)       | Budget read/enable/increase/decrease/remove operations.                                                |
| [`asset.ts`](../src/store/api/asset.ts)                 | Streaming apps, thumbnails, dedicated-server apps, 2D assets, and video assets.                        |
| [`appLink.ts`](../src/store/api/appLink.ts)             | Generated app URLs, app/config CRUD, and provider-aware deletion.                                      |
| [`upload.ts`](../src/store/api/upload.ts)               | Signed URLs, additional uploads, dedicated servers, location lookup, player kick, and DS app deletion. |
| [`streaming-app.ts`](../src/store/api/streaming-app.ts) | Executable metadata upload and forced executable-data deletion.                                        |

`additional-uploads.ts` and `ds-app.ts` currently exist but are empty; their
active operations are in `upload.ts` and `asset.ts`.

## Cache invalidation behavior

RTK Query tags connect some mutations to automatic refetching:

| Tag/group         | Important providers and invalidators                                                                                         |
| ----------------- | ---------------------------------------------------------------------------------------------------------------------------- |
| `loginStatus`     | Session-state query; username change invalidates it.                                                                         |
| `teamMembers`     | Invited-member query; invite and removal invalidate it.                                                                      |
| `streamingApp`    | App list/config queries; upload and deletion operations invalidate list/thumbnail variants.                                  |
| `logoList`        | Logo list; logo signed URL/upload-complete operations invalidate it.                                                         |
| `notification`    | Notification list; remove, clear, and read operations invalidate it.                                                         |
| Subscription tags | Status queries provide plan tags; cancel, reactivate, upgrade, downgrade, and unpause operations invalidate relevant groups. |
| `storage`         | Storage status provides it; storage mutations and unpause invalidate it.                                                     |
| `dsList`          | Dedicated-server list provides it; start/stop invalidate it.                                                                 |
| `dsUpload`        | Dedicated-server app list provides it; upload/delete invalidate it.                                                          |

Refer to [`tag-types.ts`](../src/store/tag-types.ts) for the complete tag list.

## Currently unreferenced active constants

The following 14 active `API` properties have no active `API.Name` reference
under `src`:

1. `Create_PPM_Invoice`
2. `Streaming_App_Signed_Url`
3. `Streaming_App_Alt_Signed_Url`
4. `Budget_Api`
5. `Set_Default_Payment_Method`
6. `Check_Promo`
7. `Shared_Drive_Download`
8. `App_Test_Queue_Info`
9. `Delete_Temp_App`
10. `Stream_Test_Health`
11. `Redeem_Promo`
12. `Old_Upload_Signed_Url`
13. `Stream_Test`
14. `GCS_Upload_Hook`

Do not delete them based only on frontend usage. Confirm that no external
consumer, planned feature, or dynamic access depends on them. If they are
frontend-only legacy constants, removing them will reduce ambiguity.

## Usage examples

### Use an existing RTK Query hook

Prefer the exported hook when one exists:

```tsx
const [signIn, signInState] = useSignInMutation();

await signIn({
  // Backend-defined sign-in payload
}).unwrap();
```

This keeps caching, invalidation, and shared request behavior in one place.

### Add a new RTK Query operation

```ts
const featureApi = baseApi.injectEndpoints({
  endpoints: build => ({
    featureAction: build.mutation<ResponseType, RequestType>({
      query: data => ({
        url: API.Feature_Action,
        method: 'POST',
        body: data
      })
    })
  })
});
```

Unlike many existing operations, new operations should declare request and
response types.

### Use direct Axios only for specialized flows

Direct Axios is appropriate when RTK Query is not a good fit, such as multipart
upload orchestration:

```ts
const response = await axios.post(API.R2_Initiate_Upload, payload, {
  headers: {
    'Content-Type': 'application/json',
    apiKey
  }
});
```

Avoid creating a second implementation of an operation that already has an RTK
Query hook.

## Adding or changing an endpoint safely

1. Add or change the host/path in `api.ts`.
2. Decide which base owns it: `AGW_BASE_URL`, `AUTH_BASE_URL`, or an explicitly
   external absolute URL.
3. Confirm the path in both production and staging. Avoid hardcoding one
   environment unless the service is intentionally shared.
4. Add a typed RTK Query endpoint or a typed service method.
5. Declare the exact HTTP method, request type, response type, credential
   behavior, and any non-JSON content type.
6. Add appropriate `providesTags`/`invalidatesTags` if cached data is involved.
7. Exercise both builds:

   ```bash
   pnpm build:staging
   pnpm build:prod
   ```

8. In the browser Network panel, verify the hostname, method, request body,
   credentials, response envelope, and failure behavior.
9. Update this document and, ideally, the backend OpenAPI specification.

## Security and operational notes

- Every URL in `API` can be shipped to the browser. Endpoint addresses are not
  secrets and must not be treated as authorization.
- Never place private credentials in `NEXT_PUBLIC_*` values.
- Many AGW operations appear to receive an API key in request data. Treat API
  keys as credentials even when legacy frontend architecture exposes them.
- AUTH calls with `withCredentials: true` depend on correct backend CORS,
  cookie-domain, `SameSite`, and `Secure` configuration for both environment
  host families.
- `Telegram_Notify` and its chat IDs are reachable from browser code. The
  notification backend must authenticate, authorize, validate, and rate-limit
  requests rather than trusting the client.
- `images.remotePatterns` currently allows any HTTPS hostname. That setting is
  unrelated to `API`, but it should not be confused with API CORS policy.
- Environment configuration is selected at build time. A build created with
  staging URLs remains a staging-host build even if its Vercel deployment is
  later assigned to the production domain.

## Known maintainability issues

These are observations from the current source, not backend defects:

1. Most RTK Query requests and responses are untyped.
2. The Axios error interceptor can turn rejected HTTP responses into fulfilled
   values.
3. `STAGING_PAYMENT_URL` is active configuration but unused by active routes.
4. Several names contain legacy spelling or scope mismatches, including
   `Core_Pan_*`, `Check_PPCCU_Quote` versus route `ppcu`, and
   `Reactivate_PPCCU_Sub` calling an all-subscriptions route.
5. Comments such as `//used` are incomplete, and `Delete_Config` is marked
   `//unsed` even though it is used.
6. Fourteen active constants are currently unreferenced.
7. External notification routing is fixed to the production-named notification
   host even for staging builds.
8. Request/response contracts are spread across components and hooks instead
   of being declared beside each endpoint.

The most valuable next refactor would be to replace the untyped endpoint
registry-plus-`any` pattern with domain-specific typed API modules while keeping
the environment-specific base configuration centralized.
