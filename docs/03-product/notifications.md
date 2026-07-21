# Sentiq Notifications

Version: 1.0

---

# Purpose

Notifications help athletes reflect and prepare at useful moments tied to their real sports schedule.

They are reminders, not the reason athletes return.

Notifications should reduce the gap between an event and meaningful reflection without creating pressure, guilt or artificial urgency.

---

# System Overview

Sentiq uses two complementary notification mechanisms:

- local scheduled notifications in sentiq-app;
- backend push fallback reminders through Expo push tokens.

Notifications navigate to the relevant product flow through deep links.

---

# User Schedule

Notification timing is based on the athlete's configured weekly schedule:

- timezone;
- training times by weekday;
- match times by weekday.

The schedule changes with Sports Modes:

- `competencia`;
- `construccion`;
- `ajuste`;
- `off`.

Timezone-aware scheduling is required so reminders occur at the athlete's local time.

---

# Notification Preferences

The User model stores:

- `postTrainingEval`: reminder after training;
- `postMatchEval`: reminder after a match;
- `postOffsetMin`: local post-event offset, 15 or 30 minutes;
- `preMatch24h`: preparation reminder approximately 24 hours before a match;
- `preMatch90m`: preparation reminder approximately 90 minutes before a match;
- `offDailyEnabled`: daily off-mode reminder.

Default values enable all reminders with a 15-minute post-event offset.

The notification settings shown to the athlete depend on the selected Sports Mode.

---

# Mode-Specific Behavior

## Competition Mode

Available preferences:

- post-training evaluation;
- post-match evaluation;
- 24-hour pre-match preparation;
- 90-minute pre-match preparation;
- post-event offset of 15 or 30 minutes.

## Construction Mode

Available preferences:

- post-training evaluation;
- post-event offset.

Match and pre-match reminders are disabled.

## Adjustment Mode

Post-training reminders are available only if the athlete indicates that they are still training.

Match and pre-match reminders are disabled.

## Off Mode

Regular training and match reminders are cancelled.

A daily local reminder is scheduled at 20:30 when `offDailyEnabled` is not false.

The reminder opens the off-mode quick evaluation.

---

# Local Notification Types

## Post-Training Evaluation

Scheduled from the weekly training schedule.

Timing:

- 15 or 30 minutes after training, according to preference.

Deep link:

`/evaluacion-wizard/step0?kind=training`

## Post-Match Evaluation

Scheduled from the weekly match schedule.

Timing:

- 15 or 30 minutes after the match, according to preference.

Deep link:

`/evaluacion-wizard/step0?kind=match`

## Pre-Match 24 Hours

Scheduled approximately 24 hours before a match.

Deep link:

`/preparacion`

## Pre-Match 90 Minutes

Scheduled approximately 90 minutes before a match.

Deep link:

`/preparacion`

## Off-Mode Daily Reminder

Scheduled daily at 20:30 local time.

Deep link:

`/evaluacion/rapida?modo=off`

---

# Backend Fallback Reminders

The backend provides fallback post-event push reminders for athletes with active Expo push tokens.

Fallback stages:

- `post3h`: a quick check-in approximately three hours after the scheduled event;
- `nextDay`: another reminder the following day.

Fallback reminders:

- apply to training and matches according to preferences;
- open the quick evaluation;
- include the local event date and event type;
- are skipped when an evaluation already exists for the local date;
- are deduplicated through NotificationDispatch;
- are skipped when no active token exists.

Deep link:

`/evaluacion/rapida`

The local 15/30-minute reminder is the primary experience. Backend reminders provide resilience when the local reminder did not result in an evaluation.

---

# Push Token Lifecycle

Authenticated devices register Expo push tokens through:

`POST /api/notifications/register-token`

Registration stores:

- token;
- platform;
- timezone;
- app version;
- language;
- active state;
- last-seen timestamp.

Tokens are deactivated through:

`POST /api/notifications/unregister-token`

Multiple device tokens may exist for one user.

---

# Notification Navigation

Notification payloads support:

- `action`;
- `screen`;
- nested or JSON-stringified `params`.

Only payloads with no action or `action: open` navigate.

The provider:

1. normalizes parameters to strings;
2. separates query parameters embedded in `screen`;
3. merges explicit params;
4. pushes the resulting Expo Router route;
5. falls back to `/` if navigation fails.

Duplicate notification responses are ignored through a deduplication key.

---

# Scheduling Lifecycle

When the athlete saves a Sports Mode or schedule:

1. mode, schedule and preferences are saved to the profile;
2. existing local Sentiq notification IDs are cancelled;
3. the provider schedules only reminders relevant to the new mode;
4. Home becomes the destination after saving.

Only notification IDs created by Sentiq are persisted and cancelled by this flow.

---

# Platform Behavior

The application:

- creates an Android notification channel;
- requests notification permission when required;
- handles exact-alarm requirements on supported Android versions;
- registers the current push token with the backend;
- listens for notification responses;
- respects safe navigation initialization.

Notification delivery can still depend on operating-system permissions and scheduling behavior.

---

# Copy Principles

Notification copy should:

- sound like a calm coach;
- connect directly to training, matches or reflection;
- state a useful next action;
- use the athlete's language when available.

Notification copy should not:

- shame the athlete for missing an evaluation;
- suggest diagnosis;
- create fake urgency;
- compare the athlete with others;
- imply that notification engagement equals development.

---

# Relationship With Home

Home mirrors pre-match timing with a Preparation banner.

The banner respects `preMatch24h` and `preMatch90m` preferences.

Opening a notification and opening the matching Home banner should lead to the same preparation experience.

---

# Relationship With Evaluations

Post-event notifications lead to:

- the full evaluation wizard for primary local reminders;
- the quick evaluation for backend fallback reminders and off mode.

Backend fallback checks the athlete's local date before sending so an existing evaluation suppresses the reminder.

See [evaluations.md](./evaluations.md).

---

# Product Rules

Notifications should:

- be tied to configured sports activity;
- respect athlete preferences and active mode;
- use local timezone;
- deep-link to a useful action;
- avoid duplicate backend sends;
- stop when an evaluation already exists where applicable.

Notifications should not:

- replace intrinsic product value;
- maximize opens as an end in itself;
- send irrelevant match reminders outside Competition mode;
- pressure athletes after missed activity;
- expose implementation details in user-facing copy.

---

# Success Criteria

Notifications are successful when they help athletes reflect or prepare at the right moment without becoming noise or pressure.

---

# Related Documents

- [home.md](./home.md)
- [calendar.md](./calendar.md)
- [evaluations.md](./evaluations.md)
- [ai-coach.md](./ai-coach.md)
- [premium.md](./premium.md)
