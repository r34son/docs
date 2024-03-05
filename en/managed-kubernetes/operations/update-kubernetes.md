# Updating {{ k8s }}

For {{ managed-k8s-name }}, both automatic and manual updates are available for [clusters](../concepts/index.md#kubernetes-cluster) and [node groups](../concepts/index.md#node-group). You can request a manual update of the {{ managed-k8s-name }} cluster or its nodes to the latest supported [version](../concepts/release-channels-and-updates.md) at any time. Manual updates bypass any configured maintenance windows and maintenance exceptions.

When updating the {{ k8s }} major version, first update the {{ managed-k8s-name }} cluster and then its node group.

{% note info %}

You can edit the update policy of [{{ managed-k8s-name }} clusters](#cluster-auto-upgrade) and [node groups](#node-group-auto-upgrade) at any time.

{% endnote %}

For more information, see [{#T}](../concepts/release-channels-and-updates.md).

## List of available {{ k8s }} versions {#versions-list}

{% list tabs group=instructions %}

- Management console {#console}

  To get a list of available versions for a {{ managed-k8s-name }} cluster:
  1. Go to the [folder](../../resource-manager/concepts/resources-hierarchy.md#folder) page and select **{{ ui-key.yacloud.iam.folder.dashboard.label_managed-kubernetes }}**.
  1. Click the name of the {{ managed-k8s-name }} cluster.
  1. Click **{{ ui-key.yacloud.common.edit }}** in the top-right corner.
  1. View the list of available versions in the **{{ ui-key.yacloud.k8s.clusters.create.field_master-version }}** field under **{{ ui-key.yacloud.k8s.clusters.create.section_main-cluster }}**.

  To get a list of available versions for a {{ managed-k8s-name }} node group:
  1. Go to the folder page and select **{{ ui-key.yacloud.iam.folder.dashboard.label_managed-kubernetes }}**.
  1. Click the name of the {{ managed-k8s-name }} cluster you need and go to the **{{ ui-key.yacloud.k8s.cluster.switch_nodes-manager }}** tab.
  1. Select the {{ managed-k8s-name }} node group from the list and click **{{ ui-key.yacloud.common.edit }}** in the top-right corner.
  1. Get a list of available versions in the **{{ ui-key.yacloud.k8s.node-groups.create.field_node-version }}** field.

- CLI {#cli}

  {% include [cli-install](../../_includes/cli-install.md) %}

  {% include [default-catalogue](../../_includes/default-catalogue.md) %}

  To get a list of available versions, run the following command:

  ```bash
  yc managed-kubernetes list-versions
  ```

- API {#api}

  To get a list of available versions, use the [list](../../managed-kubernetes/api-ref/Version/list.md).

{% endlist %}

## Updating a cluster {#cluster-upgrade}

### Configuring automatic updates when creating or editing a cluster {#cluster-auto-upgrade}

Select automatic update mode for your {{ managed-k8s-name }} cluster and set the update schedule:

{% list tabs group=instructions %}

- Management console {#console}

  You can specify update settings when [creating a {{ managed-k8s-name }} cluster](../../managed-kubernetes/operations/kubernetes-cluster/kubernetes-cluster-create.md) or [updating its settings](../../managed-kubernetes/operations/kubernetes-cluster/kubernetes-cluster-update.md).

  In the **{{ ui-key.yacloud.k8s.clusters.create.field_maintenance-window }}** field, select the {{ managed-k8s-name }} cluster update policy:
  * `{{ ui-key.yacloud.k8s.clusters.create.value_maintenance-disabled }}`: Select this option not to use automatic updates.
  * `{{ ui-key.yacloud.k8s.clusters.create.value_maintenance-anytime }}`: Select this option for {{ managed-k8s-name }} to manage the update installation schedule.
  * `{{ ui-key.yacloud.k8s.clusters.create.value_maintenance-daily }}`: Set the start time and duration of the update.
  * `{{ ui-key.yacloud.k8s.clusters.create.value_maintenance-weekly }}`: Set the day, start time, and duration of the update. If necessary, select multiple options using the **{{ ui-key.yacloud.k8s.clusters.create.button_add-day-of-week }}** button.

- CLI {#cli}

  Set automatic update parameters when [creating](../../managed-kubernetes/operations/kubernetes-cluster/kubernetes-cluster-create.md) or [updating](../../managed-kubernetes/operations/kubernetes-cluster/kubernetes-cluster-update.md) the {{ managed-k8s-name }} cluster:

  ```bash
  {{ yc-k8s }} cluster <create_or_update> <cluster_name_or_ID> \
  ...
    --auto-upgrade <automatic_update_mode> \
    --anytime-maintenance-window \
    --daily-maintenance-window <value> \
    --weekly-maintenance-window <value>
  ```

  Where:
  * `--auto-upgrade`: Automatic {{ managed-k8s-name }} cluster update mode. The default value is `true` (automatic updates are enabled).
  * `--anytime-maintenance-window`: Random {{ managed-k8s-name }} cluster update time.
  * `--daily-maintenance-window`: `{{ ui-key.yacloud.k8s.clusters.create.value_maintenance-daily }}` update mode.

    Example of updating a {{ managed-k8s-name }} cluster daily at 22:00 UTC, with a duration of up to 10 hours:

    ```bash
    --daily-maintenance-window 'start=22:00,duration=10h'
    ```

  * `--weekly-maintenance-window`: Automatic update on specified days.

    Example of updating a {{ managed-k8s-name }} cluster on Mondays and Tuesdays starting at 22:00 UTC, with a duration of up to 10 hours:

    ```bash
    --weekly-maintenance-window 'days=[monday,tuesday],start=22:00,duration=10h'
    ```

    To set multiple maintenance periods, provide the settings of each one in a separate `--weekly-maintenance-window` argument.

    {% note info %}

    The `--daily-maintenance-window` and `--weekly-maintenance-window` arguments require the `--auto-upgrade` argument with the `true` value. The update schedule will not be created if you set `--auto-upgrade=false`.

    {% endnote %}

  You can get the {{ managed-k8s-name }} cluster ID and name with a [list of clusters in the folder](kubernetes-cluster/kubernetes-cluster-list.md).

- {{ TF }} {#tf}

  1. Open the current configuration file with the {{ managed-k8s-name }} cluster description.

     For information about how to create this file, see [{#T}](../../managed-kubernetes/operations/kubernetes-cluster/kubernetes-cluster-create.md).
  1. Change automatic update settings in the {{ managed-k8s-name }} cluster description.

     {% note info %}

     You can select only one of the update modes — daily or on selected days. Concurrent use of update modes is not permitted.

     {% endnote %}

     * To enable the daily update mode:

       ```hcl
       resource "yandex_kubernetes_cluster" "<cluster_name>" {
         name = <cluster_name>
         ...
         maintenance_policy {
           auto_upgrade = true
           maintenance_window {
             start_time = "<update_start_time>"
             duration   = "<update_duration>"
           }
         }
       }
       ```

         Where `start_time` is the update start time in [UTC](https://en.wikipedia.org/wiki/Coordinated_Universal_Time).

     * To enable updates on selected days (multiple periods are possible):

       ```hcl
       resource "yandex_kubernetes_cluster" "<cluster_name>" {
         name = <cluster_name>
         ...
         maintenance_policy {
           auto_upgrade = true
           maintenance_window {
             day        = "<update_start_day,_e.g.,_monday>"
             start_time = "<update_start_time,_UTC>"
             duration   = "<update_duration>"
           }
           maintenance_window {
             day        = "<update_start_day,_e.g.,_monday>"
             start_time = "<update_start_time,_UTC>"
             duration   = "<update_duration>"
           }
         }
       }
       ```

     * To enable the random update time mode, do not add the `maintenance_policy` parameter section to the {{ managed-k8s-name }} cluster description. If you omit automatic update settings in the {{ managed-k8s-name }} cluster description, updates will take place at a random time.
     * To disable automatic updates:

       ```hcl
       resource "yandex_kubernetes_cluster" "<cluster_name>" {
         name = "<cluster_name>"
         ...
         maintenance_policy {
           auto_upgrade = false
         }
       }
       ```

  1. Make sure the configuration files are valid.

     {% include [terraform-validate](../../_includes/mdb/terraform/validate.md) %}

  1. Confirm updating the resources.

     {% include [terraform-apply](../../_includes/mdb/terraform/apply.md) %}

  For more information, see the [{{ TF }} provider documentation]({{ tf-provider-k8s-cluster }}).

- API {#api}

  Set up automatic updates in the `masterSpec.maintenancePolicy` section when [creating a {{ managed-k8s-name }} cluster](../../managed-kubernetes/api-ref/Cluster/create.md) or [updating its settings](../../managed-kubernetes/api-ref/Cluster/update.md).

  Use the [update](../../managed-kubernetes/api-ref/Cluster/update.md) API method and include the following in the request:
  * {{ managed-k8s-name }} cluster ID in the `clusterId` parameter. To find out the {{ managed-k8s-name }} cluster ID, [get a list of clusters in the folder](kubernetes-cluster/kubernetes-cluster-list.md).
  * Automatic update settings in the `masterSpec.maintenancePolicy` parameter.
  * List of settings to be changed in the `updateMask` parameter.

  {% include [updateMask warning](../../_includes/note-api-updatemask.md) %}

  To disable automatic updates, provide the `false` value in the `masterSpec.maintenancePolicy.autoUpgrade` parameter.

  To enable and configure the update window, provide one of the acceptable values of the `maintenanceWindow` parameter:
  * For {{ managed-k8s-name }} clusters to update at a random time, provide the `"anytime": {}` value.
  * To set up daily updates, add the `dailyMaintenanceWindow` section:

    ```json
    "dailyMaintenanceWindow": {
      "startTime": {
        "hours": "<hour_of_update_start>",
        "minutes": "<minute_of_update_start>",
        "seconds": "<second_of_update_start>",
        "nanos": "<fraction_of_a_second_of_update_start>"
      },
      "duration": "<update_duration>"
    }
    ```

    Where:

    * `hours`: Update start hour in [UTC](https://en.wikipedia.org/wikiCoordinated_Universal_Time).
    * `nanos`: Fraction of a second of update start, in nanoseconds.
    * `duration`: Duration of update period, hours.

  * To set up the update on selected days, add the `weeklyMaintenanceWindow` section:

    ```json
    "weeklyMaintenanceWindow": {
      "daysOfWeek": [
        {
          "days": [
            "<list_of_days>"
          ],
          "startTime": {
            "hours": "<hour_of_update_start>",
            "minutes": "<minute_of_update_start>",
            "seconds": "<second_of_update_start>",
            "nanos": "<fraction_of_a_second_of_update_start>"
          },
          "duration": "<update_duration>"
        }
      ]
    }
    ```

      Where:

      * `days`: List of days, e.g., `monday`, `tuesday`.
      * `hours`: Update start hour in [UTC](https://en.wikipedia.org/wiki/Coordinated_Universal_Time).
      * `nanos`: Fraction of a second of update start, in nanoseconds.
      * `duration`: Duration of update period, hours.

{% endlist %}

### Manually updating the cluster version {#cluster-manual-upgrade}

If necessary, update the {{ managed-k8s-name }} cluster version manually. You can only update your {{ managed-k8s-name }} cluster in a single step to the next minor version from the current one. Updating to newer versions is done in steps, e.g., 1.19 → 1.20 → 1.21.

{% list tabs group=instructions %}

- Management console {#console}

  1. Go to the folder page and select **{{ ui-key.yacloud.iam.folder.dashboard.label_managed-kubernetes }}**.
  1. Click the name of the {{ managed-k8s-name }} cluster.
  1. Click **{{ ui-key.yacloud.common.edit }}** in the top-right corner.
  1. In the **{{ ui-key.yacloud.k8s.clusters.create.field_master-version }}** field, select the `Upgrade to version <version_number>` option.
  1. Click **{{ ui-key.yacloud.common.save }}**.

- CLI {#cli}

  Specify the new version of {{ k8s }} in the value of the `--version` argument:

  ```bash
  {{ yc-k8s }} cluster update <cluster_name_or_ID> \
    --version <new_version>
  ```

  You can get the {{ managed-k8s-name }} cluster ID and name with a [list of clusters in the folder](kubernetes-cluster/kubernetes-cluster-list.md).

- {{ TF }} {#tf}

  1. Open the current configuration file with the {{ managed-k8s-name }} cluster description.

     For more information about creating this file, see [{#T}](../../managed-kubernetes/operations/kubernetes-cluster/kubernetes-cluster-create.md).
  1. Change the version in the {{ managed-k8s-name }} cluster description:

     ```hcl
     resource "yandex_kubernetes_cluster" "<cluster_name>" {
       name = <cluster_name>
       ...
       version = "<new_version>"
     }
     ```

  1. Make sure the configuration files are valid.

     {% include [terraform-validate](../../_includes/mdb/terraform/validate.md) %}

  1. Confirm updating the resources.

     {% include [terraform-apply](../../_includes/mdb/terraform/apply.md) %}

  For more information, see the [{{ TF }} provider documentation]({{ tf-provider-k8s-cluster}}).

- API {#api}

  Use the [update](../../managed-kubernetes/api-ref/Cluster/update.md) API method and include the following in the request:
  * {{ managed-k8s-name }} cluster ID in the `clusterId` parameter. To find out the {{ managed-k8s-name }} cluster ID, [get a list of clusters in the folder](kubernetes-cluster/kubernetes-cluster-list.md).
  * Required {{ k8s }} version in the `masterSpec.version.version` parameter.
  * List of settings to be changed in the `updateMask` parameter.

  {% include [Note API updateMask](../../_includes/note-api-updatemask.md) %}

{% endlist %}

## Updating a node group {#node-group-upgrade}

### Configuring automatic updates of a node group {#node-group-auto-upgrade}

Select automatic update mode for the {{ managed-k8s-name }} node group and set the required update schedule:

{% list tabs group=instructions %}

- Management console {#console}

  You can specify update settings when [creating a {{ managed-k8s-name }} cluster](../../managed-kubernetes/operations/node-group/node-group-create.md) or [updating its settings](../../managed-kubernetes/operations/node-group/node-group-update.md).

  In the **{{ ui-key.yacloud.k8s.node-groups.create.section_deploy }}** field, specify scaling settings of the {{ managed-k8s-name }} node group:
  * **{{ ui-key.yacloud.k8s.node-groups.create.field_max-expansion }}**: Set the maximum number of instances by which you can exceed the size of the {{ managed-k8s-name }} node group when updating it.
  * **{{ ui-key.yacloud.k8s.node-groups.create.field_max-unavailable }}**: Set the maximum number of instances by which you can reduce the size of the {{ managed-k8s-name }} node group when updating it.

  In the **{{ ui-key.yacloud.k8s.clusters.create.field_maintenance-window }}** field, select the {{ managed-k8s-name }} node group update policy:
  * `{{ ui-key.yacloud.k8s.clusters.create.value_maintenance-disabled }}`: Select this option not to use automatic updates.
  * `{{ ui-key.yacloud.k8s.clusters.create.value_maintenance-anytime }}`: Select this option for {{ managed-k8s-name }} to manage the update installation schedule.
  * `{{ ui-key.yacloud.k8s.clusters.create.value_maintenance-daily }}`: Set the start time and duration of the update.
  * `{{ ui-key.yacloud.k8s.clusters.create.value_maintenance-weekly }}`: Set the day, start time, and duration of the update. If necessary, select multiple options using the **{{ ui-key.yacloud.k8s.clusters.create.button_add-day-of-week }}** button.

- CLI {#cli}

  Set automatic update parameters when [creating](../../managed-kubernetes/operations/node-group/node-group-create.md) or [updating](../../managed-kubernetes/operations/node-group/node-group-update.md) a {{ managed-k8s-name }} node group.

  ```bash
  {{ yc-k8s }} node-group <create_or_update> <node_group_name_or_ID> \
  ...
    --max-expansion <number_of_instances> \
    --max-unavailable <number_of_instances> \
    --auto-upgrade <automatic_update_mode> \
    --auto-repair <recreation_mode> \
    --anytime-maintenance-window \
    --daily-maintenance-window <value> \
    --weekly-maintenance-window <value>
  ```

   Where:

   * `--max-expansion`: Maximum number of instances by which you can exceed the size of the {{ managed-k8s-name }} node group when updating it.
   * `--max-unavailable`: Maximum number of instances by which you can reduce the size of the {{ managed-k8s-name }} node group when updating it.

      {% note info %}

      The `--max-expansion` and `--max-unavailable` flags should be used together.

      {% endnote %}

   * `--auto-upgrade`: Automatic update mode for the {{ managed-k8s-name }} node group. The default value is `true` (automatic updates are enabled). Valid values: `true` or `false`.
   * `--auto-repair`: Mode for re-creating failed nodes. Valid values: `true` or `false`.

      The `--auto-repair` mode is at the [Preview](../../overview/concepts/launch-stages.md).
   * `--anytime-maintenance-window`: Random update time for the {{ managed-k8s-name }} node group.
   * `--daily-maintenance-window`: `{{ ui-key.yacloud.k8s.clusters.create.value_maintenance-daily }}` update mode.

      Example of updating a {{ managed-k8s-name }} node group daily at 22:00 UTC, with a duration of up to 10 hours:

      ```bash
      --daily-maintenance-window 'start=22:00,duration=10h'
      ```

   * `--weekly-maintenance-window`: Automatic update on specified days.

      Example of updating a {{ managed-k8s-name }} node group on Mondays and Tuesdays from 22:00 UTC, with a duration of up to 10 hours:

      ```bash
      --weekly-maintenance-window 'days=[monday,tuesday],start=22:00,duration=10h'
      ```

      To set multiple maintenance periods, provide the settings of each one in a separate `--weekly-maintenance-window` argument.

      {% note info %}

      The `--daily-maintenance-window` and `--weekly-maintenance-window` arguments require the `--auto-upgrade` argument with the` true` value.

      The update schedule will not be created if you set `--auto-upgrade=false`.

      {% endnote %}

   You can get the {{ managed-k8s-name }} node group ID and name with a [list of the groups in the cluster](node-group/node-group-list.md).

- {{ TF }} {#tf}

   1. Open the current configuration file describing the {{ managed-k8s-name }} node group.

      For more information about creating this file, see [{#T}](../../managed-kubernetes/operations/node-group/node-group-create.md).
   1. Change automatic update settings in the {{ managed-k8s-name }} node group description.

      {% note info %}

      You can select only one of the update modes — daily or on selected days. Concurrent use of update modes is not permitted.

      {% endnote %}

      * To enable the daily update mode:

         ```hcl
         resource "yandex_kubernetes_node_group" "<node_group_name>" {
           name = <node_group_name>
           ...
           maintenance_policy {
             auto_upgrade = true
             maintenance_window {
               start_time = "<update_start_time>"
               duration   = "<update_duration>"
             }
           }
         }
         ```

         Where `start_time` is the update start time in [UTC](https://en.wikipedia.org/wiki/Coordinated_Universal_Time).

      * To enable updates on selected days (multiple periods are possible):

         ```hcl
         resource "yandex_kubernetes_node_group" "<node_group_name>" {
           name = <node_group_name>
           ...
           maintenance_policy {
             auto_upgrade = true
             maintenance_window {
               day        = "<update_start_day,_e.g.,_monday>"
               start_time = "<update_start_time,_UTC>"
               duration   = "<update_duration>"
             }
             maintenance_window {
               day        = "<update_start_day,_e.g.,_monday>"
               start_time = "<update_start_time,_UTC>"
               duration   = "<update_duration>"
             }
           }
         }
         ```

      * To enable the random update time mode, do not add the `maintenance_policy` parameter section to the {{ managed-k8s-name }} node group description. If you omit automatic update settings in the {{ managed-k8s-name }} node group description, updates will take place at a random time.
      * To configure the {{ managed-k8s-name }} node group scaling settings applied at updates:

         ```hcl
         resource "yandex_kubernetes_node_group" "<node_group_name>" {
           name = <node_group_name>
           ...
           deploy_policy {
             max_expansion   = <maximum_number_of_instances_to_increase_node_group_size>
             max_unavailable = <maximum_number_of_instances_to_decrease_node_group_size>
           }
         }
         ```

         {% note info %}

         The `max_expansion` and `max_unavailable` parameters should be used together.

         {% endnote %}

      * To disable automatic updates:

         ```hcl
         resource "yandex_kubernetes_node_group" "<node_group_name>" {
           name = <node_group_name>
           ...
           maintenance_policy {
             auto_upgrade = false
           }
         }
         ```

   1. Make sure the configuration files are valid.

      {% include [terraform-validate](../../_includes/mdb/terraform/validate.md) %}

   1. Confirm updating the resources.

      {% include [terraform-apply](../../_includes/mdb/terraform/apply.md) %}

   For more information, see the [{{ TF }} provider documentation]({{ tf-provider-k8s-nodegroup }}).

- API {#api}

   You can set up automatic updates under `maintenancePolicy` when [creating](../../managed-kubernetes/api-ref/NodeGroup/create.md) or [updating](../../managed-kubernetes/api-ref/NodeGroup/update.md) a {{ managed-k8s-name }} node group.

   Use the [update](../../managed-kubernetes/api-ref/NodeGroup/update.md) API method and include the following in the request:
   * ID of the {{ managed-k8s-name }} node group in the `nodeGroupId` parameter. To find out the {{ managed-k8s-name }} node group ID, get a [list of groups in the cluster](node-group/node-group-list.md).
   * Automatic update settings in the `maintenancePolicy` parameter.
   * List of settings to be changed in the `updateMask` parameter.

   {% include [Note API updateMask](../../_includes/note-api-updatemask.md) %}

   To disable automatic updates, provide the `false` value in the `maintenancePolicy.autoUpgrade` parameter.

   To enable and configure the update window, provide one of the acceptable values of the `maintenanceWindow` parameter:
   * For {{ managed-k8s-name }} node groups to update at a random time, provide the `"anytime": {}` value.
   * To set up daily updates, add the `dailyMaintenanceWindow` section:

      ```json
      "dailyMaintenanceWindow": {
        "startTime": {
          "hours": "<hour_of_update_start>",
          "minutes": "<minute_of_update_start>",
          "seconds": "<second_of_update_start>",
          "nanos": "<fraction_of_a_second_of_update_start>"
        },
        "duration": "<update_duration>"
      }
      ```

      Where:

      * `hours`: Update start hour in [UTC](https://en.wikipedia.org/wiki/Coordinated_Universal_Time).
      * `nanos`: Fraction of a second of update start, in nanoseconds.
      * `duration`: Duration of update period, hours.

   * To set up the update on selected days, add the `weeklyMaintenanceWindow` section:

      ```json
      "weeklyMaintenanceWindow": {
        "daysOfWeek": [
          {
            "days": [
              "<list_of_days>"
            ],
            "startTime": {
              "hours": "<hour_of_update_start>",
              "minutes": "<minute_of_update_start>",
              "seconds": "<second_of_update_start>",
              "nanos": "<fraction_of_a_second_of_update_start>"
            },
            "duration": "<update_duration>"
          }
        ]
      }
      ```

      Where:

      * `days`: List of days, e.g., `monday`, `tuesday`.
      * `hours`: Update start hour in [UTC](https://en.wikipedia.org/wiki/Coordinated_Universal_Time).
      * `nanos`: Fraction of a second of update start, in nanoseconds.
      * `duration`: Duration of update period, hours.

   To set the scaling of a {{ managed-k8s-name }} node group, add the `deployPolicy` section:

   ```json
   "deployPolicy": {
     "maxUnavailable": "<maximum_number_of_instances>",
     "maxExpansion": "<maximum_number_of_instances>"
   }
   ```

   Where:

   * `maxUnavailable`: Maximum number of instances by which you can reduce the size of the node group.
   * `maxExpansion`: Maximum number of instances by which you can expand the size of the node group.

{% endlist %}

### Manually updating a node group version {#node-group-manual-upgrade}

If necessary, update the {{ managed-k8s-name }} node group version manually. You can only update to the next minor version from the current one. Updating to newer versions is done in steps, e.g., 1.19 → 1.20 → 1.21.

{% note warning %}

Update the {{ managed-k8s-name }} cluster version before updating the node group.

{% endnote %}

{% list tabs group=instructions %}

- Management console {#console}

  1. Go to the folder page and select **{{ ui-key.yacloud.iam.folder.dashboard.label_managed-kubernetes }}**.
  1. Click the name of the {{ managed-k8s-name }} cluster.
  1. Go to the **{{ ui-key.yacloud.k8s.cluster.switch_nodes-manager }}** tab.
  1. Select the {{ managed-k8s-name }} node group from the list.
  1. Click **{{ ui-key.yacloud.common.edit }}** in the top-right corner.
  1. In the **{{ ui-key.yacloud.k8s.node-groups.create.field_node-version }}** field, select the `Upgrade to version <version_number>` option.
  1. Click **{{ ui-key.yacloud.common.save }}**.

- CLI {#cli}

  Set automatic update parameters:

  ```bash
  {{ yc-k8s }} node-group update <node_group_ID_or_name> \
  ...
    --version <new_version>
  ```

  You can get the {{ managed-k8s-name }} node group ID and name with a [list of the groups in the cluster](node-group/node-group-list.md).

- {{ TF }} {#tf}

   1. Open the current configuration file describing the {{ managed-k8s-name }} node group.

      For more information about creating this file, see [{#T}](../../managed-kubernetes/operations/node-group/node-group-create.md).
   1. Change the version in the {{ managed-k8s-name }} node group description:

      ```hcl
      resource "yandex_kubernetes_node_group" "<node_group_name>" {
        name = <node_group_name>
        ...
        version = "<new_version>"
      }
      ```

   1. Make sure the configuration files are valid.

      {% include [terraform-validate](../../_includes/mdb/terraform/validate.md) %}

   1. Confirm updating the resources.

      {% include [terraform-apply](../../_includes/mdb/terraform/apply.md) %}

   For more information, see the [{{ TF }} provider documentation]({{ tf-provider-k8s-nodegroup }}).

- API {#api}

   Use the [update](../../managed-kubernetes/api-ref/NodeGroup/update.md) API method and include the following in the request:
   * ID of the {{ managed-k8s-name }} node group in the `nodeGroupId` parameter. To find out the {{ managed-k8s-name }} node group ID, get a [list of groups in the cluster](node-group/node-group-list.md).
   * Required version of {{ k8s }} in the `version.version` parameter.
   * List of settings to be changed in the `updateMask` parameter.

   {% include [Note API updateMask](../../_includes/note-api-updatemask.md) %}

{% endlist %}

## Updating {{ k8s }} components without changing the version {#latest-revision}

For a {{ managed-k8s-name }} cluster and a node group, updates within the same {{ k8s }} version are available. When installing the update, the major version of {{ k8s }} does not change.

This update enables you to:
* Install new packages.
* Update the {{ k8s }} image.
* Update the {{ k8s }} minor version.

The {{ managed-k8s-name }} cluster and node groups will be updated if any of the automatic update options are enabled in their settings.

### Updating for a cluster with automatic updating disabled {#cluster-latest-revision}

{% list tabs group=instructions %}

- Management console {#console}

   1. Go to the folder page and select **{{ ui-key.yacloud.iam.folder.dashboard.label_managed-kubernetes }}**.
   1. Click the name of the {{ managed-k8s-name }} cluster.
   1. Click **{{ ui-key.yacloud.common.edit }}** in the top-right corner.
   1. In the **{{ ui-key.yacloud.k8s.clusters.create.field_master-version }}** field, select `Get the latest improvements and fixes for version...`
   1. Click **{{ ui-key.yacloud.common.save}}**.

- CLI {#cli}

   Run {{ managed-k8s-name }} cluster update:

   ```bash
   {{ yc-k8s }} cluster update <cluster_name_or_ID> \
     --latest-revision
   ```

   You can get the {{ managed-k8s-name }} cluster ID and name with a [list of clusters in the folder](kubernetes-cluster/kubernetes-cluster-list.md#list).

- API {#api}

  Use the [update](../../managed-kubernetes/api-ref/Cluster/update.md) API method and include the following in the request:
  * {{ managed-k8s-name }} cluster ID in the `clusterId` parameter. To find out the {{ managed-k8s-name }} cluster ID, [get a list of clusters in the folder](kubernetes-cluster/kubernetes-cluster-list.md#list).
  * Value `true` in the `masterSpec.version.version` parameter.
  * List of settings to be changed in the `updateMask` parameter.

   {% include [Note API updateMask](../../_includes/note-api-updatemask.md) %}

{% endlist %}

### Updating for a node group with automatic updating disabled {#node-group-latest-revision}

{% list tabs group=instructions %}

- Management console {#console}

   1. Go to the folder page and select **{{ ui-key.yacloud.iam.folder.dashboard.label_managed-kubernetes }}**.
   1. Click the name of the {{ managed-k8s-name }} cluster.
   1. Go to the **{{ ui-key.yacloud.k8s.cluster.switch_nodes-manager }}** tab.
   1. Select the {{ managed-k8s-name }} node group from the list.
   1. Click **{{ ui-key.yacloud.common.edit }}** in the top-right corner.
   1. In the **{{ ui-key.yacloud.k8s.node-groups.create.field_node-version }}** field, select `Get the latest improvements and fixes for version...`
   1. Click **{{ ui-key.yacloud.common.save}}**.

- CLI {#cli}

   Update the {{ managed-k8s-name }} node group:

   ```bash
   {{ yc-k8s }} node-group update <node_group_ID_or_name> \
     --latest-revision
   ```

   You can get the {{ managed-k8s-name }} node group ID and name with a [list of the groups in the cluster](node-group/node-group-list.md).

- API {#api}

   Use the [update](../../managed-kubernetes/api-ref/NodeGroup/update.md) API method and include the following in the request:
   * ID of the {{ managed-k8s-name }} node group in the `nodeGroupId` parameter. To find out the {{ managed-k8s-name }} node group ID, get a [list of groups in the cluster](node-group/node-group-list.md).
   * `true` value in the `version.latestRevision` parameter.
   * List of settings to be changed in the `updateMask` parameter.

   {% include [Note API updateMask](../../_includes/note-api-updatemask.md) %}

{% endlist %}