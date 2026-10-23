# 03 — UI Design System

## 1. Design concept
**Premium Cosmic + Balanced Minimal.** Deep violet surfaces, lavender action accents, restrained champagne-gold highlights, and small celestial details. Keep content dominant; decorative stars/glows should be rare.

## 2. Color tokens
| Token | Hex | Usage |
|---|---|---|
| Midnight Purple | `#171329` | Main dark background |
| Card Surface | `#29223F` | Elevated cards/sheets |
| Lavender | `#A99AEF` | Primary accent, selected controls |
| Champagne Gold | `#E7C98B` | Small highlights, milestone emphasis |
| Soft White | `#F5F1FA` | Main text on dark surfaces |
| Muted Lavender | `#C7BFE5` | Secondary text on dark surfaces |
| Deep Border | `#51416E` | Subtle card outlines |
| Error | Use semantic Material error color | Errors only |
| Success | Use semantic accessible success color | Completion feedback only |

Do not use gold for long paragraphs. Verify actual contrast ratios before release; adjust token pairs if required by WCAG/accessibility guidance.

## 3. Typography
- **Manrope:** display headings, selected goal titles, large progress values.
- **Roboto:** body text, labels, controls, form fields.
- Bundle font files in resources and confirm font license/attribution requirements before release.
- Use a central typography scale; do not set arbitrary font sizes screen by screen.

Suggested initial scale (adjust after device review):
- Display: 30–32sp, Bold
- Headline large: 24sp, SemiBold
- Headline medium: 20sp, SemiBold
- Title: 16–18sp, Medium/SemiBold
- Body: 14–16sp, Regular
- Label: 12–14sp, Medium
- Caption: 12sp minimum where possible; do not make essential text tiny

Use `sp` for text and `dp` for layout. Support Android font scaling.

## 4. Shapes and spacing
- Card corner radius: 16–24dp.
- Buttons: 14–18dp radius depending on shape system.
- Text fields: 12–16dp radius.
- Standard spacing scale: 4, 8, 12, 16, 20, 24, 32dp.
- Tap targets: aim for at least 48dp.
- Keep consistent horizontal screen padding (typically 16–20dp).
- Avoid overusing borders, shadows, gradients, and glass effects.

## 5. Components to centralize
Place reusable UI primitives in `core:designsystem`:
- AppButton (primary, secondary, text, destructive)
- AppCard
- AppTopBar
- AppScaffold
- AppTextField
- AppDialog / confirmation dialog
- EmptyState
- ErrorState
- LoadingIndicator
- ProgressRing / progress bar
- SectionHeader
- RoutineTaskRow
- GoalCard (only if truly reusable and domain-neutral enough; otherwise keep feature-specific)
- Accessible icon wrapper
- Date/time picker wrappers if consistent behavior is needed

Avoid a “god component” with many booleans. Prefer a small API and composition slots.

## 6. Iconography
- Use one coherent rounded vector icon family (Material Symbols Rounded or a consistent equivalent).
- Keep icon sizes consistent: navigation 22–24dp; common actions 20–24dp; decorative illustration icons may vary.
- Provide content descriptions for meaningful icons; mark purely decorative imagery as decorative.
- Do not use emoji as the primary icon system.
- Create a centralized `AppIcons` mapping if it simplifies consistency; avoid wrapping every single icon unnecessarily.

## 7. Motion
- Short, subtle transitions and completion feedback.
- No animation should block interaction or delay startup.
- Respect system animation/reduced-motion preferences where possible.
- Breathing visualization must offer a non-animated or reduced-motion alternative.
- Do not animate every card continuously.

## 8. Dark/light theme
Dark theme is the V1 default. If light theme is implemented, define a complete token set rather than inverting dark colors mechanically. Persist theme preference in DataStore.

## 9. Accessibility
- Support font scaling and screen readers.
- Maintain meaningful semantics and content descriptions.
- Do not rely on color alone to communicate status.
- Ensure adequate contrast.
- Support keyboard/IME behavior in journal and text inputs.
- Avoid focus traps and tiny controls.
- Test with TalkBack and large font settings.

## 10. UI quality bar
Screens should feel cohesive and premium, but prioritize speed, readability, and task completion over visual effects. All user-visible strings should be centralized for localization; do not scatter hard-coded strings through composables.
