# Sentiq Early Adopters Program

Version: 1.1

Status: Current program mechanics + community strategy

---

# Purpose

This document defines the strategy for Sentiq's first generation of athletes.

Early adopters are not only users.

They are:

- Product validators
- Community founders
- Feedback partners
- Future ambassadors

---

# Program Philosophy

The first users of Sentiq are part of building the product.

They should feel:

"I am helping build something that will improve athlete development."

---

# Strategic Objective

- Validate product value
- Understand athlete behavior
- Improve the experience
- Generate testimonials
- Build the first Sentiq community

---

# Current Product Mechanics

## Capacity

Early Adopter slots are capped at the first **100** users with `isEarlyAdopter = true`.

Public stats endpoint:

`GET /api/early-adopters/stats`

Returns:

- `maxSlots` (100)
- `slotsTaken`
- `slotsRemaining`
- `earlyAdoptersCount`
- `totalActiveUsers` (context only; does not define the cap)

## Access Granted

When assigned as early adopter, the athlete typically receives:

- `isEarlyAdopter: true`
- Plan treatment compatible with Pro access (`pilot` / lifetime recognition)
- Effective Sentiq Pro features (AI objectives, monthly report, Coach Sentiq, Learning Cards)

Early adopter Pro is protected so later subscription events should not erase that access.

## Auto-Assignment Flag

Repository / environment may gate automatic assignment with:

`EARLY_ADOPTER_AUTO_ASSIGN`

When disabled, the capacity and recognition model still exist, but new registrations are not automatically enrolled.

Treat production flag state as operational configuration, not a product redesign.

---

# Early Adopter Profile

Ideal early adopters:

- Competitive youth athletes
- Players interested in improvement
- Athletes open to new tools
- Community influencers inside clubs

Sentiq is open to early adopters from any sport.

Priority outreach sports: rugby, hockey, football and basketball.

---

# Early Adopter Benefits (Current + Relationship)

Current product benefits:

- Pro-level access during / after validation assignment
- Founder / early generation recognition (`isEarlyAdopter`)

Relationship benefits:

- Direct influence on product decisions
- Earlier access to improvements
- Closer communication with the founding team

---

# What Sentiq Expects

The relationship is not transactional.

Athletes provide:

- Honest feedback
- Real usage
- Suggestions
- Optional experience sharing

They should never feel used as unpaid testers or forgotten after launch.

---

# Communication Strategy

Prefer direct relationship over generic marketing.

Welcome message direction:

"You are one of the first athletes helping shape Sentiq."

---

# Feedback Loop

```
Athlete Experience
   ↓
Feedback
   ↓
Product Improvement
   ↓
Better Experience
   ↓
More Athletes
```

Learn about:

- Value: Does Sentiq help?
- Consistency: Do they return? Why?
- Friction: What blocks usage?
- Understanding: Is the purpose clear?
- Emotion: How does Sentiq feel?

---

# Success Metrics

## Activation

Registration, first evaluation, first objective.

## Engagement

WAU, evaluation frequency, objective usage.

## Retention

Day 7 / 30 / 90.

## Advocacy

Recommendations, testimonials, organic invites (Red / word of mouth).

## Program Health

Slots remaining of the first 100, quality of feedback, retention of early cohort vs later cohorts.

---

# Early Adopter To Ambassador Journey (Strategy)

```
User
   ↓
Engaged User
   ↓
Community Member
   ↓
Ambassador
   ↓
Sentiq Representative
```

Ambassadors may later:

- Share their journey
- Introduce teammates
- Participate in content
- Represent Sentiq at events

This ambassador layer is mostly relationship strategy today, not a fully productized rewards system.

---

# Relationship With Other B2C Mechanisms

| Mechanism | Relationship |
|-----------|--------------|
| Campaigns | Separate partner codes; not early-adopter slots |
| Unlock Pro | Activity-based Pro grant for broader users |
| Paid Pro | Store subscription; early adopters keep protected access |
| Referrals (planned) | Future athlete-to-athlete attribution |
| Red | Social network; early adopters can invite teammates to follow |

---

# Relationship With Clubs

Early adopters can bridge to organizations.

The athlete relationship remains independently valuable.

---

# Product Development Rule

Feedback influences UX, features, communication and priorities.

Not every request becomes a feature.

Product vision remains the guide.

---

# Final Principle

The first 100 athletes are not merely customers.

They are the people who prove that Sentiq deserves to exist.
