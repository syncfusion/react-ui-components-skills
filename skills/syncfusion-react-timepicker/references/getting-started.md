# Getting Started with React TimePicker Component

## Table of Contents
- [Installation](#installation)
- [Module Setup](#module-setup)
- [CSS Imports](#css-imports)
- [Basic Implementation](#basic-implementation)
- [Component Registration](#component-registration)
- [Running the Application](#running-the-application)
- [Troubleshooting](#troubleshooting)

## Installation

Install the Syncfusion TimePicker package using npm:

```bash
# Using npm
npm install @syncfusion/ej2-react-calendars
```

### Dependencies

The TimePicker component requires the following packages:
- `@syncfusion/ej2-base` - Core utilities
- `@syncfusion/ej2-calendars` - Calendar and time picker controls
- React 16.8 or higher (for hooks support)

```bash
# Install dependencies
npm install @syncfusion/ej2-base @syncfusion/ej2-calendars
```

### Verify Installation

Check your `package.json` to confirm the installation:

```json
{
  "dependencies": {
    "@syncfusion/ej2-base": "^23.1.36",
    "@syncfusion/ej2-calendars": "^23.1.36",
    "react": "^18.0.0",
    "react-dom": "^18.0.0"
  }
}
```

## Module Setup

### React Functional Component Setup

For React, import directly in your component:

```tsx
// App.tsx
import { TimePickerComponent } from '@syncfusion/ej2-react-calendars';
import * as React from 'react';

function App() {
  const [selectedTime, setSelectedTime] = React.useState<Date | null>(new Date());

  return (
    <div>
      <TimePickerComponent
        value={selectedTime}
        change={(e: any) => setSelectedTime(e.value)}
      />
    </div>
  );
}

export default App;
```

## CSS Imports

### Import Themes

The TimePicker component requires CSS imports for styling. Choose one of the available themes:

**In Component (app.component.css or app.component.tsx):**

```tsx
import '@syncfusion/ej2-base/styles/material3.css';
import '@syncfusion/ej2-calendars/styles/material3.css';
```

**Or in Global Styles (styles.css or styles.scss):**

```css
/* styles.css */
@import '../node_modules/@syncfusion/ej2-base/styles/material3.css';
@import '../node_modules/@syncfusion/ej2-calendars/styles/material3.css';
```

### Available Themes

Syncfusion provides multiple pre-built themes:

| Theme | File | Best For |
|-------|------|----------|
| Material 3 | material3.css | Modern, recommended default |
| Bootstrap 5 | bootstrap5.css | Bootstrap-based projects |
| Fluent | fluent.css | Microsoft Fluent Design |
| Tailwind | tailwind.css | Tailwind CSS projects |
| Fabric | fabric.css | Office Fabric theme |
| Bootstrap 4 | bootstrap4.css | Bootstrap 4 projects |
| Material | material.css | Google Material Design |
| Highcontrast | highcontrast.css | High contrast for accessibility |

**⚠️ Important:** Choose ONE theme per application to avoid style conflicts. Mixing themes can cause unexpected styling issues.

### Theme Selection Example

```tsx
// For Material 3 (default, modern)
import '@syncfusion/ej2-base/styles/material3.css';
import '@syncfusion/ej2-calendars/styles/material3.css';

// For Bootstrap 5
// import '@syncfusion/ej2-base/styles/bootstrap5.css';
// import '@syncfusion/ej2-calendars/styles/bootstrap5.css';

// For Fluent
// import '@syncfusion/ej2-base/styles/fluent.css';
// import '@syncfusion/ej2-calendars/styles/fluent.css';
```

## Basic Implementation

### Simple Time Picker

Create a basic TimePicker with default configuration:

### React Functional Component

```tsx
import { TimePickerComponent } from '@syncfusion/ej2-react-calendars';
import * as React from 'react';
import '@syncfusion/ej2-base/styles/material3.css';
import '@syncfusion/ej2-calendars/styles/material3.css';

function App() {
  const [selectedTime, setSelectedTime] = React.useState<Date>(new Date('1/1/2018 9:00 AM'));

  const handleChange = (e: any) => {
    setSelectedTime(e.value);
  };

  return (
    <div style={{ padding: '20px', fontFamily: 'Arial, sans-serif' }}>
      <h2>Select a Time</h2>
      <TimePickerComponent
        value={selectedTime}
        change={handleChange}
      />
      <p>Selected: {selectedTime ? selectedTime.toLocaleTimeString() : 'None'}</p>
    </div>
  );
}

export default App;
```

### Time Picker with Event Handling

```tsx
import { TimePickerComponent } from '@syncfusion/ej2-react-calendars';
import * as React from 'react';

function AppWithEvents() {
  const [selectedTime, setSelectedTime] = React.useState<Date>(new Date('1/1/2018 9:00 AM'));

  const onTimeChange = (args: any): void => {
    console.log('Time changed:', args.value);
    console.log('Event type:', args.type);
    setSelectedTime(args.value);
  };

  const onTimeCreated = (): void => {
    console.log('TimePicker component created');
  };

  const onPopupOpen = (args: any): void => {
    console.log('Popup opened');
  };

  const onPopupClose = (args: any): void => {
    console.log('Popup closed');
  };

  return (
    <div style={{ padding: '20px' }}>
      <h2>Time Picker with Events</h2>
      <TimePickerComponent
        value={selectedTime}
        change={onTimeChange}
        created={onTimeCreated}
        open={onPopupOpen}
        close={onPopupClose}
      />
      <p>Current time: {selectedTime?.toLocaleTimeString()}</p>
    </div>
  );
}

export default AppWithEvents;
```

## Component Registration

### Using useRef for Imperative Access

To access TimePicker methods programmatically, use `useRef`:

```tsx
import { TimePickerComponent } from '@syncfusion/ej2-react-calendars';
import * as React from 'react';

function TimepickerWithMethods() {
  const timePickerRef = React.useRef<TimePickerComponent>(null);

  const openPopup = (): void => {
    if (timePickerRef.current) {
      timePickerRef.current.show();
    }
  };

  const closePopup = (): void => {
    if (timePickerRef.current) {
      timePickerRef.current.hide();
    }
  };

  const focusTimePicker = (): void => {
    if (timePickerRef.current) {
      timePickerRef.current.focusIn();
    }
  };

  return (
    <div>
      <TimePickerComponent
        ref={timePickerRef}
        placeholder="Select a time"
      />
      <div style={{ marginTop: '10px' }}>
        <button onClick={openPopup}>Open Popup</button>
        <button onClick={closePopup}>Close Popup</button>
        <button onClick={focusTimePicker}>Focus</button>
      </div>
    </div>
  );
}

export default TimepickerWithMethods;
```

### Accessing Component Instance

```tsx
import { TimePickerComponent } from '@syncfusion/ej2-react-calendars';
import * as React from 'react';

function GetComponentValue() {
  const timePickerRef = React.useRef<TimePickerComponent>(null);

  const getTimeValue = (): void => {
    if (timePickerRef.current) {
      const currentValue = timePickerRef.current.value;
      console.log('Current time value:', currentValue);
      console.log('Time as string:', currentValue?.toLocaleTimeString());
    }
  };

  return (
    <div>
      <TimePickerComponent
        ref={timePickerRef}
        value={new Date('1/1/2018 10:00 AM')}
      />
      <button onClick={getTimeValue}>Get Value</button>
    </div>
  );
}

export default GetComponentValue;
```

## Running the Application

### Development Server

```bash
# Start the development server (React with Vite)
npm run dev

# Or for Create React App
npm start

# Navigate to
http://localhost:5173  # React with Vite
http://localhost:3000  # Create React App
```

### Build for Production

```bash
# Create optimized production build
npm run build

# Output goes to dist/ or build/ folder
```

### Check Browser Console

Ensure no errors appear in the browser console. You should see:
- TimePicker component rendered
- No console errors or warnings

## Troubleshooting

### Issue: "Cannot find module '@syncfusion/ej2-react-calendars'"

**Solution:**
```bash
# Verify installation
npm install @syncfusion/ej2-react-calendars

# Check package.json
npm list @syncfusion/ej2-calendars

# Clear cache and reinstall if needed
rm -rf node_modules package-lock.json
npm install
```

### Issue: Styles not applied (component appears unstyled)

**Solution:**
```tsx
// Verify CSS imports at top of component or app.tsx
import '@syncfusion/ej2-base/styles/material3.css';
import '@syncfusion/ej2-calendars/styles/material3.css';

// Check file exists
// node_modules/@syncfusion/ej2-calendars/styles/material3.css

// Try clearing cache
npm cache clean --force
npm install
```

### Issue: State not updating after time selection

**Solution:**
```tsx
// Use change event handler to sync React state
const handleChange = (e: any) => {
  setSelectedTime(e.value);  // Update state
};

<TimePickerComponent
  value={selectedTime}
  change={handleChange}
/>
```

### Issue: Component not appearing in browser

**Solutions:**
1. Check browser console for errors
2. Verify CSS imports are present
3. Check component selector is correct
4. Ensure component is inside a container with height
5. Clear browser cache: Ctrl+Shift+Delete

### Issue: Popup not opening on click

**Solution:**
```tsx
// The popup should open by clicking the icon button
// If not working, check:
// 1. Component is not disabled
// 2. CSS is properly imported
// 3. No JS errors in console

<TimePickerComponent
  value={selectedTime}
  enabled={true}  // Ensure enabled
  change={handleChange}
/>
```

## Next Steps

Now that you have TimePicker installed and running:

1. **Customize Time Format** → Read [time-format-and-display.md](time-format-and-display.md) for format options
2. **Set Time Constraints** → Read [time-range-and-selection.md](time-range-and-selection.md) for min/max values
3. **Handle Events** → Read [events-and-methods.md](events-and-methods.md) for event patterns
4. **Style Your TimePicker** → Read [customization-and-styling.md](customization-and-styling.md) for themes
5. **Advanced Features** → Read [advanced-patterns.md](advanced-patterns.md) for complex scenarios

## Complete Starter Project

```bash
# Create new React project
npx create-react-app timepicker-demo
cd timepicker-demo

# Install Syncfusion packages
npm install @syncfusion/ej2-react-calendars

# Start development server
npm start
```

### Minimal Working Example

**src/App.tsx:**
```tsx
import { TimePickerComponent } from '@syncfusion/ej2-react-calendars';
import * as React from 'react';
import '@syncfusion/ej2-base/styles/material3.css';
import '@syncfusion/ej2-calendars/styles/material3.css';
import './App.css';

function App() {
  const [time, setTime] = React.useState(new Date('1/1/2018 9:00 AM'));

  return (
    <div className="App">
      <h1>React TimePicker Demo</h1>
      <TimePickerComponent
        value={time}
        change={(e: any) => setTime(e.value)}
        placeholder="Select a time"
      />
      <p>Selected: {time?.toLocaleTimeString()}</p>
    </div>
  );
}

export default App;
```

**src/index.css:**
```css
body {
  margin: 0;
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', 'Roboto', 'Oxygen',
    'Ubuntu', 'Cantarell', 'Fira Sans', 'Droid Sans', 'Helvetica Neue',
    sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

.App {
  padding: 20px;
}
```

Your TimePicker is now ready to use!
