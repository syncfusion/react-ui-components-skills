# Accessibility in React Menu Component

## Table of Contents
- [WCAG Compliance](#wcag-compliance)
- [WAI-ARIA Attributes](#wai-aria-attributes)
- [Keyboard Navigation](#keyboard-navigation)
- [Screen Reader Support](#screen-reader-support)
- [Focus Management](#focus-management)
- [Color Contrast](#color-contrast)
- [Accessible Menu Examples](#accessible-menu-examples)
- [Testing for Accessibility](#testing-for-accessibility)

## WCAG Compliance

The Syncfusion Menu component supports Web Content Accessibility Guidelines (WCAG) 2.1 standards at the AA level.

### WCAG Principles Addressed

1. **Perceivable:** Menu structure visible and navigable
2. **Operable:** Full keyboard navigation support
3. **Understandable:** Clear labels and menu structure
4. **Robust:** Compatible with assistive technologies

### Building WCAG-Compliant Menus

```jsx
import { MenuComponent, MenuItemModel } from '@syncfusion/ej2-react-navigations';
import * as React from 'react';

function AccessibleApp() {
  const menuItems: MenuItemModel[] = [
    {
      text: 'File',
      id: 'menu-file',
      htmlAttributes: {
        'role': 'menuitem',
        'aria-label': 'File menu',
        'aria-expanded': 'false'
      },
      items: [
        { 
          text: 'New', 
          id: 'menu-file-new',
          htmlAttributes: {
            'role': 'menuitem',
            'aria-label': 'Create new file'
          }
        },
        { 
          text: 'Open', 
          id: 'menu-file-open',
          htmlAttributes: {
            'role': 'menuitem',
            'aria-label': 'Open existing file'
          }
        },
        { 
          text: 'Exit', 
          id: 'menu-file-exit',
          htmlAttributes: {
            'role': 'menuitem',
            'aria-label': 'Exit application'
          }
        }
      ]
    }
  ];

  return (
    <MenuComponent 
      items={menuItems}
      role="menubar"
      aria-label="Main application menu"
    ></MenuComponent>
  );
}

export default AccessibleApp;
```

## WAI-ARIA Attributes

Use WAI-ARIA attributes to enhance screen reader support:

### Essential ARIA Attributes

```jsx
const menuItems = [
  {
    text: 'File',
    htmlAttributes: {
      'role': 'menuitem',
      'tabindex': '0',
      'aria-label': 'File menu - use arrow keys to navigate',
      'aria-expanded': 'false',
      'aria-haspopup': 'menu'
    },
    items: [
      {
        text: 'New Document',
        htmlAttributes: {
          'role': 'menuitem',
          'tabindex': '-1',
          'aria-label': 'New Document - Create a new file'
        }
      }
    ]
  }
];

<MenuComponent 
  items={menuItems}
  role="menubar"
  aria-label="Main Menu"
></MenuComponent>
```

### ARIA Attribute Reference

| Attribute | Value | Purpose |
|-----------|-------|---------|
| `role` | `menubar`, `menu`, `menuitem` | Defines component type |
| `aria-label` | String | Descriptive label for screen readers |
| `aria-expanded` | `true`, `false` | Whether submenu is open |
| `aria-haspopup` | `menu`, `true` | Indicates submenu exists |
| `aria-disabled` | `true`, `false` | Item is disabled |
| `aria-hidden` | `true`, `false` | Hide from assistive tech |
| `tabindex` | `0`, `-1` | Keyboard focus order |

## Keyboard Navigation

The Menu component supports full keyboard navigation:

### Default Keyboard Shortcuts

| Key | Action |
|-----|--------|
| `Tab` | Move focus to next menu item |
| `Shift+Tab` | Move focus to previous menu item |
| `Enter/Space` | Select focused menu item |
| `↑ Arrow` | Move to previous item in menu |
| `↓ Arrow` | Move to next item in menu |
| `← Arrow` | Close submenu / move to parent |
| `→ Arrow` | Open submenu / move to child |
| `Escape` | Close menu and return focus |
| `Home` | Move to first menu item |
| `End` | Move to last menu item |
| `Alt+F` | (With mnemonics) Open File menu |

### Enabling Keyboard Navigation

```jsx
function App() {
  const menuItems = [
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
    }
  ];

  const handleKeyDown = (e) => {
    // Menu handles keyboard automatically
    // Additional custom keyboard handling if needed
    console.log('Key pressed:', e.key);
  };

  return (
    <MenuComponent 
      items={menuItems}
      onKeyDown={handleKeyDown}
    ></MenuComponent>
  );
}
```

### Custom Keyboard Handler

```jsx
function App() {
  let menuObj: MenuComponent;

  const handleMenuKeyDown = (e) => {
    // Handle custom keyboard shortcuts
    if (e.altKey && e.key === 'f') {
      // Alt+F to open File menu
      e.preventDefault();
      focusFileMenu();
    }
  };

  React.useEffect(() => {
    window.addEventListener('keydown', handleMenuKeyDown);
    return () => window.removeEventListener('keydown', handleMenuKeyDown);
  }, []);

  return (
    <MenuComponent 
      ref={(menu) => (menuObj = menu)}
      items={menuItems}
      tabindex={0}
    ></MenuComponent>
  );
}
```

## Screen Reader Support

Ensure menu content is properly announced by screen readers:

### Descriptive Labels

```jsx
const menuItems = [
  {
    text: 'File',
    id: 'file-menu',
    htmlAttributes: {
      'aria-label': 'File menu containing document operations',
      'role': 'menuitem',
      'aria-expanded': 'false',
      'aria-haspopup': 'menu'
    },
    items: [
      {
        text: 'New',
        id: 'file-new',
        htmlAttributes: {
          'aria-label': 'Create a new document',
          'aria-describedby': 'file-new-desc'
        }
      },
      {
        text: 'Open',
        htmlAttributes: {
          'aria-label': 'Open an existing document (Ctrl+O)',
          'role': 'menuitem'
        }
      }
    ]
  }
];

<MenuComponent items={menuItems}></MenuComponent>

// Hidden descriptions for screen readers
<p id="file-new-desc" style={{ display: 'none' }}>
  Creates a new blank document
</p>
```

### Icon + Text Combinations

```jsx
const menuItems = [
  {
    text: 'Save',
    iconCss: 'e-icons e-save',
    htmlAttributes: {
      'aria-label': 'Save document (Ctrl+S)',
      'role': 'menuitem',
      'aria-describedby': 'save-description'
    }
  }
];

// Description for screen readers
<span id="save-description" className="sr-only">
  Saves the current document to disk
</span>
```

## Focus Management

Proper focus management ensures keyboard users can navigate effectively:

### Setting Initial Focus

```jsx
function App() {
  let menuObj: MenuComponent;

  const setInitialFocus = () => {
    // Set focus to first menu item on page load
    const firstItem = menuObj.element?.querySelector('.e-menu-item');
    firstItem?.focus();
  };

  React.useEffect(() => {
    setInitialFocus();
  }, []);

  return (
    <MenuComponent 
      ref={(menu) => (menuObj = menu)}
      items={menuItems}
    ></MenuComponent>
  );
}
```

### Focus Restoration

```jsx
function App() {
  let menuObj: MenuComponent;
  let previousFocus: HTMLElement | null = null;

  const handleMenuOpen = (args) => {
    // Store focus before opening menu
    previousFocus = document.activeElement as HTMLElement;
  };

  const handleMenuClose = (args) => {
    // Restore focus after closing menu
    if (previousFocus) {
      previousFocus.focus();
    }
  };

  return (
    <MenuComponent 
      ref={(menu) => (menuObj = menu)}
      items={menuItems}
      open={handleMenuOpen}
      close={handleMenuClose}
    ></MenuComponent>
  );
}
```

### Focus Order and Tabindex

```jsx
const menuItems = [
  {
    text: 'File',
    htmlAttributes: {
      'tabindex': '0'  // Include in tab order
    },
    items: [
      {
        text: 'New',
        htmlAttributes: {
          'tabindex': '-1'  // Skip in tab order (in submenu)
        }
      }
    ]
  }
];

<MenuComponent items={menuItems}></MenuComponent>
```

## Color Contrast

Ensure sufficient color contrast between text and background:

### WCAG AA Contrast Ratios

- **Normal text:** 4.5:1 minimum
- **Large text:** 3:1 minimum
- **UI components:** 3:1 minimum

### High Contrast CSS

```css
/* WCAG AA Compliant High Contrast Menu */
.accessible-menu .e-menu {
  background-color: #ffffff;
  border: 2px solid #000000;  /* High contrast border */
}

.accessible-menu .e-menu-item {
  color: #000000;            /* Dark text on light background */
  padding: 12px 16px;
  min-height: 44px;          /* Touch target size */
}

.accessible-menu .e-menu-item:hover {
  background-color: #0066cc;
  color: #ffffff;
  border-left: 3px solid #ffcc00;  /* High contrast indicator */
}

.accessible-menu .e-menu-item:focus {
  outline: 3px solid #ffcc00;
  outline-offset: 2px;
}

.accessible-menu .e-separator {
  background-color: #000000;  /* High contrast separator */
  height: 2px;
  margin: 8px 0;
}

.accessible-menu .e-menu-icon {
  min-width: 24px;
  min-height: 24px;
}
```

## Accessible Menu Examples

### Example 1: Fully Accessible File Menu

```jsx
import { MenuComponent, MenuItemModel } from '@syncfusion/ej2-react-navigations';
import * as React from 'react';

function AccessibleFileMenu() {
  const menuItems: MenuItemModel[] = [
    {
      text: 'File',
      id: 'file-menu',
      iconCss: 'e-icons e-file',
      htmlAttributes: {
        'role': 'menuitem',
        'aria-label': 'File menu',
        'aria-expanded': 'false',
        'aria-haspopup': 'menu'
      },
      items: [
        {
          text: 'New',
          iconCss: 'e-icons e-new',
          htmlAttributes: {
            'aria-label': 'New document (Ctrl+N)',
            'role': 'menuitem'
          }
        },
        {
          text: 'Open',
          iconCss: 'e-icons e-open',
          htmlAttributes: {
            'aria-label': 'Open document (Ctrl+O)',
            'role': 'menuitem'
          }
        },
        {
          separator: true,
          htmlAttributes: { 'role': 'separator' }
        },
        {
          text: 'Exit',
          iconCss: 'e-icons e-exit',
          htmlAttributes: {
            'aria-label': 'Exit application',
            'role': 'menuitem'
          }
        }
      ]
    }
  ];

  const handleMenuSelect = (args) => {
    console.log(`Selected: ${args.item.text}`);
  };

  return (
    <div className="accessible-menu-container">
      <h1>File Manager</h1>
      <MenuComponent 
        items={menuItems}
        select={handleMenuSelect}
        className="accessible-menu"
        role="menubar"
        aria-label="Main application menu"
      ></MenuComponent>
    </div>
  );
}

export default AccessibleFileMenu;
```

### Example 2: Accessible Navigation Menu

```jsx
function AccessibleNavigation() {
  const menuItems = [
    {
      text: 'Home',
      url: '/',
      htmlAttributes: {
        'aria-label': 'Home page'
      }
    },
    {
      text: 'Products',
      htmlAttributes: {
        'aria-label': 'Products menu',
        'aria-expanded': 'false'
      },
      items: [
        { 
          text: 'Electronics',
          htmlAttributes: { 'aria-label': 'Electronics products' }
        },
        { 
          text: 'Books',
          htmlAttributes: { 'aria-label': 'Books and publications' }
        }
      ]
    },
    {
      text: 'About',
      url: '/about',
      htmlAttributes: {
        'aria-label': 'About our company'
      }
    },
    {
      text: 'Contact',
      url: '/contact',
      htmlAttributes: {
        'aria-label': 'Contact us page'
      }
    }
  ];

  return (
    <nav aria-label="Primary navigation">
      <MenuComponent 
        items={menuItems}
        orientation={Orientation.Horizontal}
        className="accessible-nav"
      ></MenuComponent>
    </nav>
  );
}
```

## Testing for Accessibility

### Keyboard Testing Checklist

- [ ] All menu items are reachable via Tab/Shift+Tab
- [ ] Arrow keys navigate within menu
- [ ] Enter/Space activates selected item
- [ ] Escape closes menu
- [ ] Focus indicators are visible
- [ ] Focus returns to trigger after menu closes

### Screen Reader Testing

```jsx
// Test with NVDA (Windows) or JAWS
// Verify screen reader announces:
// 1. Menu role and label
// 2. Item text and descriptions
// 3. Expanded/collapsed state
// 4. Disabled items
// 5. Keyboard shortcuts

// Use Chrome DevTools Accessibility Tree (Ctrl+Shift+I > Accessibility tab)
```

### Color Contrast Testing

```javascript
// Use tools:
// - WebAIM Contrast Checker (webaim.org/resources/contrastchecker/)
// - Axe DevTools Chrome Extension
// - WAVE Browser Extension

// Verify:
// - Normal text: 4.5:1 ratio
// - Large text: 3:1 ratio
// - Focus indicators: 3:1 ratio
```

### Automated Testing

```jsx
import { axe, toHaveNoViolations } from 'jest-axe';

describe('Menu Accessibility', () => {
  test('Menu should have no accessibility violations', async () => {
    const { container } = render(
      <MenuComponent items={menuItems}></MenuComponent>
    );
    const results = await axe(container);
    expect(results).toHaveNoViolations();
  });
});
```

## Related Topics
- [Getting Started](getting-started.md) - Basic setup
- [Events and Interactions](events-and-interactions.md) - Event handling
- [Styling and Appearance](styling-and-appearance.md) - Visual customization
