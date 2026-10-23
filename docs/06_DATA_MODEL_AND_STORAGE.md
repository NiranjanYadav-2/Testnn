# 06 — Data Model & Local Storage

## 1. Storage choices
- Room for structured user data.
- DataStore for small preferences (theme, onboarding flags, reminder preferences only if the chosen model fits).
- Keep local-first; core flows must not require network.
- Do not store all structured app data as a single JSON blob in DataStore.
- Do not store private journal text in logs or analytics.

## 2. Suggested Room entities
Exact fields may be refined during implementation, but IDs, timestamps, status values, and relationships must be explicit.

### GoalEntity
- `id: String` (stable UUID)
- `title: String`
- `description: String?`
- `category: String`
- `status: GoalStatus`
- `targetDateEpochDay: Long?` or a documented date type
- `createdAtEpochMillis: Long`
- `updatedAtEpochMillis: Long`
- `completedAtEpochMillis: Long?`
- `sortOrder: Int`

### GoalMilestoneEntity
- `id: String`
- `goalId: String`
- `title: String`
- `notes: String?`
- `status: MilestoneStatus`
- `sortOrder: Int`
- `createdAtEpochMillis: Long`
- `completedAtEpochMillis: Long?`

### DailyTaskEntity
- `id: String`
- `goalId: String?`
- `title: String`
- `notes: String?`
- `scheduledDateEpochDay: Long?`
- `status: TaskStatus`
- `createdAtEpochMillis: Long`
- `updatedAtEpochMillis: Long`
- `completedAtEpochMillis: Long?`

### RitualDefinitionEntity (or bundled definitions + user overrides)
- `id: String`
- `type: RitualType`
- `title: String`
- `description: String`
- `enabledByDefault: Boolean`
- `estimatedDurationMinutes: Int?`
- `sortOrder: Int`

### RitualSessionEntity
- `id: String`
- `ritualType: String`
- `intentionText: String?`
- `goalId: String?`
- `localDateEpochDay: Long`
- `status: SessionStatus`
- `startedAtEpochMillis: Long`
- `updatedAtEpochMillis: Long`
- `completedAtEpochMillis: Long?`

### RitualEntryEntity
- `id: String`
- `sessionId: String`
- `sequenceNumber: Int`
- `content: String`
- `createdAtEpochMillis: Long`
- Unique index on `(sessionId, sequenceNumber)` to prevent duplicate repetition records.

### JournalEntryEntity
- `id: String`
- `type: JournalEntryType`
- `title: String?`
- `body: String`
- `goalId: String?`
- `createdAtEpochMillis: Long`
- `updatedAtEpochMillis: Long`
- `localDateEpochDay: Long`

### HabitRecordEntity
- `id: String`
- `activityId: String`
- `localDateEpochDay: Long`
- `status: CompletionStatus`
- `completedAtEpochMillis: Long?`
- Unique index on `(activityId, localDateEpochDay)` where the design allows one completion record per activity/day.

### ChallengeProgressEntity
- `id: String`
- `challengeId: String`
- `startDateEpochDay: Long`
- `status: ChallengeStatus`
- `currentDay: Int`
- `completedDays: Int`
- `updatedAtEpochMillis: Long`

### AchievementRecordEntity
- `id: String`
- `achievementKey: String`
- `earnedAtEpochMillis: Long`
- Metadata only where needed; do not duplicate all source data.

Reminder settings may live in DataStore or Room depending on whether reminders need relational queries/history. Make the choice explicit; do not keep two sources of truth.

## 3. Enums and stable values
Use enums/value types in Kotlin and explicit converters for Room. Persist stable values rather than enum ordinal numbers. Adding a new enum must not corrupt old records.

Suggested enums:
- GoalStatus: ACTIVE, PAUSED, COMPLETED, ARCHIVED
- TaskStatus: PENDING, COMPLETED, CANCELLED
- SessionStatus: IN_PROGRESS, COMPLETED, ABANDONED
- ChallengeStatus: ACTIVE, PAUSED, COMPLETED, ABANDONED
- JournalEntryType: INTENTION, GRATITUDE, MORNING_REFLECTION, EVENING_REFLECTION, RITUAL_369, FUTURE_SELF, FREEFORM

## 4. Relationships and deletion
- Milestones reference goals.
- Tasks may reference goals.
- Sessions may reference goals.
- Journal entries may reference goals.
- Define foreign key behavior carefully. Avoid cascading deletion of journal text unexpectedly.
- Before goal deletion, either:
  1. ask user whether to preserve related journal/session history and detach references, or
  2. explain exactly what will be deleted.
- Prefer soft archive for goals.
- Destructive actions need confirmation.

## 5. Date/time rules
- Store event timestamps as UTC epoch millis for ordering.
- Store local-day bucket as epoch day computed in the user's local timezone at the time of recording.
- Store reminder local time as local time + selected days, not a fixed UTC instant.
- Handle timezone change and daylight-saving transitions.
- Document streak rules and how a day is counted.
- Do not rely on device wall-clock ordering without considering changed dates; keep logic testable.

## 6. Migrations
- Use explicit Room migrations for every released schema change.
- Do not use destructive migration in production by default.
- Export Room schemas into version control.
- Add migration tests for important upgrade paths.
- Use stable UUID IDs and timestamps so future export/backup can preserve identity.

## 7. Repositories and data access
- UI never accesses DAO directly.
- Repository APIs expose domain models and Flow/suspend operations.
- Hide Room entities from UI/domain APIs.
- Avoid returning mutable lists or leaking database implementation details.
- Provide fake repositories for unit/UI tests.
- No network repository in V1 unless explicitly approved.

## 8. Export readiness
If data export is implemented, define a versioned export format with schema version and validation. Do not expose raw internal database files as the only backup contract. V2 restore must validate imports, handle duplicates/conflicts, and never silently overwrite existing data.
