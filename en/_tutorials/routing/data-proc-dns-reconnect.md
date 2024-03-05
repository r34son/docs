# Reconfiguring a network connection when recreating a {{ dataproc-name }} cluster

You may need to recreate a cluster to install software updates, transfer the load across clusters, move clusters from one availability zone to another, and perform other operations.

The example below describes how to set up DNS to quickly switch network traffic over to new [host FQDNs](../../data-proc/concepts/network.md#hostname) when recreating a {{ dataproc-name }} cluster. For the current name of the cluster master host, a network alias (CNAME record) is created in {{ dns-full-name }}. When you recreate the cluster, the CNAME record is changed to the master host's new name.

To set up DNS for your {{ dataproc-name }} cluster:

1. [Create a DNS zone and a CNAME record](#dns-record).
1. [Delete the cluster and recreate it](#recreate-cluster).

If you no longer need the resources you created, [delete them](#clear-out).

## Getting started {#deploy-infrastructure}

Prepare the infrastructure:

{% list tabs group=instructions %}

- Manually {#manual}

   1. [Create a network](../../vpc/operations/network-create.md) named `data-proc-network` with the **{{ ui-key.yacloud.vpc.networks.create.field_is-default }}** option disabled.
   1. In `data-proc-network`, [create a subnet](../../vpc/operations/subnet-create.md) with the following parameters:

      * **{{ ui-key.yacloud.vpc.subnetworks.create.field_name }}**: `data-proc-subnet-a`
      * **{{ ui-key.yacloud.vpc.subnetworks.create.field_zone }}**: `{{ region-id }}-a`
      * **{{ ui-key.yacloud.vpc.subnetworks.create.field_ip }}**: `192.168.1.0/24`

   1. [Create a NAT gateway](../../vpc/operations/create-nat-gateway.md) and a routing table named `data-proc-route-table` in `data-proc-network`. Associate the routing table with the `data-proc-subnet-a` subnet.

   1. In the `data-proc-network` subnet, [create a security group](../../vpc/operations/security-group-create.md) named `data-proc-security-group` with the following rules:

      * One rule for inbound and another one for outbound service traffic:

         * **{{ ui-key.yacloud.vpc.network.security-groups.forms.field_sg-rule-port-range }}**: `{{ port-any }}`
         * **{{ ui-key.yacloud.vpc.network.security-groups.forms.field_sg-rule-protocol }}**: `{{ ui-key.yacloud.vpc.network.security-groups.forms.value_any }}`
         * **{{ ui-key.yacloud.vpc.network.security-groups.forms.field_sg-rule-source }}**/**{{ ui-key.yacloud.vpc.network.security-groups.forms.field_sg-rule-destination }}**: `{{ ui-key.yacloud.vpc.network.security-groups.forms.value_sg-rule-destination-sg }}`
         * **{{ ui-key.yacloud.vpc.network.security-groups.forms.field_sg-rule-sg-type }}**: `{{ ui-key.yacloud.vpc.network.security-groups.forms.value_sg-rule-sg-type-self }}`

      * Rule for outgoing HTTPS traffic:

         * **{{ ui-key.yacloud.vpc.network.security-groups.forms.field_sg-rule-port-range }}**: `{{ port-https }}`
         * **{{ ui-key.yacloud.vpc.network.security-groups.forms.field_sg-rule-protocol }}**: `{{ ui-key.yacloud.vpc.network.security-groups.forms.value_tcp }}`
         * **{{ ui-key.yacloud.vpc.network.security-groups.forms.field_sg-rule-destination }}**: `{{ ui-key.yacloud.vpc.network.security-groups.forms.value_sg-rule-destination-cidr }}`
         * **{{ ui-key.yacloud.vpc.network.security-groups.forms.field_sg-rule-cidr-blocks }}**: `0.0.0.0/0`

   1. [Create a service account](../../iam/operations/sa/create.md) named `data-proc-sa` with the following roles:

      * [dataproc.agent](../../data-proc/security/index.md#dataproc-agent)
      * [storage.uploader](../../storage/security/index.md#storage-uploader)
      * [storage.viewer](../../storage/security/index.md#storage-viewer)

   1. [Create a {{ objstorage-full-name }} bucket](../../storage/operations/buckets/create.md) with restricted access.

   1. [Create a {{ dataproc-name }} cluster](../../data-proc/operations/cluster-create.md) with any suitable configuration with the following settings:

      * **{{ ui-key.yacloud.mdb.forms.base_field_service-account }}**: `data-proc-sa`
      * **{{ ui-key.yacloud.mdb.forms.config_field_form-bucket-type }}**: `{{ ui-key.yacloud.forms.label_form-list }}`
      * **{{ ui-key.yacloud.mdb.forms.config_field_bucket }}**: Select the created bucket
      * **{{ ui-key.yacloud.mdb.forms.config_field_network }}**: `data-proc-network`
      * **{{ ui-key.yacloud.mdb.forms.field_security-group }}**: `data-proc-security-group`

- {{ TF }} {#tf}

   1. If you do not have {{ TF }} yet, [set up and configure](../../tutorials/infrastructure-management/terraform-quickstart.md#install-terraform) it.
   1. [Download the file with the provider settings](https://github.com/yandex-cloud/examples/tree/master/tutorials/terraform/provider.tf). Place it in a separate working directory and [specify the parameter values](../../tutorials/infrastructure-management/terraform-quickstart.md#configure-provider).
   1. Download the [data-proc-dns-connect.tf](https://github.com/yandex-cloud/examples/blob/master/tutorials/terraform/data-proc-dns-connect.tf) configuration file to the same working directory.

      The file describes:

      * [Network](../../vpc/concepts/network.md#network).
      * [Subnet](../../vpc/concepts/network.md#subnet).
      * DNS zone and CNAME record for the cluster master host.
      * NAT gateway and routing table.
      * [Security groups](../../vpc/concepts/security-groups.md).
      * Service account to access cloud resources.
      * Bucket to store job dependencies and results.
      * {{ dataproc-name }} cluster.

   1. In the `data-proc-dns-connect.tf` file, specify the variables:

      * `folder_id`: Folder ID
      * `path_to_ssh_public_key`: Path to the public SSH key
      * `bucket`: Bucket name

   1. Run the `terraform init` command in the working directory hosting the configuration files. This command initializes the provider specified in the configuration files and enables you to use the provider resources and data sources.
   1. Make sure the {{ TF }} configuration files are correct using this command:

      ```bash
      terraform validate
      ```

      If there are any errors in the configuration files, {{ TF }} will point them out.

   1. Create the required infrastructure:

      {% include [terraform-apply](../../_includes/mdb/terraform/apply.md) %}

      {% include [explore-resources](../../_includes/mdb/terraform/explore-resources.md) %}

{% endlist %}

## Create a DNS zone and a CNAME record {#dns-record}

Create resources:

{% list tabs group=instructions %}

- Manually {#manual}

   1. [Create an internal DNS zone](../../dns/operations/zone-create-private.md) with the following settings:

      * **{{ ui-key.yacloud.dns.label_zone }}**: `data-proc-test-user.org.`.
      * **{{ ui-key.yacloud.dns.label_networks }}**: Select `data-proc-network`.
      * **{{ ui-key.yacloud.common.name }}**: `dp-private-zone`.

   1. [Create a DNS record](../../dns/operations/resource-record-create.md) of the CNAME type with the following settings:
      * **{{ ui-key.yacloud.common.name }}**: `data-proc-test-user.org.`
      * **{{ ui-key.yacloud.dns.label_records }}**: [FQDN of the {{ dataproc-name }} cluster master host](../../data-proc/operations/connect.md#fqdn)

- {{ TF }} {#tf}

   1. [Get the FQDN](../../data-proc/operations/connect.md#fqdn) of the {{ dataproc-name }} cluster master host.
   1. In the `data-proc-dns-connect.tf` file, specify the variable:

      * `dataproc_fqdn`: FQDN of the {{ dataproc-name }} cluster master host

   1. Make sure the {{ TF }} configuration files are correct using this command:

      ```bash
      terraform validate
      ```

      If there are any errors in the configuration files, {{ TF }} will point them out.

   1. Create the required infrastructure:

      {% include [terraform-apply](../../_includes/mdb/terraform/apply.md) %}

{% endlist %}

Test network access to the cluster by the CNAME record:

```text
dig data-proc-test-user.org.
<...>
;; ANSWER SECTION:
data-proc-test-user.org. 600	IN	CNAME	rc1a-dataproc-m-6ijqng07vul2mu8j.mdb.yandexcloud.net.
rc1a-dataproc-m-6ijqng07vul2mu8j.mdb.yandexcloud.net. 600 IN A 192.168.1.8
```

## Delete the cluster and recreate it {#recreate-cluster}

{% list tabs group=instructions %}

- Manually {#manual}

   1. [Delete the {{ dataproc-name }} cluster](../../data-proc/operations/cluster-delete.md) and create a new one with [identical characteristics](#deploy-infrastructure).
   1. [Change the DNS record](../../dns/operations/resource-record-update.md) that you created [earlier](#dns-record) and specify the FQDN of the master host of the newly created cluster in the **{{ ui-key.yacloud.dns.label_records }}** parameter.

- {{ TF }} {#tf}

   1. Delete the `yandex_dataproc_cluster` section in the `data-proc-dns-connect.tf` file.
   1. Make sure the {{ TF }} configuration files are correct using this command:

      ```bash
      terraform validate
      ```

      If there are any errors in the configuration files, {{ TF }} will point them out.

   1. Apply the changes:

      {% include [terraform-apply](../../_includes/mdb/terraform/apply.md) %}

   1. Add the `yandex_dataproc_cluster` section to the `data-proc-dns-connect.tf` file with the same contents as in the [source file](#deploy-infrastructure) to create a new {{ dataproc-name }} cluster.
   1. Make sure the {{ TF }} configuration files are correct using this command:

      ```bash
      terraform validate
      ```

      If there are any errors in the configuration files, {{ TF }} will point them out.

   1. Create a cluster:

      {% include [terraform-apply](../../_includes/mdb/terraform/apply.md) %}

   1. [Get the FQDN](../../data-proc/operations/connect.md#fqdn) of the master host of the newly created {{ dataproc-name }} cluster.
   1. In the `data-proc-dns-connect.tf` file, specify the variable:

      * `dataproc_fqdn`: FQDN of the cluster master host

   1. Make sure the {{ TF }} configuration files are correct using this command:

      ```bash
      terraform validate
      ```

      If there are any errors in the configuration files, {{ TF }} will point them out.

   1. Apply the changes:

      {% include [terraform-apply](../../_includes/mdb/terraform/apply.md) %}

{% endlist %}

Check if you still have network access to the cluster by the CNAME record:

```text
dig data-proc-test-user.org.
<...>
;; ANSWER SECTION:
data-proc-test-user.org. 600	IN	CNAME	rc1a-dataproc-m-lsqohjh53rfu659d.mdb.yandexcloud.net.
rc1a-dataproc-m-8kompl81232cdsu8j.mdb.yandexcloud.net. 600 IN A 192.168.1.8
```

## Delete the resources you created {#clear-out}

Some resources are not free of charge. To avoid paying for them, delete the resources you no longer need:

{% list tabs group=instructions %}

- Manually {#manual}

   1. [Delete the {{ dataproc-name }} cluster](../../data-proc/operations/cluster-delete.md).
   1. If you reserved public static IP addresses for the clusters, release and [delete them](../../vpc/operations/address-delete.md).
   1. [Delete the subnet](../../vpc/operations/subnet-delete.md).
   1. Delete the routing table and NAT gateway.
   1. [Delete the network](../../vpc/operations/network-delete.md).
   1. [Delete the DNS zone](../../dns/operations/zone-delete.md).

- {{ TF }} {#tf}

   To delete the infrastructure [created with {{ TF }}](#deploy-infrastructure):

   1. In the terminal window, go to the directory containing the infrastructure plan.
   1. Delete the `data-proc-dns-connect.tf` configuration file.
   1. Make sure the {{ TF }} configuration files are correct using this command:

      ```bash
      terraform validate
      ```

      If there are any errors in the configuration files, {{ TF }} will point them out.

   1. Confirm updating the resources.

      {% include [terraform-apply](../../_includes/mdb/terraform/apply.md) %}

      All the resources described in the `data-proc-dns-connect.tf` configuration file will be deleted.

{% endlist %}
