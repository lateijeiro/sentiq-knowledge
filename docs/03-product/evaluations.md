# Sentiq Evaluations

Version: 1.0

---

# Purpose

Evaluations are the core interaction mechanism inside Sentiq.

They create the habit of reflection that enables athlete development.

The purpose of an evaluation is not to collect information.

The purpose is to help the athlete stop, reflect and understand themselves better.

---

# Evaluation Philosophy

Traditional sports measurement focuses on external performance:

- Speed
- Strength
- Statistics
- Results

Sentiq focuses on internal development:

- Awareness
- Focus
- Confidence
- Learning
- Consistency

An evaluation should create clarity.

Not judgment.

---

# The Evaluation Loop

Every evaluation follows this cycle:

```
Experience (Training / Match / Other)
   ↓
Reflection (Dimensions + hechoBien / mejorar)
   ↓
Understanding
   ↓
Action (New Objectives)
   ↓
Review (Prior Objectives)
   ↓
Improvement
   ↓
New Experience
```

---

# Evaluation Types

Sentiq supports two evaluation modes.

---

## 1. Full Evaluation Wizard

A structured multi-step flow after training, matches or other events.

### Steps

**Step 0 — Context**

- Event type: partido (match), entrenamiento (training), otro (other)
- Date
- Dominant emotion
- Privacy toggle (public / private to followers)

**Step 1 — Five Dimensions (Likert 1–5)**

Each dimension is rated on a 1–5 scale:

- estadoEmocional (emotional state)
- estadoFisico (physical state)
- estadoEnfoque (focus)
- estadoPresion (pressure handling)
- estadoRendimiento (performance)

All five dimensions must be complete for the evaluation to count as valid for IEA.

**Step 2 — Reflection**

- hechoBien: what went well (multiple text entries)
- mejorar: what can be improved (multiple text entries)

**Step 3 — New Objectives**

- Create one or more objectives manually or via Coach Sentiq AI suggestions
- Set priority (low, mid, high) and category
- Optionally align to active Meta Foco

**Step 4 — Review Prior Objectives**

- Mark pending objectives as achieved or not achieved
- Add completion comments
- Objectives from the previous event and other pending objectives are shown separately

**Step 5 — Completion Summary**

- Average dimension score
- New objectives count
- Prior objectives achieved / not achieved
- Meta Foco alignment count
- Streak and badges

### Use Case

After training sessions, matches, or important moments.

---

## 2. Quick Evaluation

A lightweight check-in when time is limited.

### Inputs

- Event type
- Date
- Dominant emotion
- General state (1–5)
- Optional quick comment
- Off-mode: trained today (yes/no), training type (gym, cardio, tecnica, recuperacion, otro)

### Behavior

- Saves immediately as tipoEvaluacion: rapida
- Can later be expanded into a full evaluation (origenEvaluacion: desde_rapida)
- Does not include five dimensions, reflection, or objectives until expanded

### Use Case

Post-training or post-match when the athlete has limited time.

Off-mode daily reminders link directly to quick evaluation.

---

# Event Types

| Type | Key | Description |
|------|-----|-------------|
| Match | partido | Competition event |
| Training | entrenamiento | Training session |
| Other | otro | Any other sporting moment |

---

# Five Dimensions

The five Likert dimensions are the foundation of the Integral State (E) component in IEA.

| Dimension | Field | Scale |
|-----------|-------|-------|
| Emotional | estadoEmocional | 1–5 |
| Physical | estadoFisico | 1–5 |
| Focus | estadoEnfoque | 1–5 |
| Pressure | estadoPresion | 1–5 |
| Performance | estadoRendimiento | 1–5 |

Only evaluations with all five dimensions complete (values 1–5) are considered valid for IEA calculation.

---

# Reflection Fields

Reflection is embedded in the evaluation, not a standalone journal.

- **hechoBien**: array of strings — what the athlete did well
- **mejorar**: array of strings — what the athlete can improve

These fields feed Coach Sentiq AI context for objective suggestions and reports.

---

# Privacy

Each evaluation has a privado (private) flag.

- Private evaluations are hidden from followers in Red
- Public evaluations appear in the follower feed
- Privacy is set per evaluation during Step 0

---

# Sports Modes Context

Evaluations are influenced by the athlete's active sports mode:

| Mode | Key | Evaluation Context |
|------|-----|--------------------|
| Competition | competencia | Match schedules drive pre-match preparation and post-match evaluations |
| Construction | construccion | Training-focused evaluations |
| Adjustment | ajuste | Reduced schedule, optional training |
| Off | off | Daily quick evaluation reminders, off-mode training types |

The active mode (modoActivo) is stored on each evaluation.

---

# Notifications

Evaluations are triggered by push notifications based on sports mode:

- Post-training evaluation reminder
- Post-match evaluation reminder
- 24-hour pre-match preparation banner
- 90-minute pre-match preparation banner
- Off-mode daily reminder (links to quick evaluation)

Notification preferences are configurable per mode in the app.

---

# Browsing and History

The Evaluations tab allows athletes to:

- Browse evaluations by month
- Filter by event type and dominant emotion
- View evaluation detail with summary, reflection and linked objectives
- Navigate to event detail pages with full reflection history

---

# Data Model

Key fields on the Evaluacion model:

```
userId, tipoEvento, modoActivo, tipoEvaluacion, origenEvaluacion,
fecha, tz, diaLocal, mesLocal, emocionDominante,
estadoEmocional, estadoFisico, estadoEnfoque, estadoPresion, estadoRendimiento,
privado, hechoBien[], mejorar[],
nuevosObjetivos[] (refs to Objetivo),
evaluacionPrevios[] (id, texto, cumplido, comentario, evaluadoEn)
```

---

# Relationship With Objectives

Evaluations are the primary creation point for objectives.

- Step 3 creates new objectives linked to the evaluation (creadoEn)
- Step 4 reviews prior objectives (evaluacionPrevios)
- Objective completion during Step 4 feeds the O and M components of IEA

---

# Relationship With IEA

Evaluations are the primary input for IEA:

- **E (Integral State)**: average of five Likert dimensions across valid evaluations
- **C (Consistency)**: ratio of evaluations vs progressive expectation
- Minimum 3 valid evaluations required for IEA eligibility

---

# Relationship With Coach Sentiq

Evaluation data feeds AI outputs:

- Step 3: AI objective suggestions use current evaluation dimensions, reflection, Meta Foco, and recent history
- Monthly report: aggregates evaluation trends
- Learning Cards: generated from evaluation patterns

---

# Evaluation Anti-Patterns

Avoid:

- Long questionnaires
- Clinical language
- Negative scoring
- Comparing athletes
- Excessive notifications
- Making athletes feel evaluated

---

# Final Principle

An evaluation should never feel like a test.

It should feel like a conversation with yourself.

The athlete is not being measured.

The athlete is learning.
