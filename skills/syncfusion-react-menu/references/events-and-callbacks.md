---
title: Events and Callbacks
parent_skill: Implementing Menu
description: Complete reference for all MenuComponent events and event arguments
---

# Events and Callbacks

## Table of Contents
1. [Overview](#overview)
2. [Select Event](#select-event)
3. [Open/Close Events](#openclose-events)
4. [Before Events](#before-events)
5. [Render Events](#render-events)
6. [Lifecycle Events](#lifecycle-events)
7. [Event Argument Types](#event-argument-types)
8. [Complete Examples](#complete-examples)

## Overview

MenuComponent provides comprehensive event handling for user interactions and component lifecycle. Events are triggered at various points:
- User interactions (clicking, hovering)
- Menu opening/closing
- Item rendering
- Component initialization

## Select Event

### select
**Event Name:** `select`  
**Type:** `EventEmitter<MenuEventArgs>`  
**Triggered:** When a menu item is clicked

```jsx
import { MenuComponent, MenuItemModel } from '@syncfusion/ej2-react-navigations';

function App() {
  const items: MenuItemModel[] = [
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
        { text: 'Copy' },
        { text: 'Paste' }
      ]
    }
  ];

  const handleSelect = (args) => {
    console.log('Selected Item:', args.item.text);
    console.log('Event Details:', args.event);
  };

  return (
    <MenuComponent 
      items={items}
      select={handleSelect}
    />
  );
}

export default App;
```

### MenuEventArgs Structure

```typescript
interface MenuEventArgs {
  // The menu item that was selected
  item: MenuItemModel;
  
  // Original DOM event (MouseEvent or KeyboardEvent)
  event: Event;
  
  // Unique identifier for the menu
  element: HTMLElement;
  
  // Parent menu item (if item is in a sub-menu)
  parentItem?: MenuItemModel;
  
  // Current submenu's parent menu element
  target?: HTMLElement;
}
```

## Open/Close Events

### onOpen
**Event Name:** `onOpen`  
**Type:** `EventEmitter<OpenCloseMenuEventArgs>`  
**Triggered:** After a sub-menu has opened

```jsx
function App() {
  const items = [
    {
      text: 'File',
      items: [
        { text: 'New' },
        { text: 'Open' }
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

  const handleOpen = (args) => {
    console.log('Sub-menu opened');
    console.log('Parent item:', args.parentItem?.text);
    console.log('Event type:', args.event);
  };

  return (
    <MenuComponent 
      items={items}
      onOpen={handleOpen}
    />
  );
}

export default App;
```

### onClose
**Event Name:** `onClose`  
**Type:** `EventEmitter<OpenCloseMenuEventArgs>`  
**Triggered:** After a sub-menu has closed

```jsx
function App() {
  const items = [
    {
      text: 'File',
      items: [
        { text: 'New' },
        { text: 'Open' }
      ]
    }
  ];

  const handleClose = (args) => {
    console.log('Sub-menu closed');
    console.log('Parent item:', args.parentItem?.text);
    // Clean up or log analytics
  };

  return (
    <MenuComponent 
      items={items}
      onClose={handleClose}
    />
  );
}

export default App;
```

### OpenCloseMenuEventArgs Structure

```typescript
interface OpenCloseMenuEventArgs {
  // The item whose sub-menu opened/closed
  parentItem: MenuItemModel;
  
  // The submenu element that opened/closed
  element: HTMLElement;
  
  // Original event (MouseEvent, KeyboardEvent, etc.)
  event?: Event;
  
  // Custom data if needed
  [key: string]: any;
}
```

## Before Events

### beforeOpen
**Event Name:** `beforeOpen`  
**Type:** `EventEmitter<BeforeOpenCloseMenuEventArgs>`  
**Triggered:** Before a sub-menu opens (cancellable)

```jsx
function App() {
  const items = [
    {
      text: 'File',
      items: [
        { text: 'New' },
        { text: 'Open' }
      ]
    },
    {
      text: 'Disabled Menu',
      items: [
        { text: 'Item 1' }
      ]
    }
  ];

  const handleBeforeOpen = (args) => {
    // Prevent opening if parent is "Disabled Menu"
    if (args.parentItem.text === 'Disabled Menu') {
      args.cancel = true; // Cancel the open action
      console.log('Opening disabled menu prevented');
    }
  };

  return (
    <MenuComponent 
      items={items}
      beforeOpen={handleBeforeOpen}
    />
  );
}

export default App;
```

### beforeClose
**Event Name:** `beforeClose`  
**Type:** `EventEmitter<BeforeOpenCloseMenuEventArgs>`  
**Triggered:** Before a sub-menu closes (cancellable)

```jsx
function App() {
  const items = [
    {
      text: 'Tools',
      items: [
        { text: 'Settings' },
        { text: 'Preferences' }
      ]
    }
  ];

  const handleBeforeClose = (args) => {
    // Prevent closing if a specific item is selected
    console.log('About to close sub-menu');
    // You could prevent closing based on conditions
    // args.cancel = true;
  };

  return (
    <MenuComponent 
      items={items}
      beforeClose={handleBeforeClose}
    />
  );
}

export default App;
```

### BeforeOpenCloseMenuEventArgs Structure

```typescript
interface BeforeOpenCloseMenuEventArgs {
  // The item whose sub-menu is about to open/close
  parentItem: MenuItemModel;
  
  // The submenu element
  element: HTMLElement;
  
  // Original event
  event?: Event;
  
  // Set to true to cancel the operation
  cancel: boolean;
}
```

## Render Events

### beforeItemRender
**Event Name:** `beforeItemRender`  
**Type:** `EventEmitter<BeforeItemRenderEventArgs>`  
**Triggered:** Before each menu item is rendered (great for customization)

```jsx
function App() {
  const items = [
    { id: 'file', text: 'File' },
    { id: 'edit', text: 'Edit', disabled: true },
    { id: 'help', text: 'Help' }
  ];

  const handleBeforeItemRender = (args) => {
    // Customize items before rendering
    if (args.item.text === 'File') {
      // Add custom class
      if (!args.element.classList.contains('file-item')) {
        args.element.classList.add('file-item');
      }
    }
    
    // Add icons dynamically
    if (args.item.text === 'Edit') {
      args.element.style.opacity = '0.5';
    }
  };

  return (
    <MenuComponent 
      items={items}
      beforeItemRender={handleBeforeItemRender}
    />
  );
}

export default App;
```

### BeforeItemRenderEventArgs Structure

```typescript
interface BeforeItemRenderEventArgs {
  // The menu item about to be rendered
  item: MenuItemModel;
  
  // The DOM element for this item
  element: HTMLElement;
  
  // Whether item is in a sub-menu
  isSubMenu: boolean;
  
  // Parent element
  parentElement: HTMLElement;
}
```

## Lifecycle Events

### created
**Event Name:** `created`  
**Type:** `EventEmitter<Object>`  
**Triggered:** When the component is initialized and rendered

```jsx
function App() {
  const items = [
    { text: 'File' },
    { text: 'Edit' },
    { text: 'Help' }
  ];

  const handleCreated = () => {
    console.log('Menu component created and ready');
    // Initialize tooltips, analytics tracking, etc.
  };

  return (
    <MenuComponent 
      items={items}
      created={handleCreated}
    />
  );
}

export default App;
```

## Event Argument Types

### Complete Reference

```typescript
// MenuEventArgs - for select event
{
  item: MenuItemModel;
  event: MouseEvent | KeyboardEvent;
  element: HTMLElement;
  parentItem?: MenuItemModel;
  target?: HTMLElement;
}

// OpenCloseMenuEventArgs - for onOpen, onClose
{
  parentItem: MenuItemModel;
  element: HTMLElement;
  event?: Event;
}

// BeforeOpenCloseMenuEventArgs - for beforeOpen, beforeClose
{
  parentItem: MenuItemModel;
  element: HTMLElement;
  event?: Event;
  cancel: boolean;
}

// BeforeItemRenderEventArgs - for beforeItemRender
{
  item: MenuItemModel;
  element: HTMLElement;
  isSubMenu: boolean;
  parentElement: HTMLElement;
}
```

## Complete Examples

### Example 1: Comprehensive Event Handling

```jsx
import { MenuComponent } from '@syncfusion/ej2-react-navigations';
import { useState } from 'react';

function ComprehensiveEventExample() {
  const [eventLog, setEventLog] = useState([]);

  const items = [
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
        { text: 'Copy' },
        { text: 'Paste' }
      ]
    }
  ];

  const addLog = (message) => {
    setEventLog(prev => [...prev, `[${new Date().toLocaleTimeString()}] ${message}`]);
  };

  const handleSelect = (args) => {
    addLog(`✓ Selected: ${args.item.text}`);
  };

  const handleBeforeOpen = (args) => {
    addLog(`→ Opening sub-menu for: ${args.parentItem.text}`);
  };

  const handleOnOpen = (args) => {
    addLog(`◆ Opened: ${args.parentItem.text}`);
  };

  const handleBeforeClose = (args) => {
    addLog(`← Closing sub-menu for: ${args.parentItem.text}`);
  };

  const handleOnClose = (args) => {
    addLog(`◇ Closed: ${args.parentItem.text}`);
  };

  const handleCreated = () => {
    addLog('★ Menu component created');
  };

  return (
    <div className="event-demo">
      <h2>Event Handling Demo</h2>
      
      <div className="menu-container">
        <MenuComponent
          items={items}
          select={handleSelect}
          beforeOpen={handleBeforeOpen}
          onOpen={handleOnOpen}
          beforeClose={handleBeforeClose}
          onClose={handleOnClose}
          created={handleCreated}
        />
      </div>

      <div className="event-log">
        <h3>Event Log</h3>
        <div className="log-content">
          {eventLog.map((log, idx) => (
            <div key={idx} className="log-entry">{log}</div>
          ))}
        </div>
      </div>
    </div>
  );
}

export default ComprehensiveEventExample;
```

### Example 2: Conditional Menu Behavior

```jsx
function ConditionalMenuApp() {
  const [canAccessAdvanced, setCanAccessAdvanced] = useState(false);
  const [itemsSelected, setItemsSelected] = useState([]);

  const items = [
    {
      text: 'File',
      items: [
        { text: 'New' },
        { text: 'Open' },
        { text: 'Save' },
        { text: 'Save As' }
      ]
    },
    {
      text: 'Advanced',
      items: [
        { text: 'Settings' },
        { text: 'Debug Mode' },
        { text: 'Diagnostics' }
      ]
    }
  ];

  const handleBeforeOpen = (args) => {
    // Prevent opening Advanced menu if not allowed
    if (args.parentItem.text === 'Advanced' && !canAccessAdvanced) {
      args.cancel = true;
      alert('Advanced menu is not available');
    }
  };

  const handleSelect = (args) => {
    // Track selected items
    setItemsSelected(prev => [...prev, args.item.text]);
  };

  return (
    <div>
      <div className="controls">
        <label>
          <input 
            type="checkbox"
            checked={canAccessAdvanced}
            onChange={(e) => setCanAccessAdvanced(e.target.checked)}
          />
          Allow Advanced Menu
        </label>
      </div>

      <MenuComponent
        items={items}
        beforeOpen={handleBeforeOpen}
        select={handleSelect}
      />

      <div className="status">
        <h3>Selected Items:</h3>
        <ul>
          {itemsSelected.map((item, idx) => (
            <li key={idx}>{item}</li>
          ))}
        </ul>
      </div>
    </div>
  );
}

export default ConditionalMenuApp;
```

### Example 3: Item Customization on Render

```jsx
function CustomRenderExample() {
  const items = [
    { text: 'Home', iconCss: 'e-icons e-home' },
    { text: 'Products', iconCss: 'e-icons e-box' },
    { text: 'About', iconCss: 'e-icons e-info' },
    { text: 'Contact', iconCss: 'e-icons e-mail' }
  ];

  const handleBeforeItemRender = (args) => {
    // Add custom styling based on item
    if (args.item.text === 'Contact') {
      args.element.classList.add('highlight');
    }

    // Add animation class
    args.element.classList.add('fade-in');

    // Customize based on submenu status
    if (args.item.items) {
      args.element.classList.add('has-submenu');
    }
  };

  return (
    <MenuComponent
      items={items}
      beforeItemRender={handleBeforeItemRender}
    />
  );
}

export default CustomRenderExample;
```

## Best Practices

1. **Use beforeOpen/beforeClose for validation** - Prevent invalid operations before they occur
2. **Handle select events for navigation** - Direct users to appropriate pages/views
3. **Log events for analytics** - Track user interactions
4. **Use beforeItemRender for styling** - Apply dynamic classes/styles during rendering
5. **Always provide feedback** - Show users that their actions are registered

## Related Topics

- [Methods and API](./methods-api.md) - Programmatic menu control
- [Properties and Configuration](./properties-and-configuration.md) - Event properties
- [Menu Items Customization](./menu-items-customization.md) - Item manipulation with events
