# Getting Started (React)

## Table of Contents
- [Installation](#installation)
- [Quick App Example](#quick-app-example)
- [CSS / Themes](#css--themes)
- [Using refs and methods](#using-refs-and-methods)
- [Events and handlers](#events-and-handlers)
- [Troubleshooting](#troubleshooting)

## Installation

Install the React Calendar package and base utilities:

```bash
npm install @syncfusion/ej2-react-calendars @syncfusion/ej2-base
```

Add the theme CSS (import once in `index.js` or global CSS):

```js
// index.js
import React from 'react';
import { createRoot } from 'react-dom/client';
import App from './App';
import '@syncfusion/ej2-base/styles/material3.css';
import '@syncfusion/ej2-calendars/styles/material3.css';

createRoot(document.getElementById('root')).render(<App />);
```

## Quick App Example

```jsx
// App.jsx
import React, { useState } from 'react';
import { CalendarComponent } from '@syncfusion/ej2-react-calendars';

export default function App() {
  const [value, setValue] = useState(new Date());
  const onChange = (args) => setValue(args.value || args);

  return (
    <div style={{ padding: 20 }}>
      <h2>React Calendar</h2>
      <CalendarComponent value={value} change={onChange} />
      <p>Selected: {value.toDateString()}</p>
    </div>
  );
}
```

### Controlled vs Uncontrolled

- Controlled: pass `value` and update it via the `change` event.
- Uncontrolled: omit `value` and read selected date via event callbacks or methods.

## CSS / Themes

- Recommended to import one theme only (material3, bootstrap5, fluent, etc.).
- When using CSS modules or scoped styles, ensure global imports occur before component styles.

## Using refs and methods

You can obtain a component reference to call imperative methods (navigate, focus, etc.).

```jsx
import React, { useRef } from 'react';
import { CalendarComponent } from '@syncfusion/ej2-react-calendars';

export default function RefExample() {
  const calendarRef = useRef(null);

  const goToNext = () => {
    if (calendarRef.current) {
      // navigateTo(view: CalendarView, date: Date)
      calendarRef.current.navigateTo('Month', new Date(2026, 6, 1));
    }
  };

  return (
    <div>
      <CalendarComponent ref={calendarRef} />
      <button onClick={goToNext}>Go to July 2026</button>
    </div>
  );
}
```

Note: method names and available APIs are listed in the API reference file.

## Events and handlers

- `change` — user changed the active/selected date. Receives an args object with `value`.
- `created` — fired after the component is initialized.
- `destroyed` — fired when the component is removed.
- `renderDayCell` — hook to customize day cells before rendering.

Examples:

```jsx
const onChange = (args) => {
  console.log('selected:', args.value);
};

<CalendarComponent change={onChange} created={() => console.log('ready')} />
```

## Troubleshooting

- "Styles not applied": check import paths and ensure the CSS is included in the bundle.
- "Change not firing": ensure `change` prop is supplied; for controlled components ensure `value` is updated from state.
- "Cannot find module": reinstall packages and clear `node_modules` if necessary.

## Next steps

- Read `references/api-reference.md` for a condensed list of props, events, and methods.
- For date pickers and more complex date-range UX, consider `DateRangePicker` or `DatePicker` components in the same library.
