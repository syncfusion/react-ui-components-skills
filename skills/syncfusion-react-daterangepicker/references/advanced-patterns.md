# Advanced Patterns

## Table of Contents
- [Form Integration](#form-integration)
- [Complex Validation](#complex-validation)
- [Multi-Component Coordination](#multi-component-coordination)
- [Performance Optimization](#performance-optimization)
- [Persistence and State Management](#persistence-and-state-management)
- [Timezone Handling](#timezone-handling)
- [Custom Keyboard Shortcuts](#custom-keyboard-shortcuts)
- [Error Handling](#error-handling)
- [Advanced Use Cases](#advanced-use-cases)

## Form Integration

### React Hook Form Integration

```tsx
import { DateRangePickerComponent } from '@syncfusion/ej2-react-calendars';
import { useForm, Controller } from 'react-hook-form';
import * as React from 'react';

interface BookingForm {
  checkIn: [Date, Date] | null;
  checkOut: [Date, Date] | null;
  guestName: string;
  numberOfGuests: number;
}

function ReactHookFormExample() {
  const { control, handleSubmit, formState: { errors } } = useForm<BookingForm>({
    defaultValues: {
      checkIn: [new Date(), new Date(Date.now() + 86400000)],
      guestName: '',
      numberOfGuests: 1,
    },
  });

  const onSubmit = (data: BookingForm) => {
    console.log('Form submitted:', data);
    // Send to server
  };

  return (
    <form onSubmit={handleSubmit(onSubmit)} style={{ maxWidth: '500px', padding: '20px' }}>
      <h3>Hotel Booking Form</h3>

      <div style={{ marginBottom: '20px' }}>
        <label style={{ display: 'block', marginBottom: '8px' }}>
          <strong>Check-in - Check-out Dates</strong>
        </label>
        <Controller
          name="checkIn"
          control={control}
          rules={{
            validate: (value) => {
              if (!value || !value[0] || !value[1]) {
                return 'Please select both dates';
              }
              if (value[0] >= value[1]) {
                return 'Check-in must be before check-out';
              }
              return true;
            },
          }}
          render={({ field }) => (
            <DateRangePickerComponent
              id="daterangepicker"
              placeholder="Select dates"
              startDate={field.value?.[0]}
              endDate={field.value?.[1]}
              change={(e: any) => field.onChange([e.startDate, e.endDate])}
            />
          )}
        />
        {errors.checkIn && (
          <p style={{ color: 'red', fontSize: '12px', marginTop: '5px' }}>
            {errors.checkIn.message}
          </p>
        )}
      </div>

      <div style={{ marginBottom: '20px' }}>
        <label style={{ display: 'block', marginBottom: '8px' }}>
          <strong>Guest Name</strong>
        </label>
        <Controller
          name="guestName"
          control={control}
          rules={{ required: 'Name is required' }}
          render={({ field }) => (
            <input
              {...field}
              type="text"
              placeholder="Enter your name"
              style={{ width: '100%', padding: '8px', boxSizing: 'border-box' }}
            />
          )}
        />
        {errors.guestName && (
          <p style={{ color: 'red', fontSize: '12px', marginTop: '5px' }}>
            {errors.guestName.message}
          </p>
        )}
      </div>

      <div style={{ marginBottom: '20px' }}>
        <label style={{ display: 'block', marginBottom: '8px' }}>
          <strong>Number of Guests</strong>
        </label>
        <Controller
          name="numberOfGuests"
          control={control}
          rules={{
            min: { value: 1, message: 'At least 1 guest required' },
            max: { value: 8, message: 'Maximum 8 guests allowed' },
          }}
          render={({ field }) => (
            <input
              {...field}
              type="number"
              min="1"
              max="8"
              style={{ width: '100%', padding: '8px', boxSizing: 'border-box' }}
            />
          )}
        />
        {errors.numberOfGuests && (
          <p style={{ color: 'red', fontSize: '12px', marginTop: '5px' }}>
            {errors.numberOfGuests.message}
          </p>
        )}
      </div>

      <button
        type="submit"
        style={{
          width: '100%',
          padding: '12px',
          backgroundColor: '#1976d2',
          color: 'white',
          border: 'none',
          borderRadius: '4px',
          cursor: 'pointer',
          fontWeight: 'bold',
        }}
      >
        Book Now
      </button>
    </form>
  );
}

export default ReactHookFormExample;
```

### Formik Integration

```tsx
import { DateRangePickerComponent, ChangedEventArgs } from '@syncfusion/ej2-react-calendars';
import { Formik, Form, Field, ErrorMessage } from 'formik';
import * as Yup from 'yup';
import * as React from 'react';

interface FormValues {
  dateRange: [Date, Date] | null;
  email: string;
}

function FormikExample() {
  const validationSchema = Yup.object().shape({
    dateRange: Yup.array()
      .of(Yup.date())
      .min(2)
      .required('Date range is required')
      .test(
        'date-order',
        'Check-in must be before check-out',
        (value) => !value || !value[0] || !value[1] || value[0] < value[1]
      ),
    email: Yup.string()
      .email('Invalid email')
      .required('Email is required'),
  });

  const handleSubmit = (values: FormValues) => {
    console.log('Formik values:', values);
  };

  return (
    <Formik
      initialValues={{
        dateRange: [new Date(), new Date(Date.now() + 86400000)],
        email: '',
      }}
      validationSchema={validationSchema}
      onSubmit={handleSubmit}
    >
      {({ values, setFieldValue, errors, touched }) => (
        <Form style={{ maxWidth: '500px', padding: '20px' }}>
          <h3>Booking Form (Formik)</h3>

          <div style={{ marginBottom: '20px' }}>
            <label htmlFor="dateRange" style={{ display: 'block', marginBottom: '8px' }}>
              <strong>Travel Dates</strong>
            </label>
            <DateRangePickerComponent
              id="dateRange"
              placeholder="Select dates"
              startDate={values.dateRange?.[0]}
              endDate={values.dateRange?.[1]}
              change={(e: ChangedEventArgs) =>
                setFieldValue('dateRange', [e.startDate, e.endDate])
              }
            />
            <ErrorMessage name="dateRange">
              {(msg) => <p style={{ color: 'red', fontSize: '12px', marginTop: '5px' }}>{msg}</p>}
            </ErrorMessage>
          </div>

          <div style={{ marginBottom: '20px' }}>
            <label htmlFor="email" style={{ display: 'block', marginBottom: '8px' }}>
              <strong>Email</strong>
            </label>
            <Field
              as="input"
              id="email"
              name="email"
              type="email"
              placeholder="your@email.com"
              style={{
                width: '100%',
                padding: '8px',
                boxSizing: 'border-box',
                borderColor: touched.email && errors.email ? 'red' : '#ccc',
              }}
            />
            <ErrorMessage name="email">
              {(msg) => <p style={{ color: 'red', fontSize: '12px', marginTop: '5px' }}>{msg}</p>}
            </ErrorMessage>
          </div>

          <button
            type="submit"
            style={{
              width: '100%',
              padding: '12px',
              backgroundColor: '#1976d2',
              color: 'white',
              border: 'none',
              borderRadius: '4px',
              cursor: 'pointer',
            }}
          >
            Submit
          </button>
        </Form>
      )}
    </Formik>
  );
}

export default FormikExample;
```

## Complex Validation

### Multi-Rule Validation

```tsx
import { DateRangePickerComponent, ChangedEventArgs } from '@syncfusion/ej2-react-calendars';
import * as React from 'react';

interface ValidationRule {
  name: string;
  validate: (start: Date, end: Date) => boolean;
  message: string;
}

function ComplexValidationExample() {
  const [range, setRange] = React.useState<{ start: Date | null; end: Date | null }>({
    start: null,
    end: null,
  });

  const [validationResults, setValidationResults] = React.useState<
    Array<{ rule: string; passed: boolean; message: string }>
  >([]);

  const validationRules: ValidationRule[] = [
    {
      name: 'Start before end',
      validate: (start, end) => start < end,
      message: 'Start date must be before end date',
    },
    {
      name: 'Not in past',
      validate: (start) => start >= new Date(new Date().setHours(0, 0, 0, 0)),
      message: 'Start date cannot be in the past',
    },
    {
      name: 'Within 90 days',
      validate: (start, end) =>
        (end.getTime() - start.getTime()) / (1000 * 60 * 60 * 24) <= 90,
      message: 'Range cannot exceed 90 days',
    },
    {
      name: 'Business days only',
      validate: (start, end) => {
        const startDay = start.getDay();
        const endDay = end.getDay();
        // 0 = Sunday, 6 = Saturday
        return (startDay !== 0 && startDay !== 6) || (endDay !== 0 && endDay !== 6);
      },
      message: 'Must include at least one business day',
    },
    {
      name: 'Minimum 2 days',
      validate: (start, end) =>
        (end.getTime() - start.getTime()) / (1000 * 60 * 60 * 24) >= 2,
      message: 'Range must be at least 2 days',
    },
  ];

  const handleDateChange = (args: ChangedEventArgs) => {
    if (!args.startDate || !args.endDate) return;

    const start = new Date(args.startDate);
    const end = new Date(args.endDate);

    const results = validationRules.map((rule) => ({
      rule: rule.name,
      passed: rule.validate(start, end),
      message: rule.message,
    }));

    setValidationResults(results);

    const allPassed = results.every((r) => r.passed);
    if (allPassed) {
      setRange({ start, end });
    }
  };

  const allValid = validationResults.length > 0 && validationResults.every((r) => r.passed);

  return (
    <div style={{ padding: '20px', maxWidth: '600px' }}>
      <h3>Complex Validation Example</h3>

      <DateRangePickerComponent
        id="daterangepicker"
        placeholder="Select date range"
        change={handleDateChange}
      />

      {validationResults.length > 0 && (
        <div
          style={{
            marginTop: '20px',
            padding: '15px',
            backgroundColor: allValid ? '#e8f5e9' : '#ffebee',
            borderRadius: '4px',
            border: `2px solid ${allValid ? '#81c784' : '#ef5350'}`,
          }}
        >
          <h4 style={{ margin: '0 0 10px 0' }}>
            {allValid ? '✅ Validation Passed' : '❌ Validation Failed'}
          </h4>
          <ul style={{ margin: 0, paddingLeft: '20px' }}>
            {validationResults.map((result) => (
              <li
                key={result.rule}
                style={{
                  marginBottom: '5px',
                  color: result.passed ? '#2e7d32' : '#c62828',
                }}
              >
                {result.passed ? '✓' : '✗'} {result.message}
              </li>
            ))}
          </ul>
        </div>
      )}
    </div>
  );
}

export default ComplexValidationExample;
```

## Multi-Component Coordination

### Linked DateRangePickers

```tsx
import { DateRangePickerComponent, ChangedEventArgs } from '@syncfusion/ej2-react-calendars';
import * as React from 'react';

function LinkedDateRangePickersExample() {
  const [dateRanges, setDateRanges] = React.useState<{
    trip1: { start: Date | null; end: Date | null };
    trip2: { start: Date | null; end: Date | null };
  }>({
    trip1: { start: null, end: null },
    trip2: { start: null, end: null },
  });

  const handleTrip1Change = (args: ChangedEventArgs) => {
    setDateRanges((prev) => ({
      ...prev,
      trip1: { start: args.startDate, end: args.endDate },
    }));
  };

  const handleTrip2Change = (args: ChangedEventArgs) => {
    setDateRanges((prev) => ({
      ...prev,
      trip2: { start: args.startDate, end: args.endDate },
    }));
  };

  // Ensure Trip 2 starts after Trip 1 ends
  const trip2MinDate = dateRanges.trip1.end
    ? new Date(dateRanges.trip1.end.getTime() + 86400000)
    : new Date();

  const calculateBreakDays = () => {
    if (dateRanges.trip1.end && dateRanges.trip2.start) {
      return Math.ceil(
        (dateRanges.trip2.start.getTime() - dateRanges.trip1.end.getTime()) / (1000 * 60 * 60 * 24)
      );
    }
    return 0;
  };

  const calculateTotalDays = () => {
    let total = 0;
    if (dateRanges.trip1.start && dateRanges.trip1.end) {
      total += Math.ceil(
        (dateRanges.trip1.end.getTime() - dateRanges.trip1.start.getTime()) / (1000 * 60 * 60 * 24)
      ) + 1;
    }
    if (dateRanges.trip2.start && dateRanges.trip2.end) {
      total += Math.ceil(
        (dateRanges.trip2.end.getTime() - dateRanges.trip2.start.getTime()) / (1000 * 60 * 60 * 24)
      ) + 1;
    }
    return total;
  };

  return (
    <div style={{ padding: '20px', maxWidth: '700px' }}>
      <h3>Multi-Trip Planner</h3>

      <div
        style={{
          display: 'grid',
          gridTemplateColumns: '1fr 1fr',
          gap: '20px',
          marginBottom: '20px',
        }}
      >
        <div>
          <h4>🏖️ Trip 1</h4>
          <DateRangePickerComponent
            id="trip1-dates"
            placeholder="Trip 1 dates"
            startDate={dateRanges.trip1.start}
            endDate={dateRanges.trip1.end}
            change={handleTrip1Change}
          />
          {dateRanges.trip1.start && dateRanges.trip1.end && (
            <p style={{ fontSize: '12px', marginTop: '8px' }}>
              Duration: {
                Math.ceil(
                  (dateRanges.trip1.end.getTime() - dateRanges.trip1.start.getTime()) /
                  (1000 * 60 * 60 * 24)
                ) + 1
              }{' '}
              days
            </p>
          )}
        </div>

        <div>
          <h4>🏝️ Trip 2</h4>
          <DateRangePickerComponent
            id="trip2-dates"
            placeholder="Trip 2 dates"
            min={trip2MinDate}
            startDate={dateRanges.trip2.start}
            endDate={dateRanges.trip2.end}
            change={handleTrip2Change}
          />
          {dateRanges.trip2.start && dateRanges.trip2.end && (
            <p style={{ fontSize: '12px', marginTop: '8px' }}>
              Duration: {
                Math.ceil(
                  (dateRanges.trip2.end.getTime() - dateRanges.trip2.start.getTime()) /
                  (1000 * 60 * 60 * 24)
                ) + 1
              }{' '}
              days
            </p>
          )}
        </div>
      </div>

      {dateRanges.trip1.end && dateRanges.trip2.start && (
        <div style={{ padding: '15px', backgroundColor: '#f5f5f5', borderRadius: '4px' }}>
          <h4 style={{ margin: '0 0 10px 0' }}>Summary</h4>
          <p>
            <strong>Break between trips:</strong> {calculateBreakDays()} days
          </p>
          <p>
            <strong>Total vacation days:</strong> {calculateTotalDays()} days
          </p>
        </div>
      )}
    </div>
  );
}

export default LinkedDateRangePickersExample;
```

## Performance Optimization

### Lazy Loading with Virtual Scrolling

```tsx
import { DateRangePickerComponent } from '@syncfusion/ej2-react-calendars';
import * as React from 'react';

function PerformanceOptimizedExample() {
  const [dateRange, setDateRange] = React.useState<{
    start: Date | null;
    end: Date | null;
  }>({ start: null, end: null });

  const [renderCount, setRenderCount] = React.useState<number>(0);

  // Memoize the component to avoid unnecessary re-renders
  const MemoizedDateRangePicker = React.useMemo(
    () => (
      <DateRangePickerComponent
        id="daterangepicker"
        placeholder="Select date range"
        startDate={dateRange.start}
        endDate={dateRange.end}
        change={(e: any) => {
          setDateRange({ start: e.startDate, end: e.endDate });
          setRenderCount((prev) => prev + 1);
        }}
      />
    ),
    [dateRange]
  );

  return (
    <div style={{ padding: '20px' }}>
      <h3>Performance Optimized</h3>
      {MemoizedDateRangePicker}
      <p style={{ fontSize: '12px', color: '#666', marginTop: '10px' }}>
        Component renders: {renderCount}
      </p>
    </div>
  );
}

export default PerformanceOptimizedExample;
```

## Persistence and State Management

### LocalStorage Persistence

```tsx
import { DateRangePickerComponent, ChangedEventArgs } from '@syncfusion/ej2-react-calendars';
import * as React from 'react';

function LocalStoragePersistenceExample() {
  const STORAGE_KEY = 'dateRangeSelection';

  const [dateRange, setDateRange] = React.useState<{
    start: Date | null;
    end: Date | null;
  }>(() => {
    const saved = localStorage.getItem(STORAGE_KEY);
    if (saved) {
      try {
        const { start, end } = JSON.parse(saved);
        return {
          start: start ? new Date(start) : null,
          end: end ? new Date(end) : null,
        };
      } catch {
        return { start: null, end: null };
      }
    }
    return { start: null, end: null };
  });

  const handleDateChange = (args: ChangedEventArgs) => {
    const newRange = {
      start: args.startDate,
      end: args.endDate,
    };
    setDateRange(newRange);

    // Persist to localStorage
    localStorage.setItem(
      STORAGE_KEY,
      JSON.stringify({
        start: newRange.start?.toISOString(),
        end: newRange.end?.toISOString(),
      })
    );
  };

  const handleClear = () => {
    setDateRange({ start: null, end: null });
    localStorage.removeItem(STORAGE_KEY);
  };

  return (
    <div style={{ padding: '20px', maxWidth: '500px' }}>
      <h3>Persistent Date Range</h3>

      <DateRangePickerComponent
        id="daterangepicker"
        placeholder="Select date range (auto-saved)"
        startDate={dateRange.start}
        endDate={dateRange.end}
        change={handleDateChange}
      />

      {(dateRange.start || dateRange.end) && (
        <div style={{
          marginTop: '20px',
          padding: '10px',
          backgroundColor: '#f5f5f5',
          borderRadius: '4px',
        }}>
          <p>
            <strong>Saved:</strong> {dateRange.start?.toLocaleDateString()} - {dateRange.end?.toLocaleDateString()}
          </p>
          <button onClick={handleClear} style={{ padding: '5px 10px', marginTop: '10px' }}>
            Clear Saved
          </button>
        </div>
      )}
    </div>
  );
}

export default LocalStoragePersistenceExample;
```

## Timezone Handling

### Server Timezone Offset

```tsx
import { DateRangePickerComponent } from '@syncfusion/ej2-react-calendars';
import * as React from 'react';

function TimezoneHandlingExample() {
  const [timezone, setTimezone] = React.useState<number>(0);

  // Get current user timezone offset in minutes
  const userTimezoneOffset = new Date().getTimezoneOffset();

  // List of common timezones
  const timezones = [
    { label: 'UTC', value: 0 },
    { label: 'EST (UTC-5)', value: 300 },
    { label: 'CST (UTC-6)', value: 360 },
    { label: 'PST (UTC-8)', value: 480 },
    { label: 'IST (UTC+5:30)', value: -330 },
    { label: 'JST (UTC+9)', value: -540 },
  ];

  const convertToServerTimezone = (date: Date, offsetMinutes: number): Date => {
    const utcTime = date.getTime() + userTimezoneOffset * 60000;
    return new Date(utcTime - offsetMinutes * 60000);
  };

  return (
    <div style={{ padding: '20px', maxWidth: '500px' }}>
      <h3>Timezone-Aware Date Range</h3>

      <div style={{ marginBottom: '15px' }}>
        <label style={{ display: 'block', marginBottom: '8px' }}>
          <strong>Server Timezone:</strong>
        </label>
        <select
          value={timezone}
          onChange={(e) => setTimezone(Number(e.target.value))}
          style={{ padding: '8px', fontSize: '14px' }}
        >
          {timezones.map((tz) => (
            <option key={tz.value} value={tz.value}>
              {tz.label}
            </option>
          ))}
        </select>
      </div>

      <DateRangePickerComponent
        id="daterangepicker"
        placeholder="Select date range"
        serverTimezoneOffset={timezone}
      />

      <p style={{ marginTop: '15px', fontSize: '12px', color: '#666' }}>
        User Timezone Offset: {userTimezoneOffset} minutes
        <br />
        Server Timezone Offset: {timezone} minutes
      </p>
    </div>
  );
}

export default TimezoneHandlingExample;
```

## Custom Keyboard Shortcuts

### Extended Keyboard Navigation

```tsx
import { DateRangePickerComponent as DRP } from '@syncfusion/ej2-react-calendars';
import * as React from 'react';

function CustomKeyboardShortcutsExample() {
  const dateRangeRef = React.useRef<DRP>(null);
  const [shortcutLog, setShortcutLog] = React.useState<string[]>([]);

  const handleKeyDown = (e: React.KeyboardEvent<HTMLDivElement>) => {
    if (e.ctrlKey || e.metaKey) {
      switch (e.key.toLowerCase()) {
        case 't':
          // Ctrl+T: Select today
          e.preventDefault();
          const today = new Date();
          if (dateRangeRef.current) {
            dateRangeRef.current.value = [today, today];
            setShortcutLog((prev) => [...prev.slice(-4), 'Ctrl+T: Today selected']);
          }
          break;
        case 'w':
          // Ctrl+W: Select this week
          e.preventDefault();
          const startOfWeek = new Date();
          startOfWeek.setDate(startOfWeek.getDate() - startOfWeek.getDay());
          const endOfWeek = new Date(startOfWeek);
          endOfWeek.setDate(startOfWeek.getDate() + 6);
          if (dateRangeRef.current) {
            dateRangeRef.current.value = [startOfWeek, endOfWeek];
            setShortcutLog((prev) => [...prev.slice(-4), 'Ctrl+W: This week selected']);
          }
          break;
        case 'm':
          // Ctrl+M: Select this month
          e.preventDefault();
          const startOfMonth = new Date(new Date().getFullYear(), new Date().getMonth(), 1);
          const endOfMonth = new Date(
            new Date().getFullYear(),
            new Date().getMonth() + 1,
            0
          );
          if (dateRangeRef.current) {
            dateRangeRef.current.value = [startOfMonth, endOfMonth];
            setShortcutLog((prev) => [...prev.slice(-4), 'Ctrl+M: This month selected']);
          }
          break;
        case 'r':
          // Ctrl+R: Reset
          e.preventDefault();
          if (dateRangeRef.current) {
            dateRangeRef.current.value = null;
            setShortcutLog((prev) => [...prev.slice(-4), 'Ctrl+R: Reset']);
          }
          break;
      }
    }
  };

  return (
    <div
      style={{ padding: '20px', maxWidth: '600px' }}
      onKeyDown={handleKeyDown}
      tabIndex={0}
    >
      <h3>Custom Keyboard Shortcuts</h3>
      <p style={{ fontSize: '12px', color: '#666', marginBottom: '15px' }}>
        Try: Ctrl+T (Today), Ctrl+W (Week), Ctrl+M (Month), Ctrl+R (Reset)
      </p>

      <DateRangePickerComponent
        ref={dateRangeRef}
        id="daterangepicker"
        placeholder="Select date range"
      />

      {shortcutLog.length > 0 && (
        <div style={{
          marginTop: '20px',
          padding: '10px',
          border: '1px solid #ccc',
          borderRadius: '4px',
        }}>
          <h4 style={{ margin: '0 0 10px 0' }}>Shortcut Log:</h4>
          <ul style={{ margin: 0, paddingLeft: '20px', fontSize: '12px' }}>
            {shortcutLog.map((log, idx) => (
              <li key={idx}>{log}</li>
            ))}
          </ul>
        </div>
      )}
    </div>
  );
}

export default CustomKeyboardShortcutsExample;
```

## Error Handling

### Comprehensive Error Handling

```tsx
import { DateRangePickerComponent, ChangedEventArgs } from '@syncfusion/ej2-react-calendars';
import * as React from 'react';

interface DateRangeError {
  type: 'validation' | 'parsing' | 'state' | 'unknown';
  message: string;
  timestamp: Date;
  severity: 'error' | 'warning' | 'info';
}

function ErrorHandlingExample() {
  const [errors, setErrors] = React.useState<DateRangeError[]>([]);

  const addError = (error: Omit<DateRangeError, 'timestamp'>) => {
    setErrors((prev) => [...prev, { ...error, timestamp: new Date() }].slice(-5));
  };

  const handleDateChange = (args: ChangedEventArgs) => {
    try {
      // Validation error check
      if (!args.startDate || !args.endDate) {
        addError({
          type: 'validation',
          message: 'Both start and end dates are required',
          severity: 'warning',
        });
        return;
      }

      // Type checking
      if (!(args.startDate instanceof Date) || !(args.endDate instanceof Date)) {
        addError({
          type: 'parsing',
          message: 'Invalid date format received',
          severity: 'error',
        });
        return;
      }

      // Logic validation
      if (args.startDate >= args.endDate) {
        addError({
          type: 'validation',
          message: 'Start date must be before end date',
          severity: 'error',
        });
        return;
      }

      // Success
      addError({
        type: 'info',
        message: `Valid range: ${args.startDate.toLocaleDateString()} to ${args.endDate.toLocaleDateString()}`,
        severity: 'info',
      });
    } catch (err) {
      addError({
        type: 'unknown',
        message: `Unexpected error: ${err instanceof Error ? err.message : String(err)}`,
        severity: 'error',
      });
    }
  };

  return (
    <div style={{ padding: '20px', maxWidth: '600px' }}>
      <h3>Error Handling Example</h3>

      <DateRangePickerComponent
        id="daterangepicker"
        placeholder="Select date range"
        change={handleDateChange}
      />

      {errors.length > 0 && (
        <div style={{ marginTop: '20px' }}>
          <h4>Error Log:</h4>
          <div style={{
            maxHeight: '300px',
            overflowY: 'auto',
            border: '1px solid #ccc',
            borderRadius: '4px',
          }}>
            {errors.map((error, idx) => {
              const bgColor =
                error.severity === 'error' ? '#ffebee' :
                error.severity === 'warning' ? '#fff3e0' :
                '#e8f5e9';

              const textColor =
                error.severity === 'error' ? '#c62828' :
                error.severity === 'warning' ? '#e65100' :
                '#2e7d32';

              return (
                <div
                  key={idx}
                  style={{
                    padding: '10px',
                    borderBottom: '1px solid #f0f0f0',
                    backgroundColor: bgColor,
                    color: textColor,
                    fontSize: '12px',
                  }}
                >
                  <strong>[{error.type.toUpperCase()}]</strong> {error.message}
                  <br />
                  <span style={{ fontSize: '10px', opacity: 0.7 }}>
                    {error.timestamp.toLocaleTimeString()}
                  </span>
                </div>
              );
            })}
          </div>
        </div>
      )}
    </div>
  );
}

export default ErrorHandlingExample;
```

## Advanced Use Cases

### Dynamic Range Presets Based on Business Logic

```tsx
import { DateRangePickerComponent } from '@syncfusion/ej2-react-calendars';
import * as React from 'react';

interface FiscalYear {
  year: number;
  startMonth: number;
  startDate: number;
}

function FiscalYearRangeExample() {
  const [selectedFiscalYear, setSelectedFiscalYear] = React.useState<number>(
    new Date().getFullYear()
  );

  // Define fiscal year configuration (e.g., US fiscal year: Oct-Sep)
  const fiscalYearConfig: FiscalYear = {
    year: selectedFiscalYear,
    startMonth: 9, // October (0-indexed)
    startDate: 1,
  };

  const calculateFiscalYearDates = (config: FiscalYear) => {
    const startDate = new Date(config.year, config.startMonth, config.startDate);
    const endDate = new Date(config.year + 1, config.startMonth, 0); // Last day of previous month

    return { startDate, endDate };
  };

  const { startDate, endDate } = calculateFiscalYearDates(fiscalYearConfig);

  const fiscalYearOptions = Array.from({ length: 5 }, (_, i) => {
    const year = new Date().getFullYear() - 2 + i;
    return year;
  });

  return (
    <div style={{ padding: '20px', maxWidth: '500px' }}>
      <h3>Fiscal Year Range Selector</h3>

      <div style={{ marginBottom: '15px' }}>
        <label style={{ display: 'block', marginBottom: '8px' }}>
          <strong>Select Fiscal Year:</strong>
        </label>
        <select
          value={selectedFiscalYear}
          onChange={(e) => setSelectedFiscalYear(Number(e.target.value))}
          style={{ padding: '8px', fontSize: '14px', width: '100%' }}
        >
          {fiscalYearOptions.map((year) => (
            <option key={year} value={year}>
              FY {year}-{(year + 1) % 100}
            </option>
          ))}
        </select>
      </div>

      <DateRangePickerComponent
        id="daterangepicker"
        placeholder="Select date range"
        startDate={startDate}
        endDate={endDate}
        disabled={true}
      />

      <div style={{
        marginTop: '15px',
        padding: '10px',
        backgroundColor: '#f5f5f5',
        borderRadius: '4px',
      }}>
        <p>
          <strong>Fiscal Year {selectedFiscalYear}-{(selectedFiscalYear + 1) % 100}:</strong>
        </p>
        <p>{startDate.toLocaleDateString()} - {endDate.toLocaleDateString()}</p>
      </div>
    </div>
  );
}

export default FiscalYearRangeExample;
```
