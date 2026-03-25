# Attachment Pattern: SharePoint Upload via Power Automate

## Summary
This pattern defines a reusable `comAttachments` Canvas component that collects files from an Attachments control and sends each file to a Power Automate flow (`Upload-Attachment-To-SharePoint`).

## Use case
Use this component when a Canvas app needs users to upload one or more supporting files and route them to SharePoint through a flow (for example ROI evidence, email evidence, or plans).

## Inputs and assumptions
- A Power Automate flow named `Upload-Attachment-To-SharePoint` exists and accepts the argument structure shown in `OnAddFile`.
- App context variables such as `gvar_Selected_Fund` are set before uploading.
- Evidence type comes from `ComboboxCanvas2_1`.

## Behaviour
- Validates that at least one file is selected.
- Validates that an evidence type is selected.
- Iterates attachments with `ForAll(...)` and invokes the flow for each file.
- Shows success notification and resets attachment/type controls.

## Constraints and notes
- This sample keeps current variable names and flow parameter order exactly as provided.
- The preview gallery (`gal_attachmentsHold_1`) is currently hidden (`Visible: =false`).
- The example is intended as a reusable pattern and may require adaptation to your app schema and flow contract.

## Example file
- `examples/yaml/forms/attachments-sharepoint-flow.yaml`
