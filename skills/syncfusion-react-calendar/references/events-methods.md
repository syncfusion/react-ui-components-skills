# Events and Methods

## Table of Contents
- [Calendar Events](#calendar-events)
- [Event Handlers (Examples)](#event-handlers-examples)
- [Calendar Methods](#calendar-methods)
  - [navigateTo](#navigateto-view-date-iscustomdate)
  - [currentView](#currentview)
  - [addDate](#adddate-dates)
  - [removeDate](#removedate-dates)
  - [destroy](#destroy)
  - [getPersistData](#getpersistdata)
- [Using Refs for Imperative Control](#using-refs-for-imperative-control)
- [Advanced: renderDayCell Hook](#advanced-renderdaycell-hook)

---

## Calendar Events

### change

Fired when the user selects a date or the selected date changes programmatically.

**Handler signature:**
```typescript
(args: { value: Date }) => void
```

**Example:**
```jsx
const onChange = (args) => {
  console.log('Selected date:', args.value);
};

<CalendarComponent change={onChange} />
```

### created

Fired after the Calendar component is initialized and rendered.

**Handler signature:**
```typescript
() => void
```

**Use case:** Initialize component-dependent logic, fetch data for the selected month, etc.

```jsx
const onCreated = () => {
  console.log('Calendar initialized');
};

<CalendarComponent created={onCreated} />
```

### destroyed

Fired when the component is about to be removed from the DOM (cleanup phase).

**Handler signature:**
```typescript
() => void
```

**Use case:** Cleanup timers, event listeners, or external resources.

```jsx
const onDestroyed = () => {
  console.log('Calendar destroyed, cleanup resources');
};

<CalendarComponent destroyed={onDestroyed} />
```

### navigated

Fired after the view navigates (e.g., Month → Year view).

**Handler signature (may vary by version):**
```typescript
(args: any) => void
```

**Example:**
```jsx
const onNavigated = (args) => {
  console.log('Navigation event:', args);
};

<CalendarComponent navigated={onNavigated} />
```

### renderDayCell

Hook for customizing individual day cells before they are rendered. Allows you to:
- Disable specific dates
- Add custom styling
- Add visual indicators (badges, highlights)

**Handler signature:**
```typescript
(args: { date: Date; cellElement: HTMLElement; isDisabled?: boolean }) => void
```

**See the [Advanced: renderDayCell Hook](#advanced-renderdaycell-hook) section below for detailed examples.**

---

## Event Handlers (Examples)

### Example 1: Track Selection and Display

```jsx
import React, { useState } from 'react';
import { CalendarComponent } from '@syncfusion/ej2-react-calendars';

export default function EventTrackingExample() {
  const [selectedDate, setSelectedDate] = useState(new Date());
  const [eventLog, setEventLog] = useState([]);

  const onChange = (args) => {
    const newEntry = `Selected: ${args.value.toDateString()}`;
    setSelectedDate(args.value);
    setEventLog([...eventLog, newEntry]);
  };

  const onCreated = () => {
    setEventLog(['Calendar initialized']);
  };

  return (
    <div style={{ padding: 20 }}>
      <CalendarComponent
        change={onChange}
        created={onCreated}
      />
      <h3>Current: {selectedDate.toDateString()}</h3>
      <h4>Event Log:</h4>
      <ul>
        {eventLog.map((log, i) => <li key={i}>{log}</li>)}
      </ul>
    </div>
  );
}
```

### Example 2: Detect Month Changes

```jsx
import React, { useState, useRef } from 'react';
import { CalendarComponent } from '@syncfusion/ej2-react-calendars';

export default function MonthChangeDetectorExample() {
  const [currentMonth, setCurrentMonth] = useState(new Date());
  const prevMonthRef = useRef(new Date().getMonth());

  const onChange = (args) => {
    const newMonth = args.value.getMonth();
    if (newMonth !== prevMonthRef.current) {
      console.log('Month changed from', prevMonthRef.current, 'to', newMonth);
      prevMonthRef.current = newMonth;
    }
    setCurrentMonth(args.value);
  };

  return (
    <div style={{ padding: 20 }}>
      <CalendarComponent change={onChange} />
      <p>Current: {currentMonth.toLocaleString('default', { month: 'long', year: 'numeric' })}</p>
    </div>
  );
}
```

---

## Calendar Methods

Call these methods via a ref to the `CalendarComponent` instance.

### navigateTo(view, date, isCustomDate?)

Navigate the calendar to a specific view and date.

**Signature:** `navigateTo(view: CalendarView, date: Date, isCustomDate?: boolean): void`

- `view` — Target view: `'Month'`, `'Year'`, or `'Decade'`.
- `date` — The focused date in that view.
- `isCustomDate` _(optional)_ — Whether the calendar is rendered with a custom today date.

```jsx
import React, { useRef } from 'react';
import { CalendarComponent } from '@syncfusion/ej2-react-calendars';

export default function NavigateToExample() {
  const calendarRef = useRef(null);

  const jumpToDate = (view, year, month, day) => {
    if (calendarRef.current) {
      calendarRef.current.navigateTo(view, new Date(year, month - 1, day));
    }
  };

  return (
    <div style={{ padding: 20 }}>
      <CalendarComponent ref={calendarRef} />
      <div style={{ marginTop: 20 }}>
        <button onClick={() => jumpToDate('Month', 2026, 1, 1)}>Jan 2026</button>
        <button onClick={() => jumpToDate('Month', 2026, 7, 15)}>July 2026</button>
        <button onClick={() => jumpToDate('Year', 2025, 1, 1)}>Year 2025</button>
      </div>
    </div>
  );
}
```

### currentView()

Get the current view type as a string.

```jsx
const getView = () => {
  if (calendarRef.current) {
    const view = calendarRef.current.currentView();
    console.log('Current view:', view); // 'Month', 'Year', or 'Decade'
  }
};

<button onClick={getView}>Check View</button>
```

### addDate(dates)

Adds one or multiple dates to the `values` property (for multi-selection).

**Signature:** `addDate(dates: Date | Date[]): void`

```jsx
const addDates = () => {
  calendarRef.current?.addDate([new Date(2026, 10, 5), new Date(2026, 10, 10)]);
};
```

### removeDate(dates)

Removes one or multiple dates from the `values` property.

**Signature:** `removeDate(dates: Date | Date[]): void`

```jsx
const removeDates = () => {
  calendarRef.current?.removeDate(new Date(2026, 10, 5));
};
```

### destroy()

Destroys the widget and releases all resources.

**Signature:** `destroy(): void`

```jsx
const destroyCalendar = () => {
  calendarRef.current?.destroy();
};
```

### getPersistData()

Returns the properties to be maintained upon browser refresh (used with `enablePersistence`).

**Signature:** `getPersistData(): string`

```jsx
const persistData = calendarRef.current?.getPersistData();
console.log(persistData);
```

---

## Using Refs for Imperative Control

### Complete Ref Example

```jsx
import React, { useRef, useState } from 'react';
import { CalendarComponent } from '@syncfusion/ej2-react-calendars';

export default function ImperativeControlExample() {
  const calendarRef = useRef(null);
  const [status, setStatus] = useState('');

  const handleNavigate = (offset) => {
    if (calendarRef.current && calendarRef.current.value) {
      const newDate = new Date(calendarRef.current.value);
      newDate.setMonth(newDate.getMonth() + offset);
      // navigateTo(view, date)
      calendarRef.current.navigateTo('Month', newDate);
      setStatus(`Navigated to ${newDate.toLocaleString('default', { month: 'long', year: 'numeric' })}`);
    }
  };

  const handleGetInfo = () => {
    if (calendarRef.current) {
      const view = calendarRef.current.currentView();
      const value = calendarRef.current.value;
      setStatus(`View: ${view}, Date: ${value?.toDateString() || 'None'}`);
    }
  };

  return (
    <div style={{ padding: 20 }}>
      <CalendarComponent ref={calendarRef} />
      <div style={{ marginTop: 20 }}>
        <button onClick={() => handleNavigate(-1)}>← Previous Month</button>
        <button onClick={() => handleNavigate(1)}>Next Month →</button>
        <button onClick={handleGetInfo}>Get Info</button>
      </div>
      <p style={{ marginTop: 10, color: '#0066cc' }}>{status}</p>
    </div>
  );
}
```

---

## Advanced: renderDayCell Hook

The `renderDayCell` callback is invoked for every day cell before rendering. Use it to customize appearance and behavior.

### Parameters

- `args.date` — The `Date` object for the cell.
- `args.cellElement` — The DOM element representing the cell.
- `args.isDisabled` — Boolean; set to `true` to disable the cell.

### Example: Disable Weekends

```jsx
const renderDayCell = (args) => {
  const dayOfWeek = args.date.getDay();
  if (dayOfWeek === 0 || dayOfWeek === 6) {
    args.isDisabled = true;
  }
};

<CalendarComponent renderDayCell={renderDayCell} />
```

### Example: Highlight Today and Add Inline Styles

```jsx
const renderDayCell = (args) => {
  const today = new Date();
  today.setHours(0, 0, 0, 0);

  if (args.date.toDateString() === today.toDateString()) {
    args.cellElement.style.backgroundColor = '#ffc107';
    args.cellElement.style.fontWeight = 'bold';
  }
};

<CalendarComponent renderDayCell={renderDayCell} />
```

### Example: Add CSS Class for Styling

```jsx
const renderDayCell = (args) => {
  const eventDates = ['2026-03-15', '2026-03-25'];
  const dateStr = args.date.toISOString().split('T')[0];

  if (eventDates.includes(dateStr)) {
    args.cellElement.classList.add('has-event');
  }
};

// In your CSS file:
// .e-calendar .e-cell.has-event {
//   background-color: #e7f3ff;
//   border: 2px solid #0066ff;
// }

<CalendarComponent renderDayCell={renderDayCell} />
```

### Example: Complex Logic (Disable Past + Future Range)

```jsx
const renderDayCell = (args) => {
  const today = new Date();
  today.setHours(0, 0, 0, 0);

  const rangeStart = new Date(2026, 0, 1); // Jan 1, 2026
  const rangeEnd = new Date(2026, 11, 31); // Dec 31, 2026

  // Disable if before today
  if (args.date < today) {
    args.isDisabled = true;
  }
  // Disable if outside range
  else if (args.date < rangeStart || args.date > rangeEnd) {
    args.isDisabled = true;
  }
  // Highlight weekends in the valid range
  else if (args.date.getDay() === 0 || args.date.getDay() === 6) {
    args.cellElement.style.backgroundColor = '#f0f0f0';
  }
};

<CalendarComponent renderDayCell={renderDayCell} />
```

---

## Best Practices

1. **Keep event handlers lightweight** — avoid heavy computations in `change` or `renderDayCell`.
2. **Use refs sparingly** — prefer declarative React patterns (props + state) when possible.
3. **Cleanup in `destroyed`** — if you attach external event listeners in `created`, remove them in `destroyed`.
4. **Cache expensive computations** — in `renderDayCell`, compute disabled dates once outside the callback (not per cell).
5. **Use `addDate`/`removeDate` for multi-selection** — these methods are the idiomatic way to manage the `values` array imperatively.
6. **Always pass both arguments to `navigateTo`** — the method requires both `view` (CalendarView) and `date` (Date).

