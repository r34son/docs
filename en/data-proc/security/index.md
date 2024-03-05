---
title: "Access management in {{ dataproc-full-name }}"
description: "Access management in the service for creation and management of Apache Hadoop® and Apache Spark™ clusters. To allow access to {{ dataproc-name }} resources (clusters and subclusters), assign the user the required roles from the list below."
---

# Access management in {{ dataproc-name }}

{{ yandex-cloud }} users can only perform operations on resources that are allowed by the roles assigned to them. If a user does not have any roles assigned, almost all operations are forbidden.

To allow access to {{ dataproc-name }} resources (clusters or subclusters), assign the Yandex account, [service account](../../iam/concepts/users/service-accounts.md), [federated users](../../iam/concepts/federations.md), [user group](../../organization/operations/manage-groups.md), or [system group](../../iam/concepts/access-control/system-group.md) the required roles from the list below. Currently, a role can only be assigned to a parent resource (folder or cloud). Roles are inherited by nested resources.

Only users with the `admin`, `resource-manager.clouds.owner`, or `organization-manager.organizations.owner` role for a resource can assign roles for this resource.

{% note info %}

For more information about role inheritance, see [{#T}](../../resource-manager/concepts/resources-hierarchy.md#access-rights-inheritance) in the {{ resmgr-full-name }} documentation.

{% endnote %}

## Assigning roles {#grant-role}

To assign a user a role:

{% include [grant-role-console](../../_includes/grant-role-console.md) %}

## Which roles exist in the service {#roles-list}

The list below shows all roles that are considered when verifying access rights in the {{ dataproc-name }} service.

### Service roles {#service-roles}

#### dataproc.agent {#dataproc-agent}

{% include [dataproc.agent](../../_roles/dataproc/agent.md) %}

#### mdb.dataproc.agent {#mdb-dataproc-agent}

{% include [mdb.dataproc.agent](../../_roles/mdb/dataproc/agent.md) %}

#### dataproc.auditor {#dataproc-auditor}

{% include [dataproc.auditor](../../_roles/dataproc/auditor.md) %}

#### dataproc.viewer {#dataproc-viewer}

{% include [dataproc.viewer](../../_roles/dataproc/viewer.md) %}

#### dataproc.user {#dataproc-user}

{% include [dataproc.user](../../_roles/dataproc/user.md) %}

#### dataproc.provisioner {#dataproc-provisioner}

{% include [dataproc.provisioner](../../_roles/dataproc/provisioner.md) %}

#### dataproc.editor {#dataproc-editor}

{% include [dataproc.editor](../../_roles/dataproc/editor.md) %}

#### dataproc.admin {#dataproc-admin}

{% include [dataproc.admin](../../_roles/dataproc/admin.md) %}

#### managed-metastore.auditor {#managed-metastore-auditor}

{% include [managed-metastore.auditor](../../_roles/managed-metastore/auditor.md) %}

#### managed-metastore.viewer {#managed-metastore-viewer}

{% include [managed-metastore.viewer](../../_roles/managed-metastore/viewer.md) %}

#### managed-metastore.editor {#managed-metastore-editor}

{% include [managed-metastore.editor](../../_roles/managed-metastore/editor.md) %}

#### managed-metastore.admin {#managed-metastore-admin}

{% include [managed-metastore.admin](../../_roles/managed-metastore/admin.md) %}

#### mdb.auditor {#mdb-auditor}

{% include [mdb-auditor](../../_roles/mdb/auditor.md) %}

#### mdb.viewer {#mdb-viewer}

{% include [mdb-viewer](../../_roles/mdb/viewer.md) %}

#### mdb.admin {#mdb-admin}

{% include [mdb-admin](../../_roles/mdb/admin.md) %}

### Primitive roles {#primitive-roles}

{% include [roles-primitive](../../_includes/roles-primitive.md) %}
