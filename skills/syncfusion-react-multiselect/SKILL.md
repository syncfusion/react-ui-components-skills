---
name: syncfusion-react-multiselect
description: Implement Syncfusion React MultiSelect Dropdown component for multi-value selection. Use this when working with multi-select dropdowns, checkbox list pickers, or tag/chip selection interfaces. This skill covers data binding, filtering, grouping, templates, accessibility, custom values, checkbox mode, virtual scrolling, and styling options.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
  category: "Dropdowns"
---

# Implementing Syncfusion React MultiSelect

A comprehensive skill for implementing the Syncfusion React MultiSelect Dropdown component — enabling users to select multiple values from a list with support for filtering, grouping, templates, checkboxes, chips, virtual scrolling, and more.

## Component Overview

```tsx
import { MultiSelectComponent } from '@syncfusion/ej2-react-dropdowns';
import '@syncfusion/ej2-react-dropdowns/styles/tailwind3.css';
// Also import base dependencies:
// @syncfusion/ej2-base/styles/tailwind3.css
// @syncfusion/ej2-buttons/styles/tailwind3.css
// @syncfusion/ej2-inputs/styles/tailwind3.css
```

**Package:** `@syncfusion/ej2-react-dropdowns`  
**Main component:** `MultiSelectComponent`  
**Checkbox module:** `CheckBoxSelection` (inject via `<Inject services={[CheckBoxSelection]} />`)  
**Virtual scroll module:** `VirtualScroll` (inject via `<Inject services={[VirtualScroll]} />`)

## Quick Start Example

```tsx
import { MultiSelectComponent } from '@syncfusion/ej2-react-dropdowns';
import * as React from 'react';

export default function App() {
  const sportsData = ['Badminton', 'Basketball', 'Cricket', 'Football', 'Golf', 'Tennis'];

  return (
    <MultiSelectComponent
      id="multiselect"
      dataSource={sportsData}
      placeholder="Select sports"
    />
  );
}
```

With objects and field mapping:

```tsx
import { MultiSelectComponent } from '@syncfusion/ej2-react-dropdowns';
import * as React from 'react';

export default function App() {
  const sportsData = [
    { id: 'game1', sports: 'Badminton' },
    { id: 'game2', sports: 'Football' },
    { id: 'game3', sports: 'Tennis' },
  ];
  const fields = { text: 'sports', value: 'id' };

  return (
    <MultiSelectComponent
      id="multiselect"
      dataSource={sportsData}
      fields={fields}
      placeholder="Select a game"
    />
  );
}
```

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- Installation and package setup (Vite / CRA)
- CSS imports and theming
- Basic MultiSelect implementation (class & functional components)
- Binding a simple data source
- Popup height/width configuration

### Data Binding
📄 **Read:** [references/data-binding.md](references/data-binding.md)
- Array of strings vs array of objects
- Field mapping (`text`, `value`, `groupBy`, `iconCss`, `disabled`)
- Remote data via `DataManager` (OData, OData V4, Web API)
- JSON/JSONP formats
- Complex data binding gotchas

### Grouping
📄 **Read:** [references/grouping.md](references/grouping.md)
- Grouping items by category with `groupBy`
- Inline vs fixed group headers
- Group header templates
- Ordering and multi-level grouping

### Filtering
📄 **Read:** [references/filtering.md](references/filtering.md)
- Enabling `allowFiltering`
- Handling the `filtering` event
- Query API with `where()` conditions
- Filter types: startswith, contains, endswith
- Case sensitivity, multiple conditions, performance

### Templates
📄 **Read:** [references/templates.md](references/templates.md)
- Item templates for custom list item layouts
- Value/chip templates for selected display
- Group header templates
- Header, footer, no-records, and action-failure templates

### Selection Modes and Features
📄 **Read:** [references/selection-and-features.md](references/selection-and-features.md)
- Checkbox mode (inject `CheckBoxSelection`)
- Chip/tag display and the `tagging` event
- Custom values (`allowCustomValue`)
- Value binding (primitive and object types)
- Disabled items via `fields.disabled`
- Popup resizing (`allowResize`)
- Virtual scrolling for large datasets (inject `VirtualScroll`)

### Accessibility, Styling, and Localization
📄 **Read:** [references/accessibility-styling-localization.md](references/accessibility-styling-localization.md)
- WAI-ARIA attributes and keyboard shortcuts
- WCAG 2.2 / Section 508 / Screen reader support
- CSS customization (chips, wrapper, icon, delimiter)
- RTL support
- Localization with `L10n`

### API Reference
📄 **Read:** [references/api.md](references/api.md)
- Complete list of all properties with types, descriptions, and defaults
- All public methods with parameter details and return types
- All events with their argument types and trigger conditions

## Key Props Reference

| Prop | Type | When to Use |
|------|------|-------------|
| `dataSource` | `string[] \| object[] \| DataManager` | Always required — data to display |
| `fields` | `FieldSettingsModel` | When using object data; map `text`, `value`, `groupBy`, `disabled` |
| `mode` | `'Default' \| 'Box' \| 'CheckBox' \| 'Delimiter'` | Change selection display: chips (`Box`), checkboxes, or comma-delimited |
| `value` | `string[] \| number[] \| object[]` | Pre-select items on load |
| `allowFiltering` | `boolean` | Enable search-as-you-type filtering |
| `allowCustomValue` | `boolean` | Let users type values not in the list |
| `allowObjectBinding` | `boolean` | Return full objects as selected values instead of primitives |
| `enableVirtualization` | `boolean` | Use for 500+ items to improve performance |
| `allowResize` | `boolean` | Let users resize the popup |
| `popupHeight` | `string` | Limit popup list height (default: `300px`) |
| `popupWidth` | `string` | Set popup width (default: matches input) |
| `placeholder` | `string` | Input placeholder text |

## Common Use Cases

**Multi-tag input (chip display):**
→ Use `mode="Box"` (default). Each selection becomes a chip.

**Checkbox multi-selection:**
→ Use `mode="CheckBox"` and inject `CheckBoxSelection` module.

**Search/filter a long list:**
→ Set `allowFiltering={true}` and handle the `filtering` event for remote data.

**Pre-select values:**
→ Pass `value` prop as an array of value-field values: `value={['id1', 'id2']}`.

**Group by category:**
→ Add `groupBy` to the `fields` prop: `fields={{ text: 'name', value: 'id', groupBy: 'category' }}`.

**Large datasets (1000+ items):**
→ Set `enableVirtualization={true}` and inject `VirtualScroll` module.

**Disable specific options:**
→ Add a boolean `disabled` column to your data and map it: `fields={{ ..., disabled: 'isDisabled' }}`.

**Custom entry not in list:**
→ Set `allowCustomValue={true}`; handle `customValueSelection` event for custom actions.

## Decision Guide

```
User wants multiple selections?
  ├── Visual chips with filter → mode="Box" (default) + allowFiltering
  ├── Checkbox-style list      → mode="CheckBox" + Inject CheckBoxSelection
  └── Comma-separated display  → mode="Delimiter"

Data source type?
  ├── Simple strings/numbers   → Pass array directly to dataSource
  ├── Objects                  → Pass array + fields={{ text, value }}
  └── Remote API               → Use DataManager + allowFiltering + filtering event

List is very long?
  └── Yes (500+) → enableVirtualization={true} + Inject VirtualScroll

Need custom item layout?
  └── Use itemTemplate, valueTemplate, groupTemplate props

Need localization?
  └── Read references/accessibility-styling-localization.md
```
