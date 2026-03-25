# Attachment Pattern: SharePoint Upload via Power Automate

## Summary
Reusable Canvas component pattern (`comAttachments`) for collecting one or more files and uploading each file to SharePoint through a Power Automate flow.

## Use case
Use when a Canvas app needs controlled evidence/document uploads (for example ROI evidence, email evidence, plans) with per-file flow execution.

## Inputs
- Power Automate flow `Upload-Attachment-To-SharePoint` with the same argument order used in `OnAddFile`.
- App variables/context expected by the formula (for example `gvar_Selected_Fund`).
- Evidence type selected from `ComboboxCanvas2_1`.

## Outputs/behavior
- Validates that at least one attachment exists.
- Validates that an attachment type is selected.
- Executes `ForAll(att_attachments_1.Attachments, 'Upload-Attachment-To-SharePoint'.Run(...))` to upload each file.
- Shows a success notification with uploaded count.
- Resets attachments and evidence-type controls after upload.

## Constraints
- Flow contract and parameter ordering must stay aligned with the example formula.
- Current implementation depends on app-specific global variables and selected record context.
- Hidden preview gallery (`gal_attachmentsHold_1`, `Visible: false`) is optional and may be removed.

## Example file path
- `examples/yaml/forms/attachments-sharepoint-flow.yaml`
