# Events and Methods Reference

## Table of Contents
- [Event Handlers](#event-handlers) — 9 events: blur, change, cleared, close, created, destroyed, focus, itemRender, open
- [Event Arguments Structure](#event-arguments-structure)
- [Component Methods](#component-methods) — 5 methods: focusIn, focusOut, getPersistData, hide, show
- [Imperative Control with useRef](#imperative-control-with-useref)
- [Lifecycle Events](#lifecycle-events)
- [Event Patterns](#event-patterns)
- [Clearing Values and State](#clearing-values-and-state)
- [ItemRender for Custom Formatting](#itemrender-for-custom-formatting)

## Event Handlers

TimePicker emits various events throughout its lifecycle. Handle them using props:

### Change Event

Triggers when the selected time value changes:

```tsx
import { TimePickerComponent, ChangeEventArgs } from '@syncfusion/ej2-react-calendars';
import * as React from 'react';

function ChangeEventExample() {
  const [selectedTime, setSelectedTime] = React.useState<Date | null>(null);
  const [changeCount, setChangeCount] = React.useState(0);

  const handleChange = (args: ChangeEventArgs) => {
    console.log('Change event triggered');
    console.log('New value:', args.value);
    console.log('Previous value:', args.previousValue);
    console.log('Event type:', args.type);
    
    setSelectedTime(args.value);
    setChangeCount(prev => prev + 1);
  };

  return (
    <div style={{ padding: '20px' }}>
      <h3>Change Event Handler</h3>

      <TimePickerComponent
        value={selectedTime}
        change={handleChange}
        placeholder="Select time"
      />

      <div style={{ marginTop: '20px', padding: '10px', backgroundColor: '#f0f0f0' }}>
        <p>Current Time: {selectedTime?.toLocaleTimeString() || 'None'}</p>
        <p>Change Events Triggered: {changeCount}</p>
      </div>
    </div>
  );
}

export default ChangeEventExample;
```

### Open and Close Events

Triggers when the popup opens/closes:

```tsx
import { TimePickerComponent, PopupEventArgs } from '@syncfusion/ej2-react-calendars';
import * as React from 'react';

function PopupEventExample() {
  const [popupState, setPopupState] = React.useState('closed');
  const [eventLog, setEventLog] = React.useState<string[]>([]);

  const handleOpen = (args: PopupEventArgs) => {
    console.log('Popup opened');
    setPopupState('open');
    setEventLog(prev => [...prev, `[${new Date().toLocaleTimeString()}] Popup opened`]);
  };

  const handleClose = (args: PopupEventArgs) => {
    console.log('Popup closed');
    setPopupState('closed');
    setEventLog(prev => [...prev, `[${new Date().toLocaleTimeString()}] Popup closed`]);
  };

  return (
    <div style={{ padding: '20px' }}>
      <h3>Popup Events</h3>

      <TimePickerComponent
        value={new Date('1/1/2018 10:00 AM')}
        open={handleOpen}
        close={handleClose}
        placeholder="Click to open"
      />

      <div style={{ marginTop: '20px' }}>
        <p>Popup State: <strong>{popupState}</strong></p>
      </div>

      <div style={{ marginTop: '20px', padding: '10px', backgroundColor: '#f5f5f5', maxHeight: '200px', overflow: 'auto' }}>
        <h4>Event Log:</h4>
        <ul style={{ fontSize: '12px' }}>
          {eventLog.map((event, idx) => (
            <li key={idx}>{event}</li>
          ))}
        </ul>
      </div>
    </div>
  );
}

export default PopupEventExample;
```

### Focus and Blur Events

Triggers when the input gains/loses focus:

```tsx
import { TimePickerComponent, FocusEventArgs, BlurEventArgs } from '@syncfusion/ej2-react-calendars';
import * as React from 'react';

function FocusBlurExample() {
  const [isFocused, setIsFocused] = React.useState(false);
  const [focusCount, setFocusCount] = React.useState(0);

  const handleFocus = (args: FocusEventArgs) => {
    setIsFocused(true);
    setFocusCount(prev => prev + 1);
    console.log('TimePicker focused');
  };

  const handleBlur = (args: BlurEventArgs) => {
    setIsFocused(false);
    console.log('TimePicker blurred');
  };

  return (
    <div style={{ padding: '20px' }}>
      <h3>Focus and Blur Events</h3>

      <TimePickerComponent
        value={new Date('1/1/2018 10:00 AM')}
        focus={handleFocus}
        blur={handleBlur}
        placeholder="Click to focus"
      />

      <div style={{ marginTop: '20px', padding: '10px', backgroundColor: '#f0f0f0' }}>
        <p>Focused: {isFocused ? '✓ Yes' : '✗ No'}</p>
        <p>Focus Count: {focusCount}</p>
      </div>
    </div>
  );
}

export default FocusBlurExample;
```

### Cleared Event

Triggers when the value is cleared using the clear button:

```tsx
import { TimePickerComponent, ClearedEventArgs } from '@syncfusion/ej2-react-calendars';
import * as React from 'react';

function ClearedEventExample() {
  const [selectedTime, setSelectedTime] = React.useState<Date | null>(new Date('1/1/2018 10:00 AM'));
  const [clearCount, setClearCount] = React.useState(0);

  const handleCleared = (args: ClearedEventArgs) => {
    console.log('Cleared event triggered');
    setSelectedTime(null);
    setClearCount(prev => prev + 1);
  };

  return (
    <div style={{ padding: '20px' }}>
      <h3>Cleared Event</h3>

      <TimePickerComponent
        value={selectedTime}
        change={(e: any) => setSelectedTime(e.value)}
        cleared={handleCleared}
        showClearButton={true}
        placeholder="Select time (has clear button)"
      />

      <div style={{ marginTop: '20px', padding: '10px', backgroundColor: '#f0f0f0' }}>
        <p>Current Value: {selectedTime?.toLocaleTimeString() || 'Cleared'}</p>
        <p>Times Cleared: {clearCount}</p>
      </div>
    </div>
  );
}

export default ClearedEventExample;
```

## Event Arguments Structure

### ChangeEventArgs

```typescript
interface ChangeEventArgs {
  value: Date;              // New selected time
  previousValue: Date;      // Previous time value
  type: string;             // Event type ('set' or 'change')
  e: Event;                 // Native DOM event
}
```

### PopupEventArgs

```typescript
interface PopupEventArgs {
  popup: Popup;             // Popup instance
  preventDefault: Function; // Prevent default action
}
```

### FocusEventArgs

```typescript
interface FocusEventArgs {
  value: Date;              // Current time value
  e: FocusEvent;           // Native focus event
}
```

## Component Methods

### show() - Open Popup

Programmatically open the time picker popup:

```tsx
import { TimePickerComponent } from '@syncfusion/ej2-react-calendars';
import * as React from 'react';

function ShowMethodExample() {
  const timePickerRef = React.useRef<TimePickerComponent>(null);

  const handleShowPopup = () => {
    if (timePickerRef.current) {
      timePickerRef.current.show();
    }
  };

  return (
    <div style={{ padding: '20px' }}>
      <h3>Show Method Example</h3>

      <TimePickerComponent
        ref={timePickerRef}
        value={new Date('1/1/2018 10:00 AM')}
        placeholder="Select time"
      />

      <button onClick={handleShowPopup} style={{ marginTop: '10px' }}>
        Open Popup Programmatically
      </button>
    </div>
  );
}

export default ShowMethodExample;
```

### hide() - Close Popup

Programmatically close the time picker popup:

```tsx
import { TimePickerComponent } from '@syncfusion/ej2-react-calendars';
import * as React from 'react';

function HideMethodExample() {
  const timePickerRef = React.useRef<TimePickerComponent>(null);
  const [isPopupOpen, setIsPopupOpen] = React.useState(false);

  const handleHidePopup = () => {
    if (timePickerRef.current) {
      timePickerRef.current.hide();
      setIsPopupOpen(false);
    }
  };

  return (
    <div style={{ padding: '20px' }}>
      <h3>Hide Method Example</h3>

      <TimePickerComponent
        ref={timePickerRef}
        value={new Date('1/1/2018 10:00 AM')}
        open={() => setIsPopupOpen(true)}
        close={() => setIsPopupOpen(false)}
        placeholder="Select time"
      />

      <div style={{ marginTop: '10px' }}>
        <button onClick={handleHidePopup} disabled={!isPopupOpen}>
          Close Popup
        </button>
      </div>

      <p style={{ marginTop: '15px' }}>
        Popup is {isPopupOpen ? 'open' : 'closed'}
      </p>
    </div>
  );
}

export default HideMethodExample;
```

### focusIn() - Focus the Input

Set focus to the TimePicker input:

```tsx
import { TimePickerComponent } from '@syncfusion/ej2-react-calendars';
import * as React from 'react';

function FocusInMethodExample() {
  const timePickerRef = React.useRef<TimePickerComponent>(null);

  const handleFocusIn = () => {
    if (timePickerRef.current) {
      timePickerRef.current.focusIn();
    }
  };

  return (
    <div style={{ padding: '20px' }}>
      <h3>FocusIn Method Example</h3>

      <TimePickerComponent
        ref={timePickerRef}
        value={new Date('1/1/2018 10:00 AM')}
        placeholder="Select time"
      />

      <button onClick={handleFocusIn} style={{ marginTop: '10px' }}>
        Focus Input
      </button>

      <p style={{ marginTop: '15px', color: '#666' }}>
        Click button to focus the input element
      </p>
    </div>
  );
}

export default FocusInMethodExample;
```

### focusOut() - Remove Focus

Remove focus from the TimePicker input:

```tsx
import { TimePickerComponent } from '@syncfusion/ej2-react-calendars';
import * as React from 'react';

function FocusOutMethodExample() {
  const timePickerRef = React.useRef<TimePickerComponent>(null);

  const handleFocusOut = () => {
    if (timePickerRef.current) {
      timePickerRef.current.focusOut();
    }
  };

  return (
    <div style={{ padding: '20px' }}>
      <h3>FocusOut Method Example</h3>

      <TimePickerComponent
        ref={timePickerRef}
        value={new Date('1/1/2018 10:00 AM')}
        placeholder="Select time"
      />

      <button onClick={handleFocusOut} style={{ marginTop: '10px' }}>
        Remove Focus
      </button>

      <p style={{ marginTop: '15px', color: '#666' }}>
        Click button to remove focus from the input
      </p>
    </div>
  );
}

export default FocusOutMethodExample;
```

## Imperative Control with useRef

Access TimePicker instance methods and properties using `useRef`:

```tsx
import { TimePickerComponent } from '@syncfusion/ej2-react-calendars';
import * as React from 'react';

function ImperativeControlExample() {
  const timePickerRef = React.useRef<TimePickerComponent>(null);
  const [statusMessage, setStatusMessage] = React.useState('');

  const performAction = (action: string) => {
    const tp = timePickerRef.current;
    if (!tp) return;

    switch (action) {
      case 'open':
        tp.show();
        setStatusMessage('Popup opened');
        break;
      case 'close':
        tp.hide();
        setStatusMessage('Popup closed');
        break;
      case 'focus':
        tp.focusIn();
        setStatusMessage('Input focused');
        break;
      case 'blur':
        tp.focusOut();
        setStatusMessage('Input blurred');
        break;
      case 'getValue':
        setStatusMessage(`Current value: ${tp.value?.toLocaleTimeString()}`);
        break;
    }
  };

  return (
    <div style={{ padding: '20px' }}>
      <h3>Imperative Control Example</h3>

      <TimePickerComponent
        ref={timePickerRef}
        value={new Date('1/1/2018 10:00 AM')}
        placeholder="Select time"
      />

      <div style={{ marginTop: '20px', display: 'grid', gridTemplateColumns: 'repeat(2, 1fr)', gap: '10px' }}>
        <button onClick={() => performAction('open')}>Open Popup</button>
        <button onClick={() => performAction('close')}>Close Popup</button>
        <button onClick={() => performAction('focus')}>Focus Input</button>
        <button onClick={() => performAction('blur')}>Blur Input</button>
        <button onClick={() => performAction('getValue')} style={{ gridColumn: '1 / -1' }}>Get Value</button>
      </div>

      <div style={{ marginTop: '20px', padding: '10px', backgroundColor: '#e8f5e9' }}>
        <p>{statusMessage}</p>
      </div>
    </div>
  );
}

export default ImperativeControlExample;
```

## Lifecycle Events

### Created Event

Triggers when the component is created:

```tsx
import { TimePickerComponent } from '@syncfusion/ej2-react-calendars';
import * as React from 'react';

function CreatedEventExample() {
  const [isCreated, setIsCreated] = React.useState(false);
  const [createdTime, setCreatedTime] = React.useState('');

  const handleCreated = () => {
    setIsCreated(true);
    setCreatedTime(new Date().toLocaleTimeString());
    console.log('TimePicker component created');
  };

  return (
    <div style={{ padding: '20px' }}>
      <h3>Created Event</h3>

      <TimePickerComponent
        value={new Date('1/1/2018 10:00 AM')}
        created={handleCreated}
        placeholder="Select time"
      />

      <div style={{ marginTop: '20px', padding: '10px', backgroundColor: '#f0f0f0' }}>
        <p>Component Created: {isCreated ? '✓ Yes' : '✗ No'}</p>
        {createdTime && <p>Created at: {createdTime}</p>}
      </div>
    </div>
  );
}

export default CreatedEventExample;
```

### Destroyed Event

Triggers when the component is destroyed:

```tsx
import { TimePickerComponent } from '@syncfusion/ej2-react-calendars';
import * as React from 'react';

function DestroyedEventExample() {
  const [isDestroyed, setIsDestroyed] = React.useState(false);
  const [showComponent, setShowComponent] = React.useState(true);

  const handleDestroyed = () => {
    setIsDestroyed(true);
    console.log('TimePicker component destroyed');
  };

  return (
    <div style={{ padding: '20px' }}>
      <h3>Destroyed Event</h3>

      <label>
        <input
          type="checkbox"
          checked={showComponent}
          onChange={(e) => {
            setShowComponent(e.target.checked);
            setIsDestroyed(false);
          }}
        />
        {' '}Show Component
      </label>

      {showComponent && (
        <div style={{ marginTop: '15px' }}>
          <TimePickerComponent
            value={new Date('1/1/2018 10:00 AM')}
            destroyed={handleDestroyed}
            placeholder="Select time"
          />
        </div>
      )}

      <div style={{ marginTop: '20px', padding: '10px', backgroundColor: '#f0f0f0' }}>
        <p>Component Destroyed: {isDestroyed ? '✓ Yes' : '✗ No'}</p>
      </div>
    </div>
  );
}

export default DestroyedEventExample;
```

## Event Patterns

### Pattern 1: Validation on Change

```tsx
import { TimePickerComponent } from '@syncfusion/ej2-react-calendars';
import * as React from 'react';

function ValidationPatternExample() {
  const [selectedTime, setSelectedTime] = React.useState<Date | null>(null);
  const [validationMessage, setValidationMessage] = React.useState('');

  const handleChange = (e: any) => {
    const time = e.value;
    const hour = time?.getHours();

    if (hour !== undefined) {
      if (hour < 9 || hour >= 17) {
        setValidationMessage('⚠️ Business hours are 9 AM - 5 PM');
      } else {
        setValidationMessage('✓ Time is within business hours');
      }
    }

    setSelectedTime(time);
  };

  return (
    <div style={{ padding: '20px' }}>
      <h3>Validation Pattern</h3>

      <TimePickerComponent
        value={selectedTime}
        change={handleChange}
        placeholder="Select time"
      />

      <div style={{ marginTop: '15px', padding: '10px', backgroundColor: '#f5f5f5' }}>
        <p>{validationMessage}</p>
      </div>
    </div>
  );
}

export default ValidationPatternExample;
```

### Pattern 2: Auto-Save on Change

```tsx
import { TimePickerComponent } from '@syncfusion/ej2-react-calendars';
import * as React from 'react';

function AutoSavePatternExample() {
  const [selectedTime, setSelectedTime] = React.useState<Date | null>(new Date('1/1/2018 10:00 AM'));
  const [saveStatus, setSaveStatus] = React.useState('Saved');

  const handleChange = (e: any) => {
    setSelectedTime(e.value);
    setSaveStatus('Saving...');

    // Simulate API call
    setTimeout(() => {
      setSaveStatus('Saved ✓');
    }, 500);
  };

  return (
    <div style={{ padding: '20px' }}>
      <h3>Auto-Save Pattern</h3>

      <TimePickerComponent
        value={selectedTime}
        change={handleChange}
        placeholder="Select time"
      />

      <div style={{ marginTop: '15px', padding: '10px', backgroundColor: '#e8f5e9' }}>
        <p>{saveStatus}</p>
      </div>
    </div>
  );
}

export default AutoSavePatternExample;
```

## Clearing Values and State

### Using Clear Button

```tsx
import { TimePickerComponent } from '@syncfusion/ej2-react-calendars';
import * as React from 'react';

function ClearButtonExample() {
  const [selectedTime, setSelectedTime] = React.useState<Date | null>(new Date('1/1/2018 10:00 AM'));

  return (
    <div style={{ padding: '20px' }}>
      <h3>Clear Button</h3>

      <TimePickerComponent
        value={selectedTime}
        change={(e: any) => setSelectedTime(e.value)}
        showClearButton={true}
        placeholder="Select time"
      />

      <div style={{ marginTop: '15px' }}>
        <p>Current: {selectedTime?.toLocaleTimeString() || 'Not selected'}</p>
      </div>
    </div>
  );
}

export default ClearButtonExample;
```

### Programmatic Clear

```tsx
import { TimePickerComponent } from '@syncfusion/ej2-react-calendars';
import * as React from 'react';

function ProgrammaticClearExample() {
  const timePickerRef = React.useRef<TimePickerComponent>(null);
  const [selectedTime, setSelectedTime] = React.useState<Date | null>(new Date('1/1/2018 10:00 AM'));

  const handleClear = () => {
    setSelectedTime(null);
    if (timePickerRef.current) {
      timePickerRef.current.value = null;
    }
  };

  return (
    <div style={{ padding: '20px' }}>
      <h3>Programmatic Clear</h3>

      <TimePickerComponent
        ref={timePickerRef}
        value={selectedTime}
        change={(e: any) => setSelectedTime(e.value)}
        placeholder="Select time"
      />

      <button onClick={handleClear} style={{ marginTop: '10px' }}>
        Clear Value
      </button>

      <div style={{ marginTop: '15px' }}>
        <p>Current: {selectedTime?.toLocaleTimeString() || 'Not selected'}</p>
      </div>
    </div>
  );
}

export default ProgrammaticClearExample;
```

## ItemRender for Custom Formatting

The `itemRender` event allows custom formatting of time items in the popup:

```tsx
import { TimePickerComponent, ItemEventArgs } from '@syncfusion/ej2-react-calendars';
import * as React from 'react';

function ItemRenderExample() {
  const handleItemRender = (args: ItemEventArgs) => {
    const time = args.value;
    const hour = time?.getHours();

    // Highlight business hours
    if (hour !== undefined && hour >= 9 && hour < 17) {
      args.element.style.backgroundColor = '#e8f5e9';
      args.element.title = 'Business hours';
    }
  };

  return (
    <div style={{ padding: '20px' }}>
      <h3>ItemRender Event</h3>

      <TimePickerComponent
        value={new Date('1/1/2018 10:00 AM')}
        min={new Date('1/1/2018 8:00 AM')}
        max={new Date('1/1/2018 6:00 PM')}
        step={60}
        itemRender={handleItemRender}
        placeholder="Select time"
      />

      <p style={{ marginTop: '15px', color: '#666' }}>
        Green items = Business hours (9 AM - 5 PM)
      </p>
    </div>
  );
}

export default ItemRenderExample;
```

## Related Topics

- [Time Format and Display](time-format-and-display.md) - Format-related events
- [Customization and Styling](customization-and-styling.md) - Styling event handling
- [API Reference](api-reference.md) - Complete event documentation
