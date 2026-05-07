# Templates and Customization

## Table of Contents
- [Item Templates](#item-templates)
- [Template Syntax](#template-syntax)
- [Custom Rendering with beforeItemRender](#custom-rendering-with-beforeitemrender)
- [Adding Rich Content](#adding-rich-content)
- [Icon Integration](#icon-integration)
- [Advanced Customization](#advanced-customization)

## Item Templates

The `itemTemplate` property allows you to define custom HTML templates for menu items. This enables rich content beyond simple text, including:
- Custom layouts with multiple content areas
- Icons with descriptions
- Metadata and additional information
- Complex visual hierarchies

### Basic Template Example

```ts
import { enableRipple } from '@syncfusion/ej2-base';
import { ContextMenuComponent, MenuItemModel } from '@syncfusion/ej2-react-navigations';
import * as React from 'react';
import './App.css';

enableRipple(true);

function App() {
  // Define custom template with icon and description
  const template: string = `
    <div class='menu-wrapper'>
      <span class='${iconCss} icon-right'></span>
      <div class='text-content'>
        <span class='text'>${text}</span>
        <span class='description'>${description}</span>
      </div>
    </div>
  `;

  const menuItems: any = [
    {
      text: 'Selection',
      description: 'Choose from options',
      iconCss: 'e-icons e-list-unordered'
    },
    {
      text: 'Yes / No',
      description: 'Select Yes or No',
      iconCss: 'e-icons e-check-box'
    },
    {
      text: 'Text Input',
      description: 'Type own answer',
      iconCss: 'e-icons e-caption',
      items: [
        {
          text: 'Single line',
          description: 'Type answer in one line',
          iconCss: 'e-icons e-text-form'
        },
        {
          text: 'Multiple line',
          description: 'Type answer in multiple lines',
          iconCss: 'e-icons e-text-wrap'
        }
      ]
    }
  ];

  return (
    <div className='control-pane'>
      <div id='target'>Right click to open the Context Menu</div>
      <ContextMenuComponent
        target='#target'
        items={menuItems}
        itemTemplate={template}
      />
    </div>
  );
}

export default App;
```

### CSS for Template Styling

```css
.menu-wrapper {
  display: flex;
  align-items: center;
  padding: 8px 0;
  width: 100%;
}

.icon-right {
  font-size: 16px;
  margin-right: 10px;
  min-width: 24px;
  text-align: center;
}

.text-content {
  display: flex;
  flex-direction: column;
  gap: 2px;
}

.text {
  font-weight: 500;
  font-size: 14px;
  color: #333;
}

.description {
  font-size: 12px;
  color: #999;
  font-style: italic;
}

.e-contextmenu-template .e-ul {
  min-width: 200px;
}
```

## Template Syntax

Template strings use `${propertyName}` syntax to access item properties:

```ts
// Item object
const item = {
  text: 'Edit',
  iconCss: 'e-icons e-edit',
  shortcut: 'Ctrl+E'
};

// Template
const template = `
  <div class='template-item'>
    <span class='${iconCss}'></span>
    <span class='${text}'></span>
    <span class='shortcut'>${shortcut}</span>
  </div>
`;
```

## Custom Rendering with beforeItemRender

The `beforeItemRender` event triggers for each item before rendering. Use it to:
- Add custom CSS classes
- Modify item elements dynamically
- Add keyboard shortcuts
- Conditionally apply styling

### Example: Add Keyboard Shortcuts

```ts
import { createElement, enableRipple } from '@syncfusion/ej2-base';
import { ContextMenuComponent, MenuEventArgs, MenuItemModel } from '@syncfusion/ej2-react-navigations';
import * as React from 'react';

enableRipple(true);

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

  function beforeItemRender(args: MenuEventArgs) {
    if (args.item.text && shortcuts[args.item.text]) {
      // Create shortcut span
      const shortcutSpan = createElement('span', {
        className: 'shortcut',
        innerHTML: shortcuts[args.item.text]
      });
      
      // Append to item element
      args.element.appendChild(shortcutSpan);
    }
  }

  return (
    <div>
      <div id='target'>Right click to open menu</div>
      <ContextMenuComponent
        target='#target'
        items={menuItems}
        beforeItemRender={beforeItemRender}
      />
    </div>
  );
}

export default App;
```

### CSS for Shortcuts

```css
.shortcut {
  margin-left: auto;
  font-size: 11px;
  color: #999;
  font-family: monospace;
  padding-left: 16px;
  min-width: 60px;
  text-align: right;
}
```

## Adding Rich Content

Combine templates with event handlers for complex customizations:

```ts
import { ContextMenuComponent, MenuEventArgs, MenuItemModel } from '@syncfusion/ej2-react-navigations';
import * as React from 'react';

function App() {
  const template: string = `
    <div class='rich-menu-item'>
      <div class='item-icon'>
        <img src='${image}' alt='${text}' />
      </div>
      <div class='item-details'>
        <div class='item-title'>${text}</div>
        <div class='item-meta'>${category} • ${size}KB</div>
      </div>
    </div>
  `;

  const menuItems: any = [
    {
      text: 'document.pdf',
      image: '/icons/pdf.png',
      category: 'Document',
      size: 245
    },
    {
      text: 'spreadsheet.xlsx',
      image: '/icons/excel.png',
      category: 'Spreadsheet',
      size: 512
    },
    {
      text: 'presentation.pptx',
      image: '/icons/powerpoint.png',
      category: 'Presentation',
      size: 1024
    }
  ];

  function beforeOpen(args: MenuEventArgs) {
    if (args.element.classList.contains('e-ul')) {
      args.element.classList.add('e-rich-menu');
    }
  }

  return (
    <div>
      <div id='target'>Right click to open menu</div>
      <ContextMenuComponent
        target='#target'
        items={menuItems}
        itemTemplate={template}
        beforeOpen={beforeOpen}
      />
    </div>
  );
}

export default App;
```

### CSS for Rich Content

```css
.rich-menu-item {
  display: flex;
  gap: 10px;
  padding: 8px;
  min-width: 250px;
}

.item-icon {
  width: 40px;
  height: 40px;
  display: flex;
  align-items: center;
  justify-content: center;
  background: #f5f5f5;
  border-radius: 4px;
}

.item-icon img {
  width: 24px;
  height: 24px;
}

.item-details {
  flex: 1;
}

.item-title {
  font-weight: 500;
  font-size: 14px;
  color: #333;
}

.item-meta {
  font-size: 12px;
  color: #999;
  margin-top: 2px;
}

.e-rich-menu {
  min-width: 270px !important;
}
```

## Icon Integration

Add icons to menu items using the Syncfusion icon library:

```ts
import { ContextMenuComponent, MenuItemModel } from '@syncfusion/ej2-react-navigations';
import * as React from 'react';

function App() {
  const menuItems: MenuItemModel[] = [
    { text: 'Cut', iconCss: 'e-icons e-cut' },
    { text: 'Copy', iconCss: 'e-icons e-copy' },
    { text: 'Paste', iconCss: 'e-icons e-paste' },
    { separator: true },
    { text: 'Delete', iconCss: 'e-icons e-delete' },
    { text: 'Rename', iconCss: 'e-icons e-rename' }
  ];

  return (
    <div>
      <div id='target'>Right click to open menu</div>
      <ContextMenuComponent target='#target' items={menuItems} />
    </div>
  );
}

export default App;
```

**Available Icon Classes:**
- Cut: `e-cut`
- Copy: `e-copy`
- Paste: `e-paste`
- Delete: `e-delete`
- Rename: `e-rename`
- Refresh: `e-refresh`
- Download: `e-download`
- Upload: `e-upload`
- Settings: `e-settings`
- Plus: `e-add`

## Advanced Customization

### Conditional Styling

Apply CSS classes based on item state:

```ts
function beforeItemRender(args: MenuEventArgs) {
  if (args.item.disabled) {
    args.element.classList.add('custom-disabled');
  }
  
  if (args.item.id === 'special-item') {
    args.element.classList.add('custom-highlight');
  }
}
```

### Dynamic Template Selection

Use different templates based on item properties:

```ts
function App() {
  const getTemplate = (item: any) => {
    if (item.type === 'separator') {
      return '<hr class="custom-separator" />';
    }
    if (item.type === 'header') {
      return '<div class="menu-header">${text}</div>';
    }
    return '<span class="menu-text">${text}</span>';
  };

  return (
    <ContextMenuComponent
      target='#target'
      items={menuItems}
      itemTemplate={getTemplate(menuItems[0])}
    />
  );
}
```

## Best Practices

1. **Performance:** Keep templates simple to avoid rendering delays
2. **Accessibility:** Ensure custom content includes proper ARIA labels
3. **Consistency:** Match template styling with your application theme
4. **Responsiveness:** Test templates on different screen sizes
5. **Maintenance:** Document custom template properties and CSS classes
