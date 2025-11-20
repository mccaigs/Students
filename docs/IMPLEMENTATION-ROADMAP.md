# StudentsAI – Implementation Roadmap

A chronological roadmap describing how StudentsAI should be built from the current state to production.

## Phase 1 — Foundation
- Audit current codebase vs PRD and Architecture
- Finalise monorepo structure and workspace references
- Standardise TypeScript, ESLint, Prettier and Turborepo setup
- Add documentation and package-level READMEs

## Phase 2 — Authentication
- Implement Auth.js/NextAuth with email/password, Google and Microsoft
- Build login and registration flows
- Add password reset
- Add account settings page with locale preference

## Phase 3 — Billing & Subscriptions
- Add Free + paid tiers with correct metadata
- Build Stripe product/price configuration
- Implement Checkout sessions and Customer Portal
- Implement Stripe webhook at /api/webhooks/stripe
- Sync subscription state into Prisma
- Implement full plan enforcement layer

## Phase 4 — AI System (Backend)
- Replace placeholder GPT-4 code with GPT-5.1
- Build unified ai-client with structured types
- Add robust error handling and retry support
- Add token and request usage tracking

## Phase 5 — AI Tools (Frontend + Backend Integration)
- Build pages for:
    - /app/tools/writing
    - /app/tools/research
    - /app/tools/paraphrasing
    - /app/tools/literature
- Connect each page to server actions
- Integrate saving results into workspace

## Phase 6 — Workspace System
- Add Prisma tables for projects, documents and versions
- Build workspace dashboard UI
- Implement document editor
- Add version history and autosave
- Add PDF/DOCX/Markdown export

## Phase 7 — Localisation
- Integrate Next.js i18n middleware
- Add locale detection
- Add user locale override setting
- Replace all strings with localisation keys

## Phase 8 — UI/UX Library
- Build design system components in packages/ui
- Replace duplicated markup across the app
- Enforce consistent layout, spacing and typography
- Improve accessibility and responsiveness site-wide

## Phase 9 — Testing & QA
- Add unit tests for core, billing and AI helpers
- Add integration tests for server actions
- Add end-to-end tests for major flows
- Conduct full manual QA
- Fix regressions and refine stability

## Phase 10 — Deployment
- Configure environment variables for production
- Deploy to Vercel with production database
- Verify Stripe connectivity in production environment
- Perform final walk-through of every route and tool

## Phase 11 — Launch
- Complete final accessibility, performance and regression passes
- Approve launch checklist
- Release StudentsAI v1.0
