# 📁 Next.js Project Structure

> A comprehensive folder structure for Next.js applications with TypeScript and shadcn/ui

## 🎯 Overview

This structure is designed to scale from medium to large applications while maintaining code organization, developer productivity, and team collaboration. It follows modern Next.js patterns with the App Router and integrates seamlessly with shadcn/ui components.

---

## 📋 Complete Folder Structure

```
project-root
├── 📁 public/                    # 🖼️ Static Assets
│   ├── icons/                   # App icons and favicons
│   ├── images/                  # Images and graphics
│   └── logos/                   # Brand logos
│
├── 📁 src/                       # 💻 Source Code
│   ├── 📁 app/                   # 🛣️ App Router
│   │   ├── (auth)/              # 🔒 Authentication routes
│   │   │   ├── login/
│   │   │   │   └── page.tsx
│   │   │   └── register/
│   │   │       └── page.tsx
│   │   │
│   │   ├── (dashboard)/         # 📊 Dashboard routes
│   │   │   ├── dashboard/
│   │   │   │   ├── page.tsx
│   │   │   │   └── loading.tsx
│   │   │   ├── settings/
│   │   │   │   └── page.tsx
│   │   │   └── layout.tsx
│   │   │
│   │   ├── globals.css          # 🎨 Global styles
│   │   ├── layout.tsx           # 🏠 Root layout
│   │   ├── page.tsx             # 🏡 Home page
│   │   ├── loading.tsx          # ⏳ Global loading UI
│   │   ├── error.tsx            # ❌ Global error UI
│   │   └── not-found.tsx        # 🔍 404 page
│   │
│   ├── 📁 components/           # 🧩 Reusable Components
│   │   ├── 📁 ui/               # 📝 Only UI related Components (No logic: stateless)
│   │   │    ├── 📁 forms/            # 📝 Form Components
│   │   │    │   ├── login-form.tsx
│   │   │    │   └── register-form.tsx
│   │   │    │
│   │   │    ├── 📁 charts/           # 📝 Chart Components
│   │   │    │   ├── bar-chart.tsx
│   │   │    │   └── pie-chart.tsx
│   │   │    │
│   │   │    ├── 📁 buttons/           # 📝 Chart Components
│   │   │    │   ├── primary-button.tsx
│   │   │    │   └── small-button.tsx
│   │   │    │
│   │   │    ├── button.tsx
│   │   │    ├── input.tsx     # shadcn/ui Components
│   │   │    ├── dialog.tsx
│   │   │    └── card.tsx
│   │   │
│   │   │
│   │   ├── 📁 layout/           # 🏗️ Layout Components
│   │   │   ├── header.tsx       # Site header
│   │   │   ├── sidebar.tsx      # Navigation sidebar
│   │   │   ├── footer.tsx       # Site footer
│   │   │   └── navigation.tsx   # Navigation menus
│   │   │
│   │   │
│   │   ├── 📁 features/         # 🎯 Feature-Specific Components (Page specific)
│   │   │   ├── auth/            # Authentication features
│   │   │   │   ├── register-page.tsx
│   │   │   │   └── login-page.tsx
│   │   │   │
│   │   │   ├── dashboard/       # Dashboard features
│   │   │   │   ├── dashboard-stats.tsx
│   │   │   │   └── recent-activity.tsx
│   │   │   │
│   │   │   └── user/            # User management features
│   │   │       ├── user-avatar.tsx
│   │   │       └── user-menu.tsx
│   │   │
│   │   │
│   │   ├── 📁 shared/           # 🔧 Common/Shared Components
│   │   │   ├── loading-spinner.tsx
│   │   │   ├── error-boundary.tsx
│   │   │   ├── confirmation-dialog.tsx
│   │   │   └── data-table.tsx
│   │   │
│   │   │
│   │   └── 📁 providers/        # 🌐 Context Providers
│   │       ├── theme-provider.tsx
│   │       ├── auth-provider.tsx
│   │       └── providers.tsx    # Contains all the providers
│   │
│   │
│   ├── 📁 config/               # ⚙️ Configuration Files
│   │   ├── database.ts          # Database connection settings
│   │   ├── api.ts               # Authentication provider settings
│   │   ├── pages.ts             # Pages route
│   │   ├── third-party.ts       # External service configs
│   │   ├── staging-api.ts       # Staging APIs
│   │   └── prod-api.ts          # Production APIs
│   │
│   │
│   ├── 📁 constants/            # 📊 Application Constants
│   │   ├── app.ts               # App-wide constants
│   │   ├── routes.ts            # Route constants
│   │   ├── api.ts               # API-related constants
│   │   ├── ui.ts                # UI constants (colors, sizes)
│   │   ├── messages.ts          # User-facing messages
│   │   ├── regex.ts             # Regular expressions
│   │   └── status-codes.ts      # HTTP status codes
│   │
│   │
│   ├── 📁 lib/                  # 🛠️ Utility Functions
│   │   ├── utils.ts             # cn() function and utilities
│   │   ├── 📁 validations/      # 🔍 Zod Schemas
│   │   │   ├── auth.ts
│   │   │   └── user.ts
│   │   └── 📁 cookies/          # 🗄️ Cookies Utilities
│   │       ├── set-cookies.ts
│   │       └── get-cookies.ts
│   │
│   ├── 📁 hooks/                # Custom React Hooks
│   │   ├── use-auth.ts           # Authentication hook
│   │   ├── use-local-storage.ts  # Local storage hook
│   │   ├── use-debounce.ts       # Debounce hook
│   │   └── use-api.ts            # API interaction hook
│   │
│   │
│   ├── 📁 store/                # 🏪 State Management
│   │   ├── 📁 slices/           # Redux slices
│   │   │   ├── auth-slice.ts
│   │   │   └── user-slice.ts
│   │   │
│   │   └── 📁 api/              # All api goes here
│   │       ├── base-api.ts
│   │       └── auth-api.ts
│   │
│   │
│   ├── 📁 types/                # 📝 TypeScript Definitions
│   │   ├── auth.ts              # Authentication types
│   │   ├── user.ts              # User-related types
│   │   ├── api.ts               # API response types
│   │   └── database.ts          # Database types
│   │
│   │
│   └── 📁 styles/               # 🎨 Additional Styles
│   │   ├── components.css       # Component-specific styles
│   │   └── utilities.css        # Utility classes
│   │
│   │
│   └── 📁 assets/
│       ├── 📁 images/
│       └── 📁 logos/
│
└── 📁 docs/                     # 📚 Documentation
    ├── folder-structure.md      # Folder structure documentation
    └── sop.md                   # Standard Operating Procedure (SOP) for Software Developers
```
