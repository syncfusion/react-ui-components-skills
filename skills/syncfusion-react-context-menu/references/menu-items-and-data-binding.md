# Menu Items and Data Binding

## Table of Contents
- [MenuItemModel Interface](#menuitemintern-interface)
- [Creating Static Menu Items](#creating-static-menu-items)
- [Data Binding with Local Sources](#data-binding-with-local-sources)
- [Dynamic Menu Item Generation](#dynamic-menu-item-generation)
- [Nested Submenus](#nested-submenus)
- [Item Separators](#item-separators)

## MenuItemModel Interface

The `MenuItemModel` interface defines the structure for menu items. Key properties:

| Property | Type | Description |
|----------|------|-------------|
| `text` | string | Display text for the menu item |
| `id` | string | Unique identifier for the item |
| `iconCss` | string | CSS class for the icon |
| `items` | MenuItemModel[] | Nested submenu items |
| `separator` | boolean | Whether to render as a separator |
| `disabled` | boolean | Disable the menu item |
| `url` | string | Navigation URL |

## Creating Static Menu Items

Define menu items as a simple array of objects:

```ts
import { ContextMenuComponent, MenuItemModel } from '@syncfusion/ej2-react-navigations';
import * as React from 'react';

function App() {
  const menuItems: MenuItemModel[] = [
    { text: 'Cut', id: 'cut' },
    { text: 'Copy', id: 'copy' },
    { text: 'Paste', id: 'paste' },
    { text: 'Delete', id: 'delete' }
  ];

  return (
    <div>
      <div id="target">Right click to open menu</div>
      <ContextMenuComponent target="#target" items={menuItems} />
    </div>
  );
}

export default App;
```

## Data Binding with Local Sources

Bind the ContextMenu to local data sources using the `items` property. The component automatically renders items from the provided data.

### Example: Simple Array Binding

```ts
import { ContextMenuComponent, MenuItemModel } from '@syncfusion/ej2-react-navigations';
import * as React from 'react';

interface MenuItem {
  text: string;
  id: string;
}

function App() {
  const menuData: MenuItem[] = [
    { text: 'Cut', id: 'cut' },
    { text: 'Copy', id: 'copy' },
    { text: 'Paste', id: 'paste' }
  ];

  const menuItems: MenuItemModel[] = menuData.map(item => ({
    text: item.text,
    id: item.id
  }));

  return (
    <div>
      <div id="target">Right click to open menu</div>
      <ContextMenuComponent target="#target" items={menuItems} />
    </div>
  );
}

export default App;
```

### Example: Hierarchical Data Binding

Bind hierarchical data with parent-child relationships:

```ts
import { ContextMenuComponent, MenuEventArgs, MenuItemModel } from '@syncfusion/ej2-react-navigations';
import * as React from 'react';

interface IRecord {
  id: number;
  text: string;
  parentId?: number;
}

function App() {
  const data: IRecord[] = [
    { id: 1, text: 'File' },
    { id: 2, text: 'New', parentId: 1 },
    { id: 3, text: 'Open', parentId: 1 },
    { id: 4, text: 'Save', parentId: 1 },
    { id: 5, text: 'Edit' },
    { id: 6, text: 'Cut', parentId: 5 },
    { id: 7, text: 'Copy', parentId: 5 },
    { id: 8, text: 'Paste', parentId: 5 }
  ];

  function getMenuItems(): MenuItemModel[] {
    const menuItems: MenuItemModel[] = [];
    
    for (const record of data) {
      if (record.parentId) {
        // Add to parent's items array
        const parent = menuItems.find(m => m.id === `item-${record.parentId}`);
        if (parent) {
          if (!parent.items) parent.items = [];
          parent.items.push({ text: record.text, id: `item-${record.id}` });
        }
      } else {
        // Add as root item
        menuItems.push({ text: record.text, id: `item-${record.id}` });
      }
    }
    return menuItems;
  }

  function itemBeforeEvent(args: MenuEventArgs) {
    // Optional: Customize items before rendering
    if (!args.item.text) {
      args.element.classList.add('e-separator');
    }
  }

  return (
    <div>
      <div id="target">Right click to open menu</div>
      <ContextMenuComponent 
        target="#target" 
        items={getMenuItems()} 
        beforeItemRender={itemBeforeEvent}
      />
    </div>
  );
}

export default App;
```

## Dynamic Menu Item Generation

Generate menu items dynamically based on conditions or external data:

```ts
import { ContextMenuComponent, MenuItemModel } from '@syncfusion/ej2-react-navigations';
import * as React from 'react';

interface FileItem {
  name: string;
  type: 'file' | 'folder';
}

function App() {
  const [selectedFile, setSelectedFile] = React.useState<FileItem | null>(null);

  // Generate menu items based on selected file type
  function generateMenuItems(): MenuItemModel[] {
    const commonItems: MenuItemModel[] = [
      { text: 'Open', iconCss: 'e-icons e-folder-open' },
      { text: 'Cut', iconCss: 'e-icons e-cut' },
      { text: 'Copy', iconCss: 'e-icons e-copy' }
    ];

    if (selectedFile?.type === 'folder') {
      return [
        ...commonItems,
        { text: 'New Folder', iconCss: 'e-icons e-folder' },
        { text: 'Paste', iconCss: 'e-icons e-paste' }
      ];
    }

    if (selectedFile?.type === 'file') {
      return [
        ...commonItems,
        { text: 'Rename', iconCss: 'e-icons e-edit' },
        { text: 'Delete', iconCss: 'e-icons e-delete' }
      ];
    }

    return commonItems;
  }

  const handleFileSelect = (file: FileItem) => {
    setSelectedFile(file);
  };

  return (
    <div>
      <div id="target" onClick={() => handleFileSelect({ name: 'document.txt', type: 'file' })}>
        Right click on a file
      </div>
      <ContextMenuComponent target="#target" items={generateMenuItems()} />
    </div>
  );
}

export default App;
```

## Nested Submenus

Create hierarchical menus with nested items:

```ts
import { ContextMenuComponent, MenuItemModel } from '@syncfusion/ej2-react-navigations';
import * as React from 'react';

function App() {
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
        { text: 'Paste' },
        { separator: true },
        { text: 'Find' }
      ]
    },
    {
      text: 'View',
      items: [
        {
          text: 'Zoom',
          items: [
            { text: '100%' },
            { text: '150%' },
            { text: '200%' }
          ]
        },
        { text: 'Full Screen' }
      ]
    }
  ];

  return (
    <div>
      <div id="target">Right click to open menu</div>
      <ContextMenuComponent target="#target" items={menuItems} />
    </div>
  );
}

export default App;
```

## Item Separators

Add visual separators between menu items using the `separator` property:

```ts
import { ContextMenuComponent, MenuItemModel } from '@syncfusion/ej2-react-navigations';
import * as React from 'react';

function App() {
  const menuItems: MenuItemModel[] = [
    { text: 'Cut' },
    { text: 'Copy' },
    { text: 'Paste' },
    { separator: true },  // Visual separator
    { text: 'Select All' },
    { separator: true },
    { text: 'Delete' }
  ];

  return (
    <div>
      <div id="target">Right click to open menu</div>
      <ContextMenuComponent target="#target" items={menuItems} />
    </div>
  );
}

export default App;
```

## Best Practices

1. **Use unique IDs:** Assign unique `id` values to menu items for easy reference in event handlers
2. **Limit nesting depth:** Keep submenu nesting to 2-3 levels for better UX
3. **Group related items:** Use separators to logically group related menu items
4. **Label clearly:** Use descriptive text that clearly indicates the action
5. **Consider state:** Disable items that are not applicable to the current context
6. **Optimize data:** For large datasets, transform data lazily or use virtual scrolling
