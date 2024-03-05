# Получить информацию о группе виртуальных машин

После создания группы виртуальных машин вы можете получить основную информацию о группе.

Пользовательские [метаданные](../../concepts/vm-metadata.md), которые были переданы при создании или изменении группы, можно получить только с помощью CLI или API.

Чтобы получить информацию о группе виртуальных машин:

{% list tabs group=instructions %}

- Консоль управления {#console}

  1. В [консоли управления]({{ link-console-main }}) откройте каталог, в котором находится нужная группа ВМ.
  1. Выберите сервис **{{ ui-key.yacloud.iam.folder.dashboard.label_compute }}**.
  1. На панели слева выберите ![image](../../../_assets/console-icons/layers-3-diagonal.svg) **{{ ui-key.yacloud.compute.switch_groups }}**.
  1. Нажмите на имя нужной группы.

- CLI {#cli}

  {% include [cli-install.md](../../../_includes/cli-install.md) %}

  {% include [default-catalogue.md](../../../_includes/default-catalogue.md) %}

  1. Посмотрите описание команды CLI для получения информации о группе виртуальных машин:

      ```bash
      {{ yc-compute-ig }} get --help
      ```

  1. Получите список групп виртуальных машин в каталоге по умолчанию:

      {% include [instance-group-list.md](../../../_includes/instance-groups/instance-group-list.md) %}

  1. Выберите идентификатор (`ID`) или имя (`NAME`) нужной группы, например `first-instance-group`.
  1. Получите информацию о группе виртуальных машин:

      ```bash
      {{ yc-compute-ig }} get --name first-instance-group
      ```
      
- {{ TF }}

  {% include [terraform-definition](../../../_tutorials/_tutorials_includes/terraform-definition.md) %}

  {% include [terraform-install](../../../_includes/terraform-install.md) %}

  Чтобы получить информацию о группе ВМ с помощью {{ TF }}:

  1. Опишите в конфигурационном файле {{ TF }} параметры ресурсов, которые необходимо создать:

      ```
      data "yandex_compute_instance_group" "my_group" {
        instance_group_id = "<идентификатор_группы_ВМ>"
      }

      output "instancegroupvm_external_ip" {
        value = "${data.yandex_compute_instance_group.my_group.instances.*.network_interface.0.nat_ip_address}"
      }
      ```

      Где:

      * `data "yandex_compute_instance_group"` — описание источника данных для получения информации о группе ВМ:
         * `instance_group_id` — идентификатор группы ВМ.
      * `output "instancegroupvm_external_ip"` — список всех внешних IP-адресов виртуальных машин из группы, который будет выводиться в результате:
         * `value` — возвращаемое значение.
      
     Более подробную информацию о параметрах источника данных `yandex_compute_instance_group` см. в [документации провайдера]({{ tf-provider-datasources-link }}/datasource_compute_instance_group).

  1. Создайте ресурсы:

      {% include [terraform-validate-plan-apply](../../../_tutorials/_tutorials_includes/terraform-validate-plan-apply.md) %}

      {{ TF }} создаст все требуемые ресурсы и отобразит значения выходных переменных в терминале. Чтобы проверить результат, выполните команду:

      ```bash
      terraform output instancegroupvm_external_ip
      ```

      Результат:

      ```bash
      instancegroupvm_external_ip = tolist([
        "158.160.112.7",
        "158.160.2.119",
      ])
      ```
      
- API {#api}

  Воспользуйтесь методом REST API [get](../../api-ref/InstanceGroup/get.md) для ресурса [InstanceGroup](../../api-ref/InstanceGroup/index.md) или вызовом gRPC API [InstanceGroupService/Get](../../api-ref/grpc/instance_group_service.md#Get).

  Список доступных групп запрашивайте методом REST API [listInstances](../../api-ref/InstanceGroup/listInstances.md) или вызовом gRPC API [InstanceGroupService/ListInstances](../../api-ref/grpc/instance_group_service.md#ListInstances).


{% endlist %}
