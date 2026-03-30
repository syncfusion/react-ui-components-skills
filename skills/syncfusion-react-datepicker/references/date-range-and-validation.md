# Date Range & Validation

## Table of Contents
- [Min and Max Dates](#min-and-max-dates)
- [Basic Range Restriction](#basic-range-restriction)
- [Strict Mode](#strict-mode)
- [Out-of-Range Behavior](#out-of-range-behavior)
- [Error States](#error-states)
- [Common Scenarios](#common-scenarios)
- [Edge Cases](#edge-cases)

## Min and Max Dates

The `min` and `max` properties restrict which dates users can select:

- **`min`** - Earliest selectable date
- **`max`** - Latest selectable date
- Dates outside this range are disabled (grayed out) in the calendar

### Requirements

- `min` value must be less than `max` value
- Both should be valid `Date` objects
- Setting `min > max` will not restrict (settings are ignored)

## Basic Range Restriction

### Example: Select Dates in Current Month

```jsx
import React, { useState } from 'react';
import { DatePickerComponent } from '@syncfusion/ej2-react-calendars';

export default function App() {
  const today = new Date();
  const minDate = new Date(today.getFullYear(), today.getMonth(), 1);  // 1st of month
  const maxDate = new Date(today.getFullYear(), today.getMonth() + 1, 0); // Last of month

  return (
    <DatePickerComponent
      value={new Date()}
      min={minDate}
      max={maxDate}
      placeholder="Select a date in this month"
    />
  );
}
```

**Result:**
- Calendar only shows dates from March 1 - March 31, 2026
- Dates before March 1 are grayed out
- Dates after March 31 are grayed out
- User cannot click disabled dates

### Example: Birth Date Picker (Historical Dates)

```jsx
<DatePickerComponent
  value={null}
  min={new Date(1900, 0, 1)}  // January 1, 1900
  max={new Date()}             // Today
  placeholder="Select birth date"
/>
```

**Users can:**
- Navigate back to historical dates
- Select any date from 1900 to today
- Dates after today are disabled

### Example: Booking Date (Future Dates)

```jsx
<DatePickerComponent
  value={new Date()}
  min={new Date()}                    // Today
  max={new Date(Date.now() + 30*24*60*60*1000)}  // 30 days from now
  placeholder="Book within 30 days"
/>
```

**Users can:**
- Select today or future dates up to 30 days away
- Past dates are disabled
- Dates beyond 30 days are disabled

## Strict Mode

The `strictMode` property controls behavior when user enters or selects an out-of-range date.

### strictMode: false (Default)

```jsx
<DatePickerComponent
  value={new Date(2026, 0, 15)}  // Jan 15, 2026
  min={new Date(2026, 0, 10)}    // Jan 10
  max={new Date(2026, 0, 20)}    // Jan 20
  strictMode={false}
  placeholder="Select date in range"
/>
```

**Behavior:**
- User can type any date or select from calendar
- Out-of-range values are allowed but marked with error styling (red border)
- Component does NOT auto-correct
- Developer must handle validation separately
- Useful for showing "this date is invalid" error messages

### strictMode: true

```jsx
<DatePickerComponent
  value={new Date(2026, 0, 15)}
  min={new Date(2026, 0, 10)}
  max={new Date(2026, 0, 20)}
  strictMode={true}
  placeholder="Select date in range"
/>
```

**Behavior:**
- If user enters date outside range, it auto-corrects to nearest boundary
- If date < min, value becomes min date
- If date > max, value becomes max date
- No error styling appears
- Useful for "force valid input" scenarios

### Comparison: strictMode false vs true

| Scenario | strictMode: false | strictMode: true |
|----------|-------------------|-----------------|
| Min: 10, Max: 20, User types 5 | Shows error styling, value = 5 | Auto-corrects, value = 10 |
| Min: 10, Max: 20, User types 25 | Shows error styling, value = 25 | Auto-corrects, value = 20 |
| Min: 10, Max: 20, User types 15 | Valid, displays normally | Valid, displays normally |

## Out-of-Range Behavior

### Without strictMode (strictMode: false)

```jsx
const [date, setDate] = useState(new Date(2026, 0, 5));  // Jan 5

<DatePickerComponent
  value={date}
  onChange={(e) => setDate(e.value)}
  min={new Date(2026, 0, 10)}  // Jan 10
  max={new Date(2026, 0, 20)}  // Jan 20
  strictMode={false}
/>
```

**User's perspective:**
1. Value initialized to Jan 5 (outside range)
2. Input shows Jan 5 with error styling (red border)
3. Calendar opens, but Jan 5 is grayed out and unselectable
4. User must select a date within Jan 10-20
5. Once valid date selected, error styling disappears

**When to use:** Form validation, showing field-level errors, user feedback

### With strictMode (strictMode: true)

```jsx
const [date, setDate] = useState(new Date(2026, 0, 5));

<DatePickerComponent
  value={date}
  onChange={(e) => setDate(e.value)}
  min={new Date(2026, 0, 10)}
  max={new Date(2026, 0, 20)}
  strictMode={true}
/>
```

**User's perspective:**
1. Value initialized to Jan 5 (outside range)
2. Component auto-corrects to Jan 10 (the min date)
3. Input displays Jan 10 with no error
4. User sees Jan 10 as pre-selected, can choose other dates in range

**When to use:** Default selection, no error handling needed, seamless UX

## Error States

When `strictMode={false}` and user enters out-of-range date:

```jsx
import React, { useState } from 'react';
import { DatePickerComponent } from '@syncfusion/ej2-react-calendars';

export default function App() {
  const [date, setDate] = useState(null);
  const [error, setError] = useState('');
  
  const minDate = new Date(2026, 0, 10);
  const maxDate = new Date(2026, 0, 20);

  const handleChange = (e) => {
    setDate(e.value);
    
    // Manual validation
    if (e.value && (e.value < minDate || e.value > maxDate)) {
      setError(`Date must be between ${minDate.toDateString()} and ${maxDate.toDateString()}`);
    } else {
      setError('');
    }
  };

  return (
    <div>
      <DatePickerComponent
        value={date}
        change={handleChange}
        min={minDate}
        max={maxDate}
        strictMode={false}
        placeholder="Select date 10-20 Jan"
      />
      {error && <p style={{ color: 'red' }}>{error}</p>}
    </div>
  );
}
```

**User types 5 Jan (outside range):**
- Input shows red border (error styling)
- Error message displays below
- Developer can validate and show custom message

## Common Scenarios

### 1. Hotel Checkout (Min = Today, Max = Today + 30 Days)

```jsx
<DatePickerComponent
  value={new Date()}
  min={new Date()}
  max={new Date(Date.now() + 30 * 24 * 60 * 60 * 1000)}
  strictMode={true}
  format="dd/MM/yyyy"
  placeholder="Check-out date (next 30 days)"
/>
```

Ensures bookings are within reasonable timeframe.

### 2. Employee Start Date (Min = Today, Max = 180 Days)

```jsx
<DatePickerComponent
  value={new Date()}
  min={new Date()}
  max={new Date(Date.now() + 180 * 24 * 60 * 60 * 1000)}
  strictMode={false}
  format="yyyy-MM-dd"
  placeholder="Start date"
/>
```

Shows error if date outside 180-day window.

### 3. Invoice Date (Min = Current Year Start, Max = Today)

```jsx
<DatePickerComponent
  value={new Date()}
  min={new Date(new Date().getFullYear(), 0, 1)}
  max={new Date()}
  strictMode={true}
  format="dd/MM/yyyy"
  placeholder="Invoice date"
/>
```

Restricts to dates within current year, auto-corrects if needed.

### 4. Historical Report (Min = Any, Max = Today)

```jsx
<DatePickerComponent
  value={null}
  min={new Date(1990, 0, 1)}
  max={new Date()}
  strictMode={false}
  format="yyyy-MM-dd"
  placeholder="Report date"
/>
```

Allows historical dates but prevents future dates.

## Edge Cases

### Leap Year Boundaries

```jsx
// Feb 29 in leap year (2024) vs non-leap year (2025)
const leapYearMin = new Date(2024, 1, 29);  // Feb 29, 2024 (valid)
const leapYearMax = new Date(2024, 1, 29);

const nonLeapMin = new Date(2025, 1, 28);   // Feb 28, 2025 (no Feb 29)
const nonLeapMax = new Date(2025, 1, 28);

<DatePickerComponent min={nonLeapMin} max={nonLeapMax} />
```

DatePicker handles Feb 29 validity automatically.

### Month Boundary

```jsx
const minDate = new Date(2026, 0, 31);  // Jan 31
const maxDate = new Date(2026, 1, 28);  // Feb 28 (no 31st in Feb)

<DatePickerComponent min={minDate} max={maxDate} />
```

Months with different lengths are handled correctly.

### Same Day (Min = Max)

```jsx
// Only one day allowed
<DatePickerComponent
  min={new Date(2026, 0, 15)}
  max={new Date(2026, 0, 15)}
  placeholder="Only 15 Jan selectable"
/>
```

Only that single date is selectable. All other dates are disabled.

### Null/Undefined Values

```jsx
<DatePickerComponent
  value={null}  // No initial date
  min={new Date()}
  max={new Date(Date.now() + 7*24*60*60*1000)}
  placeholder="Select date"
/>
```

Component displays empty, user must select a date within range.

### Updating Range Dynamically

```jsx
const [minDate, setMinDate] = useState(new Date());
const [maxDate, setMaxDate] = useState(new Date(Date.now() + 30*24*60*60*1000));

<DatePickerComponent
  min={minDate}
  max={maxDate}
/>

// Later, update range
setMinDate(new Date(Date.now() - 7*24*60*60*1000));  // Past 7 days
setMaxDate(new Date(Date.now() + 60*24*60*60*1000)); // Next 60 days
```

Component re-renders with new range, calendar updates accordingly.

