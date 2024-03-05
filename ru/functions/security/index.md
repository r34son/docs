---
title: "Управление доступом в {{ sf-name }}"
description: "Управление доступом сервиса для запуска приложений без создания и обслуживания виртуальных машин — {{ sf-name }}. В разделе описано, на какие ресурсы можно назначить роль, какие роли действуют в сервисе."
---

# Управление доступом в {{ sf-name }}

В этом разделе вы узнаете:

* [на какие ресурсы можно назначить роль](#resources);
* [какие роли действуют в сервисе](#roles-list).

{% include [about-access-management](../../_includes/iam/about-access-management.md) %}

## На какие ресурсы можно назначить роль {#resources}

Роль можно назначить на [облако](../../resource-manager/concepts/resources-hierarchy.md#cloud), [каталог](../../resource-manager/concepts/resources-hierarchy.md#folder) и [функцию](../concepts/function.md). Роли, назначенные на облако или каталог, действуют и на функции, которые находятся в них.

## Какие роли действуют в сервисе {#roles-list}

Ниже перечислены все роли, которые учитываются при проверке прав доступа в сервисе {{ sf-name }}.

### Сервисные роли {#service-roles}

#### functions.auditor {#functions-auditor}

{% include [functions.auditor](../../_roles/functions/auditor.md) %}

#### functions.viewer {#functions-viewer}

{% include [functions.viewer](../../_roles/functions/viewer.md) %}

#### functions.functionInvoker {#functions-functionInvoker}

{% include [functions.functionInvoker](../../_roles/functions/functionInvoker.md) %}

#### functions.editor {#functions-editor}

{% include [functions.editor](../../_roles/functions/editor.md) %}


#### functions.mdbProxiesUser {#functions-mdbProxiesUser}

{% include [functions.mdbProxiesUser](../../_roles/functions/mdbProxiesUser.md) %}


#### functions.admin {#functions-admin}

{% include [functions.admin](../../_roles/functions/admin.md) %}

### Примитивные роли {#primitive-roles}

{% include [roles-primitive](../../_includes/roles-primitive.md) %}
