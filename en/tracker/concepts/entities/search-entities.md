---
sourcePath: en/tracker/api-ref/concepts/entities/search-entities.md
---
# Getting a list of entities

Use this request to get a list of entities that meet specific criteria.

The request is a unified method for getting a list of projects and portfolios – more flexible and functional than the [getting the list of projects](../projects/get-projects.md) API.

## Request format {#query}

Before making a request, [get permission to access the API](../access.md).

To get a list of entities, use an HTTP `POST` request. Request parameters are provided in the request body in JSON format.

```json
POST /{{ ver }}/entities/{entityType}/_search?fields=entityStatus
Host: {{ host }}
Authorization: OAuth <OAuth_token>
{{ org-id }}

{
   "filter":
   {
      "author": "<username>"
   }   
}
```

{% include [headings](../../../_includes/tracker/api/headings.md) %}

{% cut "Resource" %}

**Resource**

| Parameter | Description | Data type |
-------- | -------- | ----------
| \<entity_type> | Entity type:<ul><li>project for a project</li><li>portfolio for a portfolio</li></ul> | String |

{% endcut %}

{% cut "Request parameters" %}

**Additional parameters**

| Parameter | Description | Data type |
----- | ----- | -----
| [fields](./about-entities.md#query-params) | Additional fields to include in the response | String |
| perPage | Number of issues per response page. The default value is 50. | Number |
| page | Page with search results. The default value is 1. | Number |

{% note warning %}

Entity field keys sometimes differ from similar issue keys.

{% endnote %}

{% endcut %}

{% cut "Request body parameters" %}

The request body contains information required to search for entities:

**Additional parameters**

| Parameter | Description | Data type |
-------- | -------- | ----------
| input | Substring in the entity name | String |
| filter | Issue filtering parameters. The parameter can specify any field key and value for filtering. | Object |
| orderBy | Issue sorting parameters. The parameter can specify any field key for sorting. | String |
| orderAsc | Sorting direction | Logical |
| rootOnly | Output only entities that are not nested | Logical |

{% note warning %}

Entity field keys and value keys, e.g., statuses, sometimes differ from similar issue keys.

{% endnote %}

{% endcut %}

> Example 1: Getting a list of entities using a filter with an extended list of fields in search results
>
> - An HTTP POST method is used.
> - The response should only contain projects in progress with a non-empty list of followers.
> - The `Status`, `Reporter`, and `Followers` field values are displayed in the response.
>
> ```
> POST /v2/entities/project/_search?fields=entityStatus&author&followers HTTP/1.1
> Host: {{ host }}
> Authorization: OAuth <OAuth-token>
> {{ org-id }}
>
> {
>   "filter": {
>     "followers": "notEmpty()",
>     "entityStatus": "in_progress"
>   },
> }
> ```

> Example 2: Getting a sorted list of all entities
>
> - An HTTP POST method is used.
> - The response should contain all portfolios sorted by status.
> - The results are sorted in descending order.
> - The `Status` and `Reporter` field values are displayed in the response.
>
> ```
> POST /v2/entities/project/_search?fields=entityStatus&author HTTP/1.1
> Host: {{ host }}
> Authorization: OAuth <OAuth-token>
> {{ org-id }}
>
> {
>   "orderBy": "entityStatus",
>   "orderAsc":true
> }
> ```

## Response format {#section_xc3_53j_p1b}

{% list tabs %}

- Request executed successfully

   {% include [answer-200](../../../_includes/tracker/api/answer-200.md) %}

   The response body contains the results in JSON format.

   ```json
   {
      "hits": 8,
      "pages": 1,
      "values": [
        {
            "self": "https://{{ host }}/{{ ver }}/entities/project/655f8ce5d6a39833********",
            "id": "655f8ce5d6a39833********",
            "version": 7,
            "shortId": 8,
            "entityType": "project",
            "createdBy": { "self": "https://{{ host }}/{{ ver }}/users/11********", "id": "11********", "display": "Full Name", "cloudUid": "ajevuhegoggf********", "passportUid": 11******** },
            "createdAt": "2023-11-23T17:33:25.427+0000",
            "updatedAt": "2023-11-24T12:21:43.838+0000",
            "fields": {
                "entityStatus": "in_progress",
                "followers": [{ "self": "https://{{ host }}/{{ ver }}/users/22********", "id": "22********", "display": "Follower name", "cloudUid": "ajeprm0fjnql********", "passportUid": 22******** }]
            }
        },
        {
            "self": "https://{{ host }}/{{ ver }}/entities/project/655f3815a834c763********",
            "id": "655f3815a834c763********",
            "version": 4,
            "shortId": 3,
            "entityType": "project",
            "createdBy": { "self": "https://{{ host }}/{{ ver }}/users/11********", "id": "11********", "display": "Full Name", "cloudUid": "ajevuhegoggf********", "passportUid": 11******** },
            "createdAt": "2023-11-23T11:31:33.874+0000",
            "updatedAt": "2023-11-23T21:52:10.863+0000",
            "fields": {
                "entityStatus": "draft",
                "followers": [{ "self": "https://{{ host }}/{{ ver }}/users/22********", "id": "22********", "display": "Follower name", "cloudUid": "ajeprm0fjnql********", "passportUid": 22******** }]
            }
        }
      ],
      "orderBy": "entityStatus"
   }
   ```

   {% cut "Response parameters" %}

   | Parameter | Description | Data type |
   -------- | -------- | ----------
   | hits | Number of search results | Number |
   | pages | Number of pages with search results | Number |
   | values | Results Entity objects | Array of objects |

   **Entity object fields**

   | Parameter | Description | Data type |
   -------- | -------- | ----------
   | self | Address of the API resource with information about the project. | String |
   | id | Entity ID. | String |
   | version | Entity version. Each change of the parameters increases the version number. | Number |
   | shortId | Project or portfolio ID | String |
   | entityType | Entity type. | String |
   | createdBy | Block with information about the user who created the entity. | Object |
   | createdAt | Entity creation date in `YYYY-MM-DDThh:mm:ss.sss±hhmm` format. | String |
   | updatedAt | Date when the entity was last updated, in `YYYY-MM-DDThh:mm:ss.sss±hhmm` format. | String |
   | fields | Object with additional fields | Object |

   `createdBy` **object fields**

   | Parameter | Description | Data type |
   -------- | -------- | ----------
   | self | Address of the API resource with information about the user who created the entity. | String |
   | id | User ID. | Number |
   | display | Displayed user name. | String |
   | cloudUid | User unique ID in {{ org-full-name }}. | String |
   | passportUid | Unique ID of the user account in the {{ ya-360 }} organization and Yandex ID. | String |

   {% endcut %}

- Request failed

   If the request is processed incorrectly, the API returns a response with an error code:

   {% include [answer-error-400](../../../_includes/tracker/api/answer-error-400.md) %}

   {% include [answer-error-401](../../../_includes/tracker/api/answer-error-401.md) %}

   {% include [answer-error-403](../../../_includes/tracker/api/answer-error-403.md) %}

   {% include [answer-error-404](../../../_includes/tracker/api/answer-error-404.md) %}

{% endlist %}
