# Управление доступом в {{ yds-name }}

Для управления правами доступа в {{ yds-name }} используются [роли](../../iam/concepts/access-control/roles.md).

Пользователь {{ yandex-cloud }} может выполнять только те операции над ресурсами, которые разрешены назначенными ему ролями. Пока у пользователя нет никаких ролей, почти все операции ему запрещены.

Чтобы разрешить доступ к ресурсам сервиса {{ yds-full-name }} (потоки данных, базы данных {{ ydb-full-name }}, где хранятся потоки, и их пользователи), назначьте аккаунту на Яндексе, [сервисному аккаунту](../../iam/concepts/users/service-accounts.md), [федеративным пользователям](../../iam/concepts/federations.md), [группе пользователей](../../organization/operations/manage-groups.md) или [системной группе](../../iam/concepts/access-control/system-group.md) нужные роли из приведенного ниже списка. На данный момент роль может быть назначена только на родительский ресурс (каталог или облако), роли которого наследуются вложенными ресурсами.

Назначать роли на ресурс могут те, у кого есть роль `admin`, `resource-manager.clouds.owner` или `organization-manager.organizations.owner` на этот ресурс.

{% note info %}

Подробнее о наследовании ролей читайте в разделе [{#T}](../../resource-manager/concepts/resources-hierarchy.md#access-rights-inheritance) документации сервиса {{ resmgr-full-name }}.

{% endnote %}

## Назначение ролей {#grant-roles}

Чтобы назначить пользователю роль:

{% include [grant-role-console](../../_includes/grant-role-console.md) %}

## Какие роли действуют в сервисе {#roles-list}

Ниже перечислены все роли, которые учитываются при проверке прав доступа в сервисе {{ yds-name }}.

### Сервисные роли {#service-roles}

#### yds.viewer {#yds-viewer}

{% include [yds.viewer](../../_roles/yds/viewer.md) %}

#### yds.writer {#yds-writer}

{% include [yds.writer](../../_roles/yds/writer.md) %}

#### yds.editor {#yds-editor}

{% include [yds.editor](../../_roles/yds/editor.md) %}

#### yds.admin {#yds-admin}

{% include [yds.admin](../../_roles/yds/admin.md) %}

### Примитивные роли {#primitive-roles}

#### {{ roles-viewer }} {#viewer}

Пользователь с ролью `{{ roles-viewer }}` может просматривать информацию о ресурсах, например посмотреть список потоков данных, баз данных, где созданы потоки, и их свойств.

#### {{ roles-editor }} {#editor}

Пользователь с ролью `{{ roles-editor }}` может управлять любыми ресурсами, например создать поток данных или его удалить. Кроме этого, данная роль позволяет записывать данные из приложений в потоки данных.

Помимо этого роль `{{ roles-editor }}` включает в себя все разрешения роли `{{ roles-viewer }}`.

#### {{ roles-admin }} {#admin}

Пользователь с ролью `{{ roles-admin }}` может управлять правами доступа к ресурсам, например разрешить другим пользователям создавать потоки данных или просматривать информацию о них.

Помимо этого роль `{{ roles-admin }}` включает в себя все разрешения роли `{{ roles-editor }}`.
