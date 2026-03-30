# Date and Time Selection

## Table of Contents
- [Single Value Selection](#single-value-selection)
- [Two-Way Binding](#two-way-binding)
- [Setting Initial Values](#setting-initial-values)
- [Min and Max Date Constraints](#min-and-max-date-constraints)
- [Min and Max Time Constraints](#min-and-max-time-constraints)
- [Formatting Selected Values](#formatting-selected-values)
- [Getting the Selected Value](#getting-the-selected-value)

## Single Value Selection

The DateTimePicker stores one selected date and time combined in a single `Date` object.

### Basic Selection Pattern

```tsx
import React, { useState } from 'react';
import { DateTimePickerComponent } from '@syncfusion/ej2-react-calendars';

export default function DateTimeSelector() {
  const [selectedDateTime, setSelectedDateTime] = useState<Date | null>(null);

  const handleChange = (args: any) => {
    setSelectedDateTime(args.value);
    console.log('Selected:', args.value);
  };

  return (
    <div>
      <DateTimePickerComponent
        value={selectedDateTime}
        change={handleChange}
        placeholder="Pick a date and time"
      />
      {selectedDateTime && (
        <div>
          <p>Date: {selectedDateTime.toLocaleDateString()}</p>
          <p>Time: {selectedDateTime.toLocaleTimeString()}</p>
          <p>Full: {selectedDateTime.toLocaleString()}</p>
        </div>
      )}
    </div>
  );
}
```

### Accessing Date Parts

```tsx
const handleChange = (args: any) => {
  const dt = args.value as Date;
  if (dt) {
    console.log('Year:', dt.getFullYear());
    console.log('Month:', dt.getMonth() + 1); // 0-indexed
    console.log('Day:', dt.getDate());
    console.log('Hours:', dt.getHours());
    console.log('Minutes:', dt.getMinutes());
    console.log('Seconds:', dt.getSeconds());
  }
};
```

## Two-Way Binding

React doesn't have built-in two-way binding like Angular, but you can achieve it with controlled components.

### Controlled Component Pattern

```tsx
import React, { useState } from 'react';
import { DateTimePickerComponent } from '@syncfusion/ej2-react-calendars';

export default function ControlledDateTimePicker() {
  const [value, setValue] = useState<Date | null>(new Date(2025, 0, 15, 10, 30));

  return (
    <div>
      <DateTimePickerComponent
        value={value}
        change={(e) => setValue((e as any).value)}
      />
      <button onClick={() => setValue(null)}>Clear</button>
      <button onClick={() => setValue(new Date())}>Set to Now</button>
    </div>
  );
}
```

### Form Integration

```tsx
import React, { useState } from 'react';
import { DateTimePickerComponent } from '@syncfusion/ej2-react-calendars';

export default function DateTimeForm() {
  const [formData, setFormData] = useState({
    eventName: '',
    eventDateTime: new Date(),
  });

  const handleDateTimeChange = (args: any) => {
    setFormData({
      ...formData,
      eventDateTime: args.value,
    });
  };

  const handleSubmit = (e: React.FormEvent) => {
    e.preventDefault();
    console.log('Form submitted:', formData);
    // Send to API
  };

  return (
    <form onSubmit={handleSubmit}>
      <label>
        Event Name:
        <input
          type="text"
          value={formData.eventName}
          onChange={(e) => setFormData({ ...formData, eventName: e.target.value })}
        />
      </label>

      <label>
        Date and Time:
        <DateTimePickerComponent
          value={formData.eventDateTime}
          change={handleDateTimeChange}
        />
      </label>

      <button type="submit">Schedule Event</button>
    </form>
  );
}
```

## Setting Initial Values

### Initialize with Current Date/Time

```tsx
const [value, setValue] = useState<Date | null>(new Date());
```

### Initialize with Specific Date/Time

```tsx
// Specific date and time
const [value, setValue] = useState<Date | null>(
  new Date(2025, 2, 15, 14, 30, 0) // March 15, 2025, 2:30 PM
);

// Or parse from string
const [value, setValue] = useState<Date | null>(
  new Date('2025-03-15T14:30:00')
);
```

### Initialize from API Response

```tsx
import React, { useState, useEffect } from 'react';
import { DateTimePickerComponent } from '@syncfusion/ej2-react-calendars';

export default function DateTimeFromAPI() {
  const [value, setValue] = useState<Date | null>(null);

  useEffect(() => {
    // Simulate API call
    const fetchData = async () => {
      const response = await fetch('/api/event');
      const data = await response.json();
      setValue(new Date(data.scheduledDateTime)); // Parse from ISO string
    };

    fetchData();
  }, []);

  return (
    <DateTimePickerComponent
      value={value}
      change={(e) => setValue((e as any).value)}
    />
  );
}
```

## Min and Max Date Constraints

Control which dates are selectable by setting min/max bounds.

### Basic Min/Max

```tsx
<DateTimePickerComponent
  min={new Date(2025, 0, 1)}  // January 1, 2025
  max={new Date(2025, 11, 31)} // December 31, 2025
  value={new Date()}
/>
```

### Example: Appointments in Next 30 Days

```tsx
const today = new Date();
const thirtyDaysLater = new Date(today.getTime() + 30 * 24 * 60 * 60 * 1000);

<DateTimePickerComponent
  min={today}
  max={thirtyDaysLater}
  placeholder="Select within 30 days"
/>
```

### Example: No Past Dates

```tsx
const today = new Date();
today.setHours(0, 0, 0, 0); // Reset time to midnight

<DateTimePickerComponent
  min={today}
  placeholder="Select today or later"
/>
```

### Dynamic Min/Max Based on Condition

```tsx
import React, { useState } from 'react';
import { DateTimePickerComponent } from '@syncfusion/ej2-react-calendars';

export default function DynamicConstraints() {
  const [isUrgent, setIsUrgent] = useState(false);

  const today = new Date();
  const maxDate = isUrgent
    ? new Date(today.getTime() + 3 * 24 * 60 * 60 * 1000)  // 3 days if urgent
    : new Date(today.getTime() + 30 * 24 * 60 * 60 * 1000); // 30 days otherwise

  return (
    <div>
      <label>
        <input
          type="checkbox"
          checked={isUrgent}
          onChange={(e) => setIsUrgent(e.target.checked)}
        />
        Urgent (3 days only)
      </label>

      <DateTimePickerComponent
        min={today}
        max={maxDate}
        placeholder="Select date"
      />
    </div>
  );
}
```

## Min and Max Time Constraints

Restrict which times can be selected within the time picker popup.

### Basic Time Range

```tsx
<DateTimePickerComponent
  minTime={new Date(0, 0, 0, 8, 0)}   // 8:00 AM
  maxTime={new Date(0, 0, 0, 17, 0)}  // 5:00 PM
  step={30}
  placeholder="Business hours only"
/>
```

**Note:** minTime/maxTime are full Date objects; only the time portion is used.

### Example: Shift Time Slots

```tsx
const getShiftTimes = (shiftNumber: number) => {
  const shifts: Record<number, [number, number]> = {
    1: [8, 16],   // 8 AM - 4 PM
    2: [16, 0],   // 4 PM - midnight (next day)
    3: [0, 8],    // midnight - 8 AM
  };

  const [startHour, endHour] = shifts[shiftNumber];
  return {
    minTime: new Date(0, 0, 0, startHour, 0),
    maxTime: new Date(0, 0, 0, endHour, 0),
  };
};

const { minTime, maxTime } = getShiftTimes(1);

<DateTimePickerComponent minTime={minTime} maxTime={maxTime} />
```

### Example: No Early Morning, No Late Night

```tsx
<DateTimePickerComponent
  minTime={new Date(0, 0, 0, 6, 0)}   // 6:00 AM
  maxTime={new Date(0, 0, 0, 22, 0)}  // 10:00 PM
  placeholder="6 AM to 10 PM only"
/>
```

## Formatting Selected Values

Control how the date and time display in the input and how they parse.

### Using the `format` Property

```tsx
// Format: dd/MM/yyyy hh:mm a (default-like)
<DateTimePickerComponent format="dd/MM/yyyy hh:mm a" />

// Format: MM/dd/yyyy HH:mm:ss (24-hour)
<DateTimePickerComponent format="MM/dd/yyyy HH:mm:ss" />

// Format: MMMM dd, yyyy h:mm A (long-form)
<DateTimePickerComponent format="MMMM dd, yyyy h:mm A" />

// Format: yyyy-MM-dd HH:mm (ISO-like)
<DateTimePickerComponent format="yyyy-MM-dd HH:mm" />
```

### Format String Tokens

- `dd` - Day of month (01-31)
- `MM` - Month (01-12)
- `yyyy` - Full year (e.g., 2025)
- `yy` - Two-digit year (e.g., 25)
- `hh` - Hour (01-12)
- `HH` - Hour (00-23)
- `mm` - Minutes (00-59)
- `ss` - Seconds (00-59)
- `a` / `A` - AM/PM
- `MMMM` - Full month name
- `MMM` - Abbreviated month name

### Using Skeleton Format (Localization-aware)

```tsx
<DateTimePickerComponent
  format={{
    type: 'dateTime',
    skeleton: 'medium'
  }}
/>
```

### Parsing Multiple Input Formats

Allow users to enter dates in different formats:

```tsx
<DateTimePickerComponent
  inputFormats={[
    'dd/MM/yyyy hh:mm a',
    'MM/dd/yyyy hh:mm a',
    'yyyy-MM-dd HH:mm'
  ]}
  format="dd/MM/yyyy hh:mm a"
  placeholder="dd/MM/yyyy hh:mm a or MM/dd/yyyy hh:mm a"
/>
```

## Getting the Selected Value

### From Change Event

```tsx
const handleChange = (args: any) => {
  const selectedDateTime: Date = args.value;
  console.log('Selected:', selectedDateTime);
};

<DateTimePickerComponent change={handleChange} />
```

### From Ref Using currentView / navigateTo

```tsx
import React, { useRef } from 'react';
import { DateTimePickerComponent } from '@syncfusion/ej2-react-calendars';

export default function GetValue() {
  const dateTimeRef = useRef<DateTimePickerComponent>(null);

  const handleGetValue = () => {
    if (dateTimeRef.current) {
      // Access the value property
      console.log('Current value:', (dateTimeRef.current as any).value);
    }
  };

  return (
    <div>
      <DateTimePickerComponent ref={dateTimeRef} />
      <button onClick={handleGetValue}>Get Current Value</button>
    </div>
  );
}
```

### For Form Submission

```tsx
import React, { useRef } from 'react';
import { DateTimePickerComponent } from '@syncfusion/ej2-react-calendars';

export default function FormWithDateTime() {
  const dateTimeRef = useRef<DateTimePickerComponent>(null);

  const handleSubmit = (e: React.FormEvent) => {
    e.preventDefault();

    const selectedDateTime = (dateTimeRef.current as any).value as Date;

    if (!selectedDateTime) {
      alert('Please select a date and time');
      return;
    }

    // Send to API
    const payload = {
      scheduledDateTime: selectedDateTime.toISOString(),
      timestamp: selectedDateTime.getTime(),
    };

    console.log('Submitting:', payload);
  };

  return (
    <form onSubmit={handleSubmit}>
      <DateTimePickerComponent ref={dateTimeRef} />
      <button type="submit">Submit</button>
    </form>
  );
}
```

## Summary

- **Selection:** Store selected date+time in a single Date object via the `value` prop and `change` event.
- **Constraints:** Use `min`/`max` for dates and `minTime`/`maxTime` for times.
- **Formatting:** Use the `format` prop to control display and input parsing.
- **Initialization:** Set initial values in `useState` or via API.
- **Retrieval:** Access via `change` event handler or ref to `.value`.

