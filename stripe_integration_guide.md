# Stripe Integration: From A to Z

This guide walks you through the entire Stripe integration lifecycle in your codebase, moving from basic API definitions up to advanced state management.

---

## 1. The Foundation (API Configuration & RTK Query)

At the very bottom layer, the frontend does not talk directly to Stripe using `@stripe/stripe-js`. Instead, it talks to your backend Gateway (`AGW Gateway`). 

### The API Endpoints
In `src/config/api.ts`, all payment endpoints are defined. For example:
```typescript
const AGW_BASE_URL = 'https://api.eagle3dstreaming.com';
export const paymentEndpoints = {
  createCoreSubscription: AGW_BASE_URL + '/api/v3/pay/create-core-subscription-checkout-session',
  nonCheckoutMinSub: AGW_BASE_URL + '/api/v3/pay/create-min-subscription-non-checkout',
  checkStatus: AGW_BASE_URL + '/api/v3/pay/check-core-subscription-status'
}
```

### The RTK Query Slice
These endpoints are wrapped inside **Redux Toolkit Query (RTK Query)** in `src/store/api/payment.ts`. RTK Query handles the fetching, caching, and loading states automatically. 
```typescript
// Inside src/store/api/payment.ts
export const paymentApi = createApi({
  reducerPath: 'paymentApi',
  tagTypes: ['SubscriptionStatus'], // Used to auto-refresh data when payment succeeds
  endpoints: (builder) => ({
    createSubscriptionPlan: builder.mutation({
      query: (data) => ({
        url: '/api/v3/pay/create-core-subscription-checkout-session',
        method: 'POST',
        body: data,
      }),
    }),
  }),
});
export const { useCreateSubscriptionPlanMutation } = paymentApi;
```

---

## 2. The Standard Checkout Flow (Buying a Plan)

Let's look at what happens when a user clicks "Subscribe" on the **Core Monthly Plan**.

### Step A: The Button Click
In `src/components/features/new-subscription/core-plan-montly.tsx`, the component imports the RTK hook:
```tsx
import { useCreateSubscriptionPlanMutation } from '@/store/api/payment';

const CorePlanMonthly = () => {
  const [createSubscriptionPlan, { isLoading }] = useCreateSubscriptionPlanMutation();

  const handleSubscribe = async () => {
    // 1. Send request to backend
    const response = await createSubscriptionPlan({
      apiKey,
      planName: "core_monthly",
      // ...other details
    }).unwrap();

    // 2. The backend created a Stripe Checkout Session and returned the URL
    if (response.data.sessionUrl) {
       // 3. Redirect the user entirely to Stripe's secure page
       window.location.href = response.data.sessionUrl; 
    }
  }

  return <button onClick={handleSubscribe} disabled={isLoading}>Subscribe</button>
}
```

### Step B: The Stripe Hosted Page
The user is taken to `checkout.stripe.com`. They enter their credit card and hit pay. Stripe processes the payment and redirects them back to your application (usually to a `payment-successful` route).

---

## 3. The Non-Checkout Flow (1-Click Upsells)

What if the user already has a card saved on Stripe, and they just want to buy an extra 50 GB of storage? You don't want to force them through the checkout redirect again.

This is where the **Non-Checkout** endpoints come in.

```tsx
import { useNonCheckoutCreateGbSubscriptionMutation } from '@/store/api/payment';

const buyStorage = async () => {
   const response = await buyExtraStorage({ apiKey, quantity: 50 }).unwrap();
   
   if (response.status === 'success') {
      toast.success("Storage upgraded successfully!");
      // RTK Query automatically invalidates the 'SubscriptionStatus' tag
      // This forces the UI to re-fetch the new limits without refreshing the page!
   }
}
```
In this flow, your backend securely uses the user's saved `customerId` to charge them in the background.

---

## 4. Reading Data & Custom Hooks (The Brains)

Now that the user has bought a plan, how does the UI know they have it?

Instead of components making raw API calls, the codebase uses **Custom React Hooks** to parse Stripe data into clean business logic. 

If you open `src/hooks/use-subscription-v2.ts`, it does the following:

1. **Fetches the Status:** It calls `useCheckStatusQuery()` which returns the Stripe metadata from the backend.
2. **Calculates the Billing Cycle:** It looks at `current_period_start` and `current_period_end` (which come directly from the Stripe subscription object) and calculates exactly how many days the user has left in their billing cycle.
3. **Merges Limits:** It combines the user's base limits with any add-ons they purchased.

When a UI component needs to know if the user has a plan, it just does this:
```tsx
import { useSubscriptionV2 } from '@/hooks/use-subscription-v2';

const Dashboard = () => {
   const { hasSubscription, daysLeft } = useSubscriptionV2();

   if (!hasSubscription) return <p>Please subscribe</p>;
   return <p>You have {daysLeft} days remaining on your plan!</p>;
}
```

---

## 5. Advanced Concepts: Proration & Pause Collections

To complete the Stripe integration, the codebase handles edge cases natively:

### A. Proration (Upgrades / Downgrades)
If a user is halfway through a $10/month plan and wants to upgrade to a $50/month plan, how much do they owe right now?
Instead of doing math on the frontend, the codebase uses `useDataQuoteQuery`. This sends a request to Stripe saying *"What would the invoice look like if they upgraded right now?"*. Stripe calculates the exact proration (credits vs charges) and returns a formatted quote that your UI displays before the user confirms the upgrade.

### B. Payment Failures (`pause_collection`)
If a user's credit card expires, Stripe tries to charge it and fails. Your backend listens to Stripe webhooks. When it fails, Stripe marks the subscription with `pause_collection`. 

In your frontend, the `useHasPauseSubscription` hook checks for this flag. If `pause_collection` is true, the hook forces the app to render a red banner: *"Your payment failed. Please update your card."*

The user clicks the banner, triggering `useEditPaymentForCoreMutation`, which opens a Stripe portal to update their card. Once updated, the frontend fires `useUnPauseSubscriptionMutation` to resume everything.
