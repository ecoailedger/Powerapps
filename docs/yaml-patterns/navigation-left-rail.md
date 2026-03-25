# Navigation Pattern: Left Rail Gallery Component

## Summary
Reusable `comNavigation` pattern for fixed-width, left-rail navigation driven by a gallery table of label/target records.

## Use case
Use when an app has stable top-level sections and should keep persistent side navigation (for example Funds, Jobs, Reports).

## Inputs
- Destination screens referenced by each menu row (`ThisItem.Target`).
- Menu item table maintained in `Gallery1.Items`.
- Host screen/container where the component is placed.

## Outputs/behavior
- Renders one selectable row per configured menu item.
- Applies active-state styling when `App.ActiveScreen` matches the row target.
- Navigates to the selected destination with `Navigate(ThisItem.Target)` behavior.

## Constraints
- The sample uses a static in-component table; dynamic menu data sources require adaptation.
- Optional visual gradient container is hidden by default (`Visible: false`).
- Some positional/layout properties are intentionally blank due to Canvas export semantics.

## Example file path
- `examples/yaml/navigation/navigation-left-rail-gallery.yaml`
