---
title: Properties and Configuration
parent_skill: Implementing Menu
description: Complete reference for all MenuComponent properties with examples
---

# Properties and Configuration

## Table of Contents
1. [Core Properties](#core-properties)
2. [Data Binding](#data-binding)
3. [Display and Layout](#display-and-layout)
4. [Interaction](#interaction)
5. [Animation and Effects](#animation-and-effects)
6. [Mobile and Responsive](#mobile-and-responsive)
7. [State Management](#state-management)
8. [Localization and Text](#localization-and-text)
9. [Security](#security)
10. [Styling and Theming](#styling-and-theming)

## Core Properties

### items
**Type:** `MenuItemModel[]`  
**Default:** `[]`  
**Description:** Array of menu items to display in the menu component.

```jsx
import { MenuComponent, MenuItemModel } from '@syncfusion/ej2-react-navigations';

function App() {
  const items: MenuItemModel[] = [
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

  return <MenuComponent items={items} />;
}

export default App;
```

### id
**Type:** `string`  
**Default:** `undefined`  
**Description:** Unique identifier for the menu element.

```jsx
<MenuComponent id="main-menu" items={items} />
```

### template
**Type:** `string | Function`  
**Default:** `undefined`  
**Description:** Custom template for rendering menu items.

```jsx
function customItemTemplate(item) {
  return `<span class="custom-icon">${item.iconCss}</span>${item.text}`;
}

<MenuComponent 
  items={items} 
  template={customItemTemplate}
/>
```

## Data Binding

### fields
**Type:** `FieldSettingsModel`  
**Default:** `undefined`  
**Description:** Specifies field mappings for data binding from external data sources.

```jsx
import { MenuComponent } from '@syncfusion/ej2-react-navigations';

function App() {
  const data = [
    { Id: 1, Text: 'File', Childs: [
      { Id: 2, Text: 'New' },
      { Id: 3, Text: 'Open' }
    ] },
    { Id: 4, Text: 'Edit', Childs: [
      { Id: 5, Text: 'Cut' },
      { Id: 6, Text: 'Copy' }
    ] }
  ];

  const fieldSettings = {
    itemId: 'Id',
    text: 'Text',
    children: 'Childs'
  };

  return (
    <MenuComponent 
      items={data} 
      fields={fieldSettings}
    />
  );
}

export default App;
```

**FieldSettingsModel Properties:**
- `itemId` - Unique item identifier field
- `text` - Item display text field
- `iconCss` - CSS class for icons
- `url` - Navigation URL field
- `children` - Child items field
- `htmlAttributes` - HTML attributes field
- `separator` - Separator field
- `disabled` - Disabled state field

## Display and Layout

### orientation
**Type:** `Orientation` ('Horizontal' | 'Vertical')  
**Default:** `'Horizontal'`  
**Description:** Sets the orientation of the menu items.

```jsx
// Horizontal menu (default)
<MenuComponent items={items} orientation="Horizontal" />

// Vertical menu
<MenuComponent items={items} orientation="Vertical" />
```

### enableScrolling
**Type:** `boolean`  
**Default:** `true`  
**Description:** Enables scrolling for menus with content exceeding viewport.

```jsx
<MenuComponent 
  items={items} 
  enableScrolling={true}
/>

// With custom scroll settings
<MenuComponent 
  items={items} 
  enableScrolling={true}
  vScroll={{ scrollStep: 5 }}
  hScroll={{ scrollStep: 5 }}
/>
```

### cssClass
**Type:** `string`  
**Default:** `undefined`  
**Description:** Custom CSS classes to apply to the menu container.

```jsx
<MenuComponent 
  items={items} 
  cssClass="custom-menu dark-theme"
/>
```

### title
**Type:** `string`  
**Default:** `undefined`  
**Description:** Title text displayed in hamburger mode.

```jsx
<MenuComponent 
  items={items} 
  title="Menu"
/>
```

## Interaction

### showItemOnClick
**Type:** `boolean`  
**Default:** `false`  
**Description:** When enabled, sub-menus open on item click instead of hover.

```jsx
// Sub-menus open on hover (default)
<MenuComponent items={items} />

// Sub-menus open on click
<MenuComponent 
  items={items} 
  showItemOnClick={true}
/>
```

### hoverDelay
**Type:** `number`  
**Default:** `400`  
**Description:** Delay in milliseconds before sub-menu opens on hover.

```jsx
// Default 400ms delay
<MenuComponent items={items} />

// Custom delay - 200ms
<MenuComponent 
  items={items} 
  hoverDelay={200}
/>

// No delay
<MenuComponent 
  items={items} 
  hoverDelay={0}
/>
```

### target
**Type:** `string`  
**Default:** `undefined`  
**Description:** CSS selector for the element where hamburger menu toggles are added.

```jsx
<MenuComponent 
  items={items} 
  hamburgerMode={true}
  target=".navbar"
/>
```

## Animation and Effects

### animationSettings
**Type:** `MenuAnimationSettings`  
**Default:** `{ effect: 'SlideDown', duration: 400, easing: 'ease' }`  
**Description:** Configures animation effects when sub-menus open and close.

```jsx
const animationSettings = {
  effect: 'SlideDown',
  duration: 400,
  easing: 'ease'
};

<MenuComponent 
  items={items} 
  animationSettings={animationSettings}
/>
```

**Available Effects:**
- `None` - No animation
- `SlideDown` - Slide down animation
- `SlideUp` - Slide up animation
- `SlideLeft` - Slide left animation
- `SlideRight` - Slide right animation
- `FadeIn` - Fade in animation
- `FadeOut` - Fade out animation
- `ZoomIn` - Zoom in animation
- `ZoomOut` - Zoom out animation

```jsx
// Custom animation configuration
const customAnimation = {
  effect: 'ZoomIn',
  duration: 300,
  easing: 'ease-in-out'
};

<MenuComponent 
  items={items} 
  animationSettings={customAnimation}
/>
```

## Mobile and Responsive

### hamburgerMode
**Type:** `boolean`  
**Default:** `false`  
**Description:** Enables hamburger menu mode for mobile devices and responsive designs.

```jsx
// Standard menu (default)
<MenuComponent items={items} />

// Hamburger mode for mobile
<MenuComponent 
  items={items} 
  hamburgerMode={true}
  title="Navigation"
/>
```

**With Sidebar Integration:**
```jsx
import { MenuComponent } from '@syncfusion/ej2-react-navigations';
import { SidebarComponent } from '@syncfusion/ej2-react-popups';

function App() {
  let sidebarInstance: SidebarComponent;
  
  const handleHamburgerToggle = () => {
    sidebarInstance.toggle();
  };

  const items = [
    { text: 'Home' },
    { text: 'About' },
    { text: 'Products' }
  ];

  return (
    <>
      <div className="navbar">
        <button onClick={handleHamburgerToggle}>☰</button>
        <span>My Site</span>
      </div>
      <SidebarComponent 
        ref={(sidebar) => (sidebarInstance = sidebar)}
      >
        <MenuComponent 
          items={items}
          hamburgerMode={true}
        />
      </SidebarComponent>
    </>
  );
}

export default App;
```

## State Management

### enablePersistence
**Type:** `boolean`  
**Default:** `false`  
**Description:** Enables state persistence to save expanded/collapsed menu state across page reloads.

```jsx
// Without persistence (state lost on reload)
<MenuComponent items={items} />

// With persistence (state saved in localStorage)
<MenuComponent 
  items={items} 
  enablePersistence={true}
/>
```

The menu state is stored in browser localStorage with key: `{id}_menu`

## Localization and Text

### locale
**Type:** `string`  
**Default:** `'en-US'`  
**Description:** Culture/language identifier for localization.

```jsx
// English (default)
<MenuComponent items={items} locale="en-US" />

// Spanish
<MenuComponent items={items} locale="es-ES" />

// German
<MenuComponent items={items} locale="de-DE" />

// Arabic (RTL)
<MenuComponent items={items} locale="ar-AE" enableRtl={true} />
```

## Security

### enableHtmlSanitizer
**Type:** `boolean`  
**Default:** `true`  
**Description:** When enabled, sanitizes HTML content in menu items to prevent XSS attacks.

```jsx
// With sanitization enabled (recommended for user-provided content)
const items = [
  { text: '<strong>File</strong>' }
];

<MenuComponent 
  items={items} 
  enableHtmlSanitizer={true}
/>

// Disable sanitization only if content is trusted
<MenuComponent 
  items={items} 
  enableHtmlSanitizer={false}
/>
```

## Styling and Theming

### enableRtl
**Type:** `boolean`  
**Default:** `false`  
**Description:** Enables right-to-left text direction support.

```jsx
// LTR layout (default)
<MenuComponent items={items} />

// RTL layout for Arabic, Hebrew, etc.
<MenuComponent 
  items={items} 
  enableRtl={true}
/>
```

**Full RTL Example:**
```jsx
function App() {
  const items = [
    {
      text: 'ملف',
      items: [
        { text: 'جديد' },
        { text: 'فتح' },
        { text: 'حفظ' }
      ]
    },
    {
      text: 'تحرير',
      items: [
        { text: 'قص' },
        { text: 'نسخ' },
        { text: 'لصق' }
      ]
    }
  ];

  return (
    <MenuComponent 
      items={items}
      enableRtl={true}
      locale="ar-AE"
    />
  );
}

export default App;
```

## MenuItemModel Properties

Individual menu items support these properties:

| Property | Type | Purpose |
|----------|------|---------|
| `text` | string | Display text for the item |
| `iconCss` | string | CSS class for item icon |
| `url` | string | Navigation URL |
| `items` | MenuItemModel[] | Child menu items |
| `disabled` | boolean | Disable the item |
| `separator` | boolean | Render as separator |
| `htmlAttributes` | Object | Custom HTML attributes |
| `id` | string | Unique item identifier |

**Complete MenuItem Example:**
```jsx
const items = [
  {
    id: 'file',
    text: 'File',
    iconCss: 'e-icons e-folder-open',
    items: [
      {
        id: 'new',
        text: 'New',
        iconCss: 'e-icons e-new'
      },
      {
        id: 'open',
        text: 'Open',
        iconCss: 'e-icons e-open',
        url: '/open'
      },
      {
        separator: true
      },
      {
        id: 'exit',
        text: 'Exit',
        disabled: false
      }
    ]
  }
];

<MenuComponent items={items} />
```

## Animation Settings Reference

```jsx
const animationSettings = {
  // Animation effect type
  effect: 'SlideDown', // 'None', 'SlideDown', 'SlideUp', 'FadeIn', 'ZoomIn', etc.
  
  // Duration in milliseconds
  duration: 400,
  
  // Easing function
  easing: 'ease', // 'ease', 'ease-in', 'ease-out', 'ease-in-out', 'linear'
  
  // Delay before animation starts
  delay: 0
};

<MenuComponent 
  items={items}
  animationSettings={animationSettings}
/>
```

## Related Topics

- [Methods and API](./methods-api.md) - Available methods
- [Events and Callbacks](./events-and-callbacks.md) - Event handling
- [Data Binding](./data-binding.md) - Binding from data sources
- [Styling and Appearance](./styling-and-appearance.md) - Custom styling
- [Hamburger and Mobile Mode](./hamburger-mode.md) - Mobile implementation
