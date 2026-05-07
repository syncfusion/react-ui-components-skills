# Getting Started with ListView

## Table of Contents
- [Installation](#installation)
- [CSS Themes](#css-themes)
- [Basic Setup](#basic-setup)
- [First Component](#first-component)
- [TypeScript Support](#typescript-support)
- [Project Structure](#project-structure)

## Installation

### Prerequisites
- React 16.8+ (for hooks support)
- Node.js 12+
- npm or yarn

### Step 1: Install Required Packages

```bash
# Install ListView and dependencies
npm install @syncfusion/ej2-react-lists @syncfusion/ej2-base @syncfusion/ej2-data

# Or with yarn
yarn add @syncfusion/ej2-react-lists @syncfusion/ej2-base @syncfusion/ej2-data
```

### Step 2: Verify Installation

```bash
# Check installed versions
npm list @syncfusion/ej2-react-lists
npm list @syncfusion/ej2-base
npm list @syncfusion/ej2-data
```

### Step 3: License Key (Optional)

For production use, register your Syncfusion license key:

```tsx
import { registerLicense } from '@syncfusion/ej2-base';

// Register once in your app entry point
registerLicense('YOUR_SYNCFUSION_LICENSE_KEY');
```

## CSS Themes

### Available Built-in Themes

1. **Material** (Default)
```tsx
import '@syncfusion/ej2-react-lists/styles/material.css';
```

2. **Bootstrap**
```tsx
import '@syncfusion/ej2-react-lists/styles/bootstrap.css';
```

3. **Fabric (Office)**
```tsx
import '@syncfusion/ej2-react-lists/styles/fabric.css';
```

4. **Tailwind**
```tsx
import '@syncfusion/ej2-react-lists/styles/tailwind.css';
```

5. **High Contrast**
```tsx
import '@syncfusion/ej2-react-lists/styles/highcontrast.css';
```

### Bootstrap Dark Theme

```tsx
import '@syncfusion/ej2-react-lists/styles/bootstrap-dark.css';
```

### Applying a Theme

Choose ONE theme and import in your main component or App.tsx:

```tsx
// App.tsx
import React from 'react';
import { ListViewComponent } from '@syncfusion/ej2-react-lists';
import '@syncfusion/ej2-react-lists/styles/material.css'; // Choose one theme

function App() {
  return (
    <div>
      <ListViewComponent id="list" dataSource={['Item 1', 'Item 2']} />
    </div>
  );
}

export default App;
```

## Basic Setup

### Minimal Example

```tsx
import React from 'react';
import { ListViewComponent } from '@syncfusion/ej2-react-lists';
import '@syncfusion/ej2-react-lists/styles/material.css';

export function BasicList() {
  // Simple string array
  const items = ['Apple', 'Banana', 'Orange', 'Mango'];

  return (
    <ListViewComponent
      id="simple-list"
      dataSource={items}
    />
  );
}
```

### With Object Data

```tsx
import React from 'react';
import { ListViewComponent } from '@syncfusion/ej2-react-lists';
import '@syncfusion/ej2-react-lists/styles/material.css';

export function ObjectList() {
  const items = [
    { id: '1', name: 'Apple', category: 'Fruit' },
    { id: '2', name: 'Carrot', category: 'Vegetable' },
    { id: '3', name: 'Banana', category: 'Fruit' }
  ];

  return (
    <ListViewComponent
      id="object-list"
      dataSource={items}
      fields={{
        id: 'id',
        text: 'name'
      }}
    />
  );
}
```

## First Component

### Complete Working Example

```tsx
import React, { useState, useRef } from 'react';
import { ListViewComponent } from '@syncfusion/ej2-react-lists';
import '@syncfusion/ej2-react-lists/styles/material.css';

export function FirstListView() {
  const listViewRef = useRef<ListViewComponent>(null);
  const [selectedItem, setSelectedItem] = useState<any>(null);

  // Sample data
  const data = [
    { id: '1', text: 'Inbox', icon: 'e-icons e-mail' },
    { id: '2', text: 'Sent', icon: 'e-icons e-send' },
    { id: '3', text: 'Drafts', icon: 'e-icons e-edit' },
    { id: '4', text: 'Trash', icon: 'e-icons e-delete' }
  ];

  // Handle selection
  const handleSelect = (args: any) => {
    setSelectedItem(args);
    console.log('Selected:', args.text);
  };

  // Add new item
  const handleAddItem = () => {
    const newItem = { 
      id: Date.now().toString(), 
      text: `New Item ${Date.now()}`,
      icon: 'e-icons e-new'
    };
    if (listViewRef.current) {
      listViewRef.current.addItem([newItem]);
    }
  };

  return (
    <div style={{ padding: '20px' }}>
      <h2>Email Folders</h2>
      
      <button onClick={handleAddItem} style={{ marginBottom: '10px' }}>
        Add Folder
      </button>

      <ListViewComponent
        ref={listViewRef}
        id="email-list"
        dataSource={data}
        fields={{
          id: 'id',
          text: 'text',
          iconCss: 'icon'
        }}
        select={handleSelect}
        showIcon={true}
        height="300px"
      />

      {selectedItem && (
        <div style={{ marginTop: '20px', padding: '10px', border: '1px solid #ccc' }}>
          <p>Selected: <strong>{selectedItem.text}</strong></p>
          <p>ID: {selectedItem.id}</p>
        </div>
      )}
    </div>
  );
}
```

## TypeScript Support

### Type Definitions

ListView includes TypeScript type definitions. No additional setup needed.

```tsx
import React, { useRef } from 'react';
import { ListViewComponent, ListViewComponent as ListView } from '@syncfusion/ej2-react-lists';
import { SelectEventArgs, SelectedItem } from '@syncfusion/ej2-react-lists';

interface DataItem {
  id: string;
  text: string;
  category?: string;
  icon?: string;
}

export function TypedListView() {
  const listViewRef = useRef<ListView>(null);
  const data: DataItem[] = [
    { id: '1', text: 'Item 1', category: 'Category A' },
    { id: '2', text: 'Item 2', category: 'Category B' }
  ];

  const handleSelect = (args: SelectEventArgs): void => {
    console.log('Selected:', args.text);
  };

  return (
    <ListViewComponent
      ref={listViewRef}
      dataSource={data}
      fields={{ id: 'id', text: 'text', groupBy: 'category' }}
      select={handleSelect}
    />
  );
}
```

### Common Type Definitions

```tsx
// Data item fields
interface FieldSettings {
  id?: string;              // Item identifier
  text?: string;            // Display text
  tooltip?: string;         // Hover tooltip
  child?: string;           // Nested items
  icon?: string;            // Icon CSS class
  image?: string;           // Image URL
  isChecked?: string;       // Checkbox state field
  isVisible?: string;       // Visibility field
  enabled?: string;         // Enabled state field
  groupBy?: string;         // Grouping field
}

// Select event args
interface SelectEventArgs {
  id: string;
  text: string;
  element?: HTMLElement;
  data?: any;
  isChecked?: boolean;
  index?: number;
}

// Selected items
interface SelectedItem {
  id: string;
  text: string;
  element: HTMLElement;
  data: any;
}
```

## Project Structure

### Recommended Project Layout

```
my-react-app/
├── src/
│   ├── components/
│   │   └── ListView/
│   │       ├── BasicList.tsx
│   │       ├── ListWithSelection.tsx
│   │       ├── ListWithTemplates.tsx
│   │       └── ListWithFiltering.tsx
│   ├── styles/
│   │   └── listview.css
│   ├── App.tsx
│   ├── App.css
│   └── index.tsx
├── package.json
└── tsconfig.json
```

### App.tsx Setup

```tsx
import React from 'react';
import '@syncfusion/ej2-react-lists/styles/material.css';
import './App.css';
import { BasicList } from './components/ListView/BasicList';

function App() {
  return (
    <div className="app-container">
      <header>
        <h1>ListView Examples</h1>
      </header>
      <main>
        <BasicList />
      </main>
    </div>
  );
}

export default App;
```

### Custom CSS

```css
/* src/styles/listview.css */

.my-list-container {
  max-width: 500px;
  margin: 20px auto;
  border-radius: 4px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
}

.e-listview .e-list-item {
  padding: 12px 16px;
  border-bottom: 1px solid #f0f0f0;
}

.e-listview .e-list-item:hover {
  background-color: #f5f5f5;
}

.e-listview .e-list-item.e-active {
  background-color: #e3f2fd;
  border-left: 4px solid #2196f3;
}
```

## Common Issues & Solutions

### Issue: Component Not Rendering

**Solution:** Ensure CSS is imported BEFORE using the component

```tsx
// ✅ Correct order
import '@syncfusion/ej2-react-lists/styles/material.css';
import { ListViewComponent } from '@syncfusion/ej2-react-lists';

// ❌ Wrong order
import { ListViewComponent } from '@syncfusion/ej2-react-lists';
import '@syncfusion/ej2-react-lists/styles/material.css';
```

### Issue: TypeScript Errors with Ref

**Solution:** Use proper typing

```tsx
// ✅ Correct
const listViewRef = useRef<ListViewComponent>(null);

// ❌ Wrong
const listViewRef = useRef(null);
```

### Issue: Data Not Displaying

**Solution:** Check fields mapping

```tsx
// If your data has these fields:
const data = [
  { productId: 1, productName: 'Item 1' }
];

// Map them correctly:
<ListViewComponent
  dataSource={data}
  fields={{
    id: 'productId',      // ← Map to your field names
    text: 'productName'    // ← Not the default 'id' or 'text'
  }}
/>
```

## Next Steps

- **Selection:** Read [selection-filtering.md](selection-filtering.md) for handling item selection
- **Data Binding:** Read [data-binding-rendering.md](data-binding-rendering.md) for advanced data sources
- **Templates:** Read [templating-customization.md](templating-customization.md) for custom item displays
- **Advanced:** Read [advanced-features.md](advanced-features.md) for drag-drop, grouping, etc.

