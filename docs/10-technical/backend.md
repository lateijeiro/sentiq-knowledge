# Sentiq Backend

Version: 1.0

Status: Current implementation

---

# Purpose

This document describes `sentiq-back`, the shared server for the athlete app, organization portal, marketing forms and internal KPI panel.

---

# Technology

- Node.js
- Express 5
- JavaScript ES modules (`"type": "module"`)
- Mongoose 8
- MongoDB / MongoDB Atlas
- Socket.IO
- JWT
- Sentry

The backend is not currently TypeScript.

Entry point: `sentiq-back/index.js`

Development command: `npm run dev` (nodemon)

Production command: `node index.js`

---

# Directory Structure

- `routes/` — HTTP endpoints and request orchestration
- `controllers/` — selected larger domain/admin handlers
- `services/` — domain logic, integrations and aggregations
- `models/` — Mongoose schemas
- `middleware/` — auth, org scope, roles, uploads, rate limiting, feature access
- `utils/` — time/email helpers
- `db/` — secondary organization connection
- `scripts/` — scheduled generation and maintenance commands
- `data/` — static resources used by AI generation
- `docs/` — backend-specific technical notes

This is a modular monolith. Boundaries are by folder/domain, not independently deployed services.

---

# Request Pipeline

Order in `index.js` matters:

1. Load environment and register models.
2. Initialize Sentry.
3. Apply CORS allowlist.
4. Mount RevenueCat webhook with raw body parser.
5. Apply JSON body parser to remaining routes.
6. Create HTTP + Socket.IO server.
7. Mount public health/legal/contact routes.
8. Apply versioned terms acceptance gate to `/api`.
9. Mount domain routes.
10. Install Sentry and JSON error handlers.
11. Connect MongoDB and start server.
12. Start recurring in-process jobs.

RevenueCat must receive the raw request body; moving `express.json()` before that webhook can break parsing.

---

# API Domains

## Identity And Legal

- `/api/auth` — registration, login, profile, password recovery, account lifecycle
- `/api/users` — user/profile endpoints
- `/public/legal` — public policies
- `/api/legal` — legal acceptance
- `/api/contact` — rate-limited contact form
- `/api/solicitud-piloto` — public pilot request

## Athlete Product

- `/api/evaluaciones`
- `/api/objetivos`
- `/api/objetivos-sugeridos`
- `/api/meta-foco`
- `/api/progreso`
- `/api/informe-coach`
- `/api/reports`
- `/api/coach`
- `/api/preparacion`
- `/api/learning-cards`

## Social

- `/api/seguimiento`
- `/api/feed`

## Premium And Growth

- `/webhooks/revenuecat`
- `/api/subscription`
- `/api/feature-flags`
- `/api/early-adopters`
- `/api/rewards`
- `/api/benefits`
- `/api/campaigns`

## Organizations

- `/api/org`
- `/api/org/dashboard`
- `/api/org/reports`
- `/api/org/invitations`

## Notifications

- `/api/notifications`

## Internal Analytics

- `/api/admin/kpis`
- `/api/admin/cohortes`

---

# Authentication

`requireAuth`:

- Reads `Authorization: Bearer <JWT>`
- Verifies with `JWT_SECRET`
- Supports payload IDs in `id`, `_id` or `userId`
- Reloads User from MongoDB
- Rejects pending-deletion/deleted accounts
- Sets `req.user`
- Updates `lastActiveAt` asynchronously
- Attaches user context to Sentry

Passwords are hashed in auth flows using bcrypt/bcryptjs.

The backend remains the source of truth even if the mobile app has a cached user.

---

# Terms Acceptance

`requireTermsAcceptance` compares User acceptance versions with:

- `TERMS_VERSION`
- `PRIVACY_VERSION`

If missing/outdated, it returns:

- HTTP 428
- `error: TERMS_NOT_ACCEPTED`

Auth, legal and selected public invitation routes are excluded so users can authenticate and accept terms.

---

# Authorization

## Athlete / Admin

Routes combine:

- `requireAuth`
- ownership or route-specific access checks
- `requireAdmin` / `requireSuperAdmin`
- feature middleware

Do not assume a route is safe solely because it accepts a `userId`. Every new endpoint must verify ownership or permitted relationship.

## Organization

`requireOrgContext` resolves `X-Org-Id` (or request orgId), checks Membership and rejects:

- missing/invalid org
- non-members
- blocked memberships
- player access to the web portal

Role hierarchy:

`player < staff < coach < club_admin`

`requireOrgAdmin` permits club_admin or global super-admin with resolved org context.

Scope helpers prevent cross-org or unauthorized player-data access.

---

# Premium Features

Feature keys:

- `ia_objetivos`
- `informe_mensual`
- `coach_semanal`

`checkFeature` evaluates:

1. Optional admin bypass
2. User feature override
3. Direct plan permission
4. Effective premium entitlement

Effective premium may come from:

- paid subscription
- pilot/admin/legacy premium plan
- organization grant
- benefit grant (campaign, unlock program, admin, promotion)

Feature flags exposed by `/api/feature-flags` control rollout:

- subscriptions
- Unlock Pro program
- campaign benefits

Flags are not authorization; premium routes still enforce access.

---

# AI

OpenAI is called only from the backend.

Current output families:

- Objective suggestions
- Weekly/pre-match preparation
- Monthly Coach report
- Weekly/monthly Learning Cards

`aiContextBuilder` normalizes language, profile, event/mode and Meta Foco context.

AI services:

- minimize input to relevant athlete data
- normalize output formats
- apply feature checks before paid outputs
- persist reports/cards where appropriate
- use heuristic fallback for weekly coach when OpenAI is unavailable

Never expose `OPENAI_API_KEY` to clients.

---

# IEA And Progress

`ieaService` computes E/O/C/M, eligibility, confidence, tier and trend.

It uses an in-memory 15-minute cache for selected historical queries.

Evaluation/objective writes invalidate the relevant user cache.

Cache limitations:

- per-process
- lost on restart
- not shared across instances

This is acceptable for current scale but not a distributed cache.

---

# Notifications

Backend responsibilities:

- Register/deactivate Expo push tokens
- Send chunked Expo push notifications
- Deactivate `DeviceNotRegistered` tokens
- Run fallback evaluation reminders
- Deduplicate fallback sends with NotificationDispatch

Most primary schedule notifications are local to the mobile app. Backend fallback reminders send after scheduled events when no evaluation exists for the athlete's local date.

---

# Email And Media

## Email

Resend sends transactional and invitation email.

EmailLog records send/error counts for infrastructure metrics without blocking delivery.

SendGrid remains installed and may support legacy paths; current organization email helper uses Resend.

## Media

Multer accepts in-memory JPG/JPEG/PNG uploads up to 5 MB.

Cloudinary stores:

- User avatars
- Organization logos

Transformations normalize image size/quality.

---

# Subscriptions And Benefits

## RevenueCat

Webhook:

- Authenticates request header
- Parses raw body
- Deduplicates by `RcWebhookEvent.eventId`
- Updates Subscription
- Protects early-adopter/pilot access
- Avoids downgrading users with active org grants

## Benefits Engine

BenefitDefinition + UserBenefitGrant implement time-limited Pro grants with:

- idempotency key
- source attribution
- active/expired/revoked state
- stack policy
- expiration job

## Organization Grant

EntitlementGrant represents Pro granted by org player membership in the organization database.

Entitlement resolution coordinates subscription, org grant and main-database benefit grant.

---

# Background Work

## Render Cron Jobs

Configured in `render.yaml`:

- Weekly Learning Cards
- Monthly Learning Cards
- Weekly Org briefings

## In-Process Intervals

- IEA cache cleanup
- Org invitation reminders
- Fallback evaluation reminders
- Benefit grant expiration

Before horizontal scaling, move business-critical intervals to a single scheduler or enforce fully durable leasing/deduplication.

---

# Socket.IO

The server supports:

- client connection
- `join(userId)` room joining
- disconnect logging

Current Socket.IO CORS is broad and room joins trust the supplied user ID. Treat this channel as non-sensitive until it authenticates sockets and authorizes room membership.

---

# Observability

Sentry backend configuration:

- environment from runtime variables
- 20% trace sample rate
- ignores health checks
- ignores most errors below HTTP 500
- Express error handler returns optional Sentry event ID

The internal KPI panel also consumes operational usage/cost services for Render, Vercel, Atlas, OpenAI, Cloudinary and Sentry.

---

# Deployment Configuration

`render.yaml` defines:

- Node web service
- MongoDB, JWT, OpenAI, Sentry, email and RevenueCat secrets
- scheduled generation jobs

Health endpoint:

`GET /health` → `200 OK`

Default port: `PORT` or 3001.

---

# Testing And Quality

Current package scripts:

- `dev`: nodemon
- `test`: placeholder that exits with error

There is no established backend automated test suite in package scripts.

Highest-priority tests:

- JWT/account status/legal gate
- Cross-user and cross-org authorization
- Evaluation timezone/day uniqueness
- RevenueCat webhook idempotency
- Campaign capacity and cross-db rollback
- Entitlement resolution/downgrade
- Notification deduplication

---

# Related Documents

- [architecture.md](./architecture.md)
- [database.md](./database.md)
- [mobile.md](./mobile.md)
