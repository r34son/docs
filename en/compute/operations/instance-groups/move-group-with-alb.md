---
title: "Moving an instance group with an L7 load balancer to a different availability zone"
description: "In this article, you will learn how to move a {{ compute-name }} instance group with an L7 load balancer across availability zones."
---

# Moving an instance group with an L7 load balancer to a different availability zone


{% note info %}

{% include [zone-c-deprecation](../../../_includes/vpc/zone-c-deprecation.md) %}

{% endnote %}


To move an instance group with a {{ alb-full-name }} [L7 load balancer](../../../network-load-balancer/concepts):
1. [Create](../../../vpc/operations/subnet-create.md) a subnet in the availability zone to move the instance group to.
1. Enable traffic for the L7 load balancer in the new availability zone:

   {% list tabs group=instructions %}

   - Management console {#console}

      1. In the [management console]({{ link-console-main }}), select the folder where the load balancer is stored.
      1. Select **{{ ui-key.yacloud.iam.folder.dashboard.label_application-load-balancer }}**.
      1. In the the appropriate load balancer row, click ![image](../../../_assets/console-icons/ellipsis.svg) and select **{{ ui-key.yacloud.common.edit }}**.
      1. In the window that opens, under **{{ ui-key.yacloud.alb.section_allocation-settings }}**, enable traffic in the availability zone to move the instance group to.
      1. Click **{{ ui-key.yacloud.common.save }}**.

   - CLI {#cli}

      {% include [cli-install.md](../../../_includes/cli-install.md) %}

      {% include [default-catalogue.md](../../../_includes/default-catalogue.md) %}

      1. View a description of the enable traffic CLI command:

         ```bash
         yc application-load-balancer load-balancer enable-traffic --help
         ```

      1. Get a list of all L7 load balancers in the default folder:

         ```bash
         yc application-load-balancer load-balancer list
         ```

         Result:

         ```text
         +----------------------+-----------------------+-------------+----------------+---------+
         |          ID          |         NAME          |  REGION ID  | LISTENER COUNT | STATUS  |
         +----------------------+-----------------------+-------------+----------------+---------+
         | ds732hi8pn9n******** |      sample-alb1      | {{ region-id }} |              1 |  ACTIVE |
         | f3da23i86n2v******** |      sample-alb2      | {{ region-id }} |              1 |  ACTIVE |
         +----------------------+-----------------------+-------------+----------------+---------+
         ```

      1. Enable traffic:

         ```bash
         yc application-load-balancer load-balancer enable-traffic <load_balancer_name> \
           --zone <availability_zone>
         ```

         Where `--zone` is the availability zone to which you want to move your instance group.

         Result:

         ```yaml
         id: ds7pmslal3km********
         name: sample-alb1
         folder_id: b1gmit33ngp3********
         status: ACTIVE
         region_id: {{ region-id }}
         network_id: enpn46stivv8********
         allocation_policy:
           locations:
             - zone_id: {{ region-id }}-a
               subnet_id: e9bavnqlbiuk********
               disable_traffic: true
             - zone_id: {{ region-id }}-b
                 subnet_id: e2lgp8o00g06********
             - zone_id: {{ region-id }}-c
                 subnet_id: b0cv501fvp13********
         log_group_id: ckgah4eo2j0r********
         security_group_ids:
           - enpdjc5bitmj********
         created_at: "2023-08-09T08:34:24.887765763Z"
         log_options: {}
         ```

   - {{ TF }} {#tf}

      If you do not have {{ TF }} yet, [install it and configure the {{ yandex-cloud }} provider](../../../tutorials/infrastructure-management/terraform-quickstart.md#install-terraform).

      1. Open the {{ TF }} configuration file for the L7 load balancer and, under `allocation_policy`, specify the new availability zone and the ID of the previously created subnet:

         ```hcl
         ...
           allocation_policy {
             location {
               zone_id = [
                 "<old_availability_zone>",
                 "<new_availability_zone>"
               ]
               subnet_id = [
                 "<old_availability_zone_subnet_ID>",
                 "<new_availability_zone_subnet_ID>"
               ]
             }
           }
         ...
         ```

         Where:

         * `zone_id`: Availability zones the load balancer will receive traffic in.
         * `subnet_id`: IDs of subnets in the availability zones.

         For more information about resource parameters in {{ TF }}, see the [provider documentation]({{ tf-provider-resources-link }}/alb_load_balancer).

      1. Apply the changes:

         {% include [terraform-validate-plan-apply](../../../_tutorials/terraform-validate-plan-apply.md) %}

         The new availability zone will start receiving traffic in the load balancer. You can check this using the [management console]({{ link-console-main }}) or this [CLI](../../../cli/quickstart.md) command:

         ```bash
         yc alb load-balancer get <load_balancer_name>
         ```

   - API {#api}

      Use the [update](../../../application-load-balancer/api-ref/LoadBalancer/update.md) REST API method for the [LoadBalancer](../../../application-load-balancer/api-ref/LoadBalancer/index.md) resource or the [LoadBalancerService/Update](../../../application-load-balancer/api-ref/grpc/load_balancer_service.md#Update) gRPC API call.

   {% endlist %}

1. Add the group instances to the new availability zone:

   {% include [ig-create-in-another-zone](../../../_includes/compute/ig-create-in-another-zone.md) %}

1. Delete the group instances from the old availability zone:

   {% include [ig-delete-in-zone.md](../../../_includes/compute/ig-delete-in-zone.md) %}
