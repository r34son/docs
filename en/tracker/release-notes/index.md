---
title: "{{ tracker-full-name }} release notes for January 2024"
description: "Check out {{ tracker-full-name }} release notes for January 2024."
---

# {{ tracker-full-name }} release notes: January 2024

* [Updates](#top-news)
* [Fixes and improvements](#fixes)

## Updates {#top-news}

### Filters and sorting in portfolios {#project-filters}

On the [portfolio]({{ link-tracker }}pages/projects/list) page, you can now configure filters and sort results by portfolio and project in the **Projects** and **Gantt chart** tabs.

### Copying widgets on dashboards {#clone-widget}

The new interface allows [copying widgets](../user/copy-widget.md) both within the current dashboard and between dashboards.

### API update for projects and portfolios {#portfolio-projects-api}

You can now use the new, more flexible and functional API to work with checklists, comments, attachments, history, and links in projects and portfolios:

* [Add](../concepts/entities/checklists/add-checklist.md), [edit](../concepts/entities/checklists/patch-checklist.md), and [delete](../concepts/entities/checklists/delete-checklist.md) checklists and their individual items.
* [Add](../concepts/entities/comments/add-comment.md), [edit](../concepts/entities/comments/patch-comment.md), and [delete](../concepts/entities/comments/delete-comment.md) comments and [get a list](../concepts/entities/comments/get-all-comments.md) of comments.
* [Add](../concepts/entities/attachments/add-attachment.md) and [delete](../concepts/entities/attachments/delete-attachment.md) attached files. You can also [get information](../concepts/entities/attachments/get-attachment.md) about a specific attachment or [a list](../concepts/entities/attachments/get-all-attachments.md) of all files attached to an entity.
* [Add](../concepts/entities/links/add-links.md) and [delete](../concepts/entities/links/delete-link.md) links between entities.

### Local fields in queue settings {#queue-settings-local-fields}

The new interface now has a [local field](../local-fields.md) management page in the queue settings. There, you can view a list of local fields, create new fields, and edit the categories of the existing ones.

## Fixes and improvements {#fixes}


### Configuring access to creating issues without a form {#task-creating-access}

In the new interface, you can now select access rules for creating issues without a form under **Integration** in the queue settings:

* Allowed for all
* Allowed for the queue team
* Prohibited

### Managing queue access privileges in components {#queue-access-management}

On the settings page for queue access privileges, you can now quickly disable a component's influence on access privileges. To do so, go to **Access rights** in the queue settings, open the component you need, and click ![](../../_assets/console-icons/ellipsis.svg) → **Clear component access rights**. This will delete from the component all previously configured access privileges, i.e., members, issue roles, denied access. The component will no longer affect access to issues.

### Templates for project and portfolio comments {#templates}

You can now use [comment templates](../user/ticket-template.md) in portfolios and projects.

### Warning when adding an issue into a project {#add-task-warning}

When you add existing issues and projects into projects and portfolios, the service checks whether they belong to another project or portfolio. If that is the case, you will see a warning on transferring them to a new entity and deleting them from the old one.
