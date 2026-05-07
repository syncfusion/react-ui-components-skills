---
title: Hamburger and Mobile Mode
parent_skill: Implementing Menu
description: Implementation guide for hamburger menus and mobile-responsive navigation
---

# Hamburger and Mobile Mode

## Table of Contents
1. [Overview](#overview)
2. [Basic Hamburger Setup](#basic-hamburger-setup)
3. [Hamburger Properties](#hamburger-properties)
4. [Programmatic Control](#programmatic-control)
5. [Mobile Responsive Patterns](#mobile-responsive-patterns)
6. [Integration with Sidebar](#integration-with-sidebar)
7. [Styling and Customization](#styling-and-customization)
8. [Complete Examples](#complete-examples)

## Overview

Hamburger mode is a mobile-first navigation pattern where the menu collapses into a hamburger icon (☰) on smaller screens. MenuComponent provides built-in support for this pattern through the `hamburgerMode` property.

**When to use hamburger mode:**
- Mobile and tablet applications
- Responsive web applications
- Space-constrained interfaces
- Progressive disclosure patterns

## Basic Hamburger Setup

### Simple Hamburger Menu

```jsx
import { MenuComponent, MenuItemModel } from '@syncfusion/ej2-react-navigations';

function App() {
  const items: MenuItemModel[] = [
    { text: 'Home' },
    { text: 'About' },
    {
      text: 'Services',
      items: [
        { text: 'Web Design' },
        { text: 'Development' },
        { text: 'Consulting' }
      ]
    },
    { text: 'Contact' }
  ];

  return (
    <MenuComponent 
      items={items}
      hamburgerMode={true}
      title="Navigation Menu"
    />
  );
}

export default App;
```

### With Responsive Header

```jsx
function ResponsiveApp() {
  const items = [
    { text: 'Home' },
    { text: 'Products' },
    { text: 'Services' },
    { text: 'Blog' },
    { text: 'Contact' }
  ];

  return (
    <div className="app">
      <header className="navbar">
        <div className="navbar-content">
          <h1 className="logo">My App</h1>
          <MenuComponent 
            items={items}
            hamburgerMode={true}
            title="Menu"
          />
        </div>
      </header>
      <main>
        {/* Main content */}
      </main>
    </div>
  );
}

export default ResponsiveApp;
```

## Hamburger Properties

### hamburgerMode Property
**Type:** `boolean`  
**Default:** `false`

```jsx
// Disable hamburger mode
<MenuComponent items={items} hamburgerMode={false} />

// Enable hamburger mode
<MenuComponent items={items} hamburgerMode={true} />

// Enable based on screen size
const [isMobile, setIsMobile] = useState(window.innerWidth < 768);

useEffect(() => {
  const handleResize = () => setIsMobile(window.innerWidth < 768);
  window.addEventListener('resize', handleResize);
  return () => window.removeEventListener('resize', handleResize);
}, []);

<MenuComponent 
  items={items}
  hamburgerMode={isMobile}
/>
```

### title Property
**Type:** `string`  
**Default:** `undefined`

```jsx
// With title
<MenuComponent 
  items={items}
  hamburgerMode={true}
  title="Navigation"
/>

// Without title
<MenuComponent 
  items={items}
  hamburgerMode={true}
/>

// Dynamic title
<MenuComponent 
  items={items}
  hamburgerMode={true}
  title={isOpen ? "Close Menu" : "Open Menu"}
/>
```

### target Property
**Type:** `string` (CSS selector)  
**Default:** `undefined`

```jsx
// Target specific navbar element
<MenuComponent 
  items={items}
  hamburgerMode={true}
  target=".navbar"
/>

// Target by ID
<MenuComponent 
  items={items}
  hamburgerMode={true}
  target="#main-nav"
/>

// Target by class
<MenuComponent 
  items={items}
  hamburgerMode={true}
  target=".header-nav"
/>
```

## Programmatic Control

### open() Method
Opens the hamburger menu programmatically.

```jsx
import { useRef } from 'react';

function App() {
  const menuRef = useRef(null);
  const items = [
    { text: 'Home' },
    { text: 'About' },
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
      />
    </>
  );
}

export default App;
```

### close() Method
Closes the hamburger menu programmatically.

```jsx
import { useRef } from 'react';

function App() {
  const menuRef = useRef(null);
  const items = [
    { text: 'Home' },
    { text: 'About' },
    { text: 'Contact' }
  ];

  const closeMenu = () => {
    menuRef.current?.close();
  };

  const handleSelect = (args) => {
    // Auto-close menu when item selected
    closeMenu();
  };

  return (
    <>
      <button onClick={closeMenu}>Close Menu</button>
      <MenuComponent 
        ref={menuRef}
        items={items}
        hamburgerMode={true}
        select={handleSelect}
      />
    </>
  );
}

export default App;
```

### Toggle Menu
```jsx
import { useRef, useState } from 'react';

function App() {
  const menuRef = useRef(null);
  const [isOpen, setIsOpen] = useState(false);
  const items = [
    { text: 'Home' },
    { text: 'About' },
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

  return (
    <>
      <button onClick={toggleMenu} className="toggle-btn">
        {isOpen ? '✕' : '☰'}
      </button>
      <MenuComponent 
        ref={menuRef}
        items={items}
        hamburgerMode={true}
        onOpen={() => setIsOpen(true)}
        onClose={() => setIsOpen(false)}
      />
    </>
  );
}

export default App;
```

## Mobile Responsive Patterns

### Responsive Breakpoint Handling
```jsx
import { useEffect, useState } from 'react';

function ResponsiveMenu() {
  const [isMobile, setIsMobile] = useState(window.innerWidth < 768);
  const items = [
    { text: 'Home' },
    { text: 'Products' },
    { text: 'Services' },
    { text: 'Blog' },
    { text: 'Contact' }
  ];

  useEffect(() => {
    const handleResize = () => {
      setIsMobile(window.innerWidth < 768);
    };

    window.addEventListener('resize', handleResize);
    return () => window.removeEventListener('resize', handleResize);
  }, []);

  return (
    <MenuComponent 
      items={items}
      hamburgerMode={isMobile}
      orientation={isMobile ? 'Vertical' : 'Horizontal'}
    />
  );
}

export default ResponsiveMenu;
```

### Multi-breakpoint Navigation
```jsx
import { useEffect, useState } from 'react';

function MultiBreakpointNav() {
  const [deviceType, setDeviceType] = useState('desktop');
  
  const items = [
    { text: 'Home' },
    { text: 'Products' },
    { text: 'Services' },
    { text: 'Contact' }
  ];

  useEffect(() => {
    const handleResize = () => {
      const width = window.innerWidth;
      if (width < 640) {
        setDeviceType('mobile');
      } else if (width < 1024) {
        setDeviceType('tablet');
      } else {
        setDeviceType('desktop');
      }
    };

    window.addEventListener('resize', handleResize);
    handleResize(); // Call once on mount
    return () => window.removeEventListener('resize', handleResize);
  }, []);

  return (
    <div className={`nav-${deviceType}`}>
      {deviceType === 'mobile' && (
        <MenuComponent 
          items={items}
          hamburgerMode={true}
          orientation="Vertical"
        />
      )}
      {deviceType !== 'mobile' && (
        <MenuComponent 
          items={items}
          hamburgerMode={false}
          orientation="Horizontal"
        />
      )}
    </div>
  );
}

export default MultiBreakpointNav;
```

## Integration with Sidebar

### Complete Sidebar + Hamburger Integration

```jsx
import { MenuComponent, MenuItemModel } from '@syncfusion/ej2-react-navigations';
import { SidebarComponent } from '@syncfusion/ej2-react-popups';
import { useRef, useState } from 'react';

function SidebarApp() {
  const sidebarRef = useRef(null);
  const menuRef = useRef(null);
  const [isMobile, setIsMobile] = useState(window.innerWidth < 768);

  const items: MenuItemModel[] = [
    { text: 'Dashboard' },
    {
      text: 'Settings',
      items: [
        { text: 'Profile' },
        { text: 'Account' },
        { text: 'Privacy' }
      ]
    },
    { text: 'Help' },
    { text: 'Logout' }
  ];

  const handleSelect = (args) => {
    console.log('Selected:', args.item.text);
    // In mobile, close sidebar after selection
    if (isMobile) {
      sidebarRef.current?.hide();
    }
  };

  const toggleSidebar = () => {
    sidebarRef.current?.toggle();
  };

  return (
    <div className="app-container">
      {/* Header */}
      <div className="app-header">
        <button 
          className="sidebar-toggle"
          onClick={toggleSidebar}
        >
          ☰
        </button>
        <h1>My Dashboard</h1>
      </div>

      {/* Sidebar with Menu */}
      <SidebarComponent 
        ref={sidebarRef}
        width="250px"
        type="Push"
        position="Left"
      >
        <MenuComponent 
          ref={menuRef}
          items={items}
          orientation="Vertical"
          select={handleSelect}
          hamburgerMode={false}
        />
      </SidebarComponent>

      {/* Main Content */}
      <div className="app-content">
        <h2>Welcome</h2>
        <p>Main content goes here</p>
      </div>
    </div>
  );
}

export default SidebarApp;
```

## Styling and Customization

### Custom Hamburger Styling

```css
/* CSS for hamburger menu styling */

.e-menu {
  --menu-bg: #ffffff;
  --menu-text: #333333;
  --menu-hover-bg: #f0f0f0;
  --menu-active-bg: #007bff;
}

.e-menu.hamburger-mode .e-menu-popup {
  max-width: 300px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.15);
}

.e-menu.hamburger-mode .e-menu-item {
  padding: 12px 16px;
  border-bottom: 1px solid #e0e0e0;
}

.e-menu.hamburger-mode .e-menu-item:hover {
  background-color: var(--menu-hover-bg);
}

.e-menu.hamburger-mode .e-menu-item.e-selected {
  background-color: var(--menu-active-bg);
  color: white;
}

/* Mobile-specific */
@media (max-width: 768px) {
  .e-menu {
    font-size: 16px; /* Prevents zoom on iOS */
  }
  
  .e-menu.hamburger-mode .e-menu-item {
    padding: 16px;
    font-size: 16px;
  }
}
```

### Dynamic Theme Application

```jsx
function ThemesHamburgerMenu() {
  const [theme, setTheme] = useState('light');
  const items = [
    { text: 'Home' },
    { text: 'About' },
    { text: 'Contact' }
  ];

  return (
    <div className={`app theme-${theme}`}>
      <button onClick={() => setTheme('light')}>Light</button>
      <button onClick={() => setTheme('dark')}>Dark</button>
      
      <MenuComponent 
        items={items}
        hamburgerMode={true}
        cssClass={`menu-${theme}`}
      />
    </div>
  );
}

export default ThemesHamburgerMenu;
```

## Complete Examples

### Example 1: Simple Mobile Navigation

```jsx
import { MenuComponent } from '@syncfusion/ej2-react-navigations';

function MobileNav() {
  const items = [
    { text: 'Home', iconCss: 'e-icons e-home' },
    { text: 'Shop', iconCss: 'e-icons e-shopping-cart' },
    { text: 'About', iconCss: 'e-icons e-info' },
    { text: 'Contact', iconCss: 'e-icons e-mail' }
  ];

  return (
    <div className="mobile-app">
      <header>
        <h1>Mobile Store</h1>
      </header>
      <MenuComponent 
        items={items}
        hamburgerMode={true}
        title="Menu"
        orientation="Vertical"
      />
    </div>
  );
}

export default MobileNav;
```

### Example 2: Responsive Navigation with State Persistence

```jsx
import { MenuComponent } from '@syncfusion/ej2-react-navigations';
import { useRef, useEffect } from 'react';

function PersistentMenu() {
  const menuRef = useRef(null);
  
  const items = [
    { text: 'Dashboard' },
    {
      text: 'Analytics',
      items: [
        { text: 'Reports' },
        { text: 'Metrics' }
      ]
    },
    { text: 'Users' },
    { text: 'Settings' }
  ];

  // Restore menu state after component mounts
  useEffect(() => {
    const savedState = localStorage.getItem('menuOpen');
    if (savedState === 'true') {
      menuRef.current?.open();
    }
  }, []);

  const handleOpen = () => {
    localStorage.setItem('menuOpen', 'true');
  };

  const handleClose = () => {
    localStorage.setItem('menuOpen', 'false');
  };

  return (
    <MenuComponent 
      ref={menuRef}
      items={items}
      hamburgerMode={true}
      enablePersistence={true}
      onOpen={handleOpen}
      onClose={handleClose}
    />
  );
}

export default PersistentMenu;
```

### Example 3: Full-Featured Responsive App

```jsx
import { MenuComponent } from '@syncfusion/ej2-react-navigations';
import { useRef, useState, useEffect } from 'react';

function FullApp() {
  const menuRef = useRef(null);
  const [isMobile, setIsMobile] = useState(window.innerWidth < 768);
  const [currentPage, setCurrentPage] = useState('home');

  const items = [
    { 
      text: 'Home',
      select: () => setCurrentPage('home')
    },
    {
      text: 'Products',
      items: [
        { text: 'Electronics' },
        { text: 'Books' },
        { text: 'Clothing' }
      ],
      select: () => setCurrentPage('products')
    },
    {
      text: 'Account',
      items: [
        { text: 'Profile' },
        { text: 'Orders' },
        { text: 'Logout' }
      ]
    }
  ];

  useEffect(() => {
    const handleResize = () => setIsMobile(window.innerWidth < 768);
    window.addEventListener('resize', handleResize);
    return () => window.removeEventListener('resize', handleResize);
  }, []);

  const handleItemSelect = (args) => {
    if (isMobile) {
      menuRef.current?.close();
    }
  };

  return (
    <div className="app">
      <header className="app-header">
        <h1>E-Store</h1>
      </header>

      <MenuComponent 
        ref={menuRef}
        items={items}
        hamburgerMode={isMobile}
        orientation={isMobile ? 'Vertical' : 'Horizontal'}
        select={handleItemSelect}
        title="Navigation"
      />

      <main className="app-content">
        {currentPage === 'home' && <h2>Welcome Home</h2>}
        {currentPage === 'products' && <h2>Our Products</h2>}
      </main>
    </div>
  );
}

export default FullApp;
```

## Best Practices

1. **Always provide feedback** - Show menu state (open/closed) visually
2. **Close menu on selection** - Improve UX by closing after navigation
3. **Responsive breakpoints** - Test at 375px, 768px, 1024px, 1280px
4. **Touch-friendly** - Ensure tap targets are at least 44×44px
5. **Keyboard support** - Ensure menu works with keyboard navigation
6. **State persistence** - Save menu state for better UX
7. **Performance** - Debounce resize events for smooth interaction

## Related Topics

- [Properties and Configuration](./properties-and-configuration.md) - All properties
- [Methods API](./methods-api.md) - open() and close() methods
- [Styling and Appearance](./styling-and-appearance.md) - Custom styling
- [Use Case Scenarios](./use-case-scenarios.md) - Real-world examples
