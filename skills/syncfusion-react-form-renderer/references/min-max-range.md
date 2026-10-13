# Min/Max Range Based on Another Field

## Table of Contents
- [Overview](#overview)
- [`minMaxRange`](#minmaxrange)
- [Rule properties](#rule-properties)
- [Bounds by widget type](#bounds-by-widget-type)
- [Worked examples](#worked-examples)
- [Fixed bounds for common widgets](#fixed-bounds-for-common-widgets)
- [Key guidance](#key-guidance)

## Overview

Some forms need range limits that move with another field. Examples include limiting a purchased quantity by a total, requiring a departure date to be on or after an arrival date, or setting a meeting time from a permitted time field. `minMaxRange` re-applies bounds when the source field changes.

## `minMaxRange`

The Form Renderer supports this through `minMaxRange` on a field. The property accepts an array of rules, each of which can set `minValue` or `maxValue` to a constant or to another field reference. The rule remains in sync when the source field value changes.

```json
{
  "properties": {
    "totalQuantity": {
      "name": "totalQuantity",
      "type": "number",
      "label": "Total Quantity",
      "widget": "number"
    },
    "purchasedQuantity": {
      "name": "purchasedQuantity",
      "type": "number",
      "label": "Purchased Quantity",
      "widget": "number",
      "minMaxRange": [
        {
          "enabled": true,
          "maxValue": {
            "field": "number_1"
          }
        }
      ]
    }
  }
}
```

## Rule properties

Each `minMaxRange` item includes:

- `enabled`: whether the rule is active. Set it to `false` to keep the rule in the schema without applying it.
- `minValue`: optional lower bound, either a constant in the widget's value type or a field reference.
- `maxValue`: optional upper bound, either a constant in the widget's value type or a field reference.

At least one of `minValue` or `maxValue` must be present. A field reference is an object whose `field` points to the source field's `id`, not its name or label. Provide bounds in the value type appropriate to the target widget: numeric values for numeric fields and the corresponding date, date-time, or time value for temporal fields.

## Bounds by widget type

| Widget | Bound value interpretation | Behavior |
| --- | --- | --- |
| `number` | Numeric value | Constrains the numeric field. In the documented example, entering a value above the dynamic maximum resets it to that maximum. |
| `date` | Date value | Constrains the date picker against a date bound. |
| `dateTime` | Date-time value | Constrains the date-time picker against a date-time bound. |
| `time` | Time value | Constrains the time picker against a time bound. |
| `dateRange` | Date value | Bounds apply to both the start and end of the selected range. |
| `rangeSlider` | Numeric value | Bounds shape the slider track. |

Use `minValue` to establish a lower bound, `maxValue` to establish an upper bound, or both in the same rule. Use static field-level `min` / `max` for fixed bounds; use `minMaxRange` when the limit should follow another field.

## Worked examples

In each example, the target references the source field by `id`. The source and target widgets use matching value types.

### Number

This caps `purchasedQuantity` at `totalQuantity` and sets a dynamic lower bound from `minimumQuantity`:

```json
{
  "minimumQuantity": {
    "id": "number_minimum",
    "name": "minimumQuantity",
    "type": "number",
    "label": "Minimum Quantity",
    "widget": "number"
  },
  "totalQuantity": {
    "id": "number_total",
    "name": "totalQuantity",
    "type": "number",
    "label": "Total Quantity",
    "widget": "number"
  },
  "purchasedQuantity": {
    "id": "number_purchased",
    "name": "purchasedQuantity",
    "type": "number",
    "label": "Purchased Quantity",
    "widget": "number",
    "minMaxRange": [
      {
        "enabled": true,
        "minValue": { "field": "number_minimum" },
        "maxValue": { "field": "number_total" }
      }
    ]
  }
}
```

### Date

Bind the earliest allowed departure date to the arrival-date field:

```json
{
  "arrivalDate": {
    "id": "date_arrival",
    "name": "arrivalDate",
    "type": "string",
    "label": "Arrival Date",
    "widget": "date"
  },
  "departureDate": {
    "id": "date_departure",
    "name": "departureDate",
    "type": "string",
    "label": "Departure Date",
    "widget": "date",
    "minMaxRange": [
      {
        "enabled": true,
        "minValue": { "field": "date_arrival" }
      }
    ]
  }
}
```

### Date-time

Bind the earliest meeting date-time to another date-time field. A `maxValue` can be added to the same rule when a latest date-time source is also present.

```json
{
  "earliestDateTime": {
    "id": "datetime_earliest",
    "name": "earliestDateTime",
    "type": "string",
    "label": "Earliest Date and Time",
    "widget": "dateTime"
  },
  "meetingDateTime": {
    "id": "datetime_meeting",
    "name": "meetingDateTime",
    "type": "string",
    "label": "Meeting Date and Time",
    "widget": "dateTime",
    "minMaxRange": [
      {
        "enabled": true,
        "minValue": { "field": "datetime_earliest" }
      }
    ]
  }
}
```

### Time

Bind the earliest allowed meeting time to a source time field:

```json
{
  "earliestTime": {
    "id": "time_earliest",
    "name": "earliestTime",
    "type": "string",
    "label": "Earliest Time",
    "widget": "time"
  },
  "meetingTime": {
    "id": "time_meeting",
    "name": "meetingTime",
    "type": "string",
    "label": "Meeting Time",
    "widget": "time",
    "minMaxRange": [
      {
        "enabled": true,
        "minValue": { "field": "time_earliest" }
      }
    ]
  }
}
```

## Fixed bounds for common widgets

Use `minMaxRange` for fixed or dynamically referenced bounds on `number`, `date`, `time`, and `dateTime` fields. Temporal fields use `type: "string"` and a widget-matching `format`: `date`, `time`, or `date-time`. Set `enabled` to `true` to apply the rule; `false` leaves it declared but inactive.

For `dateTime`, use `minMaxRange` for the date/date-time bounds and the separate `minMaxTimeRange` array for the time-of-day bounds. Keep lower values less than or equal to upper values.

```json
{
  "version": "0.1.0",
  "properties": {
    "number": {
      "id": "number_1",
      "name": "numberValue",
      "type": "number",
      "label": "Number",
      "widget": "number",
      "labelPosition": "top",
      "minMaxRange": [
        { "enabled": true, "minValue": 2, "maxValue": 56 }
      ]
    },
    "date": {
      "id": "date_1",
      "name": "dateValue",
      "type": "string",
      "format": "date",
      "label": "Date",
      "widget": "date",
      "labelPosition": "top",
      "minMaxRange": [
        { "enabled": true, "minValue": "10/19/2026", "maxValue": "10/31/2026" }
      ]
    },
    "time": {
      "id": "time_1",
      "name": "timeValue",
      "type": "string",
      "format": "time",
      "label": "Time",
      "widget": "time",
      "labelPosition": "top",
      "minMaxRange": [
        { "enabled": true, "minValue": "01:00 AM", "maxValue": "03:00 AM" }
      ]
    },
    "dateTime": {
      "id": "dateTime_1",
      "name": "dateTimeValue",
      "type": "string",
      "format": "date-time",
      "label": "Date / Time",
      "widget": "dateTime",
      "minMaxRange": [
        { "enabled": true, "minValue": "10/21/2026", "maxValue": "10/27/2026" }
      ],
      "minMaxTimeRange": [
        { "enabled": true, "minValue": "03:00 AM", "maxValue": "08:00 AM" }
      ]
    }
  }
}
```

In this example, the number, date, and time fields each use `minMaxRange`. The date-time field applies date bounds and time-of-day bounds separately.

## Supported components

This behavior works with numeric and date-like widgets such as:

- Numeric TextBox
- Date Picker
- DateTime Picker
- Date Range Picker
- Time Picker
- Range Slider

## Key guidance

Use `minMaxRange` when a bound comes from another field and static `min` / `max` when the bound is fixed. Match the source widget's value type to the target widget, reference the source by `id`, and include at least one bound in every rule.
