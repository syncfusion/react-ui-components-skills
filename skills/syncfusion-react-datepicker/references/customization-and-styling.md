# Customization & Styling

## Table of Contents
- [CSS Classes](#css-classes)
- [renderDayCell Event](#renderdaycell-event)
- [Component States](#component-states)
- [Custom CSS Styling](#custom-css-styling)
- [Common Customizations](#common-customizations)
- [Disabling Specific Dates](#disabling-specific-dates)
- [Complete Examples](#complete-examples)

## CSS Classes

DatePicker applies CSS classes throughout its elements for targeting styles.

### Main Component Classes

| Class | Applied To | Purpose |
|-------|-----------|---------|
| `e-date-wrapper` | Container wrapper | Wraps entire DatePicker |
| `e-datepicker` | Input element | DatePicker input field |
| `e-float-text` | Label element | Floating label (if used) |
| `e-date-icon` | Calendar icon | Dropdown icon |
| `e-popup-wrapper` | Popup container | Calendar popup wrapper |
| `e-calendar` | Calendar element | Main calendar container |

### Calendar Layout Classes

| Class | Applied To | Purpose |
|-------|-----------|---------|
| `e-header` | Calendar header | Header section (weekday names) |
| `e-title` | Calendar title | Title showing month/year |
| `e-icon-container` | Icon container | Holds nav arrows |
| `e-prev` | Previous arrow | Left/back arrow |
| `e-next` | Next arrow | Right/forward arrow |
| `e-weekend` | Weekend days | Saturday, Sunday cells |
| `e-other-month` | Other month dates | Grayed out dates from adjacent months |

### Day Cell Classes

| Class | Applied To | Purpose |
|-------|-----------|---------|
| `e-day` | Each day cell | Individual date cell |
| `e-selected` | Selected date | Currently selected date |
| `e-disabled` | Disabled dates | Out of range, disabled dates |
| `e-today` | Today's date | Current day highlight |

### State Classes

| Class | Applied To | Purpose |
|-------|-----------|---------|
| `e-disabled` | Input element | When DatePicker is disabled |
| `e-readonly` | Input element | When DatePicker is readonly |
| `e-focus` | Input element | When focused |

## renderDayCell Event

The `renderDayCell` event allows custom styling and logic for individual day cells.

### Event Handler Signature

```jsx
function onRenderDayCell(args: RenderDayCellEventArgs) {
  // args.date: Date object for the cell
  // args.element: HTMLElement of the day cell
  // args.isDisabled: boolean - can set to disable
  // args.isSelected: boolean - read-only current state
}
```

### Basic Example: Disable Weekends

```jsx
import React from 'react';
import { DatePickerComponent, RenderDayCellEventArgs } from '@syncfusion/ej2-react-calendars';

export default function App() {
  const onRenderDayCell = (args: RenderDayCellEventArgs) => {
    // Get day of week: 0=Sunday, 1=Monday, ..., 6=Saturday
    const dayOfWeek = args.date.getDay();
    
    if (dayOfWeek === 0 || dayOfWeek === 6) {
      // Disable Saturday (6) and Sunday (0)
      args.isDisabled = true;
    }
  };

  return (
    <DatePickerComponent
      renderDayCell={onRenderDayCell}
      placeholder="Weekdays only"
    />
  );
}
```

**Result:** Saturday and Sunday cells are grayed out, unselectable

### Example: Highlight Specific Dates

```jsx
const onRenderDayCell = (args: RenderDayCellEventArgs) => {
  const date = args.date;
  
  // Highlight 15th and 25th of each month
  if (date.getDate() === 15 || date.getDate() === 25) {
    args.element.style.backgroundColor = '#ffeb3b';
    args.element.style.fontWeight = 'bold';
  }
};

<DatePickerComponent
  renderDayCell={onRenderDayCell}
  placeholder="Special dates highlighted"
/>
```

**Result:** Days 15 and 25 have yellow background and bold text

### Example: Holiday Styling

```jsx
const holidays = [
  { date: new Date(2026, 0, 1), name: 'New Year' },
  { date: new Date(2026, 11, 25), name: 'Christmas' }
];

const onRenderDayCell = (args: RenderDayCellEventArgs) => {
  const holiday = holidays.find(
    h => h.date.getTime() === args.date.getTime()
  );
  
  if (holiday) {
    args.element.style.backgroundColor = '#ff5252';
    args.element.style.color = 'white';
    args.element.title = holiday.name;
  }
};

<DatePickerComponent
  renderDayCell={onRenderDayCell}
  placeholder="Holidays highlighted"
/>
```

**Result:** Holiday dates show red background with white text, hover shows holiday name

## Component States

### Disabled State

```jsx
<DatePickerComponent
  value={new Date()}
  enabled={false}
  placeholder="This is disabled"
/>
```

**Styling:**
- Input appears grayed out
- Calendar icon disabled
- Cannot click or type
- Apply CSS: `.e-disabled { opacity: 0.6; pointer-events: none; }`

### Readonly State

```jsx
<DatePickerComponent
  value={new Date()}
  readonly={true}
  placeholder="This is readonly"
/>
```

**Styling:**
- Input shows value but cannot edit
- Calendar still opens on icon click
- Can select dates from calendar
- Cannot type directly
- Apply CSS: `.e-readonly { background-color: #f5f5f5; }`

### Focused State

```jsx
<DatePickerComponent
  value={new Date()}
  focus={(e) => console.log('Focused')}
  blur={(e) => console.log('Blurred')}
  placeholder="Focus me"
/>
```

**Styling:**
- Input has focus outline
- Apply CSS: `.e-datepicker:focus { outline: 2px solid blue; }`

## Custom CSS Styling

### Using cssClass Property

```jsx
<DatePickerComponent
  value={new Date()}
  cssClass="my-custom-picker"
  placeholder="Custom styled"
/>
```

In your CSS file:

```css
/* Input styling */
.my-custom-picker {
  border: 2px solid #2196F3;
  border-radius: 8px;
  padding: 10px;
  font-size: 16px;
}

/* Calendar popup styling */
.my-custom-picker.e-popup-wrapper {
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
  border-radius: 4px;
}

/* Day cells */
.my-custom-picker .e-day {
  border-radius: 4px;
  margin: 2px;
}

/* Selected day */
.my-custom-picker .e-selected {
  background-color: #2196F3;
  color: white;
}

/* Hover effect */
.my-custom-picker .e-day:hover {
  background-color: #e3f2fd;
}
```

### Theme Customization

Different themes available during import:

```jsx
// Material 3 (default, modern)
import '@syncfusion/ej2-react-calendars/styles/material3.css';

// Bootstrap 5
import '@syncfusion/ej2-react-calendars/styles/bootstrap5.css';

// Fluent Design
import '@syncfusion/ej2-react-calendars/styles/fluent.css';

// High Contrast (accessibility)
import '@syncfusion/ej2-react-calendars/styles/highcontrast.css';
```

## Common Customizations

### 1. Disable Weekends

```jsx
const onRenderDayCell = (args: RenderDayCellEventArgs) => {
  if (args.date.getDay() === 0 || args.date.getDay() === 6) {
    args.isDisabled = true;
  }
};

<DatePickerComponent
  renderDayCell={onRenderDayCell}
  placeholder="Weekdays only"
/>
```

### 2. Disable Past Dates

```jsx
const onRenderDayCell = (args: RenderDayCellEventArgs) => {
  if (args.date < new Date()) {
    args.isDisabled = true;
  }
};

<DatePickerComponent
  renderDayCell={onRenderDayCell}
  placeholder="Select future dates"
/>
```

### 3. Disable Future Dates

```jsx
const onRenderDayCell = (args: RenderDayCellEventArgs) => {
  if (args.date > new Date()) {
    args.isDisabled = true;
  }
};

<DatePickerComponent
  renderDayCell={onRenderDayCell}
  placeholder="Select past dates"
/>
```

### 4. Highlight Today

```jsx
const onRenderDayCell = (args: RenderDayCellEventArgs) => {
  const today = new Date();
  if (args.date.toDateString() === today.toDateString()) {
    args.element.style.backgroundColor = '#4CAF50';
    args.element.style.color = 'white';
    args.element.style.fontWeight = 'bold';
  }
};

<DatePickerComponent
  renderDayCell={onRenderDayCell}
  placeholder="Today highlighted"
/>
```

### 5. Show Birth Day Style

```jsx
const birthDate = new Date(1995, 2, 15);

const onRenderDayCell = (args: RenderDayCellEventArgs) => {
  // Check if birth day (day and month match, ignore year)
  if (args.date.getDate() === birthDate.getDate() &&
      args.date.getMonth() === birthDate.getMonth()) {
    args.element.style.backgroundColor = '#9C27B0';
    args.element.style.color = 'white';
  }
};

<DatePickerComponent
  renderDayCell={onRenderDayCell}
  placeholder="Birth day highlighted yearly"
/>
```

### 6. Custom Placeholder Styling

```css
.e-datepicker::placeholder {
  color: #999;
  font-style: italic;
}
```

### 7. Readonly with Custom Styling

```jsx
<DatePickerComponent
  value={new Date()}
  readonly={true}
  cssClass="readonly-picker"
  placeholder="View only"
/>
```

```css
.readonly-picker {
  background-color: #f0f0f0;
  border: 1px solid #ccc;
  color: #666;
}

.readonly-picker:focus {
  outline: none;
}
```

## Disabling Specific Dates

### Disable Individual Dates

```jsx
const disabledDates = [
  new Date(2026, 2, 19),
  new Date(2026, 2, 25)
];

const onRenderDayCell = (args: RenderDayCellEventArgs) => {
  const isDisabledDate = disabledDates.some(
    d => d.getTime() === args.date.getTime()
  );
  
  if (isDisabledDate) {
    args.isDisabled = true;
  }
};

<DatePickerComponent
  renderDayCell={onRenderDayCell}
  placeholder="Some dates disabled"
/>
```

### Disable with Min/Max (Recommended)

```jsx
<DatePickerComponent
  min={new Date(2026, 2, 10)}
  max={new Date(2026, 2, 20)}
  placeholder="10-20 March only"
/>
```

This is simpler and more performant than `renderDayCell`.

## Complete Examples

### Example 1: Appointment Picker (Weekdays, Business Hours)

```jsx
import React from 'react';
import { DatePickerComponent, RenderDayCellEventArgs } from '@syncfusion/ej2-react-calendars';

export default function App() {
  const onRenderDayCell = (args: RenderDayCellEventArgs) => {
    const dayOfWeek = args.date.getDay();
    
    // Disable weekends
    if (dayOfWeek === 0 || dayOfWeek === 6) {
      args.isDisabled = true;
    }
  };

  return (
    <DatePickerComponent
      value={new Date()}
      renderDayCell={onRenderDayCell}
      min={new Date()}
      max={new Date(Date.now() + 30 * 24 * 60 * 60 * 1000)}
      format="dddd, MMMM d, yyyy"
      placeholder="Select appointment date (weekdays only)"
    />
  );
}
```

### Example 2: Birthday with Custom Colors

```jsx
import React from 'react';
import { DatePickerComponent, RenderDayCellEventArgs } from '@syncfusion/ej2-react-calendars';
import './App.css';

export default function App() {
  const birthDate = new Date(1995, 2, 15);

  const onRenderDayCell = (args: RenderDayCellEventArgs) => {
    if (args.date.getDate() === birthDate.getDate() &&
        args.date.getMonth() === birthDate.getMonth()) {
      args.element.classList.add('birthday-cell');
    }
  };

  return (
    <DatePickerComponent
      value={new Date()}
      renderDayCell={onRenderDayCell}
      cssClass="birthday-picker"
      placeholder="Select your birthday"
    />
  );
}
```

**CSS:**
```css
.birthday-cell {
  background-color: #FF69B4;
  color: white;
  font-weight: bold;
  border-radius: 50%;
}

.birthday-picker .e-selected {
  background-color: #2196F3;
  box-shadow: 0 0 0 2px #FF69B4;
}
```

### Example 3: Project Timeline (Available Dates)

```jsx
const availableDates = [
  new Date(2026, 2, 15),
  new Date(2026, 2, 16),
  new Date(2026, 2, 22),
  new Date(2026, 2, 23),
  new Date(2026, 2, 24)
];

const onRenderDayCell = (args: RenderDayCellEventArgs) => {
  const isAvailable = availableDates.some(
    d => d.getTime() === args.date.getTime()
  );
  
  if (!isAvailable) {
    args.isDisabled = true;
  } else {
    args.element.style.backgroundColor = '#4CAF50';
    args.element.style.color = 'white';
  }
};

<DatePickerComponent
  renderDayCell={onRenderDayCell}
  placeholder="Select available date"
/>
```

