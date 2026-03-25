# Tabs Pattern: Record Details with Lazy Data Load

## Summary
Reusable tabbed detail-screen pattern that switches visible content containers by selected tab value and performs conditional data loading when specific tabs are opened.

## Use case
Use for complex record-detail pages where users need to move between related sections (for example Job, Cost Line Items, Purchase Order Allocations) without leaving the screen.

## Inputs
- `TabList` control configuration (`Items`, `DefaultSelectedItems`, selected value checks).
- Record context (`gvar_Selected_FundJob`) and data source procedure for allocations.
- Screen-level state initialized in `OnVisible`.
- Container visibility formulas keyed to `tabPO_3.Selected.Value`.

## Outputs/behavior
- Shows or hides each content region based on active tab.
- Uses tab `OnSelect` to conditionally refresh allocation data when the allocations tab is selected.
- Preserves a single-page detail experience while segmenting content into focused panes.

## Constraints
- Tab labels in `Items` must exactly match all visibility and conditional formulas.
- Data-load logic is tied to app-specific stored-procedure calls and variable names.
- Very large per-tab layouts should be split into components to reduce complexity.

## Example file path
- `examples/yaml/tabs/tabs-record-details.yaml`
