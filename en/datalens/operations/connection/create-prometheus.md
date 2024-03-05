---
title: "How to create a {{ prometheus-name }} connection in {{ datalens-full-name }}"
description: "Follow this guide to create a {{ prometheus-name }} connection."
---

# Creating a {{ prometheus-name }} connection

To create a {{ prometheus-name }} connection:


{% include [datalens-workbooks-collections-note](../../../_includes/datalens/operations/datalens-workbooks-collections-note.md) %}


1. Go to the [connections page]({{ link-datalens-main }}/connections).
1. Click **Create connection**.
1. Select the **{{ prometheus-name }}** source.
1. Specify the connection parameters:

   * **Hostname**: Specify the path to the {{ prometheus-name }} host. You can specify multiple hosts in a comma-separated list. If you are unable to connect to the first host, {{ datalens-short-name }} will select the next one from the list.
   * **Port**: Specify the {{ prometheus-name }} connection port.
   * **URL**: Specify the URL of the {{ prometheus-name }} API.
   * **Username**: Specify the username for the {{ prometheus-name }} connection.
   * **Password**: Enter the password for the user.
   * **Cache TTL in seconds**: Specify the cache time-to-live or leave the default value. The recommended value is 300 seconds (5 minutes).
   * **HTTPS**: Enable the secure connection option if your server supports HTTPS.

1. Click **Create connection**.
1. Enter the connection name and click **Create**.

{% include [datalens-check-host](../../../_includes/datalens/operations/datalens-check-host.md) %}
