---
title: "Pricing for {{ vision-name }}"
description: "This article describes {{ vision-short-name }} pricing."
editable: false
---

# {{ vision-name }} pricing

## What goes into the cost of using {{ vision-short-name }} {#rules}

{{ vision-name }} allows you to recognize image text.

Each successful request for image recognition performed using any [recognition model](concepts/ocr/index.md#models) is charged as a single _pricing unit_:

* If your request includes multiple images or a PDF file consisting of multiple pages, each image or page will be charged separately.
* If you send two requests to recognize text on the same image, you will be charged two billing units. This makes sense when a text is written in [languages from different language models](./concepts/ocr/supported-languages.md) (for example, Arabic and Hebrew).
* Only successful analysis attempts are chargeable. You will not be charged if the server returned an error or the request configuration was incorrect.

## Pricing {#prices}




{% include notitle [usd.md](../_pricing/vision/usd.md) %}
