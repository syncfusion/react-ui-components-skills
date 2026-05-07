---
title: Use Case Scenarios
parent_skill: Implementing Menu
description: Real-world use cases and implementation patterns for MenuComponent
---

# Use Case Scenarios

## Table of Contents
1. [File Menu (Desktop App Pattern)](#file-menu-desktop-app-pattern)
2. [Scrollable Menu](#scrollable-menu)
3. [Toolbar Menu Integration](#toolbar-menu-integration)
4. [Hamburger Menu with Sidebar](#hamburger-menu-with-sidebar)
5. [Mobile Navigation](#mobile-navigation)
6. [Breadcrumb Navigation](#breadcrumb-navigation)
7. [Context Menu](#context-menu)
8. [Dashboard Navigation](#dashboard-navigation)

## File Menu (Desktop App Pattern)

Classic file menu with keyboard shortcuts and icons.

```jsx
import { MenuComponent, MenuItemModel } from '@syncfusion/ej2-react-navigations';

function DesktopAppMenu() {
  const items: MenuItemModel[] = [
    {
      text: 'File',
      items: [
        { text: 'New', iconCss: 'e-icons e-new', htmlAttributes: { title: 'Ctrl+N' } },
        { text: 'Open', iconCss: 'e-icons e-open', htmlAttributes: { title: 'Ctrl+O' } },
        { separator: true },
        { text: 'Save', iconCss: 'e-icons e-save', htmlAttributes: { title: 'Ctrl+S' } },
        { text: 'Save As...', iconCss: 'e-icons e-save-as' },
        { separator: true },
        { text: 'Exit', htmlAttributes: { title: 'Alt+F4' } }
      ]
    },
    {
      text: 'Edit',
      items: [
        { text: 'Undo', iconCss: 'e-icons e-undo', disabled: true },
        { text: 'Redo', iconCss: 'e-icons e-redo', disabled: true },
        { separator: true },
        { text: 'Cut', iconCss: 'e-icons e-cut' },
        { text: 'Copy', iconCss: 'e-icons e-copy' },
        { text: 'Paste', iconCss: 'e-icons e-paste' }
      ]
    },
    {
      text: 'View',
      items: [
        { text: 'Zoom In', iconCss: 'e-icons e-zoom-in' },
        { text: 'Zoom Out', iconCss: 'e-icons e-zoom-out' },
        { text: 'Reset', iconCss: 'e-icons e-zoom-reset' }
      ]
    },
    {
      text: 'Help',
      items: [
        { text: 'Documentation', iconCss: 'e-icons e-help' },
        { text: 'About', iconCss: 'e-icons e-info' }
      ]
    }
  ];

  const handleSelect = (args) => {
    const action = args.item.text;
    console.log(`Executing: ${action}`);
    // Handle actual menu actions
  };

  return (
    <div className="desktop-app">
      <div className="menu-bar">
        <MenuComponent 
          items={items}
          select={handleSelect}
          orientation="Horizontal"
        />
      </div>
      <div className="content">
        <p>Application content here</p>
      </div>
    </div>
  );
}

export default DesktopAppMenu;
```

## Scrollable Menu

Menu with vertical scrolling for large item lists.

```jsx
import { MenuComponent, MenuItemModel } from '@syncfusion/ej2-react-navigations';

function ScrollableMenuExample() {
  // Generate large item list
  const items: MenuItemModel[] = [
    { text: 'Quick Links' },
    { separator: true },
    ...Array.from({ length: 50 }, (_, i) => ({
      text: `Link ${i + 1}`
    }))
  ];

  return (
    <div className="scrollable-menu-container">
      <h3>Scrollable Menu (Click to open)</h3>
      <MenuComponent 
        items={items}
        enableScrolling={true}
        orientation="Vertical"
        cssClass="scrollable-menu"
      />
      
      <style>{`
        .scrollable-menu-container {
          width: 250px;
        }

        .scrollable-menu {
          max-height: 400px;
          overflow-y: auto;
          border: 1px solid #ccc;
          border-radius: 4px;
        }

        .scrollable-menu .e-menu-item {
          padding: 10px 15px;
        }
      `}</style>
    </div>
  );
}

export default ScrollableMenuExample;
```

### Horizontal Scrolling Menu

```jsx
function HorizontalScrollableMenu() {
  const items = Array.from({ length: 30 }, (_, i) => ({
    text: `Tab ${i + 1}`
  }));

  return (
    <MenuComponent 
      items={items}
      orientation="Horizontal"
      enableScrolling={true}
      hScroll={{ scrollStep: 3 }}
    />
  );
}

export default HorizontalScrollableMenu;
```

## Toolbar Menu Integration

Menu integrated into application toolbar.

```jsx
import { MenuComponent } from '@syncfusion/ej2-react-navigations';
import { ToolbarComponent } from '@syncfusion/ej2-react-navigations';

function ToolbarWithMenu() {
  const menuItems = [
    {
      text: 'File',
      items: [
        { text: 'New Document' },
        { text: 'Open' },
        { text: 'Save' }
      ]
    },
    {
      text: 'Insert',
      items: [
        { text: 'Image' },
        { text: 'Table' },
        { text: 'Link' }
      ]
    },
    {
      text: 'Format',
      items: [
        { text: 'Bold' },
        { text: 'Italic' },
        { text: 'Underline' }
      ]
    }
  ];

  return (
    <div className="editor-app">
      <div className="toolbar-container">
        <div className="logo">
          <h2>Text Editor</h2>
        </div>
        <MenuComponent 
          items={menuItems}
          orientation="Horizontal"
          cssClass="toolbar-menu"
        />
      </div>
      
      <textarea 
        className="editor"
        placeholder="Start typing..."
      />

      <style>{`
        .toolbar-container {
          display: flex;
          align-items: center;
          gap: 20px;
          padding: 10px;
          border-bottom: 2px solid #007bff;
          background: #f8f9fa;
        }

        .toolbar-menu .e-menu-item {
          padding: 8px 16px;
        }

        .editor {
          width: 100%;
          height: 500px;
          padding: 15px;
          font-family: 'Courier New', monospace;
          font-size: 14px;
        }
      `}</style>
    </div>
  );
}

export default ToolbarWithMenu;
```

## Hamburger Menu with Sidebar

Mobile-first design with hamburger menu and sidebar integration.

```jsx
import { MenuComponent } from '@syncfusion/ej2-react-navigations';
import { SidebarComponent } from '@syncfusion/ej2-react-popups';
import { useRef, useState, useEffect } from 'react';

function HamburgerSidebarApp() {
  const sidebarRef = useRef(null);
  const menuRef = useRef(null);
  const [isMobile, setIsMobile] = useState(window.innerWidth < 768);
  const [currentSection, setCurrentSection] = useState('dashboard');

  const items = [
    {
      text: 'Dashboard',
      iconCss: 'e-icons e-dashboard'
    },
    {
      text: 'Analytics',
      iconCss: 'e-icons e-chart',
      items: [
        { text: 'Sales Report' },
        { text: 'Traffic Analysis' },
        { text: 'Performance Metrics' }
      ]
    },
    {
      text: 'Products',
      iconCss: 'e-icons e-box',
      items: [
        { text: 'All Products' },
        { text: 'Categories' },
        { text: 'Inventory' }
      ]
    },
    {
      text: 'Users',
      iconCss: 'e-icons e-people'
    },
    {
      text: 'Settings',
      iconCss: 'e-icons e-settings',
      items: [
        { text: 'Account' },
        { text: 'Preferences' },
        { text: 'Notifications' }
      ]
    }
  ];

  useEffect(() => {
    const handleResize = () => setIsMobile(window.innerWidth < 768);
    window.addEventListener('resize', handleResize);
    return () => window.removeEventListener('resize', handleResize);
  }, []);

  const handleToggleSidebar = () => {
    sidebarRef.current?.toggle();
  };

  const handleMenuSelect = (args) => {
    setCurrentSection(args.item.text.toLowerCase());
    if (isMobile) {
      sidebarRef.current?.hide();
    }
  };

  return (
    <div className="app-layout">
      {/* Header */}
      <header className="app-header">
        <button 
          className="sidebar-toggle"
          onClick={handleToggleSidebar}
        >
          ☰
        </button>
        <h1 className="app-title">Admin Dashboard</h1>
      </header>

      {/* Sidebar */}
      <SidebarComponent
        ref={sidebarRef}
        width="250px"
        type={isMobile ? "Over" : "Push"}
        position="Left"
        className="app-sidebar"
      >
        <MenuComponent
          ref={menuRef}
          items={items}
          orientation="Vertical"
          select={handleMenuSelect}
          cssClass="sidebar-menu"
        />
      </SidebarComponent>

      {/* Main Content */}
      <main className="app-content">
        <section className={`content-section section-${currentSection}`}>
          <h2>{currentSection.charAt(0).toUpperCase() + currentSection.slice(1)}</h2>
          <p>Main content for {currentSection} section goes here</p>
        </section>
      </main>

      <style>{`
        .app-layout {
          display: flex;
          flex-direction: column;
          height: 100vh;
        }

        .app-header {
          display: flex;
          align-items: center;
          gap: 15px;
          padding: 15px;
          background: #007bff;
          color: white;
          box-shadow: 0 2px 4px rgba(0,0,0,0.1);
        }

        .sidebar-toggle {
          background: none;
          border: none;
          color: white;
          font-size: 24px;
          cursor: pointer;
        }

        .app-content {
          flex: 1;
          padding: 20px;
          overflow-y: auto;
        }

        .sidebar-menu .e-menu-item {
          padding: 12px 15px;
          border-bottom: 1px solid #f0f0f0;
        }

        @media (max-width: 768px) {
          .app-sidebar {
            position: fixed;
            left: 0;
            top: 0;
            height: 100vh;
            z-index: 1000;
          }
        }
      `}</style>
    </div>
  );
}

export default HamburgerSidebarApp;
```

## Mobile Navigation

Bottom navigation pattern for mobile apps.

```jsx
import { MenuComponent } from '@syncfusion/ej2-react-navigations';

function MobileBottomNav() {
  const items = [
    { text: 'Home', iconCss: 'e-icons e-home' },
    { text: 'Search', iconCss: 'e-icons e-search' },
    { text: 'Add', iconCss: 'e-icons e-plus' },
    { text: 'Likes', iconCss: 'e-icons e-heart' },
    { text: 'Profile', iconCss: 'e-icons e-user' }
  ];

  const handleSelect = (args) => {
    console.log('Navigate to:', args.item.text);
  };

  return (
    <div className="mobile-app">
      <main className="mobile-content">
        <h2>Mobile App Content</h2>
      </main>

      <div className="bottom-nav">
        <MenuComponent 
          items={items}
          select={handleSelect}
          orientation="Horizontal"
          cssClass="mobile-nav-menu"
        />
      </div>

      <style>{`
        .mobile-app {
          display: flex;
          flex-direction: column;
          height: 100vh;
        }

        .mobile-content {
          flex: 1;
          overflow-y: auto;
          padding: 20px;
        }

        .bottom-nav {
          position: fixed;
          bottom: 0;
          left: 0;
          right: 0;
          background: white;
          border-top: 1px solid #e0e0e0;
        }

        .mobile-nav-menu {
          display: flex;
          justify-content: space-around;
        }

        .mobile-nav-menu .e-menu-item {
          flex: 1;
          text-align: center;
          padding: 12px 0;
        }
      `}</style>
    </div>
  );
}

export default MobileBottomNav;
```

## Breadcrumb Navigation

Breadcrumb-style menu for showing navigation path.

```jsx
import { MenuComponent } from '@syncfusion/ej2-react-navigations';

function BreadcrumbMenu() {
  const items = [
    {
      text: 'Home',
      items: [
        { text: 'Dashboard' },
        { text: 'Welcome' }
      ]
    },
    {
      text: 'Products',
      items: [
        { text: 'Electronics' },
        { text: 'Books' },
        { text: 'Clothing' }
      ]
    },
    {
      text: 'Laptops',
      items: [
        { text: 'Gaming' },
        { text: 'Business' },
        { text: 'Ultrabook' }
      ]
    },
    {
      text: 'Gaming Laptops',
      items: [] // Current page
    }
  ];

  return (
    <div className="breadcrumb-container">
      <MenuComponent 
        items={items}
        orientation="Horizontal"
        cssClass="breadcrumb-menu"
      />
      <style>{`
        .breadcrumb-menu {
          display: flex;
          gap: 0;
        }

        .breadcrumb-menu .e-menu-item {
          padding: 8px 0;
        }

        .breadcrumb-menu .e-menu-item::after {
          content: ' / ';
          margin: 0 8px;
        }

        .breadcrumb-menu .e-menu-item:last-child::after {
          content: '';
        }
      `}</style>
    </div>
  );
}

export default BreadcrumbMenu;
```

## Context Menu

Right-click context menu implementation.

```jsx
import { MenuComponent } from '@syncfusion/ej2-react-navigations';
import { useState, useRef } from 'react';

function ContextMenuApp() {
  const menuRef = useRef(null);
  const [position, setPosition] = useState({ top: 0, left: 0 });
  const [show, setShow] = useState(false);

  const items = [
    { text: 'Cut', iconCss: 'e-icons e-cut' },
    { text: 'Copy', iconCss: 'e-icons e-copy' },
    { text: 'Paste', iconCss: 'e-icons e-paste' },
    { separator: true },
    { text: 'Delete', iconCss: 'e-icons e-delete' },
    { text: 'Properties' }
  ];

  const handleContextMenu = (e) => {
    e.preventDefault();
    setPosition({ top: e.clientY, left: e.clientX });
    setShow(true);
  };

  const handleDocumentClick = () => {
    setShow(false);
  };

  const handleSelect = (args) => {
    console.log('Context action:', args.item.text);
    setShow(false);
  };

  return (
    <div 
      className="context-menu-area"
      onContextMenu={handleContextMenu}
      onClick={handleDocumentClick}
    >
      <div className="content-box">
        <p>Right-click here for context menu</p>
      </div>

      {show && (
        <div 
          className="context-menu"
          style={{
            position: 'fixed',
            top: `${position.top}px`,
            left: `${position.left}px`
          }}
        >
          <MenuComponent 
            ref={menuRef}
            items={items}
            select={handleSelect}
            orientation="Vertical"
          />
        </div>
      )}

      <style>{`
        .context-menu-area {
          width: 100%;
          height: 300px;
          background: #f5f5f5;
          border: 2px dashed #ccc;
          border-radius: 4px;
          display: flex;
          align-items: center;
          justify-content: center;
        }

        .content-box {
          padding: 20px;
          background: white;
          border-radius: 4px;
          box-shadow: 0 2px 4px rgba(0,0,0,0.1);
        }

        .context-menu {
          background: white;
          border: 1px solid #ccc;
          border-radius: 4px;
          box-shadow: 0 2px 8px rgba(0,0,0,0.15);
          z-index: 1000;
        }
      `}</style>
    </div>
  );
}

export default ContextMenuApp;
```

## Dashboard Navigation

Multi-level dashboard menu with active item highlighting.

```jsx
import { MenuComponent } from '@syncfusion/ej2-react-navigations';
import { useState } from 'react';

function DashboardNav() {
  const [activeItem, setActiveItem] = useState('dashboard');

  const items = [
    {
      id: 'dashboard',
      text: 'Dashboard',
      iconCss: 'e-icons e-dashboard'
    },
    {
      id: 'analytics',
      text: 'Analytics',
      iconCss: 'e-icons e-chart',
      items: [
        { text: 'Overview' },
        { text: 'Reports' },
        { text: 'Predictions' }
      ]
    },
    {
      id: 'sales',
      text: 'Sales',
      iconCss: 'e-icons e-shopping-cart',
      items: [
        { text: 'Orders' },
        { text: 'Invoices' },
        { text: 'Customers' }
      ]
    },
    {
      id: 'inventory',
      text: 'Inventory',
      iconCss: 'e-icons e-box'
    },
    {
      id: 'settings',
      text: 'Settings',
      iconCss: 'e-icons e-settings'
    }
  ];

  const handleSelect = (args) => {
    setActiveItem(args.item.text.toLowerCase());
  };

  const handleBeforeItemRender = (args) => {
    if (args.item.text === activeItem) {
      args.element.classList.add('active-item');
    }
  };

  return (
    <div className="dashboard">
      <div className="sidebar-nav">
        <MenuComponent 
          items={items}
          orientation="Vertical"
          select={handleSelect}
          beforeItemRender={handleBeforeItemRender}
          cssClass="dashboard-menu"
        />
      </div>

      <div className="dashboard-content">
        <h2>{activeItem.charAt(0).toUpperCase() + activeItem.slice(1)}</h2>
        <p>Content for {activeItem}</p>
      </div>

      <style>{`
        .dashboard {
          display: flex;
          height: 100vh;
        }

        .sidebar-nav {
          width: 250px;
          background: #f8f9fa;
          border-right: 1px solid #e0e0e0;
          overflow-y: auto;
        }

        .dashboard-menu .e-menu-item {
          padding: 12px 15px;
          border-left: 3px solid transparent;
        }

        .dashboard-menu .e-menu-item.active-item {
          background: #007bff;
          color: white;
          border-left-color: #0056b3;
        }

        .dashboard-content {
          flex: 1;
          padding: 30px;
          overflow-y: auto;
        }
      `}</style>
    </div>
  );
}

export default DashboardNav;
```

## Related Topics

- [Hamburger and Mobile Mode](./hamburger-mode.md) - Mobile patterns
- [Properties and Configuration](./properties-and-configuration.md) - All options
- [Methods API](./methods-api.md) - Programmatic control
- [Events and Callbacks](./events-and-callbacks.md) - Event handling
