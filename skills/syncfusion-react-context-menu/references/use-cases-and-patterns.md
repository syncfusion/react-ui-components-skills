# Use Cases and Patterns

## Table of Contents
- [Common Context Menu Patterns](#common-context-menu-patterns)
- [Dynamic Item Management](#dynamic-item-management)
- [Separators and Grouping](#separators-and-grouping)
- [Multi-Level Nesting Examples](#multi-level-nesting-examples)
- [Real-World Integration Scenarios](#real-world-integration-scenarios)
- [File Operations](#file-operations)
- [Editor Actions](#editor-actions)
- [Data Grid Operations](#data-grid-operations)

## Common Context Menu Patterns

### Pattern 1: Simple Text Operations

Basic context menu for text editing tasks:

```ts
import { ContextMenuComponent, MenuItemModel } from '@syncfusion/ej2-react-navigations';
import * as React from 'react';

function TextEditorMenu() {
  const menuItems: MenuItemModel[] = [
    { text: 'Cut', iconCss: 'e-icons e-cut' },
    { text: 'Copy', iconCss: 'e-icons e-copy' },
    { text: 'Paste', iconCss: 'e-icons e-paste' },
    { separator: true },
    { text: 'Select All', iconCss: 'e-icons e-select-all' }
  ];

  const handleSelect = (args) => {
    const text = args.item?.text || '';
    
    switch (text) {
      case 'Cut':
        document.execCommand('cut');
        break;
      case 'Copy':
        document.execCommand('copy');
        break;
      case 'Paste':
        document.execCommand('paste');
        break;
      case 'Select All':
        document.execCommand('selectAll');
        break;
    }
  };

  return (
    <div>
      <textarea id='target' placeholder='Type something...'></textarea>
      <ContextMenuComponent target='#target' items={menuItems} select={handleSelect} />
    </div>
  );
}

export default TextEditorMenu;
```

### Pattern 2: Image Operations

Context menu for image manipulation:

```ts
function ImageContextMenu() {
  const [selectedImage, setSelectedImage] = React.useState<HTMLImageElement | null>(null);

  const menuItems: MenuItemModel[] = [
    { text: 'Save Image', id: 'save', iconCss: 'e-icons e-download' },
    { text: 'Copy Image', id: 'copy', iconCss: 'e-icons e-copy' },
    { separator: true },
    { text: 'View in New Tab', id: 'view', iconCss: 'e-icons e-open' },
    { separator: true },
    { text: 'Image Properties', id: 'properties', iconCss: 'e-icons e-info' }
  ];

  const handleSelect = (args) => {
    const image = selectedImage;
    if (!image) return;

    switch (args.item?.id) {
      case 'save':
        const link = document.createElement('a');
        link.href = image.src;
        link.download = 'image.jpg';
        link.click();
        break;
      case 'copy':
        navigator.clipboard.write([
          new ClipboardItem({ 'image/png': fetch(image.src) })
        ]);
        break;
      case 'view':
        window.open(image.src, '_blank');
        break;
      case 'properties':
        alert(`Image Size: ${image.naturalWidth}x${image.naturalHeight}`);
        break;
    }
  };

  const handleContextMenu = (e: React.MouseEvent<HTMLImageElement>) => {
    setSelectedImage(e.currentTarget);
  };

  return (
    <div>
      <img 
        id='target' 
        src='image.jpg' 
        alt='Sample' 
        onContextMenu={handleContextMenu}
      />
      <ContextMenuComponent target='#target' items={menuItems} select={handleSelect} />
    </div>
  );
}

export default ImageContextMenu;
```

### Pattern 3: Link Operations

Context menu for hyperlink handling:

```ts
function LinkContextMenu() {
  const menuItems: MenuItemModel[] = [
    { text: 'Open Link', id: 'open', iconCss: 'e-icons e-open' },
    { text: 'Open in New Tab', id: 'new-tab', iconCss: 'e-icons e-window-new' },
    { separator: true },
    { text: 'Copy Link', id: 'copy', iconCss: 'e-icons e-copy' },
    { text: 'Copy Link Text', id: 'copy-text', iconCss: 'e-icons e-text-copy' },
    { separator: true },
    { text: 'Edit Link', id: 'edit', iconCss: 'e-icons e-edit' },
    { text: 'Remove Link', id: 'remove', iconCss: 'e-icons e-remove' }
  ];

  const handleSelect = (args) => {
    const link = document.querySelector('#target') as HTMLAnchorElement;
    
    switch (args.item?.id) {
      case 'open':
        window.location.href = link.href;
        break;
      case 'new-tab':
        window.open(link.href, '_blank');
        break;
      case 'copy':
        navigator.clipboard.writeText(link.href);
        break;
      case 'copy-text':
        navigator.clipboard.writeText(link.textContent || '');
        break;
      case 'edit':
        console.log('Edit link:', link.href);
        break;
      case 'remove':
        link.replaceWith(link.textContent || '');
        break;
    }
  };

  return (
    <div>
      <a id='target' href='https://example.com'>Example Link</a>
      <ContextMenuComponent target='#target' items={menuItems} select={handleSelect} />
    </div>
  );
}

export default LinkContextMenu;
```

## Dynamic Item Management

### Add Items Dynamically

```ts
function DynamicItemsMenu() {
  const [items, setItems] = React.useState<MenuItemModel[]>([
    { text: 'Delete', id: 'delete' }
  ]);

  const addItem = (newItem: MenuItemModel) => {
    setItems([...items, newItem]);
  };

  const removeItem = (id: string) => {
    setItems(items.filter(item => item.id !== id));
  };

  const toggleItem = (id: string) => {
    setItems(items.map(item =>
      item.id === id ? { ...item, disabled: !item.disabled } : item
    ));
  };

  React.useEffect(() => {
    // Add items after 2 seconds
    setTimeout(() => {
      addItem({ text: 'Rename', id: 'rename' });
    }, 2000);

    // Remove items after 5 seconds
    setTimeout(() => {
      removeItem('delete');
    }, 5000);
  }, []);

  return (
    <div>
      <div id='target'>Right click to see dynamic menu</div>
      <ContextMenuComponent target='#target' items={items} />
    </div>
  );
}

export default DynamicItemsMenu;
```

### Enable/Disable Items Conditionally

```ts
function ConditionalItemsMenu() {
  const [hasSelection, setHasSelection] = React.useState(false);

  const menuItems: MenuItemModel[] = [
    { text: 'Cut', disabled: !hasSelection },
    { text: 'Copy', disabled: !hasSelection },
    { text: 'Paste', disabled: false },
    { separator: true },
    { text: 'Select All' }
  ];

  const handleBeforeOpen = () => {
    const selection = window.getSelection()?.toString();
    setHasSelection(!!(selection && selection.length > 0));
  };

  return (
    <div>
      <div id='target' onContextMenu={handleBeforeOpen}>
        Select text and right-click
      </div>
      <ContextMenuComponent 
        target='#target' 
        items={menuItems}
        beforeOpen={handleBeforeOpen}
      />
    </div>
  );
}

export default ConditionalItemsMenu;
```

## Separators and Grouping

### Logical Item Grouping

```ts
function GroupedMenu() {
  const menuItems: MenuItemModel[] = [
    // File operations
    { text: 'New', iconCss: 'e-icons e-file-new' },
    { text: 'Open', iconCss: 'e-icons e-folder-open' },
    { text: 'Save', iconCss: 'e-icons e-save' },
    { separator: true },

    // Edit operations
    { text: 'Cut', iconCss: 'e-icons e-cut' },
    { text: 'Copy', iconCss: 'e-icons e-copy' },
    { text: 'Paste', iconCss: 'e-icons e-paste' },
    { separator: true },

    // Format operations
    { text: 'Bold', iconCss: 'e-icons e-bold' },
    { text: 'Italic', iconCss: 'e-icons e-italic' },
    { text: 'Underline', iconCss: 'e-icons e-underline' },
    { separator: true },

    // View/Help
    { text: 'Properties', iconCss: 'e-icons e-properties' }
  ];

  return (
    <div>
      <div id='target'>Right click for grouped menu</div>
      <ContextMenuComponent target='#target' items={menuItems} />
    </div>
  );
}

export default GroupedMenu;
```

## Multi-Level Nesting Examples

### File Explorer Pattern

```ts
function FileExplorerMenu() {
  const menuItems: MenuItemModel[] = [
    { text: 'Create', iconCss: 'e-icons e-new',
      items: [
        { text: 'Folder', iconCss: 'e-icons e-folder' },
        { text: 'File', iconCss: 'e-icons e-file' },
        { text: 'Document', iconCss: 'e-icons e-document' }
      ]
    },
    { separator: true },
    { text: 'Cut', iconCss: 'e-icons e-cut' },
    { text: 'Copy', iconCss: 'e-icons e-copy' },
    { text: 'Paste', iconCss: 'e-icons e-paste' },
    { separator: true },
    { text: 'Delete', iconCss: 'e-icons e-delete' },
    { text: 'Rename', iconCss: 'e-icons e-rename' },
    { separator: true },
    { text: 'Properties', iconCss: 'e-icons e-properties' }
  ];

  return (
    <div>
      <div id='target'>Right click for file menu</div>
      <ContextMenuComponent target='#target' items={menuItems} />
    </div>
  );
}

export default FileExplorerMenu;
```

### Application Menu Pattern

```ts
function AppMenu() {
  const menuItems: MenuItemModel[] = [
    {
      text: 'File',
      items: [
        { text: 'New' },
        { text: 'Open' },
        { text: 'Save' },
        { separator: true },
        { text: 'Recent Files',
          items: [
            { text: 'Document 1' },
            { text: 'Document 2' },
            { text: 'Document 3' }
          ]
        },
        { separator: true },
        { text: 'Exit' }
      ]
    },
    {
      text: 'Edit',
      items: [
        { text: 'Undo' },
        { text: 'Redo' },
        { separator: true },
        { text: 'Cut' },
        { text: 'Copy' },
        { text: 'Paste' }
      ]
    },
    {
      text: 'View',
      items: [
        { text: 'Full Screen' },
        { text: 'Zoom',
          items: [
            { text: '50%' },
            { text: '100%' },
            { text: '150%' },
            { text: '200%' }
          ]
        }
      ]
    }
  ];

  return (
    <div>
      <div id='target'>Right click for app menu</div>
      <ContextMenuComponent target='#target' items={menuItems} />
    </div>
  );
}

export default AppMenu;
```

## Real-World Integration Scenarios

### Integration with Text Editor

```ts
function TextEditorWithContext() {
  const handleSelect = (args) => {
    const editor = document.querySelector('#editor') as HTMLTextAreaElement;
    const text = args.item?.text || '';

    switch (text) {
      case 'Spell Check':
        console.log('Running spell check...');
        break;
      case 'Grammar Check':
        console.log('Running grammar check...');
        break;
      case 'Change Language':
        console.log('Opening language selector...');
        break;
    }
  };

  const menuItems: MenuItemModel[] = [
    { text: 'Cut' },
    { text: 'Copy' },
    { text: 'Paste' },
    { separator: true },
    { text: 'Spell Check', iconCss: 'e-icons e-check' },
    { text: 'Grammar Check', iconCss: 'e-icons e-validation' },
    { separator: true },
    { text: 'Change Language', iconCss: 'e-icons e-globe' }
  ];

  return (
    <div>
      <textarea id='editor' placeholder='Type here...' />
      <ContextMenuComponent target='#editor' items={menuItems} select={handleSelect} />
    </div>
  );
}

export default TextEditorWithContext;
```

### Integration with Task Management

```ts
function TaskManagerContext() {
  const handleSelect = (args) => {
    const item = args.item;
    
    switch (item?.text) {
      case 'Mark Complete':
        console.log('Marking task as complete');
        break;
      case 'Assign To':
        console.log('Opening assignment dialog');
        break;
      case 'Set Priority':
        console.log('Opening priority selector');
        break;
      case 'Add Comment':
        console.log('Opening comment panel');
        break;
      case 'Delete Task':
        if (confirm('Delete this task?')) {
          console.log('Task deleted');
        }
        break;
    }
  };

  const menuItems: MenuItemModel[] = [
    { text: 'Mark Complete', id: 'complete' },
    { text: 'Assign To', id: 'assign' },
    { separator: true },
    { text: 'Set Priority', id: 'priority',
      items: [
        { text: 'Low' },
        { text: 'Medium' },
        { text: 'High' },
        { text: 'Critical' }
      ]
    },
    { separator: true },
    { text: 'Add Comment', id: 'comment' },
    { text: 'Add Attachment', id: 'attach' },
    { separator: true },
    { text: 'Delete Task', id: 'delete' }
  ];

  return (
    <div>
      <div id='target' className='task-item'>
        Task Item
      </div>
      <ContextMenuComponent target='#target' items={menuItems} select={handleSelect} />
    </div>
  );
}

export default TaskManagerContext;
```

## File Operations

### Complete File Management Menu

```ts
function FileManagementMenu() {
  const handleBeforeOpen = (args) => {
    const target = args.event?.target as HTMLElement;
    const isReadOnly = target?.classList.contains('read-only');
    
    // Disable edit operations for read-only files
    if (isReadOnly) {
      args.items = args.items?.map(item => ({
        ...item,
        disabled: ['Rename', 'Delete', 'Move'].includes(item.text as string)
      }));
    }
  };

  const menuItems: MenuItemModel[] = [
    { text: 'Open', iconCss: 'e-icons e-open' },
    { text: 'Open With', iconCss: 'e-icons e-open',
      items: [
        { text: 'Text Editor' },
        { text: 'Image Viewer' },
        { text: 'Code Editor' }
      ]
    },
    { separator: true },
    { text: 'Cut', iconCss: 'e-icons e-cut' },
    { text: 'Copy', iconCss: 'e-icons e-copy' },
    { text: 'Paste', iconCss: 'e-icons e-paste' },
    { separator: true },
    { text: 'Rename', iconCss: 'e-icons e-rename' },
    { text: 'Delete', iconCss: 'e-icons e-delete' },
    { text: 'Move', iconCss: 'e-icons e-move' },
    { separator: true },
    { text: 'Compress', iconCss: 'e-icons e-compress' },
    { text: 'Properties', iconCss: 'e-icons e-properties' }
  ];

  return (
    <div>
      <div id='target'>Right click on file</div>
      <ContextMenuComponent 
        target='#target' 
        items={menuItems}
        beforeOpen={handleBeforeOpen}
      />
    </div>
  );
}

export default FileManagementMenu;
```

## Best Practices

1. **Logical Grouping:** Use separators to group related operations
2. **Icon Consistency:** Use consistent icons for similar operations
3. **Nesting Depth:** Limit nesting to 2-3 levels maximum
4. **Keyboard Shortcuts:** Show available shortcuts in menu items
5. **Disabled States:** Disable operations that aren't applicable
6. **Context Awareness:** Adjust menu based on selected element type
7. **User Feedback:** Provide confirmation for destructive actions
8. **Performance:** Keep menus responsive with efficient event handlers
