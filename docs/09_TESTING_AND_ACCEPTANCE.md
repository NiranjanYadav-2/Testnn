# 09 — Testing Strategy & Acceptance Criteria

## 1. Test pyramid
- Unit tests for domain rules, ViewModels, mappers, streak calculations, progress formulas, and reminder calculations.
- Room DAO/repository tests for persistence and relations.
- Compose UI tests for primary flows, validation, accessibility semantics, and empty/error states.
- Integration tests for onboarding → goal creation → daily action → progress.
- Manual smoke tests on supported Android versions and at least one small and one large screen.
- Architecture/dependency tests to prevent forbidden module dependencies.

## 2. Essential test scenarios
### Onboarding
- Complete with all options.
- Skip goal creation.
- Skip rituals.
- Deny notifications.
- Kill/restart app mid-onboarding and resume correctly.

### Goals and tasks
- Create/edit/pause/resume/complete/archive goal.
- Invalid empty title rejected.
- Milestone progress calculation correct.
- Task completion persists after restart.
- Goal deletion handles related data according to documented policy.

### 369
- Morning requires 3 persisted repetitions.
- Afternoon requires 6 persisted repetitions.
- Night requires 9 persisted repetitions.
- Partial session resumes after process death.
- Duplicate taps do not duplicate repetition sequence numbers.
- Session completion only occurs when required repetitions are persisted.
- Editing intention does not corrupt an existing session.

### Journal
- Create/edit/delete entries.
- Draft survives process death if autosave is promised.
- No journal content in logs.
- Large text and keyboard/IME work correctly.

### Reminders
- Permission granted/denied.
- Individual reminder toggles work.
- Edited time replaces old schedule.
- Completion suppresses pending reminder when specified.
- Correct destination opens from notification.
- Reboot/timezone handling follows chosen strategy.

### Progress
- Empty history is handled.
- Streak calculation has unit tests for consecutive days, missed day, timezone changes, and daylight-saving boundaries as relevant.
- Metrics match underlying persisted records.
- No fake/demo metrics in production.

### Offline
- App launches and all V1 core features work in airplane mode.
- No required network permission/request unless justified and approved.

## 3. Architecture checks
- `core:*` never depends on `feature:*`.
- Feature modules do not depend on other feature implementations.
- No circular dependencies.
- UI does not access DAOs.
- No hard-coded secrets.
- No global mutable service locator.
- Shared design system remains domain-neutral.

## 4. Build checks
At minimum, in a compatible Android SDK/JDK environment:
```bash
./gradlew test
./gradlew assembleDebug
```
If an Android lint task is configured:
```bash
./gradlew lint
```
For all modules, ensure test tasks are actually included/executed. Record the actual result and environment. Do not claim a successful build if the command was not run or failed.

## 5. Definition of Done (per feature)
- Requirements implemented.
- UI states defined.
- Real persistence integrated.
- Validation and error handling present.
- Tests added and passing.
- Accessibility reviewed.
- Offline behavior verified.
- Module dependency rules respected.
- No placeholder buttons/actions that pretend to work.
- Documentation updated.
