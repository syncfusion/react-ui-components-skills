# Date Range Selection

## Table of Contents
- [Overview](#overview)
- [Start and End Dates](#start-and-end-dates)
- [Date Range Constraints](#date-range-constraints)
- [Disabled Dates](#disabled-dates)
- [Preset Ranges](#preset-ranges)
- [Range Validation](#range-validation)
- [State Management](#state-management)
- [Common Patterns](#common-patterns)

## Overview

Date range selection is the core functionality of DateRangePicker. Users select a start date and an end date to define a time period. The component handles:
- Initial date range values
- Min/max date constraints
- Disabled dates that cannot be selected
- Custom validation logic
- Programmatic value updates

## Start and End Dates

### Basic Date Range Setup

The `startDate` and `endDate` properties define the selected date range:

```tsx
import { DateRangePickerComponent } from '@syncfusion/ej2-react-calendars';
import * as React from 'react';

function App() {
  const [range, setRange] = React.useState<{
    start: Date | null;
    end: Date | null;
  }>({
    start: new Date(2024, 9, 1), // October 1, 2024
    end: new Date(2024, 10, 15),  // November 15, 2024
  });

  return (
    <DateRangePickerComponent
      id="daterangepicker"
      startDate={range.start}
      endDate={range.end}
      change={(e: any) => setRange({ start: e.startDate, end: e.endDate })}
    />
  );
}

export default App;
```

### Reading Selected Dates

```tsx
import { DateRangePickerComponent, ChangedEventArgs } from '@syncfusion/ej2-react-calendars';
import * as React from 'react';

function App() {
  const handleDateChange = (args: ChangedEventArgs) => {
    console.log('Start Date:', args.startDate);
    console.log('End Date:', args.endDate);
    console.log('Selected Value:', args.value); // [startDate, endDate]
  };

  return (
    <DateRangePickerComponent
      id="daterangepicker"
      placeholder="Select date range"
      change={handleDateChange}
    />
  );
}

export default App;
```

## Date Range Constraints

### Minimum and Maximum Dates

Use `min` and `max` properties to restrict the selectable date range:

```tsx
import { DateRangePickerComponent } from '@syncfusion/ej2-react-calendars';
import * as React from 'react';

function TravelBooking() {
  const today = new Date();
  const minDate = new Date(today); // Cannot book in past
  const maxDate = new Date(today);
  maxDate.setMonth(maxDate.getMonth() + 6); // Up to 6 months ahead

  return (
    <div style={{ padding: '20px' }}>
      <h3>Book Your Trip</h3>
      <DateRangePickerComponent
        id="daterangepicker"
        placeholder="Select check-in and check-out"
        min={minDate}
        max={maxDate}
      />
      <p style={{ fontSize: '12px', color: '#666' }}>
        You can book dates from today ({minDate.toLocaleDateString()}) 
        up to 6 months ahead ({maxDate.toLocaleDateString()})
      </p>
    </div>
  );
}

export default TravelBooking;
```

### Range Duration Validation

Validate the number of days between start and end dates:

```tsx
import { DateRangePickerComponent, ChangedEventArgs } from '@syncfusion/ej2-react-calendars';
import * as React from 'react';

function VacationRequest() {
  const [range, setRange] = React.useState<{
    start: Date | null;
    end: Date | null;
  }>({ start: null, end: null });

  const [validationError, setValidationError] = React.useState<string>('');

  const MAX_VACATION_DAYS = 30;
  const MIN_VACATION_DAYS = 1;

  const handleDateChange = (args: ChangedEventArgs) => {
    setValidationError('');

    if (!args.startDate || !args.endDate) {
      return;
    }

    const start = new Date(args.startDate);
    const end = new Date(args.endDate);

    // Ensure start is before end
    if (start > end) {
      setValidationError('Start date must be before end date');
      return;
    }

    // Calculate days
    const daysDifference = Math.ceil(
      (end.getTime() - start.getTime()) / (1000 * 60 * 60 * 24)
    );

    // Validate minimum days
    if (daysDifference < MIN_VACATION_DAYS) {
      setValidationError(`Minimum ${MIN_VACATION_DAYS} day required`);
      return;
    }

    // Validate maximum days
    if (daysDifference > MAX_VACATION_DAYS) {
      setValidationError(`Cannot exceed ${MAX_VACATION_DAYS} days`);
      return;
    }

    setRange({ start, end });
  };

  return (
    <div style={{ padding: '20px', maxWidth: '500px' }}>
      <h3>Request Vacation Days</h3>
      <DateRangePickerComponent
        id="daterangepicker"
        placeholder="Select vacation dates"
        change={handleDateChange}
        startDate={range.start}
        endDate={range.end}
      />

      {validationError && (
        <div style={{
          marginTop: '10px',
          padding: '10px',
          backgroundColor: '#ffebee',
          color: '#c62828',
          borderRadius: '4px',
          border: '1px solid #ef5350'
        }}>
          ⚠️ {validationError}
        </div>
      )}

      {range.start && range.end && !validationError && (
        <div style={{
          marginTop: '10px',
          padding: '10px',
          backgroundColor: '#e8f5e9',
          color: '#2e7d32',
          borderRadius: '4px',
          border: '1px solid #81c784'
        }}>
          ✓ Vacation request valid ({
            Math.ceil((range.end.getTime() - range.start.getTime()) / (1000 * 60 * 60 * 24)) + 1
          } days)
        </div>
      )}
    </div>
  );
}

export default VacationRequest;
```



## Preset Ranges

### Quick Selection Buttons

Provide preset date ranges for common selections:

```tsx
import { DateRangePickerComponent } from '@syncfusion/ej2-react-calendars';
import * as React from 'react';

function AnalyticsDashboard() {
  const [range, setRange] = React.useState<{
    start: Date | null;
    end: Date | null;
  }>({ start: null, end: null });

  const getPresetRanges = () => {
    const today = new Date();

    return {
      'Today': { start: new Date(today), end: new Date(today) },
      'Yesterday': {
        start: new Date(today.getTime() - 86400000),
        end: new Date(today.getTime() - 86400000),
      },
      'Last 7 Days': {
        start: new Date(today.getTime() - 7 * 86400000),
        end: new Date(today),
      },
      'Last 30 Days': {
        start: new Date(today.getTime() - 30 * 86400000),
        end: new Date(today),
      },
      'This Month': {
        start: new Date(today.getFullYear(), today.getMonth(), 1),
        end: new Date(today),
      },
      'Last Month': {
        start: new Date(today.getFullYear(), today.getMonth() - 1, 1),
        end: new Date(today.getFullYear(), today.getMonth(), 0),
      },
      'Last Quarter': {
        start: new Date(today.getFullYear(), today.getMonth() - 3, 1),
        end: new Date(today),
      },
      'Year to Date': {
        start: new Date(today.getFullYear(), 0, 1),
        end: new Date(today),
      },
    };
  };

  const handlePresetClick = (preset: { start: Date; end: Date }) => {
    setRange(preset);
  };

  const presets = getPresetRanges();

  return (
    <div style={{ padding: '20px' }}>
      <h3>Analytics Report</h3>

      <div style={{ marginBottom: '20px' }}>
        <DateRangePickerComponent
          id="daterangepicker"
          placeholder="Or select custom range"
          startDate={range.start}
          endDate={range.end}
          change={(e: any) => setRange({ start: e.startDate, end: e.endDate })}
        />
      </div>

      <div style={{ marginBottom: '20px' }}>
        <h4>Quick Select:</h4>
        <div style={{
          display: 'grid',
          gridTemplateColumns: 'repeat(auto-fit, minmax(120px, 1fr))',
          gap: '10px',
        }}>
          {Object.entries(presets).map(([label, preset]) => (
            <button
              key={label}
              onClick={() => handlePresetClick(preset)}
              style={{
                padding: '10px',
                backgroundColor: range.start === preset.start ? '#1976d2' : '#f0f0f0',
                color: range.start === preset.start ? 'white' : 'black',
                border: '1px solid #ddd',
                borderRadius: '4px',
                cursor: 'pointer',
                fontWeight: range.start === preset.start ? 'bold' : 'normal',
              }}
            >
              {label}
            </button>
          ))}
        </div>
      </div>

      {range.start && range.end && (
        <div style={{
          padding: '15px',
          backgroundColor: '#f5f5f5',
          borderRadius: '4px',
          marginTop: '20px',
        }}>
          <p>
            <strong>Selected Period:</strong> {range.start.toLocaleDateString()} - {range.end.toLocaleDateString()}
          </p>
          <p>
            <strong>Duration:</strong> {
              Math.ceil((range.end.getTime() - range.start.getTime()) / 86400000) + 1
            } days
          </p>
        </div>
      )}
    </div>
  );
}

export default AnalyticsDashboard;
```

## Range Validation

### Complex Validation Rules

```tsx
import { DateRangePickerComponent, ChangedEventArgs } from '@syncfusion/ej2-react-calendars';
import * as React from 'react';

interface DateRangeValidation {
  isValid: boolean;
  errors: string[];
}

function AdvancedRangeValidation() {
  const [range, setRange] = React.useState<{
    start: Date | null;
    end: Date | null;
  }>({ start: null, end: null });

  const [validation, setValidation] = React.useState<DateRangeValidation>({
    isValid: true,
    errors: [],
  });

  const validateDateRange = (args: ChangedEventArgs): DateRangeValidation => {
    const errors: string[] = [];
    const today = new Date();
    today.setHours(0, 0, 0, 0);

    if (!args.startDate || !args.endDate) {
      return { isValid: false, errors: ['Both start and end dates are required'] };
    }

    const start = new Date(args.startDate);
    const end = new Date(args.endDate);
    start.setHours(0, 0, 0, 0);
    end.setHours(0, 0, 0, 0);

    // Rule 1: Start before end
    if (start > end) {
      errors.push('Start date must be before end date');
    }

    // Rule 2: Not in past
    if (start < today) {
      errors.push('Start date cannot be in the past');
    }

    // Rule 3: Maximum range
    const daysDiff = Math.ceil((end.getTime() - start.getTime()) / 86400000);
    if (daysDiff > 90) {
      errors.push('Date range cannot exceed 90 days');
    }

    // Rule 4: Minimum range
    if (daysDiff < 1) {
      errors.push('At least 1 day required');
    }

    // Rule 5: No blackout dates (e.g., specific months)
    const blackoutMonths = [11]; // December
    if (start.getMonth() === blackoutMonths[0]) {
      errors.push('December bookings are not available');
    }

    // Rule 6: Start day of week
    if (start.getDay() === 0) {
      errors.push('Range must start on a weekday (Monday-Friday)');
    }

    return {
      isValid: errors.length === 0,
      errors,
    };
  };

  const handleDateChange = (args: ChangedEventArgs) => {
    const result = validateDateRange(args);
    setValidation(result);

    if (result.isValid) {
      setRange({ start: args.startDate as Date, end: args.endDate as Date });
    }
  };

  return (
    <div style={{ padding: '20px', maxWidth: '500px' }}>
      <h3>Advanced Range Validation</h3>

      <DateRangePickerComponent
        id="daterangepicker"
        placeholder="Select date range"
        change={handleDateChange}
      />

      {!validation.isValid && (
        <div style={{
          marginTop: '15px',
          padding: '15px',
          backgroundColor: '#ffebee',
          borderRadius: '4px',
          border: '2px solid #ef5350',
        }}>
          <h4 style={{ margin: '0 0 10px 0', color: '#c62828' }}>❌ Validation Failed:</h4>
          <ul style={{ margin: 0, paddingLeft: '20px' }}>
            {validation.errors.map((error, idx) => (
              <li key={idx} style={{ marginBottom: '5px', color: '#c62828' }}>
                {error}
              </li>
            ))}
          </ul>
        </div>
      )}

      {validation.isValid && range.start && range.end && (
        <div style={{
          marginTop: '15px',
          padding: '15px',
          backgroundColor: '#e8f5e9',
          borderRadius: '4px',
          border: '2px solid #81c784',
        }}>
          <h4 style={{ margin: '0 0 10px 0', color: '#2e7d32' }}>✅ Valid Selection:</h4>
          <p style={{ margin: '5px 0', color: '#2e7d32' }}>
            {range.start.toLocaleDateString('en-US', { weekday: 'long' })} - {range.end.toLocaleDateString('en-US', { weekday: 'long' })}
          </p>
        </div>
      )}
    </div>
  );
}

export default AdvancedRangeValidation;
```

## State Management

### Controlled Component

```tsx
import { DateRangePickerComponent } from '@syncfusion/ej2-react-calendars';
import * as React from 'react';

function ControlledDateRange() {
  const [range, setRange] = React.useState<{
    start: Date | null;
    end: Date | null;
  }>({
    start: new Date(),
    end: new Date(Date.now() + 7 * 86400000),
  });

  const handleReset = () => {
    setRange({ start: null, end: null });
  };

  const handleToday = () => {
    setRange({ start: new Date(), end: new Date() });
  };

  return (
    <div style={{ padding: '20px' }}>
      <DateRangePickerComponent
        id="daterangepicker"
        startDate={range.start}
        endDate={range.end}
        change={(e: any) => setRange({ start: e.startDate, end: e.endDate })}
      />

      <div style={{ marginTop: '15px' }}>
        <button onClick={handleToday} style={{ marginRight: '10px', padding: '8px 12px' }}>
          Today
        </button>
        <button onClick={handleReset} style={{ padding: '8px 12px' }}>
          Reset
        </button>
      </div>

      <pre style={{ marginTop: '20px', backgroundColor: '#f5f5f5', padding: '10px' }}>
        {JSON.stringify(range, null, 2)}
      </pre>
    </div>
  );
}

export default ControlledDateRange;
```

## Common Patterns

### Pattern: Hotel Booking

```tsx
import { DateRangePickerComponent } from '@syncfusion/ej2-react-calendars';
import * as React from 'react';

function HotelBooking() {
  const [range, setRange] = React.useState<{
    checkIn: Date | null;
    checkOut: Date | null;
  }>({ checkIn: null, checkOut: null });

  const today = new Date();
  const minDate = new Date(today);
  const maxDate = new Date(today);
  maxDate.setMonth(maxDate.getMonth() + 6);

  const handleDateChange = (e: any) => {
    if (e.startDate && e.endDate) {
      setRange({ checkIn: e.startDate, checkOut: e.endDate });
    }
  };

  const calculateNights = () => {
    if (range.checkIn && range.checkOut) {
      return Math.ceil(
        (range.checkOut.getTime() - range.checkIn.getTime()) / (1000 * 60 * 60 * 24)
      );
    }
    return 0;
  };

  const nights = calculateNights();
  const pricePerNight = 150;
  const totalPrice = nights * pricePerNight;

  return (
    <div style={{ padding: '20px', maxWidth: '600px' }}>
      <h2>🏨 Hotel Booking</h2>

      <DateRangePickerComponent
        id="daterangepicker"
        placeholder="Select check-in and check-out"
        min={minDate}
        max={maxDate}
        startDate={range.checkIn}
        endDate={range.checkOut}
        change={handleDateChange}
      />

      {range.checkIn && range.checkOut && (
        <div style={{ marginTop: '20px', padding: '15px', backgroundColor: '#f9f9f9' }}>
          <table style={{ width: '100%' }}>
            <tbody>
              <tr>
                <td><strong>Check-in:</strong></td>
                <td>{range.checkIn.toLocaleDateString()}</td>
              </tr>
              <tr>
                <td><strong>Check-out:</strong></td>
                <td>{range.checkOut.toLocaleDateString()}</td>
              </tr>
              <tr style={{ borderTop: '1px solid #ddd', paddingTop: '10px' }}>
                <td><strong>Nights:</strong></td>
                <td>{nights}</td>
              </tr>
              <tr>
                <td><strong>Price per night:</strong></td>
                <td>${pricePerNight}</td>
              </tr>
              <tr style={{ fontSize: '18px', fontWeight: 'bold', borderTop: '2px solid #333' }}>
                <td>Total:</td>
                <td>${totalPrice}</td>
              </tr>
            </tbody>
          </table>
        </div>
      )}
    </div>
  );
}

export default HotelBooking;
```
