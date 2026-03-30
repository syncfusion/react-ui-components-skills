# Advanced Patterns

## Table of Contents
- [Form Submission with Validation](#form-submission-with-validation)
- [Keyboard Shortcuts and KeyConfigs](#keyboard-shortcuts-and-keyconfigs)
- [Server Timezone Offset Handling](#server-timezone-offset-handling)
- [Persistence and localStorage](#persistence-and-localstorage)
- [Multi-Component Integration](#multi-component-integration)
- [Performance Optimization](#performance-optimization)
- [Error Handling Patterns](#error-handling-patterns)
- [Complex Validation Scenarios](#complex-validation-scenarios)

## Form Submission with Validation

### Basic Form with TimePicker

```tsx
import { TimePickerComponent } from '@syncfusion/ej2-react-calendars';
import { ButtonComponent } from '@syncfusion/ej2-react-buttons';
import * as React from 'react';

interface ScheduleFormData {
  startTime: Date | null;
  endTime: Date | null;
  title: string;
  errors: Record<string, string>;
}

function ScheduleFormExample() {
  const [formData, setFormData] = React.useState<ScheduleFormData>({
    startTime: null,
    endTime: null,
    title: '',
    errors: {}
  });

  const validateForm = (): boolean => {
    const errors: Record<string, string> = {};

    if (!formData.title.trim()) {
      errors.title = 'Title is required';
    }

    if (!formData.startTime) {
      errors.startTime = 'Start time is required';
    }

    if (!formData.endTime) {
      errors.endTime = 'End time is required';
    }

    if (formData.startTime && formData.endTime) {
      if (formData.startTime >= formData.endTime) {
        errors.endTime = 'End time must be after start time';
      }
    }

    setFormData(prev => ({ ...prev, errors }));
    return Object.keys(errors).length === 0;
  };

  const handleSubmit = (e: React.FormEvent) => {
    e.preventDefault();

    if (validateForm()) {
      console.log('Form submitted:', {
        title: formData.title,
        startTime: formData.startTime?.toLocaleTimeString(),
        endTime: formData.endTime?.toLocaleTimeString()
      });
      // Submit to API
    }
  };

  return (
    <form onSubmit={handleSubmit} style={{ padding: '20px', maxWidth: '400px' }}>
      <h3>Schedule Meeting</h3>

      <div style={{ marginBottom: '15px' }}>
        <label>Title:</label>
        <input
          type="text"
          value={formData.title}
          onChange={(e) => setFormData(prev => ({ ...prev, title: e.target.value }))}
          placeholder="Meeting title"
          style={{ width: '100%', padding: '8px', marginTop: '5px' }}
        />
        {formData.errors.title && <p style={{ color: 'red', fontSize: '12px' }}>{formData.errors.title}</p>}
      </div>

      <div style={{ marginBottom: '15px' }}>
        <label>Start Time:</label>
        <TimePickerComponent
          value={formData.startTime}
          min={new Date('1/1/2018 8:00 AM')}
          max={new Date('1/1/2018 6:00 PM')}
          change={(e: any) => setFormData(prev => ({ ...prev, startTime: e.value }))}
          placeholder="Select start time"
        />
        {formData.errors.startTime && <p style={{ color: 'red', fontSize: '12px' }}>{formData.errors.startTime}</p>}
      </div>

      <div style={{ marginBottom: '15px' }}>
        <label>End Time:</label>
        <TimePickerComponent
          value={formData.endTime}
          min={formData.startTime || new Date('1/1/2018 8:00 AM')}
          max={new Date('1/1/2018 6:00 PM')}
          change={(e: any) => setFormData(prev => ({ ...prev, endTime: e.value }))}
          placeholder="Select end time"
        />
        {formData.errors.endTime && <p style={{ color: 'red', fontSize: '12px' }}>{formData.errors.endTime}</p>}
      </div>

      <ButtonComponent type="submit" isPrimary={true} style={{ width: '100%' }}>
        Schedule Meeting
      </ButtonComponent>
    </form>
  );
}

export default ScheduleFormExample;
```

## Keyboard Shortcuts and KeyConfigs

### Custom Keyboard Configuration

```tsx
import { TimePickerComponent } from '@syncfusion/ej2-react-calendars';
import * as React from 'react';

function KeyConfigExample() {
  // Custom keyboard shortcuts
  const germanKeyConfigs = {
    enter: 'enter',
    escape: 'escape',
    up: 'uparrow',
    down: 'downarrow',
    home: 'ctrl+home',
    end: 'ctrl+end',
    open: 'alt+downarrow',
    close: 'alt+uparrow',
  };

  return (
    <div style={{ padding: '20px' }}>
      <h3>Custom Keyboard Shortcuts</h3>

      <TimePickerComponent
        value={new Date('1/1/2018 10:00 AM')}
        keyConfigs={germanKeyConfigs}
        placeholder="Select time"
      />

      <div style={{ marginTop: '20px', padding: '10px', backgroundColor: '#f5f5f5' }}>
        <h4>Keyboard Shortcuts:</h4>
        <ul style={{ fontSize: '12px' }}>
          <li><strong>Enter:</strong> Select time</li>
          <li><strong>Escape:</strong> Close popup</li>
          <li><strong>↑ ↓:</strong> Navigate times</li>
          <li><strong>Ctrl+Home:</strong> First time</li>
          <li><strong>Ctrl+End:</strong> Last time</li>
          <li><strong>Alt+↓:</strong> Open popup</li>
          <li><strong>Alt+↑:</strong> Close popup</li>
        </ul>
      </div>
    </div>
  );
}

export default KeyConfigExample;
```

## Server Timezone Offset Handling

### Multi-Timezone Support

```tsx
import { TimePickerComponent } from '@syncfusion/ej2-react-calendars';
import * as React from 'react';

interface TimeZone {
  name: string;
  offset: number;
}

function TimezoneExample() {
  const timeZones: TimeZone[] = [
    { name: 'PST (UTC-8)', offset: -480 },
    { name: 'CST (UTC-6)', offset: -360 },
    { name: 'EST (UTC-5)', offset: -300 },
    { name: 'GMT (UTC+0)', offset: 0 },
    { name: 'CET (UTC+1)', offset: 60 },
    { name: 'IST (UTC+5:30)', offset: 330 },
  ];

  const [selectedTimezone, setSelectedTimezone] = React.useState<TimeZone>(timeZones[2]);
  const [selectedTime, setSelectedTime] = React.useState<Date | null>(new Date('1/1/2018 10:00 AM'));

  const handleTimezoneChange = (tz: TimeZone) => {
    setSelectedTimezone(tz);
    console.log(`Timezone changed to: ${tz.name}`);
  };

  return (
    <div style={{ padding: '20px', maxWidth: '400px' }}>
      <h3>Multi-Timezone Time Picker</h3>

      <div style={{ marginBottom: '15px' }}>
        <label>Select Timezone:</label>
        <select
          value={selectedTimezone.name}
          onChange={(e) => {
            const tz = timeZones.find(t => t.name === e.target.value);
            if (tz) handleTimezoneChange(tz);
          }}
          style={{ width: '100%', padding: '8px', marginTop: '5px' }}
        >
          {timeZones.map(tz => (
            <option key={tz.name} value={tz.name}>{tz.name}</option>
          ))}
        </select>
      </div>

      <div>
        <label>Select Time:</label>
        <TimePickerComponent
          value={selectedTime}
          serverTimezoneOffset={selectedTimezone.offset}
          change={(e: any) => setSelectedTime(e.value)}
          placeholder="Select time"
        />
      </div>

      <div style={{ marginTop: '20px', padding: '10px', backgroundColor: '#e3f2fd' }}>
        <p><strong>Timezone:</strong> {selectedTimezone.name}</p>
        <p><strong>Offset:</strong> {selectedTimezone.offset} minutes</p>
        <p><strong>Time:</strong> {selectedTime?.toLocaleTimeString()}</p>
      </div>
    </div>
  );
}

export default TimezoneExample;
```

## Persistence and localStorage

### Auto-Save to localStorage

```tsx
import { TimePickerComponent } from '@syncfusion/ej2-react-calendars';
import * as React from 'react';

function PersistenceExample() {
  const STORAGE_KEY = 'timepicker_value';
  const [selectedTime, setSelectedTime] = React.useState<Date | null>(() => {
    const saved = localStorage.getItem(STORAGE_KEY);
    return saved ? new Date(saved) : new Date('1/1/2018 10:00 AM');
  });

  const [lastSaved, setLastSaved] = React.useState<string>(() => {
    return localStorage.getItem(`${STORAGE_KEY}_timestamp`) || '';
  });

  const handleChange = (e: any) => {
    const newTime = e.value;
    setSelectedTime(newTime);
    
    // Save to localStorage
    localStorage.setItem(STORAGE_KEY, newTime.toISOString());
    localStorage.setItem(`${STORAGE_KEY}_timestamp`, new Date().toLocaleTimeString());
    setLastSaved(new Date().toLocaleTimeString());
  };

  const handleClearStorage = () => {
    localStorage.removeItem(STORAGE_KEY);
    localStorage.removeItem(`${STORAGE_KEY}_timestamp`);
    setSelectedTime(new Date('1/1/2018 10:00 AM'));
    setLastSaved('');
  };

  return (
    <div style={{ padding: '20px', maxWidth: '400px' }}>
      <h3>Time Persistence with localStorage</h3>

      <TimePickerComponent
        value={selectedTime}
        change={handleChange}
        placeholder="Select time"
      />

      <div style={{ marginTop: '20px', padding: '10px', backgroundColor: '#f5f5f5' }}>
        <p><strong>Current Value:</strong> {selectedTime?.toLocaleTimeString()}</p>
        <p><strong>Last Saved:</strong> {lastSaved || 'Never'}</p>
        <button onClick={handleClearStorage} style={{ marginTop: '10px' }}>
          Clear Storage
        </button>
      </div>

      <p style={{ marginTop: '15px', fontSize: '12px', color: '#666' }}>
        Time is automatically saved to browser localStorage and restored on page refresh
      </p>
    </div>
  );
}

export default PersistenceExample;
```

## Multi-Component Integration

### TimePicker with DatePicker and Duration Calculator

```tsx
import { TimePickerComponent } from '@syncfusion/ej2-react-calendars';
import * as React from 'react';

function EventSchedulerExample() {
  const [eventData, setEventData] = React.useState({
    date: new Date('2024-03-15'),
    startTime: new Date('1/1/2018 2:00 PM'),
    endTime: new Date('1/1/2018 3:00 PM'),
  });

  const calculateDuration = (): number => {
    return Math.round((eventData.endTime.getTime() - eventData.startTime.getTime()) / 60000);
  };

  const handleStartTimeChange = (e: any) => {
    setEventData(prev => ({
      ...prev,
      startTime: e.value,
      // Auto-set end time to 1 hour later
      endTime: new Date(e.value.getTime() + 60 * 60 * 1000)
    }));
  };

  return (
    <div style={{ padding: '20px', maxWidth: '500px' }}>
      <h3>Event Scheduler</h3>

      <div style={{ marginBottom: '15px' }}>
        <label>Event Date:</label>
        <input
          type="date"
          value={eventData.date.toISOString().split('T')[0]}
          onChange={(e) => setEventData(prev => ({
            ...prev,
            date: new Date(e.target.value)
          }))}
          style={{ width: '100%', padding: '8px', marginTop: '5px' }}
        />
      </div>

      <div style={{ marginBottom: '15px' }}>
        <label>Start Time:</label>
        <TimePickerComponent
          value={eventData.startTime}
          format="hh:mm a"
          change={handleStartTimeChange}
          placeholder="Select start time"
        />
      </div>

      <div style={{ marginBottom: '15px' }}>
        <label>End Time:</label>
        <TimePickerComponent
          value={eventData.endTime}
          min={eventData.startTime}
          format="hh:mm a"
          change={(e: any) => setEventData(prev => ({ ...prev, endTime: e.value }))}
          placeholder="Select end time"
        />
      </div>

      <div style={{ padding: '10px', backgroundColor: '#e8f5e9' }}>
        <p><strong>Date:</strong> {eventData.date.toLocaleDateString()}</p>
        <p><strong>Duration:</strong> {calculateDuration()} minutes</p>
        <p><strong>Time Range:</strong> {eventData.startTime.toLocaleTimeString()} - {eventData.endTime.toLocaleTimeString()}</p>
      </div>
    </div>
  );
}

export default EventSchedulerExample;
```

## Performance Optimization

### Lazy Loading with Memoization

```tsx
import { TimePickerComponent } from '@syncfusion/ej2-react-calendars';
import * as React from 'react';

interface TimeSlotProps {
  onTimeSelect: (time: Date) => void;
}

// Memoized component to prevent unnecessary re-renders
const TimePicker = React.memo(({ onTimeSelect }: TimeSlotProps) => {
  const [selectedTime, setSelectedTime] = React.useState<Date | null>(new Date('1/1/2018 10:00 AM'));

  const handleChange = (e: any) => {
    setSelectedTime(e.value);
    onTimeSelect(e.value);
  };

  return (
    <TimePickerComponent
      value={selectedTime}
      change={handleChange}
      placeholder="Select time"
    />
  );
});

TimePicker.displayName = 'TimePicker';

function PerformanceExample() {
  const [selectedTimes, setSelectedTimes] = React.useState<Date[]>([]);

  const handleTimeSelect = React.useCallback((time: Date) => {
    setSelectedTimes(prev => [...prev, time]);
  }, []);

  return (
    <div style={{ padding: '20px' }}>
      <h3>Performance Optimized TimePicker</h3>

      <TimePicker onTimeSelect={handleTimeSelect} />

      <div style={{ marginTop: '20px' }}>
        <p><strong>Selected Times Count:</strong> {selectedTimes.length}</p>
      </div>
    </div>
  );
}

export default PerformanceExample;
```

## Error Handling Patterns

### Comprehensive Error Handling

```tsx
import { TimePickerComponent } from '@syncfusion/ej2-react-calendars';
import * as React from 'react';

interface ErrorState {
  hasError: boolean;
  message: string;
  timestamp: Date | null;
}

function ErrorHandlingExample() {
  const [selectedTime, setSelectedTime] = React.useState<Date | null>(new Date('1/1/2018 10:00 AM'));
  const [errorState, setErrorState] = React.useState<ErrorState>({
    hasError: false,
    message: '',
    timestamp: null
  });

  const handleChange = (e: any) => {
    try {
      if (!e.value) {
        throw new Error('No time selected');
      }

      const selectedHour = e.value.getHours();
      if (selectedHour < 9 || selectedHour > 17) {
        throw new Error('Selected time is outside business hours (9 AM - 5 PM)');
      }

      setSelectedTime(e.value);
      setErrorState({ hasError: false, message: '', timestamp: null });
    } catch (error) {
      const message = error instanceof Error ? error.message : 'Unknown error occurred';
      setErrorState({
        hasError: true,
        message,
        timestamp: new Date()
      });
    }
  };

  return (
    <div style={{ padding: '20px', maxWidth: '400px' }}>
      <h3>Error Handling</h3>

      <TimePickerComponent
        value={selectedTime}
        min={new Date('1/1/2018 9:00 AM')}
        max={new Date('1/1/2018 5:00 PM')}
        change={handleChange}
        placeholder="Select time"
      />

      {errorState.hasError && (
        <div style={{
          marginTop: '15px',
          padding: '10px',
          backgroundColor: '#ffebee',
          border: '1px solid #f44336',
          borderRadius: '4px',
          color: '#c62828'
        }}>
          <strong>Error:</strong> {errorState.message}
          <br />
          <small>{errorState.timestamp?.toLocaleTimeString()}</small>
        </div>
      )}

      {!errorState.hasError && selectedTime && (
        <div style={{
          marginTop: '15px',
          padding: '10px',
          backgroundColor: '#e8f5e9',
          border: '1px solid #4caf50',
          borderRadius: '4px',
          color: '#2e7d32'
        }}>
          <strong>Success:</strong> Time selected: {selectedTime.toLocaleTimeString()}
        </div>
      )}
    </div>
  );
}

export default ErrorHandlingExample;
```

## Complex Validation Scenarios

### Multi-Tier Validation System

```tsx
import { TimePickerComponent } from '@syncfusion/ej2-react-calendars';
import { ButtonComponent } from '@syncfusion/ej2-react-buttons';
import * as React from 'react';

interface ValidationRule {
  name: string;
  validate: (time: Date) => boolean;
  message: string;
}

function ComplexValidationExample() {
  const [startTime, setStartTime] = React.useState<Date | null>(new Date('1/1/2018 10:00 AM'));
  const [endTime, setEndTime] = React.useState<Date | null>(new Date('1/1/2018 11:00 AM'));
  const [validationResults, setValidationResults] = React.useState<Record<string, boolean>>({});

  const validationRules: ValidationRule[] = [
    {
      name: 'Business Hours',
      validate: (time: Date) => time.getHours() >= 9 && time.getHours() < 17,
      message: 'Must be within 9 AM - 5 PM'
    },
    {
      name: 'Not Lunch Time',
      validate: (time: Date) => !(time.getHours() === 12 && time.getMinutes() >= 0 && time.getMinutes() < 60),
      message: 'Cannot be during lunch hour (12 PM)'
    },
    {
      name: 'Minimum 30 Minutes',
      validate: (time: Date) => endTime && (endTime.getTime() - time.getTime()) >= 30 * 60 * 1000,
      message: 'Meeting must be at least 30 minutes'
    }
  ];

  const validateTime = (time: Date | null, type: string) => {
    if (!time) return;

    const results: Record<string, boolean> = {};
    validationRules.forEach(rule => {
      results[`${type}_${rule.name}`] = rule.validate(time);
    });

    setValidationResults(prev => ({ ...prev, ...results }));
  };

  const handleStartTimeChange = (e: any) => {
    setStartTime(e.value);
    validateTime(e.value, 'start');
  };

  const handleEndTimeChange = (e: any) => {
    setEndTime(e.value);
    validateTime(e.value, 'end');
  };

  const isFormValid = Object.values(validationResults).every(v => v);

  return (
    <div style={{ padding: '20px', maxWidth: '500px' }}>
      <h3>Complex Multi-Tier Validation</h3>

      <div style={{ marginBottom: '15px' }}>
        <label>Start Time:</label>
        <TimePickerComponent
          value={startTime}
          min={new Date('1/1/2018 9:00 AM')}
          max={new Date('1/1/2018 5:00 PM')}
          change={handleStartTimeChange}
          placeholder="Select start time"
        />
      </div>

      <div style={{ marginBottom: '20px' }}>
        <label>End Time:</label>
        <TimePickerComponent
          value={endTime}
          min={startTime || new Date('1/1/2018 9:00 AM')}
          max={new Date('1/1/2018 5:00 PM')}
          change={handleEndTimeChange}
          placeholder="Select end time"
        />
      </div>

      <div style={{ marginBottom: '20px', padding: '15px', backgroundColor: '#f5f5f5' }}>
        <h4>Validation Results:</h4>
        {validationRules.map(rule => {
          const startValid = validationResults[`start_${rule.name}`] !== false;
          const endValid = validationResults[`end_${rule.name}`] !== false;
          return (
            <div key={rule.name} style={{ marginBottom: '10px' }}>
              <p style={{ margin: '0 0 5px 0' }}>
                <strong>{rule.name}:</strong>
              </p>
              <p style={{ margin: '0 0 5px 5px', color: startValid ? '#4caf50' : '#f44336' }}>
                Start: {startValid ? '✓' : '✗'} {rule.message}
              </p>
              <p style={{ margin: '0 0 5px 5px', color: endValid ? '#4caf50' : '#f44336' }}>
                End: {endValid ? '✓' : '✗'} {rule.message}
              </p>
            </div>
          );
        })}
      </div>

      <ButtonComponent
        isPrimary={true}
        disabled={!isFormValid}
        style={{ width: '100%' }}
      >
        {isFormValid ? 'Schedule Meeting' : 'Validation Errors Exist'}
      </ButtonComponent>
    </div>
  );
}

export default ComplexValidationExample;
```

## Best Practices Summary

1. **Always validate** time ranges before submission
2. **Use memoization** for performance with large lists
3. **Provide clear feedback** for validation errors
4. **Handle timezone** differences for global apps
5. **Persist user preferences** for better UX
6. **Use callbacks** for complex state management
7. **Test edge cases** like DST transitions
8. **Implement proper error boundaries** for error handling

## Related Topics

- [Time Range and Selection](time-range-and-selection.md)
- [Events and Methods](events-and-methods.md)
- [Customization and Styling](customization-and-styling.md)
- [API Reference](api-reference.md)
