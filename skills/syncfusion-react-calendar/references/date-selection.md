# Date Selection Patterns

## Table of Contents
- [Single Date Selection](#single-date-selection)
- [Multiple Date Selection](#multiple-date-selection)
- [Date Range Selection](#date-range-selection)
- [Min/Max Date Constraints](#minmax-date-constraints)
- [Disabling Specific Dates](#disabling-specific-dates)
- [Reading Selection State](#reading-selection-state)

---

## Single Date Selection

The default Calendar behavior. User clicks a date, and it becomes selected.

```jsx
import React, { useState } from 'react';
import { CalendarComponent } from '@syncfusion/ej2-react-calendars';

export default function SingleDateExample() {
  const [selectedDate, setSelectedDate] = useState(new Date());

  const onChange = (args) => {
    console.log('Selected date:', args.value);
    setSelectedDate(args.value);
  };

  return (
    <div style={{ padding: 20 }}>
      <h3>Pick a single date</h3>
      <CalendarComponent value={selectedDate} change={onChange} />
      <p>You picked: {selectedDate.toDateString()}</p>
    </div>
  );
}
```

**Key point:** The `value` prop holds a single `Date` object. Update via the `change` event.

---

## Multiple Date Selection

Use the `isMultiSelection` and `values` props to enable native multiple date selection. The `change` event returns `args.values` (array) when multi-selection is active.

```jsx
import React, { useState } from 'react';
import { CalendarComponent } from '@syncfusion/ej2-react-calendars';

export default function MultiDateExample() {
  const [selectedDates, setSelectedDates] = useState([
    new Date(2026, 10, 5),
    new Date(2026, 10, 12),
  ]);

  const onChange = (args) => {
    // args.values contains the updated Date[] array
    if (args.values) {
      setSelectedDates(args.values);
    }
  };

  return (
    <div style={{ padding: 20 }}>
      <h3>Pick multiple dates</h3>
      <CalendarComponent
        isMultiSelection={true}
        values={selectedDates}
        change={onChange}
      />
      <p>Selected dates:</p>
      <ul>
        {selectedDates.map((d, i) => (
          <li key={i}>{d.toDateString()}</li>
        ))}
      </ul>
    </div>
  );
}
```

**Key points:**
- Set `isMultiSelection={true}` to enable the built-in multi-date selection mode.
- Use the `values` prop (not `value`) to provide the initial selection array.
- In the `change` handler, read `args.values` to get the full updated array.
- To add/remove dates imperatively, use the `addDate()` and `removeDate()` methods via a ref.

---

## Date Range Selection

For selecting a date range (start–end), use conditional state to track both dates.

```jsx
import React, { useState } from 'react';
import { CalendarComponent } from '@syncfusion/ej2-react-calendars';

export default function DateRangeExample() {
  const [startDate, setStartDate] = useState(null);
  const [endDate, setEndDate] = useState(null);

  const onChange = (args) => {
    if (!startDate || (startDate && endDate)) {
      // First click or reset: set start date
      setStartDate(args.value);
      setEndDate(null);
    } else {
      // Second click: set end date
      const start = startDate;
      const end = args.value;
      if (end < start) {
        setStartDate(end);
        setEndDate(start);
      } else {
        setStartDate(start);
        setEndDate(end);
      }
    }
  };

  return (
    <div style={{ padding: 20 }}>
      <h3>Select a date range</h3>
      <CalendarComponent value={startDate} change={onChange} />
      <p>
        Start: {startDate ? startDate.toDateString() : 'Not selected'}
        <br />
        End: {endDate ? endDate.toDateString() : 'Not selected'}
      </p>
      <button onClick={() => { setStartDate(null); setEndDate(null); }}>
        Reset
      </button>
    </div>
  );
}
```

For a more polished UX with visual range highlighting, use the `renderDayCell` hook (see events-methods.md).

---

## Min/Max Date Constraints

Restrict selectable dates to a given range.

```jsx
import React, { useState } from 'react';
import { CalendarComponent } from '@syncfusion/ej2-react-calendars';

export default function ConstrainedExample() {
  const [value, setValue] = useState(new Date());
  const minDate = new Date(2026, 0, 1);   // Jan 1, 2026
  const maxDate = new Date(2026, 11, 31); // Dec 31, 2026

  return (
    <div style={{ padding: 20 }}>
      <h3>Pick a date in 2026</h3>
      <CalendarComponent
        value={value}
        min={minDate}
        max={maxDate}
        change={(e) => setValue(e.value)}
      />
      <p>Selected: {value.toDateString()}</p>
    </div>
  );
}
```

**Result:** Dates outside the min/max range appear disabled (grayed out), and users cannot click them.

---

## Disabling Specific Dates

Use `renderDayCell` to disable individual dates (e.g., weekends, holidays).

```jsx
import React, { useState } from 'react';
import { CalendarComponent } from '@syncfusion/ej2-react-calendars';

export default function DisableDatesExample() {
  const [value, setValue] = useState(new Date());

  // Disable weekends and specific holidays
  const holidayDates = [
    new Date(2026, 11, 25), // Christmas
  ];

  const renderDayCell = (args) => {
    const date = args.date;
    const dayOfWeek = date.getDay();

    // Disable weekends
    if (dayOfWeek === 0 || dayOfWeek === 6) {
      args.isDisabled = true;
    }

    // Disable holidays
    if (holidayDates.some(h => h.toDateString() === date.toDateString())) {
      args.isDisabled = true;
    }
  };

  return (
    <div style={{ padding: 20 }}>
      <h3>Weekends and holidays disabled</h3>
      <CalendarComponent
        value={value}
        change={(e) => setValue(e.value)}
        renderDayCell={renderDayCell}
      />
    </div>
  );
}
```

---

## Reading Selection State

### Direct from the component (using ref)

```jsx
import React, { useRef } from 'react';
import { CalendarComponent } from '@syncfusion/ej2-react-calendars';

export default function ReadStateExample() {
  const calendarRef = useRef(null);

  const getSelected = () => {
    if (calendarRef.current) {
      console.log('Current value:', calendarRef.current.value);
    }
  };

  return (
    <div>
      <CalendarComponent ref={calendarRef} />
      <button onClick={getSelected}>Get Selected Date</button>
    </div>
  );
}
```

### Via React state (recommended)

Always keep React state in sync with the component via the `change` event, as shown in the single-date example above. This is the idiomatic React pattern.

---

## Best Practices

1. **Always use React state** to track the selected date(s) so your component re-renders correctly.
2. **Use `renderDayCell` for complex logic** like disabling specific dates or adding custom styling.
3. **Validate dates** before storing or sending to the backend (check min/max, format, etc.).
4. **Provide feedback** via `p` tags or alerts when a user selects an invalid date.
5. **Test edge cases** like the 29th of February, month boundaries, and timezone differences.

---

## Common Pitfalls

- **Not syncing React state**: If you update only the component's internal value without updating React state, re-renders may not reflect the new selection.
- **Comparing dates as strings inconsistently**: Always use `.toDateString()` or timestamp comparison for consistency.
- **Forgetting to disable the selected date in range mode**: After the user picks a start date, you may want to disable that date in subsequent picks to avoid start === end.
