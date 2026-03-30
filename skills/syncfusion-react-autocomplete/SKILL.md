---
name: syncfusion-react-autocomplete
description: Implement the Syncfusion React AutoComplete component for type-ahead suggestions and filtered dropdowns. Use this when working with AutoComplete input fields, local or remote data binding, filtering strategies, item/group templates, or virtualization in React applications. This skill covers installation, data binding, filtering, grouping, accessibility (WAI-ARIA), and API usage for ej2-react-dropdowns AutoComplete.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
  category: "Dropdowns"
---

# Syncfusion React AutoComplete

The Syncfusion React AutoComplete component provides a matched suggestion list as the user types into an input field, allowing selection from the filtered results. It supports local and remote data, rich filtering options, templates, grouping, virtualization, and full accessibility.

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- Package installation (`@syncfusion/ej2-react-dropdowns`)
- CSS imports and theme configuration
- Basic component setup (functional and class components)
- Binding a simple string array
- Configuring popup height and width (`popupHeight`, `popupWidth`)

### Data Binding
📄 **Read:** [references/data-binding.md](references/data-binding.md)
- Array of strings or numbers
- Array of objects with `fields` mapping (`value`, `groupBy`, `iconCss`)
- Array of complex/nested objects (dot-notation field mapping)
- Remote data with `DataManager` (ODataV4Adaptor, WebApiAdaptor)
- Using `query` property to filter/select remote data
- `sortOrder` for alphabetical ordering

### Filtering
📄 **Read:** [references/filtering.md](references/filtering.md)
- Filter types: `StartsWith`, `EndsWith`, `Contains`
- `suggestionCount` – limit number of suggestions
- `minLength` – minimum characters before search triggers
- `ignoreCase` – case-sensitive filtering
- `ignoreAccent` – diacritics filtering
- `debounceDelay` – delay filtering to reduce requests
- Custom filtering with the `filtering` event

### Grouping
📄 **Read:** [references/grouping.md](references/grouping.md)
- Grouping items using `fields.groupBy`
- Fixed and inline group headers
- Custom group header with `groupTemplate`

### Templates
📄 **Read:** [references/templates.md](references/templates.md)
- `itemTemplate` – customize each list item
- `groupTemplate` – customize group header
- `headerTemplate` – static popup header
- `footerTemplate` – static popup footer
- `noRecordsTemplate` – message when no data found
- `actionFailureTemplate` – message on remote fetch failure

### Value Binding
📄 **Read:** [references/value-binding.md](references/value-binding.md)
- Binding primitive values (string, number)
- Object binding with `allowObjectBinding`
- Presetting selected values with the `value` property

### Virtualization
📄 **Read:** [references/virtual-scroll.md](references/virtual-scroll.md)
- `enableVirtualization` for large datasets
- Injecting the `VirtualScroll` service
- Virtual scrolling with local data, remote data, and grouping
- Customizing item count with `query.take()`

### Disabled Items
📄 **Read:** [references/disabled-items.md](references/disabled-items.md)
- Disabling items via `fields.disabled`
- `disableItem` method for dynamic disabling
- Disabling the entire component with `enabled={false}`

### Accessibility and Localization
📄 **Read:** [references/accessibility-localization.md](references/accessibility-localization.md)
- WAI-ARIA roles and attributes
- Keyboard navigation shortcuts
- RTL support (`enableRtl`)
- Localization with `L10n` (noRecordsTemplate, actionFailureTemplate text)
- WCAG 2.2 compliance overview

### Styling and Customization
📄 **Read:** [references/styling.md](references/styling.md)
- CSS class targets for wrapper, icon, focus, placeholder, selection
- Float label customization
- Popup item appearance
- `cssClass` property for custom class injection
- Popup resize (`allowResize`)
- Mandatory asterisk styling

### How-To: Autofill, Highlight, and Icons
📄 **Read:** [references/how-to.md](references/how-to.md)
- `autofill` – suggest first matched item on Arrow Down
- `highlight` – highlight typed characters in suggestion list
- Icon support with `fields.iconCss`

### API Reference
📄 **Read:** [references/api.md](references/api.md)
- All properties with types, defaults, and descriptions
- All methods with parameters and return types
- All events with descriptions

## Quick Start Example

> **Installation:** Pin packages to a specific major version to reduce supply-chain risk.
> ```bash
> npm install @syncfusion/ej2-react-dropdowns@^33.x.x @syncfusion/ej2-react-inputs@^33.x.x @syncfusion/ej2-base@^33.x.x
> ```

```tsx
import { AutoCompleteComponent } from '@syncfusion/ej2-react-dropdowns'; // ^33.x.x
import '@syncfusion/ej2-base/styles/tailwind3.css';
import '@syncfusion/ej2-react-inputs/styles/tailwind3.css';
import '@syncfusion/ej2-react-dropdowns/styles/tailwind3.css';

const sportsData: string[] = [
  'Badminton', 'Basketball', 'Cricket', 'Football',
  'Golf', 'Hockey', 'Rugby', 'Snooker', 'Tennis'
];

export default function App() {
  return (
    <AutoCompleteComponent
      id="sports-ac"
      dataSource={sportsData}
      placeholder="Find a game"
    />
  );
}
```

## Common Patterns

### Object data source with field mapping
```tsx
const sportsData = [
  { id: 'Game1', game: 'Badminton' },
  { id: 'Game2', game: 'Basketball' },
];
const fields = { value: 'game' };

<AutoCompleteComponent dataSource={sportsData} fields={fields} placeholder="Find a game" />
```

### Remote data binding
> **Note:** Replace the URL below with your own OData endpoint. Do **not** use a live third-party URL in production.

```tsx
import { DataManager, ODataV4Adaptor, Query } from '@syncfusion/ej2-data';

const customerData = new DataManager({
  adaptor: new ODataV4Adaptor,
  crossDomain: true,
  url: 'https://your-api-endpoint.example.com/odata/' // ← replace with your own endpoint
});
const query = new Query().from('Customers').select(['ContactName', 'CustomerID']).take(6);
const fields = { value: 'ContactName' };

<AutoCompleteComponent
  dataSource={customerData}
  query={query}
  fields={fields}
  sortOrder="Ascending"
  placeholder="Find a customer"
/>
```

### Filtering with custom options
```tsx
<AutoCompleteComponent
  dataSource={sportsData}
  filterType="StartsWith"
  minLength={2}
  suggestionCount={5}
  debounceDelay={300}
  ignoreCase={true}
  placeholder="Type to search"
/>
```

### Accessing methods via ref
```tsx
import { AutoCompleteComponent } from '@syncfusion/ej2-react-dropdowns';
import { useRef } from 'react';

export default function App() {
  const acRef = useRef<AutoCompleteComponent>(null);

  return (
    <>
      <AutoCompleteComponent ref={acRef} dataSource={sportsData} placeholder="Find a game" />
      <button onClick={() => acRef.current?.showPopup()}>Open</button>
      <button onClick={() => acRef.current?.clear()}>Clear</button>
    </>
  );
}
```
