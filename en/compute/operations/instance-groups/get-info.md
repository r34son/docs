# Getting information about an instance group

After creating an instance group, you can get basic information about the group.

You can only use the CLI or API to retrieve user [metadata](../../concepts/vm-metadata.md) transmitted when creating or editing a group.

To get information about an instance group:

{% list tabs group=instructions %}

- Management console {#console}

   1. In the [management console]({{ link-console-main }}), open the folder with the appropriate instance group.
   1. Select **{{ ui-key.yacloud.iam.folder.dashboard.label_compute }}**.
   1. In the left-hand panel, select ![image](../../../_assets/console-icons/layers-3-diagonal.svg) **{{ ui-key.yacloud.compute.switch_groups }}**.
   1. Click the name of the group.

- CLI {#cli}

   {% include [cli-install.md](../../../_includes/cli-install.md) %}

   {% include [default-catalogue.md](../../../_includes/default-catalogue.md) %}

   1. See the description of the CLI get instance group info command:

      ```bash
      {{ yc-compute-ig }} get --help
      ```

   1. Get a list of instance groups in the default folder:

      {% include [instance-group-list.md](../../../_includes/instance-groups/instance-group-list.md) %}

   1. Select the group `ID` or `NAME`, e.g., `first-instance-group`.
   1. Get information about the instance group:

      ```bash
      {{ yc-compute-ig }} get --name first-instance-group
      ```

- {{ TF }}

   {% include [terraform-definition](../../../_tutorials/terraform-definition.md) %}

   {% include [terraform-install](../../../_includes/terraform-install.md) %}

   To get information about an instance group using {{ TF }}:

   1. In the {{ TF }} configuration file, describe the parameters of the resources you want to create:

      ```
      data "yandex_compute_instance_group" "my_group" {
        instance_group_id = "<instance_group_ID>"
      }

      output "instancegroupvm_external_ip" {
        value = "${data.yandex_compute_instance_group.my_group.instances.*.network_interface.0.nat_ip_address}"
      }
      ```

      Where:

      * `data "yandex_compute_instance_group"`: Description of the data source to get instance group information from:
         * `instance_group_id`: Instance group ID.
      * `output "instancegroupvm_external_ip"`: Full list of external IP addresses of the group's instances for the resulting output:
         * `value`: Returned value.

      For more information about the `yandex_compute_instance_group` data source parameters, see the [provider documentation]({{ tf-provider-datasources-link }}/datasource_compute_instance_group).

   1. Create resources:

      {% include [terraform-validate-plan-apply](../../../_tutorials/terraform-validate-plan-apply.md) %}

      {{ TF }} will create the required resources and display the output variable values in the terminal. To check the results, run:

      ```bash
      terraform output instancegroupvm_external_ip
      ```

      Result:

      ```bash
      instancegroupvm_external_ip = tolist([
        "158.160.112.7",
        "158.160.2.119",
      ])
      ```

- API {#api}

   Use the [get](../../api-ref/InstanceGroup/get.md) REST API method for the [InstanceGroup](../../api-ref/InstanceGroup/index.md) resource or the [InstanceGroupService/Get](../../api-ref/grpc/instance_group_service.md#Get) gRPC API call.

   To request the list of available instance groups, use the [listInstances](../../api-ref/InstanceGroup/listInstances.md) REST API method or the [InstanceGroupService/ListInstances](../../api-ref/grpc/instance_group_service.md#ListInstances) gRPC API call.


{% endlist %}
