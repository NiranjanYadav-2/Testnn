# 05 — Feature Specifications

## 1. Onboarding
**Purpose:** help the user configure the app without forcing an account.
- Welcome and product explanation.
- Optional first goal.
- Choose rituals.
- Configure routine/reminder times.
- Contextual notification permission request.
- Completion and Home navigation.
- Persist onboarding completion and resumable progress.
- Skip options; no blocking if permission is denied.

Acceptance: user can finish with no goal, no rituals, and notifications disabled.

## 2. Home
- Greeting/date.
- Main goal summary if configured.
- Today's checklist from selected routine.
- Next recommended activity.
- 369 status only if enabled.
- Practical action card.
- Current streak/consistency summary.
- Empty states and links to setup.
- Read saved data, not hard-coded demo values.

Acceptance: completing an activity updates Home after returning, without app restart.

## 3. Goals
Fields:
- ID, title (required), description (optional), category, status, created/updated timestamps, target date (optional), completion date (optional).
- User may have multiple active goals.
- Goal states: ACTIVE, PAUSED, COMPLETED, ARCHIVED.
- Allow edit, pause/resume, complete, archive, delete (confirm destructive actions).
- Milestones are optional and ordered.
- Progress must be derived from milestones/tasks with a documented formula. Do not let multiple screens calculate different progress rules.

Acceptance: goal survives restart; status transitions are validated; deletion behavior for child milestones/tasks is defined.

## 4. Daily actions/tasks
- Task title, optional note, optional due date/time, linked goal ID optional, status, created/completed timestamps.
- Mark complete/uncomplete.
- Avoid shame language for overdue tasks.
- Daily action may be suggested by user or created by user; do not invent mandatory goals.
- If task is linked to a goal, use stable goal ID; handle goal deletion/archiving safely.

Acceptance: task completion appears in Home and Progress from persisted state.

## 5. Rituals library
Ritual definitions should be data-driven where practical, but do not create a generic engine so complex it obscures simple flows.
Each ritual has title, description, type, optional estimated duration, optional enabled state and sort order.

### 369 method
- User selects or writes an intention.
- Standard stages: morning 3 repetitions, afternoon 6, evening/night 9.
- Each repetition autosaves immediately or in safe small batches.
- Show progress for current stage and day.
- Session can resume after app close/process death.
- A stage is complete only when required repetitions are persisted.
- User can edit an intention for a new session; define behavior for editing an active session.
- Allow reminder times to be configured or disabled.
- History list shows date and completion state.
- Do not claim the method guarantees outcomes.

### Affirmations
- Bundled starter affirmations grouped by topic.
- User can favorite and create/edit their own.
- Optional read/repeat mode; avoid claiming it changes reality automatically.
- Save completion/history if progress tracking is enabled.

### Visualization
- User selects a prompt or writes their own.
- Optional timer with pause/resume/finish.
- No forced audio or internet requirement.
- Make timer state robust to lifecycle events; explain if timer resumes based on elapsed wall-clock time.

### Gratitude
- Prompt for 1–3 items by default, user-configurable.
- Save entries with date and timestamps.
- Allow editing.
- Avoid forcing positivity when user is having a hard day; “something you appreciate or found meaningful” is acceptable.

### Future-self scripting
- Long-form text editor.
- Autosave or explicit save, clearly indicated.
- Restore draft after process death.
- Support large text and IME.
- Keep journal privacy note visible in settings/about.

### Breathing/relaxation
- Simple paced breathing flow, configurable duration if implemented.
- Pause/stop at any time.
- Reduced-motion alternative.
- No medical claims; include “stop if uncomfortable” if relevant.

## 6. Journal
- Entry types: intention, gratitude, morning reflection, evening reflection, 369, scripting, freeform.
- Fields: stable ID, type, title optional, body, created/updated timestamps, associated goal ID optional, date bucket.
- User can create, edit, search (if implemented), filter, and delete.
- Clarify autosave behavior.
- All data persists locally.
- Avoid accidental data loss: confirm discard when leaving unsaved changes if autosave is not used.

## 7. Routine / habits
- User selects daily activities.
- Defaults may include:
  - Morning: intention, gratitude, 369 morning (if enabled).
  - Afternoon: 369 afternoon (if enabled), affirmation, practical task.
  - Night: 369 night (if enabled), evening reflection, next-day intention.
- These are editable defaults, not mandatory steps.
- Record completion by local date and activity ID.
- Define timezone/day-boundary behavior and streak rule.
- Do not reset a user's streak just because a notification was missed.

## 8. Challenges
Examples:
- 7-day gratitude.
- 7-day intention setting.
- 21-day consistency.
- Optional participation.
- Progress saved locally.
- Define pause, restart, and missed-day behavior.
- Completion reward should be modest and non-manipulative.

## 9. Progress
Metrics:
- Ritual/activity completion by day/week/month.
- Practical task completion.
- Current and best streak.
- Milestones completed.
- Challenge progress.
- Explain formulas in app/help if not self-evident.
- Avoid outcome predictions or ungrounded scores.
- Handle no data gracefully.

## 10. Reminders
- Per-reminder enable/disable.
- User-defined local time and days.
- Contextual notification copy.
- Snooze if implemented.
- Open the relevant feature/activity from notification.
- Avoid excessive follow-ups; no guilt-based copy.
- Handle permission denial and OS-level disabled notifications.
- Reschedule after reboot/timezone/time changes as needed.

## 11. Settings
- Theme preference (dark default).
- Reminder center.
- Routine configuration.
- About/version/privacy statement.
- Export local data if implemented.
- Delete all user data with explicit confirmation.
- No login required in V1.

## 12. Cross-feature contract examples
- Home requests summary data via a Home-owned use case/repository query; it does not import Goals UI/ViewModel.
- Progress calculates metrics from repository interfaces or dedicated summary queries; it does not scan feature UI state.
- Reminders receive stable activity/route identifiers, not direct ViewModel references.
- Deleting a goal must have documented behavior for linked tasks, milestones, and journal references.
