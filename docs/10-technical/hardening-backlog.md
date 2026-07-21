# Technical Hardening Backlog

Version: 1.0

Status: Audit backlog — items are not considered resolved unless explicitly marked

---

# Purpose

This document converts the technical audit into a prioritized backlog.

It covers security, privacy, data integrity, reliability and engineering quality. It does not replace issue tracking or a formal security review.

---

# Priority Definitions

- **P0 — Immediate:** credible unauthorized access, exposed credentials or severe data-integrity risk
- **P1 — Before scaling:** material privacy, authorization, entitlement or operational reliability risk
- **P2 — Planned hardening:** maintainability, observability, performance and developer-experience improvements

Recommended status values:

- `Open`
- `In progress`
- `Needs decision`
- `Verified`
- `Accepted risk`

A code change alone is not enough to mark an item Verified; each item requires a test or documented validation.

---

# P0 — Immediate

## P0.1 Audit and protect legacy mutations

Status: Open

Finding:

Some objective, following and user/social endpoints appear to accept caller-supplied user IDs without consistently applying `requireAuth` and ownership checks.

Required outcome:

- Every mutation requires authentication unless intentionally public.
- Actor identity comes from `req.user`, not a body/query `userId`.
- Operations on another user require an explicit relationship or admin policy.
- Add negative tests for anonymous and cross-user requests.

Primary areas:

- `routes/objetivos.js`
- `routes/seguimiento.js`
- `routes/users.js`
- `routes/evaluaciones.js`

Verification:

- Anonymous mutations return 401.
- User A cannot read or modify private data belonging to User B.
- Allowed Red and Org use cases still work.

## P0.2 Fix evaluation ownership and privacy boundaries

Status: Open

Finding:

Evaluation reads and privacy updates rely in places on supplied `userId`/`viewerId`; omitting or falsifying viewer context may expose private evaluations. Privacy updates must verify ownership.

Required outcome:

- Derive viewer identity from JWT.
- Enforce owner/follower/org policy in one reusable service.
- Verify ownership on evaluation updates.
- Ensure date/detail/summary/feed routes use the same policy.

Verification:

- Private evaluations are hidden from unauthorized Red users.
- Privacy cannot be changed by another athlete.
- Org visibility follows the product decision in P1.1.

## P0.3 Repair organization invitation security

Status: Open

Finding:

Invitation acceptance/revocation and registration contain inconsistent authentication assumptions. The audited registration path may store a supplied password without the normal hashing flow and uses CommonJS `require()` inside an ESM backend.

Required outcome:

- Hash passwords through the same auth service used by normal registration.
- Authenticate administrative revoke/remind operations.
- Clearly separate token-authorized public actions from JWT-authorized actions.
- Remove ESM-incompatible runtime calls.
- Add token expiry, reuse and invalid-token tests.

Verification:

- No plaintext password is persisted.
- Invitation acceptance succeeds in the Node ESM runtime.
- Revocation requires authorized org context.
- Consumed, expired and forged tokens are rejected.

## P0.4 Rotate and remove exposed credentials

Status: Open

Finding:

The audit detected plaintext database/Atlas credentials in a repository-local `.env`. Whether the file is tracked could not be verified.

Required outcome:

- Treat exposed credentials as compromised and rotate them.
- Confirm `.env` and provider credentials are ignored and absent from history.
- Move secrets to Render/EAS/Vercel secret stores.
- Review MongoDB Atlas access logs and network access.
- Document secret ownership and rotation.

Verification:

- Old credentials no longer authenticate.
- Secret scanning passes.
- Production services work with replacement credentials.

## P0.5 Protect admin organization creation

Status: Open

Finding:

`POST /api/org/admin/orgs` appears authenticated but not consistently restricted to `isSuperAdmin`, unlike neighboring admin routes.

Required outcome:

- Apply the same global super-admin middleware as other `/api/org/admin/*` operations.
- Audit the full admin route namespace for consistent policy.

Verification:

- Normal authenticated users receive 403.
- Super-admin creation continues to work.

---

# P1 — Privacy, Integrity And Scale Readiness

## P1.1 Decide Org visibility of private evaluations

Status: Needs decision

Decision required:

Does `Evaluacion.privado` mean:

1. Hidden only from Red/social users, but visible to authorized organization staff; or
2. Hidden from organization staff too unless the athlete explicitly shares it?

Current implementation may include private evaluations in Org dashboards and persisted reports.

Required outcome:

- Write the product/privacy rule.
- Reflect it in consent and UI copy.
- Apply it consistently to dashboards, player detail, reports and exports.
- Define whether aggregate metrics may include private records.
- Define historical access when membership ends.

## P1.2 Bound organization access to membership windows

Status: Open

Finding:

Some player detail/aggregate views may include data from before the athlete joined an organization.

Required outcome:

- Define whether Org can access pre-membership data.
- Default queries to active/historical Membership windows.
- Prevent access after the permitted window unless explicitly retained by policy.
- Apply the same range policy to generated Org reports.

## P1.3 Fix multi-organization entitlement behavior

Status: Open

Finding:

`User.plan` duplicates entitlement truth. A user with multiple organization memberships may receive only the first org grant; revoking it can incorrectly downgrade access despite another valid membership.

Required outcome:

- Resolve Pro from all active grants rather than the mutable legacy plan.
- Create one idempotent grant per user/org/source.
- Recompute effective entitlement after every grant/revoke.
- Treat `User.plan` as legacy/display state or remove it as entitlement truth.

Verification:

- Two-org membership remains Pro when either valid grant remains.
- Revoking the final grant downgrades only when no other Pro source exists.

## P1.4 Make cross-database commercial flows recoverable

Status: Open

Finding:

Campaign capacity is reserved in org DB while benefit grants are written in main DB. There is compensating rollback but no atomic cross-database transaction.

Required outcome:

- Define durable operation/idempotency state.
- Make retries converge after failure at every write boundary.
- Add reconciliation for reserved capacity without grant and grant without CampaignUser.
- Alert on inconsistent records.

## P1.5 Harden Socket.IO

Status: Open

Finding:

Socket.IO accepts broad origins, has no handshake authentication and allows clients to request arbitrary user rooms.

Required outcome:

- Validate JWT during socket handshake.
- Derive room identity from authenticated user.
- Apply an explicit origin allowlist.
- Avoid global broadcasts for user-sensitive events.
- Document event payload privacy.

## P1.6 Move recurring business jobs to durable scheduling

Status: Open

Finding:

Invitation reminders, fallback notifications and benefit expiry run as intervals inside the web process. Multiple instances can duplicate work; restarts interrupt schedules.

Required outcome:

- Move critical jobs to Render cron/worker or introduce distributed leases.
- Preserve idempotency in dispatch/grant/report models.
- Add job run metrics and failure alerts.

## P1.7 Establish automated tests and CI

Status: Open

Minimum backend coverage:

- JWT, account status and legal gate
- Cross-user and cross-org authorization
- Evaluation privacy and timezone/day uniqueness
- Invitation lifecycle/password hashing
- RevenueCat idempotency
- Entitlement resolution across multiple sources
- Campaign recovery
- Notification deduplication

Minimum mobile coverage:

- Auth/legal/onboarding redirects
- Evaluation wizard completion
- Notification deep links
- Pro reconciliation
- Spanish/English key completeness

CI should run lint, typecheck and tests on every pull request.

## P1.8 Secure mobile token storage

Status: Open

Finding:

Mobile JWT is stored in AsyncStorage.

Required outcome:

- Migrate bearer token to Expo SecureStore/Keychain/Keystore.
- Keep only non-sensitive cached profile data in AsyncStorage.
- Define migration and logout behavior for existing installs.

## P1.9 Strengthen session lifecycle

Status: Open

Finding:

JWTs are long-lived and there is no visible refresh-token, revocation or device-session mechanism.

Required outcome:

- Decide session duration and refresh strategy.
- Support revocation after password reset, deletion or suspected compromise.
- Consider server-side token version/session records.
- Keep web localStorage exposure in the threat model.

---

# P2 — Planned Hardening

## P2.1 Standardize validation and error contracts

Status: Open

- Apply schema validation consistently to request body/query/params.
- Use a shared error shape and error codes.
- Add explicit 404 handling.
- Avoid returning implementation details.

## P2.2 Add baseline HTTP protections

Status: Open

- Security headers
- Global/request-specific rate limits
- Request IDs
- Payload-size limits
- Structured audit events for sensitive admin actions
- CORS configuration shared with Socket.IO

## P2.3 Improve database consistency

Status: Open

- Normalize `Evaluacion.userId` from String to ObjectId through a migration.
- Add indexes for common Objetivo and InformeCoach queries.
- Prevent duplicate pending Red invitations.
- Consider uniqueness for invoice numbers and invitation token hashes.
- Review `autoIndex: true` in production and adopt managed migrations.
- Reconcile denormalized Membership arrays in Rosters/Teams.

## P2.4 Normalize timezone behavior

Status: Open

- Use one documented default timezone policy.
- Remove server-local `startOf/endOf('day')` from athlete-scoped analytics.
- Handle daylight-saving 23/25-hour local days.
- Backfill legacy evaluations missing `diaLocal`.

## P2.5 Improve readiness and graceful operation

Status: Open

- Add readiness checks for both MongoDB connections.
- Do not report ready before critical database connections succeed.
- Add graceful shutdown for HTTP, Socket.IO and Mongoose.
- Pin supported Node version.
- Document required environment variables, including `MONGO_ORG_URI`.

## P2.6 Reduce process-local state

Status: Open

- Replace or explicitly accept per-process IEA/admin caches.
- Replace in-memory generation locks where duplicate work matters.
- Define cache invalidation and stale-data behavior.

## P2.7 Improve mobile server-state handling

Status: Open

- Define consistent stale time, retry and invalidation behavior.
- Decide whether a query-cache library is justified.
- Do not describe the app as offline-capable without persistent data and mutation sync.
- Consider persistence only for data that is safe and useful offline.

## P2.8 Review observability privacy

Status: Open

- Review mobile `sendDefaultPii: true`.
- Redact sensitive request, evaluation and report fields.
- Replace unstructured console logs with structured, leveled logging.
- Restrict or remove public `/sentry-test` in production.
- Define retention for EmailLog, reports, mixed AI context and ProductEvent properties.

## P2.9 Split oversized backend modules

Status: Open

- Decompose `routes/org.js` by organization domain.
- Move validation and business logic into tested services.
- Keep route handlers focused on HTTP translation and orchestration.

---

# Recommended Execution Order

1. Rotate credentials and close unauthenticated/cross-user mutations.
2. Fix evaluation privacy/ownership and invitation registration.
3. Decide Org privacy and historical access policy.
4. Fix multi-org entitlement resolution.
5. Add regression tests and CI for the corrected boundaries.
6. Harden Socket.IO and recurring jobs.
7. Address database/timezone migrations and operational improvements.

---

# Definition Of Done

An item can be marked Verified only when:

- The intended policy is documented.
- Implementation is merged and deployed.
- Positive and negative automated tests exist.
- Existing data has been migrated/reconciled if required.
- Logs/metrics demonstrate expected production behavior.
- Security/privacy-sensitive changes have been independently reviewed.

---

# Related Documents

- [README.md](./README.md)
- [architecture.md](./architecture.md)
- [backend.md](./backend.md)
- [database.md](./database.md)
- [mobile.md](./mobile.md)
