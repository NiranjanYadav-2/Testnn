# ManifestMate — Documentation & Claude Handoff Pack

**Purpose:** This pack is the product, design, architecture, implementation, and QA source of truth for building ManifestMate with Claude or another coding assistant.

## Product summary
ManifestMate is an offline-first Android app that helps people clarify goals, practice intention-setting rituals, journal, and take practical actions toward their goals. Rituals are presented as reflection/focus habits, not as guaranteed ways to make outcomes happen.

## Locked decisions
- App name: **ManifestMate** (working name; check Play Store name availability, trademarks, and domain before public release).
- Platform: Android.
- Language/UI: Kotlin + Jetpack Compose.
- Architecture: highly modular, loosely coupled, feature-isolated Gradle modules.
- V1: offline-first; core features work without login, backend, or internet.
- V2: backup/restore may be added later; design V1 data and repository boundaries so this is possible.
- Main navigation: Home, Goals, Rituals, Journal, Progress.
- Visual direction: premium cosmic, balanced and readable—not cluttered.
- Palette: Midnight Purple `#171329`, Lavender `#A99AEF`, Champagne Gold `#E7C98B`, Soft White `#F5F1FA`.
- Typography: Manrope for brand/display headings; Roboto for body text and controls. Bundle font files and verify their licenses before shipping.
- Icons: consistent rounded vector icons, preferably Material Symbols Rounded or another consistent Android-compatible vector set.
- Strong separation: no feature should import another feature's internal UI/ViewModel/repository implementation.
- Do not put an entire screen or feature into one giant file.

## Documents
1. `docs/01_PRODUCT_REQUIREMENTS.md` — product goals, scope, personas, non-goals, acceptance criteria.
2. `docs/02_INFORMATION_ARCHITECTURE_UX.md` — navigation, screens, user flows, onboarding.
3. `docs/03_UI_DESIGN_SYSTEM.md` — colors, typography, spacing, components, accessibility.
4. `docs/04_MODULAR_ARCHITECTURE.md` — Gradle modules, dependency rules, contracts, package structure.
5. `docs/05_FEATURE_SPECIFICATIONS.md` — detailed behavior for each feature.
6. `docs/06_DATA_MODEL_AND_STORAGE.md` — Room entities, relations, persistence, migration rules.
7. `docs/07_NOTIFICATIONS_AND_SCHEDULING.md` — reminder preferences and Android scheduling requirements.
8. `docs/08_PRIVACY_SECURITY_AND_BACKUP.md` — privacy, offline storage, future backup boundaries.
9. `docs/09_TESTING_AND_ACCEPTANCE.md` — test strategy and release gates.
10. `docs/10_BUILD_AND_RELEASE.md` — build setup, CI, signing, APK/AAB delivery.
11. `docs/11_IMPLEMENTATION_ROADMAP.md` — implementation phases and Definition of Done.
12. `CLAUDE_MASTER_PROMPT.md` — instructions to give Claude before coding.
13. `CLAUDE_PHASE_PROMPTS.md` — phase-by-phase prompts for controlled implementation.

## How to use with Claude
1. Upload this ZIP to Claude or extract it in the repository.
2. Read `CLAUDE_MASTER_PROMPT.md` first.
3. Ask Claude to review all docs and return a proposed implementation plan before editing files.
4. Build in phases from `docs/11_IMPLEMENTATION_ROADMAP.md`; do not ask for the whole app in one giant response.
5. After each phase, run Gradle build/tests and review changed files.
6. Ask Claude to update these docs when a decision changes. Keep the docs and code aligned.

## Important implementation notes
- This is a specification pack, not an already-built Android app.
- Exact dependency versions, `minSdk`, `targetSdk`, package ID, and CI environment must be selected and verified at implementation time against current Android/Gradle compatibility.
- Do not claim a build passed unless the build was actually run and its result inspected.
- Do not commit signing keys, passwords, service-account private keys, or secrets.
