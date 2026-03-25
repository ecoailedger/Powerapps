# YAML Control Property Guardrails

## Rectangle controls
- For `Rectangle@2.3.0`, do **not** use corner radius properties:
  - `RadiusTopLeft`
  - `RadiusTopRight`
  - `RadiusBottomLeft`
  - `RadiusBottomRight`
- These properties are not valid for that control type in this repo's Canvas YAML usage and should be avoided in all future examples.

## Icon controls
- Prefer `Classic/Icon@2.5.0` when using standard classic icon properties (such as `Color`, `Icon`, `Height`, and `Width`).
- Avoid mixing non-classic icon control declarations with classic-only properties.
