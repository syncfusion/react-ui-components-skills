# Filtering

## Table of Contents
- [Overview](#overview)
- [Basic Filtering](#basic-filtering)
- [Filter Types](#filter-types)
- [Case-Sensitive Filtering](#case-sensitive-filtering)
- [Remote Filtering with Minimum Characters](#remote-filtering-with-minimum-characters)
- [Diacritics Filtering](#diacritics-filtering)
- [Debounce Delay](#debounce-delay)
- [Virtual Scroll + Filtering](#virtual-scroll--filtering)
- [Gotchas](#gotchas)

---

## Overview

Enable `allowFiltering` to show a search box in the popup. As the user types, the `filtering` event fires — handle it to filter your data and call `args.updateData()` to return results.

---

## Basic Filtering

```tsx
import { DropDownListComponent, FilteringEventArgs } from '@syncfusion/ej2-react-dropdowns';
import { Query } from '@syncfusion/ej2-data';

export default function App() {
  const searchData = [
    { Index: 's1', Country: 'Alaska' },
    { Index: 's2', Country: 'California' },
    { Index: 's3', Country: 'Florida' },
    { Index: 's4', Country: 'Georgia' },
  ];
  const fields = { text: 'Country', value: 'Index' };

  function onFiltering(args: FilteringEventArgs) {
    let query = new Query();
    // 'startswith' filter — pass true for case-insensitive (4th param)
    query = args.text !== ''
      ? query.where('Country', 'startswith', args.text, true)
      : query;
    args.updateData(searchData, query);
  }

  return (
    <DropDownListComponent
      dataSource={searchData}
      fields={fields}
      allowFiltering={true}
      filtering={onFiltering}
      popupHeight="250px"
      placeholder="Select a country"
    />
  );
}
```

---

## Filter Types

Choose the filter operator in the `query.where()` call:

| Operator | Behavior |
|---|---|
| `'startswith'` | Matches items that begin with the typed text |
| `'contains'` | Matches items that contain the typed text anywhere |
| `'endsWith'` | Matches items that end with the typed text |

```tsx
// Contains filter
function onFiltering(args: FilteringEventArgs) {
  let query = new Query();
  query = args.text !== ''
    ? query.where('Country', 'contains', args.text, true)  // case-insensitive
    : query;
  args.updateData(searchData, query);
}

// EndsWith filter
function onFiltering(args: FilteringEventArgs) {
  let query = new Query();
  query = args.text !== ''
    ? query.where('Country', 'endsWith', args.text, true)
    : query;
  args.updateData(searchData, query);
}
```

---

## Case-Sensitive Filtering

The 4th parameter of `query.where()` controls case sensitivity:
- `true` → case-insensitive (default recommendation)
- `false` → case-sensitive (user must type exact casing)

```tsx
function onFiltering(args: FilteringEventArgs) {
  let query = new Query();
  query = args.text !== ''
    ? query.where('Country', 'contains', args.text, false)  // case-SENSITIVE
    : query;
  args.updateData(searchData, query);
}
```

---

## Remote Filtering with Minimum Characters

For remote data sources, avoid firing a server request on every keystroke. Return early until the user has typed at least N characters:

```tsx
import { DataManager, ODataV4Adaptor, Query } from '@syncfusion/ej2-data';
import { DropDownListComponent, FilteringEventArgs } from '@syncfusion/ej2-react-dropdowns';

export default function App() {
  const searchData = new DataManager({
    adaptor: new ODataV4Adaptor,
    crossDomain: true,
    url: 'https://services.odata.org/V4/Northwind/Northwind.svc/Customers',
  });
  const fields = { text: 'ContactName', value: 'CustomerID' };
  const query = new Query().select(['ContactName', 'CustomerID']).take(7);

  function onFiltering(e: FilteringEventArgs) {
    if (e.text === '') {
      // Show full list when search is cleared
      e.updateData(searchData);
    } else {
      // Only filter after 3 characters to limit requests
      if (e.text.length < 3) return;
      let q = new Query().select(['ContactName', 'CustomerID']);
      q = q.where('ContactName', 'startswith', e.text, true);
      e.updateData(searchData, q);
    }
  }

  return (
    <DropDownListComponent
      dataSource={searchData}
      query={query}
      fields={fields}
      allowFiltering={true}
      filtering={onFiltering}
      sortOrder="Ascending"
      popupHeight="250px"
      placeholder="Select a customer"
    />
  );
}
```

---

## Diacritics Filtering

Enable `ignoreAccent` to ignore accent characters (é, ñ, ü, etc.) when filtering international data:

```tsx
const diacriticsData = [
  'Aeróbics',
  'Aeróbics en Agua',
  'Aerografía',
  'Aeromodelaje',
  'Águilas',
  'Ajedrez',
];

<DropDownListComponent
  dataSource={diacriticsData}
  allowFiltering={true}
  ignoreAccent={true}
  filterBarPlaceholder="e.g: aero"
  placeholder="Select a value"
/>
```

With `ignoreAccent={true}`, typing "aero" matches "Aeróbics", "Aerografía", etc.

---

## Debounce Delay

Control the delay (in ms) before filtering fires after each keystroke. Default is 300ms. Set to `0` to disable debouncing:

```tsx
<DropDownListComponent
  dataSource={sportsData}
  allowFiltering={true}
  debounceDelay={300}
  placeholder="Select a game"
/>
```

Increase `debounceDelay` for remote sources to reduce server load.

---

## Virtual Scroll + Filtering

When `enableVirtualization` is active, `allowFiltering` still works. The component filters the virtualized data on the client side (for local data) or sends filtered requests to the server (for remote data):

```tsx
import { DropDownListComponent, Inject, VirtualScroll } from '@syncfusion/ej2-react-dropdowns';

<DropDownListComponent
  dataSource={records}
  fields={fields}
  enableVirtualization={true}
  allowFiltering={true}
  popupHeight="200px"
  placeholder="e.g. Item 1"
>
  <Inject services={[VirtualScroll]} />
</DropDownListComponent>
```

---

## Gotchas

- **Always call `args.updateData()`** — even when `args.text === ''` (pass original data to reset). Failing to call it leaves the list empty.
- **`filtering` event vs `allowFiltering` prop** — you need both: `allowFiltering={true}` shows the search box; the `filtering` event handles the actual filtering logic.
- **Class components** — must `.bind(this)` in the filtering prop: `filtering={this.onFiltering.bind(this)}`.
- **Remote data minimum characters** — without a `minLength` guard, every keystroke fires a server request, which can be very slow.
- **`debounceDelay={0}`** disables the built-in delay entirely; only do this for local data.

## See Also

- [Data Binding](data-binding.md) — configure the data source
- [How-To: Highlight Filtering](how-to.md#highlight-filtered-text) — highlight matched characters
- [How-To: Limit Search Results](how-to.md#limit-search-results) — cap the number of results returned
