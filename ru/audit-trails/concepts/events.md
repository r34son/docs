# Справочник событий уровня конфигурации

Значение поля `event_type` (_тип события_) аудитного лога уровня конфигурации (Control Plane) определяется сервисом-источником события.

Общий вид значения:

```text
{{ at-event-prefix }}.audit.<имя_сервиса>.<имя_события>
```

Ниже описаны события для сервисов:

* [{{ api-gw-name }}](#api-gateway)
* [{{ alb-name }}](#alb)
* [{{ at-name }}](#audit-trails)
* [{{ certificate-manager-name }}](#certificate-manager)
* [{{ dns-name }}](#dns)
* [{{ sf-name }}](#functions)
* [{{ backup-name }}](#backup)
* [{{ cdn-name }}](#cdn)
* [{{ cloud-logging-name }}](#cloud-logging)
* [{{ compute-name }}](#compute)
* [{{ container-registry-name }}](#container-registry)
* [{{ dataproc-name }}](#dataproc)
* [{{ data-transfer-name }}](#datatransfer)
* [{{ ml-platform-name }}](#datasphere)
* [{{ iam-name }}](#iam)
* [{{ iot-name }}](#iot)
* [{{ kms-name }}](#kms)
* [{{ lockbox-name }}](#lockbox)
* [{{ mkf-short-name }}](#managed-service-for-kafka)
* [{{ mch-short-name }}](#managed-service-for-clickhouse)
* [{{ mgl-full-name }}](#managed-service-for-gitlab)
* [{{ mgp-short-name }}](#managed-service-for-greenplum)
* [{{ mmg-short-name }}](#managed-service-for-mongodb)
* [{{ managed-k8s-name }}](#managed-service-for-kubernetes)
* [{{ mmy-short-name }}](#managed-service-for-mysql)
* [{{ mpg-short-name }}](#managed-service-for-postgresql)
* [{{ mrd-short-name }}](#managed-service-for-redis)
* [{{ mes-short-name }}](#managed-service-for-elasticsearch)
* [{{ mos-short-name }}](#managed-service-for-opensearch)
* [{{ network-load-balancer-name }}](#network-load-balancer)
* [{{ objstorage-name }}](#objstorage)
* [{{ search-api-name }}](#searchapi)
* [{{ serverless-containers-name }}](#serverless-containers)
* [{{ org-name }}](#organization)
* [{{ resmgr-name }}](#resmgr)
* [{{ sws-name }}](#smartwebsecurity)
* [{{ captcha-name }}](#smartcaptcha)
* [{{ vpc-name }}](#vpc)
* [{{ ydb-short-name }}](#ydb)
* [{{ yq-short-name }}](#yq)

## {{ api-gw-name }} {#api-gateway}

Имя сервиса — `serverless.apigateway`.

Имя события | Описание
--- | ---
`CreateApiGateway` | Создание шлюза
`DeleteApiGateway` | Удаление шлюза
`UpdateApiGateway` | Изменение шлюза


## {{ alb-name }} {#alb}

Имя сервиса — `apploadbalancer`.

{% include [alb-events](../../_includes/audit-trails/events/alb-events.md) %}

## {{ at-name }} {#audit-trails}

Имя сервиса — `audittrails`.

Имя события | Описание
--- | ---
`CreateTrail` | Создание трейла
`DeleteTrail` | Удаление трейла
`SetTrailAccessBindings` | Назначение привязок прав доступа для трейла
`UpdateTrail` | Изменение трейла
`UpdateTrailAccessBindings` | Изменение привязок прав доступа для трейла

## {{ sf-name }} {#functions}

Имя сервиса — `serverless`.

Имя события | Описание
--- | ---
`functions.CreateFunction` | Создание функции
`functions.CreateFunctionVersion` | Создание версии функции
`functions.DeleteFunction` | Удаление функции
`functions.DeleteFunctionVersion` | Удаление версии функции
`functions.RemoveFunctionTag` | Удаление тега функции
`functions.RemoveScalingPolicy` | Удаление политики масштабирования функции
`functions.SetFunctionTag` | Назначение тега функции
`functions.SetFunctionAccessBindings` | Назначение привязок прав доступа для функции
`functions.SetScalingPolicy` | Назначение политики масштабирования функции
`functions.UpdateFunction` | Изменение функции
`functions.UpdateFunctionAccessBindings` | Изменение привязок прав доступа для функции
`mdbproxy.CreateProxy` | Создание прокси
`mdbproxy.DeleteProxy` | Удаление прокси
`mdbproxy.UpdateProxy` | Изменение прокси
`triggers.CreateTrigger` | Создание триггера
`triggers.DeleteTrigger` | Удаление триггера
`triggers.UpdateTrigger` | Изменение триггера


## {{ backup-name }} {#backup}

Имя сервиса — `backup`.

{% include [backup-events](../../_includes/audit-trails/events/backup-events.md) %}


## {{ cdn-name }} {#cdn}

Имя сервиса — `cdn`.

{% include [cdn-events](../../_includes/audit-trails/events/cdn-events.md) %}

## {{ certificate-manager-name }} {#certificate-manager}

Имя сервиса — `certificatemanager`.

{% include [cm-events](../../_includes/audit-trails/events/cm-events.md) %}

## {{ dns-name }} {#dns}

Имя сервиса — `dns`.

{% include [dns-events](../../_includes/audit-trails/events/dns-events.md) %}

## {{ cloud-logging-name }} {#cloud-logging-name}

Имя сервиса — `logging`.

Имя события | Описание
--- | ---
`CreateLogGroup` | Создание лог-группы
`UpdateLogGroup` | Изменение лог-группы
`DeleteLogGroup` | Удаление лог-группы
`SetLogGroupAccessBindings` | Назначение привязок прав доступа для лог-группы
`UpdateLogGroupAccessBindings` | Изменение привязок прав доступа для лог-группы

## {{ compute-name }} {#compute}

Имя сервиса — `compute`.

{% include [compute-events](../../_includes/audit-trails/events/compute-events.md) %}

## {{ container-registry-name }} {#container-registry}

Имя сервиса — `containerregistry`.

Имя события | Описание
--- | ---
`CreateImage` | Создание образа
`CreateImageTag` | Создание тега образа
`CreateLifecyclePolicy` | Создание политики жизненного цикла
`CreateRegistry` | Создание реестра
`CreateRepository` | Создание репозитория
`CreateScanPolicy` | Создание политики сканирования
`DeleteImage` | Удаление образа
`DeleteImageTag` | Удаление тега образа
`DeleteLifecyclePolicy` | Удаление политики жизненного цикла
`DeleteRegistry` | Удаление реестра
`DeleteRepository` | Удаление репозитория
`DeleteScanPolicy` | Удаление политики сканирования
`ScanImage` | Сканирование образа
`UpdateIpPermission` | Изменение политики доступа с IP-адресов
`UpdateLifecyclePolicy` | Изменение политики жизненного цикла
`UpdateRegistry` | Изменение реестра
`UpdateScanPolicy` | Изменение политики сканирования
`UpdateRegistryAccessBindings` | Изменение привязок прав доступа на реестр  
`UpdateRepositoryAccessBindings` | Изменение привязок прав доступа на репозиторий
`SetRegistryAccessBindings`  | Назначение привязок прав доступа на реестр
`SetRepositoryAccessBindings` | Назначение привязок прав доступа на репозиторий

## {{ dataproc-name }} {#dataproc}

Имя сервиса — `dataproc`.

Имя события | Описание
--- | ---
`CreateCluster` | Создание кластера
`CreateSubcluster` | Создание подкластера
`DeleteCluster` | Удаление кластера
`DeleteSubcluster` | Удаление подкластера
`StartCluster` | Запуск кластера
`StopCluster` | Остановка кластера
`UpdateCluster` | Изменение кластера
`UpdateSubcluster` | Изменение подкластера

## {{ data-transfer-name }} {#datatransfer}

Имя сервиса — `datatransfer`.

Имя события | Описание
--- | ---
`ActivateTransfer` | Активация трансфера
`CreateEndpoint` | Создание эндпоинта
`CreateTransfer` | Создание трансфера
`DeactivateTransfer` | Деактивация трансфера
`DeleteEndpoint` | Удаление эндпоинта
`DeleteTransfer` | Удаление трансфера
`FreezeTransferVersion` | Фиксация для трансфера определенной версии data plane
`RestartTransfer` | Перезапуск трансфера
`UnfreezeTransferVersion` | Разрешение обновления трансфера до последней версии data planе
`UpdateEndpoint` | Изменение эндпоинта
`UpdateTransfer` | Изменение трансфера
`UpdateTransferVersion` | Обновление версии data planе трансфера

## {{ ml-platform-name }} {#datasphere}

Имя сервиса — `datasphere`.

Имя события | Описание
--- | ---
`CreateCommunity` | Создание сообщества
`CreateProject` | Создание проекта
`DeleteCommunity` | Удаление сообщества
`DeleteProject` | Удаление проекта
`SetCommunityAccessBindings` | Назначение привязок прав доступа для сообщества
`SetProjectAccessBindings` | Назначение привязок прав доступа для проекта
`UpdateCommunity` | Изменение сообщества
`UpdateCommunityAccessBindings` | Изменение привязок прав доступа для сообщества
`UpdateProject` | Изменение проекта
`UpdateProjectAccessBindings` | Изменение привязок прав доступа для проекта

## {{ iam-name }} {#iam}

Имя сервиса — `iam`.

{% include [iam-events](../../_includes/audit-trails/events/iam-events.md) %}

## {{ iot-name }} {#iot}

Имя сервиса — `iot`.

Имя события | Описание
--- | ---
`devices.CreateDevice` | Создание устройства
`devices.CreateRegistry` | Создание реестра
`devices.DeleteDevice` | Удаление устройства
`devices.DeleteRegistry` | Удаление реестра
`devices.UpdateDevice` | Изменение устройства
`devices.UpdateRegistry` | Изменение реестра

## {{ kms-name }} {#kms}

Имя сервиса — `kms`.

{% include [kms-events](../../_includes/audit-trails/events/kms-events.md) %}

## {{ lockbox-name }} {#lockbox}

Имя сервиса — `lockbox`.

{% include [lockbox-events](../../_includes/audit-trails/events/lockbox-events.md) %}

## {{ mkf-short-name }} {#managed-service-for-kafka}

Имя сервиса — `mdb.kafka`

Имя события | Описание
--- | ---
`CreateCluster` | Создание кластера
`DeleteCluster` | Удаление кластера
`StartCluster` | Запуск кластера
`StopCluster` | Остановка кластера
`UpdateCluster` | Изменение кластера
`CreateConnector` | Создание коннектора
`CreateTopic` | Создание топика
`CreateUser` | Создание пользователя кластера
`DeleteConnector` | Удаление коннектора
`DeleteTopic` | Удаление топика
`DeleteUser` | Удаление пользователя кластера
`GrantUserPermission` | Назначение прав пользователю кластера
`MoveCluster` | Перемещение кластера
`PauseConnector` | Приостановка коннектора
`ResumeConnector` | Возобновление работы коннектора
`RevokeUserPermission` | Отзыв прав у пользователя кластера
`UpdateConnector` | Изменение коннектора
`UpdateTopic` | Изменение топика
`UpdateUser` | Изменение пользователя кластера

## {{ mch-short-name }} {#managed-service-for-clickhouse}

Имя сервиса — `mdb.clickhouse`.

Имя события | Описание
--- | ---
`AddClusterHosts` | Добавление новых хостов в кластер
`AddClusterShard` | Добавление шарда в кластер
`AddClusterZookeeper` | Добавление подкластера ZooKeeper в кластер
`BackupCluster` | Создание резервной копии
`CreateCluster` | Создание кластера
`CreateClusterExternalDictionary` | Создание внешнего словаря
`CreateDatabase` | Создание базы данных
`CreateFormatSchema` | Создание схемы формата данных
`CreateMlModel` | Создание модели машинного обучения
`CreateShardGroup` | Создание группы шардов
`CreateUser` | Создание пользователя базы данных
`DeleteCluster` | Удаление кластера
`DeleteClusterExternalDictionary` | Изменение внешнего словаря
`DeleteClusterHosts` | Удаление хостов из кластера
`DeleteClusterShard` | Удаление шарда из кластера
`DeleteDatabase` | Удаление базы данных
`DeleteFormatSchema` | Удаление схемы формата данных
`DeleteMlModel` | Удаление модели машинного обучения
`DeleteShardGroup` | Удаление группы шардов
`DeleteUser` | Удаление пользователя базы данных
`GrantUserPermission` | Назначение прав пользователю базы данных
`MoveCluster` | Перемещение кластера
`RestoreCluster` | Создание нового кластера из резервной копии
`RevokeUserPermission` | Отзыв прав у пользователя базы данных
`StartCluster` | Запуск кластера
`StopCluster` | Остановка кластера
`UpdateCluster` | Изменение кластера
`UpdateClusterShard` | Изменение шарда в кластере
`UpdateFormatSchema` | Изменение схемы формата данных
`UpdateMlModel` | Изменение модели машинного обучения
`UpdateShardGroup` | Изменение группы шардов
`UpdateUser` | Изменение пользователя базы данных


## {{ mgl-name }} {#managed-service-for-gitlab}

Имя сервиса — `gitlab`.

Имя события | Описание
--- | ---
`BackupInstance` | Создание резервной копии
`CreateInstance` | Создание инстанса
`DeleteInstance` | Удаление инстанса
`StartInstance` | Запуск инстанса
`StopInstance` | Остановка инстанса
`UpdateInstance` | Изменение инстанса
`UpdateOmniauthInstance` | Изменение настроек OmniAuth
`UpgradeInstance` | Обновление версии GitLab
`CleanupRegistryInstance` | Очистка Docker Registry
`ResizeInstance` | Изменение размера инстанса


## {{ mgp-short-name }} {#managed-service-for-greenplum}

Имя сервиса — `mdb.greenplum`.

Имя события | Описание
--- | ---
`CreateCluster` | Создание кластера
`DeleteCluster` | Удаление кластера
`ExpandCluster` | Расширение кластера
`RestoreCluster` | Создание нового кластера из резервной копии
`StartCluster` | Запуск кластера
`StopCluster` | Остановка кластера
`UpdateCluster` | Изменение кластера


## {{ mmg-short-name }} {#managed-service-for-mongodb}

Имя сервиса — `mdb.mongodb`.

Имя события | Описание
--- | ---
`AddClusterHosts` | Добавление новых хостов в кластер
`AddClusterShard` | Добавление шарда в кластер
`BackupCluster` | Создание резервной копии
`CreateCluster` | Создание кластера
`CreateDatabase` | Создание базы данных
`CreateUser` | Создание пользователя базы данных
`DeleteCluster` | Удаление кластера
`DeleteClusterHosts` | Удаление хостов из кластера
`DeleteClusterShard` | Удаление шарда из кластера
`DeleteDatabase` | Удаление базы данных
`DeleteUser` | Удаление пользователя базы данных
`EnableClusterSharding` | Включение шардирования для кластера
`GrantUserPermission` | Назначение прав пользователю базы данных
`MoveCluster` | Перемещение кластера
`RestoreCluster` | Создание нового кластера из резервной копии
`RevokeUserPermission` | Отзыв прав у пользователя базы данных
`StartCluster` | Запуск кластера
`StopCluster` | Остановка кластера
`UpdateCluster` | Изменение кластера
`UpdateUser` | Изменение пользователя базы данных
`ResetupHosts` | Ресинхронизация хоста
`RestartHosts` | Перезагрузка хоста
`StepdownHosts` | Смена мастера хоста


## {{ managed-k8s-name }} {#managed-service-for-kubernetes}

Имя сервиса — `k8s`.

{% include [managed-k8s-events](../../_includes/audit-trails/events/managed-k8s-events.md) %}

## {{ mmy-short-name }} {#managed-service-for-mysql}

Имя сервиса — `mdb.mysql`.

Имя события | Описание
--- | ---
`AddClusterHosts` | Добавление новых хостов в кластер
`BackupCluster` | Создание резервной копии
`CreateCluster` | Создание кластера
`CreateDatabase` | Создание базы данных
`CreateUser` | Создание пользователя базы данных
`DeleteBackup` | Удаление резервной копии
`DeleteCluster` | Удаление кластера
`DeleteClusterHosts` | Удаление хостов из кластера
`DeleteDatabase` | Удаление базы данных
`DeleteUser` | Удаление пользователя базы данных
`GrantUserPermission` | Назначение прав пользователю базы данных
`MoveCluster` | Перемещение кластера
`RescheduleMaintenance` | Перенос запланированного технического обслуживания
`RestoreCluster` | Создание нового кластера из резервной копии
`RevokeUserPermission` | Отзыв прав у пользователя базы данных
`StartCluster` | Запуск кластера
`StartClusterFailover` | Запуск переключения мастера для кластера
`StopCluster` | Остановка кластера
`UpdateCluster` | Изменение кластера
`UpdateClusterHosts` | Изменение хостов в кластере
`UpdateUser` | Изменение пользователя базы данных

## {{ mpg-short-name }} {#managed-service-for-postgresql}

Имя сервиса — `mdb.postgresql`.

Имя события | Описание
--- | ---
`AddClusterHosts` | Добавление новых хостов в кластер
`BackupCluster` | Создание резервной копии
`CreateCluster` | Создание кластера
`CreateDatabase` | Создание базы данных
`CreateUser` | Создание пользователя базы данных
`DeleteBackup` | Удаление резервной копии
`DeleteCluster` | Удаление кластера
`DeleteClusterHosts` | Удаление хостов из кластера
`DeleteDatabase` | Удаление базы данных
`DeleteUser` | Удаление пользователя базы данных
`GrantUserPermission` | Назначение прав пользователю базы данных
`MoveCluster` | Перемещение кластера
`RestoreCluster` | Создание нового кластера из резервной копии
`RevokeUserPermission` | Отзыв прав у пользователя базы данных
`StartCluster` | Запуск кластера
`StopCluster` | Остановка кластера
`UpdateCluster` | Изменение кластера
`UpdateClusterHosts` | Изменение хостов в кластере
`UpdateDatabase` | Изменение базы данных
`UpdateUser` | Изменение пользователя базы данных

## {{ mrd-short-name }} {#managed-service-for-redis}

Имя сервиса — `mdb.redis`.

Имя события | Описание
--- | ---
`AddClusterHosts` | Добавление новых хостов в кластер
`AddClusterShard` | Добавление шарда в кластер
`BackupCluster` | Создание резервной копии
`CreateCluster` | Создание кластера
`DeleteCluster` | Удаление кластера
`DeleteClusterHosts` | Удаление хостов из кластера
`DeleteClusterShard` | Удаление шарда из кластера
`MoveCluster` | Перемещение кластера
`RebalanceCluster` | Перебалансировка кластера
`RestoreCluster` | Создание нового кластера из резервной копии
`StartCluster` | Запуск кластера
`StartClusterFailover` | Запуск переключения мастера для кластера
`StopCluster` | Остановка кластера
`UpdateCluster` | Изменение кластера
`UpdateClusterHosts` | Изменение хостов кластера

## {{ mes-short-name }} {#managed-service-for-elasticsearch}

Имя сервиса — `mdb.elasticsearch`.

Имя события | Описание
--- | ---
`AddClusterHosts` | Добавление новых хостов в кластер
`BackupCluster` | Создание резервной копии
`CreateCluster` | Создание кластера
`DeleteCluster` | Удаление кластера
`DeleteClusterHosts` | Удаление хостов из кластера
`RescheduleMaintenance` | Перенос запланированного технического обслуживания
`RestoreCluster` | Создание нового кластера из резервной копии
`StartCluster` | Запуск кластера
`StopCluster` | Остановка кластера
`UpdateCluster` | Изменение кластера

## {{ mos-short-name }} {#managed-service-for-opensearch}

Имя сервиса — `mdb.opensearch`.

Имя события | Описание
--- | ---
`AddDashboardsNodeGroup` | Добавление группы хостов типа `Dashboards`
`AddOpenSearchNodeGroup` | Добавление группы хостов типа `OpenSearch`
`BackupCluster` | Создание резервной копии
`CreateCluster` | Создание кластера
`DeleteCluster` | Удаление кластера
`DeleteDashboardsNodeGroup` | Удаление группы хостов типа `Dashboards`
`DeleteOpenSearchNodeGroup` | Удаление группы хостов типа `OpenSearch`
`RescheduleMaintenance` | Перенос запланированного технического обслуживания
`RestoreCluster` | Создание нового кластера из резервной копии
`StartCluster` | Запуск кластера
`StopCluster` | Остановка кластера
`UpdateCluster` | Изменение кластера
`UpdateOpenSearchNodeGroup` | Изменение группы хостов типа `OpenSearch`

## {{ network-load-balancer-name }} {#network-load-balancer}

Имя сервиса — `loadbalancer`.

{% include [network-load-balancer-events](../../_includes/audit-trails/events/network-load-balancer-events.md) %}

## {{ objstorage-name }} {#objstorage}

Имя сервиса — `storage`.

{% include [storage-events](../../_includes/audit-trails/events/storage-events.md) %}

## {{ search-api-name }} {#searchapi}

Имя сервиса — `searchapi`.

Имя события | Описание
--- | ---
`CreateCustomer` | Создание клиента
`DeleteCustomer` | Удаление клиента
`UpdateCustomer` | Изменение клиента

## {{ serverless-containers-name }} {#serverless-containers}

Имя сервиса — `serverless.containers`.

Имя события | Описание
--- | ---
`CreateContainer` | Создание контейнера
`DeleteContainer` | Удаление контейнера
`DeployContainerRevision` | Создание ревизии контейнера
`RollbackContainer` | Откат контейнера к целевой ревизии
`SetContainerAccessBindings` | Назначение привязок прав доступа к контейнеру 
`UpdateContainer` | Изменение контейнера
`UpdateContainerAccessBindings` | Изменение привязок прав доступа к контейнеру
`mdbproxy.CreateProxy` | Создание прокси
`mdbproxy.DeleteProxy` | Удаление прокси
`mdbproxy.UpdateProxy` | Изменение прокси
`triggers.CreateTrigger` | Создание триггера
`triggers.DeleteTrigger` | Удаление триггера
`triggers.UpdateTrigger` | Изменение триггера

## {{ org-name }} {#organization}

Имя сервиса — `organizationmanager`.

{% include [org-events](../../_includes/audit-trails/events/org-events.md) %}

## {{ resmgr-name }} {#resmgr}

Имя сервиса — `resourcemanager`.

{% include [resmgr-events](../../_includes/audit-trails/events/resmgr-events.md) %}

## {{ sws-name }} {#smartwebsecurity}

Имя сервиса — `smartwebsecurity`.

Имя события | Описание
--- | ---
`CreateSecurityProfile` | Создание профиля безопасности
`DeleteSecurityProfile` | Удаление профиля безопасности
`UpdateSecurityProfile` | Изменение профиля безопасности

## {{ captcha-name }} {#smartcaptcha}

Имя сервиса — `smartcaptcha`.

{% include [captcha-events](../../_includes/audit-trails/events/captcha-events.md) %}

## {{ vpc-name }} {#vpc}

Имя сервиса — `network`.

{% include [vpc-events](../../_includes/audit-trails/events/vpc-events.md) %}

## {{ ydb-short-name }} {#ydb}

Имя сервиса — `ydb`.

Имя события | Описание
--- | ---
`BackupDatabase` | Создание [бэкапа](../../glossary/backup.md) базы данных
`CreateDatabase` | Создание базы данных
`DeleteBackup` | Удаление бэкапа базы данных
`DeleteDatabase` | Удаление базы данных
`MoveDatabase` | Перемещение базы данных
`RestoreBackup` | Восстановление базы данных из бэкапа
`StartDatabase` | Запуск базы данных
`StopDatabase` | Остановка базы данных
`UpdateDatabase` | Изменение базы данных

## {{ yq-short-name }} {#yq}

Имя сервиса — `yq`.

Имя события | Описание
--- | ---
`ControlQuery` | Управление запросом
`CreateBinding` | Создание привязки к данным
`CreateConnection` | Создание соединения
`CreateQuery` | Создание запроса
`DeleteBinding` | Удаление привязки к данным
`DeleteConnection` | Удаление соединения
`DeleteQuery` | Удаление запроса
`UpdateBinding` | Изменение привязки к данным
`UpdateConnection` | Изменение соединения
`UpdateQuery` | Изменение запроса

{% include [clickhouse-disclaimer](../../_includes/clickhouse-disclaimer.md) %}