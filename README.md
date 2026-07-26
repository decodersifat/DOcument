# Eagle 3D Streaming Control Panel — Complete Repository Onboarding

This documentation is the source-code-grounded onboarding guide for `cp-next`. It is written for an engineer who knows JavaScript, TypeScript, React, and Next.js but has no prior knowledge of this product.

The repository snapshot documented here contains:

- 363 tracked files.
- 29,252 lines across 312 tracked text files.
- 18 user-visible App Router routes plus Next.js's generated `/_not-found` route.
- 193 component files, 20 custom-hook files, 21 Redux/store files, and 7 configuration modules under `src`.
- No test files, no route `loading.tsx` files, no route `error.tsx` files, no `middleware.ts`, no Server Actions, and no Next.js API route handlers.
- One verified internal circular dependency:
  `components/features/dedicated-server/application.tsx` ↔
  `components/features/dedicated-server/dedicated-server-card-details.tsx`.

The facts in these documents come from reading every tracked path, tracing imports and exports, mapping RTK Query endpoints to consumers, and running a successful staging production build with Next.js 15.5.21. Where the source does not establish a backend behavior, the documentation says so explicitly.

## Reading order

1. [Product, architecture, startup, routes, and request flow](./01-product-and-runtime.md)
2. [Technology stack, state, components, forms, UI, styling, performance, and errors](./02-frontend-architecture.md)
3. [API calls, authentication, environment variables, configuration, and dependency graph](./03-data-auth-and-configuration.md)
4. [Recursive folder guide and exhaustive file-by-file catalog](./04-file-catalog.md)
5. [Architecture diagrams](./05-diagrams.md)
6. [Learning roadmap, 14-day plan, and glossary](./06-learning-plan-and-glossary.md)
7. [Verified code-review findings](./07-code-review.md)
8. [150 repository-specific interview questions and answers](./08-interview-bank.md)

## How to use this guide while developing

- Start with the route in `src/app`, but expect most route files to be thin wrappers.
- Follow the wrapper into `src/components/features/<feature>`.
- Follow custom hooks into `src/hooks`; most hooks aggregate several RTK Query calls.
- Follow generated query/mutation hooks into `src/store/api`.
- Resolve endpoint constants in `src/config/api.ts`.
- For streaming-app uploads, read the provider and its UI together:
  `upload-sequence-provider.tsx`, `app-upload-modal.tsx`, `upload-details.tsx`,
  `sequence.tsx`, `normalizeAppExeData.ts`, the R2 services, and `apiCall.ts`.
- For subscriptions, first decide whether the code is handling a legacy plan or the
  newer coded Core/GB/minute plans. The two models coexist and are deliberately
  documented separately.

## Evidence and uncertainty convention

Statements use the following meanings:

- **Observed:** directly established by source or verified build output.
- **Inferred:** strongly implied by payload shapes, endpoint names, and UI behavior.
- **Unknown:** cannot be established because the backend, Firebase rules, infrastructure,
  Stripe configuration, or product policy is outside this repository.
- **Defect/risk:** a concrete inconsistency, security problem, likely bug, or maintainability
  concern visible in this snapshot. These are not silently treated as intended behavior.

## Snapshot caveats

- `package.json` declares compatible ranges such as `next: ^15.5.3`; the installed build
  reports Next.js 15.5.21. The lockfile is the authoritative dependency-resolution record.
- The successful build was performed with `NEXT_PUBLIC_ENV=staging`. All routes were marked
  `○ (Static)`.
- The normal `npm run build:staging` command could not start in the current installation
  because the `cross-env` executable was missing from `node_modules/.bin`; invoking the
  installed Next CLI with the environment variable set directly succeeded.
- `.env` is intentionally not documented with values. Only variable names and their source
  consumers are recorded.
- Existing files in `docs/` contain mojibake in several places and the SuperAdmin guide
  contradicts the implementation about URL cleanup. The onboarding suite records actual
  behavior from source.

## Documentation maintenance rule

When a pull request changes a route, provider, API slice, environment variable, business
rule, or tracked file, update the corresponding onboarding document in the same pull
request. If the production build's route table changes, update the rendering table in
Section 5.
