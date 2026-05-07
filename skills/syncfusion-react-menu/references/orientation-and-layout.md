# Orientation and Layout in React Menu Component

## Table of Contents
- [Horizontal vs Vertical Orientation](#horizontal-vs-vertical-orientation)
- [Changing Orientation](#changing-orientation)
- [Sub-Menu Positioning](#sub-menu-positioning)
- [Horizontal Scrolling](#horizontal-scrolling)
- [Vertical Scrolling](#vertical-scrolling)
- [Responsive Layout](#responsive-layout)
- [Practical Layout Patterns](#practical-layout-patterns)

## Horizontal vs Vertical Orientation

The Menu component supports two primary orientations:

### Vertical Orientation (Default)

Traditional dropdown menus where items stack vertically:

```jsx
import { MenuComponent, MenuItemModel, Orientation } from '@syncfusion/ej2-react-navigations';
import * as React from 'react';

function App() {
  const menuItems: MenuItemModel[] = [
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
    },
    {
      text: 'View'
    }
  ];

  // Vertical is default, explicit for clarity
  return (
    <MenuComponent 
      items={menuItems}
      orientation={Orientation.Vertical}
    ></MenuComponent>
  );
}

export default App;
```

### Horizontal Orientation

Menu items display horizontally in a row, typically used for top navigation bars:

```jsx
import { Orientation } from '@syncfusion/ej2-react-navigations';

function App() {
  return (
    <MenuComponent 
      items={menuItems}
      orientation={Orientation.Horizontal}
    ></MenuComponent>
  );
}
```

## Changing Orientation

### Orientation Property

```jsx
interface MenuComponent {
  orientation?: Orientation;
}

enum Orientation {
  Vertical = 'Vertical',
  Horizontal = 'Horizontal'
}
```

### Complete Horizontal Menu Example

```jsx
import { MenuComponent, MenuItemModel, Orientation } from '@syncfusion/ej2-react-navigations';
import * as React from 'react';
import './App.css';

function App() {
  const menuItems: MenuItemModel[] = [
    {
      text: 'Home',
      url: '/'
    },
    {
      text: 'Products',
      items: [
        { text: 'Electronics' },
        { text: 'Clothing' },
        { text: 'Books' }
      ]
    },
    {
      text: 'Services',
      items: [
        { text: 'Consulting' },
        { text: 'Support' }
      ]
    },
    {
      text: 'About Us'
    },
    {
      text: 'Contact',
      url: '/contact'
    }
  ];

  return (
    <nav className="navbar">
      <MenuComponent 
        items={menuItems}
        orientation={Orientation.Horizontal}
      ></MenuComponent>
    </nav>
  );
}

// CSS
const styles = `
  .navbar {
    background-color: #333;
    padding: 0;
  }

  .navbar .e-menu {
    background-color: transparent;
    border: none;
  }

  .navbar .e-menu-item {
    color: white;
    padding: 12px 20px;
  }

  .navbar .e-menu-item:hover {
    background-color: #0066cc;
  }
`;

export default App;
```

## Sub-Menu Positioning

Control where sub-menus appear relative to their parent items:

### Vertical Sub-Menu Positions

For **Vertical** menus, sub-menus appear to the right or left:

```jsx
import { MenuComponent, Orientation, SubMenuPosition } from '@syncfusion/ej2-react-navigations';

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

  // Sub-menu opens to the right (default)
  return (
    <MenuComponent 
      items={menuItems}
      orientation={Orientation.Vertical}
      // SubMenuPosition.RightBefore or RightAfter
    ></MenuComponent>
  );
}
```

### Horizontal Sub-Menu Positions

For **Horizontal** menus, sub-menus appear above or below:

```jsx
function App() {
  // Sub-menu opens below parent (default)
  return (
    <MenuComponent 
      items={menuItems}
      orientation={Orientation.Horizontal}
      // Sub-menus drop down below the menu bar
    ></MenuComponent>
  );
}
```

### Sub-Menu Positioning Behavior

```jsx
const menuItems = [
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
      { text: 'Toolbars' },
      { text: 'Sidebar' },
      { text: 'Status Bar' }
    ]
  }
];

// The menu automatically positions sub-menus to prevent viewport overflow
<MenuComponent 
  items={menuItems}
  orientation={Orientation.Horizontal}
></MenuComponent>
```

## Horizontal Scrolling

Enable horizontal scrolling for menus with many items:

```jsx
import { MenuComponent, HScrollSettings, HScroll } from '@syncfusion/ej2-react-navigations';

function App() {
  // Many items requiring horizontal scroll
  const menuItems = Array.from({ length: 50 }, (_, i) => ({
    text: `Item ${i + 1}`,
    id: `item-${i + 1}`
  }));

  const hScrollSettings: HScrollSettings = {
    enable: true,
    scrollStep: 100  // Pixels to scroll per arrow click
  };

  return (
    <MenuComponent 
      items={menuItems}
      orientation={Orientation.Horizontal}
      hScroll={hScrollSettings}
    ></MenuComponent>
  );
}
```

### HScrollSettings Configuration

```jsx
interface HScrollSettings {
  // Enable/disable horizontal scroll
  enable?: boolean;

  // Distance (in pixels) to scroll per arrow click
  scrollStep?: number;
}

// Example with custom scroll step
const hScrollSettings: HScrollSettings = {
  enable: true,
  scrollStep: 200  // Scroll 200px per click
};
```

## Vertical Scrolling

Enable vertical scrolling for menus with many items:

```jsx
import { MenuComponent, VScrollSettings } from '@syncfusion/ej2-react-navigations';

function App() {
  // Many items in a sub-menu requiring vertical scroll
  const menuItems = [
    {
      text: 'Countries',
      items: Array.from({ length: 100 }, (_, i) => ({
        text: `Country ${i + 1}`,
        id: `country-${i + 1}`
      }))
    }
  ];

  const vScrollSettings: VScrollSettings = {
    enable: true,
    scrollStep: 50,
    scrollHeight: '300px'  // Max height before scrolling
  };

  return (
    <MenuComponent 
      items={menuItems}
      vScroll={vScrollSettings}
    ></MenuComponent>
  );
}
```

### VScrollSettings Configuration

```jsx
interface VScrollSettings {
  // Enable/disable vertical scroll
  enable?: boolean;

  // Distance (in pixels) to scroll per arrow click
  scrollStep?: number;

  // Maximum height of scrollable area
  scrollHeight?: string;  // e.g., '300px', '500px'
}

// Example with custom height
const vScrollSettings: VScrollSettings = {
  enable: true,
  scrollStep: 100,
  scrollHeight: '250px'
};
```

### Practical Vertical Scroll Example

```jsx
function App() {
  const dataSource = [
    {
      category: 'Fruits',
      items: [
        { fruit: 'Apple' },
        { fruit: 'Banana' },
        { fruit: 'Orange' },
        { fruit: 'Mango' },
        { fruit: 'Pineapple' },
        { fruit: 'Kiwi' },
        { fruit: 'Grape' },
        { fruit: 'Watermelon' },
        { fruit: 'Strawberry' },
        { fruit: 'Blueberry' }
      ]
    }
  ];

  const fields = {
    text: 'category',
    children: 'items'
  };

  const vScrollSettings: VScrollSettings = {
    enable: true,
    scrollHeight: '300px'
  };

  return (
    <MenuComponent 
      items={dataSource}
      fields={fields}
      vScroll={vScrollSettings}
    ></MenuComponent>
  );
}
```

## Responsive Layout

Adapt menu layout based on screen size:

### Media Queries Approach

```css
/* Desktop: Horizontal menu */
@media (min-width: 768px) {
  .navbar .e-menu {
    display: flex;
    flex-direction: row;
  }

  .navbar .e-menu-item {
    padding: 12px 20px;
    border-right: 1px solid #ddd;
  }
}

/* Mobile: Vertical menu (hamburger) */
@media (max-width: 768px) {
  .navbar {
    position: relative;
  }

  .navbar .e-menu {
    display: none;
    position: absolute;
    top: 100%;
    left: 0;
    right: 0;
    flex-direction: column;
    background-color: #333;
    z-index: 1000;
  }

  .navbar .e-menu.show {
    display: flex;
  }

  .navbar .e-menu-item {
    padding: 12px 16px;
    border-bottom: 1px solid #555;
  }
}
```

### JavaScript Responsive Menu

```jsx
import { MenuComponent, Orientation } from '@syncfusion/ej2-react-navigations';
import * as React from 'react';

function App() {
  const [isMobile, setIsMobile] = React.useState(window.innerWidth < 768);

  React.useEffect(() => {
    const handleResize = () => {
      setIsMobile(window.innerWidth < 768);
    };

    window.addEventListener('resize', handleResize);
    return () => window.removeEventListener('resize', handleResize);
  }, []);

  const menuItems = [
    { text: 'Home' },
    { text: 'Products', items: [{ text: 'Electronics' }, { text: 'Books' }] },
    { text: 'Services' },
    { text: 'Contact' }
  ];

  return (
    <MenuComponent 
      items={menuItems}
      orientation={isMobile ? Orientation.Vertical : Orientation.Horizontal}
      className={isMobile ? 'mobile-menu' : 'desktop-menu'}
    ></MenuComponent>
  );
}

export default App;
```

## Practical Layout Patterns

### Pattern 1: Top Navigation Bar with Responsive Hamburger

```jsx
function App() {
  const [showMenu, setShowMenu] = React.useState(false);
  const [isMobile, setIsMobile] = React.useState(window.innerWidth < 768);

  React.useEffect(() => {
    const handleResize = () => {
      setIsMobile(window.innerWidth < 768);
    };

    window.addEventListener('resize', handleResize);
    return () => window.removeEventListener('resize', handleResize);
  }, []);

  const menuItems = [
    { text: 'Home', url: '/' },
    {
      text: 'Products',
      items: [
        { text: 'Electronics' },
        { text: 'Clothing' }
      ]
    },
    { text: 'Contact', url: '/contact' }
  ];

  return (
    <nav className="navbar">
      {isMobile && (
        <button 
          className="hamburger"
          onClick={() => setShowMenu(!showMenu)}
        >
          ☰
        </button>
      )}
      <MenuComponent 
        items={menuItems}
        orientation={isMobile ? Orientation.Vertical : Orientation.Horizontal}
        className={isMobile && !showMenu ? 'hidden' : ''}
      ></MenuComponent>
    </nav>
  );
}

// CSS
const styles = `
  .navbar {
    background-color: #333;
    display: flex;
    align-items: center;
  }

  .hamburger {
    display: none;
    background: none;
    border: none;
    color: white;
    font-size: 24px;
    cursor: pointer;
    padding: 12px 16px;
  }

  @media (max-width: 768px) {
    .hamburger {
      display: block;
    }

    .navbar .e-menu.hidden {
      display: none;
    }
  }
`;
```

### Pattern 2: Multi-Column Horizontal Menu

```jsx
function App() {
  const menuItems = [
    { text: 'Home' },
    {
      text: 'Products',
      items: [
        { text: 'Category 1', items: [{ text: 'Item 1' }, { text: 'Item 2' }] },
        { text: 'Category 2', items: [{ text: 'Item 3' }, { text: 'Item 4' }] }
      ]
    },
    { text: 'Services' }
  ];

  return (
    <MenuComponent 
      items={menuItems}
      orientation={Orientation.Horizontal}
      className="multi-column-menu"
    ></MenuComponent>
  );
}

const styles = `
  .multi-column-menu .e-submenu {
    display: flex;
    flex-wrap: wrap;
    max-width: 600px;
  }

  .multi-column-menu .e-menu-item {
    flex: 1 1 50%;
    min-width: 150px;
  }
`;
```

### Pattern 3: Vertical Sidebar Navigation

```jsx
function SidebarMenu() {
  const menuItems = [
    { text: 'Dashboard', iconCss: 'e-icons e-home' },
    { text: 'Users', iconCss: 'e-icons e-user-profile' },
    { text: 'Settings', iconCss: 'e-icons e-settings' },
    {
      text: 'Admin',
      iconCss: 'e-icons e-lock',
      items: [
        { text: 'System Settings' },
        { text: 'User Roles' },
        { text: 'Permissions' }
      ]
    }
  ];

  return (
    <aside className="sidebar">
      <MenuComponent 
        items={menuItems}
        orientation={Orientation.Vertical}
        className="sidebar-menu"
      ></MenuComponent>
    </aside>
  );
}

const styles = `
  .sidebar {
    width: 250px;
    background-color: #2c3e50;
    height: 100vh;
    overflow-y: auto;
  }

  .sidebar-menu .e-menu {
    border: none;
    background-color: transparent;
  }

  .sidebar-menu .e-menu-item {
    color: #ecf0f1;
    padding: 12px 16px;
    border-left: 3px solid transparent;
    transition: all 0.3s;
  }

  .sidebar-menu .e-menu-item:hover {
    background-color: #34495e;
    border-left-color: #3498db;
  }
`;
```

## Related Topics
- [Getting Started](getting-started.md) - Basic setup
- [Styling and Appearance](styling-and-appearance.md) - Styling options
