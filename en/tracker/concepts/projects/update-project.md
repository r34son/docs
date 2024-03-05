---
sourcePath: en/tracker/api-ref/concepts/projects/update-project.md
---
# Edit a project

Use this request to update information about a [project](../../manager/project-new.md).

You can also use the new, more flexible and functional [updating entity information](../entities/update-entity.md) API that provides a unified method for updating information about projects and portfolios.

## Request format {#query}

Before making the request, [get permission to access the API](../access.md).

To edit a project, use an HTTP `PUT` request. Request parameters are provided in the request body in JSON format.

```
PUT /{{ ver }}/projects/<project_ID>?version=<version_number>
Host: {{ host }}
Authorization: OAuth <OAuth_token>
{{ org-id }}

{
    "queues": "<queue_key>"
}
```

{% include [headings](../../../_includes/tracker/api/headings.md) %}

{% cut "Resource" %}

| Parameter | Description | Data type |
-------- | -------- | ----------
| \<project_ID> | Project ID | Number |

{% endcut %}


{% cut "Request parameters" %}

**Required parameters**

| Parameter | Description | Data type |
-------- | -------- | ----------
| version | Project version. Changes are only made to the current version of the project. | Number |

**Additional parameters**

| Parameter | Description | Data type |
-------- | -------- | ----------
| expand | Additional fields to include in the response:<ul><li>`queues`: Project queues </li></ul> | String |

{% endcut %}

{% cut "Request body parameters" %}

**Required parameters**

| Parameter | Description | Data type |
-------- | -------- | ----------
| queues | Issues to include in the project | String |

**Additional parameters**

| Parameter | Description | Data type |
-------- | -------- | ----------
| name | Project name | String |
| description | Project description. This parameter is not displayed in the {{ tracker-name }} interface. | String |
| lead | ID or username of the project assignee | Number / String |
| status | Stage of the project:<ul><li>`DRAFT`: Draft</li><li>`IN_PROGRESS`: In progress</li><li>`LAUNCHED`: Launched</li><li>`POSTPONED`: Postponed </li></ul> | String |
| startDate | Project start date in `YYYY-MM-DD` format | String |
| endDate | Project end date in `YYYY-MM-DD` format | String |

{% endcut %}

## Response format {#answer}

{% list tabs %}

- Request executed successfully

   {% include [answer-200](../../../_includes/tracker/api/answer-200.md) %}

   The response body contains information about the updated project in JSON format.

   ```json
   {
       "self": "https://{{ host }}/v2/projects/9",
       "id": "9",
       "version": 5,
       "key": "Project",
       "name": "Project",
       "description": "Project with updates",
       "lead": {
           "self": "https://{{ host }}/v2/users/12********",
           "id": "12********",
           "display": "Full Name"
       },
       "status": "launched",
       "startDate": "2020-11-16",
       "endDate": "2020-12-16"
   }
   ```

   {% cut "Response parameters" %}

   | Parameter | Description | Data type |
   -------- | -------- | ----------
   | self | Address of the API resource with information about the project. | String |
   | id | Project ID. | Number |
   | version | Project version. Each change of the parameters increases the version number. | Number |
   | key | Project key. Matches the project name. | String |
   | name | Project name | String |
   | description | Project description. This parameter is not displayed in the {{ tracker-name }} interface. | String |
   | lead | Block with information about the project assignee | Object |
   | status | Stage of the project:<ul><li>`DRAFT`: Draft</li><li>`IN_PROGRESS`: In progress</li><li>`LAUNCHED`: Launched</i><li>`POSTPONED`: Postponed </li></ul> | String |
   | startDate | Project start date in `YYYY-MM-DD` format | String |
   | endDate | Project end date in `YYYY-MM-DD` format | String |

   `lead` **object fields**

   | Parameter | Description | Data type |
   -------- | -------- | ----------
   | self | Address of the API resource with information about the user. | String |
   | id | User ID. | Number |
   | display | Displayed user name. | String |

   {% endcut %}


- Request failed

   If the request is processed incorrectly, the API returns a response with an error code:

   {% include [answer-error-400](../../../_includes/tracker/api/answer-error-400.md) %}

   {% include [answer-error-401](../../../_includes/tracker/api/answer-error-401.md) %}

   {% include [answer-error-403](../../../_includes/tracker/api/answer-error-403.md) %}

   {% include [answer-error-412](../../../_includes/tracker/api/answer-error-412.md) %}

   {% include [answer-error-428](../../../_includes/tracker/api/answer-error-428.md) %}


{% endlist %}