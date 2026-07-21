# Sentiq Pro

Version: 1.0

---

# Purpose

Sentiq Pro is the athlete app's premium access layer.

It unlocks advanced AI-generated development guidance while keeping the core reflection and objective experience available to free athletes.

The product should communicate Pro as additional development value, not as artificial restriction or pressure.

---

# Product Location

Primary mobile surfaces:

- `sentiq-app/app/planes/index.tsx`: plan overview, benefits and current access;
- `sentiq-app/app/planes/comprar.tsx`: package selection, purchase and restore;
- Home: Pro upsell, campaign access and Unlock Pro cards;
- Progress: monthly report and Learning Cards;
- Evaluation Step 3: AI objective suggestions;
- Preparation: Coach Sentiq weekly message.

Backend:

- `/api/subscription`;
- RevenueCat webhook;
- plan and feature middleware;
- entitlement resolver;
- benefit, campaign and organization grants.

---

# Plans

The backend recognizes:

- `free`;
- `premium`;
- `pilot`;
- `admin`.

The athlete app presents the effective experience as Free or Sentiq Pro.

Pilot and admin plans receive the same configured premium features as the premium plan.

---

# Pro Features

Configured premium feature keys:

- `ia_objetivos`: Coach Sentiq objective suggestions;
- `informe_mensual`: monthly personal development report;
- `coach_semanal`: weekly preparation guidance.

Learning Cards also require effective Pro access. Their backend access may reuse the weekly coach feature override.

Current Pro value presented in the plan screen:

- weekly and monthly Learning Cards;
- AI-generated objective suggestions;
- monthly development reports;
- weekly or pre-match Coach Sentiq preparation.

Pro does not turn Coach Sentiq into a conversational chatbot.

---

# Effective Pro Access

Access is resolved from multiple valid sources.

## Paid Subscription

A RevenueCat-backed store subscription is active when:

- the Subscription record has `status: active`;
- `expiresAt` is absent or in the future.

## Pilot Plan

Users with `plan: pilot` receive Pro access.

## Admin Plan

Users with `plan: admin` receive Pro access.

## Legacy Premium Plan

Users with `plan: premium` remain Pro even when the source predates the current entitlement system.

## Organization Grant

An athlete may receive Pro through active membership in a Sentiq Org organization.

The grant is represented by an active EntitlementGrant with source `org_player_membership`.

Removing the organization grant does not downgrade the athlete if another valid Pro source remains.

## Benefit Grant

Rewards, unlock programs and campaigns may grant time-limited or permanent Pro access.

Campaign-based access is identified separately so Home can show the campaign card.

## Early Adopter

The mobile application also treats `isEarlyAdopter` as effective Pro access.

---

# Source Resolution

The backend entitlement resolver determines whether access is active and reports its source.

Current source priority:

1. Pilot plan;
2. Admin plan;
3. Paid subscription;
4. Organization grant;
5. Benefit grant;
6. Legacy premium plan.

The mobile application maps effective access to user-facing categories:

- subscription;
- admin grant;
- early adopter grant;
- pilot grant;
- organization grant;
- benefit grant.

Users should see that they have Pro and, when useful, why. They should not need to understand the internal entitlement hierarchy.

---

# RevenueCat Purchase Flow

Purchases are available only when:

- RevenueCat is configured;
- the subscriptions feature flag is enabled;
- store offerings are available.

Flow:

1. Load RevenueCat offerings;
2. Select an available package;
3. Purchase through the native store;
4. Read active RevenueCat entitlement;
5. Poll the backend until effective Pro is reflected;
6. Synchronize mobile premium state;
7. Return to the previous screen.

The exact package identifiers and localized prices come from RevenueCat offerings, not hard-coded documentation.

---

# Restore and Manage

The purchase screen supports:

- restoring previous purchases;
- opening native subscription management;
- retrying offering loading;
- synchronizing the backend when the store and server disagree.

The plan screen shows an out-of-sync warning only when reconciliation is required.

---

# Subscription Data

The Subscription model stores:

- user ID;
- platform (`ios` or `android`);
- entitlement;
- product ID;
- period type (`trial`, `intro`, `normal`);
- status (`active`, `inactive`);
- expiration;
- purchase date;
- RevenueCat customer ID;
- latest event such as renewal, cancellation, expiration or billing issue.

RevenueCat remains the source for store transaction state. The backend resolves that state together with non-commercial grants.

---

# Home Presentation Rules

Home prevents overlapping Pro messages.

Priority:

1. Campaign Pro card;
2. Unlock Pro program card;
3. Subscription upsell.

An upsell appears only for athletes without effective Pro access and only when subscriptions and RevenueCat are enabled.

Athletes who already have grant-based access see access-specific copy instead of a purchase prompt.

---

# Unlock Pro and Campaigns

## Unlock Pro

The Unlock Pro program tracks staged user activity and may grant Pro benefits.

Home can show:

- current program progress;
- stage completion celebrations;
- closure flow when the program ends.

## Campaigns

Partner campaigns may:

- use campaign codes;
- enforce capacity;
- grant Pro access;
- show a campaign-specific Home card;
- track participation and completion.

Campaign and reward access is resolved through benefit grants, not through RevenueCat purchases.

---

# Product Rules

Sentiq Pro should:

- explain concrete athlete value;
- use the native store for paid subscriptions;
- preserve access when any valid entitlement source remains;
- make restore and synchronization available;
- keep purchase messaging separate from grant messaging;
- respect feature flags and store availability.

Sentiq Pro should not:

- claim that payment improves the athlete by itself;
- hide the source of grant-based access when that context is useful;
- show multiple competing upsells;
- hard-code prices outside store offerings;
- revoke access while another valid entitlement exists.

---

# Success Criteria

Sentiq Pro is successful when athletes understand the additional value, can purchase or restore reliably, and always receive the access their effective entitlement grants.

---

# Related Documents

- [home.md](./home.md)
- [ai-coach.md](./ai-coach.md)
- [objectives.md](./objectives.md)
- [notifications.md](./notifications.md)
