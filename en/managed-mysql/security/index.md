---
title: "Access management in {{ mmy-full-name }}"
description: "Access management in the {{ MY }} database creation and management service. This section describes the resources for which you can assign a role, the roles existing in the service, and the roles required to perform a particular action."
---

# Access management in {{ mmy-name }}


In this section, you will learn:

* [Which resources you can assign a role for](#resources).
* [Which roles exist in the service](#roles-list).
* [Which roles are required](#required-roles) for particular actions.

{% include [about-access-management](../../_includes/iam/about-access-management.md) %}

## Which resources you can assign a role for {#resources}

{% include [basic-resources](../../_includes/iam/basic-resources-for-access-control.md) %}

To allow access to {{ mmy-name }} service resources (DB clusters and hosts, cluster backups, databases, and their users), assign the user the appropriate roles for the folder or cloud hosting the resources.

## Which roles exist in the service {#roles-list}

The chart below shows which roles are available in the service and how they inherit each other's permissions. For example, the `{{ roles-editor }}` role includes all the permissions of `{{ roles-viewer }}`. You can find the description of each role under the chart.

![image](../../_assets/mdb/roles-managed-mysql.svg)

### Service roles {#service-roles}

#### managed-mysql.auditor {#managed-mysql-auditor}

{% include [managed-mysql.auditor](../../_roles/managed-mysql/auditor.md) %}

#### managed-mysql.viewer {#managed-mysql-viewer}

{% include [managed-mysql.viewer](../../_roles/managed-mysql/viewer.md) %}

#### managed-mysql.editor {#managed-mysql-editor}

{% include [managed-mysql.editor](../../_roles/managed-mysql/editor.md) %}

#### managed-mysql.admin {#managed-mysql-admin}

{% include [managed-mysql.admin](../../_roles/managed-mysql/admin.md) %}

#### mdb.auditor {#mdb-auditor}

{% include [mdb-auditor](../../_roles/mdb/auditor.md) %}

#### mdb.viewer {#mdb-viewer}

{% include [mdb-viewer](../../_roles/mdb/viewer.md) %}

#### mdb.admin {#mdb-admin}

{% include [mdb-admin](../../_roles/mdb/admin.md) %}

#### vpc.publicAdmin {#vpc-public-admin}

{% include [vpc-publicadmin](../../_roles/vpc/publicAdmin.md) %}


### Primitive roles {#primitive-roles}

{% include [roles-primitive](../../_includes/roles-primitive.md) %}

## Roles required {#required-roles}

To use the service, you need the `{{ roles.mmy.editor }}` [role](../../iam/concepts/access-control/roles.md) or higher for the folder where a cluster is created. The `{{ roles.mmy.viewer }}` role only enables you to view the cluster list.

You can always assign a role with more permissions. For instance, you can assign `{{ roles.mmy.admin }}` instead of `{{ roles.mmy.editor }}`.

## What's next {#whats-next}

* [How to assign a role](../../iam/operations/roles/grant.md).
* [How to revoke a role](../../iam/operations/roles/revoke.md).
* [Learn more about access management in {{ yandex-cloud }}](../../iam/concepts/access-control/index.md).
* [Learn more about inheriting roles](../../resource-manager/concepts/resources-hierarchy.md#access-rights-inheritance).

