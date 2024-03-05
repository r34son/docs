#### Limits {#yq-limits}

| Type of limit | Value |
--- | ---
| Maximum query result retention time | 24 hours |
| Maximum streaming query runtime (preview stage) | 7 days |
| Maximum analytical query duration | 24 hours |
| Maximum number of rows in a table of a managed database | 1,000,000 |
| Maximum query execution results size | 20 MB |

#### Quotas {#yq-quotas}

| Type of limit | Value |
--- | ---
| Number of concurrent analytical queries | 100 |
| Number of concurrent streaming queries | 100 |
| Maximum analytical query duration | 30 minutes |

{% note warning %}

The maximum streaming query runtime (preview stage) is 7 days. Once expired, the query is forced to stop. To run streaming queries for an unlimited time, contact {{ yandex-cloud }} support.

{% endnote %}
