# Unfinished Pages Inbox

Use this file to drop in rough/unfinished Power Apps page ideas that you want Codex to improve.

## How to use
- Paste one unfinished page draft at a time under a new section.
- Include as much context as you have (goal, data source, controls, known issues).
- Keep any existing sections so we can iterate on versions.

## Suggested section template

~~~markdown
## Page: <name>

### Goal
<what this page should do>

### Current draft
```yaml
# paste your unfinished Power Apps YAML here
```

### Notes / constraints
- <business rules>
- <required data sources>
- <known bugs>

### What to improve
- <layout>
- <performance>
- <accessibility>
~~~

## Page: Workspace View (email triage)

### Goal
Create a unified workspace for email triage with thread review, assignment, deal context, and activity tracking.

### Current draft
```yaml
Screens:
  scr_WorkspaceView:
    Properties:
      Fill: =RGBA(242, 244, 248, 1)
      OnVisible: |+
        =UpdateContext({ctxActiveTab: "Thread"});
        If(
            IsBlank(gblCurrentView),
            Set(gblCurrentView, "email");
            Set(gblThemeMode, "light");
            Set(gblActorName, User().FullName);
            Set(gblCurrentUserId, LookUp(Assignees, email = User().Email, id));
            Set(gblSelectedFolder, "inbox");
            Set(gblSelectedQueue, "all");
            Set(gblSelectedAssigneeFilter, "all");
            Set(gblSearchText, "");
            Set(gblShowReplyComposer, false);
            Set(gblShowComposePopup, false);
            Set(gblShowDealPopup, false);
            Set(gblToastVisible, false);
            Set(gblToastMessage, "")
        );
        ClearCollect(colAssignees, Assignees);
        ClearCollect(colMacroTemplates, Filter(MacroTemplates, isArchived = false));
        ClearCollect(colPipelineStages, PipelineStages);
        ClearCollect(
            colEmails,
            SortByColumns(Filter(Emails, folder = gblSelectedFolder), "receivedAt", SortOrder.Descending)
        );
        ClearCollect(colDeals, Deals);
        ClearCollect(colAssigneeFilters, Table({id: "all", name: "All assignees"}));
        Collect(colAssigneeFilters, colAssignees);
        If(
            Not(IsBlank(First(colEmails))),
            Set(gblSelectedEmailId, First(colEmails).id);
            Set(gblSelectedDealId, First(colEmails).dealId);
            ClearCollect(colCurrentThread, Filter(EmailMessages, emailId = gblSelectedEmailId))
        );

    Children:
      - vcon_appShell:
          Control: GroupContainer@1.5.0
          Variant: AutoLayout
          Properties:
            DropShadow: =DropShadow.None
            Height: =Parent.Height
            LayoutDirection: =LayoutDirection.Vertical
            Width: =Parent.Width
          Children:
            - hcon_topBar:
                Control: GroupContainer@1.5.0
                Variant: AutoLayout
                Properties:
                  Fill: =RGBA(15, 23, 42, 1)
                  FillPortions: =0
                  Height: =56
                  LayoutAlignItems: =LayoutAlignItems.Center
                  LayoutDirection: =LayoutDirection.Horizontal
                  LayoutGap: =8
                  PaddingLeft: =16
                  PaddingRight: =16
                Children:
                  - lbl_logo:
                      Control: Text@0.0.51
                      Properties:
                        FontColor: =RGBA(255, 255, 255, 1)
                        Size: =18
                        Text: ="PipeMailer"
                        VerticalAlign: =VerticalAlign.Middle
                        Weight: ='TextCanvas.Weight'.Bold
                        Width: =130
                  - txt_search:
                      Control: Classic/TextInput@2.3.2
                      Properties:
                        BorderColor: =RGBA(71, 85, 105, 1)
                        Default: =gblSearchText
                        Fill: =RGBA(30, 41, 59, 1)
                        HintText: ="Search emails, deals, RMA…"
                        HoverBorderColor: =RGBA(147, 197, 253, 1)
                        OnChange: =Set(gblSearchText, Self.Text)
                        Width: =300
                  - btn_viewEmail:
                      Control: Button@0.0.45
                      Properties:
                        Appearance: =If(gblCurrentView = "email", 'ButtonCanvas.Appearance'.Primary, 'ButtonCanvas.Appearance'.Secondary)
                        OnSelect: =Set(gblCurrentView, "email")
                        Text: ="Email View"
                        Width: =104
                  - btn_viewPipeline:
                      Control: Button@0.0.45
                      Properties:
                        Appearance: =If(gblCurrentView = "pipeline", 'ButtonCanvas.Appearance'.Primary, 'ButtonCanvas.Appearance'.Secondary)
                        OnSelect: =Set(gblCurrentView, "pipeline")
                        Text: ="Pipeline View"
                        Width: =116
                  - btn_viewDashboard:
                      Control: Button@0.0.45
                      Properties:
                        Appearance: =If(gblCurrentView = "dashboard", 'ButtonCanvas.Appearance'.Primary, 'ButtonCanvas.Appearance'.Secondary)
                        OnSelect: =Set(gblCurrentView, "dashboard")
                        Text: ="Dashboard"
                        Width: =108
                  - btn_compose:
                      Control: Button@0.0.45
                      Properties:
                        BasePaletteColor: =RGBA(59, 130, 246, 1)
                        OnSelect: =Set(gblShowComposePopup, true)
                        Text: ="+ Add Email"
                        Width: =108
                  - btn_createDeal:
                      Control: Button@0.0.45
                      Properties:
                        Appearance: ='ButtonCanvas.Appearance'.Secondary
                        OnSelect: =Set(gblShowDealPopup, true)
                        Text: ="Add Deal / Return"
                        Width: =148
                  - btn_toggleTheme:
                      Control: Button@0.0.45
                      Properties:
                        Appearance: ='ButtonCanvas.Appearance'.Secondary
                        OnSelect: =Set(gblThemeMode, If(gblThemeMode = "light", "dark", "light"))
                        Text: =If(gblThemeMode = "light", "Dark", "Light")
                        Width: =72
            - hcon_body:
                Control: GroupContainer@1.5.0
                Variant: AutoLayout
                Properties:
                  DropShadow: =DropShadow.None
                  LayoutDirection: =LayoutDirection.Horizontal
                Children:
                  - vcon_sidebar:
                      Control: GroupContainer@1.5.0
                      Variant: AutoLayout
                      Properties:
                        DropShadow: =DropShadow.None
                        Fill: =RGBA(248, 250, 252, 1)
                        FillPortions: =0
                        LayoutDirection: =LayoutDirection.Vertical
                        PaddingBottom: =12
                        PaddingTop: =12
                        Width: =200
                      Children:
                        - lbl_navHeading:
                            Control: Text@0.0.51
                            Properties:
                              FontColor: =RGBA(148, 163, 184, 1)
                              Height: =28
                              PaddingLeft: =12
                              Size: =10
                              Text: ="NAVIGATION"
                              VerticalAlign: =VerticalAlign.Middle
                              Weight: ='TextCanvas.Weight'.Bold
                        - gal_folders:
                            Control: Gallery@2.15.0
                            Variant: Vertical
                            Properties:
                              FillPortions: =0
                              Height: =152
                              Items: |-
                                =Table({id: "inbox", label: "Inbox"}, {id: "sent", label: "Sent"}, {id: "drafts", label: "Drafts"}, {id: "archive", label: "Archive"})
                              TemplatePadding: =0
                              TemplateSize: =38
                            Children:
                              - rec_folderBg:
                                  Control: Rectangle@2.3.0
                                  Properties:
                                    Fill: =If(ThisItem.id = gblSelectedFolder, RGBA(219, 234, 254, 1), RGBA(0, 0, 0, 0))
                                    Height: =Parent.TemplateHeight
                                    OnSelect: |-
                                      =Set(gblSelectedFolder, ThisItem.id);
                                      ClearCollect(
                                          colEmails,
                                          SortByColumns(Filter(Emails, folder = gblSelectedFolder), "receivedAt", SortOrder.Descending)
                                      )
                                    Width: =Parent.TemplateWidth
                              - lbl_folderLabel:
                                  Control: Text@0.0.51
                                  Properties:
                                    FontColor: =If(ThisItem.id = gblSelectedFolder, RGBA(29, 78, 216, 1), RGBA(51, 65, 85, 1))
                                    Height: =Parent.TemplateHeight
                                    PaddingLeft: =12
                                    Size: =13
                                    Text: =ThisItem.label
                                    VerticalAlign: =VerticalAlign.Middle
                                    Weight: =If(ThisItem.id = gblSelectedFolder, 'TextCanvas.Weight'.Semibold, 'TextCanvas.Weight'.Regular)
                                    Width: =148
                              - lbl_folderCount:
                                  Control: Text@0.0.51
                                  Properties:
                                    Align: ='TextCanvas.Align'.End
                                    FontColor: =RGBA(148, 163, 184, 1)
                                    Height: =Parent.TemplateHeight
                                    Size: =11
                                    Text: =Text(CountIf(Emails, folder = ThisItem.id))
                                    VerticalAlign: =VerticalAlign.Middle
                                    Width: =34
                                    X: =160
                        - rect_sidebarDiv1:
                            Control: Rectangle@2.3.0
                            Properties:
                              Fill: =RGBA(226, 232, 240, 1)
                              Height: =1
                              Width: =176
                              X: =12
                        - lbl_queuesHeading:
                            Control: Text@0.0.51
                            Properties:
                              FontColor: =RGBA(148, 163, 184, 1)
                              Height: =28
                              PaddingLeft: =12
                              Size: =10
                              Text: ="QUEUES"
                              VerticalAlign: =VerticalAlign.Middle
                              Weight: ='TextCanvas.Weight'.Bold
                        - gal_queues:
                            Control: Gallery@2.15.0
                            Variant: Vertical
                            Properties:
                              FillPortions: =0
                              Height: =152
                              Items: |-
                                =Table({id: "all", label: "All"}, {id: "unassigned", label: "Unassigned"}, {id: "mine", label: "Mine"}, {id: "team", label: "Team"})
                              TemplatePadding: =0
                              TemplateSize: =38
                            Children:
                              - rec_queueBg:
                                  Control: Rectangle@2.3.0
                                  Properties:
                                    Fill: =If(ThisItem.id = gblSelectedQueue, RGBA(219, 234, 254, 1), RGBA(0, 0, 0, 0))
                                    Height: =Parent.TemplateHeight
                                    OnSelect: |-
                                      =Set(gblSelectedQueue, ThisItem.id);
                                      ClearCollect(
                                          colEmails,
                                          SortByColumns(
                                              Filter(
                                                  Emails,
                                                  folder = gblSelectedFolder,
                                                  (gblSelectedQueue = "all") Or
                                                  (gblSelectedQueue = "mine" And assigneeId = gblCurrentUserId) Or
                                                  (gblSelectedQueue = "unassigned" And IsBlank(assigneeId)) Or
                                                  (gblSelectedQueue = "team" And Not(IsBlank(assigneeId)))
                                              ),
                                              "receivedAt", SortOrder.Descending
                                          )
                                      )
                                    Width: =Parent.TemplateWidth
                              - lbl_queueLabel:
                                  Control: Text@0.0.51
                                  Properties:
                                    FontColor: =If(ThisItem.id = gblSelectedQueue, RGBA(29, 78, 216, 1), RGBA(51, 65, 85, 1))
                                    Height: =Parent.TemplateHeight
                                    PaddingLeft: =12
                                    Size: =13
                                    Text: =ThisItem.label
                                    VerticalAlign: =VerticalAlign.Middle
                                    Weight: =If(ThisItem.id = gblSelectedQueue, 'TextCanvas.Weight'.Semibold, 'TextCanvas.Weight'.Regular)
                                    Width: =148
                        - rect_sidebarDiv2:
                            Control: Rectangle@2.3.0
                            Properties:
                              Fill: =RGBA(226, 232, 240, 1)
                              Height: =1
                              Width: =176
                              X: =12
                        - lbl_stagesHeading:
                            Control: Text@0.0.51
                            Properties:
                              FontColor: =RGBA(148, 163, 184, 1)
                              Height: =28
                              PaddingLeft: =12
                              Size: =10
                              Text: ="PIPELINE STAGES"
                              VerticalAlign: =VerticalAlign.Middle
                              Weight: ='TextCanvas.Weight'.Bold
                        - gal_pipelineStages:
                            Control: Gallery@2.15.0
                            Variant: Vertical
                            Properties:
                              Items: =colPipelineStages
                              TemplatePadding: =0
                              TemplateSize: =34
                            Children:
                              - lbl_stageLink:
                                  Control: Text@0.0.51
                                  Properties:
                                    FontColor: =RGBA(59, 130, 246, 1)
                                    Height: =Parent.TemplateHeight
                                    PaddingLeft: =12
                                    Size: =13
                                    Text: =ThisItem.Value
                                    VerticalAlign: =VerticalAlign.Middle
                  - vcon_emailList:
                      Control: GroupContainer@1.5.0
                      Variant: AutoLayout
                      Properties:
                        DropShadow: =DropShadow.None
                        Fill: =RGBA(255, 255, 255, 1)
                        FillPortions: =0
                        LayoutDirection: =LayoutDirection.Vertical
                        Width: =360
                      Children:
                        - gal_assigneeFilters:
                            Control: Gallery@2.15.0
                            Variant: Horizontal
                            Properties:
                              FillPortions: =0
                              Height: =44
                              Items: =colAssigneeFilters
                              TemplatePadding: =6
                              TemplateSize: =110
                            Children:
                              - btn_assigneeChip:
                                  Control: Button@0.0.45
                                  Properties:
                                    Appearance: =If(gblSelectedAssigneeFilter = ThisItem.id, 'ButtonCanvas.Appearance'.Primary, 'ButtonCanvas.Appearance'.Secondary)
                                    BasePaletteColor: =RGBA(59, 130, 246, 1)
                                    Height: =28
                                    OnSelect: =Set(gblSelectedAssigneeFilter, ThisItem.id)
                                    Text: =ThisItem.name
                                    Width: =98
                        - rect_listDivider:
                            Control: Rectangle@2.3.0
                            Properties:
                              AlignInContainer: =AlignInContainer.Stretch
                              Fill: =RGBA(226, 232, 240, 1)
                              Height: =1
                              Width: =1
                        - gal_emails:
                            Control: Gallery@2.15.0
                            Variant: Vertical
                            Properties:
                              Items: |-
                                =Sort(
                                    Filter(
                                        colEmails,
                                        (gblSelectedQueue = "all") Or
                                        (gblSelectedQueue = "mine" And assigneeId = gblCurrentUserId) Or
                                        (gblSelectedQueue = "unassigned" And IsBlank(assigneeId)) Or
                                        (gblSelectedQueue = "team" And Not(IsBlank(assigneeId))),
                                        (gblSelectedAssigneeFilter = "all") Or assigneeId = gblSelectedAssigneeFilter,
                                        IsBlank(gblSearchText) Or (gblSearchText in subject) Or (gblSearchText in from)
                                    ),
                                    receivedAt, SortOrder.Descending
                                )
                              OnSelect: |-
                                =Set(gblSelectedEmailId, ThisItem.id);
                                Set(gblSelectedDealId, ThisItem.dealId);
                                Set(gblShowReplyComposer, false);
                                ClearCollect(colCurrentThread, Filter(EmailMessages, emailId = gblSelectedEmailId));
                                UpdateContext({ctxActiveTab: "Thread"})
                              TemplatePadding: =0
                              TemplateSize: =94
                            Children:
                              - rec_emailRowBg:
                                  Control: Rectangle@2.3.0
                                  Properties:
                                    Fill: =If(ThisItem.id = gblSelectedEmailId, RGBA(239, 246, 255, 1), If(ThisItem.computedSlaStatus = "overdue" Or ThisItem.computedSlaStatus = "breached", RGBA(255, 241, 242, 1), RGBA(255, 255, 255, 1)))
                                    Height: =Parent.TemplateHeight
                                    Width: =Parent.TemplateWidth
                              - rec_unreadDot:
                                  Control: Rectangle@2.3.0
                                  Properties:
                                    Fill: =RGBA(59, 130, 246, 1)
                                    Height: =6
                                    Visible: =Not(ThisItem.isRead)
                                    Width: =6
                                    X: =8
                                    Y: =20
                              - lbl_emailFrom:
                                  Control: Text@0.0.51
                                  Properties:
                                    FontColor: =RGBA(15, 23, 42, 1)
                                    Height: =20
                                    Size: =13
                                    Text: =ThisItem.from
                                    VerticalAlign: =VerticalAlign.Middle
                                    Weight: =If(Not(ThisItem.isRead), 'TextCanvas.Weight'.Bold, 'TextCanvas.Weight'.Normal)
                                    Width: =216
                                    X: =22
                                    Y: =8
                              - lbl_emailDate:
                                  Control: Text@0.0.51
                                  Properties:
                                    Align: ='TextCanvas.Align'.End
                                    FontColor: =RGBA(148, 163, 184, 1)
                                    Height: =20
                                    Size: =11
                                    Text: =ThisItem.relativeDate
                                    VerticalAlign: =VerticalAlign.Middle
                                    Width: =108
                                    X: =244
                                    Y: =8
                              - lbl_emailSubject:
                                  Control: Text@0.0.51
                                  Properties:
                                    FontColor: =RGBA(51, 65, 85, 1)
                                    Height: =18
                                    Size: =12
                                    Text: =ThisItem.subject
                                    Weight: =If(Not(ThisItem.isRead), 'TextCanvas.Weight'.Semibold, 'TextCanvas.Weight'.Normal)
                                    Width: =330
                                    X: =22
                                    Y: =28
                              - lbl_emailSnippet:
                                  Control: Text@0.0.51
                                  Properties:
                                    FontColor: =RGBA(148, 163, 184, 1)
                                    Height: =16
                                    Size: =11
                                    Text: =ThisItem.snippet
                                    Width: =330
                                    X: =22
                                    Y: =46
                              - lbl_assigneeBadge:
                                  Control: Text@0.0.51
                                  Properties:
                                    FontColor: =RGBA(71, 85, 105, 1)
                                    Height: =18
                                    Size: =10
                                    Text: =Coalesce(LookUp(colAssignees, id = ThisItem.assigneeId, name), "Unassigned")
                                    VerticalAlign: =VerticalAlign.Middle
                                    Width: =86
                                    X: =22
                                    Y: =66
                              - lbl_priorityBadge:
                                  Control: Text@0.0.51
                                  Properties:
                                    FontColor: =Switch(ThisItem.priority, "urgent", RGBA(153, 27, 27, 1), "high", RGBA(154, 52, 18, 1), "medium", RGBA(133, 77, 14, 1), RGBA(71, 85, 105, 1))
                                    Height: =18
                                    Size: =10
                                    Text: =Switch(ThisItem.priority, "urgent", "Urgent", "high", "High", "medium", "Medium", "low", "Low", "Medium")
                                    VerticalAlign: =VerticalAlign.Middle
                                    Width: =60
                                    X: =114
                                    Y: =66
                              - lbl_slaBadge:
                                  Control: Text@0.0.51
                                  Properties:
                                    FontColor: =Switch(ThisItem.computedSlaStatus, "atRisk", RGBA(133, 77, 14, 1), "overdue", RGBA(153, 27, 27, 1), "breached", RGBA(127, 29, 29, 1), RGBA(20, 83, 45, 1))
                                    Height: =18
                                    Size: =10
                                    Text: =Switch(ThisItem.computedSlaStatus, "onTrack", "On track", "atRisk", "At risk", "overdue", "Overdue", "breached", "Breached", "On track")
                                    VerticalAlign: =VerticalAlign.Middle
                                    Width: =70
                                    X: =180
                                    Y: =66
                              - lbl_approvalBadge:
                                  Control: Text@0.0.51
                                  Properties:
                                    FontColor: =Switch(ThisItem.approvalStatus, "required", RGBA(120, 53, 15, 1), "approved", RGBA(20, 83, 45, 1), "rejected", RGBA(153, 27, 27, 1), RGBA(0, 0, 0, 1))
                                    Height: =18
                                    Size: =10
                                    Text: =Switch(ThisItem.approvalStatus, "required", "Approval req.", "approved", "Approved", "rejected", "Rejected", "")
                                    VerticalAlign: =VerticalAlign.Middle
                                    Visible: =Not(IsBlank(ThisItem.approvalStatus)) And ThisItem.approvalStatus <> "none"
                                    Width: =86
                                    X: =256
                                    Y: =66
                              - rect_emailRowDivider:
                                  Control: Rectangle@2.3.0
                                  Properties:
                                    Fill: =RGBA(241, 245, 249, 1)
                                    Height: =1
                                    Width: =Parent.TemplateWidth
                                    Y: =Parent.TemplateHeight - 1
                  - vcon_rightPanel:
                      Control: GroupContainer@1.5.0
                      Variant: AutoLayout
                      Properties:
                        DropShadow: =DropShadow.None
                        Fill: =RGBA(255, 255, 255, 1)
                        LayoutDirection: =LayoutDirection.Vertical
                      Children:
                        - hcon_tabStrip:
                            Control: GroupContainer@1.5.0
                            Variant: AutoLayout
                            Properties:
                              Fill: =RGBA(248, 250, 252, 1)
                              FillPortions: =0
                              Height: =44
                              LayoutAlignItems: =LayoutAlignItems.End
                              LayoutDirection: =LayoutDirection.Horizontal
                            Children:
                              - btn_tabThread:
                                  Control: Button@0.0.45
                                  Properties:
                                    Appearance: ='ButtonCanvas.Appearance'.Transparent
                                    FontColor: =If(ctxActiveTab = "Thread", RGBA(29, 78, 216, 1), RGBA(100, 116, 139, 1))
                                    Height: =44
                                    OnSelect: |-
                                      =UpdateContext({ctxActiveTab: "Thread"})
                                    Text: ="Thread"
                              - rect_threadUnderline:
                                  Control: Rectangle@2.3.0
                                  Properties:
                                    Fill: =If(ctxActiveTab = "Thread", RGBA(59, 130, 246, 1), RGBA(0, 0, 0, 0))
                                    Height: =3
                                    Width: =96
                                    Y: =41
                              - btn_tabAssignment:
                                  Control: Button@0.0.45
                                  Properties:
                                    Appearance: ='ButtonCanvas.Appearance'.Transparent
                                    FontColor: =If(ctxActiveTab = "Assignment", RGBA(29, 78, 216, 1), RGBA(100, 116, 139, 1))
                                    Height: =44
                                    OnSelect: |-
                                      =UpdateContext({ctxActiveTab: "Assignment"})
                                    Text: ="Assignment"
                                    Width: =108
                              - rect_assignmentUnderline:
                                  Control: Rectangle@2.3.0
                                  Properties:
                                    Fill: =If(ctxActiveTab = "Assignment", RGBA(59, 130, 246, 1), RGBA(0, 0, 0, 0))
                                    Height: =3
                                    Width: =108
                                    Y: =41
                              - btn_tabDeal:
                                  Control: Button@0.0.45
                                  Properties:
                                    Appearance: ='ButtonCanvas.Appearance'.Transparent
                                    FontColor: =If(ctxActiveTab = "Deal", RGBA(29, 78, 216, 1), RGBA(100, 116, 139, 1))
                                    Height: =44
                                    OnSelect: |-
                                      =UpdateContext({ctxActiveTab: "Deal"})
                                    Text: ="Deal"
                                    Width: =80
                              - rect_dealUnderline:
                                  Control: Rectangle@2.3.0
                                  Properties:
                                    Fill: =If(ctxActiveTab = "Deal", RGBA(59, 130, 246, 1), RGBA(0, 0, 0, 0))
                                    Height: =3
                                    Width: =80
                                    Y: =41
                              - btn_tabActivity:
                                  Control: Button@0.0.45
                                  Properties:
                                    Appearance: ='ButtonCanvas.Appearance'.Transparent
                                    FontColor: =If(ctxActiveTab = "Activity", RGBA(29, 78, 216, 1), RGBA(100, 116, 139, 1))
                                    Height: =44
                                    OnSelect: |-
                                      =UpdateContext({ctxActiveTab: "Activity"})
                                    Text: ="Activity"
                                    Width: =88
                              - rect_activityUnderline:
                                  Control: Rectangle@2.3.0
                                  Properties:
                                    Fill: =If(ctxActiveTab = "Activity", RGBA(59, 130, 246, 1), RGBA(0, 0, 0, 0))
                                    Height: =3
                                    Width: =88
                                    Y: =41
                        - rect_tabStripBorder:
                            Control: Rectangle@2.3.0
                            Properties:
                              AlignInContainer: =AlignInContainer.Stretch
                              Fill: =RGBA(226, 232, 240, 1)
                              Height: =1
                              Width: =1
                        - vcon_tabThread:
                            Control: GroupContainer@1.5.0
                            Variant: AutoLayout
                            Properties:
                              DropShadow: =DropShadow.None
                              LayoutDirection: =LayoutDirection.Vertical
                              Visible: =ctxActiveTab = "Thread"
                            Children:
                              - lbl_threadEmpty:
                                  Control: Text@0.0.51
                                  Properties:
                                    Align: ='TextCanvas.Align'.Center
                                    FontColor: =RGBA(148, 163, 184, 1)
                                    Text: ="Select an email to view the full thread."
                                    VerticalAlign: =VerticalAlign.Middle
                                    Visible: =IsBlank(gblSelectedEmailId)
                              - hcon_threadActions:
                                  Control: GroupContainer@1.5.0
                                  Variant: AutoLayout
                                  Properties:
                                    Fill: =RGBA(248, 250, 252, 1)
                                    FillPortions: =0
                                    Height: =44
                                    LayoutAlignItems: =LayoutAlignItems.Center
                                    LayoutDirection: =LayoutDirection.Horizontal
                                    LayoutGap: =8
                                    PaddingLeft: =12
                                    Visible: =Not(IsBlank(gblSelectedEmailId))
                                  Children:
                                    - btn_reply:
                                        Control: Button@0.0.45
                                        Properties:
                                          BasePaletteColor: =RGBA(59, 130, 246, 1)
                                          Height: =28
                                          OnSelect: |-
                                            =Set(gblShowReplyComposer, true);
                                            Set(gblReplyMode, "reply")
                                          Text: ="Reply"
                                          Width: =80
                                    - btn_replyAll:
                                        Control: Button@0.0.45
                                        Properties:
                                          Appearance: ='ButtonCanvas.Appearance'.Secondary
                                          Height: =28
                                          OnSelect: |-
                                            =Set(gblShowReplyComposer, true);
                                            Set(gblReplyMode, "replyAll")
                                          Text: ="Reply all"
                                          Width: =80
                              - con_threadMeta:
                                  Control: GroupContainer@1.5.0
                                  Variant: AutoLayout
                                  Properties:
                                    DropShadow: =DropShadow.None
                                    Fill: =RGBA(248, 250, 252, 1)
                                    FillPortions: =0
                                    LayoutDirection: =LayoutDirection.Vertical
                                    LayoutGap: =4
                                    PaddingBottom: =10
                                    PaddingLeft: =16
                                    PaddingRight: =16
                                    PaddingTop: =10
                                    Visible: =Not(IsBlank(gblSelectedEmailId))
                                  Children:
                                    - lbl_metaSubject:
                                        Control: Text@0.0.51
                                        Properties:
                                          FontColor: =RGBA(51, 65, 85, 1)
                                          Height: =18
                                          Size: =12
                                          Text: |-
                                            ="Subject: " & LookUp(colEmails, id = gblSelectedEmailId, subject)
                                    - lbl_metaDeal:
                                        Control: Text@0.0.51
                                        Properties:
                                          FontColor: =If(IsBlank(gblSelectedDealId), RGBA(148, 163, 184, 1), RGBA(51, 65, 85, 1))
                                          Height: =18
                                          Size: =12
                                          Text: |-
                                            ="Linked deal: " & Coalesce(LookUp(colDeals, id = gblSelectedDealId, title), "Unlinked")
                                    - lbl_metaSla:
                                        Control: Text@0.0.51
                                        Properties:
                                          FontColor: =RGBA(51, 65, 85, 1)
                                          Height: =18
                                          Size: =12
                                          Text: |-
                                            ="SLA: " & Switch(LookUp(colEmails, id = gblSelectedEmailId, computedSlaStatus), "onTrack", "On track", "atRisk", "At risk", "overdue", "Overdue", "breached", "Breached", "On track")
                                    - lbl_metaDue:
                                        Control: Text@0.0.51
                                        Properties:
                                          FontColor: =RGBA(51, 65, 85, 1)
                                          Height: =18
                                          Size: =12
                                          Text: |-
                                            ="Resolution due: " & Text(LookUp(colEmails, id = gblSelectedEmailId, resolutionDueAt), "dd/mm/yyyy HH:mm")
                              - gal_thread:
                                  Control: Gallery@2.15.0
                                  Variant: Vertical
                                  Properties:
                                    Items: =colCurrentThread
                                    TemplatePadding: =8
                                    TemplateSize: =120
                                    Visible: =Not(IsBlank(gblSelectedEmailId))
                                  Children:
                                    - rec_msgBg:
                                        Control: Rectangle@2.3.0
                                        Properties:
                                          Fill: =RGBA(248, 250, 252, 1)
                                          Height: =Parent.TemplateHeight - 16
                                          Width: =Parent.TemplateWidth - 16
                                          X: =8
                                          Y: =8
                                    - lbl_msgFrom:
                                        Control: Text@0.0.51
                                        Properties:
                                          FontColor: =RGBA(15, 23, 42, 1)
                                          Height: =18
                                          Size: =12
                                          Text: =ThisItem.from
                                          Weight: ='TextCanvas.Weight'.Semibold
                                          Width: =200
                                          X: =20
                                          Y: =16
                                    - lbl_msgAt:
                                        Control: Text@0.0.51
                                        Properties:
                                          Align: ='TextCanvas.Align'.End
                                          FontColor: =RGBA(148, 163, 184, 1)
                                          Height: =18
                                          Size: =11
                                          Text: =Text(DateTimeValue(ThisItem.at), "dd mmm yyyy HH:mm")
                                          Width: =Parent.TemplateWidth - 240
                                          X: =Parent.TemplateWidth - Self.Width - 12
                                          Y: =16
                                    - lbl_msgBody:
                                        Control: Text@0.0.51
                                        Properties:
                                          FontColor: =RGBA(51, 65, 85, 1)
                                          Height: =Parent.TemplateHeight - 48
                                          Size: =12
                                          Text: =ThisItem.body
                                          Width: =Parent.TemplateWidth - 40
                                          X: =20
                                          Y: =38
                              - con_replyComposer:
                                  Control: GroupContainer@1.5.0
                                  Variant: AutoLayout
                                  Properties:
                                    DropShadow: =DropShadow.None
                                    Fill: =RGBA(248, 250, 252, 1)
                                    FillPortions: =0
                                    LayoutDirection: =LayoutDirection.Vertical
                                    LayoutGap: =8
                                    PaddingBottom: =12
                                    PaddingLeft: =12
                                    PaddingRight: =12
                                    PaddingTop: =12
                                    Visible: =gblShowReplyComposer And Not(IsBlank(gblSelectedEmailId))
                                  Children:
                                    - txt_replyTo:
                                        Control: Classic/TextInput@2.3.2
                                        Properties:
                                          AlignInContainer: =AlignInContainer.Stretch
                                          Default: =LookUp(colEmails, id = gblSelectedEmailId, from)
                                          Height: =32
                                          HintText: ="To:"
                                    - txt_replySubject:
                                        Control: Classic/TextInput@2.3.2
                                        Properties:
                                          AlignInContainer: =AlignInContainer.Stretch
                                          Default: |-
                                            ="Re: " & LookUp(colEmails, id = gblSelectedEmailId, subject)
                                          Height: =32
                                          HintText: ="Subject:"
                                    - txt_replyBody:
                                        Control: Classic/TextInput@2.3.2
                                        Properties:
                                          AlignInContainer: =AlignInContainer.Stretch
                                          Height: =80
                                          HintText: ="Type your reply…"
                                          Mode: =TextMode.MultiLine
                                    - hcon_replyActions:
                                        Control: GroupContainer@1.5.0
                                        Variant: AutoLayout
                                        Properties:
                                          DropShadow: =DropShadow.None
                                          FillPortions: =0
                                          Height: =36
                                          LayoutAlignItems: =LayoutAlignItems.Center
                                          LayoutDirection: =LayoutDirection.Horizontal
                                          LayoutGap: =8
                                        Children:
                                          - btn_sendReply:
                                              Control: Button@0.0.45
                                              Properties:
                                                BasePaletteColor: =RGBA(59, 130, 246, 1)
                                                OnSelect: |-
                                                  =Patch(Emails, LookUp(Emails, id = gblSelectedEmailId), {lastReplyAt: Now(), isRead: true});
                                                  Patch(EmailMessages, Defaults(EmailMessages), {emailId: gblSelectedEmailId, from: gblActorName, at: Text(Now(), "yyyy-mm-ddTHH:mm:ssZ"), body: txt_replyBody.Text, direction: "outbound"});
                                                  ClearCollect(colCurrentThread, Filter(EmailMessages, emailId = gblSelectedEmailId));
                                                  Set(gblShowReplyComposer, false);
                                                  Set(gblToastMessage, "Reply sent.");
                                                  Set(gblToastVisible, true);
                                                  Timer_Toast.Start()
                                                Text: ="Send"
                                                Width: =80
                                          - btn_cancelReply:
                                              Control: Button@0.0.45
                                              Properties:
                                                Appearance: ='ButtonCanvas.Appearance'.Secondary
                                                OnSelect: =Set(gblShowReplyComposer, false)
                                                Text: ="Cancel"
                                                Width: =80
                        - vcon_tabAssignment:
                            Control: GroupContainer@1.5.0
                            Variant: AutoLayout
                            Properties:
                              DropShadow: =DropShadow.None
                              LayoutDirection: =LayoutDirection.Vertical
                              LayoutGap: =8
                              PaddingBottom: =16
                              PaddingLeft: =16
                              PaddingRight: =16
                              PaddingTop: =16
                              Visible: =ctxActiveTab = "Assignment"
                            Children:
                              - lbl_assignEmpty:
                                  Control: Text@0.0.51
                                  Properties:
                                    Align: ='TextCanvas.Align'.Center
                                    FontColor: =RGBA(148, 163, 184, 1)
                                    Text: ="No thread selected."
                                    Visible: =IsBlank(gblSelectedEmailId)
                              - lbl_ownerLabel:
                                  Control: Text@0.0.51
                                  Properties:
                                    FontColor: =RGBA(100, 116, 139, 1)
                                    Height: =18
                                    Size: =11
                                    Text: ="Current owner"
                                    Visible: =Not(IsBlank(gblSelectedEmailId))
                              - lbl_ownerValue:
                                  Control: Text@0.0.51
                                  Properties:
                                    FontColor: =RGBA(15, 23, 42, 1)
                                    Height: =22
                                    Size: =13
                                    Text: =Coalesce(LookUp(colAssignees, id = LookUp(colEmails, id = gblSelectedEmailId, assigneeId), name), "Unassigned")
                                    Visible: =Not(IsBlank(gblSelectedEmailId))
                                    Weight: ='TextCanvas.Weight'.Semibold
                              - lbl_assignedAtLabel:
                                  Control: Text@0.0.51
                                  Properties:
                                    FontColor: =RGBA(100, 116, 139, 1)
                                    Height: =18
                                    Size: =11
                                    Text: ="Assigned at"
                                    Visible: =Not(IsBlank(gblSelectedEmailId))
                              - lbl_assignedAtValue:
                                  Control: Text@0.0.51
                                  Properties:
                                    FontColor: =RGBA(51, 65, 85, 1)
                                    Height: =18
                                    Size: =12
                                    Text: =Coalesce(Text(LookUp(colEmails, id = gblSelectedEmailId, assignedAt), "dd/mm/yyyy HH:mm"), "—")
                                    Visible: =Not(IsBlank(gblSelectedEmailId))
                              - hcon_assignControls:
                                  Control: GroupContainer@1.5.0
                                  Variant: AutoLayout
                                  Properties:
                                    DropShadow: =DropShadow.None
                                    FillPortions: =0
                                    Height: =36
                                    LayoutAlignItems: =LayoutAlignItems.Center
                                    LayoutDirection: =LayoutDirection.Horizontal
                                    LayoutGap: =8
                                    Visible: =Not(IsBlank(gblSelectedEmailId))
                                  Children:
                                    - drp_assigneeSelect:
                                        Control: ComboBox@0.0.51
                                        Properties:
                                          Items: =colAssignees
                                          LayoutMinWidth: =200
                                          OnChange: =Set(gblStagedAssigneeId, LookUp(colAssignees, name = Self.SelectedText.Value, id))
                                    - btn_applyAssign:
                                        Control: Button@0.0.45
                                        Properties:
                                          BasePaletteColor: =RGBA(59, 130, 246, 1)
                                          OnSelect: |-
                                            =If(
                                                Not(IsBlank(gblStagedAssigneeId)),
                                                Patch(Emails, LookUp(Emails, id = gblSelectedEmailId), {assigneeId: gblStagedAssigneeId, assignedAt: Now()});
                                                Patch(colEmails, LookUp(colEmails, id = gblSelectedEmailId), {assigneeId: gblStagedAssigneeId, assignedAt: Text(Now(), "dd/mm/yyyy HH:mm")});
                                                Set(gblToastMessage, "Assignee updated.");
                                                Set(gblToastVisible, true);
                                                Timer_Toast.Start()
                                            )
                                          Text: =If(IsBlank(LookUp(colEmails, id = gblSelectedEmailId, assigneeId)), "Assign", "Reassign")
                                          Width: =88
                              - rect_assignHistDivider:
                                  Control: Rectangle@2.3.0
                                  Properties:
                                    AlignInContainer: =AlignInContainer.Stretch
                                    Fill: =RGBA(226, 232, 240, 1)
                                    Height: =1
                                    Visible: =Not(IsBlank(gblSelectedEmailId))
                                    Width: =1
                              - lbl_assignHistHeading:
                                  Control: Text@0.0.51
                                  Properties:
                                    FontColor: =RGBA(148, 163, 184, 1)
                                    Height: =20
                                    Size: =10
                                    Text: ="ASSIGNMENT HISTORY"
                                    Visible: =Not(IsBlank(gblSelectedEmailId))
                                    Weight: ='TextCanvas.Weight'.Bold
                              - gal_assignHistory:
                                  Control: Gallery@2.15.0
                                  Variant: Vertical
                                  Properties:
                                    Items: =Filter(EmailAssignments, emailId = gblSelectedEmailId)
                                    TemplatePadding: =0
                                    TemplateSize: =36
                                    Visible: =Not(IsBlank(gblSelectedEmailId))
                                  Children:
                                    - lbl_histName:
                                        Control: Text@0.0.51
                                        Properties:
                                          FontColor: =RGBA(51, 65, 85, 1)
                                          Height: =Parent.TemplateHeight
                                          Size: =12
                                          Text: =Coalesce(LookUp(colAssignees, id = ThisItem.assigneeId, name), "—")
                                          VerticalAlign: =VerticalAlign.Middle
                                          Width: =200
                                    - lbl_histAt:
                                        Control: Text@0.0.51
                                        Properties:
                                          Align: ='TextCanvas.Align'.End
                                          FontColor: =RGBA(148, 163, 184, 1)
                                          Height: =Parent.TemplateHeight
                                          Size: =11
                                          Text: =Text(ThisItem.assignedAt, "dd/mm/yyyy HH:mm")
                                          VerticalAlign: =VerticalAlign.Middle
                                          X: =208
                        - vcon_tabDeal:
                            Control: GroupContainer@1.5.0
                            Variant: AutoLayout
                            Properties:
                              DropShadow: =DropShadow.None
                              LayoutDirection: =LayoutDirection.Vertical
                              LayoutGap: =8
                              PaddingBottom: =16
                              PaddingLeft: =16
                              PaddingRight: =16
                              PaddingTop: =16
                              Visible: =ctxActiveTab = "Deal"
                            Children:
                              - lbl_dealEmpty:
                                  Control: Text@0.0.51
                                  Properties:
                                    Align: ='TextCanvas.Align'.Center
                                    FontColor: =RGBA(148, 163, 184, 1)
                                    Text: ="No linked deal."
                                    Visible: =IsBlank(gblSelectedDealId)
                              - lbl_dealTitle:
                                  Control: Text@0.0.51
                                  Properties:
                                    FontColor: =RGBA(15, 23, 42, 1)
                                    Height: =28
                                    Size: =16
                                    Text: =LookUp(colDeals, id = gblSelectedDealId, title)
                                    Visible: =Not(IsBlank(gblSelectedDealId))
                                    Weight: ='TextCanvas.Weight'.Bold
                              - lbl_dealContact:
                                  Control: Text@0.0.51
                                  Properties:
                                    FontColor: =RGBA(100, 116, 139, 1)
                                    Height: =20
                                    Size: =12
                                    Text: =LookUp(colDeals, id = gblSelectedDealId, contact)
                                    Visible: =Not(IsBlank(gblSelectedDealId))
                              - con_dealKpis:
                                  Control: GroupContainer@1.5.0
                                  Variant: AutoLayout
                                  Properties:
                                    DropShadow: =DropShadow.None
                                    Fill: =RGBA(248, 250, 252, 1)
                                    FillPortions: =0
                                    LayoutDirection: =LayoutDirection.Vertical
                                    LayoutGap: =6
                                    PaddingBottom: =12
                                    PaddingLeft: =16
                                    PaddingRight: =16
                                    PaddingTop: =12
                                    Visible: =Not(IsBlank(gblSelectedDealId))
                                  Children:
                                    - lbl_dealStage:
                                        Control: Text@0.0.51
                                        Properties:
                                          FontColor: =RGBA(51, 65, 85, 1)
                                          Height: =18
                                          Size: =12
                                          Text: |-
                                            ="Stage: " & LookUp(colDeals, id = gblSelectedDealId, stage)
                                    - lbl_dealValue:
                                        Control: Text@0.0.51
                                        Properties:
                                          FontColor: =RGBA(15, 23, 42, 1)
                                          Height: =18
                                          Size: =12
                                          Text: |-
                                            ="Value: £" & Text(LookUp(colDeals, id = gblSelectedDealId, value), "[$-en-GB]#,###")
                                          Weight: ='TextCanvas.Weight'.Semibold
                                    - lbl_dealProb:
                                        Control: Text@0.0.51
                                        Properties:
                                          FontColor: =RGBA(51, 65, 85, 1)
                                          Height: =18
                                          Size: =12
                                          Text: |-
                                            ="Probability: " & Text(LookUp(colDeals, id = gblSelectedDealId, probability)) & "%"
                              - lbl_returnHeading:
                                  Control: Text@0.0.51
                                  Properties:
                                    FontColor: =RGBA(148, 163, 184, 1)
                                    Height: =20
                                    Size: =10
                                    Text: ="RETURN CASE"
                                    Visible: =Not(IsBlank(LookUp(ReturnCases, dealId = gblSelectedDealId)))
                                    Weight: ='TextCanvas.Weight'.Bold
                              - con_returnMeta:
                                  Control: GroupContainer@1.5.0
                                  Variant: AutoLayout
                                  Properties:
                                    DropShadow: =DropShadow.None
                                    Fill: =RGBA(248, 250, 252, 1)
                                    LayoutDirection: =LayoutDirection.Vertical
                                    LayoutGap: =4
                                    PaddingBottom: =12
                                    PaddingLeft: =16
                                    PaddingRight: =16
                                    PaddingTop: =12
                                    Visible: =Not(IsBlank(LookUp(ReturnCases, dealId = gblSelectedDealId)))
                                  Children:
                                    - lbl_rma:
                                        Control: Text@0.0.51
                                        Properties:
                                          Height: =18
                                          Size: =12
                                          Text: |-
                                            ="RMA: " & LookUp(ReturnCases, dealId = gblSelectedDealId, rmaNumber)
                                    - lbl_order:
                                        Control: Text@0.0.51
                                        Properties:
                                          Height: =18
                                          Size: =12
                                          Text: |-
                                            ="Order: " & LookUp(ReturnCases, dealId = gblSelectedDealId, orderNumber)
                                    - lbl_sku:
                                        Control: Text@0.0.51
                                        Properties:
                                          Height: =18
                                          Size: =12
                                          Text: |-
                                            ="SKU: " & LookUp(ReturnCases, dealId = gblSelectedDealId, sku)
                                    - lbl_qty:
                                        Control: Text@0.0.51
                                        Properties:
                                          Height: =18
                                          Size: =12
                                          Text: |-
                                            ="Qty: " & Text(LookUp(ReturnCases, dealId = gblSelectedDealId, quantity))
                                    - lbl_returnReason:
                                        Control: Text@0.0.51
                                        Properties:
                                          Height: =18
                                          Size: =12
                                          Text: |-
                                            ="Reason: " & LookUp(ReturnCases, dealId = gblSelectedDealId, returnReason)
                                    - lbl_condition:
                                        Control: Text@0.0.51
                                        Properties:
                                          Height: =18
                                          Size: =12
                                          Text: |-
                                            ="Condition: " & LookUp(ReturnCases, dealId = gblSelectedDealId, condition)
                                    - lbl_disposition:
                                        Control: Text@0.0.51
                                        Properties:
                                          Height: =18
                                          Size: =12
                                          Text: |-
                                            ="Disposition: " & LookUp(ReturnCases, dealId = gblSelectedDealId, disposition)
                                    - lbl_refundAmount:
                                        Control: Text@0.0.51
                                        Properties:
                                          FontColor: =RGBA(15, 23, 42, 1)
                                          Height: =18
                                          Size: =12
                                          Text: |-
                                            ="Refund: £" & Text(LookUp(ReturnCases, dealId = gblSelectedDealId, refundAmount), "[$-en-GB]#,###.##")
                                          Weight: ='TextCanvas.Weight'.Semibold
                        - vcon_tabActivity:
                            Control: GroupContainer@1.5.0
                            Variant: AutoLayout
                            Properties:
                              DropShadow: =DropShadow.None
                              LayoutDirection: =LayoutDirection.Vertical
                              LayoutGap: =8
                              PaddingBottom: =16
                              PaddingLeft: =16
                              PaddingRight: =16
                              PaddingTop: =16
                              Visible: =ctxActiveTab = "Activity"
                            Children:
                              - lbl_activityEmpty:
                                  Control: Text@0.0.51
                                  Properties:
                                    Align: ='TextCanvas.Align'.Center
                                    FontColor: =RGBA(148, 163, 184, 1)
                                    Text: ="No activity yet for this thread."
                                    Visible: =IsBlank(gblSelectedEmailId)
                              - lbl_internalActLabel:
                                  Control: Text@0.0.51
                                  Properties:
                                    FontColor: =RGBA(148, 163, 184, 1)
                                    Height: =18
                                    Size: =10
                                    Text: ="INTERNAL ACTIONS"
                                    Visible: =Not(IsBlank(gblSelectedEmailId))
                                    Weight: ='TextCanvas.Weight'.Bold
                              - txt_internalNote:
                                  Control: Classic/TextInput@2.3.2
                                  Properties:
                                    AlignInContainer: =AlignInContainer.Stretch
                                    Height: =60
                                    HintText: ="Add an internal note (not visible to customer)…"
                                    Mode: =TextMode.MultiLine
                                    Visible: =Not(IsBlank(gblSelectedEmailId))
                              - btn_addNote:
                                  Control: Button@0.0.45
                                  Properties:
                                    Appearance: ='ButtonCanvas.Appearance'.Secondary
                                    BasePaletteColor: =RGBA(59, 130, 246, 1)
                                    OnSelect: |-
                                      =If(
                                          IsBlank(txt_internalNote.Text),
                                          Notify("Note cannot be empty.", NotificationType.Warning),
                                          Patch(EmailActivities, Defaults(EmailActivities), {emailId: gblSelectedEmailId, actorName: gblActorName, type: "internal-note", visibility: "internal", body: txt_internalNote.Text, timestamp: Now()});
                                          Reset(txt_internalNote);
                                          Set(gblToastMessage, "Note added.");
                                          Set(gblToastVisible, true);
                                          Timer_Toast.Start()
                                      )
                                    Text: ="Add internal note"
                                    Visible: =Not(IsBlank(gblSelectedEmailId))
                                    Width: =140
                              - txt_approvalContext:
                                  Control: Classic/TextInput@2.3.2
                                  Properties:
                                    AlignInContainer: =AlignInContainer.Stretch
                                    Height: =60
                                    HintText: ="Approval context or decision comment…"
                                    Mode: =TextMode.MultiLine
                                    Visible: =Not(IsBlank(gblSelectedEmailId))
                              - hcon_approvalButtons:
                                  Control: GroupContainer@1.5.0
                                  Variant: AutoLayout
                                  Properties:
                                    DropShadow: =DropShadow.None
                                    FillPortions: =0
                                    Height: =36
                                    LayoutAlignItems: =LayoutAlignItems.Center
                                    LayoutDirection: =LayoutDirection.Horizontal
                                    LayoutGap: =8
                                    Visible: =Not(IsBlank(gblSelectedEmailId))
                                  Children:
                                    - btn_requestApproval:
                                        Control: Button@0.0.45
                                        Properties:
                                          BasePaletteColor: =RGBA(251, 191, 36, 1)
                                          OnSelect: |-
                                            =Patch(Emails, LookUp(Emails, id = gblSelectedEmailId), {approvalStatus: "required"});
                                            Patch(colEmails, LookUp(colEmails, id = gblSelectedEmailId), {approvalStatus: "required"});
                                            Set(gblToastMessage, "Approval requested.");
                                            Set(gblToastVisible, true);
                                            Timer_Toast.Start()
                                          Text: ="Request approval"
                                          Width: =136
                                    - btn_approve:
                                        Control: Button@0.0.45
                                        Properties:
                                          BasePaletteColor: =RGBA(34, 197, 94, 1)
                                          OnSelect: |-
                                            =Patch(Emails, LookUp(Emails, id = gblSelectedEmailId), {approvalStatus: "approved"});
                                            Patch(colEmails, LookUp(colEmails, id = gblSelectedEmailId), {approvalStatus: "approved"});
                                            Set(gblToastMessage, "Approved.");
                                            Set(gblToastVisible, true);
                                            Timer_Toast.Start()
                                          Text: ="Approve"
                                          Width: =80
                                    - btn_reject:
                                        Control: Button@0.0.45
                                        Properties:
                                          Appearance: ='ButtonCanvas.Appearance'.Secondary
                                          OnSelect: |-
                                            =Patch(Emails, LookUp(Emails, id = gblSelectedEmailId), {approvalStatus: "rejected"});
                                            Patch(colEmails, LookUp(colEmails, id = gblSelectedEmailId), {approvalStatus: "rejected"});
                                            Set(gblToastMessage, "Rejected.");
                                            Set(gblToastVisible, true);
                                            Timer_Toast.Start()
                                          Text: ="Reject"
                                          Width: =80
                              - rect_timelineDivider:
                                  Control: Rectangle@2.3.0
                                  Properties:
                                    AlignInContainer: =AlignInContainer.Stretch
                                    Fill: =RGBA(226, 232, 240, 1)
                                    Height: =1
                                    Visible: =Not(IsBlank(gblSelectedEmailId))
                                    Width: =1
                              - lbl_timelineHeading:
                                  Control: Text@0.0.51
                                  Properties:
                                    FontColor: =RGBA(148, 163, 184, 1)
                                    Height: =18
                                    Size: =10
                                    Text: ="ACTIVITY TIMELINE"
                                    Visible: =Not(IsBlank(gblSelectedEmailId))
                                    Weight: ='TextCanvas.Weight'.Bold
                              - gal_activityTimeline:
                                  Control: Gallery@2.15.0
                                  Variant: Vertical
                                  Properties:
                                    Items: =SortByColumns(Filter(EmailActivities, emailId = gblSelectedEmailId), "timestamp", SortOrder.Ascending)
                                    TemplatePadding: =4
                                    TemplateSize: =96
                                    Visible: =Not(IsBlank(gblSelectedEmailId))
                                  Children:
                                    - rec_tlBg:
                                        Control: Rectangle@2.3.0
                                        Properties:
                                          Fill: =If(ThisItem.visibility = "internal", RGBA(254, 249, 195, 1), RGBA(248, 250, 252, 1))
                                          Height: =Parent.TemplateHeight - 8
                                          Width: =Parent.TemplateWidth
                                          Y: =4
                                    - lbl_tlActor:
                                        Control: Text@0.0.51
                                        Properties:
                                          FontColor: =RGBA(15, 23, 42, 1)
                                          Height: =18
                                          Size: =12
                                          Text: =Coalesce(ThisItem.actorName, "System")
                                          Weight: ='TextCanvas.Weight'.Semibold
                                          Width: =180
                                          X: =10
                                          Y: =12
                                    - lbl_tlVisibility:
                                        Control: Text@0.0.51
                                        Properties:
                                          FontColor: =If(ThisItem.visibility = "internal", RGBA(133, 77, 14, 1), RGBA(20, 83, 45, 1))
                                          Height: =16
                                          Size: =10
                                          Text: =If(ThisItem.visibility = "internal", "Internal", "Public")
                                          VerticalAlign: =VerticalAlign.Middle
                                          Width: =60
                                          X: =196
                                          Y: =13
                                    - lbl_tlTimestamp:
                                        Control: Text@0.0.51
                                        Properties:
                                          Align: ='TextCanvas.Align'.End
                                          FontColor: =RGBA(148, 163, 184, 1)
                                          Height: =18
                                          Size: =11
                                          Text: =Text(ThisItem.timestamp, "dd/mm HH:mm")
                                          Width: =86
                                          X: =Parent.TemplateWidth - 96
                                          Y: =12
                                    - lbl_tlBody:
                                        Control: Text@0.0.51
                                        Properties:
                                          FontColor: =RGBA(51, 65, 85, 1)
                                          Height: =Parent.TemplateHeight - 46
                                          Size: =12
                                          Text: =ThisItem.body
                                          Width: =Parent.TemplateWidth - 20
                                          X: =10
                                          Y: =34
      - con_toast:
          Control: GroupContainer@1.5.0
          Variant: AutoLayout
          Properties:
            DropShadow: =DropShadow.Regular
            Fill: =RGBA(15, 23, 42, 0.92)
            Height: =48
            LayoutAlignItems: =LayoutAlignItems.Center
            LayoutDirection: =LayoutDirection.Horizontal
            PaddingLeft: =16
            Visible: =gblToastVisible
            Width: =300
            X: =Parent.Width - 316
            Y: =Parent.Height - 64
          Children:
            - lbl_toastMessage:
                Control: Text@0.0.51
                Properties:
                  FontColor: =RGBA(248, 250, 252, 1)
                  Size: =13
                  Text: =gblToastMessage
                  VerticalAlign: =VerticalAlign.Middle
      - Timer_Toast:
          Control: Timer@2.1.0
          Properties:
            Duration: =2500
            OnTimerEnd: |-
              =Set(gblToastVisible, false);
              Set(gblToastMessage, "")
            Visible: =false

```

### Notes / constraints
- Added to the unfinished inbox to track ongoing work on this page in the AI branch.
- Treat this draft as work-in-progress before promotion to finalized examples.

### What to improve
- Stabilize data operations and state updates across filters/tabs.
- Improve responsiveness and accessibility for dense triage workflows.
- Add validation and edge-case handling for assignment/reply/activity actions.

