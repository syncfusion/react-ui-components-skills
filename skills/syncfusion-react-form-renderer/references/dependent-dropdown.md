# Dependent Dropdowns

## Overview

A dependent dropdown is a cascading selection where the options in one dropdown are filtered by the value in another field. Common examples include Country → State and Category → Product.

## `choiceBasedField`

The Form Renderer configures dependent dropdowns through `choiceBasedField` in a field's `conditions` object.

```json
{
  "properties": {
    "continent": {
      "name": "continent",
      "type": "string",
      "label": "Continent",
      "widget": "dropdown",
      "options": ["North America", "Asia", "Africa"]
    },
    "country": {
      "name": "country",
      "type": "string",
      "label": "Country",
      "widget": "dropdown",
      "options": ["USA", "Canada", "India", "Japan"],
      "conditions": {
        "choiceBasedField": {
          "primaryFieldId": "dropdown_1",
          "choiceMapping": {
            "North America": ["USA", "Canada"],
            "Asia": ["India", "Japan"]
          },
          "showAllWhenNotMapped": false
        }
      }
    }
  }
}
```

## Members

The `choiceBasedField` object supports:

- `primaryFieldId`: the parent field identifier that drives filtering
- `choiceMapping`: a map of parent value to child options
- `showAllWhenNotMapped`: whether to show all options when no mapping is found

## Cascading chains

You can chain dependent dropdowns across more than two levels. Each field in the chain can define its own `choiceBasedField`, with `primaryFieldId` pointing to the previous field.

## Key guidance

Link dependent dropdowns by `id`, not by `name`, to avoid breaking the mapping when the internal field name changes. This keeps the cascade stable while the schema remains easy to manage.
