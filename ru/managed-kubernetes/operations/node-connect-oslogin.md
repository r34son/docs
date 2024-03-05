---
title: "Как подключиться к узлу кластера {{ k8s }} через {{ oslogin }} в {{ managed-k8s-full-name }}"
description: "Следуя данной инструкции, вы сможете подключиться к узлу через {{ oslogin }}."
---

# Подключение к узлу через {{ oslogin }}

[{{ oslogin }}](../../organization/concepts/os-login.md) используется вместо SSH-ключей для доступа к виртуальным машинам {{ yandex-cloud }} через SSH. С помощью {{ oslogin }} вы можете подключиться к узлам {{ managed-k8s-name }}.

{% note info %}

Для подключения через {{ oslogin }} на узле должен быть включен [внешний доступ](./node-group/node-group-update.md#node-internet-access).

{% endnote %}

[Настройте узел кластера](#configure-node), после чего подключитесь к нему одним из двух способов:

* [с помощью CLI](#connect-via-cli);
* [с помощью SSH](#connect-via-ssh).

## Перед началом работы

1. {% include [cli-install](../../_includes/cli-install.md) %}

   {% include [default-catalogue](../../_includes/default-catalogue.md) %}

1. [Включите доступ через {{ oslogin }}](../../organization/operations/os-login-access.md) на уровне организации.

1. Убедитесь, что аккаунту, с которого вы подключаетесь к узлу, [назначена одна из ролей](../../iam/operations/roles/grant.md):

    * `compute.osLogin` — для доступа к узлу без прав sudo;
    * `compute.osAdminLogin` — для доступа с правами sudo.

## Настройте узел {#configure-node}

Подготовьте узел кластера к подключению:

1. Убедитесь, что для узла включен [внешний доступ](./node-group/node-group-update.md#node-internet-access).

1. Чтобы активировать доступ к узлу через {{ oslogin }}:

   {% list tabs group=instructions %}

   - С помощью CLI {#cli}

      Добавьте параметр `enable-oslogin=true` в конфигурацию узла:

      ```bash
      {{ yc-k8s }} node-group update --name <имя_группы_узлов> --metadata enable-oslogin=true
      ```

   - С помощью {{ TF }} {#tf}

      1. Откройте актуальный конфигурационный файл {{ TF }} с описанием группы узлов {{ managed-k8s-name }}.

         О том, как создать такой файл, см. в разделе [{#T}](./node-group/node-group-create.md).

      1. Добавьте параметр `enable-oslogin = "true"` в блок `metadata`:

         ```hcl
         resource "yandex_kubernetes_node_group" "<имя_группы_узлов>" {
         ...
           instance_template {
           ...
             metadata         = {
               enable-oslogin = "true"
             }
           }
         }
         ```
      1. Проверьте корректность конфигурационных файлов.

         {% include [terraform-validate](../../_includes/mdb/terraform/validate.md) %}

      1. Подтвердите изменение ресурсов.

         {% include [terraform-apply](../../_includes/mdb/terraform/apply.md) %}

       Подробнее см. в [документации провайдера {{ TF }}]({{ tf-provider-k8s-nodegroup }}).

   {% endlist %}

## Подключитесь к узлу с помощью CLI {#connect-via-cli}

1. Посмотрите описание команды CLI для подключения к узлу:

    ```bash
    yc compute ssh --help
    ```

1. Чтобы узнать имя нужного узла, получите список узлов в кластере:

    ```bash
    {{ yc-k8s }} node-group list-nodes --name <имя_группы_узлов>
    ```

    Пример результата:

    ```bash
    +----------------------+-----------------+---------------------------+-------------+--------+
    | CLOUD INSTANCE       | KUBERNETES NODE | RESOURCES                 | DISK        | STATUS |
    +----------------------+-----------------+---------------------------+-------------+--------+
    | fhmmh23ugigb******** | <имя_узла>      | 4 100% core(s), 8.0 GB of | 64.0 GB ssd | READY  |
    | RUNNING_ACTUAL       |                 | memory                    |             |        |
    +----------------------+-----------------+---------------------------+-------------+--------+
    ```

1. Подключитесь к узлу:

    ```bash
    yc compute ssh --name <имя_узла>
    ```

## Подключитесь к узлу с помощью SSH {#connect-via-ssh}

1. [Экспортируйте сертификат](../../compute/operations/vm-connect/os-login-export-certificate.md) {{ oslogin }}.

   {% note info %}

   Сертификат действителен один час. По истечении этого времени для подключения к узлу экспортируйте новый сертификат.

   {% endnote %}

1. Узнайте публичный адрес узла:

   1. Получите идентификатор группы узлов:

      ```bash
      {{ yc-k8s }} node-group list
      ```

      Результат:

      ```bash
      +------------------------------+----------------------+-----------+----------------------+---------------------+---------+------+
      |          ID                  |      CLUSTER ID      |   NAME    |  INSTANCE GROUP ID   |     CREATED AT      | STATUS  | SIZE |
      +------------------------------+----------------------+-----------+----------------------+---------------------+---------+------+
      | <идентификатор_группы_узлов> | cato4gqs0ave******** | ng-name   | cl17a1c3mbau******** | 2024-02-08 04:25:06 | RUNNING |    1 |
      +------------------------------+----------------------+-----------+----------------------+---------------------+---------+------+
      ```

      Нужный параметр находится в столбце `ID`.

   1. Посмотрите список узлов {{ managed-k8s-name }}, которые принадлежат этой группе:

      ```bash
      yc compute instance-group list-instances <идентификатор_группы_узлов>
      ```

      Результат:

      ```bash
      +----------------------+---------------------------+----------------+-------------+----------------------+----------------+
      |     INSTANCE ID      |           NAME            |  EXTERNAL IP   | INTERNAL IP |        STATUS        | STATUS MESSAGE |
      +----------------------+---------------------------+----------------+-------------+----------------------+----------------+
      | fhm8nq5p7t0r******** | cl12kvrgj493rhrkimmb-**** | 84.201.156.211 | 10.128.0.36 | RUNNING_ACTUAL [25m] |                |
      +----------------------+---------------------------+----------------+-------------+----------------------+----------------+
      ```

      Публичный IP-адрес узла {{ managed-k8s-name }} указан в столбце `EXTERNAL IP`.

1. Подключитесь к ВМ:

    ```bash
    ssh -i <путь_к_файлу_сертификата> <имя_пользователя>@<публичный_IP-адрес_узла>
    ```

    Где:

    * `<путь_к_файлу_сертификата>` — путь к сохраненному ранее файлу `Identity` сертификата. Например: `/home/user1/.ssh/yc-cloud-id-b1gia87mbaom********-orgusername`.
    * `<имя_пользователя>` — имя пользователя в организации. Оно указано в конце имени экспортированного сертификата {{ oslogin }}. В примере выше это `orgusername`.
    * `<публичный_IP-адрес_узла>` — полученный ранее публичный адрес узла.

    При первом подключении к узлу появится предупреждение о неизвестном хосте:

    ```text
    The authenticity of host '158.160.**.** (158.160.**.**)' can't be established.
    ECDSA key fingerprint is SHA256:PoaSwqxRc8g6iOXtiH7ayGHpSN0MXwUfWHk********.
    Are you sure you want to continue connecting (yes/no)?
    ```

    Введите в терминале слово `yes` и нажмите **Enter**.