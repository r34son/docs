# {{ at-full-name }} audit log reference

{{ at-name }} supports tracking of [management (control plane) events](../audit-trails/concepts/format.md) and [data (data plane) events](../audit-trails/concepts/format-data-plane.md) for {{ mmy-full-name }}.

The general view of the `event_type` field value is as follows:

```text
{{ at-event-prefix }}.audit.mdb.mysql.<event_name>
```

## Management event reference {#control-plane-events}

{% include [mysql-events](../_includes/audit-trails/events/mysql-events.md) %}

## Data event reference {#data-plane-events}

{% include notitle [preview](../_includes/note-preview-by-request.md) %}

{% include [mmy-events-dp](../_includes/audit-trails/events/mmy-events-dp.md) %}