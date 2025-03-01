---
title: "{{ at-full-name }}. Обзор сервиса"
description: "{{ at-full-name }} позволяет собирать аудитные логи о происходящих с ресурсами {{ yandex-cloud }} событиях и выгружать эти логи для дальнейшего анализа или экспорта."
---

# Обзор сервиса {{ at-full-name }}

{{ at-full-name }} позволяет собирать аудитные логи уровня конфигурации и аудитные логи уровня сервисов ресурсов {{ yandex-cloud }} и загружать их в бакет {{ objstorage-name }}, в поток данных {{ yds-name }} или в лог-группу {{ cloud-logging-name }}:

* [Загрузка аудитных логов в бакет](../operations/index.md#bucket).
* [Загрузка аудитных логов в Cloud Logging](../operations/index.md#logging).
* [Загрузка аудитных логов в поток данных](../operations/index.md#data-streams).

Сбор аудитных логов позволяет применять инструменты анализа и оперативного реагирования на события {{ yandex-cloud }}:

* [Поиск по аудитным логам в бакете](../tutorials/search-bucket.md).
* [Поиск по аудитным логам в лог-группе](../tutorials/search-cloud-logging.md).
* [Загрузка аудитных логов в SIEM](./export-siem.md).
* [Настройка алертов в {{ monitoring-full-name }}](../tutorials/alerts-monitoring.md).

## Аудитные логи уровня конфигурации {#control-plane-logs}

{{ at-name }} собирает аудитные логи [уровня конфигурации (Control Plane)](./control-plane-vs-data-plane.md#control-plane-events) для следующих сервисов {{ yandex-cloud }}:

* {{ alb-full-name }}
* {{ at-full-name }}
* {{ certificate-manager-full-name }}
* {{ dns-full-name }}
* {{ cloud-logging-full-name }}
* {{ compute-full-name }}
* {{ iam-full-name }}
* {{ kms-full-name }}
* {{ lockbox-full-name }}
* {{ mch-full-name }}
* {{ mgl-full-name }}
* {{ mmg-full-name }}
* {{ managed-k8s-name }}
* {{ mmy-full-name }}
* {{ mpg-full-name }}
* {{ mrd-full-name }}
* {{ network-load-balancer-full-name }}
* {{ objstorage-full-name }}
* {{ org-full-name }}
* {{ resmgr-full-name }}
* {{ vpc-full-name }}
* {{ ydb-full-name }}
* {{ yq-full-name }}

Логируются следующие [события уровня конфигурации](./events.md):

* Входы федеративных пользователей.
* Создание/удаление сервисных аккаунтов.
* Создание/удаление ключей сервисных аккаунтов.
* Изменение ролей пользователей и сервисных аккаунтов.
* Создание/удаление ресурсов.
* Изменение настроек ресурсов.
* Остановка/перезагрузка ресурса.
* Изменение политик доступа.
* Создание/изменение групп безопасности.
* Действия с ключами шифрования и секретами.

## Аудитные логи уровня сервисов {#data-plane-logs}

{% include notitle [preview](../../_includes/note-preview-by-request.md) %}

{{ at-name }} собирает аудитные логи [уровня сервисов (Data Plane)](./control-plane-vs-data-plane.md#data-plane-events) для следующих сервисов {{ yandex-cloud }}:

* {{ dns-full-name }}
* {{ lockbox-full-name }}
* {{ kms-full-name }}
* {{ objstorage-full-name }}

Логируются следующие [события уровня сервисов](events-data-plane.md):

* Результат выполнения DNS запроса от клиента Cloud DNS.
* Факт доступа к содержимому секрета.
* Расшифрование шифртекста с помощью ключа.
* Шифрование тестовой строки с помощью ключа.
* Формирование ключа с высокой энтропией для шифрования по схеме Envelope Encryption.
* Расшифрование шифртекста ключом с предыдущей версией и шифрование расшифрованного текста новой версией этого же ключа.
* Изменение ACL объекта в бакете.
* Создание объекта в бакете.
* Удаление объекта из бакета.
* Удаление тегов объекта в бакете.
* Изменение тегов объекта в бакете.
* Изменение объекта в бакете.

## Существующие ограничения сервиса {#known-restrictions}

В аудитный лог не попадают ошибки аутентификации. Например, если пользователь отправит к [API](../../glossary/rest-api.md) запрос без IAM-токена, информации об этом не будет в аудитных логах.

Ошибки авторизации в аудитный лог попадают. Например, в логе будет сообщение об ошибке, если пользователь попытался создать ресурс, но у него не хватило разрешений.

В сервисе действуют [квоты и лимиты](limits.md).

Если вы загружаете аудитные логи в лог-группу или поток данных, то их размер не должен превышать как лимиты {{ at-name }}, так и лимиты сервисов [{{ cloud-logging-full-name }}](../../logging/concepts/limits.md) и [{{ yds-full-name }}](../../data-streams/concepts/limits.md). При превышении лимитов информация в аудитных логах событий большого размера будет неполной.

Рекомендуем также загружать аудитные логи в бакет {{ objstorage-name }}.

{% note info %}

{% include [error-note](../../_includes/audit-trails/error-note.md) %}

{% endnote %}

{% include [clickhouse-disclaimer](../../_includes/clickhouse-disclaimer.md) %}