# Styling and Customization

## Table of Contents
- [Overview](#overview)
- [CSS Classes Reference](#css-classes-reference)
- [Built-in Themes](#built-in-themes)
- [Custom Styling](#custom-styling)
- [Styling with Utility Frameworks](#styling-with-utility-frameworks)
- [RTL Support](#rtl-support)
- [Responsive Design](#responsive-design)
- [Common Customization Patterns](#common-customization-patterns)

## Overview

The Accordion component can be styled using:
1. **Built-in themes** - Pre-configured color schemes (Tailwind, Bootstrap, Fluent, Material)
2. **CSS classes** - Target specific accordion elements
3. **Custom CSS** - Override default styles with your own CSS
4. **Utility frameworks** - Use Tailwind/Bootstrap classes directly
5. **CSS variables** - Modify theme colors without rewriting CSS

## CSS Classes Reference

The Accordion generates these CSS classes for styling:

### Main Container Classes

| Class | Purpose | Example |
|-------|---------|---------|
| `.e-accordion` | Main accordion container | Background color, overall styling |
| `.e-accordion-item` | Individual accordion item | Spacing, borders between items |

### Header Classes

| Class | Purpose | Example |
|-------|---------|---------|
| `.e-accordion-header` | Header clickable area | Background, text color, padding |
| `.e-accordion-header-icon` | Expand/collapse icon | Icon color, size |
| `.e-icons` | Icon wrapper | Icon styling |

### Content Classes

| Class | Purpose | Example |
|-------|---------|---------|
| `.e-accordion-content` | Panel content area | Padding, text color, background |
| `.e-accordion-control` | Expanded item indicator | Used to target active items |

### State Classes

| Class | Purpose | Example |
|-------|---------|---------|
| `.e-disabled` | Disabled item state | Opacity, cursor changes |
| `.e-focus` | Focused item (keyboard nav) | Outline, focus indicator |

### Styling Example

```css
/* Override header styling */
.e-accordion-header {
  background-color: #2c3e50;
  color: white;
  font-weight: 600;
  padding: 15px;
}

/* Style content area */
.e-accordion-content {
  padding: 20px;
  background-color: #f8f9fa;
  color: #333;
}

/* Target expanded items */
.e-accordion-item.e-accordion-control .e-accordion-header {
  background-color: #0066ff;
  color: white;
}

/* Disable styling */
.e-accordion-item.e-disabled .e-accordion-header {
  opacity: 0.5;
  cursor: not-allowed;
}
```

## Built-in Themes

Choose from pre-configured themes by importing the appropriate CSS file:

### Available Themes

```css
/* Tailwind 3 (Modern, minimal) */
@import '../node_modules/@syncfusion/ej2-react-navigations/styles/tailwind3.css';

/* Bootstrap 5.3 (Familiar Bootstrap styling) */
@import '../node_modules/@syncfusion/ej2-react-navigations/styles/bootstrap5.3.css';

/* Fluent 2 (Microsoft Fluent Design) */
@import '../node_modules/@syncfusion/ej2-react-navigations/styles/fluent2.css';

/* Material 3 (Google Material Design) */
@import '../node_modules/@syncfusion/ej2-react-navigations/styles/material3.css';
```

### Switching Themes at Runtime

```jsx
import React, { useState } from 'react';
import { 
  AccordionComponent, 
  AccordionItemDirective, 
  AccordionItemsDirective 
} from '@syncfusion/ej2-react-navigations';

export default function App() {
  const [theme, setTheme] = useState('tailwind');

  const loadTheme = (themeName) => {
    const head = document.head;
    const existingLink = head.querySelector('link[data-theme]');
    
    if (existingLink) {
      existingLink.remove();
    }

    const link = document.createElement('link');
    link.rel = 'stylesheet';
    link.href = `/styles/${themeName}.css`;
    link.setAttribute('data-theme', themeName);
    head.appendChild(link);
    
    setTheme(themeName);
  };

  return (
    <div>
      <div style={{ marginBottom: '20px' }}>
        <label>Choose Theme: </label>
        <select onChange={(e) => loadTheme(e.target.value)}>
          <option value='tailwind'>Tailwind 3</option>
          <option value='bootstrap5.3'>Bootstrap 5.3</option>
          <option value='fluent2'>Fluent 2</option>
          <option value='material3'>Material 3</option>
        </select>
      </div>

      <AccordionComponent>
        <AccordionItemsDirective>
          <AccordionItemDirective header='Item 1' content='Content 1' />
          <AccordionItemDirective header='Item 2' content='Content 2' />
        </AccordionItemsDirective>
      </AccordionComponent>
    </div>
  );
}
```

## Custom Styling

### Add Custom CSS Classes

Use the `cssClass` property to add custom classes:

```jsx
<AccordionComponent cssClass='custom-accordion my-accordion'>
  <AccordionItemsDirective>
    <AccordionItemDirective 
      header='Custom Item' 
      content='Content here'
      cssClass='custom-item'
    />
  </AccordionItemsDirective>
</AccordionComponent>
```

Then define custom styles:

```css
.custom-accordion {
  border-radius: 12px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
  overflow: hidden;
}

.custom-accordion .custom-item .e-accordion-header {
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
  border-left: 4px solid #667eea;
}

.custom-accordion .custom-item .e-accordion-content {
  border-left: 4px solid #667eea;
  background-color: #f5f7fa;
}
```

### Inline Styling

For quick customization:

```jsx
const headerStyle = {
  backgroundColor: '#2c3e50',
  color: 'white',
  padding: '15px 20px',
  fontWeight: 'bold'
};

const contentStyle = {
  padding: '20px',
  backgroundColor: '#ecf0f1',
  lineHeight: '1.6'
};

<AccordionComponent>
  {/* Note: Inline styles won't work on Syncfusion components directly */}
  {/* Use CSS classes instead */}
</AccordionComponent>
```

## Styling with Utility Frameworks

### Using Tailwind CSS

```jsx
export default function App() {
  return (
    <div className='p-8 bg-gray-100 min-h-screen'>
      <h1 className='text-2xl font-bold mb-6'>FAQ</h1>
      
      <AccordionComponent cssClass='space-y-4'>
        <AccordionItemsDirective>
          <AccordionItemDirective 
            header='How do I get started?' 
            content='Visit our documentation...'
            cssClass='border border-gray-300 rounded-lg'
          />
          <AccordionItemDirective 
            header='What is pricing?' 
            content='We offer flexible pricing plans...'
            cssClass='border border-gray-300 rounded-lg'
          />
        </AccordionItemsDirective>
      </AccordionComponent>
    </div>
  );
}
```

### Custom Tailwind Styles

```css
/* In your Tailwind CSS file */
@layer components {
  .accordion-custom {
    @apply rounded-lg shadow-md overflow-hidden border border-gray-200;
  }

  .accordion-custom .e-accordion-header {
    @apply bg-blue-600 text-white font-semibold px-4 py-3 hover:bg-blue-700 transition-colors;
  }

  .accordion-custom .e-accordion-content {
    @apply bg-gray-50 px-4 py-4 text-gray-700;
  }

  .accordion-custom .e-accordion-item.e-accordion-control .e-accordion-header {
    @apply bg-blue-700;
  }
}
```

Then use:
```jsx
<AccordionComponent cssClass='accordion-custom'>
  {/* items */}
</AccordionComponent>
```

### Using Bootstrap Classes

```jsx
<AccordionComponent cssClass='card'>
  <AccordionItemsDirective>
    <AccordionItemDirective 
      header='Bootstrap Item' 
      content='Styled with Bootstrap classes'
      cssClass='card-header'
    />
  </AccordionItemsDirective>
</AccordionComponent>
```

## RTL Support

Enable right-to-left layout for Arabic, Hebrew, and other RTL languages:

### Enable RTL in HTML

```html
<!DOCTYPE html>
<html dir="rtl">
  <head>
    <meta charset="UTF-8">
    <title>RTL Accordion</title>
  </head>
  <body>
    <div id="root"></div>
  </body>
</html>
```

### RTL in React Component

```jsx
import React from 'react';
import { 
  AccordionComponent, 
  AccordionItemDirective, 
  AccordionItemsDirective 
} from '@syncfusion/ej2-react-navigations';

export default function App() {
  return (
    <div dir='rtl'>
      <AccordionComponent>
        <AccordionItemsDirective>
          <AccordionItemDirective header='عنصر 1' content='المحتوى 1' />
          <AccordionItemDirective header='عنصر 2' content='المحتوى 2' />
        </AccordionItemsDirective>
      </AccordionComponent>
    </div>
  );
}
```

**RTL Automatic Features:**
- Headers/content reversed
- Icons flipped appropriately
- Padding/margins mirrored
- Text alignment adjusted

### RTL-Specific Styling

```css
/* Right-to-left specific styles */
[dir="rtl"] .e-accordion {
  direction: rtl;
}

[dir="rtl"] .e-accordion-header-icon {
  float: left;
  margin-left: 10px;
  margin-right: 0;
}

[dir="rtl"] .e-accordion-content {
  text-align: right;
}
```

## Responsive Design

### Mobile-First Approach

```jsx
import React from 'react';
import './responsive.css';
import { 
  AccordionComponent, 
  AccordionItemDirective, 
  AccordionItemsDirective 
} from '@syncfusion/ej2-react-navigations';

export default function App() {
  return (
    <AccordionComponent cssClass='accordion-responsive'>
      <AccordionItemsDirective>
        <AccordionItemDirective 
          header='Mobile-Friendly Item' 
          content='Content adapts to screen size' 
        />
      </AccordionItemsDirective>
    </AccordionComponent>
  );
}
```

### Responsive CSS

```css
/* Mobile: Default (small screens) */
.accordion-responsive {
  margin: 0;
}

.accordion-responsive .e-accordion-header {
  padding: 12px;
  font-size: 14px;
}

.accordion-responsive .e-accordion-content {
  padding: 15px;
  font-size: 13px;
}

/* Tablet: Medium screens */
@media (min-width: 768px) {
  .accordion-responsive .e-accordion-header {
    padding: 15px 20px;
    font-size: 15px;
  }

  .accordion-responsive .e-accordion-content {
    padding: 20px;
    font-size: 14px;
  }
}

/* Desktop: Large screens */
@media (min-width: 1024px) {
  .accordion-responsive {
    max-width: 800px;
    margin: 0 auto;
  }

  .accordion-responsive .e-accordion-header {
    padding: 18px 24px;
    font-size: 16px;
  }

  .accordion-responsive .e-accordion-content {
    padding: 25px;
    font-size: 15px;
  }
}
```

### Responsive with Tailwind

```jsx
<AccordionComponent cssClass='sm:text-sm md:text-base lg:text-lg'>
  <AccordionItemsDirective>
    <AccordionItemDirective 
      header='Responsive' 
      content='Scales with screen size'
      cssClass='px-2 sm:px-4 md:px-6'
    />
  </AccordionItemsDirective>
</AccordionComponent>
```

## Common Customization Patterns

### Pattern 1: Professional Business Style

```jsx
// App.css
.business-accordion {
  background-color: white;
  border: 1px solid #d0d0d0;
  border-radius: 4px;
}

.business-accordion .e-accordion-header {
  background-color: #f5f5f5;
  color: #333;
  font-weight: 500;
  border-bottom: 1px solid #d0d0d0;
  transition: background-color 0.2s ease;
}

.business-accordion .e-accordion-header:hover {
  background-color: #e8e8e8;
}

.business-accordion .e-accordion-content {
  padding: 20px;
  color: #555;
  line-height: 1.5;
}

// Usage
<AccordionComponent cssClass='business-accordion'>
  {/* items */}
</AccordionComponent>
```

### Pattern 2: Modern Gradient Style

```css
.modern-accordion {
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  border-radius: 12px;
  overflow: hidden;
  box-shadow: 0 10px 30px rgba(102, 126, 234, 0.3);
}

.modern-accordion .e-accordion-item {
  border: none;
  margin: 0;
}

.modern-accordion .e-accordion-header {
  background: rgba(255, 255, 255, 0.1);
  color: white;
  border-bottom: 1px solid rgba(255, 255, 255, 0.2);
  transition: all 0.3s ease;
}

.modern-accordion .e-accordion-header:hover {
  background: rgba(255, 255, 255, 0.15);
}

.modern-accordion .e-accordion-item.e-accordion-control .e-accordion-header {
  background: rgba(255, 255, 255, 0.2);
}

.modern-accordion .e-accordion-content {
  background: rgba(255, 255, 255, 0.05);
  color: white;
  backdrop-filter: blur(10px);
}
```

### Pattern 3: Minimal Flat Design

```css
.minimal-accordion {
  border: none;
  background: transparent;
}

.minimal-accordion .e-accordion-item {
  border: none;
  margin: 8px 0;
  border-bottom: 2px solid #e0e0e0;
}

.minimal-accordion .e-accordion-header {
  background: transparent;
  color: #222;
  font-size: 16px;
  font-weight: 600;
  padding: 12px 0;
}

.minimal-accordion .e-accordion-content {
  padding: 12px 0;
  color: #666;
  background: transparent;
}
```

---

## Troubleshooting

**Issue: Styles not applying**
- Verify CSS file is imported in correct order (before component)
- Check CSS specificity conflicts
- Use DevTools to inspect applied styles
- Clear browser cache

**Issue: Theme not changing**
- Ensure correct theme CSS file path
- Verify theme imports dependency files
- Check that all required theme files are loaded
- Restart development server

**Issue: RTL not working**
- Verify `dir="rtl"` is on parent element
- Check that language is set correctly
- Inspect element to confirm RTL class applied
- Test with actual RTL content (Arabic, Hebrew)

**Issue: Responsive styles not working**
- Verify media queries are correct
- Check viewport meta tag is present
- Use mobile device or browser dev tools
- Test at actual breakpoint widths
