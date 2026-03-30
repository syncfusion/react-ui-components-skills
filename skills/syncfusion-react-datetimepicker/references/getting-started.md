# Getting Started with DateTimePicker

## Table of Contents
- [Installation](#installation)
- [Module Setup](#module-setup)
- [CSS Imports](#css-imports)
- [Basic Implementation](#basic-implementation)
- [Component Registration](#component-registration)
- [Running the Application](#running-the-application)
- [Troubleshooting](#troubleshooting)

## Installation

Install the Syncfusion DateTimePicker package using npm:

```bash
# Using npm
npm install @syncfusion/ej2-react-calendars

# Or using yarn
yarn add @syncfusion/ej2-react-calendars
```

### Dependencies

The DateTimePicker component requires:
- `@syncfusion/ej2-base` - Core utilities
- `@syncfusion/ej2-calendars` - Calendar and DateTimePicker controls
- `@syncfusion/ej2-inputs` - Input components (optional, for enhanced features)
- React 16.8+ (hooks support recommended)

```bash
npm install @syncfusion/ej2-base @syncfusion/ej2-calendars
```

## Module Setup

### Functional Component Setup (Recommended)

Import `DateTimePickerComponent` directly in your component:

```tsx
import React, { useState } from 'react';
import { DateTimePickerComponent } from '@syncfusion/ej2-react-calendars';

export default function App() {
  const [value, setValue] = useState<Date | null>(new Date());

  return (
    <DateTimePickerComponent
      value={value}
      change={(e) => setValue((e as any).value)}
    />
  );
}
```

### Class Component Setup

```tsx
import React from 'react';
import { DateTimePickerComponent } from '@syncfusion/ej2-react-calendars';

class DateTimeApp extends React.Component {
  constructor(props: any) {
    super(props);
    this.state = { value: new Date() };
  }

  handleChange = (args: any) => {
    this.setState({ value: args.value });
  };

  render() {
    return (
      <DateTimePickerComponent
        value={this.state.value}
        change={this.handleChange}
      />
    );
  }
}

export default DateTimeApp;
```

## CSS Imports

Import the DateTimePicker theme CSS in your application. Choose one theme to avoid conflicts.

### In Component CSS

```css
/* MyComponent.css */
@import '../node_modules/@syncfusion/ej2-base/styles/material3.css';
@import '../node_modules/@syncfusion/ej2-calendars/styles/material3.css';
```

Then import the CSS in your component:

```tsx
import './MyComponent.css';
import { DateTimePickerComponent } from '@syncfusion/ej2-react-calendars';

export default function MyComponent() {
  return <DateTimePickerComponent />;
}
```

### In Global Styles (index.css)

```css
/* src/index.css */
@import '../node_modules/@syncfusion/ej2-base/styles/material3.css';
@import '../node_modules/@syncfusion/ej2-calendars/styles/material3.css';
```

### Available Themes

- **material3.css** - Material Design 3 (default, modern)
- **bootstrap5.css** - Bootstrap 5 theme
- **fluent.css** - Microsoft Fluent Design System
- **tailwind.css** - Tailwind CSS compatible theme
- **fabric.css** - Office Fabric theme
- **material.css** - Original Material Design

**Tip:** Choose one theme per application to avoid CSS conflicts.

## Basic Implementation

### Minimal Working Example

```tsx
import React, { useState } from 'react';
import { DateTimePickerComponent } from '@syncfusion/ej2-react-calendars';
import './App.css';

export default function App() {
  const [selectedDateTime, setSelectedDateTime] = useState<Date | null>(new Date());

  return (
    <div style={{ padding: '20px' }}>
      <h2>Select Date and Time</h2>
      <DateTimePickerComponent
        value={selectedDateTime}
        change={(e) => setSelectedDateTime((e as any).value)}
        format="dd/MM/yyyy hh:mm a"
        placeholder="Select date and time"
      />
      <p>Selected: {selectedDateTime?.toLocaleString()}</p>
    </div>
  );
}
```

### With Event Handlers

```tsx
import React, { useState } from 'react';
import { DateTimePickerComponent, ChangedEventArgs } from '@syncfusion/ej2-react-calendars';

export default function DateTimeWithEvents() {
  const [selectedDateTime, setSelectedDateTime] = useState<Date | null>(new Date());

  const handleChange = (args: ChangedEventArgs) => {
    console.log('Date changed:', args.value);
    setSelectedDateTime(args.value as Date);
  };

  const handleOpen = () => {
    console.log('DateTimePicker opened');
  };

  const handleClose = () => {
    console.log('DateTimePicker closed');
  };

  const handleCreated = () => {
    console.log('DateTimePicker created');
  };

  return (
    <DateTimePickerComponent
      value={selectedDateTime}
      change={handleChange}
      open={handleOpen}
      close={handleClose}
      created={handleCreated}
      format="MM/dd/yyyy hh:mm:ss a"
    />
  );
}
```

## Component Registration

### Using ViewChild (Imperative Access)

To programmatically call methods on the DateTimePicker:

```tsx
import React, { useRef } from 'react';
import { DateTimePickerComponent } from '@syncfusion/ej2-react-calendars';

export default function DateTimeRef() {
  const dateTimeRef = useRef<DateTimePickerComponent>(null);

  const handleNavigate = () => {
    if (dateTimeRef.current) {
      // Navigate to year view
      dateTimeRef.current.navigateTo('Year', new Date(2025, 0, 1));
      console.log('Current view:', dateTimeRef.current.currentView());
    }
  };

  const handleFocus = () => {
    if (dateTimeRef.current) {
      dateTimeRef.current.focusIn();
    }
  };

  return (
    <div>
      <DateTimePickerComponent ref={dateTimeRef} />
      <button onClick={handleNavigate}>Navigate to Year</button>
      <button onClick={handleFocus}>Focus Component</button>
    </div>
  );
}
```

### Register with onClick Handler

```tsx
import React, { useState } from 'react';
import { DateTimePickerComponent } from '@syncfusion/ej2-react-calendars';

export default function DateTimeWithButton() {
  const [value, setValue] = useState<Date | null>(new Date());
  const [view, setView] = useState<string>('Month');

  return (
    <div>
      <DateTimePickerComponent value={value} change={(e) => setValue((e as any).value)} />
      <button onClick={() => setView('Year')}>Show Year View</button>
      <p>Current view: {view}</p>
    </div>
  );
}
```

## Running the Application

### Development Server

```bash
# Start the development server
npm start

# Navigate to
http://localhost:3000
```

### Build for Production

```bash
# Create optimized production build
npm run build

# Output goes to build/ folder
```

### Deploy

After building, deploy the `build/` folder to your hosting platform (Vercel, Netlify, AWS S3, etc.).

## Troubleshooting

### Issue: "Cannot find module '@syncfusion/ej2-react-calendars'"

**Solution:**
- Run `npm install @syncfusion/ej2-react-calendars`
- Verify the package is listed in `package.json`
- Clear `node_modules` and run `npm install` again

### Issue: Styles not applied

**Solution:**
- Verify CSS import path is correct in your CSS or index.css
- Check that the theme CSS file exists in `node_modules/@syncfusion/ej2-calendars/styles/`
- Ensure `@import` statement comes before other styles
- Use only one theme per application

### Issue: change event not firing

**Solution:**
- Use the `change` prop (not `onChange`) — this is the Syncfusion event name
- Import `ChangedEventArgs` type if using TypeScript
- Wrap handler: `change={(e) => setValue((e as any).value)}`
- Verify the component is not in readonly mode
- Check console for errors

### Issue: Placeholder not visible

**Solution:**
- Add `placeholder="Select date and time"` prop
- Use `floatLabelType="Auto"` for floating labels
- Verify CSS is imported correctly

### Issue: Cannot access methods (navigate, focusIn, etc.)

**Solution:**
- Use `useRef` to get component reference
- Call methods on the ref: `dateTimeRef.current.navigateTo(...)`
- Ensure component is fully loaded before calling methods

## Next Steps

Now that DateTimePicker is installed and running:

1. **Learn Date & Time Selection** → Read [date-time-selection.md](date-time-selection.md) for min/max constraints and formatting
2. **Configure Time Intervals** → Read [time-configuration.md](time-configuration.md) for step, minTime/maxTime settings
3. **Handle Events** → Read [events-and-methods.md](events-and-methods.md) for event handlers and methods
4. **Customize Appearance** → Read [styling-and-customization.md](styling-and-customization.md) for themes and CSS
5. **Advanced Features** → Read [advanced-features.md](advanced-features.md) for masked input, strict mode, etc.
6. **Accessibility** → Read [accessibility.md](accessibility.md) for keyboard navigation and WCAG compliance

## Complete Working Project

```bash
# Create new React app
npx create-react-app datetime-demo

# Navigate to project
cd datetime-demo

# Install Syncfusion
npm install @syncfusion/ej2-react-calendars

# Start development server
npm start
```

**Complete `src/App.tsx`:**

```tsx
import React, { useState } from 'react';
import { DateTimePickerComponent } from '@syncfusion/ej2-react-calendars';
import './App.css';

export default function App() {
  const [selectedDateTime, setSelectedDateTime] = useState<Date | null>(new Date());

  return (
    <div style={{ padding: '40px', fontFamily: 'Arial, sans-serif' }}>
      <h1>DateTimePicker Demo</h1>
      <DateTimePickerComponent
        value={selectedDateTime}
        change={(e) => setSelectedDateTime((e as any).value)}
        format="MM/dd/yyyy hh:mm a"
        step={15}
        placeholder="Select date and time"
        width="300px"
      />
      <p>Selected: {selectedDateTime?.toLocaleString()}</p>
    </div>
  );
}
```

**Complete `src/App.css`:**

```css
@import '../node_modules/@syncfusion/ej2-base/styles/material3.css';
@import '../node_modules/@syncfusion/ej2-calendars/styles/material3.css';

body {
  margin: 0;
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', 'Roboto', 'Oxygen',
    'Ubuntu', 'Cantarell', 'Fira Sans', 'Droid Sans', 'Helvetica Neue',
    sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

code {
  font-family: source-code-pro, Menlo, Monaco, Consolas, 'Courier New',
    monospace;
}

.e-datetimepicker {
  margin: 20px 0;
}
```

Your DateTimePicker is ready!
