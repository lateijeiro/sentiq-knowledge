# Sentiq Knowledge Base

Official product, business and technical knowledge repository for Sentiq.

> Helping young athletes develop clarity, consistency and confidence.

---

## Purpose

This repository is the shared source of truth for:

- Product meaning and behavior
- Brand and positioning
- B2C and B2B strategy
- Technical architecture
- Known risks and constraints
- Decisions that should survive individual chat conversations

It is designed for founders, product work, design, engineering and AI assistants such as Claude.

The goal is to understand Sentiq without reading months of ChatGPT or Claude conversations.

---

## Using This Repository With Claude

### Claude Project

Recommended setup:

1. Create a Claude Project for Sentiq.
2. Upload or sync this repository as Project Knowledge.
3. Add the contents of `CLAUDE.md` to the Project's custom instructions.
4. Start new conversations inside that Project.
5. Return durable conclusions to this repository.

Claude chat does not have access to the application repositories unless code is explicitly provided. Technical claims that may have changed should be marked as needing verification.

### First Files To Read

1. [`CLAUDE.md`](./CLAUDE.md) — how the AI should reason
2. [`docs/00-overview.md`](./docs/00-overview.md) — executive product summary
3. The README or documents for the relevant domain

Do not load every document into every answer when a smaller relevant set is enough.

---

## What Sentiq Is

Sentiq is an athlete development platform that helps young athletes improve through structured reflection, objectives, progress and practical guidance.

It is not a therapy, psychology or mental-health product.

Coach Sentiq produces specific outputs; it is not a conversational chatbot and does not replace coaches.

---

## Product Ecosystem

Sentiq consists of five separate software projects:

- **sentiq-app** — athlete mobile app
- **sentiq-back** — shared backend API and jobs
- **sentiq-org** — organization and super-admin portal
- **sentiq-front** — marketing and legal site
- **sentiq-kpi-panel** — internal analytics panel

This Knowledge Base is a sixth, independent repository documenting the product ecosystem as a whole.

---

## Repository Structure

```text
README.md
CLAUDE.md
docs/
  00-overview.md
  01-vision.md
  02-brand.md
  03-product/
    README.md
    product-principles.md
    personas.md
    jobs-to-be-done.md
    evaluations.md
    objectives.md
    iea.md
    ai-coach.md
    home.md
    calendar.md
    premium.md
    notifications.md
  04-b2c/
    README.md
    user-acquisition.md
    onboarding.md
    free-plan.md
    early-adopters.md
    retention.md
    referrals.md
    growth-loops.md
    campaigns.md
  05-b2b/
    README.md
    clubs.md
    sentiq-org.md
    coach-experience.md
    clubes-fundadores.md
    pilot-program.md
    pricing.md
    sales-process.md
  10-technical/
    README.md
    architecture.md
    backend.md
    database.md
    mobile.md
    hardening-backlog.md
```

---

## Section Guide

### Overview, Vision And Brand

- [`00-overview.md`](./docs/00-overview.md)
- [`01-vision.md`](./docs/01-vision.md)
- [`02-brand.md`](./docs/02-brand.md)

Use these for positioning, purpose, principles and language.

### Product

[`docs/03-product/`](./docs/03-product/)

Use for current user-facing behavior: evaluations, objectives, IEA, Coach Sentiq, Home, calendar, Pro and notifications.

### B2C

[`docs/04-b2c/`](./docs/04-b2c/)

Use for athlete acquisition, onboarding, free/Pro strategy, retention, referrals, campaigns and growth loops.

### B2B

[`docs/05-b2b/`](./docs/05-b2b/)

Use for clubs, Sentiq Org, coach experience, pilots, pricing and sales.

### Technical

[`docs/10-technical/`](./docs/10-technical/)

Use for audited architecture, backend, data and mobile context.

Known risks:

[`docs/10-technical/hardening-backlog.md`](./docs/10-technical/hardening-backlog.md)

Backlog findings are not resolved unless explicitly marked and supported by verification evidence.

---

## Status Vocabulary

Documents and claims should use:

- **Current** — implemented or currently agreed behavior
- **Planned** — defined future work
- **Strategy** — direction or hypothesis
- **Needs validation** — insufficient product/business evidence
- **Needs technical verification** — requires current code/runtime inspection
- **Deprecated** — historical, no longer authoritative

Plans are not evidence of completion.

---

## Source Of Truth Rules

Within this repository:

1. Current documents
2. Technical architecture snapshots
3. Decision records
4. Planned documents
5. Strategy documents
6. Implementation plans
7. Chat history

For actual current software behavior, application code and runtime configuration are more authoritative than this repository.

When documentation and code conflict:

- Verify the implementation.
- Identify implementation drift or outdated documentation.
- State the discrepancy.
- Update the appropriate source.

---

## Documentation Workflow

When a meaningful conclusion emerges from Claude, ChatGPT, research or user feedback:

1. Determine whether it is fact, evidence, hypothesis, recommendation or decision.
2. Verify it against existing documentation.
3. Mark Current, Planned or Strategy.
4. Update the most specific document.
5. Preserve decision rationale when it matters.
6. Record what evidence is still missing.

Do not paste entire conversations into the repository. Distill durable knowledge.

---

## Product Stage And Priorities

Stage: Early-stage startup.

Default priorities:

1. Athlete value
2. Product validation
3. Retention and consistent use
4. Simplicity
5. Coach and club usefulness
6. Revenue validation
7. Organic growth
8. Scalability after demand

These priorities should be reviewed as evidence and company stage change.

---

## Documentation Quality Rules

- Prefer clear statements over aspirational language.
- Separate Current from Planned.
- Add evidence or source context for important claims.
- Do not invent implementation, users or metrics.
- Keep product language non-clinical.
- Update links when files move.
- Review time-sensitive documents periodically.
- Treat documentation changes as product work.

---

## Initial Git Setup

This folder is ready to become its own repository.

From this directory:

```bash
git init
git add .
git commit -m "Initialize Sentiq knowledge base"
```

Create and add the remote separately when ready.

Do not commit credentials, exports containing personal data or raw customer conversations.

---

## Final Principle

This repository exists to preserve context, improve decisions and reduce dependence on individual chat histories.

If it becomes outdated, it should be treated as a maintenance problem—not as reliable truth.
