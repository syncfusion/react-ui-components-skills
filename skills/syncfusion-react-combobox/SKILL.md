---
name: syncfusion-react-combobox
description: Implement Syncfusion React ComboBox component for dropdown inputs with search and filtering capabilities. Use this when working with searchable dropdowns, data binding, templates, grouping, or virtual scrolling. This skill covers installation, data sources (local/remote), filtering, grouping, templates, advanced features like virtual scrolling and localization, and styling options.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
  category: "Dropdowns"
---

# Implementing Syncfusion React ComboBox

## Component Overview

The ComboBox is a **specialized dropdown input** that bridges typed input fields with selection lists. Key characteristics:

- **Hybrid input:** User can type to filter OR select from dropdown
- **Smart filtering:** Default filter searches by text, customizable for complex scenarios
- **Flexible data:** Supports strings, JSON objects, OData, Web APIs, DataManager
- **Rich UI:** Templates for items, headers, footers, selected values, no-results states
- **Performance:** Virtual scrolling efficiently handles thousands of items
- **Global ready:** Built-in localization, RTL support, ARIA attributes
- **Accessible:** Full keyboard navigation, screen reader support

**Installation:** `npm install @syncfusion/ej2-react-dropdowns`

---

## Documentation Navigation Guide

Choose your starting point based on your task:

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)

When to read:
- Setting up ComboBox in a new project
- First-time component implementation
- Understanding basic usage (class vs functional components)
- Adding CSS imports and themes
- Enabling custom values

### Data Binding & Sources
📄 **Read:** [references/data-binding.md](references/data-binding.md)

When to read:
- Binding local data (string arrays, JSON objects)
- Connecting to remote APIs (OData, Web API, DataManager)
- Mapping object fields (text, value, groupBy, iconCss)
- Handling complex data transformations
- Understanding data source configuration

### Filtering & Search Behavior
📄 **Read:** [references/filtering-and-search.md](references/filtering-and-search.md)

When to read:
- Implementing text filtering
- Creating custom filter functions
- Configuring search behavior (case sensitivity, partial matching)
- Performance optimization for large datasets
- Handling no-results scenarios

### Grouping & Sorting
📄 **Read:** [references/grouping-and-sorting.md](references/grouping-and-sorting.md)

When to read:
- Organizing items into logical groups
- Customizing group header appearance
- Applying sort orders (ascending/descending)
- Combining grouping with filtering
- Multi-level grouping scenarios

### Templates & Customization
📄 **Read:** [references/templates-and-customization.md](references/templates-and-customization.md)

When to read:
- Creating custom item templates
- Displaying rich content (images, icons, descriptions)
- Header/footer templates (e.g., action buttons, summaries)
- Selected value template formatting
- CSS class-based styling and theme customization

### Advanced Features
📄 **Read:** [references/advanced-features.md](references/advanced-features.md)

When to read:
- Virtual scrolling for large datasets (10,000+ items)
- Internationalization (i18n) and language switching
- RTL (right-to-left) support for Arabic/Hebrew
- Accessibility compliance (WCAG 2.1, ARIA attributes)
- Disabled states and multi-select workflows
- Keyboard shortcuts and focus management

### Styling & Theming
📄 **Read:** [references/styling-and-theming.md](references/styling-and-theming.md)

When to read:
- Applying built-in Syncfusion themes (Material, Bootstrap, Tailwind)
- CSS variable customization for brand colors
- Theme Studio integration for design customization
- Responsive design patterns
- Dark mode / light mode support

### Popup Resizing
📄 **Read:** [references/popup-resizing.md](references/popup-resizing.md)

When to read:
- Allowing users to dynamically resize the dropdown
- Saving resize preferences across sessions
- Handling long content with custom templates
- Mobile-friendly dropdown sizing

### How-To Guide
📄 **Read:** [references/how-to-guide.md](references/how-to-guide.md)

When to read:
- Implementing autofill (auto-complete while typing)
- Creating cascading/dependent dropdowns (Country → State → City)
- Displaying icons in list items
- Common practical implementation scenarios

### Troubleshooting
📄 **Read:** [references/troubleshooting.md](references/troubleshooting.md)

When to read:
- Performance issues (slow rendering, lag)
- Data binding problems
- Migration from EJ1 ComboBox
- Common edge cases and workarounds
- Debugging tips and community resources

### API Reference
📄 **Read:** [references/api.md](references/api.md)

When to read:
- Looking up a specific property, method, or event name and its type
- Understanding default values for any configuration option
- Checking event argument shapes (`ChangeEventArgs`, `FilteringEventArgs`, etc.)
- Reviewing all available methods for programmatic control
- Exploring interface models (`FieldSettingsModel`, `PopupEventArgs`, etc.)

---

## Quick Start Example

### Installation & Setup

```bash
npm install @syncfusion/ej2-react-dropdowns
```

### Basic ComboBox (Functional Component)

```tsx
import { ComboBoxComponent } from '@syncfusion/ej2-react-dropdowns';
import '@syncfusion/ej2-base/styles/tailwind3.css';
import '@syncfusion/ej2-react-dropdowns/styles/tailwind3.css';

export default function App() {
  const sportsList = ['Badminton', 'Cricket', 'Football', 'Golf', 'Tennis'];

  return (
    <ComboBoxComponent 
      id="combobox"
      dataSource={sportsList}
      placeholder="Select a sport"
      allowCustom={true}
    />
  );
}
```

### With Data Binding (JSON Objects)

```tsx
export default function App() {
  const sportsData = [
    { Id: 'game1', Game: 'Badminton', Players: 2 },
    { Id: 'game2', Game: 'Football', Players: 11 },
    { Id: 'game3', Game: 'Cricket', Players: 11 }
  ];

  const fields = { text: 'Game', value: 'Id' };

  return (
    <ComboBoxComponent 
      id="combobox"
      dataSource={sportsData}
      fields={fields}
      placeholder="Select a game"
      change={(e) => console.log('Selected:', e.value)}
    />
  );
}
```

---

## Common Patterns

### Pattern 1: Filtered Search for User Selection
**Problem:** User needs to find items in a list of 500+ entries.  
**Solution:** Use filtering with controlled input to display only matching items. For very large datasets (5,000+ items), combine with `enableVirtualization` and inject `VirtualScroll`.

```tsx
import { ComboBoxComponent, Inject, VirtualScroll } from '@syncfusion/ej2-react-dropdowns';

const [filterValue, setFilterValue] = useState('');

<ComboBoxComponent 
  dataSource={largeDataset}
  fields={{ text: 'name', value: 'id' }}
  filtering={(e) => filterByName(e, 'name')}
  change={(e) => setFilterValue(e.value)}
  allowFiltering={true}
  enableVirtualization={true}
  popupHeight="200px"
>
  <Inject services={[VirtualScroll]} />
</ComboBoxComponent>
```

### Pattern 2: Grouped Categories
**Problem:** Items need to be organized by type for clarity.  
**Solution:** Use groupBy field mapping with data grouped by category.

```tsx
const categorizedData = [
  { Category: 'Sports', Item: 'Cricket', value: 1 },
  { Category: 'Sports', Item: 'Football', value: 2 },
  { Category: 'Games', Item: 'Chess', value: 3 },
  { Category: 'Games', Item: 'Carrom', value: 4 }
];

const fields = { 
  text: 'Item', 
  value: 'value', 
  groupBy: 'Category' 
};

<ComboBoxComponent dataSource={categorizedData} fields={fields} />
```

### Pattern 3: Remote Data with Loading
**Problem:** Fetch data from an API based on user search.  
**Solution:** Use DataManager with Web API adapter.

```tsx
import { DataManager, WebApiAdaptor } from '@syncfusion/ej2-data';

const dataManager = new DataManager({
  url: 'https://api.example.com/sports',
  adaptor: new WebApiAdaptor()
});

<ComboBoxComponent 
  dataSource={dataManager}
  fields={{ text: 'name', value: 'id' }}
  allowFiltering={true}
/>
```

### Pattern 4: Custom Template for Rich Content
**Problem:** Display icons or additional info with each item.  
**Solution:** Use itemTemplate prop for custom HTML rendering.

```tsx
const itemTemplate = (props) => {
  return (
    <div className="flex items-center gap-2">
      <span className={`icon icon-${props.value}`}></span>
      <span>{props.name}</span>
      <small className="text-gray-500">({props.players} players)</small>
    </div>
  );
};

<ComboBoxComponent 
  dataSource={sportsData}
  itemTemplate={itemTemplate}
  fields={{ text: 'name', value: 'id' }}
/>
```

---

## Key Props Reference

| Prop | Type | Description | Common Use |
|------|------|-------------|-----------|
| `dataSource` | Array\|DataManager | Data items to display | All use cases |
| `fields` | FieldSettingsModel | Map data fields (text, value, groupBy, iconCss, disabled) | Complex data |
| `placeholder` | string | Hint text when empty | UX guidance |
| `allowCustom` | boolean | Allow user to enter custom values not in the list. Default: `true` | Open-ended input |
| `allowFiltering` | boolean | Enable filter bar (search box) in the popup. Default: `false` | Search capability |
| `enableVirtualization` | boolean | Enable virtual scrolling for large datasets. Requires `<Inject services={[VirtualScroll]} />`. Default: `false` | Large datasets (5,000+ items) |
| `sortOrder` | SortOrder | `'None'` \| `'Ascending'` \| `'Descending'`. Default: `null` | Sorted lists |
| `popupHeight` | string\|number | Dropdown height (e.g., `'300px'`). Default: `'300px'` | Layout control |
| `popupWidth` | string\|number | Dropdown width (e.g., `'100%'`). Default: `'100%'` | Layout control |
| `enabled` | boolean | Enable/disable component. Default: `true` | Conditional rendering |
| `readonly` | boolean | Prevent user input. Default: `false` | View-only mode |
| `showClearButton` | boolean | Show clear (×) button. Default: `true` | Allow clearing selection |
| `change` | EmitType\<ChangeEventArgs\> | Fires when selection changes | Event handling |
| `filtering` | EmitType\<FilteringEventArgs\> | Fires when user types; use for custom filter logic | Advanced search |
| `itemTemplate` | Function\|string | Custom item HTML for each list item | Rich UI |

---

## Next Steps

1. **New to ComboBox?** → Start with [getting-started.md](references/getting-started.md)
2. **Need specific data?** → Go to [data-binding.md](references/data-binding.md)
3. **Performance concerns?** → Check [advanced-features.md](references/advanced-features.md)
4. **Design customization?** → See [styling-and-theming.md](references/styling-and-theming.md)
5. **Common scenarios?** → Explore [how-to-guide.md](references/how-to-guide.md) (autofill, cascading, icons)
6. **Resizable dropdowns?** → Read [popup-resizing.md](references/popup-resizing.md)
7. **Stuck?** → Visit [troubleshooting.md](references/troubleshooting.md)
8. **Need full API details?** → See [api.md](references/api.md) for all properties, methods, events, and interface models
