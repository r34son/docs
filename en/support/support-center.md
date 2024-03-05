---
title: "{{ yandex-cloud }} {{ support-center-name }}"
description: "This guide will help you get started with the {{ yandex-cloud }} {{ support-center-name }}: find troubleshooting recommendations, create or view support tickets, or change your service plan."
---

# Working with {{ support-center-name }}

{{ support-center-name }} will help you troubleshoot {{ yandex-cloud }} issues and create or view support tickets. You can also change your service plan in {{ support-center-name }}.

## Getting started {#before-you-begin}

1. Go to the [management console]({{ link-console-main }}) and log in to {{ yandex-cloud }} or create an account if you do not have one yet.
1. Open the {{ support-center-name }} [home page]({{ link-console-support }}).
1. Select the [organization](../organization/quickstart.md) to work with {{ support-center-name }} in or [create a new one](../organization/operations/enable-org).

If you are using the support center from your own organization, open the [**{{ ui-key.yacloud.billing.label_service }}**]({{ link-console-billing }}) page and make sure you have a [billing account](../billing/concepts/billing-account.md) linked and it has the `ACTIVE` or `TRIAL_ACTIVE` status.

## Troubleshooting {#search-for-solution}

To find answers to questions about {{ yandex-cloud }}, do the following in {{ support-center-name }}:

1. Open the {{ support-center-name }} [home page]({{ link-console-support }}).
1. Go to **{{ ui-key.support-center.common.tickets }}**.
1. Check out the answers to FAQs using quick search buttons.
1. If you did not find your question in the FAQ list:
   1. In the **{{ ui-key.support-center.search.common.value_search-input-placeholder }}** window, provide a brief description of your issue, e.g., `How to restore access to a billing account`. {{ yandex-cloud }} {{ support-center-name }} will take your query to the technical support knowledge base and return all relevant articles.
   1. If no suitable article is found, a notification window will appear under the search bar. In this window, click **{{ ui-key.support-center.search.common.action_search-in-documentation }}** to find the answer to your question in the {{ yandex-cloud }} documentation.

## Creating a request {#create-request}

If a [search for a way to resolve your issue](#finding-solution) in the support knowledge base and the {{ yandex-cloud }} documentation produced no results, create a request for support:

1. Open the {{ support-center-name }} [home page]({{ link-console-support }}).
1. Go to **{{ ui-key.support-center.common.tickets }}**.
1. Click **{{ ui-key.support-center.tickets.common.action_create-ticket }}**.
1. Fill out the following fields in the **{{ ui-key.support-center.ticket.create.title_create-ticket-page }}** form that opens:
   * **{{ ui-key.yacloud.support.ticket.create.field_type }}**: Select the request type that describes your issue best.
   * **{{ ui-key.yacloud.support.ticket.create.field_service }}**: Specify the service you have an issue with.
   * **{{ ui-key.yacloud.support.ticket.create.field_summary }}**: Enter the subject of your request.
   * **{{ ui-key.yacloud.support.ticket.create.field_description }}**: Provide a detailed description of the issue. For best results, specify the resource ID and the event date and time.
   * **{{ ui-key.yacloud.support.ticket.create.field_attachments }}**: Optionally attach screenshots or other evidence that may help describe your request in better detail.
   * **{{ ui-key.yacloud.support.ticket.create.field_access-type }}**: Select who will be able to see your request.
1. Click **{{ ui-key.support-center.ticket.create.action_create-ticket }}**.

## Viewing requests {#view-requests}

You can view all submitted requests you have access to on the {{ support-center-name }} [home page]({{ link-console-support }}) under **{{ ui-key.support-center.tickets.list.title_ticket_table }}**.

To quickly find the request you need, use filters and sorting by:
* Request author
* Request status
* Service specified in the request
* Response status
* Request type

To reset the request filters, click **{{ ui-key.support-center.tickets.filters.action_reset-filters }}**.

## Changing your service plan {#change-pricing}

{% include [change-tariff](../_includes/support/change-pricing.md) %}

## Changing a billing account for the current service plan {#change-ba}

{% include [change-ba](../_includes/support/change-ba.md) %}