# Time Configuration and Intervals

## Table of Contents
- [Time Step Configuration](#time-step-configuration)
- [Time Format](#time-format)
- [Min and Max Time Constraints](#min-and-max-time-constraints)
- [Scroll Position](#scroll-position)
- [Time Picker Popup Behavior](#time-picker-popup-behavior)
- [Examples and Use Cases](#examples-and-use-cases)

## Time Step Configuration

The `step` property controls the time interval in minutes between adjacent time options in the time picker popup list.

### Basic Step Usage

```tsx
// 30-minute intervals (default)
<DateTimePickerComponent step={30} />

// 15-minute intervals
<DateTimePickerComponent step={15} />

// 60-minute intervals (hourly)
<DateTimePickerComponent step={60} />

// Custom: 5-minute intervals
<DateTimePickerComponent step={5} />
```

### Step Value Impact

When you open the time picker popup:
- **step={15}**: Times shown: 12:00, 12:15, 12:30, 12:45, 1:00, ...
- **step={30}**: Times shown: 12:00, 12:30, 1:00, 1:30, ...
- **step={60}**: Times shown: 12:00, 1:00, 2:00, 3:00, ...

### Dynamic Step Based on Context

```tsx
import React, { useState } from 'react';
import { DateTimePickerComponent } from '@syncfusion/ej2-react-calendars';

export default function DynamicStep() {
  const [precision, setPrecision] = useState('standard'); // or 'precise'

  const stepValue = precision === 'precise' ? 5 : 30;

  return (
    <div>
      <label>
        <select value={precision} onChange={(e) => setPrecision(e.target.value)}>
          <option value="standard">Standard (30 min)</option>
          <option value="precise">Precise (5 min)</option>
        </select>
      </label>

      <DateTimePickerComponent
        step={stepValue}
        placeholder="Select time"
      />
    </div>
  );
}
```

### Recommended Step Values by Use Case

| Use Case | Step (minutes) | Example Times |
|----------|----------------|---------------|
| Shift scheduling | 60 | 8:00, 9:00, 10:00 AM |
| Appointment booking | 15, 30 | 2:00, 2:15, 2:30 PM |
| Medical appointments | 10, 15 | 9:00, 9:10, 9:20 AM |
| Hourly slots | 60 | 10:00 AM, 11:00 AM |
| Fine-grain scheduling | 5 | 2:00, 2:05, 2:10 PM |

## Time Format

The `timeFormat` property controls how times display in the time picker popup list.

### Basic Time Format Usage

```tsx
// Default 12-hour format with AM/PM
<DateTimePickerComponent timeFormat="hh:mm a" />

// 24-hour format
<DateTimePickerComponent timeFormat="HH:mm" />

// With seconds
<DateTimePickerComponent timeFormat="HH:mm:ss" />

// 12-hour with seconds
<DateTimePickerComponent timeFormat="hh:mm:ss a" />
```

### Time Format Tokens

- `hh` - Hour (01-12)
- `HH` - Hour (00-23)
- `mm` - Minutes (00-59)
- `ss` - Seconds (00-59)
- `a` / `A` - AM/PM
- `tt` - AM/PM

### Combined: Format + TimeFormat

```tsx
<DateTimePickerComponent
  format="dd/MM/yyyy hh:mm a"      // Overall format for the input
  timeFormat="hh:mm a"              // Time list display format
  step={30}
/>
```

### Example: Showing Seconds in Time List

```tsx
<DateTimePickerComponent
  timeFormat="HH:mm:ss"
  step={10}  // Every 10 seconds
/>
```

## Min and Max Time Constraints

Restrict the time range available in the time picker popup.

### Basic Min/Max Time

```tsx
// Available times: 9 AM to 5 PM
<DateTimePickerComponent
  minTime={new Date(0, 0, 0, 9, 0)}   // 9:00 AM
  maxTime={new Date(0, 0, 0, 17, 0)}  // 5:00 PM
  step={30}
/>
```

**Note:** Only the time portion (hours, minutes, seconds) of the Date object is used.

### Example: Business Hours

```tsx
<DateTimePickerComponent
  minTime={new Date(0, 0, 0, 8, 0)}    // 8 AM
  maxTime={new Date(0, 0, 0, 18, 0)}   // 6 PM
  placeholder="Business hours: 8 AM - 6 PM"
/>
```

### Example: Evening Appointments

```tsx
<DateTimePickerComponent
  minTime={new Date(0, 0, 0, 17, 0)}   // 5 PM
  maxTime={new Date(0, 0, 0, 21, 0)}   // 9 PM
  placeholder="Evening slots: 5 PM - 9 PM"
/>
```

### Conditional Time Constraints

```tsx
import React, { useState } from 'react';
import { DateTimePickerComponent } from '@syncfusion/ej2-react-calendars';

export default function ConditionalTimeConstraints() {
  const [appointmentType, setAppointmentType] = useState('standard');

  const getTimeConstraints = () => {
    switch (appointmentType) {
      case 'emergency':
        // 24 hours
        return { minTime: null, maxTime: null };
      case 'evening':
        return {
          minTime: new Date(0, 0, 0, 17, 0),
          maxTime: new Date(0, 0, 0, 21, 0),
        };
      case 'standard':
      default:
        return {
          minTime: new Date(0, 0, 0, 9, 0),
          maxTime: new Date(0, 0, 0, 17, 0),
        };
    }
  };

  const { minTime, maxTime } = getTimeConstraints();

  return (
    <div>
      <label>
        Appointment Type:
        <select value={appointmentType} onChange={(e) => setAppointmentType(e.target.value)}>
          <option value="standard">Standard (9 AM - 5 PM)</option>
          <option value="evening">Evening (5 PM - 9 PM)</option>
          <option value="emergency">Emergency (24 hours)</option>
        </select>
      </label>

      <DateTimePickerComponent
        minTime={minTime}
        maxTime={maxTime}
        step={30}
        placeholder="Select time"
      />
    </div>
  );
}
```

## Scroll Position

The `scrollTo` property sets the initial scroll position of the time list when no value is selected or the value is outside the list.

### Basic scrollTo Usage

```tsx
// Scroll to 2 PM when time picker opens
<DateTimePickerComponent
  scrollTo={new Date(0, 0, 0, 14, 0)}
  placeholder="Default scroll at 2 PM"
/>
```

### Example: Always Show Lunch Hour First

```tsx
<DateTimePickerComponent
  scrollTo={new Date(0, 0, 0, 12, 0)}  // Noon
  minTime={new Date(0, 0, 0, 9, 0)}
  maxTime={new Date(0, 0, 0, 18, 0)}
  step={30}
/>
```

### Example: Scroll to Current Hour

```tsx
import React, { useState, useEffect } from 'react';
import { DateTimePickerComponent } from '@syncfusion/ej2-react-calendars';

export default function ScrollToNowExample() {
  const [scrollTime, setScrollTime] = useState<Date | null>(null);

  useEffect(() => {
    const now = new Date();
    const scrollTo = new Date(0, 0, 0, now.getHours(), 0);
    setScrollTime(scrollTo);
  }, []);

  return (
    <DateTimePickerComponent
      scrollTo={scrollTime}
      placeholder="Scroll to current hour"
    />
  );
}
```

## Time Picker Popup Behavior

### Opening the Popup

```tsx
// Open on icon click (default)
<DateTimePickerComponent />

// Open on input focus
<DateTimePickerComponent openOnFocus={true} />
```

### Controlling Popup Display

```tsx
import React, { useState } from 'react';
import { DateTimePickerComponent } from '@syncfusion/ej2-react-calendars';

export default function PopupControl() {
  const handleOpen = () => {
    console.log('Popup opened');
  };

  const handleClose = () => {
    console.log('Popup closed');
  };

  return (
    <DateTimePickerComponent
      open={handleOpen}
      close={handleClose}
      openOnFocus={true}
    />
  );
}
```

### Full-Screen Mode (Mobile)

```tsx
// On mobile devices, popup takes full screen
<DateTimePickerComponent
  fullScreenMode={true}
  placeholder="Full-screen on mobile"
/>
```

## Examples and Use Cases

### Case 1: Appointment Booking (15-minute slots, 9 AM - 5 PM)

```tsx
import React, { useState } from 'react';
import { DateTimePickerComponent } from '@syncfusion/ej2-react-calendars';

export default function AppointmentBooking() {
  const [appointmentDateTime, setAppointmentDateTime] = useState<Date | null>(null);

  const handleBooking = () => {
    if (appointmentDateTime) {
      const payload = {
        appointmentTime: appointmentDateTime.toISOString(),
      };
      console.log('Booking:', payload);
      // Send to API
    }
  };

  return (
    <div style={{ padding: '20px' }}>
      <h3>Book an Appointment</h3>
      <DateTimePickerComponent
        value={appointmentDateTime}
        change={(e) => setAppointmentDateTime((e as any).value)}
        min={new Date()}
        minTime={new Date(0, 0, 0, 9, 0)}
        maxTime={new Date(0, 0, 0, 17, 0)}
        step={15}
        format="MM/dd/yyyy hh:mm a"
        timeFormat="hh:mm a"
        placeholder="Select appointment date and time"
      />
      <button onClick={handleBooking} disabled={!appointmentDateTime}>
        Confirm Booking
      </button>
    </div>
  );
}
```

### Case 2: Server Maintenance Window (Hourly slots, evenings only)

```tsx
<DateTimePickerComponent
  minTime={new Date(0, 0, 0, 18, 0)}
  maxTime={new Date(0, 0, 0, 23, 59)}
  step={60}
  scrollTo={new Date(0, 0, 0, 20, 0)}
  format="MM/dd/yyyy HH:mm"
  placeholder="Select maintenance window"
/>
```

### Case 3: Medical Clinic (10-minute appointments, no lunch break)

```tsx
<DateTimePickerComponent
  min={new Date()}
  minTime={new Date(0, 0, 0, 8, 0)}   // 8 AM
  maxTime={new Date(0, 0, 0, 17, 0)}  // 5 PM
  step={10}
  format="dd/MM/yyyy hh:mm a"
  timeFormat="hh:mm a"
  placeholder="Available: 8 AM - 5 PM (10 min slots)"
/>
```

### Case 4: Flexible Scheduling with Dynamic Constraints

```tsx
import React, { useState } from 'react';
import { DateTimePickerComponent } from '@syncfusion/ej2-react-calendars';

export default function FlexibleScheduling() {
  const [duration, setDuration] = useState(30); // minutes

  return (
    <div>
      <label>
        Duration:
        <select value={duration} onChange={(e) => setDuration(Number(e.target.value))}>
          <option value={15}>15 minutes</option>
          <option value={30}>30 minutes</option>
          <option value={60}>1 hour</option>
        </select>
      </label>

      <DateTimePickerComponent
        step={duration}
        min={new Date()}
        minTime={new Date(0, 0, 0, 8, 0)}
        maxTime={new Date(0, 0, 0, 17, 0)}
        placeholder={`Select ${duration}-minute slot`}
      />
    </div>
  );
}
```

### Case 5: Time Picker with Seconds (Precise Timing)

```tsx
<DateTimePickerComponent
  step={1}  // 1 second
  timeFormat="HH:mm:ss"
  format="dd/MM/yyyy HH:mm:ss"
  scrollTo={new Date(0, 0, 0, new Date().getHours(), 0, 0)}
  placeholder="Precise timestamp"
/>
```

## Summary

- **step**: Controls time interval granularity (15, 30, 60 minutes common).
- **timeFormat**: Controls how times display in the popup list.
- **minTime/maxTime**: Restrict available time range.
- **scrollTo**: Set initial scroll position in time list.
- **openOnFocus**: Control when popup appears.
- **fullScreenMode**: Enable full-screen on mobile.

