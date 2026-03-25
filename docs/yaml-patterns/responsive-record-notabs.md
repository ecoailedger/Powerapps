# Responsive Pattern: Record Detail (No Tabs)

## Summary
Reusable responsive record-detail layout pattern for a single-page detail experience without tab controls, using stacked auto-layout sections and scrollable content regions.

## Use case
Use when a detail screen should present one primary record context and related sections in a continuous layout rather than tabbed panes.

## Inputs
- Selected record/global context variables (for example `gvar_Selected_PO`, `gvar_currentPeriodRecord`).
- Theme variables (`varTheme.Main.*`).
- Back-end procedures/collections used during screen initialization (for example allocation loading in `OnVisible`).
- Local UI state context fields (`showPoDrawer`, `sortBy`, `selStatus`, etc.).

## Outputs/behavior
- Initializes page UI state and formatting helpers on screen entry.
- Loads related allocation data for the active record.
- Renders a fixed left navigation rail and a flexible detail body.
- Uses nested auto-layout containers and scroll overflow for long form/detail sections.

## Constraints
- Strongly coupled to app-specific globals, connectors, and stored-procedure contracts.
- Formula defaults and context initialization order are important for screen stability.
- Large nested container structure can increase maintenance cost if not modularized.

## Example file path
- `examples/yaml/responsive/responsive-record-notabs.yaml`
