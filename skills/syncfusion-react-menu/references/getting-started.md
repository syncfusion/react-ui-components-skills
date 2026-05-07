# Getting Started with React Menu Component

## Table of Contents
- [Dependencies](#dependencies)
- [Development Environment Setup](#development-environment-setup)
- [Installing Syncfusion Packages](#installing-syncfusion-packages)
- [Adding Stylesheets](#adding-stylesheets)
- [Creating Your First Menu](#creating-your-first-menu)
- [MenuItemModel Structure](#menuitemmodel-structure)
- [Rendering and Initialization](#rendering-and-initialization)

## Dependencies

The Menu component requires the following packages:

```
@syncfusion/ej2-react-navigations
├── @syncfusion/ej2-react-base
├── @syncfusion/ej2-navigations
└── @syncfusion/ej2-base
```

These dependencies handle the core navigation functionality, base UI utilities, and React integration.

## Development Environment Setup

### Using Vite (Recommended)

Vite provides faster development and smaller bundle sizes compared to create-react-app.

**Create a new React application with JavaScript:**
```bash
npm create vite@latest my-app -- --template react
cd my-app
npm run dev
```

**Create a new React application with TypeScript:**
```bash
npm create vite@latest my-app -- --template react-ts
cd my-app
npm run dev
```

### Using create-react-app

Alternatively, you can use the traditional create-react-app setup:

```bash
npx create-react-app my-app
cd my-app
npm start
```

## Installing Syncfusion Packages

Install the Menu component and its dependencies:

```bash
npm install @syncfusion/ej2-react-navigations --save
```

This command automatically installs all required peer dependencies.

## Adding Stylesheets

Add the Menu component's CSS imports to your main `App.css` file. Include styles for all required Syncfusion dependencies:

```css
/* Base styles */
@import "../node_modules/@syncfusion/ej2-base/styles/tailwind3.css";

/* Button styles (used by Menu) */
@import "../node_modules/@syncfusion/ej2-buttons/styles/tailwind3.css";

/* Popup styles (for sub-menus) */
@import "../node_modules/@syncfusion/ej2-popups/styles/tailwind3.css";

/* Navigation and Menu styles */
@import "../node_modules/@syncfusion/ej2-navigations/styles/tailwind3.css";
```

**Available themes:**
- `tailwind3.css` (default)
- `bootstrap5.3.css`
- `fluent2.css`
- `material3.css`

Choose the theme that matches your design requirements and replace `tailwind3` accordingly.

## Creating Your First Menu

### Basic Menu with MenuItemModel

Create a functional component with a simple menu structure:

```jsx
import { MenuComponent, MenuItemModel } from '@syncfusion/ej2-react-navigations';
import * as React from 'react';
import './App.css';

function App() {
  // Define menu items
  let menuItems: MenuItemModel[] = [
    {
      text: 'File',
      items: [
        { text: 'New' },
        { text: 'Open' },
        { text: 'Save' },
        { text: 'Close' }
      ]
    },
    {
      text: 'Edit',
      items: [
        { text: 'Cut' },
        { text: 'Copy' },
        { text: 'Paste' }
      ]
    },
    {
      text: 'View',
      items: [
        { text: 'Toolbar' },
        { text: 'Sidebar' }
      ]
    },
    {
      text: 'Tools',
      items: [
        { text: 'Options' },
        { text: 'Settings' }
      ]
    },
    {
      text: 'Help'
    }
  ];

  return (
    <div className="App">
      <h1>Menu Component</h1>
      <MenuComponent items={menuItems}></MenuComponent>
    </div>
  );
}

export default App;
```

## MenuItemModel Structure

MenuItemModel defines the properties for each menu item:

```jsx
interface MenuItemModel {
  // Text displayed for the menu item
  text?: string;

  // Array of sub-menu items (creates hierarchical menus)
  items?: MenuItemModel[];

  // CSS ID for the item element
  id?: string;

  // URL for navigation links
  url?: string;

  // Icon CSS class (e.g., 'e-icons e-cut')
  iconCss?: string;

  // Icon for selected state
  iconCss?: string;

  // HTML attributes (data-*, aria-*, etc.)
  htmlAttributes?: { [key: string]: string };

  // Separator line between items
  separator?: boolean;

  // Disable the menu item
  disabled?: boolean;

  // Hide the menu item
  hidden?: boolean;
}
```

### Practical Examples

**Menu with icons:**
```jsx
const menuItems: MenuItemModel[] = [
  {
    text: 'File',
    iconCss: 'e-icons e-file',
    items: [
      { text: 'New', iconCss: 'e-icons e-new' },
      { text: 'Open', iconCss: 'e-icons e-open' },
      { separator: true },
      { text: 'Exit', iconCss: 'e-icons e-exit' }
    ]
  }
];
```

**Menu with separators:**
```jsx
const menuItems: MenuItemModel[] = [
  { text: 'File' },
  { text: 'Edit' },
  { separator: true },
  { text: 'Help' }
];
```

**Menu with mixed items:**
```jsx
const menuItems: MenuItemModel[] = [
  {
    text: 'File',
    items: [
      { text: 'New' },
      { text: 'Open' },
      { separator: true },
      { text: 'Recent Files' },
      { separator: true },
      { text: 'Exit' }
    ]
  }
];
```

## Rendering and Initialization

### Enable Ripple Effect (Optional)

For Material Design ripple effects on menu items:

```jsx
import { enableRipple } from '@syncfusion/ej2-base';
import { MenuComponent, MenuItemModel } from '@syncfusion/ej2-react-navigations';

enableRipple(true);

function App() {
  // Component code...
}
```

### Complete Application Example

```jsx
import { enableRipple } from '@syncfusion/ej2-base';
import { MenuComponent, MenuItemModel } from '@syncfusion/ej2-react-navigations';
import * as React from 'react';
import './App.css';

enableRipple(true);

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
        { text: 'Paste' }
      ]
    }
  ];

  return (
    <div className="App">
      <MenuComponent items={menuItems}></MenuComponent>
    </div>
  );
}

export default App;
```

### Styling the Menu Container

Add CSS to style the menu wrapper:

```css
.App {
  max-width: 1200px;
  margin: 0 auto;
  padding: 20px;
}

/* Menu container styling */
.e-menu {
  background-color: #f5f5f5;
  border: 1px solid #ddd;
  border-radius: 4px;
}

/* Menu item hover state */
.e-menu-item:hover {
  background-color: #e0e0e0;
}
```

## Next Steps

- Read [data-binding.md](data-binding.md) to populate menus from JSON data sources
- Read [events-and-interactions.md](events-and-interactions.md) to respond to menu item clicks
- Read [menu-items-customization.md](menu-items-customization.md) to dynamically add/remove items
