# StudentsAI – Architecture Document

## 1. Architectural Overview
StudentsAI is built as a cloud‑only, multi‑tenant SaaS platform. It follows a modular monorepo architecture to support scalability, maintainability and separation of concerns. All compute‑intensive operations, including AI interactions, occur via OpenAI’s cloud models. The platform is deployed on Vercel.

## 2. Monorepo Structure
The application is organised as follows:

```
apps/
  web/                  # Next.js application (App Router)
  landing/              # Marketing site (optional)

packages/
  ui/                   # Shared UI components
  core/                 # Domain logic and shared utilities
  ai-client/            # Centralised OpenAI clients and safety rules
  billing/              # Stripe integration layer
  localisation/         # i18n configuration and locale files

docs/                   # Project-wide documentation
```

### 2.1 apps/web
The main user-facing application. Responsibilities include:
- Routing and navigation
- Pages, layouts and server components
- Server actions for business operations
- Rendering dashboards, tools and workspace views

### 2.2 packages/ui
A shared component library containing:
- Buttons, inputs, dialogs and layout primitives
- Theming and typography
- Form components using a consistent design system

### 2.3 packages/core
Contains shared domain logic:
- Plan definitions and usage limits
- Validation schemas (Zod)
- Utility functions used across apps
- Database access abstractions

### 2.4 packages/ai-client
Centralised OpenAI integration:
- A type-safe wrapper for OpenAI calls
- Error and rate‑limit handling
- Helper functions for academic writing, paraphrasing, research and literature analysis
- Safety and content constraints

### 2.5 packages/billing
Handles all interaction with Stripe:
- Subscription creation and management
- Webhooks for billing events
- Integration helpers for plan enforcement

### 2.6 packages/localisation
Internationalisation layer:
- Locale files (en-GB, en-US, en-AU, en-CA, fallback en)
- Auto‑detection logic
- Translation utilities

## 3. Frontend Architecture
The frontend uses Next.js App Router. Key principles:
- Prefer Server Components where practical
- Use Client Components only for interactive elements
- Server Actions for form submissions and business logic
- Tailwind CSS for styling

### 3.1 Routing
Core routes:
- `/` – Landing
- `/auth/*` – Sign in, sign up, password reset
- `/app` – Authenticated app shell
- `/app/dashboard` – Usage, documents and plan status
- `/app/tools/*` – AI‑powered study tools

### 3.2 State Management
- Minimal client‑side state
- Server Actions and URL‑based state where possible
- React Query only if necessary for interactive features

## 4. Backend Architecture
Backend logic is implemented using Next.js server actions and route handlers.

### 4.1 Database
- PostgreSQL accessed via Prisma
- Core tables: users, sessions, plans, subscriptions, usage_records, documents, settings

### 4.2 API Layer
All backend behaviour occurs through:
- Server Actions
- Route handlers under `app/api` for webhook endpoints

### 4.3 Usage Tracking
- Each AI request logs tokens or request counts
- Enforcement applied before an action is executed

## 5. Integration Layer

### 5.1 OpenAI
All AI calls pass through `packages/ai-client`.
- GPT‑5.1 base models
- Fine‑tuned models for academic writing, research, paraphrasing and literature review
- Uniform request schema to maintain control

### 5.2 Stripe
Stripe responsibilities include:
- Subscription lifecycle
- Customer Portal integration
- Webhook handling via `/api/webhooks/stripe`

## 6. Localisation Architecture
- Uses Next.js i18n framework
- Middleware determines locale from headers or IP
- Users may override locale in settings

## 7. Security Model
- Secure session handling (httpOnly cookies)
- All secrets stored in `.env`
- Strict Content Security Policy
- Granular plan‑based access checks
- Webhook signature verification

## 8. Deployment
- Deployed on Vercel
- PostgreSQL hosted on a managed service (e.g. Neon or Supabase)
- Environment variables configured via Vercel dashboard
- CI: Linting, type checking and tests run on every push

## 9. Non‑Functional Requirements
- High availability and scalable architecture
- Maintainable code organisation
- Clear boundaries between concerns
- Automated testing for stability and regression prevention

## 10. Future Extensions
The architecture supports future enhancements including:
- Additional English variants
- Productivity utilities (note boards, planners)
- Optional native mobile wrapper
- AI model fine‑tuning updates and new specialisations

