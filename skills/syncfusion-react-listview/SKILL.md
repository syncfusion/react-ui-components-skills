---
name: syncfusion-react-listview
description: Implement Syncfusion React ListView component for displaying dynamic lists with single/multiple selection, templates, filtering, grouping, drag-drop, and virtualization. Use this when you need list visualization, item management, selection handling, custom templates, and data filtering. Supports local/remote data binding, checkboxes, animations, RTL, and accessibility features.
metadata:
  author: "Syncfusion Inc"
  category: "Lists"
  version: "33.1.44"
---

# Implementing Syncfusion React ListView

A comprehensive guide to implementing the Syncfusion React ListView component for displaying dynamic, interactive lists with advanced features like templating, filtering, selection, and data management.

## When to Use This Skill

Use this skill when you need to:
- Display lists of items with rich templating and customization
- Implement single or multiple item selection
- Add, remove, or update list items dynamically
- Filter and search list data
- Create nested or grouped lists
- Handle user interactions (scroll, select, check)
- Apply custom animations and styling
- Support drag-and-drop operations
- Optimize performance with virtualization
- Implement data binding (local or remote)
- Add checkboxes or custom icons
- Provide accessibility features (WCAG 2.1)

## Component Overview

The **ListView** component displays a collection of items in a scrollable, interactive list. It supports:

- **Data Sources:** Arrays, objects, DataManager, remote APIs
- **Selection Modes:** None, single, multiple, or checkbox-based
- **Templates:** Item templates, header templates, group templates
- **Data Operations:** Filtering, sorting, grouping, searching
- **Performance:** Virtual scrolling for 1000+ items
- **Interactions:** Click, select, scroll events
- **Styling:** Built-in themes, custom CSS, RTL support
- **Accessibility:** WCAG 2.1 Level AA compliant

## Installation & Setup

### Step 1: Install Syncfusion Packages

```bash
npm install @syncfusion/ej2-react-lists @syncfusion/ej2-base @syncfusion/ej2-data
```

### Step 2: Import Required Modules

```tsx
import { ListViewComponent } from '@syncfusion/ej2-react-lists';
// For additional features:
import { Inject, Virtualization } from '@syncfusion/ej2-react-lists';
```

### Step 3: Import CSS

```tsx
import '@syncfusion/ej2-react-lists/styles/material.css'; // or other themes
```

## Quick Start Example

```tsx
import React from 'react';
import { ListViewComponent } from '@syncfusion/ej2-react-lists';

export function BasicListView() {
  const data = [
    { id: '1', text: 'Item 1' },
    { id: '2', text: 'Item 2' },
    { id: '3', text: 'Item 3' }
  ];

  return (
    <ListViewComponent
      id="list"
      dataSource={data}
      fields={{ text: 'text', id: 'id' }}
    />
  );
}
```

## Core Concepts

### 1. Data Binding

**Local Array:**
```tsx
const items = ['Apple', 'Banana', 'Orange'];
<ListViewComponent dataSource={items} />
```

**Object Array with Mapping:**
```tsx
const data = [
  { productId: 1, productName: 'Laptop', category: 'Electronics' },
  { productId: 2, productName: 'Desk', category: 'Furniture' }
];

<ListViewComponent
  dataSource={data}
  fields={{
    id: 'productId',
    text: 'productName',
    groupBy: 'category'
  }}
/>
```

### 2. Selection Handling

```tsx
const handleSelect = (args) => {
  console.log('Selected item:', args.text, args.id);
};

<ListViewComponent
  dataSource={data}
  select={handleSelect}
/>
```

### 3. Item Management

- **Add Items:** `addItem(data, fields?)`
- **Remove Items:** `removeItem(item)`
- **Update Items:** Replace in dataSource and refresh
- **Get Selection:** `getSelectedItems()`

### 4. Templates

```tsx
// Item Template
const itemTemplate = (props) => (
  <div className="e-list-wrapper">
    <span>{props.text}</span>
    <span className="e-list-content">{props.category}</span>
  </div>
);

<ListViewComponent
  dataSource={data}
  fields={fields}
  template={itemTemplate}
/>
```

## Documentation & Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- Installation and imports
- Basic setup and first component
- CSS themes and styling
- Minimal working example
- Project structure

### Data Binding & Rendering
📄 **Read:** [references/data-binding-rendering.md](references/data-binding-rendering.md)
- Local array data sources
- Object mapping with fields
- Remote data with DataManager
- Query filtering on remote data
- Load on demand and pagination
- Dynamic data updates

### Item Management (CRUD)
📄 **Read:** [references/item-management.md](references/item-management.md)
- Adding new items dynamically
- Removing items by reference
- Updating existing items
- Batch operations
- Finding items in list
- Managing nested list items

### Selection & Filtering
📄 **Read:** [references/selection-filtering.md](references/selection-filtering.md)
- Single item selection
- Multiple item selection
- Checkbox-based selection
- Programmatic selection
- Filtering list items
- Search functionality
- Selection events and callbacks

### Templating & Customization
📄 **Read:** [references/templating-customization.md](references/templating-customization.md)
- Custom item templates (JSX, function, string)
- Header templates with `headerTemplate`
- Group header templates with `groupTemplate`
- Template data context and variables
- Dynamic templates based on device
- CSS customization with classes
- RTL template support

### Advanced Features
📄 **Read:** [references/advanced-features.md](references/advanced-features.md)
- Grouping and sorting
- Nested lists with hierarchy (up to 3 levels)
- Checkbox state management
- Animations and effects
- Custom icons and image display
- Enable/disable item states

### Performance & Virtualization
📄 **Read:** [references/performance-virtualization.md](references/performance-virtualization.md)
- Virtual scrolling for large datasets (1000+)
- Enabling virtualization
- Refresh item heights
- Memory optimization tips
- Combining with pagination
- Performance best practices
- Dataset size recommendations

### API Reference & Properties
📄 **Read:** [references/api-reference.md](references/api-reference.md)
- Complete property documentation
- Method signatures and parameters
- Event handler arguments
- Default values for all properties
- Property type specifications
- Usage examples for each API

### Accessibility & Events
📄 **Read:** [references/accessibility-events.md](references/accessibility-events.md)
- WCAG 2.1 compliance
- Keyboard navigation
- Screen reader support
- ARIA attributes
- Focus management
- Event lifecycle
- Event arguments and data

## Common Patterns

### Pattern 1: List with Selection

User clicks item → select event fires → update UI

```tsx
const [selected, setSelected] = React.useState(null);

const handleSelect = (args) => {
  setSelected(args);
};

<ListViewComponent
  dataSource={items}
  select={handleSelect}
/>
```

### Pattern 2: Add/Remove Items

```tsx
const listViewRef = React.useRef(null);

const addItem = () => {
  listViewRef.current.addItem([{ text: 'New Item', id: Date.now() }]);
};

const removeItem = (item) => {
  listViewRef.current.removeItem(item);
};

<ListViewComponent ref={listViewRef} dataSource={items} />
```

### Pattern 3: Filtered List

```tsx
const [filteredData, setFilteredData] = React.useState(items);

const handleFilter = (searchText) => {
  const filtered = items.filter(item =>
    item.text.toLowerCase().includes(searchText.toLowerCase())
  );
  setFilteredData(filtered);
};

<ListViewComponent dataSource={filteredData} />
```

### Pattern 4: Multiple Selection with Checkboxes

```tsx
<ListViewComponent
  dataSource={items}
  showCheckBox={true}
  fields={{ id: 'id', text: 'text', isChecked: 'checked' }}
/>
```

## Key Properties

| Property | Type | Default | Purpose |
|----------|------|---------|---------|
| `dataSource` | Array/DataManager | `[]` | Data to display |
| `fields` | FieldSettingsModel | `defaultMappedFields` | Map data to fields |
| `template` | string/function/JSX | `null` | Custom item template |
| `headerTemplate` | string/function/JSX | `null` | Custom header template |
| `groupTemplate` | string/function/JSX | `null` | Custom group template |
| `showCheckBox` | boolean | `false` | Show checkboxes |
| `checkBoxPosition` | 'Left'\|'Right' | `'Left'` | Checkbox position |
| `sortOrder` | 'None'\|'Ascending'\|'Descending' | `'None'` | Data sort order |
| `enableVirtualization` | boolean | `false` | Virtual scrolling |
| `height` | number\|string | `''` | List height |
| `width` | number\|string | `''` | List width |
| `cssClass` | string | `''` | Custom CSS class |
| `enabled` | boolean | `true` | Enable/disable component |
| `enableRtl` | boolean | `false` | Right-to-left support |
| `animation` | AnimationSettings | `{...}` | Item animations |

## Key Methods

| Method | Purpose |
|--------|---------|
| `addItem(data, fields?)` | Add new items to list |
| `removeItem(item)` | Remove item from list |
| `removeMultipleItems(items)` | Remove multiple items at once |
| `selectItem(item)` | Select an item |
| `selectMultipleItems(items)` | Select multiple items |
| `unselectItem(item?)` | Deselect item(s) |
| `getSelectedItems()` | Get current selection |
| `findItem(item)` | Find item details |
| `checkItem(item)` | Check a checkbox item |
| `uncheckItem(item)` | Uncheck checkbox item |
| `checkAllItems()` | Check all items |
| `uncheckAllItems()` | Uncheck all items |
| `enableItem(item)` | Enable disabled item |
| `disableItem(item)` | Disable item |
| `hideItem(item)` | Hide item |
| `showItem(item)` | Show hidden item |
| `back()` | Navigate back from nested list |
| `refreshItemHeight()` | Refresh item heights (virtualization) |
| `destroy()` | Clean up component |

## Key Events

| Event | Triggers When |
|-------|----------------|
| `select` | Item is selected |
| `actionBegin` | Any action starts |
| `actionComplete` | Any action completes |
| `actionFailure` | Remote data fetch fails |
| `scroll` | User scrolls to top/bottom |

## Common Use Cases

1. **Display Settings List:** Grouped items with icons
2. **Contact List:** Multi-line templates with images
3. **Nested Navigation:** Drill-down hierarchy
4. **Filterable Search:** Dynamic data filtering
5. **Todo List:** Checkboxes and item removal
6. **Product Catalog:** Pagination and filtering
7. **Chat Messages:** Reverse grouping by date
8. **Notification Feed:** Scrolling with actions
9. **Multi-Select Picker:** Checkboxes and buttons
10. **Hierarchical Menu:** Nested items with back navigation

## Tips & Best Practices

- ✅ Always map `fields` correctly for data to display
- ✅ Use templates for rich UI beyond plain text
- ✅ Enable virtualization for 1000+ items
- ✅ Use `DataManager` for filtering/sorting large remote data
- ✅ Memoize templates and callbacks to prevent re-renders
- ✅ Use `cssClass` for lightweight customization
- ✅ Handle `select` event for user interactions
- ✅ Clean up with `destroy()` when component unmounts
- ❌ Don't render complex components in every item template
- ❌ Don't forget to set proper `fields` mapping

## Troubleshooting

**Items not displaying?**
- Verify `dataSource` is populated
- Check `fields` mapping matches data structure
- Ensure `text` field is mapped correctly

**Selection not working?**
- Verify `select` event handler is bound
- Check item has valid `id` field
- Ensure item is not disabled

**Performance issues?**
- Enable `enableVirtualization={true}`
- Reduce template complexity
- Use `DataManager` for remote filtering instead of client-side

**Styling issues?**
- Import CSS theme file
- Check `cssClass` is applied to root element
- Inspect CSS specificity conflicts

---

**Next Steps:** 
- Read appropriate reference files based on your use case
- Check API Reference for complete property/method documentation
- Review Accessibility guide for WCAG compliance
- Explore code examples for your specific feature

