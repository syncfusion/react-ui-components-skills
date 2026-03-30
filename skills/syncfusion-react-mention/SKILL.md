---
name: syncfusion-react-mention
description: Implement the Syncfusion React Mention (MentionComponent) for @-tagging users or items inside editable content areas. Use this when adding @mention or #hashtag dropdowns, binding local or remote data to a mention popup, customizing suggestion lists, filtering mention results, or configuring trigger characters. This skill covers MentionComponent setup, data binding, templates, and event handling in React applications.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
  category: "Dropdowns"
---

# Syncfusion React Mention Component

The Syncfusion React `MentionComponent` attaches to a target editable element (e.g. a `<div contenteditable>`) and displays a suggestion popup when the user types a trigger character (default `@`). Selecting an item inserts it inline into the editor.

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- Installation and package setup (`@syncfusion/ej2-react-dropdowns`)
- CSS theme imports
- Basic `MentionComponent` setup with `target` prop
- Binding a simple data source
- Custom trigger character (`mentionChar`) and `showMentionChar`

> ⚠️ **Security Note:** Installing npm packages (`@syncfusion/ej2-react-dropdowns` and related) must be a deliberate, user-confirmed step. Do not allow automated agents to run `npm install` without explicit user approval, as this introduces supply-chain risk.

### Working with Data
📄 **Read:** [references/working-with-data.md](references/working-with-data.md)
- Binding array of strings, JSON objects, and complex nested data
- Mapping `fields` (text, value, groupBy, iconCss)
- Remote data with OData V4 (`ODataV4Adaptor`)
- Remote data with Web API (`WebApiAdaptor`)
- Using `query` to filter/select remote fields

### Filtering Data
📄 **Read:** [references/filtering-data.md](references/filtering-data.md)
- Setting minimum filter character length (`minLength`)
- Changing filter type: `Contains`, `StartsWith`, `EndsWith`
- Allowing spaces within search text (`allowSpaces`)
- Customizing the suggestion count (`suggestionCount`)
- Debounce delay for filtering (`debounceDelay`)

### Templates
📄 **Read:** [references/template.md](references/template.md)
- Item template (`itemTemplate`) for custom list rendering
- Display template (`displayTemplate`) for selected value format
- No-records template (`noRecordsTemplate`)
- Spinner/loading template (`spinnerTemplate`)
- Group header template (`groupTemplate`)

### Customization
📄 **Read:** [references/customization.md](references/customization.md)
- Show/hide mention character in output (`showMentionChar`)
- Appending suffix text after selection (`suffixText`)
- Configuring popup height and width (`popupHeight`, `popupWidth`)
- Custom trigger character (`mentionChar`)
- Leading space requirement (`requireLeadingSpace`)
- CSS class customization (`cssClass`)
- Highlight matched characters (`highlight`)
- Ignore accent/case in search (`ignoreAccent`, `ignoreCase`)
- Z-index for popup (`zIndex`)

### Sorting
📄 **Read:** [references/sorting.md](references/sorting.md)
- Sorting suggestion list: `Ascending`, `Descending`, `None`

### Disabled Items
📄 **Read:** [references/disabled-items.md](references/disabled-items.md)
- Disabling items via `fields.disabled`
- Dynamically disabling items with `disableItem()` method

### Accessibility
📄 **Read:** [references/accessibility.md](references/accessibility.md)
- WAI-ARIA attributes (`aria-selected`, `aria-activedescendent`, `aria-owns`)
- Keyboard navigation shortcuts
- WCAG 2.2 and Section 508 compliance
- RTL support (`enableRtl`)

### Localization
📄 **Read:** [references/localization.md](references/localization.md)
- Localizing the `noRecordsTemplate` text via `L10n`
- Setting `locale` property

### API Reference
📄 **Read:** [references/api.md](references/api.md)
- All properties, methods, and events
- `target`, `dataSource`, `fields`, `mentionChar`, `minLength`, `suggestionCount`
- Methods: `addItem`, `disableItem`, `getDataByValue`, `getItems`, `showPopup`, `hidePopup`, `search`, `destroy`
- Events: `select`, `change`, `filtering`, `beforeOpen`, `opened`, `closed`, `dataBound`, `actionBegin`, `actionComplete`, `actionFailure`

## Quick Start Example

```tsx
import { MentionComponent } from '@syncfusion/ej2-react-dropdowns';
import * as React from 'react';

// CSS imports
// @import "../node_modules/@syncfusion/ej2-base/styles/tailwind3.css";
// @import "../node_modules/@syncfusion/ej2-react-dropdowns/styles/tailwind3.css";

function App() {
  const mentionTarget = '#commentBox';
  const users = [
    { Name: 'Selma Rose', EmailId: 'selma@example.com' },
    { Name: 'Robert', EmailId: 'robert@example.com' },
    { Name: 'William', EmailId: 'william@example.com' },
  ];
  const fields = { text: 'Name' };

  return (
    <div>
      <label>Comments</label>
      <div id="commentBox" placeholder="Type @ to mention a user"></div>
      <MentionComponent
        target={mentionTarget}
        dataSource={users}
        fields={fields}
      />
    </div>
  );
}

export default App;
```

## Common Patterns

### Custom Trigger Character
Use `mentionChar` to trigger with `#` instead of `@`:
```tsx
<MentionComponent
  target="#editor"
  dataSource={tags}
  mentionChar="#"
  showMentionChar={true}
/>
```

### Remote Data with Filtering
```tsx
import { DataManager, ODataV4Adaptor, Query } from '@syncfusion/ej2-data';

// NOTE: Replace the URL below with your own API endpoint.
// Do not use third-party or public endpoints in production.
const dataSource = new DataManager({
  url: 'https://your-api-endpoint.example.com/odata/',
  adaptor: new ODataV4Adaptor,
  crossDomain: true,
});
const query = new Query().from('Customers').select(['ContactName', 'CustomerID']).take(6);
const fields = { text: 'ContactName', value: 'CustomerID' };

<MentionComponent
  target="#editor"
  dataSource={dataSource}
  fields={fields}
  query={query}
  minLength={2}
  popupWidth="250px"
/>
```

### Handling Selection Events
```tsx
import { SelectEventArgs } from '@syncfusion/ej2-react-dropdowns';

function onSelect(args: SelectEventArgs) {
  console.log('Selected item:', args.itemData);
  console.log('Selected text:', args.text);
}

<MentionComponent
  target="#editor"
  dataSource={users}
  fields={{ text: 'Name' }}
  select={onSelect}
/>
```

### Popup Configuration
```tsx
<MentionComponent
  target="#editor"
  dataSource={data}
  fields={{ text: 'Name' }}
  popupHeight="200px"
  popupWidth="300px"
  suggestionCount={10}
  sortOrder="Ascending"
  suffixText="&nbsp;"
/>
```

## Key Props Summary

| Prop | Type | Default | Purpose |
|------|------|---------|---------|
| `target` | `string\|HTMLElement` | — | CSS selector or element for the editable area |
| `dataSource` | `array\|DataManager` | `[]` | Data for suggestions |
| `fields` | `FieldSettingsModel` | `{text:null,value:null}` | Maps data fields |
| `mentionChar` | `string` | `'@'` | Trigger character |
| `showMentionChar` | `boolean` | `false` | Prepend trigger char to inserted text |
| `minLength` | `number` | `0` | Min chars before search |
| `suggestionCount` | `number` | `25` | Max items in popup |
| `filterType` | `FilterType` | `'Contains'` | Filter match strategy |
| `allowSpaces` | `boolean` | `false` | Allow spaces in search |
| `sortOrder` | `SortOrder` | `'None'` | Sort direction |
| `popupHeight` | `string\|number` | `'300px'` | Popup height |
| `popupWidth` | `string\|number` | `'auto'` | Popup width |
| `suffixText` | `string` | `null` | Text appended after selection |
| `requireLeadingSpace` | `boolean` | `true` | Space required before trigger char |
| `highlight` | `boolean` | `false` | Highlight search characters |
