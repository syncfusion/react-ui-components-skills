# Events and Lifecycle

## Table of Contents
- [Overview](#overview)
- [Component Lifecycle](#component-lifecycle)
- [Expand/Collapse Events](#expandcollapse-events)
- [Click Events](#click-events)
- [Event Arguments Reference](#event-arguments-reference)
- [Real-World Event Patterns](#real-world-event-patterns)
- [Preventing Default Actions](#preventing-default-actions)

## Overview

The Accordion component provides comprehensive event handling for tracking user interactions and component lifecycle. You can respond to expand/collapse actions, clicks, and initialization events using event callbacks.

## Component Lifecycle

### Created Event

Fires once the component rendering is completed.

```jsx
import React from 'react';
import { 
  AccordionComponent, 
  AccordionItemDirective, 
  AccordionItemsDirective 
} from '@syncfusion/ej2-react-navigations';

export default function App() {
  const onCreated = () => {
    console.log('Accordion component has been created and rendered');
    // Initialize external dependencies, set up observers, etc.
  };

  return (
    <AccordionComponent created={onCreated}>
      <AccordionItemsDirective>
        <AccordionItemDirective header='Item 1' content='Content 1' />
        <AccordionItemDirective header='Item 2' content='Content 2' />
      </AccordionItemsDirective>
    </AccordionComponent>
  );
}
```

**Use Cases:**
- Initialize data or state based on component readiness
- Set up event listeners on the component
- Load initial data from external sources
- Trigger animations or transitions
- Track component creation for analytics

### Destroyed Event

Fires when the component gets destroyed and removed from DOM.

```jsx
export default function App() {
  const onDestroyed = () => {
    console.log('Accordion has been destroyed');
    // Clean up resources, remove listeners, etc.
  };

  return (
    <AccordionComponent destroyed={onDestroyed}>
      {/* accordion items */}
    </AccordionComponent>
  );
}
```

**Use Cases:**
- Clean up event listeners
- Release memory and resources
- Stop timers or intervals
- Persist user state before removal
- Disconnect from external services

## Expand/Collapse Events

The Accordion fires events before and after expand/collapse actions, allowing you to validate, prevent, or respond to state changes.

### Expanding Event (Before Action)

Fires **before** an accordion item is expanded or collapsed. Use this to prevent the action.

```jsx
import React, { useRef } from 'react';
import { ExpandEventArgs } from '@syncfusion/ej2-navigations';

export default function App() {
  const accordionRef = useRef(null);

  const onExpanding = (args: ExpandEventArgs) => {
    console.log('Event Type:', args.name);           // 'expanding'
    console.log('Item Index:', args.index);          // Index of item
    console.log('Is Expanding?', args.isExpanded);   // true = expanding, false = collapsing
    console.log('Item Element:', args.item);         // DOM element
    
    // Prevent specific item from collapsing
    if (args.index === 0 && !args.isExpanded) {
      args.cancel = true;  // Prevent collapse
      console.log('Item 0 cannot be collapsed');
    }
  };

  return (
    <AccordionComponent ref={accordionRef} expanding={onExpanding}>
      <AccordionItemsDirective>
        <AccordionItemDirective 
          expanded={true}
          header='Always Open' 
          content='Cannot collapse this item' 
        />
        <AccordionItemDirective 
          header='Can Toggle' 
          content='This can expand and collapse' 
        />
      </AccordionItemsDirective>
    </AccordionComponent>
  );
}
```

**args Properties:**
- `name` - Event name ('expanding')
- `index` - Item index being expanded/collapsed
- `isExpanded` - true if expanding, false if collapsing
- `item` - The accordion item element
- `content` - Content element
- `header` - Header element
- `isInteracted` - true if user-triggered, false if programmatic
- `cancel` - Set to true to prevent the action

**Use Cases:**
- Validate before allowing collapse (keep at least one open)
- Prevent expanding/collapsing certain items
- Load content dynamically before expansion
- Track user interactions for analytics
- Implement custom expand/collapse logic

### Expanded Event (After Action)

Fires **after** an accordion item has been expanded or collapsed. Use this to respond to state changes.

```jsx
import React, { useState, useRef } from 'react';
import { ExpandedEventArgs } from '@syncfusion/ej2-navigations';

export default function App() {
  const [expandedItems, setExpandedItems] = useState([]);

  const onExpanded = (args: ExpandedEventArgs) => {
    console.log('Event Type:', args.name);           // 'expanded'
    console.log('Item Index:', args.index);          // Index of item
    console.log('Is Now Expanded?', args.isExpanded); // true = expanded, false = collapsed
    
    if (args.isExpanded) {
      setExpandedItems(prev => 
        prev.includes(args.index) ? prev : [...prev, args.index]
      );
    } else {
      setExpandedItems(prev => prev.filter(i => i !== args.index));
    }
  };

  return (
    <div>
      <p>Currently expanded items: {expandedItems.join(', ')}</p>
      
      <AccordionComponent expanded={onExpanded}>
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

**args Properties:**
- `name` - Event name ('expanded')
- `index` - Item index that was expanded/collapsed
- `isExpanded` - true if item is now expanded, false if collapsed
- `item` - The accordion item element
- `content` - Content element
- `header` - Header element
- `isInteracted` - true if user-triggered

**Use Cases:**
- Update UI based on expanded state
- Save user preferences
- Load or display related content
- Trigger animations or transitions
- Sync with parent component state
- Log user interactions

## Click Events

### Clicked Event

Fires whenever the user clicks anywhere within the Accordion component.

```jsx
import React, { useRef } from 'react';
import { AccordionClickArgs } from '@syncfusion/ej2-navigations';

export default function App() {
  const accordionRef = useRef(null);

  const onClicked = (args: AccordionClickArgs) => {
    console.log('Event Type:', args.name);           // 'clicked'
    console.log('Event Target:', args.originalEvent.target);  // Clicked element
    console.log('Item Index:', args.item);           // Index of item clicked
    
    // Detect what was clicked
    const target = args.originalEvent.target as HTMLElement;
    if (target.closest('.e-accordion-header')) {
      console.log('Header was clicked');
    } else if (target.closest('.e-accordion-content')) {
      console.log('Content area was clicked');
    }
  };

  return (
    <AccordionComponent ref={accordionRef} clicked={onClicked}>
      <AccordionItemsDirective>
        <AccordionItemDirective 
          header='Click anywhere' 
          content='Click to see events' 
        />
      </AccordionItemsDirective>
    </AccordionComponent>
  );
}
```

**args Properties:**
- `name` - Event name ('clicked')
- `originalEvent` - Native browser event
- `item` - Index of clicked item
- `element` - DOM element that was clicked

**Use Cases:**
- Track all user interactions
- Implement custom click handling
- Detect header vs content clicks
- Enable right-click context menus
- Implement drag-and-drop
- Analytics and user tracking

## Event Arguments Reference

### ExpandEventArgs (expanding event)

```typescript
{
  name: 'expanding';
  index: number;              // 0-based index of item
  isExpanded: boolean;        // true = expanding, false = collapsing
  item: AccordionItemModel;   // Item object
  content: HTMLElement;       // Content DOM element
  header: HTMLElement;        // Header DOM element
  isInteracted: boolean;      // User action vs programmatic
  cancel: boolean;            // Set true to prevent action
}
```

### ExpandedEventArgs (expanded event)

```typescript
{
  name: 'expanded';
  index: number;              // 0-based index of item
  isExpanded: boolean;        // true = expanded, false = collapsed
  item: AccordionItemModel;   // Item object
  content: HTMLElement;       // Content DOM element
  header: HTMLElement;        // Header DOM element
  isInteracted: boolean;      // User action vs programmatic
}
```

### AccordionClickArgs (clicked event)

```typescript
{
  name: 'clicked';
  originalEvent: MouseEvent;  // Native browser event
  item: number;               // Item index
  element: HTMLElement;       // Clicked element
}
```

## Real-World Event Patterns

### Pattern 1: Keep One Panel Always Open

Prevent the last open panel from closing in Single mode:

```jsx
import React, { useRef, useState } from 'react';
import { ExpandEventArgs } from '@syncfusion/ej2-navigations';

export default function App() {
  const accordionRef = useRef(null);
  const [clickTarget, setClickTarget] = useState(null);

  const onExpanding = (args: ExpandEventArgs) => {
    if (!args.isExpanded) {  // Trying to collapse
      const expandedCount = accordionRef.current?.element
        .querySelectorAll('.e-selected').length || 0;
      
      if (expandedCount === 1 && clickTarget === args.header) {
        args.cancel = true;  // Prevent last open panel from closing
      }
    }
  };

  const onClicked = (args: AccordionClickArgs) => {
    setClickTarget(args.originalEvent.target.closest('.e-accordion-header'));
  };

  return (
    <AccordionComponent 
      ref={accordionRef}
      expandMode='Single'
      expanding={onExpanding}
      clicked={onClicked}
    >
      <AccordionItemsDirective>
        <AccordionItemDirective 
          expanded={true}
          header='Always Open' 
          content='At least one panel must stay open' 
        />
        <AccordionItemDirective header='Item 2' content='Content 2' />
        <AccordionItemDirective header='Item 3' content='Content 3' />
      </AccordionItemsDirective>
    </AccordionComponent>
  );
}
```

### Pattern 2: Dynamic Content Loading on Expand

Load content from API only when panel expands:

```jsx
import React, { useRef, useState } from 'react';
import { ExpandedEventArgs } from '@syncfusion/ej2-navigations';

export default function App() {
  const accordionRef = useRef(null);
  const [contentCache, setContentCache] = useState({});
  const [loading, setLoading] = useState({});

  const onExpanded = async (args: ExpandedEventArgs) => {
    if (args.isExpanded && !contentCache[args.index]) {
      setLoading(prev => ({ ...prev, [args.index]: true }));
      
      try {
        const response = await fetch(`/api/content/${args.index}`);
        const data = await response.json();
        setContentCache(prev => ({
          ...prev,
          [args.index]: data.content
        }));
      } catch (error) {
        console.error('Failed to load content:', error);
      } finally {
        setLoading(prev => ({ ...prev, [args.index]: false }));
      }
    }
  };

  return (
    <AccordionComponent expanded={onExpanded}>
      <AccordionItemsDirective>
        <AccordionItemDirective 
          header='Lazy Load 1' 
          content={() => (
            <>
              {loading[0] && <p>Loading...</p>}
              {contentCache[0] && <p>{contentCache[0]}</p>}
            </>
          )}
        />
        <AccordionItemDirective 
          header='Lazy Load 2' 
          content={() => (
            <>
              {loading[1] && <p>Loading...</p>}
              {contentCache[1] && <p>{contentCache[1]}</p>}
            </>
          )}
        />
      </AccordionItemsDirective>
    </AccordionComponent>
  );
}
```

### Pattern 3: Custom Expand/Collapse Sequence

Use events to customize expand/collapse behavior:

```jsx
import React, { useRef, useState } from 'react';

export default function App() {
  const accordionRef = useRef(null);
  let isCollapsed = false;
  let expandIndex = null;

  const onExpanding = (args) => {
    if (args.isExpanded && !isCollapsed) {
      args.cancel = true;
      expandIndex = accordionRef.current.items.indexOf(args.item);
      isCollapsed = true;
    }
  };

  const onExpanded = (args) => {
    if (!args.isExpanded && isCollapsed) {
      accordionRef.current.expandItem(true, expandIndex);
      isCollapsed = false;
    }
  };

  return (
    <AccordionComponent
      ref={accordionRef}
      expandMode='Single'
      expanding={onExpanding}
      expanded={onExpanded}
    >
      <AccordionItemsDirective>
        <AccordionItemDirective header='Item 1' content='Content 1' />
        <AccordionItemDirective header='Item 2' content='Content 2' />
        <AccordionItemDirective header='Item 3' content='Content 3' />
      </AccordionItemsDirective>
    </AccordionComponent>
  );
}
```

## Preventing Default Actions

### Using args.cancel

Set `args.cancel = true` in the `expanding` event to prevent the action:

```jsx
const onExpanding = (args: ExpandEventArgs) => {
  // Prevent specific index from expanding
  if (args.index === 2) {
    args.cancel = true;
  }
};
```

### Conditional Prevention

```jsx
const onExpanding = (args: ExpandEventArgs) => {
  // Only allow expanding (prevent collapsing)
  if (!args.isExpanded) {
    args.cancel = true;
  }
};
```

### Validation-Based Prevention

```jsx
const onExpanding = (args: ExpandEventArgs) => {
  // Check some condition before allowing
  if (!userHasPermission && args.index === 0) {
    args.cancel = true;
    alert('You do not have permission to view this section');
  }
};
```

---

## Troubleshooting

**Issue: Events not firing**
- Verify event handler is attached correctly
- Check event name spelling (created vs create)
- Ensure handler function accepts args parameter

**Issue: Cannot prevent expand/collapse**
- Use `expanding` event, not `expanded` (only expanding allows cancel)
- Verify `args.cancel = true` is set
- Check browser console for errors

**Issue: State not updating after event**
- Use state management (useState) if updating component state
- Ensure event handler is properly bound
- Check if component is re-rendering

**Issue: Memory leaks from event listeners**
- Use proper cleanup if attaching DOM listeners
- Remove listeners on component destroy
- Be cautious with closures in event handlers

