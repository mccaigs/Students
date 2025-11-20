# StudentsAI – Rules

This document defines the explicit, enforceable rules the AI coding assistant must follow when working inside the StudentsAI codebase. It complements the broader guidance in `docs/codex.md`, and the authoritative specifications in `docs/PRD.md` and `docs/ARCHITECTURE.md`.

These rules exist to ensure consistency, safety, architectural integrity and long‑term maintainability across the project.

---

## 1. Hierarchy of Truth
When generating or modifying code, you must follow this exact priority order:

1. **docs/PRD.md** – Product behaviour and feature requirements.
2. **docs/ARCHITECTURE.md** – Architectural structure and technical expectations.
3. **docs/codex.md** – Development methodologies, coding standards and working practices.
4. **docs/rules.md** – Operational constraints and behavioural rules.
5. **User instructions** – Follow unless they conflict with any of the above.

If conflicts arise, always choose the option closest to the top of the hierarchy.

---

## 2. Scope and Boundaries
The assistant must respect the following absolute boundaries:

- **Audience**: Individual Further Education (FE) and Higher Education (HE) students only.
- **Prohibited**: Any feature aimed at institutions, campuses, enterprise clients or administrators.
- **AI Providers**: Only **OpenAI** models may be used for inference. Never integrate Gemini, Claude, or any other provider.
- **Deployment Surface**: Cloud-only; never introduce local/on-device inference.
- **Pricing Structure**: Must match the tiers defined in `docs/PRD.md` exactly.

---

## 3. Technology Stack Rules
You must use and preserve the project’s official stack:

- Next.js (App Router)
- React + TypeScript
- Tailwind CSS
- PostgreSQL with Prisma
- Auth.js/NextAuth
- Stripe billing
- OpenAI GPT‑5.1 and fine‑tuned models
- Vercel deployment

You must **not** replace, supplement or modify this stack unless explicitly requested.

---

## 4. Code Placement Rules
All code must live in its correct layer according to the architecture.

### 4.1 apps/web
- Routing
- Pages + layouts
- Server Components and Client Components
- Server Actions
- User-facing UI

### 4.2 packages/ui
- Reusable, stylistically unified UI components
- Presentation logic only
- No business logic

### 4.3 packages/core
- Plan definitions
- Usage rules
- Shared types and validation
- Domain utilities

### 4.4 packages/ai-client
- Centralised OpenAI wrappers
- No other model providers
- Structured input/output
- Error handling

### 4.5 packages/billing
- Stripe integration
- Pricing metadata
- Webhooks
- Customer Portal helpers

### 4.6 packages/localisation
- i18n configuration
- Locale files
- Locale detection logic

The assistant must never duplicate logic across layers.

---

## 5. Behavioural Rules for Code Generation

### 5.1 Read Before Writing
Always inspect the surrounding file and related files before modifying code.

### 5.2 Explain Changes
After applying edits, always summarise:
- What changed
- Why it changed
- Files affected
- Follow‑up tasks (if any)

### 5.3 Maintain Small, Coherent Changes
Do not produce large cross‑cutting changes unless explicitly instructed.

### 5.4 Respect Existing Conventions
Follow the formatting, patterns and conventions already present in the repo.

### 5.5 Avoid Breaking Changes
If a change would break an exported interface, update all dependent code.

---

## 6. Security and Compliance Rules

- Never log secrets, tokens, or sensitive personal information.
- All environment variables must be referenced via `process.env`.
- Do not output or hard‑code real API keys.
- Ensure all Stripe webhooks verify signatures.
- Apply plan enforcement on the server, not just the UI.
- Uphold GDPR expectations as described in PRD and Architecture.

---

## 7. Validation and Error Handling

- All external input must be validated using Zod (or architecture‑approved equivalent).
- User‑facing errors must be safe and friendly.
- Server-side errors must be caught and handled predictably.

---

## 8. Testing Requirements

For every substantial feature:
- Add or update unit tests.
- Add integration tests if server actions or business logic are involved.
- Ensure no regression failures.

End‑to‑end tests should cover:
- Authentication flows
- Plan purchase flows
- AI generation flows
- Workspace creation and usage
- Localisation override behaviour

---

## 9. Prohibited Behaviours
The assistant must not:

- Introduce enterprise or campus features.
- Replace OpenAI with other models.
- Add new pricing tiers.
- Invent features not described in PRD.
- Duplicate logic across packages.
- Produce UI that violates the design system.
- Generate code that bypasses plan or usage enforcement.
- Commit large architectural refactors without approval.

---

## 10. Definition of Done
A task is considered finished ONLY when:
- It aligns with PRD functionality.
- It adheres to the architecture structure.
- It respects all codex and rules constraints.
- It includes appropriate validation and error handling.
- It uses the correct layer (core, ai-client, billing, localisation, ui).
- It includes or updates tests.
- It introduces no breaking regressions.

Only work that satisfies **all** the above criteria is considered complete.

