# Sentiq Mobile Application

Version: 1.0

Status: Current implementation

---

# Purpose

This document describes the architecture of `sentiq-app`, the athlete-facing mobile application.

---

# Technology

- Expo 54
- React Native 0.81
- React 19
- TypeScript 5.9
- Expo Router 6
- Zustand 5
- Axios
- React Native Paper
- RevenueCat (`react-native-purchases`)
- Sentry React Native
- Expo Notifications
- i18next / react-i18next / ICU

The app targets iOS and Android. Expo web configuration exists but the athlete product is designed primarily as a native app.

---

# Build And Runtime Configuration

Key files:

- `app.json` — base Expo metadata
- `app.config.ts` — profile-aware runtime configuration
- `eas.json` — EAS build/submission profiles

Application identity:

- Scheme: `sentiq`
- iOS bundle: `com.sentiq.app`
- Android package: `com.sentiq.app`
- Portrait orientation
- Light UI

Build profiles include pilot, internal Android and RevenueCat testing.

API URL resolution priority:

1. `EXPO_PUBLIC_API_URL`
2. Expo `extra.API_URL`
3. `https://sentiq-back.onrender.com`

Expo Updates uses app-version runtime policy, reducing incompatible OTA updates across native versions.

---

# Root Provider Tree

`app/_layout.tsx` initializes:

```text
I18nextProvider
└── GestureHandlerRootView
    └── BottomSheetModalProvider
        └── SafeAreaProvider
            └── PaperProvider
                └── Navigation ThemeProvider
                    └── AuthProvider
                        └── NotificationsProvider
                            └── AuthRedirect / Stack
```

Global Toast and StatusBar are also mounted.

Sentry wraps the root only when enabled and configured for the build profile.

---

# Navigation

Expo Router provides file-based routing with typed-route experiment enabled.

## Root Routing

`app/index.tsx` redirects to Home; `AuthRedirect` then resolves the real destination.

Routing gates:

- No user → login
- Incomplete onboarding → onboarding Step 1
- Pending post-core schedule setup → Sports Mode setup
- Authenticated + complete → Home
- Legal reacceptance is triggered by API response and legal context

The native splash remains visible until navigation, auth and schedule-pending state are ready.

## Main Tabs

Authenticated tabs:

- Home
- Objectives
- Evaluations
- Progress
- Red

## Feature Routes

Major route groups:

- `(auth)` — login, register, forgot/reset password
- `onboarding` — profile, objective, mode and schedule
- `evaluacion-wizard` — full evaluation steps 0–5
- `evaluacion/rapida` — quick evaluation
- `evento/[fecha]` — event detail
- `meta-foco` — wizard/edit/close
- `modos` — Sports Mode setup and alerts
- `preparacion` — match preparation
- `informe` — Coach report
- `planes` — Pro overview/purchase
- `rewards` — Unlock Pro
- `siguiendo` / `red` — social graph and followed-user views
- `legal` — terms/privacy/acceptance

---

# Authentication And Session

`AuthContext` owns:

- current user
- auth initialization/loading
- login/register/logout
- legal reacceptance state
- client feature helper

Persistence in AsyncStorage:

- `token`
- `userId`
- cached `user`

Startup:

1. Load cached user optimistically.
2. Load token.
3. Validate token through `/api/auth/me`.
4. Replace cache with server User.
5. Reconcile language and legal status.

Network failures keep the cached session for UX continuity.

401 clears session and redirects to login.

Pending/deleted account response clears session and explains deactivation.

This is partial offline tolerance, not full offline product support: most screens still require API data and writes are not queued.

---

# API Client

`lib/api.ts` creates a shared Axios instance.

- 60-second timeout to tolerate backend cold starts
- Request interceptor reads JWT from AsyncStorage
- Response interceptor handles legal 428, auth 401 and deleted-account 403
- Development builds log request/response details

New API calls should use this client unless a platform limitation requires otherwise.

Do not log sensitive payloads in production.

---

# State Management

## React Context

Used for cross-cutting services:

- Authentication/User
- Notifications scheduling API

## Zustand

Used for feature workflows and server-backed view state:

- Evaluation wizard
- Evaluation lists/details
- Objectives/pagination/reviews
- Meta Foco and wizard
- IEA
- Red feed/invitations
- Onboarding/registration
- Premium/RevenueCat
- Unlock Pro

## Local Component State

Used for transient UI state, form fields, filters and modals.

Guideline:

- Context for app-wide service/session state
- Zustand for shared feature workflow/cache
- Local state for screen-only UI
- Backend remains durable source of truth

---

# Evaluation Flow

Full evaluation wizard uses `useEvaluacionStore` across steps:

1. Event type/date/emotion/privacy
2. Five 1–5 dimensions
3. Reflection (`hechoBien`, `mejorar`)
4. New objectives and optional AI suggestions
5. Prior objective review
6. Completion summary

Quick evaluations can later hydrate the full wizard and complete the same record.

After save, related stores/caches must refresh rather than assuming local wizard state is durable.

---

# Notifications

`NotificationsProvider` handles:

- Native permission/channel setup
- Push token registration/unregistration
- Foreground presentation
- Tap/deep-link navigation
- Local pre-match reminders
- Local post-training/post-match reminders
- Off-mode daily reminder
- Fallback quick-evaluation scheduling where enabled
- Persistence/cancellation of Sentiq-created notification IDs

Scheduling uses athlete timezone and weekly schedule.

Notification payload navigation normalizes query parameters and JSON-stringified params.

See [../03-product/notifications.md](../03-product/notifications.md).

---

# RevenueCat And Pro

RevenueCat initialization is profile/kill-switch controlled.

`lib/revenuecat.ts`:

- configures platform API key once
- logs in with Sentiq User ID
- reads CustomerInfo/offerings
- purchases/restores/manages subscriptions
- logs out on Sentiq logout

`usePremiumStore` holds RevenueCat customer info and offerings.

Effective access also comes from server grants:

- Early adopter / pilot
- Admin
- Organization grant
- Campaign/Unlock benefit grant

The app may hide or show UI, but backend feature middleware is authoritative.

Remote rollout flags come from `/api/feature-flags` and default conservatively to false on error.

---

# Internationalization

Supported languages:

- Spanish (`es`, default/fallback)
- English (`en`)

i18next loads static namespaces with ICU formatting.

Language sources:

- saved local preference
- device locale
- authenticated User language

Auth reconciles app and server language after login.

Every new user-facing string should be added to both locale trees and the appropriate namespace.

---

# Observability

Sentry mobile is enabled outside development when DSN is configured.

Configured capabilities:

- Error capture
- Session replay sampling
- Error replay
- Feedback integration
- Release and environment metadata

Authentication associates Sentry user context after login and clears it on logout.

Avoid sending unnecessary personal data; `sendDefaultPii` is currently enabled and should be reviewed against privacy requirements.

---

# Media

Profile images use Expo Image Picker and backend Cloudinary upload.

The backend validates image type/size; the client should still provide friendly validation and compression where appropriate.

---

# Deep Links

App scheme: `sentiq://`

Android intent filter accepts the Sentiq scheme.

Notifications deep-link to:

- evaluation wizard
- quick evaluation
- preparation
- other Expo Router paths carried in payload

Deep-link routes must still pass auth/legal/onboarding gates.

---

# Offline And Caching Behavior

Available:

- cached user/session snapshot
- local language
- onboarding schedule-pending marker
- local notification IDs
- Zustand in-memory caches

Not currently a complete offline architecture:

- no durable mutation queue
- no conflict resolution
- no offline database for evaluation history
- feature stores usually refetch from API

Do not advertise full offline support.

---

# Security Considerations

- JWT is stored in AsyncStorage, not platform secure keychain storage
- Runtime mobile `extra` is public; never place server secrets there
- RevenueCat public SDK keys may be bundled; webhook/server secrets must not
- Backend is responsible for authorization and privacy
- Development API logs can expose payloads locally
- Deep links must not bypass route/server access checks

Moving auth tokens to secure storage is a future hardening opportunity.

---

# Quality And Testing

Available script:

- `npm run lint` → Expo ESLint

No mobile test script is defined.

Priority test areas:

- Auth/legal/onboarding redirects
- Evaluation wizard persistence/completion
- Timezone calendar dates
- Notification scheduling/deep links
- RevenueCat + server entitlement reconciliation
- Red privacy/access
- Spanish/English key completeness

---

# Development Rules

- Use Expo Router routes, not ad-hoc navigation state.
- Use the shared Axios client.
- Treat Zustand as cache/workflow state, not database.
- Keep server authorization independent from UI feature checks.
- Add translations in Spanish and English.
- Test physical-device behavior for notifications, purchases and deep links.
- Keep build-profile flags conservative and document rollout changes.

---

# Related Documents

- [architecture.md](./architecture.md)
- [backend.md](./backend.md)
- [database.md](./database.md)
- [../03-product/README.md](../03-product/README.md)
