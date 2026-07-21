# Sentiq Product

Version: 1.0

---

# Product Overview

Sentiq is an athlete development platform designed to help young athletes improve through reflection, objective setting, continuous learning and personal growth.

The product transforms personal development into a structured habit tied to training and competition.

The athlete does not use Sentiq to complete tasks.

The athlete uses Sentiq to understand themselves better and improve over time.

---

# Product Philosophy

Most sports platforms focus on external performance.

They measure:

- Speed
- Strength
- Statistics
- Results
- Physical performance

Sentiq focuses on internal development.

It helps athletes understand:

- How they feel
- How they think
- What they want to improve
- How they are progressing

The objective is not to replace traditional sports training.

The objective is to complete it.

---

# The Sentiq Development Loop

The entire product is built around a continuous improvement cycle.

```
Reflect (Evaluation)
   ↓
Understand (Dimensions + Reflection)
   ↓
Set Objective
   ↓
Practice
   ↓
Review Progress (IEA + Trends)
   ↓
Improve
   ↓
Reflect Again
```

Every product decision should strengthen this loop.

---

# Core Product Systems

Sentiq is composed of interconnected systems across the athlete app and organization portal.

---

# 1. Evaluation System

## Purpose

Create awareness through event-based reflection.

## What it provides

- Full evaluation wizard after training, matches or other events
- Quick evaluation for lightweight check-ins
- Five 1–5 Likert dimensions: emotional, physical, focus, pressure, performance
- Dominant emotion selection
- Reflection fields: what went well, what to improve
- Privacy control per evaluation
- Historical browsing by month, event type and emotion
- Expand quick evaluation into full evaluation

## Product Goal

Create the habit: "Take a moment to understand yourself after every event."

See: [evaluations.md](./evaluations.md)

---

# 2. Objective System

## Purpose

Transform awareness into action.

## What it provides

- Objectives created during evaluations or manually
- AI-generated objective suggestions (Coach Sentiq)
- Follower-suggested objectives (Red)
- Priority levels: low, mid, high
- Categories: emocional, mental, comportamental, tecnico, reflexivo
- States: pending, achieved, not achieved
- Completion comments
- Meta Foco alignment
- Re-evaluation of prior objectives during next evaluation

## Product Goal

Help athletes answer: "What am I working on?"

See: [objectives.md](./objectives.md)

---

# 3. Meta Foco

## Purpose

Provide a medium-term development direction above individual objectives.

## What it provides

- Title, description, time horizon, success criteria
- Ambition level: suave, realista, desafiante
- States: activa, pausada, cerrada
- Optional public visibility to followers
- Objectives can align explicitly to Meta Foco
- Meta Foco alignment feeds the M component of IEA

## Product Goal

Help athletes answer: "Where am I heading?"

---

# 4. Progress System

## Purpose

Make growth visible.

## What it provides

- KPI summary (average score, objective completion rate, streak)
- Emotional trend and warning classification
- Dimension averages over time
- Evaluations and objectives by week
- IEA (Índice de Evolución del Atleta)
- Monthly AI report (Coach Sentiq)
- Learning Cards tab (Pro): weekly and monthly AI-generated insights

## Product Goal

Help athletes see: "I am improving."

See: [iea.md](./iea.md)

---

# 5. Coach Sentiq (AI)

## Purpose

Provide personalized guidance through AI-generated outputs.

## What it provides

- Objective suggestions during post-event reflection
- Weekly preparation message before matches
- Monthly personal development report
- Weekly and monthly Learning Cards

Coach Sentiq is not a conversational chatbot. It generates specific outputs at defined moments.

## Product Goal

Provide guidance without replacing human relationships.

See: [ai-coach.md](./ai-coach.md)

---

# 6. Red (Follower Network)

## Purpose

Enable peer support and shared development.

## What it provides

- Search and invite followers by user or email
- Accept/reject follow invitations
- Feed of follower evaluations and shared Learning Cards
- Public profile with evaluation history
- Suggest objectives to followed users
- Per-evaluation privacy control

Red is a follower network, not a comparison leaderboard.

---

# 7. Sports Modes

## Purpose

Adapt the product experience to the athlete's current competitive context.

## Modes

- **competencia**: competition season — match schedules, pre-match preparation, post-match evaluations
- **construccion**: building phase — training-focused notifications and evaluations
- **ajuste**: adjustment phase — reduced training schedule
- **off**: off-season — daily reminders, optional off-mode training types

Modes affect notification timing, preparation banners, and AI context.

---

# 8. Organization Experience (Sentiq Org)

## Purpose

Allow clubs and academies to implement athlete development at scale.

## What it provides

- **Pulse Dashboard**: evaluation volume, emotional climate, objective completion, IEA readiness, attention alerts, trends
- **Players**: directory, invitations (single, bulk, CSV), individual profiles with evaluation history, IEA and insights
- **Staff & Structure**: roles (club_admin, coach, staff, player), rosters, teams, assignments
- **Reports**: weekly briefing, adoption ROI
- **Settings & Billing**: organization profile, limits, plan usage, invoice history

## Product Goal

Help organizations build stronger development cultures.

---

# Primary User Journey

## Step 1 — Discover Sentiq

The athlete understands: "This can help me improve."

## Step 2 — Complete first evaluation

The athlete experiences value quickly through reflection and AI objective suggestions.

## Step 3 — Identify development areas

The athlete gains awareness through dimensions and reflection.

## Step 4 — Create objectives

The athlete chooses what to improve, optionally aligned to Meta Foco.

## Step 5 — Build consistency

The athlete returns regularly after training and matches.

## Step 6 — See progress

The athlete recognizes growth through IEA, trends and streaks.

---

# Product North Star

The North Star metric is not downloads.

It is meaningful athlete development.

The strongest signal of product success is:

An athlete consistently returning because Sentiq helps them improve.

---

# Activation Moment

The athlete should experience the first value as quickly as possible.

The ideal activation moment:

After completing their first evaluation and receiving meaningful guidance.

The athlete should think:

"That was useful."

"I understand myself better."

"I want to continue."

---

# Retention Philosophy

Retention does not come from notifications.

Retention comes from value.

Users return because:

- They learn something about themselves.
- They see progress.
- They have objectives.
- They feel supported.
- They want to continue improving.

---

# What Sentiq Will Never Become

Sentiq will never become:

- A social network based on comparison or ranking.
- A leaderboard of emotional scores.
- A replacement for coaches.
- A diagnostic tool.
- A complex tracking system.
- A productivity app.
- A conversational AI chatbot.
- A parent-facing product.

---

# Product Success Criteria

A successful Sentiq experience means:

The athlete:

- Reflects regularly after events.
- Understands themselves better.
- Builds meaningful objectives.
- Recognizes progress through IEA and trends.

The coach:

- Has better conversations through Sentiq Org insights.
- Understands athletes better.
- Supports development more effectively.

The club:

- Builds a stronger development culture through organization tools.

---

# Related Documents

- [home.md](./home.md)
- [calendar.md](./calendar.md)
- [evaluations.md](./evaluations.md)
- [objectives.md](./objectives.md)
- [iea.md](./iea.md)
- [ai-coach.md](./ai-coach.md)
- [premium.md](./premium.md)
- [notifications.md](./notifications.md)
- [personas.md](./personas.md)
- [jobs-to-be-done.md](./jobs-to-be-done.md)
- [product-principles.md](./product-principles.md)
