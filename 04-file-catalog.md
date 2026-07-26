# Sections 3 and 22 — Recursive Folder and Every-File Catalogue

> Baseline: the 363 files returned by `git ls-files` during the audit, plus an explanation of the new onboarding folder. Generated/build/dependency directories (`.next`, `node_modules`, `.git`) are intentionally excluded. Empty source directories visible in the working tree are included in the folder guide.

## Section 3 — Recursive Folder-by-Folder Guide

| Folder | Purpose | Responsibility / key contents | Relationships | Never place here |
|---|---|---|---|---|
| `.github` | Repository automation. | Workflow definitions. (2 tracked baseline files below this folder.) | Deployment/build triggers. | Application runtime code or credentials. |
| `.github/workflows` | CI/CD entry points. | Production and semi-production workflow YAML. (2 tracked baseline files below this folder.) | Root scripts and secret-backed runner inputs. | Feature logic or literal secrets. |
| `.vscode` | Shared editor settings. | Non-runtime developer defaults. (3 tracked baseline files below this folder.) | Local IDE behavior. | Personal machine paths or secrets. |
| `docs` | Project knowledge base. | SOP, structure, Super Admin, and onboarding documentation. (3 tracked baseline files below this folder.) | All source/config areas. | Runtime implementation. |
| `docs/onboarding` | Generated evidence-based onboarding set. | Architectural, file-level, review, learning, and interview material. (0 tracked baseline files below this folder.) | Entire tracked repository baseline. | Secrets or claims not supported by code. |
| `public` | Static web root. | Directly served icons, images, manifest, robots, and fonts. (6 tracked baseline files below this folder.) | Components refer to these by URL. | TypeScript or protected data. |
| `public/fonts` | Local font binaries. | Font files served from the public origin. (1 tracked baseline files below this folder.) | CSS/font loaders. | Licenses not approved for redistribution. |
| `src` | Application source root. | Routes, components, state, services, config, types, and assets. (330 tracked baseline files below this folder.) | `@/*` TypeScript alias. | CI scripts or deployment secrets. |
| `src/app` | Next.js App Router root. | Root layout, global CSS, route groups, and 404. (31 tracked baseline files below this folder.) | Feature components/providers. | General reusable feature implementations. |
| `src/app/(with-public-layout)` | Public route group. | Sign-in, signup, reset, username, and mail route entries. (5 tracked baseline files below this folder.) | Root layout/providers and auth features. | Authenticated sidebar-only UI. |
| `src/app/(with-public-layout)/fb-mail` | Public route group. | Sign-in, signup, reset, username, and mail route entries. (1 tracked baseline files below this folder.) | Root layout/providers and auth features. | Authenticated sidebar-only UI. |
| `src/app/(with-public-layout)/repass` | Public route group. | Sign-in, signup, reset, username, and mail route entries. (1 tracked baseline files below this folder.) | Root layout/providers and auth features. | Authenticated sidebar-only UI. |
| `src/app/(with-public-layout)/signin` | Public route group. | Sign-in, signup, reset, username, and mail route entries. (1 tracked baseline files below this folder.) | Root layout/providers and auth features. | Authenticated sidebar-only UI. |
| `src/app/(with-public-layout)/signup` | Public route group. | Sign-in, signup, reset, username, and mail route entries. (1 tracked baseline files below this folder.) | Root layout/providers and auth features. | Authenticated sidebar-only UI. |
| `src/app/(with-public-layout)/username` | Public route group. | Sign-in, signup, reset, username, and mail route entries. (1 tracked baseline files below this folder.) | Root layout/providers and auth features. | Authenticated sidebar-only UI. |
| `src/app/(withSidebarLayout)` | Authenticated route group. | Client session gate, navigation shell, protected page entries. (21 tracked baseline files below this folder.) | Feature components and provider stack. | Security-critical authorization assumptions. |
| `src/app/(withSidebarLayout)/additional-uploads` | Authenticated route group. | Client session gate, navigation shell, protected page entries. (1 tracked baseline files below this folder.) | Feature components and provider stack. | Security-critical authorization assumptions. |
| `src/app/(withSidebarLayout)/analytics` | Authenticated route group. | Client session gate, navigation shell, protected page entries. (1 tracked baseline files below this folder.) | Feature components and provider stack. | Security-critical authorization assumptions. |
| `src/app/(withSidebarLayout)/credit-card-failed` | Authenticated route group. | Client session gate, navigation shell, protected page entries. (1 tracked baseline files below this folder.) | Feature components and provider stack. | Security-critical authorization assumptions. |
| `src/app/(withSidebarLayout)/credit-card-successful` | Authenticated route group. | Client session gate, navigation shell, protected page entries. (1 tracked baseline files below this folder.) | Feature components and provider stack. | Security-critical authorization assumptions. |
| `src/app/(withSidebarLayout)/credit-card-successful-core` | Authenticated route group. | Client session gate, navigation shell, protected page entries. (1 tracked baseline files below this folder.) | Feature components and provider stack. | Security-critical authorization assumptions. |
| `src/app/(withSidebarLayout)/developer-section` | Authenticated route group. | Client session gate, navigation shell, protected page entries. (1 tracked baseline files below this folder.) | Feature components and provider stack. | Security-critical authorization assumptions. |
| `src/app/(withSidebarLayout)/multiplayer-server` | Authenticated route group. | Client session gate, navigation shell, protected page entries. (1 tracked baseline files below this folder.) | Feature components and provider stack. | Security-critical authorization assumptions. |
| `src/app/(withSidebarLayout)/payment-failed` | Authenticated route group. | Client session gate, navigation shell, protected page entries. (1 tracked baseline files below this folder.) | Feature components and provider stack. | Security-critical authorization assumptions. |
| `src/app/(withSidebarLayout)/payment-successful` | Authenticated route group. | Client session gate, navigation shell, protected page entries. (1 tracked baseline files below this folder.) | Feature components and provider stack. | Security-critical authorization assumptions. |
| `src/app/(withSidebarLayout)/plans` | Authenticated route group. | Client session gate, navigation shell, protected page entries. (1 tracked baseline files below this folder.) | Feature components and provider stack. | Security-critical authorization assumptions. |
| `src/app/(withSidebarLayout)/team` | Authenticated route group. | Client session gate, navigation shell, protected page entries. (1 tracked baseline files below this folder.) | Feature components and provider stack. | Security-critical authorization assumptions. |
| `src/app/(withSidebarLayout)/upload` | Authenticated route group. | Client session gate, navigation shell, protected page entries. (7 tracked baseline files below this folder.) | Feature components and provider stack. | Security-critical authorization assumptions. |
| `src/app/(withSidebarLayout)/utilities` | Authenticated route group. | Client session gate, navigation shell, protected page entries. (1 tracked baseline files below this folder.) | Feature components and provider stack. | Security-critical authorization assumptions. |
| `src/assets` | Bundled source assets. | Images imported through modules. (38 tracked baseline files below this folder.) | Feature components. | Public-by-URL files or logic. |
| `src/assets/images` | Bundled source assets. | Images imported through modules. (37 tracked baseline files below this folder.) | Feature components. | Public-by-URL files or logic. |
| `src/components` | React component root. | Feature, shared, provider, navigation, and UI composition. (193 tracked baseline files below this folder.) | Hooks/store/config. | Transport-only services. |
| `src/components/admin` | Specialized component group. | Components indicated by the folder name. (1 tracked baseline files below this folder.) | Root providers/layout or feature consumers. | Unrelated domain logic. |
| `src/components/auth` | Specialized component group. | Components indicated by the folder name. (1 tracked baseline files below this folder.) | Root providers/layout or feature consumers. | Unrelated domain logic. |
| `src/components/consent` | Specialized component group. | Components indicated by the folder name. (1 tracked baseline files below this folder.) | Root providers/layout or feature consumers. | Unrelated domain logic. |
| `src/components/error` | Specialized component group. | Components indicated by the folder name. (2 tracked baseline files below this folder.) | Root providers/layout or feature consumers. | Unrelated domain logic. |
| `src/components/features` | Product feature components. | Domain-oriented screen and workflow modules. (111 tracked baseline files below this folder.) | Shared/UI/hooks/store. | Generic primitives that have multiple consumers. |
| `src/components/features/additional-upload` | Additional Upload feature boundary. | Screens, dialogs, tables, and workflow presentation for this domain. (2 tracked baseline files below this folder.) | Shared/UI components, hooks, API hooks. | Unrelated feature business logic. |
| `src/components/features/analytics` | Analytics feature boundary. | Screens, dialogs, tables, and workflow presentation for this domain. (7 tracked baseline files below this folder.) | Shared/UI components, hooks, API hooks. | Unrelated feature business logic. |
| `src/components/features/analytics/charts` | Analytics feature boundary. | Screens, dialogs, tables, and workflow presentation for this domain. (2 tracked baseline files below this folder.) | Shared/UI components, hooks, API hooks. | Unrelated feature business logic. |
| `src/components/features/dedicated-server` | Dedicated Server feature boundary. | Screens, dialogs, tables, and workflow presentation for this domain. (6 tracked baseline files below this folder.) | Shared/UI components, hooks, API hooks. | Unrelated feature business logic. |
| `src/components/features/developer-section` | Developer Section feature boundary. | Screens, dialogs, tables, and workflow presentation for this domain. (6 tracked baseline files below this folder.) | Shared/UI components, hooks, API hooks. | Unrelated feature business logic. |
| `src/components/features/my-account` | My Account feature boundary. | Screens, dialogs, tables, and workflow presentation for this domain. (4 tracked baseline files below this folder.) | Shared/UI components, hooks, API hooks. | Unrelated feature business logic. |
| `src/components/features/new-subscription` | New Subscription feature boundary. | Screens, dialogs, tables, and workflow presentation for this domain. (8 tracked baseline files below this folder.) | Shared/UI components, hooks, API hooks. | Unrelated feature business logic. |
| `src/components/features/plan` | Plan feature boundary. | Screens, dialogs, tables, and workflow presentation for this domain. (14 tracked baseline files below this folder.) | Shared/UI components, hooks, API hooks. | Unrelated feature business logic. |
| `src/components/features/plan/dialog` | Plan feature boundary. | Screens, dialogs, tables, and workflow presentation for this domain. (5 tracked baseline files below this folder.) | Shared/UI components, hooks, API hooks. | Unrelated feature business logic. |
| `src/components/features/plan/features` | Plan feature boundary. | Screens, dialogs, tables, and workflow presentation for this domain. (2 tracked baseline files below this folder.) | Shared/UI components, hooks, API hooks. | Unrelated feature business logic. |
| `src/components/features/post-payment` | Post Payment feature boundary. | Screens, dialogs, tables, and workflow presentation for this domain. (1 tracked baseline files below this folder.) | Shared/UI components, hooks, API hooks. | Unrelated feature business logic. |
| `src/components/features/signin` | Signin feature boundary. | Screens, dialogs, tables, and workflow presentation for this domain. (1 tracked baseline files below this folder.) | Shared/UI components, hooks, API hooks. | Unrelated feature business logic. |
| `src/components/features/singup` | Singup feature boundary. | Screens, dialogs, tables, and workflow presentation for this domain. (1 tracked baseline files below this folder.) | Shared/UI components, hooks, API hooks. | Unrelated feature business logic. |
| `src/components/features/streaming-app` | Streaming App feature boundary. | Screens, dialogs, tables, and workflow presentation for this domain. (24 tracked baseline files below this folder.) | Shared/UI components, hooks, API hooks. | Unrelated feature business logic. |
| `src/components/features/streaming-app/config` | Streaming App feature boundary. | Screens, dialogs, tables, and workflow presentation for this domain. (10 tracked baseline files below this folder.) | Shared/UI components, hooks, API hooks. | Unrelated feature business logic. |
| `src/components/features/streaming-app/demo-apps` | Streaming App feature boundary. | Screens, dialogs, tables, and workflow presentation for this domain. (2 tracked baseline files below this folder.) | Shared/UI components, hooks, API hooks. | Unrelated feature business logic. |
| `src/components/features/subscription` | Subscription feature boundary. | Screens, dialogs, tables, and workflow presentation for this domain. (28 tracked baseline files below this folder.) | Shared/UI components, hooks, API hooks. | Unrelated feature business logic. |
| `src/components/features/subscription/modal` | Subscription feature boundary. | Screens, dialogs, tables, and workflow presentation for this domain. (3 tracked baseline files below this folder.) | Shared/UI components, hooks, API hooks. | Unrelated feature business logic. |
| `src/components/features/subscription/new-subscription-summary` | Subscription feature boundary. | Screens, dialogs, tables, and workflow presentation for this domain. (4 tracked baseline files below this folder.) | Shared/UI components, hooks, API hooks. | Unrelated feature business logic. |
| `src/components/features/subscription/subscription-details` | Subscription feature boundary. | Screens, dialogs, tables, and workflow presentation for this domain. (7 tracked baseline files below this folder.) | Shared/UI components, hooks, API hooks. | Unrelated feature business logic. |
| `src/components/features/subscription/subscription-summary` | Subscription feature boundary. | Screens, dialogs, tables, and workflow presentation for this domain. (8 tracked baseline files below this folder.) | Shared/UI components, hooks, API hooks. | Unrelated feature business logic. |
| `src/components/features/team` | Team feature boundary. | Screens, dialogs, tables, and workflow presentation for this domain. (3 tracked baseline files below this folder.) | Shared/UI components, hooks, API hooks. | Unrelated feature business logic. |
| `src/components/features/username` | Username feature boundary. | Screens, dialogs, tables, and workflow presentation for this domain. (1 tracked baseline files below this folder.) | Shared/UI components, hooks, API hooks. | Unrelated feature business logic. |
| `src/components/features/utilities` | Utilities feature boundary. | Screens, dialogs, tables, and workflow presentation for this domain. (5 tracked baseline files below this folder.) | Shared/UI components, hooks, API hooks. | Unrelated feature business logic. |
| `src/components/navbar` | Header/navigation UI. | Account, notification, subscription, and theme controls. (5 tracked baseline files below this folder.) | Session/team/subscription hooks. | Page-specific bodies. |
| `src/components/providers` | Client context providers. | Cross-tree workflow state and effects. (7 tracked baseline files below this folder.) | Protected layout and feature hooks/services. | Unrelated leaf presentation. |
| `src/components/providers/upload` | Client context providers. | Cross-tree workflow state and effects. (4 tracked baseline files below this folder.) | Protected layout and feature hooks/services. | Unrelated leaf presentation. |
| `src/components/shared` | Cross-feature presentation. | Reusable cards, dialogs, loaders, logos, and helpers. (21 tracked baseline files below this folder.) | UI primitives and feature consumers. | Single-feature policy. |
| `src/components/shared/dialog` | Cross-feature presentation. | Reusable cards, dialogs, loaders, logos, and helpers. (4 tracked baseline files below this folder.) | UI primitives and feature consumers. | Single-feature policy. |
| `src/components/shared/logo` | Cross-feature presentation. | Reusable cards, dialogs, loaders, logos, and helpers. (7 tracked baseline files below this folder.) | UI primitives and feature consumers. | Single-feature policy. |
| `src/components/sidebar` | Sidebar shell. | Primary navigation and user/team affordances. (5 tracked baseline files below this folder.) | Protected layout. | Feature implementations. |
| `src/components/ui` | Design-system source components. | shadcn/Radix primitives and small semantic wrappers. (39 tracked baseline files below this folder.) | Global tokens and shared components. | Feature API calls or business policy. |
| `src/components/ui/field` | Design-system source components. | shadcn/Radix primitives and small semantic wrappers. (3 tracked baseline files below this folder.) | Global tokens and shared components. | Feature API calls or business policy. |
| `src/components/ui/headlines` | Design-system source components. | shadcn/Radix primitives and small semantic wrappers. (1 tracked baseline files below this folder.) | Global tokens and shared components. | Feature API calls or business policy. |
| `src/components/ui/search` | Design-system source components. | shadcn/Radix primitives and small semantic wrappers. (1 tracked baseline files below this folder.) | Global tokens and shared components. | Feature API calls or business policy. |
| `src/config` | Runtime configuration. | Environment selection, endpoints, Firebase, links. (7 tracked baseline files below this folder.) | Services/store/layout. | Mutable UI state or secrets. |
| `src/constant` | Static domain constants. | Plans, defaults, copy, limits. (2 tracked baseline files below this folder.) | Features/hooks. | Remote data or side effects. |
| `src/helpers` | Cross-feature helpers. | Upload, telemetry, and utility orchestration. (2 tracked baseline files below this folder.) | Providers/services/config. | Route entry files. |
| `src/helpers/axios` | HTTP transport foundation. | Shared Axios instance and RTK adapter. (2 tracked baseline files below this folder.) | All RTK endpoints. | Feature-specific response policy. |
| `src/hooks` | Custom hook layer. | Reusable derived state and side-effect orchestration. (20 tracked baseline files below this folder.) | RTK Query, contexts, config, and components. | JSX-heavy page layout. |
| `src/lib` | Low-level library helpers. | Class composition and similar primitives. (1 tracked baseline files below this folder.) | All component layers. | Feature policy. |
| `src/services` | External integration services. | Storage provider and R2 orchestration. (3 tracked baseline files below this folder.) | Config/Axios and hooks/providers. | UI markup. |
| `src/services/r2` | R2 multipart service. | Initiate, upload parts, retry, complete. (2 tracked baseline files below this folder.) | Upload provider and API config. | React rendering. |
| `src/store` | Redux/RTK Query root. | Store, tags, local slices, endpoint modules. (21 tracked baseline files below this folder.) | Providers and components. | Direct DOM code. |
| `src/store/api` | REST endpoint layer. | RTK Query endpoint injections. (17 tracked baseline files below this folder.) | API config and Axios base query. | Component rendering or unreviewed direct fetches. |
| `src/store/slices` | Local Redux state. | Non-server client slices. (1 tracked baseline files below this folder.) | Store and typed hooks. | RTK server cache duplication. |
| `src/styles` | Style/font helpers. | Non-global style setup. (1 tracked baseline files below this folder.) | Layouts/components. | Business logic. |
| `src/types` | Shared TypeScript contracts. | API/domain/component types. (2 tracked baseline files below this folder.) | Store/hooks/components. | Runtime side effects. |
| `src/utils` | Reusable utilities. | Normalization, validation, formatting, downloads. (9 tracked baseline files below this folder.) | Hooks/features/services. | React page composition. |

### Section 3 closeout

**Key takeaways**

- Routes compose features; features consume shared/UI components and hooks; hooks/store/services reach external systems.
- Route groups affect URL organization and layout/provider inheritance, not URL segments.
- Generated and dependency folders are not source architecture.
- Empty folders and placeholders should have an explicit owner or be removed.

**Common pitfalls**

- Adding business logic to `components/ui`.
- Adding a page without understanding its route-group providers.
- Treating public assets as private.
- Creating another API layer inside a feature.

**Questions to ask a mentor**

- Which empty folders are intentional?
- What is the preferred feature-boundary convention?
- Which shared components are stable design-system APIs?

**Related files to read next**

- `src/app/layout.tsx`
- `src/app/(withSidebarLayout)/layout.tsx`
- `src/components/providers.tsx`
- `tsconfig.json`

**Practical exercises**

- Place a hypothetical billing table in the correct folder and explain why.
- Trace one route from `app` to API config.

---

## Section 22 — Every File Explained

### How to read this catalogue

- **Runtime** states where the module executes or is consumed.
- **Exports / surface** lists statically detected exports and top-level declarations.
- **Hooks / local state** is a static signal, not a complete dynamic trace.
- **Dependencies** separates internal project imports from external packages.
- **Consumers** lists direct static importers; framework/config roots may legitimately have none.
- “Client-bound” means the file is pulled below a client boundary even if it lacks a valid `use client` directive.

### Repository root

#### `.editorconfig`

- **Why it exists / responsibility:**  repository/tooling configuration.
- **Runtime and size:** Build/tooling or documentation; 314 bytes.
- **Exports / hooks / state:** Not applicable to this non-source file.
- **Dependencies / consumers:** Referenced by tooling, URL strings, CSS, metadata, CI, or documentation; static TypeScript imports do not provide a complete signal.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `.env.example`

- **Why it exists / responsibility:** .Env repository/tooling configuration.
- **Runtime and size:** Build/tooling or documentation; 1482 bytes.
- **Exports / hooks / state:** Not applicable to this non-source file.
- **Dependencies / consumers:** Referenced by tooling, URL strings, CSS, metadata, CI, or documentation; static TypeScript imports do not provide a complete signal.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `.env.staging`

- **Why it exists / responsibility:** .Env repository/tooling configuration.
- **Runtime and size:** Build/tooling or documentation; 62 bytes.
- **Exports / hooks / state:** Not applicable to this non-source file.
- **Dependencies / consumers:** Referenced by tooling, URL strings, CSS, metadata, CI, or documentation; static TypeScript imports do not provide a complete signal.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

### .github/workflows

#### `.github/workflows/redeploy-prod.yml`

- **Why it exists / responsibility:** Redeploy Prod GitHub Actions build/deployment workflow.
- **Runtime and size:** CI/deploy time; 511 bytes.
- **Exports / hooks / state:** Not applicable to this non-source file.
- **Dependencies / consumers:** Referenced by tooling, URL strings, CSS, metadata, CI, or documentation; static TypeScript imports do not provide a complete signal.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `.github/workflows/semi-prod.yml`

- **Why it exists / responsibility:** Semi Prod GitHub Actions build/deployment workflow.
- **Runtime and size:** CI/deploy time; 2637 bytes.
- **Exports / hooks / state:** Not applicable to this non-source file.
- **Dependencies / consumers:** Referenced by tooling, URL strings, CSS, metadata, CI, or documentation; static TypeScript imports do not provide a complete signal.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

### Repository root

#### `.gitignore`

- **Why it exists / responsibility:**  repository/tooling configuration.
- **Runtime and size:** Build/tooling or documentation; 597 bytes.
- **Exports / hooks / state:** Not applicable to this non-source file.
- **Dependencies / consumers:** Referenced by tooling, URL strings, CSS, metadata, CI, or documentation; static TypeScript imports do not provide a complete signal.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `.npmrc`

- **Why it exists / responsibility:**  repository/tooling configuration.
- **Runtime and size:** Build/tooling or documentation; 48 bytes.
- **Exports / hooks / state:** Not applicable to this non-source file.
- **Dependencies / consumers:** Referenced by tooling, URL strings, CSS, metadata, CI, or documentation; static TypeScript imports do not provide a complete signal.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `.prettierrc.json`

- **Why it exists / responsibility:** .Prettierrc repository/tooling configuration.
- **Runtime and size:** Build/tooling or documentation; 487 bytes.
- **Exports / hooks / state:** Not applicable to this non-source file.
- **Dependencies / consumers:** Referenced by tooling, URL strings, CSS, metadata, CI, or documentation; static TypeScript imports do not provide a complete signal.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

### .vscode

#### `.vscode/custom.code-snippets`

- **Why it exists / responsibility:** Custom shared editor preference.
- **Runtime and size:** Build/tooling or documentation; 1775 bytes.
- **Exports / hooks / state:** Not applicable to this non-source file.
- **Dependencies / consumers:** Referenced by tooling, URL strings, CSS, metadata, CI, or documentation; static TypeScript imports do not provide a complete signal.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `.vscode/extensions.json`

- **Why it exists / responsibility:** Extensions shared editor preference.
- **Runtime and size:** Build/tooling or documentation; 794 bytes.
- **Exports / hooks / state:** Not applicable to this non-source file.
- **Dependencies / consumers:** Referenced by tooling, URL strings, CSS, metadata, CI, or documentation; static TypeScript imports do not provide a complete signal.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `.vscode/settings.json`

- **Why it exists / responsibility:** Settings shared editor preference.
- **Runtime and size:** Build/tooling or documentation; 1438 bytes.
- **Exports / hooks / state:** Not applicable to this non-source file.
- **Dependencies / consumers:** Referenced by tooling, URL strings, CSS, metadata, CI, or documentation; static TypeScript imports do not provide a complete signal.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

### Repository root

#### `components.json`

- **Why it exists / responsibility:** Components repository/tooling configuration.
- **Runtime and size:** Build/tooling or documentation; 450 bytes.
- **Exports / hooks / state:** Not applicable to this non-source file.
- **Dependencies / consumers:** Referenced by tooling, URL strings, CSS, metadata, CI, or documentation; static TypeScript imports do not provide a complete signal.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `config.prod.json`

- **Why it exists / responsibility:** Config.Prod repository/tooling configuration.
- **Runtime and size:** Build/tooling or documentation; 541 bytes.
- **Exports / hooks / state:** Not applicable to this non-source file.
- **Dependencies / consumers:** Referenced by tooling, URL strings, CSS, metadata, CI, or documentation; static TypeScript imports do not provide a complete signal.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `config.staging.json`

- **Why it exists / responsibility:** Config.Staging repository/tooling configuration.
- **Runtime and size:** Build/tooling or documentation; 567 bytes.
- **Exports / hooks / state:** Not applicable to this non-source file.
- **Dependencies / consumers:** Referenced by tooling, URL strings, CSS, metadata, CI, or documentation; static TypeScript imports do not provide a complete signal.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

### docs

#### `docs/folder-structure.md`

- **Why it exists / responsibility:** Folder Structure existing project documentation.
- **Runtime and size:** Build/tooling or documentation; 7666 bytes.
- **Exports / hooks / state:** Not applicable to this non-source file.
- **Dependencies / consumers:** Referenced by tooling, URL strings, CSS, metadata, CI, or documentation; static TypeScript imports do not provide a complete signal.
- **Improvement / caution:** Keep synchronized with implementation changes.

#### `docs/sop.md`

- **Why it exists / responsibility:** Sop existing project documentation.
- **Runtime and size:** Build/tooling or documentation; 6569 bytes.
- **Exports / hooks / state:** Not applicable to this non-source file.
- **Dependencies / consumers:** Referenced by tooling, URL strings, CSS, metadata, CI, or documentation; static TypeScript imports do not provide a complete signal.
- **Improvement / caution:** Keep synchronized with implementation changes.

#### `docs/SUPER_ADMIN_LOGIN.md`

- **Why it exists / responsibility:** SUPER ADMIN LOGIN existing project documentation.
- **Runtime and size:** Build/tooling or documentation; 7977 bytes.
- **Exports / hooks / state:** Not applicable to this non-source file.
- **Dependencies / consumers:** Referenced by tooling, URL strings, CSS, metadata, CI, or documentation; static TypeScript imports do not provide a complete signal.
- **Improvement / caution:** Keep synchronized with implementation changes.

### Repository root

#### `ecosystem.config.json`

- **Why it exists / responsibility:** Ecosystem.Config repository/tooling configuration.
- **Runtime and size:** Build/tooling or documentation; 494 bytes.
- **Exports / hooks / state:** Not applicable to this non-source file.
- **Dependencies / consumers:** Referenced by tooling, URL strings, CSS, metadata, CI, or documentation; static TypeScript imports do not provide a complete signal.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `eslint.config.mjs`

- **Why it exists / responsibility:** Eslint.Config repository/tooling configuration.
- **Runtime and size:** Build/tooling or documentation; 3435 bytes.
- **Exports / hooks / state:** Not applicable to this non-source file.
- **Dependencies / consumers:** Referenced by tooling, URL strings, CSS, metadata, CI, or documentation; static TypeScript imports do not provide a complete signal.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `LICENSE`

- **Why it exists / responsibility:** LICENSE repository support file.
- **Runtime and size:** Build/tooling or documentation; 1096 bytes.
- **Exports / hooks / state:** Not applicable to this non-source file.
- **Dependencies / consumers:** Referenced by tooling, URL strings, CSS, metadata, CI, or documentation; static TypeScript imports do not provide a complete signal.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `next.config.ts`

- **Why it exists / responsibility:** Next.Config repository/tooling configuration.
- **Runtime and size:** Shared/build-time eligible module; 15 lines.
- **Exports / surface:** `default`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** None; state bindings None; contexts None.
- **Dependencies:** internal None; external `next`.
- **Rendering and rerenders:** No JSX detected; it runs when imported or when an exported function/hook is called.
- **Direct consumers:** None; static analysis found no internal importer, so verify framework entry/dynamic use.
- **Improvement / caution:** Restrict remote images and add reviewed security headers.

#### `package.json`

- **Why it exists / responsibility:** Package repository/tooling configuration.
- **Runtime and size:** Build/tooling or documentation; 3104 bytes.
- **Exports / hooks / state:** Not applicable to this non-source file.
- **Dependencies / consumers:** Referenced by tooling, URL strings, CSS, metadata, CI, or documentation; static TypeScript imports do not provide a complete signal.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `pnpm-workspace.yaml`

- **Why it exists / responsibility:** Pnpm Workspace repository support file.
- **Runtime and size:** Build/tooling or documentation; 98 bytes.
- **Exports / hooks / state:** Not applicable to this non-source file.
- **Dependencies / consumers:** Referenced by tooling, URL strings, CSS, metadata, CI, or documentation; static TypeScript imports do not provide a complete signal.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `postcss.config.mjs`

- **Why it exists / responsibility:** Postcss.Config repository/tooling configuration.
- **Runtime and size:** Build/tooling or documentation; 99 bytes.
- **Exports / hooks / state:** Not applicable to this non-source file.
- **Dependencies / consumers:** Referenced by tooling, URL strings, CSS, metadata, CI, or documentation; static TypeScript imports do not provide a complete signal.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

### public

#### `public/file.svg`

- **Why it exists / responsibility:** File browser-served static asset or metadata file.
- **Runtime and size:** Static asset; 391 bytes.
- **Exports / hooks / state:** Not applicable to this non-source file.
- **Dependencies / consumers:** Referenced by tooling, URL strings, CSS, metadata, CI, or documentation; static TypeScript imports do not provide a complete signal.
- **Improvement / caution:** Compress, name semantically, and remove only after checking string/dynamic references.

### public/fonts

#### `public/fonts/Spantaran.woff2`

- **Why it exists / responsibility:** Spantaran browser-served local font asset.
- **Runtime and size:** Static asset; 15756 bytes.
- **Exports / hooks / state:** Not applicable to this non-source file.
- **Dependencies / consumers:** Referenced by tooling, URL strings, CSS, metadata, CI, or documentation; static TypeScript imports do not provide a complete signal.
- **Improvement / caution:** Compress, name semantically, and remove only after checking string/dynamic references.

### public

#### `public/globe.svg`

- **Why it exists / responsibility:** Globe browser-served static asset or metadata file.
- **Runtime and size:** Static asset; 1035 bytes.
- **Exports / hooks / state:** Not applicable to this non-source file.
- **Dependencies / consumers:** Referenced by tooling, URL strings, CSS, metadata, CI, or documentation; static TypeScript imports do not provide a complete signal.
- **Improvement / caution:** Compress, name semantically, and remove only after checking string/dynamic references.

#### `public/next.svg`

- **Why it exists / responsibility:** Next browser-served static asset or metadata file.
- **Runtime and size:** Static asset; 1375 bytes.
- **Exports / hooks / state:** Not applicable to this non-source file.
- **Dependencies / consumers:** Referenced by tooling, URL strings, CSS, metadata, CI, or documentation; static TypeScript imports do not provide a complete signal.
- **Improvement / caution:** Compress, name semantically, and remove only after checking string/dynamic references.

#### `public/vercel.svg`

- **Why it exists / responsibility:** Vercel browser-served static asset or metadata file.
- **Runtime and size:** Static asset; 128 bytes.
- **Exports / hooks / state:** Not applicable to this non-source file.
- **Dependencies / consumers:** Referenced by tooling, URL strings, CSS, metadata, CI, or documentation; static TypeScript imports do not provide a complete signal.
- **Improvement / caution:** Compress, name semantically, and remove only after checking string/dynamic references.

#### `public/window.svg`

- **Why it exists / responsibility:** Window browser-served static asset or metadata file.
- **Runtime and size:** Static asset; 385 bytes.
- **Exports / hooks / state:** Not applicable to this non-source file.
- **Dependencies / consumers:** Referenced by tooling, URL strings, CSS, metadata, CI, or documentation; static TypeScript imports do not provide a complete signal.
- **Improvement / caution:** Compress, name semantically, and remove only after checking string/dynamic references.

### Repository root

#### `README.md`

- **Why it exists / responsibility:** README repository support file.
- **Runtime and size:** Build/tooling or documentation; 650 bytes.
- **Exports / hooks / state:** Not applicable to this non-source file.
- **Dependencies / consumers:** Referenced by tooling, URL strings, CSS, metadata, CI, or documentation; static TypeScript imports do not provide a complete signal.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

### src/app/(with-public-layout)

#### `src/app/(with-public-layout)/fb-mail/page.tsx`

- **Why it exists / responsibility:** App Router page entry for `/fb-mail`; composes the route's primary feature component.
- **Runtime and size:** Client module; 185 lines.
- **Exports / surface:** `default`, `ResetPasswordForm`, `resetPassHandler`, `ResetPasswordLoading`, `ResetPasswordPage`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** `useRouter`, `useResetPassConfirmMutation`, `useSearchParams`, `useState`; state bindings `showPass`, `setShowPass`, `passwordError`, `setPasswordError`, `pass1`, `setPass1`, `pass2`, `setPass2`; contexts None.
- **Dependencies:** internal `src/components/ui/alert.tsx`, `src/components/ui/button.tsx`, `src/components/ui/input.tsx`, `src/components/ui/label.tsx`, `src/assets/images/eagle-update-logo.svg`, `src/utils/validation.ts`, `src/store/api/auth.ts`, `src/config/page.ts`; external `react`, `next/image`, `next/link`, `next/navigation`, `lucide-react`, `sonner`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** None.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/app/(with-public-layout)/repass/page.tsx`

- **Why it exists / responsibility:** App Router page entry for `/repass`; composes the route's primary feature component.
- **Runtime and size:** Client module; 105 lines.
- **Exports / surface:** `ResetPasswordPage`, `ResetPasswordPage`, `submitHandler(e: React.FormEvent<HTMLFormElement>)`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** `useState`, `useResetPasswordMutation`; state bindings `email`, `setEmail`, `emailFormatError`, `setEmailFormatError`; contexts None.
- **Dependencies:** internal `src/components/ui/button.tsx`, `src/components/ui/input.tsx`, `src/components/ui/label.tsx`, `src/assets/images/eagle-update-logo.svg`, `src/store/api/auth.ts`, `src/utils/validation.ts`; external `react`, `next/image`, `next/link`, `lucide-react`, `sonner`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** None.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/app/(with-public-layout)/signin/page.tsx`

- **Why it exists / responsibility:** App Router page entry for `/signin`; composes the route's primary feature component.
- **Runtime and size:** Server-rendered/static route entry; 6 lines.
- **Exports / surface:** `SignIn`, `SignIn`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** None; state bindings None; contexts None.
- **Dependencies:** internal `src/components/features/signin/signin-page.tsx`; external None.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** None.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/app/(with-public-layout)/signup/page.tsx`

- **Why it exists / responsibility:** App Router page entry for `/signup`; composes the route's primary feature component.
- **Runtime and size:** Server-rendered/static route entry; 6 lines.
- **Exports / surface:** `SignUp`, `SignUp`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** None; state bindings None; contexts None.
- **Dependencies:** internal `src/components/features/singup/signup-page.tsx`; external None.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** None.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/app/(with-public-layout)/username/page.tsx`

- **Why it exists / responsibility:** App Router page entry for `/username`; composes the route's primary feature component.
- **Runtime and size:** Server-rendered/static route entry; 6 lines.
- **Exports / surface:** `Unsername`, `Unsername`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** None; state bindings None; contexts None.
- **Dependencies:** internal `src/components/features/username/username-page.tsx`; external None.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** None.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

### src/app/(withSidebarLayout)

#### `src/app/(withSidebarLayout)/additional-uploads/page.tsx`

- **Why it exists / responsibility:** App Router page entry for `/additional-uploads`; composes the route's primary feature component.
- **Runtime and size:** Server-rendered/static route entry; 6 lines.
- **Exports / surface:** `AdditionalUpload`, `AdditionalUpload`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** None; state bindings None; contexts None.
- **Dependencies:** internal `src/components/features/additional-upload/additional-upload-page.tsx`; external None.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** None.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/app/(withSidebarLayout)/analytics/page.tsx`

- **Why it exists / responsibility:** App Router page entry for `/analytics`; composes the route's primary feature component.
- **Runtime and size:** Server-rendered/static route entry; 6 lines.
- **Exports / surface:** `Analytics`, `Analytics`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** None; state bindings None; contexts None.
- **Dependencies:** internal `src/components/features/analytics/analytics-page.tsx`; external None.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** None.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/app/(withSidebarLayout)/credit-card-failed/page.js`

- **Why it exists / responsibility:** App Router page entry for `/credit-card-failed`; composes the route's primary feature component.
- **Runtime and size:** Client module; 54 lines.
- **Exports / surface:** `PaymentFailed`, `PaymentFailed`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** None; state bindings None; contexts None.
- **Dependencies:** internal `src/components/ui/button.tsx`, `src/assets/index.ts`; external `next/image`, `lucide-react`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** None.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/app/(withSidebarLayout)/credit-card-successful-core/page.tsx`

- **Why it exists / responsibility:** App Router page entry for `/credit-card-successful-core`; composes the route's primary feature component.
- **Runtime and size:** Client module; 74 lines.
- **Exports / surface:** `PaymentSuccessfulCore`, `PaymentSuccessfulCore`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** `useSearchParams`, `useUserInfo`, `useSubscriptionV2`, `usePaymentMethodConfirmationCoreMutation`, `useEffect`; state bindings None; contexts None.
- **Dependencies:** internal `src/components/ui/button.tsx`, `src/assets/index.ts`, `src/store/api/payment.ts`, `src/hooks/use-subscription-v2.ts`, `src/hooks/use-user-info.ts`; external `react`, `next/image`, `next/navigation`, `lucide-react`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** None.
- **Improvement / caution:** Keep effect dependencies complete and verify cleanup under React Strict Mode.

#### `src/app/(withSidebarLayout)/credit-card-successful/page.tsx`

- **Why it exists / responsibility:** App Router page entry for `/credit-card-successful`; composes the route's primary feature component.
- **Runtime and size:** Client module; 53 lines.
- **Exports / surface:** `PaymentSuccessful`, `PaymentSuccessful`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** None; state bindings None; contexts None.
- **Dependencies:** internal `src/components/ui/button.tsx`, `src/assets/index.ts`; external `next/image`, `lucide-react`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** None.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/app/(withSidebarLayout)/developer-section/page.tsx`

- **Why it exists / responsibility:** App Router page entry for `/developer-section`; composes the route's primary feature component.
- **Runtime and size:** Server-rendered/static route entry; 6 lines.
- **Exports / surface:** `DeveloperSection`, `DeveloperSection`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** None; state bindings None; contexts None.
- **Dependencies:** internal `src/components/features/developer-section/developer-section-page.tsx`; external None.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** None.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/app/(withSidebarLayout)/layout.tsx`

- **Why it exists / responsibility:** Route-group layout; performs the client session gate and composes the authenticated navigation/provider shell.
- **Runtime and size:** Client module; 92 lines.
- **Exports / surface:** `SidebarLayout`, `SidebarLayout({ children }: { children: React.ReactNode })`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** `useRouter`, `useState`, `useEffect`, `useLoginStatusQuery`, `useApiKeyQuery`, `useCustomerIdQuery`; state bindings `loading`, `setLoading`, `redirecting`, `setRedirecting`; contexts None.
- **Dependencies:** internal `src/components/sidebar/app-sidebar.tsx`, `src/components/ui/sidebar.tsx`, `src/components/navbar/header.tsx`, `src/store/api/auth.ts`, `src/store/api/api-key.ts`, `src/config/page.ts`, `src/store/api/user-info.ts`, `src/components/providers/upload/upload-sequence-provider.tsx`, plus 5 more; external `react`, `next/navigation`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** None.
- **Improvement / caution:** Keep effect dependencies complete and verify cleanup under React Strict Mode.

#### `src/app/(withSidebarLayout)/multiplayer-server/page.tsx`

- **Why it exists / responsibility:** App Router page entry for `/multiplayer-server`; composes the route's primary feature component.
- **Runtime and size:** Server-rendered/static route entry; 6 lines.
- **Exports / surface:** `DedicatedServer`, `DedicatedServer`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** None; state bindings None; contexts None.
- **Dependencies:** internal `src/components/features/dedicated-server/dedicated-server-page.tsx`; external None.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** None.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/app/(withSidebarLayout)/page.tsx`

- **Why it exists / responsibility:** App Router page entry for `/`; composes the route's primary feature component.
- **Runtime and size:** Server-rendered/static route entry; 10 lines.
- **Exports / surface:** `Home`, `Home`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** None; state bindings None; contexts None.
- **Dependencies:** internal `src/components/features/streaming-app/streaming-app-page.tsx`; external None.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** None.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/app/(withSidebarLayout)/payment-failed/page.tsx`

- **Why it exists / responsibility:** App Router page entry for `/payment-failed`; composes the route's primary feature component.
- **Runtime and size:** Client module; 54 lines.
- **Exports / surface:** `PaymentFailed`, `PaymentFailed`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** None; state bindings None; contexts None.
- **Dependencies:** internal `src/components/ui/button.tsx`, `src/assets/index.ts`; external `next/image`, `lucide-react`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** None.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/app/(withSidebarLayout)/payment-successful/page.tsx`

- **Why it exists / responsibility:** App Router page entry for `/payment-successful`; composes the route's primary feature component.
- **Runtime and size:** Server-rendered/static route entry; 6 lines.
- **Exports / surface:** `PaymentSuccessful`, `PaymentSuccessful`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** None; state bindings None; contexts None.
- **Dependencies:** internal `src/components/features/post-payment/payment-successful-page.tsx`; external None.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** None.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/app/(withSidebarLayout)/plans/page.tsx`

- **Why it exists / responsibility:** App Router page entry for `/plans`; composes the route's primary feature component.
- **Runtime and size:** Server-rendered/static route entry; 11 lines.
- **Exports / surface:** `Plan`, `Plan`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** None; state bindings None; contexts None.
- **Dependencies:** internal `src/components/features/plan/features/all-pan.tsx`; external None.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** None.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/app/(withSidebarLayout)/team/page.tsx`

- **Why it exists / responsibility:** App Router page entry for `/team`; composes the route's primary feature component.
- **Runtime and size:** Server-rendered/static route entry; 6 lines.
- **Exports / surface:** `Team`, `Team`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** None; state bindings None; contexts None.
- **Dependencies:** internal `src/components/features/team/team-page.tsx`; external None.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** None.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/app/(withSidebarLayout)/upload/app-upload-modal.tsx`

- **Why it exists / responsibility:** App Upload Modal repository/tooling configuration.
- **Runtime and size:** Client module; 485 lines.
- **Exports / surface:** `UploadDialog`, `UploadDialog`, `getVersionNumber(versionString: string | null)`, `handleRetry`, `handleModalClose`, `handleModalOpenChange(open: boolean)`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** `useUploadSequence`, `useSubscription`, `useStorage`, `useSubscriptionV2`, `useStorageV2`, `useCheckExactAssetNameMatch`, `useState`, `useEffect`; state bindings `showExeNotFoundError`, `setShowExeNotFoundError`, `showTutorial`, `setShowTutorial`, `hasShownUploadCompleteModal`, `setHasShownUploadCompleteModal`, `psPluginModal`, `setPsPluginModal`, `linuxBuildModal`, `setLinuxBuildModal`, `noPluginWarningModal`, `setNoPluginWarningModal`, plus 4 more; contexts None.
- **Dependencies:** internal `src/components/ui/button.tsx`, `src/components/ui/dialog.tsx`, `src/components/ui/input.tsx`, `src/components/ui/label.tsx`, `src/components/ui/switch.tsx`, `src/components/shared/file-drop-zone.tsx`, `src/components/shared/dialog/custom-dialog.tsx`, `src/components/shared/dialog/dialog-header.tsx`, plus 11 more; external `react`, `lucide-react`, `sonner`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/components/features/streaming-app/application.tsx`.
- **Improvement / caution:** Decompose by responsibility and add focused tests before changing behavior.

#### `src/app/(withSidebarLayout)/upload/sequence.tsx`

- **Why it exists / responsibility:** Sequence repository/tooling configuration.
- **Runtime and size:** Client module; 258 lines.
- **Exports / surface:** `default`, `Sequence({ showExeNotFoundError }: { showExeNotFoundError: boolean })`, `renderLoadingStates(step: string)`.
- **Props / inputs visible at declarations:** `Sequence: { showExeNotFoundError }: { showExeNotFoundError: boolean }`.
- **Hooks / local state:** `useUploadSequence`; state bindings None; contexts None.
- **Dependencies:** internal `src/components/shared/logo/ExeLogo.tsx`, `src/components/shared/logo/SDDownloadLogo.tsx`, `src/components/shared/logo/UploadLogo.tsx`, `src/components/shared/logo/SDExtractLogo.tsx`, `src/components/shared/logo/AppTestQueueLogo.tsx`, `src/components/shared/logo/AppTestLogo.tsx`, `src/components/providers/upload/upload-sequence-provider.tsx`; external `react`, `lucide-react`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/app/(withSidebarLayout)/upload/app-upload-modal.tsx`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/app/(withSidebarLayout)/upload/StorageProgressBar.tsx`

- **Why it exists / responsibility:** Storage Progress Bar repository/tooling configuration.
- **Runtime and size:** Client module; 38 lines.
- **Exports / surface:** None.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** None; state bindings None; contexts None.
- **Dependencies:** internal `src/lib/utils.ts`; external `react`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/app/(withSidebarLayout)/upload/upload-details.tsx`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/app/(withSidebarLayout)/upload/upload-details.tsx`

- **Why it exists / responsibility:** Upload Details repository/tooling configuration.
- **Runtime and size:** Server-rendered/static route entry; 222 lines.
- **Exports / surface:** `UploadDetails`, `UploadDetails`, `getUploadStatusText`, `handleImageSelect(e: React.ChangeEvent<HTMLInputElement>)`, `handleDragOver(e: React.DragEvent)`, `handleDrop(e: React.DragEvent)`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** `useUploadSequence`, `useSubscription`, `useSubscriptionV2`, `useStorage`, `useStorageV2`, `useState`, `useRef`, `useEffect`; state bindings `thumbnailPreview`, `setThumbnailPreview`; contexts None.
- **Dependencies:** internal `src/lib/utils.ts`, `src/app/(withSidebarLayout)/upload/StorageProgressBar.tsx`, `src/utils/size-formatter.ts`, `src/components/providers/upload/upload-sequence-provider.tsx`, `src/hooks/use-storage.ts`, `src/utils/upload.ts`, `src/hooks/use-storageV2.ts`, `src/hooks/use-subscription.ts`, plus 1 more; external `react`, `lucide-react`, `sonner`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/app/(withSidebarLayout)/upload/app-upload-modal.tsx`.
- **Improvement / caution:** Keep effect dependencies complete and verify cleanup under React Strict Mode.

#### `src/app/(withSidebarLayout)/upload/UploadError.tsx`

- **Why it exists / responsibility:** Upload Error repository/tooling configuration.
- **Runtime and size:** Server-rendered/static route entry; 134 lines.
- **Exports / surface:** `UploadError`, `UploadError({ onShowTutorial }: { onShowTutorial: () => void })`.
- **Props / inputs visible at declarations:** `UploadError: { onShowTutorial }: { onShowTutorial: () => void }`.
- **Hooks / local state:** `useUploadSequence`; state bindings None; contexts None.
- **Dependencies:** internal `src/components/providers/upload/upload-sequence-provider.tsx`; external `lucide-react`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/app/(withSidebarLayout)/upload/app-upload-modal.tsx`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/app/(withSidebarLayout)/upload/use-before-unload.ts`

- **Why it exists / responsibility:** Use Before Unload repository/tooling configuration.
- **Runtime and size:** Server-rendered/static route entry; 23 lines.
- **Exports / surface:** `default`, `useBeforeUnload(message: string, shouldWarn: boolean)`, `handleBeforeUnload(event: BeforeUnloadEvent)`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** `useEffect`; state bindings None; contexts None.
- **Dependencies:** internal None; external `react`.
- **Rendering and rerenders:** No JSX detected; it runs when imported or when an exported function/hook is called.
- **Direct consumers:** `src/components/features/streaming-app/application.tsx`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/app/(withSidebarLayout)/upload/VideoTutorialModal.tsx`

- **Why it exists / responsibility:** Video Tutorial Modal repository/tooling configuration.
- **Runtime and size:** Server-rendered/static route entry; 48 lines.
- **Exports / surface:** `default`, `VideoTutorial({ onBack }: { onBack: () => void })`.
- **Props / inputs visible at declarations:** `VideoTutorial: { onBack }: { onBack: () => void }`.
- **Hooks / local state:** None; state bindings None; contexts None.
- **Dependencies:** internal `src/components/ui/button.tsx`, `src/components/ui/dialog.tsx`, `src/app/fonts.ts`; external `react`, `lucide-react`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/app/(withSidebarLayout)/upload/app-upload-modal.tsx`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/app/(withSidebarLayout)/utilities/page.tsx`

- **Why it exists / responsibility:** App Router page entry for `/utilities`; composes the route's primary feature component.
- **Runtime and size:** Server-rendered/static route entry; 6 lines.
- **Exports / surface:** `Utilities`, `Utilities`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** None; state bindings None; contexts None.
- **Dependencies:** internal `src/components/features/utilities/utilities-page.tsx`; external None.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** None.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

### src/app

#### `src/app/favicon.ico`

- **Why it exists / responsibility:** Favicon repository/tooling configuration.
- **Runtime and size:** Build/tooling or documentation; 448081 bytes.
- **Exports / hooks / state:** Not applicable to this non-source file.
- **Dependencies / consumers:** Referenced by tooling, URL strings, CSS, metadata, CI, or documentation; static TypeScript imports do not provide a complete signal.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/app/fonts.ts`

- **Why it exists / responsibility:** Fonts repository/tooling configuration.
- **Runtime and size:** Server-rendered/static route entry; 6 lines.
- **Exports / surface:** `spantaran`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** None; state bindings None; contexts None.
- **Dependencies:** internal None; external `next/font/local`.
- **Rendering and rerenders:** No JSX detected; it runs when imported or when an exported function/hook is called.
- **Direct consumers:** `src/app/(withSidebarLayout)/upload/VideoTutorialModal.tsx`, `src/components/features/plan/new-pricing-card.tsx`, `src/components/features/plan/plan-page.tsx`, `src/components/features/singup/signup-page.tsx`, `src/components/features/username/username-page.tsx`, `src/components/shared/dialog/dialog-header.tsx`, `src/styles/custom-font.ts`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/app/globals.css`

- **Why it exists / responsibility:** Global Tailwind v4 imports, design tokens, dark-mode variables, and base element rules.
- **Runtime and size:** Build/tooling or documentation; 6592 bytes.
- **Exports / hooks / state:** Not applicable to this non-source file.
- **Dependencies / consumers:** Referenced by tooling, URL strings, CSS, metadata, CI, or documentation; static TypeScript imports do not provide a complete signal.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/app/layout.tsx`

- **Why it exists / responsibility:** Root App Router layout; defines metadata, fonts, global scripts, CSS, and application providers.
- **Runtime and size:** Server-rendered/static route entry; 74 lines.
- **Exports / surface:** `metadata`, `RootLayout`, `RootLayout({
  children
}: Readonly<{
  children: React.ReactNode;
}>)`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** None; state bindings None; contexts None.
- **Dependencies:** internal `src/app/globals.css`, `src/components/ui/sonner.tsx`, `src/components/providers/providers.tsx`, `src/components/error/uncaught-error-catcher.tsx`, `src/components/consent/ConsentBanner.tsx`, `src/components/auth/super-admin-handler.tsx`; external `next/script`, `next/font/google`, `next`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** None; static analysis found no internal importer, so verify framework entry/dynamic use.
- **Improvement / caution:** Confirm whether this is a framework root, planned module, or dead code before retaining it.

#### `src/app/not-found.tsx`

- **Why it exists / responsibility:** Global 404 route UI.
- **Runtime and size:** Server-rendered/static route entry; 6 lines.
- **Exports / surface:** `NotFound`, `NotFound`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** None; state bindings None; contexts None.
- **Dependencies:** internal None; external `next/navigation`.
- **Rendering and rerenders:** No JSX detected; it runs when imported or when an exported function/hook is called.
- **Direct consumers:** None; static analysis found no internal importer, so verify framework entry/dynamic use.
- **Improvement / caution:** Confirm whether this is a framework root, planned module, or dead code before retaining it.

### src/assets/images

#### `src/assets/images/companies.jpg`

- **Why it exists / responsibility:** Companies source-imported static visual asset.
- **Runtime and size:** Static asset; 48326 bytes.
- **Exports / hooks / state:** Not applicable to this non-source file.
- **Dependencies / consumers:** Referenced by tooling, URL strings, CSS, metadata, CI, or documentation; static TypeScript imports do not provide a complete signal.
- **Improvement / caution:** Compress, name semantically, and remove only after checking string/dynamic references.

#### `src/assets/images/complany-logo.png`

- **Why it exists / responsibility:** Complany Logo source-imported static visual asset.
- **Runtime and size:** Static asset; 16794 bytes.
- **Exports / hooks / state:** Not applicable to this non-source file.
- **Dependencies / consumers:** Referenced by tooling, URL strings, CSS, metadata, CI, or documentation; static TypeScript imports do not provide a complete signal.
- **Improvement / caution:** Compress, name semantically, and remove only after checking string/dynamic references.

#### `src/assets/images/core.png`

- **Why it exists / responsibility:** Core source-imported static visual asset.
- **Runtime and size:** Static asset; 57072 bytes.
- **Exports / hooks / state:** Not applicable to this non-source file.
- **Dependencies / consumers:** Referenced by tooling, URL strings, CSS, metadata, CI, or documentation; static TypeScript imports do not provide a complete signal.
- **Improvement / caution:** Compress, name semantically, and remove only after checking string/dynamic references.

#### `src/assets/images/CP1.jpg`

- **Why it exists / responsibility:** CP1 source-imported static visual asset.
- **Runtime and size:** Static asset; 27899 bytes.
- **Exports / hooks / state:** Not applicable to this non-source file.
- **Dependencies / consumers:** Referenced by tooling, URL strings, CSS, metadata, CI, or documentation; static TypeScript imports do not provide a complete signal.
- **Improvement / caution:** Compress, name semantically, and remove only after checking string/dynamic references.

#### `src/assets/images/CP10.jpg`

- **Why it exists / responsibility:** CP10 source-imported static visual asset.
- **Runtime and size:** Static asset; 35908 bytes.
- **Exports / hooks / state:** Not applicable to this non-source file.
- **Dependencies / consumers:** Referenced by tooling, URL strings, CSS, metadata, CI, or documentation; static TypeScript imports do not provide a complete signal.
- **Improvement / caution:** Compress, name semantically, and remove only after checking string/dynamic references.

#### `src/assets/images/CP11.jpg`

- **Why it exists / responsibility:** CP11 source-imported static visual asset.
- **Runtime and size:** Static asset; 35443 bytes.
- **Exports / hooks / state:** Not applicable to this non-source file.
- **Dependencies / consumers:** Referenced by tooling, URL strings, CSS, metadata, CI, or documentation; static TypeScript imports do not provide a complete signal.
- **Improvement / caution:** Compress, name semantically, and remove only after checking string/dynamic references.

#### `src/assets/images/CP2.jpg`

- **Why it exists / responsibility:** CP2 source-imported static visual asset.
- **Runtime and size:** Static asset; 30722 bytes.
- **Exports / hooks / state:** Not applicable to this non-source file.
- **Dependencies / consumers:** Referenced by tooling, URL strings, CSS, metadata, CI, or documentation; static TypeScript imports do not provide a complete signal.
- **Improvement / caution:** Compress, name semantically, and remove only after checking string/dynamic references.

#### `src/assets/images/CP3.jpg`

- **Why it exists / responsibility:** CP3 source-imported static visual asset.
- **Runtime and size:** Static asset; 29179 bytes.
- **Exports / hooks / state:** Not applicable to this non-source file.
- **Dependencies / consumers:** Referenced by tooling, URL strings, CSS, metadata, CI, or documentation; static TypeScript imports do not provide a complete signal.
- **Improvement / caution:** Compress, name semantically, and remove only after checking string/dynamic references.

#### `src/assets/images/CP4.jpg`

- **Why it exists / responsibility:** CP4 source-imported static visual asset.
- **Runtime and size:** Static asset; 31297 bytes.
- **Exports / hooks / state:** Not applicable to this non-source file.
- **Dependencies / consumers:** Referenced by tooling, URL strings, CSS, metadata, CI, or documentation; static TypeScript imports do not provide a complete signal.
- **Improvement / caution:** Compress, name semantically, and remove only after checking string/dynamic references.

#### `src/assets/images/CP5.jpg`

- **Why it exists / responsibility:** CP5 source-imported static visual asset.
- **Runtime and size:** Static asset; 33461 bytes.
- **Exports / hooks / state:** Not applicable to this non-source file.
- **Dependencies / consumers:** Referenced by tooling, URL strings, CSS, metadata, CI, or documentation; static TypeScript imports do not provide a complete signal.
- **Improvement / caution:** Compress, name semantically, and remove only after checking string/dynamic references.

#### `src/assets/images/CP6.jpg`

- **Why it exists / responsibility:** CP6 source-imported static visual asset.
- **Runtime and size:** Static asset; 28446 bytes.
- **Exports / hooks / state:** Not applicable to this non-source file.
- **Dependencies / consumers:** Referenced by tooling, URL strings, CSS, metadata, CI, or documentation; static TypeScript imports do not provide a complete signal.
- **Improvement / caution:** Compress, name semantically, and remove only after checking string/dynamic references.

#### `src/assets/images/CP7.jpg`

- **Why it exists / responsibility:** CP7 source-imported static visual asset.
- **Runtime and size:** Static asset; 31435 bytes.
- **Exports / hooks / state:** Not applicable to this non-source file.
- **Dependencies / consumers:** Referenced by tooling, URL strings, CSS, metadata, CI, or documentation; static TypeScript imports do not provide a complete signal.
- **Improvement / caution:** Compress, name semantically, and remove only after checking string/dynamic references.

#### `src/assets/images/CP8.jpg`

- **Why it exists / responsibility:** CP8 source-imported static visual asset.
- **Runtime and size:** Static asset; 29178 bytes.
- **Exports / hooks / state:** Not applicable to this non-source file.
- **Dependencies / consumers:** Referenced by tooling, URL strings, CSS, metadata, CI, or documentation; static TypeScript imports do not provide a complete signal.
- **Improvement / caution:** Compress, name semantically, and remove only after checking string/dynamic references.

#### `src/assets/images/CP9.jpg`

- **Why it exists / responsibility:** CP9 source-imported static visual asset.
- **Runtime and size:** Static asset; 31528 bytes.
- **Exports / hooks / state:** Not applicable to this non-source file.
- **Dependencies / consumers:** Referenced by tooling, URL strings, CSS, metadata, CI, or documentation; static TypeScript imports do not provide a complete signal.
- **Improvement / caution:** Compress, name semantically, and remove only after checking string/dynamic references.

#### `src/assets/images/Discord-Icon.png`

- **Why it exists / responsibility:** Discord Icon source-imported static visual asset.
- **Runtime and size:** Static asset; 1552 bytes.
- **Exports / hooks / state:** Not applicable to this non-source file.
- **Dependencies / consumers:** Referenced by tooling, URL strings, CSS, metadata, CI, or documentation; static TypeScript imports do not provide a complete signal.
- **Improvement / caution:** Compress, name semantically, and remove only after checking string/dynamic references.

#### `src/assets/images/ds-default-pic.jpg`

- **Why it exists / responsibility:** Ds Default Pic source-imported static visual asset.
- **Runtime and size:** Static asset; 35443 bytes.
- **Exports / hooks / state:** Not applicable to this non-source file.
- **Dependencies / consumers:** Referenced by tooling, URL strings, CSS, metadata, CI, or documentation; static TypeScript imports do not provide a complete signal.
- **Improvement / caution:** Compress, name semantically, and remove only after checking string/dynamic references.

#### `src/assets/images/eagle-bg.svg`

- **Why it exists / responsibility:** Eagle Bg source-imported static visual asset.
- **Runtime and size:** Static asset; 1457 bytes.
- **Exports / hooks / state:** Not applicable to this non-source file.
- **Dependencies / consumers:** Referenced by tooling, URL strings, CSS, metadata, CI, or documentation; static TypeScript imports do not provide a complete signal.
- **Improvement / caution:** Compress, name semantically, and remove only after checking string/dynamic references.

#### `src/assets/images/eagle-logo.png`

- **Why it exists / responsibility:** Eagle Logo source-imported static visual asset.
- **Runtime and size:** Static asset; 45150 bytes.
- **Exports / hooks / state:** Not applicable to this non-source file.
- **Dependencies / consumers:** Referenced by tooling, URL strings, CSS, metadata, CI, or documentation; static TypeScript imports do not provide a complete signal.
- **Improvement / caution:** Compress, name semantically, and remove only after checking string/dynamic references.

#### `src/assets/images/eagle-update-logo.svg`

- **Why it exists / responsibility:** Eagle Update Logo source-imported static visual asset.
- **Runtime and size:** Static asset; 8608 bytes.
- **Exports / hooks / state:** Not applicable to this non-source file.
- **Dependencies / consumers:** Referenced by tooling, URL strings, CSS, metadata, CI, or documentation; static TypeScript imports do not provide a complete signal.
- **Improvement / caution:** Compress, name semantically, and remove only after checking string/dynamic references.

#### `src/assets/images/enterprise.png`

- **Why it exists / responsibility:** Enterprise source-imported static visual asset.
- **Runtime and size:** Static asset; 33415 bytes.
- **Exports / hooks / state:** Not applicable to this non-source file.
- **Dependencies / consumers:** Referenced by tooling, URL strings, CSS, metadata, CI, or documentation; static TypeScript imports do not provide a complete signal.
- **Improvement / caution:** Compress, name semantically, and remove only after checking string/dynamic references.

#### `src/assets/images/failed.svg`

- **Why it exists / responsibility:** Failed source-imported static visual asset.
- **Runtime and size:** Static asset; 24338 bytes.
- **Exports / hooks / state:** Not applicable to this non-source file.
- **Dependencies / consumers:** Referenced by tooling, URL strings, CSS, metadata, CI, or documentation; static TypeScript imports do not provide a complete signal.
- **Improvement / caution:** Compress, name semantically, and remove only after checking string/dynamic references.

#### `src/assets/images/fb.png`

- **Why it exists / responsibility:** Fb source-imported static visual asset.
- **Runtime and size:** Static asset; 1462 bytes.
- **Exports / hooks / state:** Not applicable to this non-source file.
- **Dependencies / consumers:** Referenced by tooling, URL strings, CSS, metadata, CI, or documentation; static TypeScript imports do not provide a complete signal.
- **Improvement / caution:** Compress, name semantically, and remove only after checking string/dynamic references.

#### `src/assets/images/google-logo.png`

- **Why it exists / responsibility:** Google Logo source-imported static visual asset.
- **Runtime and size:** Static asset; 73925 bytes.
- **Exports / hooks / state:** Not applicable to this non-source file.
- **Dependencies / consumers:** Referenced by tooling, URL strings, CSS, metadata, CI, or documentation; static TypeScript imports do not provide a complete signal.
- **Improvement / caution:** Compress, name semantically, and remove only after checking string/dynamic references.

#### `src/assets/images/instra.png`

- **Why it exists / responsibility:** Instra source-imported static visual asset.
- **Runtime and size:** Static asset; 1401 bytes.
- **Exports / hooks / state:** Not applicable to this non-source file.
- **Dependencies / consumers:** Referenced by tooling, URL strings, CSS, metadata, CI, or documentation; static TypeScript imports do not provide a complete signal.
- **Improvement / caution:** Compress, name semantically, and remove only after checking string/dynamic references.

#### `src/assets/images/limit.png`

- **Why it exists / responsibility:** Limit source-imported static visual asset.
- **Runtime and size:** Static asset; 4531 bytes.
- **Exports / hooks / state:** Not applicable to this non-source file.
- **Dependencies / consumers:** Referenced by tooling, URL strings, CSS, metadata, CI, or documentation; static TypeScript imports do not provide a complete signal.
- **Improvement / caution:** Compress, name semantically, and remove only after checking string/dynamic references.

#### `src/assets/images/limit.svg`

- **Why it exists / responsibility:** Limit source-imported static visual asset.
- **Runtime and size:** Static asset; 24532 bytes.
- **Exports / hooks / state:** Not applicable to this non-source file.
- **Dependencies / consumers:** Referenced by tooling, URL strings, CSS, metadata, CI, or documentation; static TypeScript imports do not provide a complete signal.
- **Improvement / caution:** Compress, name semantically, and remove only after checking string/dynamic references.

#### `src/assets/images/Linkedin-Icon.png`

- **Why it exists / responsibility:** Linkedin Icon source-imported static visual asset.
- **Runtime and size:** Static asset; 1378 bytes.
- **Exports / hooks / state:** Not applicable to this non-source file.
- **Dependencies / consumers:** Referenced by tooling, URL strings, CSS, metadata, CI, or documentation; static TypeScript imports do not provide a complete signal.
- **Improvement / caution:** Compress, name semantically, and remove only after checking string/dynamic references.

#### `src/assets/images/linux.svg`

- **Why it exists / responsibility:** Linux source-imported static visual asset.
- **Runtime and size:** Static asset; 10293 bytes.
- **Exports / hooks / state:** Not applicable to this non-source file.
- **Dependencies / consumers:** Referenced by tooling, URL strings, CSS, metadata, CI, or documentation; static TypeScript imports do not provide a complete signal.
- **Improvement / caution:** Compress, name semantically, and remove only after checking string/dynamic references.

#### `src/assets/images/notification-icon.svg`

- **Why it exists / responsibility:** Notification Icon source-imported static visual asset.
- **Runtime and size:** Static asset; 1184 bytes.
- **Exports / hooks / state:** Not applicable to this non-source file.
- **Dependencies / consumers:** Referenced by tooling, URL strings, CSS, metadata, CI, or documentation; static TypeScript imports do not provide a complete signal.
- **Improvement / caution:** Compress, name semantically, and remove only after checking string/dynamic references.

#### `src/assets/images/sidebar-icon.svg`

- **Why it exists / responsibility:** Sidebar Icon source-imported static visual asset.
- **Runtime and size:** Static asset; 330 bytes.
- **Exports / hooks / state:** Not applicable to this non-source file.
- **Dependencies / consumers:** Referenced by tooling, URL strings, CSS, metadata, CI, or documentation; static TypeScript imports do not provide a complete signal.
- **Improvement / caution:** Compress, name semantically, and remove only after checking string/dynamic references.

#### `src/assets/images/upload-img.svg`

- **Why it exists / responsibility:** Upload Img source-imported static visual asset.
- **Runtime and size:** Static asset; 2255 bytes.
- **Exports / hooks / state:** Not applicable to this non-source file.
- **Dependencies / consumers:** Referenced by tooling, URL strings, CSS, metadata, CI, or documentation; static TypeScript imports do not provide a complete signal.
- **Improvement / caution:** Compress, name semantically, and remove only after checking string/dynamic references.

#### `src/assets/images/upload-video.svg`

- **Why it exists / responsibility:** Upload Video source-imported static visual asset.
- **Runtime and size:** Static asset; 793 bytes.
- **Exports / hooks / state:** Not applicable to this non-source file.
- **Dependencies / consumers:** Referenced by tooling, URL strings, CSS, metadata, CI, or documentation; static TypeScript imports do not provide a complete signal.
- **Improvement / caution:** Compress, name semantically, and remove only after checking string/dynamic references.

#### `src/assets/images/user-logo.png`

- **Why it exists / responsibility:** User Logo source-imported static visual asset.
- **Runtime and size:** Static asset; 8292 bytes.
- **Exports / hooks / state:** Not applicable to this non-source file.
- **Dependencies / consumers:** Referenced by tooling, URL strings, CSS, metadata, CI, or documentation; static TypeScript imports do not provide a complete signal.
- **Improvement / caution:** Compress, name semantically, and remove only after checking string/dynamic references.

#### `src/assets/images/user-support.png`

- **Why it exists / responsibility:** User Support source-imported static visual asset.
- **Runtime and size:** Static asset; 8307 bytes.
- **Exports / hooks / state:** Not applicable to this non-source file.
- **Dependencies / consumers:** Referenced by tooling, URL strings, CSS, metadata, CI, or documentation; static TypeScript imports do not provide a complete signal.
- **Improvement / caution:** Compress, name semantically, and remove only after checking string/dynamic references.

#### `src/assets/images/windows.svg`

- **Why it exists / responsibility:** Windows source-imported static visual asset.
- **Runtime and size:** Static asset; 514 bytes.
- **Exports / hooks / state:** Not applicable to this non-source file.
- **Dependencies / consumers:** Referenced by tooling, URL strings, CSS, metadata, CI, or documentation; static TypeScript imports do not provide a complete signal.
- **Improvement / caution:** Compress, name semantically, and remove only after checking string/dynamic references.

#### `src/assets/images/x.png`

- **Why it exists / responsibility:** X source-imported static visual asset.
- **Runtime and size:** Static asset; 1472 bytes.
- **Exports / hooks / state:** Not applicable to this non-source file.
- **Dependencies / consumers:** Referenced by tooling, URL strings, CSS, metadata, CI, or documentation; static TypeScript imports do not provide a complete signal.
- **Improvement / caution:** Compress, name semantically, and remove only after checking string/dynamic references.

#### `src/assets/images/you-tube.png`

- **Why it exists / responsibility:** You Tube source-imported static visual asset.
- **Runtime and size:** Static asset; 1364 bytes.
- **Exports / hooks / state:** Not applicable to this non-source file.
- **Dependencies / consumers:** Referenced by tooling, URL strings, CSS, metadata, CI, or documentation; static TypeScript imports do not provide a complete signal.
- **Improvement / caution:** Compress, name semantically, and remove only after checking string/dynamic references.

### src/assets

#### `src/assets/index.ts`

- **Why it exists / responsibility:** Index source-imported static visual asset.
- **Runtime and size:** Static asset; 82 lines.
- **Exports / surface:** `default`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** None; state bindings None; contexts None.
- **Dependencies:** internal `src/assets/images/eagle-logo.png`, `src/assets/images/user-logo.png`, `src/assets/images/ds-default-pic.jpg`, `src/assets/images/eagle-update-logo.svg`, `src/assets/images/google-logo.png`, `src/assets/images/limit.svg`, `src/assets/images/upload-img.svg`, `src/assets/images/upload-video.svg`, plus 28 more; external None.
- **Rendering and rerenders:** No JSX detected; it runs when imported or when an exported function/hook is called.
- **Direct consumers:** `src/app/(withSidebarLayout)/credit-card-failed/page.js`, `src/app/(withSidebarLayout)/credit-card-successful-core/page.tsx`, `src/app/(withSidebarLayout)/credit-card-successful/page.tsx`, `src/app/(withSidebarLayout)/payment-failed/page.tsx`, `src/components/features/dedicated-server/dedicated-server-card.tsx`, `src/components/features/my-account/profile-settings.tsx`, `src/components/features/plan/faq.tsx`, `src/components/features/plan/new-pricing-card.tsx`, plus 19 more.
- **Improvement / caution:** Compress, name semantically, and remove only after checking string/dynamic references.

### src/components/admin

#### `src/components/admin/super-admin-url-generator.tsx`

- **Why it exists / responsibility:** Super Admin Url Generator Super Admin support component.
- **Runtime and size:** Client module; 148 lines.
- **Exports / surface:** `SuperAdminUrlGenerator`, `SuperAdminUrlGenerator`, `handleGenerate(e: React.FormEvent)`, `handleCopy`, `handleReset`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** `useState`; state bindings `form`, `setForm`, `generatedUrl`, `setGeneratedUrl`, `copied`, `setCopied`; contexts None.
- **Dependencies:** internal `src/components/ui/button.tsx`, `src/components/ui/input.tsx`, `src/components/ui/card.tsx`, `src/utils/super-admin-login.ts`; external `react`, `lucide-react`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** None; static analysis found no internal importer, so verify framework entry/dynamic use.
- **Improvement / caution:** Replace URL/localStorage credentials with a short-lived audited backend exchange.

### src/components/auth

#### `src/components/auth/super-admin-handler.tsx`

- **Why it exists / responsibility:** Super Admin Handler authentication-support component.
- **Runtime and size:** Client module; 47 lines.
- **Exports / surface:** `SuperAdminHandler`, `SuperAdminHandler`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** `useRouter`, `useEffect`; state bindings None; contexts None.
- **Dependencies:** internal `src/utils/super-admin-login.ts`, `src/config/page.ts`; external `react`, `next/navigation`.
- **Rendering and rerenders:** No JSX detected; it runs when imported or when an exported function/hook is called.
- **Direct consumers:** `src/app/layout.tsx`.
- **Improvement / caution:** Replace URL/localStorage credentials with a short-lived audited backend exchange.

### src/components/consent

#### `src/components/consent/ConsentBanner.tsx`

- **Why it exists / responsibility:** Client consent-banner UI and browser preference storage.
- **Runtime and size:** Client module; 91 lines.
- **Exports / surface:** `ConsentBanner`, `ConsentBanner`, `handleConsent(consent: 'accepted' | 'rejected')`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** `useState`, `useEffect`; state bindings `isVisible`, `setIsVisible`; contexts None.
- **Dependencies:** internal None; external `react`, `js-cookie`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/app/layout.tsx`.
- **Improvement / caution:** Keep effect dependencies complete and verify cleanup under React Strict Mode.

### src/components/error

#### `src/components/error/error-service.ts`

- **Why it exists / responsibility:** Error Service error reporting or fallback UI.
- **Runtime and size:** Shared/build-time eligible module; 98 lines.
- **Exports / surface:** `getDeviceInfo`, `uncaughtNotify`, `getDeviceInfo`, `uncaughtNotify(errorName: string, errorInfo: any, username: string, email: string)`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** None; state bindings None; contexts None.
- **Dependencies:** internal `src/config/api.ts`; external `axios`.
- **Rendering and rerenders:** No JSX detected; it runs when imported or when an exported function/hook is called.
- **Direct consumers:** `src/components/error/uncaught-error-catcher.tsx`.
- **Improvement / caution:** Confirm cancellation, error normalization, and cache synchronization for direct Axios traffic.

#### `src/components/error/uncaught-error-catcher.tsx`

- **Why it exists / responsibility:** Uncaught Error Catcher error reporting or fallback UI.
- **Runtime and size:** Client module; 22 lines.
- **Exports / surface:** `UnCaughtErrorCatcher`, `UnCaughtErrorCatcher`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** `useUserInfo`, `useEffect`; state bindings None; contexts None.
- **Dependencies:** internal `src/components/error/error-service.ts`, `src/hooks/use-user-info.ts`; external `react`.
- **Rendering and rerenders:** No JSX detected; it runs when imported or when an exported function/hook is called.
- **Direct consumers:** `src/app/layout.tsx`.
- **Improvement / caution:** Keep effect dependencies complete and verify cleanup under React Strict Mode.

### src/components/features

#### `src/components/features/additional-upload/additional-upload-details.tsx`

- **Why it exists / responsibility:** Additional Upload feature module responsible for the additional upload details portion of its screen or workflow.
- **Runtime and size:** Client-bound through an importing client module; 39 lines.
- **Exports / surface:** `AdditionalUploadDetails`, `AdditionalUploadDetails`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** `useAdditionalUpload`; state bindings None; contexts None.
- **Dependencies:** internal `src/components/ui/progress.tsx`, `src/lib/utils.ts`, `src/utils/size-formatter.ts`, `src/components/providers/upload/additional-upload-provider.tsx`; external `lucide-react`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/components/features/additional-upload/additional-upload-page.tsx`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/components/features/additional-upload/additional-upload-page.tsx`

- **Why it exists / responsibility:** Additional Upload feature module responsible for the additional upload page portion of its screen or workflow.
- **Runtime and size:** Client module; 252 lines.
- **Exports / surface:** `AdditionalUploadsPage`, `AdditionalUploadsPage`, `handleFileSelect(selectedFile: File | null)`, `handleBackButton`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** `useUserInfo`, `useAdditionalUploadAppListQuery`, `useState`, `useAdditionalUpload`; state bindings `activeTab`, `setActiveTab`; contexts None.
- **Dependencies:** internal `src/components/ui/button.tsx`, `src/components/ui/tooltip.tsx`, `src/components/ui/dialog.tsx`, `src/components/shared/tabs.tsx`, `src/components/shared/common-table.tsx`, `src/components/shared/file-drop-zone.tsx`, `src/components/shared/dialog/custom-dialog.tsx`, `src/components/shared/dialog/dialog-header.tsx`, plus 5 more; external `react`, `lucide-react`, `@tanstack/react-table`, `date-fns`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/app/(withSidebarLayout)/additional-uploads/page.tsx`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/components/features/analytics/analytics-page.tsx`

- **Why it exists / responsibility:** Analytics feature module responsible for the analytics page portion of its screen or workflow.
- **Runtime and size:** Client module; 112 lines.
- **Exports / surface:** `AnalyticsPage`, `AnalyticsPage`, `handleApply`, `renderContent`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** `useState`, `useUserInfo`, `useEffect`, `useStreamRecordQuery`; state bindings `activeTab`, `setActiveTab`, `localEndDate`, `setLocalEndDate`, `localStartDate`, `setLocalStartDate`, `queryParams`, `setQueryParams`; contexts None.
- **Dependencies:** internal `src/components/shared/tabs.tsx`, `src/components/ui/headlines/page-title.tsx`, `src/hooks/use-user-info.ts`, `src/store/api/analytics.ts`, `src/components/features/analytics/types.ts`, `src/components/features/analytics/streaming-min-tab.tsx`, `src/components/features/analytics/ccu-tab.tsx`; external `react`, `date-fns`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/app/(withSidebarLayout)/analytics/page.tsx`.
- **Improvement / caution:** Keep effect dependencies complete and verify cleanup under React Strict Mode.

#### `src/components/features/analytics/ccu-tab.tsx`

- **Why it exists / responsibility:** Analytics feature module responsible for the ccu tab portion of its screen or workflow.
- **Runtime and size:** Client module; 114 lines.
- **Exports / surface:** `CcuTab`, `formatCcuTime(seconds: number)`, `CcuTab({
  localStartDate,
  localEndDate,
  onStartDateChange,
  onEndDateChange,
  onApply,
  streamData,
  isLoading,
  error
}: CcuTabProps)`.
- **Props / inputs visible at declarations:** `CcuTab: {
  localStartDate,
  localEndDate,
  onStartDateChange,
  onEndDateChange,
  onApply,
  streamData,
  isLoading,
  error
}: CcuTabProps`.
- **Hooks / local state:** `useCCUDataFromRecords`, `useMemo`; state bindings None; contexts None.
- **Dependencies:** internal `src/components/shared/common-table.tsx`, `src/components/shared/date-picker.tsx`, `src/components/ui/button.tsx`, `src/components/ui/card.tsx`, `src/hooks/use-ccu-data.ts`, `src/components/features/analytics/utils.ts`; external `react`, `@tanstack/react-table`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/components/features/analytics/analytics-page.tsx`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/components/features/analytics/charts/bar.tsx`

- **Why it exists / responsibility:** Analytics feature module responsible for the bar portion of its screen or workflow.
- **Runtime and size:** Client-bound through an importing client module; 69 lines.
- **Exports / surface:** `StreamBarChart`, `StreamBarChart({ streamRecordData }: BarChartProps)`.
- **Props / inputs visible at declarations:** `StreamBarChart: { streamRecordData }: BarChartProps`.
- **Hooks / local state:** `useState`; state bindings `filter`, `setFilter`; contexts None.
- **Dependencies:** internal `src/components/ui/card.tsx`, `src/components/ui/chart.tsx`, `src/components/ui/label.tsx`, `src/components/ui/select.tsx`, `src/components/features/analytics/utils.ts`, `src/components/features/analytics/types.ts`; external `react`, `recharts`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/components/features/analytics/streaming-min-tab.tsx`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/components/features/analytics/charts/pie.tsx`

- **Why it exists / responsibility:** Analytics feature module responsible for the pie portion of its screen or workflow.
- **Runtime and size:** Client-bound through an importing client module; 110 lines.
- **Exports / surface:** `StreamPieChart`, `StreamPieChart({ streamRecordData }: AppPieChartProps)`.
- **Props / inputs visible at declarations:** `StreamPieChart: { streamRecordData }: AppPieChartProps`.
- **Hooks / local state:** `useState`, `useMemo`; state bindings `filter`, `setFilter`, `activeIndex`, `setActiveIndex`; contexts None.
- **Dependencies:** internal `src/components/ui/card.tsx`, `src/components/ui/select.tsx`, `src/components/features/analytics/utils.ts`, `src/components/features/analytics/types.ts`, `src/components/ui/chart.tsx`; external `react`, `recharts`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/components/features/analytics/streaming-min-tab.tsx`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/components/features/analytics/streaming-min-tab.tsx`

- **Why it exists / responsibility:** Analytics feature module responsible for the streaming min tab portion of its screen or workflow.
- **Runtime and size:** Client module; 148 lines.
- **Exports / surface:** `StreamingMinTab`, `StreamingMinTab({
  localStartDate,
  localEndDate,
  onStartDateChange,
  onEndDateChange,
  onApply,
  streamData,
  totalSessions,
  totalStreamMinutes,
  isLoading
}: StreamingMinTabProps)`.
- **Props / inputs visible at declarations:** `StreamingMinTab: {
  localStartDate,
  localEndDate,
  onStartDateChange,
  onEndDateChange,
  onApply,
  streamData,
  totalSessions,
  totalStreamMinutes,
  isLoading
}: StreamingMinTabProps`.
- **Hooks / local state:** `useMemo`; state bindings None; contexts None.
- **Dependencies:** internal `src/components/shared/common-table.tsx`, `src/components/shared/date-picker.tsx`, `src/components/ui/button.tsx`, `src/components/ui/card.tsx`, `src/components/features/analytics/types.ts`, `src/components/features/analytics/charts/bar.tsx`, `src/components/features/analytics/charts/pie.tsx`, `src/components/features/analytics/utils.ts`; external `react`, `@tanstack/react-table`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/components/features/analytics/analytics-page.tsx`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/components/features/analytics/types.ts`

- **Why it exists / responsibility:** Analytics feature module responsible for the types portion of its screen or workflow.
- **Runtime and size:** Shared/build-time eligible module; 16 lines.
- **Exports / surface:** `TAnalyticsData`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** None; state bindings None; contexts None.
- **Dependencies:** internal None; external None.
- **Rendering and rerenders:** No JSX detected; it runs when imported or when an exported function/hook is called.
- **Direct consumers:** `src/components/features/analytics/analytics-page.tsx`, `src/components/features/analytics/charts/bar.tsx`, `src/components/features/analytics/charts/pie.tsx`, `src/components/features/analytics/streaming-min-tab.tsx`, `src/components/features/analytics/utils.ts`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/components/features/analytics/utils.ts`

- **Why it exists / responsibility:** Analytics feature module responsible for the utils portion of its screen or workflow.
- **Runtime and size:** Shared/build-time eligible module; 67 lines.
- **Exports / surface:** `chartFilters`, `formatAnalyticsRowDate`, `formatAppVersion`, `formatAnalyticsDateInput`, `filterAhsanName`, `chartDataNormalize`, `formatAnalyticsRowDate(value: unknown)`, `formatAppVersion(version: unknown)`, `formatAnalyticsDateInput(date: Date)`, `filterAhsanName(name: unknown)`, `chartDataNormalize(data: TAnalyticsData[], filter: string)`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** None; state bindings None; contexts None.
- **Dependencies:** internal `src/components/features/analytics/types.ts`; external `date-fns`.
- **Rendering and rerenders:** No JSX detected; it runs when imported or when an exported function/hook is called.
- **Direct consumers:** `src/components/features/analytics/ccu-tab.tsx`, `src/components/features/analytics/charts/bar.tsx`, `src/components/features/analytics/charts/pie.tsx`, `src/components/features/analytics/streaming-min-tab.tsx`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/components/features/dedicated-server/application.tsx`

- **Why it exists / responsibility:** Dedicated Server feature module responsible for the application portion of its screen or workflow.
- **Runtime and size:** Client-bound through an importing client module; 193 lines.
- **Exports / surface:** `AppData`, `Application`, `Application`, `handleCardClick(app: AppData)`, `handleUploadSuccess`, `handleUploadClose`, `handleAddNewApp`, `handleUpdateVersion(app: AppData)`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** `useUserInfo`, `useSubscription`, `useSubscriptionV2`, `useStorageV2`, `useTrialV2`, `useUploadSequence`, `useDsAppQuery`, `useState`; state bindings `selectedApp`, `setSelectedApp`, `uploadModalOpen`, `setUploadModalOpen`, `serverModalOpen`, `setServerModalOpen`, `searchTerm`, `setSearchTerm`, `sortBy`, `setSortBy`, `showAll`, `setShowAll`, plus 2 more; contexts None.
- **Dependencies:** internal `src/components/ui/input.tsx`, `src/components/ui/button.tsx`, `src/constant/asset.ts`, `src/hooks/use-user-info.ts`, `src/store/api/asset.ts`, `src/components/features/dedicated-server/dedicated-server-card.tsx`, `src/components/features/dedicated-server/dedicated-server-app-upload.tsx`, `src/components/features/dedicated-server/dedicated-server-card-details.tsx`, plus 7 more; external `react`, `lucide-react`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/components/features/dedicated-server/dedicated-server-card-details.tsx`, `src/components/features/dedicated-server/dedicated-server-page.tsx`.
- **Improvement / caution:** Consider a reducer/state machine to make transitions explicit.

#### `src/components/features/dedicated-server/dedicated-server-app-upload.tsx`

- **Why it exists / responsibility:** Dedicated Server feature module responsible for the dedicated server app upload portion of its screen or workflow.
- **Runtime and size:** Client-bound through an importing client module; 176 lines.
- **Exports / surface:** `DedicatedServerAppUpload`, `DedicatedServerAppUpload({
  isOpen,
  onClose,
  onSuccess,
  selectedApp,
  isUpdatingVersion = false
}: UploadApplicationModalProps)`, `handleFileSelect(file: File | null)`, `handleBackButton`, `handleClose`, `handleUploadWithCallback`, `getModalTitle`.
- **Props / inputs visible at declarations:** `DedicatedServerAppUpload: {
  isOpen,
  onClose,
  onSuccess,
  selectedApp,
  isUpdatingVersion = false
}: UploadApplicationModalProps`.
- **Hooks / local state:** `useDedicatedServerUpload`, `useEffect`; state bindings None; contexts None.
- **Dependencies:** internal `src/components/ui/button.tsx`, `src/components/ui/input.tsx`, `src/components/ui/label.tsx`, `src/components/shared/dialog/custom-dialog.tsx`, `src/components/shared/dialog/dialog-header.tsx`, `src/components/ui/dialog.tsx`, `src/components/shared/file-drop-zone.tsx`, `src/components/providers/upload/dedicated-server-upload-provider.tsx`, plus 2 more; external `react`, `lucide-react`, `sonner`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/components/features/dedicated-server/application.tsx`.
- **Improvement / caution:** Keep effect dependencies complete and verify cleanup under React Strict Mode.

#### `src/components/features/dedicated-server/dedicated-server-card-details.tsx`

- **Why it exists / responsibility:** Dedicated Server feature module responsible for the dedicated server card details portion of its screen or workflow.
- **Runtime and size:** Client-bound through an importing client module; 271 lines.
- **Exports / surface:** `DedicatedServerCardDetails`, `DedicatedServerCardDetails({ isOpen, onClose, selectedApp }: ServerDetailsModalProps)`, `handleStartServer`, `handleStopServer(server: ServerInstance)`, `handleDeleteApp`, `handleDeleteClick`, `handleCopy(copiedText: string)`.
- **Props / inputs visible at declarations:** `DedicatedServerCardDetails: { isOpen, onClose, selectedApp }: ServerDetailsModalProps`.
- **Hooks / local state:** `useUserInfo`, `useState`, `useDsInstanceStartMutation`, `useDsInstanceStopMutation`, `useDsAppDeleteMutation`, `useDedicatedServerInstanceListQuery`, `useGetLocationDetailsQuery`, `useMemo`; state bindings `stoppingServers`, `setStoppingServers`, `isRemoveModalOpen`, `setIsRemoveModalOpen`; contexts None.
- **Dependencies:** internal `src/components/ui/button.tsx`, `src/components/shared/dialog/custom-dialog.tsx`, `src/components/shared/dialog/dialog-header.tsx`, `src/components/ui/dialog.tsx`, `src/components/shared/common-table.tsx`, `src/hooks/use-user-info.ts`, `src/store/api/upload.ts`, `src/utils/common.ts`, plus 2 more; external `react`, `@tanstack/react-table`, `sonner`, `lucide-react`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/components/features/dedicated-server/application.tsx`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/components/features/dedicated-server/dedicated-server-card.tsx`

- **Why it exists / responsibility:** Dedicated Server feature module responsible for the dedicated server card portion of its screen or workflow.
- **Runtime and size:** Client-bound through an importing client module; 66 lines.
- **Exports / surface:** `DedicatedServerCard`, `DedicatedServerCard({ app, onClick, onUpdateVersion }: DedicatedServerCardProps)`, `handleShareClick(e: React.MouseEvent)`.
- **Props / inputs visible at declarations:** `DedicatedServerCard: { app, onClick, onUpdateVersion }: DedicatedServerCardProps`.
- **Hooks / local state:** None; state bindings None; contexts None.
- **Dependencies:** internal `src/components/ui/card.tsx`, `src/components/ui/button.tsx`, `src/assets/index.ts`, `src/components/ui/tooltip.tsx`; external `next/image`, `lucide-react`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/components/features/dedicated-server/application.tsx`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/components/features/dedicated-server/dedicated-server-page.tsx`

- **Why it exists / responsibility:** Dedicated Server feature module responsible for the dedicated server page portion of its screen or workflow.
- **Runtime and size:** Client module; 56 lines.
- **Exports / surface:** `DedicatedServerPage`, `DedicatedServerPage`, `renderContent`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** `useUserInfo`, `useState`, `useDsAppQuery`; state bindings `activeTab`, `setActiveTab`; contexts None.
- **Dependencies:** internal `src/components/shared/tabs.tsx`, `src/components/features/dedicated-server/application.tsx`, `src/components/features/utilities/multiplayer-app-monitor.tsx`, `src/components/ui/headlines/page-title.tsx`, `src/store/api/asset.ts`, `src/hooks/use-user-info.ts`, `src/constant/asset.ts`, `src/components/shared/loader.tsx`; external `react`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/app/(withSidebarLayout)/multiplayer-server/page.tsx`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/components/features/dedicated-server/dedicated-server-upload-details.tsx`

- **Why it exists / responsibility:** Dedicated Server feature module responsible for the dedicated server upload details portion of its screen or workflow.
- **Runtime and size:** Client-bound through an importing client module; 35 lines.
- **Exports / surface:** `DedicatedServerUploadDetails`, `DedicatedServerUploadDetails`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** `useDedicatedServerUpload`; state bindings None; contexts None.
- **Dependencies:** internal `src/components/ui/progress.tsx`, `src/lib/utils.ts`, `src/utils/size-formatter.ts`, `src/components/providers/upload/dedicated-server-upload-provider.tsx`, `src/utils/upload.ts`; external `lucide-react`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/components/features/dedicated-server/dedicated-server-app-upload.tsx`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/components/features/developer-section/api-key-tab.tsx`

- **Why it exists / responsibility:** Developer Section feature module responsible for the api key tab portion of its screen or workflow.
- **Runtime and size:** Client module; 76 lines.
- **Exports / surface:** `ApiKeyTab`, `ApiKeyTab`, `handleGenerate`, `copyHandler`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** `useApiKeyQuery`, `useGenerateApiKeyMutation`, `useState`; state bindings `openConfirmDialog`, `setOpenConfirmDialog`; contexts None.
- **Dependencies:** internal `src/components/ui/button.tsx`, `src/components/ui/label.tsx`, `src/components/shared/dialog/alert-dialog.tsx`, `src/components/ui/field/text-area.tsx`, `src/store/api/api-key.ts`, `src/components/shared/tooltip-info.tsx`, `src/utils/common.ts`; external `react`, `sonner`, `lucide-react`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/components/features/developer-section/developer-section-page.tsx`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/components/features/developer-section/developer-section-page.tsx`

- **Why it exists / responsibility:** Developer Section feature module responsible for the developer section page portion of its screen or workflow.
- **Runtime and size:** Client module; 83 lines.
- **Exports / surface:** `DeveloperSectionPage`, `DeveloperSectionPage`, `renderContent`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** `useState`, `useUserInfo`, `useRouter`, `useEffect`; state bindings `activeTab`, `setActiveTab`; contexts None.
- **Dependencies:** internal `src/components/shared/tabs.tsx`, `src/components/features/developer-section/version-control-tab.tsx`, `src/components/features/developer-section/token-generation-tab.tsx`, `src/components/features/developer-section/streaming-api-key-tab.tsx`, `src/components/features/developer-section/api-key-tab.tsx`, `src/components/ui/headlines/page-title.tsx`, `src/components/features/developer-section/patch-file-upload.tsx`, `src/config/page.ts`, plus 3 more; external `react`, `next/navigation`, `lucide-react`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/app/(withSidebarLayout)/developer-section/page.tsx`.
- **Improvement / caution:** Keep effect dependencies complete and verify cleanup under React Strict Mode.

#### `src/components/features/developer-section/patch-file-upload.tsx`

- **Why it exists / responsibility:** Developer Section feature module responsible for the patch file upload portion of its screen or workflow.
- **Runtime and size:** Client-bound through an importing client module; 9 lines.
- **Exports / surface:** `PatchFile`, `PatchFile`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** None; state bindings None; contexts None.
- **Dependencies:** internal `src/components/shared/tooltip-info.tsx`, `src/components/ui/button.tsx`; external None.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/components/features/developer-section/developer-section-page.tsx`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/components/features/developer-section/streaming-api-key-tab.tsx`

- **Why it exists / responsibility:** Developer Section feature module responsible for the streaming api key tab portion of its screen or workflow.
- **Runtime and size:** Client module; 75 lines.
- **Exports / surface:** `StreamingApiKeyTab`, `StreamingApiKeyTab`, `handleGenerate`, `copyHandler`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** `useApiKeyQuery`, `useGenerateApiKeyMutation`, `useState`; state bindings `openConfirmDialog`, `setOpenConfirmDialog`; contexts None.
- **Dependencies:** internal `src/components/ui/button.tsx`, `src/components/ui/label.tsx`, `src/store/api/api-key.ts`, `src/components/shared/dialog/alert-dialog.tsx`, `src/components/ui/field/text-area.tsx`, `src/components/shared/tooltip-info.tsx`, `src/utils/common.ts`; external `react`, `lucide-react`, `sonner`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/components/features/developer-section/developer-section-page.tsx`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/components/features/developer-section/token-generation-tab.tsx`

- **Why it exists / responsibility:** Developer Section feature module responsible for the token generation tab portion of its screen or workflow.
- **Runtime and size:** Client module; 99 lines.
- **Exports / surface:** `TokenGenerationTab`, `TokenGenerationTab`, `handleGenerate`, `copyHandler`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** `useGenerateTokenMutation`, `useLoginStatusQuery`, `useState`; state bindings `jsonInput`, `setJsonInput`, `expiryMinutes`, `setExpiryMinutes`, `token`, `setToken`; contexts None.
- **Dependencies:** internal `src/components/ui/button.tsx`, `src/components/ui/label.tsx`, `src/components/ui/textarea.tsx`, `src/store/api/api-key.ts`, `src/components/ui/field/text-area.tsx`, `src/components/ui/field/custom-input.tsx`, `src/store/api/auth.ts`, `src/components/shared/tooltip-info.tsx`, plus 1 more; external `react`, `lucide-react`, `sonner`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/components/features/developer-section/developer-section-page.tsx`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/components/features/developer-section/version-control-tab.tsx`

- **Why it exists / responsibility:** Developer Section feature module responsible for the version control tab portion of its screen or workflow.
- **Runtime and size:** Client-bound through an importing client module; 85 lines.
- **Exports / surface:** `VersionControlTab`, `VersionControlTab`, `handleSwitchClick`, `handleVersionControl`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** `useUserInfo`, `useUserAllInfoQuery`, `useUpdateVersionControlMutation`, `useState`; state bindings `openConfirmDialog`, `setOpenConfirmDialog`, `proposedStatus`, `setProposedStatus`; contexts None.
- **Dependencies:** internal `src/components/ui/button.tsx`, `src/components/ui/switch.tsx`, `src/store/api/api-key.ts`, `src/components/shared/dialog/alert-dialog.tsx`, `src/store/api/user-info.ts`, `src/hooks/use-user-info.ts`; external `react`, `lucide-react`, `sonner`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/components/features/developer-section/developer-section-page.tsx`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/components/features/my-account/my-account.tsx`

- **Why it exists / responsibility:** My Account feature module responsible for the my account portion of its screen or workflow.
- **Runtime and size:** Client module; 57 lines.
- **Exports / surface:** `MyAccount`, `MyAccount({
  modalOpen,
  setModalOpen,
  activeTab: propActiveTab,
  setActiveTab: propSetActiveTab
}: AccountModalProps)`, `backHandler`.
- **Props / inputs visible at declarations:** `MyAccount: {
  modalOpen,
  setModalOpen,
  activeTab: propActiveTab,
  setActiveTab: propSetActiveTab
}: AccountModalProps`.
- **Hooks / local state:** `useState`; state bindings `internalTab`, `setInternalTab`; contexts None.
- **Dependencies:** internal `src/components/shared/tabs.tsx`, `src/components/features/my-account/profile-settings.tsx`, `src/components/features/subscription/subscription.tsx`, `src/components/features/my-account/transaction.tsx`, `src/components/shared/dialog/custom-dialog.tsx`, `src/components/ui/dialog.tsx`, `src/components/shared/dialog/dialog-header.tsx`; external `react`, `lucide-react`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/components/features/subscription/new-subscription-summary/core-plan.tsx`, `src/components/features/subscription/new-subscription-summary/pause-plan.tsx`, `src/components/features/subscription/subscription-summary/cancel-subscription.tsx`, `src/components/features/subscription/subscription-summary/ppccu-subscription.tsx`, `src/components/features/subscription/subscription-summary/ppl-subscription.tsx`, `src/components/features/subscription/subscription-summary/ppm-subscription.tsx`, `src/components/features/subscription/subscription-summary/prepaid-minute.tsx`, `src/components/navbar/header.tsx`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/components/features/my-account/profile-settings.tsx`

- **Why it exists / responsibility:** My Account feature module responsible for the profile settings portion of its screen or workflow.
- **Runtime and size:** Client module; 537 lines.
- **Exports / surface:** `ProfileSettings`, `ProfileSettings`, `handleInputChange(field: keyof typeof formData, value: string)`, `handleEdit(field: keyof typeof isEditing)`, `handleCancel(field: keyof typeof formData)`, `handleVerify(field: keyof typeof isEditing)`, `handleConfirmUsernameChange`, `handleDeleteUsername`, `handleMobileConfirm`, `handleConfirmEmailChange`, `handleResetPassword`, `handleAccount`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** `useRouter`, `useUserInfo`, `useProfileLogoQuery`, `useLogoutMutation`, `useResetPasswordMutation`, `useVerifyUserNameMutation`, `useVerifyEmailMutation`, `useChangeUsernameMutation`, `useChangeEmailMutation`, `useUpdatePhoneNumberMutation`, `useRemoveUsernameMutation`, `useDeleteAccountMutation`, plus 2 more; state bindings `isResetModalOpen`, `setIsResetModalOpen`, `isConfirmUsernameChangeOpen`, `setIsConfirmUsernameChangeOpen`, `isConfirmEmailChangeOpen`, `setIsConfirmEmailChangeOpen`, `isConfirmPhoneChangeOpen`, `setIsConfirmPhoneChangeOpen`, `isConfirmDeleteUsernameOpen`, `setIsConfirmDeleteUsernameOpen`, `isConfirmDeleteAccountOpen`, `setIsConfirmDeleteAccountOpen`, plus 12 more; contexts None.
- **Dependencies:** internal `src/components/ui/button.tsx`, `src/components/ui/card.tsx`, `src/components/ui/input.tsx`, `src/components/ui/label.tsx`, `src/components/ui/avatar.tsx`, `src/hooks/use-user-info.ts`, `src/store/api/user-info.ts`, `src/assets/index.ts`, plus 7 more; external `react`, `next/navigation`, `lucide-react`, `react-phone-number-input`, `react-phone-number-input/style.css`, `sonner`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/components/features/my-account/my-account.tsx`.
- **Improvement / caution:** Decompose by responsibility and add focused tests before changing behavior.

#### `src/components/features/my-account/transaction.tsx`

- **Why it exists / responsibility:** My Account feature module responsible for the transaction portion of its screen or workflow.
- **Runtime and size:** Client module; 136 lines.
- **Exports / surface:** `Transaction`, `Transaction`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** `useUserInfo`, `useInvoiceListQuery`, `useMemo`; state bindings None; contexts None.
- **Dependencies:** internal `src/hooks/use-user-info.ts`, `src/store/api/payment.ts`, `src/components/shared/common-table.tsx`, `src/components/ui/button.tsx`, `src/utils/date-formatter.ts`, `src/config/doc.ts`, `src/components/shared/doc-link.tsx`; external `react`, `@tanstack/react-table`, `lucide-react`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/components/features/my-account/my-account.tsx`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/components/features/my-account/upload-profile.tsx`

- **Why it exists / responsibility:** My Account feature module responsible for the upload profile portion of its screen or workflow.
- **Runtime and size:** Client-bound through an importing client module; 182 lines.
- **Exports / surface:** `UploadProfile`, `UploadProfile`, `handleFileSelect(event: React.ChangeEvent<HTMLInputElement>)`, `handleFileChange(selectedFile: File)`, `handleDragOver(e: React.DragEvent)`, `handleDrop(e: React.DragEvent)`, `triggerFileInput`, `handleUploadClick`, `handleRemoveFile`, `handleCloseDialog`, `handleConfirmUpload`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** `useUserInfo`, `useUploadCompleteMutation`, `useState`, `useRef`, `useCommonUpload`; state bindings `isUploadDialogOpen`, `setIsUploadDialogOpen`, `previewUrl`, `setPreviewUrl`; contexts None.
- **Dependencies:** internal `src/components/ui/button.tsx`, `src/components/shared/dialog/alert-dialog.tsx`, `src/hooks/use-common-upload.ts`, `src/constant/asset.ts`, `src/store/api/user-info.ts`, `src/hooks/use-user-info.ts`; external `react`, `lucide-react`, `sonner`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/components/features/my-account/profile-settings.tsx`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/components/features/new-subscription/core-plan-montly-storage.tsx`

- **Why it exists / responsibility:** New Subscription feature module responsible for the core plan montly storage portion of its screen or workflow.
- **Runtime and size:** Client-bound through an importing client module; 307 lines.
- **Exports / surface:** `CorePlanMontlyStorage`, `CorePlanMontlyStorage`, `handleAdjustStorageClick`, `handleStorageModalClose`, `upgradeStorageHandler`, `handleReactiveClick(type: 'gb')`, `handleReactiveSubscription`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** `useUserInfo`, `useCardDetailsV2`, `useHasPauseSubscription`, `useSubscriptionV2`, `useStorageV2`, `useUpgradeSubscriptionMutation`, `useDowngradeSubscriptionMutation`, `useNonCheckoutCreateGbSubscriptionMutation`, `useUnPauseSubscriptionMutation`, `useState`, `useEffect`, `useDataQuoteQuery`; state bindings `storage`, `setStorage`, `isStorageModalOpen`, `setIsStorageModalOpen`, `shouldFetchStorageQuote`, `setShouldFetchStorageQuote`, `isReactiveDialogOpen`, `setReactiveDialogOpen`; contexts None.
- **Dependencies:** internal `src/components/ui/button.tsx`, `src/components/ui/slider.tsx`, `src/hooks/use-user-info.ts`, `src/store/api/payment.ts`, `src/components/shared/dialog/alert-dialog.tsx`, `src/config/page.ts`, `src/hooks/use-storageV2.ts`, `src/hooks/use-subscription-v2.ts`, plus 2 more; external `react`, `lucide-react`, `sonner`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/components/features/new-subscription/core-plan-montly.tsx`.
- **Improvement / caution:** Keep effect dependencies complete and verify cleanup under React Strict Mode.

#### `src/components/features/new-subscription/core-plan-montly.tsx`

- **Why it exists / responsibility:** New Subscription feature module responsible for the core plan montly portion of its screen or workflow.
- **Runtime and size:** Client-bound through an importing client module; 464 lines.
- **Exports / surface:** `CorePlanMontly`, `CorePlanMontly`, `handleAdjustMinutesClick`, `handleModalClose`, `upgradeMinHandler`, `handleCancelReasonConfirm(reason: string)`, `handleCancelSub`, `handleReactiveClick(type: 'min' | 'core')`, `handleReactiveSubscription`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** `useUserInfo`, `useCardDetailsV2`, `useHasPauseSubscription`, `useSubscriptionV2`, `useMinutesV2`, `useNonCheckoutCreateMinSubscriptionMutation`, `useCreateSubscriptionPlanMutation`, `useUpgradeSubscriptionMutation`, `useDowngradeSubscriptionMutation`, `useState`, `useCancelAllSubMutation`, `useUnPauseSubscriptionMutation`, plus 2 more; state bindings `isCancelReasonOpen`, `setCancelReasonOpen`, `isCancelDialogOpen`, `setCancelDialogOpen`, `cancelReason`, `setCancelReason`, `isReactiveDialogOpen`, `setReactiveDialogOpen`, `reactiveType`, `setReactiveType`, `minutes`, `setMinutes`, plus 4 more; contexts None.
- **Dependencies:** internal `src/components/shared/doc-link.tsx`, `src/components/ui/button.tsx`, `src/components/ui/card.tsx`, `src/config/doc.ts`, `src/components/ui/slider.tsx`, `src/hooks/use-subscription-v2.ts`, `src/store/api/payment.ts`, `src/components/features/new-subscription/new-billing.tsx`, plus 12 more; external `react`, `lucide-react`, `sonner`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/components/features/subscription/subscription.tsx`.
- **Improvement / caution:** Consider a reducer/state machine to make transitions explicit.

#### `src/components/features/new-subscription/core-plan-yearly-storage.tsx`

- **Why it exists / responsibility:** New Subscription feature module responsible for the core plan yearly storage portion of its screen or workflow.
- **Runtime and size:** Client-bound through an importing client module; 308 lines.
- **Exports / surface:** `CorePlanYearlyStorage`, `CorePlanYearlyStorage`, `handleAdjustStorageClick`, `handleStorageModalClose`, `upgradeStorageHandler`, `getStoragePriceDisplay`, `handleReactiveClick`, `handleReactiveSubscription`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** `useUserInfo`, `useCardDetailsV2`, `useSubscriptionV2`, `useStorageV2`, `useUpgradeSubscriptionMutation`, `useDowngradeSubscriptionMutation`, `useHasPauseSubscription`, `useUnPauseSubscriptionMutation`, `useNonCheckoutCreateGbSubscriptionMutation`, `useState`, `useEffect`, `useDataQuoteQuery`; state bindings `storage`, `setStorage`, `isStorageModalOpen`, `setIsStorageModalOpen`, `shouldFetchStorageQuote`, `setShouldFetchStorageQuote`, `isReactiveDialogOpen`, `setReactiveDialogOpen`; contexts None.
- **Dependencies:** internal `src/components/ui/button.tsx`, `src/components/ui/slider.tsx`, `src/hooks/use-user-info.ts`, `src/store/api/payment.ts`, `src/components/shared/dialog/alert-dialog.tsx`, `src/config/page.ts`, `src/hooks/use-storageV2.ts`, `src/hooks/use-subscription-v2.ts`, plus 2 more; external `react`, `lucide-react`, `sonner`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/components/features/new-subscription/core-plan-yearly.tsx`.
- **Improvement / caution:** Keep effect dependencies complete and verify cleanup under React Strict Mode.

#### `src/components/features/new-subscription/core-plan-yearly.tsx`

- **Why it exists / responsibility:** New Subscription feature module responsible for the core plan yearly portion of its screen or workflow.
- **Runtime and size:** Client-bound through an importing client module; 473 lines.
- **Exports / surface:** `CorePlanYearly`, `CorePlanYearly`, `handleAdjustMinutesClick`, `handleModalClose`, `handleCancelReasonConfirm(reason: string)`, `handleCancelSub`, `upgradeMinHandler`, `getMinPriceDisplay`, `handleReactiveClick(type: 'min' | 'core')`, `handleReactiveSubscription`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** `useUserInfo`, `useCardDetailsV2`, `useSubscriptionV2`, `useHasPauseSubscription`, `useUnPauseSubscriptionMutation`, `useMinutesV2`, `useUpgradeSubscriptionMutation`, `useDowngradeSubscriptionMutation`, `useNonCheckoutCreateMinSubscriptionMutation`, `useState`, `useCancelAllSubMutation`, `useEffect`, plus 1 more; state bindings `minutes`, `setMinutes`, `isMinModalOpen`, `setIsMinModalOpen`, `shouldFetchQuote`, `setShouldFetchQuote`, `isCancelReasonOpen`, `setCancelReasonOpen`, `isCancelDialogOpen`, `setCancelDialogOpen`, `cancelReason`, `setCancelReason`, plus 4 more; contexts None.
- **Dependencies:** internal `src/components/shared/doc-link.tsx`, `src/components/ui/button.tsx`, `src/components/ui/card.tsx`, `src/config/doc.ts`, `src/components/ui/slider.tsx`, `src/hooks/use-subscription-v2.ts`, `src/store/api/payment.ts`, `src/components/features/new-subscription/new-billing.tsx`, plus 12 more; external `console`, `react`, `lucide-react`, `sonner`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/components/features/subscription/subscription.tsx`.
- **Improvement / caution:** Consider a reducer/state machine to make transitions explicit.

#### `src/components/features/new-subscription/new-billing.tsx`

- **Why it exists / responsibility:** New Subscription feature module responsible for the new billing portion of its screen or workflow.
- **Runtime and size:** Client-bound through an importing client module; 60 lines.
- **Exports / surface:** `NewBilling`, `NewBilling`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** `useSubscriptionV2`, `useStorageV2`, `useMinutesV2`; state bindings None; contexts None.
- **Dependencies:** internal `src/components/ui/card.tsx`, `src/components/ui/separator.tsx`, `src/hooks/use-subscription-v2.ts`, `src/hooks/use-storageV2.ts`, `src/hooks/use-minute-v2.ts`, `src/utils/date-formatter.ts`, `src/components/features/new-subscription/new-payment-info.tsx`; external None.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/components/features/new-subscription/core-plan-montly.tsx`, `src/components/features/new-subscription/core-plan-yearly.tsx`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/components/features/new-subscription/new-payment-info.tsx`

- **Why it exists / responsibility:** New Subscription feature module responsible for the new payment info portion of its screen or workflow.
- **Runtime and size:** Client-bound through an importing client module; 107 lines.
- **Exports / surface:** `NewPaymentInfo`, `NewPaymentInfo`, `handleConfirm`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** `useEditPaymentForCoreMutation`, `useUserInfo`, `useSubscriptionV2`, `useHasPauseSubscription`, `useCardDetailsV2`, `useState`; state bindings `isConfirmDialogOpen`, `setIsConfirmDialogOpen`; contexts None.
- **Dependencies:** internal `src/components/ui/button.tsx`, `src/components/shared/dialog/alert-dialog.tsx`, `src/hooks/use-user-info.ts`, `src/config/page.ts`, `src/components/shared/doc-link.tsx`, `src/config/doc.ts`, `src/store/api/payment.ts`, `src/hooks/use-card-details-v2.ts`, plus 3 more; external `react`, `lucide-react`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/components/features/new-subscription/new-billing.tsx`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/components/features/new-subscription/new-trial.tsx`

- **Why it exists / responsibility:** New Subscription feature module responsible for the new trial portion of its screen or workflow.
- **Runtime and size:** Client-bound through an importing client module; 117 lines.
- **Exports / surface:** `NewTrial`, `NewTrial`, `handleBuyClick`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** `useTrialV2`; state bindings None; contexts None.
- **Dependencies:** internal `src/components/shared/doc-link.tsx`, `src/components/ui/button.tsx`, `src/components/ui/card.tsx`, `src/config/doc.ts`, `src/components/shared/loader.tsx`, `src/config/page.ts`, `src/hooks/use-trial-v2.ts`, `src/components/ui/progress.tsx`; external `next/navigation`, `lucide-react`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/components/features/subscription/subscription-summary/subscription-summary-section.tsx`, `src/components/features/subscription/subscription.tsx`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/components/features/new-subscription/terminated-new-subscription.tsx`

- **Why it exists / responsibility:** New Subscription feature module responsible for the terminated new subscription portion of its screen or workflow.
- **Runtime and size:** Client-bound through an importing client module; 4 lines.
- **Exports / surface:** `TerminatedNewSubscription`, `TerminatedNewSubscription`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** None; state bindings None; contexts None.
- **Dependencies:** internal None; external None.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** None; static analysis found no internal importer, so verify framework entry/dynamic use.
- **Improvement / caution:** Confirm whether this is a framework root, planned module, or dead code before retaining it.

#### `src/components/features/plan/dialog/core-plan.tsx`

- **Why it exists / responsibility:** Plan feature module responsible for the core plan portion of its screen or workflow.
- **Runtime and size:** Client module; 64 lines.
- **Exports / surface:** `CorePlan`, `CorePlan({ open, onClose, billingCycle }: CorePlanModalProps)`, `handleConfirm`.
- **Props / inputs visible at declarations:** `CorePlan: { open, onClose, billingCycle }: CorePlanModalProps`.
- **Hooks / local state:** `useUserInfo`, `useCreateSubscriptionPlanMutation`; state bindings None; contexts None.
- **Dependencies:** internal `src/components/ui/label.tsx`, `src/components/shared/dialog/alert-dialog.tsx`, `src/config/page.ts`, `src/hooks/use-user-info.ts`, `src/store/api/payment.ts`, `src/hooks/use-subscription-v2.ts`; external `sonner`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/components/features/plan/new-pricing-card.tsx`, `src/components/features/plan/new-pricing.tsx`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/components/features/plan/dialog/pccu.tsx`

- **Why it exists / responsibility:** Plan feature module responsible for the pccu portion of its screen or workflow.
- **Runtime and size:** Client module; 127 lines.
- **Exports / surface:** `Ppccu`, `Ppccu({ open, onClose }: PpccuPlanDialogProps)`, `incrementCcu`, `decrementCcu`, `handleConfirm`.
- **Props / inputs visible at declarations:** `Ppccu: { open, onClose }: PpccuPlanDialogProps`.
- **Hooks / local state:** `useSubscription`, `useUserInfo`, `useCreatePpccuSubMutation`, `useState`; state bindings `ccuCount`, `setCcuCount`; contexts None.
- **Dependencies:** internal `src/components/ui/label.tsx`, `src/components/ui/button.tsx`, `src/components/shared/dialog/alert-dialog.tsx`, `src/store/api/api-key.ts`, `src/store/api/user-info.ts`, `src/store/api/auth.ts`, `src/store/api/subscription.ts`, `src/hooks/use-subscription.ts`, plus 2 more; external `react`, `lucide-react`, `sonner`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/components/features/plan/plan-page.tsx`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/components/features/plan/dialog/ppl.tsx`

- **Why it exists / responsibility:** Plan feature module responsible for the ppl portion of its screen or workflow.
- **Runtime and size:** Client module; 129 lines.
- **Exports / surface:** `Ppl`, `Ppl({ open, onClose }: PplPlanDialogProps)`, `incrementLicense`, `decrementLicense`, `handleConfirm`.
- **Props / inputs visible at declarations:** `Ppl: { open, onClose }: PplPlanDialogProps`.
- **Hooks / local state:** `useSubscription`, `useUserInfo`, `useCreatePplSubMutation`, `useState`; state bindings `licenseCount`, `setLicenseCount`; contexts None.
- **Dependencies:** internal `src/components/ui/label.tsx`, `src/components/ui/button.tsx`, `src/components/shared/dialog/alert-dialog.tsx`, `src/hooks/use-subscription.ts`, `src/store/api/api-key.ts`, `src/store/api/auth.ts`, `src/store/api/user-info.ts`, `src/store/api/subscription.ts`, plus 2 more; external `react`, `lucide-react`, `sonner`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/components/features/plan/plan-page.tsx`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/components/features/plan/dialog/ppm.tsx`

- **Why it exists / responsibility:** Plan feature module responsible for the ppm portion of its screen or workflow.
- **Runtime and size:** Client module; 77 lines.
- **Exports / surface:** `Ppm`, `Ppm({ open, onClose }: PplPlanDialogProps)`, `handleConfirm`.
- **Props / inputs visible at declarations:** `Ppm: { open, onClose }: PplPlanDialogProps`.
- **Hooks / local state:** `useSubscription`, `useSetupCardMutation`, `useApiKeyQuery`, `useState`, `useCustomerId`; state bindings `promoCode`, `setPromoCode`; contexts None.
- **Dependencies:** internal `src/components/ui/label.tsx`, `src/components/shared/dialog/alert-dialog.tsx`, `src/hooks/use-subscription.ts`, `src/store/api/api-key.ts`, `src/components/ui/input.tsx`, `src/hooks/use-customer-id.ts`, `src/config/page.ts`, `src/store/api/credit-card.ts`; external `react`, `sonner`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/components/features/plan/plan-page.tsx`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/components/features/plan/dialog/prepaid-minute.tsx`

- **Why it exists / responsibility:** Plan feature module responsible for the prepaid minute portion of its screen or workflow.
- **Runtime and size:** Client module; 174 lines.
- **Exports / surface:** `PrepaidMinute`, `PrepaidMinute({ open, onClose }: MinutePlanDialogProps)`, `incrementMinutes`, `decrementMinutes`, `handleConfirm`.
- **Props / inputs visible at declarations:** `PrepaidMinute: { open, onClose }: MinutePlanDialogProps`.
- **Hooks / local state:** `useSubscription`, `useUserInfo`, `useCreatePrepaidMinuteSubMutation`, `useState`; state bindings `minutes`, `setMinutes`, `autoRenew`, `setAutoRenew`; contexts None.
- **Dependencies:** internal `src/components/ui/label.tsx`, `src/components/ui/radio-group.tsx`, `src/components/ui/tooltip.tsx`, `src/components/ui/button.tsx`, `src/components/shared/dialog/alert-dialog.tsx`, `src/hooks/use-subscription.ts`, `src/store/api/api-key.ts`, `src/store/api/user-info.ts`, plus 5 more; external `react`, `lucide-react`, `sonner`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/components/features/plan/plan-page.tsx`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/components/features/plan/faq.tsx`

- **Why it exists / responsibility:** Plan feature module responsible for the faq portion of its screen or workflow.
- **Runtime and size:** Client module; 305 lines.
- **Exports / surface:** `Faq`, `Faq`, `scrollToTop`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** `useState`; state bindings `openItem`, `setOpenItem`; contexts None.
- **Dependencies:** internal `src/assets/index.ts`, `src/components/ui/accordion.tsx`, `src/config/url.ts`; external `react`, `next/image`, `lucide-react`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/components/features/plan/features/all-pan.tsx`, `src/components/features/plan/testimonial.tsx`.
- **Improvement / caution:** Decompose by responsibility and add focused tests before changing behavior.

#### `src/components/features/plan/features/all-pan.tsx`

- **Why it exists / responsibility:** Plan feature module responsible for the all pan portion of its screen or workflow.
- **Runtime and size:** Client module; 72 lines.
- **Exports / surface:** `AllPan`, `AllPan`, `onScroll`, `handleScrollToggle`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** `useUserInfo`, `useState`, `useEffect`; state bindings `showScrollUp`, `setShowScrollUp`; contexts None.
- **Dependencies:** internal `src/hooks/use-user-info.ts`, `src/components/features/plan/faq.tsx`, `src/components/features/plan/new-pricing.tsx`, `src/components/features/plan/testimonial.tsx`, `src/components/features/plan/plan-page.tsx`; external `react`, `lucide-react`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/app/(withSidebarLayout)/plans/page.tsx`.
- **Improvement / caution:** Keep effect dependencies complete and verify cleanup under React Strict Mode.

#### `src/components/features/plan/features/features.tsx`

- **Why it exists / responsibility:** Plan feature module responsible for the features portion of its screen or workflow.
- **Runtime and size:** Client module; 448 lines.
- **Exports / surface:** `default`, `Features({
  onClose,
  onBuyNow
}: {
  onClose: () => void;
  onBuyNow?: (billingPeriod: 'monthly' | 'yearly') => void;
})`, `handleBuyNow`, `renderFeatureValue(value: any)`.
- **Props / inputs visible at declarations:** `Features: {
  onClose,
  onBuyNow
}: {
  onClose: () => void;
  onBuyNow?: (billingPeriod: 'monthly' | 'yearly') => void;
}`.
- **Hooks / local state:** `useState`; state bindings `billingPeriod`, `setBillingPeriod`; contexts None.
- **Dependencies:** internal `src/components/ui/tabs.tsx`, `src/components/ui/tooltip.tsx`, `src/config/url.ts`; external `react`, `lucide-react`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/components/features/plan/new-pricing-card.tsx`, `src/components/features/plan/new-pricing.tsx`.
- **Improvement / caution:** Decompose by responsibility and add focused tests before changing behavior.

#### `src/components/features/plan/new-pricing-card.tsx`

- **Why it exists / responsibility:** Plan feature module responsible for the new pricing card portion of its screen or workflow.
- **Runtime and size:** Client module; 167 lines.
- **Exports / surface:** `NewPricingCard`, `NewPricingCard({
  item,
  onCTA,
  disabled,
  onShowFeatures
}: {
  item: any;
  onCTA?: () => void;
  disabled?: boolean;
  onShowFeatures?: () => void;
})`.
- **Props / inputs visible at declarations:** `NewPricingCard: {
  item,
  onCTA,
  disabled,
  onShowFeatures
}: {
  item: any;
  onCTA?: () => void;
  disabled?: boolean;
  onShowFeatures?: () => void;
}`.
- **Hooks / local state:** None; state bindings None; contexts None.
- **Dependencies:** internal `src/components/ui/tabs.tsx`, `src/components/features/plan/dialog/core-plan.tsx`, `src/config/url.ts`, `src/assets/index.ts`, `src/components/shared/dialog/custom-dialog.tsx`, `src/components/features/plan/features/features.tsx`, `src/components/ui/dialog.tsx`, `src/components/shared/dialog/pricing-dialog.tsx`, plus 5 more; external `react`, `lucide-react`, `sonner`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/components/features/plan/new-pricing.tsx`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/components/features/plan/new-pricing.tsx`

- **Why it exists / responsibility:** Plan feature module responsible for the new pricing portion of its screen or workflow.
- **Runtime and size:** Client module; 180 lines.
- **Exports / surface:** `NewPrice`, `NewPrice`, `handleCTA(action: string, cycle?: 'monthly' | 'yearly')`, `handleShowFeatures`, `handleFeatureBuyNow(period: 'monthly' | 'yearly')`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** `useSubscription`, `useSubscriptionV2`, `useState`; state bindings `showCoreModal`, `setShowCoreModal`, `modalOpen`, `setModalOpen`, `billingCycle`, `setBillingCycle`; contexts None.
- **Dependencies:** internal `src/components/ui/tabs.tsx`, `src/components/features/plan/new-pricing-card.tsx`, `src/components/features/plan/dialog/core-plan.tsx`, `src/config/url.ts`, `src/assets/index.ts`, `src/components/shared/dialog/custom-dialog.tsx`, `src/components/features/plan/features/features.tsx`, `src/components/ui/dialog.tsx`, plus 3 more; external `react`, `sonner`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/components/features/plan/features/all-pan.tsx`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/components/features/plan/plan-page.tsx`

- **Why it exists / responsibility:** Plan feature module responsible for the plan page portion of its screen or workflow.
- **Runtime and size:** Client module; 174 lines.
- **Exports / surface:** `PlanPage`, `PlanPage`, `handleSelectPlan(planTitle: string)`, `handleCloseDialog`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** `useState`, `useUserInfo`, `useCheckStatusQuery`; state bindings `activeDialog`, `setActiveDialog`; contexts None.
- **Dependencies:** internal `src/app/fonts.ts`, `src/components/features/plan/pricing-card.tsx`, `src/constant/plan.ts`, `src/components/features/plan/dialog/prepaid-minute.tsx`, `src/components/features/plan/dialog/pccu.tsx`, `src/components/features/plan/dialog/ppl.tsx`, `src/config/url.ts`, `src/components/features/plan/dialog/ppm.tsx`, plus 2 more; external `react`, `next/link`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/components/features/plan/features/all-pan.tsx`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/components/features/plan/pricing-card.tsx`

- **Why it exists / responsibility:** Plan feature module responsible for the pricing card portion of its screen or workflow.
- **Runtime and size:** Client module; 116 lines.
- **Exports / surface:** `PricingCard`, `PricingCard({ plan, onButtonClick }: PricingCardProps)`.
- **Props / inputs visible at declarations:** `PricingCard: { plan, onButtonClick }: PricingCardProps`.
- **Hooks / local state:** `useSubscription`; state bindings None; contexts None.
- **Dependencies:** internal `src/components/ui/button.tsx`, `src/components/ui/card.tsx`, `src/components/ui/tooltip.tsx`, `src/lib/utils.ts`, `src/hooks/use-subscription.ts`, `src/config/doc.ts`; external `lucide-react`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/components/features/plan/plan-page.tsx`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/components/features/plan/testimonial-carousel.tsx`

- **Why it exists / responsibility:** Plan feature module responsible for the testimonial carousel portion of its screen or workflow.
- **Runtime and size:** Client module; 219 lines.
- **Exports / surface:** `TestimonialCarousel`, `TestimonialCarousel`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** `useState`; state bindings `showAllTestimonials`, `setShowAllTestimonials`; contexts None.
- **Dependencies:** internal None; external `react`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/components/features/plan/testimonial.tsx`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/components/features/plan/testimonial.tsx`

- **Why it exists / responsibility:** Plan feature module responsible for the testimonial portion of its screen or workflow.
- **Runtime and size:** Client-bound through an importing client module; 43 lines.
- **Exports / surface:** `Testimonial`, `Testimonial`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** None; state bindings None; contexts None.
- **Dependencies:** internal `src/assets/index.ts`, `src/components/features/plan/faq.tsx`, `src/components/features/plan/testimonial-carousel.tsx`; external `next/image`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/components/features/plan/features/all-pan.tsx`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/components/features/post-payment/payment-successful-page.tsx`

- **Why it exists / responsibility:** Post Payment feature module responsible for the payment successful page portion of its screen or workflow.
- **Runtime and size:** Client module; 99 lines.
- **Exports / surface:** `PaymentSuccessfulPage`, `PaymentSuccessfulPage`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** `useRouter`, `useSearchParams`, `useUserInfo`, `useUserAllInfoQuery`, `useAddWatcherMutation`, `useSaveCardMutation`, `useCreatePpmSubMutation`, `useEffect`; state bindings None; contexts None.
- **Dependencies:** internal `src/components/ui/button.tsx`, `src/constant/plan.ts`, `src/store/api/user-info.ts`, `src/store/api/subscription.ts`, `src/hooks/use-user-info.ts`, `src/config/page.ts`, `src/store/api/credit-card.ts`, `src/assets/index.ts`; external `react`, `next/navigation`, `next/image`, `lucide-react`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/app/(withSidebarLayout)/payment-successful/page.tsx`.
- **Improvement / caution:** Keep effect dependencies complete and verify cleanup under React Strict Mode.

#### `src/components/features/signin/signin-page.tsx`

- **Why it exists / responsibility:** Signin feature module responsible for the signin page portion of its screen or workflow.
- **Runtime and size:** Client module; 247 lines.
- **Exports / surface:** `SignInPage`, `SignInPage`, `handleSubmit(e: FormEvent<HTMLFormElement>)`, `googleLoginHandler`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** `useRouter`, `useSignInMutation`, `useGenerateCookieMutation`, `useState`, `useEffect`; state bindings `redirect`, `setRedirect`, `email`, `setEmail`, `password`, `setPassword`, `error`, `setError`, `isProcessing`, `setIsProcessing`; contexts None.
- **Dependencies:** internal `src/components/ui/button.tsx`, `src/components/ui/field/custom-input.tsx`, `src/store/api/auth.ts`, `src/config/page.ts`, `src/config/firebase.ts`, `src/assets/index.ts`, `src/utils/validation.ts`, `src/utils/super-admin-login.ts`; external `react`, `next/navigation`, `next/link`, `next/image`, `firebase/auth`, `lucide-react`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/app/(with-public-layout)/signin/page.tsx`.
- **Improvement / caution:** Consider a reducer/state machine to make transitions explicit.

#### `src/components/features/singup/signup-page.tsx`

- **Why it exists / responsibility:** Singup feature module responsible for the signup page portion of its screen or workflow.
- **Runtime and size:** Client module; 522 lines.
- **Exports / surface:** `SignupPage`, `StatusIcon({
  field,
  loading,
  error,
  focusedField
})`, `SignupPage`, `emailVerify`, `passwordContinueHandler`, `usernameHandler`, `verifyOTPHandler`, `sendOTPHandler`, `googleSignUpHandler`, `debouncedEmailApiCheck(val: string)`, `debouncedUsernameApiCheck(val: string)`, `handleEmailChange(e: React.ChangeEvent<HTMLInputElement>)`, plus 1 more.
- **Props / inputs visible at declarations:** `StatusIcon: {
  field,
  loading,
  error,
  focusedField
}`.
- **Hooks / local state:** `useRouter`, `useVerifyEmailMutation`, `useVerifyUserNameMutation`, `useSendOtpMutation`, `useVerifyOtpMutation`, `useSignUpMutation`, `useSignInMutation`, `useCreateConfigMutation`, `useApiKeyQuery`, `useGenerateCookieMutation`, `useChangeUsernameMutation`, `useState`, plus 1 more; state bindings `step`, `setStep`, `focusedField`, `setFocusedField`, `email`, `setEmail`, `password`, `setPassword`, `username`, `setUsername`, `otp`, `setOtp`, plus 12 more; contexts None.
- **Dependencies:** internal `src/components/ui/button.tsx`, `src/components/ui/input.tsx`, `src/components/ui/alert.tsx`, `src/components/ui/separator.tsx`, `src/components/ui/input-otp.tsx`, `src/assets/index.ts`, `src/store/api/auth.ts`, `src/app/fonts.ts`, plus 8 more; external `react`, `next/image`, `next/navigation`, `lucide-react`, `sonner`, `firebase/auth`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/app/(with-public-layout)/signup/page.tsx`.
- **Improvement / caution:** Decompose by responsibility and add focused tests before changing behavior.

#### `src/components/features/streaming-app/api-notify.ts`

- **Why it exists / responsibility:** Streaming App feature module responsible for the api notify portion of its screen or workflow.
- **Runtime and size:** Shared/build-time eligible module; 275 lines.
- **Exports / surface:** `generateUUID`, `getTimes`, `getLocationDetails`, `telegramNotify`, `createUploadLog`, `uploadCompleteCall`, `isValidIPv4(ip: unknown)`, `generateUUID`, `getTimes(startTime: Date | string | number, endTime: Date | string | number)`, `getGeoLocation`, `getLocationDetails(ip: string)`, `telegramNotify(username: string, appName: string = '', file: FileWithDates, singnedUrlRes: SignedUrlResponse, assetType: string, failureMessage: string | null | boolean, uploadProgress: number, uploadStartTime: Date, uploadEndTime: Date)`, plus 2 more.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** None; state bindings None; contexts None.
- **Dependencies:** internal `src/config/api.ts`; external `axios`.
- **Rendering and rerenders:** No JSX detected; it runs when imported or when an exported function/hook is called.
- **Direct consumers:** `src/components/features/streaming-app/video-upload.tsx`, `src/components/providers/upload/additional-upload-provider.tsx`, `src/components/providers/upload/dedicated-server-upload-provider.tsx`, `src/components/providers/upload/upload-sequence-provider.tsx`, `src/hooks/use-common-upload.ts`.
- **Improvement / caution:** Confirm cancellation, error normalization, and cache synchronization for direct Axios traffic.

#### `src/components/features/streaming-app/application.tsx`

- **Why it exists / responsibility:** Streaming App feature module responsible for the application portion of its screen or workflow.
- **Runtime and size:** Client-bound through an importing client module; 581 lines.
- **Exports / surface:** `Application`, `Application`, `normalizeAppName(value: string)`, `fetchBuildTypes`, `handleCreateStreamLink`, `handleCreateMeetingLink`, `handleDeleteApp(appName: string)`, `hasLinks`, `renderContent`, `backHandler`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** `useUserInfo`, `useSubscription`, `useSubscriptionV2`, `useMinutesV2`, `useStorageV2`, `useTrialV2`, `useUploadSequence`, `useStreamingApp`, `useStreamingAppQuery`, `useStreamingAppThumbnailQuery`, `useSaveAppUrlMutation`, `useDeleteStreamingAppMutation`, plus 7 more; state bindings `addAppModalOpen`, `setAddAppModalOpen`, `cardModalOpen`, `setCardModalOpen`, `deleteDialogOpen`, `setDeleteDialogOpen`, `appToDelete`, `setAppToDelete`, `selectedApp`, `setSelectedApp`, `searchTerm`, `setSearchTerm`, plus 8 more; contexts None.
- **Dependencies:** internal `src/hooks/use-streaming-app.ts`, `src/store/api/asset.ts`, `src/components/features/streaming-app/streaming-app-card.tsx`, `src/components/ui/input.tsx`, `src/components/shared/dialog/custom-dialog.tsx`, `src/components/shared/dialog/dialog-header.tsx`, `src/components/ui/dialog.tsx`, `src/components/ui/button.tsx`, plus 29 more; external `react`, `react-hook-form`, `uuid`, `lucide-react`, `firebase/firestore`, `firebase/app`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/components/features/streaming-app/streaming-app-section.tsx`.
- **Improvement / caution:** Consider a reducer/state machine to make transitions explicit.

#### `src/components/features/streaming-app/config/common-tab.tsx`

- **Why it exists / responsibility:** Streaming App feature module responsible for the common tab portion of its screen or workflow.
- **Runtime and size:** Client module; 220 lines.
- **Exports / surface:** `CommonTab`, `CommonTab`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** `useFormContext`, `useState`, `useEffect`; state bindings `showPassword`, `setShowPassword`; contexts None.
- **Dependencies:** internal `src/components/ui/form.tsx`, `src/components/ui/input.tsx`, `src/components/ui/label.tsx`, `src/components/ui/select.tsx`, `src/components/ui/switch.tsx`, `src/components/features/streaming-app/config/shared-form.tsx`, `src/components/features/streaming-app/config/types.ts`, `src/components/shared/doc-link.tsx`, plus 1 more; external `react`, `react-hook-form`, `lucide-react`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/components/features/streaming-app/config/config-settings-form.tsx`.
- **Improvement / caution:** Keep effect dependencies complete and verify cleanup under React Strict Mode.

#### `src/components/features/streaming-app/config/config-settings-form.tsx`

- **Why it exists / responsibility:** Streaming App feature module responsible for the config settings form portion of its screen or workflow.
- **Runtime and size:** Client module; 173 lines.
- **Exports / surface:** `ConfigSettingsForm`, `ConfigSettingsForm({ formInstance }: ConfigSettingsFormProps)`, `onSubmit(formData)`, `handleTabChange(key: SettingsTab)`, `renderContent`.
- **Props / inputs visible at declarations:** `ConfigSettingsForm: { formInstance }: ConfigSettingsFormProps`.
- **Hooks / local state:** `useUserInfo`, `useConfigContext`, `useGetConfigQuery`, `useEditConfigMutation`, `useState`, `useEffect`; state bindings `isAdvanced`, `setIsAdvanced`, `activeTab`, `setActiveTab`, `isSaveModalOpen`, `setIsSaveModalOpen`; contexts None.
- **Dependencies:** internal `src/components/ui/button.tsx`, `src/components/ui/label.tsx`, `src/components/ui/switch.tsx`, `src/components/shared/tabs.tsx`, `src/components/providers/config-provider.tsx`, `src/store/api/appLink.ts`, `src/hooks/use-user-info.ts`, `src/components/features/streaming-app/config/common-tab.tsx`, plus 5 more; external `react`, `lucide-react`, `react-hook-form`, `sonner`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/components/features/streaming-app/application.tsx`.
- **Improvement / caution:** Keep effect dependencies complete and verify cleanup under React Strict Mode.

#### `src/components/features/streaming-app/config/customization-tab.tsx`

- **Why it exists / responsibility:** Streaming App feature module responsible for the customization tab portion of its screen or workflow.
- **Runtime and size:** Client module; 262 lines.
- **Exports / surface:** `CustomizationTab`, `CustomizationTab`, `handleRemoveAsset(fieldName: keyof FormValues)`, `handleRemoveVideo(videoUrl: string)`, `renderImagePreview`, `renderVideoPreview`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** `useConfigContext`, `useState`, `useFormContext`; state bindings `customizationType`, `setCustomizationType`; contexts None.
- **Dependencies:** internal `src/components/features/streaming-app/config/shared-form.tsx`, `src/components/ui/label.tsx`, `src/components/ui/form.tsx`, `src/components/ui/button.tsx`, `src/components/providers/config-provider.tsx`, `src/components/features/streaming-app/config/types.ts`, `src/config/doc.ts`, `src/components/shared/doc-link.tsx`; external `react`, `lucide-react`, `react-hook-form`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/components/features/streaming-app/config/config-settings-form.tsx`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/components/features/streaming-app/config/developer-tab.tsx`

- **Why it exists / responsibility:** Streaming App feature module responsible for the developer tab portion of its screen or workflow.
- **Runtime and size:** Client module; 207 lines.
- **Exports / surface:** `DeveloperTab`, `DeveloperTab`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** `useFormContext`; state bindings None; contexts None.
- **Dependencies:** internal `src/components/ui/form.tsx`, `src/components/ui/input.tsx`, `src/components/ui/label.tsx`, `src/components/ui/select.tsx`, `src/components/ui/switch.tsx`, `src/components/features/streaming-app/config/types.ts`, `src/components/features/streaming-app/config/shared-form.tsx`, `src/components/shared/doc-link.tsx`, plus 1 more; external `react`, `react-hook-form`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/components/features/streaming-app/config/config-settings-form.tsx`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/components/features/streaming-app/config/image-asset-list.tsx`

- **Why it exists / responsibility:** Streaming App feature module responsible for the image asset list portion of its screen or workflow.
- **Runtime and size:** Client module; 152 lines.
- **Exports / surface:** `ImageAssetList`, `ImageCard({ url, onSelect, isSelected }: ImageCardProps)`, `ImageAssetList`, `assetSelectHandler(url: string)`, `handleAddSelection`, `renderEmptyState`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** `useUserInfo`, `useConfigContext`, `useFormContext`, `useAsset2dQuery`, `useState`, `useEffect`; state bindings `activeTab`, `setActiveTab`, `selectedImageUrl`, `setSelectedImageUrl`; contexts None.
- **Dependencies:** internal `src/components/ui/button.tsx`, `src/components/shared/tabs.tsx`, `src/components/providers/config-provider.tsx`, `src/store/api/asset.ts`, `src/hooks/use-user-info.ts`, `src/components/features/streaming-app/utils.ts`, `src/components/shared/loader.tsx`; external `react`, `lucide-react`, `react-hook-form`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/components/features/streaming-app/application.tsx`.
- **Improvement / caution:** Keep effect dependencies complete and verify cleanup under React Strict Mode.

#### `src/components/features/streaming-app/config/image-upload.tsx`

- **Why it exists / responsibility:** Streaming App feature module responsible for the image upload portion of its screen or workflow.
- **Runtime and size:** Client module; 195 lines.
- **Exports / surface:** `ImageUpload`, `ImageUpload`, `handleFileChange(selectedFile: File)`, `handleBrowseClick`, `handleInputChange(e: React.ChangeEvent<HTMLInputElement>)`, `handleRemoveFile`, `handleUploadClick`, `formatFileSize(bytes: number)`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** `useConfigContext`, `useRef`, `useState`, `useCommonUpload`, `useCallback`; state bindings `dragActive`, `setDragActive`, `previewUrl`, `setPreviewUrl`; contexts None.
- **Dependencies:** internal `src/components/ui/button.tsx`, `src/hooks/use-common-upload.ts`, `src/constant/asset.ts`, `src/components/providers/config-provider.tsx`, `src/components/ui/progress.tsx`, `src/utils/common.ts`; external `react`, `next/image`, `lucide-react`, `sonner`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/components/features/streaming-app/application.tsx`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/components/features/streaming-app/config/shared-form.tsx`

- **Why it exists / responsibility:** Streaming App feature module responsible for the shared form portion of its screen or workflow.
- **Runtime and size:** Client module; 219 lines.
- **Exports / surface:** `FormFieldFile`, `SimpleSwitchField`, `FormFieldInput`, `Tooltip({ children, content }: TooltipProps)`, `FormFieldFile({
  onClick,
  onRemove,
  name,
  label,
  description,
  currentValue,
  link
}: FormFieldFileProps)`, `SimpleSwitchField({
  name,
  label,
  description,
  link
}: {
  name: keyof FormValues;
  label: string;
  description: string;
  link?: string;
})`, `FormFieldInput({
  name,
  label,
  link,
  description,
  placeholder,
  unit = 'Mins',
  type = 'text',
  disabled = false,
  className
}: {
  name: keyof FormValues;
  label: string;
  link?: string;
  description?: string;
  placeholder?: string;
  unit?: string;
  type?: string;
  disabled?: boolean;
  className?: string;
})`.
- **Props / inputs visible at declarations:** `Tooltip: { children, content }: TooltipProps`, `FormFieldFile: {
  onClick,
  onRemove,
  name,
  label,
  description,
  currentValue,
  link
}: FormFieldFileProps`, `SimpleSwitchField: {
  name,
  label,
  description,
  link
}: {
  name: keyof FormValues;
  label: string;
  description: string;
  link?: string;
}`, `FormFieldInput: {
  name,
  label,
  link,
  description,
  placeholder,
  unit = 'Mins',
  type = 'text',
  disabled = false,
  className
}: {
  name: keyof FormValues;
  label: string;
  link?: string;
  description?: string;
  placeholder?: string;
  unit?: string;
  type?: string;
  disabled?: boolean;
  className?: string;
}`.
- **Hooks / local state:** `useFormContext`; state bindings None; contexts None.
- **Dependencies:** internal `src/components/ui/button.tsx`, `src/components/ui/form.tsx`, `src/components/ui/input.tsx`, `src/components/ui/label.tsx`, `src/components/ui/switch.tsx`, `src/components/features/streaming-app/config/types.ts`, `src/components/shared/doc-link.tsx`, `src/components/ui/tooltip.tsx`, plus 1 more; external `react`, `react-hook-form`, `lucide-react`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/components/features/streaming-app/config/common-tab.tsx`, `src/components/features/streaming-app/config/customization-tab.tsx`, `src/components/features/streaming-app/config/developer-tab.tsx`, `src/components/features/streaming-app/config/ui-tab.tsx`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/components/features/streaming-app/config/types.ts`

- **Why it exists / responsibility:** Streaming App feature module responsible for the types portion of its screen or workflow.
- **Runtime and size:** Shared/build-time eligible module; 50 lines.
- **Exports / surface:** `FormValues`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** None; state bindings None; contexts None.
- **Dependencies:** internal None; external None.
- **Rendering and rerenders:** No JSX detected; it runs when imported or when an exported function/hook is called.
- **Direct consumers:** `src/components/features/streaming-app/config/common-tab.tsx`, `src/components/features/streaming-app/config/customization-tab.tsx`, `src/components/features/streaming-app/config/developer-tab.tsx`, `src/components/features/streaming-app/config/shared-form.tsx`, `src/components/features/streaming-app/config/ui-tab.tsx`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/components/features/streaming-app/config/ui-tab.tsx`

- **Why it exists / responsibility:** Streaming App feature module responsible for the ui tab portion of its screen or workflow.
- **Runtime and size:** Client module; 106 lines.
- **Exports / surface:** `UITab`, `UITab`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** `useFormContext`; state bindings None; contexts None.
- **Dependencies:** internal `src/components/ui/form.tsx`, `src/components/ui/label.tsx`, `src/components/ui/switch.tsx`, `src/components/features/streaming-app/config/types.ts`, `src/components/features/streaming-app/config/shared-form.tsx`, `src/config/doc.ts`, `src/components/shared/doc-link.tsx`; external `react`, `react-hook-form`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/components/features/streaming-app/config/config-settings-form.tsx`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/components/features/streaming-app/config/video-asset-list.tsx`

- **Why it exists / responsibility:** Streaming App feature module responsible for the video asset list portion of its screen or workflow.
- **Runtime and size:** Client module; 156 lines.
- **Exports / surface:** `VideoAssetList`, `VideoCard({ url, onSelect, isSelected }: VideoCardProps)`, `VideoAssetList`, `handleVideoSelect(videoUrl: string)`, `handleAddSelection`, `renderEmptyState`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** `useUserInfo`, `useConfigContext`, `useFormContext`, `useAssetVideoQuery`, `useState`; state bindings `activeTab`, `setActiveTab`, `selectedVideo`, `setSelectedVideo`; contexts None.
- **Dependencies:** internal `src/components/ui/button.tsx`, `src/components/shared/tabs.tsx`, `src/components/providers/config-provider.tsx`, `src/store/api/asset.ts`, `src/hooks/use-user-info.ts`, `src/components/features/streaming-app/utils.ts`, `src/components/shared/loader.tsx`; external `react`, `lucide-react`, `react-hook-form`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/components/features/streaming-app/application.tsx`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/components/features/streaming-app/demo-apps/demo-app-card.tsx`

- **Why it exists / responsibility:** Streaming App feature module responsible for the demo app card portion of its screen or workflow.
- **Runtime and size:** Client-bound through an importing client module; 61 lines.
- **Exports / surface:** `DemoAppCard`, `DemoAppCard({ demoApp, onClick, disabled = false }: DemoAppCardProps)`.
- **Props / inputs visible at declarations:** `DemoAppCard: { demoApp, onClick, disabled = false }: DemoAppCardProps`.
- **Hooks / local state:** None; state bindings None; contexts None.
- **Dependencies:** internal `src/components/ui/card.tsx`, `src/assets/index.ts`; external `next/image`, `lucide-react`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/components/features/streaming-app/demo-apps/demo-application.tsx`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/components/features/streaming-app/demo-apps/demo-application.tsx`

- **Why it exists / responsibility:** Streaming App feature module responsible for the demo application portion of its screen or workflow.
- **Runtime and size:** Client-bound through an importing client module; 260 lines.
- **Exports / surface:** `DemoApplication`, `DemoApplication`, `generateHostUrl(demoApp: TDemoApp)`, `generateHostIframe(demoApp: TDemoApp)`, `copyHandler(text: string, message: string)`, `handleDeleteApp(appName: string)`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** `useUserInfo`, `useAppUrlQuery`, `useDeleteStreamingAppMutation`, `useState`, `useMemo`; state bindings `cardModalOpen`, `setCardModalOpen`, `selectedDemoApp`, `setSelectedDemoApp`, `deleteDialogOpen`, `setDeleteDialogOpen`, `appToDelete`, `setAppToDelete`; contexts None.
- **Dependencies:** internal `src/hooks/use-user-info.ts`, `src/store/api/appLink.ts`, `src/components/features/streaming-app/demo-apps/demo-app-card.tsx`, `src/components/shared/dialog/custom-dialog.tsx`, `src/components/shared/dialog/dialog-header.tsx`, `src/components/ui/dialog.tsx`, `src/components/ui/button.tsx`, `src/components/ui/select.tsx`, plus 3 more; external `react`, `@tanstack/react-table`, `lucide-react`, `sonner`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/components/features/streaming-app/streaming-app-section.tsx`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/components/features/streaming-app/meeting-link-tab.tsx`

- **Why it exists / responsibility:** Streaming App feature module responsible for the meeting link tab portion of its screen or workflow.
- **Runtime and size:** Client module; 725 lines.
- **Exports / surface:** `TMeetingUrl`, `TAppUrlResponse`, `MeetingLinkTab`, `HostLinkCell({
  meeting,
  username,
  streamBaseUrl,
  getSelectedVersion
}: {
  meeting: TMeetingUrl;
  username: string;
  streamBaseUrl: string;
  getSelectedVersion: (id: string) => string;
})`, `generateHostUrl`, `generateHostIframe`, `copyHandler(text: string, message: string)`, `MeetingLinkTab({ selectedApp, onCreateMeetingLink, isSaveLoading }: MeetingLinkTabProps)`, `handleVersionChange(meetingId: string, version: string)`, `getSelectedVersion(meetingId: string)`, `handleCreateConfig`, `handleConfigChange(meeting: TMeetingUrl, value: string)`, plus 15 more.
- **Props / inputs visible at declarations:** `HostLinkCell: {
  meeting,
  username,
  streamBaseUrl,
  getSelectedVersion
}: {
  meeting: TMeetingUrl;
  username: string;
  streamBaseUrl: string;
  getSelectedVersion: (id: string) => string;
}`, `MeetingLinkTab: { selectedApp, onCreateMeetingLink, isSaveLoading }: MeetingLinkTabProps`.
- **Hooks / local state:** `useUserInfo`, `useGetConfigQuery`, `useConfigContext`, `useSubscription`, `useStreamingAppQuery`, `useCreateConfigMutation`, `useSaveAppUrlMutation`, `useDeleteConfigMutation`, `useDeleteAppVersionMutation`, `useConfigListQuery`, `useAppUrlQuery`, `useState`, plus 1 more; state bindings `versionSelections`, `setVersionSelections`, `isConfigModalOpen`, `setIsConfigModalOpen`, `newConfigName`, `setNewConfigName`, `isCreatingConfig`, `setIsCreatingConfig`, `selectedMeeting`, `setSelectedMeeting`, `isChangeConfigModalOpen`, `setIsChangeConfigModalOpen`, plus 14 more; contexts None.
- **Dependencies:** internal `src/components/ui/button.tsx`, `src/components/ui/select.tsx`, `src/components/ui/input.tsx`, `src/components/shared/dialog/alert-dialog.tsx`, `src/components/features/streaming-app/streaming-app-table.tsx`, `src/hooks/use-user-info.ts`, `src/store/api/appLink.ts`, `src/components/features/streaming-app/utils.ts`, plus 9 more; external `react`, `@tanstack/react-table`, `lucide-react`, `sonner`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/components/features/streaming-app/application.tsx`.
- **Improvement / caution:** Decompose by responsibility and add focused tests before changing behavior.

#### `src/components/features/streaming-app/normalizeAppExeData.ts`

- **Why it exists / responsibility:** Streaming App feature module responsible for the normalize app exe data portion of its screen or workflow.
- **Runtime and size:** Shared/build-time eligible module; 94 lines.
- **Exports / surface:** `normalizeAppExeData`, `normalizeAppExeData(data: any)`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** None; state bindings None; contexts None.
- **Dependencies:** internal None; external None.
- **Rendering and rerenders:** No JSX detected; it runs when imported or when an exported function/hook is called.
- **Direct consumers:** `src/components/providers/upload/upload-sequence-provider.tsx`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/components/features/streaming-app/streaming-app-card.tsx`

- **Why it exists / responsibility:** Streaming App feature module responsible for the streaming app card portion of its screen or workflow.
- **Runtime and size:** Client-bound through an importing client module; 202 lines.
- **Exports / surface:** `StreamingAppCard`, `StreamingAppCard({
  app,
  onClick,
  onUpdate,
  onUpload,
  disabled = false,
  isUpdating = false,
  progress = 0,
  loadingState = '',
  imageIndex = 0,
  hasError = false,
  errorMessage = '',
  isGlobalUploading = false
}: StreamingAppCardProps)`, `isReadyToPlay`.
- **Props / inputs visible at declarations:** `StreamingAppCard: {
  app,
  onClick,
  onUpdate,
  onUpload,
  disabled = false,
  isUpdating = false,
  progress = 0,
  loadingState = '',
  imageIndex = 0,
  hasError = false,
  errorMessage = '',
  isGlobalUploading = false
}: StreamingAppCardProps`.
- **Hooks / local state:** None; state bindings None; contexts None.
- **Dependencies:** internal `src/components/ui/card.tsx`, `src/components/ui/button.tsx`, `src/components/ui/progress.tsx`, `src/assets/index.ts`, `src/components/ui/tooltip.tsx`, `src/utils/asset.ts`; external `next/image`, `lucide-react`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/components/features/streaming-app/application.tsx`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/components/features/streaming-app/streaming-app-page.tsx`

- **Why it exists / responsibility:** Streaming App feature module responsible for the streaming app page portion of its screen or workflow.
- **Runtime and size:** Client module; 25 lines.
- **Exports / surface:** `StreamingAppPage`, `StreamingAppPage`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** `useSubscription`, `useTrial`, `useStreamingApp`; state bindings None; contexts None.
- **Dependencies:** internal `src/hooks/use-subscription.ts`, `src/components/features/subscription/subscription-summary/subscription-summary-section.tsx`, `src/components/features/streaming-app/streaming-app-section.tsx`, `src/hooks/use-trial.ts`, `src/hooks/use-streaming-app.ts`, `src/components/shared/loader.tsx`; external None.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/app/(withSidebarLayout)/page.tsx`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/components/features/streaming-app/streaming-app-section.tsx`

- **Why it exists / responsibility:** Streaming App feature module responsible for the streaming app section portion of its screen or workflow.
- **Runtime and size:** Client module; 77 lines.
- **Exports / surface:** `StreamingAppSection`, `StreamingAppSection`, `fetchUserDocuments`, `renderContent`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** `useState`, `useUserInfo`, `useEffect`; state bindings `activeTab`, `setActiveTab`; contexts None.
- **Dependencies:** internal `src/components/shared/tabs.tsx`, `src/components/ui/headlines/page-title.tsx`, `src/components/features/streaming-app/application.tsx`, `src/components/features/utilities/streaming-app-monitor.tsx`, `src/components/providers/upload/upload-sequence-provider.tsx`, `src/config/firebase.ts`, `src/hooks/use-user-info.ts`, `src/components/features/streaming-app/demo-apps/demo-application.tsx`; external `react`, `firebase/firestore`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/components/features/streaming-app/streaming-app-page.tsx`.
- **Improvement / caution:** Keep effect dependencies complete and verify cleanup under React Strict Mode.

#### `src/components/features/streaming-app/streaming-app-table.tsx`

- **Why it exists / responsibility:** Streaming App feature module responsible for the streaming app table portion of its screen or workflow.
- **Runtime and size:** Client module; 240 lines.
- **Exports / surface:** `StreamingAppTable`, `StreamingAppTable({
  columns,
  data,
  showToolbar = false,
  showPagination = true,
  filterPlaceholder,
  pageSizeOptions = [5, 20, 30, 40, 50],
  isLoading = false
}: CommonTableProps<TData, TValue>)`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** `useState`, `useReactTable`; state bindings `sorting`, `setSorting`, `columnFilters`, `setColumnFilters`, `globalFilter`, `setGlobalFilter`, `columnVisibility`, `setColumnVisibility`, `rowSelection`, `setRowSelection`; contexts None.
- **Dependencies:** internal `src/components/ui/table.tsx`, `src/components/ui/button.tsx`, `src/components/ui/input.tsx`, `src/components/ui/dropdown-menu.tsx`, `src/components/ui/select.tsx`, `src/utils/common.ts`, `src/components/shared/loader.tsx`; external `react`, `@tanstack/react-table`, `lucide-react`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/components/features/streaming-app/demo-apps/demo-application.tsx`, `src/components/features/streaming-app/meeting-link-tab.tsx`, `src/components/features/streaming-app/streaming-link-tab.tsx`.
- **Improvement / caution:** Consider a reducer/state machine to make transitions explicit.

#### `src/components/features/streaming-app/streaming-link-tab.tsx`

- **Why it exists / responsibility:** Streaming App feature module responsible for the streaming link tab portion of its screen or workflow.
- **Runtime and size:** Client-bound through an importing client module; 696 lines.
- **Exports / surface:** `TStreamingUrls`, `StreamingLinkTab`, `StreamingLinkCell({
  streaming,
  username,
  streamBaseUrl,
  getSelectedVersion
}: {
  streaming: TStreamingUrls;
  username: string;
  streamBaseUrl: string;
  getSelectedVersion: (id: string) => string;
})`, `generateHostUrl`, `generateHostIframe`, `copyHandler(text: string, message: string)`, `StreamingLinkTab({ selectedApp, onCreateStreamLink, isSaveLoading }: StreamingLinkTabProps)`, `handleVersionChange(streamingId: string, version: string)`, `getSelectedVersion(streamingId: string)`, `handleCreateConfig`, `handleConfigChange(streaming: TStreamingUrls, value: string)`, `confirmConfigChange`, plus 12 more.
- **Props / inputs visible at declarations:** `StreamingLinkCell: {
  streaming,
  username,
  streamBaseUrl,
  getSelectedVersion
}: {
  streaming: TStreamingUrls;
  username: string;
  streamBaseUrl: string;
  getSelectedVersion: (id: string) => string;
}`, `StreamingLinkTab: { selectedApp, onCreateStreamLink, isSaveLoading }: StreamingLinkTabProps`.
- **Hooks / local state:** `useUserInfo`, `useGetConfigQuery`, `useConfigContext`, `useSubscription`, `useStreamingAppQuery`, `useCreateConfigMutation`, `useSaveAppUrlMutation`, `useDeleteConfigMutation`, `useDeleteAppVersionMutation`, `useConfigListQuery`, `useAppUrlQuery`, `useState`, plus 1 more; state bindings `versionSelections`, `setVersionSelections`, `isConfigModalOpen`, `setIsConfigModalOpen`, `newConfigName`, `setNewConfigName`, `isCreatingConfig`, `setIsCreatingConfig`, `selectedStreaming`, `setSelectedStreaming`, `isChangeConfigModalOpen`, `setIsChangeConfigModalOpen`, plus 14 more; contexts None.
- **Dependencies:** internal `src/hooks/use-user-info.ts`, `src/store/api/appLink.ts`, `src/types/common.ts`, `src/components/ui/select.tsx`, `src/components/ui/input.tsx`, `src/components/ui/button.tsx`, `src/components/shared/dialog/alert-dialog.tsx`, `src/components/features/streaming-app/streaming-app-table.tsx`, plus 9 more; external `react`, `@tanstack/react-table`, `lucide-react`, `sonner`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/components/features/streaming-app/application.tsx`.
- **Improvement / caution:** Consider a reducer/state machine to make transitions explicit.

#### `src/components/features/streaming-app/temporary-upload-card.tsx`

- **Why it exists / responsibility:** Streaming App feature module responsible for the temporary upload card portion of its screen or workflow.
- **Runtime and size:** Client module; 91 lines.
- **Exports / surface:** `TemporaryUploadCard`, `getProgressStep(progress: number, loadingState: string, hasError: boolean)`, `TemporaryUploadCard({
  appName,
  progress,
  loadingState,
  onClick,
  thumbnailPreview,
  hasError = false,
  errorMessage
}: TemporaryUploadCardProps)`.
- **Props / inputs visible at declarations:** `TemporaryUploadCard: {
  appName,
  progress,
  loadingState,
  onClick,
  thumbnailPreview,
  hasError = false,
  errorMessage
}: TemporaryUploadCardProps`.
- **Hooks / local state:** None; state bindings None; contexts None.
- **Dependencies:** internal `src/assets/index.ts`, `src/components/ui/card.tsx`, `src/components/ui/progress.tsx`; external `next/image`, `lucide-react`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/components/features/streaming-app/application.tsx`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/components/features/streaming-app/utils.ts`

- **Why it exists / responsibility:** Streaming App feature module responsible for the utils portion of its screen or workflow.
- **Runtime and size:** Client-bound through an importing client module; 197 lines.
- **Exports / surface:** `useCheckExactAssetNameMatch`, `generateName`, `checkConfigName`, `FormValues`, `defaultConfig`, `createAnonymizedUrl`, `useCheckExactAssetNameMatch`, `checkExactAssetNameMatch(searchString: string)`, `generateName(length: number)`, `checkConfigName(text: string)`, `createAnonymizedUrl(userName: string, appName: string, configName: string, streamBaseUrl: string, appVersion?: string)`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** `useStreamingApp`; state bindings None; contexts None.
- **Dependencies:** internal `src/hooks/use-streaming-app.ts`; external `axios`.
- **Rendering and rerenders:** No JSX detected; it runs when imported or when an exported function/hook is called.
- **Direct consumers:** `src/app/(withSidebarLayout)/upload/app-upload-modal.tsx`, `src/components/features/singup/signup-page.tsx`, `src/components/features/streaming-app/application.tsx`, `src/components/features/streaming-app/application.tsx`, `src/components/features/streaming-app/config/config-settings-form.tsx`, `src/components/features/streaming-app/config/image-asset-list.tsx`, `src/components/features/streaming-app/config/video-asset-list.tsx`, `src/components/features/streaming-app/meeting-link-tab.tsx`, plus 2 more.
- **Improvement / caution:** Confirm cancellation, error normalization, and cache synchronization for direct Axios traffic.

#### `src/components/features/streaming-app/video-upload.tsx`

- **Why it exists / responsibility:** Streaming App feature module responsible for the video upload portion of its screen or workflow.
- **Runtime and size:** Client module; 188 lines.
- **Exports / surface:** `VideoUpload`, `VideoUpload`, `handleFileChange(selectedFile: File)`, `handleBrowseClick`, `handleInputChange(e: React.ChangeEvent<HTMLInputElement>)`, `handleRemoveFile`, `handleUploadClick`, `formatFileSize(bytes: number)`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** `useUserInfo`, `useConfigContext`, `useRef`, `useState`, `useCommonUpload`, `useCallback`; state bindings `dragActive`, `setDragActive`, `previewUrl`, `setPreviewUrl`; contexts None.
- **Dependencies:** internal `src/components/ui/button.tsx`, `src/hooks/use-common-upload.ts`, `src/constant/asset.ts`, `src/components/providers/config-provider.tsx`, `src/components/ui/progress.tsx`, `src/utils/common.ts`, `src/store/api/user-info.ts`, `src/hooks/use-user-info.ts`, plus 1 more; external `react`, `lucide-react`, `sonner`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/components/features/streaming-app/application.tsx`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/components/features/subscription/bill-summary-ppm.tsx`

- **Why it exists / responsibility:** Subscription feature module responsible for the bill summary ppm portion of its screen or workflow.
- **Runtime and size:** Client-bound through an importing client module; 72 lines.
- **Exports / surface:** `BillSummaryPpm`, `BillSummaryPpm({ disablePaymentEdit = false }: BillSummaryPpmProps)`.
- **Props / inputs visible at declarations:** `BillSummaryPpm: { disablePaymentEdit = false }: BillSummaryPpmProps`.
- **Hooks / local state:** `useSubscription`, `useStorage`, `useMinute`; state bindings None; contexts None.
- **Dependencies:** internal `src/components/ui/card.tsx`, `src/components/ui/separator.tsx`, `src/utils/date-formatter.ts`, `src/components/features/subscription/payment-info.tsx`, `src/hooks/use-subscription.ts`, `src/hooks/use-storage.ts`, `src/hooks/use-minute.ts`; external None.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/components/features/subscription/subscription-details/ppm.tsx`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/components/features/subscription/bill-summary.tsx`

- **Why it exists / responsibility:** Subscription feature module responsible for the bill summary portion of its screen or workflow.
- **Runtime and size:** Client-bound through an importing client module; 85 lines.
- **Exports / surface:** `BillSummary`, `BillSummary({ disablePaymentEdit = false }: BillSummaryProps)`.
- **Props / inputs visible at declarations:** `BillSummary: { disablePaymentEdit = false }: BillSummaryProps`.
- **Hooks / local state:** `useSubscription`, `useStorage`; state bindings None; contexts None.
- **Dependencies:** internal `src/components/ui/card.tsx`, `src/components/ui/separator.tsx`, `src/utils/date-formatter.ts`, `src/components/features/subscription/payment-info.tsx`, `src/hooks/use-subscription.ts`, `src/hooks/use-storage.ts`, `src/constant/plan.ts`; external None.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/components/features/subscription/subscription-details/ppccu.tsx`, `src/components/features/subscription/subscription-details/ppl.tsx`, `src/components/features/subscription/subscription-details/prepaid-minute.tsx`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/components/features/subscription/cancel-reason-dialog.tsx`

- **Why it exists / responsibility:** Subscription feature module responsible for the cancel reason dialog portion of its screen or workflow.
- **Runtime and size:** Client module; 92 lines.
- **Exports / surface:** `CancelReasonDialog`, `CancelReasonDialog({ open, onClose, onConfirm }: CancelReasonDialogProps)`, `handleConfirm`.
- **Props / inputs visible at declarations:** `CancelReasonDialog: { open, onClose, onConfirm }: CancelReasonDialogProps`.
- **Hooks / local state:** `useState`; state bindings `selectedReason`, `setSelectedReason`, `otherReason`, `setOtherReason`; contexts None.
- **Dependencies:** internal `src/components/ui/dialog.tsx`, `src/components/ui/button.tsx`, `src/components/ui/input.tsx`, `src/components/ui/label.tsx`; external `react`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/components/features/new-subscription/core-plan-montly.tsx`, `src/components/features/new-subscription/core-plan-yearly.tsx`, `src/components/features/subscription/subscription-details/ppccu.tsx`, `src/components/features/subscription/subscription-details/ppl.tsx`, `src/components/features/subscription/subscription-details/ppm.tsx`, `src/components/features/subscription/subscription-details/prepaid-minute.tsx`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/components/features/subscription/modal/ppccu-ppl-modify-modal.tsx`

- **Why it exists / responsibility:** Subscription feature module responsible for the ppccu ppl modify modal portion of its screen or workflow.
- **Runtime and size:** Client-bound through an importing client module; 229 lines.
- **Exports / surface:** `PpccuPplModifyModal`, `PpccuPplModifyModal({ isDialogOpen, setIsDialogOpen }: PpccuPplModifyModal)`, `handleNext`, `handleBackDialog(event?: React.MouseEvent<HTMLElement>)`, `handleConfirm`.
- **Props / inputs visible at declarations:** `PpccuPplModifyModal: { isDialogOpen, setIsDialogOpen }: PpccuPplModifyModal`.
- **Hooks / local state:** `useSubscription`, `useUserInfo`, `useCardDetails`, `useUpgradePpccuMutation`, `useUpgradePplMutation`, `useDowngradePpccuMutation`, `useDowngradePplMutation`, `useState`, `usePpccuQuoteQuery`, `usePplQuoteQuery`; state bindings `selectedQuantity`, `setSelectedQuantity`, `step`, `setStep`; contexts None.
- **Dependencies:** internal `src/components/shared/dialog/alert-dialog.tsx`, `src/hooks/use-user-info.ts`, `src/hooks/use-subscription.ts`, `src/utils/date-formatter.ts`, `src/components/ui/field/select-drop-down.tsx`, `src/utils/options.ts`, `src/constant/plan.ts`, `src/store/api/subscription.ts`, plus 1 more; external `react`, `sonner`, `lucide-react`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/components/features/subscription/subscription-details/ppccu.tsx`, `src/components/features/subscription/subscription-details/ppl.tsx`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/components/features/subscription/modal/prepaid-minute-modify-modal.tsx`

- **Why it exists / responsibility:** Subscription feature module responsible for the prepaid minute modify modal portion of its screen or workflow.
- **Runtime and size:** Client-bound through an importing client module; 238 lines.
- **Exports / surface:** `PrepaidMinuteModifyModal`, `PrepaidMinuteModifyModal({ isDialogOpen, setIsDialogOpen }: PrepaidMinuteModifyModalProps)`, `handleMinuteDialogConfirm`, `handleBackDialog(event?: React.MouseEvent<HTMLElement>)`.
- **Props / inputs visible at declarations:** `PrepaidMinuteModifyModal: { isDialogOpen, setIsDialogOpen }: PrepaidMinuteModifyModalProps`.
- **Hooks / local state:** `useUserInfo`, `useCardDetails`, `useSubscription`, `useUserAllInfoQuery`, `useUpgradePrepaidMinuteMutation`, `useDowngradePrepaidMinuteMutation`, `useAddWatcherMutation`, `useState`, `useEffect`, `usePrepaidMinuteQuoteQuery`; state bindings `step`, `setStep`, `newMinute`, `setNewMinute`; contexts None.
- **Dependencies:** internal `src/components/shared/dialog/alert-dialog.tsx`, `src/utils/date-formatter.ts`, `src/hooks/use-subscription.ts`, `src/components/ui/button.tsx`, `src/store/api/user-info.ts`, `src/hooks/use-user-info.ts`, `src/store/api/subscription.ts`, `src/hooks/use-card-details.ts`; external `react`, `sonner`, `lucide-react`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/components/features/subscription/subscription-details/prepaid-minute.tsx`.
- **Improvement / caution:** Keep effect dependencies complete and verify cleanup under React Strict Mode.

#### `src/components/features/subscription/modal/storage-modify-modal.tsx`

- **Why it exists / responsibility:** Subscription feature module responsible for the storage modify modal portion of its screen or workflow.
- **Runtime and size:** Client-bound through an importing client module; 202 lines.
- **Exports / surface:** `StorageModifyModal`, `StorageModifyModal({ isDialogOpen, setIsDialogOpen }: StorageModifyModalProps)`, `handleNext`, `handleBackDialog(event?: React.MouseEvent<HTMLElement>)`, `handleConfirm`.
- **Props / inputs visible at declarations:** `StorageModifyModal: { isDialogOpen, setIsDialogOpen }: StorageModifyModalProps`.
- **Hooks / local state:** `useStorage`, `useSubscription`, `useUserInfo`, `useCardDetails`, `useCreateStorageSubMutation`, `useUpgradeStorageMutation`, `useDowngradeStorageMutation`, `useState`, `useStorageQuoteQuery`; state bindings `selectedStorage`, `setSelectedStorage`, `step`, `setStep`; contexts None.
- **Dependencies:** internal `src/components/shared/dialog/alert-dialog.tsx`, `src/hooks/use-user-info.ts`, `src/hooks/use-subscription.ts`, `src/store/api/storage.ts`, `src/hooks/use-storage.ts`, `src/utils/date-formatter.ts`, `src/components/ui/field/select-drop-down.tsx`, `src/config/page.ts`, plus 3 more; external `react`, `sonner`, `lucide-react`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/components/features/subscription/storage-utilized.tsx`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/components/features/subscription/new-subscription-summary/core-plan.tsx`

- **Why it exists / responsibility:** Subscription feature module responsible for the core plan portion of its screen or workflow.
- **Runtime and size:** Client-bound through an importing client module; 129 lines.
- **Exports / surface:** `CorePlan`, `CorePlan`, `handleButton`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** `useUserInfo`, `useSubscriptionV2`, `useMinutesV2`, `useStorageV2`, `useState`; state bindings `modalOpen`, `setModalOpen`; contexts None.
- **Dependencies:** internal `src/components/ui/button.tsx`, `src/components/ui/card.tsx`, `src/components/ui/progress.tsx`, `src/hooks/use-storageV2.ts`, `src/hooks/use-subscription-v2.ts`, `src/assets/index.ts`, `src/hooks/use-minute-v2.ts`, `src/components/features/my-account/my-account.tsx`, plus 1 more; external `react`, `next/image`, `lucide-react`, `sonner`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/components/features/subscription/subscription-summary/subscription-summary-section.tsx`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/components/features/subscription/new-subscription-summary/expried-trial-plan.tsx`

- **Why it exists / responsibility:** Subscription feature module responsible for the expried trial plan portion of its screen or workflow.
- **Runtime and size:** Client-bound through an importing client module; 96 lines.
- **Exports / surface:** `ExpritedTrialPlan`, `ExpritedTrialPlan`, `handleBuyClick`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** `useRouter`, `useTrialV2`, `useUserInfo`; state bindings None; contexts None.
- **Dependencies:** internal `src/components/ui/button.tsx`, `src/components/ui/card.tsx`, `src/config/page.ts`, `src/hooks/use-trial-v2.ts`, `src/assets/index.ts`, `src/hooks/use-user-info.ts`; external `next/navigation`, `next/image`, `lucide-react`, `sonner`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/components/features/subscription/subscription-summary/subscription-summary-section.tsx`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/components/features/subscription/new-subscription-summary/pause-plan.tsx`

- **Why it exists / responsibility:** Subscription feature module responsible for the pause plan portion of its screen or workflow.
- **Runtime and size:** Client-bound through an importing client module; 56 lines.
- **Exports / surface:** `PausePlan`, `PausePlan`, `handleUpdateClick`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** `useState`, `useUserInfo`; state bindings `modalOpen`, `setModalOpen`, `activeTab`, `setActiveTab`; contexts None.
- **Dependencies:** internal `src/components/ui/button.tsx`, `src/components/ui/card.tsx`, `src/assets/index.ts`, `src/components/features/my-account/my-account.tsx`, `src/hooks/use-user-info.ts`; external `react`, `next/image`, `sonner`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/components/features/subscription/subscription-summary/subscription-summary-section.tsx`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/components/features/subscription/new-subscription-summary/trial-plan.tsx`

- **Why it exists / responsibility:** Subscription feature module responsible for the trial plan portion of its screen or workflow.
- **Runtime and size:** Client-bound through an importing client module; 107 lines.
- **Exports / surface:** `TrialPlan`, `TrialPlan`, `handleBuyClick`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** `useRouter`, `useTrialV2`, `useUserInfo`; state bindings None; contexts None.
- **Dependencies:** internal `src/components/ui/button.tsx`, `src/components/ui/card.tsx`, `src/components/ui/progress.tsx`, `src/config/page.ts`, `src/hooks/use-trial-v2.ts`, `src/assets/index.ts`, `src/hooks/use-user-info.ts`; external `next/navigation`, `next/image`, `sonner`, `lucide-react`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/components/features/subscription/subscription-summary/subscription-summary-section.tsx`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/components/features/subscription/payment-info.tsx`

- **Why it exists / responsibility:** Subscription feature module responsible for the payment info portion of its screen or workflow.
- **Runtime and size:** Client-bound through an importing client module; 108 lines.
- **Exports / surface:** `PaymentInfo`, `PaymentInfo({ disableEdit = false }: PaymentInfoProps)`, `handleConfirm`.
- **Props / inputs visible at declarations:** `PaymentInfo: { disableEdit = false }: PaymentInfoProps`.
- **Hooks / local state:** `useEditPaymentMutation`, `useUserInfo`, `useCardDetails`, `useOldSubPause`, `useStorage`, `useState`; state bindings `isConfirmDialogOpen`, `setIsConfirmDialogOpen`; contexts None.
- **Dependencies:** internal `src/components/ui/button.tsx`, `src/components/shared/dialog/alert-dialog.tsx`, `src/store/api/payment.ts`, `src/hooks/use-user-info.ts`, `src/hooks/use-card-details.ts`, `src/config/page.ts`, `src/components/shared/doc-link.tsx`, `src/config/doc.ts`, plus 3 more; external `react`, `lucide-react`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/components/features/subscription/bill-summary-ppm.tsx`, `src/components/features/subscription/bill-summary.tsx`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/components/features/subscription/storage-utilized.tsx`

- **Why it exists / responsibility:** Subscription feature module responsible for the storage utilized portion of its screen or workflow.
- **Runtime and size:** Client module; 125 lines.
- **Exports / surface:** `StorageUtilized`, `StorageUtilized({ disableEditActions = false }: StorageUtilizedProps)`, `handleReactiveClick`, `handleReactiveSubscription`.
- **Props / inputs visible at declarations:** `StorageUtilized: { disableEditActions = false }: StorageUtilizedProps`.
- **Hooks / local state:** `useUserInfo`, `useStorage`, `useSubscription`, `useUnPauseSubscriptionMutation`, `useState`; state bindings `isDialogOpen`, `setIsDialogOpen`, `isReactiveDialogOpen`, `setReactiveDialogOpen`; contexts None.
- **Dependencies:** internal `src/components/ui/progress.tsx`, `src/components/ui/button.tsx`, `src/hooks/use-subscription.ts`, `src/hooks/use-storage.ts`, `src/components/features/subscription/modal/storage-modify-modal.tsx`, `src/components/shared/dialog/alert-dialog.tsx`, `src/components/shared/doc-link.tsx`, `src/config/doc.ts`, plus 2 more; external `react`, `lucide-react`, `sonner`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/components/features/subscription/subscription-details/ppccu.tsx`, `src/components/features/subscription/subscription-details/ppl.tsx`, `src/components/features/subscription/subscription-details/ppm.tsx`, `src/components/features/subscription/subscription-details/prepaid-minute.tsx`, `src/components/features/subscription/subscription-details/terminated-sub.tsx`, `src/components/features/subscription/subscription-details/trial.tsx`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/components/features/subscription/subscription-details/ppccu.tsx`

- **Why it exists / responsibility:** Subscription feature module responsible for the ppccu portion of its screen or workflow.
- **Runtime and size:** Client module; 220 lines.
- **Exports / surface:** `PpccuDetails`, `PpccuDetails`, `handleCancelReasonConfirm(reason: string)`, `handleCancelSub`, `handleReactiveClick`, `handleReactiveSubscription`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** `useMinute`, `useUserInfo`, `useSubscription`, `useStorage`, `useOldSubPause`, `useCancelAllSubMutation`, `useUnPauseSubscriptionMutation`, `useState`; state bindings `isCancelReasonOpen`, `setCancelReasonOpen`, `isCancelDialogOpen`, `setCancelDialogOpen`, `cancelReason`, `setCancelReason`, `isDialogOpen`, `setIsDialogOpen`, `isReactiveDialogOpen`, `setReactiveDialogOpen`; contexts None.
- **Dependencies:** internal `src/components/ui/button.tsx`, `src/components/ui/card.tsx`, `src/components/features/subscription/storage-utilized.tsx`, `src/components/shared/dialog/alert-dialog.tsx`, `src/components/features/subscription/cancel-reason-dialog.tsx`, `src/utils/notifications.ts`, `src/store/api/subscription.ts`, `src/hooks/use-minute.ts`, plus 11 more; external `react`, `lucide-react`, `sonner`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/components/features/subscription/subscription.tsx`.
- **Improvement / caution:** Consider a reducer/state machine to make transitions explicit.

#### `src/components/features/subscription/subscription-details/ppl.tsx`

- **Why it exists / responsibility:** Subscription feature module responsible for the ppl portion of its screen or workflow.
- **Runtime and size:** Client-bound through an importing client module; 212 lines.
- **Exports / surface:** `Ppl`, `Ppl`, `handleCancelReasonConfirm(reason: string)`, `handleCancelSub`, `handleReactiveClick`, `handleReactiveSubscription`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** `useMinute`, `useUserInfo`, `useSubscription`, `useCancelAllSubMutation`, `useOldSubPause`, `useStorage`, `useUnPauseSubscriptionMutation`, `useState`; state bindings `isCancelReasonOpen`, `setCancelReasonOpen`, `isCancelDialogOpen`, `setCancelDialogOpen`, `cancelReason`, `setCancelReason`, `isDialogOpen`, `setIsDialogOpen`, `isReactiveDialogOpen`, `setReactiveDialogOpen`; contexts None.
- **Dependencies:** internal `src/components/ui/button.tsx`, `src/components/ui/card.tsx`, `src/components/features/subscription/storage-utilized.tsx`, `src/components/shared/dialog/alert-dialog.tsx`, `src/components/features/subscription/cancel-reason-dialog.tsx`, `src/utils/notifications.ts`, `src/store/api/subscription.ts`, `src/hooks/use-subscription.ts`, plus 9 more; external `react`, `lucide-react`, `sonner`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/components/features/subscription/subscription.tsx`.
- **Improvement / caution:** Consider a reducer/state machine to make transitions explicit.

#### `src/components/features/subscription/subscription-details/ppm.tsx`

- **Why it exists / responsibility:** Subscription feature module responsible for the ppm portion of its screen or workflow.
- **Runtime and size:** Client-bound through an importing client module; 399 lines.
- **Exports / surface:** `Ppm`, `Ppm`, `handleCancelReasonConfirm(reason: string)`, `handleCancelSub`, `handleStreamLimit`, `forecastedMonthlyCost`, `streamingCapCloseHandler`, `handleRemoveBudget`, `handleReactiveClick`, `handleReactiveSubscription`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** `useUserInfo`, `useMinute`, `useSubscription`, `useCancelAllSubMutation`, `useCancelPpmSubMutation`, `useBudgetEnableMutation`, `useBudgetIncreaseMutation`, `useBudgetDecreaseMutation`, `useBudgetRemoveMutation`, `useOldSubPause`, `useStorage`, `useUnPauseSubscriptionMutation`, plus 3 more; state bindings `isCancelReasonOpen`, `setCancelReasonOpen`, `isCancelDialogOpen`, `setCancelDialogOpen`, `cancelReason`, `setCancelReason`, `removeStreamLimit`, `setRemoveStreamLimit`, `isStreamingCapDialogOpen`, `setStreamingCapDialogOpen`, `isReactiveDialogOpen`, `setReactiveDialogOpen`, plus 2 more; contexts None.
- **Dependencies:** internal `src/components/ui/button.tsx`, `src/components/ui/card.tsx`, `src/components/features/subscription/storage-utilized.tsx`, `src/components/shared/dialog/alert-dialog.tsx`, `src/components/features/subscription/cancel-reason-dialog.tsx`, `src/utils/notifications.ts`, `src/components/ui/switch.tsx`, `src/store/api/subscription.ts`, plus 10 more; external `react`, `lucide-react`, `sonner`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/components/features/subscription/subscription.tsx`.
- **Improvement / caution:** Consider a reducer/state machine to make transitions explicit.

#### `src/components/features/subscription/subscription-details/prepaid-minute.tsx`

- **Why it exists / responsibility:** Subscription feature module responsible for the prepaid minute portion of its screen or workflow.
- **Runtime and size:** Client-bound through an importing client module; 244 lines.
- **Exports / surface:** `PrepaidMinute`, `PrepaidMinute`, `handleCancelReasonConfirm(reason: string)`, `handleCancelSub`, `autoRenewHandler`, `handleReactiveClick`, `handleReactiveSubscription`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** `useUserInfo`, `useSubscription`, `useStorage`, `useCancelAllSubMutation`, `useAutoRenewSubMutation`, `useAddWatcherMutation`, `useUnPauseSubscriptionMutation`, `useUserAllInfoQuery`, `useOldSubPause`, `useState`; state bindings `isCancelReasonOpen`, `setCancelReasonOpen`, `isCancelDialogOpen`, `setCancelDialogOpen`, `cancelReason`, `setCancelReason`, `isDialogOpen`, `setIsDialogOpen`, `isReactiveDialogOpen`, `setReactiveDialogOpen`; contexts None.
- **Dependencies:** internal `src/components/ui/button.tsx`, `src/components/ui/card.tsx`, `src/components/features/subscription/storage-utilized.tsx`, `src/components/shared/dialog/alert-dialog.tsx`, `src/components/features/subscription/cancel-reason-dialog.tsx`, `src/utils/notifications.ts`, `src/components/ui/switch.tsx`, `src/store/api/subscription.ts`, plus 9 more; external `react`, `lucide-react`, `sonner`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/components/features/subscription/subscription.tsx`.
- **Improvement / caution:** Consider a reducer/state machine to make transitions explicit.

#### `src/components/features/subscription/subscription-details/terminated-new-sub.tsx`

- **Why it exists / responsibility:** Subscription feature module responsible for the terminated new sub portion of its screen or workflow.
- **Runtime and size:** Client-bound through an importing client module; 124 lines.
- **Exports / surface:** `TerminatedNewSub`, `TerminatedNewSub`, `handleConfirm`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** `useUserInfo`, `useReactiveAllSubMutation`, `useSubscriptionV2`, `useState`, `useStorageV2`; state bindings `isConfirmDialogOpen`, `setIsConfirmDialogOpen`; contexts None.
- **Dependencies:** internal `src/components/shared/dialog/alert-dialog.tsx`, `src/components/ui/button.tsx`, `src/components/ui/progress.tsx`, `src/hooks/use-storageV2.ts`, `src/hooks/use-subscription-v2.ts`, `src/hooks/use-user-info.ts`, `src/store/api/subscription.ts`, `src/utils/date-formatter.ts`; external `react`, `lucide-react`, `sonner`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/components/features/subscription/subscription.tsx`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/components/features/subscription/subscription-details/terminated-sub.tsx`

- **Why it exists / responsibility:** Subscription feature module responsible for the terminated sub portion of its screen or workflow.
- **Runtime and size:** Client module; 127 lines.
- **Exports / surface:** `TerminatedSub`, `TerminatedSub`, `handleConfirm`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** `useReactiveAllSubMutation`, `useSubscription`, `useUserInfo`, `useMinute`, `useState`; state bindings `isConfirmDialogOpen`, `setIsConfirmDialogOpen`; contexts None.
- **Dependencies:** internal `src/components/ui/button.tsx`, `src/components/features/subscription/storage-utilized.tsx`, `src/components/shared/dialog/alert-dialog.tsx`, `src/hooks/use-subscription.ts`, `src/store/api/subscription.ts`, `src/store/api/api-key.ts`, `src/hooks/use-customer-id.ts`, `src/utils/date-formatter.ts`, plus 3 more; external `react`, `lucide-react`, `sonner`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/components/features/subscription/subscription.tsx`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/components/features/subscription/subscription-details/trial.tsx`

- **Why it exists / responsibility:** Subscription feature module responsible for the trial portion of its screen or workflow.
- **Runtime and size:** Client module; 105 lines.
- **Exports / surface:** `Trial`, `Trial`, `handleConfirm`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** `useTrial`, `useState`; state bindings `isConfirmDialogOpen`, `setIsConfirmDialogOpen`, `promoCode`, `setPromoCode`; contexts None.
- **Dependencies:** internal `src/components/ui/button.tsx`, `src/components/ui/card.tsx`, `src/components/features/subscription/storage-utilized.tsx`, `src/components/shared/dialog/alert-dialog.tsx`, `src/components/ui/input.tsx`, `src/hooks/use-trial.ts`; external `react`, `next/link`, `lucide-react`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** None; static analysis found no internal importer, so verify framework entry/dynamic use.
- **Improvement / caution:** Confirm whether this is a framework root, planned module, or dead code before retaining it.

#### `src/components/features/subscription/subscription-summary/cancel-subscription.tsx`

- **Why it exists / responsibility:** Subscription feature module responsible for the cancel subscription portion of its screen or workflow.
- **Runtime and size:** Client module; 61 lines.
- **Exports / surface:** `CancelSubscription`, `CancelSubscription`, `handleUpdateClick`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** `useState`, `useUserInfo`; state bindings `activeTab`, `setActiveTab`, `modalOpen`, `setModalOpen`; contexts None.
- **Dependencies:** internal `src/components/ui/button.tsx`, `src/components/ui/card.tsx`, `src/components/features/my-account/my-account.tsx`, `src/hooks/use-user-info.ts`; external `react`, `lucide-react`, `sonner`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/components/features/subscription/subscription-summary/subscription-summary-section.tsx`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/components/features/subscription/subscription-summary/expired-subscription.tsx`

- **Why it exists / responsibility:** Subscription feature module responsible for the expired subscription portion of its screen or workflow.
- **Runtime and size:** Client module; 54 lines.
- **Exports / surface:** `ExpiredSubscription`, `ExpiredSubscription`, `handleBuyClick`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** `useRouter`; state bindings None; contexts None.
- **Dependencies:** internal `src/components/ui/button.tsx`, `src/components/ui/progress.tsx`, `src/config/page.ts`; external `next/navigation`, `lucide-react`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/components/features/subscription/subscription-summary/subscription-summary-section.tsx`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/components/features/subscription/subscription-summary/ppccu-subscription.tsx`

- **Why it exists / responsibility:** Subscription feature module responsible for the ppccu subscription portion of its screen or workflow.
- **Runtime and size:** Client module; 85 lines.
- **Exports / surface:** `PpccuSubscription`, `PpccuSubscription`, `handleUpdateClick`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** `useSubscription`, `useStorage`, `useMinute`, `useState`, `useUserInfo`; state bindings `activeTab`, `setActiveTab`, `modalOpen`, `setModalOpen`; contexts None.
- **Dependencies:** internal `src/components/ui/button.tsx`, `src/components/ui/card.tsx`, `src/components/ui/progress.tsx`, `src/hooks/use-subscription.ts`, `src/hooks/use-minute.ts`, `src/hooks/use-storage.ts`, `src/components/features/my-account/my-account.tsx`, `src/hooks/use-user-info.ts`; external `react`, `lucide-react`, `sonner`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/components/features/subscription/subscription-summary/subscription-summary-section.tsx`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/components/features/subscription/subscription-summary/ppl-subscription.tsx`

- **Why it exists / responsibility:** Subscription feature module responsible for the ppl subscription portion of its screen or workflow.
- **Runtime and size:** Client module; 85 lines.
- **Exports / surface:** `PplSubscription`, `PplSubscription`, `handleUpdateClick`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** `useSubscription`, `useStorage`, `useMinute`, `useState`, `useUserInfo`; state bindings `activeTab`, `setActiveTab`, `modalOpen`, `setModalOpen`; contexts None.
- **Dependencies:** internal `src/components/ui/button.tsx`, `src/components/ui/card.tsx`, `src/components/ui/progress.tsx`, `src/hooks/use-subscription.ts`, `src/hooks/use-storage.ts`, `src/hooks/use-minute.ts`, `src/components/features/my-account/my-account.tsx`, `src/hooks/use-user-info.ts`; external `react`, `lucide-react`, `sonner`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/components/features/subscription/subscription-summary/subscription-summary-section.tsx`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/components/features/subscription/subscription-summary/ppm-subscription.tsx`

- **Why it exists / responsibility:** Subscription feature module responsible for the ppm subscription portion of its screen or workflow.
- **Runtime and size:** Client module; 85 lines.
- **Exports / surface:** `PpmSubscription`, `PpmSubscription`, `handleUpdateClick`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** `useSubscription`, `useStorage`, `useMinute`, `useState`, `useUserInfo`; state bindings `activeTab`, `setActiveTab`, `modalOpen`, `setModalOpen`; contexts None.
- **Dependencies:** internal `src/components/ui/button.tsx`, `src/components/ui/card.tsx`, `src/components/ui/progress.tsx`, `src/hooks/use-subscription.ts`, `src/hooks/use-minute.ts`, `src/hooks/use-storage.ts`, `src/components/features/my-account/my-account.tsx`, `src/hooks/use-user-info.ts`; external `react`, `lucide-react`, `sonner`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/components/features/subscription/subscription-summary/subscription-summary-section.tsx`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/components/features/subscription/subscription-summary/prepaid-minute.tsx`

- **Why it exists / responsibility:** Subscription feature module responsible for the prepaid minute portion of its screen or workflow.
- **Runtime and size:** Client-bound through an importing client module; 99 lines.
- **Exports / surface:** `PrepaidMinute`, `PrepaidMinute`, `handleUpdateClick`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** `useUserInfo`, `useSubscription`, `useStorage`, `useUserAllInfoQuery`, `useState`; state bindings `activeTab`, `setActiveTab`, `modalOpen`, `setModalOpen`; contexts None.
- **Dependencies:** internal `src/components/ui/button.tsx`, `src/components/ui/card.tsx`, `src/hooks/use-storage.ts`, `src/store/api/user-info.ts`, `src/hooks/use-user-info.ts`, `src/hooks/use-subscription.ts`, `src/components/features/my-account/my-account.tsx`; external `react`, `lucide-react`, `sonner`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/components/features/subscription/subscription-summary/subscription-summary-section.tsx`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/components/features/subscription/subscription-summary/subscription-summary-section.tsx`

- **Why it exists / responsibility:** Subscription feature module responsible for the subscription summary section portion of its screen or workflow.
- **Runtime and size:** Client module; 68 lines.
- **Exports / surface:** `SubscriptionSummarySection`, `SubscriptionSummarySection`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** `useSubscription`, `useTrial`, `useSubscriptionV2`, `useTrialV2`, `useHasPauseSubscription`, `useOldSubPause`, `useStorage`; state bindings None; contexts None.
- **Dependencies:** internal `src/hooks/use-subscription.ts`, `src/components/features/subscription/subscription-summary/cancel-subscription.tsx`, `src/components/features/subscription/subscription-summary/expired-subscription.tsx`, `src/components/features/subscription/subscription-summary/ppccu-subscription.tsx`, `src/components/features/subscription/subscription-summary/trial-subscription.tsx`, `src/constant/plan.ts`, `src/components/features/subscription/subscription-summary/prepaid-minute.tsx`, `src/components/features/subscription/subscription-summary/ppl-subscription.tsx`, plus 12 more; external None.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/components/features/streaming-app/streaming-app-page.tsx`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/components/features/subscription/subscription-summary/trial-subscription.tsx`

- **Why it exists / responsibility:** Subscription feature module responsible for the trial subscription portion of its screen or workflow.
- **Runtime and size:** Client module; 90 lines.
- **Exports / surface:** `TrialSubscription`, `TrialSubscription`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** `useTrial`, `useStorage`, `useMinute`; state bindings None; contexts None.
- **Dependencies:** internal `src/components/ui/button.tsx`, `src/components/ui/card.tsx`, `src/components/ui/progress.tsx`, `src/hooks/use-trial.ts`, `src/hooks/use-minute.ts`, `src/hooks/use-storage.ts`; external `lucide-react`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/components/features/subscription/subscription-summary/subscription-summary-section.tsx`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/components/features/subscription/subscription.tsx`

- **Why it exists / responsibility:** Subscription feature module responsible for the subscription portion of its screen or workflow.
- **Runtime and size:** Client-bound through an importing client module; 63 lines.
- **Exports / surface:** `Subscription`, `Subscription`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** `useSubscription`, `useSubscriptionV2`, `useTrialV2`, `useTrial`; state bindings None; contexts None.
- **Dependencies:** internal `src/hooks/use-subscription.ts`, `src/hooks/use-trial.ts`, `src/constant/plan.ts`, `src/components/features/subscription/subscription-details/ppccu.tsx`, `src/components/features/subscription/subscription-details/ppl.tsx`, `src/components/features/subscription/subscription-details/prepaid-minute.tsx`, `src/components/features/subscription/subscription-details/terminated-sub.tsx`, `src/components/features/subscription/subscription-details/ppm.tsx`, plus 7 more; external None.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/components/features/my-account/my-account.tsx`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/components/features/team/invited-tab.tsx`

- **Why it exists / responsibility:** Team feature module responsible for the invited tab portion of its screen or workflow.
- **Runtime and size:** Client module; 60 lines.
- **Exports / surface:** `InvitedTab`, `InvitedTab`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** `useTeamInfoQuery`, `useMemo`; state bindings None; contexts None.
- **Dependencies:** internal `src/components/ui/avatar.tsx`, `src/store/api/auth.ts`, `src/components/shared/common-table.tsx`; external `react`, `@tanstack/react-table`, `lucide-react`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/components/features/team/team-page.tsx`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/components/features/team/my-team-tab.tsx`

- **Why it exists / responsibility:** Team feature module responsible for the my team tab portion of its screen or workflow.
- **Runtime and size:** Client module; 153 lines.
- **Exports / surface:** `MyTeamTab`, `MyTeamTab`, `handleInvite`, `handleRemoveMember`, `handleRemovingAction(email: string)`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** `useInvitedTeamMemberQuery`, `useRemoveTeamMemberMutation`, `useInviteNewMemberMutation`, `useState`, `useMemo`; state bindings `isInviteModalOpen`, `setIsInviteModalOpen`, `isRemoveModalOpen`, `setIsRemoveModalOpen`, `newEmail`, `setNewEmail`, `emailToRemove`, `setEmailToRemove`; contexts None.
- **Dependencies:** internal `src/components/ui/avatar.tsx`, `src/components/ui/button.tsx`, `src/components/ui/input.tsx`, `src/store/api/auth.ts`, `src/components/shared/dialog/alert-dialog.tsx`, `src/components/shared/common-table.tsx`, `src/utils/validation.ts`; external `react`, `@tanstack/react-table`, `lucide-react`, `sonner`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/components/features/team/team-page.tsx`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/components/features/team/team-page.tsx`

- **Why it exists / responsibility:** Team feature module responsible for the team page portion of its screen or workflow.
- **Runtime and size:** Client module; 52 lines.
- **Exports / surface:** `TeamPage`, `TeamPage`, `renderContent`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** `useState`, `useUserInfo`; state bindings `activeTab`, `setActiveTab`; contexts None.
- **Dependencies:** internal `src/components/shared/tabs.tsx`, `src/components/features/team/my-team-tab.tsx`, `src/components/features/team/invited-tab.tsx`, `src/components/ui/headlines/page-title.tsx`, `src/hooks/use-user-info.ts`; external `react`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/app/(withSidebarLayout)/team/page.tsx`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/components/features/username/username-page.tsx`

- **Why it exists / responsibility:** Username feature module responsible for the username page portion of its screen or workflow.
- **Runtime and size:** Client module; 331 lines.
- **Exports / surface:** `UsernamePage`, `UsernamePage`, `handleSubmit(e: React.FormEvent)`, `handleLogout`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** `useRouter`, `useUserInfo`, `useChangeUsernameMutation`, `useUpdatePhoneNumberMutation`, `useLogoutMutation`, `useLoginStatusQuery`, `useState`, `useEffect`; state bindings `username`, `setUsername`, `phoneNumber`, `setPhoneNumber`, `hearAbout`, `setHearAbout`, `newsletter`, `setNewsletter`, `usernameFormatError`, `setUsernameFormatError`, `mobileWarning`, `setMobileWarning`, plus 4 more; contexts None.
- **Dependencies:** internal `src/components/ui/button.tsx`, `src/components/ui/checkbox.tsx`, `src/assets/index.ts`, `src/app/fonts.ts`, `src/hooks/use-user-info.ts`, `src/components/ui/field/custom-input.tsx`, `src/utils/validation.ts`, `src/store/api/auth.ts`, plus 2 more; external `react`, `next/image`, `next/navigation`, `lucide-react`, `react-phone-number-input`, `sonner`, `react-phone-number-input/style.css`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/app/(with-public-layout)/username/page.tsx`.
- **Improvement / caution:** Decompose by responsibility and add focused tests before changing behavior.

#### `src/components/features/utilities/download-content.tsx`

- **Why it exists / responsibility:** Utilities feature module responsible for the download content portion of its screen or workflow.
- **Runtime and size:** Client-bound through an importing client module; 90 lines.
- **Exports / surface:** `DownloadContent`, `DownloadContent`, `downloadBuilds`, `downloadElLauncher`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** None; state bindings None; contexts None.
- **Dependencies:** internal `src/components/ui/button.tsx`, `src/config/url.ts`; external `lucide-react`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/components/features/utilities/utilities-page.tsx`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/components/features/utilities/multiplayer-app-monitor.tsx`

- **Why it exists / responsibility:** Utilities feature module responsible for the multiplayer app monitor portion of its screen or workflow.
- **Runtime and size:** Client module; 176 lines.
- **Exports / surface:** `MultiplayerAppMonitor`, `MultiplayerAppMonitor`, `startMonitor`, `handleStopServer(server: ServerInstance)`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** `useUserInfo`, `useState`, `useDsInstanceStopMutation`, `useRef`, `useEffect`, `useMemo`; state bindings `dsMonitorData`, `setDsMonitorData`, `loading`, `setLoading`; contexts None.
- **Dependencies:** internal `src/components/ui/button.tsx`, `src/components/shared/common-table.tsx`, `src/config/firebase.ts`, `src/hooks/use-user-info.ts`, `src/store/api/upload.ts`; external `react`, `@tanstack/react-table`, `firebase/auth`, `firebase/firestore`, `sonner`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/components/features/dedicated-server/dedicated-server-page.tsx`, `src/components/features/utilities/utilities-page.tsx`.
- **Improvement / caution:** Keep effect dependencies complete and verify cleanup under React Strict Mode.

#### `src/components/features/utilities/remote-editior-streaming.tsx`

- **Why it exists / responsibility:** Utilities feature module responsible for the remote editior streaming portion of its screen or workflow.
- **Runtime and size:** Client-bound through an importing client module; 66 lines.
- **Exports / surface:** `RemoteEditorStreaming`, `RemoteEditorStreaming`, `handleRemoteEditorControl`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** `useUserInfo`, `useConfigListQuery`, `useState`; state bindings `selectedConfig`, `setSelectedConfig`; contexts None.
- **Dependencies:** internal `src/components/ui/button.tsx`, `src/components/ui/select.tsx`, `src/hooks/use-user-info.ts`, `src/store/api/appLink.ts`; external `react`, `lucide-react`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/components/features/utilities/utilities-page.tsx`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/components/features/utilities/streaming-app-monitor.tsx`

- **Why it exists / responsibility:** Utilities feature module responsible for the streaming app monitor portion of its screen or workflow.
- **Runtime and size:** Client module; 193 lines.
- **Exports / surface:** `StreamingAppMonitor`, `StreamingAppMonitor`, `handleKickPlayer(stream: StreamInstance)`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** `useUserInfo`, `useKickPlayerMutation`, `useState`, `useRef`, `useEffect`, `useMemo`; state bindings `streamMonitorData`, `setStreamMonitorData`, `loading`, `setLoading`; contexts None.
- **Dependencies:** internal `src/components/ui/button.tsx`, `src/components/shared/common-table.tsx`, `src/config/firebase.ts`, `src/hooks/use-user-info.ts`, `src/store/api/upload.ts`; external `react`, `@tanstack/react-table`, `firebase/auth`, `firebase/firestore`, `sonner`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/components/features/streaming-app/streaming-app-section.tsx`, `src/components/features/utilities/utilities-page.tsx`.
- **Improvement / caution:** Keep effect dependencies complete and verify cleanup under React Strict Mode.

#### `src/components/features/utilities/utilities-page.tsx`

- **Why it exists / responsibility:** Utilities feature module responsible for the utilities page portion of its screen or workflow.
- **Runtime and size:** Client module; 52 lines.
- **Exports / surface:** `UtilitiesPage`, `UtilitiesPage`, `renderContent`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** `useState`; state bindings `activeTab`, `setActiveTab`; contexts None.
- **Dependencies:** internal `src/components/shared/tabs.tsx`, `src/components/features/utilities/streaming-app-monitor.tsx`, `src/components/features/utilities/download-content.tsx`, `src/components/features/utilities/multiplayer-app-monitor.tsx`, `src/components/ui/headlines/page-title.tsx`, `src/components/features/utilities/remote-editior-streaming.tsx`; external `react`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/app/(withSidebarLayout)/utilities/page.tsx`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

### src/components/navbar

#### `src/components/navbar/header.tsx`

- **Why it exists / responsibility:** Header navbar/header subcomponent.
- **Runtime and size:** Client-bound through an importing client module; 121 lines.
- **Exports / surface:** `Header`, `Header`, `handleProfileClick`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** `useRouter`, `useUserInfo`, `useTeamInfoQuery`, `useProfileLogoQuery`, `useState`; state bindings `modalOpen`, `setModalOpen`; contexts None.
- **Dependencies:** internal `src/components/ui/avatar.tsx`, `src/components/ui/select.tsx`, `src/components/ui/sidebar.tsx`, `src/components/navbar/notification-dropdown.tsx`, `src/components/features/my-account/my-account.tsx`, `src/hooks/use-user-info.ts`, `src/store/api/user-info.ts`, `src/store/api/auth.ts`, plus 1 more; external `react`, `next/navigation`, `sonner`, `lucide-react`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/app/(withSidebarLayout)/layout.tsx`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/components/navbar/mode-toggle.tsx`

- **Why it exists / responsibility:** Mode Toggle navbar/header subcomponent.
- **Runtime and size:** Client module; 25 lines.
- **Exports / surface:** `ModeToggle`, `ModeToggle`, `toggleTheme`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** `useTheme`; state bindings None; contexts None.
- **Dependencies:** internal `src/components/ui/button.tsx`; external `react`, `lucide-react`, `next-themes`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** None; static analysis found no internal importer, so verify framework entry/dynamic use.
- **Improvement / caution:** Confirm whether this is a framework root, planned module, or dead code before retaining it.

#### `src/components/navbar/notification-dropdown.tsx`

- **Why it exists / responsibility:** Notification Dropdown navbar/header subcomponent.
- **Runtime and size:** Client module; 200 lines.
- **Exports / surface:** `NotificationDropdown`, `NotificationDropdown`, `handleMarkAllRead`, `handleRemove(id: string)`, `handleClearAll`, `backHandler`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** `useUserInfo`, `useNotificationQuery`, `useRemoveSingleNotificationMutation`, `useClearAllNotificationMutation`, `useReadAllNotificationMutation`, `useState`, `useMemo`, `useEffect`; state bindings `modalOpen`, `setModalOpen`, `dropdownOpen`, `setDropdownOpen`; contexts None.
- **Dependencies:** internal `src/components/ui/button.tsx`, `src/components/ui/badge.tsx`, `src/components/ui/dropdown-menu.tsx`, `src/store/api/user-info.ts`, `src/hooks/use-user-info.ts`, `src/components/shared/dialog/custom-dialog.tsx`, `src/components/navbar/notification-view-all.tsx`, `src/assets/index.ts`; external `react`, `next/image`, `lucide-react`, `sonner`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/components/navbar/header.tsx`.
- **Improvement / caution:** Keep effect dependencies complete and verify cleanup under React Strict Mode.

#### `src/components/navbar/notification-view-all.tsx`

- **Why it exists / responsibility:** Notification View All navbar/header subcomponent.
- **Runtime and size:** Client module; 110 lines.
- **Exports / surface:** `NotificationViewAll`, `NotificationViewAll({
  notificationList,
  isFetching,
  handleClearAll,
  handleRemove,
  backHandler
}: NotificationViewAllProps)`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** None; state bindings None; contexts None.
- **Dependencies:** internal `src/components/ui/button.tsx`, `src/components/shared/common-table.tsx`, `src/components/ui/dialog.tsx`, `src/components/shared/dialog/dialog-header.tsx`; external `lucide-react`, `@tanstack/react-table`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/components/navbar/notification-dropdown.tsx`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/components/navbar/ppccu-subscription.tsx`

- **Why it exists / responsibility:** Ppccu Subscription navbar/header subcomponent.
- **Runtime and size:** Client-bound through an importing client module; 60 lines.
- **Exports / surface:** `PpccuSubscription`, `PpccuSubscription`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** None; state bindings None; contexts None.
- **Dependencies:** internal `src/components/ui/button.tsx`, `src/components/ui/card.tsx`, `src/components/ui/progress.tsx`; external `lucide-react`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** None; static analysis found no internal importer, so verify framework entry/dynamic use.
- **Improvement / caution:** Confirm whether this is a framework root, planned module, or dead code before retaining it.

### src/components/providers

#### `src/components/providers/config-provider.tsx`

- **Why it exists / responsibility:** Config Provider context/provider module that owns cross-component workflow state and side effects.
- **Runtime and size:** Client module; 64 lines.
- **Exports / surface:** `ConfigProvider`, `useConfigContext`, `ConfigProvider({ children }: { children: ReactNode })`, `resetConfig`, `useConfigContext`.
- **Props / inputs visible at declarations:** `ConfigProvider: { children }: { children: ReactNode }`.
- **Hooks / local state:** `useState`, `useContext`; state bindings `breadCrumb`, `setBreadCrumb`, `screen`, `setScreen`, `selectedConfig`, `setSelectedConfig`, `selectedField`, `setSelectedField`; contexts `Config`.
- **Dependencies:** internal None; external `react`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/app/(withSidebarLayout)/layout.tsx`, `src/components/features/streaming-app/application.tsx`, `src/components/features/streaming-app/config/config-settings-form.tsx`, `src/components/features/streaming-app/config/customization-tab.tsx`, `src/components/features/streaming-app/config/image-asset-list.tsx`, `src/components/features/streaming-app/config/image-upload.tsx`, `src/components/features/streaming-app/config/video-asset-list.tsx`, `src/components/features/streaming-app/meeting-link-tab.tsx`, plus 2 more.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/components/providers/providers.tsx`

- **Why it exists / responsibility:** Providers context/provider module that owns cross-component workflow state and side effects.
- **Runtime and size:** Client module; 18 lines.
- **Exports / surface:** `default`, `Providers({ children }: TChildrenProps)`.
- **Props / inputs visible at declarations:** `Providers: { children }: TChildrenProps`.
- **Hooks / local state:** None; state bindings None; contexts None.
- **Dependencies:** internal `src/types/common.ts`, `src/components/providers/theme-provider.tsx`, `src/store/store.ts`; external `react-redux`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/app/layout.tsx`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/components/providers/theme-provider.tsx`

- **Why it exists / responsibility:** Theme Provider context/provider module that owns cross-component workflow state and side effects.
- **Runtime and size:** Client module; 14 lines.
- **Exports / surface:** `ThemeProvider`, `ThemeProvider({ children }: { children: ReactNode })`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** None; state bindings None; contexts None.
- **Dependencies:** internal None; external `react`, `next-themes`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/components/providers/providers.tsx`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/components/providers/upload/additional-upload-provider.tsx`

- **Why it exists / responsibility:** Additional Upload Provider context/provider module that owns cross-component workflow state and side effects.
- **Runtime and size:** Client-bound through an importing client module; 167 lines.
- **Exports / surface:** `AdditionalUploadProvider`, `useAdditionalUpload`, `AdditionalUploadProvider({ children }: { children: ReactNode })`, `resetState`, `handleCancel`, `handleUpload`, `useAdditionalUpload`.
- **Props / inputs visible at declarations:** `AdditionalUploadProvider: { children }: { children: ReactNode }`.
- **Hooks / local state:** `useUserInfo`, `useAdditionalUploadAppListQuery`, `useAdditionalUploadAppSignedUrlMutation`, `useState`, `useContext`; state bindings `modalOpen`, `setModalOpen`, `step`, `setStep`, `file`, `setFile`, `zipFile`, `setZipFile`, `appName`, `setAppName`, `isUploading`, `setIsUploading`, plus 8 more; contexts `UploadContext`.
- **Dependencies:** internal `src/store/api/upload.ts`, `src/utils/size-formatter.ts`, `src/hooks/use-user-info.ts`, `src/components/features/streaming-app/api-notify.ts`; external `react`, `axios`, `sonner`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/app/(withSidebarLayout)/layout.tsx`, `src/components/features/additional-upload/additional-upload-details.tsx`, `src/components/features/additional-upload/additional-upload-page.tsx`.
- **Improvement / caution:** Consider a reducer/state machine to make transitions explicit.

#### `src/components/providers/upload/apiCall.ts`

- **Why it exists / responsibility:** Api Call context/provider module that owns cross-component workflow state and side effects.
- **Runtime and size:** Shared/build-time eligible module; 222 lines.
- **Exports / surface:** `linuxBuildMessage`, `doContainRestrictedCharacters`, `generateUUID`, `createNotification`, `notifyFirstUpload`, `sendMailAfterUpload`, `uploadSequenceSuccessNotify`, `uploadSequenceFailureNotify`, `uploadSequenceStuckNotify`, `signedUrlChangeNotify`, `linuxBuildMessage(data: any)`, `doContainRestrictedCharacters(text)`, plus 8 more.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** None; state bindings None; contexts None.
- **Dependencies:** internal `src/config/api.ts`, `src/config/url.ts`, `src/config/firebase.ts`, `src/hooks/use-streaming-app.ts`; external `axios`, `firebase/firestore`.
- **Rendering and rerenders:** No JSX detected; it runs when imported or when an exported function/hook is called.
- **Direct consumers:** `src/components/providers/upload/upload-sequence-provider.tsx`.
- **Improvement / caution:** Confirm cancellation, error normalization, and cache synchronization for direct Axios traffic.

#### `src/components/providers/upload/dedicated-server-upload-provider.tsx`

- **Why it exists / responsibility:** Dedicated Server Upload Provider context/provider module that owns cross-component workflow state and side effects.
- **Runtime and size:** Client-bound through an importing client module; 164 lines.
- **Exports / surface:** `DedicatedServerUploadProvider`, `useDedicatedServerUpload`, `DedicatedServerUploadProvider({ children }: { children: ReactNode })`, `resetState`, `handleCancel`, `handleUpload`, `useDedicatedServerUpload`.
- **Props / inputs visible at declarations:** `DedicatedServerUploadProvider: { children }: { children: ReactNode }`.
- **Hooks / local state:** `useUserInfo`, `useDedicatedServerAppUploadLinkMutation`, `useDsAppQuery`, `useState`, `useContext`; state bindings `modalOpen`, `setModalOpen`, `step`, `setStep`, `zipFile`, `setZipFile`, `appName`, `setAppName`, `isUploading`, `setIsUploading`, `uploadProgress`, `setUploadProgress`, plus 6 more; contexts `UploadContext`.
- **Dependencies:** internal `src/store/api/upload.ts`, `src/hooks/use-user-info.ts`, `src/store/api/asset.ts`, `src/components/features/streaming-app/api-notify.ts`; external `react`, `axios`, `sonner`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/app/(withSidebarLayout)/layout.tsx`, `src/components/features/dedicated-server/dedicated-server-app-upload.tsx`, `src/components/features/dedicated-server/dedicated-server-upload-details.tsx`.
- **Improvement / caution:** Consider a reducer/state machine to make transitions explicit.

#### `src/components/providers/upload/upload-sequence-provider.tsx`

- **Why it exists / responsibility:** Upload Sequence Provider context/provider module that owns cross-component workflow state and side effects.
- **Runtime and size:** Client-bound through an importing client module; 1236 lines.
- **Exports / surface:** `UploadSequenceProvider`, `useUploadSequence`, `UploadSequenceProvider({ children }: { children: ReactNode })`, `cleanupListener`, `clearLowSpeedTimer`, `showClickToDismissToast(message: string)`, `handleClick`, `resetState`, `uploadSuccessFullState`, `handleUpdate(appName: string)`, `handleCancel`, `confirmCancelHandler`, plus 11 more.
- **Props / inputs visible at declarations:** `UploadSequenceProvider: { children }: { children: ReactNode }`.
- **Hooks / local state:** `useUserInfo`, `useConfigListQuery`, `useStreamingAppQuery`, `useStreamingAppThumbnailQuery`, `useNotificationQuery`, `useStorageStatusQuery`, `useStorageUtilizedQuery`, `useCheckStatusQuery`, `useCreateConfigMutation`, `useUploadStreamingAppSignedUrlMutation`, `useAltUploadStreamingAppSignedUrlMutation`, `useExeInfoUploadMutation`, plus 7 more; state bindings `modalOpen`, `setModalOpen`, `step`, `setStep`, `zipFile`, `setZipFile`, `appName`, `setAppName`, `isUploading`, `setIsUploading`, `loadingState`, `setLoadingState`, plus 42 more; contexts `UploadContext`.
- **Dependencies:** internal `src/utils/size-formatter.ts`, `src/store/api/upload.ts`, `src/config/firebase.ts`, `src/hooks/use-user-info.ts`, `src/store/api/streaming-app.ts`, `src/components/features/streaming-app/normalizeAppExeData.ts`, `src/utils/upload.ts`, `src/components/providers/upload/apiCall.ts`, plus 12 more; external `react`, `axios`, `sonner`, `firebase/firestore`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/app/(withSidebarLayout)/layout.tsx`, `src/app/(withSidebarLayout)/upload/UploadError.tsx`, `src/app/(withSidebarLayout)/upload/app-upload-modal.tsx`, `src/app/(withSidebarLayout)/upload/sequence.tsx`, `src/app/(withSidebarLayout)/upload/upload-details.tsx`, `src/components/features/dedicated-server/application.tsx`, `src/components/features/streaming-app/application.tsx`, `src/components/features/streaming-app/streaming-app-section.tsx`.
- **Improvement / caution:** Split transport, ZIP validation, Firestore processing, telemetry, and UI state; persist recoverable workflow state.

### src/components/shared

#### `src/components/shared/app-display-card.tsx`

- **Why it exists / responsibility:** Reusable App Display Card presentation or interaction shared across multiple product features.
- **Runtime and size:** Client module; 176 lines.
- **Exports / surface:** `AppDisplayCard`, `AppDisplayCard`, `ActionMenu({}: { app: App })`.
- **Props / inputs visible at declarations:** `ActionMenu: {}: { app: App }`.
- **Hooks / local state:** `useState`; state bindings `viewMode`, `setViewMode`; contexts None.
- **Dependencies:** internal `src/components/ui/card.tsx`, `src/components/ui/button.tsx`, `src/components/ui/table.tsx`, `src/components/ui/dropdown-menu.tsx`; external `react`, `next/image`, `lucide-react`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** None; static analysis found no internal importer, so verify framework entry/dynamic use.
- **Improvement / caution:** Confirm whether this is a framework root, planned module, or dead code before retaining it.

#### `src/components/shared/common-table.tsx`

- **Why it exists / responsibility:** Reusable Common Table presentation or interaction shared across multiple product features.
- **Runtime and size:** Client module; 233 lines.
- **Exports / surface:** `CommonTable`, `CommonTable({
  columns,
  data,
  showToolbar = false,
  showPagination = true,
  filterPlaceholder,
  pageSizeOptions = [5, 20, 30, 40, 50],
  isLoading = false
}: CommonTableProps<TData, TValue>)`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** `useState`, `useReactTable`; state bindings `sorting`, `setSorting`, `columnFilters`, `setColumnFilters`, `globalFilter`, `setGlobalFilter`, `columnVisibility`, `setColumnVisibility`, `rowSelection`, `setRowSelection`; contexts None.
- **Dependencies:** internal `src/components/ui/table.tsx`, `src/components/ui/button.tsx`, `src/components/ui/input.tsx`, `src/components/ui/dropdown-menu.tsx`, `src/components/ui/select.tsx`, `src/utils/common.ts`; external `react`, `@tanstack/react-table`, `lucide-react`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/components/features/additional-upload/additional-upload-page.tsx`, `src/components/features/analytics/ccu-tab.tsx`, `src/components/features/analytics/streaming-min-tab.tsx`, `src/components/features/dedicated-server/dedicated-server-card-details.tsx`, `src/components/features/my-account/transaction.tsx`, `src/components/features/team/invited-tab.tsx`, `src/components/features/team/my-team-tab.tsx`, `src/components/features/utilities/multiplayer-app-monitor.tsx`, plus 2 more.
- **Improvement / caution:** Consider a reducer/state machine to make transitions explicit.

#### `src/components/shared/data-table.tsx`

- **Why it exists / responsibility:** Reusable Data Table presentation or interaction shared across multiple product features.
- **Runtime and size:** Client module; 183 lines.
- **Exports / surface:** `DataTable`, `DataTable`, `handlePageChange(page: number)`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** `useState`; state bindings `currentPage`, `setCurrentPage`; contexts None.
- **Dependencies:** internal `src/components/ui/table.tsx`, `src/components/ui/pagination.tsx`; external `react`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** None; static analysis found no internal importer, so verify framework entry/dynamic use.
- **Improvement / caution:** Confirm whether this is a framework root, planned module, or dead code before retaining it.

#### `src/components/shared/date-picker.tsx`

- **Why it exists / responsibility:** Reusable Date Picker presentation or interaction shared across multiple product features.
- **Runtime and size:** Client-bound through an importing client module; 23 lines.
- **Exports / surface:** `DateTimeInput`, `DateTimeInput({ label, value, onChange, type }: DateTimeInputProps)`.
- **Props / inputs visible at declarations:** `DateTimeInput: { label, value, onChange, type }: DateTimeInputProps`.
- **Hooks / local state:** None; state bindings None; contexts None.
- **Dependencies:** internal None; external `react`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/components/features/analytics/ccu-tab.tsx`, `src/components/features/analytics/streaming-min-tab.tsx`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/components/shared/dialog/alert-dialog.tsx`

- **Why it exists / responsibility:** Reusable Alert Dialog presentation or interaction shared across multiple product features.
- **Runtime and size:** Client module; 59 lines.
- **Exports / surface:** `AlertDialog`, `AlertDialog({
  open,
  onClose,
  onConfirm,
  title = 'Are You Sure You Want to Proceed?',
  children,
  confirmText = 'Confirm',
  cancelText = 'Cancel',
  isPending = false,
  disable = false
}: AlertDialogProps)`.
- **Props / inputs visible at declarations:** `AlertDialog: {
  open,
  onClose,
  onConfirm,
  title = 'Are You Sure You Want to Proceed?',
  children,
  confirmText = 'Confirm',
  cancelText = 'Cancel',
  isPending = false,
  disable = false
}: AlertDialogProps`.
- **Hooks / local state:** None; state bindings None; contexts None.
- **Dependencies:** internal `src/components/ui/dialog.tsx`, `src/components/ui/button.tsx`; external `react`, `lucide-react`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/app/(withSidebarLayout)/upload/app-upload-modal.tsx`, `src/components/features/dedicated-server/dedicated-server-card-details.tsx`, `src/components/features/developer-section/api-key-tab.tsx`, `src/components/features/developer-section/streaming-api-key-tab.tsx`, `src/components/features/developer-section/version-control-tab.tsx`, `src/components/features/my-account/profile-settings.tsx`, `src/components/features/my-account/upload-profile.tsx`, `src/components/features/new-subscription/core-plan-montly-storage.tsx`, plus 27 more.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/components/shared/dialog/custom-dialog.tsx`

- **Why it exists / responsibility:** Reusable Custom Dialog presentation or interaction shared across multiple product features.
- **Runtime and size:** Client module; 28 lines.
- **Exports / surface:** `CustomDialog`, `CustomDialog({ modalOpen, setModalOpen, children }: ResponsiveModalProps)`.
- **Props / inputs visible at declarations:** `CustomDialog: { modalOpen, setModalOpen, children }: ResponsiveModalProps`.
- **Hooks / local state:** None; state bindings None; contexts None.
- **Dependencies:** internal `src/components/ui/dialog.tsx`; external `react`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/app/(withSidebarLayout)/upload/app-upload-modal.tsx`, `src/components/features/additional-upload/additional-upload-page.tsx`, `src/components/features/dedicated-server/dedicated-server-app-upload.tsx`, `src/components/features/dedicated-server/dedicated-server-card-details.tsx`, `src/components/features/my-account/my-account.tsx`, `src/components/features/plan/new-pricing-card.tsx`, `src/components/features/plan/new-pricing.tsx`, `src/components/features/streaming-app/application.tsx`, plus 2 more.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/components/shared/dialog/dialog-header.tsx`

- **Why it exists / responsibility:** Reusable Dialog Header presentation or interaction shared across multiple product features.
- **Runtime and size:** Client module; 29 lines.
- **Exports / surface:** `DialogHeader`, `DialogHeader({ title, onBack }: DialogHeaderProps)`.
- **Props / inputs visible at declarations:** `DialogHeader: { title, onBack }: DialogHeaderProps`.
- **Hooks / local state:** None; state bindings None; contexts None.
- **Dependencies:** internal `src/app/fonts.ts`; external `react`, `lucide-react`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/app/(withSidebarLayout)/upload/app-upload-modal.tsx`, `src/components/features/additional-upload/additional-upload-page.tsx`, `src/components/features/dedicated-server/dedicated-server-app-upload.tsx`, `src/components/features/dedicated-server/dedicated-server-card-details.tsx`, `src/components/features/my-account/my-account.tsx`, `src/components/features/streaming-app/application.tsx`, `src/components/features/streaming-app/demo-apps/demo-application.tsx`, `src/components/navbar/notification-view-all.tsx`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/components/shared/dialog/pricing-dialog.tsx`

- **Why it exists / responsibility:** Reusable Pricing Dialog presentation or interaction shared across multiple product features.
- **Runtime and size:** Client module; 28 lines.
- **Exports / surface:** `PricingDialog`, `PricingDialog({ modalOpen, setModalOpen, children }: ResponsiveModalProps)`.
- **Props / inputs visible at declarations:** `PricingDialog: { modalOpen, setModalOpen, children }: ResponsiveModalProps`.
- **Hooks / local state:** None; state bindings None; contexts None.
- **Dependencies:** internal `src/components/ui/dialog.tsx`; external `react`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/components/features/plan/new-pricing-card.tsx`, `src/components/features/plan/new-pricing.tsx`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/components/shared/doc-link.tsx`

- **Why it exists / responsibility:** Reusable Doc Link presentation or interaction shared across multiple product features.
- **Runtime and size:** Client-bound through an importing client module; 20 lines.
- **Exports / surface:** `default`, `DocLink({ url }: titleHeader)`.
- **Props / inputs visible at declarations:** `DocLink: { url }: titleHeader`.
- **Hooks / local state:** None; state bindings None; contexts None.
- **Dependencies:** internal None; external `react`, `lucide-react`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/components/features/my-account/profile-settings.tsx`, `src/components/features/my-account/transaction.tsx`, `src/components/features/new-subscription/core-plan-montly.tsx`, `src/components/features/new-subscription/core-plan-yearly.tsx`, `src/components/features/new-subscription/new-payment-info.tsx`, `src/components/features/new-subscription/new-trial.tsx`, `src/components/features/streaming-app/config/common-tab.tsx`, `src/components/features/streaming-app/config/customization-tab.tsx`, plus 9 more.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/components/shared/file-drop-zone.tsx`

- **Why it exists / responsibility:** Reusable File Drop Zone presentation or interaction shared across multiple product features.
- **Runtime and size:** Client module; 162 lines.
- **Exports / surface:** `FileDropzone`, `FileDropzone({ file, onFileSelect, accept, className, fileType }: FileDropzoneProps)`, `handleFileValidation(selectedFile: File)`, `handleDragOver(event: React.DragEvent<HTMLDivElement>)`, `handleDragLeave(event: React.DragEvent<HTMLDivElement>)`, `handleDrop(event: React.DragEvent<HTMLDivElement>)`, `handleFileChange(event: React.ChangeEvent<HTMLInputElement>)`, `handleDropzoneClick`, `handleChooseDifferentFile(event: React.MouseEvent)`, `renderFileIcon`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** `useState`, `useId`; state bindings `isDragging`, `setIsDragging`; contexts None.
- **Dependencies:** internal `src/lib/utils.ts`; external `react`, `lucide-react`, `sonner`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/app/(withSidebarLayout)/upload/app-upload-modal.tsx`, `src/components/features/additional-upload/additional-upload-page.tsx`, `src/components/features/dedicated-server/dedicated-server-app-upload.tsx`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/components/shared/full-page-loader.tsx`

- **Why it exists / responsibility:** Reusable Full Page Loader presentation or interaction shared across multiple product features.
- **Runtime and size:** Client-bound through an importing client module; 45 lines.
- **Exports / surface:** `FullPageLoader`, `FullPageLoader`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** None; state bindings None; contexts None.
- **Dependencies:** internal None; external None.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** None; static analysis found no internal importer, so verify framework entry/dynamic use.
- **Improvement / caution:** Confirm whether this is a framework root, planned module, or dead code before retaining it.

#### `src/components/shared/loader.tsx`

- **Why it exists / responsibility:** Reusable Loader presentation or interaction shared across multiple product features.
- **Runtime and size:** Client-bound through an importing client module; 25 lines.
- **Exports / surface:** `Loader`, `Loader`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** None; state bindings None; contexts None.
- **Dependencies:** internal None; external `react-spinners`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/app/(withSidebarLayout)/layout.tsx`, `src/components/features/dedicated-server/dedicated-server-page.tsx`, `src/components/features/new-subscription/core-plan-montly.tsx`, `src/components/features/new-subscription/core-plan-yearly.tsx`, `src/components/features/new-subscription/new-trial.tsx`, `src/components/features/streaming-app/config/image-asset-list.tsx`, `src/components/features/streaming-app/config/video-asset-list.tsx`, `src/components/features/streaming-app/streaming-app-page.tsx`, plus 2 more.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/components/shared/logo/AppTestLogo.tsx`

- **Why it exists / responsibility:** Reusable App Test Logo presentation or interaction shared across multiple product features.
- **Runtime and size:** Client-bound through an importing client module; 28 lines.
- **Exports / surface:** `default`, `AppTestLogo({ color = '#706F6F', w = 34, h = 34 })`.
- **Props / inputs visible at declarations:** `AppTestLogo: { color = '#706F6F', w = 34, h = 34 }`.
- **Hooks / local state:** None; state bindings None; contexts None.
- **Dependencies:** internal None; external `react`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/app/(withSidebarLayout)/upload/sequence.tsx`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/components/shared/logo/AppTestQueueLogo.tsx`

- **Why it exists / responsibility:** Reusable App Test Queue Logo presentation or interaction shared across multiple product features.
- **Runtime and size:** Client-bound through an importing client module; 21 lines.
- **Exports / surface:** `default`, `AppTestQueueLogo({ color = '#706F6F', w = 36, h = 36 })`.
- **Props / inputs visible at declarations:** `AppTestQueueLogo: { color = '#706F6F', w = 36, h = 36 }`.
- **Hooks / local state:** None; state bindings None; contexts None.
- **Dependencies:** internal None; external `react`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/app/(withSidebarLayout)/upload/sequence.tsx`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/components/shared/logo/ExeLogo.tsx`

- **Why it exists / responsibility:** Reusable Exe Logo presentation or interaction shared across multiple product features.
- **Runtime and size:** Client-bound through an importing client module; 23 lines.
- **Exports / surface:** `ExeLogo`, `ExeLogo({ color = '#706F6F', w = 36, h = 36 })`.
- **Props / inputs visible at declarations:** `ExeLogo: { color = '#706F6F', w = 36, h = 36 }`.
- **Hooks / local state:** None; state bindings None; contexts None.
- **Dependencies:** internal None; external `react`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/app/(withSidebarLayout)/upload/sequence.tsx`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/components/shared/logo/LaunchApp.tsx`

- **Why it exists / responsibility:** Reusable Launch App presentation or interaction shared across multiple product features.
- **Runtime and size:** Client-bound through an importing client module; 15 lines.
- **Exports / surface:** `default`, `LaunchApp({ color = '#706F6F', w = 18, h = 17 })`.
- **Props / inputs visible at declarations:** `LaunchApp: { color = '#706F6F', w = 18, h = 17 }`.
- **Hooks / local state:** None; state bindings None; contexts None.
- **Dependencies:** internal None; external `react`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** None; static analysis found no internal importer, so verify framework entry/dynamic use.
- **Improvement / caution:** Confirm whether this is a framework root, planned module, or dead code before retaining it.

#### `src/components/shared/logo/SDDownloadLogo.tsx`

- **Why it exists / responsibility:** Reusable SDDownload Logo presentation or interaction shared across multiple product features.
- **Runtime and size:** Client-bound through an importing client module; 31 lines.
- **Exports / surface:** `default`, `SDDownloadLogo({ color = '#706F6F', w = 36, h = 36 })`.
- **Props / inputs visible at declarations:** `SDDownloadLogo: { color = '#706F6F', w = 36, h = 36 }`.
- **Hooks / local state:** None; state bindings None; contexts None.
- **Dependencies:** internal None; external `react`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/app/(withSidebarLayout)/upload/sequence.tsx`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/components/shared/logo/SDExtractLogo.tsx`

- **Why it exists / responsibility:** Reusable SDExtract Logo presentation or interaction shared across multiple product features.
- **Runtime and size:** Client-bound through an importing client module; 47 lines.
- **Exports / surface:** `default`, `SDExtractLogo({ color = '#706F6F', w = 35, h = 35 })`.
- **Props / inputs visible at declarations:** `SDExtractLogo: { color = '#706F6F', w = 35, h = 35 }`.
- **Hooks / local state:** None; state bindings None; contexts None.
- **Dependencies:** internal None; external `react`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/app/(withSidebarLayout)/upload/sequence.tsx`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/components/shared/logo/UploadLogo.tsx`

- **Why it exists / responsibility:** Reusable Upload Logo presentation or interaction shared across multiple product features.
- **Runtime and size:** Client-bound through an importing client module; 17 lines.
- **Exports / surface:** `default`, `UploadLogo({ color = '#706F6F', w = 36, h = 36 })`.
- **Props / inputs visible at declarations:** `UploadLogo: { color = '#706F6F', w = 36, h = 36 }`.
- **Hooks / local state:** None; state bindings None; contexts None.
- **Dependencies:** internal None; external `react`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/app/(withSidebarLayout)/upload/sequence.tsx`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/components/shared/tabs.tsx`

- **Why it exists / responsibility:** Reusable Tabs presentation or interaction shared across multiple product features.
- **Runtime and size:** Client-bound through an importing client module; 36 lines.
- **Exports / surface:** `Tabs`, `Tabs({ tabs, activeTab, onTabChange, className }: TabsProps)`.
- **Props / inputs visible at declarations:** `Tabs: { tabs, activeTab, onTabChange, className }: TabsProps`.
- **Hooks / local state:** None; state bindings None; contexts None.
- **Dependencies:** internal `src/lib/utils.ts`; external None.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/components/features/additional-upload/additional-upload-page.tsx`, `src/components/features/analytics/analytics-page.tsx`, `src/components/features/dedicated-server/dedicated-server-page.tsx`, `src/components/features/developer-section/developer-section-page.tsx`, `src/components/features/my-account/my-account.tsx`, `src/components/features/streaming-app/application.tsx`, `src/components/features/streaming-app/config/config-settings-form.tsx`, `src/components/features/streaming-app/config/image-asset-list.tsx`, plus 4 more.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/components/shared/tooltip-info.tsx`

- **Why it exists / responsibility:** Reusable Tooltip Info presentation or interaction shared across multiple product features.
- **Runtime and size:** Client-bound through an importing client module; 18 lines.
- **Exports / surface:** `ToolTipInfo`, `ToolTipInfo({ children }: TChildrenProps)`.
- **Props / inputs visible at declarations:** `ToolTipInfo: { children }: TChildrenProps`.
- **Hooks / local state:** None; state bindings None; contexts None.
- **Dependencies:** internal `src/components/ui/tooltip.tsx`, `src/types/common.ts`; external `lucide-react`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/components/features/developer-section/api-key-tab.tsx`, `src/components/features/developer-section/patch-file-upload.tsx`, `src/components/features/developer-section/streaming-api-key-tab.tsx`, `src/components/features/developer-section/token-generation-tab.tsx`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

### src/components/sidebar

#### `src/components/sidebar/app-sidebar.tsx`

- **Why it exists / responsibility:** App Sidebar sidebar navigation subcomponent.
- **Runtime and size:** Client module; 62 lines.
- **Exports / surface:** `AppSidebar`, `AppSidebar({ ...props }: React.ComponentProps<typeof Sidebar>)`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** `useUserInfo`; state bindings None; contexts None.
- **Dependencies:** internal `src/components/sidebar/nav-main.tsx`, `src/components/sidebar/team-switcher.tsx`, `src/components/ui/sidebar.tsx`, `src/config/page.ts`, `src/hooks/use-user-info.ts`; external `react`, `lucide-react`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/app/(withSidebarLayout)/layout.tsx`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/components/sidebar/nav-main.tsx`

- **Why it exists / responsibility:** Nav Main sidebar navigation subcomponent.
- **Runtime and size:** Client module; 99 lines.
- **Exports / surface:** `NavMain`, `NavMain({ items }: NavMainProps)`, `handleLogout`.
- **Props / inputs visible at declarations:** `NavMain: { items }: NavMainProps`.
- **Hooks / local state:** `usePathname`, `useLogoutMutation`; state bindings None; contexts None.
- **Dependencies:** internal `src/components/ui/sidebar.tsx`, `src/store/api/auth.ts`, `src/config/page.ts`; external `next/link`, `next/navigation`, `lucide-react`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/components/sidebar/app-sidebar.tsx`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/components/sidebar/nav-projects.tsx`

- **Why it exists / responsibility:** Nav Projects sidebar navigation subcomponent.
- **Runtime and size:** Client module; 84 lines.
- **Exports / surface:** `NavProjects`, `NavProjects({ projects }: NavProjectsProps)`.
- **Props / inputs visible at declarations:** `NavProjects: { projects }: NavProjectsProps`.
- **Hooks / local state:** `useSidebar`; state bindings None; contexts None.
- **Dependencies:** internal `src/components/ui/dropdown-menu.tsx`, `src/components/ui/sidebar.tsx`; external `lucide-react`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** None; static analysis found no internal importer, so verify framework entry/dynamic use.
- **Improvement / caution:** Confirm whether this is a framework root, planned module, or dead code before retaining it.

#### `src/components/sidebar/nav-user.tsx`

- **Why it exists / responsibility:** Nav User sidebar navigation subcomponent.
- **Runtime and size:** Client module; 99 lines.
- **Exports / surface:** `NavUser`, `NavUser({ user }: NavUserProps)`.
- **Props / inputs visible at declarations:** `NavUser: { user }: NavUserProps`.
- **Hooks / local state:** `useSidebar`; state bindings None; contexts None.
- **Dependencies:** internal `src/components/ui/avatar.tsx`, `src/components/ui/dropdown-menu.tsx`, `src/components/ui/sidebar.tsx`; external `lucide-react`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** None; static analysis found no internal importer, so verify framework entry/dynamic use.
- **Improvement / caution:** Confirm whether this is a framework root, planned module, or dead code before retaining it.

#### `src/components/sidebar/team-switcher.tsx`

- **Why it exists / responsibility:** Team Switcher sidebar navigation subcomponent.
- **Runtime and size:** Client module; 25 lines.
- **Exports / surface:** `TeamSwitcher`, `TeamSwitcher`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** None; state bindings None; contexts None.
- **Dependencies:** internal `src/assets/index.ts`, `src/components/ui/avatar.tsx`, `src/components/ui/sidebar.tsx`; external None.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/components/sidebar/app-sidebar.tsx`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

### src/components/ui

#### `src/components/ui/accordion.tsx`

- **Why it exists / responsibility:** Source-owned shadcn/Radix Accordion primitive or small field/headline/search building block.
- **Runtime and size:** Client module; 55 lines.
- **Exports / surface:** `Accordion({ ...props }: React.ComponentProps<typeof AccordionPrimitive.Root>)`, `AccordionItem({ className, ...props }: React.ComponentProps<typeof AccordionPrimitive.Item>)`, `AccordionTrigger({ className, children, ...props }: React.ComponentProps<typeof AccordionPrimitive.Trigger>)`, `AccordionContent({ className, children, ...props }: React.ComponentProps<typeof AccordionPrimitive.Content>)`.
- **Props / inputs visible at declarations:** `AccordionItem: { className, ...props }: React.ComponentProps<typeof AccordionPrimitive.Item>`, `AccordionTrigger: { className, children, ...props }: React.ComponentProps<typeof AccordionPrimitive.Trigger>`, `AccordionContent: { className, children, ...props }: React.ComponentProps<typeof AccordionPrimitive.Content>`.
- **Hooks / local state:** None; state bindings None; contexts None.
- **Dependencies:** internal `src/lib/utils.ts`; external `react`, `@radix-ui/react-accordion`, `lucide-react`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/components/features/plan/faq.tsx`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/components/ui/alert.tsx`

- **Why it exists / responsibility:** Source-owned shadcn/Radix Alert primitive or small field/headline/search building block.
- **Runtime and size:** Client-bound through an importing client module; 50 lines.
- **Exports / surface:** `Alert({ className, variant, ...props }: React.ComponentProps<'div'> & VariantProps<typeof alertVariants>)`, `AlertTitle({ className, ...props }: React.ComponentProps<'div'>)`, `AlertDescription({ className, ...props }: React.ComponentProps<'div'>)`.
- **Props / inputs visible at declarations:** `AlertTitle: { className, ...props }: React.ComponentProps<'div'>`, `AlertDescription: { className, ...props }: React.ComponentProps<'div'>`.
- **Hooks / local state:** None; state bindings None; contexts None.
- **Dependencies:** internal `src/lib/utils.ts`; external `react`, `class-variance-authority`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/app/(with-public-layout)/fb-mail/page.tsx`, `src/components/features/singup/signup-page.tsx`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/components/ui/avatar.tsx`

- **Why it exists / responsibility:** Source-owned shadcn/Radix Avatar primitive or small field/headline/search building block.
- **Runtime and size:** Client module; 36 lines.
- **Exports / surface:** `Avatar({ className, ...props }: React.ComponentProps<typeof AvatarPrimitive.Root>)`, `AvatarImage({ className, ...props }: React.ComponentProps<typeof AvatarPrimitive.Image>)`, `AvatarFallback({ className, ...props }: React.ComponentProps<typeof AvatarPrimitive.Fallback>)`.
- **Props / inputs visible at declarations:** `AvatarImage: { className, ...props }: React.ComponentProps<typeof AvatarPrimitive.Image>`, `AvatarFallback: { className, ...props }: React.ComponentProps<typeof AvatarPrimitive.Fallback>`.
- **Hooks / local state:** None; state bindings None; contexts None.
- **Dependencies:** internal `src/lib/utils.ts`; external `react`, `@radix-ui/react-avatar`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/components/features/my-account/profile-settings.tsx`, `src/components/features/team/invited-tab.tsx`, `src/components/features/team/my-team-tab.tsx`, `src/components/navbar/header.tsx`, `src/components/sidebar/nav-user.tsx`, `src/components/sidebar/team-switcher.tsx`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/components/ui/badge.tsx`

- **Why it exists / responsibility:** Source-owned shadcn/Radix Badge primitive or small field/headline/search building block.
- **Runtime and size:** Client-bound through an importing client module; 38 lines.
- **Exports / surface:** `Badge({
  className,
  variant,
  asChild = false,
  ...props
}: React.ComponentProps<'span'> & VariantProps<typeof badgeVariants> & { asChild?: boolean })`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** None; state bindings None; contexts None.
- **Dependencies:** internal `src/lib/utils.ts`; external `react`, `@radix-ui/react-slot`, `class-variance-authority`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/components/features/plan/new-pricing-card.tsx`, `src/components/navbar/notification-dropdown.tsx`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/components/ui/breadcrumb.tsx`

- **Why it exists / responsibility:** Source-owned shadcn/Radix Breadcrumb primitive or small field/headline/search building block.
- **Runtime and size:** Client-bound through an importing client module; 94 lines.
- **Exports / surface:** `Breadcrumb({ ...props }: React.ComponentProps<'nav'>)`, `BreadcrumbList({ className, ...props }: React.ComponentProps<'ol'>)`, `BreadcrumbItem({ className, ...props }: React.ComponentProps<'li'>)`, `BreadcrumbLink({
  asChild,
  className,
  ...props
}: React.ComponentProps<'a'> & {
  asChild?: boolean;
})`, `BreadcrumbPage({ className, ...props }: React.ComponentProps<'span'>)`, `BreadcrumbSeparator({ children, className, ...props }: React.ComponentProps<'li'>)`, `BreadcrumbEllipsis({ className, ...props }: React.ComponentProps<'span'>)`.
- **Props / inputs visible at declarations:** `BreadcrumbList: { className, ...props }: React.ComponentProps<'ol'>`, `BreadcrumbItem: { className, ...props }: React.ComponentProps<'li'>`, `BreadcrumbLink: {
  asChild,
  className,
  ...props
}: React.ComponentProps<'a'> & {
  asChild?: boolean;
}`, `BreadcrumbPage: { className, ...props }: React.ComponentProps<'span'>`, `BreadcrumbSeparator: { children, className, ...props }: React.ComponentProps<'li'>`, `BreadcrumbEllipsis: { className, ...props }: React.ComponentProps<'span'>`.
- **Hooks / local state:** None; state bindings None; contexts None.
- **Dependencies:** internal `src/lib/utils.ts`; external `react`, `@radix-ui/react-slot`, `lucide-react`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** None; static analysis found no internal importer, so verify framework entry/dynamic use.
- **Improvement / caution:** Confirm whether this is a framework root, planned module, or dead code before retaining it.

#### `src/components/ui/button.tsx`

- **Why it exists / responsibility:** Source-owned shadcn/Radix Button primitive or small field/headline/search building block.
- **Runtime and size:** Client-bound through an importing client module; 55 lines.
- **Exports / surface:** `Button({
  className,
  variant,
  size,
  asChild = false,
  ...props
}: React.ComponentProps<'button'> &
  VariantProps<typeof buttonVariants> & {
    asChild?: boolean;
  })`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** None; state bindings None; contexts None.
- **Dependencies:** internal `src/lib/utils.ts`; external `react`, `@radix-ui/react-slot`, `class-variance-authority`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/app/(with-public-layout)/fb-mail/page.tsx`, `src/app/(with-public-layout)/repass/page.tsx`, `src/app/(withSidebarLayout)/credit-card-failed/page.js`, `src/app/(withSidebarLayout)/credit-card-successful-core/page.tsx`, `src/app/(withSidebarLayout)/credit-card-successful/page.tsx`, `src/app/(withSidebarLayout)/payment-failed/page.tsx`, `src/app/(withSidebarLayout)/upload/VideoTutorialModal.tsx`, `src/app/(withSidebarLayout)/upload/app-upload-modal.tsx`, plus 82 more.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/components/ui/card.tsx`

- **Why it exists / responsibility:** Source-owned shadcn/Radix Card primitive or small field/headline/search building block.
- **Runtime and size:** Client-bound through an importing client module; 57 lines.
- **Exports / surface:** `Card({ className, ...props }: React.ComponentProps<'div'>)`, `CardHeader({ className, ...props }: React.ComponentProps<'div'>)`, `CardTitle({ className, ...props }: React.ComponentProps<'div'>)`, `CardDescription({ className, ...props }: React.ComponentProps<'div'>)`, `CardAction({ className, ...props }: React.ComponentProps<'div'>)`, `CardContent({ className, ...props }: React.ComponentProps<'div'>)`, `CardFooter({ className, ...props }: React.ComponentProps<'div'>)`.
- **Props / inputs visible at declarations:** `CardHeader: { className, ...props }: React.ComponentProps<'div'>`, `CardTitle: { className, ...props }: React.ComponentProps<'div'>`, `CardDescription: { className, ...props }: React.ComponentProps<'div'>`, `CardAction: { className, ...props }: React.ComponentProps<'div'>`, `CardContent: { className, ...props }: React.ComponentProps<'div'>`, `CardFooter: { className, ...props }: React.ComponentProps<'div'>`.
- **Hooks / local state:** None; state bindings None; contexts None.
- **Dependencies:** internal `src/lib/utils.ts`; external `react`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/components/admin/super-admin-url-generator.tsx`, `src/components/features/analytics/ccu-tab.tsx`, `src/components/features/analytics/charts/bar.tsx`, `src/components/features/analytics/charts/pie.tsx`, `src/components/features/analytics/streaming-min-tab.tsx`, `src/components/features/dedicated-server/dedicated-server-card.tsx`, `src/components/features/my-account/profile-settings.tsx`, `src/components/features/new-subscription/core-plan-montly.tsx`, plus 27 more.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/components/ui/chart.tsx`

- **Why it exists / responsibility:** Source-owned shadcn/Radix Chart primitive or small field/headline/search building block.
- **Runtime and size:** Client module; 295 lines.
- **Exports / surface:** `ChartConfig`, `useChart`, `ChartContainer({
  id,
  className,
  children,
  config,
  ...props
}: React.ComponentProps<'div'> & {
  config: ChartConfig;
  children: React.ComponentProps<typeof RechartsPrimitive.ResponsiveContainer>['children'];
})`, `ChartStyle({ id, config }: { id: string; config: ChartConfig })`, `ChartTooltipContent({
  active,
  payload,
  className,
  indicator = 'dot',
  hideLabel = false,
  hideIndicator = false,
  label,
  labelFormatter,
  labelClassName,
  formatter,
  color,
  nameKey,
  labelKey
}: React.ComponentProps<typeof RechartsPrimitive.Tooltip> &
  React.ComponentProps<'div'> & {
    hideLabel?: boolean;
    hideIndicator?: boolean;
    indicator?: 'line' | 'dot' | 'dashed';
    nameKey?: string;
    labelKey?: string;
  })`, `ChartLegendContent({
  className,
  hideIcon = false,
  payload,
  verticalAlign = 'bottom',
  nameKey
}: React.ComponentProps<'div'> &
  Pick<RechartsPrimitive.LegendProps, 'payload' | 'verticalAlign'> & {
    hideIcon?: boolean;
    nameKey?: string;
  })`, `getPayloadConfigFromPayload(config: ChartConfig, payload: unknown, key: string)`.
- **Props / inputs visible at declarations:** `ChartStyle: { id, config }: { id: string; config: ChartConfig }`, `ChartTooltipContent: {
  active,
  payload,
  className,
  indicator = 'dot',
  hideLabel = false,
  hideIndicator = false,
  label,
  labelFormatter,
  labelClassName,
  formatter,
  color,
  nameKey,
  labelKey
}: React.ComponentProps<typeof RechartsPrimitive.Tooltip> &
  React.ComponentProps<'div'> & {
    hideLabel?: boolean;
    hideIndicator?: boolean;
    indicator?: 'line' | 'dot' | 'dashed';
    nameKey?: string;
    labelKey?: string;
  }`, `ChartLegendContent: {
  className,
  hideIcon = false,
  payload,
  verticalAlign = 'bottom',
  nameKey
}: React.ComponentProps<'div'> &
  Pick<RechartsPrimitive.LegendProps, 'payload' | 'verticalAlign'> & {
    hideIcon?: boolean;
    nameKey?: string;
  }`.
- **Hooks / local state:** `useContext`, `useId`, `useChart`, `useMemo`; state bindings None; contexts `ChartContext`.
- **Dependencies:** internal `src/lib/utils.ts`; external `react`, `recharts`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/components/features/analytics/charts/bar.tsx`, `src/components/features/analytics/charts/pie.tsx`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/components/ui/checkbox.tsx`

- **Why it exists / responsibility:** Source-owned shadcn/Radix Checkbox primitive or small field/headline/search building block.
- **Runtime and size:** Client module; 31 lines.
- **Exports / surface:** `Checkbox({ className, ...props }: React.ComponentProps<typeof CheckboxPrimitive.Root>)`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** None; state bindings None; contexts None.
- **Dependencies:** internal `src/lib/utils.ts`; external `react`, `@radix-ui/react-checkbox`, `lucide-react`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/components/features/username/username-page.tsx`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/components/ui/collapsible.tsx`

- **Why it exists / responsibility:** Source-owned shadcn/Radix Collapsible primitive or small field/headline/search building block.
- **Runtime and size:** Client module; 18 lines.
- **Exports / surface:** `Collapsible({ ...props }: React.ComponentProps<typeof CollapsiblePrimitive.Root>)`, `CollapsibleTrigger({ ...props }: React.ComponentProps<typeof CollapsiblePrimitive.CollapsibleTrigger>)`, `CollapsibleContent({ ...props }: React.ComponentProps<typeof CollapsiblePrimitive.CollapsibleContent>)`.
- **Props / inputs visible at declarations:** `CollapsibleTrigger: { ...props }: React.ComponentProps<typeof CollapsiblePrimitive.CollapsibleTrigger>`, `CollapsibleContent: { ...props }: React.ComponentProps<typeof CollapsiblePrimitive.CollapsibleContent>`.
- **Hooks / local state:** None; state bindings None; contexts None.
- **Dependencies:** internal None; external `@radix-ui/react-collapsible`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** None; static analysis found no internal importer, so verify framework entry/dynamic use.
- **Improvement / caution:** Confirm whether this is a framework root, planned module, or dead code before retaining it.

#### `src/components/ui/dialog.tsx`

- **Why it exists / responsibility:** Source-owned shadcn/Radix Dialog primitive or small field/headline/search building block.
- **Runtime and size:** Client module; 124 lines.
- **Exports / surface:** `Dialog({ ...props }: React.ComponentProps<typeof DialogPrimitive.Root>)`, `DialogTrigger({ ...props }: React.ComponentProps<typeof DialogPrimitive.Trigger>)`, `DialogPortal({ ...props }: React.ComponentProps<typeof DialogPrimitive.Portal>)`, `DialogClose({ ...props }: React.ComponentProps<typeof DialogPrimitive.Close>)`, `DialogOverlay({ className, ...props }: React.ComponentProps<typeof DialogPrimitive.Overlay>)`, `DialogContent({
  className,
  children,
  showCloseButton = true,
  ...props
}: React.ComponentProps<typeof DialogPrimitive.Content> & {
  showCloseButton?: boolean;
})`, `DialogHeader({ className, ...props }: React.ComponentProps<'div'>)`, `DialogFooter({ className, ...props }: React.ComponentProps<'div'>)`, `DialogTitle({ className, ...props }: React.ComponentProps<typeof DialogPrimitive.Title>)`, `DialogDescription({ className, ...props }: React.ComponentProps<typeof DialogPrimitive.Description>)`.
- **Props / inputs visible at declarations:** `DialogTrigger: { ...props }: React.ComponentProps<typeof DialogPrimitive.Trigger>`, `DialogPortal: { ...props }: React.ComponentProps<typeof DialogPrimitive.Portal>`, `DialogClose: { ...props }: React.ComponentProps<typeof DialogPrimitive.Close>`, `DialogOverlay: { className, ...props }: React.ComponentProps<typeof DialogPrimitive.Overlay>`, `DialogContent: {
  className,
  children,
  showCloseButton = true,
  ...props
}: React.ComponentProps<typeof DialogPrimitive.Content> & {
  showCloseButton?: boolean;
}`, `DialogHeader: { className, ...props }: React.ComponentProps<'div'>`, plus 3 more.
- **Hooks / local state:** None; state bindings None; contexts None.
- **Dependencies:** internal `src/lib/utils.ts`; external `react`, `@radix-ui/react-dialog`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/app/(withSidebarLayout)/upload/VideoTutorialModal.tsx`, `src/app/(withSidebarLayout)/upload/app-upload-modal.tsx`, `src/components/features/additional-upload/additional-upload-page.tsx`, `src/components/features/dedicated-server/dedicated-server-app-upload.tsx`, `src/components/features/dedicated-server/dedicated-server-card-details.tsx`, `src/components/features/my-account/my-account.tsx`, `src/components/features/plan/new-pricing-card.tsx`, `src/components/features/plan/new-pricing.tsx`, plus 7 more.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/components/ui/dropdown-menu.tsx`

- **Why it exists / responsibility:** Source-owned shadcn/Radix Dropdown Menu primitive or small field/headline/search building block.
- **Runtime and size:** Client module; 221 lines.
- **Exports / surface:** `DropdownMenu({ ...props }: React.ComponentProps<typeof DropdownMenuPrimitive.Root>)`, `DropdownMenuPortal({ ...props }: React.ComponentProps<typeof DropdownMenuPrimitive.Portal>)`, `DropdownMenuTrigger({ ...props }: React.ComponentProps<typeof DropdownMenuPrimitive.Trigger>)`, `DropdownMenuContent({
  className,
  sideOffset = 4,
  ...props
}: React.ComponentProps<typeof DropdownMenuPrimitive.Content>)`, `DropdownMenuGroup({ ...props }: React.ComponentProps<typeof DropdownMenuPrimitive.Group>)`, `DropdownMenuItem({
  className,
  inset,
  variant = 'default',
  ...props
}: React.ComponentProps<typeof DropdownMenuPrimitive.Item> & {
  inset?: boolean;
  variant?: 'default' | 'destructive';
})`, `DropdownMenuCheckboxItem({
  className,
  children,
  checked,
  ...props
}: React.ComponentProps<typeof DropdownMenuPrimitive.CheckboxItem>)`, `DropdownMenuRadioGroup({ ...props }: React.ComponentProps<typeof DropdownMenuPrimitive.RadioGroup>)`, `DropdownMenuRadioItem({
  className,
  children,
  ...props
}: React.ComponentProps<typeof DropdownMenuPrimitive.RadioItem>)`, `DropdownMenuLabel({
  className,
  inset,
  ...props
}: React.ComponentProps<typeof DropdownMenuPrimitive.Label> & {
  inset?: boolean;
})`, `DropdownMenuSeparator({ className, ...props }: React.ComponentProps<typeof DropdownMenuPrimitive.Separator>)`, `DropdownMenuShortcut({ className, ...props }: React.ComponentProps<'span'>)`, plus 3 more.
- **Props / inputs visible at declarations:** `DropdownMenuPortal: { ...props }: React.ComponentProps<typeof DropdownMenuPrimitive.Portal>`, `DropdownMenuTrigger: { ...props }: React.ComponentProps<typeof DropdownMenuPrimitive.Trigger>`, `DropdownMenuContent: {
  className,
  sideOffset = 4,
  ...props
}: React.ComponentProps<typeof DropdownMenuPrimitive.Content>`, `DropdownMenuGroup: { ...props }: React.ComponentProps<typeof DropdownMenuPrimitive.Group>`, `DropdownMenuItem: {
  className,
  inset,
  variant = 'default',
  ...props
}: React.ComponentProps<typeof DropdownMenuPrimitive.Item> & {
  inset?: boolean;
  variant?: 'default' | 'destructive';
}`, `DropdownMenuCheckboxItem: {
  className,
  children,
  checked,
  ...props
}: React.ComponentProps<typeof DropdownMenuPrimitive.CheckboxItem>`, plus 8 more.
- **Hooks / local state:** None; state bindings None; contexts None.
- **Dependencies:** internal `src/lib/utils.ts`; external `react`, `@radix-ui/react-dropdown-menu`, `lucide-react`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/components/features/streaming-app/streaming-app-table.tsx`, `src/components/navbar/notification-dropdown.tsx`, `src/components/shared/app-display-card.tsx`, `src/components/shared/common-table.tsx`, `src/components/sidebar/nav-projects.tsx`, `src/components/sidebar/nav-user.tsx`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/components/ui/field/custom-input.tsx`

- **Why it exists / responsibility:** Source-owned shadcn/Radix Custom Input primitive or small field/headline/search building block.
- **Runtime and size:** Client module; 76 lines.
- **Exports / surface:** `CustomInput`, `CustomInput({
  id,
  label,
  type = 'text',
  value,
  onChange,
  required = false,
  inputClassName = '',
  placeholder = '',
  disabled = false
}: FormFieldProps)`, `togglePasswordVisibility`.
- **Props / inputs visible at declarations:** `CustomInput: {
  id,
  label,
  type = 'text',
  value,
  onChange,
  required = false,
  inputClassName = '',
  placeholder = '',
  disabled = false
}: FormFieldProps`.
- **Hooks / local state:** `useState`; state bindings `showPassword`, `setShowPassword`; contexts None.
- **Dependencies:** internal `src/components/ui/label.tsx`, `src/components/ui/input.tsx`; external `react`, `lucide-react`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/components/features/developer-section/token-generation-tab.tsx`, `src/components/features/signin/signin-page.tsx`, `src/components/features/username/username-page.tsx`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/components/ui/field/select-drop-down.tsx`

- **Why it exists / responsibility:** Source-owned shadcn/Radix Select Drop Down primitive or small field/headline/search building block.
- **Runtime and size:** Client module; 65 lines.
- **Exports / surface:** `SelectDropdown`, `SelectDropdown({
  options,
  value,
  onValueChange,
  placeholder = 'Select',
  disabled = false,
  className = '',
  triggerClassName = '',
  valueClassName = '',
  placeholderClassName = '',
  contentClassName = '',
  itemClassName = ''
}: SelectDropdownProps)`.
- **Props / inputs visible at declarations:** `SelectDropdown: {
  options,
  value,
  onValueChange,
  placeholder = 'Select',
  disabled = false,
  className = '',
  triggerClassName = '',
  valueClassName = '',
  placeholderClassName = '',
  contentClassName = '',
  itemClassName = ''
}: SelectDropdownProps`.
- **Hooks / local state:** None; state bindings None; contexts None.
- **Dependencies:** internal `src/components/ui/select.tsx`, `src/lib/utils.ts`; external None.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/components/features/subscription/modal/ppccu-ppl-modify-modal.tsx`, `src/components/features/subscription/modal/storage-modify-modal.tsx`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/components/ui/field/text-area.tsx`

- **Why it exists / responsibility:** Source-owned shadcn/Radix Text Area primitive or small field/headline/search building block.
- **Runtime and size:** Client-bound through an importing client module; 22 lines.
- **Exports / surface:** `ApiKeyTextarea`, `ApiKeyTextarea({ value }: ApiKeyTextareaProps)`.
- **Props / inputs visible at declarations:** `ApiKeyTextarea: { value }: ApiKeyTextareaProps`.
- **Hooks / local state:** None; state bindings None; contexts None.
- **Dependencies:** internal `src/components/ui/textarea.tsx`; external `lucide-react`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/components/features/developer-section/api-key-tab.tsx`, `src/components/features/developer-section/streaming-api-key-tab.tsx`, `src/components/features/developer-section/token-generation-tab.tsx`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/components/ui/form.tsx`

- **Why it exists / responsibility:** Source-owned shadcn/Radix Form primitive or small field/headline/search building block.
- **Runtime and size:** Client module; 140 lines.
- **Exports / surface:** `FormField({
  ...props
}: ControllerProps<TFieldValues, TName>)`, `useFormField`, `FormItem({ className, ...props }: React.ComponentProps<'div'>)`, `FormLabel({ className, ...props }: React.ComponentProps<typeof LabelPrimitive.Root>)`, `FormControl({ ...props }: React.ComponentProps<typeof Slot>)`, `FormDescription({ className, ...props }: React.ComponentProps<'p'>)`, `FormMessage({ className, ...props }: React.ComponentProps<'p'>)`.
- **Props / inputs visible at declarations:** `FormField: {
  ...props
}: ControllerProps<TFieldValues, TName>`, `FormItem: { className, ...props }: React.ComponentProps<'div'>`, `FormLabel: { className, ...props }: React.ComponentProps<typeof LabelPrimitive.Root>`, `FormControl: { ...props }: React.ComponentProps<typeof Slot>`, `FormDescription: { className, ...props }: React.ComponentProps<'p'>`, `FormMessage: { className, ...props }: React.ComponentProps<'p'>`.
- **Hooks / local state:** `useContext`, `useFormContext`, `useFormState`, `useId`, `useFormField`; state bindings None; contexts `FormFieldContext`, `FormItemContext`.
- **Dependencies:** internal `src/lib/utils.ts`, `src/components/ui/label.tsx`; external `react`, `@radix-ui/react-label`, `@radix-ui/react-slot`, `react-hook-form`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/components/features/streaming-app/config/common-tab.tsx`, `src/components/features/streaming-app/config/customization-tab.tsx`, `src/components/features/streaming-app/config/developer-tab.tsx`, `src/components/features/streaming-app/config/shared-form.tsx`, `src/components/features/streaming-app/config/ui-tab.tsx`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/components/ui/headlines/page-title.tsx`

- **Why it exists / responsibility:** Source-owned shadcn/Radix Page Title primitive or small field/headline/search building block.
- **Runtime and size:** Client-bound through an importing client module; 21 lines.
- **Exports / surface:** `PageTitle`, `PageTitle({ title, subTitle, url }: THeader)`.
- **Props / inputs visible at declarations:** `PageTitle: { title, subTitle, url }: THeader`.
- **Hooks / local state:** None; state bindings None; contexts None.
- **Dependencies:** internal `src/components/shared/doc-link.tsx`, `src/config/doc.ts`; external None.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/components/features/additional-upload/additional-upload-page.tsx`, `src/components/features/analytics/analytics-page.tsx`, `src/components/features/dedicated-server/dedicated-server-page.tsx`, `src/components/features/developer-section/developer-section-page.tsx`, `src/components/features/streaming-app/streaming-app-section.tsx`, `src/components/features/team/team-page.tsx`, `src/components/features/utilities/utilities-page.tsx`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/components/ui/input-otp.tsx`

- **Why it exists / responsibility:** Source-owned shadcn/Radix Input Otp primitive or small field/headline/search building block.
- **Runtime and size:** Client module; 70 lines.
- **Exports / surface:** `InputOTP({
  className,
  containerClassName,
  ...props
}: React.ComponentProps<typeof OTPInput> & {
  containerClassName?: string;
})`, `InputOTPGroup({ className, ...props }: React.ComponentProps<'div'>)`, `InputOTPSlot({
  index,
  className,
  ...props
}: React.ComponentProps<'div'> & {
  index: number;
})`, `InputOTPSeparator({ ...props }: React.ComponentProps<'div'>)`.
- **Props / inputs visible at declarations:** `InputOTPGroup: { className, ...props }: React.ComponentProps<'div'>`, `InputOTPSlot: {
  index,
  className,
  ...props
}: React.ComponentProps<'div'> & {
  index: number;
}`, `InputOTPSeparator: { ...props }: React.ComponentProps<'div'>`.
- **Hooks / local state:** `useContext`; state bindings None; contexts None.
- **Dependencies:** internal `src/lib/utils.ts`; external `react`, `input-otp`, `lucide-react`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/components/features/singup/signup-page.tsx`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/components/ui/input.tsx`

- **Why it exists / responsibility:** Source-owned shadcn/Radix Input primitive or small field/headline/search building block.
- **Runtime and size:** Client-bound through an importing client module; 22 lines.
- **Exports / surface:** `Input({ className, type, ...props }: React.ComponentProps<'input'>)`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** None; state bindings None; contexts None.
- **Dependencies:** internal `src/lib/utils.ts`; external `react`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/app/(with-public-layout)/fb-mail/page.tsx`, `src/app/(with-public-layout)/repass/page.tsx`, `src/app/(withSidebarLayout)/upload/app-upload-modal.tsx`, `src/components/admin/super-admin-url-generator.tsx`, `src/components/features/dedicated-server/application.tsx`, `src/components/features/dedicated-server/dedicated-server-app-upload.tsx`, `src/components/features/my-account/profile-settings.tsx`, `src/components/features/plan/dialog/ppm.tsx`, plus 15 more.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/components/ui/label.tsx`

- **Why it exists / responsibility:** Source-owned shadcn/Radix Label primitive or small field/headline/search building block.
- **Runtime and size:** Client module; 23 lines.
- **Exports / surface:** `Label({ className, ...props }: React.ComponentProps<typeof LabelPrimitive.Root>)`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** None; state bindings None; contexts None.
- **Dependencies:** internal `src/lib/utils.ts`; external `react`, `@radix-ui/react-label`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/app/(with-public-layout)/fb-mail/page.tsx`, `src/app/(with-public-layout)/repass/page.tsx`, `src/app/(withSidebarLayout)/upload/app-upload-modal.tsx`, `src/components/features/analytics/charts/bar.tsx`, `src/components/features/dedicated-server/dedicated-server-app-upload.tsx`, `src/components/features/developer-section/api-key-tab.tsx`, `src/components/features/developer-section/streaming-api-key-tab.tsx`, `src/components/features/developer-section/token-generation-tab.tsx`, plus 15 more.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/components/ui/pagination.tsx`

- **Why it exists / responsibility:** Source-owned shadcn/Radix Pagination primitive or small field/headline/search building block.
- **Runtime and size:** Client-bound through an importing client module; 103 lines.
- **Exports / surface:** `Pagination({ className, ...props }: React.ComponentProps<'nav'>)`, `PaginationContent({ className, ...props }: React.ComponentProps<'ul'>)`, `PaginationItem({ ...props }: React.ComponentProps<'li'>)`, `PaginationLink({ className, isActive, size = 'icon', ...props }: PaginationLinkProps)`, `PaginationPrevious({ className, ...props }: React.ComponentProps<typeof PaginationLink>)`, `PaginationNext({ className, ...props }: React.ComponentProps<typeof PaginationLink>)`, `PaginationEllipsis({ className, ...props }: React.ComponentProps<'span'>)`.
- **Props / inputs visible at declarations:** `PaginationContent: { className, ...props }: React.ComponentProps<'ul'>`, `PaginationItem: { ...props }: React.ComponentProps<'li'>`, `PaginationLink: { className, isActive, size = 'icon', ...props }: PaginationLinkProps`, `PaginationPrevious: { className, ...props }: React.ComponentProps<typeof PaginationLink>`, `PaginationNext: { className, ...props }: React.ComponentProps<typeof PaginationLink>`, `PaginationEllipsis: { className, ...props }: React.ComponentProps<'span'>`.
- **Hooks / local state:** None; state bindings None; contexts None.
- **Dependencies:** internal `src/lib/utils.ts`, `src/components/ui/button.tsx`, `src/components/ui/button.tsx`; external `react`, `lucide-react`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/components/shared/data-table.tsx`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/components/ui/popover.tsx`

- **Why it exists / responsibility:** Source-owned shadcn/Radix Popover primitive or small field/headline/search building block.
- **Runtime and size:** Client module; 44 lines.
- **Exports / surface:** `Popover({ ...props }: React.ComponentProps<typeof PopoverPrimitive.Root>)`, `PopoverTrigger({ ...props }: React.ComponentProps<typeof PopoverPrimitive.Trigger>)`, `PopoverContent({
  className,
  align = 'center',
  sideOffset = 4,
  ...props
}: React.ComponentProps<typeof PopoverPrimitive.Content>)`, `PopoverAnchor({ ...props }: React.ComponentProps<typeof PopoverPrimitive.Anchor>)`.
- **Props / inputs visible at declarations:** `PopoverTrigger: { ...props }: React.ComponentProps<typeof PopoverPrimitive.Trigger>`, `PopoverContent: {
  className,
  align = 'center',
  sideOffset = 4,
  ...props
}: React.ComponentProps<typeof PopoverPrimitive.Content>`, `PopoverAnchor: { ...props }: React.ComponentProps<typeof PopoverPrimitive.Anchor>`.
- **Hooks / local state:** None; state bindings None; contexts None.
- **Dependencies:** internal `src/lib/utils.ts`; external `react`, `@radix-ui/react-popover`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** None; static analysis found no internal importer, so verify framework entry/dynamic use.
- **Improvement / caution:** Confirm whether this is a framework root, planned module, or dead code before retaining it.

#### `src/components/ui/progress.tsx`

- **Why it exists / responsibility:** Source-owned shadcn/Radix Progress primitive or small field/headline/search building block.
- **Runtime and size:** Client module; 26 lines.
- **Exports / surface:** `Progress({ className, value, ...props }: React.ComponentProps<typeof ProgressPrimitive.Root>)`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** None; state bindings None; contexts None.
- **Dependencies:** internal `src/lib/utils.ts`; external `react`, `@radix-ui/react-progress`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/components/features/additional-upload/additional-upload-details.tsx`, `src/components/features/dedicated-server/dedicated-server-upload-details.tsx`, `src/components/features/new-subscription/new-trial.tsx`, `src/components/features/streaming-app/config/image-upload.tsx`, `src/components/features/streaming-app/streaming-app-card.tsx`, `src/components/features/streaming-app/temporary-upload-card.tsx`, `src/components/features/streaming-app/video-upload.tsx`, `src/components/features/subscription/new-subscription-summary/core-plan.tsx`, plus 11 more.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/components/ui/radio-group.tsx`

- **Why it exists / responsibility:** Source-owned shadcn/Radix Radio Group primitive or small field/headline/search building block.
- **Runtime and size:** Client module; 35 lines.
- **Exports / surface:** `RadioGroup({ className, ...props }: React.ComponentProps<typeof RadioGroupPrimitive.Root>)`, `RadioGroupItem({ className, ...props }: React.ComponentProps<typeof RadioGroupPrimitive.Item>)`.
- **Props / inputs visible at declarations:** `RadioGroupItem: { className, ...props }: React.ComponentProps<typeof RadioGroupPrimitive.Item>`.
- **Hooks / local state:** None; state bindings None; contexts None.
- **Dependencies:** internal `src/lib/utils.ts`; external `react`, `@radix-ui/react-radio-group`, `lucide-react`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/components/features/plan/dialog/prepaid-minute.tsx`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/components/ui/search/search-bar.tsx`

- **Why it exists / responsibility:** Source-owned shadcn/Radix Search Bar primitive or small field/headline/search building block.
- **Runtime and size:** Client module; 26 lines.
- **Exports / surface:** `SearchBar`, `SearchBar({ placeholder = 'Search...', onChange }: SearchBarProps)`.
- **Props / inputs visible at declarations:** `SearchBar: { placeholder = 'Search...', onChange }: SearchBarProps`.
- **Hooks / local state:** None; state bindings None; contexts None.
- **Dependencies:** internal `src/components/ui/input.tsx`; external `lucide-react`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** None; static analysis found no internal importer, so verify framework entry/dynamic use.
- **Improvement / caution:** Confirm whether this is a framework root, planned module, or dead code before retaining it.

#### `src/components/ui/select.tsx`

- **Why it exists / responsibility:** Source-owned shadcn/Radix Select primitive or small field/headline/search building block.
- **Runtime and size:** Client module; 162 lines.
- **Exports / surface:** `Select({ ...props }: React.ComponentProps<typeof SelectPrimitive.Root>)`, `SelectGroup({ ...props }: React.ComponentProps<typeof SelectPrimitive.Group>)`, `SelectValue({ ...props }: React.ComponentProps<typeof SelectPrimitive.Value>)`, `SelectTrigger({
  className,
  size = 'default',
  children,
  ...props
}: React.ComponentProps<typeof SelectPrimitive.Trigger> & {
  size?: 'sm' | 'default';
})`, `SelectContent({
  className,
  children,
  position = 'popper',
  ...props
}: React.ComponentProps<typeof SelectPrimitive.Content>)`, `SelectLabel({ className, ...props }: React.ComponentProps<typeof SelectPrimitive.Label>)`, `SelectItem({ className, children, ...props }: React.ComponentProps<typeof SelectPrimitive.Item>)`, `SelectSeparator({ className, ...props }: React.ComponentProps<typeof SelectPrimitive.Separator>)`, `SelectScrollUpButton({ className, ...props }: React.ComponentProps<typeof SelectPrimitive.ScrollUpButton>)`, `SelectScrollDownButton({
  className,
  ...props
}: React.ComponentProps<typeof SelectPrimitive.ScrollDownButton>)`.
- **Props / inputs visible at declarations:** `SelectGroup: { ...props }: React.ComponentProps<typeof SelectPrimitive.Group>`, `SelectValue: { ...props }: React.ComponentProps<typeof SelectPrimitive.Value>`, `SelectTrigger: {
  className,
  size = 'default',
  children,
  ...props
}: React.ComponentProps<typeof SelectPrimitive.Trigger> & {
  size?: 'sm' | 'default';
}`, `SelectContent: {
  className,
  children,
  position = 'popper',
  ...props
}: React.ComponentProps<typeof SelectPrimitive.Content>`, `SelectLabel: { className, ...props }: React.ComponentProps<typeof SelectPrimitive.Label>`, `SelectItem: { className, children, ...props }: React.ComponentProps<typeof SelectPrimitive.Item>`, plus 3 more.
- **Hooks / local state:** None; state bindings None; contexts None.
- **Dependencies:** internal `src/lib/utils.ts`; external `react`, `@radix-ui/react-select`, `lucide-react`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/components/features/analytics/charts/bar.tsx`, `src/components/features/analytics/charts/pie.tsx`, `src/components/features/dedicated-server/application.tsx`, `src/components/features/streaming-app/application.tsx`, `src/components/features/streaming-app/config/common-tab.tsx`, `src/components/features/streaming-app/config/developer-tab.tsx`, `src/components/features/streaming-app/demo-apps/demo-application.tsx`, `src/components/features/streaming-app/meeting-link-tab.tsx`, plus 7 more.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/components/ui/separator.tsx`

- **Why it exists / responsibility:** Source-owned shadcn/Radix Separator primitive or small field/headline/search building block.
- **Runtime and size:** Client module; 30 lines.
- **Exports / surface:** `Separator({
  className,
  orientation = 'horizontal',
  decorative = true,
  ...props
}: React.ComponentProps<typeof SeparatorPrimitive.Root>)`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** None; state bindings None; contexts None.
- **Dependencies:** internal `src/lib/utils.ts`; external `react`, `@radix-ui/react-separator`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/components/features/new-subscription/new-billing.tsx`, `src/components/features/singup/signup-page.tsx`, `src/components/features/subscription/bill-summary-ppm.tsx`, `src/components/features/subscription/bill-summary.tsx`, `src/components/ui/sidebar.tsx`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/components/ui/sheet.tsx`

- **Why it exists / responsibility:** Source-owned shadcn/Radix Sheet primitive or small field/headline/search building block.
- **Runtime and size:** Client module; 105 lines.
- **Exports / surface:** `Sheet({ ...props }: React.ComponentProps<typeof SheetPrimitive.Root>)`, `SheetTrigger({ ...props }: React.ComponentProps<typeof SheetPrimitive.Trigger>)`, `SheetClose({ ...props }: React.ComponentProps<typeof SheetPrimitive.Close>)`, `SheetPortal({ ...props }: React.ComponentProps<typeof SheetPrimitive.Portal>)`, `SheetOverlay({ className, ...props }: React.ComponentProps<typeof SheetPrimitive.Overlay>)`, `SheetContent({
  className,
  children,
  side = 'right',
  ...props
}: React.ComponentProps<typeof SheetPrimitive.Content> & {
  side?: 'top' | 'right' | 'bottom' | 'left';
})`, `SheetHeader({ className, ...props }: React.ComponentProps<'div'>)`, `SheetFooter({ className, ...props }: React.ComponentProps<'div'>)`, `SheetTitle({ className, ...props }: React.ComponentProps<typeof SheetPrimitive.Title>)`, `SheetDescription({ className, ...props }: React.ComponentProps<typeof SheetPrimitive.Description>)`.
- **Props / inputs visible at declarations:** `SheetTrigger: { ...props }: React.ComponentProps<typeof SheetPrimitive.Trigger>`, `SheetClose: { ...props }: React.ComponentProps<typeof SheetPrimitive.Close>`, `SheetPortal: { ...props }: React.ComponentProps<typeof SheetPrimitive.Portal>`, `SheetOverlay: { className, ...props }: React.ComponentProps<typeof SheetPrimitive.Overlay>`, `SheetContent: {
  className,
  children,
  side = 'right',
  ...props
}: React.ComponentProps<typeof SheetPrimitive.Content> & {
  side?: 'top' | 'right' | 'bottom' | 'left';
}`, `SheetHeader: { className, ...props }: React.ComponentProps<'div'>`, plus 3 more.
- **Hooks / local state:** None; state bindings None; contexts None.
- **Dependencies:** internal `src/lib/utils.ts`; external `react`, `@radix-ui/react-dialog`, `lucide-react`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/components/ui/sidebar.tsx`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/components/ui/sidebar.tsx`

- **Why it exists / responsibility:** Source-owned shadcn/Radix Sidebar primitive or small field/headline/search building block.
- **Runtime and size:** Client module; 690 lines.
- **Exports / surface:** `useSidebar`, `SidebarProvider({
  defaultOpen = true,
  open: openProp,
  onOpenChange: setOpenProp,
  className,
  style,
  children,
  ...props
}: React.ComponentProps<'div'> & {
  defaultOpen?: boolean;
  open?: boolean;
  onOpenChange?: (open: boolean) => void;
})`, `handleKeyDown(event: KeyboardEvent)`, `Sidebar({
  side = 'left',
  variant = 'sidebar',
  collapsible = 'offcanvas',
  className,
  children,
  ...props
}: React.ComponentProps<'div'> & {
  side?: 'left' | 'right';
  variant?: 'sidebar' | 'floating' | 'inset';
  collapsible?: 'offcanvas' | 'icon' | 'none';
})`, `SidebarTrigger({ className, onClick, ...props }: React.ComponentProps<typeof Button>)`, `SidebarRail({ className, ...props }: React.ComponentProps<'button'>)`, `SidebarInset({ className, ...props }: React.ComponentProps<'main'>)`, `SidebarInput({ className, ...props }: React.ComponentProps<typeof Input>)`, `SidebarHeader({ className, ...props }: React.ComponentProps<'div'>)`, `SidebarFooter({ className, ...props }: React.ComponentProps<'div'>)`, `SidebarSeparator({ className, ...props }: React.ComponentProps<typeof Separator>)`, `SidebarContent({ className, ...props }: React.ComponentProps<'div'>)`, plus 13 more.
- **Props / inputs visible at declarations:** `Sidebar: {
  side = 'left',
  variant = 'sidebar',
  collapsible = 'offcanvas',
  className,
  children,
  ...props
}: React.ComponentProps<'div'> & {
  side?: 'left' | 'right';
  variant?: 'sidebar' | 'floating' | 'inset';
  collapsible?: 'offcanvas' | 'icon' | 'none';
}`, `SidebarTrigger: { className, onClick, ...props }: React.ComponentProps<typeof Button>`, `SidebarRail: { className, ...props }: React.ComponentProps<'button'>`, `SidebarInset: { className, ...props }: React.ComponentProps<'main'>`, `SidebarInput: { className, ...props }: React.ComponentProps<typeof Input>`, `SidebarHeader: { className, ...props }: React.ComponentProps<'div'>`, plus 16 more.
- **Hooks / local state:** `useContext`, `useIsMobile`, `useState`, `useCallback`, `useEffect`, `useMemo`, `useSidebar`; state bindings `openMobile`, `setOpenMobile`, `_open`, `_setOpen`; contexts `SidebarContext`.
- **Dependencies:** internal `src/hooks/use-mobile.ts`, `src/lib/utils.ts`, `src/components/ui/button.tsx`, `src/components/ui/input.tsx`, `src/components/ui/separator.tsx`, `src/components/ui/sheet.tsx`, `src/components/ui/skeleton.tsx`, `src/components/ui/tooltip.tsx`, plus 1 more; external `react`, `next/image`, `@radix-ui/react-slot`, `class-variance-authority`, `lucide-react`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/app/(withSidebarLayout)/layout.tsx`, `src/components/navbar/header.tsx`, `src/components/sidebar/app-sidebar.tsx`, `src/components/sidebar/nav-main.tsx`, `src/components/sidebar/nav-projects.tsx`, `src/components/sidebar/nav-user.tsx`, `src/components/sidebar/team-switcher.tsx`.
- **Improvement / caution:** Decompose by responsibility and add focused tests before changing behavior.

#### `src/components/ui/skeleton.tsx`

- **Why it exists / responsibility:** Source-owned shadcn/Radix Skeleton primitive or small field/headline/search building block.
- **Runtime and size:** Client-bound through an importing client module; 8 lines.
- **Exports / surface:** `Skeleton({ className, ...props }: React.ComponentProps<'div'>)`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** None; state bindings None; contexts None.
- **Dependencies:** internal `src/lib/utils.ts`; external None.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/components/ui/sidebar.tsx`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/components/ui/slider.tsx`

- **Why it exists / responsibility:** Source-owned shadcn/Radix Slider primitive or small field/headline/search building block.
- **Runtime and size:** Client module; 58 lines.
- **Exports / surface:** `Slider({
  className,
  defaultValue,
  value,
  min = 0,
  max = 100,
  ...props
}: React.ComponentProps<typeof SliderPrimitive.Root>)`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** `useMemo`; state bindings None; contexts None.
- **Dependencies:** internal `src/lib/utils.ts`; external `react`, `@radix-ui/react-slider`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/components/features/new-subscription/core-plan-montly-storage.tsx`, `src/components/features/new-subscription/core-plan-montly.tsx`, `src/components/features/new-subscription/core-plan-yearly-storage.tsx`, `src/components/features/new-subscription/core-plan-yearly.tsx`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/components/ui/sonner.tsx`

- **Why it exists / responsibility:** Source-owned shadcn/Radix Sonner primitive or small field/headline/search building block.
- **Runtime and size:** Client module; 27 lines.
- **Exports / surface:** `Toaster({ ...props }: ToasterProps)`.
- **Props / inputs visible at declarations:** `Toaster: { ...props }: ToasterProps`.
- **Hooks / local state:** `useTheme`; state bindings None; contexts None.
- **Dependencies:** internal None; external `next-themes`, `sonner`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/app/layout.tsx`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/components/ui/switch.tsx`

- **Why it exists / responsibility:** Source-owned shadcn/Radix Switch primitive or small field/headline/search building block.
- **Runtime and size:** Client module; 30 lines.
- **Exports / surface:** `Switch({ className, ...props }: React.ComponentProps<typeof SwitchPrimitive.Root>)`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** None; state bindings None; contexts None.
- **Dependencies:** internal `src/lib/utils.ts`; external `react`, `@radix-ui/react-switch`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/app/(withSidebarLayout)/upload/app-upload-modal.tsx`, `src/components/features/developer-section/version-control-tab.tsx`, `src/components/features/streaming-app/config/common-tab.tsx`, `src/components/features/streaming-app/config/config-settings-form.tsx`, `src/components/features/streaming-app/config/developer-tab.tsx`, `src/components/features/streaming-app/config/shared-form.tsx`, `src/components/features/streaming-app/config/ui-tab.tsx`, `src/components/features/subscription/subscription-details/ppm.tsx`, plus 1 more.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/components/ui/table.tsx`

- **Why it exists / responsibility:** Source-owned shadcn/Radix Table primitive or small field/headline/search building block.
- **Runtime and size:** Client module; 76 lines.
- **Exports / surface:** `Table({ className, ...props }: React.ComponentProps<'table'>)`, `TableHeader({ className, ...props }: React.ComponentProps<'thead'>)`, `TableBody({ className, ...props }: React.ComponentProps<'tbody'>)`, `TableFooter({ className, ...props }: React.ComponentProps<'tfoot'>)`, `TableRow({ className, ...props }: React.ComponentProps<'tr'>)`, `TableHead({ className, ...props }: React.ComponentProps<'th'>)`, `TableCell({ className, ...props }: React.ComponentProps<'td'>)`, `TableCaption({ className, ...props }: React.ComponentProps<'caption'>)`.
- **Props / inputs visible at declarations:** `TableHeader: { className, ...props }: React.ComponentProps<'thead'>`, `TableBody: { className, ...props }: React.ComponentProps<'tbody'>`, `TableFooter: { className, ...props }: React.ComponentProps<'tfoot'>`, `TableRow: { className, ...props }: React.ComponentProps<'tr'>`, `TableHead: { className, ...props }: React.ComponentProps<'th'>`, `TableCell: { className, ...props }: React.ComponentProps<'td'>`, plus 1 more.
- **Hooks / local state:** None; state bindings None; contexts None.
- **Dependencies:** internal `src/lib/utils.ts`; external `react`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/components/features/streaming-app/streaming-app-table.tsx`, `src/components/shared/app-display-card.tsx`, `src/components/shared/common-table.tsx`, `src/components/shared/data-table.tsx`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/components/ui/tabs.tsx`

- **Why it exists / responsibility:** Source-owned shadcn/Radix Tabs primitive or small field/headline/search building block.
- **Runtime and size:** Client module; 44 lines.
- **Exports / surface:** `Tabs({ className, ...props }: React.ComponentProps<typeof TabsPrimitive.Root>)`, `TabsList({ className, ...props }: React.ComponentProps<typeof TabsPrimitive.List>)`, `TabsTrigger({ className, ...props }: React.ComponentProps<typeof TabsPrimitive.Trigger>)`, `TabsContent({ className, ...props }: React.ComponentProps<typeof TabsPrimitive.Content>)`.
- **Props / inputs visible at declarations:** `TabsList: { className, ...props }: React.ComponentProps<typeof TabsPrimitive.List>`, `TabsTrigger: { className, ...props }: React.ComponentProps<typeof TabsPrimitive.Trigger>`, `TabsContent: { className, ...props }: React.ComponentProps<typeof TabsPrimitive.Content>`.
- **Hooks / local state:** None; state bindings None; contexts None.
- **Dependencies:** internal `src/lib/utils.ts`; external `react`, `@radix-ui/react-tabs`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/components/features/plan/features/features.tsx`, `src/components/features/plan/new-pricing-card.tsx`, `src/components/features/plan/new-pricing.tsx`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/components/ui/textarea.tsx`

- **Why it exists / responsibility:** Source-owned shadcn/Radix Textarea primitive or small field/headline/search building block.
- **Runtime and size:** Client-bound through an importing client module; 19 lines.
- **Exports / surface:** `Textarea({ className, ...props }: React.ComponentProps<'textarea'>)`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** None; state bindings None; contexts None.
- **Dependencies:** internal `src/lib/utils.ts`; external `react`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/components/features/developer-section/token-generation-tab.tsx`, `src/components/ui/field/text-area.tsx`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/components/ui/toggle-group.tsx`

- **Why it exists / responsibility:** Source-owned shadcn/Radix Toggle Group primitive or small field/headline/search building block.
- **Runtime and size:** Client module; 69 lines.
- **Exports / surface:** `ToggleGroup({
  className,
  variant,
  size,
  children,
  ...props
}: React.ComponentProps<typeof ToggleGroupPrimitive.Root> & VariantProps<typeof toggleVariants>)`, `ToggleGroupItem({
  className,
  children,
  variant,
  size,
  ...props
}: React.ComponentProps<typeof ToggleGroupPrimitive.Item> & VariantProps<typeof toggleVariants>)`.
- **Props / inputs visible at declarations:** `ToggleGroupItem: {
  className,
  children,
  variant,
  size,
  ...props
}: React.ComponentProps<typeof ToggleGroupPrimitive.Item> & VariantProps<typeof toggleVariants>`.
- **Hooks / local state:** `useContext`; state bindings None; contexts `ToggleGroupContext`.
- **Dependencies:** internal `src/lib/utils.ts`, `src/components/ui/toggle.tsx`; external `react`, `@radix-ui/react-toggle-group`, `class-variance-authority`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** None; static analysis found no internal importer, so verify framework entry/dynamic use.
- **Improvement / caution:** Confirm whether this is a framework root, planned module, or dead code before retaining it.

#### `src/components/ui/toggle.tsx`

- **Why it exists / responsibility:** Source-owned shadcn/Radix Toggle primitive or small field/headline/search building block.
- **Runtime and size:** Client module; 43 lines.
- **Exports / surface:** `Toggle({
  className,
  variant,
  size,
  ...props
}: React.ComponentProps<typeof TogglePrimitive.Root> & VariantProps<typeof toggleVariants>)`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** None; state bindings None; contexts None.
- **Dependencies:** internal `src/lib/utils.ts`; external `react`, `@radix-ui/react-toggle`, `class-variance-authority`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/components/ui/toggle-group.tsx`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/components/ui/tooltip.tsx`

- **Why it exists / responsibility:** Source-owned shadcn/Radix Tooltip primitive or small field/headline/search building block.
- **Runtime and size:** Client module; 50 lines.
- **Exports / surface:** `TooltipProvider({ delayDuration = 0, ...props }: React.ComponentProps<typeof TooltipPrimitive.Provider>)`, `Tooltip({ ...props }: React.ComponentProps<typeof TooltipPrimitive.Root>)`, `TooltipTrigger({ ...props }: React.ComponentProps<typeof TooltipPrimitive.Trigger>)`, `TooltipContent({
  className,
  sideOffset = 0,
  children,
  ...props
}: React.ComponentProps<typeof TooltipPrimitive.Content>)`.
- **Props / inputs visible at declarations:** `Tooltip: { ...props }: React.ComponentProps<typeof TooltipPrimitive.Root>`, `TooltipTrigger: { ...props }: React.ComponentProps<typeof TooltipPrimitive.Trigger>`, `TooltipContent: {
  className,
  sideOffset = 0,
  children,
  ...props
}: React.ComponentProps<typeof TooltipPrimitive.Content>`.
- **Hooks / local state:** None; state bindings None; contexts None.
- **Dependencies:** internal `src/lib/utils.ts`; external `react`, `@radix-ui/react-tooltip`.
- **Rendering and rerenders:** Renders JSX. It rerenders when its props, consumed context/store query state, or listed local state changes; memoization is not assumed unless implemented in the file.
- **Direct consumers:** `src/components/features/additional-upload/additional-upload-page.tsx`, `src/components/features/dedicated-server/dedicated-server-card.tsx`, `src/components/features/new-subscription/new-payment-info.tsx`, `src/components/features/plan/dialog/prepaid-minute.tsx`, `src/components/features/plan/features/features.tsx`, `src/components/features/plan/pricing-card.tsx`, `src/components/features/streaming-app/config/shared-form.tsx`, `src/components/features/streaming-app/streaming-app-card.tsx`, plus 3 more.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

### src/config

#### `src/config/api.ts`

- **Why it exists / responsibility:** Api application configuration source.
- **Runtime and size:** Shared/build-time eligible module; 175 lines.
- **Exports / surface:** `PREVIOUS_CP_URL`, `HOME_URL`, `EAGLE_DOMAIN`, `CURRENT_CP_URL`, `R2_UPLOAD_DOMAIN`, `API`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** None; state bindings None; contexts None.
- **Dependencies:** internal `src/config/config-loader.ts`; external None.
- **Rendering and rerenders:** No JSX detected; it runs when imported or when an exported function/hook is called.
- **Direct consumers:** `src/app/(withSidebarLayout)/layout.tsx`, `src/components/error/error-service.ts`, `src/components/features/streaming-app/api-notify.ts`, `src/components/features/subscription/subscription-details/ppl.tsx`, `src/components/providers/upload/apiCall.ts`, `src/components/providers/upload/upload-sequence-provider.tsx`, `src/config/endpoint-selector.ts`, `src/services/r2/r2-upload-api.ts`, plus 16 more.
- **Improvement / caution:** Remove confirmed-dead constants and generate typed clients from an API schema.

#### `src/config/config-loader.ts`

- **Why it exists / responsibility:** Config Loader application configuration source.
- **Runtime and size:** Shared/build-time eligible module; 37 lines.
- **Exports / surface:** `default`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** None; state bindings None; contexts None.
- **Dependencies:** internal `config.prod.json`, `config.staging.json`; external None.
- **Rendering and rerenders:** No JSX detected; it runs when imported or when an exported function/hook is called.
- **Direct consumers:** `src/config/api.ts`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/config/doc.ts`

- **Why it exists / responsibility:** Doc application configuration source.
- **Runtime and size:** Shared/build-time eligible module; 56 lines.
- **Exports / surface:** `DOC`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** None; state bindings None; contexts None.
- **Dependencies:** internal None; external None.
- **Rendering and rerenders:** No JSX detected; it runs when imported or when an exported function/hook is called.
- **Direct consumers:** `src/components/features/developer-section/developer-section-page.tsx`, `src/components/features/my-account/profile-settings.tsx`, `src/components/features/my-account/transaction.tsx`, `src/components/features/new-subscription/core-plan-montly.tsx`, `src/components/features/new-subscription/core-plan-yearly.tsx`, `src/components/features/new-subscription/new-payment-info.tsx`, `src/components/features/new-subscription/new-trial.tsx`, `src/components/features/plan/pricing-card.tsx`, plus 11 more.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/config/endpoint-selector.ts`

- **Why it exists / responsibility:** Endpoint Selector application configuration source.
- **Runtime and size:** Shared/build-time eligible module; 43 lines.
- **Exports / surface:** `getStreamingAppListEndpoint`, `getDeleteStreamingAppEndpoint`, `getDeleteStreamingAppVersionEndpoint`, `selectEndpoint`, `getStreamingAppListEndpoint`, `getDeleteStreamingAppEndpoint`, `getDeleteStreamingAppVersionEndpoint`, `selectEndpoint(coreWaveEndpoint: string, r2Endpoint: string)`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** None; state bindings None; contexts None.
- **Dependencies:** internal `src/config/api.ts`, `src/services/storage-provider.ts`; external None.
- **Rendering and rerenders:** No JSX detected; it runs when imported or when an exported function/hook is called.
- **Direct consumers:** `src/store/api/appLink.ts`, `src/store/api/asset.ts`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/config/firebase.ts`

- **Why it exists / responsibility:** Firebase application configuration source.
- **Runtime and size:** Shared/build-time eligible module; 71 lines.
- **Exports / surface:** `appExeDataFBConfig`, `firebaseServices`, `initializeFirebaseApp(config: FirebaseConfig, appName: string)`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** None; state bindings None; contexts None.
- **Dependencies:** internal None; external `firebase/app`, `firebase/firestore`, `firebase/auth`.
- **Rendering and rerenders:** No JSX detected; it runs when imported or when an exported function/hook is called.
- **Direct consumers:** `src/components/features/signin/signin-page.tsx`, `src/components/features/singup/signup-page.tsx`, `src/components/features/streaming-app/application.tsx`, `src/components/features/streaming-app/streaming-app-section.tsx`, `src/components/features/utilities/multiplayer-app-monitor.tsx`, `src/components/features/utilities/streaming-app-monitor.tsx`, `src/components/providers/upload/apiCall.ts`, `src/components/providers/upload/upload-sequence-provider.tsx`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/config/page.ts`

- **Why it exists / responsibility:** Page application configuration source.
- **Runtime and size:** Shared/build-time eligible module; 21 lines.
- **Exports / surface:** `PAGES`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** None; state bindings None; contexts None.
- **Dependencies:** internal None; external None.
- **Rendering and rerenders:** No JSX detected; it runs when imported or when an exported function/hook is called.
- **Direct consumers:** `src/app/(with-public-layout)/fb-mail/page.tsx`, `src/app/(withSidebarLayout)/layout.tsx`, `src/components/auth/super-admin-handler.tsx`, `src/components/features/developer-section/developer-section-page.tsx`, `src/components/features/my-account/profile-settings.tsx`, `src/components/features/new-subscription/core-plan-montly-storage.tsx`, `src/components/features/new-subscription/core-plan-montly.tsx`, `src/components/features/new-subscription/core-plan-yearly-storage.tsx`, plus 19 more.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/config/url.ts`

- **Why it exists / responsibility:** Url application configuration source.
- **Runtime and size:** Shared/build-time eligible module; 14 lines.
- **Exports / surface:** `URL`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** None; state bindings None; contexts None.
- **Dependencies:** internal None; external None.
- **Rendering and rerenders:** No JSX detected; it runs when imported or when an exported function/hook is called.
- **Direct consumers:** `src/components/features/plan/faq.tsx`, `src/components/features/plan/features/features.tsx`, `src/components/features/plan/new-pricing-card.tsx`, `src/components/features/plan/new-pricing.tsx`, `src/components/features/plan/plan-page.tsx`, `src/components/features/streaming-app/meeting-link-tab.tsx`, `src/components/features/streaming-app/streaming-link-tab.tsx`, `src/components/features/utilities/download-content.tsx`, plus 1 more.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

### src/constant

#### `src/constant/asset.ts`

- **Why it exists / responsibility:** Asset business/UI constants.
- **Runtime and size:** Shared/build-time eligible module; 9 lines.
- **Exports / surface:** `ASSET`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** None; state bindings None; contexts None.
- **Dependencies:** internal None; external None.
- **Rendering and rerenders:** No JSX detected; it runs when imported or when an exported function/hook is called.
- **Direct consumers:** `src/components/features/dedicated-server/application.tsx`, `src/components/features/dedicated-server/dedicated-server-page.tsx`, `src/components/features/my-account/upload-profile.tsx`, `src/components/features/streaming-app/config/image-upload.tsx`, `src/components/features/streaming-app/video-upload.tsx`, `src/components/providers/upload/upload-sequence-provider.tsx`, `src/hooks/use-common-upload.ts`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/constant/plan.ts`

- **Why it exists / responsibility:** Plan business/UI constants.
- **Runtime and size:** Shared/build-time eligible module; 20 lines.
- **Exports / surface:** `PLAN_NAME`, `PLAN_STATUS`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** None; state bindings None; contexts None.
- **Dependencies:** internal None; external None.
- **Rendering and rerenders:** No JSX detected; it runs when imported or when an exported function/hook is called.
- **Direct consumers:** `src/components/features/plan/dialog/prepaid-minute.tsx`, `src/components/features/plan/plan-page.tsx`, `src/components/features/post-payment/payment-successful-page.tsx`, `src/components/features/subscription/bill-summary.tsx`, `src/components/features/subscription/modal/ppccu-ppl-modify-modal.tsx`, `src/components/features/subscription/subscription-details/ppccu.tsx`, `src/components/features/subscription/subscription-details/ppm.tsx`, `src/components/features/subscription/subscription-summary/subscription-summary-section.tsx`, plus 5 more.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

### src/helpers/axios

#### `src/helpers/axios/axiosBaseQuery.ts`

- **Why it exists / responsibility:** Shared Axios transport axios base query layer.
- **Runtime and size:** Shared/build-time eligible module; 49 lines.
- **Exports / surface:** `axiosBaseQuery`, `axiosBaseQuery({ baseUrl }: TBaseUrl)`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** None; state bindings None; contexts None.
- **Dependencies:** internal `src/helpers/axios/axiosInstance.ts`; external `@reduxjs/toolkit/query`, `axios`.
- **Rendering and rerenders:** No JSX detected; it runs when imported or when an exported function/hook is called.
- **Direct consumers:** `src/store/api/base-api.ts`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/helpers/axios/axiosInstance.ts`

- **Why it exists / responsibility:** Shared Axios transport axios instance layer.
- **Runtime and size:** Shared/build-time eligible module; 34 lines.
- **Exports / surface:** None.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** None; state bindings None; contexts None.
- **Dependencies:** internal None; external `axios`.
- **Rendering and rerenders:** No JSX detected; it runs when imported or when an exported function/hook is called.
- **Direct consumers:** `src/helpers/axios/axiosBaseQuery.ts`, `src/utils/notifications.ts`.
- **Improvement / caution:** Reject interceptor errors and add transport tests.

### src/hooks

#### `src/hooks/use-card-details-v2.ts`

- **Why it exists / responsibility:** Custom Use Card Details V2 hook; packages reusable server-state, derived business state, or browser behavior.
- **Runtime and size:** Client-bound through an importing client module; 25 lines.
- **Exports / surface:** `useCardDetailsV2`, `useCardDetailsV2`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** `useUserInfo`, `useSubscriptionV2`, `usePaymentMethodQuery`, `useDefaultPaymentMethodQuery`; state bindings None; contexts None.
- **Dependencies:** internal `src/store/api/credit-card.ts`, `src/hooks/use-subscription-v2.ts`, `src/hooks/use-user-info.ts`; external None.
- **Rendering and rerenders:** No JSX detected; it runs when imported or when an exported function/hook is called.
- **Direct consumers:** `src/components/features/new-subscription/core-plan-montly-storage.tsx`, `src/components/features/new-subscription/core-plan-montly.tsx`, `src/components/features/new-subscription/core-plan-yearly-storage.tsx`, `src/components/features/new-subscription/core-plan-yearly.tsx`, `src/components/features/new-subscription/new-payment-info.tsx`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/hooks/use-card-details.ts`

- **Why it exists / responsibility:** Custom Use Card Details hook; packages reusable server-state, derived business state, or browser behavior.
- **Runtime and size:** Client-bound through an importing client module; 34 lines.
- **Exports / surface:** `useCardDetails`, `useCardDetails`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** `useSubscription`, `useUserInfo`, `usePaymentMethodQuery`, `useDefaultPaymentMethodQuery`; state bindings None; contexts None.
- **Dependencies:** internal `src/store/api/credit-card.ts`, `src/hooks/use-subscription.ts`, `src/hooks/use-user-info.ts`, `src/constant/plan.ts`; external None.
- **Rendering and rerenders:** No JSX detected; it runs when imported or when an exported function/hook is called.
- **Direct consumers:** `src/components/features/subscription/modal/ppccu-ppl-modify-modal.tsx`, `src/components/features/subscription/modal/prepaid-minute-modify-modal.tsx`, `src/components/features/subscription/modal/storage-modify-modal.tsx`, `src/components/features/subscription/payment-info.tsx`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/hooks/use-ccu-data.ts`

- **Why it exists / responsibility:** Custom Use Ccu Data hook; packages reusable server-state, derived business state, or browser behavior.
- **Runtime and size:** Client-bound through an importing client module; 209 lines.
- **Exports / surface:** `TStreamRecord`, `TCcuFrequencyInterval`, `TCcuStats`, `TUseCCUDataResult`, `useCCUData`, `useCCUDataFromRecords`, `useCCUData(startDate: Date, endDate: Date)`, `useCCUDataFromRecords({ rows, loading = false, error = null }: UseCCUFromRecordsParams)`, `calculateFrequency(intervals: Array<Pick<TCcuFrequencyInterval, 'start' | 'end'>>)`, `calculateFrequencyStatsWithIntervals(data: TCcuFrequencyInterval[])`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** `useCallback`, `useMemo`; state bindings None; contexts None.
- **Dependencies:** internal None; external `react`.
- **Rendering and rerenders:** No JSX detected; it runs when imported or when an exported function/hook is called.
- **Direct consumers:** `src/components/features/analytics/ccu-tab.tsx`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/hooks/use-common-upload.ts`

- **Why it exists / responsibility:** Custom Use Common Upload hook; packages reusable server-state, derived business state, or browser behavior.
- **Runtime and size:** Client-bound through an importing client module; 121 lines.
- **Exports / surface:** `useCommonUpload`, `useCommonUpload({ assetType, appName }: CommonUploadProps)`, `handleUpload`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** `useUserInfo`, `useProfileLogoUploadSignedUrlMutation`, `useAsset2dUploadSignedUrlMutation`, `useAssetVideoUploadSignedUrlMutation`, `useAppThumbnailSignUrlMutation`, `useState`; state bindings `file`, `setFile`, `isUploading`, `setIsUploading`, `uploadProgress`, `setUploadProgress`, `uploadSpeed`, `setUploadSpeed`, `estimatedTime`, `setEstimatedTime`; contexts None.
- **Dependencies:** internal `src/hooks/use-user-info.ts`, `src/constant/asset.ts`, `src/store/api/upload.ts`, `src/store/api/user-info.ts`, `src/components/features/streaming-app/api-notify.ts`; external `react`, `axios`, `sonner`.
- **Rendering and rerenders:** No JSX detected; it runs when imported or when an exported function/hook is called.
- **Direct consumers:** `src/components/features/my-account/upload-profile.tsx`, `src/components/features/streaming-app/config/image-upload.tsx`, `src/components/features/streaming-app/video-upload.tsx`, `src/components/providers/upload/upload-sequence-provider.tsx`.
- **Improvement / caution:** Consider a reducer/state machine to make transitions explicit.

#### `src/hooks/use-customer-id.ts`

- **Why it exists / responsibility:** Custom Use Customer Id hook; packages reusable server-state, derived business state, or browser behavior.
- **Runtime and size:** Client-bound through an importing client module; 20 lines.
- **Exports / surface:** `useCustomerId`, `useCustomerId`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** `useLoginStatusQuery`, `useApiKeyQuery`, `useCustomerIdQuery`; state bindings None; contexts None.
- **Dependencies:** internal `src/store/api/api-key.ts`, `src/store/api/auth.ts`, `src/store/api/user-info.ts`; external None.
- **Rendering and rerenders:** No JSX detected; it runs when imported or when an exported function/hook is called.
- **Direct consumers:** `src/components/features/plan/dialog/ppm.tsx`, `src/components/features/subscription/subscription-details/terminated-sub.tsx`, `src/hooks/use-user-info.ts`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/hooks/use-debounce.ts`

- **Why it exists / responsibility:** Custom Use Debounce hook; packages reusable server-state, derived business state, or browser behavior.
- **Runtime and size:** Client-bound through an importing client module; 27 lines.
- **Exports / surface:** `useDebounce`, `useDebounce(callback: (...args: T) => void, delay: number)`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** `useRef`, `useEffect`, `useCallback`; state bindings None; contexts None.
- **Dependencies:** internal None; external `react`.
- **Rendering and rerenders:** No JSX detected; it runs when imported or when an exported function/hook is called.
- **Direct consumers:** `src/components/features/singup/signup-page.tsx`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/hooks/use-has-pause-sub-old.tsx`

- **Why it exists / responsibility:** Custom Use Has Pause Sub Old hook; packages reusable server-state, derived business state, or browser behavior.
- **Runtime and size:** Client-bound through an importing client module; 37 lines.
- **Exports / surface:** `useOldSubPause`, `useOldSubPause`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** `useSubscription`; state bindings None; contexts None.
- **Dependencies:** internal `src/hooks/use-subscription.ts`; external None.
- **Rendering and rerenders:** No JSX detected; it runs when imported or when an exported function/hook is called.
- **Direct consumers:** `src/components/features/subscription/payment-info.tsx`, `src/components/features/subscription/subscription-details/ppccu.tsx`, `src/components/features/subscription/subscription-details/ppl.tsx`, `src/components/features/subscription/subscription-details/ppm.tsx`, `src/components/features/subscription/subscription-details/prepaid-minute.tsx`, `src/components/features/subscription/subscription-summary/subscription-summary-section.tsx`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/hooks/use-has-pause-sub.ts`

- **Why it exists / responsibility:** Custom Use Has Pause Sub hook; packages reusable server-state, derived business state, or browser behavior.
- **Runtime and size:** Client-bound through an importing client module; 40 lines.
- **Exports / surface:** `useHasPauseSubscription`, `useHasPauseSubscription`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** `useSubscriptionV2`, `useMinutesV2`, `useStorageV2`; state bindings None; contexts None.
- **Dependencies:** internal `src/hooks/use-storageV2.ts`, `src/hooks/use-minute-v2.ts`, `src/hooks/use-subscription-v2.ts`; external None.
- **Rendering and rerenders:** No JSX detected; it runs when imported or when an exported function/hook is called.
- **Direct consumers:** `src/components/features/new-subscription/core-plan-montly-storage.tsx`, `src/components/features/new-subscription/core-plan-montly.tsx`, `src/components/features/new-subscription/core-plan-yearly-storage.tsx`, `src/components/features/new-subscription/core-plan-yearly.tsx`, `src/components/features/new-subscription/new-payment-info.tsx`, `src/components/features/subscription/subscription-summary/subscription-summary-section.tsx`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/hooks/use-minute-v2.ts`

- **Why it exists / responsibility:** Custom Use Minute V2 hook; packages reusable server-state, derived business state, or browser behavior.
- **Runtime and size:** Client-bound through an importing client module; 65 lines.
- **Exports / surface:** `useMinutesV2`, `useMinutesV2`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** `useUserInfo`, `useSubscriptionV2`, `useCheckStatusQuery`, `useCheckStatusMinQuery`; state bindings None; contexts None.
- **Dependencies:** internal `src/store/api/payment.ts`, `src/hooks/use-user-info.ts`, `src/store/api/minute.ts`, `src/hooks/use-subscription-v2.ts`; external None.
- **Rendering and rerenders:** No JSX detected; it runs when imported or when an exported function/hook is called.
- **Direct consumers:** `src/components/features/dedicated-server/application.tsx`, `src/components/features/new-subscription/core-plan-montly.tsx`, `src/components/features/new-subscription/core-plan-yearly.tsx`, `src/components/features/new-subscription/new-billing.tsx`, `src/components/features/streaming-app/application.tsx`, `src/components/features/subscription/new-subscription-summary/core-plan.tsx`, `src/hooks/use-has-pause-sub.ts`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/hooks/use-minute.ts`

- **Why it exists / responsibility:** Custom Use Minute hook; packages reusable server-state, derived business state, or browser behavior.
- **Runtime and size:** Client-bound through an importing client module; 36 lines.
- **Exports / surface:** `useMinute`, `useMinute`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** `useUserInfo`, `useSubscription`, `useTrial`, `useMinuteStreamedQuery`; state bindings None; contexts None.
- **Dependencies:** internal `src/constant/plan.ts`, `src/hooks/use-subscription.ts`, `src/store/api/minute.ts`, `src/store/api/api-key.ts`, `src/hooks/use-trial.ts`, `src/hooks/use-user-info.ts`; external None.
- **Rendering and rerenders:** No JSX detected; it runs when imported or when an exported function/hook is called.
- **Direct consumers:** `src/components/features/subscription/bill-summary-ppm.tsx`, `src/components/features/subscription/subscription-details/ppccu.tsx`, `src/components/features/subscription/subscription-details/ppl.tsx`, `src/components/features/subscription/subscription-details/ppm.tsx`, `src/components/features/subscription/subscription-details/terminated-sub.tsx`, `src/components/features/subscription/subscription-summary/ppccu-subscription.tsx`, `src/components/features/subscription/subscription-summary/ppl-subscription.tsx`, `src/components/features/subscription/subscription-summary/ppm-subscription.tsx`, plus 1 more.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/hooks/use-mobile.ts`

- **Why it exists / responsibility:** Custom Use Mobile hook; packages reusable server-state, derived business state, or browser behavior.
- **Runtime and size:** Client-bound through an importing client module; 23 lines.
- **Exports / surface:** `useIsMobile`, `useIsMobile`, `onChange`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** `useState`, `useEffect`; state bindings `isMobile`, `setIsMobile`; contexts None.
- **Dependencies:** internal None; external `react`.
- **Rendering and rerenders:** No JSX detected; it runs when imported or when an exported function/hook is called.
- **Direct consumers:** `src/components/ui/sidebar.tsx`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/hooks/use-storage.ts`

- **Why it exists / responsibility:** Custom Use Storage hook; packages reusable server-state, derived business state, or browser behavior.
- **Runtime and size:** Client module; 55 lines.
- **Exports / surface:** `useStorage`, `useStorage`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** `useUserInfo`, `useStorageStatusQuery`, `useStorageUtilizedQuery`; state bindings None; contexts None.
- **Dependencies:** internal `src/store/api/storage.ts`, `src/utils/common.ts`, `src/hooks/use-user-info.ts`; external None.
- **Rendering and rerenders:** No JSX detected; it runs when imported or when an exported function/hook is called.
- **Direct consumers:** `src/app/(withSidebarLayout)/upload/app-upload-modal.tsx`, `src/app/(withSidebarLayout)/upload/upload-details.tsx`, `src/components/features/subscription/bill-summary-ppm.tsx`, `src/components/features/subscription/bill-summary.tsx`, `src/components/features/subscription/modal/storage-modify-modal.tsx`, `src/components/features/subscription/payment-info.tsx`, `src/components/features/subscription/storage-utilized.tsx`, `src/components/features/subscription/subscription-details/ppccu.tsx`, plus 9 more.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/hooks/use-storageV2.ts`

- **Why it exists / responsibility:** Custom Use Storage V2 hook; packages reusable server-state, derived business state, or browser behavior.
- **Runtime and size:** Client-bound through an importing client module; 67 lines.
- **Exports / surface:** `useStorageV2`, `useStorageV2`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** `useUserInfo`, `useSubscriptionV2`, `useCheckStatusGbQuery`, `useStorageUtilizedQuery`; state bindings None; contexts None.
- **Dependencies:** internal `src/store/api/payment.ts`, `src/hooks/use-user-info.ts`, `src/store/api/storage.ts`, `src/hooks/use-subscription-v2.ts`; external None.
- **Rendering and rerenders:** No JSX detected; it runs when imported or when an exported function/hook is called.
- **Direct consumers:** `src/app/(withSidebarLayout)/upload/app-upload-modal.tsx`, `src/app/(withSidebarLayout)/upload/upload-details.tsx`, `src/components/features/dedicated-server/application.tsx`, `src/components/features/new-subscription/core-plan-montly-storage.tsx`, `src/components/features/new-subscription/core-plan-yearly-storage.tsx`, `src/components/features/new-subscription/new-billing.tsx`, `src/components/features/streaming-app/application.tsx`, `src/components/features/subscription/new-subscription-summary/core-plan.tsx`, plus 2 more.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/hooks/use-streamed.ts`

- **Why it exists / responsibility:** Custom Use Streamed hook; packages reusable server-state, derived business state, or browser behavior.
- **Runtime and size:** Client-bound through an importing client module; 52 lines.
- **Exports / surface:** `useStreamed`, `useStreamed`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** `useUserInfo`, `useCheckStatusQuery`, `useMinuteStreamedQuery`; state bindings None; contexts None.
- **Dependencies:** internal `src/store/api/minute.ts`, `src/hooks/use-user-info.ts`, `src/store/api/payment.ts`; external None.
- **Rendering and rerenders:** No JSX detected; it runs when imported or when an exported function/hook is called.
- **Direct consumers:** None; static analysis found no internal importer, so verify framework entry/dynamic use.
- **Improvement / caution:** Confirm whether this is a framework root, planned module, or dead code before retaining it.

#### `src/hooks/use-streaming-app.ts`

- **Why it exists / responsibility:** Custom Use Streaming App hook; packages reusable server-state, derived business state, or browser behavior.
- **Runtime and size:** Client-bound through an importing client module; 22 lines.
- **Exports / surface:** `useStreamingApp`, `useStreamingApp`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** `useUserInfo`, `useStreamingAppQuery`, `useStreamingAppThumbnailQuery`; state bindings None; contexts None.
- **Dependencies:** internal `src/store/api/asset.ts`, `src/hooks/use-user-info.ts`; external None.
- **Rendering and rerenders:** No JSX detected; it runs when imported or when an exported function/hook is called.
- **Direct consumers:** `src/components/features/streaming-app/application.tsx`, `src/components/features/streaming-app/streaming-app-page.tsx`, `src/components/features/streaming-app/utils.ts`, `src/components/providers/upload/apiCall.ts`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/hooks/use-subscription-v2.ts`

- **Why it exists / responsibility:** Custom Use Subscription V2 hook; packages reusable server-state, derived business state, or browser behavior.
- **Runtime and size:** Client-bound through an importing client module; 96 lines.
- **Exports / surface:** `useSubscriptionV2`, `useSubscriptionV2`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** `useUserInfo`, `useUserAllInfoQuery`, `useCheckStatusQuery`, `useMinuteStreamedQuery`; state bindings None; contexts None.
- **Dependencies:** internal `src/store/api/payment.ts`, `src/hooks/use-user-info.ts`, `src/utils/date-formatter.ts`, `src/store/api/minute.ts`, `src/store/api/user-info.ts`; external None.
- **Rendering and rerenders:** No JSX detected; it runs when imported or when an exported function/hook is called.
- **Direct consumers:** `src/app/(withSidebarLayout)/credit-card-successful-core/page.tsx`, `src/app/(withSidebarLayout)/upload/app-upload-modal.tsx`, `src/app/(withSidebarLayout)/upload/upload-details.tsx`, `src/components/features/dedicated-server/application.tsx`, `src/components/features/new-subscription/core-plan-montly-storage.tsx`, `src/components/features/new-subscription/core-plan-montly.tsx`, `src/components/features/new-subscription/core-plan-yearly-storage.tsx`, `src/components/features/new-subscription/core-plan-yearly.tsx`, plus 15 more.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/hooks/use-subscription.ts`

- **Why it exists / responsibility:** Custom Use Subscription hook; packages reusable server-state, derived business state, or browser behavior.
- **Runtime and size:** Client module; 80 lines.
- **Exports / surface:** `useSubscription`, `useSubscription`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** `useUserInfo`, `usePpccuStatusQuery`, `usePplStatusQuery`, `usePrepaidMinuteStatusQuery`, `usePpmStatusQuery`; state bindings None; contexts None.
- **Dependencies:** internal `src/constant/plan.ts`, `src/store/api/api-key.ts`, `src/store/api/auth.ts`, `src/store/api/subscription.ts`, `src/utils/common.ts`, `src/hooks/use-user-info.ts`; external None.
- **Rendering and rerenders:** No JSX detected; it runs when imported or when an exported function/hook is called.
- **Direct consumers:** `src/app/(withSidebarLayout)/upload/app-upload-modal.tsx`, `src/app/(withSidebarLayout)/upload/upload-details.tsx`, `src/components/features/dedicated-server/application.tsx`, `src/components/features/plan/dialog/pccu.tsx`, `src/components/features/plan/dialog/ppl.tsx`, `src/components/features/plan/dialog/ppm.tsx`, `src/components/features/plan/dialog/prepaid-minute.tsx`, `src/components/features/plan/new-pricing.tsx`, plus 25 more.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/hooks/use-trial-v2.ts`

- **Why it exists / responsibility:** Custom Use Trial V2 hook; packages reusable server-state, derived business state, or browser behavior.
- **Runtime and size:** Client-bound through an importing client module; 68 lines.
- **Exports / surface:** `useTrialV2`, `useTrialV2`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** `useUserInfo`, `useUserAllInfoQuery`, `useStorageUtilizedQuery`, `useMinuteStreamedQuery`; state bindings None; contexts None.
- **Dependencies:** internal `src/store/api/storage.ts`, `src/hooks/use-user-info.ts`, `src/hooks/use-subscription-v2.ts`, `src/store/api/minute.ts`, `src/store/api/user-info.ts`; external None.
- **Rendering and rerenders:** No JSX detected; it runs when imported or when an exported function/hook is called.
- **Direct consumers:** `src/components/features/dedicated-server/application.tsx`, `src/components/features/new-subscription/new-trial.tsx`, `src/components/features/streaming-app/application.tsx`, `src/components/features/subscription/new-subscription-summary/expried-trial-plan.tsx`, `src/components/features/subscription/new-subscription-summary/trial-plan.tsx`, `src/components/features/subscription/subscription-summary/subscription-summary-section.tsx`, `src/components/features/subscription/subscription.tsx`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/hooks/use-trial.ts`

- **Why it exists / responsibility:** Custom Use Trial hook; packages reusable server-state, derived business state, or browser behavior.
- **Runtime and size:** Client-bound through an importing client module; 29 lines.
- **Exports / surface:** `useTrial`, `useTrial`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** `useApiKeyQuery`, `useUserAllInfoQuery`; state bindings None; contexts None.
- **Dependencies:** internal `src/store/api/api-key.ts`, `src/store/api/user-info.ts`, `src/utils/date-formatter.ts`; external None.
- **Rendering and rerenders:** No JSX detected; it runs when imported or when an exported function/hook is called.
- **Direct consumers:** `src/components/features/streaming-app/application.tsx`, `src/components/features/streaming-app/streaming-app-page.tsx`, `src/components/features/subscription/subscription-details/trial.tsx`, `src/components/features/subscription/subscription-summary/subscription-summary-section.tsx`, `src/components/features/subscription/subscription-summary/trial-subscription.tsx`, `src/components/features/subscription/subscription.tsx`, `src/hooks/use-minute.ts`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/hooks/use-user-info.ts`

- **Why it exists / responsibility:** Custom Use User Info hook; packages reusable server-state, derived business state, or browser behavior.
- **Runtime and size:** Client-bound through an importing client module; 104 lines.
- **Exports / surface:** `useUserInfo`, `useUserInfo`, `checkSuperAdminSession`, `handleStorageChange(e: StorageEvent)`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** `useState`, `useLoginStatusQuery`, `useApiKeyQuery`, `useCustomerId`, `useEffect`; state bindings `selectedMember`, `setSelectedMember`, `isSuperAdmin`, `setIsSuperAdmin`; contexts None.
- **Dependencies:** internal `src/store/api/api-key.ts`, `src/store/api/auth.ts`, `src/hooks/use-customer-id.ts`, `src/utils/super-admin-login.ts`; external `react`.
- **Rendering and rerenders:** No JSX detected; it runs when imported or when an exported function/hook is called.
- **Direct consumers:** `src/app/(withSidebarLayout)/credit-card-successful-core/page.tsx`, `src/components/error/uncaught-error-catcher.tsx`, `src/components/features/additional-upload/additional-upload-page.tsx`, `src/components/features/analytics/analytics-page.tsx`, `src/components/features/dedicated-server/application.tsx`, `src/components/features/dedicated-server/dedicated-server-card-details.tsx`, `src/components/features/dedicated-server/dedicated-server-page.tsx`, `src/components/features/developer-section/developer-section-page.tsx`, plus 69 more.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

### src/lib

#### `src/lib/utils.ts`

- **Why it exists / responsibility:** Utils low-level shared library helper.
- **Runtime and size:** Shared/build-time eligible module; 7 lines.
- **Exports / surface:** `cn`, `cn(...inputs: ClassValue[])`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** None; state bindings None; contexts None.
- **Dependencies:** internal None; external `clsx`, `tailwind-merge`.
- **Rendering and rerenders:** No JSX detected; it runs when imported or when an exported function/hook is called.
- **Direct consumers:** `src/app/(withSidebarLayout)/upload/StorageProgressBar.tsx`, `src/app/(withSidebarLayout)/upload/upload-details.tsx`, `src/components/features/additional-upload/additional-upload-details.tsx`, `src/components/features/dedicated-server/dedicated-server-upload-details.tsx`, `src/components/features/plan/pricing-card.tsx`, `src/components/features/streaming-app/config/shared-form.tsx`, `src/components/shared/file-drop-zone.tsx`, `src/components/shared/tabs.tsx`, plus 33 more.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

### src/services/r2

#### `src/services/r2/optimized-r2-upload.ts`

- **Why it exists / responsibility:** Cloudflare R2 multipart-upload optimized r2 upload service.
- **Runtime and size:** Shared/build-time eligible module; 247 lines.
- **Exports / surface:** `OptimizedR2Upload`, `OptimizedR2Upload`, `reportProgress`, `attemptUpload(attempt: number)`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** None; state bindings None; contexts None.
- **Dependencies:** internal `src/types/r2-upload.ts`, `src/services/r2/r2-upload-api.ts`; external `axios`.
- **Rendering and rerenders:** No JSX detected; it runs when imported or when an exported function/hook is called.
- **Direct consumers:** `src/components/providers/upload/upload-sequence-provider.tsx`.
- **Improvement / caution:** Confirm cancellation, error normalization, and cache synchronization for direct Axios traffic.

#### `src/services/r2/r2-upload-api.ts`

- **Why it exists / responsibility:** Cloudflare R2 multipart-upload r2 upload api service.
- **Runtime and size:** Shared/build-time eligible module; 72 lines.
- **Exports / surface:** `R2UploadApi`, `getApiHeaders(apiKey: string)`, `unwrapAgwData(response: { data: AgwSuccessResponse<T> })`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** None; state bindings None; contexts None.
- **Dependencies:** internal `src/config/api.ts`, `src/types/r2-upload.ts`; external `axios`.
- **Rendering and rerenders:** No JSX detected; it runs when imported or when an exported function/hook is called.
- **Direct consumers:** `src/services/r2/optimized-r2-upload.ts`.
- **Improvement / caution:** Confirm cancellation, error normalization, and cache synchronization for direct Axios traffic.

### src/services

#### `src/services/storage-provider.ts`

- **Why it exists / responsibility:** Storage Provider integration service outside React.
- **Runtime and size:** Shared/build-time eligible module; 20 lines.
- **Exports / surface:** `getStorageProvider`, `getStorageProvider`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** None; state bindings None; contexts None.
- **Dependencies:** internal `src/config/api.ts`; external `axios`.
- **Rendering and rerenders:** No JSX detected; it runs when imported or when an exported function/hook is called.
- **Direct consumers:** `src/components/providers/upload/upload-sequence-provider.tsx`, `src/config/endpoint-selector.ts`.
- **Improvement / caution:** Confirm cancellation, error normalization, and cache synchronization for direct Axios traffic.

### src/store/api

#### `src/store/api/additional-uploads.ts`

- **Why it exists / responsibility:** RTK Query Additional Uploads module; currently contains no endpoint implementation.
- **Runtime and size:** Shared/build-time eligible module; 1 lines.
- **Exports / surface:** None.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** None; state bindings None; contexts None.
- **Dependencies:** internal None; external None.
- **Rendering and rerenders:** No JSX detected; it runs when imported or when an exported function/hook is called.
- **Direct consumers:** None; static analysis found no internal importer, so verify framework entry/dynamic use.
- **Improvement / caution:** Remove if obsolete or add a tracked issue and implementation contract.

#### `src/store/api/analytics.ts`

- **Why it exists / responsibility:** RTK Query endpoint injection for Analytics: streamRecord.
- **Runtime and size:** Shared/build-time eligible module; 17 lines.
- **Exports / surface:** `useStreamRecordQuery`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** None; state bindings None; contexts None.
- **Dependencies:** internal `src/config/api.ts`, `src/store/api/base-api.ts`; external None.
- **Rendering and rerenders:** No JSX detected; it runs when imported or when an exported function/hook is called.
- **Direct consumers:** `src/components/features/analytics/analytics-page.tsx`.
- **Improvement / caution:** Add typed request/response generics and document auth/error contracts.

#### `src/store/api/api-key.ts`

- **Why it exists / responsibility:** RTK Query endpoint injection for Api Key: apiKey, generateToken, generateApiKey, updateVersionControl.
- **Runtime and size:** Shared/build-time eligible module; 48 lines.
- **Exports / surface:** `useApiKeyQuery`, `useGenerateApiKeyMutation`, `useGenerateTokenMutation`, `useUpdateVersionControlMutation`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** None; state bindings None; contexts None.
- **Dependencies:** internal `src/config/api.ts`, `src/store/api/base-api.ts`; external None.
- **Rendering and rerenders:** No JSX detected; it runs when imported or when an exported function/hook is called.
- **Direct consumers:** `src/app/(withSidebarLayout)/layout.tsx`, `src/components/features/developer-section/api-key-tab.tsx`, `src/components/features/developer-section/streaming-api-key-tab.tsx`, `src/components/features/developer-section/token-generation-tab.tsx`, `src/components/features/developer-section/version-control-tab.tsx`, `src/components/features/plan/dialog/pccu.tsx`, `src/components/features/plan/dialog/ppl.tsx`, `src/components/features/plan/dialog/ppm.tsx`, plus 8 more.
- **Improvement / caution:** Add typed request/response generics and document auth/error contracts.

#### `src/store/api/appLink.ts`

- **Why it exists / responsibility:** RTK Query endpoint injection for App Link: appUrl, deleteStreamingApp, saveAppUrl, configList, createConfig, deleteConfig, deleteAppVersion, getConfig, editConfig.
- **Runtime and size:** Shared/build-time eligible module; 124 lines.
- **Exports / surface:** `appLinkApi`, `useAppUrlQuery`, `useDeleteStreamingAppMutation`, `useSaveAppUrlMutation`, `useConfigListQuery`, `useCreateConfigMutation`, `useDeleteConfigMutation`, `useDeleteAppVersionMutation`, `useGetConfigQuery`, `useEditConfigMutation`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** None; state bindings None; contexts None.
- **Dependencies:** internal `src/config/api.ts`, `src/store/api/base-api.ts`, `src/store/tag-types.ts`, `src/config/endpoint-selector.ts`; external None.
- **Rendering and rerenders:** No JSX detected; it runs when imported or when an exported function/hook is called.
- **Direct consumers:** `src/components/features/singup/signup-page.tsx`, `src/components/features/streaming-app/application.tsx`, `src/components/features/streaming-app/config/config-settings-form.tsx`, `src/components/features/streaming-app/demo-apps/demo-application.tsx`, `src/components/features/streaming-app/meeting-link-tab.tsx`, `src/components/features/streaming-app/streaming-link-tab.tsx`, `src/components/features/utilities/remote-editior-streaming.tsx`, `src/components/providers/upload/upload-sequence-provider.tsx`.
- **Improvement / caution:** Add typed request/response generics and document auth/error contracts.

#### `src/store/api/asset.ts`

- **Why it exists / responsibility:** RTK Query endpoint injection for Asset: streamingApp, streamingAppThumbnail, dsApp, asset2d, assetVideo.
- **Runtime and size:** Shared/build-time eligible module; 83 lines.
- **Exports / surface:** `useStreamingAppQuery`, `useStreamingAppThumbnailQuery`, `useDsAppQuery`, `useAsset2dQuery`, `useAssetVideoQuery`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** None; state bindings None; contexts None.
- **Dependencies:** internal `src/config/api.ts`, `src/store/api/base-api.ts`, `src/utils/asset.ts`, `src/store/tag-types.ts`, `src/config/endpoint-selector.ts`; external None.
- **Rendering and rerenders:** No JSX detected; it runs when imported or when an exported function/hook is called.
- **Direct consumers:** `src/components/features/dedicated-server/application.tsx`, `src/components/features/dedicated-server/dedicated-server-page.tsx`, `src/components/features/streaming-app/application.tsx`, `src/components/features/streaming-app/config/image-asset-list.tsx`, `src/components/features/streaming-app/config/video-asset-list.tsx`, `src/components/features/streaming-app/meeting-link-tab.tsx`, `src/components/features/streaming-app/streaming-link-tab.tsx`, `src/components/providers/upload/dedicated-server-upload-provider.tsx`, plus 2 more.
- **Improvement / caution:** Add typed request/response generics and document auth/error contracts.

#### `src/store/api/auth.ts`

- **Why it exists / responsibility:** RTK Query endpoint injection for Auth: loginStatus, signIn, logout, signUp, verifyEmail, verifyUserName, changeUsername, changeEmail, sendOtp, verifyOtp, generateCookie, teamInfo, invitedTeamMember, removeTeamMember, inviteNewMember, updatePhoneNumber, removeUsername, deleteAccount, resetPassword, resetPassConfirm.
- **Runtime and size:** Shared/build-time eligible module; 187 lines.
- **Exports / surface:** `authApi`, `useLoginStatusQuery`, `useSignInMutation`, `useGenerateCookieMutation`, `useTeamInfoQuery`, `useInvitedTeamMemberQuery`, `useRemoveTeamMemberMutation`, `useInviteNewMemberMutation`, `useVerifyEmailMutation`, `useVerifyUserNameMutation`, `useSendOtpMutation`, `useVerifyOtpMutation`, plus 9 more.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** None; state bindings None; contexts None.
- **Dependencies:** internal `src/config/api.ts`, `src/store/tag-types.ts`, `src/store/api/base-api.ts`; external None.
- **Rendering and rerenders:** No JSX detected; it runs when imported or when an exported function/hook is called.
- **Direct consumers:** `src/app/(with-public-layout)/fb-mail/page.tsx`, `src/app/(with-public-layout)/repass/page.tsx`, `src/app/(withSidebarLayout)/layout.tsx`, `src/components/features/developer-section/token-generation-tab.tsx`, `src/components/features/my-account/profile-settings.tsx`, `src/components/features/plan/dialog/pccu.tsx`, `src/components/features/plan/dialog/ppl.tsx`, `src/components/features/plan/dialog/prepaid-minute.tsx`, plus 10 more.
- **Improvement / caution:** Add typed request/response generics and document auth/error contracts.

#### `src/store/api/base-api.ts`

- **Why it exists / responsibility:** RTK Query Base Api module; currently contains no endpoint implementation.
- **Runtime and size:** Shared/build-time eligible module; 14 lines.
- **Exports / surface:** `baseApi`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** None; state bindings None; contexts None.
- **Dependencies:** internal `src/store/tag-types.ts`, `src/helpers/axios/axiosBaseQuery.ts`; external `@reduxjs/toolkit/query/react`.
- **Rendering and rerenders:** No JSX detected; it runs when imported or when an exported function/hook is called.
- **Direct consumers:** `src/store/api/analytics.ts`, `src/store/api/api-key.ts`, `src/store/api/appLink.ts`, `src/store/api/asset.ts`, `src/store/api/auth.ts`, `src/store/api/budget-api.ts`, `src/store/api/credit-card.ts`, `src/store/api/minute.ts`, plus 7 more.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/store/api/budget-api.ts`

- **Why it exists / responsibility:** RTK Query endpoint injection for Budget Api: budgetInfo, budgetEnable, budgetIncrease, budgetDecrease, budgetRemove.
- **Runtime and size:** Shared/build-time eligible module; 51 lines.
- **Exports / surface:** `useBudgetInfoQuery`, `useBudgetEnableMutation`, `useBudgetIncreaseMutation`, `useBudgetDecreaseMutation`, `useBudgetRemoveMutation`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** None; state bindings None; contexts None.
- **Dependencies:** internal `src/config/api.ts`, `src/store/api/base-api.ts`; external None.
- **Rendering and rerenders:** No JSX detected; it runs when imported or when an exported function/hook is called.
- **Direct consumers:** `src/components/features/subscription/subscription-details/ppm.tsx`.
- **Improvement / caution:** Add typed request/response generics and document auth/error contracts.

#### `src/store/api/credit-card.ts`

- **Why it exists / responsibility:** RTK Query endpoint injection for Credit Card: paymentMethod, defaultPaymentMethod, saveCard, setupCard.
- **Runtime and size:** Shared/build-time eligible module; 39 lines.
- **Exports / surface:** `usePaymentMethodQuery`, `useDefaultPaymentMethodQuery`, `useSaveCardMutation`, `useSetupCardMutation`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** None; state bindings None; contexts None.
- **Dependencies:** internal `src/config/api.ts`, `src/store/api/base-api.ts`; external None.
- **Rendering and rerenders:** No JSX detected; it runs when imported or when an exported function/hook is called.
- **Direct consumers:** `src/components/features/plan/dialog/ppm.tsx`, `src/components/features/post-payment/payment-successful-page.tsx`, `src/hooks/use-card-details-v2.ts`, `src/hooks/use-card-details.ts`.
- **Improvement / caution:** Add typed request/response generics and document auth/error contracts.

#### `src/store/api/ds-app.ts`

- **Why it exists / responsibility:** RTK Query Ds App module; currently contains no endpoint implementation.
- **Runtime and size:** Shared/build-time eligible module; 1 lines.
- **Exports / surface:** None.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** None; state bindings None; contexts None.
- **Dependencies:** internal None; external None.
- **Rendering and rerenders:** No JSX detected; it runs when imported or when an exported function/hook is called.
- **Direct consumers:** None; static analysis found no internal importer, so verify framework entry/dynamic use.
- **Improvement / caution:** Remove if obsolete or add a tracked issue and implementation contract.

#### `src/store/api/minute.ts`

- **Why it exists / responsibility:** RTK Query endpoint injection for Minute: minuteStreamed.
- **Runtime and size:** Shared/build-time eligible module; 17 lines.
- **Exports / surface:** `useMinuteStreamedQuery`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** None; state bindings None; contexts None.
- **Dependencies:** internal `src/config/api.ts`, `src/store/api/base-api.ts`; external None.
- **Rendering and rerenders:** No JSX detected; it runs when imported or when an exported function/hook is called.
- **Direct consumers:** `src/hooks/use-minute-v2.ts`, `src/hooks/use-minute.ts`, `src/hooks/use-streamed.ts`, `src/hooks/use-subscription-v2.ts`, `src/hooks/use-trial-v2.ts`.
- **Improvement / caution:** Add typed request/response generics and document auth/error contracts.

#### `src/store/api/payment.ts`

- **Why it exists / responsibility:** RTK Query endpoint injection for Payment: editPayment, editPaymentForCore, paymentMethodConfirmationCore, invoiceList, createSubscriptionPlan, checkStatus, checkStatusGb, checkStatusMin, dataQuote, upgradeSubscription, downgradeSubscription, nonCheckoutCreateMinSubscription, nonCheckoutCreateGbSubscription, unPauseSubscription.
- **Runtime and size:** Shared/build-time eligible module; 152 lines.
- **Exports / surface:** `paymentApi`, `useEditPaymentMutation`, `useEditPaymentForCoreMutation`, `usePaymentMethodConfirmationCoreMutation`, `useInvoiceListQuery`, `useCreateSubscriptionPlanMutation`, `useCheckStatusQuery`, `useDataQuoteQuery`, `useUpgradeSubscriptionMutation`, `useDowngradeSubscriptionMutation`, `useNonCheckoutCreateMinSubscriptionMutation`, `useNonCheckoutCreateGbSubscriptionMutation`, plus 3 more.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** None; state bindings None; contexts None.
- **Dependencies:** internal `src/config/api.ts`, `src/store/api/base-api.ts`, `src/store/tag-types.ts`; external None.
- **Rendering and rerenders:** No JSX detected; it runs when imported or when an exported function/hook is called.
- **Direct consumers:** `src/app/(withSidebarLayout)/credit-card-successful-core/page.tsx`, `src/components/features/my-account/transaction.tsx`, `src/components/features/new-subscription/core-plan-montly-storage.tsx`, `src/components/features/new-subscription/core-plan-montly.tsx`, `src/components/features/new-subscription/core-plan-yearly-storage.tsx`, `src/components/features/new-subscription/core-plan-yearly.tsx`, `src/components/features/new-subscription/new-payment-info.tsx`, `src/components/features/plan/dialog/core-plan.tsx`, plus 12 more.
- **Improvement / caution:** Add typed request/response generics and document auth/error contracts.

#### `src/store/api/storage.ts`

- **Why it exists / responsibility:** RTK Query endpoint injection for Storage: storageStatus, storageUtilized, storageQuote, createStorageSub, upgradeStorage, downgradeStorage.
- **Runtime and size:** Shared/build-time eligible module; 64 lines.
- **Exports / surface:** `useStorageStatusQuery`, `useStorageUtilizedQuery`, `useStorageQuoteQuery`, `useCreateStorageSubMutation`, `useUpgradeStorageMutation`, `useDowngradeStorageMutation`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** None; state bindings None; contexts None.
- **Dependencies:** internal `src/config/api.ts`, `src/store/api/base-api.ts`, `src/store/tag-types.ts`; external None.
- **Rendering and rerenders:** No JSX detected; it runs when imported or when an exported function/hook is called.
- **Direct consumers:** `src/components/features/streaming-app/application.tsx`, `src/components/features/subscription/modal/storage-modify-modal.tsx`, `src/components/providers/upload/upload-sequence-provider.tsx`, `src/hooks/use-storage.ts`, `src/hooks/use-storageV2.ts`, `src/hooks/use-trial-v2.ts`.
- **Improvement / caution:** Add typed request/response generics and document auth/error contracts.

#### `src/store/api/streaming-app.ts`

- **Why it exists / responsibility:** RTK Query endpoint injection for Streaming App: exeInfoUpload, deleteAppExeData.
- **Runtime and size:** Shared/build-time eligible module; 24 lines.
- **Exports / surface:** `streamingAppApi`, `useExeInfoUploadMutation`, `useDeleteAppExeDataMutation`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** None; state bindings None; contexts None.
- **Dependencies:** internal `src/config/api.ts`, `src/store/api/base-api.ts`; external None.
- **Rendering and rerenders:** No JSX detected; it runs when imported or when an exported function/hook is called.
- **Direct consumers:** `src/components/providers/upload/upload-sequence-provider.tsx`.
- **Improvement / caution:** Add typed request/response generics and document auth/error contracts.

#### `src/store/api/subscription.ts`

- **Why it exists / responsibility:** RTK Query endpoint injection for Subscription: ppccuStatus, pplStatus, prepaidMinuteStatus, ppmStatus, createPpmSub, createPpccuSub, createPplSub, createPrepaidMinuteSub, cancelAllSub, cancelPpmSub, reactiveAllSub, ppccuQuote, pplQuote, prepaidMinuteQuote, upgradePpccu, downgradePpccu, upgradePpl, downgradePpl, upgradePrepaidMinute, downgradePrepaidMinute, addWatcher, autoRenewSub.
- **Runtime and size:** Shared/build-time eligible module; 209 lines.
- **Exports / surface:** `usePpccuStatusQuery`, `usePplStatusQuery`, `usePrepaidMinuteStatusQuery`, `usePpmStatusQuery`, `useCreatePpccuSubMutation`, `useCreatePplSubMutation`, `useCreatePrepaidMinuteSubMutation`, `useCancelAllSubMutation`, `useReactiveAllSubMutation`, `useAddWatcherMutation`, `usePpccuQuoteQuery`, `usePplQuoteQuery`, plus 10 more.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** None; state bindings None; contexts None.
- **Dependencies:** internal `src/config/api.ts`, `src/store/api/base-api.ts`, `src/store/tag-types.ts`; external None.
- **Rendering and rerenders:** No JSX detected; it runs when imported or when an exported function/hook is called.
- **Direct consumers:** `src/components/features/new-subscription/core-plan-montly.tsx`, `src/components/features/new-subscription/core-plan-yearly.tsx`, `src/components/features/plan/dialog/pccu.tsx`, `src/components/features/plan/dialog/ppl.tsx`, `src/components/features/plan/dialog/prepaid-minute.tsx`, `src/components/features/post-payment/payment-successful-page.tsx`, `src/components/features/subscription/modal/ppccu-ppl-modify-modal.tsx`, `src/components/features/subscription/modal/prepaid-minute-modify-modal.tsx`, plus 7 more.
- **Improvement / caution:** Add typed request/response generics and document auth/error contracts.

#### `src/store/api/upload.ts`

- **Why it exists / responsibility:** RTK Query endpoint injection for Upload: uploadStreamingAppSignedUrl, altUploadStreamingAppSignedUrl, appThumbnailSignUrl, asset2dUploadSignedUrl, assetVideoUploadSignedUrl, profileLogoUploadSignedUrl, additionalUploadAppSignedUrl, additionalUploadAppList, dedicatedServerAppUploadLink, dedicatedServerInstanceList, dsInstanceStop, dsInstanceStart, getLocationDetails, kickPlayer, dsAppDelete.
- **Runtime and size:** Shared/build-time eligible module; 143 lines.
- **Exports / surface:** `uploadApi`, `useUploadStreamingAppSignedUrlMutation`, `useAltUploadStreamingAppSignedUrlMutation`, `useProfileLogoUploadSignedUrlMutation`, `useAsset2dUploadSignedUrlMutation`, `useAssetVideoUploadSignedUrlMutation`, `useAdditionalUploadAppSignedUrlMutation`, `useAdditionalUploadAppListQuery`, `useDedicatedServerAppUploadLinkMutation`, `useDedicatedServerInstanceListQuery`, `useDsInstanceStopMutation`, `useDsInstanceStartMutation`, plus 4 more.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** None; state bindings None; contexts None.
- **Dependencies:** internal `src/config/api.ts`, `src/store/api/base-api.ts`, `src/utils/asset.ts`, `src/store/tag-types.ts`; external None.
- **Rendering and rerenders:** No JSX detected; it runs when imported or when an exported function/hook is called.
- **Direct consumers:** `src/components/features/additional-upload/additional-upload-page.tsx`, `src/components/features/dedicated-server/dedicated-server-card-details.tsx`, `src/components/features/utilities/multiplayer-app-monitor.tsx`, `src/components/features/utilities/streaming-app-monitor.tsx`, `src/components/providers/upload/additional-upload-provider.tsx`, `src/components/providers/upload/dedicated-server-upload-provider.tsx`, `src/components/providers/upload/upload-sequence-provider.tsx`, `src/hooks/use-common-upload.ts`.
- **Improvement / caution:** Add typed request/response generics and document auth/error contracts.

#### `src/store/api/user-info.ts`

- **Why it exists / responsibility:** RTK Query endpoint injection for User Info: customerId, userAllInfo, profileLogo, notification, removeSingleNotification, clearAllNotification, readAllNotification, uploadLogo, uploadComplete.
- **Runtime and size:** Shared/build-time eligible module; 91 lines.
- **Exports / surface:** `useCustomerIdQuery`, `useUserAllInfoQuery`, `useProfileLogoQuery`, `useNotificationQuery`, `useRemoveSingleNotificationMutation`, `useClearAllNotificationMutation`, `useReadAllNotificationMutation`, `useUploadLogoMutation`, `useUploadCompleteMutation`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** None; state bindings None; contexts None.
- **Dependencies:** internal `src/config/api.ts`, `src/store/api/base-api.ts`, `src/store/tag-types.ts`; external None.
- **Rendering and rerenders:** No JSX detected; it runs when imported or when an exported function/hook is called.
- **Direct consumers:** `src/app/(withSidebarLayout)/layout.tsx`, `src/components/features/developer-section/version-control-tab.tsx`, `src/components/features/my-account/profile-settings.tsx`, `src/components/features/my-account/upload-profile.tsx`, `src/components/features/plan/dialog/pccu.tsx`, `src/components/features/plan/dialog/ppl.tsx`, `src/components/features/plan/dialog/prepaid-minute.tsx`, `src/components/features/post-payment/payment-successful-page.tsx`, plus 12 more.
- **Improvement / caution:** Add typed request/response generics and document auth/error contracts.

### src/store

#### `src/store/hooks.ts`

- **Why it exists / responsibility:** Typed Redux dispatch and selector hook declarations; currently not imported internally.
- **Runtime and size:** Shared/build-time eligible module; 9 lines.
- **Exports / surface:** `useAppDispatch`, `useAppSelector`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** None; state bindings None; contexts None.
- **Dependencies:** internal `src/store/store.ts`; external `react-redux`.
- **Rendering and rerenders:** No JSX detected; it runs when imported or when an exported function/hook is called.
- **Direct consumers:** None; static analysis found no internal importer, so verify framework entry/dynamic use.
- **Improvement / caution:** Confirm whether this is a framework root, planned module, or dead code before retaining it.

### src/store/slices

#### `src/store/slices/user-slice.ts`

- **Why it exists / responsibility:** User Slice client-state Redux slice.
- **Runtime and size:** Shared/build-time eligible module; 24 lines.
- **Exports / surface:** `userSlice`, `setSelectedUserId`, `default`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** None; state bindings None; contexts None.
- **Dependencies:** internal None; external `@reduxjs/toolkit`.
- **Rendering and rerenders:** No JSX detected; it runs when imported or when an exported function/hook is called.
- **Direct consumers:** `src/store/store.ts`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

### src/store

#### `src/store/store.ts`

- **Why it exists / responsibility:** Store repository/tooling configuration.
- **Runtime and size:** Shared/build-time eligible module; 19 lines.
- **Exports / surface:** `store`, `RootState`, `AppDispatch`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** None; state bindings None; contexts None.
- **Dependencies:** internal `src/store/api/base-api.ts`, `src/store/slices/user-slice.ts`; external `@reduxjs/toolkit`.
- **Rendering and rerenders:** No JSX detected; it runs when imported or when an exported function/hook is called.
- **Direct consumers:** `src/components/providers/providers.tsx`, `src/store/hooks.ts`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/store/tag-types.ts`

- **Why it exists / responsibility:** Authoritative RTK Query cache-tag names.
- **Runtime and size:** Shared/build-time eligible module; 23 lines.
- **Exports / surface:** `tagTypes`, `tagTypesList`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** None; state bindings None; contexts None.
- **Dependencies:** internal None; external None.
- **Rendering and rerenders:** No JSX detected; it runs when imported or when an exported function/hook is called.
- **Direct consumers:** `src/components/features/streaming-app/meeting-link-tab.tsx`, `src/components/features/streaming-app/streaming-link-tab.tsx`, `src/store/api/appLink.ts`, `src/store/api/asset.ts`, `src/store/api/auth.ts`, `src/store/api/base-api.ts`, `src/store/api/payment.ts`, `src/store/api/storage.ts`, plus 3 more.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

### src/styles

#### `src/styles/custom-font.ts`

- **Why it exists / responsibility:** Custom Font styling/font helper.
- **Runtime and size:** Shared/build-time eligible module; 14 lines.
- **Exports / surface:** `customText`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** None; state bindings None; contexts None.
- **Dependencies:** internal `src/app/fonts.ts`; external None.
- **Rendering and rerenders:** No JSX detected; it runs when imported or when an exported function/hook is called.
- **Direct consumers:** None; static analysis found no internal importer, so verify framework entry/dynamic use.
- **Improvement / caution:** Confirm whether this is a framework root, planned module, or dead code before retaining it.

### src/types

#### `src/types/common.ts`

- **Why it exists / responsibility:** Common TypeScript data contract declarations.
- **Runtime and size:** Shared/build-time eligible module; 38 lines.
- **Exports / surface:** `TChildrenProps`, `TAppInfo`, `ResponseSuccessType`, `IGenericErrorMessage`, `IGenericErrorResponse`, `TMouseEvent`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** None; state bindings None; contexts None.
- **Dependencies:** internal None; external None.
- **Rendering and rerenders:** No JSX detected; it runs when imported or when an exported function/hook is called.
- **Direct consumers:** `src/components/features/streaming-app/meeting-link-tab.tsx`, `src/components/features/streaming-app/streaming-link-tab.tsx`, `src/components/providers/providers.tsx`, `src/components/shared/tooltip-info.tsx`.
- **Improvement / caution:** Keep aligned with backend schemas; prefer generated/validated contracts.

#### `src/types/r2-upload.ts`

- **Why it exists / responsibility:** R2 Upload TypeScript data contract declarations.
- **Runtime and size:** Shared/build-time eligible module; 45 lines.
- **Exports / surface:** `R2InitiateData`, `R2PartUrl`, `R2UploadedPart`, `R2UploadResult`, `R2ProgressEvent`, `R2SpeedUpdate`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** None; state bindings None; contexts None.
- **Dependencies:** internal None; external None.
- **Rendering and rerenders:** No JSX detected; it runs when imported or when an exported function/hook is called.
- **Direct consumers:** `src/services/r2/optimized-r2-upload.ts`, `src/services/r2/r2-upload-api.ts`.
- **Improvement / caution:** Keep aligned with backend schemas; prefer generated/validated contracts.

### src/utils

#### `src/utils/asset.ts`

- **Why it exists / responsibility:** Asset pure or mostly-pure reusable utility.
- **Runtime and size:** Shared/build-time eligible module; 107 lines.
- **Exports / surface:** `images`, `normalizeAsset`, `normalizeAsset(blobs: TBlob)`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** None; state bindings None; contexts None.
- **Dependencies:** internal `src/assets/index.ts`; external None.
- **Rendering and rerenders:** No JSX detected; it runs when imported or when an exported function/hook is called.
- **Direct consumers:** `src/components/features/streaming-app/application.tsx`, `src/components/features/streaming-app/streaming-app-card.tsx`, `src/store/api/asset.ts`, `src/store/api/upload.ts`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/utils/common.ts`

- **Why it exists / responsibility:** Common pure or mostly-pure reusable utility.
- **Runtime and size:** Shared/build-time eligible module; 70 lines.
- **Exports / surface:** `copyToClipboard`, `isActiveSubscription`, `downloadCSV`, `hasWhiteSpace`, `doContainRestrictedCharacters`, `copyToClipboard(text: string)`, `isActiveSubscription(planStatus: string)`, `downloadCSV(data: any[], fileName = 'data.csv')`, `hasWhiteSpace(str: string)`, `doContainRestrictedCharacters(text: string)`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** None; state bindings None; contexts None.
- **Dependencies:** internal `src/constant/plan.ts`; external None.
- **Rendering and rerenders:** No JSX detected; it runs when imported or when an exported function/hook is called.
- **Direct consumers:** `src/components/features/dedicated-server/dedicated-server-app-upload.tsx`, `src/components/features/dedicated-server/dedicated-server-card-details.tsx`, `src/components/features/developer-section/api-key-tab.tsx`, `src/components/features/developer-section/streaming-api-key-tab.tsx`, `src/components/features/developer-section/token-generation-tab.tsx`, `src/components/features/streaming-app/config/image-upload.tsx`, `src/components/features/streaming-app/demo-apps/demo-application.tsx`, `src/components/features/streaming-app/meeting-link-tab.tsx`, plus 6 more.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/utils/date-formatter.ts`

- **Why it exists / responsibility:** Date Formatter pure or mostly-pure reusable utility.
- **Runtime and size:** Shared/build-time eligible module; 26 lines.
- **Exports / surface:** `formatDateTime`, `formatDateUtil`, `formatDateTime(value: number)`, `formatDateUtil(startUnix: number, endUnix: number)`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** None; state bindings None; contexts None.
- **Dependencies:** internal None; external `date-fns`.
- **Rendering and rerenders:** No JSX detected; it runs when imported or when an exported function/hook is called.
- **Direct consumers:** `src/components/features/my-account/transaction.tsx`, `src/components/features/new-subscription/core-plan-montly.tsx`, `src/components/features/new-subscription/core-plan-yearly.tsx`, `src/components/features/new-subscription/new-billing.tsx`, `src/components/features/subscription/bill-summary-ppm.tsx`, `src/components/features/subscription/bill-summary.tsx`, `src/components/features/subscription/modal/ppccu-ppl-modify-modal.tsx`, `src/components/features/subscription/modal/prepaid-minute-modify-modal.tsx`, plus 7 more.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/utils/notifications.ts`

- **Why it exists / responsibility:** Notifications pure or mostly-pure reusable utility.
- **Runtime and size:** Shared/build-time eligible module; 52 lines.
- **Exports / surface:** `sendNewSignupNotify`, `sendCancelSubscriptionNotify`, `sendNewSignupNotify(username: string, userEmail: string, phoneNumber?: string, leadSource?: string)`, `sendCancelSubscriptionNotify(username: string, userEmail: string, plan: string, reason: string)`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** None; state bindings None; contexts None.
- **Dependencies:** internal `src/helpers/axios/axiosInstance.ts`, `src/config/api.ts`; external None.
- **Rendering and rerenders:** No JSX detected; it runs when imported or when an exported function/hook is called.
- **Direct consumers:** `src/components/features/new-subscription/core-plan-montly.tsx`, `src/components/features/new-subscription/core-plan-yearly.tsx`, `src/components/features/subscription/subscription-details/ppccu.tsx`, `src/components/features/subscription/subscription-details/ppl.tsx`, `src/components/features/subscription/subscription-details/ppm.tsx`, `src/components/features/subscription/subscription-details/prepaid-minute.tsx`, `src/components/features/username/username-page.tsx`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/utils/options.ts`

- **Why it exists / responsibility:** Options pure or mostly-pure reusable utility.
- **Runtime and size:** Shared/build-time eligible module; 14 lines.
- **Exports / surface:** `storageOptions`, `ppccuOptions`, `pplOptions`, `generateOptions(count: number, unit: string, multiplier: number = 1)`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** None; state bindings None; contexts None.
- **Dependencies:** internal None; external None.
- **Rendering and rerenders:** No JSX detected; it runs when imported or when an exported function/hook is called.
- **Direct consumers:** `src/components/features/subscription/modal/ppccu-ppl-modify-modal.tsx`, `src/components/features/subscription/modal/storage-modify-modal.tsx`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/utils/size-formatter.ts`

- **Why it exists / responsibility:** Size Formatter pure or mostly-pure reusable utility.
- **Runtime and size:** Shared/build-time eligible module; 14 lines.
- **Exports / surface:** `formatFileSize`, `toGB`, `formatFileSize(bytes: number)`, `toGB(bytes: number)`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** None; state bindings None; contexts None.
- **Dependencies:** internal None; external None.
- **Rendering and rerenders:** No JSX detected; it runs when imported or when an exported function/hook is called.
- **Direct consumers:** `src/app/(withSidebarLayout)/upload/upload-details.tsx`, `src/components/features/additional-upload/additional-upload-details.tsx`, `src/components/features/dedicated-server/dedicated-server-upload-details.tsx`, `src/components/providers/upload/additional-upload-provider.tsx`, `src/components/providers/upload/upload-sequence-provider.tsx`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/utils/super-admin-login.ts`

- **Why it exists / responsibility:** Super Admin Login pure or mostly-pure reusable utility.
- **Runtime and size:** Shared/build-time eligible module; 190 lines.
- **Exports / surface:** `extractUserInfoFromUrl`, `hasSuperAdminUrlParam`, `validateUserInfo`, `createSuperAdminSession`, `clearSuperAdminSession`, `isSuperAdminSession`, `generateSuperAdminUrl`, `extractUserInfoFromUrl`, `hasSuperAdminUrlParam`, `validateUserInfo(userInfo: UserInfo | null)`, `createSuperAdminSession(userInfo: UserInfo)`, `clearSuperAdminSession`, plus 2 more.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** None; state bindings None; contexts None.
- **Dependencies:** internal None; external None.
- **Rendering and rerenders:** No JSX detected; it runs when imported or when an exported function/hook is called.
- **Direct consumers:** `src/components/admin/super-admin-url-generator.tsx`, `src/components/auth/super-admin-handler.tsx`, `src/components/features/signin/signin-page.tsx`, `src/hooks/use-user-info.ts`.
- **Improvement / caution:** Replace URL/localStorage credentials with a short-lived audited backend exchange.

#### `src/utils/upload.ts`

- **Why it exists / responsibility:** Upload pure or mostly-pure reusable utility.
- **Runtime and size:** Shared/build-time eligible module; 158 lines.
- **Exports / surface:** `zipChecker`, `getEstimatedTime`, `isExtractable(file: File)`, `checkEngineAndSh(directoryObj: ZipEntries)`, `isExeExist(entries: ZipEntries)`, `zipChecker(file: File)`, `getEstimatedTime(seconds: number)`.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** None; state bindings None; contexts None.
- **Dependencies:** internal None; external `sonner`, `unzipit`.
- **Rendering and rerenders:** No JSX detected; it runs when imported or when an exported function/hook is called.
- **Direct consumers:** `src/app/(withSidebarLayout)/upload/upload-details.tsx`, `src/components/features/dedicated-server/dedicated-server-upload-details.tsx`, `src/components/providers/upload/upload-sequence-provider.tsx`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `src/utils/validation.ts`

- **Why it exists / responsibility:** Validation pure or mostly-pure reusable utility.
- **Runtime and size:** Shared/build-time eligible module; 48 lines.
- **Exports / surface:** `isValidEmail`, `isValidPassword`, `isValidUsername`, `isValidInternationalMobileNumber`, `isJSONValid`, `isURL`, `isBase64`, `isValidEmail(email: string)`, `isValidPassword(password: string)`, `isValidUsername(username: string)`, `isValidInternationalMobileNumber(mobileNumber: string)`, `isJSONValid(input: any)`, plus 2 more.
- **Props / inputs visible at declarations:** None.
- **Hooks / local state:** None; state bindings None; contexts None.
- **Dependencies:** internal None; external None.
- **Rendering and rerenders:** No JSX detected; it runs when imported or when an exported function/hook is called.
- **Direct consumers:** `src/app/(with-public-layout)/fb-mail/page.tsx`, `src/app/(with-public-layout)/repass/page.tsx`, `src/components/features/my-account/profile-settings.tsx`, `src/components/features/signin/signin-page.tsx`, `src/components/features/singup/signup-page.tsx`, `src/components/features/team/my-team-tab.tsx`, `src/components/features/username/username-page.tsx`.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

### Repository root

#### `tsconfig.json`

- **Why it exists / responsibility:** Tsconfig repository/tooling configuration.
- **Runtime and size:** Build/tooling or documentation; 756 bytes.
- **Exports / hooks / state:** Not applicable to this non-source file.
- **Dependencies / consumers:** Referenced by tooling, URL strings, CSS, metadata, CI, or documentation; static TypeScript imports do not provide a complete signal.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

#### `verify-build-config.ps1`

- **Why it exists / responsibility:** Verify Build Config repository/tooling configuration.
- **Runtime and size:** Build/tooling or documentation; 4124 bytes.
- **Exports / hooks / state:** Not applicable to this non-source file.
- **Dependencies / consumers:** Referenced by tooling, URL strings, CSS, metadata, CI, or documentation; static TypeScript imports do not provide a complete signal.
- **Improvement / caution:** Retain single responsibility; add coverage when behavior changes.

### New onboarding files created by this audit

- `docs/onboarding/README.md` indexes the set and records scope/evidence rules.
- `01-product-and-runtime.md` explains Sections 1 and 4–7.
- `02-frontend-architecture.md` explains Sections 2, 8–10, and 13–18.
- `03-data-auth-and-configuration.md` explains Sections 11–12 and 19–21.
- This file provides Sections 3 and 22.
- `05-diagrams.md`, `06-learning-plan-and-glossary.md`, `07-code-review.md`, and `08-interview-bank.md` complete Sections 23–28.

### Section 22 closeout

**Key takeaways**

- The tracked baseline contains 363 files; every baseline path appears in this section.
- Static analysis is excellent for exports/imports/hooks, but runtime behavior must still be verified at call sites.
- Very large client files and untyped endpoint modules deserve the earliest decomposition effort.
- Assets and “unused” files require string/dynamic-reference checks before deletion.

**Common pitfalls**

- Assuming no static importer means dead code.
- Assuming a missing `use client` directive means server execution when the module is imported below a client boundary.
- Editing shadcn source without checking all consumers.
- Refactoring an API response shape without inspecting hooks and components together.

**Questions to ask a mentor**

- Which files are generated or vendor-derived?
- Which placeholders correspond to committed roadmap work?
- Where are backend payload contracts maintained?

**Related files to read next**

- Use each entry’s direct consumers and dependencies as the next-reading chain.
- Start with the provider stack, API base, `useUserInfo`, and upload sequence.

**Practical exercises**

- Pick one route and annotate every catalogue entry in its import chain.
- Confirm or disprove three zero-incoming candidates.
- Add a typed endpoint and update its catalogue entry.

