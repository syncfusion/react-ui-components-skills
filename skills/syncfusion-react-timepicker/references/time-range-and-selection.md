# Time Range and Selection Configuration

## Table of Contents
- [Minimum and Maximum Time](#minimum-and-maximum-time)
- [Time Step Intervals](#time-step-intervals)
- [ScrollTo Default Position](#scrollto-default-position)
- [Value Binding and Updates](#value-binding-and-updates)
- [Read-Only and Disabled States](#read-only-and-disabled-states)
- [Open on Focus](#open-on-focus)
- [Time Popup List Population](#time-popup-list-population)
- [Practical Examples](#practical-examples)

## Minimum and Maximum Time

Restrict time selection to a specific range using `min` and `max` properties:

### Basic Min/Max Configuration

```tsx
import { TimePickerComponent } from '@syncfusion/ej2-react-calendars';
import * as React from 'react';

function TimeRangeExample() {
  const [selectedTime, setSelectedTime] = React.useState(new Date('1/1/2018 10:00 AM'));

  // Business hours: 9 AM - 5 PM
  const minTime = new Date('1/1/2018 9:00 AM');
  const maxTime = new Date('1/1/2018 5:00 PM');

  return (
    <div style={{ padding: '20px' }}>
      <h3>Business Hours Time Picker</h3>
      <p>Available: 9:00 AM - 5:00 PM</p>

      <TimePickerComponent
        value={selectedTime}
        min={minTime}
        max={maxTime}
        change={(e: any) => setSelectedTime(e.value)}
        placeholder="Select time"
      />

      <p>Selected: {selectedTime?.toLocaleTimeString()}</p>
    </div>
  );
}

export default TimeRangeExample;
```

### Dynamic Min/Max Based on User Input

```tsx
import { TimePickerComponent } from '@syncfusion/ej2-react-calendars';
import * as React from 'react';

function FlexibleTimeRange() {
  const [startTime, setStartTime] = React.useState(new Date('1/1/2018 9:00 AM'));
  const [endTime, setEndTime] = React.useState(new Date('1/1/2018 5:00 PM'));

  return (
    <div style={{ padding: '20px' }}>
      <h3>Meeting Time Range</h3>

      <div>
        <label>Start Time:</label>
        <TimePickerComponent
          value={startTime}
          max={endTime}
          change={(e: any) => setStartTime(e.value)}
        />
      </div>

      <div style={{ marginTop: '15px' }}>
        <label>End Time (must be after start):</label>
        <TimePickerComponent
          value={endTime}
          min={startTime}
          change={(e: any) => setEndTime(e.value)}
        />
      </div>

      <div style={{ marginTop: '15px', padding: '10px', backgroundColor: '#f0f0f0' }}>
        <p>
          Meeting Duration: {Math.round((endTime.getTime() - startTime.getTime()) / 60000)} minutes
        </p>
      </div>
    </div>
  );
}

export default FlexibleTimeRange;
```

## Time Step Intervals

The `step` property defines the interval between time items in the popup list:

### Common Step Values

```tsx
import { TimePickerComponent } from '@syncfusion/ej2-react-calendars';
import * as React from 'react';

function StepIntervalExample() {
  return (
    <div style={{ padding: '20px' }}>
      <h3>Time Step Intervals</h3>

      {/* 15-minute intervals for quick selection */}
      <div>
        <label>15-Minute Intervals (Quick):</label>
        <TimePickerComponent
          value={new Date('1/1/2018 10:00 AM')}
          step={15}
          placeholder="Select time"
        />
      </div>

      {/* 30-minute intervals (default) */}
      <div style={{ marginTop: '15px' }}>
        <label>30-Minute Intervals (Default):</label>
        <TimePickerComponent
          value={new Date('1/1/2018 10:00 AM')}
          step={30}
          placeholder="Select time"
        />
      </div>

      {/* 60-minute intervals for hourly slots */}
      <div style={{ marginTop: '15px' }}>
        <label>Hourly Intervals (60 minutes):</label>
        <TimePickerComponent
          value={new Date('1/1/2018 10:00 AM')}
          step={60}
          placeholder="Select time"
        />
      </div>

      {/* 5-minute intervals for precise timing */}
      <div style={{ marginTop: '15px' }}>
        <label>5-Minute Intervals (Precise):</label>
        <TimePickerComponent
          value={new Date('1/1/2018 10:00 AM')}
          step={5}
          placeholder="Select time"
        />
      </div>
    </div>
  );
}

export default StepIntervalExample;
```

### Choosing the Right Step Value

| Step | Use Case | Example |
|------|----------|---------|
| 5 | Precise timing, medical/scientific | 09:05, 09:10, 09:15 |
| 15 | Appointments, meetings | 09:00, 09:15, 09:30 |
| 30 | Default, general purposes | 09:00, 09:30, 10:00 |
| 60 | Hourly slots, broadcast times | 09:00, 10:00, 11:00 |

## ScrollTo Default Position

When the popup is first opened, the `scrollTo` property determines which time is visible:

```tsx
import { TimePickerComponent } from '@syncfusion/ej2-react-calendars';
import * as React from 'react';

function ScrollToExample() {
  return (
    <div style={{ padding: '20px' }}>
      <h3>ScrollTo Behavior</h3>

      {/* Scroll to 12:00 PM when popup opens */}
      <div>
        <label>Scroll to 12:00 PM:</label>
        <TimePickerComponent
          scrollTo={new Date('1/1/2018 12:00 PM')}
          placeholder="Click to open"
        />
      </div>

      {/* Scroll to 9:00 AM for morning appointments */}
      <div style={{ marginTop: '15px' }}>
        <label>Scroll to 9:00 AM (Business Start):</label>
        <TimePickerComponent
          scrollTo={new Date('1/1/2018 9:00 AM')}
          placeholder="Click to open"
        />
      </div>

      {/* Scroll to current time */}
      <div style={{ marginTop: '15px' }}>
        <label>Scroll to Current Time:</label>
        <TimePickerComponent
          scrollTo={new Date()}
          placeholder="Click to open"
        />
      </div>
    </div>
  );
}

export default ScrollToExample;
```

## Value Binding and Updates

### Two-Way Binding with State

```tsx
import { TimePickerComponent } from '@syncfusion/ej2-react-calendars';
import * as React from 'react';

function ValueBindingExample() {
  const [selectedTime, setSelectedTime] = React.useState<Date | null>(new Date('1/1/2018 10:00 AM'));

  const handleChange = (e: any) => {
    setSelectedTime(e.value);
  };

  const handleClear = () => {
    setSelectedTime(null);
  };

  const handleSet = (hour: number, minute: number) => {
    const newTime = new Date();
    newTime.setHours(hour, minute, 0);
    setSelectedTime(newTime);
  };

  return (
    <div style={{ padding: '20px' }}>
      <h3>Value Management</h3>

      <TimePickerComponent
        value={selectedTime}
        change={handleChange}
        placeholder="Select time"
      />

      <div style={{ marginTop: '20px' }}>
        <h4>Programmatic Control:</h4>
        <button onClick={() => handleSet(9, 0)}>Set 9:00 AM</button>
        <button onClick={() => handleSet(14, 30)}>Set 2:30 PM</button>
        <button onClick={() => handleSet(17, 0)}>Set 5:00 PM</button>
        <button onClick={handleClear}>Clear</button>
      </div>

      <div style={{ marginTop: '20px', padding: '10px', backgroundColor: '#f0f0f0' }}>
        <p>Current Value: {selectedTime?.toLocaleTimeString() || 'No value selected'}</p>
      </div>
    </div>
  );
}

export default ValueBindingExample;
```

### Getting Current Value

```tsx
import { TimePickerComponent } from '@syncfusion/ej2-react-calendars';
import * as React from 'react';

function GetValueExample() {
  const timePickerRef = React.useRef<TimePickerComponent>(null);
  const [displayValue, setDisplayValue] = React.useState('');

  const handleGetValue = () => {
    if (timePickerRef.current?.value) {
      const time = timePickerRef.current.value;
      setDisplayValue(time.toLocaleTimeString());
    } else {
      setDisplayValue('No value selected');
    }
  };

  return (
    <div style={{ padding: '20px' }}>
      <h3>Get Current Value</h3>

      <TimePickerComponent
        ref={timePickerRef}
        value={new Date('1/1/2018 10:00 AM')}
        placeholder="Select time"
      />

      <button onClick={handleGetValue} style={{ marginTop: '10px' }}>
        Get Time Value
      </button>

      <div style={{ marginTop: '15px' }}>
        <p>Selected Time: {displayValue}</p>
      </div>
    </div>
  );
}

export default GetValueExample;
```

## Read-Only and Disabled States

### Read-Only Mode

In read-only mode, users can see the value but cannot change it:

```tsx
import { TimePickerComponent } from '@syncfusion/ej2-react-calendars';
import * as React from 'react';

function ReadOnlyExample() {
  return (
    <div style={{ padding: '20px' }}>
      <h3>Read-Only Time Picker</h3>

      <label>Business Hours (Read-Only):</label>
      <TimePickerComponent
        value={new Date('1/1/2018 9:00 AM')}
        readonly={true}
        placeholder="This is read-only"
      />

      <p style={{ marginTop: '15px', color: '#666' }}>
        Users can see the value but cannot modify it. Popup still opens for viewing.
      </p>
    </div>
  );
}

export default ReadOnlyExample;
```

### Disabled State

In disabled state, the component is grayed out and completely non-interactive:

```tsx
import { TimePickerComponent } from '@syncfusion/ej2-react-calendars';
import * as React from 'react';

function DisabledExample() {
  const [isDisabled, setIsDisabled] = React.useState(false);

  return (
    <div style={{ padding: '20px' }}>
      <h3>Disabled Time Picker</h3>

      <label>
        <input
          type="checkbox"
          checked={isDisabled}
          onChange={(e) => setIsDisabled(e.target.checked)}
        />
        {' '}Disable TimePicker
      </label>

      <div style={{ marginTop: '15px' }}>
        <label>Time (Disabled when above checked):</label>
        <TimePickerComponent
          value={new Date('1/1/2018 10:00 AM')}
          enabled={!isDisabled}
          placeholder="Select time"
        />
      </div>

      <p style={{ marginTop: '15px', color: '#666' }}>
        When disabled, the component is non-interactive and appears grayed out.
      </p>
    </div>
  );
}

export default DisabledExample;
```

### Conditional Enable/Disable

```tsx
import { TimePickerComponent } from '@syncfusion/ej2-react-calendars';
import * as React from 'react';

function ConditionalStateExample() {
  const [useCustomTime, setUseCustomTime] = React.useState(false);
  const [selectedTime, setSelectedTime] = React.useState(new Date('1/1/2018 10:00 AM'));

  return (
    <div style={{ padding: '20px' }}>
      <h3>Conditional Enable/Disable</h3>

      <label>
        <input
          type="checkbox"
          checked={useCustomTime}
          onChange={(e) => setUseCustomTime(e.target.checked)}
        />
        {' '}Use Custom Time
      </label>

      <div style={{ marginTop: '15px' }}>
        <label>Select Time:</label>
        <TimePickerComponent
          value={selectedTime}
          enabled={useCustomTime}
          change={(e: any) => setSelectedTime(e.value)}
          placeholder="Select time"
        />
      </div>

      <p style={{ marginTop: '15px', color: '#666' }}>
        TimePicker is {useCustomTime ? 'enabled' : 'disabled'}. 
        Enable to customize time selection.
      </p>
    </div>
  );
}

export default ConditionalStateExample;
```

## Open on Focus

By default, the popup opens only when clicking the calendar icon. Use `openOnFocus` to open on input focus:

```tsx
import { TimePickerComponent } from '@syncfusion/ej2-react-calendars';
import * as React from 'react';

function OpenOnFocusExample() {
  return (
    <div style={{ padding: '20px' }}>
      <h3>Open on Focus Examples</h3>

      {/* Default: Opens on icon click only */}
      <div>
        <label>Default Behavior (Click icon to open):</label>
        <TimePickerComponent
          value={new Date('1/1/2018 10:00 AM')}
          openOnFocus={false}
          placeholder="Click icon to open"
        />
      </div>

      {/* Open on focus: Opens when input gets focus */}
      <div style={{ marginTop: '15px' }}>
        <label>Open on Focus (Click or focus input):</label>
        <TimePickerComponent
          value={new Date('1/1/2018 10:00 AM')}
          openOnFocus={true}
          placeholder="Focus or click to open"
        />
      </div>

      <p style={{ marginTop: '15px', color: '#666' }}>
        openOnFocus=true improves mobile UX by opening popup automatically
      </p>
    </div>
  );
}

export default OpenOnFocusExample;
```

## Time Popup List Population

The popup displays time values based on min, max, and step properties:

### Example Calculation

```
min: 9:00 AM
max: 5:00 PM
step: 30 minutes

Popup displays:
9:00 AM
9:30 AM
10:00 AM
10:30 AM
... (continues)
4:30 PM
5:00 PM
```

### Practical Popup Scenarios

```tsx
import { TimePickerComponent } from '@syncfusion/ej2-react-calendars';
import * as React from 'react';

function PopupScenarioExample() {
  return (
    <div style={{ padding: '20px' }}>
      <h3>Popup List Scenarios</h3>

      {/* Small range with fine intervals */}
      <div>
        <label>Precise Scheduling (5-min intervals):</label>
        <TimePickerComponent
          min={new Date('1/1/2018 9:00 AM')}
          max={new Date('1/1/2018 12:00 PM')}
          step={5}
          placeholder="9:00 AM - 12:00 PM"
        />
        <small>Popup shows: 9:00, 9:05, 9:10, 9:15... up to 12:00</small>
      </div>

      {/* Hourly slots */}
      <div style={{ marginTop: '20px' }}>
        <label>Hourly Time Slots:</label>
        <TimePickerComponent
          min={new Date('1/1/2018 8:00 AM')}
          max={new Date('1/1/2018 6:00 PM')}
          step={60}
          placeholder="8:00 AM - 6:00 PM hourly"
        />
        <small>Popup shows: 8:00, 9:00, 10:00... up to 6:00</small>
      </div>

      {/* Half-hour slots */}
      <div style={{ marginTop: '20px' }}>
        <label>Half-Hour Time Slots:</label>
        <TimePickerComponent
          min={new Date('1/1/2018 9:00 AM')}
          max={new Date('1/1/2018 5:00 PM')}
          step={30}
          placeholder="9:00 AM - 5:00 PM (30-min)"
        />
        <small>Popup shows: 9:00, 9:30, 10:00, 10:30... up to 5:00</small>
      </div>
    </div>
  );
}

export default PopupScenarioExample;
```

## Practical Examples

### Example 1: Doctor Appointment Booking

```tsx
import { TimePickerComponent } from '@syncfusion/ej2-react-calendars';
import * as React from 'react';

function DoctorAppointment() {
  const [appointmentTime, setAppointmentTime] = React.useState<Date | null>(null);

  return (
    <div style={{ padding: '20px', maxWidth: '400px' }}>
      <h3>Book Doctor Appointment</h3>

      <TimePickerComponent
        value={appointmentTime}
        min={new Date('1/1/2018 9:00 AM')}
        max={new Date('1/1/2018 5:00 PM')}
        step={30}
        change={(e: any) => setAppointmentTime(e.value)}
        placeholder="Select appointment time"
      />

      {appointmentTime && (
        <div style={{ marginTop: '20px', padding: '10px', backgroundColor: '#e8f5e9' }}>
          <strong>Appointment Details:</strong>
          <p>Time: {appointmentTime.toLocaleTimeString('en-US', { hour: '2-digit', minute: '2-digit', hour12: true })}</p>
          <p>Duration: 30 minutes</p>
        </div>
      )}
    </div>
  );
}

export default DoctorAppointment;
```

### Example 2: Flight Check-In Time

```tsx
import { TimePickerComponent } from '@syncfusion/ej2-react-calendars';
import * as React from 'react';

function FlightCheckIn() {
  const [checkInTime, setCheckInTime] = React.useState<Date | null>(new Date('1/1/2018 2:00 PM'));

  const departureTime = new Date('1/1/2018 4:00 PM');
  const maxCheckInTime = new Date('1/1/2018 3:30 PM'); // 30 min before departure

  return (
    <div style={{ padding: '20px', maxWidth: '400px' }}>
      <h3>Flight Check-In Time</h3>

      <div style={{ marginBottom: '15px', padding: '10px', backgroundColor: '#f5f5f5' }}>
        <p><strong>Flight Departure:</strong> {departureTime.toLocaleTimeString()}</p>
        <p><strong>Check-In Closes:</strong> {maxCheckInTime.toLocaleTimeString()}</p>
      </div>

      <TimePickerComponent
        value={checkInTime}
        min={new Date('1/1/2018 12:00 PM')}
        max={maxCheckInTime}
        step={15}
        change={(e: any) => setCheckInTime(e.value)}
        placeholder="Select check-in time"
      />

      {checkInTime && (
        <div style={{ marginTop: '20px', padding: '10px', backgroundColor: '#e3f2fd' }}>
          <p>Check-in at: {checkInTime.toLocaleTimeString()}</p>
          <p>Time until departure: {Math.round((departureTime.getTime() - checkInTime.getTime()) / 60000)} minutes</p>
        </div>
      )}
    </div>
  );
}

export default FlightCheckIn;
```

## Related Topics

- [Time Format and Display](time-format-and-display.md) - Learn about time formatting
- [Events and Methods](events-and-methods.md) - Handle range-related events
- [API Reference](api-reference.md) - Complete properties documentation
