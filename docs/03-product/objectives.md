# Sentiq Objectives System

Version: 1.0

---

# Purpose

Objectives transform reflection into action.

An athlete can understand themselves better through reflection, but improvement requires intentional behavior.

Objectives create the bridge between:

"I know what I want to improve"

and

"I am actively working on it."

---

# Objective Philosophy

Sentiq does not believe in generic goals.

Many athletes have ambitions:

"I want to improve."

"I want to become stronger."

"I want to be more confident."

These intentions are valuable but difficult to execute.

Sentiq converts intentions into specific development actions.

---

# What a Sentiq Objective Is

A specific behavior or development focus that an athlete intentionally practices over time.

Good objectives define a behavior:

"After making a mistake, I will focus on the next action instead of thinking about the previous one."

Bad objectives are abstract:

"Become mentally stronger."

---

# Objective Categories

Objectives are classified into five development categories:

| Category | Key | Description |
|----------|-----|-------------|
| Emotional | emocional | Emotional regulation, awareness, response |
| Mental | mental | Mindset, focus, mental preparation |
| Behavioral | comportamental | Actions, habits, conduct during events |
| Technical | tecnico | Sport-specific skills and technique |
| Reflective | reflexivo | Self-evaluation, learning, awareness |

Categories are assigned during creation (Step 3 of evaluation wizard) or inferred by Coach Sentiq AI suggestions.

---

# Priority

Every objective has a priority level:

| Priority | Key | Usage |
|----------|-----|-------|
| Low | low | Secondary focus |
| Medium | mid | Default priority |
| High | high | Primary focus |

Priority is visible in the Objectives tab and filterable for pending objectives.

---

# Origin

Objectives are created from three sources:

| Origin | Key | Description |
|--------|-----|-------------|
| Manual | manual | Created by the athlete during evaluation or manually |
| Coach Sentiq | auto | AI-generated suggestion accepted by the athlete |
| Follower | invitado | Suggested by a follower through Red |

The origin is stored and displayed in the Objectives tab.

---

# Objective States

Objectives use a result-based state model, not a lifecycle state machine.

| State | Condition | Description |
|-------|-----------|-------------|
| Pending | cumplido = null | Active, awaiting review in next evaluation |
| Achieved | cumplido = true | Marked as achieved during evaluation review |
| Not achieved | cumplido = false | Marked as not achieved during evaluation review |

The activo flag controls whether an objective appears in future evaluation reviews:

- cumplido = true → activo = false (removed from pending list)
- cumplido = false → activo = true (remains pending for next evaluation)

There is no "Suggested", "Archived" or "Active" state as a formal enum. "Suggested" is an origin (auto/invitado), not a state.

---

# Objective Lifecycle

The real flow in the product:

```
Create (during evaluation Step 3, or via follower suggestion)
   ↓
Practice (between evaluations)
   ↓
Review (during evaluation Step 4 — mark achieved / not achieved)
   ↓
Comment (optional completion comment)
   ↓
Continue (if still pending) or Close (if achieved)
```

Objectives are not edited after creation. They are evaluated and closed.

There is no formal modify, replace or archive action in the current product.

---

# Creation During Evaluation

Step 3 of the full evaluation wizard:

1. Athlete writes objectives manually or accepts Coach Sentiq AI suggestions
2. Sets priority (low, mid, high)
3. Optionally aligns to active Meta Foco
4. Objectives are saved linked to the evaluation (creadoEn)

Coach Sentiq suggestions consider:

- Current evaluation dimensions and dominant emotion
- hechoBien and mejorar reflection fields
- Active Meta Foco context
- Athlete sport, role and country
- Recent AI suggestions (to avoid repetition)

AI objective suggestions require Pro feature (FEATURES.IA_OBJETIVOS).

---

# Review During Evaluation

Step 4 of the full evaluation wizard:

1. Prior objectives from the last event of the same type are shown first
2. Other pending objectives are shown separately
3. Athlete marks each as achieved or not achieved
4. Optional completion comment per objective
5. Results are saved and feed IEA O and M components

Objectives can also be reviewed outside evaluations via the Objectives tab popup (PopupEditarObjetivo).

---

# Meta Foco Alignment

When the athlete has an active Meta Foco, objectives can be explicitly aligned:

- alinearMetaFoco toggle during Step 3
- extra.alineacionDeclarada: 'meta' or 'independiente'
- metaFocoId links to the active Meta Foco

Meta Foco alignment affects the M component of IEA:

M = (achieved objectives aligned to Meta Foco / total achieved objectives) × 100

---

# Follower-Suggested Objectives

Through Red, followers can suggest objectives to athletes they follow:

- Origin: invitado
- sugeridoPor stores follower identity (userId, nombre, apellido, displayName)
- Athlete can accept and create the objective during their next evaluation
- If the athlete has a public Meta Foco, the follower can suggest alignment

---

# Objectives Tab

The Objectives tab (Objetivos) in sentiq-app provides:

- Filter by state: pending, achieved, not achieved
- Priority filter for pending objectives (all, low, mid, high)
- Paginated history with summary counts
- Origin badge (Coach Sentiq, follower, manual)
- Completion comments on reviewed objectives
- Edit popup to mark achieved/not achieved with comment

---

# Data Model

Key fields on the Objetivo model:

```
userId, texto, categoria, prioridad, activo, cumplido,
fechaCumplimiento, comentario, creadoEn (Evaluacion ref),
evaluadoEn (Evaluacion ref), origen, metaFocoId,
sugeridoPor { userId, nombre, apellido, displayName },
extra { alineacionDeclarada: 'meta' | 'independiente' }
```

---

# Relationship With Evaluations

Evaluations answer: "What happened?"

Objectives answer: "What am I going to improve?"

Together: Reflection + Action = Development

- Created in evaluation Step 3
- Reviewed in evaluation Step 4
- Quick evaluations do not create objectives until expanded to full

---

# Relationship With IEA

Objectives contribute to two IEA components:

- **O (Objective Completion)**: (achieved / evaluated) × 100 during the period
- **M (Meta Foco Alignment)**: (achieved aligned to Meta Foco / total achieved) × 100

Minimum 1 evaluated objective required for IEA eligibility.

---

# Objective Anti-Patterns

Avoid:

- Too many objectives at once
- Generic goals without specific behavior
- Unrealistic expectations
- External pressure
- Competitive comparison
- Objectives without action

---

# Final Principle

An objective is not a destination.

It is a direction.

Sentiq helps athletes choose where to grow and build consistency through repeated evaluation and review.
