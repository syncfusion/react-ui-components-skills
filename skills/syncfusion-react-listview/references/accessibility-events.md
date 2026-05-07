# Accessibility & Events

## Table of Contents
- [WCAG 2.1 Compliance](#wcag-21-compliance)
- [Keyboard Navigation](#keyboard-navigation)
- [Screen Reader Support](#screen-reader-support)
- [ARIA Attributes](#aria-attributes)
- [Focus Management](#focus-management)
- [Event Lifecycle](#event-lifecycle)
- [Complete Accessible Example](#complete-accessible-example)

## WCAG 2.1 Compliance

The Syncfusion ListView component is built to meet WCAG 2.1 Level AA standards.

### Compliance Features

```tsx
// 1. Semantic HTML Structure
// ListView renders proper list elements (<ul>, <li>)

// 2. Color Contrast
// All text meets 4.5:1 contrast ratio for normal text
// 3:1 for large text (18pt+)

// 3. Text Alternatives
// Icons have aria-label attributes

// 4. Keyboard Access
// All functionality accessible via keyboard

// 5. Focus Indicators
// Clear visible focus indicators on all interactive elements

<ListViewComponent
  // Built-in accessibility features
  htmlAttributes={{
    role: 'list',
    'aria-label': 'Navigation items'
  }}
/>
```

### Accessibility Checklist

- ✅ Semantic HTML structure
- ✅ Proper heading hierarchy
- ✅ Color contrast compliance
- ✅ Keyboard navigation support
- ✅ Screen reader support
- ✅ Focus management
- ✅ ARIA labels and roles
- ✅ Touch target size (min 44x44px)

## Keyboard Navigation

### Default Keyboard Shortcuts

```tsx
// Key Bindings:
// ↑ Arrow Up        - Select previous item
// ↓ Arrow Down      - Select next item
// Home              - Select first item
// End               - Select last item
// Enter/Space       - Activate/Toggle checkbox
// Shift + Click     - Multi-select range
// Ctrl + Click      - Toggle select
// Tab               - Move focus to next item (when focusable)
// Shift + Tab       - Move focus to previous item

<ListViewComponent
  // Keyboard navigation is enabled by default
/>
```

### Custom Keyboard Handler

```tsx
import { useRef } from 'react';

export function KeyboardAccessibleListView() {
  const listViewRef = useRef<ListViewComponent>(null);
  const [selectedIndex, setSelectedIndex] = useState(0);

  const handleKeyDown = (e: React.KeyboardEvent) => {
    const data = listViewRef.current?.dataSource as any[];

    switch (e.key) {
      case 'ArrowUp':
        e.preventDefault();
        setSelectedIndex(Math.max(0, selectedIndex - 1));
        listViewRef.current?.selectItem({ id: data[selectedIndex - 1]?.id });
        break;

      case 'ArrowDown':
        e.preventDefault();
        setSelectedIndex(Math.min(data.length - 1, selectedIndex + 1));
        listViewRef.current?.selectItem({ id: data[selectedIndex + 1]?.id });
        break;

      case 'Home':
        e.preventDefault();
        setSelectedIndex(0);
        listViewRef.current?.selectItem({ id: data[0]?.id });
        break;

      case 'End':
        e.preventDefault();
        setSelectedIndex(data.length - 1);
        listViewRef.current?.selectItem({ id: data[data.length - 1]?.id });
        break;

      case 'Enter':
      case ' ':
        e.preventDefault();
        // Handle activation
        break;
    }
  };

  return (
    <div onKeyDown={handleKeyDown}>
      <ListViewComponent ref={listViewRef} />
    </div>
  );
}
```

### Keyboard Navigation Interaction Matrix

| Key | Action | Precondition | Result |
|-----|--------|--------------|--------|
| ↑ | Previous Item | Any item focused | Focus moves to previous item |
| ↓ | Next Item | Any item focused | Focus moves to next item |
| Home | First Item | ListView focused | Focus moves to first item |
| End | Last Item | ListView focused | Focus moves to last item |
| Enter | Select/Activate | Item focused | Item selected; select event fired |
| Space | Toggle Checkbox | Checkbox enabled | Checkbox toggled |
| Shift + ↓ | Range Select | showCheckBox=true | Multiple items selected |
| Ctrl + A | Select All | showCheckBox=true | All items selected |

## Screen Reader Support

### Screen Reader Testing

```tsx
// Test with popular screen readers:
// - NVDA (Windows, open source)
// - JAWS (Windows, commercial)
// - VoiceOver (macOS, iOS, built-in)
// - TalkBack (Android, built-in)

export function ScreenReaderAccessibleListView() {
  return (
    <div role="application" aria-label="Task Management Application">
      <h1>Tasks</h1>

      <ListViewComponent
        htmlAttributes={{
          role: 'list',
          'aria-label': 'Task list with checkboxes'
        }}
        headerTitle="My Tasks"
        showHeader={true}
        showCheckBox={true}
      />
    </div>
  );
}
```

### Announcements

```tsx
import { useRef } from 'react';

export function ScreenReaderAnnouncementListView() {
  const listViewRef = useRef<ListViewComponent>(null);
  const [announcement, setAnnouncement] = useState('');

  const handleSelect = (args: any) => {
    // Announce to screen readers
    setAnnouncement(`Selected: ${args.text}`);
  };

  return (
    <div>
      {/* Live region for screen reader announcements */}
      <div
        role="status"
        aria-live="polite"
        aria-atomic="true"
        style={{ position: 'absolute', left: '-10000px' }}
      >
        {announcement}
      </div>

      <ListViewComponent
        ref={listViewRef}
        select={handleSelect}
      />
    </div>
  );
}
```

## ARIA Attributes

### Core ARIA Labels

```tsx
interface AccessibleListItem {
  id: string;
  text: string;
  description: string;
  disabled?: boolean;
  checked?: boolean;
}

const data: AccessibleListItem[] = [
  {
    id: '1',
    text: 'Inbox',
    description: 'View all inbox messages',
    disabled: false,
    checked: false
  }
];

export function AccessibleListViewWithARIA() {
  const itemTemplate = (props: AccessibleListItem) => (
    <div
      role="listitem"
      aria-label={`${props.text}. ${props.description}`}
      aria-disabled={props.disabled}
      aria-checked={props.checked}
    >
      <span>{props.text}</span>
      <small>{props.description}</small>
    </div>
  );

  return (
    <ListViewComponent
      dataSource={data}
      template={itemTemplate}
      htmlAttributes={{
        role: 'list',
        'aria-label': 'Navigation menu',
        'aria-describedby': 'list-instructions'
      }}
    />
  );
}
```

### ARIA Attributes Reference

| Attribute | Value | Purpose |
|-----------|-------|---------|
| `role` | `'list'`, `'listitem'` | Define semantic role |
| `aria-label` | String | Accessible label |
| `aria-labelledby` | ID reference | Label by element ID |
| `aria-describedby` | ID reference | Description by element ID |
| `aria-selected` | `true`, `false` | Item selection state |
| `aria-checked` | `true`, `false`, `'mixed'` | Checkbox state |
| `aria-disabled` | `true`, `false` | Item disabled state |
| `aria-readonly` | `true`, `false` | Read-only state |
| `aria-live` | `'polite'`, `'assertive'` | Live region updates |
| `aria-atomic` | `true`, `false` | Announce entire region |
| `aria-sort` | `'ascending'`, `'descending'`, `'none'` | Sort direction |

## Focus Management

### Tab Order and Focus

```tsx
import { useRef } from 'react';

export function FocusManagementListView() {
  const listViewRef = useRef<ListViewComponent>(null);
  const addButtonRef = useRef<HTMLButtonElement>(null);

  const handleKeyDown = (e: React.KeyboardEvent) => {
    // Handle Tab key for custom focus management
    if (e.key === 'Tab') {
      const focusableElements = [
        addButtonRef.current,
        listViewRef.current?.element
      ];

      const currentIndex = focusableElements.indexOf(e.target as any);
      const nextIndex = e.shiftKey
        ? currentIndex - 1
        : currentIndex + 1;

      if (nextIndex >= 0 && nextIndex < focusableElements.length) {
        focusableElements[nextIndex]?.focus();
      }
    }
  };

  return (
    <div onKeyDown={handleKeyDown}>
      <button ref={addButtonRef} aria-label="Add new item">
        Add Item
      </button>

      <ListViewComponent
        ref={listViewRef}
        tabIndex={0}
        htmlAttributes={{
          'aria-label': 'Item list'
        }}
      />
    </div>
  );
}
```

### Focus Indicators

```tsx
// CSS for visible focus indicators
const focusStyles = `
  .e-list-item:focus {
    outline: 2px solid #2196F3;
    outline-offset: 2px;
  }

  .e-list-item:focus-visible {
    box-shadow: 0 0 0 3px rgba(33, 150, 243, 0.3);
  }

  /* High contrast mode support */
  @media (prefers-contrast: more) {
    .e-list-item:focus {
      outline: 3px solid currentColor;
    }
  }
`;

// Apply to component
<ListViewComponent
  cssClass="accessible-list"
  htmlAttributes={{
    'data-focus-visible': 'true'
  }}
/>
```

### Programmatic Focus

```tsx
const handleMoveFocus = (direction: 'next' | 'prev') => {
  const items = document.querySelectorAll('.e-list-item');
  const currentIndex = Array.from(items).findIndex(
    item => item === document.activeElement
  );

  let nextIndex = currentIndex + (direction === 'next' ? 1 : -1);
  nextIndex = Math.max(0, Math.min(items.length - 1, nextIndex));

  (items[nextIndex] as HTMLElement)?.focus();
};
```

## Event Lifecycle

### Event Flow Diagram

```
User Interaction (click, keyboard, etc.)
    ↓
actionBegin fires
    ↓
Validation & Processing
    ↓
actionComplete fires (if successful)
      OR
actionFailure fires (if error)
    ↓
After event handlers complete
```

### Complete Event Example

```tsx
import { useRef, useState } from 'react';
import { ListViewComponent } from '@syncfusion/ej2-react-lists';

export function EventLifecycleExample() {
  const listViewRef = useRef<ListViewComponent>(null);
  const [eventLog, setEventLog] = useState<string[]>([]);

  const logEvent = (msg: string) => {
    console.log(msg);
    setEventLog(prev => [...prev, `${new Date().toLocaleTimeString()}: ${msg}`]);
  };

  const handleActionBegin = (args: any) => {
    logEvent(`[actionBegin] eventType: ${args.eventType}`);

    // Can cancel action here
    if (args.eventType === 'select') {
      if (args.data?.preventSelect) {
        args.cancel = true;
        logEvent('Selection cancelled');
      }
    }
  };

  const handleActionComplete = (args: any) => {
    logEvent(`[actionComplete] eventType: ${args.eventType}`);
  };

  const handleActionFailure = (args: any) => {
    logEvent(`[actionFailure] Error: ${args.error}`);
  };

  const handleSelect = (args: any) => {
    logEvent(`[select] Selected: ${args.text}`);
  };

  const handleScroll = (args: any) => {
    logEvent(
      `[scroll] scrollTop: ${args.scrollTop}, isAtEnd: ${args.isAtEnd}`
    );
  };

  const data = [
    { id: '1', text: 'Item 1' },
    { id: '2', text: 'Item 2' },
    { id: '3', text: 'Item 3' }
  ];

  return (
    <div style={{ display: 'grid', gridTemplateColumns: '1fr 1fr', gap: '20px' }}>
      <div>
        <h3>ListView</h3>
        <ListViewComponent
          ref={listViewRef}
          dataSource={data}
          height="300px"
          actionBegin={handleActionBegin}
          actionComplete={handleActionComplete}
          actionFailure={handleActionFailure}
          select={handleSelect}
          scroll={handleScroll}
        />
      </div>

      <div>
        <h3>Event Log</h3>
        <div
          style={{
            border: '1px solid #ddd',
            padding: '10px',
            height: '300px',
            overflowY: 'auto',
            fontSize: '12px',
            fontFamily: 'monospace'
          }}
        >
          {eventLog.map((log, idx) => (
            <div key={idx}>{log}</div>
          ))}
        </div>
      </div>
    </div>
  );
}
```

## Complete Accessible Example

```tsx
import { useRef, useState } from 'react';
import { ListViewComponent } from '@syncfusion/ej2-react-lists';

interface AccessibleItem {
  id: string;
  text: string;
  description: string;
  completed: boolean;
}

export function CompleteAccessibleListView() {
  const listViewRef = useRef<ListViewComponent>(null);
  const [items, setItems] = useState<AccessibleItem[]>([
    {
      id: '1',
      text: 'Review documentation',
      description: 'WCAG 2.1 compliance guidelines',
      completed: false
    },
    {
      id: '2',
      text: 'Implement keyboard support',
      description: 'Arrow keys, Tab, Enter functionality',
      completed: true
    },
    {
      id: '3',
      text: 'Test with screen readers',
      description: 'NVDA, JAWS, VoiceOver compatibility',
      completed: false
    }
  ]);
  const [announcement, setAnnouncement] = useState('');

  const handleSelect = (args: any) => {
    const item = args.data as AccessibleItem;
    setAnnouncement(
      `Selected ${item.text}. ${item.completed ? 'Completed' : 'Incomplete'}`
    );
  };

  const handleToggleComplete = (itemId: string) => {
    setItems(items.map(item =>
      item.id === itemId ? { ...item, completed: !item.completed } : item
    ));

    const item = items.find(i => i.id === itemId);
    setAnnouncement(
      `${item?.text} marked as ${!item?.completed ? 'completed' : 'incomplete'}`
    );
  };

  const itemTemplate = (props: AccessibleItem) => (
    <div
      style={{
        padding: '12px',
        display: 'flex',
        justifyContent: 'space-between',
        alignItems: 'center',
        borderBottom: '1px solid #f0f0f0'
      }}
    >
      <div>
        <div
          style={{
            textDecoration: props.completed ? 'line-through' : 'none',
            fontWeight: '500',
            marginBottom: '4px'
          }}
        >
          {props.text}
        </div>
        <div
          style={{
            fontSize: '13px',
            color: '#666',
            marginBottom: '8px'
          }}
        >
          {props.description}
        </div>
      </div>

      <button
        onClick={() => handleToggleComplete(props.id)}
        aria-label={`Toggle ${props.text} completion status`}
        aria-pressed={props.completed}
        style={{
          padding: '4px 8px',
          minWidth: '44px',
          minHeight: '44px',
          backgroundColor: props.completed ? '#4CAF50' : '#e0e0e0',
          color: props.completed ? 'white' : '#333',
          border: 'none',
          borderRadius: '4px',
          cursor: 'pointer',
          fontSize: '13px',
          fontWeight: '500'
        }}
      >
        {props.completed ? '✓' : 'O'}
      </button>
    </div>
  );

  return (
    <div style={{ maxWidth: '800px' }}>
      {/* Live region for announcements */}
      <div
        role="status"
        aria-live="polite"
        aria-atomic="true"
        style={{
          position: 'absolute',
          left: '-10000px',
          width: '1px',
          height: '1px',
          overflow: 'hidden'
        }}
      >
        {announcement}
      </div>

      <div
        style={{
          marginBottom: '20px',
          padding: '15px',
          backgroundColor: '#f5f5f5',
          borderRadius: '4px'
        }}
      >
        <h2 style={{ marginTop: 0 }}>Accessible Task List</h2>

        <div id="list-instructions" style={{ fontSize: '14px', color: '#666' }}>
          <p>Use arrow keys to navigate, Enter to select, Space to toggle checkbox.</p>
          <p>
            <strong>Keyboard Shortcuts:</strong>
          </p>
          <ul style={{ margin: '5px 0' }}>
            <li>↑ / ↓ - Navigate between items</li>
            <li>Home / End - Jump to first/last item</li>
            <li>Enter - Select item</li>
            <li>Space - Toggle completion</li>
          </ul>
        </div>
      </div>

      <ListViewComponent
        ref={listViewRef}
        dataSource={items}
        template={itemTemplate}
        select={handleSelect}
        height="400px"
        htmlAttributes={{
          role: 'list',
          'aria-label': 'Task list with completion status',
          'aria-describedby': 'list-instructions'
        }}
      />

      <div
        style={{
          marginTop: '15px',
          padding: '10px',
          backgroundColor: '#e3f2fd',
          borderRadius: '4px',
          fontSize: '12px'
        }}
      >
        <strong>Accessibility Features:</strong>
        <ul style={{ margin: '5px 0', paddingLeft: '20px' }}>
          <li>✓ WCAG 2.1 Level AA compliant</li>
          <li>✓ Full keyboard navigation support</li>
          <li>✓ Screen reader compatible</li>
          <li>✓ ARIA labels and live regions</li>
          <li>✓ Focus indicators visible</li>
          <li>✓ Min 44x44px touch targets</li>
          <li>✓ High contrast mode support</li>
        </ul>
      </div>
    </div>
  );
}
```

## Testing for Accessibility

### Automated Testing

```tsx
// Install jest-axe for accessibility testing
// npm install jest-axe

import { axe, toHaveNoViolations } from 'jest-axe';
import { render } from '@testing-library/react';

expect.extend(toHaveNoViolations);

test('ListView should have no accessibility violations', async () => {
  const { container } = render(
    <ListViewComponent
      dataSource={[{ id: '1', text: 'Item' }]}
    />
  );

  const results = await axe(container);
  expect(results).toHaveNoViolations();
});
```

### Manual Testing Checklist

- [ ] Keyboard navigation works (arrow keys, Tab, Enter)
- [ ] Focus indicators visible on all interactive elements
- [ ] Screen reader announces items and their state
- [ ] Color contrast meets 4.5:1 ratio (normal text)
- [ ] All functionality available without mouse
- [ ] Touch targets at least 44x44px
- [ ] Form labels associated with inputs
- [ ] Error messages clearly described
- [ ] Animation doesn't prevent content access
- [ ] Works with browser zoom at 200%

