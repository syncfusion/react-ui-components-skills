# Advanced Features

## Table of Contents
- [Scrollable Context Menus](#scrollable-context-menus)
- [Animation Settings](#animation-settings)
- [Menu Open Types](#menu-open-types)
- [Overflow Handling](#overflow-handling)
- [Position and Offset Configuration](#position-and-offset-configuration)
- [Menu Effects](#menu-effects)
- [Advanced Scenarios](#advanced-scenarios)
- [Advanced Configuration Properties](#advanced-configuration-properties)
  - [Filter Property](#filter-property)
  - [Hover Delay Property](#hover-delay-property)
  - [Show Item on Click Property](#show-item-on-click-property)
  - [Enable Persistence Property](#enable-persistence-property)
  - [Enable HTML Sanitizer Property](#enable-html-sanitizer-property)
  - [Locale Property](#locale-property)
  - [CSS Class Property](#css-class-property)
- [Best Practices](#best-practices)

## Scrollable Context Menus

Enable scrolling for large menu lists that exceed viewport height:

### Enable Scrolling

```ts
import { ContextMenuComponent, MenuItemModel } from '@syncfusion/ej2-react-navigations';
import * as React from 'react';

function App() {
  // Large menu item list
  const menuItems: MenuItemModel[] = Array.from({ length: 30 }, (_, i) => ({
    text: `Item ${i + 1}`,
    id: `item-${i + 1}`
  }));

  return (
    <div>
      <div id='target'>Right click to open scrollable menu</div>
      <ContextMenuComponent
        target='#target'
        items={menuItems}
        enableScrolling={true}  // Enable scrolling
      />
    </div>
  );
}

export default App;
```

### Customize Scroll Behavior

```ts
import { ContextMenuComponent, MenuItemModel, ContextMenuModel, ScrollDirection } from '@syncfusion/ej2-react-navigations';
import * as React from 'react';

interface ScrollSettings {
  verticalScroll?: boolean;
  horizontalScroll?: boolean;
  scrollHeight?: string;
  scrollWidth?: string;
}

function App() {
  const menuItems: MenuItemModel[] = Array.from({ length: 50 }, (_, i) => ({
    text: `Menu Item ${i + 1}`,
    id: `item-${i + 1}`
  }));

  // Configure scroll settings
  const verticalScrollSettings = {
    enable: true,
    height: '300px'  // Set max height for scrolling
  };

  const horizontalScrollSettings = {
    enable: false
  };

  return (
    <div>
      <div id='target'>Right click to open menu</div>
      <ContextMenuComponent
        target='#target'
        items={menuItems}
        enableScrolling={true}
      />
    </div>
  );
}

export default App;
```

### CSS for Scrollable Menu

```css
/* Customize scrollbar appearance */
.e-contextmenu-wrapper .e-ul {
  max-height: 300px;
  overflow-y: auto;
  scrollbar-width: thin;
  scrollbar-color: #999 #f0f0f0;
}

/* Webkit browsers (Chrome, Safari) */
.e-contextmenu-wrapper .e-ul::-webkit-scrollbar {
  width: 8px;
}

.e-contextmenu-wrapper .e-ul::-webkit-scrollbar-track {
  background: #f0f0f0;
  border-radius: 4px;
}

.e-contextmenu-wrapper .e-ul::-webkit-scrollbar-thumb {
  background: #999;
  border-radius: 4px;
}

.e-contextmenu-wrapper .e-ul::-webkit-scrollbar-thumb:hover {
  background: #666;
}
```

## Animation Settings

Control menu opening and closing animations:

### Configure Animation Effects

```ts
import { ContextMenuComponent, MenuItemModel } from '@syncfusion/ej2-react-navigations';
import * as React from 'react';

function App() {
  const menuItems: MenuItemModel[] = [
    { text: 'Cut' },
    { text: 'Copy' },
    { text: 'Paste' }
  ];

  // Define animation settings
  const animationSettings = {
    effect: 'FadeIn',        // FadeIn, SlideDown, Zoom, FadeOut, etc.
    duration: 400,           // Duration in milliseconds
    easing: 'ease-in-out'    // CSS easing function
  };

  return (
    <div>
      <div id='target'>Right click to open menu with animation</div>
      <ContextMenuComponent
        target='#target'
        items={menuItems}
        animationSettings={animationSettings}
      />
    </div>
  );
}

export default App;
```

### Available Animation Effects

| Effect | Description |
|--------|-------------|
| `None` | No animation |
| `FadeIn` | Fade in effect |
| `FadeOut` | Fade out effect |
| `SlideDown` | Slide down from top |
| `SlideUp` | Slide up to top |
| `Zoom` | Zoom in/out |
| `ZoomIn` | Zoom in effect |
| `ZoomOut` | Zoom out effect |

### Example: Custom Animation

```ts
const animationSettings = {
  effect: 'SlideDown',
  duration: 300,
  easing: 'cubic-bezier(0.25, 0.46, 0.45, 0.94)'
};

// For submenu opening
const subMenuAnimationSettings = {
  effect: 'Zoom',
  duration: 200
};
```

## Menu Open Types

Control how the context menu opens when triggered:

### Open Type Options

```ts
import { ContextMenuComponent, MenuItemModel } from '@syncfusion/ej2-react-navigations';
import * as React from 'react';

function App() {
  const menuItems: MenuItemModel[] = [
    { text: 'Cut' },
    { text: 'Copy' },
    { text: 'Paste' }
  ];

  return (
    <div>
      {/* Right-click to open (default) */}
      <div id='target1'>Right click to open</div>
      <ContextMenuComponent target='#target1' items={menuItems} />

      {/* Context menu on click */}
      <div id='target2'>Click to open</div>
      <ContextMenuComponent
        target='#target2'
        items={menuItems}
        filter='#target2'
      />

      {/* Context menu on hover */}
      <div id='target3'>Hover to open</div>
      <ContextMenuComponent target='#target3' items={menuItems} />
    </div>
  );
}

export default App;
```

## Overflow Handling

Handle menu overflow when positioned near viewport edges:

### Auto-Position on Overflow

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
        { text: 'Close' },
        { text: 'Exit' }
      ]
    },
    {
      text: 'Edit',
      items: [
        { text: 'Cut' },
        { text: 'Copy' },
        { text: 'Paste' },
        { text: 'Select All' }
      ]
    }
  ];

  return (
    <div style={{ position: 'relative', height: '500px' }}>
      <div id='target' style={{ 
        position: 'absolute', 
        bottom: '20px', 
        right: '20px',
        width: '100px',
        height: '100px',
        border: '1px solid gray'
      }}>
        Right click here
      </div>
      <ContextMenuComponent
        target='#target'
        items={menuItems}
        // Auto-adjusts position to fit within viewport
      />
    </div>
  );
}

export default App;
```

## Position and Offset Configuration

Control where the context menu appears relative to the click point:

### Custom Positioning

```ts
import { ContextMenuComponent, MenuEventArgs, MenuItemModel } from '@syncfusion/ej2-react-navigations';
import * as React from 'react';

function App() {
  const menuItems: MenuItemModel[] = [
    { text: 'Option 1' },
    { text: 'Option 2' },
    { text: 'Option 3' }
  ];

  const handleBeforeOpen = (args: MenuEventArgs) => {
    if (args.event) {
      const event = args.event as MouseEvent;
      
      // Get click position
      const x = event.clientX;
      const y = event.clientY;
      
      // Apply custom offset
      const offsetX = 10;  // 10px right
      const offsetY = 10;  // 10px down
      
      console.log(`Menu will open at: ${x + offsetX}, ${y + offsetY}`);
    }
  };

  return (
    <div>
      <div id='target'>Right click to open menu</div>
      <ContextMenuComponent
        target='#target'
        items={menuItems}
        beforeOpen={handleBeforeOpen}
      />
    </div>
  );
}

export default App;
```

## Menu Effects

Apply visual effects to menu elements:

### Shadow and Elevation

```css
/* Add shadow elevation */
.e-contextmenu-wrapper {
  box-shadow: 0 5px 15px rgba(0, 0, 0, 0.3);
  border-radius: 8px;
}

/* Depth effect */
.e-contextmenu-wrapper {
  box-shadow: 
    0 3px 1px -2px rgba(0,0,0,.2),
    0 2px 2px 0 rgba(0,0,0,.14),
    0 1px 5px 0 rgba(0,0,0,.12);
}
```

### Hover Effects

```css
/* Item hover effect */
.e-contextmenu-wrapper .e-menu-item:hover {
  background-color: #f5f5f5;
  transform: translateX(2px);
  transition: all 0.2s ease;
}

/* Scale on hover */
.e-contextmenu-wrapper .e-menu-item:hover {
  transform: scale(1.02);
  transform-origin: left center;
}
```

## Advanced Scenarios

### Nested Menus with Custom Structure

```ts
function App() {
  const menuItems: MenuItemModel[] = [
    {
      text: 'File',
      items: [
        { text: 'New' },
        {
          text: 'Recent',
          items: [
            { text: 'Document 1' },
            { text: 'Document 2' },
            { text: 'Document 3' }
          ]
        },
        { text: 'Open' },
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
    }
  ];

  return (
    <div>
      <div id='target'>Right click for nested menus</div>
      <ContextMenuComponent target='#target' items={menuItems} />
    </div>
  );
}
```

### Dynamic Menu Adjustment

```ts
function App() {
  const [menuItems, setMenuItems] = React.useState<MenuItemModel[]>([
    { text: 'Cut' },
    { text: 'Copy' },
    { text: 'Paste' }
  ]);

  const handleBeforeOpen = (args: MenuEventArgs) => {
    // Adjust menu based on context
    const target = args.event?.target as HTMLElement;
    
    if (target?.tagName === 'IMG') {
      setMenuItems([
        { text: 'Save Image' },
        { text: 'Copy Image' },
        { text: 'View Image' }
      ]);
    } else if (target?.tagName === 'A') {
      setMenuItems([
        { text: 'Open Link' },
        { text: 'Copy Link' },
        { text: 'Save Link' }
      ]);
    }
  };

  return (
    <div>
      <img id='target' src='image.jpg' alt='Image' />
      <a href='#' id='target2'>Link</a>
      <ContextMenuComponent target='#target' items={menuItems} beforeOpen={handleBeforeOpen} />
    </div>
  );
}
```

## Advanced Configuration Properties

### Filter Property

The `filter` property restricts context menu activation to specific child elements within the target:

```ts
import { ContextMenuComponent, MenuItemModel } from '@syncfusion/ej2-react-navigations';
import * as React from 'react';

function App() {
  const menuItems: MenuItemModel[] = [
    { text: 'Cut' },
    { text: 'Copy' },
    { text: 'Paste' }
  ];

  return (
    <div>
      {/* Context menu appears only on table rows */}
      <table id="dataTable">
        <tr><td>Row 1</td></tr>
        <tr><td>Row 2</td></tr>
        <tr><td>Row 3</td></tr>
      </table>
      <ContextMenuComponent
        target="#dataTable"
        filter="tr"  // Only activate on rows
        items={menuItems}
      />
    </div>
  );
}

export default App;
```

**Use Cases:**
- Table row operations: `filter="tr"`
- List item operations: `filter="li"`
- Dialog-specific actions: `filter=".dialog-content"`
- Conditional element types: `filter=".editable"`

---

### Hover Delay Property

The `hoverDelay` property controls milliseconds before submenu appears on hover:

```ts
import { ContextMenuComponent, MenuItemModel } from '@syncfusion/ej2-react-navigations';
import * as React from 'react';

function App() {
  const menuItems: MenuItemModel[] = [
    { text: 'File' },
    {
      text: 'Edit',
      items: [
        { text: 'Cut' },
        { text: 'Copy' },
        { text: 'Paste' }
      ]
    },
    { text: 'View' }
  ];

  return (
    <div>
      <div id="target">Right-click here</div>
      {/* Submenu appears after 600ms hover */}
      <ContextMenuComponent
        target="#target"
        items={menuItems}
        hoverDelay={600}
      />
    </div>
  );
}

export default App;
```

**Common Values:**
- `400ms` - Default (fast, for experienced users)
- `600ms` - Standard (moderate, balanced UX)
- `800ms` - Slow (for touchpads or accessibility)

---

### Show Item on Click Property

The `showItemOnClick` property forces submenus to open only on click:

```ts
import { ContextMenuComponent, MenuItemModel } from '@syncfusion/ej2-react-navigations';
import * as React from 'react';

function App() {
  const menuItems: MenuItemModel[] = [
    { text: 'File' },
    {
      text: 'Recent',
      items: [
        { text: 'Document 1.pdf' },
        { text: 'Document 2.pdf' },
        { text: 'Document 3.pdf' }
      ]
    },
    { text: 'Help' }
  ];

  return (
    <div>
      <div id="target">Right-click here</div>
      {/* Submenus open only on arrow keys or click, not hover */}
      <ContextMenuComponent
        target="#target"
        items={menuItems}
        showItemOnClick={true}
      />
    </div>
  );
}

export default App;
```

**Benefits:**
- Prevents accidental submenu opening
- Required keyboard navigation (Enter or arrow keys)
- Better for touch interfaces
- Reduces cognitive load

---

### Enable Persistence Property

The `enablePersistence` property saves component state across browser sessions:

```ts
import { ContextMenuComponent, MenuItemModel } from '@syncfusion/ej2-react-navigations';
import * as React from 'react';

function App() {
  const menuItems: MenuItemModel[] = [
    {
      text: 'File',
      items: [
        { text: 'New', expanded: true },
        { text: 'Open' },
        { text: 'Save' }
      ]
    },
    { text: 'Edit' },
    { text: 'View' }
  ];

  return (
    <div>
      <div id="target">Right-click here</div>
      {/* Expanded/collapsed state persists across page reloads */}
      <ContextMenuComponent
        target="#target"
        items={menuItems}
        enablePersistence={true}
      />
    </div>
  );
}

export default App;
```

**Storage:** Uses browser's localStorage
**Use Cases:**
- Remember user menu preferences
- Maintain menu state across sessions
- Persistent collapse/expand states

---

### Enable HTML Sanitizer Property

The `enableHtmlSanitizer` property sanitizes HTML content to prevent XSS attacks:

```ts
import { ContextMenuComponent, MenuItemModel } from '@syncfusion/ej2-react-navigations';
import * as React from 'react';

function App() {
  // Potentially unsafe HTML content
  const menuItems: MenuItemModel[] = [
    { 
      text: 'Safe Item',
      htmlAttributes: {
        'title': 'Normal item'
      }
    },
    {
      text: 'Item with Script',
      // HTML sanitizer will remove scripts
      htmlAttributes: {
        'data-content': '<img src=x onerror="alert(\'XSS\')">'
      }
    }
  ];

  return (
    <div>
      <div id="target">Right-click here</div>
      {/* Enable sanitization for user-generated content */}
      <ContextMenuComponent
        target="#target"
        items={menuItems}
        enableHtmlSanitizer={true}  // Default: true
      />
    </div>
  );
}

export default App;
```

**Security Best Practices:**
- Always enable for user-generated content (default: true)
- Disable only for trusted, server-rendered content
- Validate on backend before rendering
- Never trust untrusted URLs or scripts

---

### Locale Property

The `locale` property sets component language and formatting:

```ts
import { ContextMenuComponent, MenuItemModel } from '@syncfusion/ej2-react-navigations';
import * as React from 'react';

function App() {
  const menuItems: MenuItemModel[] = [
    { text: 'Cortar', id: 'cut' },      // Spanish: Cut
    { text: 'Copiar', id: 'copy' },    // Spanish: Copy
    { text: 'Pegar', id: 'paste' }     // Spanish: Paste
  ];

  return (
    <div>
      <div id="target">Haz clic derecho aquí</div>
      {/* Spanish localization */}
      <ContextMenuComponent
        target="#target"
        items={menuItems}
        locale="es-ES"
      />
    </div>
  );
}

export default App;
```

**Common Locale Codes:**
- `en-US` - English (default)
- `es-ES` - Spanish
- `fr-FR` - French
- `de-DE` - German
- `ja-JP` - Japanese
- `zh-CN` - Chinese (Simplified)
- `ar-AE` - Arabic
- `he-IL` - Hebrew (RTL)

---

### CSS Class Property

The `cssClass` property adds custom CSS classes to the context menu wrapper:

```ts
import { ContextMenuComponent, MenuItemModel } from '@syncfusion/ej2-react-navigations';
import * as React from 'react';

function App() {
  const menuItems: MenuItemModel[] = [
    { text: 'Cut' },
    { text: 'Copy' },
    { text: 'Paste' }
  ];

  return (
    <div>
      <div id="target">Right-click here</div>
      <ContextMenuComponent
        target="#target"
        items={menuItems}
        cssClass="custom-dark-menu premium-style"
      />
      
      <style>{`
        /* Apply to custom-dark-menu class */
        .custom-dark-menu.e-contextmenu {
          background: #2d2d2d;
          color: #ffffff;
          border-color: #555;
        }
        
        .custom-dark-menu .e-menu-item {
          color: #ffffff;
        }
        
        .custom-dark-menu .e-menu-item:hover {
          background: #3d3d3d;
        }
        
        /* Premium styling */
        .premium-style.e-contextmenu {
          border-radius: 8px;
          box-shadow: 0 4px 16px rgba(0,0,0,0.3);
        }
      `}</style>
    </div>
  );
}

export default App;
```

---

## Best Practices

1. **Animation Performance:** Use fast animations (200-300ms) for better UX
2. **Scroll Height:** Set appropriate max height for scrollable menus (250-350px)
3. **Submenu Timing:** Use hoverDelay of 500-600ms for balance
4. **Filter Usage:** Use filter property to limit context menu to specific elements
5. **Offset Spacing:** Maintain 5-10px offset from click point
6. **Viewport Safety:** Always ensure menu stays within viewport bounds
7. **Touch Targets:** Make items at least 44px tall for touch devices
8. **Performance:** Avoid excessive nesting (3 levels max)
9. **Security:** Enable HTML sanitizer for user-generated content
10. **Accessibility:** Use showItemOnClick=true for keyboard-first interfaces
