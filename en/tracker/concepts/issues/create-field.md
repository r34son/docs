---
sourcePath: en/tracker/api-ref/concepts/issues/create-field.md
---
# Creating an issue field

Use this request to create an issue [global field](../../user/create-param#section_global_field) of issue.

## Request format {#query}

Before making a request, [get permission to access the API](../access.md).

To create a field, use an HTTP `POST` request. Request parameters are provided in the request body in JSON format:

```json
POST /{{ ver }}/fields
Host: {{ host }}
Authorization: OAuth <OAuth token>
{{ org-id }}

{
    "name":
    {
        "en": "Name in English",
        "ru": "Name in Russian"
    },
    "id": "global_field_key",
    "category": "0000000000000001********",
    "type": "ru.yandex.startrek.core.fields.StringFieldType"
}
```

{% include [headings](../../../_includes/tracker/api/headings.md) %}

{% cut "Request body parameters" %}

**Required parameters**

| Parameter | Description | Data type |
----- | ----- | -----
| name | Field name:<ul><li>`en`: In English</li><li>`ru`: In Russian</li></ul> | String |
| id | Field ID | String |
| category | Object with information about the field category.<br/>To get a list of all categories, use the HTTP request:<br/>`GET /v2/fields/categories` | String |
| type | Field type:<ul><li>`ru.yandex.startrek.core.fields.DateFieldType`: Date</li><li>`ru.yandex.startrek.core.fields.DateTimeFieldType`: Date/time</li><li>`ru.yandex.startrek.core.fields.StringFieldType`: One-line text field</li><li>`ru.yandex.startrek.core.fields.TextFieldType`: Multi-line text field</li><li>`ru.yandex.startrek.core.fields.FloatFieldType`: Fractional number</li><li>`ru.yandex.startrek.core.fields.IntegerFieldType`: Integer</li><li>`ru.yandex.startrek.core.fields.UserFieldType`: User's name</li><li>`ru.yandex.startrek.core.fields.UriFieldType`: Link</li> | String |

**Additional parameters**

| Parameter | Description | Data type |
----- | ----- | -----
| [optionsProvider](#optionsProvider1) | Object with information about the list items | Object |
| order | Sequence number in the list of organization fields: [{{ link-admin-fields }}]({{ link-admin-fields }}) | Number |
| description | Field description | String |
| readonly | Shows if the field value is editable:<ul><li>`true`: Non-editable</li><li>`false`: Editable</li></ul> | Logical |
| visible | Indicates if the field is visible in the interface:<ul><li>`true`: Always visible</li><li>`false`: Not visible</li></ul> | Logical |
| hidden | Indicates if the field should be hidden in the interface:<ul><li>`true`: Hide the field even if it is not empty</li><li>`false`: Do not hide the field</li></ul> | Logical |
| container | Indicates if you can specify multiple values in the field (like in the **Tags** field):<ul><li>`true`: You can specify multiple values in the field</li><li>`false`: You can only specify one value in the field</li></ul>This parameter can be used for the following types of fields:<ul><li>`ru.yandex.startrek.core.fields.StringFieldType`: One-line text field</li><li>`ru.yandex.startrek.core.fields.UserFieldType`: User's name</li><li>Drop-down list (see the `optionsProvider` description)</li></ul> | Logical |

`optionsProvider` **object fields** {#optionsProvider1}

| Parameter | Description | Data type |
-------- | -------- | ----------
| type | Type of drop-down list: <ul><li>`FixedListOptionsProvider`: List of strings or numbers (for the `ru.yandex.startrek.core.fields.StringFieldType` or `ru.yandex.startrek.core.fields.IntegerFieldType` field types)</li><li>`FixedUserListOptionsProvider`: List of users (for the `ru.yandex.startrek.core.fields.UserFieldType` field type)</li></ul> | String |
| values | Drop-down list values | Array of strings |

{% endcut %}

> Example: Create a local field of the **Drop-down list** type with a fixed set of string values.
>
> - An HTTP POST method is used.
> - Field type: `FixedListOptionsProvider`.
> - Drop-down list values: list item 1, list item 2, list item 3.
>
> ```json
> POST /v2/fields
> Host: {{ host }}
> Authorization: OAuth <OAuth_token>
> X-Org-Id or X-Cloud-Org-Id: <organization_ID>
>
> {
>    "name":
>    {
>        "en": "Name in English",
>        "ru": "Name in Russian"
>    },
>    "id": "myglobalfield",
>    "category": "0000000000000003********",
>    "type": "ru.yandex.startrek.core.fields.StringFieldType",
>    "optionsProvider": {
>        "type": "FixedListOptionsProvider",
>        "values": [
>            "list item 1",
>            "list item 1",
>            "list item 3"
>        ]
>    }
> }
> ```
>
> {% note info %}
>
> If you only have a {{ org-full-name }} organization, use the `X-Cloud-Org-ID` header; if only {{ ya-360 }} or both organization types, use `X-Org-ID`.
>
> {% endnote %}

## Response format {#answer}

{% list tabs %}

- Request executed successfully

   {% include [answer-200](../../../_includes/tracker/api/answer-200.md) %}

   The response body contains information about the created issue field in JSON format.

   ```json
    {
     "self": "https://{{ host }}/v2/fields/global_field_key",
     "id": "global_field_key",
     "name": "Field name in Russian",
     "description": "Field description",
     "key": "global_field_key",
     "version": 1,
     "schema": {
         "type": "array",
         "items": "string",
         "required": false
     },
     "readonly": false,
     "options": true,
     "suggest": false,
     "optionsProvider": {
         "type": "FixedListOptionsProvider",
         "needValidation": true,
         "values": [
             "List item 1",
             "List item 2",
             "List item 3"
         ]
     },
     "queryProvider": {
         "type": "StringOptionalQueryProvider"
     },
     "order": 5,
     "category": {
         "self": "https://{{ host }}/v2/fields/categories/0000000000000001********",
         "id": "0000000000000001********",
         "display": "System"
     },
     "type": "standard"
   }
   ```

   {% cut "Response parameters" %}

   | Parameter | Description | Data type |
   ----- | ----- | -----
   | type | Field type | String |
   | self | Address of the API resource with information about the field | String |
   | id | Unique field ID | String |
   | name | Field name | String |
   | description | Field description | String |
   | key | Field key | String |
   | version | Field version; each change to the field increases the version number | Number |
   | [schema](#schema) | Object with information about the field value's data type | Object |
   | readonly | Shows if the field value is editable:<ul><li>`true`: Non-editable</li><li>`false`: Editable</li></ul> | Logical |
   | options | Shows if the list of values is restricted:<ul><li>`true`: The list of values is not restricted, you can set any value</li><li>`false`: The list of values is restricted by the organization's settings</li></ul> | Logical |
   | suggest | Enables/disables search suggestions when entering field values:<ul><li>`true`: Enabled</li><li>`false`: Disabled</li></ul> | Logical |
   | [optionsProvider](#optionsProvider) | Object with information about the drop-down list items | Object |
   | [queryProvider](#queryProvider) | Object with information about the query language class.<br/>You cannot change the class using the API. | Object |
   | order | Sequence number in the list of organization fields: [{{ link-admin-fields }}]({{ link-admin-fields }}) | Number |
   | [category](#category) | Object with information about the field category.<br/>To get a list of all categories, use the HTTP request:<br/>`GET /v2/fields/categories` | Object |


   `schema` **object fields** {#schema}

   | Parameter | Description | Data type |
   -------- | -------- | ----------
   | type | Field value type Possible data types:<ul><li>`string`: String; available for fields with a single value</li><li>`array`: Array; available for fields with multiple values</li></ul> | String |
   | items | Value type; available for fields with multiple values | String |
   | required | Shows if the field is required:<ul><li>`true`: Required</li><li>`false`: Optional</li></ul> | Logical |

   `optionsProvider` **object fields** {#optionsProvider}

   | Parameter | Description | Data type |
   -------- | -------- | ----------
   | type | Type of drop-down list | String |
   | needValidation | Indicates if a list value requires validation:<ul><li>`true`: Yes</li><li>`false`: Do not validate the field value</li></ul> | Logical |
   | values | List items | Array of strings |

   `queryProvider` **object fields** {#queryProvider}

   | Parameter | Description | Data type |
   -------- | -------- | ----------
   | type | Query language type | String |

   `category` **object fields** {#category}

   | Parameter | Description | Data type |
   -------- | -------- | ----------
   | self | Address of the API resource with information about the field category | String |
   | id | Field category ID | String |
   | display | Category name displayed | String |

   {% endcut %}

- Request failed

   If the request is processed incorrectly, the API returns a response with an error code:

   {% include [answer-error-400](../../../_includes/tracker/api/answer-error-400.md) %}

   {% include [answer-error-401](../../../_includes/tracker/api/answer-error-401.md) %}

   {% include [answer-error-403](../../../_includes/tracker/api/answer-error-403.md) %}

   {% include [answer-error-404](../../../_includes/tracker/api/answer-error-404.md) %}

   {% include [answer-error-422](../../../_includes/tracker/api/answer-error-422.md) %}

   {% include [answer-error-500](../../../_includes/tracker/api/answer-error-500.md) %}

   {% include [answer-error-503](../../../_includes/tracker/api/answer-error-503.md) %}

{% endlist %}