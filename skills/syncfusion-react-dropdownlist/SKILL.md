---
name: syncfusion-react-dropdownlist
description: Implement the Syncfusion React DropDownList component for single-value selection from a predefined list. Use this when adding a dropdown, select box, or searchable list using Syncfusion React. This skill covers data binding (local, remote/OData), filtering, grouping, templates, and cascading dropdowns.
metadata:
  author: "Syncfusion Inc"
  category: "Dropdowns"
  version: "33.1.44"
---

# Syncfusion React DropDownList

The DropDownList component provides a list of predefined values from which users can select a single value. It supports local and remote data binding, filtering, grouping, custom templates, virtual scrolling, accessibility, and rich customization.

## Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- Installation and package setup (`@syncfusion/ej2-react-dropdowns`)
- CSS imports with tailwind3 theme
- Basic DropDownList (functional and class components)
- Binding a data source
- Configuring popup height and width

### Data Binding & Value Binding
📄 **Read:** [references/data-binding.md](references/data-binding.md)
- Binding primitive arrays (strings, numbers)
- Binding JSON object arrays with `fields` mapping (`text`, `value`, `groupBy`, `iconCss`)
- Remote data with `DataManager` (OData, ODataV4, WebAPI adaptors)
- Value binding: preselecting values, binding primitive vs. complex objects
- Disabling individual items with `fields.disabled`

> ⚠️ **Network notice:** Using `DataManager` with remote adaptors (`ODataAdaptor`, `ODataV4Adaptor`, `WebApiAdaptor`) causes the component to initiate **outbound HTTP requests** at runtime. Ensure all remote endpoints are trusted, authenticated, and do not expose sensitive data before wiring up remote data binding in production.

### Filtering
📄 **Read:** [references/filtering.md](references/filtering.md)
- Enabling `allowFiltering` for search-as-you-type
- Handling the `filtering` event with `updateData`
- **Preventing default filtering** — always set `args.preventDefaultAction = true` in custom handlers
- Filter types: `startswith`, `contains`, `endsWith`, case-sensitive options
- **Filtering by multiple fields** — use `Predicate` with `.or()` to match typed text against both `text` and `value` fields (or any combination)
- Remote/server-side filtering with minimum character guard
- Diacritics filtering (`ignoreAccent`), debounce delay
- Highlight filtered text, custom search logic

### Grouping & Templates
📄 **Read:** [references/grouping-and-templates.md](references/grouping-and-templates.md)
- Grouping items with the `groupBy` field
- Fixed and inline group headers
- Custom group header template
- Item template, value (selected) template
- Header and footer templates
- No-records and action-failure templates

### Features & Configuration
📄 **Read:** [references/features-and-configuration.md](references/features-and-configuration.md)
- Sorting (`sortOrder`: ascending, descending)
- Virtual scrolling for large lists (`enableVirtualization`)
- Popup resize (`allowResize`)
- Incremental search, clear button, readonly, disabled
- RTL support, Preact usage

### Accessibility, Styling & Localization
📄 **Read:** [references/accessibility-styling-localization.md](references/accessibility-styling-localization.md)
- WCAG 2.2 / Section 508 compliance
- Keyboard navigation shortcuts
- ARIA roles and attributes
- **`cssClass` prop** — scoped per-instance CSS class, multiple classes, conditional classes, built-in utility classes (`e-error`, `e-success`)
- CSS class customization (wrapper, icon, popup, list items, placeholder)
- Theming (tailwind3, material3, bootstrap5, fluent2)
- Localization (`L10n`, `noRecordsTemplate`, `actionFailureTemplate`)
- RTL layout (`enableRtl`)

### API Reference
📄 **Read:** [references/api.md](references/api.md)
- Complete properties reference with types, defaults, and usage examples
- All methods with signatures, parameters, and return types
- All events with argument interfaces and usage examples
- Interface details: `FieldSettingsModel`, `ChangeEventArgs`, `SelectEventArgs`, `PopupEventArgs`, `FilteringEventArgs`
- Quick-reference summary tables for properties, methods, and events

### How-To Patterns
📄 **Read:** [references/how-to.md](references/how-to.md)
- Add, remove, or modify items dynamically
- Cascading (dependent) dropdowns
- Multiple cascading dropdowns
- Remote data how-to
- Close popup on scroll, tooltip on items, icons in items
- Value change event, clearing selected value

---

## Quick Start Example

**Step 1 — Install the package** using your package manager (verify the package against the [official Syncfusion npm registry](https://www.npmjs.com/package/@syncfusion/ej2-react-dropdowns) before installing):

```
npm install @syncfusion/ej2-react-dropdowns --save
```

> ⚠️ **Supply-chain notice:** `@syncfusion/ej2-react-dropdowns` is a third-party package published by Syncfusion Inc. Always pin to a specific verified version in your `package.json` (e.g., `"@syncfusion/ej2-react-dropdowns": "27.x.x"`) and validate integrity via your lockfile (`package-lock.json` or `yarn.lock`) before deploying to production.

**Step 2 — Add CSS imports** in `src/App.css`:

```css
@import "../node_modules/@syncfusion/ej2-base/styles/tailwind3.css";
@import "../node_modules/@syncfusion/ej2-inputs/styles/tailwind3.css";
@import "../node_modules/@syncfusion/ej2-react-dropdowns/styles/tailwind3.css";
```

**Step 3 — Use the component:**

```tsx
import { DropDownListComponent } from '@syncfusion/ej2-react-dropdowns';
import './App.css';

export default function App() {
  const sportsData: string[] = ['Badminton', 'Cricket', 'Football', 'Golf', 'Tennis'];

  return (
    <DropDownListComponent
      id="ddlelement"
      dataSource={sportsData}
      placeholder="Select a game"
    />
  );
}
```

## Common Patterns

### JSON Object Data with Fields Mapping
```tsx
import { DropDownListComponent } from '@syncfusion/ej2-react-dropdowns';

export default function App() {
  const countryData = [
    { Id: 'au', Country: 'Australia' },
    { Id: 'br', Country: 'Brazil' },
    { Id: 'cn', Country: 'China' },
    { Id: 'in', Country: 'India' },
  ];
  const fields = { text: 'Country', value: 'Id' };

  return (
    <DropDownListComponent
      dataSource={countryData}
      fields={fields}
      placeholder="Select a country"
    />
  );
}
```

### Filtering (Search-as-you-type)
```tsx
import { DropDownListComponent, FilteringEventArgs } from '@syncfusion/ej2-react-dropdowns';
import { Query } from '@syncfusion/ej2-data';

export default function App() {
  const searchData = [
    { Index: 's1', Country: 'Alaska' },
    { Index: 's2', Country: 'California' },
    { Index: 's3', Country: 'Florida' },
  ];
  const fields = { text: 'Country', value: 'Index' };

  function onFiltering(args: FilteringEventArgs) {
    args.preventDefaultAction = true; // prevent built-in filter from running alongside custom logic
    let query = new Query();
    query = args.text !== '' ? query.where('Country', 'startswith', args.text, true) : query;
    args.updateData(searchData, query);
  }

  return (
    <DropDownListComponent
      dataSource={searchData}
      fields={fields}
      allowFiltering={true}
      filtering={onFiltering}
      placeholder="Select a country"
    />
  );
}
```

### Preselect a Value
```tsx
<DropDownListComponent
  dataSource={sportsData}
  value="Cricket"
  placeholder="Select a game"
/>
```

### Grouped Items
```tsx
const vegetableData = [
  { Vegetable: 'Cabbage', Category: 'Leafy and Salad', Id: 'item1' },
  { Vegetable: 'Chickpea', Category: 'Beans', Id: 'item6' },
  { Vegetable: 'Garlic',   Category: 'Bulb and Stem', Id: 'item9' },
];
const fields = { groupBy: 'Category', text: 'Vegetable', value: 'Id' };

<DropDownListComponent dataSource={vegetableData} fields={fields} placeholder="Select a vegetable" />
```

## Key Props

| Prop | Type | Purpose |
|---|---|---|
| `dataSource` | `any[] \| DataManager` | Data for the list items |
| `fields` | `FieldSettingsModel` | Maps `text`, `value`, `groupBy`, `iconCss`, `disabled` |
| `value` | `string \| number` | Selected/preselected value |
| `placeholder` | `string` | Input placeholder text |
| `allowFiltering` | `boolean` | Enables search filtering |
| `filtering` | `event` | Handler for custom filter logic |
| `popupHeight` | `string` | Popup list height (default auto) |
| `popupWidth` | `string` | Popup list width (default matches input) |
| `sortOrder` | `SortOrder` | `'None'`, `'Ascending'`, `'Descending'` |
| `enableVirtualization` | `boolean` | Virtual scroll for large data |
| `allowResize` | `boolean` | User-resizable popup |
| `enabled` | `boolean` | Enable/disable entire component |
| `readonly` | `boolean` | Read-only mode |
| `showClearButton` | `boolean` | Shows ✕ button to clear selection |
| `enableRtl` | `boolean` | Right-to-left layout |

## Common Use Cases

**Simple static list** → Pass `string[]` to `dataSource`, done.

**Data from API** → Use `DataManager` with `WebApiAdaptor`; read `references/data-binding.md`. ⚠️ Remote adaptors open outbound HTTP connections — ensure endpoints are trusted.

**Search/filter as user types** → Set `allowFiltering={true}`, handle `filtering` event; read `references/filtering.md`.

**Categorized items** → Map `groupBy` field; read `references/grouping-and-templates.md`.

**Custom item layout** → Use `itemTemplate`; read `references/grouping-and-templates.md`.

**Large dataset (10k+ items)** → Enable `enableVirtualization` + inject `VirtualScroll`; read `references/features-and-configuration.md`.

**Dependent dropdowns** → Reload second dropdown's `dataSource` on first's `change` event; read `references/how-to.md`.
