---
sourcePath: en/tracker/api-ref/concepts/projects/get-projects.md
---
# Get a list of all projects

Use this request to get a list of all organization [projects](../../manager/project-new.md).

You can also use the new, more flexible and functional [getting entity list](../entities/search-entities.md) API that provides a unified method for getting a list of projects and portfolios.

## Request format {#query}

Before making the request, [get permission to access the API](../access.md).

To get a list of projects, use an HTTP `GET` request.

```
GET /{{ ver }}/projects
Host: {{ host }}
Authorization: OAuth <OAuth token>
{{ org-id }}
```

{% include [headings](../../../_includes/tracker/api/headings.md) %}

{% cut "Request parameters" %}

**Additional parameters**

| Parameter | Description | Data type |
-------- | -------- | ----------
| expand | Additional fields to include in the response:<ul><li>`queues`: Project queues </li></ul> | String |

{% endcut %}

## Response format {#answer}

{% list tabs %}

- Request executed successfully

   {% include [answer-200](../../../_includes/tracker/api/answer-200.md) %}

   The response body contains information about all the organization's projects in JSON format.

   ```json
   {
       "self": "https://{{ host }}/v2/projects/1",
       "id": "1",
       "version": 1,
       "key": "Project 1",
       "name": "Project 1",
       "description": "First project",
       "lead": {
           "self": "https://{{ host }}/v2/users/12********",
           "id": "12********",
           "display": "Full Name"
       },
       "status": "launched",
       "startDate": "2020-11-01",
       "endDate": "2020-12-01"
   },
   {
       "self": "https://{{ host }}/v2/projects/2",
       "id": "2",
       "version": 1,
       "key": "Project 2",
       "name": "Project 2",
       "description": "Another project",
       "lead": {
           "self": "https://{{ host }}/v2/users/12********",
           "id": "12********",
           "display": "Full Name"
       },
       "status": "launched",
       "startDate": "2020-11-02",
       "endDate": "2020-12-02"
   },
   {
       ...
   },
   {
       ...
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
   | status | Stage of the project:<ul><li>`DRAFT`: Draft</li><li>`IN_PROGRESS`: In progress</li><li>`LAUNCHED`: Launched</li><li>`POSTPONED`: Postponed </li></ul> | String |
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


{% endlist %}