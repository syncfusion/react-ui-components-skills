# Styling and Appearance in React Menu Component

## Table of Contents
- [Icons Integration](#icons-integration)
- [Separator Support](#separator-support)
- [Mnemonic UI](#mnemonic-ui)
- [Right-to-Left Support](#right-to-left-support)
- [CSS Customization](#css-customization)
- [Theme Integration](#theme-integration)
- [Animation Settings](#animation-settings)
- [Custom Styling Examples](#custom-styling-examples)

## Icons Integration

Add icons to menu items using the `iconCss` property. Syncfusion includes the `e-icons` library:

### Basic Icon Usage

```jsx
import { MenuComponent, MenuItemModel } from '@syncfusion/ej2-react-navigations';
import * as React from 'react';

function App() {
  const menuItems: MenuItemModel[] = [
    {
      text: 'File',
      iconCss: 'e-icons e-file',
      items: [
        { text: 'New', iconCss: 'e-icons e-new' },
        { text: 'Open', iconCss: 'e-icons e-open' },
        { text: 'Save', iconCss: 'e-icons e-save' },
        { separator: true },
        { text: 'Exit', iconCss: 'e-icons e-exit' }
      ]
    },
    {
      text: 'Edit',
      iconCss: 'e-icons e-edit',
      items: [
        { text: 'Cut', iconCss: 'e-icons e-cut' },
        { text: 'Copy', iconCss: 'e-icons e-copy' },
        { text: 'Paste', iconCss: 'e-icons e-paste' }
      ]
    },
    {
      text: 'View',
      iconCss: 'e-icons e-view'
    }
  ];

  return (
    <MenuComponent items={menuItems}></MenuComponent>
  );
}

export default App;
```

### Common Icon Classes

```jsx
const iconClasses = {
  // File operations
  'file': 'e-icons e-file',
  'new': 'e-icons e-new',
  'open': 'e-icons e-open',
  'save': 'e-icons e-save',
  'delete': 'e-icons e-delete',
  'print': 'e-icons e-print',
  
  // Editing
  'edit': 'e-icons e-edit',
  'cut': 'e-icons e-cut',
  'copy': 'e-icons e-copy',
  'paste': 'e-icons e-paste',
  'undo': 'e-icons e-undo',
  'redo': 'e-icons e-redo',
  
  // Navigation
  'back': 'e-icons e-back',
  'forward': 'e-icons e-forward',
  'home': 'e-icons e-home',
  'menu': 'e-icons e-menu',
  
  // Settings
  'settings': 'e-icons e-settings',
  'user': 'e-icons e-user-profile',
  'logout': 'e-icons e-logout'
};

const menuItems = [
  {
    text: 'File',
    iconCss: iconClasses.file,
    items: [
      { text: 'New', iconCss: iconClasses.new }
    ]
  }
];
```

### Custom Icons

Use custom icon classes with Font Awesome or other libraries:

```jsx
const menuItems = [
  {
    text: 'File',
    iconCss: 'fas fa-file',  // Font Awesome icon
    items: [
      { text: 'New', iconCss: 'fas fa-file-alt' }
    ]
  },
  {
    text: 'Tools',
    iconCss: 'fas fa-tools',
    items: [
      { text: 'Settings', iconCss: 'fas fa-cog' }
    ]
  }
];

// Include Font Awesome in your HTML
// <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css">
```

## Separator Support

Separators provide visual grouping of related menu items:

```jsx
const menuItems = [
  {
    text: 'File',
    items: [
      { text: 'New' },
      { text: 'Open' },
      { text: 'Save' },
      { separator: true },  // Visual line separator
      { text: 'Recent Files' },
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
      { text: 'Paste' },
      { separator: true },
      { text: 'Select All' }
    ]
  }
];

<MenuComponent items={menuItems}></MenuComponent>
```

### Styling Separators

```css
/* Separator styling */
.e-separator {
  margin: 8px 0;
  height: 1px;
  background-color: #e0e0e0;
}

/* Dark theme separators */
.e-separator.dark {
  background-color: #444;
}

/* Colored separators */
.e-separator.primary {
  background-color: #007bff;
}
```

## Mnemonic UI

Add keyboard shortcuts (mnemonics) to menu items for quick access:

### Creating Mnemonic Items

```jsx
import { MenuComponent, MenuItemModel } from '@syncfusion/ej2-react-navigations';
import * as React from 'react';

function App() {
  const menuItems: MenuItemModel[] = [
    {
      text: '<u>F</u>ile',  // Underline indicates mnemonic key
      items: [
        { text: '<u>N</u>ew', htmlAttributes: { 'data-key': 'N' } },
        { text: '<u>O</u>pen', htmlAttributes: { 'data-key': 'O' } },
        { text: '<u>S</u>ave', htmlAttributes: { 'data-key': 'S' } }
      ]
    },
    {
      text: '<u>E</u>dit',
      items: [
        { text: 'C<u>u</u>t', htmlAttributes: { 'data-key': 'U' } },
        { text: '<u>C</u>opy', htmlAttributes: { 'data-key': 'C' } },
        { text: '<u>P</u>aste', htmlAttributes: { 'data-key': 'P' } }
      ]
    }
  ];

  React.useEffect(() => {
    // Handle keyboard shortcuts
    const handleKeyDown = (e) => {
      if (e.altKey) {
        const key = e.key.toUpperCase();
        // Activate menu item based on mnemonic
        activateMenuItemByKey(key);
      }
    };

    window.addEventListener('keydown', handleKeyDown);
    return () => window.removeEventListener('keydown', handleKeyDown);
  }, []);

  return (
    <MenuComponent items={menuItems}></MenuComponent>
  );
}

export default App;
```

### Styling Mnemonics

```css
/* Underlined mnemonic key */
.e-menu-item u {
  text-decoration: underline;
  text-decoration-color: currentColor;
  text-underline-offset: 2px;
}

/* Highlight mnemonic on focus */
.e-menu-item:focus u {
  color: #007bff;
  font-weight: bold;
}
```

## Right-to-Left Support

Enable RTL layout for right-to-left languages:

```jsx
import { enableRtl } from '@syncfusion/ej2-base';
import { MenuComponent } from '@syncfusion/ej2-react-navigations';

// Enable RTL globally
enableRtl(true);

function App() {
  const menuItems = [
    {
      text: 'الملف',  // Arabic: File
      items: [
        { text: 'جديد' },        // New
        { text: 'فتح' },         // Open
        { text: 'حفظ' }          // Save
      ]
    },
    {
      text: 'تحرير',  // Edit
      items: [
        { text: 'قص' },   // Cut
        { text: 'نسخ' },  // Copy
        { text: 'لصق' }   // Paste
      ]
    }
  ];

  return (
    <MenuComponent 
      items={menuItems}
      enableRtl={true}
    ></MenuComponent>
  );
}

export default App;
```

### Per-Component RTL

```jsx
<MenuComponent 
  items={menuItems}
  enableRtl={true}
></MenuComponent>
```

### CSS for RTL

```css
.e-rtl .e-menu {
  direction: rtl;
  text-align: right;
}

.e-rtl .e-menu-icon {
  margin-left: 8px;  /* Changed from margin-right */
  margin-right: 0;
}
```

## CSS Customization

### Adding Custom CSS Classes

```jsx
const menuItems = [
  {
    text: 'File',
    htmlAttributes: { 'class': 'file-menu important' },
    items: [
      { text: 'New', htmlAttributes: { 'class': 'new-item' } },
      { text: 'Exit', htmlAttributes: { 'class': 'exit-item danger' } }
    ]
  }
];

// Custom styles
const styles = `
  .file-menu {
    font-weight: bold;
    color: #0066cc;
  }

  .file-menu.important {
    text-transform: uppercase;
  }

  .new-item {
    color: #28a745;
  }

  .exit-item.danger {
    color: #dc3545;
  }

  .exit-item.danger:hover {
    background-color: #f8d7da;
  }
`;
```

### Styling Menu Wrapper

```css
/* Menu container */
.e-menu {
  background-color: #f8f9fa;
  border: 1px solid #dee2e6;
  border-radius: 4px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

/* Menu items */
.e-menu-item {
  padding: 8px 16px;
  transition: background-color 0.2s ease;
}

.e-menu-item:hover {
  background-color: #e9ecef;
}

.e-menu-item.e-selected {
  background-color: #0066cc;
  color: white;
}

/* Sub-menu popup */
.e-submenu {
  border: 1px solid #dee2e6;
  border-radius: 4px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.15);
}

/* Icons */
.e-menu-icon {
  width: 20px;
  height: 20px;
  margin-right: 8px;
  vertical-align: middle;
}

/* Separator */
.e-separator {
  margin: 8px 0;
  height: 1px;
  background-color: #dee2e6;
}
```

## Theme Integration

### Using Syncfusion Themes

Apply different themes by changing CSS imports:

```css
/* Tailwind theme (default) */
@import "../node_modules/@syncfusion/ej2-navigations/styles/tailwind3.css";

/* Or use alternative themes */
/* @import "../node_modules/@syncfusion/ej2-navigations/styles/bootstrap5.3.css"; */
/* @import "../node_modules/@syncfusion/ej2-navigations/styles/fluent2.css"; */
/* @import "../node_modules/@syncfusion/ej2-navigations/styles/material3.css"; */
```

### Light and Dark Modes

```jsx
function App() {
  const [isDarkMode, setIsDarkMode] = React.useState(false);

  React.useEffect(() => {
    if (isDarkMode) {
      document.body.classList.add('dark-theme');
    } else {
      document.body.classList.remove('dark-theme');
    }
  }, [isDarkMode]);

  return (
    <div>
      <button onClick={() => setIsDarkMode(!isDarkMode)}>
        Toggle Dark Mode
      </button>
      <MenuComponent items={menuItems}></MenuComponent>
    </div>
  );
}
```

### Dark Mode Styles

```css
.dark-theme {
  background-color: #1e1e1e;
  color: #e0e0e0;
}

.dark-theme .e-menu {
  background-color: #2d2d2d;
  border-color: #444;
}

.dark-theme .e-menu-item {
  color: #e0e0e0;
}

.dark-theme .e-menu-item:hover {
  background-color: #3d3d3d;
}

.dark-theme .e-separator {
  background-color: #444;
}
```

## Animation Settings

Control menu opening and closing animations:

```jsx
import { MenuAnimationSettings, MenuComponent } from '@syncfusion/ej2-react-navigations';

function App() {
  const animationSettings: MenuAnimationSettings = {
    effect: 'SlideDown',      // Animation effect
    duration: 400,            // Duration in milliseconds
    easing: 'ease-out'        // Easing function
  };

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

  return (
    <MenuComponent 
      items={menuItems}
      animationSettings={animationSettings}
    ></MenuComponent>
  );
}
```

### Available Animation Effects

```jsx
// SlideDown - Default
animationSettings = { effect: 'SlideDown', duration: 300 };

// Fade
animationSettings = { effect: 'Fade', duration: 300 };

// ZoomIn
animationSettings = { effect: 'ZoomIn', duration: 300 };

// SlideLeft
animationSettings = { effect: 'SlideLeft', duration: 300 };

// None (no animation)
animationSettings = { effect: 'None', duration: 0 };
```

## Custom Styling Examples

### Modern Dark Menu

```jsx
const styles = `
  .modern-menu .e-menu {
    background: linear-gradient(135deg, #1e1e1e 0%, #2d2d2d 100%);
    border: none;
    border-radius: 8px;
    box-shadow: 0 8px 16px rgba(0, 0, 0, 0.3);
  }

  .modern-menu .e-menu-item {
    color: #e0e0e0;
    padding: 10px 16px;
    border-left: 3px solid transparent;
    transition: all 0.3s ease;
  }

  .modern-menu .e-menu-item:hover {
    background-color: #0066cc;
    border-left-color: #00d4ff;
    color: white;
  }

  .modern-menu .e-menu-icon {
    color: #00d4ff;
  }
`;
```

### Material Design Menu

```jsx
const animationSettings: MenuAnimationSettings = {
  effect: 'Fade',
  duration: 200,
  easing: 'ease-in-out'
};

// Include Material theme CSS
// @import "../node_modules/@syncfusion/ej2-navigations/styles/material3.css";

<MenuComponent 
  items={menuItems}
  animationSettings={animationSettings}
  className="material-menu"
></MenuComponent>
```

## Related Topics
- [Getting Started](getting-started.md) - Basic styling setup
- [Orientation and Layout](orientation-and-layout.md) - Layout customization
