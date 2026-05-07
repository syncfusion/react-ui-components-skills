# Getting Started with Context Menu

## Table of Contents
- [Setup Development Environment](#setup-development-environment)
- [Adding Syncfusion Packages](#adding-syncfusion-packages)
- [Adding Style Sheets](#adding-style-sheets)
- [Add ContextMenu Component](#add-contextmenu-component)
- [Run the Application](#run-the-application)

## Setup Development Environment

### Using Vite (Recommended)

Vite provides a faster development environment, smaller bundle sizes, and optimized builds compared to traditional tools like `create-react-app`.

**Create a new React application:**

```bash
npm create vite@latest my-app -- --template react
cd my-app
npm install
```

**For TypeScript environment:**

```bash
npm create vite@latest my-app -- --template react-ts
cd my-app
npm install
```

For detailed setup instructions, refer to the [Vite installation guide](https://vitejs.dev/guide).

### Using Create React App

Alternatively, you can use `create-react-app`:

```bash
npx create-react-app my-app
cd my-app
```

## Adding Syncfusion Packages

Install the Syncfusion ContextMenu component and its dependencies using npm:

```bash
npm install @syncfusion/ej2-react-navigations --save
```

This command installs:
- `@syncfusion/ej2-react-navigations` (ContextMenu component)
- All required peer dependencies listed above

## Adding Style Sheets

Add the required CSS stylesheets to your application. Update your `src/App.css` or create a dedicated styles file:

```css
@import "../node_modules/@syncfusion/ej2-base/styles/tailwind3.css";
@import "../node_modules/@syncfusion/ej2-buttons/styles/tailwind3.css";
@import "../node_modules/@syncfusion/ej2-lists/styles/tailwind3.css";
@import "../node_modules/@syncfusion/ej2-popups/styles/tailwind3.css";
@import "../node_modules/@syncfusion/ej2-inputs/styles/tailwind3.css";
@import "../node_modules/@syncfusion/ej2-navigations/styles/tailwind3.css";

/* Context Menu target element styling */
#target {
    border: 1px dashed #ccc;
    height: 150px;
    padding: 10px;
    position: relative;
    text-align: justify;
    color: #666;
    user-select: none;
    border-radius: 4px;
}
```

**Available themes:** Replace `tailwind3` with your preferred theme:
- `bootstrap5.3.css`
- `fluent2.css`
- `material3.css`

## Add ContextMenu Component

Create a basic ContextMenu in your `src/App.tsx` or `src/App.jsx`:

```ts
import { ContextMenuComponent, MenuItemModel } from '@syncfusion/ej2-react-navigations';
import * as React from 'react';
import './App.css';

function App() {
  const menuItems: MenuItemModel[] = [
    { text: 'Cut' },
    { text: 'Copy' },
    { text: 'Paste' }
  ];

  return (
    <div>
      <div id="target">Right click / Touch hold to open the ContextMenu</div>
      <ContextMenuComponent target="#target" items={menuItems} />
    </div>
  );
}

export default App;
```

### Component Properties

- **`target`**: CSS selector for the element that triggers the context menu (right-click or touch-hold)
- **`items`**: Array of menu items using `MenuItemModel` interface
- **`text`**: Display text for each menu item

## Run the Application

Start the development server:

```bash
npm run dev
```

For `create-react-app`:

```bash
npm start
```

Open your browser and navigate to `http://localhost:5173` (Vite) or `http://localhost:3000` (CRA).

### Testing the Context Menu

1. Right-click on the target area (or touch-hold on mobile)
2. A popup menu appears with Cut, Copy, Paste options
3. Hover over items to see hover effects
4. Click an item to select it

## Complete Getting Started Example

```ts
import { enableRipple } from '@syncfusion/ej2-base';
import { ContextMenuComponent, MenuItemModel } from '@syncfusion/ej2-react-navigations';
import * as React from 'react';
import './App.css';

enableRipple(true);

function App() {
  const menuItems: MenuItemModel[] = [
    { text: 'Cut' },
    { text: 'Copy' },
    { text: 'Paste' },
    { separator: true },
    { text: 'Delete' }
  ];

  return (
    <div className="container">
      <h2>Context Menu Getting Started</h2>
      <div id="target">
        Right click / Touch hold to open the ContextMenu
      </div>
      <ContextMenuComponent 
        id="contextmenu" 
        target="#target" 
        items={menuItems}
      />
    </div>
  );
}

export default App;
```

## Accessing ContextMenu Methods

Use React refs to access component methods for programmatic control:

```ts
import { ContextMenuComponent, MenuItemModel } from '@syncfusion/ej2-react-navigations';
import * as React from 'react';

function App() {
  const menuRef = React.useRef<ContextMenuComponent>(null);

  const menuItems: MenuItemModel[] = [
    { text: 'Cut', id: 'cut' },
    { text: 'Copy', id: 'copy' },
    { text: 'Paste', id: 'paste' }
  ];

  // Programmatically open menu
  const openMenu = () => {
    menuRef.current?.open(100, 150);
  };

  // Programmatically close menu
  const closeMenu = () => {
    menuRef.current?.close();
  };

  // Enable/disable items
  const disablePaste = () => {
    menuRef.current?.enableItems(['Paste'], false);
  };

  // Show/hide items
  const hideDelete = () => {
    menuRef.current?.hideItems(['Delete']);
  };

  return (
    <div>
      <div id="target">Right click to open menu</div>
      
      <div style={{ marginTop: '20px' }}>
        <button onClick={openMenu}>Open Menu</button>
        <button onClick={closeMenu}>Close Menu</button>
        <button onClick={disablePaste}>Disable Paste</button>
        <button onClick={hideDelete}>Hide Delete</button>
      </div>

      <ContextMenuComponent
        ref={menuRef}
        target="#target"
        items={menuItems}
      />
    </div>
  );
}

export default App;
```

## Essential ContextMenu Methods

| Method | Purpose |
|--------|---------|
| `open(top, left, target?)` | Open menu at coordinates |
| `close()` | Close the menu |
| `enableItems(items[], enable?, isUniqueId?)` | Enable/disable items |
| `showItems(items[], isUniqueId?)` | Show hidden items |
| `hideItems(items[], isUniqueId?)` | Hide items from display |
| `removeItems(items[], isUniqueId?)` | Remove items from menu |
| `insertAfter(items[], text, isUniqueId?)` | Insert items after target |
| `insertBefore(items[], text, isUniqueId?)` | Insert items before target |
| `getItemIndex(item, isUniqueId?)` | Get item index/indices |
| `setItem(item, id?, isUniqueId?)` | Update item properties |
| `destroy()` | Clean up component |

## Component Properties Reference

### Core Properties

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `target` | string | - | CSS selector for trigger element |
| `items` | MenuItemModel[] | [] | Menu items array |
| `animationSettings` | MenuAnimationSettingsModel | - | Animation configuration |
| `enableScrolling` | boolean | false | Enable scrolling for large menus |
| `enableRtl` | boolean | false | Right-to-left layout |
| `cssClass` | string | - | Custom CSS classes |
| `filter` | string | - | Filter child elements |
| `hoverDelay` | number | 400 | Milliseconds before submenu opens |
| `showItemOnClick` | boolean | false | Force click to open submenus |
| `enableHtmlSanitizer` | boolean | true | Sanitize HTML content |
| `enablePersistence` | boolean | false | Persist state across sessions |
| `locale` | string | en-US | Localization language |
| `itemTemplate` | string \| Function | - | Custom item template |

### Event Properties

| Event | Trigger | Use Case |
|-------|---------|----------|
| `beforeOpen` | Before menu opens | Prevent/modify before opening |
| `onOpen` | After menu opens | Initialize, fetch data |
| `beforeClose` | Before menu closes | Prevent/save state |
| `onClose` | After menu closes | Cleanup |
| `select` | Item clicked | Execute action |
| `beforeItemRender` | Before item renders | Customize appearance |
| `created` | Component initialized | Setup |

## Next Steps

- **Customize menu items:** Add icons, nested menus, and custom templates (see [Menu Items and Data Binding](menu-items-and-data-binding.md))
- **Handle events:** Respond to item clicks with the `select` event (see [Events and Interaction](events-and-interaction.md))
- **Bind data:** Use local data sources to populate menu items dynamically
- **Enhance appearance:** Apply custom CSS and themes (see [Styling and Appearance](styling-and-appearance.md))
- **Ensure accessibility:** Implement keyboard navigation and ARIA support (see [Accessibility and Keyboard Navigation](accessibility-and-keyboard-navigation.md))
- **Explore advanced features:** Scrolling, animations, persistence (see [Advanced Features](advanced-features.md))
