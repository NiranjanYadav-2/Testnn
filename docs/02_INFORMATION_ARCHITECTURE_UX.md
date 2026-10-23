# 02 — Information Architecture & UX

## 1. Main navigation
Use a bottom navigation bar with five destinations:
1. Home
2. Goals
3. Rituals
4. Journal
5. Progress

Settings, reminder controls, challenge details, and individual activity screens are secondary destinations. Avoid adding more items to the bottom bar.

## 2. First-launch flow
1. Splash/brand transition (brief; do not delay app startup unnecessarily).
2. Welcome screen explaining that the app combines intention, reflection, and practical action.
3. Choose or create a first goal; allow Skip.
4. Choose optional rituals (369, affirmation, visualization, gratitude, scripting, breathing).
5. Configure routine schedule with editable times; all reminders are optional.
6. Explain notifications, then request Android notification permission only when contextually appropriate and supported by the OS.
7. Show a short review screen and enter Home.

Rules:
- Do not request notification permission immediately on cold launch without context.
- Allow users to skip onboarding steps and finish later.
- Save onboarding progress so app interruptions do not force a restart.
- Do not block core functionality if notifications are denied.
- Do not imply that users must select 369; routines are customizable.

## 3. Home screen hierarchy
Top:
- Greeting and date.
- Optional profile/settings affordance (no account implied).
- Main goal card, if a goal exists.

Middle:
- Today’s routine progress.
- Next suggested activity / Continue button.
- Practical daily action card.
- 369 session status if the user enabled it.
- Current streak or consistency indicator, presented gently.

Bottom:
- Five-item navigation.

Rules:
- Prioritize what to do next; avoid a wall of metrics.
- Each card is tappable only if it has a clear destination/action.
- Use clear empty states when no goal/routine exists.
- Do not fabricate progress or show demo data in production.

## 4. Goals
Goals list:
- Active, paused, completed, and archived filters or sections.
- Add goal action.
- Each goal card: title, category, target date (optional), progress from milestones/tasks, status.

Goal detail:
- Goal statement and description.
- Milestones.
- Practical actions/tasks.
- Related affirmations (optional).
- Edit, pause/resume, complete, archive, delete.

Categories:
- Career
- Business
- Money
- Relationships
- Confidence
- Personal growth
- Health/wellbeing (neutral; not medical advice)
- Custom/Other

## 5. Rituals library
Show individual ritual cards with:
- Name and short purpose.
- Estimated duration.
- Start/Continue action.
- Last completed / today status where useful.

Ritual activity screens must be purpose-built:
- 369: intention text, repetition count, session stage, autosave, resume.
- Affirmations: selected statements, read/repeat flow, optional favorites.
- Visualization: timer, prompt, pause/finish, no forced audio.
- Gratitude: editable list of items.
- Future-self scripting: long-form editor with autosave.
- Breathing: accessible paced animation with pause/stop and reduced-motion alternative.

## 6. Journal
- Daily entry editor with prompts and free writing.
- Separate entry types (intention, gratitude, morning reflection, evening reflection, 369 log, future-self script) or a unified journal model with a type field.
- Calendar/history list.
- Search if implemented.
- Edit and delete.
- Clear empty state and local privacy explanation.
- Autosave or explicit save behavior must be consistent and clearly indicated.

## 7. Progress
Display honest, explainable metrics:
- Activities completed over selected period.
- Routine completion rate.
- Current and best streak, with rules explained.
- Milestones completed.
- Challenge progress.
- Simple weekly/monthly chart.
Avoid presenting progress as a prediction of external success.

## 8. Challenges
Examples:
- 7-day gratitude practice.
- 7-day intention-setting.
- 21-day consistency practice.
Challenges must be optional and restart/pause behavior defined. Do not imply scientifically guaranteed effects or make a missed day a moral failure.

## 9. Reminders center
- List reminders by routine/time.
- Enable/disable each independently.
- Edit time and days.
- Optional snooze.
- Explain OS notification permission and that exact delivery may vary by device/battery settings.
- Show a simple status if notifications are disabled at OS level.

## 10. Settings
- Theme: dark (default), optional system/light preference if implemented.
- Reminder preferences.
- Routine customization.
- Data export and delete-all-data controls if included.
- About, version, privacy note, open-source licenses.
- No account screen in V1.

## 11. Empty/loading/error states
Every screen must define:
- empty state;
- loading state if asynchronous work exists;
- recoverable error state;
- no-permission state where relevant;
- success feedback after save/complete/delete.

## 12. Navigation and state
- Preserve screen state sensibly on tab changes.
- Avoid duplicate destinations on repeated taps.
- Back behavior must be consistent.
- Do not pass full domain objects through navigation; pass stable IDs and load data from repositories.
