# Issue types

A queue may include different types of issues, e.g., **New feature**, **Improvement**, or **Bug**. Issue types help you distribute work among team members by activity (**New feature**, **Bug**, or **Improvement**) or decompose issues ([Epic](epic-new.md), **Story**, or **Issue**).

The queue owner and users granted [permission to access the queue settings](queue-access.md) specify:

* Queue issue types
* [Resolutions](create-resolution.md) for each issue type
* [Workflows](add-workflow.md) for each issue type

In {{ tracker-name }}, there are system issue types that are listed in the [{{ tracker-name }} settings]({{ link-admin-types }}). You can use these issue types in any queues. If you do not find a suitable issue type among the system ones, a {{ tracker-name }} [administrator](../role-model.md) can create a new issue type. It will appear in the issue type list and users will be able to use it in their queues.

## Adding an issue type to a queue {#add-ticket-type-queue}

To add a new issue type to the queue:

1. Go to the queue page.

1. In the top-right corner, click ![](../../_assets/tracker/svg/settings-old.svg) **{{ ui-key.startrek.ui_components_PageQueue_header.settings }}**.

1. Go to the **{{ ui-key.startrek.ui_components_page-queue-admin_QueueAdminPageContent.menu-item-workflows }}** tab. It shows the list of workflows in the queue and the issue types and resolutions set in each workflow.

1. To add an issue type to an existing workflow, click **Add issue type** in the respective workflow section and select the appropriate issue type from the list of system issue types. To learn how to add an issue type to a new workflow, see the [guide on creating workflows](add-workflow.md#create).

To set the default type for a queue issue, select one of the options in the {{ ui-key.startrek.ui_components_queue-admin-tab-workflows_WorkflowsStateList.default-type-title }} field.

## Creating an issue type {#create-ticket-type}

{% note warning %}

Only an [administrator](../role-model.md) can create new issue types. Once the issue type has been created, you cannot delete or edit it.

{% endnote %}

To create a new issue type:

1. Make sure you are authorized in {{ tracker-full-name }} as an [administrator](../role-model.md).

1. In the left-hand panel, select ![](../../_assets/tracker/svg/admin.svg) **Administration** → **Issue types**.

1. Click **Add issue type**.

1. Fill out the fields below:
   * **Name**: Enter a name for your new issue type in Russian.
   * **Name in English**: It will be generated automatically based on the Russian name.
   * **Key**: It will be generated automatically based on the Russian name.

1. Click **Create**.

You can also create a new issue type on the workflows page. The created issue type will be immediately added to the current queue's workflow. However, you can also use it in other queues.

1. Go to the queue page.

1. In the top-right corner, click ![](../../_assets/tracker/svg/settings-old.svg) **{{ ui-key.startrek.ui_components_PageQueue_header.settings }}**.

1. Go to the **{{ ui-key.startrek.ui_components_page-queue-admin_QueueAdminPageContent.menu-item-workflows }}** tab. It shows the list of workflows in the queue. Select the workflow to add the new issue type to and click ![](../../_assets/tracker/svg/arrow.svg) in the respective workflow line.

1. Click **Add issue type**, start typing the name of the new issue type, and click the create button.

1. Fill out the fields below:
   * **Name**: Enter a name for your new issue type in Russian.
   * **Name in English**: It will be generated automatically based on the Russian name.
   * **Key**: It will be generated automatically based on the Russian name.

1. Click **Create**.

## Removing an issue type from the queue {#delete-ticket-type}

Before removing an issue type from the queue, make sure it is not used in any of the queue issues.

1. Open the queue page.

1. Use filters to [find all the queue issues](quick-filters.md) that belong to the type you want to remove and change it to any other type the queue supports.

1. In the top-right corner of the queue page, click ![](../../_assets/tracker/svg/settings-old.svg) **{{ ui-key.startrek.ui_components_PageQueue_header.settings }}**.

1. Go to the **{{ ui-key.startrek.ui_components_page-queue-admin_QueueAdminPageContent.menu-item-workflows }}** tab. It shows the list of workflows in the queue. Select the workflow containing the issue type you want to remove.

1. Hover over the line with the issue type to be removed, click the issue type and then click ![](../../_assets/tracker/svg/actions.svg) → **Remove issue type from the queue** in the respective workflow line.
