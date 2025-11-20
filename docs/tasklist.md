# StudentsAI – TASK LIST

A clean, readable checklist of all engineering work required to complete the StudentsAI platform.

## 1. Core Infrastructure
- [ ] Validate monorepo folder structure
- [ ] Standardise TypeScript, ESLint, Prettier and Turborepo configs
- [ ] Ensure all package imports resolve correctly
- [ ] Add README.md to each package
- [ ] Clean unused boilerplate files

## 2. Authentication
- [x] Complete Auth.js/NextAuth configuration
- [ ] Implement email/password registration
- [x] Add Google OAuth
- [x] Add Microsoft OAuth
- [x] Build /auth/login and /auth/register pages
- [ ] Add password reset flow
- [ ] Build /app/settings/account page
- [ ] Add user locale preference field

## 3. Billing
- [x] Add Free tier
- [ ] Create Stripe products + prices for all paid tiers
- [ ] Implement Checkout session creation
- [ ] Integrate Stripe Customer Portal
- [ ] Implement /api/webhooks/stripe with signature verification
- [ ] Sync subscription state to Prisma user model
- [x] Add server-side plan enforcement utilities
- [ ] Update UI to reflect real plan status

## 4. AI System
- [ ] Upgrade all AI calls to GPT-5.1
- [x] Implement academic writing helper
- [x] Implement research assistant helper
- [x] Implement paraphrasing helper
- [x] Implement literature review helper
- [ ] Add structured input/output types
- [ ] Add robust error handling
- [x] Add usage tracking hooks

## 5. Usage Tracking
- [x] Add usage_records table to Prisma schema
- [ ] Track request count per tool
- [ ] Track token usage per OpenAI call
- [x] Enforce plan limits before executing any action
- [ ] Display real usage in dashboard widgets

## 6. Localisation
- [ ] Integrate Next.js i18n middleware
- [ ] Add locale detection from headers/IP
- [ ] Implement user override stored in settings
- [ ] Replace all hard-coded text with translation keys

## 7. UI/UX
- [ ] Build shared UI components (Button, Input, Card, Dialog)
- [ ] Implement /app navigation shell
- [ ] Build all /app/tools/* pages
- [ ] Replace raw Tailwind markup with UI kit components
- [ ] Standardise spacing, typography and colours

## 8. Workspace System
- [ ] Add tables: projects, documents, versions
- [ ] Build workspace dashboard
- [ ] Implement document editor
- [ ] Add create/rename/delete functionality
- [ ] Implement version history
- [ ] Add export formats (PDF, DOCX, Markdown)

## 9. Testing
- [ ] Configure unit test environment
- [ ] Add tests for core logic
- [ ] Add tests for AI helpers (mocked)
- [ ] Add integration tests for server actions
- [ ] Add E2E tests for:
      - Auth
      - Plan purchase
      - AI tool usage
      - Workspace flows
      - Localisation

## 10. Deployment
- [ ] Add production environment variables
- [ ] Configure Vercel project
- [ ] Apply Prisma migrations
- [ ] Confirm production builds succeed

## 11. Final QA
- [ ] Full manual walkthrough
- [ ] Accessibility audit
- [ ] Validate error states and fallbacks
- [ ] Performance review
- [ ] Prepare launch checklist
