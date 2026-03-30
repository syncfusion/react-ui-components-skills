# Getting Started with DatePicker

## Table of Contents
- [Installation](#installation)
- [CSS Theme Import](#css-theme-import)
- [Component Import](#component-import)
- [Basic JSX Setup](#basic-jsx-setup)
- [Functional Component](#functional-component)
- [Class Component](#class-component)
- [Running Your Application](#running-your-application)

## Installation

Install the Syncfusion React Calendar package using npm:

```bash
npm install @syncfusion/ej2-react-calendars --save
```

The `--save` flag adds the package to your `package.json` dependencies.

> **Note:** The DatePicker component is part of the `@syncfusion/ej2-react-calendars` package. Installing this package provides access to DatePicker, Calendar, DateRangePicker, DateTimePicker, and TimePicker components.

### Verify Installation

After installation, verify the package was added to your project:

```bash
npm list @syncfusion/ej2-react-calendars
```

You should see the package listed with its version.

## CSS Theme Import

The DatePicker requires CSS styles for proper rendering. Import the theme CSS in your main App component or CSS file.

### Available Themes

Syncfusion provides several built-in themes:
- **material3** - Material Design 3 (default, modern)
- **bootstrap5** - Bootstrap 5 theme
- **bootstrap4** - Bootstrap 4 theme
- **fluent** - Microsoft Fluent Design
- **fabric** - Microsoft Fabric theme
- **highcontrast** - High contrast for accessibility
- **tailwind** - Tailwind CSS theme

### Import CSS (Recommended in App.jsx)

```jsx
import '@syncfusion/ej2-base/styles/material3.css';
import '@syncfusion/ej2-buttons/styles/material3.css';
import '@syncfusion/ej2-inputs/styles/material3.css';
import '@syncfusion/ej2-popups/styles/material3.css';
import '@syncfusion/ej2-react-calendars/styles/material3.css';
```

**Why these files?**
- `ej2-base` - Base styles required by all components
- `ej2-buttons` - Buttons used in DatePicker (calendar navigation, today button)
- `ej2-inputs` - Input field styles for the date textbox
- `ej2-popups` - Popup wrapper for the calendar
- `ej2-react-calendars` - DatePicker and calendar specific styles

### Alternative: Import in CSS File

If you prefer to keep CSS separate, add to your `App.css`:

```css
@import "../node_modules/@syncfusion/ej2-base/styles/material3.css";
@import "../node_modules/@syncfusion/ej2-buttons/styles/material3.css";
@import "../node_modules/@syncfusion/ej2-inputs/styles/material3.css";
@import "../node_modules/@syncfusion/ej2-popups/styles/material3.css";
@import "../node_modules/@syncfusion/ej2-react-calendars/styles/material3.css";
```

Then import the CSS file in your component:

```jsx
import './App.css';
```

## Component Import

Import the `DatePickerComponent` from the package:

```jsx
import { DatePickerComponent } from '@syncfusion/ej2-react-calendars';
```

You can also import related types if using TypeScript:

```jsx
import { DatePickerComponent, DatePickerChangeEventArgs } from '@syncfusion/ej2-react-calendars';
```

## Basic JSX Setup

The simplest DatePicker requires only the component tag:

```jsx
<DatePickerComponent id="datepicker" />
```

The `id` attribute is recommended for form accessibility and element identification.

### Adding a Value

Set an initial date value:

```jsx
<DatePickerComponent
  id="datepicker"
  value={new Date()}
/>
```

### Adding a Placeholder

Add a placeholder for user guidance:

```jsx
<DatePickerComponent
  id="datepicker"
  value={new Date()}
  placeholder="Select a date"
/>
```

## Functional Component

Modern React uses functional components with hooks. Here's a complete example:

```jsx
import React, { useState } from 'react';
import { DatePickerComponent } from '@syncfusion/ej2-react-calendars';
import '@syncfusion/ej2-base/styles/material3.css';
import '@syncfusion/ej2-buttons/styles/material3.css';
import '@syncfusion/ej2-inputs/styles/material3.css';
import '@syncfusion/ej2-popups/styles/material3.css';
import '@syncfusion/ej2-react-calendars/styles/material3.css';

export default function App() {
  const [selectedDate, setSelectedDate] = useState(new Date());

  return (
    <div style={{ padding: '20px' }}>
      <h3>DatePicker Example</h3>
      <DatePickerComponent
        id="datepicker"
        value={selectedDate}
        change={(e) => setSelectedDate(e.value)}
        placeholder="Select a date"
      />
      <p>Selected Date: {selectedDate?.toDateString()}</p>
    </div>
  );
}
```

**Key points:**
- `useState` manages the selected date state
- `change` event updates the state when user selects a date
- `e.value` contains the selected Date object
- Display the selected date below the picker

## Class Component

If using class-based components, here's the equivalent:

```jsx
import React from 'react';
import { DatePickerComponent } from '@syncfusion/ej2-react-calendars';
import '@syncfusion/ej2-base/styles/material3.css';
import '@syncfusion/ej2-buttons/styles/material3.css';
import '@syncfusion/ej2-inputs/styles/material3.css';
import '@syncfusion/ej2-popups/styles/material3.css';
import '@syncfusion/ej2-react-calendars/styles/material3.css';

export default class App extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      selectedDate: new Date()
    };
  }

  handleDateChange = (e) => {
    this.setState({ selectedDate: e.value });
  }

  render() {
    return (
      <div style={{ padding: '20px' }}>
        <h3>DatePicker Example</h3>
        <DatePickerComponent
          id="datepicker"
          value={this.state.selectedDate}
          change={this.handleDateChange}
          placeholder="Select a date"
        />
        <p>Selected Date: {this.state.selectedDate?.toDateString()}</p>
      </div>
    );
  }
}
```

**Key points:**
- State stores the selected date in `this.state`
- `handleDateChange` method updates state
- Arrow function ensures proper `this` binding
- Class component pattern for legacy projects

## Running Your Application

After setting up the DatePicker, run your React application:

```bash
npm run dev
```

For Create React App:

```bash
npm start
```

The application should open in your browser at `http://localhost:5173` (Vite) or `http://localhost:3000` (CRA).

### Verify DatePicker Works

1. Click the input field or the calendar icon
2. Calendar popup should appear
3. Click on a date to select it
4. Selected date should display in the input field
5. State should update (if bound to React state)

### Troubleshooting

**Calendar not appearing:**
- Verify all CSS files are imported
- Check browser console for errors
- Ensure `@syncfusion/ej2-react-calendars` is installed

**Styling looks wrong:**
- Verify theme CSS imports are correct
- Check that all required CSS files are imported (ej2-base, ej2-popups, etc.)
- Clear browser cache and refresh

**Value not updating:**
- Ensure `change` event handler is defined (not `onChange`)
- Verify state is being updated in the handler
- Check that `value` prop is bound to state

## Complete API Reference

The `DatePickerComponent` accepts the following props:

### Properties

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `allowEdit` | boolean | true | Specifies whether the input textbox is editable or not |
| `calendarMode` | CalendarType | Gregorian | Gets or sets the Calendar's Type (gregorian or islamic) |
| `cssClass` | string | null | Root CSS class to customize appearance |
| `dayHeaderFormat` | DayHeaderFormats | Short | Format of day names in header (Short, Narrow, Abbreviated, Wide) |
| `depth` | CalendarView | Month | Maximum level of view (Month, Year, Decade) |
| `enableMask` | boolean | false | Specifies whether it is a masked datepicker |
| `enablePersistence` | boolean | false | Enable persisting component's state between page reloads |
| `enableRtl` | boolean | false | Enable right-to-left rendering |
| `enabled` | boolean | true | Specifies the component to be disabled or not |
| `firstDayOfWeek` | number | 0 | Gets or sets the Calendar's first day of the week |
| `floatLabelType` | FloatLabelType | Never | Placeholder text float behavior (Never, Always, Auto) |
| `format` | string \| FormatObject | null | Format of the value displayed in the component |
| `fullScreenMode` | boolean | false | Display popup full screen on mobile devices |
| `htmlAttributes` | { [key: string]: string } | {} | Additional HTML attributes for the element |
| `inputFormats` | string[] \| FormatObject[] | null | Array of acceptable date input formats for parsing |
| `keyConfigs` | { [key: string]: string } | null | Customizes key actions in DatePicker |
| `locale` | string | '' | Overrides global culture and localization value |
| `maskPlaceholder` | MaskPlaceholderModel | {...} | Placeholder displayed on masked datepicker |
| `max` | Date | new Date(2099, 11, 31) | Maximum selectable date |
| `min` | Date | new Date(1900, 00, 01) | Minimum selectable date |
| `openOnFocus` | boolean | false | Open popup while focusing the date input |
| `placeholder` | string | null | Placeholder text displayed in textbox |
| `readonly` | boolean | false | Component in readonly state |
| `serverTimezoneOffset` | number | null | Server time zone offset for initial date processing |
| `showClearButton` | boolean | true | Show or hide the clear icon in textbox |
| `showTodayButton` | boolean | true | Show or hide the today button |
| `start` | CalendarView | Month | Initial view when calendar opens (Month, Year, Decade) |
| `strictMode` | boolean | false | Restricts entry to valid dates within range only |
| `value` | Date | null | Gets or sets the selected date |
| `weekNumber` | boolean | false | Display week number of the year |
| `weekRule` | WeekRule | FirstDay | Rule for defining the first week of the year |
| `width` | number \| string | null | Width of the DatePicker component |
| `zIndex` | number | 1000 | z-index value of the datePicker popup element |

### Methods

| Method | Returns | Description |
|--------|---------|-------------|
| `currentView()` | string | Gets the current view of the DatePicker |
| `destroy()` | void | Destroys the widget |
| `focusIn()` | void | Sets the focus to widget for interaction |
| `focusOut()` | void | Removes the focus from widget |
| `getPersistData()` | string | Gets properties to be maintained upon browser refresh |
| `hide()` | void | Hides the Calendar popup |
| `navigateTo(view, date)` | void | Navigates to specified month/year/decade view |
| `removeDate(dates)` | void | Removes single or multiple dates from values property |
| `show()` | void | Shows the Calendar popup |

### Events

| Event | Args Type | Description |
|-------|-----------|-------------|
| `blur` | BlurEventArgs | Triggers when the input loses focus |
| `change` | ChangedEventArgs | Triggers when the Calendar value is changed |
| `cleared` | ClearedEventArgs | Triggers when datepicker value is cleared using clear button |
| `close` | PreventableEventArgs \| PopupObjectArgs | Triggers when the popup is closed |
| `created` | Object | Triggers when the component is created |
| `destroyed` | Object | Triggers when the component is destroyed |
| `focus` | FocusEventArgs | Triggers when the input gets focus |
| `navigated` | NavigatedEventArgs | Triggers when Calendar is navigated to another level |
| `open` | PreventableEventArgs \| PopupObjectArgs | Triggers when the popup is opened |
| `renderDayCell` | RenderDayCellEventArgs | Triggers when each day cell of the Calendar is rendered |

