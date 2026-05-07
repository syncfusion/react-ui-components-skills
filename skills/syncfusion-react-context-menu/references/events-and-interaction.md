# Events and Interaction

## Table of Contents
- [Context Menu Events](#context-menu-events)
- [beforeOpen Event](#beforeopen-event)
- [beforeClose Event](#beforeclose-event)
- [onOpen and onClose Events](#onopen-and-onclose-events)
- [select Event](#select-event)
- [beforeItemRender Event](#beforeitemrender-event)
- [Creating Dialogs on Item Click](#creating-dialogs-on-item-click)
- [Event Propagation](#event-propagation)

## Context Menu Events

The ContextMenu component provides several events for handling user interactions:

| Event | Trigger | Use Case |
|-------|---------|----------|
| `beforeOpen` | Before menu opens | Prevent opening, customize items |
| `beforeClose` | Before menu closes | Prevent closing, save state |
| `onOpen` | After menu opens | Initialize UI, fetch data |
| `onClose` | After menu closes | Cleanup, save state |
| `select` | Item selected | Execute action, update model |
| `beforeItemRender` | Before item renders | Customize appearance |

## beforeOpen Event

The `beforeOpen` event fires before the context menu opens. Use it to:
- Prevent menu from opening in certain conditions
- Dynamically modify menu items
- Load data before display

### Example: Conditional Menu Opening

```ts
import { ContextMenuComponent, MenuEventArgs, MenuItemModel } from '@syncfusion/ej2-react-navigations';
import * as React from 'react';

function App() {
  const [selectedText, setSelectedText] = React.useState<string>('');

  const menuItems: MenuItemModel[] = [
    { text: 'Cut' },
    { text: 'Copy' },
    { text: 'Paste' },
    { text: 'Delete' }
  ];

  const handleBeforeOpen = (args: MenuEventArgs) => {
    // Get selected text
    const text = window.getSelection()?.toString() || '';
    setSelectedText(text);

    // Prevent menu opening if no selection
    if (!text) {
      args.cancel = true;
      console.log('Menu prevented: No text selected');
    }
  };

  return (
    <div>
      <div id='target' style={{ padding: '20px', border: '1px solid gray' }}>
        Select some text and right-click to open the menu
      </div>
      <ContextMenuComponent
        target='#target'
        items={menuItems}
        beforeOpen={handleBeforeOpen}
      />
    </div>
  );
}

export default App;
```

### Example: Dynamic Item Updates

```ts
const handleBeforeOpen = (args: MenuEventArgs) => {
  // Update items based on context
  const target = args.event?.target as HTMLElement;
  const isImageElement = target?.tagName === 'IMG';

  // Modify menu items dynamically
  if (isImageElement) {
    args.items = [
      { text: 'Save Image', iconCss: 'e-icons e-download' },
      { text: 'Copy Image Link', iconCss: 'e-icons e-copy' },
      { text: 'Open in New Tab', iconCss: 'e-icons e-open' }
    ];
  }
};
```

## beforeClose Event

The `beforeClose` event fires before the context menu closes:

```ts
import { ContextMenuComponent, MenuEventArgs, MenuItemModel } from '@syncfusion/ej2-react-navigations';
import * as React from 'react';

function App() {
  const [hasUnsavedChanges, setHasUnsavedChanges] = React.useState<boolean>(false);

  const menuItems: MenuItemModel[] = [
    { text: 'Save' },
    { text: 'Close' }
  ];

  const handleBeforeClose = (args: MenuEventArgs) => {
    // Prevent closing if unsaved changes
    if (hasUnsavedChanges) {
      const confirmed = window.confirm('You have unsaved changes. Close anyway?');
      if (!confirmed) {
        args.cancel = true;
      }
    }
  };

  return (
    <div>
      <div id='target'>Right click to open menu</div>
      <ContextMenuComponent
        target='#target'
        items={menuItems}
        beforeClose={handleBeforeClose}
      />
    </div>
  );
}

export default App;
```

## onOpen and onClose Events

These events fire after the menu has opened or closed:

```ts
import { ContextMenuComponent, MenuEventArgs, MenuItemModel } from '@syncfusion/ej2-react-navigations';
import * as React from 'react';

function App() {
  const [menuState, setMenuState] = React.useState<'closed' | 'open'>('closed');

  const menuItems: MenuItemModel[] = [
    { text: 'New' },
    { text: 'Open' },
    { text: 'Save' }
  ];

  const handleOnOpen = (args: MenuEventArgs) => {
    console.log('Menu opened');
    setMenuState('open');
    
    // Perform actions after menu opens
    // - Fetch data
    // - Initialize components
    // - Update UI
  };

  const handleOnClose = (args: MenuEventArgs) => {
    console.log('Menu closed');
    setMenuState('closed');
    
    // Perform cleanup after menu closes
    // - Save state
    // - Cancel pending operations
  };

  return (
    <div>
      <div id='target'>Right click to open menu (State: {menuState})</div>
      <ContextMenuComponent
        target='#target'
        items={menuItems}
        onOpen={handleOnOpen}
        onClose={handleOnClose}
      />
    </div>
  );
}

export default App;
```

## select Event

The `select` event fires when a menu item is clicked:

### Basic Item Selection

```ts
import { ContextMenuComponent, MenuEventArgs, MenuItemModel } from '@syncfusion/ej2-react-navigations';
import * as React from 'react';

function App() {
  const [selectedAction, setSelectedAction] = React.useState<string>('');

  const menuItems: MenuItemModel[] = [
    { text: 'Cut', id: 'cut' },
    { text: 'Copy', id: 'copy' },
    { text: 'Paste', id: 'paste' },
    { text: 'Delete', id: 'delete' }
  ];

  const handleSelect = (args: MenuEventArgs) => {
    const itemText = args.item?.text || '';
    setSelectedAction(`You selected: ${itemText}`);
    
    console.log('Selected item:', itemText);
    console.log('Item ID:', args.item?.id);
    
    // Execute action based on selection
    switch (itemText) {
      case 'Cut':
        document.execCommand('cut');
        break;
      case 'Copy':
        document.execCommand('copy');
        break;
      case 'Paste':
        document.execCommand('paste');
        break;
      case 'Delete':
        console.log('Delete action triggered');
        break;
    }
  };

  return (
    <div>
      <div id='target'>Right click to open menu</div>
      <ContextMenuComponent
        target='#target'
        items={menuItems}
        select={handleSelect}
      />
      {selectedAction && <p>{selectedAction}</p>}
    </div>
  );
}

export default App;
```

### Advanced Item Selection with Callbacks

```ts
const handleSelect = async (args: MenuEventArgs) => {
  const itemText = args.item?.text || '';

  try {
    // Show loading state
    console.log(`Processing: ${itemText}...`);

    // Execute async operations
    switch (itemText) {
      case 'Export to PDF':
        await exportToPdf();
        break;
      case 'Send Email':
        await sendEmail();
        break;
      case 'Sync Data':
        await syncData();
        break;
    }

    console.log(`${itemText} completed successfully`);
  } catch (error) {
    console.error(`Error during ${itemText}:`, error);
  }
};
```

## beforeItemRender Event

The `beforeItemRender` event allows customization before each item renders:

```ts
import { createElement } from '@syncfusion/ej2-base';
import { ContextMenuComponent, MenuEventArgs, MenuItemModel } from '@syncfusion/ej2-react-navigations';
import * as React from 'react';

function App() {
  const menuItems: MenuItemModel[] = [
    { text: 'Cut' },
    { text: 'Copy' },
    { text: 'Paste' },
    { text: 'Select All' }
  ];

  const shortcuts: { [key: string]: string } = {
    'Cut': 'Ctrl+X',
    'Copy': 'Ctrl+C',
    'Paste': 'Ctrl+V',
    'Select All': 'Ctrl+A'
  };

  const handleBeforeItemRender = (args: MenuEventArgs) => {
    const itemText = args.item?.text || '';

    // Add keyboard shortcut span
    if (shortcuts[itemText]) {
      const shortcutSpan = createElement('span', {
        className: 'shortcut-key',
        innerHTML: shortcuts[itemText]
      });
      args.element.appendChild(shortcutSpan);
    }

    // Disable certain items based on conditions
    if (itemText === 'Paste' && !canPaste()) {
      args.element.classList.add('e-disabled');
    }
  };

  const canPaste = () => {
    // Check if paste is available
    return !!navigator.clipboard;
  };

  return (
    <div>
      <div id='target'>Right click to open menu</div>
      <ContextMenuComponent
        target='#target'
        items={menuItems}
        beforeItemRender={handleBeforeItemRender}
      />
    </div>
  );
}

export default App;
```

## Creating Dialogs on Item Click

Open dialogs or modals when menu items are selected:

```ts
import { DialogComponent } from '@syncfusion/ej2-react-popups';
import { ContextMenuComponent, MenuEventArgs, MenuItemModel } from '@syncfusion/ej2-react-navigations';
import * as React from 'react';

function App() {
  const [dialogVisible, setDialogVisible] = React.useState<boolean>(false);
  const [dialogTitle, setDialogTitle] = React.useState<string>('');
  const [selectedItem, setSelectedItem] = React.useState<string>('');

  const menuItems: MenuItemModel[] = [
    { text: 'Properties', id: 'properties' },
    { text: 'Settings', id: 'settings' },
    { text: 'About', id: 'about' }
  ];

  const handleSelect = (args: MenuEventArgs) => {
    const itemText = args.item?.text || '';
    setSelectedItem(itemText);
    setDialogTitle(`${itemText} Dialog`);
    setDialogVisible(true);
  };

  const handleDialogClose = () => {
    setDialogVisible(false);
  };

  return (
    <div>
      <div id='target'>Right click to open context menu</div>
      <ContextMenuComponent
        target='#target'
        items={menuItems}
        select={handleSelect}
      />

      <DialogComponent
        isModal={true}
        visible={dialogVisible}
        header={dialogTitle}
        onClose={handleDialogClose}
      >
        <p>Content for {selectedItem}</p>
        <button onClick={handleDialogClose}>Close</button>
      </DialogComponent>
    </div>
  );
}

export default App;
```

## Event Propagation

Control event propagation to prevent unwanted behavior:

```ts
const handleSelect = (args: MenuEventArgs) => {
  // Stop event propagation to parent elements
  if (args.event) {
    args.event.stopPropagation();
  }

  // Prevent default browser behavior
  if (args.event) {
    args.event.preventDefault();
  }

  // Execute custom action
  console.log('Item selected:', args.item?.text);
};
```

## Common Event Patterns

### Pattern 1: Multi-Select with Modifiers

```ts
const handleSelect = (args: MenuEventArgs) => {
  const event = args.event as MouseEvent;
  
  if (event.ctrlKey || event.metaKey) {
    // Multi-select mode
    console.log('Multi-select:', args.item?.text);
  } else if (event.shiftKey) {
    // Range select mode
    console.log('Range select:', args.item?.text);
  } else {
    // Single select
    console.log('Single select:', args.item?.text);
  }
};
```

### Pattern 2: Debounced Actions

```ts
import { useState, useRef } from 'react';

function App() {
  const timeoutRef = useRef<NodeJS.Timeout>();

  const handleSelect = (args: MenuEventArgs) => {
    // Clear previous timeout
    if (timeoutRef.current) {
      clearTimeout(timeoutRef.current);
    }

    // Debounce the action
    timeoutRef.current = setTimeout(() => {
      console.log('Executing action for:', args.item?.text);
      // Execute action
    }, 300);
  };

  return (
    <div>
      <div id='target'>Right click to open menu</div>
      <ContextMenuComponent
        target='#target'
        items={menuItems}
        select={handleSelect}
      />
    </div>
  );
}

export default App;
```

## Best Practices

1. **Keep event handlers simple:** Move complex logic to separate functions
2. **Handle errors gracefully:** Wrap async operations in try-catch
3. **Cancel propagation when needed:** Prevent parent handlers from executing
4. **Clean up resources:** Remove event listeners when component unmounts
5. **Provide user feedback:** Show loading states or confirmation dialogs
6. **Validate selections:** Ensure items can actually be acted upon
