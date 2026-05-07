# Events and Interactions in React Menu Component

## Table of Contents
- [Event System Overview](#event-system-overview)
- [Menu Item Selection Events](#menu-item-selection-events)
- [Open and Close Events](#open-and-close-events)
- [Event Arguments](#event-arguments)
- [Preventing Default Actions](#preventing-default-actions)
- [Common Event Patterns](#common-event-patterns)
- [Practical Workflows](#practical-workflows)

## Event System Overview

The Menu component provides a comprehensive event system for detecting user interactions:

| Event | Trigger | Usage |
|-------|---------|-------|
| `select` | Menu item clicked | Handle item selection |
| `beforeOpen` | Before sub-menu opens | Modify/disable items |
| `beforeClose` | Before sub-menu closes | Cleanup operations |
| `open` | Sub-menu opened | Track menu state |
| `close` | Sub-menu closed | Track menu state |

## Menu Item Selection Events

### The `select` Event

Triggered when a user clicks a menu item:

```jsx
import { MenuComponent, MenuItemModel } from '@syncfusion/ej2-react-navigations';
import * as React from 'react';

function App() {
  const handleMenuSelect = (args) => {
    console.log(`Selected item: ${args.item.text}`);
    console.log(`Parent: ${args.parentItem?.text}`);
    
    // Perform action based on selected item
    if (args.item.text === 'Save') {
      saveDocument();
    } else if (args.item.text === 'Exit') {
      exitApplication();
    }
  };

  const menuItems: MenuItemModel[] = [
    {
      text: 'File',
      items: [
        { text: 'New' },
        { text: 'Open' },
        { text: 'Save' },
        { separator: true },
        { text: 'Exit' }
      ]
    },
    {
      text: 'Edit',
      items: [
        { text: 'Cut' },
        { text: 'Copy' },
        { text: 'Paste' }
      ]
    }
  ];

  return (
    <MenuComponent 
      items={menuItems}
      select={handleMenuSelect}
    ></MenuComponent>
  );
}

export default App;
```

### Getting Selected Item Information

```jsx
const handleMenuSelect = (args) => {
  const selectedItem = args.item;
  
  console.log('Item Properties:');
  console.log(`- Text: ${selectedItem.text}`);
  console.log(`- ID: ${selectedItem.id}`);
  console.log(`- URL: ${selectedItem.url}`);
  console.log(`- Icon: ${selectedItem.iconCss}`);
  console.log(`- Disabled: ${selectedItem.disabled}`);
  console.log(`- HTML Attributes:`, selectedItem.htmlAttributes);
  
  // Parent information (for sub-items)
  console.log(`Parent: ${args.parentItem?.text}`);
};
```

## Open and Close Events

### beforeOpen Event

Triggered before a sub-menu opens. Use to modify items, update data, or prevent opening:

```jsx
const handleBeforeOpen = (args) => {
  console.log(`Opening: ${args.item.text}`);
  
  // Modify items before they display
  if (args.item.text === 'File') {
    // Update menu items dynamically
    args.item.items = getRecentFiles();
  }
  
  // Prevent opening if needed
  if (shouldPreventOpen(args.item)) {
    args.cancel = true;
  }
};

const menuItems = [
  {
    text: 'File',
    items: [
      { text: 'New' },
      { text: 'Open' },
      { text: 'Recent Files' }
    ]
  }
];

<MenuComponent 
  items={menuItems}
  beforeOpen={handleBeforeOpen}
></MenuComponent>
```

### beforeClose Event

Triggered before a sub-menu closes:

```jsx
const handleBeforeClose = (args) => {
  console.log(`Closing: ${args.item.text}`);
  
  // Perform cleanup
  saveMenuState(args.item.text);
  
  // Prevent closing if needed
  if (hasUnsavedChanges()) {
    args.cancel = true;
  }
};

<MenuComponent 
  items={menuItems}
  beforeClose={handleBeforeClose}
></MenuComponent>
```

### open and close Events

Triggered after menu opens/closes:

```jsx
const handleOpen = (args) => {
  console.log(`Opened: ${args.item.text}`);
  // Track analytics, update UI state, etc.
};

const handleClose = (args) => {
  console.log(`Closed: ${args.item.text}`);
  // Cleanup after menu closes
};

<MenuComponent 
  items={menuItems}
  open={handleOpen}
  close={handleClose}
></MenuComponent>
```

## Event Arguments

### MenuEventArgs (for select event)

```jsx
interface MenuEventArgs {
  // Item that was selected
  item: MenuItemModel;
  
  // Parent item (if item is a sub-menu)
  parentItem?: MenuItemModel;
  
  // Event object
  event?: Event;
  
  // Element that was clicked
  element?: HTMLElement;
}
```

### OpenCloseMenuEventArgs

```jsx
interface OpenCloseMenuEventArgs {
  // Item being opened/closed
  item: MenuItemModel;
  
  // Enable/disable default action
  cancel?: boolean;
  
  // Items array being shown
  items?: MenuItemModel[];
  
  // Parent item
  parentItem?: MenuItemModel;
  
  // Event object
  event?: Event;
}
```

## Preventing Default Actions

Cancel events to prevent default behavior:

```jsx
const handleBeforeOpen = (args) => {
  // Prevent opening File menu if user is not logged in
  if (args.item.text === 'File' && !isUserLoggedIn()) {
    args.cancel = true;
    showLoginDialog();
  }
};

const handleBeforeClose = (args) => {
  // Prevent closing if there are unsaved changes
  if (hasUnsavedChanges()) {
    args.cancel = true;
    showSavePrompt();
  }
};
```

## Common Event Patterns

### Pattern 1: Item Selection with Route Navigation

```jsx
import { useNavigate } from 'react-router-dom';
import { MenuComponent } from '@syncfusion/ej2-react-navigations';

function App() {
  const navigate = useNavigate();

  const handleMenuSelect = (args) => {
    const routes: { [key: string]: string } = {
      'Dashboard': '/dashboard',
      'Profile': '/profile',
      'Settings': '/settings'
    };
    
    const route = routes[args.item.text];
    if (route) {
      navigate(route);
    }
  };

  const menuItems = [
    {
      text: 'Navigation',
      items: [
        { text: 'Dashboard' },
        { text: 'Profile' },
        { text: 'Settings' }
      ]
    }
  ];

  return (
    <MenuComponent 
      items={menuItems}
      select={handleMenuSelect}
    ></MenuComponent>
  );
}
```

### Pattern 2: Dynamic Menu Updates on Open

```jsx
const handleBeforeOpen = async (args) => {
  if (args.item.text === 'Recent Files') {
    try {
      // Fetch recent files from API
      const recentFiles = await fetchRecentFiles();
      
      // Update menu items
      args.item.items = recentFiles.map(file => ({
        text: file.name,
        url: file.path
      }));
      
      // Set flag to prevent re-fetching
      args.item.dataLoaded = true;
    } catch (error) {
      console.error('Failed to load recent files:', error);
    }
  }
};
```

### Pattern 3: Conditional Item Disabling

```jsx
const handleBeforeOpen = (args) => {
  // Disable Edit menu if nothing is selected
  if (args.item.text === 'Edit') {
    args.item.items.forEach(item => {
      if (['Cut', 'Copy', 'Delete'].includes(item.text)) {
        item.disabled = !hasSelection();
      }
    });
  }

  // Disable Paste if clipboard is empty
  if (args.item.text === 'Edit') {
    args.item.items.forEach(item => {
      if (item.text === 'Paste') {
        item.disabled = !hasClipboardContent();
      }
    });
  }
};
```

## showItemOnClick Property

By default, sub-menus open on hover. Set `showItemOnClick` to open on click:

```jsx
function App() {
  const menuItems = [
    {
      text: 'File',
      items: [
        { text: 'New' },
        { text: 'Open' },
        { text: 'Save' }
      ]
    }
  ];

  // Open sub-menus on click instead of hover
  return (
    <MenuComponent 
      items={menuItems}
      showItemOnClick={true}
    ></MenuComponent>
  );
}
```

Combined with select event:

```jsx
function App() {
  const handleMenuSelect = (args) => {
    console.log(`Clicked: ${args.item.text}`);
    
    // Perform action immediately
    if (!args.item.items) {
      executeMenuAction(args.item.text);
    }
  };

  return (
    <MenuComponent 
      items={menuItems}
      showItemOnClick={true}
      select={handleMenuSelect}
    ></MenuComponent>
  );
}
```

## Practical Workflows

### Workflow 1: Menu with Action Confirmation

```jsx
function App() {
  let menuObj: MenuComponent;
  const [showConfirm, setShowConfirm] = React.useState(false);
  const [selectedAction, setSelectedAction] = React.useState(null);

  const handleMenuSelect = (args) => {
    // Actions requiring confirmation
    if (['Delete', 'Clear Cache', 'Reset'].includes(args.item.text)) {
      setSelectedAction(args.item.text);
      setShowConfirm(true);
    } else {
      executeAction(args.item.text);
    }
  };

  const confirmAction = () => {
    executeAction(selectedAction);
    setShowConfirm(false);
  };

  const menuItems = [
    {
      text: 'Tools',
      items: [
        { text: 'Delete' },
        { text: 'Clear Cache' },
        { text: 'Reset' }
      ]
    }
  ];

  return (
    <>
      <MenuComponent 
        ref={(menu) => (menuObj = menu)}
        items={menuItems}
        select={handleMenuSelect}
      ></MenuComponent>
      {showConfirm && (
        <div className="confirmation-dialog">
          <p>Confirm: {selectedAction}?</p>
          <button onClick={confirmAction}>Confirm</button>
          <button onClick={() => setShowConfirm(false)}>Cancel</button>
        </div>
      )}
    </>
  );
}
```

### Workflow 2: Tracking Menu Analytics

```jsx
const handleMenuSelect = (args) => {
  // Track user interaction
  trackEvent({
    category: 'Menu',
    action: 'Item Selected',
    label: args.item.text,
    parent: args.parentItem?.text,
    timestamp: new Date().toISOString()
  });
};

const handleBeforeOpen = (args) => {
  // Track menu openings
  trackEvent({
    category: 'Menu',
    action: 'Menu Opened',
    label: args.item.text,
    timestamp: new Date().toISOString()
  });
};
```

### Workflow 3: Context-Aware Menu Behavior

```jsx
const handleBeforeOpen = (args) => {
  const userRole = getCurrentUserRole();
  const context = getCurrentContext();

  // Admin gets more options
  if (args.item.text === 'Tools') {
    if (userRole !== 'admin') {
      args.item.items = args.item.items.filter(
        item => !['User Management', 'System Settings'].includes(item.text)
      );
    }
  }

  // Disable options based on context
  if (context === 'read-only') {
    args.item.items.forEach(item => {
      if (['Save', 'Delete', 'Edit'].includes(item.text)) {
        item.disabled = true;
      }
    });
  }
};
```

## Related Topics
- [Menu Items Customization](menu-items-customization.md) - Modify items
- [Getting Started](getting-started.md) - Basic event setup
