# Getting Started with Accordion

## Table of Contents
- [Installation](#installation)
- [CSS Imports](#css-imports)
- [Basic Setup](#basic-setup)
- [Initialization Methods](#initialization-methods)
- [Minimal Working Example](#minimal-working-example)
- [Item Configuration](#item-configuration)
- [Tracking Expanded Items with expandedIndices](#tracking-expanded-items-with-expandedindices)
- [Customizing Headers with headerTemplate](#customizing-headers-with-headertemplate)
- [Customizing Appearance](#customizing-appearance)

## Installation

Install the Syncfusion React navigations package using npm:

```bash
npm install @syncfusion/ej2-react-navigations --save
```

This package includes the Accordion component and its dependencies:
- `@syncfusion/ej2-base` - Core utilities
- `@syncfusion/ej2-react-base` - React bindings
- `@syncfusion/ej2-navigations` - Navigation components
- `@syncfusion/ej2-buttons` - Button component (used by headers)
- `@syncfusion/ej2-popups` - Popup utilities

## CSS Imports

Add component styles to your application. Choose the theme that matches your design:

```css
/* In your App.css or App.tsx */
@import '../node_modules/@syncfusion/ej2-base/styles/tailwind3.css';
@import '../node_modules/@syncfusion/ej2-buttons/styles/tailwind3.css';
@import '../node_modules/@syncfusion/ej2-popups/styles/tailwind3.css';
@import '../node_modules/@syncfusion/ej2-react-navigations/styles/tailwind3.css';
```

**Available Themes:**
- `tailwind3.css` - Modern Tailwind design
- `bootstrap5.3.css` - Bootstrap 5.3 styling
- `fluent2.css` - Microsoft Fluent 2 design
- `material3.css` - Material Design 3

Then import the CSS file in your React component:

```jsx
import './App.css';
```

## Basic Setup

Import the Accordion component and required directives:

```jsx
import { 
  AccordionComponent, 
  AccordionItemDirective, 
  AccordionItemsDirective 
} from '@syncfusion/ej2-react-navigations';
```

These three components work together:
- `AccordionComponent` - Main container
- `AccordionItemsDirective` - Wrapper for items
- `AccordionItemDirective` - Individual collapsible panels

## Initialization Methods

### Method 1: Using Items API (Recommended)

Declare accordion items using component directives:

```jsx
import React from 'react';
import { 
  AccordionComponent, 
  AccordionItemDirective, 
  AccordionItemsDirective 
} from '@syncfusion/ej2-react-navigations';
import './App.css';

export default function App() {
  const aspContent = () => (
    <div>Microsoft ASP.NET is a set of technologies for building Web applications.</div>
  );

  const mvcContent = () => (
    <div>The Model-View-Controller (MVC) architectural pattern separates an application into three main components.</div>
  );

  const jsContent = () => (
    <div>JavaScript (JS) is an interpreted computer programming language.</div>
  );

  return (
    <AccordionComponent>
      <AccordionItemsDirective>
        <AccordionItemDirective header='ASP.NET' content={aspContent} />
        <AccordionItemDirective header='ASP.NET MVC' content={mvcContent} />
        <AccordionItemDirective header='JavaScript' content={jsContent} />
      </AccordionItemsDirective>
    </AccordionComponent>
  );
}
```

**Benefits:** Clear component structure, easy to manage state, preferred for React applications

### Method 2: Using HTML Markup

Use native HTML elements as accordion structure:

```jsx
import React from 'react';
import { AccordionComponent } from '@syncfusion/ej2-react-navigations';
import './App.css';

export default function App() {
  return (
    <AccordionComponent>
      <div>
        <div>
          <div>ASP.NET</div>
        </div>
        <div>
          <div>Microsoft ASP.NET is a set of technologies...</div>
        </div>
      </div>
      <div>
        <div>
          <div>ASP.NET MVC</div>
        </div>
        <div>
          <div>The Model-View-Controller (MVC) architectural pattern...</div>
        </div>
      </div>
    </AccordionComponent>
  );
}
```

**HTML Structure:**
```
AccordionComponent
  └─ div (item container)
      ├─ div (header container)
      │   └─ div (header text/content)
      └─ div (panel container)
          └─ div (panel content)
```

**When to use:** For simple static content or migrating from HTML-based templates

## Minimal Working Example

Complete working example with two collapsible panels:

```jsx
import React from 'react';
import { 
  AccordionComponent, 
  AccordionItemDirective, 
  AccordionItemsDirective 
} from '@syncfusion/ej2-react-navigations';
import '@syncfusion/ej2-base/styles/tailwind3.css';
import '@syncfusion/ej2-buttons/styles/tailwind3.css';
import '@syncfusion/ej2-popups/styles/tailwind3.css';
import '@syncfusion/ej2-react-navigations/styles/tailwind3.css';

export default function App() {
  return (
    <div className='p-8'>
      <h1>My First Accordion</h1>
      
      <AccordionComponent expandMode='Multiple'>
        <AccordionItemsDirective>
          <AccordionItemDirective 
            header='What is React?' 
            content='React is a JavaScript library for building user interfaces with reusable components.' 
          />
          <AccordionItemDirective 
            header='What is JSX?' 
            content='JSX is a syntax extension that allows you to write HTML-like code in JavaScript.' 
          />
          <AccordionItemDirective 
            header='What are Hooks?' 
            content='Hooks are functions that let you "hook into" React state and lifecycle features.' 
          />
        </AccordionItemsDirective>
      </AccordionComponent>
    </div>
  );
}
```

**Run the application:**
```bash
npm run dev
```

The accordion will render with three collapsible panels, all initially collapsed. Click headers to expand/collapse.

## Item Configuration

Each accordion item can be configured with specific properties to control its appearance, behavior, and state.

### Item Properties Reference

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `header` | string | - | Header text or JSX component for the panel header |
| `content` | string \| JSX | - | Content text or JSX component for the panel body |
| `expanded` | boolean | false | Whether the item is initially expanded |
| `disabled` | boolean | false | Whether the item is disabled and cannot be clicked |
| `cssClass` | string | - | Custom CSS class to apply to the item |

### Basic Item Configuration

```jsx
import React from 'react';
import { 
  AccordionComponent, 
  AccordionItemDirective, 
  AccordionItemsDirective 
} from '@syncfusion/ej2-react-navigations';

export default function App() {
  return (
    <AccordionComponent>
      <AccordionItemsDirective>
        {/* Initially expanded item */}
        <AccordionItemDirective 
          header='Always Open'
          content='This item starts expanded'
          expanded={true}
        />
        
        {/* Disabled item */}
        <AccordionItemDirective 
          header='Locked Section'
          content='This item is disabled and cannot be clicked'
          disabled={true}
        />
        
        {/* Normal item */}
        <AccordionItemDirective 
          header='Regular Item'
          content='This is a normal collapsible item'
        />
      </AccordionItemsDirective>
    </AccordionComponent>
  );
}
```

### Setting Per-Item Initial Expansion

Use the `expanded` property to control which items are open when the component loads:

```jsx
<AccordionComponent expandMode='Multiple'>
  <AccordionItemsDirective>
    <AccordionItemDirective 
      expanded={true}
      header='Expanded by Default' 
      content='This panel opens automatically' 
    />
    <AccordionItemDirective 
      expanded={true}
      header='Also Expanded' 
      content='Multiple items can be open with Multiple mode' 
    />
    <AccordionItemDirective 
      expanded={false}
      header='Collapsed Item' 
      content='This one starts closed' 
    />
  </AccordionItemsDirective>
</AccordionComponent>
```

### Disabling Specific Items

Mark items as disabled to prevent user interaction:

```jsx
<AccordionComponent>
  <AccordionItemsDirective>
    <AccordionItemDirective 
      header='Available' 
      content='User can click this' 
    />
    <AccordionItemDirective 
      disabled={true}
      header='Coming Soon' 
      content='This feature will be available soon' 
    />
    <AccordionItemDirective 
      disabled={true}
      header='Premium Only' 
      content='This requires a premium subscription' 
    />
    <AccordionItemDirective 
      header='Available' 
      content='User can click this' 
    />
  </AccordionItemsDirective>
</AccordionComponent>
```

**Styling disabled items:**
```css
.e-accordion-item.e-disabled .e-accordion-header {
  opacity: 0.6;
  cursor: not-allowed;
}
```

### Applying Custom CSS to Items

Use the `cssClass` property to style individual items differently:

```jsx
<AccordionComponent>
  <AccordionItemsDirective>
    <AccordionItemDirective 
      header='Warning Section'
      content='This item has special styling'
      cssClass='warning-item'
    />
    <AccordionItemDirective 
      header='Success Section'
      content='This item indicates success'
      cssClass='success-item'
    />
    <AccordionItemDirective 
      header='Info Section'
      content='This item shows information'
      cssClass='info-item'
    />
  </AccordionItemsDirective>
</AccordionComponent>
```

**Styling each type:**
```css
/* Warning item styling */
.warning-item .e-accordion-header {
  background-color: #ff9800;
  color: white;
  border-left: 4px solid #e65100;
}

.warning-item .e-accordion-content {
  background-color: #fff3e0;
  border-left: 4px solid #ff9800;
}

/* Success item styling */
.success-item .e-accordion-header {
  background-color: #4caf50;
  color: white;
  border-left: 4px solid #2e7d32;
}

.success-item .e-accordion-content {
  background-color: #f1f8e9;
  border-left: 4px solid #4caf50;
}

/* Info item styling */
.info-item .e-accordion-header {
  background-color: #2196f3;
  color: white;
  border-left: 4px solid #1565c0;
}

.info-item .e-accordion-content {
  background-color: #e3f2fd;
  border-left: 4px solid #2196f3;
}
```

### Dynamic Item Configuration

Configure items dynamically based on data:

```jsx
import React, { useState } from 'react';
import { 
  AccordionComponent, 
  AccordionItemDirective, 
  AccordionItemsDirective 
} from '@syncfusion/ej2-react-navigations';

export default function App() {
  const [items] = useState([
    { 
      header: 'FAQ #1',
      content: 'First frequently asked question...',
      expanded: true,
      disabled: false,
      cssClass: 'faq-item'
    },
    { 
      header: 'FAQ #2',
      content: 'Second frequently asked question...',
      expanded: false,
      disabled: false,
      cssClass: 'faq-item'
    },
    { 
      header: 'FAQ #3',
      content: 'Third frequently asked question...',
      expanded: false,
      disabled: true,
      cssClass: 'faq-item'
    }
  ]);

  return (
    <AccordionComponent>
      <AccordionItemsDirective>
        {items.map((item, index) => (
          <AccordionItemDirective
            key={index}
            header={item.header}
            content={item.content}
            expanded={item.expanded}
            disabled={item.disabled}
            cssClass={item.cssClass}
          />
        ))}
      </AccordionItemsDirective>
    </AccordionComponent>
  );
}
```

## Tracking Expanded Items with expandedIndices

The `expandedIndices` property returns an array of indices for currently expanded items. Use this to track state or control which panels are open.

### Getting Currently Expanded Items

```jsx
import React, { useRef } from 'react';
import { 
  AccordionComponent, 
  AccordionItemDirective, 
  AccordionItemsDirective 
} from '@syncfusion/ej2-react-navigations';

export default function App() {
  const accordionRef = useRef(null);

  const checkExpandedItems = () => {
    const expanded = accordionRef.current?.expandedIndices;
    console.log('Currently expanded items:', expanded);
    // Output example: [0, 2] means items at index 0 and 2 are expanded
  };

  return (
    <div>
      <button onClick={checkExpandedItems}>Check Expanded Items</button>

      <AccordionComponent ref={accordionRef} expandMode='Multiple'>
        <AccordionItemsDirective>
          <AccordionItemDirective header='Item 1' content='Content 1' />
          <AccordionItemDirective header='Item 2' content='Content 2' />
          <AccordionItemDirective header='Item 3' content='Content 3' />
        </AccordionItemsDirective>
      </AccordionComponent>
    </div>
  );
}
```

### Using expandedIndices in Events

```jsx
import React, { useRef, useState } from 'react';
import { ExpandedEventArgs } from '@syncfusion/ej2-navigations';

export default function App() {
  const accordionRef = useRef(null);
  const [expandedList, setExpandedList] = useState([]);

  const onExpanded = (args: ExpandedEventArgs) => {
    const expanded = accordionRef.current?.expandedIndices || [];
    setExpandedList(expanded);
    console.log('Expanded indices after change:', expanded);
  };

  return (
    <div>
      <p>Expanded items: [{expandedList.join(', ')}]</p>

      <AccordionComponent ref={accordionRef} expanded={onExpanded} expandMode='Multiple'>
        <AccordionItemsDirective>
          <AccordionItemDirective header='Item 1' content='Content 1' />
          <AccordionItemDirective header='Item 2' content='Content 2' />
          <AccordionItemDirective header='Item 3' content='Content 3' />
        </AccordionItemsDirective>
      </AccordionComponent>
    </div>
  );
}
```

### Wizard Pattern with expandedIndices

Real-world example using `expandedIndices` to control a multi-step wizard:

```jsx
import React, { useRef } from 'react';

export default function App() {
  const accordionRef = useRef(null);

  const allowNext = (): boolean => {
    const expanded = accordionRef.current?.expandedIndices || [];
    // Only allow moving to next step if current step is complete
    return expanded[expanded.length - 1] >= 0;
  };

  const goToStep = (stepIndex: number) => {
    accordionRef.current?.expandItem(true, stepIndex);
  };

  return (
    <div>
      <div style={{ marginBottom: '15px' }}>
        <button onClick={() => goToStep(0)}>Step 1: Signin</button>
        <button onClick={() => goToStep(1)} disabled={!allowNext()}>
          Step 2: Address
        </button>
        <button onClick={() => goToStep(2)} disabled={!allowNext()}>
          Step 3: Payment
        </button>
      </div>

      <AccordionComponent ref={accordionRef} expandMode='Single'>
        <AccordionItemsDirective>
          <AccordionItemDirective expanded={true} header='Sign In' content='Email and password...' />
          <AccordionItemDirective header='Delivery Address' content='Address details...' />
          <AccordionItemDirective header='Card Details' content='Payment information...' />
        </AccordionItemsDirective>
      </AccordionComponent>
    </div>
  );
}
```

### Setting Initial expandedIndices

Set which items should be expanded when the component loads:

```jsx
<AccordionComponent expandMode='Multiple' expandedIndices={[0, 2]}>
  <AccordionItemsDirective>
    <AccordionItemDirective header='Item 1' content='Expanded on load' />
    <AccordionItemDirective header='Item 2' content='Collapsed on load' />
    <AccordionItemDirective header='Item 3' content='Expanded on load' />
  </AccordionItemsDirective>
</AccordionComponent>
```

## Customizing Headers with headerTemplate

The `headerTemplate` property allows you to render custom JSX or components as accordion headers instead of plain text.

### Basic Header Template

```jsx
import React from 'react';
import { 
  AccordionComponent, 
  AccordionItemDirective, 
  AccordionItemsDirective 
} from '@syncfusion/ej2-react-navigations';

export default function App() {
  const customHeader1 = () => (
    <div style={{ display: 'flex', alignItems: 'center', gap: '10px' }}>
      <span style={{ fontSize: '20px' }}>📋</span>
      <span>Document List</span>
    </div>
  );

  const customHeader2 = () => (
    <div style={{ display: 'flex', alignItems: 'center', gap: '10px' }}>
      <span style={{ fontSize: '20px' }}>⚙️</span>
      <span>Settings</span>
    </div>
  );

  return (
    <AccordionComponent>
      <AccordionItemsDirective>
        <AccordionItemDirective 
          headerTemplate={customHeader1}
          content='List of documents here'
        />
        <AccordionItemDirective 
          headerTemplate={customHeader2}
          content='Configuration settings here'
        />
      </AccordionItemsDirective>
    </AccordionComponent>
  );
}
```

### Header Template with Status Badge

```jsx
const headerWithBadge = () => (
  <div style={{ display: 'flex', justifyContent: 'space-between', alignItems: 'center' }}>
    <span>User Information</span>
    <span 
      style={{
        backgroundColor: '#4caf50',
        color: 'white',
        padding: '2px 8px',
        borderRadius: '12px',
        fontSize: '12px'
      }}
    >
      Complete
    </span>
  </div>
);

<AccordionComponent>
  <AccordionItemsDirective>
    <AccordionItemDirective 
      headerTemplate={headerWithBadge}
      content='User details form'
    />
  </AccordionItemsDirective>
</AccordionComponent>
```

### Header Template with Dynamic Data

```jsx
import React, { useState } from 'react';

export default function App() {
  const [items] = useState([
    { id: 1, title: 'Task 1', status: 'pending', priority: 'high' },
    { id: 2, title: 'Task 2', status: 'in-progress', priority: 'medium' },
    { id: 3, title: 'Task 3', status: 'completed', priority: 'low' }
  ]);

  const getStatusColor = (status: string) => {
    const colors = {
      'pending': '#ff9800',
      'in-progress': '#2196f3',
      'completed': '#4caf50'
    };
    return colors[status] || '#999';
  };

  const taskHeader = (item) => (
    <div style={{ display: 'flex', justifyContent: 'space-between', alignItems: 'center', width: '100%' }}>
      <div>
        <strong>{item.title}</strong>
        <span style={{ marginLeft: '10px', fontSize: '12px', color: '#666' }}>
          Priority: {item.priority}
        </span>
      </div>
      <span
        style={{
          backgroundColor: getStatusColor(item.status),
          color: 'white',
          padding: '4px 12px',
          borderRadius: '4px',
          fontSize: '12px'
        }}
      >
        {item.status}
      </span>
    </div>
  );

  return (
    <AccordionComponent>
      <AccordionItemsDirective>
        {items.map((item) => (
          <AccordionItemDirective 
            key={item.id}
            headerTemplate={() => taskHeader(item)}
            content={`Details for ${item.title}`}
          />
        ))}
      </AccordionItemsDirective>
    </AccordionComponent>
  );
}
```

### Header Template with Syncfusion Components

```jsx
import React from 'react';
import { ButtonComponent } from '@syncfusion/ej2-react-buttons';
import { ChipListComponent } from '@syncfusion/ej2-react-buttons';

export default function App() {
  const advancedHeader = () => (
    <div style={{ display: 'flex', justifyContent: 'space-between', alignItems: 'center' }}>
      <div>
        <h4>Advanced Settings</h4>
        <small>Configure component behavior</small>
      </div>
      <ButtonComponent cssClass='e-small'>Edit</ButtonComponent>
    </div>
  );

  return (
    <AccordionComponent>
      <AccordionItemsDirective>
        <AccordionItemDirective 
          headerTemplate={advancedHeader}
          content='Settings configuration here'
        />
      </AccordionItemsDirective>
    </AccordionComponent>
  );
}
```

### Header Template with Icons and Counters

```jsx
const headerWithCounter = (label: string, count: number) => (
  <div style={{ display: 'flex', justifyContent: 'space-between', alignItems: 'center' }}>
    <span>{label}</span>
    <span 
      style={{
        backgroundColor: '#f0f0f0',
        padding: '2px 8px',
        borderRadius: '50%',
        minWidth: '24px',
        textAlign: 'center',
        fontWeight: 'bold'
      }}
    >
      {count}
    </span>
  </div>
);

<AccordionComponent>
  <AccordionItemsDirective>
    <AccordionItemDirective 
      headerTemplate={() => headerWithCounter('Notifications', 5)}
      content='List of 5 notifications'
    />
    <AccordionItemDirective 
      headerTemplate={() => headerWithCounter('Messages', 12)}
      content='List of 12 messages'
    />
    <AccordionItemDirective 
      headerTemplate={() => headerWithCounter('Tasks', 3)}
      content='List of 3 tasks'
    />
  </AccordionItemsDirective>
</AccordionComponent>
```

## Customizing Appearance

### Adding Custom CSS Classes

Use the `cssClass` property to apply custom styles:

```jsx
<AccordionComponent cssClass='custom-accordion'>
  <AccordionItemsDirective>
    <AccordionItemDirective 
      header='Section 1' 
      content='Content here'
      cssClass='custom-item'
    />
  </AccordionItemsDirective>
</AccordionComponent>
```

Then style in your CSS:

```css
.custom-accordion {
  background-color: #f5f5f5;
  border-radius: 8px;
}

.custom-item .e-accordion-header {
  background-color: #007bff;
  color: white;
  font-weight: bold;
}

.custom-item .e-accordion-content {
  padding: 20px;
}
```

### Using Built-in Classes

The Accordion generates these classes automatically:

```css
.e-accordion              /* Main container */
.e-accordion-item        /* Individual item */
.e-accordion-header      /* Header section */
.e-accordion-content     /* Content panel */
.e-accordion-control     /* Expanded item */
.e-disabled              /* Disabled item */
```

### Common Styling Tasks

**Change header background:**
```css
.e-accordion-header {
  background-color: #2c3e50;
  color: white;
}
```

**Add padding to content:**
```css
.e-accordion-content {
  padding: 20px 15px;
}
```

**Customize borders:**
```css
.e-accordion-item {
  border: 1px solid #ddd;
  margin: 10px 0;
}
```

---

## Next Steps

1. **Expand Modes** - Control whether one or multiple panels can be open
2. **Animation Effects** - Add smooth transitions when panels expand/collapse
3. **Content Loading** - Load content dynamically from data sources or APIs
4. **Advanced Features** - Nested accordions, events, and React hooks patterns

## Troubleshooting

**Issue: Styles not appearing**
- Verify all CSS imports are present in correct order
- Check that theme CSS file matches your chosen theme
- Ensure CSS file is imported before component usage

**Issue: Component not rendering**
- Confirm package is installed: `npm list @syncfusion/ej2-react-navigations`
- Check that component imports match your component names
- Verify React version compatibility

**Issue: Content not showing**
- For Items API: ensure `content` prop is provided
- For HTML markup: verify DOM structure follows required hierarchy
- Check browser console for errors
