# Advanced Features

## Table of Contents
- [Nested Accordions](#nested-accordions)
- [Events and Lifecycle](#events-and-lifecycle)
- [React Hooks Integration](#react-hooks-integration)
- [Keyboard Navigation](#keyboard-navigation)
- [Accessibility Features](#accessibility-features)
- [Performance Optimization](#performance-optimization)
- [Advanced Patterns](#advanced-patterns)

## Component Methods

Programmatically control the accordion using component methods.

### Methods Reference

| Method | Parameters | Return | Description |
|--------|-----------|--------|-------------|
| `addItem(item, index)` | `item`: AccordionItem, `index?`: number | void | Add a new item to the accordion |
| `removeItem(index)` | `index`: number | void | Remove item at specified index |
| `enableItem(index, enable)` | `index`: number, `enable`: boolean | void | Enable or disable an item |
| `hideItem(index, hide)` | `index`: number, `hide`: boolean | void | Hide or show an item |
| `expandItem(expand, index)` | `expand`: boolean, `index`: number | void | Expand or collapse specific item |
| `select(index)` | `index`: number | void | Select/expand item at index |
| `destroy()` | - | void | Destroy the component |

### Adding Items Dynamically

Use `addItem()` to insert new accordion items at runtime:

```jsx
import React, { useRef, useState } from 'react';
import { 
  AccordionComponent, 
  AccordionItemDirective, 
  AccordionItemsDirective 
} from '@syncfusion/ej2-react-navigations';

export default function App() {
  const accordionRef = useRef(null);
  const [itemCount, setItemCount] = useState(2);

  const addNewItem = () => {
    const newIndex = itemCount;
    const newItem = {
      header: `Item ${newIndex + 1}`,
      content: `Content for item ${newIndex + 1}`
    };

    accordionRef.current?.addItem(newItem, newIndex);
    setItemCount(newIndex + 1);
  };

  return (
    <div>
      <button onClick={addNewItem}>Add New Item</button>

      <AccordionComponent ref={accordionRef}>
        <AccordionItemsDirective>
          <AccordionItemDirective header='Item 1' content='Content 1' />
          <AccordionItemDirective header='Item 2' content='Content 2' />
        </AccordionItemsDirective>
      </AccordionComponent>
    </div>
  );
}
```

### Removing Items

Use `removeItem()` to delete accordion items:

```jsx
import React, { useRef } from 'react';

export default function App() {
  const accordionRef = useRef(null);

  const removeLastItem = () => {
    const accordion = accordionRef.current;
    if (accordion && accordion.items.length > 0) {
      const lastIndex = accordion.items.length - 1;
      accordion.removeItem(lastIndex);
    }
  };

  const removeItemAt = (index) => {
    accordionRef.current?.removeItem(index);
  };

  return (
    <div>
      <button onClick={removeLastItem}>Remove Last Item</button>
      <button onClick={() => removeItemAt(0)}>Remove First Item</button>

      <AccordionComponent ref={accordionRef}>
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

### Enabling/Disabling Items

Use `enableItem()` to toggle item interactivity:

```jsx
import React, { useRef, useState } from 'react';

export default function App() {
  const accordionRef = useRef(null);
  const [disabledItems, setDisabledItems] = useState(new Set());

  const toggleItemState = (index) => {
    const accordion = accordionRef.current;
    const isCurrentlyDisabled = disabledItems.has(index);
    
    accordion?.enableItem(index, isCurrentlyDisabled);
    
    const newDisabled = new Set(disabledItems);
    if (isCurrentlyDisabled) {
      newDisabled.delete(index);
    } else {
      newDisabled.add(index);
    }
    setDisabledItems(newDisabled);
  };

  return (
    <div>
      <p>Disabled items: {Array.from(disabledItems).join(', ') || 'None'}</p>
      
      <button onClick={() => toggleItemState(0)}>Toggle Item 1</button>
      <button onClick={() => toggleItemState(1)}>Toggle Item 2</button>

      <AccordionComponent ref={accordionRef}>
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

### Hiding/Showing Items

Use `hideItem()` to toggle item visibility:

```jsx
import React, { useRef, useState } from 'react';

export default function App() {
  const accordionRef = useRef(null);
  const [visibleItems, setVisibleItems] = useState(
    new Set([0, 1, 2]) // All items initially visible
  );

  const toggleItemVisibility = (index) => {
    const accordion = accordionRef.current;
    const isCurrentlyVisible = visibleItems.has(index);
    
    accordion?.hideItem(index, isCurrentlyVisible);
    
    const newVisible = new Set(visibleItems);
    if (isCurrentlyVisible) {
      newVisible.delete(index);
    } else {
      newVisible.add(index);
    }
    setVisibleItems(newVisible);
  };

  const showAllItems = () => {
    for (let i = 0; i < 3; i++) {
      accordionRef.current?.hideItem(i, false);
    }
    setVisibleItems(new Set([0, 1, 2]));
  };

  return (
    <div>
      <button onClick={showAllItems}>Show All Items</button>
      <button onClick={() => toggleItemVisibility(0)}>Toggle Item 1</button>
      <button onClick={() => toggleItemVisibility(1)}>Toggle Item 2</button>

      <AccordionComponent ref={accordionRef}>
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

### Expanding/Collapsing Items

Use `expandItem()` to programmatically control expand state:

```jsx
import React, { useRef } from 'react';

export default function App() {
  const accordionRef = useRef(null);

  const expandAll = () => {
    const accordion = accordionRef.current;
    if (accordion) {
      for (let i = 0; i < accordion.items.length; i++) {
        accordion.expandItem(true, i);
      }
    }
  };

  const collapseAll = () => {
    const accordion = accordionRef.current;
    if (accordion) {
      for (let i = 0; i < accordion.items.length; i++) {
        accordion.expandItem(false, i);
      }
    }
  };

  const toggleItemExpand = (index) => {
    const accordion = accordionRef.current;
    if (accordion) {
      // Get current state and toggle
      const header = accordion.element.querySelectorAll('.e-accordion-header')[index];
      const isExpanded = header?.classList.contains('e-selected');
      accordion.expandItem(!isExpanded, index);
    }
  };

  return (
    <div>
      <button onClick={expandAll}>Expand All</button>
      <button onClick={collapseAll}>Collapse All</button>
      <button onClick={() => toggleItemExpand(0)}>Toggle Item 1</button>

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

### Selecting Items

Use `select()` method as a shorthand to expand an item:

```jsx
import React, { useRef } from 'react';

export default function App() {
  const accordionRef = useRef(null);

  const selectItem = (index) => {
    accordionRef.current?.select(index);
  };

  return (
    <div>
      <button onClick={() => selectItem(0)}>Select Item 1</button>
      <button onClick={() => selectItem(1)}>Select Item 2</button>
      <button onClick={() => selectItem(2)}>Select Item 3</button>

      <AccordionComponent ref={accordionRef}>
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

### Destroying Components

Use `destroy()` to clean up the accordion:

```jsx
import React, { useRef } from 'react';

export default function App() {
  const accordionRef = useRef(null);

  const destroyAccordion = () => {
    accordionRef.current?.destroy();
    console.log('Accordion destroyed');
  };

  return (
    <div>
      <button onClick={destroyAccordion}>Destroy Accordion</button>

      <AccordionComponent ref={accordionRef}>
        <AccordionItemsDirective>
          <AccordionItemDirective header='Item 1' content='Content 1' />
          <AccordionItemDirective header='Item 2' content='Content 2' />
        </AccordionItemsDirective>
      </AccordionComponent>
    </div>
  );
}
```

### Complete Example: Dynamic Item Management

```jsx
import React, { useRef, useState } from 'react';
import { 
  AccordionComponent, 
  AccordionItemDirective, 
  AccordionItemsDirective 
} from '@syncfusion/ej2-react-navigations';

export default function App() {
  const accordionRef = useRef(null);
  const [items, setItems] = useState([
    { id: 1, header: 'Item 1', content: 'Content 1' },
    { id: 2, header: 'Item 2', content: 'Content 2' }
  ]);

  const addItem = () => {
    const newId = Math.max(...items.map(i => i.id), 0) + 1;
    const newItem = {
      id: newId,
      header: `Item ${newId}`,
      content: `Content ${newId}`
    };
    
    setItems([...items, newItem]);
    accordionRef.current?.addItem(newItem);
  };

  const removeItem = (index) => {
    accordionRef.current?.removeItem(index);
    setItems(items.filter((_, i) => i !== index));
  };

  const expandAllItems = () => {
    for (let i = 0; i < items.length; i++) {
      accordionRef.current?.expandItem(true, i);
    }
  };

  const collapseAllItems = () => {
    for (let i = 0; i < items.length; i++) {
      accordionRef.current?.expandItem(false, i);
    }
  };

  return (
    <div>
      <div style={{ marginBottom: '20px', display: 'flex', gap: '10px' }}>
        <button onClick={addItem}>Add Item</button>
        <button onClick={expandAllItems}>Expand All</button>
        <button onClick={collapseAllItems}>Collapse All</button>
      </div>

      <AccordionComponent ref={accordionRef} expandMode='Multiple'>
        <AccordionItemsDirective>
          {items.map((item, index) => (
            <AccordionItemDirective 
              key={item.id}
              header={item.header}
              content={() => (
                <div>
                  {item.content}
                  <button onClick={() => removeItem(index)}>Remove</button>
                </div>
              )}
            />
          ))}
        </AccordionItemsDirective>
      </AccordionComponent>
    </div>
  );
}
```

## Nested Accordions

Create hierarchical accordion structures with nested panels:

### Basic Nested Accordion

```jsx
import React from 'react';
import { 
  AccordionComponent, 
  AccordionItemDirective, 
  AccordionItemsDirective 
} from '@syncfusion/ej2-react-navigations';

function NestedAccordionContent() {
  return (
    <AccordionComponent>
      <AccordionItemsDirective>
        <AccordionItemDirective 
          header='Nested Item 1' 
          content='Nested content 1' 
        />
        <AccordionItemDirective 
          header='Nested Item 2' 
          content='Nested content 2' 
        />
      </AccordionItemsDirective>
    </AccordionComponent>
  );
}

export default function App() {
  return (
    <AccordionComponent expandMode='Single'>
      <AccordionItemsDirective>
        <AccordionItemDirective 
          header='Parent 1' 
          content={() => <NestedAccordionContent />}
        />
        <AccordionItemDirective 
          header='Parent 2' 
          content='Simple content' 
        />
      </AccordionItemsDirective>
    </AccordionComponent>
  );
}
```

### Multi-Level Nesting

```jsx
function Level3Accordion() {
  return (
    <AccordionComponent expandMode='Multiple'>
      <AccordionItemsDirective>
        <AccordionItemDirective header='Level 3-A' content='Deep content A' />
        <AccordionItemDirective header='Level 3-B' content='Deep content B' />
      </AccordionItemsDirective>
    </AccordionComponent>
  );
}

function Level2Accordion() {
  return (
    <AccordionComponent expandMode='Multiple'>
      <AccordionItemsDirective>
        <AccordionItemDirective 
          header='Level 2-A' 
          content={() => <Level3Accordion />}
        />
        <AccordionItemDirective 
          header='Level 2-B' 
          content='Content 2-B' 
        />
      </AccordionItemsDirective>
    </AccordionComponent>
  );
}

export default function App() {
  return (
    <AccordionComponent>
      <AccordionItemsDirective>
        <AccordionItemDirective 
          header='Level 1' 
          content={() => <Level2Accordion />}
        />
      </AccordionItemsDirective>
    </AccordionComponent>
  );
}
```

### Styling Nested Accordions

```css
/* Parent accordion styling */
.e-accordion {
  background: white;
  border-radius: 8px;
}

/* Nested accordion styling (one level deeper) */
.e-accordion .e-accordion-content .e-accordion {
  border: 1px solid #e0e0e0;
  border-radius: 4px;
  margin-top: 10px;
}

/* Third level nesting */
.e-accordion .e-accordion-content .e-accordion .e-accordion-content .e-accordion {
  border: 1px solid #f0f0f0;
  background: #fafafa;
}
```

## Events and Lifecycle

Handle accordion interactions using event callbacks:

### Available Events

```jsx
import React, { useState } from 'react';
import { 
  AccordionComponent, 
  AccordionItemDirective, 
  AccordionItemsDirective 
} from '@syncfusion/ej2-react-navigations';

export default function App() {
  const [eventLog, setEventLog] = useState([]);

  const onExpand = (args) => {
    console.log('Panel expanding:', args);
    setEventLog(prev => [...prev, `Expanding item ${args.index}`]);
  };

  const onExpanded = (args) => {
    console.log('Panel expanded:', args);
    setEventLog(prev => [...prev, `Expanded item ${args.index}`]);
  };

  const onCollapse = (args) => {
    console.log('Panel collapsing:', args);
    setEventLog(prev => [...prev, `Collapsing item ${args.index}`]);
  };

  const onCollapsed = (args) => {
    console.log('Panel collapsed:', args);
    setEventLog(prev => [...prev, `Collapsed item ${args.index}`]);
  };

  return (
    <div>
      <AccordionComponent
        expanding={onExpand}
        expanded={onExpanded}
        collapsing={onCollapse}
        collapsed={onCollapsed}
      >
        <AccordionItemsDirective>
          <AccordionItemDirective header='Item 1' content='Content 1' />
          <AccordionItemDirective header='Item 2' content='Content 2' />
        </AccordionItemsDirective>
      </AccordionComponent>

      <div style={{ marginTop: '20px', padding: '10px', backgroundColor: '#f5f5f5' }}>
        <h4>Event Log:</h4>
        {eventLog.map((event, index) => (
          <div key={index}>{event}</div>
        ))}
      </div>
    </div>
  );
}
```

### Event Properties

Each event receives an `args` object with:

```js
{
  index: number,              // Index of accordion item (0-based)
  item: HTMLElement,          // DOM element of accordion item
  content: HTMLElement,       // Content element
  header: HTMLElement,        // Header element
  isInteracted: boolean,      // Whether user interacted or programmatic
  name: string                // Event name
}
```

### Preventing Expand/Collapse

```jsx
const onExpanding = (args) => {
  if (/* some condition */) {
    args.cancel = true;  // Prevent expand
  }
};

<AccordionComponent expanding={onExpanding}>
  {/* items */}
</AccordionComponent>
```

## React Hooks Integration

Use React hooks to manage accordion state:

### useState for Controlled State

```jsx
import React, { useState } from 'react';
import { 
  AccordionComponent, 
  AccordionItemDirective, 
  AccordionItemsDirective 
} from '@syncfusion/ej2-react-navigations';

export default function App() {
  const [expandedItems, setExpandedItems] = useState([0]);  // Item 0 expanded

  const handleExpanded = (args) => {
    setExpandedItems(prev => 
      prev.includes(args.index) ? prev : [...prev, args.index]
    );
  };

  const handleCollapsed = (args) => {
    setExpandedItems(prev => prev.filter(i => i !== args.index));
  };

  return (
    <div>
      <p>Expanded items: {expandedItems.join(', ')}</p>

      <AccordionComponent
        expanded={handleExpanded}
        collapsed={handleCollapsed}
      >
        <AccordionItemsDirective>
          <AccordionItemDirective 
            expanded={expandedItems.includes(0)}
            header='Item 1' 
            content='Content 1' 
          />
          <AccordionItemDirective 
            expanded={expandedItems.includes(1)}
            header='Item 2' 
            content='Content 2' 
          />
        </AccordionItemsDirective>
      </AccordionComponent>
    </div>
  );
}
```

### useRef for Direct Component Access

```jsx
import React, { useRef } from 'react';
import { AccordionComponent, AccordionItemDirective, AccordionItemsDirective } from '@syncfusion/ej2-react-navigations';

export default function App() {
  const accordionRef = useRef(null);

  const expandAll = () => {
    const accordion = accordionRef.current;
    // Expand all items programmatically
    for (let i = 0; i < accordion.items.length; i++) {
      accordion.expandItem(true, i);
    }
  };

  const collapseAll = () => {
    const accordion = accordionRef.current;
    for (let i = 0; i < accordion.items.length; i++) {
      accordion.collapseItem(i);
    }
  };

  return (
    <div>
      <button onClick={expandAll}>Expand All</button>
      <button onClick={collapseAll}>Collapse All</button>

      <AccordionComponent ref={accordionRef}>
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

### useEffect for Side Effects

```jsx
import React, { useState, useEffect, useRef } from 'react';

export default function App() {
  const [itemCount, setItemCount] = useState(3);
  const accordionRef = useRef(null);

  useEffect(() => {
    console.log(`Item count changed to ${itemCount}`);
    // Perform cleanup or updates when itemCount changes
  }, [itemCount]);

  useEffect(() => {
    // Expand first item on mount
    if (accordionRef.current) {
      accordionRef.current.expandItem(true, 0);
    }
  }, []);

  return (
    <div>
      <AccordionComponent ref={accordionRef}>
        <AccordionItemsDirective>
          {/* Generate items based on itemCount */}
          {Array.from({ length: itemCount }).map((_, i) => (
            <AccordionItemDirective 
              key={i}
              header={`Item ${i + 1}`} 
              content={`Content ${i + 1}`} 
            />
          ))}
        </AccordionItemsDirective>
      </AccordionComponent>
    </div>
  );
}
```

## Keyboard Navigation

The Accordion supports full keyboard navigation by default:

### Navigation Keys

| Key | Action |
|-----|--------|
| `Space` / `Enter` | Expand/collapse focused header |
| `Down Arrow` | Move focus to next header |
| `Up Arrow` | Move focus to previous header |
| `Home` | Move focus to first header |
| `End` | Move focus to last header |
| `Tab` | Move to next focusable element |
| `Shift + Tab` | Move to previous focusable element |

### Enable/Disable Keyboard Navigation

Keyboard navigation is enabled by default. To programmatically control focus:

```jsx
import React, { useRef } from 'react';

export default function App() {
  const accordionRef = useRef(null);

  const focusHeader = (index) => {
    if (accordionRef.current) {
      const header = accordionRef.current.element
        .querySelectorAll('.e-accordion-header')[index];
      if (header) {
        header.focus();
      }
    }
  };

  return (
    <div>
      <button onClick={() => focusHeader(0)}>Focus Item 1</button>
      <button onClick={() => focusHeader(1)}>Focus Item 2</button>

      <AccordionComponent ref={accordionRef}>
        <AccordionItemsDirective>
          <AccordionItemDirective header='Item 1' content='Content 1' />
          <AccordionItemDirective header='Item 2' content='Content 2' />
        </AccordionItemsDirective>
      </AccordionComponent>
    </div>
  );
}
```

## Accessibility Features

The Accordion is fully accessible with built-in ARIA support:

### ARIA Attributes

| Attribute | Purpose | Value |
|-----------|---------|-------|
| `role="button"` | Header role | Indicates clickable element |
| `aria-expanded` | Expand state | `true` or `false` |
| `aria-controls` | Content control | Points to content panel ID |
| `aria-labelledby` | Content label | Points to header ID |
| `aria-hidden` | Content visibility | `true` (collapsed) or `false` (expanded) |

These are automatically set by the component.

### Screen Reader Example

```jsx
// With proper ARIA attributes, screen readers announce:
// "Item 1 button, collapsed, press Space to expand"
// On expand:
// "Item 1 button, expanded, press Space to collapse"

<AccordionComponent>
  <AccordionItemsDirective>
    <AccordionItemDirective 
      header='Frequently Asked Questions'
      content='Common questions and answers'
    />
  </AccordionItemsDirective>
</AccordionComponent>
```

### Testing Accessibility

```jsx
// Test with accessibility tools
import { axe, toHaveNoViolations } from 'jest-axe';

test('Accordion has no accessibility violations', async () => {
  const { container } = render(
    <AccordionComponent>
      <AccordionItemsDirective>
        <AccordionItemDirective header='Item' content='Content' />
      </AccordionItemsDirective>
    </AccordionComponent>
  );
  
  const results = await axe(container);
  expect(results).toHaveNoViolations();
});
```

## Performance Optimization

### Virtualization for Large Lists

For 100+ items, use virtualization:

```jsx
import React, { useState, useEffect } from 'react';
import { FixedSizeList } from 'react-window';

export default function App() {
  const [items, setItems] = useState([]);

  useEffect(() => {
    // Generate 1000 items
    const largeDataset = Array.from({ length: 1000 }, (_, i) => ({
      id: i,
      header: `Item ${i + 1}`,
      content: `Content for item ${i + 1}`
    }));
    setItems(largeDataset);
  }, []);

  const Row = ({ index, style }) => (
    <div style={style}>
      <div className='e-accordion-item'>
        <div className='e-accordion-header'>
          {items[index].header}
        </div>
        <div className='e-accordion-content'>
          {items[index].content}
        </div>
      </div>
    </div>
  );

  return (
    <FixedSizeList
      height={600}
      itemCount={items.length}
      itemSize={50}
      width='100%'
    >
      {Row}
    </FixedSizeList>
  );
}
```

### Pagination Instead of Virtual Scroll

```jsx
import React, { useState } from 'react';
import { 
  AccordionComponent, 
  AccordionItemDirective, 
  AccordionItemsDirective 
} from '@syncfusion/ej2-react-navigations';

export default function App() {
  const [currentPage, setCurrentPage] = useState(1);
  const itemsPerPage = 10;

  const allItems = Array.from({ length: 500 }, (_, i) => ({
    id: i,
    header: `Item ${i + 1}`,
    content: `Content ${i + 1}`
  }));

  const startIdx = (currentPage - 1) * itemsPerPage;
  const paginatedItems = allItems.slice(startIdx, startIdx + itemsPerPage);

  return (
    <div>
      <AccordionComponent>
        <AccordionItemsDirective>
          {paginatedItems.map((item) => (
            <AccordionItemDirective 
              key={item.id}
              header={item.header}
              content={item.content}
            />
          ))}
        </AccordionItemsDirective>
      </AccordionComponent>

      <div style={{ marginTop: '20px' }}>
        <button 
          onClick={() => setCurrentPage(p => Math.max(1, p - 1))}
          disabled={currentPage === 1}
        >
          Previous
        </button>
        <span> Page {currentPage} </span>
        <button 
          onClick={() => setCurrentPage(p => p + 1)}
          disabled={startIdx + itemsPerPage >= allItems.length}
        >
          Next
        </button>
      </div>
    </div>
  );
}
```

### Memoization for Performance

```jsx
import React, { memo, useState } from 'react';

// Memoized item component
const AccordionItem = memo(({ header, content }) => (
  <AccordionItemDirective header={header} content={content} />
));

export default function App() {
  const [items] = useState(Array.from({ length: 100 }, (_, i) => ({
    header: `Item ${i + 1}`,
    content: `Content ${i + 1}`
  })));

  return (
    <AccordionComponent>
      <AccordionItemsDirective>
        {items.map((item, index) => (
          <AccordionItem 
            key={index}
            header={item.header}
            content={item.content}
          />
        ))}
      </AccordionItemsDirective>
    </AccordionComponent>
  );
}
```

## Advanced Patterns

### Pattern 1: Synchronized Accordions

Multiple accordions that expand/collapse together:

```jsx
import React, { useState } from 'react';

export default function App() {
  const [expandedIndex, setExpandedIndex] = useState(0);

  return (
    <div>
      <h3>Accordion 1</h3>
      <AccordionComponent>
        <AccordionItemsDirective>
          <AccordionItemDirective 
            expanded={expandedIndex === 0}
            header='Item 1-A' 
            content='...' 
          />
          <AccordionItemDirective 
            expanded={expandedIndex === 1}
            header='Item 1-B' 
            content='...' 
          />
        </AccordionItemsDirective>
      </AccordionComponent>

      <h3>Accordion 2</h3>
      <AccordionComponent>
        <AccordionItemsDirective>
          <AccordionItemDirective 
            expanded={expandedIndex === 0}
            header='Item 2-A' 
            content='...' 
          />
          <AccordionItemDirective 
            expanded={expandedIndex === 1}
            header='Item 2-B' 
            content='...' 
          />
        </AccordionItemsDirective>
      </AccordionComponent>
    </div>
  );
}
```

### Pattern 2: Search/Filter within Accordion

```jsx
import React, { useState, useMemo } from 'react';

export default function App() {
  const [searchTerm, setSearchTerm] = useState('');

  const allItems = [
    { id: 1, header: 'React Basics', content: 'Learn React...' },
    { id: 2, header: 'React Hooks', content: 'Master Hooks...' },
    { id: 3, header: 'React Router', content: 'Navigation...' }
  ];

  const filteredItems = useMemo(() => {
    return allItems.filter(item =>
      item.header.toLowerCase().includes(searchTerm.toLowerCase())
    );
  }, [searchTerm]);

  return (
    <div>
      <input 
        placeholder='Search accordion items...'
        value={searchTerm}
        onChange={(e) => setSearchTerm(e.target.value)}
      />

      <AccordionComponent>
        <AccordionItemsDirective>
          {filteredItems.map((item) => (
            <AccordionItemDirective 
              key={item.id}
              header={item.header}
              content={item.content}
            />
          ))}
        </AccordionItemsDirective>
      </AccordionComponent>

      {filteredItems.length === 0 && <p>No items found</p>}
    </div>
  );
}
```

---

## Troubleshooting

**Issue: Events not firing**
- Verify event handler is attached correctly
- Check event name (expanded vs expanding)
- Ensure handler function accepts args parameter

**Issue: useRef not working**
- Verify ref is attached to component with `ref={accordionRef}`
- Check that ref.current exists before accessing
- Ensure component is fully rendered

**Issue: Performance degrading with many items**
- Implement pagination or virtualization
- Use React.memo() to prevent unnecessary re-renders
- Check DevTools Performance tab for bottlenecks

**Issue: Accessibility features not working**
- Verify keyboard navigation is not disabled
- Test with screen readers (NVDA, JAWS, VoiceOver)
- Check ARIA attributes in DevTools

