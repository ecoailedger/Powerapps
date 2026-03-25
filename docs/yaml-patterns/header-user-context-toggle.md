# Header Pattern: Contextual Screen Header with User Profile

## Summary
This pattern defines a reusable `comHeader` Canvas component that combines app navigation, dynamic screen titles/descriptions, and user context (name, title, avatar).

## Use case
Use this component at the top of screens when you want a single header pattern that:
- shows back navigation except on home,
- shows per-screen title/description text,
- shows signed-in user identity details.

## Inputs and assumptions
- `varShowUserCtx` controls expanded/collapsed header height.
- `varTheme.Main.BackgroundColor` is available for theming.
- Office 365 Users connector is configured for `MyProfileV2()` and `UserPhotoV2(...)`.
- Screen names used in `Switch(...)` match your app (`scrHome`, `scrPO`, `scrReports`, etc.).

## Behaviour
- Header height toggles between `60` and `312` via `varShowUserCtx`.
- Back button appears on all screens except `scrHome`.
- Title and description text are selected from `Switch(App.ActiveScreen.Name, ...)` mappings.
- User full name, job title, and avatar are rendered from user/context functions.

## Constraints and notes
- Current title/description mappings are static in the component and should be updated as screens evolve.
- Component references a specific image asset (`'ChatGPT Image Sep 16, 2025, 10_22_10 AM'`).
- Several layout fields are intentionally blank to reflect Canvas export format.

## Example file
- `examples/yaml/navigation/header-user-context-toggle.yaml`
