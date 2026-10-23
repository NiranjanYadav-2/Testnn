# 04 — Modular Architecture & Dependency Rules

## 1. Architectural goals
- Strong feature isolation and low coupling.
- Offline-first and testable.
- Clear ownership of UI state, business rules, and persistence.
- Adding a feature should not require editing unrelated features.
- Shared infrastructure has small, stable APIs.
- Future backup/restore can be added behind repository boundaries.

## 2. Recommended Gradle module layout
Use this as the target structure. Claude should validate module count and Gradle setup before implementation, but must preserve strong feature boundaries.

```text
ManifestMate/
├── app/
├── core/
│   ├── common/
│   ├── model/
│   ├── designsystem/
│   ├── navigation/
│   ├── database/
│   ├── datastore/
│   ├── domain/
│   ├── notifications/
│   ├── testing/
│   └── analytics/          # optional; omit unless a real offline/local need exists
├── feature/
│   ├── onboarding/
│   ├── home/
│   ├── goals/
│   ├── rituals/
│   ├── ritual369/
│   ├── affirmations/
│   ├── visualization/
│   ├── gratitude/
│   ├── journal/
│   ├── dailyactions/
│   ├── challenges/
│   ├── progress/
│   ├── reminders/
│   └── settings/
├── build-logic/             # convention plugins if justified
└── gradle/
```

Each listed directory intended as a Gradle module must be included in `settings.gradle.kts` with matching build files. Claude must not leave empty/non-buildable placeholder modules. If implementation complexity justifies combining very small features, explain the trade-off first; the default preference is isolation.

## 3. Responsibility boundaries
- `:app`: Android application class, startup, dependency injection composition, app-level navigation host, manifest integration. Keep business logic out.
- `:core:common`: generic result types, dispatchers/time abstractions only if truly shared.
- `:core:model`: shared primitive/domain models that multiple features need. Avoid turning it into a dumping ground.
- `:core:designsystem`: theme, tokens, reusable generic Compose components.
- `:core:navigation`: route/destination contracts and navigation abstractions; must not depend on feature implementations.
- `:core:database`: Room database, entities, DAOs, migrations. Do not expose DAOs to UI modules.
- `:core:datastore`: preferences storage.
- `:core:domain`: only genuinely cross-feature domain contracts/use cases. Feature-specific rules should stay in the owning feature.
- `:core:notifications`: local notification channels, scheduling adapter, permission/status helpers.
- `:core:testing`: test fixtures, fake clocks, test dispatchers, reusable test helpers.
- Feature modules: own UI, ViewModel/state, feature logic, feature-specific data access/repository implementation or internal packages, and tests.

## 4. Dependency rules
Allowed high-level dependencies:
- `:app` may depend on feature entry APIs and core modules; it is the composition root.
- Feature modules may depend on `core:designsystem`, `core:model`, `core:common`, and stable domain contracts where needed.
- Feature modules must not depend on other feature modules' implementation.
- `core:*` modules must not depend on `feature:*` modules.
- `core:designsystem` must not depend on domain or feature code.
- `core:database` must not depend on UI or feature modules.
- No circular Gradle dependencies.
- Do not pass a ViewModel from one feature into another.
- Do not use global mutable singletons/service locators to bypass boundaries.
- Avoid shared mutable state. Use repositories and stable contracts.
- Navigation arguments should usually be stable IDs, not full serialized domain objects.

## 5. Feature package pattern
Inside a feature module, keep layers separate:
```text
feature/goals/src/main/kotlin/.../goals/
├── ui/
│   ├── GoalsScreen.kt
│   ├── GoalDetailScreen.kt
│   └── components/
├── presentation/
│   ├── GoalsViewModel.kt
│   ├── GoalsUiState.kt
│   └── GoalsUiEvent.kt
├── domain/
│   ├── model/
│   ├── repository/
│   └── usecase/
├── data/
│   ├── repository/
│   ├── mapper/
│   └── local/
└── navigation/
    └── GoalsNavigation.kt
```
Do not create files just to match a template if the code does not need them. Keep every file focused.

## 6. Data flow
Typical flow:
UI event → ViewModel → use case/domain rule → repository interface → repository implementation → Room/DataStore → observable state → UI.

UI must not access DAO directly. Composables should be mostly stateless; hoist state to ViewModel when state survives recomposition or is business-relevant. Do not put all business logic into ViewModels.

## 7. Inter-feature communication
Prefer:
- shared stable domain IDs and repository contracts;
- explicit callbacks or navigation results;
- app-level orchestration for workflows spanning modules;
- observable persisted data when two features need to reflect the same change.

Example: completing a goal milestone updates the Goals repository. Home and Progress read derived summaries through their own use cases/repository queries. They do not import `GoalsViewModel`.

Avoid a generic event bus unless a clear use case requires it. If using events, document ownership, delivery semantics, and test behavior.

## 8. Dependency injection
Use constructor injection. Select a DI framework only after verifying current compatibility and project needs; do not mix DI patterns. Keep dependency graph construction in the composition root or module providers. Do not use service locator access from UI.

## 9. State and coroutines
- Use Kotlin coroutines and Flow for async/persistent streams.
- Expose immutable `StateFlow<UiState>` from ViewModels.
- Use structured concurrency and lifecycle-aware collection.
- Inject dispatchers/clock where testability requires it.
- Do not block the main thread with database/file work.
- Handle cancellation correctly.

## 10. Enforcing isolation
- Add architecture/dependency tests or Gradle checks where practical.
- Keep package visibility/internal APIs narrow.
- Review dependency tree for cycles.
- Use module-level unit tests.
- CI must run at least `./gradlew test` and a debug assemble task.
- If available, add lint/static checks.
- Do not claim architecture isolation solely because code is in separate folders; enforce it through module dependencies and public APIs.

## 11. Avoid overengineering
The user strongly prefers isolation. Still, each module must have a real boundary and meaningful public API. Do not make a Gradle module for every button or trivial data class. Keep small components as files/packages inside the owning feature or design system.
