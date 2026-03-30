---
name: syncfusion-react-list-box
description: Guide for implementing Syncfusion React ListBox component. Use this when working with list selection components, build dropdown-like lists with custom templates, enable multi-selection, implement drag-and-drop functionality, add filtering/searching to lists, create accessibility-compliant list boxes, or need to handle list item templates and styling. Perfect for selection menus, item pickers, dual-list transfers, and filterable item lists.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
  category: "Dropdowns"
---

# Implementing Syncfusion React ListBox

The ListBox component is a versatile React dropdown-alternative that displays items in a scrollable list format. It supports single/multiple selection, custom templates, drag-and-drop, filtering, and grouping, making it ideal for building accessible, feature-rich selection interfaces.

## Documentation Guide

Navigate to specific topics based on your implementation needs:

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- React app setup (Vite/Create React App)
- Installing Syncfusion packages
- CSS imports and theming
- Basic ListBox component creation
- Running the application

### Selection & Events
📄 **Read:** [references/selection.md](references/selection.md)
- Single selection mode
- Multiple selection mode
- Selection events and handlers
- Programmatic selection management
- Working with selected items

### Data Binding & Structure
📄 **Read:** [references/data-binding.md](references/data-binding.md)
- Array and object data binding
- Text and value properties
- Data source configuration
- Grouping data
- Hierarchical data structures

### Custom Templates & Icons
📄 **Read:** [references/icons-and-templates.md](references/icons-and-templates.md)
- Icon rendering in items
- Custom item templates
- HTML content rendering
- Template variables and syntax
- Conditional rendering

### Advanced Features
📄 **Read:** [references/features.md](references/features.md)
- Drag and drop functionality
- Filtering and search
- Sorting and grouping
- Dual ListBox (transfer list)
- Scroller configuration
- Item enable/disable

### Styling & Appearance
📄 **Read:** [references/style-and-appearance.md](references/style-and-appearance.md)
- CSS class customization
- Theme integration
- Styling items and groups
- Responsive design
- Custom CSS variables

### Accessibility
📄 **Read:** [references/accessibility.md](references/accessibility.md)
- WCAG 2.2 compliance
- Keyboard navigation
- ARIA attributes
- Screen reader support
- RTL (right-to-left) support

### How-To Guides
📄 **Read:** [references/how-to-guides.md](references/how-to-guides.md)
- Add items dynamically
- Select items programmatically
- Enable/disable items
- Filter ListBox data
- Enable scroller for long lists
- Form integration & submission

### Dual ListBox (Transfer)
📄 **Read:** [references/dual-list-box.md](references/dual-list-box.md)
- Two-way item transfer between lists
- Toolbar operations (move up/down, transfer)
- Permission and skill assignment patterns
- Custom styling and responsive design
- Capacity limits and validation

### API Reference
📄 **Read:** [references/api.md](references/api.md)
- Complete list of all properties with types and defaults
- All public methods with parameter details and return types
- All events with full event argument interfaces
- Sub-interfaces: `SelectionSettingsModel`, `ToolbarSettingsModel`, `FieldSettingsModel`, `SourceDestinationModel`

---

## Quick Start Example

Basic ListBox with single selection:

```tsx
import { ListBoxComponent } from '@syncfusion/ej2-react-dropdowns';
import './App.css';

function App() {
  const data = [
    { text: 'JavaScript', id: '1' },
    { text: 'TypeScript', id: '2' },
    { text: 'React', id: '3' },
    { text: 'Vue', id: '4' },
    { text: 'Angular', id: '5' }
  ];

  const handleChange = (e) => {
    console.log('Selected:', e.value);
  };

  return (
    <ListBoxComponent 
      dataSource={data}
      fields={{ text: 'text', value: 'id' }}
      selectionSettings={{ mode: 'Single' }}
      change={handleChange}
    />
  );
}

export default App;
```

---

## Common Patterns

### Multiple Selection
```tsx
<ListBoxComponent 
  dataSource={data}
  selectionSettings={{ mode: 'Multiple' }}
/>
```

### Checkbox Selection
> **Requires injecting `CheckBoxSelection` service.**

```tsx
import { ListBoxComponent, SelectionSettingsModel, Inject, CheckBoxSelection } from '@syncfusion/ej2-react-dropdowns';

const selectionSettings: SelectionSettingsModel = { showCheckbox: true };

<ListBoxComponent dataSource={data} selectionSettings={selectionSettings}>
  <Inject services={[CheckBoxSelection]} />
</ListBoxComponent>
```

### With Search/Filter
```tsx
<ListBoxComponent 
  dataSource={data}
  allowFiltering={true}
  filterBarPlaceholder="Search items"
/>
```

### Custom Item Template
```tsx
const itemTemplate = (props) => {
  return (
    <div>
      <span className="icon">{props.icon}</span>
      <span>{props.text}</span>
    </div>
  );
}

<ListBoxComponent 
  dataSource={data}
  itemTemplate={itemTemplate}
/>
```

### Grouping Items
```tsx
<ListBoxComponent 
  dataSource={groupedData}
  fields={{ text: 'text', groupBy: 'category' }}
/>
```

---

## Key Props & Configuration

| Prop | Purpose | Example |
|------|---------|---------|
| `dataSource` | Array of items to display | `[{ text: 'Item', id: '1' }]` |
| `fields` | Maps data properties to display | `{ text: 'name', value: 'id' }` |
| `selectionSettings` | Defines selection mode and checkbox display. For `showCheckbox: true`, inject `CheckBoxSelection` service | `{ mode: 'Multiple' }` / `{ showCheckbox: true }` |
| `allowFiltering` | Enables filter search box | `true` |
| `allowDragAndDrop` | Enables item drag-drop | `true` |
| `itemTemplate` | Custom template for items | Function returning JSX |
| `enabled` | Enables/disables the component | `true` / `false` |

---

## Common Use Cases

1. **Select Framework** - Single selection from framework list with icons
2. **Multi-Select Languages** - Multiple selection with search filter
3. **Skill Picker** - Custom templates with badges and descriptions
4. **Drag-Drop Transfer** - Dual ListBox for moving items between lists
5. **Grouped Categories** - Organizing items by category with group headers
6. **Searchable Item List** - Large list with filter functionality
7. **Accessible Menu** - Full keyboard navigation and screen reader support

---

## Next Steps

1. **Start with Getting Started** for initial setup
2. **Choose your use case** (selection mode, templates, features)
3. **Read relevant reference** for implementation details
4. **Copy code examples** and customize for your needs
5. **Use Accessibility guide** for WCAG compliance

**Need help?** Each reference file contains examples, edge cases, and troubleshooting tips.
