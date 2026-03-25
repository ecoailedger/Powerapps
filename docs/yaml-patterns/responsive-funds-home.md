# Responsive Pattern: Funds Home Layout

## Summary
Reusable responsive home-screen scaffold pattern that combines a fixed left rail with fluid main content and auto-layout containers for list-centric pages.

## Use case
Use for dashboard/list screens that need to scale across desktop/tablet widths while keeping consistent spacing, filters, and action controls.

## Inputs
- Theme tokens/variables such as `varTheme.Main.*` and `varTheme.Body.Container.*`.
- Navigation component `comNavigation`.
- Data collections used by controls (for example `ColFunds` for filtering).
- Context variables initialized in `OnVisible` for modal state and column sizing (`w_*` values).

## Outputs/behavior
- Builds a two-column responsive shell: fixed-width sidebar + flexible main area.
- Initializes local UI state and reusable column-width variables in `OnVisible`.
- Provides header actions (search, created-by filter, reset, add-fund action) in a responsive toolbar.
- Uses auto-layout groups to keep headers/content aligned as available width changes.

## Constraints
- Depends on app-specific variables, theme schema, and data collections.
- Width tuning (`w_*`) is manually maintained and may need recalibration by device profile.
- Includes commented logic placeholders that should be either implemented or removed in production.

## Example file path
- `examples/yaml/responsive/responsive-funds-home.yaml`
