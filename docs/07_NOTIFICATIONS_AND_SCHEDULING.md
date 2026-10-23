# 07 — Notifications & Scheduling Specification

## 1. Purpose
Reminders are optional prompts to help users follow their chosen routine. They must be gentle, configurable, and non-judgmental.

## 2. Example defaults (all editable and optional)
- Morning intention: 7:00 AM
- 369 morning: 7:15 AM
- Afternoon ritual: 1:00 PM
- Practical action: 5:00 PM
- Evening reflection / 369 night: 9:00 PM

These are starter defaults only. Do not force all reminders on; onboarding should let users choose.

## 3. Controls
- Each reminder independently enabled/disabled.
- Time editable.
- Optional day-of-week selection.
- Snooze only if implemented with clear behavior.
- Completion should cancel or suppress obsolete reminders for that activity/day.
- Prevent duplicate scheduled notifications.
- Provide “Open activity” navigation from notification.
- Respect OS notification permission and app notification settings.

## 4. Android behavior
- Handle Android 13+ notification runtime permission.
- Do not request permission before explaining the benefit.
- Notification permission denial must not break app features.
- Use a scheduling strategy appropriate to requirements:
  - WorkManager for deferrable work, not exact clock-time alarms.
  - AlarmManager only if exact timing is genuinely required and policy/permission constraints are handled.
  - Prefer battery-friendly inexact scheduling where exact delivery is not essential.
- Do not promise exact delivery: Android battery optimization, vendor restrictions, and OS policy can delay reminders.
- Reschedule after reboot, timezone change, and wall-clock changes where required by the selected scheduling approach.
- Test behavior on actual devices/emulators across supported API levels.

## 5. Notification copy
Good:
- “A quiet moment for your intention?”
- “Ready for a small step toward your goal?”
- “Your evening reflection is waiting, if you have time.”

Avoid:
- “You are failing your routine.”
- “You will lose your manifestation if you skip today.”
- High-frequency or manipulative reminders.

## 6. Scheduling architecture
- `core:notifications` owns channels, permission/status helpers, notification builder, scheduler adapter, receiver/worker integration.
- Feature modules request scheduling through a narrow contract; they do not directly manage AlarmManager/WorkManager internals.
- Store reminder preferences in one authoritative persistence source.
- Use unique work/alarm identities to prevent duplicates.
- Notifications should carry stable destination/activity IDs, not serialized ViewModels or sensitive journal text.
- Never include private journal content in notification text by default.

## 7. Test cases
- Permission granted.
- Permission denied.
- OS notifications disabled after reminders configured.
- Reminder edited/disabled.
- Activity completed before reminder fires.
- Duplicate scheduling attempt.
- Device reboot.
- Timezone/time change.
- Daylight-saving boundary where applicable.
- App upgraded with existing reminder settings.
- Notification opens the correct activity.
