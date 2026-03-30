# Calendar Views and Navigation

## Table of Contents
- [View Types](#view-types)
- [Navigating Between Views](#navigating-between-views)
- [Controlling Initial View](#controlling-initial-view)
- [Limiting View Depth](#limiting-view-depth)
- [Month, Year, and Decade Selection](#month-year-and-decade-selection)
- [Programmatic Navigation](#programmatic-navigation)

---

## View Types

The Calendar has three hierarchical views:

1. **Month** (default) — Shows a single month grid. User can click dates or navigate months.
2. **Year** — Shows all 12 months of a year. User can click a month or navigate years.
3. **Decade** — Shows a 10-year range (e.g., 2020–2029). User can click a year or navigate decades.

```
Decade View: 2020 2021 2022 ... 2029
    ↓ (click a year)
Year View: Jan Feb Mar ... Dec
    ↓ (click a month)
Month View: 1 2 3 ... 28 29 30 31
    ↓ (click a day)
Selection complete
```

---

## Navigating Between Views

### Automatic (Click-based)

In **Month** view:
- Click month header (e.g., "March 2026") → navigate to **Year** view for that year.
- Click year header (e.g., "2026") → navigate to **Decade** view containing that year.

In **Year** view:
- Click month name → select that month and return to **Month** view.

In **Decade** view:
- Click year number → navigate to **Year** view for that year.

### Programmatic (using ref)

The `navigateTo` method requires two arguments: the target view (`CalendarView`) and the focused date (`Date`).

```jsx
import React, { useRef } from 'react';
import { CalendarComponent } from '@syncfusion/ej2-react-calendars';

export default function NavigationExample() {
  const calendarRef = useRef(null);

  const goToJuly2026 = () => {
    if (calendarRef.current) {
      // navigateTo(view: CalendarView, date: Date)
      calendarRef.current.navigateTo('Month', new Date(2026, 6, 1));
    }
  };

  const goToYear2025 = () => {
    if (calendarRef.current) {
      calendarRef.current.navigateTo('Year', new Date(2025, 0, 1));
    }
  };

  return (
    <div style={{ padding: 20 }}>
      <CalendarComponent ref={calendarRef} />
      <button onClick={goToJuly2026}>Go to July 2026</button>
      <button onClick={goToYear2025}>Go to Year 2025</button>
    </div>
  );
}
```

---

## Controlling Initial View

Use the `start` prop to specify which view appears when the calendar loads.

```jsx
import React from 'react';
import { CalendarComponent } from '@syncfusion/ej2-react-calendars';

export default function InitialViewExample() {
  return (
    <div style={{ padding: 20 }}>
      <h3>Start in Month view (default)</h3>
      <CalendarComponent start="Month" />

      <h3>Start in Year view</h3>
      <CalendarComponent start="Year" />

      <h3>Start in Decade view</h3>
      <CalendarComponent start="Decade" />
    </div>
  );
}
```

**Use case:** If you want users to pick a year first, start with `start="Year"` or `start="Decade"`.

---

## Limiting View Depth

Use the `depth` prop to restrict the deepest view users can navigate to. Typically matches `start`.

```jsx
import React from 'react';
import { CalendarComponent } from '@syncfusion/ej2-react-calendars';

export default function DepthExample() {
  return (
    <div style={{ padding: 20 }}>
      <h3>Only Month view (no Year or Decade)</h3>
      <CalendarComponent start="Month" depth="Month" />

      <h3>Month and Year only (no Decade)</h3>
      <CalendarComponent start="Month" depth="Year" />

      <h3>All views enabled (default)</h3>
      <CalendarComponent start="Month" depth="Decade" />
    </div>
  );
}
```

**Best practice:** Set `depth={start}` for a cleaner UX, or use `depth="Year"` if you don't want users drilling down to individual decades.

---

## Month, Year, and Decade Selection

### Selecting a Month (in Month view)

```jsx
const [selectedDate, setSelectedDate] = useState(new Date(2026, 2, 15)); // March 15, 2026

<CalendarComponent
  value={selectedDate}
  change={(e) => setSelectedDate(e.value)}
/>
```

When the user clicks a day, the `change` event fires and `args.value` is the selected `Date`.

### Selecting a Year (via Year view)

```jsx
import React, { useRef, useState } from 'react';
import { CalendarComponent } from '@syncfusion/ej2-react-calendars';

export default function YearSelectionExample() {
  const calendarRef = useRef(null);
  const [selectedYear, setSelectedYear] = useState(2026);

  const onChange = (args) => {
    setSelectedYear(args.value.getFullYear());
    console.log('Selected year:', args.value.getFullYear());
  };

  return (
    <div style={{ padding: 20 }}>
      <h3>Pick a year</h3>
      <CalendarComponent
        ref={calendarRef}
        start="Year"
        depth="Year"
        change={onChange}
      />
      <p>Selected year: {selectedYear}</p>
    </div>
  );
}
```

**Note:** Even in Year view, the `change` event returns a full `Date` object. Extract the year with `.getFullYear()`.

### Selecting a Decade

```jsx
import React, { useState } from 'react';
import { CalendarComponent } from '@syncfusion/ej2-react-calendars';

export default function DecadeSelectionExample() {
  const [selectedYear, setSelectedYear] = useState(2025);

  const onChange = (args) => {
    setSelectedYear(args.value.getFullYear());
  };

  return (
    <div style={{ padding: 20 }}>
      <h3>Pick a year from the decade</h3>
      <CalendarComponent
        start="Decade"
        depth="Decade"
        change={onChange}
      />
      <p>Selected year: {selectedYear}</p>
    </div>
  );
}
```

---

## Programmatic Navigation

### Get Current View

```jsx
const calendarRef = useRef(null);

const getCurrentView = () => {
  if (calendarRef.current) {
    const view = calendarRef.current.currentView();
    console.log('Current view:', view); // 'Month', 'Year', or 'Decade'
  }
};

<CalendarComponent ref={calendarRef} />
<button onClick={getCurrentView}>Check Current View</button>
```

### Navigate to Specific Date

The `navigateTo` method signature is: `navigateTo(view: CalendarView, date: Date, isCustomDate?: boolean): void`

```jsx
const navigateTo = (view, year, month, day = 1) => {
  if (calendarRef.current) {
    calendarRef.current.navigateTo(view, new Date(year, month, day));
  }
};

// Navigate to January 2025 in Month view
navigateTo('Month', 2025, 0, 1);

// Navigate to Year view for 2025
navigateTo('Year', 2025, 0, 1);
```

### Listen to Navigation (navigated event)

Some versions of Syncfusion support a `navigated` event that fires after view changes.

```jsx
const onNavigated = (args) => {
  console.log('Navigated. Current view:', args.value);
};

<CalendarComponent navigated={onNavigated} />
```

---

## Common Patterns

### Year-Month Picker

Start in Year view, then move to Month view:

```jsx
<CalendarComponent start="Year" depth="Month" />
```

When user clicks a month, `change` fires and you get a `Date` (first day of the selected month by default).

### Quick Year/Decade Selector

For forms where you only need year selection:

```jsx
<CalendarComponent start="Year" depth="Year" />
```

The user only sees years and cannot drill down to individual months/days.

### Multi-month Navigation

Navigate to the next/previous month programmatically:

```jsx
const goToPrevMonth = () => {
  const ref = calendarRef.current;
  if (ref && ref.value) {
    const prev = new Date(ref.value);
    prev.setMonth(prev.getMonth() - 1);
    // navigateTo(view, date)
    ref.navigateTo('Month', prev);
  }
};

const goToNextMonth = () => {
  const ref = calendarRef.current;
  if (ref && ref.value) {
    const next = new Date(ref.value);
    next.setMonth(next.getMonth() + 1);
    ref.navigateTo('Month', next);
  }
};

<button onClick={goToPrevMonth}>← Previous</button>
<CalendarComponent ref={calendarRef} />
<button onClick={goToNextMonth}>Next →</button>
```

---

## Notes

- **View transitions are automatic** when you click headers or month/year cells; you typically don't need to change `start` or `depth` after initialization.
- **The `change` event always returns a `Date` object**, regardless of the current view.
- **Programmatic navigation** via `navigateTo()` is useful for keyboard shortcuts, external buttons, or conditional flows.
