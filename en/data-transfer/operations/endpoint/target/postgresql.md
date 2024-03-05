---
title: "How to configure a {{ PG }} target endpoint in {{ data-transfer-full-name }}"
description: "In this tutorial, you will learn how to set up a {{ PG }} target endpoint in {{ data-transfer-full-name }}."
---
# Transferring data to a {{ PG }} target endpoint

{{ data-transfer-full-name }} enables you to migrate data to a {{ PG }} database and implement various scenarios of data transfer, processing and transformation. To implement a transfer:

1. [Explore possible data transfer scenarios](#scenarios).
1. [Configure one of the supported data sources](#supported-sources).
1. [Prepare the {{ PG }}](#prepare) database for the transfer.
1. [Configure the target endpoint](#endpoint-settings) in {{ data-transfer-full-name }}.
1. [Create](../../transfer.md#create) a transfer and [start](../../transfer.md#activate) it.
1. [Perform required operations with the database](../../../../_includes/data-transfer/endpoints/sources/pg-work-with-db.md) and [control the transfer](../../monitoring.md).
1. In case of any issues, [use ready-made solutions](#troubleshooting) to resolve them.

## Scenarios for transferring data to {{ PG }} {#scenarios}

1. {% include [migration](../../../../_includes/data-transfer/scenario-captions/migration.md) %}

   * [Migrating the {{ PG }} cluster](../../../tutorials/managed-postgresql.md).
   * [Migrating data from AWS RDS for {{ PG }}](../../../tutorials/rds-to-mpg.md).
   * [Migration with storage changed from {{ MY }} to {{ PG }}](../../../tutorials/mmy-to-mpg.md).

1. {% include [queue](../../../../_includes/data-transfer/scenario-captions/queue.md) %}

   * [Delivering data from {{ KF }} to {{ PG }}](../../../tutorials/mkf-to-mpg.md).

1. {% include [data-mart](../../../../_includes/data-transfer/scenario-captions/data-mart.md) %}

   * [Loading data from {{ GP }} to {{ PG }}](../../../tutorials/greenplum-to-postgresql.md).
   * [Loading data from {{ objstorage-name }} to {{ PG }}](../../../tutorials/object-storage-to-postgresql.md).

For a detailed description of possible {{ data-transfer-full-name }} data transfer scenarios, see [Tutorials](../../../tutorials/index.md).

## Configuring the data source {#supported-sources}

Configure one of the supported data sources:

* [{{ PG }}](../source/postgresql.md).
* [{{ MY }}](../source/mysql.md).
* [{{ GP }}](../source/greenplum.md).
* [{{ objstorage-full-name }}](../source/object-storage.md).
* [{{ KF }}](../source/kafka.md).
* [Airbyte](../../../transfer-matrix.md#airbyte).
* [{{ DS }}](../source/data-streams.md).
* [{{ ydb-name }}](../source/ydb.md).
* [Oracle](../source/oracle.md).

For a complete list of supported sources and targets in {{ data-transfer-full-name }}, see [Available Transfers](../../../transfer-matrix.md).

## Preparing the target database {#prepare}

{% include [prepare db](../../../../_includes/data-transfer/endpoints/targets/pg-prepare.md) %}

## Configuring the {{ PG }} target endpoint {#endpoint-settings}

When [creating](../index.md#create) or [editing](../index.md#update) an endpoint, you can define:

* [{{ mpg-full-name }} cluster](#managed-service) connection or [custom installation](#on-premise) settings, including those based on {{ compute-full-name }} VMs. These are required parameters.
* [Additional parameters](#additional-settings).

### {{ mpg-name }} cluster {#managed-service}


{% note warning %}

To create or edit an endpoint of a managed database, you need to have the [`{{ roles.mpg.viewer }}` role](../../../../managed-postgresql/security/index.md#mpg-viewer) or the [`viewer` primitive role](../../../../iam/roles-reference.md#viewer) assigned for the folder where this managed database cluster resides.

{% endnote %}


Connecting to the database with the cluster ID specified in {{ yandex-cloud }}.

{% list tabs group=instructions %}

- Management console {#console}

   {% include [Managed Postgresql](../../../../_includes/data-transfer/necessary-settings/ui/managed-postgresql.md) %}

- CLI {#cli}

   * Endpoint type: `postgres-target`.

   {% include [Managed PostgreSQL CLI](../../../../_includes/data-transfer/necessary-settings/cli/managed-postgresql.md) %}

- {{ TF }} {#tf}

   * Endpoint type: `postgres_target`.

   {% include [Managed PostgreSQL Terraform](../../../../_includes/data-transfer/necessary-settings/terraform/managed-postgresql.md) %}

   Here is an example of the configuration file structure:

   
   ```hcl
   resource "yandex_datatransfer_endpoint" "<endpoint_name_in_{{ TF }}>" {
     name = "<endpoint_name>"
     settings {
       postgres_target {
         security_groups = ["<list_of_security_group_IDs>"]
         connection {
           mdb_cluster_id = "<cluster_ID>"
         }
         database = "<migrated_database_name>"
         user     = "<username_for_connection>"
         password {
           raw = "<user_password>"
         }
       }
     }
   }
   ```


   For more information, see the [{{ TF }} provider documentation]({{ tf-provider-dt-endpoint }}).

- API {#api}

   {% include [Managed PostgreSQL API](../../../../_includes/data-transfer/necessary-settings/api/managed-postgresql.md) %}

{% endlist %}

### Custom installation {#on-premise}

For OnPremise, all fields are filled in manually.

{% list tabs group=instructions %}

- Management console {#console}

   {% include [On premise Postgresql](../../../../_includes/data-transfer/necessary-settings/ui/on-premise-postgresql.md) %}

- CLI {#cli}

   * Endpoint type: `postgres-target`.

   {% include [On premise PostgreSQL CLI](../../../../_includes/data-transfer/necessary-settings/cli/on-premise-postgresql.md) %}

- {{ TF }} {#tf}

   * Endpoint type: `postgres_target`.

   {% include [On premise PostgreSQL Terraform](../../../../_includes/data-transfer/necessary-settings/terraform/on-premise-postgresql.md) %}

   Here is an example of the configuration file structure:

   
   ```hcl
   resource "yandex_datatransfer_endpoint" "<endpoint_name_in_{{ TF }}>" {
     name = "<endpoint_name>"
     settings {
       postgres_target {
         security_groups = ["<list_of_security_group_IDs>"]
         connection {
           on_premise {
             hosts = ["<list_of_hosts>"]
             port  = <port_for_connection>
           }
         }
         database = "<migrated_database_name>"
         user     = "<username_for_connection>"
         password {
           raw = "<user_password>"
         }
       }
     }
   }
   ```


    For more information, see the [{{ TF }} provider documentation]({{ tf-provider-dt-endpoint }}).

- API {#api}

   {% include [On premise PostgreSQL API](../../../../_includes/data-transfer/necessary-settings/api/on-premise-postgresql.md) %}

{% endlist %}

### Additional settings {#additional-settings}

{% list tabs group=instructions %}

- Management console {#console}

   * **{{ ui-key.yc-data-transfer.data-transfer.console.form.postgres.console.form.postgres.PostgresTarget.cleanup_policy.title }}**: Select a way to clean up data in the target database before the transfer:

      * `{{ ui-key.yc-data-transfer.data-transfer.console.form.common.console.form.common.CleanupPolicy.DISABLED.title }}`: Select this option if you are only going to do replication without copying data.

      * `{{ ui-key.yc-data-transfer.data-transfer.console.form.common.console.form.common.CleanupPolicy.DROP.title }}`: Completely delete tables included in the transfer (used by default).

         Use this option so that the latest version of the table schema is always transferred to the target database from the source whenever the transfer is activated.

      * `{{ ui-key.yc-data-transfer.data-transfer.console.form.common.console.form.common.CleanupPolicy.TRUNCATE.title }}`: Delete only the data from the tables included in the transfer but leave the schema.

         Use this option if the schema in the target database differs from the one that would have been transferred from the source during the transfer.

   * **{{ ui-key.yc-data-transfer.data-transfer.console.form.postgres.console.form.postgres.PostgresTargetAdvancedSettings.save_tx_boundaries.title }}**: Enable so that the service writes data to the target database only after fully reading the transaction data from the source database.

      
      {% note warning %}

      This feature is at the [Preview](../../../../overview/concepts/launch-stages.md) stage.

      {% endnote %}


{% endlist %}

After configuring the data source and target, [create and start the transfer](../../transfer.md#create).

## Operations with the database during transfer {#db-actions}

{% include [work with db](../../../../_includes/data-transfer/endpoints/sources/pg-work-with-db.md) %}

## Troubleshooting data transfer issues {#troubleshooting}

Known issues when using a {{ PG }} endpoint:

* [Stopping a transfer's master transaction session](#master-trans-stop).
* [Exceeding the connection time-to-live quota](#conn-duration-quota).
* [VIEW transfer error](#view).
* [Error when adding a table entry by constraint](#constraint).
* [Error when migrating all schema tables](#schema).
* [Low transfer speed](#low-speed).
* [Unable to transfer child tables](#successor-tables).
* [Insufficient replication slots in a source database](#replication-slots).
* [No data transfer after changing a source endpoint](#no-data-transfer).
* [Transfer error when changing a master host](#master-change).
* [Error when transferring nested transactions](#inner-tables).
* [Error transferring tables with deferred constraints](#deferrable-constr).
* [Cannot create a replication slot at the activation step](#lock-replication).
* [Excessive WAL size increase](#excessive-wal).
* [Error when replicating from an external source](#external-replication).
* [Error when transfering tables without primary keys](#primary-keys).
* [Error when dropping a table under the Drop cleanup policy](#drop-table-error).

See a full list of recommendations in the [Troubleshooting](../../../troubleshooting/index.md) section.

{% include [master-trans-stop](../../../../_includes/data-transfer/troubles/postgresql/master-trans-stop.md) %}

{% include [conn-duration-quota](../../../../_includes/data-transfer/troubles/postgresql/conn-duration-quota.md) %}

{% include [view](../../../../_includes/data-transfer/troubles/postgresql/view.md) %}

{% include [constraint](../../../../_includes/data-transfer/troubles/postgresql/constraint.md) %}

{% include [schema](../../../../_includes/data-transfer/troubles/postgresql/schema.md) %}

{% include [low-speed](../../../../_includes/data-transfer/troubles/postgresql/low-speed.md) %}

{% include [successor-tables](../../../../_includes/data-transfer/troubles/postgresql/successor-tables.md) %}

{% include [replication-slots](../../../../_includes/data-transfer/troubles/postgresql/replication-slots.md) %}

{% include [no-data-transfer](../../../../_includes/data-transfer/troubles/postgresql/no-data-transfer.md) %}

{% include [master-change](../../../../_includes/data-transfer/troubles/postgresql/master-change.md) %}

{% include [inner-tables](../../../../_includes/data-transfer/troubles/postgresql/inner-tables.md) %}

{% include [deferrable-tables](../../../../_includes/data-transfer/troubles/postgresql/deferrable-constraints.md) %}

{% include [lock-replication](../../../../_includes/data-transfer/troubles/postgresql/lock-replication.md) %}

{% include [excessive-wal](../../../../_includes/data-transfer/troubles/postgresql/excessive-wal.md) %}

{% include [external-replication](../../../../_includes/data-transfer/troubles/postgresql/external-replication.md) %}

{% include [primary-keys](../../../../_includes/data-transfer/troubles/primary-keys.md) %}

{% include [drop-table-error](../../../../_includes/data-transfer/troubles/drop-table-error.md) %}
