# Menu Item Customization in React Menu Component

## Table of Contents
- [Adding Menu Items](#adding-menu-items)
- [Removing Menu Items](#removing-menu-items)
- [Enabling and Disabling Items](#enabling-and-disabling-items)
- [Showing and Hiding Items](#showing-and-hiding-items)
- [Custom HTML Attributes](#custom-html-attributes)
- [Separator Support](#separator-support)
- [Practical Workflows](#practical-workflows)

## Adding Menu Items

Menu items can be dynamically added to the menu using the `insertBefore()` and `insertAfter()` methods on the MenuComponent instance.

### insertAfter() Method

Adds items after a specified menu item:

```jsx
import { MenuComponent } from '@syncfusion/ej2-react-navigations';
import * as React from 'react';

function App() {
  let menuObj: MenuComponent;

  const addAfterFile = () => {
    // Insert new item after 'File' menu
    menuObj.insertAfter(
      [{ text: 'Recent Files' }],  // Items to add
      'File'                        // Target item to insert after
    );
  };

  const menuItems = [
    {
      text: 'File',
      items: [
        { text: 'New' },
        { text: 'Open' },
        { text: 'Save' }
      ]
    },
    {
      text: 'Edit',
      items: [
        { text: 'Cut' },
        { text: 'Copy' }
      ]
    }
  ];

  return (
    <div>
      <button onClick={addAfterFile}>Add Recent Files</button>
      <MenuComponent 
        ref={(menu) => (menuObj = menu)}
        items={menuItems}
      ></MenuComponent>
    </div>
  );
}

export default App;
```

### insertBefore() Method

Adds items before a specified menu item:

```jsx
const addBeforeExit = () => {
  // Insert separator and settings before Exit
  menuObj.insertBefore(
    [
      { separator: true },
      { text: 'Settings' }
    ],
    'Exit'  // Target item to insert before
  );
};
```

### Adding Items to Sub-Menus

Insert items into nested sub-menus:

```jsx
const addToSubmenu = () => {
  // Add item to File sub-menu before 'Exit'
  menuObj.insertBefore(
    [{ text: 'Print' }],
    'Exit'  // Item in sub-menu
  );
};

// For deeply nested items, use the full path or item reference
const dataSource = [
  {
    continent: 'Asia',
    countries: [
      { country: 'China' },
      { country: 'India' },
      { country: 'Japan' }
    ]
  },
  {
    continent: 'North America',
    countries: [
      { country: 'Canada' },
      { country: 'USA' }
    ]
  }
];

const addCountry = () => {
  // Add country after India
  menuObj.insertAfter(
    [{ country: 'Thailand' }],
    'India'
  );
};
```

### Practical Example: Dynamic Menu Building

```jsx
function App() {
  let menuObj: MenuComponent;
  const [itemCount, setItemCount] = React.useState(3);

  const addDynamicItem = () => {
    const newItem = { text: `New Item ${itemCount}` };
    menuObj.insertAfter([newItem], 'Edit');
    setItemCount(itemCount + 1);
  };

  const baseItems = [
    { text: 'File' },
    { text: 'Edit' },
    { text: 'View' }
  ];

  return (
    <div>
      <button onClick={addDynamicItem}>Add Item</button>
      <MenuComponent 
        ref={(menu) => (menuObj = menu)}
        items={baseItems}
      ></MenuComponent>
    </div>
  );
}
```

## Removing Menu Items

Menu items can be removed using the `removeItems()` method:

```jsx
const removeItems = () => {
  // Remove single item by text
  menuObj.removeItems(['Exit']);
  
  // Remove multiple items
  menuObj.removeItems(['South America', 'Mexico']);
};
```

### Complete Removal Example

```jsx
import { MenuComponent } from '@syncfusion/ej2-react-navigations';
import * as React from 'react';

function App() {
  let menuObj: MenuComponent;

  const dataSource = [
    {
      continent: 'Asia',
      countries: [
        { country: 'China' },
        { country: 'India' },
        { country: 'Japan' }
      ]
    },
    {
      continent: 'North America',
      countries: [
        { country: 'Canada' },
        { country: 'Mexico' },
        { country: 'USA' }
      ]
    },
    {
      continent: 'South America',
      countries: [
        { country: 'Brazil' },
        { country: 'Colombia' }
      ]
    }
  ];

  const removeCountries = () => {
    // Remove specific items
    menuObj.removeItems(['South America', 'Mexico']);
  };

  const fields = {
    text: 'continent',
    children: 'countries'
  };

  return (
    <div>
      <button onClick={removeCountries}>Remove Countries</button>
      <MenuComponent 
        ref={(menu) => (menuObj = menu)}
        items={dataSource}
        fields={fields}
      ></MenuComponent>
    </div>
  );
}

export default App;
```

## Enabling and Disabling Items

Menu items can be enabled or disabled based on conditions or user interactions.

### Disable Items Initially

```jsx
const menuItems = [
  {
    text: 'File',
    items: [
      { text: 'New' },
      { text: 'Open' },
      { text: 'Save', disabled: true },  // Disabled by default
      { text: 'Close', disabled: true }
    ]
  }
];

<MenuComponent items={menuItems}></MenuComponent>
```

### Dynamic Enabling/Disabling

```jsx
const handleItemSelect = (args) => {
  // Disable certain items based on selection
  if (args.item.text === 'New') {
    // Find and disable 'Close' item
    const closeItem = menuObj.element.querySelector('[data-text="Close"]');
    if (closeItem) {
      closeItem.disabled = true;
    }
  }
};
```

### Conditional Disabling with beforeOpen Event

```jsx
function App() {
  let menuObj: MenuComponent;

  const handleBeforeOpen = (args) => {
    // Check parent item to decide on child item state
    if (args.parentItem?.text === 'File') {
      // Disable Save if no document is open
      if (!hasOpenDocument) {
        args.items.forEach(item => {
          if (item.text === 'Save') {
            item.disabled = true;
          }
        });
      }
    }
  };

  const menuItems = [
    {
      text: 'File',
      items: [
        { text: 'New' },
        { text: 'Save' },
        { text: 'Close' }
      ]
    }
  ];

  return (
    <MenuComponent 
      ref={(menu) => (menuObj = menu)}
      items={menuItems}
      beforeOpen={handleBeforeOpen}
    ></MenuComponent>
  );
}
```

## Showing and Hiding Items

Control item visibility without removing them from the menu:

```jsx
// Hide items initially
const menuItems = [
  {
    text: 'File',
    items: [
      { text: 'New' },
      { text: 'Open' },
      { text: 'Advanced Options', hidden: true }  // Hidden initially
    ]
  }
];

// Show/hide items dynamically
const toggleAdvancedOptions = () => {
  const items = menuObj.items;
  items.forEach(item => {
    if (item.text === 'File') {
      item.items.forEach(subItem => {
        if (subItem.text === 'Advanced Options') {
          subItem.hidden = !subItem.hidden;
        }
      });
    }
  });
  menuObj.items = items;  // Refresh menu
};
```

### Practical Example: Role-Based Menu Visibility

```jsx
function App() {
  let menuObj: MenuComponent;
  const [userRole, setUserRole] = React.useState('viewer');

  const baseItems = [
    {
      text: 'File',
      items: [
        { text: 'New', hidden: userRole === 'viewer' },
        { text: 'Open' },
        { text: 'Delete', hidden: userRole !== 'admin' }
      ]
    },
    {
      text: 'Tools',
      items: [
        { text: 'Settings', hidden: userRole !== 'admin' },
        { text: 'Preferences' }
      ]
    }
  ];

  const promoteUser = () => {
    setUserRole('admin');
  };

  return (
    <div>
      <button onClick={promoteUser}>Promote to Admin</button>
      <MenuComponent 
        ref={(menu) => (menuObj = menu)}
        items={baseItems}
      ></MenuComponent>
    </div>
  );
}
```

## Custom HTML Attributes

Add custom HTML attributes to menu items for additional functionality or styling:

```jsx
const menuItems = [
  {
    text: 'File',
    htmlAttributes: {
      'data-role': 'menu-category',
      'aria-label': 'File operations',
      'data-toggle': 'tooltip',
      'title': 'File management options'
    },
    items: [
      {
        text: 'Save',
        htmlAttributes: {
          'data-action': 'save-document',
          'class': 'premium-feature',
          'aria-describedby': 'save-help'
        }
      }
    ]
  }
];

<MenuComponent items={menuItems}></MenuComponent>
```

### Custom Attributes for Styling

```jsx
const menuItems = [
  {
    text: 'Export',
    htmlAttributes: { 'class': 'export-item highlight' },
    items: [
      { text: 'PDF', htmlAttributes: { 'class': 'format-pdf' } },
      { text: 'Excel', htmlAttributes: { 'class': 'format-excel' } },
      { text: 'CSV', htmlAttributes: { 'class': 'format-csv' } }
    ]
  }
];

// CSS
const style = `
  .export-item {
    font-weight: bold;
  }
  .export-item.highlight {
    color: #0066cc;
  }
  .format-pdf { color: #d41c1f; }
  .format-excel { color: #1fa02b; }
  .format-csv { color: #737373; }
`;
```

## Separator Support

Use separators to visually group related menu items:

```jsx
const menuItems = [
  {
    text: 'File',
    items: [
      { text: 'New' },
      { text: 'Open' },
      { text: 'Save' },
      { separator: true },  // Visual separator
      { text: 'Recent Files' },
      { separator: true },
      { text: 'Exit' }
    ]
  },
  {
    text: 'Edit',
    items: [
      { text: 'Cut' },
      { text: 'Copy' },
      { text: 'Paste' },
      { separator: true },
      { text: 'Select All' }
    ]
  }
];

<MenuComponent items={menuItems}></MenuComponent>
```

## Practical Workflows

### Workflow 1: Context Menu with Dynamic Items

```jsx
function ContextMenu() {
  let menuObj: MenuComponent;
  const [selectedItem, setSelectedItem] = React.useState(null);

  const showContextMenu = (e) => {
    const items = [
      { text: `Selected: ${selectedItem}` },
      { separator: true },
      { text: 'Edit' },
      { text: 'Delete' },
      { text: 'Duplicate' }
    ];
    setContextMenuItems(items);
  };

  return (
    <div onContextMenu={showContextMenu}>
      <MenuComponent 
        ref={(menu) => (menuObj = menu)}
        items={menuItems}
      ></MenuComponent>
    </div>
  );
}
```

### Workflow 2: Dependent Menu Items

```jsx
function DependentMenu() {
  let menuObj: MenuComponent;
  const [selectedFile, setSelectedFile] = React.useState(null);

  const handleBeforeOpen = (args) => {
    if (args.item.text === 'Edit' && !selectedFile) {
      args.item.disabled = true;  // Disable Edit if no file selected
    }
  };

  return (
    <MenuComponent 
      items={menuItems}
      beforeOpen={handleBeforeOpen}
    ></MenuComponent>
  );
}
```

### Workflow 3: Adding Submenu Items Dynamically

```jsx
const addSubmenuItem = (parentText, newItem) => {
  const items = menuObj.items;
  
  // Find parent and add child
  const parent = items.find(item => item.text === parentText);
  if (parent) {
    if (!parent.items) {
      parent.items = [];
    }
    parent.items.push(newItem);
    menuObj.items = items;  // Refresh
  }
};

// Usage
addSubmenuItem('File', { text: 'Import' });
```

## Related Topics
- [Events and Interactions](events-and-interactions.md) - Respond to item changes
- [Getting Started](getting-started.md) - Basic menu setup
