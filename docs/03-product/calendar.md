# Sentiq Evaluation Calendar

Version: 1.0

---

# Purpose

The calendar gives athletes a monthly view of their evaluation history and a direct entry point for reflection.

It helps the athlete answer:

- When did I reflect?
- Was the event a match or training session?
- What happened on a specific date?
- Do I want to add an evaluation for today or a past date?

---

# Product Location

The calendar is a component inside Home.

It is not a separate tab or standalone calendar product.

Implementation:

- `sentiq-app/components/CalendarioMensual.tsx`
- Rendered by `sentiq-app/app/(tabs)/home.tsx`

---

# Data Source

The calendar loads the authenticated athlete's evaluations from:

`GET /api/evaluaciones?userId={userId}`

The user ID is provided by Home when available. AsyncStorage is used only as a fallback.

---

# Filters

Home controls the calendar with three filters:

- `todos`: all evaluations;
- `partido`: match evaluations only;
- `entrenamiento`: training evaluations only.

The selected filter affects date markings only.

Evaluations of type `otro` appear under `todos`.

---

# Date Markings

Each date with an evaluation is marked according to event type:

- Match (`partido`): blue;
- Training (`entrenamiento`): red;
- Other (`otro`): gray.

The calendar uses the evaluation's local day through `evaluacionDiaCalendario`.

This prevents timezone conversion from moving an evaluation to the wrong calendar day.

---

# Day Selection Behavior

## Date With an Existing Evaluation

Selecting a marked date opens the event detail:

`/evento/[fecha]/tabs`

The route receives:

- `fecha`: selected local date;
- `from: home`.

The event detail provides access to summary, reflection and objectives for that date.

---

## Current or Past Date Without an Evaluation

If the selected date is not in the future:

- In normal sports modes, the full evaluation wizard opens at Step 0 with the date preselected.
- In `off` mode, the quick evaluation opens with `modo=off` and the date preselected.

This makes the calendar both a history view and a reflection entry point.

---

## Future Date Without an Evaluation

The athlete cannot create an evaluation on an empty future date.

The calendar displays a “Not allowed” message.

Upcoming match preparation is handled by the athlete's schedule and preparation system, not by manually creating future evaluations.

---

# Selected Date

Selecting a date applies a temporary visual emphasis.

The selection does not modify the evaluation or create data by itself.

---

# Refresh Behavior

The calendar reloads evaluations:

- when mounted;
- when the filter changes;
- when Home increments `refreshSeq` through pull-to-refresh;
- when the Home tab regains focus;
- when the user ID changes.

Transient network failures use a limited retry with backend warm-up and short backoff.

---

# Relationship With Evaluations

The calendar displays evaluation dates; it does not maintain a separate event model.

Its source of truth is the `Evaluacion` collection.

Both full and quick evaluations can appear because both are stored as Evaluacion records.

The calendar must use local-day fields when available:

- `diaLocal`;
- evaluation timezone;
- fallback event date normalization.

See [evaluations.md](./evaluations.md).

---

# Relationship With Sports Modes

The active sports mode changes creation behavior:

- `competencia`, `construccion`, `ajuste`: open the full evaluation wizard;
- `off`: open the off-mode quick evaluation.

The calendar does not configure sports modes or schedules.

---

# Product Rules

The calendar should:

- represent the athlete's real evaluation history;
- preserve local dates across timezones;
- make past reflection easy to revisit;
- allow low-friction evaluation creation;
- visually distinguish event types without ranking them.

The calendar should not:

- act as a match scheduling tool;
- allow empty future evaluations;
- imply that a marked date represents an external sports event without an evaluation;
- compare athletes or evaluation frequency publicly.

---

# Success Criteria

The calendar is successful when an athlete can understand their reflection consistency at a glance and open or create the relevant evaluation with one action.

---

# Related Documents

- [home.md](./home.md)
- [evaluations.md](./evaluations.md)
- [notifications.md](./notifications.md)
