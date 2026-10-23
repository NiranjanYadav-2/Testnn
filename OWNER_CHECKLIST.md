# Owner Decisions & Release Checklist

This file tracks items that must be decided or verified before public release.

## Before implementation
- [ ] Choose permanent application ID/package name.
- [ ] Confirm minimum Android API target based on intended device coverage.
- [ ] Verify Android Gradle Plugin / Gradle / Kotlin / JDK compatibility.
- [ ] Choose and pin dependency injection approach.
- [ ] Decide whether V1 includes data export.
- [ ] Confirm data deletion and linked-record behavior.
- [ ] Confirm streak and local-day/timezone rules.
- [ ] Verify font/icon asset licenses.

## Before public release
- [ ] Check Play Store name availability and trademark conflicts for “ManifestMate”.
- [ ] Verify launcher icon and adaptive icon.
- [ ] Verify all main flows in offline mode.
- [ ] Run unit, UI, database, migration, and architecture tests.
- [ ] Build and test release APK/AAB.
- [ ] Securely configure signing credentials.
- [ ] Review requested permissions and exported components.
- [ ] Prepare privacy policy and Play data disclosures.
- [ ] Test notification permission denial and scheduling behavior.
- [ ] Test TalkBack, font scaling, and different screen sizes.
- [ ] Confirm no demo data, fake progress, TODO-only core paths, or private logs remain.
