# 01 — Product Requirements Document (PRD)

## 1. Product vision
ManifestMate helps users build a consistent personal routine around their goals. It combines intention-setting and reflection rituals with concrete actions and progress tracking.

**Product principle:** “Manifest your goals. Practice daily. Take action. Track your journey.”

The app must not claim that manifestation rituals guarantee money, relationships, career outcomes, or other results. It should encourage agency and self-reflection without blaming users when goals do not happen.

## 2. Problem statement
Users may have goals but struggle to convert them into repeatable daily behavior. ManifestMate gives them a lightweight place to:
- define a goal and milestones;
- choose personal rituals;
- record intentions and reflections;
- receive optional reminders;
- complete practical tasks;
- review consistency and progress.

## 3. Target users
- People who enjoy journaling, affirmations, visualization, or gratitude practices.
- People who want a daily routine connected to personal goals.
- Users who value privacy and want core functionality without creating an account.
- Users who need simple progress feedback rather than complex project-management tools.

## 4. Product principles
1. Offline-first and privacy-conscious.
2. Calm, premium, welcoming, and readable.
3. Action-oriented, not only inspirational.
4. Flexible: users can customize, skip, pause, or disable routines.
5. Non-judgmental: missed days are normal; no shame language.
6. Accessible: contrast, scalable text, semantic labels, and touch targets.
7. Modular code and clear boundaries.
8. Honest progress: show completed activities and user-defined milestones, not supernatural outcome claims.

## 5. V1 scope
### Included
- First-launch onboarding and goal setup.
- Home dashboard.
- Goal creation/editing/archiving/completion.
- Milestones and practical tasks.
- Ritual library and activity screens:
  - 369 method;
  - affirmations;
  - visualization timer;
  - gratitude;
  - future-self scripting;
  - breathing/relaxation.
- Journal entries and reflection prompts.
- Daily routine checklist.
- Streaks and activity history.
- Basic progress charts and milestone summaries.
- Daily challenges with local progress.
- Local notifications/reminders with per-reminder controls.
- Settings, theme preferences, data export where feasible, and local data deletion.
- Room persistence and DataStore preferences.
- Tests and build documentation.

### Excluded from V1
- Mandatory account/login.
- Cloud sync or backup service.
- AI-generated personalized advice that requires a network.
- Public social feed/community.
- Payments/subscriptions.
- Claims that the app or rituals guarantee outcomes.
- Required internet access for core features.

## 6. V2-ready design (not V1 implementation)
Potential future backup/restore should be possible through repository boundaries and versioned data models. Do not implement a cloud provider in V1 unless explicitly approved. Do not expose journal content to third parties by default.

## 7. Primary success criteria
- User can complete onboarding without account creation.
- User can create a goal and add at least one practical action.
- User can complete and revisit a 369 session.
- Journal entries persist after process death/relaunch.
- Home reflects saved state from local storage.
- Reminders can be enabled/disabled individually.
- Core app works in airplane mode.
- App handles empty, loading, error, and permission-denied states gracefully.
- Build and automated tests pass in the declared environment.

## 8. UX tone and language
Tone: encouraging, calm, respectful, practical.
Good: “A small step still counts.” / “Would you like to continue?”
Avoid: “You failed your manifestation.” / “You attracted this negative result.” / “This ritual guarantees success.”

## 9. Data ownership
All V1 user-created data is stored locally on the device. Explain that uninstalling the app or clearing its storage can remove local data unless the user exports it. Provide a clear delete-data action and confirmation.

## 10. Product acceptance criteria
A feature is not complete just because its screen renders. It must:
- save and reload real data;
- have input validation;
- handle cancellation and empty states;
- expose accessible labels;
- include relevant unit/UI tests;
- follow module dependency rules;
- be usable offline;
- have no hard-coded secrets;
- be documented where behavior is non-obvious.
