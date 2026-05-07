---
title: Methods API
parent_skill: Implementing Menu
description: Complete reference for all MenuComponent methods with examples
---

# Methods API

## Table of Contents
1. [Item Manipulation](#item-manipulation)
2. [Item State Management](#item-state-management)
3. [Item Access](#item-access)
4. [Hamburger Control](#hamburger-control)
5. [Lifecycle](#lifecycle)

## Item Manipulation

### insertBefore()
**Signature:** `insertBefore(items: MenuItemModel[], target: string | HTMLElement): void`  
**Description:** Inserts menu items before a specified target item.

```jsx
import { MenuComponent, MenuItemModel } from '@syncfusion/ej2-react-navigations';
import { useRef } from 'react';

function App() {
  const menuRef = useRef(null);
  const items = [
    { id: 'file', text: 'File' },
    { id: 'edit', text: 'Edit' },
    { id: 'help', text: 'Help' }
  ];

  const insertBeforeEdit = () => {
    // Insert "Tools" menu before "Edit"
    const newItems = [
      { id: 'tools', text: 'Tools' }
    ];
    menuRef.current?.insertBefore(newItems, '#edit');
  };

  return (
    <>
      <button onClick={insertBeforeEdit}>Insert Before Edit</button>
      <MenuComponent 
        ref={menuRef}
        items={items}
      />
    </>
  );
}

export default App;
```

### insertAfter()
**Signature:** `insertAfter(items: MenuItemModel[], target: string | HTMLElement): void`  
**Description:** Inserts menu items after a specified target item.

```jsx
function App() {
  const menuRef = useRef(null);
  const items = [
    { id: 'file', text: 'File' },
    { id: 'edit', text: 'Edit' },
    { id: 'help', text: 'Help' }
  ];

  const insertAfterEdit = () => {
    // Insert "View" menu after "Edit"
    const newItems = [
      {
        id: 'view',
        text: 'View',
        items: [
          { text: 'Zoom In' },
          { text: 'Zoom Out' }
        ]
      }
    ];
    menuRef.current?.insertAfter(newItems, '#edit');
  };

  return (
    <>
      <button onClick={insertAfterEdit}>Insert After Edit</button>
      <MenuComponent 
        ref={menuRef}
        items={items}
      />
    </>
  );
}

export default App;
```

### removeItems()
**Signature:** `removeItems(targets: string[] | HTMLElement[]): void`  
**Description:** Removes menu items by their ID or element reference.

```jsx
function App() {
  const menuRef = useRef(null);
  const items = [
    { id: 'file', text: 'File' },
    { id: 'edit', text: 'Edit' },
    { id: 'tools', text: 'Tools' },
    { id: 'help', text: 'Help' }
  ];

  const removeTools = () => {
    // Remove the "Tools" menu
    menuRef.current?.removeItems(['#tools']);
  };

  const removeMultiple = () => {
    // Remove multiple items
    menuRef.current?.removeItems(['#tools', '#help']);
  };

  return (
    <>
      <button onClick={removeTools}>Remove Tools</button>
      <button onClick={removeMultiple}>Remove Tools & Help</button>
      <MenuComponent 
        ref={menuRef}
        items={items}
      />
    </>
  );
}

export default App;
```

## Item State Management

### enableItems()
**Signature:** `enableItems(targets: string[] | HTMLElement[], enable: boolean): void`  
**Description:** Enables or disables menu items.

```jsx
function App() {
  const menuRef = useRef(null);
  const items = [
    { id: 'file', text: 'File' },
    { id: 'edit', text: 'Edit' },
    { id: 'tools', text: 'Tools' }
  ];

  const disableSave = () => {
    // Disable the "Edit" menu
    menuRef.current?.enableItems(['#edit'], false);
  };

  const enableAll = () => {
    // Enable all items
    menuRef.current?.enableItems(['#file', '#edit', '#tools'], true);
  };

  return (
    <>
      <button onClick={disableSave}>Disable Edit</button>
      <button onClick={enableAll}>Enable All</button>
      <MenuComponent 
        ref={menuRef}
        items={items}
      />
    </>
  );
}

export default App;
```

### showItems()
**Signature:** `showItems(targets: string[] | HTMLElement[], show?: boolean): void`  
**Description:** Shows hidden menu items.

```jsx
function App() {
  const menuRef = useRef(null);
  const [hiddenItems, setHiddenItems] = useState(['#advanced', '#debug']);
  
  const items = [
    { id: 'file', text: 'File' },
    { id: 'edit', text: 'Edit' },
    { id: 'advanced', text: 'Advanced', style: { display: 'none' } }
  ];

  const showAdvanced = () => {
    // Show hidden "Advanced" menu
    menuRef.current?.showItems(['#advanced']);
  };

  return (
    <>
      <button onClick={showAdvanced}>Show Advanced</button>
      <MenuComponent 
        ref={menuRef}
        items={items}
      />
    </>
  );
}

export default App;
```

### hideItems()
**Signature:** `hideItems(targets: string[] | HTMLElement[]): void`  
**Description:** Hides visible menu items.

```jsx
function App() {
  const menuRef = useRef(null);
  const items = [
    { id: 'file', text: 'File' },
    { id: 'edit', text: 'Edit' },
    { id: 'developer', text: 'Developer Tools' }
  ];

  const hideDeveloperTools = () => {
    // Hide the "Developer Tools" menu
    menuRef.current?.hideItems(['#developer']);
  };

  return (
    <>
      <button onClick={hideDeveloperTools}>Hide Dev Tools</button>
      <MenuComponent 
        ref={menuRef}
        items={items}
      />
    </>
  );
}

export default App;
```

## Item Access

### setItem()
**Signature:** `setItem(item: MenuItemModel, target: string | HTMLElement): void`  
**Description:** Updates properties of an existing menu item.

```jsx
function App() {
  const menuRef = useRef(null);
  const items = [
    { id: 'file', text: 'File', iconCss: 'e-icons e-folder' },
    { id: 'edit', text: 'Edit', iconCss: 'e-icons e-edit' }
  ];

  const updateFile = () => {
    // Update the "File" menu item
    const updatedItem = {
      id: 'file',
      text: 'File (Updated)',
      iconCss: 'e-icons e-folder-open'
    };
    menuRef.current?.setItem(updatedItem, '#file');
  };

  const addChildItems = () => {
    // Add sub-items to File menu
    const updatedItem = {
      id: 'file',
      text: 'File',
      items: [
        { text: 'New' },
        { text: 'Open' },
        { text: 'Save' }
      ]
    };
    menuRef.current?.setItem(updatedItem, '#file');
  };

  return (
    <>
      <button onClick={updateFile}>Update File Menu</button>
      <button onClick={addChildItems}>Add File Sub-items</button>
      <MenuComponent 
        ref={menuRef}
        items={items}
      />
    </>
  );
}

export default App;
```

### getItemIndex()
**Signature:** `getItemIndex(target: string | HTMLElement): number`  
**Description:** Returns the index of a menu item in the menu.

```jsx
function App() {
  const menuRef = useRef(null);
  const items = [
    { id: 'file', text: 'File' },
    { id: 'edit', text: 'Edit' },
    { id: 'view', text: 'View' },
    { id: 'help', text: 'Help' }
  ];

  const logIndex = () => {
    const index = menuRef.current?.getItemIndex('#edit');
    console.log(`Edit menu index: ${index}`); // Output: 1
  };

  const getMultiple = () => {
    const fileIndex = menuRef.current?.getItemIndex('#file');
    const helpIndex = menuRef.current?.getItemIndex('#help');
    console.log(`File: ${fileIndex}, Help: ${helpIndex}`); // Output: File: 0, Help: 3
  };

  return (
    <>
      <button onClick={logIndex}>Get Edit Index</button>
      <button onClick={getMultiple}>Get Multiple Indices</button>
      <MenuComponent 
        ref={menuRef}
        items={items}
      />
    </>
  );
}

export default App;
```

## Hamburger Control

### open()
**Signature:** `open(): void`  
**Description:** Opens the hamburger menu (in hamburgerMode).

```jsx
function App() {
  const menuRef = useRef(null);
  const items = [
    { text: 'Home' },
    { text: 'About' },
    {
      text: 'Products',
      items: [
        { text: 'Product 1' },
        { text: 'Product 2' }
      ]
    },
    { text: 'Contact' }
  ];

  const openMenu = () => {
    menuRef.current?.open();
  };

  return (
    <>
      <button onClick={openMenu}>Open Menu</button>
      <MenuComponent 
        ref={menuRef}
        items={items}
        hamburgerMode={true}
        title="Navigation"
      />
    </>
  );
}

export default App;
```

### close()
**Signature:** `close(): void`  
**Description:** Closes the hamburger menu (in hamburgerMode).

```jsx
function App() {
  const menuRef = useRef(null);
  const items = [
    { text: 'Home' },
    { text: 'About' },
    { text: 'Products' },
    { text: 'Contact' }
  ];

  const closeMenu = () => {
    menuRef.current?.close();
  };

  const handleItemSelect = (args) => {
    // Auto-close menu after item selection
    closeMenu();
  };

  return (
    <>
      <button onClick={closeMenu}>Close Menu</button>
      <MenuComponent 
        ref={menuRef}
        items={items}
        hamburgerMode={true}
        select={handleItemSelect}
      />
    </>
  );
}

export default App;
```

**Responsive Example with open() and close():**
```jsx
function ResponsiveMenu() {
  const menuRef = useRef(null);
  const [isOpen, setIsOpen] = useState(false);
  
  const items = [
    { text: 'Home' },
    { text: 'About' },
    { text: 'Services' },
    { text: 'Contact' }
  ];

  const toggleMenu = () => {
    if (isOpen) {
      menuRef.current?.close();
      setIsOpen(false);
    } else {
      menuRef.current?.open();
      setIsOpen(true);
    }
  };

  const handleItemSelect = () => {
    menuRef.current?.close();
    setIsOpen(false);
  };

  return (
    <>
      <div className="navbar">
        <h1>My App</h1>
        <button 
          className="menu-toggle"
          onClick={toggleMenu}
        >
          ☰
        </button>
      </div>
      <MenuComponent 
        ref={menuRef}
        items={items}
        hamburgerMode={true}
        select={handleItemSelect}
      />
    </>
  );
}

export default ResponsiveMenu;
```

## Lifecycle

### destroy()
**Signature:** `destroy(): void`  
**Description:** Destroys the menu component and cleans up resources.

```jsx
import { useEffect } from 'react';

function App() {
  const menuRef = useRef(null);
  const items = [
    { text: 'File' },
    { text: 'Edit' },
    { text: 'Help' }
  ];

  useEffect(() => {
    // Cleanup on component unmount
    return () => {
      menuRef.current?.destroy();
    };
  }, []);

  return (
    <MenuComponent 
      ref={menuRef}
      items={items}
    />
  );
}

export default App;
```

## Complete Example: Dynamic Menu Management

```jsx
import { MenuComponent } from '@syncfusion/ej2-react-navigations';
import { useRef, useState } from 'react';

function DynamicMenuApp() {
  const menuRef = useRef(null);
  const [logs, setLogs] = useState([]);

  const baseItems = [
    { id: 'file', text: 'File' },
    { id: 'edit', text: 'Edit' },
    { id: 'view', text: 'View' },
    { id: 'help', text: 'Help' }
  ];

  const addLog = (message) => {
    setLogs(prev => [...prev, `[${new Date().toLocaleTimeString()}] ${message}`]);
  };

  const operations = {
    insertAfter: () => {
      menuRef.current?.insertAfter(
        [{ id: 'tools', text: 'Tools' }],
        '#view'
      );
      addLog('Inserted "Tools" after "View"');
    },
    
    removeEdit: () => {
      menuRef.current?.removeItems(['#edit']);
      addLog('Removed "Edit" menu');
    },
    
    disableView: () => {
      menuRef.current?.enableItems(['#view'], false);
      addLog('Disabled "View" menu');
    },
    
    enableAll: () => {
      menuRef.current?.enableItems(['#file', '#edit', '#view', '#help'], true);
      addLog('Enabled all menus');
    },
    
    updateHelp: () => {
      menuRef.current?.setItem(
        { 
          id: 'help',
          text: 'Help & Support',
          iconCss: 'e-icons e-question'
        },
        '#help'
      );
      addLog('Updated "Help" menu');
    },
    
    getFileIndex: () => {
      const index = menuRef.current?.getItemIndex('#file');
      addLog(`File menu index: ${index}`);
    }
  };

  return (
    <div className="container">
      <h1>Dynamic Menu Management</h1>
      
      <div className="operations">
        {Object.entries(operations).map(([key, func]) => (
          <button 
            key={key}
            onClick={func}
            className="op-btn"
          >
            {key.replace(/([A-Z])/g, ' $1').trim()}
          </button>
        ))}
      </div>

      <div className="menu-container">
        <MenuComponent 
          ref={menuRef}
          items={baseItems}
        />
      </div>

      <div className="logs">
        <h3>Operation Log</h3>
        <ul>
          {logs.map((log, idx) => (
            <li key={idx}>{log}</li>
          ))}
        </ul>
      </div>
    </div>
  );
}

export default DynamicMenuApp;
```

## Related Topics

- [Properties and Configuration](./properties-and-configuration.md) - Available properties
- [Events and Callbacks](./events-and-callbacks.md) - Event handling
- [Menu Items Customization](./menu-items-customization.md) - Item management examples
- [Hamburger and Mobile Mode](./hamburger-mode.md) - Mobile implementation
