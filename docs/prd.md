# StudentsAI – Product Requirements Document (PRD)

## 1. Overview
StudentsAI is a cloud‑only SaaS platform designed exclusively for individual college and university students. It provides study assistance through academic writing tools, research helpers, paraphrasing utilities and literature analysis powered entirely by OpenAI models. The platform is global and defaults to UK English, with regional variants automatically selected based on a user’s location.

## 2. Core Principles
StudentsAI operates on several mandatory principles:
- All users are individual tenants; no institutional or enterprise licensing is supported.
- All AI processing is performed in the cloud using OpenAI GPT‑5.1 and fine‑tuned derivatives.
- No device‑side inference or self‑hosted models.
- A unified, consistent UX across desktop and mobile.
- Clear plan boundaries and usage limits.

## 3. Pricing and Plans
The service offers the following subscription tiers, billed monthly:
- **Free** – limited usage of AI tools and basic features.
- **Standard (£12)** – expanded limits and access to core study tools.
- **Pro (£19)** – higher limits, additional research tools and quality enhancements.
- **VIP (£27)** – advanced features, priority processing and higher quotas.
- **Ultimate (£35)** – full access to every tool, highest limits and premium reliability.

Each tier defines both feature access and usage limits. Plans apply to users individually and cannot be shared.

## 4. Target Users
The product serves:
- Further Education (FE) students
- Higher Education (HE) students
- Undergraduate and postgraduate learners requiring structured academic support

## 5. Feature Set
### 5.1 Academic Writing Assistant
- Generates essays, reports, structured arguments and reflective writing.
- Provides citation‑ready content in user‑selected academic styles.
- Produces outlines, thesis statements and supporting evidence.

### 5.2 Research Assistant
- Summarises academic articles.
- Produces reading lists and thematic analyses.
- Extracts key theories, models and debates.

### 5.3 Paraphrasing and Rewriting Tools
- Sentence‑level and paragraph‑level rewriting.
- Tone and complexity adjustments.
- Originality‑focused rephrasing.

### 5.4 Literature Review Generator
- Synthesises multiple sources.
- Identifies academic gaps and themes.
- Generates structured literature review sections.

### 5.5 Personal Workspace
- Saved projects (essays, research notes, prompts).
- Version history for generated content.
- Export tools: PDF, DOCX, Markdown.

### 5.6 Localisation
- Default UK English.
- Support for US, Australian, Canadian and other English variants.
- Automatic suggestion based on IP with manual override.

### 5.7 Usage Tracking
- Token and request counting per tool.
- Enforcement of plan-level limits.
- Dashboard for users to view their usage.

## 6. Technical Requirements
- Built using Next.js (App Router) with TypeScript.
- Styled using Tailwind CSS.
- Database: PostgreSQL accessed via Prisma.
- Authentication: Email/password and OAuth (Google and Microsoft).
- Billing: Stripe integration for subscription payments.
- AI: OpenAI GPT‑5.1 models and fine‑tuned variants.
- Deployment: Vercel as the primary hosting provider.

## 7. Security and Compliance
- All secrets stored in secure environment variables.
- No sensitive data stored without explicit user consent.
- Compliance with GDPR and applicable academic integrity expectations.

## 8. Non‑Functional Requirements
- High availability and consistent performance.
- Clear error handling and user‑friendly fallback messages.
- Predictable latency for AI interactions.
- Scalable architecture supporting global users.

## 9. Roadmap (High-Level)
1. Core platform scaffolding and authentication.
2. Subscription integration and usage limits.
3. AI tool implementation.
4. Personal workspace and document management.
5. Localisation layer.
6. Final QA, testing and optimisation.

