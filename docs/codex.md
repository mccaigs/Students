# StudentsAI – Codex

Author: System  
Audience: AI coding assistant working in Windsurf on the StudentsAI repo.  
Purpose: Define non-negotiable rules, constraints and working practices for implementing StudentsAI.

---

## 1. Mission

You are developing **StudentsAI**, a cloud-only SaaS platform for individual college and university students.

Your job is to:

- Implement and maintain the codebase so that it **matches**:
  - `docs/PRD.md`
  - `docs/ARCHITECTURE.md`
- Keep the implementation **consistent, secure, maintainable and testable**.
- Prioritise **correctness and clarity** over cleverness.

You must treat `docs/PRD.md` and `docs/ARCHITECTURE.md` as the primary source of product truth.

If there is ever a conflict between this file and those documents, prefer:

1. `docs/PRD.md` (product behaviour),
2. `docs/ARCHITECTURE.md` (structure and architecture),
3. then this `codex.md` (working practices).

---

## 2. Core Principles

You must obey these project rules at all times:

1. **Individual students only**  
   - StudentsAI is for **individual FE/HE students**.  
   - Do not add features, flows, tables or copy for:
     - “Enterprise”
     - “Campus licences”
     - “Institutional dashboards”
     - “Admin portals for universities”

2. **Cloud-only, OpenAI-only runtime**  
   - All AI inference for end users must go through **OpenAI** models only.
   - Do not integrate runtime calls to:
     - Gemini
     - Claude
     - Local models
     - Any other third-party model provider
   - All AI calls must go through the central AI client in `packages/ai-client`.

3. **Pricing and plans**  
   Implement and preserve these tiers exactly:

   - Free – limited usage
   - Standard – £12/month
   - Pro – £19/month
   - VIP – £27/month
   - Ultimate – £35/month

   Rules:

   - Plans are applied **per user**. No shared or organisation-level accounts.
   - Each plan has usage limits and feature access as defined in `docs/PRD.md`.
   - Do not introduce additional plans or hidden “trial” variants without explicit mention in the PRD.

4. **Stack (non-negotiable)**  
   - Framework: Next.js (App Router) + React + TypeScript
   - Styling: Tailwind CSS
   - Database: PostgreSQL with Prisma
   - Auth: NextAuth/Auth.js (email + Google + Microsoft)
   - Billing: Stripe subscriptions
   - Deployment: Vercel

5. **Monorepo structure**  
   You must respect the architecture described in `docs/ARCHITECTURE.md`. The key structure:

   - `apps/web` – main Next.js app
   - `apps/landing` – marketing site (optional, but if present must follow architecture)
   - `packages/ui` – design system and shared UI components
   - `packages/core` – domain logic, plan definitions, validation, shared utilities
   - `packages/ai-client` – OpenAI integration
   - `packages/billing` – Stripe integration
   - `packages/localisation` – i18n config and locale files
   - `docs/` – documentation, including PRD and architecture

---

## 3. Behavioural Guidelines for the Assistant

When acting as the coding assistant in Windsurf, you must:

1. **Read before writing**  
   - Before significant work, skim:
     - `docs/PRD.md`
     - `docs/ARCHITECTURE.md`
     - Any nearby `README.md` files
   - Before editing a file, read enough of it to understand context and style.

2. **Plan first**  
   - For any substantial feature, first propose:
     - A short description of the change
     - Files to be touched/created
     - Any schema changes
   - Keep changes grouped and coherent.

3. **Explain changes**  
   After editing, always summarise:
   - What you changed
   - Why you changed it
   - Which files are affected
   - Any follow-up work required

4. **Prefer incremental improvements**  
   - Avoid huge, sweeping refactors unless explicitly requested.
   - If a refactor is necessary, outline it first and apply it in small pieces.

5. **Keep interfaces stable where possible**  
   - When changing exported functions/components, consider call sites.
   - If you must break an API, update all usage sites in the same change.

---

## 4. Technical Rules

### 4.1 AI Integration (`packages/ai-client`)

- All AI calls must go through `packages/ai-client`.
- Use:
  - GPT-5.1 base model (or project-specific OpenAI model as specified in the PRD).
  - Named entry points for:
    - Academic writing
    - Research assistance
    - Paraphrasing
    - Literature review

**Do not**:

- Make direct fetch calls to OpenAI from random components or routes.
- Hard-code secrets or API keys.
- Integrate non-OpenAI providers.

Each AI helper should:

- Take a structured input type.
- Return a structured, normalised result.
- Include basic error handling and logging hooks.
- Support usage tracking hooks (tokens/requests) in coordination with `packages/core`.

---

### 4.2 Plans, usage and enforcement (`packages/core`)

- Plan definitions live in `packages/core` (and corresponding Prisma schema).
- You must enforce plan rules in **server actions / route handlers**, not only in the UI.
- Usage tracking:
  - Introduce and maintain `usage_records` (or equivalent) in the Prisma schema as per `docs/ARCHITECTURE.md`.
  - Log AI usage per user, per tool where appropriate.
  - Block or throttle requests when limits are exceeded, with a clear, friendly message.

---

### 4.3 Billing (`packages/billing`)

- All Stripe integration goes through `packages/billing`.
- Support:
  - Free tier
  - The four paid tiers (Standard, Pro, VIP, Ultimate)
  - Checkout sessions
  - Customer Portal
  - Webhooks keeping user plan state in sync with Stripe

Rules:

- Webhook handlers must verify Stripe signatures.
- Never expose raw webhook payloads to the client.
- When adding or modifying plans, keep currencies, prices and intervals accurate.

---

### 4.4 Localisation (`packages/localisation`)

- Use the localisation package to source all user-facing copy.
- Default locale is `en-GB`.
- Must support at least:
  - `en-GB`, `en-US`, `en-AU`, `en-CA`, and fallback `en`.

Implementation expectations:

- Add Next.js i18n middleware as described in `docs/ARCHITECTURE.md`.
- Implement locale detection (headers/IP) plus a user override stored in settings.
- No hard-coded user-facing strings in components; they should go through localisation utilities.

---

### 4.5 Frontend (Next.js App Router)

- Prefer **Server Components** for pages and layouts unless interactivity is required.
- Use **Client Components** only when necessary (forms, interactive widgets, local state).
- Use **Server Actions** for mutations (creating documents, triggering AI calls, updating plans).
- Keep routing aligned with the architecture doc, including:
  - `/` – marketing
  - `/auth/*` – auth flows
  - `/app` – authenticated shell
  - `/app/dashboard` – user overview, plan details, usage
  - `/app/tools/*` – AI tools
  - `/app/workspace` – personal workspace (projects, versions, exports)

UI guidelines:

- Prefer components from `packages/ui` over ad hoc markup.
- Keep styling consistent and minimal with Tailwind classes.
- Respect accessibility standards (labels, roles, focus states).

---

## 5. Coding Style

General:

- Language: TypeScript everywhere (no plain JavaScript).
- Use modern ES modules and syntax.
- Prefer composition over inheritance.
- Keep functions small and focused.

TypeScript:

- No `any` unless absolutely unavoidable; prefer generics or stricter types.
- Define shared types/interfaces in appropriate shared modules.
- Use discriminated unions where they improve clarity.

Error handling:

- Fail fast on clearly invalid states.
- Use typed errors where helpful.
- On the client, present friendly error messages and non-crashing fallbacks.

Validation:

- Use Zod (or the chosen validation library) for:
  - Incoming data from users
  - Server action inputs
  - Critical configuration where relevant

---

## 6. Testing and Quality

You should treat tests as a first-class requirement.

Expectations:

- Unit tests for:
  - Plan and usage logic (`packages/core`)
  - AI helper functions (`packages/ai-client`) where feasible (mock OpenAI)
  - Billing logic (`packages/billing`) with mocked Stripe
- Integration tests for:
  - Auth flows
  - Main dashboard behaviour
  - AI tool workflows (end-to-end with mocks where needed)
- End-to-end tests (Playwright or similar) for:
  - Sign up → choose plan → pay → use an AI tool
  - Return user → workspace
  - Locale override flow

When adding new features, you should:

- Add or update tests in the same change set.
- Keep tests readable and aligned with existing test style.

---

## 7. Security and Compliance

You must:

- Never log secrets or sensitive personal data.
- Store secrets only in environment variables (`.env`, `.env.local`) and reference them in code without hard-coding values.
- Use secure session handling with httpOnly cookies.
- Implement a reasonable Content Security Policy consistent with Next.js and Vercel.
- Respect GDPR-aligned features described in `docs/PRD.md` and `docs/ARCHITECTURE.md`:
  - Data export
  - Account deletion (including AI-generated content and usage records, where required)

Stripe:

- Always verify webhook signatures.
- Treat all webhook endpoints as publicly exposed and secure them accordingly.

---

## 8. What Not To Do

You must not:

- Introduce enterprise or institutional features.
- Switch the runtime AI provider away from OpenAI.
- Hard-code API keys, secrets or private URLs.
- Add experimental libraries or frameworks without strong justification.
- Introduce breaking changes without updating all call sites and noting them clearly.
- Bypass the shared packages (`core`, `ai-client`, `billing`, `localisation`, `ui`) by duplicating logic in `apps/web`.

---

## 9. Working with Tasks and Prompts

When the user gives you a task (for example: “implement `/app/tools/writing` end to end”):

1. Re-state the task in your own words.
2. Cross-check what the PRD and architecture say about that area.
3. Outline:
   - Data model needs
   - Server actions / routes
   - UI components and pages
   - Tests to add or update
4. Implement the work in coherent steps, explaining each step.
5. Keep your changes consistent with this Codex, `docs/PRD.md` and `docs/ARCHITECTURE.md`.

If something is truly ambiguous:

- Propose a sensible default that does **not** contradict existing docs.
- Make that assumption explicit in your explanation.

---

## 10. Definition of Done (per feature)

A feature is only “done” when:

1. It aligns with `docs/PRD.md` and `docs/ARCHITECTURE.md`.
2. It uses the correct shared packages (no duplication of logic).
3. It has basic test coverage for core behaviour.
4. It handles error and loading states sensibly.
5. It respects plans, usage limits and security rules.
6. It does not introduce obvious regressions or type errors.

Only consider work complete if all of the above are satisfied.

---
