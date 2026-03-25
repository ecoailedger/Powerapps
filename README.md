# PowerApps Patterns Repo

## Quick Start

This repository collects practical building blocks for Power Apps development:

- **Canvas YAML examples** for reusable UI and interaction patterns.
- **Power Fx references** and snippets for common formulas and behaviors.
- **PCF references** for component-level patterns and supporting guidance.

### YAML example destinations

Place new Canvas YAML examples in the exact destination folder that matches the pattern category:

- `examples/yaml/galleries/`
- `examples/yaml/forms/`
- `examples/yaml/navigation/`
- `examples/yaml/modals/`
- `examples/yaml/tabs/`
- `examples/yaml/responsive/`

## How to contribute a YAML example

Use this checklist before opening a PR:

- [ ] Choose the correct destination folder under `examples/yaml/`.
- [ ] Follow the relevant pattern guidance in `docs/yaml-patterns/`.
- [ ] Keep the example focused on a single reusable pattern.
- [ ] Verify the file name follows the naming convention below.
- [ ] Add or update companion docs in `docs/yaml-patterns/` when introducing a new pattern.

## Naming convention

Use **kebab-case** file names with **pattern-oriented** names.

- ✅ Good: `modal-confirm-delete.yaml`
- ✅ Good: `form-inline-validation.yaml`
- ❌ Avoid: `MyModal.yaml`, `example1.yaml`, `test.yaml`

Naming format recommendation:

`<pattern>-<intent>-<variant>.yaml`

Examples:

- `gallery-filter-search.yaml`
- `navigation-left-rail.yaml`
- `tabs-sticky-header.yaml`
