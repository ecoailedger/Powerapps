# Header Pattern: Contextual Screen Header with User Profile

## Summary
Reusable `comHeader` pattern for a top app bar that combines back navigation, dynamic screen title/description mappings, and signed-in user profile context.

## Use case
Use for multi-screen Canvas apps that need a single standardized header with:
- conditional back navigation,
- centralized screen metadata text,
- user identity details (name, title, avatar).

## Inputs
- Boolean context variable `varShowUserCtx` to toggle compact/expanded header state.
- Theme object values such as `varTheme.Main.BackgroundColor`.
- Office 365 Users connector for `MyProfileV2()` and `UserPhotoV2(...)`.
- Valid screen-name mapping in `Switch(App.ActiveScreen.Name, ...)`.

## Outputs/behavior
- Header height toggles between compact and expanded states.
- Back button is hidden on home and shown on non-home screens.
- Title and description are selected from screen-name `Switch(...)` mappings.
- User name, job title, and avatar render from profile connector calls.

## Constraints
- `Switch(...)` mappings are static and must be updated as screens change.
- The sample references a specific image asset name and app-specific theme variables.
- Blank layout fields are intentionally preserved from Canvas YAML export format.

## Example file path
- `examples/yaml/navigation/header-user-context-toggle.yaml`
