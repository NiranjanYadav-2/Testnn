# Claude Phase Prompts — ManifestMate

Use one prompt at a time. Do not ask Claude to implement every phase in a single response.

## Phase 0 — Audit and plan
Read `CLAUDE_MASTER_PROMPT.md` and all docs. Inspect the repository without modifying files. Report existing files, environment/toolchain, proposed Gradle module graph, dependencies, risks, unresolved decisions, and exact Phase 1 plan. Do not code yet.

## Phase 1 — Foundation
Implement only the build foundation: Gradle wrapper/version catalog, app shell, core common/model/designsystem/navigation, theme tokens, Manrope/Roboto setup (after verifying licenses), icon approach, reusable base components, and five-tab navigation. No fake feature logic beyond clear placeholder/empty states. Add tests where useful. Run tests and debug assemble; report exact output.

## Phase 2 — Persistence
Implement Room schema, DAOs, migrations/schema export, DataStore preferences, repository boundaries, and test fixtures. No feature UI should access DAOs. Add persistence tests and migration tests. Run the build and tests.

## Phase 3 — Onboarding and Home
Implement resumable onboarding, optional goal/ritual choices, reminder preference setup, Home dashboard with real state and empty states. Notification permission must be requested contextually, not forced. Test skip/restore/offline paths.

## Phase 4 — Goals and daily actions
Implement goal CRUD, status transitions, milestones, tasks, relationship/deletion behavior, and one documented progress formula. Ensure Home can observe summaries without importing Goals implementation. Add tests.

## Phase 5 — Rituals and 369
Implement ritual library and individual activity screens. For 369, persist each repetition safely, enforce 3/6/9 stage counts, support resume after process death, and prevent duplicate records. Then implement affirmations, gratitude, visualization, scripting, and breathing. Add focused tests.

## Phase 6 — Journal and routine
Implement local journal CRUD, entry types, daily routine configuration, habit records, streak logic, and history. Ensure journal text is not logged and autosave behavior is clear. Test process death, large text, empty states, and streak edge cases.

## Phase 7 — Reminders and challenges
Implement notification channels, permission state, scheduling adapter, cancellation/rescheduling, unique identities, reboot/timezone handling appropriate to chosen API, reminder center, and challenge progress. Do not promise exact delivery unless exact-alarm requirements are legitimately met. Test permission denial and duplicate scheduling.

## Phase 8 — Progress and Settings
Implement metrics from persisted data, charts, milestones, achievements, settings, theme preference, delete-data flow, and optional export only if approved. Add tests proving metrics match stored records.

## Phase 9 — Release hardening
Run all tests, architecture checks, lint if configured, debug and release builds. Review offline mode, accessibility, font scaling, permissions, app icon, privacy disclosures, signing configuration, APK/AAB contents, and README. Report any unverified item honestly.

## Standard completion prompt after each phase
Before moving on, inspect your changes against the relevant documentation. Fix broken builds/tests, remove accidental placeholders, verify no feature dependency violations, update docs if needed, and provide a concise report of files changed and exact commands/results.
