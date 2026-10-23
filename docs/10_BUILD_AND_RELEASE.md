# 10 — Build & Release

## 1. Toolchain
Use compatible, mutually supported versions of:
- Android Gradle Plugin
- Gradle wrapper
- Kotlin
- Compose compiler/plugin setup appropriate to the selected Kotlin version
- JDK
- Android SDK / compile SDK / target SDK

Claude must verify current compatibility from official documentation when selecting versions. Do not randomly mix versions from old tutorials. Pin versions through a version catalog or centralized convention.

## 2. Package ID
Use a clearly marked placeholder such as `com.example.manifestmate` during development only. Before release, choose a permanent reverse-domain application ID. Once published, changing the application ID creates a different Play Store app.

## 3. SDK levels
Select and document `minSdk`, `compileSdk`, and `targetSdk` based on current Play policy, required device coverage, and library compatibility. Do not silently assume old SDK targets are acceptable. The product owner should approve the final minimum supported Android version.

## 4. Build files
- `settings.gradle.kts`
- root `build.gradle.kts`
- `gradle/libs.versions.toml` (recommended)
- module `build.gradle.kts` files
- Gradle wrapper files
- `.gitignore`
- README with setup/build instructions

Do not leave included modules without build scripts or source sets.

## 5. Build commands
Typical commands (adapt to actual configured tasks):
```bash
./gradlew projects
./gradlew test
./gradlew assembleDebug
./gradlew bundleRelease
```
Release signing must be configured securely; do not place signing credentials in source control.

## 6. CI guidance
A GitHub Actions workflow may:
- check out code;
- set up the required JDK;
- set up Android SDK if needed;
- cache Gradle dependencies;
- run unit tests;
- assemble debug APK;
- optionally build release AAB when secure signing secrets are configured;
- upload build artifacts with a sensible retention period.

Do not store keystores or credentials in repository files. Do not expose secrets in logs. Keep CI optional for local development.

## 7. Deliverables
For a development ZIP, include source and build configuration, not machine-specific `.gradle/`, `build/`, or local SDK files. Do not include keystores or secrets.
- Source code
- Gradle wrapper
- Module configs
- Tests
- README
- Docs
- Optional CI workflow
- No generated secrets or personal local paths

## 8. Release readiness
- Verify app label and launcher icon.
- Confirm notification permission behavior.
- Test cold start, process death, rotation/resizing, and back navigation.
- Review app size and startup time.
- Check accessibility and font scaling.
- Verify privacy policy/data declarations.
- Test release build, not only debug build.
- Verify app signing and version code/name.
- Ensure no debug/demo content is visible.
