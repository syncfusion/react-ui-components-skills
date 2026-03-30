# Events and Methods

## Table of Contents
- [Overview](#overview)
- [Event Handlers](#event-handlers)
- [Component Methods](#component-methods)
- [Event Argument Structures](#event-argument-structures)
- [Imperative Access with useRef](#imperative-access-with-useref)
- [Lifecycle Events](#lifecycle-events)
- [Best Practices](#best-practices)
- [Common Patterns](#common-patterns)

## Overview

DateRangePicker provides multiple events for responding to user interactions and methods for programmatic control. Key events include:
- `change` - Date range selection changed
- `select` - Date range being selected
- `open` - Popup opened
- `close` - Popup closed
- `blur` - Component lost focus
- `focus` - Component gained focus
- `created` - Component initialized
- `destroyed` - Component destroyed

## Event Handlers

### Change Event

Fired when the user completes date range selection:

```tsx
import { DateRangePickerComponent, ChangedEventArgs } from '@syncfusion/ej2-react-calendars';
import * as React from 'react';

function ChangeEventExample() {
  const handleChange = (args: ChangedEventArgs) => {
    console.log('Change Event:', {
      startDate: args.startDate,
      endDate: args.endDate,
      value: args.value,
      isInteracted: args.isInteracted,
      type: args.type,
    });
  };

  return (
    <DateRangePickerComponent
      id="daterangepicker"
      placeholder="Select date range"
      change={handleChange}
    />
  );
}

export default ChangeEventExample;
```

### Select Event

Fired while range is being selected (fires multiple times):

```tsx
import { DateRangePickerComponent, DateRangeSelectingEventArgs } from '@syncfusion/ej2-react-calendars';
import * as React from 'react';

function SelectEventExample() {
  const [selectionState, setSelectionState] = React.useState<string>('');

  const handleSelect = (args: DateRangeSelectingEventArgs) => {
    console.log('Select Event:', {
      startDate: args.startDate,
      endDate: args.endDate,
    });

    // Validate selection during selection process
    if (args.endDate) {
      const daysDiff = Math.ceil(
        (args.endDate.getTime() - args.startDate!.getTime()) / (1000 * 60 * 60 * 24)
      );
      setSelectionState(`${daysDiff} days selected`);
    }
  };

  return (
    <div style={{ padding: '20px' }}>
      <DateRangePickerComponent
        id="daterangepicker"
        placeholder="Select date range"
        select={handleSelect}
      />
      <p style={{ marginTop: '10px' }}>{selectionState}</p>
    </div>
  );
}

export default SelectEventExample;
```

### Open Event

Fired when date range popup opens:

```tsx
import { DateRangePickerComponent } from '@syncfusion/ej2-react-calendars';
import * as React from 'react';

function OpenEventExample() {
  const [openCount, setOpenCount] = React.useState<number>(0);

  const handleOpen = (e: any) => {
    setOpenCount((prev) => prev + 1);
    console.log('Popup opened. Times opened:', openCount + 1);
  };

  return (
    <div style={{ padding: '20px' }}>
      <DateRangePickerComponent
        id="daterangepicker"
        placeholder="Select date range"
        open={handleOpen}
      />
      <p>Popup opened {openCount} times</p>
    </div>
  );
}

export default OpenEventExample;
```

### Close Event

Fired when date range popup closes:

```tsx
import { DateRangePickerComponent, ChangedEventArgs } from '@syncfusion/ej2-react-calendars';
import * as React from 'react';

function CloseEventExample() {
  const [isOpen, setIsOpen] = React.useState<boolean>(false);
  const [lastSelection, setLastSelection] = React.useState<string>('');

  const handleOpen = () => {
    setIsOpen(true);
  };

  const handleClose = (e: any) => {
    setIsOpen(false);
  };

  const handleChange = (args: ChangedEventArgs) => {
    if (args.startDate && args.endDate) {
      setLastSelection(
        `${args.startDate.toLocaleDateString()} - ${args.endDate.toLocaleDateString()}`
      );
    }
  };

  return (
    <div style={{ padding: '20px' }}>
      <DateRangePickerComponent
        id="daterangepicker"
        placeholder="Select date range"
        open={handleOpen}
        close={handleClose}
        change={handleChange}
      />
      <p>Status: {isOpen ? '🔓 Open' : '🔒 Closed'}</p>
      {lastSelection && <p>Last selection: {lastSelection}</p>}
    </div>
  );
}

export default CloseEventExample;
```

### Focus and Blur Events

```tsx
import { DateRangePickerComponent } from '@syncfusion/ej2-react-calendars';
import * as React from 'react';

function FocusBlurExample() {
  const [focusState, setFocusState] = React.useState<string>('Not focused');

  const handleFocus = () => {
    setFocusState('🎯 Focused');
  };

  const handleBlur = () => {
    setFocusState('👋 Blurred');
  };

  return (
    <div style={{ padding: '20px' }}>
      <DateRangePickerComponent
        id="daterangepicker"
        placeholder="Select date range"
        focus={handleFocus}
        blur={handleBlur}
      />
      <p style={{ marginTop: '10px' }}>{focusState}</p>
    </div>
  );
}

export default FocusBlurExample;
```

## Component Methods

### show() - Open Popup Programmatically

```tsx
import { DateRangePickerComponent as DRP } from '@syncfusion/ej2-react-calendars';
import * as React from 'react';

function ShowMethodExample() {
  const dateRangeRef = React.useRef<DRP>(null);

  const handleShow = () => {
    if (dateRangeRef.current) {
      dateRangeRef.current.show();
    }
  };

  return (
    <div style={{ padding: '20px' }}>
      <button onClick={handleShow} style={{ padding: '8px 12px', marginBottom: '15px' }}>
        📅 Open Calendar
      </button>

      <DateRangePickerComponent
        ref={dateRangeRef}
        id="daterangepicker"
        placeholder="Select date range"
      />
    </div>
  );
}

export default ShowMethodExample;
```

### hide() - Close Popup Programmatically

```tsx
import { DateRangePickerComponent as DRP } from '@syncfusion/ej2-react-calendars';
import * as React from 'react';

function HideMethodExample() {
  const dateRangeRef = React.useRef<DRP>(null);
  const [isManuallyClosed, setIsManuallyClosed] = React.useState<boolean>(false);

  const handleHide = () => {
    if (dateRangeRef.current) {
      dateRangeRef.current.hide();
      setIsManuallyClosed(true);
      setTimeout(() => setIsManuallyClosed(false), 2000);
    }
  };

  return (
    <div style={{ padding: '20px' }}>
      <button onClick={handleHide} style={{ padding: '8px 12px', marginBottom: '15px' }}>
        ✕ Close Calendar
      </button>

      <DateRangePickerComponent
        ref={dateRangeRef}
        id="daterangepicker"
        placeholder="Select date range"
      />

      {isManuallyClosed && <p style={{ color: 'green' }}>✓ Calendar closed</p>}
    </div>
  );
}

export default HideMethodExample;
```

### focusIn() - Set Focus

```tsx
import { DateRangePickerComponent as DRP } from '@syncfusion/ej2-react-calendars';
import * as React from 'react';

function FocusInMethodExample() {
  const dateRangeRef = React.useRef<DRP>(null);

  const handleFocusIn = () => {
    if (dateRangeRef.current) {
      dateRangeRef.current.focusIn();
    }
  };

  return (
    <div style={{ padding: '20px' }}>
      <button onClick={handleFocusIn} style={{ padding: '8px 12px', marginBottom: '15px' }}>
        ✏️ Focus Input
      </button>

      <DateRangePickerComponent
        ref={dateRangeRef}
        id="daterangepicker"
        placeholder="Select date range"
      />
    </div>
  );
}

export default FocusInMethodExample;
```

### Reset Value

```tsx
import { DateRangePickerComponent as DRP } from '@syncfusion/ej2-react-calendars';
import * as React from 'react';

function ResetValueExample() {
  const dateRangeRef = React.useRef<DRP>(null);

  const handleReset = () => {
    if (dateRangeRef.current) {
      dateRangeRef.current.value = null;
    }
  };

  return (
    <div style={{ padding: '20px' }}>
      <button onClick={handleReset} style={{ padding: '8px 12px', marginBottom: '15px' }}>
        🔄 Clear Selection
      </button>

      <DateRangePickerComponent
        ref={dateRangeRef}
        id="daterangepicker"
        placeholder="Select date range"
        startDate={new Date(2024, 9, 1)}
        endDate={new Date(2024, 10, 15)}
      />
    </div>
  );
}

export default ResetValueExample;
```

### Get Current Values

```tsx
import { DateRangePickerComponent as DRP } from '@syncfusion/ej2-react-calendars';
import * as React from 'react';

function GetValuesExample() {
  const dateRangeRef = React.useRef<DRP>(null);
  const [values, setValues] = React.useState<{
    start: Date | null;
    end: Date | null;
  }>({ start: null, end: null });

  const handleGetValues = () => {
    if (dateRangeRef.current) {
      setValues({
        start: dateRangeRef.current.startDate,
        end: dateRangeRef.current.endDate,
      });
    }
  };

  return (
    <div style={{ padding: '20px' }}>
      <DateRangePickerComponent
        ref={dateRangeRef}
        id="daterangepicker"
        placeholder="Select date range"
      />

      <button onClick={handleGetValues} style={{ padding: '8px 12px', marginTop: '15px' }}>
        📋 Get Values
      </button>

      {(values.start || values.end) && (
        <div style={{
          marginTop: '15px',
          padding: '10px',
          backgroundColor: '#f5f5f5',
          borderRadius: '4px',
        }}>
          <p>
            <strong>Start Date:</strong> {values.start?.toLocaleDateString() || 'None'}
          </p>
          <p>
            <strong>End Date:</strong> {values.end?.toLocaleDateString() || 'None'}
          </p>
        </div>
      )}
    </div>
  );
}

export default GetValuesExample;
```

## Event Argument Structures

### ChangedEventArgs

```typescript
interface ChangedEventArgs {
  startDate: Date;
  endDate: Date;
  value: [Date, Date];
  isInteracted: boolean;
  type: 'touch' | 'mouse' | 'keyboard';
  event?: Event;
  element?: HTMLElement;
  preventDefault(): void;
}
```

### DateRangeSelectingEventArgs

```typescript
interface DateRangeSelectingEventArgs {
  startDate: Date;
  endDate?: Date;
  isInteracted: boolean;
  event?: Event;
  preventDefault(): void;
}
```

## Imperative Access with useRef

### Complete Imperative Control Example

```tsx
import { DateRangePickerComponent as DRP, ChangedEventArgs } from '@syncfusion/ej2-react-calendars';
import * as React from 'react';

function ImperativeControlExample() {
  const dateRangeRef = React.useRef<DRP>(null);
  const [values, setValues] = React.useState<{
    start: Date | null;
    end: Date | null;
  }>({ start: null, end: null });

  const handleChange = (args: ChangedEventArgs) => {
    setValues({ start: args.startDate, end: args.endDate });
  };

  const actions = [
    {
      label: '📅 Open',
      fn: () => dateRangeRef.current?.show(),
    },
    {
      label: '✕ Close',
      fn: () => dateRangeRef.current?.hide(),
    },
    {
      label: '↔️ Swap',
      fn: () => {
        if (dateRangeRef.current && values.start && values.end) {
          dateRangeRef.current.value = [values.end, values.start];
        }
      },
    },
    {
      label: '🔄 Reset',
      fn: () => {
        if (dateRangeRef.current) {
          dateRangeRef.current.value = null;
          setValues({ start: null, end: null });
        }
      },
    },
    {
      label: '✏️ Focus',
      fn: () => dateRangeRef.current?.focusIn(),
    },
  ];

  return (
    <div style={{ padding: '20px', maxWidth: '600px' }}>
      <h3>Imperative Control</h3>

      <DateRangePickerComponent
        ref={dateRangeRef}
        id="daterangepicker"
        placeholder="Select date range"
        change={handleChange}
      />

      <div style={{
        display: 'grid',
        gridTemplateColumns: 'repeat(auto-fit, minmax(100px, 1fr))',
        gap: '10px',
        marginTop: '20px',
      }}>
        {actions.map((action) => (
          <button
            key={action.label}
            onClick={action.fn}
            style={{
              padding: '10px',
              backgroundColor: '#1976d2',
              color: 'white',
              border: 'none',
              borderRadius: '4px',
              cursor: 'pointer',
            }}
          >
            {action.label}
          </button>
        ))}
      </div>

      {(values.start || values.end) && (
        <div style={{
          marginTop: '20px',
          padding: '15px',
          backgroundColor: '#e3f2fd',
          borderRadius: '4px',
        }}>
          <p>
            <strong>Start:</strong> {values.start?.toLocaleDateString()}
          </p>
          <p>
            <strong>End:</strong> {values.end?.toLocaleDateString()}
          </p>
        </div>
      )}
    </div>
  );
}

export default ImperativeControlExample;
```

## Lifecycle Events

### Created Event

```tsx
import { DateRangePickerComponent } from '@syncfusion/ej2-react-calendars';
import * as React from 'react';

function CreatedEventExample() {
  const [isCreated, setIsCreated] = React.useState<boolean>(false);

  const handleCreated = () => {
    setIsCreated(true);
    console.log('DateRangePicker component created');
  };

  return (
    <div style={{ padding: '20px' }}>
      <DateRangePickerComponent
        id="daterangepicker"
        placeholder="Select date range"
        created={handleCreated}
      />
      {isCreated && <p style={{ color: 'green' }}>✓ Component initialized</p>}
    </div>
  );
}

export default CreatedEventExample;
```

### Destroyed Event

```tsx
import { DateRangePickerComponent } from '@syncfusion/ej2-react-calendars';
import * as React from 'react';

function DestroyedEventExample() {
  const [isVisible, setIsVisible] = React.useState<boolean>(true);
  const [destroyCount, setDestroyCount] = React.useState<number>(0);

  const handleDestroyed = () => {
    setDestroyCount((prev) => prev + 1);
    console.log('DateRangePicker component destroyed');
  };

  return (
    <div style={{ padding: '20px' }}>
      {isVisible && (
        <DateRangePickerComponent
          id="daterangepicker"
          placeholder="Select date range"
          destroyed={handleDestroyed}
        />
      )}

      <div style={{ marginTop: '20px' }}>
        <button
          onClick={() => setIsVisible(!isVisible)}
          style={{ padding: '8px 12px', marginRight: '10px' }}
        >
          {isVisible ? '🗑️ Destroy' : '✨ Create'}
        </button>
        <span>Component destroyed {destroyCount} times</span>
      </div>
    </div>
  );
}

export default DestroyedEventExample;
```

## Best Practices

### Practice 1: Event Validation

```tsx
import { DateRangePickerComponent, ChangedEventArgs } from '@syncfusion/ej2-react-calendars';
import * as React from 'react';

function EventValidationExample() {
  const [validationLog, setValidationLog] = React.useState<string[]>([]);

  const handleSelect = (args: any) => {
    setValidationLog((prev) => [
      ...prev,
      `Select: Range starting from ${args.startDate?.toLocaleDateString()}`,
    ]);
  };

  const handleChange = (args: ChangedEventArgs) => {
    if (args.startDate && args.endDate) {
      const daysDiff = Math.ceil(
        (args.endDate.getTime() - args.startDate.getTime()) / (1000 * 60 * 60 * 24)
      );

      let status = 'Valid';
      if (daysDiff > 60) status = 'Too long (>60 days)';
      else if (daysDiff < 1) status = 'Invalid (same day)';

      setValidationLog((prev) => [
        ...prev,
        `Change: ${daysDiff} days selected - ${status}`,
      ]);
    }
  };

  return (
    <div style={{ padding: '20px', display: 'grid', gridTemplateColumns: '1fr 1fr', gap: '20px' }}>
      <div>
        <DateRangePickerComponent
          id="daterangepicker"
          placeholder="Select date range"
          select={handleSelect}
          change={handleChange}
        />
      </div>

      <div style={{ padding: '10px', border: '1px solid #ccc', borderRadius: '4px' }}>
        <h4>Event Log:</h4>
        <ul style={{ maxHeight: '300px', overflowY: 'auto', margin: 0, paddingLeft: '20px' }}>
          {validationLog.map((log, idx) => (
            <li key={idx} style={{ marginBottom: '5px', fontSize: '12px' }}>
              {log}
            </li>
          ))}
        </ul>
      </div>
    </div>
  );
}

export default EventValidationExample;
```

## Common Patterns

### Pattern: Debounced Event Handler

```tsx
import { DateRangePickerComponent, ChangedEventArgs } from '@syncfusion/ej2-react-calendars';
import * as React from 'react';

function DebouncedEventExample() {
  const [result, setResult] = React.useState<string>('');
  const timeoutRef = React.useRef<NodeJS.Timeout>();

  const handleChange = (args: ChangedEventArgs) => {
    // Clear existing timeout
    if (timeoutRef.current) clearTimeout(timeoutRef.current);

    // Set new timeout
    timeoutRef.current = setTimeout(() => {
      setResult(
        `Range selected: ${args.startDate?.toLocaleDateString()} - ${args.endDate?.toLocaleDateString()}`
      );
    }, 500);
  };

  return (
    <div style={{ padding: '20px', maxWidth: '500px' }}>
      <DateRangePickerComponent
        id="daterangepicker"
        placeholder="Select date range"
        change={handleChange}
      />
      {result && <p style={{ marginTop: '10px', color: 'green' }}>✓ {result}</p>}
    </div>
  );
}

export default DebouncedEventExample;
```
