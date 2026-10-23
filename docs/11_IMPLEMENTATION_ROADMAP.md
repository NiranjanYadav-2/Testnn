# 11 — Implementation Roadmap

Build in small verified phases. Do not generate the whole app in one giant code dump.

## Phase 0 — Repository audit and design decisions
Deliver:
- confirm package ID placeholder;
- confirm min/target/compile SDK and toolchain compatibility;
- final module graph;
- dependency diagram;
- file tree;
- ADRs for major decisions (Room, DI, notification scheduling, navigation).
Exit gate: owner approves plan before broad code generation.

## Phase 1 — Build foundation
Implement:
- Gradle wrapper/version catalog;
- app module;
- core common/model/designsystem/navigation;
- theme, typography, icons, reusable base components;
- dependency injection choice;
- basic app shell and bottom navigation.
Exit gate: `./gradlew assembleDebug` passes; five tabs render; architecture graph has no cycles.

## Phase 2 — Persistence foundation
Implement:
- Room database and schema export;
- DataStore preferences;
- repository patterns;
- migrations and test fixtures.
Exit gate: database/repository tests pass; sample test data is not bundled into production behavior.

## Phase 3 — Onboarding and Home
Implement:
- resumable onboarding;
- goal/ritual preference setup;
- home summaries and empty states;
- no forced notification permission.
Exit gate: first-launch flow works fully offline and can be skipped safely.

## Phase 4 — Goals and daily actions
Implement:
- CRUD goals;
- milestones;
- daily tasks;
- derived progress formula;
- relation/deletion policy.
Exit gate: persistence and UI tests pass.

## Phase 5 — Rituals and 369
Implement:
- ritual library;
- 369 session state machine and autosave/resume;
- affirmations;
- gratitude;
- visualization timer;
- future-self scripting;
- breathing/relaxation.
Exit gate: partial sessions survive process death; tests cover required repetition counts.

## Phase 6 — Journal and routine
Implement:
- journal CRUD;
- prompts/reflections;
- daily routine customization;
- habit records and streak rules.
Exit gate: data persists; journal text is not logged; routine can be customized.

## Phase 7 — Notifications and challenges
Implement:
- reminder center;
- permission handling;
- scheduling, cancellation, rescheduling;
- challenge definitions/progress.
Exit gate: reminders can be individually controlled and do not duplicate.

## Phase 8 — Progress and Settings
Implement:
- weekly/monthly summaries;
- completion metrics;
- achievement rules;
- theme/settings;
- delete-data flow and optional export if approved.
Exit gate: metrics reconcile with stored records.

## Phase 9 — Quality and release
- architecture/dependency checks;
- accessibility review;
- offline test;
- test on supported API levels;
- build debug and release;
- inspect APK/AAB;
- privacy/permission review;
- write final README and changelog.
Exit gate: all mandatory tests pass, limitations are documented, release signing handled securely.

## Phase control
At the end of every phase Claude must report:
1. files created/modified;
2. architecture decisions made;
3. commands run and exact results;
4. known issues;
5. next phase plan;
6. whether documentation needs updating.

Do not move on while the current phase is failing to build unless the blocker is explicitly documented and approved.
