# Navigation Pattern: Left Rail Gallery Component

## Summary
This pattern defines a reusable Canvas component (`comNavigation`) that renders a fixed-width left navigation rail with a menu gallery. The active destination is visually highlighted and each row navigates to a target screen.

## Use case
Use this pattern when an app has a stable set of top-level sections (for example Funds, Jobs, Reports) and benefits from a persistent left-side menu.

## Inputs and assumptions
- The app contains screen objects referenced in the table (for example `scrHome`, `scrJobs`, `scrReports`).
- The host app imports the component and places it on screens where navigation is needed.
- Menu item labels and destinations are maintained directly in the `Gallery1.Items` table.

## Behaviour
- The gallery renders one button per menu item.
- The currently active screen is highlighted using fill/color/font-weight styles.
- Selecting a menu item navigates to `ThisItem.Target`.

## Constraints and notes
- The HTML gradient container is currently hidden (`Visible: =false`), so the visual gradient is disabled unless that property is changed.
- The sample keeps a static table for simplicity; data-driven navigation can replace the table when needed.
- `Size`, `X`, and `Y` are intentionally left blank in places to mirror Canvas designer output.

## Example file
- `examples/yaml/navigation/navigation-left-rail-gallery.yaml`
