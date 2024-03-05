# Migrating data from {{ objstorage-full-name }} to {{ mpg-full-name }} using {{ data-transfer-full-name }}

You can migrate data from {{ objstorage-name }} to the {{ mpg-name }} table using the {{ data-transfer-name }} service. To do this:

1. [Prepare the test data](#prepare-data).
1. [Set up the transfer](#prepare-transfer).
1. [Activate the transfer](#activate-transfer).

If you no longer need the resources you created, [delete them](#clear-out).

## Getting started {#before-you-begin}

Prepare the infrastructure:

{% list tabs group=resources %}

- Manually {#manual}

   1. [Create a {{ mpg-name }} target cluster](../../managed-postgresql/operations/cluster-create.md) in any suitable [configuration](../../managed-postgresql/concepts/instance-types.md) with the following settings:

      * Allocated public access to the cluster hosts.
      * **{{ ui-key.yacloud.mdb.forms.database_field_name }}**: `db1`.
      * **{{ ui-key.yacloud.mdb.forms.database_field_user-login }}**: `pg-user`.
      * **{{ ui-key.yacloud.mdb.forms.database_field_user-password }}**: `<user_password>`.

   
   1. If you are using security groups in a cluster, make sure they are [configured correctly](../../managed-postgresql/operations/connect.md#configuring-security-groups) and allow connecting to it.


   1. [Create a {{ objstorage-name }} bucket](../../storage/operations/buckets/create.md).

   1. [Create a service account](../../iam/operations/sa/create.md#create-sa) named `storage-viewer` with the `storage.viewer` role. The transfer will use it to access the bucket.
   1. [Create a static access key](../../iam/operations/sa/create-access-key.md) for the `storage-viewer` service account.

- {{ TF }} {#tf}

   1. {% include [terraform-install-without-setting](../../_includes/mdb/terraform/install-without-setting.md) %}
   1. {% include [terraform-authentication](../../_includes/mdb/terraform/authentication.md) %}
   1. {% include [terraform-setting](../../_includes/mdb/terraform/setting.md) %}
   1. {% include [terraform-configure-provider](../../_includes/mdb/terraform/configure-provider.md) %}

   1. Download the [objstorage-to-postgres.tf](https://github.com/yandex-cloud-examples/yc-data-transfer-from-object-storage-to-postgresql/blob/main/objstorage-to-postgres.tf) configuration file to the same working directory.

      This file describes:

      * [Network](../../vpc/concepts/network.md#network).
      * [Subnet](../../vpc/concepts/network.md#subnet).
      * [Security group](../../vpc/concepts/security-groups.md) required to connect to a cluster.
      * Service account to be used to create and access the bucket.
      * {{ lockbox-name }} secret, which will store the static key of the service account to configure the source endpoint.
      * {{ objstorage-name }} source bucket.
      * {{ mpg-name }} target cluster.
      * Target endpoint.
      * Transfer.

   1. In the `objstorage-to-postgres.tf` file, specify:

      * `folder_id`: Cloud directory ID, the same one specified in the provider settings.
      * `bucket_name`: Bucket name according to the [naming rules](../../storage/concepts/bucket.md#naming).
      * `pg_password`: {{ PG }} user password.

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

1. Create `demo_data.csv` on the running instance and fill it with test data:

   ```csv
   1,Anna
   2,Robert
   3,Umar
   4,Algul
   5,Viktor
   ```

1. [Upload](../../storage/operations/objects/upload.md#simple) the prepared file to the {{ objstorage-name }} bucket.

## Set up the transfer {#prepare-transfer}

1. [Create a source endpoint](../../data-transfer/operations/endpoint/source/object-storage.md) of the `{{ objstorage-name }}` type with the following settings:

   * **{{ ui-key.yacloud.data-transfer.forms.label-database_type }}**: `{{ ui-key.yacloud.data-transfer.label_endpoint-type-OBJECT_STORAGE }}`.
   * **{{ ui-key.yc-data-transfer.data-transfer.endpoint.airbyte.s3_source.endpoint.airbyte.s3_source.S3Source.Provider.bucket.title }}**: Bucket name in {{ objstorage-name }}.
   * **{{ ui-key.yc-data-transfer.data-transfer.endpoint.airbyte.s3_source.endpoint.airbyte.s3_source.S3Source.Provider.aws_access_key_id.title }}**: Public part of the service account static key. If you created the infrastructure with {{ TF }}, [copy the key value from the {{ lockbox-name }} secret](../../lockbox/operations/secret-get-info.md#secret-contents).
   * **{{ ui-key.yc-data-transfer.data-transfer.endpoint.airbyte.s3_source.endpoint.airbyte.s3_source.S3Source.Provider.aws_secret_access_key.title }}**: Private part of the service account static key. If you created the infrastructure with {{ TF }}, [copy the key value from the {{ lockbox-name }} secret](../../lockbox/operations/secret-get-info.md#secret-contents).
   * **{{ ui-key.yc-data-transfer.data-transfer.endpoint.airbyte.s3_source.endpoint.airbyte.s3_source.S3Source.Provider.endpoint.title }}**: `https://storage.yandexcloud.net`.
   * **{{ ui-key.yc-data-transfer.data-transfer.console.form.object_storage.console.form.object_storage.ObjectStorageSource.ObjectStorageEventSource.SQS.region.title }}**: `ru-central1`.
   * **{{ ui-key.yc-data-transfer.data-transfer.console.form.object_storage.console.form.object_storage.ObjectStorageTarget.format.title }}**: `{{ ui-key.yc-data-transfer.data-transfer.console.form.object_storage.console.form.object_storage.ObjectStorageSource.ObjectStorageReaderFormat.csv.title }}`.
   * **{{ ui-key.yc-data-transfer.data-transfer.console.form.object_storage.console.form.object_storage.ObjectStorageSource.ObjectStorageReaderFormat.Csv.delimiter.title }}**: Comma mark (`,`).
   * **{{ ui-key.yc-data-transfer.data-transfer.transfer.transfer.RenameTablesTransformer.rename_tables.array_item_label }}**: CSV file name in a bucket, e.g., `demo_data.csv`.
   * **{{ ui-key.yc-data-transfer.data-transfer.console.form.object_storage.console.form.object_storage.ObjectStorageSource.result_schema.title }}**: Select `{{ ui-key.yc-data-transfer.data-transfer.console.form.object_storage.console.form.object_storage.ObjectStorageDataSchema.data_schema.title }}` and specify field names and data types:

      * `Id` : `Int64`.
      * `Name`: `UTF8`.

   Leave the default values for the other parameters.

1. Create a target endpoint and a transfer:

   {% list tabs group=resources %}

   - Manually {#manual}

      1. [Create a target endpoint](../../data-transfer/operations/endpoint/target/postgresql.md) of the `{{ PG }}` type and specify the cluster connection parameters in it:

         * **{{ ui-key.yc-data-transfer.data-transfer.console.form.postgres.console.form.postgres.PostgresConnection.connection_type.title }}**: `{{ ui-key.yc-data-transfer.data-transfer.console.form.postgres.console.form.postgres.PostgresConnectionType.mdb_cluster_id.title }}`.
         * **{{ ui-key.yc-data-transfer.data-transfer.console.form.postgres.console.form.postgres.PostgresConnectionType.mdb_cluster_id.title }}**: Name of the {{ PG }} source cluster from the drop-down list.
         * **{{ ui-key.yc-data-transfer.data-transfer.console.form.postgres.console.form.postgres.PostgresConnection.database.title }}**: `db1`.
         * **{{ ui-key.yc-data-transfer.data-transfer.console.form.postgres.console.form.postgres.PostgresConnection.user.title }}**: `pg-user`.
         * **{{ ui-key.yc-data-transfer.data-transfer.console.form.postgres.console.form.postgres.PostgresConnection.password.title }}**: `<user_password>`.

      1. [Create a transfer](../../data-transfer/operations/transfer.md#create) of the **_{{ ui-key.yc-data-transfer.data-transfer.console.form.transfer.console.form.transfer.TransferType.snapshot.title }}_** type that will use the created endpoints.

   - {{ TF }} {#tf}

      1. In the `objstorage-to-postgres.tf` file, specify the variables:

         * `source_endpoint_id`: ID of the source endpoint.
         * `transfer_enabled`: Set `1` to enable transfer creation.

      1. Make sure the {{ TF }} configuration files are correct using this command:

         ```bash
         terraform validate
         ```

         If there are any errors in the configuration files, {{ TF }} will point them out.

      1. Create the required infrastructure:

         {% include [terraform-apply](../../_includes/mdb/terraform/apply.md) %}

   {% endlist %}

## Activate the transfer {#activate-transfer}

1. [Activate the transfer](../../data-transfer/operations/transfer.md#activate) and wait for its status to change to **{{ ui-key.yacloud.data-transfer.label_connector-status-DONE }}**.
1. [Connect to the {{ mpg-name }} target cluster database](../../managed-postgresql/operations/connect.md).
1. Run the following query:

   ```sql
   SELECT * FROM public."demo_data.csv";
   ```

   {% cut "Sample response" %}

   ```sql
    __file_name  | __row_index | Id |  Name  
   --------------+-------------+----+--------
   demo_data.csv |           1 |  1 | Anna
   demo_data.csv |           2 |  2 | Robert
   demo_data.csv |           3 |  3 | Umar
   demo_data.csv |           4 |  4 | Algul
   demo_data.csv |           5 |  5 | Viktor
   ```

   {% endcut %}

Data successfully migrated.

## Delete the resources you created {#clear-out}

Some resources are not free of charge. To avoid paying for them, delete the resources you no longer need:

* [Transfer](../../data-transfer/operations/transfer.md#delete).
* [Source endpoint](../../data-transfer/operations/endpoint/index.md#delete).
* Delete the other resources depending on how they were created:

   {% list tabs group=resources %}

   - Manually {#manual}

      * [Target endpoint](../../data-transfer/operations/endpoint/index.md#delete).
      * [{{ mpg-name }}](../../managed-postgresql/operations/cluster-delete.md).
      * [{{ objstorage-name }} bucket](../../storage/operations/buckets/delete.md).

   - {{ TF }} {#tf}

      1. Delete all objects from the bucket.
      1. In the terminal window, go to the directory containing the infrastructure plan.
      1. Delete the `objstorage-to-postgres.tf` configuration file.
      1. Make sure the {{ TF }} configuration files are correct using this command:

         ```bash
         terraform validate
         ```

         If there are any errors in the configuration files, {{ TF }} will point them out.

      1. Confirm updating the resources.

         {% include [terraform-apply](../../_includes/mdb/terraform/apply.md) %}

         All the resources described in the `objstorage-to-postgres.tf` configuration file will be deleted.

   {% endlist %}
