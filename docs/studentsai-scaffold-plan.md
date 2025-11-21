# StudentsAI Scaffold Plan

This plan translates the StudentsAI documentation into an actionable scaffold for building the platform from scratch.

## 1. Directory Structure

Root monorepo layout follows the documented architecture, with focus areas requested for `app` and supporting folders.

```
apps/
  web/
    app/
      (marketing)/
        layout.tsx
        page.tsx
      auth/
        login/page.tsx
        register/page.tsx
        reset-password/page.tsx
      api/
        auth/
        billing/
        documents/
        storage/
        search/
        admin/
      app/
        layout.tsx
        page.tsx
        dashboard/page.tsx
        documents/
          [documentId]/page.tsx
        tools/
          writing/page.tsx
          research/page.tsx
          paraphrase/page.tsx
          literature/page.tsx
        settings/page.tsx
        billing/page.tsx
        admin/
          layout.tsx
          users/page.tsx
          plans/page.tsx
          logs/page.tsx
    components/
      ui/
      navigation/
      dashboard/
      editor/
      assistant/
      billing/
      forms/
    hooks/
    providers/
    lib/
      auth/
      billing/
      ai/
      storage/
      search/
      rls/
    utils/
    styles/
    types/
    features/
      auth/
      billing/
      dashboard/
      documents/
      ai/
      admin/
    models/
      prisma/
      view-models/
```

Packages mirror the architecture:

```
packages/
  ui/
  core/
  ai-client/
  billing/
  localisation/
```

Shared database definitions live in `packages/core` with Prisma schema under `/db` and generated client bindings consumed by `apps/web`.

## 2. Database Schema (Prisma)

Key tables reflecting multi-tenant SaaS with usage tracking:

- `User`: id, email, hashedPassword, name, role (student/admin), locale, createdAt, updatedAt.
- `Session`: id, userId, sessionToken, expires, createdAt.
- `SubscriptionPlan`: id, name (Free, Standard, Pro, VIP, Ultimate), priceGBP, features, requestLimit, tokenLimit, createdAt.
- `UserSubscription`: id, userId, planId, stripeCustomerId, stripeSubscriptionId, status, currentPeriodEnd, cancelAtPeriodEnd, createdAt, updatedAt.
- `Project` (or `Document`): id, userId, title, content, status, version, lastUsedAt, createdAt, updatedAt.
- `UserUploadedFile`: id, userId, storageProvider (google_drive/icloud/dropbox), externalId, filename, mimeType, size, createdAt.
- `UserAIProfile`: id, userId, tone, stylePreferences (JSON), summary, embeddingRef, createdAt, updatedAt.
- `AIInteraction`: id, userId, tool (writing/research/paraphrase/literature), projectId, prompt, responseRef, tokenCount, costEstimate, createdAt.
- `AdminLog`: id, adminId, action, targetType, targetId, metadata (JSON), createdAt.
- `ApiUsageLog`: id, userId, route, method, statusCode, latencyMs, tokenCount, createdAt.
- `UsageRecord`: id, userId, tool, tokensUsed, requestsUsed, period (month), createdAt, updatedAt.

## 3. API Routes (Next.js App Router)

- Auth: `/api/auth/[...nextauth]` for email/password + Google + Microsoft; password reset and verification helpers under `/api/auth/*`.
- Billing: `/api/billing/checkout`, `/api/billing/portal`, Stripe webhooks at `/api/webhooks/stripe`.
- AI interactions: `/api/ai/{writing|research|paraphrase|literature}` using `packages/ai-client` and enforcing plan/usage.
- Documents: `/api/documents` (CRUD), `/api/documents/[id]/history`, `/api/documents/[id]/export`.
- User uploads: `/api/uploads` for storage connector handshakes and callbacks.
- AI profile: `/api/profile/tone` for uploading samples and updating personal agent settings.
- Admin: `/api/admin/users`, `/api/admin/plans`, `/api/admin/logs` with admin-only guards.
- Search integration: `/api/search/query` proxying approved search providers.

## 4. Component List (React + Shadcn)

- Shell: App layout, marketing layout, locale switcher, theme toggle.
- Navigation: Sidebar, topbar, breadcrumb, command palette.
- Auth: Login form, registration form, password reset form, OAuth buttons.
- Dashboard: Usage summary cards, plan status, recent documents, AI activity feed.
- Editor: Rich text/MDX editor (Google Docs style), document toolbar, version history panel, export modal.
- AI Assistant: Prompt composer, response stream, citation viewer, tone selector, saved prompts list.
- Billing: Plan selector, pricing table, checkout button, portal link, invoice list.
- Settings: Profile form, locale selector, notification toggles, storage connector manager.
- Documents: Document list/table, filters, document row actions, empty states, upload modal.
- Admin: User table, subscription overview, plan configurator, audit log viewer.
- Feedback/Errors: Toasts, inline validation messages, friendly limit reached banner.

## 5. Initial Roadmap

- **Phase 1: Core auth + billing** – NextAuth setup, Prisma models for users/sessions/plans, Stripe checkout + webhooks, plan state stored in DB.
- **Phase 2: Dashboard + document system** – App shell, document CRUD, versioning, usage dashboard.
- **Phase 3: AI integration** – `packages/ai-client` wrappers, server actions and usage tracking for writing/research/paraphrase/literature tools.
- **Phase 4: Editor + writing-style learning** – Collaborative-style editor, upload-based tone training, per-user AI profile storage.
- **Phase 5: Admin panel** – Admin-only routes, plan management, audit logs, user oversight with RLS-friendly queries.
- **Phase 6: Web search** – Search proxy route with safety filters, UI integration inside assistant.
- **Phase 7: Public website + blog** – Landing pages, marketing site (apps/landing), MDX knowledge base.

## 6. Clarifications Needed

To proceed without assumptions, please confirm:

1. Preferred search provider(s) and API contract for the in-app search proxy.
2. Exact feature and usage limits per plan (token/request caps per tool) to encode in `packages/core` and Stripe metadata.
3. Required storage scopes and callback URLs for Google Drive, iCloud and Dropbox integrations.
4. Expected format for AI-generated citations and exports (e.g., APA/MLA defaults, PDF/DOCX templates).
5. Any accessibility targets (WCAG level) and analytics tooling that must be supported from day one.
