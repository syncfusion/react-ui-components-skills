# Form Schema

## Schema structure

The form schema is a single JSON object passed to the component by the `schema` property. The root object typically includes:

- `version`: schema version
- `properties`: field definitions keyed by field name
- `layout`: rendering tree for fields and containers
- `settings`: form-wide options such as the form name and size

```json
{
  "version": "0.1.0",
  "properties": {
    "firstName": {
      "name": "firstName",
      "type": "string",
      "label": "First Name",
      "widget": "textbox"
    }
  },
  "layout": [
    { "type": "field", "propertyId": "firstName" }
  ],
  "settings": {
    "name": "Customer Feedback Form"
  }
}
```

## Form Builder schema property

The Form Builder receives the initial form definition through its `schema` property. The schema is the source of truth for the fields and layout that appear on the design canvas:

| Property | Type | Purpose |
| --- | --- | --- |
| `schema` | `Schema \| string \| Record<string, unknown> \| null` | Initial form definition supplied as a schema object, JSON string, or record. |

Use the `schema` property when the customer asks for an initial form. Keep field definitions in `properties`, place fields and layout containers in `layout`, and put form-wide options in `settings`. Use the Form Builder's visual designer to extend or edit the loaded schema.

## Field properties

Every field entry in `properties` shares common metadata. Use these documented properties to adapt an individual field to the requested UI:

- `name`: unique programmatic name used in `formState`, expressions, and submit payloads
- `id`: internal identifier used by conditions and dependent dropdowns
- `widget`: the field control to render; choose it to match the requested interaction
- `label`: visible and accessible field label
- `type`: data type associated with the field, such as `string`, `number`, `boolean`, or `array`
- `tooltip`: explanatory tooltip shown with the field
- `disabled`: render the field non-interactive
- `labelPosition`: `Top`, `Bottom`, `Left`, or `Right`; defaults to `Top`
- `size`: size variant such as `Small`, `Medium`, or `Bigger`
- `min` / `max`: fixed bounds for supported numeric, date, and time widgets

The docs also show `options` on dropdown fields. For changing options based on another field, use the documented `choiceBasedField` pattern. Do not assume an undocumented property is available for a widget; use only widget-specific properties shown in the source docs.

The `name` value must be unique within the form, and the `id` is used for internal mappings, especially in conditional and cascading logic.

## Complete supported widget inventory

The source schema documentation lists these 26 supported field widgets. Select a widget based on the interaction the customer wants, while keeping the schema `type` consistent with the documented widget type:

| Widget | Documented type | Choose it for |
| --- | --- | --- |
| `textbox` | `string` | Single-line text entry. |
| `textarea` | `string` | Multi-line text entry. |
| `number` | `number` | Numeric entry. |
| `inputMask` | `string` | Structured masked text such as phone numbers or IDs. |
| `rangeSlider` | `number` | Numeric input selected on a range slider. |
| `rating` | `number` | Star-rating input. |
| `checkbox` | `boolean` | A single on/off choice. |
| `switch` | `boolean` | A toggle-style on/off choice. |
| `radio` | `string` | One selection from a radio group. |
| `checkboxGroup` | `array` | Multiple selections from a checkbox group. |
| `dropdown` | `string` | One selection from a dropdown list. |
| `multiselect` | `array` | Multiple selections from a dropdown. |
| `date` | `string` | Date selection. |
| `dateTime` | `string` | Date-and-time selection. |
| `time` | `string` | Time selection. |
| `dateRange` | `string` | Date-range selection. |
| `colorPicker` | `color` | Color selection. |
| `button` | `button` | Submit, reset, or plain action. |
| `splitButton` | `string` | A primary button action with secondary dropdown actions. |
| `message` | `message` | Inline informational, success, warning, or error content; no input. |
| `signature` | `sign` | Hand-drawn signature input stored as a base64 data URL. |
| `imageEditor` | `image` | Image crop, draw, or annotation; stored as a data URL. |
| `fileUpload` | `file` | Single or multiple file upload. |
| `staticHtml` | `string` | Render HTML from `defaultValue`; it does not add a submission value. |
| `richTextEditor` | `string` | Rich-text entry. |
| `dataGrid` | `grid` | Editable grid with column definitions under the field's `columns` array. |

This is a closed list. An unlisted `widget` is treated as a plain `textbox`; do not invent or substitute widget names. For remote options on dropdown, multiselect, and checkbox-group fields, see [Remote Data Binding](remote-data-binding-in-components.md). For grid data sources, column definitions, query operations, and conditional data, see [Data Grid Features](data-grid-features.md).

## Match UI preferences to widgets

When a customer describes how a field should behave, map that preference to the closest documented widget rather than choosing from the data type alone:

- free-form one-line or multi-line text: `textbox` or `textarea`
- masked text: `inputMask`
- numeric entry, slider, or star score: `number`, `rangeSlider`, or `rating`
- one boolean choice: `checkbox` or `switch`, matching the requested visual interaction
- one choice or multiple choices: `radio` / `dropdown` or `checkboxGroup` / `multiselect`
- date, time, or date interval: `date`, `dateTime`, `time`, or `dateRange`
- color, signature, image editing, or file selection: `colorPicker`, `signature`, `imageEditor`, or `fileUpload`
- non-input content or actions: `message`, `staticHtml`, `button`, or `splitButton`
- rich text or editable tabular data: `richTextEditor` or `dataGrid`

For each requested field, configure its `widget`, matching `type`, stable unique `name` and `id`, user-facing `label`, and any applicable documented shared properties. Add only widget-specific keys that are present in the source docs or a supplied schema example.

## Layout containers

The `layout` array is where you decide how fields are arranged. Common container types include:

- `panel`: titled grouping panel
- `table`: rows and columns of fields
- `tabs`: tabbed sections
- `card`: boxed content area

Each container is a layout node. Field definitions stay in `properties`; container children refer to them using `propertyId`. Use these exported layouts as patterns and replace the property IDs, labels, and content with the fields in the form.

### Panel

```json
{
  "layout": [
    {
      "type": "panel",
      "label": "Personal Information",
      "children": [
        { "type": "field", "propertyId": "firstName" },
        { "type": "field", "propertyId": "country" }
      ]
    }
  ]
}
```

### Table

A table declares its dimensions with `rows` and `cols`. Its `cells` value is a two-dimensional array; each cell has `row`, `col`, and `children`, and children use ordinary field layout nodes.

```json
{
  "type": "table",
  "id": "table_1",
  "name": "table_1",
  "label": "Contact Details",
  "rows": 2,
  "cols": 2,
  "cells": [
    [
      {
        "row": 0,
        "col": 0,
        "children": [{ "type": "field", "propertyId": "firstName" }]
      },
      {
        "row": 0,
        "col": 1,
        "children": [{ "type": "field", "propertyId": "lastName" }]
      }
    ],
    [
      {
        "row": 1,
        "col": 0,
        "children": [{ "type": "field", "propertyId": "email" }]
      },
      {
        "row": 1,
        "col": 1,
        "children": [{ "type": "field", "propertyId": "phone" }]
      }
    ]
  ]
}
```

The example assumes `firstName`, `lastName`, `email`, and `phone` are keys in `properties`. Place the table node in the root `layout` array; other nodes such as a submit field can follow it.

### Tabs

Use `tabOptions` for the tab labels and `tabs` for the tab contents. Each tab entry has a `header` and a `children` array of layout nodes.

```json
{
  "type": "tabs",
  "id": "tabs_1",
  "name": "detailsTabs",
  "label": "Details",
  "tabOptions": ["Contact", "Notes"],
  "tabs": [
    {
      "header": "Contact",
      "children": [{ "type": "field", "propertyId": "email" }]
    },
    {
      "header": "Notes",
      "children": [{ "type": "field", "propertyId": "notes" }]
    }
  ]
}
```

Define `email` and `notes` in `properties`. Keep the `tabOptions` labels aligned with the corresponding tab headers.

### Card

A card groups fields under a label and can include `cardSubtitle`. Its `children` array contains the fields to render inside the card.

```json
{
  "type": "card",
  "id": "card_1",
  "name": "profileCard",
  "label": "Profile",
  "cardSubtitle": "Contact information",
  "children": [
    { "type": "field", "propertyId": "firstName" },
    { "type": "field", "propertyId": "email" }
  ]
}
```

Define `firstName` and `email` in `properties`. As with the other containers, place the card node in the root `layout` array.

Layout containers belong in `layout`, not in `properties`. Their child fields must still be defined in `properties` and referenced by layout nodes.

## Validation and logic are also schema-driven

The `properties` object is where validation rules, conditions, expression values, and custom settings are declared. This keeps the entire form declarative and easier to maintain.

## Key guidance

Use a consistent `id` and `name` pattern across the schema, and keep layout items aligned to the corresponding property keys. This makes expressions, conditions, and dependent logic significantly easier to debug.
