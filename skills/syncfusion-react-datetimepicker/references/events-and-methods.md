# Events and Methods

## Table of Contents
- [Event Types Overview](#event-types-overview)
- [Change Event](#change-event)
- [Focus and Blur Events](#focus-and-blur-events)
- [Open and Close Events](#open-and-close-events)
- [Navigation and Render Events](#navigation-and-render-events)
- [Lifecycle Events](#lifecycle-events)
- [Methods Reference](#methods-reference)
- [Advanced Examples](#advanced-examples)

## Event Types Overview

The DateTimePicker emits events at various points in its lifecycle:

| Event | When it fires | Use case |
|-------|---------------|----------|
| `change` | When date/time value changes | Update form state, validate input |
| `blur` | When input loses focus | Blur validation, cleanup |
| `focus` | When input gains focus | Show hints, analytics |
| `open` | When popup opens | Track user engagement |
| `close` | When popup closes | Save draft, analytics |
| `created` | Component fully initialized | Setup, initialize data |
| `destroyed` | Component cleanup | Cleanup event listeners |
| `navigated` | Calendar view changes (Month→Year) | Track calendar navigation |
| `renderDayCell` | Each day cell renders | Disable dates, customize appearance |
| `cleared` | Clear button clicked | Reset related fields |

## Change Event

Fires when the user selects a date and time value.

### Basic Change Handler

```tsx
import React, { useState } from 'react';
import { DateTimePickerComponent, ChangedEventArgs } from '@syncfusion/ej2-react-calendars';

export default function ChangeEventExample() {
  const [selectedDateTime, setSelectedDateTime] = useState<Date | null>(null);

  const handleChange = (args: ChangedEventArgs) => {
    console.log('New value:', args.value);
    console.log('Previous value:', args.previousValue);
    setSelectedDateTime(args.value as Date);
  };

  return (
    <div>
      <DateTimePickerComponent
        change={handleChange}
        placeholder="Select date and time"
      />
      {selectedDateTime && (
        <p>Selected: {selectedDateTime.toLocaleString()}</p>
      )}
    </div>
  );
}
```

### ChangedEventArgs Properties

```tsx
interface ChangedEventArgs {
  value: Date;              // The new selected date/time
  previousValue: Date;      // The previously selected value
  event: Event;             // DOM event
  element: HTMLInputElement;  // The input element
  isInteracted: boolean;    // Whether user-triggered or programmatic
}
```

### Validation in Change Event

```tsx
const handleChange = (args: ChangedEventArgs) => {
  const selected = args.value as Date;

  // Example: Validate minimum 24 hours from now
  const now = new Date();
  const tomorrow = new Date(now.getTime() + 24 * 60 * 60 * 1000);

  if (selected < tomorrow) {
    console.warn('Please select a date at least 24 hours from now');
    // You may reset the value or show an error
  }
};
```

### Form State Update in Change

```tsx
const [formState, setFormState] = useState({
  eventName: '',
  eventDateTime: null as Date | null,
  isValid: false,
});

const handleChange = (args: ChangedEventArgs) => {
  setFormState({
    ...formState,
    eventDateTime: args.value as Date,
    isValid: Boolean(formState.eventName && args.value),
  });
};
```

## Focus and Blur Events

Handle when the input receives and loses focus.

### Basic Focus/Blur Handlers

```tsx
import React from 'react';
import { DateTimePickerComponent } from '@syncfusion/ej2-react-calendars';

export default function FocusBlurExample() {
  const handleFocus = () => {
    console.log('DateTimePicker focused');
  };

  const handleBlur = () => {
    console.log('DateTimePicker blurred');
  };

  return (
    <DateTimePickerComponent
      focus={handleFocus}
      blur={handleBlur}
      placeholder="Click here to focus"
    />
  );
}
```

### Auto-fill or Reset on Blur

```tsx
import React, { useState } from 'react';
import { DateTimePickerComponent } from '@syncfusion/ej2-react-calendars';

export default function BlurAutoFill() {
  const [value, setValue] = useState<Date | null>(null);

  const handleBlur = () => {
    // Auto-fill to current time if not selected
    if (!value) {
      setValue(new Date());
    }
  };

  return (
    <DateTimePickerComponent
      value={value}
      change={(e) => setValue((e as any).value)}
      blur={handleBlur}
      placeholder="Auto-fill on blur if empty"
    />
  );
}
```

### Focus for Analytics/Tracking

```tsx
const handleFocus = () => {
  // Track which field was interacted with
  analytics.track('datetimepicker_focused', {
    timestamp: new Date(),
    userId: currentUser.id,
  });
};

<DateTimePickerComponent focus={handleFocus} />
```

## Open and Close Events

Track when the calendar/time picker popup appears and disappears.

### Basic Open/Close Handlers

```tsx
import React, { useState } from 'react';
import { DateTimePickerComponent } from '@syncfusion/ej2-react-calendars';

export default function OpenCloseExample() {
  const [isOpen, setIsOpen] = useState(false);

  const handleOpen = () => {
    console.log('Popup opened');
    setIsOpen(true);
  };

  const handleClose = () => {
    console.log('Popup closed');
    setIsOpen(false);
  };

  return (
    <div>
      <DateTimePickerComponent
        open={handleOpen}
        close={handleClose}
        placeholder="Select date and time"
      />
      <p>Popup status: {isOpen ? 'Open' : 'Closed'}</p>
    </div>
  );
}
```

### Prefetch Data on Open

```tsx
const handleOpen = () => {
  // Fetch available slots when user opens picker
  fetchAvailableSlots().then(slots => {
    // Update UI with available options
    console.log('Available slots:', slots);
  });
};

<DateTimePickerComponent open={handleOpen} />
```

### Save Draft on Close

```tsx
const handleClose = () => {
  // Auto-save form draft
  saveDraft({
    eventDateTime: value,
    savedAt: new Date(),
  });
};

<DateTimePickerComponent close={handleClose} />
```

## Navigation and Render Events

Handle calendar view changes and day cell rendering.

### Navigated Event

Fires when user navigates between calendar views (Month → Year → Decade).

```tsx
import React, { useState } from 'react';
import { DateTimePickerComponent, NavigatedEventArgs } from '@syncfusion/ej2-react-calendars';

export default function NavigatedExample() {
  const [currentView, setCurrentView] = useState('Month');

  const handleNavigated = (args: NavigatedEventArgs) => {
    console.log('Navigated to view:', args.view);
    console.log('Focused date:', args.date);
    setCurrentView(args.view as string);
  };

  return (
    <div>
      <DateTimePickerComponent
        navigated={handleNavigated}
        placeholder="Navigate calendar"
      />
      <p>Current view: {currentView}</p>
    </div>
  );
}
```

### RenderDayCell Event

Customize the appearance of each day cell in the calendar.

```tsx
import React from 'react';
import { DateTimePickerComponent, RenderDayCellEventArgs } from '@syncfusion/ej2-react-calendars';

export default function RenderDayCellExample() {
  const handleRenderDayCell = (args: RenderDayCellEventArgs) => {
    // Disable weekends (Saturday = 6, Sunday = 0)
    const dayOfWeek = args.date?.getDay();
    if (dayOfWeek === 0 || dayOfWeek === 6) {
      (args as any).isDisabled = true;
      args.element?.classList.add('e-disabled');
    }

    // Highlight holidays
    const holidays = [
      new Date(2025, 11, 25), // Christmas
      new Date(2025, 0, 1),   // New Year
    ];

    if (holidays.some(h => h.toDateString() === args.date?.toDateString())) {
      args.element?.classList.add('holiday');
    }
  };

  return (
    <DateTimePickerComponent
      renderDayCell={handleRenderDayCell}
      placeholder="Weekends disabled, holidays highlighted"
    />
  );
}
```

### Custom Styling with renderDayCell

```css
/* In your CSS file */
.e-datetimepicker .e-disabled {
  background-color: #f0f0f0;
  opacity: 0.5;
}

.e-datetimepicker .holiday {
  background-color: #ffe0e0;
  font-weight: bold;
}
```

## Lifecycle Events

Handle component creation and destruction.

### Created Event

Fires after component is fully initialized.

```tsx
import React, { useRef } from 'react';
import { DateTimePickerComponent } from '@syncfusion/ej2-react-calendars';

export default function CreatedExample() {
  const dateTimeRef = useRef<DateTimePickerComponent>(null);

  const handleCreated = () => {
    console.log('DateTimePicker created');
    // Initialize related data or set focus
    if (dateTimeRef.current) {
      (dateTimeRef.current as any).focusIn();
    }
  };

  return (
    <DateTimePickerComponent
      ref={dateTimeRef}
      created={handleCreated}
      placeholder="Auto-focus on create"
    />
  );
}
```

### Destroyed Event

Fires when component is destroyed.

```tsx
const handleDestroyed = () => {
  console.log('DateTimePicker destroyed');
  // Cleanup: remove listeners, clear timers, etc.
};

<DateTimePickerComponent destroyed={handleDestroyed} />
```

### Cleared Event

Fires when the clear button is clicked. The `ClearedEventArgs` object does not carry the previous value — track it in state if needed before clearing.

```tsx
import React, { useState } from 'react';
import { DateTimePickerComponent, ClearedEventArgs } from '@syncfusion/ej2-react-calendars';

export default function ClearedExample() {
  const [value, setValue] = useState<Date | null>(new Date());

  const handleCleared = (args: ClearedEventArgs) => {
    // Note: ClearedEventArgs does not contain the previous value.
    // If you need the previous value, track it in state before clearing.
    console.log('Value cleared by user');
    setValue(null);
  };

  return (
    <DateTimePickerComponent
      value={value}
      change={(e) => setValue((e as any).value)}
      cleared={handleCleared}
      showClearButton={true}
    />
  );
}
```

## Methods Reference

> **Methods available on the DateTimePicker component instance (via ref):**
> `currentView()`, `destroy()`, `focusIn()`, `focusOut()`, `getPersistData()`, `navigateTo(view, date)`, `onPropertyChanged(newProp, oldProp)`, `removeDate(dates)`

### currentView()

Returns the currently active calendar view (Month, Year, or Decade).

```tsx
import React, { useRef } from 'react';
import { DateTimePickerComponent } from '@syncfusion/ej2-react-calendars';

export default function CurrentViewExample() {
  const dateTimeRef = useRef<DateTimePickerComponent>(null);

  const handleCheck = () => {
    const view = (dateTimeRef.current as any).currentView();
    console.log('Current view:', view); // "Month", "Year", or "Decade"
  };

  return (
    <div>
      <DateTimePickerComponent ref={dateTimeRef} />
      <button onClick={handleCheck}>Check View</button>
    </div>
  );
}
```

### getPersistData()

Returns a serialized string of the properties to be maintained upon browser refresh (i.e., the persistable state).

```tsx
import React, { useRef } from 'react';
import { DateTimePickerComponent } from '@syncfusion/ej2-react-calendars';

export default function GetPersistDataExample() {
  const dateTimeRef = useRef<DateTimePickerComponent>(null);

  const handleGetPersistData = () => {
    if (dateTimeRef.current) {
      const persistData = (dateTimeRef.current as any).getPersistData();
      console.log('Persist data:', persistData); // JSON string with value
    }
  };

  return (
    <div>
      <DateTimePickerComponent ref={dateTimeRef} value={new Date()} />
      <button onClick={handleGetPersistData}>Get Persist Data</button>
    </div>
  );
}
```

### removeDate(dates)

Removes a single date or multiple dates from the Calendar's `values` property.

```tsx
import React, { useRef } from 'react';
import { DateTimePickerComponent } from '@syncfusion/ej2-react-calendars';

export default function RemoveDateExample() {
  const dateTimeRef = useRef<DateTimePickerComponent>(null);

  const handleRemove = () => {
    if (dateTimeRef.current) {
      // Remove a specific date from the calendar values
      (dateTimeRef.current as any).removeDate(new Date(2025, 5, 15));
    }
  };

  return (
    <div>
      <DateTimePickerComponent ref={dateTimeRef} />
      <button onClick={handleRemove}>Remove June 15</button>
    </div>
  );
}
```

### onPropertyChanged(newProp, oldProp)

Called internally when any property value changes. `newProp` contains updated properties and `oldProp` contains the previous values. This is an internal lifecycle method — do not call it directly.

```tsx
// Internal usage: called automatically by the framework
// newProp: DateTimePickerModel — new dynamic property values
// oldProp: DateTimePickerModel — previous property values
// Returns void
```

### navigateTo(view, date)

Programmatically navigate to a specific view and date.

```tsx
import React, { useRef } from 'react';
import { DateTimePickerComponent } from '@syncfusion/ej2-react-calendars';

export default function NavigateToExample() {
  const dateTimeRef = useRef<DateTimePickerComponent>(null);

  const navigateToYear = () => {
    (dateTimeRef.current as any).navigateTo('Year', new Date(2025, 0, 1));
  };

  const navigateToDecade = () => {
    (dateTimeRef.current as any).navigateTo('Decade', new Date(2020, 0, 1));
  };

  const navigateToMonth = () => {
    (dateTimeRef.current as any).navigateTo('Month', new Date());
  };

  return (
    <div>
      <DateTimePickerComponent ref={dateTimeRef} />
      <button onClick={navigateToYear}>Go to Year View</button>
      <button onClick={navigateToDecade}>Go to Decade View</button>
      <button onClick={navigateToMonth}>Go to Month View</button>
    </div>
  );
}
```

### focusIn()

Programmatically set focus to the input.

```tsx
const handleFocus = () => {
  (dateTimeRef.current as any).focusIn();
};

<button onClick={handleFocus}>Focus DateTimePicker</button>
```

### focusOut()

Remove focus from the input.

```tsx
const handleBlur = () => {
  (dateTimeRef.current as any).focusOut();
};

<button onClick={handleBlur}>Blur DateTimePicker</button>
```

### destroy()

Completely destroy the widget and remove all event listeners.

```tsx
const handleDestroy = () => {
  (dateTimeRef.current as any).destroy();
};

<button onClick={handleDestroy}>Destroy Component</button>
```

## Advanced Examples

### Example 1: Controlled DateTimePicker with Validation

```tsx
import React, { useState } from 'react';
import { DateTimePickerComponent, ChangedEventArgs } from '@syncfusion/ej2-react-calendars';

export default function ControlledWithValidation() {
  const [value, setValue] = useState<Date | null>(null);
  const [errors, setErrors] = useState<string[]>([]);

  const handleChange = (args: ChangedEventArgs) => {
    const newErrors: string[] = [];

    if (!args.value) {
      newErrors.push('Date and time are required');
    } else {
      const selected = args.value as Date;
      const now = new Date();

      if (selected < now) {
        newErrors.push('Cannot select past dates');
      }

      if (selected.getHours() < 9 || selected.getHours() >= 17) {
        newErrors.push('Outside business hours (9 AM - 5 PM)');
      }
    }

    setErrors(newErrors);
    setValue(args.value as Date);
  };

  return (
    <div style={{ padding: '20px' }}>
      <DateTimePickerComponent
        value={value}
        change={handleChange}
        min={new Date()}
        minTime={new Date(0, 0, 0, 9, 0)}
        maxTime={new Date(0, 0, 0, 17, 0)}
        placeholder="Select valid appointment"
      />
      {errors.length > 0 && (
        <div style={{ color: 'red', marginTop: '10px' }}>
          {errors.map((err, i) => <p key={i}>❌ {err}</p>)}
        </div>
      )}
      {value && errors.length === 0 && (
        <p style={{ color: 'green' }}>✅ Valid selection: {value.toLocaleString()}</p>
      )}
    </div>
  );
}
```

### Example 2: LinkedDateTimePickers (Start/End Times)

```tsx
import React, { useState } from 'react';
import { DateTimePickerComponent, ChangedEventArgs } from '@syncfusion/ej2-react-calendars';

export default function LinkedDateTimePickers() {
  const [startDateTime, setStartDateTime] = useState<Date | null>(new Date());
  const [endDateTime, setEndDateTime] = useState<Date | null>(
    new Date(new Date().getTime() + 60 * 60 * 1000)
  );

  const handleStartChange = (args: ChangedEventArgs) => {
    setStartDateTime(args.value as Date);

    // Auto-set end time to 1 hour later
    if (args.value) {
      const endTime = new Date((args.value as Date).getTime() + 60 * 60 * 1000);
      setEndDateTime(endTime);
    }
  };

  const handleEndChange = (args: ChangedEventArgs) => {
    if (args.value && startDateTime) {
      if ((args.value as Date) > startDateTime) {
        setEndDateTime(args.value as Date);
      } else {
        alert('End time must be after start time');
      }
    }
  };

  return (
    <div style={{ padding: '20px' }}>
      <h3>Schedule Time Period</h3>

      <div style={{ marginBottom: '15px' }}>
        <label>Start:</label>
        <DateTimePickerComponent
          value={startDateTime}
          change={handleStartChange}
          format="MM/dd/yyyy hh:mm a"
        />
      </div>

      <div>
        <label>End:</label>
        <DateTimePickerComponent
          value={endDateTime}
          change={handleEndChange}
          min={startDateTime}
          format="MM/dd/yyyy hh:mm a"
        />
      </div>

      {startDateTime && endDateTime && (
        <p>
          Duration: {Math.floor((endDateTime.getTime() - startDateTime.getTime()) / 60000)} minutes
        </p>
      )}
    </div>
  );
}
```

## Summary

- **change**: Value changed; use for validation and state updates.
- **focus/blur**: Track input interaction and blur validation.
- **open/close**: Prefetch data or save drafts.
- **navigated**: Monitor calendar view navigation.
- **renderDayCell**: Customize day appearance (disable, highlight).
- **cleared**: Fires when clear button is clicked; does not carry the previous value.
- **created/destroyed**: Component lifecycle hooks.
- **Methods**: `currentView()`, `navigateTo(view, date)`, `focusIn()`, `focusOut()`, `destroy()`, `getPersistData()`, `removeDate(dates)`, `onPropertyChanged(newProp, oldProp)` (internal).

