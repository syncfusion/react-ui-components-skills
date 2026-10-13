# Data Grid Features

## Table of Contents
- [Data sources](#data-sources)
- [Grid columns](#grid-columns)
- [Column expressions and validation](#column-expressions-and-validation)
- [Conditional data](#conditional-data)
- [Remote query operations](#remote-query-operations)
- [Key guidance](#key-guidance)

## Data sources

Define a grid as a `dataGrid` field with `type: "grid"`. Its `dataSource` can contain local static records or a remote URL source. A remote source can include `type`, `url`, `headers`, and `adaptorType`; the supplied example uses `UrlAdaptor`.

```json
{
  "employeeRecord": {
    "id": "dataGrid_1",
    "name": "employeeRecord",
    "type": "grid",
    "label": "Employee Record",
    "widget": "dataGrid",
    "dataSource": [
      { "EmployeeID": "1", "Employees": "Ada", "Designation": "Developer" }
    ],
    "columns": [
      {
        "field": "EmployeeID",
        "headerText": "Employee ID",
        "type": "string",
        "width": "120",
        "editType": "textbox",
        "validationRules": {}
      }
    ],
    "pageSize": 8
  }
}
```

Place the grid field key in the form `layout` like any other field. For a remote source, use a URL configuration object instead of the static array.

## Grid columns

Each `columns` entry describes a grid column. The supplied schemas use `field`, `headerText`, `type`, `width`, `editType`, and `validationRules`. They also demonstrate `allowEditing` to control editability and `isPrimaryKey` to mark a key column.

```json
{
  "field": "price",
  "headerText": "Price",
  "type": "number",
  "width": "120",
  "editType": "numericedit",
  "validationRules": { "required": true },
  "allowEditing": true
}
```

Use column configuration for the grid's editing behavior; form-level validation rules are not a substitute for `validationRules` on a grid column.

## Column expressions and validation

A column may define `expression` to calculate a cell from other column fields. A column may also define `customValidation` expressions that use `input` and `{fieldName}` references.

```json
{
  "columns": [
    {
      "field": "deliveryQuantity",
      "headerText": "Delivery Quantity",
      "type": "number",
      "editType": "numericedit",
      "allowEditing": true,
      "customValidation": [
        {
          "expression": "valid = (input < {stockQuantity}) ? true : 'Delivery should be less than stock'"
        }
      ]
    },
    {
      "field": "price",
      "headerText": "Price",
      "type": "number",
      "editType": "numericedit",
      "allowEditing": true
    },
    {
      "field": "total",
      "headerText": "Total Cost",
      "type": "number",
      "editType": "numericedit",
      "expression": "{deliveryQuantity} * {price} ? {deliveryQuantity} * {price} : undefined"
    }
  ]
}
```

Keep expression and validation field references aligned with the grid column `field` values.

## Conditional data

Use `conditions.conditionalData` when a grid should use different data sources based on form values. Each entry pairs a condition with the data source to use when it matches. Conditions reference the controlling form field by its `id`; filters in the data source select matching grid rows.

```json
{
  "conditions": {
    "conditionalData": [
      {
        "condition": {
          "condition": "and",
          "rules": [
            {
              "label": "Choose Designation",
              "field": "dropdown_designation",
              "operator": "equal",
              "type": "string",
              "value": "Developer"
            }
          ]
        },
        "dataSource": {
          "type": "url",
          "url": "https://services.syncfusion.com/react/production/api/UrlDataSource",
          "headers": [],
          "adaptorType": "UrlAdaptor",
          "filters": [
            { "field": "Designation", "operator": "equal", "value": "Developer" }
          ]
        }
      }
    ]
  }
}
```

Add one conditional-data entry for each condition/data-source combination needed. A grid data source can be static or remote; use the appropriate source shape in each entry.

## Remote query operations

The supplied grid configuration supports remote `search`, `sort`, `filters`, `skip`, and `take`. Search includes `enabled`, `fields`, and `key`; sort includes `enabled`, `field`, and `direction`. Filter `value` may reference another form field by `{fieldName}`.

```json
{
  "dataSource": {
    "type": "url",
    "url": "https://services.syncfusion.com/react/production/api/UrlDataSource",
    "headers": [],
    "adaptorType": "UrlAdaptor",
    "search": { "enabled": true, "fields": [], "key": "" },
    "filters": [
      { "field": "Designation", "operator": "equal", "value": "{combobox}" }
    ],
    "take": 500
  }
}
```

The query configuration UI describes `skip` as **Offset** and `take` as **Limit**. Set values appropriate to the remote service; the sample's `take: 500` is not a universal page size.

## Key guidance

- Keep column `field` names aligned with the data-source records.
- Use `conditionalData` for condition-driven source changes; use remote `filters` for filtering rows at the endpoint.
- Use field `id` in condition rules and `{name}` in expression or query-value references.
- Confirm the endpoint response and adaptor expectations before relying on a particular remote response format.