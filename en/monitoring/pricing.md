---
title: "{{ monitoring-full-name }} pricing policy"
description: "This article describes the {{ monitoring-name }} pricing policy."
editable: false
---

# {{ monitoring-full-name }} pricing

## What is included in the {{ monitoring-short-name }} cost {#rules}

The cost of using {{ monitoring-short-name }} includes writing custom metrics via the [{{ monitoring-short-name }} API](api-ref/index.md) and writing any metrics via the [{{ prometheus-name }} Remote API](operations/prometheus/index.md) as well as reading any metrics via the [{{ monitoring-short-name }} API](api-ref/index.md).

Reading any metrics via the {{ prometheus-name }} Remote API is currently not charged.

Pricing features:
* After writing or reading the first 50 million values via the {{ monitoring-short-name }} API, the writing cost is reduced. For more information, refer to [Pricing](#prices).
* There is no charge for writing {{ yandex-cloud }} resource metrics collected automatically.
* Reading metrics via the {{ monitoring-short-name }} interface and {{ yandex-cloud }} console is not charged.
* Incoming and outgoing traffic in {{ monitoring-short-name }} is not charged.

### Example of cost calculation {#example}

The cost of using {{ monitoring-short-name }} for 30 days while writing 20 metrics at a rate of **1 value per minute** via the {{ monitoring-short-name }} API:


> 20 × 1 × (60 × 24 × 30) = 864,000 values = 0.864 million values
> 
> 
> 0.864 × $0.0784 = $0.0677376 = $0.07 
>
> Total: $0.07



Where:

* 20 is the number of metrics.
* 1 is the number of values written per minute.
* (60 × 24 × 30) is the number of minutes in 30 days.
* $0.0784 is the cost of writing 1 million values (up to 50 million values).

The cost of using {{ monitoring-short-name }} for 30 days while writing 20 metrics at a rate of **1 value per second** via the {{ monitoring-short-name }} API:


> 20 × 1 × (60 × 60 × 24 × 30) = 51,840,000 values = 51.84 million values
> 
> 
>  50 × $0.0784 + (51.84 - 50) × $0.0448 = $4.00
>
> Total: $4.00



Where:

* 20 is the number of metrics.
* 1 is the number of values written per second.
* (60 × 60 × 24 × 30) is the number of seconds in 30 days.
* $0.0784 is the cost of writing 1 million values (up to 50 million values).
* $0.0448 is the cost of writing 1 million values (over 50 million values).

The cost of exporting 100 metrics from {{ monitoring-short-name }} to your own installation of the {{ prometheus-name }} monitoring system with a polling interval of **15 seconds** for 30 days via the {{ monitoring-short-name }} API:


> 100 × (60/15) × (60 × 24 × 30) = 17,280,000 values = 17.28 million values
> 
> 
>  17.28 × $0.0560 = $0.97
>
> Total: $0.97



Where:

* 100 is the number of metrics.
* (60 / 15) is the number of values read per minute.
* (60 × 24 × 30) is the number of minutes in 30 days.
* $0.0560 is the cost of reading 1 million values (up to 50 million values).

## Pricing {#prices}

### {{ monitoring-short-name }} API





The minimum billing unit is 1 metric value. The cost is rounded to the nearest cent.

For example, the cost of writing the first 100,000 values is `(100,000 values / 1 million) × $0.0784 = $0.00784`, which is rounded to `$0.01`. The cost of writing 150,000 values is `(150,000 values / 1 million) × $0.0784 = $0.01176`, which is rounded to `$0.01`. Where `$0.0784` is the cost per 1 million values (when writing up to 50 million values).

{% include [usd.md](../_pricing/monitoring/usd.md) %}




### {{ prometheus-name }} Remote API

{% note info %}

The prices are in effect as of March 12, 2024.

{% endnote %}




{% include [usd-prometheus.md](../_pricing/monitoring/usd-prometheus.md) %}


{% include [trademark](../_includes/monitoring/trademark.md) %}
