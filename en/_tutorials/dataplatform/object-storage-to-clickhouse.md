# Loading data from {{ objstorage-full-name }} to {{ mch-full-name }} using {{ data-transfer-full-name }}

You can migrate data from {{ objstorage-name }} to the {{ mch-name }} table using {{ data-transfer-name }}. To do this:

1. [Prepare the test data](#prepare-data).
1. [Prepare and activate the transfer](#prepare-transfer).
1. [Test the transfer](#verify-transfer).

If you no longer need the resources you created, [delete them](#clear-out).

## Getting started {#before-you-begin}

Prepare the infrastructure:

{% list tabs group=instructions %}

- Manually {#manual}

   1. [Create a {{ mch-name }} target cluster](../../managed-clickhouse/operations/cluster-create.md) in any suitable configuration with the following settings:

      * Number of {{ CH }} hosts: At least two, which is required to enable replication in the cluster.
      * Public access to cluster hosts: Allowed.
      * **{{ ui-key.yacloud.mdb.forms.database_field_name }}**: `db1`.
      * **{{ ui-key.yacloud.mdb.forms.database_field_user-login }}**: `user1`.
      * **{{ ui-key.yacloud.mdb.forms.database_field_user-password }}**: `<user_password>`.

   
   1. If you are using security groups in a cluster, make sure they are [configured correctly](../../managed-clickhouse/operations/connect.md#configuring-security-groups) and allow connecting to it.


   1. [Create an {{ objstorage-name }} bucket](../../storage/operations/buckets/create.md).

   1. [Create a service account](../../iam/operations/sa/create.md#create-sa) named `storage-viewer` with the `storage.viewer` role. The transfer will use it to access the bucket.

   1. [Create a static access key](../../iam/operations/sa/create-access-key.md) for the `storage-viewer` service account.

- {{ TF }} {#tf}

   1. {% include [terraform-install-without-setting](../../_includes/mdb/terraform/install-without-setting.md) %}
   1. {% include [terraform-authentication](../../_includes/mdb/terraform/authentication.md) %}
   1. {% include [terraform-setting](../../_includes/mdb/terraform/setting.md) %}
   1. {% include [terraform-configure-provider](../../_includes/mdb/terraform/configure-provider.md) %}

   1. Download the [object-storage-to-clickhouse.tf](https://github.com/yandex-cloud-examples/yc-data-transfer-from-object-storage-to-clickhouse/blob/main/object-storage-to-clickhouse.tf) configuration file to the same working directory.

      This file describes:

      * [Network](../../vpc/concepts/network.md#network).
      * [Subnet](../../vpc/concepts/network.md#subnet).
      * [Security group](../../vpc/concepts/security-groups.md) required to connect to a cluster.
      * Service account to be used to create and access the bucket.
      * {{ lockbox-name }} secret, which will store the static key of the service account to configure the source endpoint.
      * {{ objstorage-name }} source bucket.
      * {{ mch-name }} target cluster.
      * Target endpoint.
      * Transfer.

   1. In the `object-storage-to-clickhouse.tf` file, specify:

      * `folder_id`: Cloud directory ID, the same one specified in the provider settings.
      * `bucket_name`: Bucket name according to the [naming requirements](../../storage/concepts/bucket.md#naming).
      * `ch_password`: {{ CH }} user password.

   1. Make sure the {{ TF }} configuration files are correct using this command:

      ```bash
      terraform validate
      ```

      If there are any errors in the configuration files, {{ TF }} will point them out.

   1. Create the required infrastructure:

      {% include [terraform-apply](../../_includes/mdb/terraform/apply.md) %}

      {% include [explore-resources](../../_includes/mdb/terraform/explore-resources.md) %}

{% endlist %}

## Prepare the test data {#prepare-data}

1. Prepare two CSV files with test data:

   * `demo_data1.csv`:

      ```csv
      1,Anna
      2,Robert
      3,Umar
      4,Algul
      5,Viktor
      ```

   * `demo_data2.csv`:

      ```csv
      6,Maria
      7,Alex
      ```

1. [Upload](../../storage/operations/objects/upload.md#simple) the `demo_data1.csv` file to the {{ objstorage-name }} bucket.

## Prepare and activate the transfer {#prepare-transfer}

1. [Create a source endpoint](../../data-transfer/operations/endpoint/source/object-storage.md#endpoint-settings) of the `{{ objstorage-name }}` type with the following settings:

   * **{{ ui-key.yacloud.data-transfer.forms.label-database_type }}**: `{{ ui-key.yacloud.data-transfer.label_endpoint-type-OBJECT_STORAGE }}`.
   * **{{ ui-key.yc-data-transfer.data-transfer.endpoint.airbyte.s3_source.endpoint.airbyte.s3_source.S3Source.Provider.bucket.title }}**: Bucket name in {{ objstorage-name }}.
   * **{{ ui-key.yc-data-transfer.data-transfer.endpoint.airbyte.s3_source.endpoint.airbyte.s3_source.S3Source.Provider.aws_access_key_id.title }}**: Public part of the service account static key. If you created your infrastructure with {{ TF }}, [copy the key value from the {{ lockbox-name }} secret](../../lockbox/operations/secret-get-info.md#secret-contents).
   * **{{ ui-key.yc-data-transfer.data-transfer.endpoint.airbyte.s3_source.endpoint.airbyte.s3_source.S3Source.Provider.aws_secret_access_key.title }}**: Private part of the service account static key. If you created your infrastructure with {{ TF }}, [copy the key value from the {{ lockbox-name }} secret](../../lockbox/operations/secret-get-info.md#secret-contents).
   * **{{ ui-key.yc-data-transfer.data-transfer.endpoint.airbyte.s3_source.endpoint.airbyte.s3_source.S3Source.Provider.endpoint.title }}**: `https://storage.yandexcloud.net`.
   * **{{ ui-key.yc-data-transfer.data-transfer.console.form.object_storage.console.form.object_storage.ObjectStorageSource.ObjectStorageEventSource.SQS.region.title }}**: `ru-central1`.
   * **{{ ui-key.yc-data-transfer.data-transfer.console.form.object_storage.console.form.object_storage.ObjectStorageTarget.format.title }}**: `{{ ui-key.yc-data-transfer.data-transfer.console.form.object_storage.console.form.object_storage.ObjectStorageSource.ObjectStorageReaderFormat.csv.title }}`.
   * **{{ ui-key.yc-data-transfer.data-transfer.console.form.object_storage.console.form.object_storage.ObjectStorageSource.ObjectStorageReaderFormat.Csv.delimiter.title }}**: Comma mark (`,`).
   * **{{ ui-key.yc-data-transfer.data-transfer.transfer.transfer.RenameTablesTransformer.rename_tables.array_item_label }}**: `table1`.
   * **{{ ui-key.yc-data-transfer.data-transfer.console.form.object_storage.console.form.object_storage.ObjectStorageSource.result_schema.title }}**: Select `{{ ui-key.yc-data-transfer.data-transfer.console.form.object_storage.console.form.object_storage.ObjectStorageDataSchema.data_schema.title }}` and specify field names and data types:

      * `Id`: `Int64`.
      * `Name`: `UTF8`.

   Leave the default values for the other parameters.

1. Create a target endpoint and a transfer:

   {% list tabs group=instructions %}

   - Manually {#manual}

      1. [Create a target endpoint](../../data-transfer/operations/endpoint/target/clickhouse.md#endpoint-settings) of the `{{ CH }}` type and specify the cluster connection parameters in it:

         * **{{ ui-key.yc-data-transfer.data-transfer.console.form.clickhouse.console.form.clickhouse.ClickHouseConnection.connection_type.title }}**: `{{ ui-key.yc-data-transfer.data-transfer.console.form.clickhouse.console.form.clickhouse.ClickHouseConnectionType.managed.title }}`.
         * **{{ ui-key.yc-data-transfer.data-transfer.console.form.clickhouse.console.form.clickhouse.ClickHouseManaged.mdb_cluster_id.title }}**: `<name_of_{{ CH }}_target_cluster>` from the drop-down list.
         * **{{ ui-key.yc-data-transfer.data-transfer.console.form.clickhouse.console.form.clickhouse.ClickHouseConnection.database.title }}**: `db1`.
         * **{{ ui-key.yc-data-transfer.data-transfer.console.form.clickhouse.console.form.clickhouse.ClickHouseCredentials.user.title }}**: `user1`.
         * **{{ ui-key.yc-data-transfer.data-transfer.console.form.clickhouse.console.form.clickhouse.ClickHouseCredentials.password.title }}**: `<user_password>`.

      1. [Create a transfer](../../data-transfer/operations/transfer.md#create) of the **_{{ ui-key.yc-data-transfer.data-transfer.console.form.transfer.console.form.transfer.TransferType.snapshot_and_increment.title }}_** type that will use the created endpoints.

      1. [Activate the transfer](../../data-transfer/operations/transfer.md#activate) and wait for its status to change to **{{ ui-key.yacloud.data-transfer.label_connector-status-RUNNING }}**.

   - {{ TF }} {#tf}

      1. In the `object-storage-to-clickhouse.tf` file, specify the values for these parameters:

         * `source_endpoint_id`: Source endpoint ID.
         * `transfer_enabled`: Set `1` to enable transfer creation.

      1. Make sure the {{ TF }} configuration files are correct using this command:

         ```bash
         terraform validate
         ```

         If there are any errors in the configuration files, {{ TF }} will point them out.

      1. Create the required infrastructure:

         {% include [terraform-apply](../../_includes/mdb/terraform/apply.md) %}

      1. The transfer is activated automatically. Wait for its status to change to **{{ ui-key.yacloud.data-transfer.label_connector-status-RUNNING }}**.

   {% endlist %}

## Test the transfer {#verify-transfer}

To check if the transfer performs properly, test the copy and replication processes.

### Test the copy process {#verify-copy}

1. [Connect to the `db1` database](../../managed-clickhouse/operations/connect.md) in the {{ mch-name }} target cluster.

1. Run the following query:

   ```sql
   SELECT * FROM db1.table1;
   ```

   {% cut "Sample response" %}

   ```sql
     __file_name  | __row_index | Id |  Name
   --------------+-------------+----+--------
   demo_data1.csv |           1 |  1 | Anna
   demo_data1.csv |           2 |  2 | Robert
   demo_data1.csv |           3 |  3 | Umar
   demo_data1.csv |           4 |  4 | Algul
   demo_data1.csv |           5 |  5 | Viktor
   ```

   {% endcut %}

### Test the replication process {#verify-replication}

1. [Upload](../../storage/operations/objects/upload.md#simple) the `demo_data2.csv` file to the {{ objstorage-name }} bucket.

1. Make sure the data from the `demo_data2.csv` file has been added to the target database:

   1. [Connect to the](../../managed-clickhouse/operations/connect.md) `db1` database in the {{ mch-name }} target cluster.

   1. Run the following query:

      ```sql
      SELECT * FROM db1.table1;
      ```

      {% cut "Sample response" %}

      ```sql
        __file_name  | __row_index | Id |  Name
      --------------+-------------+----+--------
      demo_data1.csv |           1 |  1 | Anna
      demo_data1.csv |           2 |  2 | Robert
      demo_data1.csv |           3 |  3 | Umar
      demo_data1.csv |           4 |  4 | Algul
      demo_data1.csv |           5 |  5 | Viktor
      demo_data2.csv |           1 |  6 | Maria
      demo_data2.csv |           2 |  7 | Alex
      ```

      {% endcut %}

## Delete the resources you created {#clear-out}

{% note info %}

Before deleting the created resources, [deactivate the transfer](../../data-transfer/operations/transfer.md#deactivate).

{% endnote %}

Some resources are not free of charge. To avoid paying for them, delete the resources you no longer need:

* [Transfer](../../data-transfer/operations/transfer.md#delete).
* [Source endpoint](../../data-transfer/operations/endpoint/index.md#delete).
* Delete the other resources depending on how they were created:

   {% list tabs group=instructions %}

   - Manually {#manual}

      * [Target endpoint](../../data-transfer/operations/endpoint/index.md#delete).
      * [{{ mch-name }} cluster](../../managed-clickhouse/operations/cluster-delete.md).
      * [{{ objstorage-name }} bucket](../../storage/operations/buckets/delete.md).

   - {{ TF }} {#tf}

      If you created your resources using {{ TF }}:

      1. Delete all objects from the bucket.
      1. In the terminal window, go to the directory containing the infrastructure plan.
      1. Delete the `object-storage-to-clickhouse.tf` configuration file.
      1. Make sure the {{ TF }} configuration files are correct using this command:

         ```bash
         terraform validate
         ```

         If there are any errors in the configuration files, {{ TF }} will point them out.

      1. Confirm updating the resources.

         {% include [terraform-apply](../../_includes/mdb/terraform/apply.md) %}

         All the resources described in the `object-storage-to-clickhouse.tf` configuration file will be deleted.

   {% endlist %}
