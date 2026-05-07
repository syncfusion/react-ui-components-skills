# Advanced Features

## Table of Contents
- [Grouping and Sorting](#grouping-and-sorting)
- [Nested Lists](#nested-lists)
- [Checkboxes & States](#checkboxes--states)
- [Animations](#animations)
- [Icons and Images](#icons-and-images)
- [Hyperlink Navigation](#hyperlink-navigation)
- [Item Disable/Enable](#item-disableenable)

## Grouping and Sorting

### Group by Field

```tsx
interface GroupedItem {
  id: string;
  text: string;
  category: string;
}

const data: GroupedItem[] = [
  { id: '1', text: 'Apple', category: 'Fruit' },
  { id: '2', text: 'Carrot', category: 'Vegetable' },
  { id: '3', text: 'Banana', category: 'Fruit' },
  { id: '4', text: 'Tomato', category: 'Vegetable' }
];

<ListViewComponent
  dataSource={data}
  fields={{
    id: 'id',
    text: 'text',
    groupBy: 'category'  // ← Groups by category
  }}
/>
```

### Sorting Options

```tsx
// Ascending sort
<ListViewComponent
  dataSource={data}
  sortOrder="Ascending"
/>

// Descending sort
<ListViewComponent
  dataSource={data}
  sortOrder="Descending"
/>

// No sort
<ListViewComponent
  dataSource={data}
  sortOrder="None"
/>
```

### Sort with Field Mapping

```tsx
const data = [
  { id: '1', name: 'Charlie', sortKey: 'c' },
  { id: '2', name: 'Alice', sortKey: 'a' },
  { id: '3', name: 'Bob', sortKey: 'b' }
];

<ListViewComponent
  dataSource={data}
  fields={{
    id: 'id',
    text: 'name',
    sortBy: 'sortKey'  // Sort by sortKey, not name
  }}
  sortOrder="Ascending"
/>
```

## Nested Lists

### Hierarchical Data Structure

```tsx
interface TreeItem {
  id: string;
  text: string;
  child?: TreeItem[];
}

const hierarchyData: TreeItem[] = [
  {
    id: '1',
    text: 'Fruits',
    child: [
      { id: '1-1', text: 'Apple' },
      { id: '1-2', text: 'Banana' },
      { id: '1-3', text: 'Orange' }
    ]
  },
  {
    id: '2',
    text: 'Vegetables',
    child: [
      { id: '2-1', text: 'Carrot' },
      { id: '2-2', text: 'Tomato' },
      { id: '2-3', text: 'Lettuce' }
    ]
  }
];

<ListViewComponent
  dataSource={hierarchyData}
  fields={{
    id: 'id',
    text: 'text',
    child: 'child'  // ← Specify child items field
  }}
/>
```

### Navigation Between Nested Lists

```tsx
import { useRef } from 'react';
import { ListViewComponent } from '@syncfusion/ej2-react-lists';

export function NestedListView() {
  const listViewRef = useRef<ListViewComponent>(null);

  const handleSelect = (args: any) => {
    // When user selects a parent item with children
    if (args.data?.child && args.data.child.length > 0) {
      // Navigate to child list (happens automatically in ListView)
      console.log('Navigated to:', args.text);
    }
  };

  const handleBack = () => {
    // Go back to parent list
    listViewRef.current?.back?.();
  };

  return (
    <div>
      <button onClick={handleBack} style={{ marginBottom: '10px' }}>
        ← Back
      </button>

      <ListViewComponent
        ref={listViewRef}
        dataSource={hierarchyData}
        fields={{
          id: 'id',
          text: 'text',
          child: 'child'
        }}
        select={handleSelect}
      />
    </div>
  );
}
```

### Three-Level Hierarchy

```tsx
const threeLevel: TreeItem[] = [
  {
    id: '1',
    text: 'Electronics',
    child: [
      {
        id: '1-1',
        text: 'Computers',
        child: [
          { id: '1-1-1', text: 'Laptop' },
          { id: '1-1-2', text: 'Desktop' }
        ]
      },
      {
        id: '1-2',
        text: 'Phones',
        child: [
          { id: '1-2-1', text: 'Smartphone' },
          { id: '1-2-2', text: 'Tablet' }
        ]
      }
    ]
  }
];
```

## Checkboxes & States

### Checkbox States

```tsx
interface CheckableItem {
  id: string;
  text: string;
  checked?: boolean;
}

const data: CheckableItem[] = [
  { id: '1', text: 'Item 1', checked: true },
  { id: '2', text: 'Item 2', checked: false },
  { id: '3', text: 'Item 3', checked: true }
];

<ListViewComponent
  dataSource={data}
  fields={{
    id: 'id',
    text: 'text',
    isChecked: 'checked'
  }}
  showCheckBox={true}
/>
```

### Manage Checkbox States

```tsx
import { useRef } from 'react';

export function CheckboxManagementExample() {
  const listViewRef = useRef<ListViewComponent>(null);
  const [items, setItems] = useState(data);

  const handleCheckAll = () => {
    listViewRef.current?.checkAllItems?.();
    const updated = items.map(item => ({ ...item, checked: true }));
    setItems(updated);
  };

  const handleUncheckAll = () => {
    listViewRef.current?.uncheckAllItems?.();
    const updated = items.map(item => ({ ...item, checked: false }));
    setItems(updated);
  };

  const getCheckedItems = () => {
    const selected = listViewRef.current?.getSelectedItems?.();
    console.log('Checked items:', selected);
  };

  return (
    <div>
      <button onClick={handleCheckAll}>Check All</button>
      <button onClick={handleUncheckAll}>Uncheck All</button>
      <button onClick={getCheckedItems}>Get Checked</button>

      <ListViewComponent
        ref={listViewRef}
        dataSource={items}
        showCheckBox={true}
        checkBoxPosition="Left"
      />
    </div>
  );
}
```

## Animations

### Apply Animations

```tsx
import { ListViewComponent } from '@syncfusion/ej2-react-lists';

export function AnimatedListView() {
  const data = [
    { id: '1', text: 'Item 1' },
    { id: '2', text: 'Item 2' }
  ];

  return (
    <ListViewComponent
      dataSource={data}
      animation={{
        effect: 'Zoom',      // Zoom, FadeIn, SlideLeft, SlideRight, etc.
        duration: 500,       // milliseconds
        easing: 'ease-in-out'
      }}
    />
  );
}
```

### Animation Options

```tsx
// Available effects:
const effects = [
  'None',
  'SlideLeft',
  'SlideRight',
  'SlideUp',
  'SlideDown',
  'FadeIn',
  'Zoom',
  'Expand'
];

// Easing options:
const easings = [
  'ease',
  'ease-in',
  'ease-out',
  'ease-in-out',
  'linear'
];

// Example with different effect
<ListViewComponent
  animation={{
    effect: 'SlideDown',
    duration: 400,
    easing: 'ease-out'
  }}
/>
```

## Icons and Images

### Show Icons

```tsx
interface IconItem {
  id: string;
  text: string;
  icon: string;  // CSS class name
}

const data: IconItem[] = [
  { id: '1', text: 'Inbox', icon: 'e-icons e-mail' },
  { id: '2', text: 'Sent', icon: 'e-icons e-send' },
  { id: '3', text: 'Drafts', icon: 'e-icons e-edit' },
  { id: '4', text: 'Trash', icon: 'e-icons e-delete' }
];

<ListViewComponent
  dataSource={data}
  fields={{
    id: 'id',
    text: 'text',
    iconCss: 'icon'
  }}
  showIcon={true}
/>
```

### Display Images

```tsx
interface ImageItem {
  id: string;
  text: string;
  image: string;  // URL to image
}

const data: ImageItem[] = [
  { id: '1', text: 'Profile 1', image: '/images/profile1.jpg' },
  { id: '2', text: 'Profile 2', image: '/images/profile2.jpg' }
];

<ListViewComponent
  dataSource={data}
  fields={{
    id: 'id',
    text: 'text',
    image: 'image'
  }}
/>
```

### Custom Icon Template

```tsx
const iconTemplate = (props: any) => (
  <div style={{ display: 'flex', alignItems: 'center' }}>
    <span style={{
      width: '30px',
      height: '30px',
      backgroundColor: props.color,
      borderRadius: '50%',
      display: 'flex',
      alignItems: 'center',
      justifyContent: 'center',
      color: 'white',
      marginRight: '10px',
      fontWeight: 'bold'
    }}>
      {props.initials}
    </span>
    <span>{props.text}</span>
  </div>
);

const data = [
  { id: '1', text: 'Alice Johnson', initials: 'AJ', color: '#2196F3' },
  { id: '2', text: 'Bob Smith', initials: 'BS', color: '#4CAF50' }
];

<ListViewComponent
  dataSource={data}
  template={iconTemplate}
/>
```

## Item Disable/Enable

### Disable Specific Items

```tsx
interface DisableableItem {
  id: string;
  text: string;
  enabled?: boolean;
}

const data: DisableableItem[] = [
  { id: '1', text: 'Item 1', enabled: true },
  { id: '2', text: 'Item 2 (Disabled)', enabled: false },
  { id: '3', text: 'Item 3', enabled: true }
];

<ListViewComponent
  dataSource={data}
  fields={{
    id: 'id',
    text: 'text',
    enabled: 'enabled'
  }}
/>
```

### Enable/Disable Programmatically

```tsx
const handleDisableItem = (itemId: string) => {
  const item = { id: itemId };
  listViewRef.current?.disableItem(item);
};

const handleEnableItem = (itemId: string) => {
  const item = { id: itemId };
  listViewRef.current?.enableItem(item);
};
```

### Hide/Show Items

```tsx
interface VisibleItem {
  id: string;
  text: string;
  visible?: boolean;
}

const data: VisibleItem[] = [
  { id: '1', text: 'Visible Item', visible: true },
  { id: '2', text: 'Hidden Item', visible: false }
];

<ListViewComponent
  dataSource={data}
  fields={{
    id: 'id',
    text: 'text',
    isVisible: 'visible'
  }}
/>
```

### Toggle Visibility

```tsx
const handleHideItem = (itemId: string) => {
  const item = { id: itemId };
  listViewRef.current?.hideItem(item);
};

const handleShowItem = (itemId: string) => {
  const item = { id: itemId };
  listViewRef.current?.showItem(item);
};
```

## Complete Advanced Example

```tsx
import { useRef, useState } from 'react';
import { ListViewComponent } from '@syncfusion/ej2-react-lists';

interface AdvancedItem {
  id: string;
  text: string;
  category: string;
  icon: string;
  enabled: boolean;
  checked?: boolean;
}

export function CompleteAdvancedListView() {
  const listViewRef = useRef<ListViewComponent>(null);
  const [view, setView] = useState('grid');

  const data: AdvancedItem[] = [
    { id: '1', text: 'High Priority', category: 'Work', icon: 'e-icons e-flag', enabled: true, checked: false },
    { id: '2', text: 'Meeting Notes', category: 'Work', icon: 'e-icons e-notes', enabled: true, checked: true },
    { id: '3', text: 'Vacation Plans', category: 'Personal', icon: 'e-icons e-calendar', enabled: false, checked: false }
  ];

  const itemTemplate = (props: AdvancedItem) => (
    <div style={{
      display: 'flex',
      alignItems: 'center',
      padding: '10px',
      opacity: props.enabled ? 1 : 0.5,
      pointerEvents: props.enabled ? 'auto' : 'none'
    }}>
      <i className={props.icon} style={{ marginRight: '10px', color: '#2196F3' }}></i>
      <div style={{ flex: 1 }}>
        <div>{props.text}</div>
        <div style={{ fontSize: '12px', color: '#999' }}>{props.category}</div>
      </div>
      {props.checked && <span>✓</span>}
    </div>
  );

  return (
    <div>
      <div style={{ marginBottom: '15px' }}>
        <button onClick={() => listViewRef.current?.checkAllItems?.()}>
          Check All
        </button>
        <button onClick={() => listViewRef.current?.uncheckAllItems?.()}>
          Uncheck All
        </button>
      </div>

      <ListViewComponent
        ref={listViewRef}
        dataSource={data}
        fields={{
          id: 'id',
          text: 'text',
          groupBy: 'category',
          iconCss: 'icon',
          enabled: 'enabled',
          isChecked: 'checked'
        }}
        template={itemTemplate}
        showCheckBox={true}
        sortOrder="Ascending"
        animation={{
          effect: 'Zoom',
          duration: 300,
          easing: 'ease-in-out'
        }}
        height="400px"
      />
    </div>
  );
}
```

