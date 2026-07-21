# Sentiq Home

Version: 1.0

---

# Purpose

Home is the athlete's contextual starting point in sentiq-app.

It does not represent a single workflow. It brings together the most relevant next actions, current development context, recent learning and evaluation history.

Home should help the athlete answer:

- What needs my attention now?
- What am I currently working toward?
- What have I done recently?
- What is my next useful action?

---

# Location

Home is the first tab in the authenticated mobile application.

Implementation:

- `sentiq-app/app/(tabs)/home.tsx`
- Route: `/(tabs)/home`

Users who have not accepted the current legal terms are redirected to the legal acceptance flow before Home is displayed.

---

# Main Surfaces

## Pending Invitations

If the athlete has pending Red invitations, Home shows a banner with the invitation count.

Selecting the banner opens:

`/siguiendo?tab=invitaciones`

The banner is hidden when there are no pending invitations.

---

## Match Preparation

Home loads the athlete's next scheduled match.

A preparation banner is shown only when:

- an upcoming match exists;
- the match is inside the relevant time window;
- the corresponding notification preference is enabled.

Supported windows:

- approximately 24 hours before the match;
- approximately 90 minutes before the match.

Selecting the banner opens the Preparation screen with the relevant context.

The banner uses the athlete's configured schedule and sports mode.

---

## Welcome and Introduction

Home includes:

- `WelcomeCard`;
- `IntroCard`.

These components provide orientation and contextual onboarding without replacing the main development flows.

---

## Meta Foco

`MetaFocoCard` displays the athlete's current medium-term development direction.

From Home, the athlete can understand:

- the active Meta Foco;
- its time horizon;
- its ambition level;
- how current objectives connect to it.

Meta Foco is not a daily objective. It is the higher-level direction above individual objectives.

---

## Learning Cards

Athletes with effective Sentiq Pro access may see:

- the current weekly Learning Card;
- the current monthly Learning Card.

Cards are loaded independently. A card is displayed only when it exists and has finished loading.

Learning Cards summarize patterns from the athlete's development history. They are not courses or a formal curriculum.

---

## Evaluation Filters

Home provides three filters for the calendar:

- All (`todos`);
- Match (`partido`);
- Training (`entrenamiento`).

The filter changes which evaluation dates are marked in the calendar.

It does not modify or delete evaluation data.

---

## Evaluation Calendar

`CalendarioMensual` is embedded directly in Home.

It provides:

- month navigation;
- marked dates for existing evaluations;
- event-type visual distinction;
- navigation to event detail;
- creation of evaluations from past or current empty dates.

See [calendar.md](./calendar.md).

---

## Red Invitation CTA

If the athlete has no followers, Home shows an invitation call to action.

The CTA encourages the athlete to build their Red support network.

It is hidden when followers already exist or follower state is still loading.

---

## Pro Access Cards

Home may show one of several commercial or benefit cards:

- Campaign Pro card for campaign-based access;
- Unlock Pro program progress card;
- Sentiq Pro subscription upsell.

Priority rules prevent competing cards from appearing together:

1. Campaign Pro card;
2. Unlock Pro program card;
3. Subscription upsell.

The subscription upsell appears only when:

- the athlete does not already have effective Pro access;
- no campaign or unlock card takes priority;
- subscriptions are enabled;
- RevenueCat is available.

See [premium.md](./premium.md).

---

# Refresh Behavior

Pull-to-refresh reloads:

- follower state;
- pending Red invitations;
- calendar evaluations;
- next scheduled match.

Transient network errors do not remove known follower state. Home shows a retry-oriented snackbar instead.

---

# Personalization Inputs

Home behavior depends on:

- authenticated user;
- legal acceptance state;
- active sports mode;
- training and match schedule;
- notification preferences;
- effective Pro access;
- campaign or benefit grants;
- Red follower and invitation state;
- available Learning Cards;
- active Meta Foco;
- existing evaluations.

---

# Product Rules

Home should:

- prioritize context over generic content;
- show only actions relevant to the athlete now;
- avoid presenting multiple competing commercial messages;
- keep evaluation history immediately accessible;
- respect Pro entitlements and notification preferences;
- remain useful when optional content is unavailable.

Home should not:

- become an analytics dashboard;
- duplicate the Progress tab;
- expose internal entitlement logic;
- show preparation outside the configured time windows;
- imply that Learning Cards are a conversational AI experience.

---

# Success Criteria

Home is successful when the athlete can quickly identify the most relevant next action and continue their development journey with minimal navigation.

---

# Related Documents

- [calendar.md](./calendar.md)
- [evaluations.md](./evaluations.md)
- [objectives.md](./objectives.md)
- [ai-coach.md](./ai-coach.md)
- [premium.md](./premium.md)
- [notifications.md](./notifications.md)
