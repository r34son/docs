---
title: "{{ datalens-full-name }} release notes for January 2024"
description: "Check out {{ datalens-full-name }} release notes for January 2024."
---

# {{ datalens-full-name }} release notes: January 2024

* [Updates](#top-news)
* [Fixes and improvements](#fixes)

## Updates {#top-news}

### Required selector value {#required-selector}

Enabled setting a [selector](../dashboard/selector.md) as a required parameter. To do this, enable the **Required field** option when [creating](../operations/dashboard/add-selector.md) or editing a selector.

### Gradient fill type for numeric measures {#dimension-gradient}

Enabled **Gradient** as the fill type for the `Whole number` and `Decimal number` measures located in the **Colors** chart section. It is supported by all chart types except for the ones below:

* [{#T}](../visualization-ref/line-chart.md)
* [{#T}](../visualization-ref/area-chart.md)
* [{#T}](../visualization-ref/normalized-area-chart.md)


### Tags in Favorites {#labels}

You can now add tags to objects marked as favorites and use them to search for specific items. A tag replaces the object name and is only visible to you. To add a tag click the ![tag](../../_assets/console-icons/tag.svg) icon that appears as you hover over the object.

## Fixes and improvements {#fixes}


### Editing parameters in the wizard {#wizard-params-edit}

You can now edit a parameter from the wizard interface by clicking ![ellipsis](../../_assets/console-icons/ellipsis.svg) → **Edit** next to the parameter name.

### Adding fields to QL chart sections {#ql-add-fields}

You can now add new fields not only by dragging them to the [QL chart](../concepts/chart/ql-charts.md) section, but also using the ![plus](../../_assets/console-icons/plus.svg) icon.

### Unused QL chart fields {#unused-fields}

Enabled warnings for QL chart fields that cleared from the query after editing.

### Editing a selector {#selector-edit}


In the selector editing window, added the **Open** button for a selected dataset.


### Dashboard path {#dashboard-save}

You will now be prompted to set the path when saving your new dashboard, rather than after creating one.

