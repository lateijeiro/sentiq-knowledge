# Sentiq B2C Onboarding

Version: 1.1

Status: Current product flow + Ideal activation journey (planned experience improvements)

---

# Purpose

This document defines the first experience of an athlete inside Sentiq.

The objective of onboarding is not collecting information for its own sake.

The objective is preparing the athlete so the first evaluation can create real value quickly.

---

# Onboarding Philosophy

The first minutes determine whether an athlete understands Sentiq.

Onboarding should create:

- Understanding
- Trust
- Curiosity
- Enough context to start reflecting

Onboarding should not become a long questionnaire or a homework session.

---

# Important Distinction

## Current Onboarding (shipped)

A short setup flow that collects essential profile and routine context.

## Ideal Activation Journey (strategy)

The broader first-session outcome: first evaluation + first objective + sense of personal value.

Today, first evaluation usually happens after onboarding, from Home, calendar, or notifications — not inside the onboarding screens.

---

# Current Onboarding Flow

Implemented in `sentiq-app/app/onboarding/`.

Users with `onboardingCompleto = false` are routed into onboarding before the main tabs.

```
Create Account / Login
   ↓
Legal acceptance (when required)
   ↓
Step 1 — Product introduction
   ↓
Step 2 — Role, sport, country
   ↓
Step 3 — General development objective
   ↓
Sports Mode selection
   ↓
Weekly schedule + notification preferences
   ↓
Home
```

### Step 1 — Introduction

Communicates what Sentiq is and why reflection matters.

### Step 2 — Athlete profile

Collects:

- Role (`jugador`, `coach`, `otro`)
- Sport
- Country

### Step 3 — General objective

Collects a high-level development focus (not yet a structured Objetivo entity).

### Sports Mode

Athlete chooses:

- `competencia`
- `construccion`
- `ajuste`
- `off`

### Schedule And Notifications

Athlete configures:

- Training week times
- Match week times (when relevant)
- Notification preferences for that mode

This enables retention reminders and preparation banners after onboarding.

---

# What Current Onboarding Does Not Include

Current onboarding does **not** force:

- First full evaluation
- First AI insight
- First Objetivo creation
- Meta Foco creation
- Coach invitation
- Social login

Those belong to activation after Home.

---

# Ideal First-Session Activation (Strategy)

After onboarding, the athlete should reach value quickly:

```
Arrive at Home
   ↓
Complete first evaluation (wizard or quick → expand)
   ↓
Create first objective (manual or Coach Sentiq if Pro)
   ↓
See that reflection can become action
   ↓
Return after next training / match
```

Activation metrics should track this journey, not only onboarding completion.

---

# Account Creation

Current:

- Email-based registration / login

Planned:

- Social login (if it reduces friction without harming trust)

Principles:

- Minimum required fields
- No long forms before value

---

# Progressive Profiling

Not everything needs to be collected during onboarding.

Can be collected later when it improves the experience:

- Position
- Competition level
- Deeper goals
- Coach / club relationships

---

# Empty States

New users should never feel the app is empty.

Prefer:

"Your development journey starts today."

Over:

"No evaluations yet."

Home, calendar and notifications should make the next action obvious.

---

# Activation Metrics (Current)

Track:

- Registration completion
- Onboarding completion
- Time to first evaluation
- First objective creation
- Day 1 / Day 7 return

---

# Common Failures To Avoid

- Too much explanation before any action
- Too many questions
- Showing Pro features before core value
- Treating onboarding completion as product success
- Assuming first evaluation happens inside onboarding

---

# Planned Improvements

Possible evolution:

- Stronger guided first evaluation after schedule setup
- Personalized first challenge
- Clearer empty-state CTA from Home
- Club-based onboarding paths
- Sport-specific journeys

---

# Final Principle

Move the athlete from curiosity to commitment.

Onboarding prepares the routine.

The first evaluation starts the relationship.
