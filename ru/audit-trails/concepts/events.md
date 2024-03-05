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
* [{{ cloud-apps-name }}](#cloud-apps)
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
* [{{ load-testing-name }}](#loadtesting)
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

{% include [api-gw-events](../../_includes/audit-trails/events/api-gw-events.md) %}


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

{% include [functions-events](../../_includes/audit-trails/events/functions-events.md) %}


## {{ backup-name }} {#backup}

Имя сервиса — `backup`.

{% include [backup-events](../../_includes/audit-trails/events/backup-events.md) %}


## {{ cdn-name }} {#cdn}

Имя сервиса — `cdn`.

{% include [cdn-events](../../_includes/audit-trails/events/cdn-events.md) %}

## {{ certificate-manager-name }} {#certificate-manager}

Имя сервиса — `certificatemanager`.

{% include [cm-events](../../_includes/audit-trails/events/cm-events.md) %}

## {{ cloud-apps-name }} {#cloud-apps}

Имя сервиса — `cloudapps`.

Имя события | Описание
--- | ---
`CreateCloudApplication` | Создание приложения
`DeleteCloudApplication` | Удаление приложения
`UpdateCloudApplication` | Изменение приложения

## {{ dns-name }} {#dns}

Имя сервиса — `dns`.

{% include [dns-events](../../_includes/audit-trails/events/dns-events.md) %}

## {{ cloud-logging-name }} {#cloud-logging-name}

Имя сервиса — `logging`.

{% include [logging-events](../../_includes/audit-trails/events/logging-events.md) %}


## {{ compute-name }} {#compute}

Имя сервиса — `compute`.

{% include [compute-events](../../_includes/audit-trails/events/compute-events.md) %}

## {{ container-registry-name }} {#container-registry}

Имя сервиса — `containerregistry`.

{% include [container-registry-events](../../_includes/audit-trails/events/container-registry-events.md) %}

## {{ dataproc-name }} {#dataproc}

Имя сервиса — `dataproc`.

{% include [dataproc-events](../../_includes/audit-trails/events/dataproc-events.md) %}

## {{ data-transfer-name }} {#datatransfer}

Имя сервиса — `datatransfer`.

{% include [datatransfer-events](../../_includes/audit-trails/events/datatransfer-events.md) %}

## {{ ml-platform-name }} {#datasphere}

Имя сервиса — `datasphere`.

{% include [ds-events](../../_includes/audit-trails/events/ds-events.md) %}

## {{ iam-name }} {#iam}

Имя сервиса — `iam`.

{% include [iam-events](../../_includes/audit-trails/events/iam-events.md) %}

## {{ iot-name }} {#iot}

Имя сервиса — `iot`.

{% include [iot-events](../../_includes/audit-trails/events/iot-events.md) %}

## {{ kms-name }} {#kms}

Имя сервиса — `kms`.

{% include [kms-events](../../_includes/audit-trails/events/kms-events.md) %}

## {{ load-testing-name }} {#loadtesting}

Имя сервиса — `loadtesting`.

{% include [load-testing-events](../../_includes/audit-trails/events/load-testing-events.md) %}

## {{ lockbox-name }} {#lockbox}

Имя сервиса — `lockbox`.

{% include [lockbox-events](../../_includes/audit-trails/events/lockbox-events.md) %}

## {{ mkf-short-name }} {#managed-service-for-kafka}

Имя сервиса — `mdb.kafka`

{% include [managed-kafka-events](../../_includes/audit-trails/events/managed-kafka-events.md) %}

## {{ mch-short-name }} {#managed-service-for-clickhouse}

Имя сервиса — `mdb.clickhouse`.

{% include [mch-events](../../_includes/audit-trails/events/mch-events.md) %}


## {{ mgl-name }} {#managed-service-for-gitlab}

Имя сервиса — `gitlab`.

{% include [mgl-events](../../_includes/audit-trails/events/mgl-events.md) %}


## {{ mgp-short-name }} {#managed-service-for-greenplum}

Имя сервиса — `mdb.greenplum`.

{% include [mgp-events](../../_includes/audit-trails/events/managed-greenplum-events.md) %}


## {{ mmg-short-name }} {#managed-service-for-mongodb}

Имя сервиса — `mdb.mongodb`.

{% include [mongodb-events](../../_includes/audit-trails/events/mongodb-events.md) %}


## {{ managed-k8s-name }} {#managed-service-for-kubernetes}

Имя сервиса — `k8s`.

{% include [managed-k8s-events](../../_includes/audit-trails/events/managed-k8s-events.md) %}

## {{ mmy-short-name }} {#managed-service-for-mysql}

Имя сервиса — `mdb.mysql`.

{% include [mysql-events](../../_includes/audit-trails/events/mysql-events.md) %}

## {{ mpg-short-name }} {#managed-service-for-postgresql}

Имя сервиса — `mdb.postgresql`.

{% include [mpg-events](../../_includes/audit-trails/events/managed-postgresql-events.md) %}

## {{ mrd-short-name }} {#managed-service-for-redis}

Имя сервиса — `mdb.redis`.

{% include [mrd-events](../../_includes/audit-trails/events/mrd-events.md) %}

## {{ mes-short-name }} {#managed-service-for-elasticsearch}

Имя сервиса — `mdb.elasticsearch`.

{% include [mes-events](../../_includes/audit-trails/events/mes-events.md) %}

## {{ mos-short-name }} {#managed-service-for-opensearch}

Имя сервиса — `mdb.opensearch`.

{% include [mos-events](../../_includes/audit-trails/events/mos-events.md) %}

## {{ network-load-balancer-name }} {#network-load-balancer}

Имя сервиса — `loadbalancer`.

{% include [network-load-balancer-events](../../_includes/audit-trails/events/network-load-balancer-events.md) %}

## {{ objstorage-name }} {#objstorage}

Имя сервиса — `storage`.

{% include [storage-events](../../_includes/audit-trails/events/storage-events.md) %}

## {{ search-api-name }} {#searchapi}

Имя сервиса — `searchapi`.

{% include [searchapi-events](../../_includes/audit-trails/events/searchapi-events.md) %}

## {{ serverless-containers-name }} {#serverless-containers}

Имя сервиса — `serverless.containers`.

{% include [serverless-containers-events](../../_includes/audit-trails/events/serverless-containers-events.md) %}

## {{ org-name }} {#organization}

Имя сервиса — `organizationmanager`.

{% include [org-events](../../_includes/audit-trails/events/org-events.md) %}

## {{ resmgr-name }} {#resmgr}

Имя сервиса — `resourcemanager`.

{% include [resmgr-events](../../_includes/audit-trails/events/resmgr-events.md) %}

## {{ sws-name }} {#smartwebsecurity}

Имя сервиса — `smartwebsecurity`.

{% include [sws-events](../../_includes/audit-trails/events/sws-events.md) %}

## {{ captcha-name }} {#smartcaptcha}

Имя сервиса — `smartcaptcha`.

{% include [captcha-events](../../_includes/audit-trails/events/captcha-events.md) %}

## {{ vpc-name }} {#vpc}

Имя сервиса — `network`.

{% include [vpc-events](../../_includes/audit-trails/events/vpc-events.md) %}

## {{ ydb-short-name }} {#ydb}

Имя сервиса — `ydb`.

{% include [ydb-events](../../_includes/audit-trails/events/ydb-events.md) %}

## {{ yq-short-name }} {#yq}

Имя сервиса — `yq`.

{% include [yq-events](../../_includes/audit-trails/events/yq-events.md) %}


{% include [clickhouse-disclaimer](../../_includes/clickhouse-disclaimer.md) %}