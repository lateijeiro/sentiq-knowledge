# Sentiq IEA
# Índice de Evolución del Atleta

Version: 1.0

---

# Purpose

The Índice de Evolución del Atleta (IEA) is a development indicator designed to help athletes understand their own evolution process.

The IEA does not measure talent.

The IEA does not measure athletic ability.

The IEA does not rank athletes.

The IEA measures engagement with personal development.

---

# Core Philosophy

Traditional sports metrics answer:

"How good is this athlete?"

Sentiq answers a different question:

"Is this athlete actively developing?"

The IEA represents the second question.

---

# What The IEA Is

The IEA is:

- A personal evolution indicator (0–100)
- A reflection of development consistency
- A visualization of progress over time
- A motivation tool
- A conversation starter for coaches

---

# What The IEA Is Not

The IEA is not:

- A performance score
- A ranking between athletes
- A psychological evaluation
- A measure of potential
- A judgment of athlete value

---

# Formula

When the athlete meets eligibility requirements:

```
IEA = (E + O + C + M) / valid_components
```

Components with null values are excluded from the average, allowing partial IEA when some dimensions lack data.

---

# Components

## E — Integral State (0–100)

**Definition**: Normalized average of the five Likert dimensions across valid evaluations in the period.

**Formula per event**:

```
likertAvg = average(estadoEmocional, estadoFisico, estadoEnfoque, estadoPresion, estadoRendimiento)
E_event = (likertAvg / 5) × 100
```

**Period formula**: E = average(E_event) for all valid events in the range.

**Requirements**: Only events with all five Likert fields complete (values 1–5) are included. Incomplete events are excluded entirely.

---

## O — Objective Completion (0–100)

**Definition**: Percentage of objectives achieved vs evaluated in the period.

**Formula**:

```
O = (objetivosCumplidos / objetivosEvaluados) × 100
```

**Requirements**: Only objectives with explicit cumplido (true/false) count. If objetivosEvaluados = 0, then O = null.

---

## C — Consistency (0–100)

**Definition**: Ratio of evaluations performed vs progressive expectation based on user tenure.

**Requirements**: Only activated with 4 or more valid evaluations. If fewer than 4, C = null.

**Progressive expectation**:

| User tenure | Expected evaluations/week |
|-------------|--------------------------|
| Days 1–14 | 1 per week |
| Days 15–28 | 2 per week |
| Day 29+ | 3 per week |

**Formula**:

```
expectativaTotal = weeksInPeriod × expectativaPorSemana
C = min((evalsValidos / expectativaTotal) × 100, 100)
```

New users have reduced expectations. More evaluations than expected caps at 100.

---

## M — Meta Foco Alignment (0–100)

**Definition**: Percentage of achieved objectives that are aligned with the athlete's Meta Foco.

**Formula**:

```
M = (objetivosCumplidosAlineados / objetivosCumplidos) × 100
```

**Requirements**: Only counts objectives with cumplido = true. Alignment is determined by extra.alineacionDeclarada = 'meta'. If objetivosCumplidos = 0, then M = null.

This component measures: "Of what you achieved, how much was aligned with your main goal?"

---

# Eligibility Gate

The IEA is not shown until the athlete meets minimum data requirements:

- 3 or more valid evaluations (all five Likert dimensions complete)
- 1 or more evaluated objectives

If the gate is not met: status = "calibrando" (no numeric IEA).

If the gate is met: status = "ready" (numeric IEA displayed).

---

# Additional Metrics

## Confidence (0–100)

Indicates how reliable the calculated IEA is based on available data.

```
conf_evals = min(validEvents / 10, 1)
conf_obj = min(evaluatedObjectives / 5, 1)
confidence = round(100 × min(conf_evals, conf_obj))
```

Examples:

- 5 evals, 3 objectives → confidence = 50%
- 12 evals, 8 objectives → confidence = 100%

Displayed as a badge so the athlete knows if more data is needed.

---

## Tier

Qualitative classification of the IEA value:

| Range | Tier | Meaning |
|-------|------|---------|
| < 50 | base | Needs attention |
| 50–64 | en_progreso | In development |
| 65–79 | solido | Good progress |
| ≥ 80 | elite | Elite level |

Displayed as a colored badge in the Progress tab.

---

## Trend

Change in IEA points comparing the last 28 days vs the previous 28 days.

Positive trend = improving development engagement.

Negative trend = declining engagement (not declining ability).

---

# Where IEA Appears

## Athlete App (sentiq-app)

- Progress tab: IEACard with score, tier, confidence, trend, and breakdown
- Weekly series chart

## Organization Portal (sentiq-org)

- Pulse Dashboard: IEA readiness across roster/team
- Individual player profiles: IEA score, trend, and insights
- Scoped by organization, roster, team and date range

---

# Communication Rules

The IEA should always be explained.

Never show only a number.

Bad:

"Your IEA is 72."

Good:

"Your evolution indicator shows how consistently you are working on your development."

---

# Tone

The IEA should feel:

Encouraging.

Informative.

Personal.

Never:

Judgmental.

Competitive.

Clinical.

---

# IEA And Athletes

The athlete should think:

"I can influence this."

Not:

"This defines me."

---

# IEA And Coaches

The IEA can support conversations in Sentiq Org.

It should never replace coaching judgment.

A coach should never use the IEA as the only evaluation of a player.

Organizations may use aggregated evolution insights.

Never individual ranking.

---

# Privacy Principles

Athlete development information is personal.

Access in Sentiq Org follows organization membership permissions.

Athletes own their journey.

Private evaluations do not appear in follower feeds.

---

# Final Principle

The IEA is not a score that tells athletes who they are.

It is a mirror that helps them understand who they are becoming.
