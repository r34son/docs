---
title: "How to create a project in {{ tracker-full-name }}"
description: "In this tutorial, you will learn how to create a project in {{ tracker-name }}."
---

# How to create a project

## Creating a project {#create-project}

You can create a project from scratch or based on an issue.

### New project {#new-project}

To create a project:

1. Go to any page a project can be created from:

   * In the left-hand panel, select ![](../../_assets/tracker/svg/project.svg)&nbsp;**Projects** and click **Create** → **Project**.
   * In the [list of portfolios and projects](my-projects.md) of the **List** tab, click **Create project** under the list.

1. Enter a name for the project. Try to make it short and clear to give a clue to the project.

1. On the **About the project** tab, add the information:

   * Project description: what are you working on and what goals you have.
   * Attachments: working materials of the project.
   * Checklist: a list of milestones or goals of the project.
   * **Status**: Specify the current stage of the project.
   * **Start date** and **End date**.
   * **Responsible**, **Clients**, and **Participants**: Start typing the name or login of the employee and select a relevant option from the list.
   * **Tags**: Add or select the tags that would make it easier to find the project.

1. [Add](#add-tickets) issues to the project. You can add issues from the project page or from the issue page, as well as by using [bulk editing](bulk-change.md).

   The project is not linked to any issue queue: that is why you can add issues to it from any queue that you have access to. Access to project issues depends on the [access settings](../user/queue.md) of the queue that the issue belongs to.

   {% note info %}

   * You cannot add the same issue to multiple projects.
   * You can add no more than 2,000 issues to a project.

   {% endnote %}

1. A new project is available to all the organization's employees by default. To restrict access to the project, in the upper-right corner of the page, click the lock icon and select **Members only**. In this case, the project will only be available to the users listed in the fields: **Participants**, **Reporter**, **Clients**, and **Responsible**.

### Converting an issue to a project {#convert-from-task}

The converted issue will be added to a project and the new project will show its parameters:
* Name, description, checklist.
* Start date and end date.
* Reporter, assignee, and followers.

Issue comments are converted to project comments and displayed in the **About the project** tab.

To create a project based on an issue:

1. Open the issue page. The issue should not belong to another project. To delete an issue from the project, clear the **{{ ui-key.startrek-backend.fields.issue.project-key-value }}** field in the right-hand panel.

1. In the top-right corner, select **{{ ui-key.startrek.ui_components_issue-actions_IssueMenu.title }}** → **{{ ui-key.startrek.ui_components_issue-actions_IssueMenu.convert-to-project }}**, then click **Convert**.

## Adding issues to a project {#add-tickets}

#### From the project page {#from-project}

1. Go to the **Issue list** tab, then click **{{ ui-key.startrek.ui_components_projects_Table.add-issue }}**.

1. To create a new issue:

   1. Select **New issue**.
   1. Select the name of the issue, select the queue, then click **Enter**.

1. To add an existing issue:

   1. Select **Existing issue**.
   1. Start typing the issue's key or name, then pick the option you need from the list.

   {% note alert %}

   An issue may only belong to one project. Adding an issue from another project means it will be moved to the new project.

   {% endnote %}

#### From the issue page {#from-ticket}

1. Open the issue page.

1. Click the **{{ ui-key.startrek-backend.fields.issue.project-key-value }}** field in the right-hand panel. If you do not see the **{{ ui-key.startrek-backend.fields.issue.project-key-value }}** field, add it by clicking **{{ ui-key.startrek.ui_components_issues-import_IssuesImportFilters.add-parameters }}**.

1. Start typing the project's name in the **{{ ui-key.startrek-backend.fields.issue.project-key-value }}** field and pick the option you need from the list of suggestions.

#### Adding multiple issues {#from-bulk}

1. Select the issues you need using [filters](../user/create-filter.md).

1. Select the issues that you want to add to the project.

1. In the left-hand panel, click ![](../../_assets/horizontal-ellipsis.svg) and select **{{ ui-key.startrek.ui_components_IssueBulkActionPanel.add-to-projects }}**.

1. Start typing the project name and pick the option you need from the list that appears.

1. Wait for the issues to be processed.

#### Importing issues {#from-import}

1. Open your project page.

1. In the top-right corner, click **Import issues**.

1. Click ![](../../_assets/tracker/svg/add-task.svg)&nbsp;**{{ ui-key.startrek.ui_components_issues-import_IssuesImportFilters.add-parameters }}** and specify an issue selection criterion.

1. Click **{{ ui-key.startrek.ui_components_issues-import_IssuesImportDialog.import }}** and wait until your issues are imported.

## Deleting a project {#delete}

{% note alert %}

A project can be deleted by its author or the user specified in the **Responsible** field.

{% endnote %}

To delete a project:

1. In the left-hand panel, select ![](../../_assets/tracker/svg/project.svg)&nbsp;**Projects** or follow the [direct link]({{ link-tracker }}pages/projects/list) and open the project page.

1. In the top-right corner of the page, click ![](../../_assets/horizontal-ellipsis.svg) and select **Delete project**.