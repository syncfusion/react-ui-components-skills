# Getting Started with DateRangePicker Component

## Table of Contents
- [Installation](#installation)
- [CSS Imports](#css-imports)
- [Basic Implementation](#basic-implementation)
- [Setup with Vite](#setup-with-vite)
- [Setup with Create React App](#setup-with-create-react-app)
- [Component Registration](#component-registration)
- [Running the Application](#running-the-application)
- [Troubleshooting](#troubleshooting)

## Installation

Install the Syncfusion DateRangePicker package using npm:

```bash
# Using npm
npm install @syncfusion/ej2-react-calendars --save

# Or using ng add (for Angular projects transitioning to React)
npm install @syncfusion/ej2-react-calendars @syncfusion/ej2-base @syncfusion/ej2-buttons @syncfusion/ej2-lists @syncfusion/ej2-inputs @syncfusion/ej2-popups
```

### Dependencies

The DateRangePicker component requires the following packages:
- `@syncfusion/ej2-base` - Core utilities and base components
- `@syncfusion/ej2-calendars` - Calendar controls
- `@syncfusion/ej2-buttons` - Button component for interactions
- `@syncfusion/ej2-lists` - List component for date lists
- `@syncfusion/ej2-inputs` - Input component for text input
- `@syncfusion/ej2-popups` - Popup component for calendar display
- React 16.8 or higher (hooks support)
- React DOM matching React version

```bash
npm install @syncfusion/ej2-base @syncfusion/ej2-calendars @syncfusion/ej2-buttons @syncfusion/ej2-lists @syncfusion/ej2-inputs @syncfusion/ej2-popups
```

## CSS Imports

Import the required CSS files for DateRangePicker. Choose one theme and import it globally:

### In Component (Recommended for component scope)

```tsx
// app.component.tsx
import { DateRangePickerComponent } from '@syncfusion/ej2-react-calendars';
import '@syncfusion/ej2-base/styles/material3.css';
import '@syncfusion/ej2-buttons/styles/material3.css';
import '@syncfusion/ej2-lists/styles/material3.css';
import '@syncfusion/ej2-inputs/styles/material3.css';
import '@syncfusion/ej2-popups/styles/material3.css';
import '@syncfusion/ej2-calendars/styles/material3.css';

function App() {
  return <DateRangePickerComponent id="daterangepicker" />;
}

export default App;
```

### In Global Styles (Recommended for app-wide theme)

```css
/* src/App.css or src/styles.css */
@import '../node_modules/@syncfusion/ej2-base/styles/material3.css';
@import '../node_modules/@syncfusion/ej2-buttons/styles/material3.css';
@import '../node_modules/@syncfusion/ej2-lists/styles/material3.css';
@import '../node_modules/@syncfusion/ej2-inputs/styles/material3.css';
@import '../node_modules/@syncfusion/ej2-popups/styles/material3.css';
@import '../node_modules/@syncfusion/ej2-calendars/styles/material3.css';
```

Then import in your main file:

```tsx
// src/App.tsx
import './App.css';
import { DateRangePickerComponent } from '@syncfusion/ej2-react-calendars';

function App() {
  return <DateRangePickerComponent id="daterangepicker" />;
}

export default App;
```

### Available Themes

Choose one of the following themes:
- **material3.css** - Material Design 3 (default, recommended)
- **bootstrap5.css** - Bootstrap 5 theme
- **fluent.css** - Microsoft Fluent Design
- **tailwind.css** - Tailwind CSS theme
- **fabric.css** - Office Fabric theme
- **bootstrap.css** - Bootstrap 4 theme (legacy)
- **material.css** - Material Design (legacy)

**Important:** Import only one theme per application to avoid conflicts.

## Basic Implementation

### Functional Component (Recommended)

```tsx
import { DateRangePickerComponent } from '@syncfusion/ej2-react-calendars';
import * as React from 'react';
import './App.css';

function App() {
  const [selectedRange, setSelectedRange] = React.useState<{
    startDate: Date | null;
    endDate: Date | null;
  }>({
    startDate: null,
    endDate: null,
  });

  const handleDateRangeChange = (args: any) => {
    setSelectedRange({
      startDate: args.startDate,
      endDate: args.endDate,
    });
  };

  return (
    <div style={{ padding: '20px' }}>
      <h2>Date Range Picker</h2>
      <DateRangePickerComponent
        id="daterangepicker"
        placeholder="Select a date range"
        change={handleDateRangeChange}
      />
      {selectedRange.startDate && selectedRange.endDate && (
        <div style={{ marginTop: '20px', padding: '10px', backgroundColor: '#f0f0f0' }}>
          <p>
            <strong>Selected Range:</strong>
          </p>
          <p>Start: {selectedRange.startDate.toLocaleDateString()}</p>
          <p>End: {selectedRange.endDate.toLocaleDateString()}</p>
        </div>
      )}
    </div>
  );
}

export default App;
```

### Class Component

```tsx
import { DateRangePickerComponent, ChangedEventArgs } from '@syncfusion/ej2-react-calendars';
import * as React from 'react';
import './App.css';

export default class App extends React.Component<{}, any> {
  public state = {
    startDate: null,
    endDate: null,
  };

  public onDateRangeChange = (args: ChangedEventArgs) => {
    this.setState({
      startDate: args.startDate,
      endDate: args.endDate,
    });
  };

  public render() {
    return (
      <div style={{ padding: '20px' }}>
        <h2>Date Range Picker</h2>
        <DateRangePickerComponent
          id="daterangepicker"
          placeholder="Select a date range"
          change={this.onDateRangeChange}
        />
        {this.state.startDate && this.state.endDate && (
          <div style={{ marginTop: '20px', padding: '10px', backgroundColor: '#f0f0f0' }}>
            <p>
              <strong>Selected Range:</strong>
            </p>
            <p>Start: {this.state.startDate.toLocaleDateString()}</p>
            <p>End: {this.state.endDate.toLocaleDateString()}</p>
          </div>
        )}
      </div>
    );
  }
}
```

### With Predefined Date Range

```tsx
import { DateRangePickerComponent } from '@syncfusion/ej2-react-calendars';
import * as React from 'react';

function App() {
  const startDate = new Date('10/07/2017');
  const endDate = new Date('11/15/2017');

  return (
    <div style={{ padding: '20px' }}>
      <h2>Predefined Date Range</h2>
      <DateRangePickerComponent
        id="daterangepicker"
        placeholder="Select a date range"
        startDate={startDate}
        endDate={endDate}
      />
      <p>
        Default range: {startDate.toLocaleDateString()} - {endDate.toLocaleDateString()}
      </p>
    </div>
  );
}

export default App;
```

## Component Registration

### Using useRef for Imperative Access

```tsx
import { DateRangePickerComponent, DateRangePickerComponent as DRP } from '@syncfusion/ej2-react-calendars';
import * as React from 'react';

function App() {
  const dateRangeRef = React.useRef<DRP>(null);

  const handleShowPopup = () => {
    if (dateRangeRef.current) {
      dateRangeRef.current.show();
    }
  };

  const handleGetValue = () => {
    if (dateRangeRef.current) {
      console.log('Start Date:', dateRangeRef.current.startDate);
      console.log('End Date:', dateRangeRef.current.endDate);
    }
  };

  const handleClearValue = () => {
    if (dateRangeRef.current) {
      dateRangeRef.current.value = null;
    }
  };

  return (
    <div style={{ padding: '20px' }}>
      <h2>DateRangePicker with Methods</h2>
      <DateRangePickerComponent
        ref={dateRangeRef}
        id="daterangepicker"
        placeholder="Select a date range"
      />

      <div style={{ marginTop: '20px' }}>
        <button onClick={handleShowPopup} style={{ marginRight: '10px', padding: '5px 10px' }}>
          Show Popup
        </button>
        <button onClick={handleGetValue} style={{ marginRight: '10px', padding: '5px 10px' }}>
          Get Value
        </button>
        <button onClick={handleClearValue} style={{ padding: '5px 10px' }}>
          Clear Value
        </button>
      </div>
    </div>
  );
}

export default App;
```

## Setup with Vite

### Create New Vite Project

```bash
# Create new Vite React project
npm create vite@latest my-daterangepicker-app -- --template react

# Navigate to project
cd my-daterangepicker-app

# Install dependencies
npm install

# Install Syncfusion DateRangePicker
npm install @syncfusion/ej2-react-calendars

# Install all required dependencies
npm install @syncfusion/ej2-base @syncfusion/ej2-buttons @syncfusion/ej2-lists @syncfusion/ej2-inputs @syncfusion/ej2-popups

# Start development server
npm run dev
```

### Add CSS Imports to App.jsx

```jsx
import { DateRangePickerComponent } from '@syncfusion/ej2-react-calendars';
import '@syncfusion/ej2-base/styles/material3.css';
import '@syncfusion/ej2-buttons/styles/material3.css';
import '@syncfusion/ej2-lists/styles/material3.css';
import '@syncfusion/ej2-inputs/styles/material3.css';
import '@syncfusion/ej2-popups/styles/material3.css';
import '@syncfusion/ej2-calendars/styles/material3.css';
import './App.css';

function App() {
  return (
    <div style={{ padding: '20px' }}>
      <h1>DateRangePicker Demo</h1>
      <DateRangePickerComponent id="daterangepicker" placeholder="Select a date range" />
    </div>
  );
}

export default App;
```

## Setup with Create React App

### Create New CRA Project

```bash
# Create new Create React App project (if using CRA)
npx create-react-app my-daterangepicker-app

# Navigate to project
cd my-daterangepicker-app

# Install Syncfusion DateRangePicker
npm install @syncfusion/ej2-react-calendars

# Install all required dependencies
npm install @syncfusion/ej2-base @syncfusion/ej2-buttons @syncfusion/ej2-lists @syncfusion/ej2-inputs @syncfusion/ej2-popups
```

### Add CSS Imports to src/App.css

```css
@import '../node_modules/@syncfusion/ej2-base/styles/material3.css';
@import '../node_modules/@syncfusion/ej2-buttons/styles/material3.css';
@import '../node_modules/@syncfusion/ej2-lists/styles/material3.css';
@import '../node_modules/@syncfusion/ej2-inputs/styles/material3.css';
@import '../node_modules/@syncfusion/ej2-popups/styles/material3.css';
@import '../node_modules/@syncfusion/ej2-calendars/styles/material3.css';
```

### Update src/App.js

```jsx
import { DateRangePickerComponent } from '@syncfusion/ej2-react-calendars';
import './App.css';

function App() {
  return (
    <div style={{ padding: '20px' }}>
      <h1>DateRangePicker Demo</h1>
      <DateRangePickerComponent id="daterangepicker" placeholder="Select a date range" />
    </div>
  );
}

export default App;
```

## Running the Application

### Development Server

```bash
# Start development server (Vite)
npm run dev

# Start development server (Create React App)
npm start

# Default ports:
# Vite: http://localhost:5173
# CRA: http://localhost:3000
```

### Build for Production

```bash
# Create production build
npm run build

# Build output goes to dist/ (Vite) or build/ (CRA)
```

### Preview Production Build

```bash
# Preview production build locally (Vite only)
npm run preview
```

## Troubleshooting

### Issue: Module Not Found Error

```
Error: Cannot find module '@syncfusion/ej2-react-calendars'
```

**Solution:**
1. Ensure package is installed: `npm install @syncfusion/ej2-react-calendars`
2. Clear node_modules: `rm -rf node_modules && npm install`
3. Verify package.json has the dependency
4. Restart development server: `npm run dev` (or `npm start` for CRA)

### Issue: CSS Not Loading / Styles Missing

```
Component renders but without Syncfusion styles
```

**Solution:**
1. Verify CSS imports are in correct order:
   ```tsx
   import '@syncfusion/ej2-base/styles/material3.css';
   import '@syncfusion/ej2-buttons/styles/material3.css';
   import '@syncfusion/ej2-lists/styles/material3.css';
   import '@syncfusion/ej2-inputs/styles/material3.css';
   import '@syncfusion/ej2-popups/styles/material3.css';
   import '@syncfusion/ej2-calendars/styles/material3.css';
   ```
2. Ensure CSS files exist in node_modules
3. Clear browser cache (Ctrl+Shift+Delete)
4. Restart development server

### Issue: TypeScript Errors with useRef

```typescript
Error: Property 'show' does not exist on type 'DateRangePickerComponent'
```

**Solution:**
```tsx
import { DateRangePickerComponent as DRP } from '@syncfusion/ej2-react-calendars';
import * as React from 'react';

const ref = React.useRef<DRP>(null);

// Access properties:
ref.current?.show();
ref.current?.startDate;
```

### Issue: Popup Not Showing

```
DateRangePicker renders but popup doesn't open on click
```

**Solution:**
1. Check component has `id` attribute
2. Ensure event handlers are correctly bound
3. Verify z-index isn't blocked by other elements
4. Check browser console for JavaScript errors

### Issue: Date Format Not Working

```
Custom format string not applied to display
```

**Solution:**
1. Use valid format strings: `'M/d/yyyy'`, `'dd-MMM-yyyy'`, `'yyyy-MM-dd'`
2. Ensure locale matches format expectations
3. Test format string directly:
   ```tsx
   <DateRangePickerComponent
     format="dd-MMM-yyyy"
     locale="en-US"
   />
   ```

## Complete Working Example

**Create new file: `src/DateRangePickerDemo.tsx`**

```tsx
import { DateRangePickerComponent } from '@syncfusion/ej2-react-calendars';
import * as React from 'react';
import '@syncfusion/ej2-base/styles/material3.css';
import '@syncfusion/ej2-buttons/styles/material3.css';
import '@syncfusion/ej2-lists/styles/material3.css';
import '@syncfusion/ej2-inputs/styles/material3.css';
import '@syncfusion/ej2-popups/styles/material3.css';
import '@syncfusion/ej2-calendars/styles/material3.css';

function DateRangePickerDemo() {
  const [dateRange, setDateRange] = React.useState<{
    start: Date | null;
    end: Date | null;
  }>({
    start: null,
    end: null,
  });

  const handleChange = (args: any) => {
    setDateRange({
      start: args.startDate,
      end: args.endDate,
    });
  };

  return (
    <div style={{ padding: '40px', fontFamily: 'Arial, sans-serif' }}>
      <h1>📅 DateRangePicker Component Demo</h1>
      
      <div style={{ marginBottom: '30px' }}>
        <h3>Select Date Range:</h3>
        <DateRangePickerComponent
          id="daterangepicker"
          placeholder="Click to select dates"
          change={handleChange}
          style={{ width: '100%' }}
        />
      </div>

      {dateRange.start && dateRange.end && (
        <div style={{
          padding: '20px',
          backgroundColor: '#e8f5e9',
          borderRadius: '4px',
          border: '1px solid #4caf50'
        }}>
          <h3>✅ Selected Range:</h3>
          <p>
            <strong>From:</strong> {dateRange.start.toLocaleDateString('en-US', {
              weekday: 'long',
              year: 'numeric',
              month: 'long',
              day: 'numeric'
            })}
          </p>
          <p>
            <strong>To:</strong> {dateRange.end.toLocaleDateString('en-US', {
              weekday: 'long',
              year: 'numeric',
              month: 'long',
              day: 'numeric'
            })}
          </p>
          <p>
            <strong>Duration:</strong> {
              Math.ceil((dateRange.end.getTime() - dateRange.start.getTime()) / (1000 * 60 * 60 * 24)) + 1
            } days
          </p>
        </div>
      )}
    </div>
  );
}

export default DateRangePickerDemo;
```

**Update `src/App.tsx`:**

```tsx
import DateRangePickerDemo from './DateRangePickerDemo';

function App() {
  return <DateRangePickerDemo />;
}

export default App;
```

## Next Steps

Now that you have DateRangePicker installed and running:

1. **Learn Date Range Selection** → Read [date-range-selection.md](date-range-selection.md) to understand start/end dates and validation
2. **Format Dates** → Read [date-range-formatting.md](date-range-formatting.md) for format options and localization
3. **Handle Events** → Read [events-and-methods.md](events-and-methods.md) for event handling and imperative methods
4. **Customize Component** → Read [customization-and-styling.md](customization-and-styling.md) for themes and styling
5. **Advanced Patterns** → Read [advanced-patterns.md](advanced-patterns.md) for complex scenarios
6. **API Reference** → Read [api-reference.md](api-reference.md) for complete API documentation
