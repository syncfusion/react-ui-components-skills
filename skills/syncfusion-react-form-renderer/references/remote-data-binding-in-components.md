# Remote Data Binding in Components

## Table of Contents
- [Supported fields](#supported-fields)
- [Remote options schema](#remote-options-schema)
- [Adaptors and response mapping](#adaptors-and-response-mapping)
- [Search, sort, filters, and paging](#search-sort-filters-and-paging)
- [Use another field as a query value](#use-another-field-as-a-query-value)
- [Key guidance](#key-guidance)

## Supported fields

Remote option binding is supported for these form widgets:

- `dropdown`
- `multiselect`
- `checkboxGroup`

For these widgets, `options` can be a local array or a remote source object. Keep `type` aligned with the widget: the supplied remote dropdown example uses `string`; multiselect and checkbox-group fields represent multiple selected values.

## Remote options schema

Set the field's `options` to a URL source object. The example uses the `WebApiAdaptor` and extracts choices from a nested response with `dataPath`:

```json
{
  "dropdownList": {
    "id": "dropdown_1",
    "name": "dropdownList",
    "type": "string",
    "label": "Dropdown List",
    "options": {
      "type": "url",
      "url": "https://mp9d2842e717a36a4c75.free.beeceptor.com/",
      "headers": [],
      "adaptorType": "WebApiAdaptor",
      "dataPath": "response.data[0].records"
    },
    "widget": "dropdown"
  }
}
```

Use the field's normal `properties` entry and place its key in `layout` as a `field` node. Replace the sample endpoint and response path with the service endpoint and response shape used by the application.

## Adaptors and response mapping

The remote-source configuration shown here uses:

- `type: "url"` to declare a remote URL source
- `url` for the endpoint
- `headers` for request headers
- `adaptorType` to select the request/response adaptor
- `dataPath` to locate records inside a nested response

The supplied UI lists **Web API Adaptor**, **URL Adaptor**, and **OData V4 Adaptor**. Example schema identifiers provided are `WebApiAdaptor` and `UrlAdaptor`; use the serialized value produced by the Form Builder for OData V4 rather than guessing its casing.

## Search, sort, filters, and paging

The remote query configuration supports search, sorting, filters, and paging. Search specifies whether it is enabled, which response fields to search, and the key to send. Sorting specifies whether it is enabled, the field, and direction. Each filter specifies a field, operator, and value.

```json
{
  "type": "url",
  "url": "https://example.com/api/options",
  "headers": [],
  "adaptorType": "WebApiAdaptor",
  "search": {
    "enabled": true,
    "fields": ["EmployeeName"],
    "key": "{searchTerm}"
  },
  "sort": {
    "enabled": true,
    "field": "EmployeeName",
    "direction": "descending"
  },
  "filters": [
    {
      "field": "Designation",
      "operator": "equal",
      "value": "{selectedDesignation}"
    }
  ],
  "skip": 0,
  "take": 50
}
```

The query UI labels paging inputs as **Offset** and **Limit**; the schema examples use `skip` and `take` for those values.

## Use another field as a query value

Use `{fieldName}` in the search key or filter value to reference another form field at runtime. `fieldName` is the referenced field's schema `name`, not its label or `id`.

```json
{
  "filters": [
    {
      "field": "Designation",
      "operator": "equal",
      "value": "{chooseDesignation}"
    }
  ]
}
```

This lets the remote request respond to a value selected elsewhere in the form.

## Key guidance

- Use `dataPath` when the endpoint's records are nested rather than at the response root.
- Configure search `fields` to identify the remote fields searched by the request.
- Use `{fieldName}` references for values sourced from other form fields.
- Match `skip` / `take` values to the service's paging behavior and avoid assuming endpoint-specific response shapes.