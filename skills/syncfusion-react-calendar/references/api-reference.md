# API Reference

Source: Syncfusion React Calendar API documentation — https://ej2.syncfusion.com/react/documentation/api/calendar/index-default

## Table of Contents
- [Properties](#properties)
- [Methods](#methods)
- [Events](#events)
- [Enums](#enums)
- [Examples](#examples)

---

## Properties

| Property | Type | Default | Description |
|---|---|---|---|
| `calendarMode` | `CalendarType` | `'Gregorian'` | Gets or sets the Calendar's type — `'Gregorian'` or `'Islamic'`. |
| `cssClass` | `string` | `null` | Root CSS class for the Calendar, used to override styles. |
| `dayHeaderFormat` | `DayHeaderFormats` | `'Short'` | Format of day names in the header. Options: `'Short'`, `'Narrow'`, `'Abbreviated'`, `'Wide'`. |
| `depth` | `CalendarView` | `'Month'` | Maximum navigation level. Must be ≤ `start`. Options: `'Month'`, `'Year'`, `'Decade'`. |
| `enablePersistence` | `boolean` | `false` | Persists the `value` state across page reloads. |
| `enableRtl` | `boolean` | `false` | Renders the component in right-to-left direction. |
| `enabled` | `boolean` | `true` | Enables or disables the component. |
| `firstDayOfWeek` | `number` | `0` | First day of the week (0 = Sunday). Defaults to current culture. |
| `isMultiSelection` | `boolean` | `false` | Enables multiple date selection. Use with the `values` prop. |
| `keyConfigs` | `{ [key: string]: string }` | `null` | Customizes keyboard shortcut mappings. |
| `locale` | `string` | `''` | Overrides global culture/localization (e.g., `'en-US'`, `'fr-FR'`). |
| `max` | `Date` | `new Date(2099, 11, 31)` | Maximum selectable date. |
| `min` | `Date` | `new Date(1900, 00, 01)` | Minimum selectable date. |
| `serverTimezoneOffset` | `number` | `null` | Processes the initial date using a server time zone offset. |
| `showTodayButton` | `boolean` | `true` | Shows or hides the Today button. |
| `start` | `CalendarView` | `'Month'` | Initial view when the Calendar opens. Options: `'Month'`, `'Year'`, `'Decade'`. |
| `value` | `Date` | `null` | The selected date. |
| `values` | `Date[]` | `null` | Multiple selected dates. Used with `isMultiSelection={true}`. |
| `weekNumber` | `boolean` | `false` | Shows the ISO week number of the year. |
| `weekRule` | `WeekRule` | `'FirstDay'` | Rule for defining the first week of the year. |

> **Tip:** For controlled usage in React, pass `value` and update it in `change` event handlers.

### `keyConfigs` Example

```jsx
import React from 'react';
import { CalendarComponent } from '@syncfusion/ej2-react-calendars';

export default function KeyConfigsExample() {
  const keyConfigs = {
    select: 'space',
    home: 'ctrl+home',
    end: 'ctrl+end',
  };

  return <CalendarComponent keyConfigs={keyConfigs} />;
}
```

### `isMultiSelection` + `values` Example

```jsx
import React from 'react';
import { CalendarComponent } from '@syncfusion/ej2-react-calendars';

export default function MultiSelectExample() {
  const values = [new Date('11/20/2026'), new Date('11/28/2026'), new Date('11/02/2026')];

  return <CalendarComponent isMultiSelection={true} values={values} />;
}
```

---

## Methods

Call these methods via a `ref` to the `CalendarComponent` instance.

| Method | Signature | Returns | Description |
|---|---|---|---|
| `addDate` | `addDate(dates: Date \| Date[]): void` | `void` | Adds one or multiple dates to the `values` property. |
| `currentView` | `currentView(): string` | `string` | Returns the current view — `'Month'`, `'Year'`, or `'Decade'`. |
| `destroy` | `destroy(): void` | `void` | Destroys the widget and cleans up resources. |
| `getPersistData` | `getPersistData(): string` | `string` | Returns the properties maintained on browser refresh. |
| `navigateTo` | `navigateTo(view: CalendarView, date: Date, isCustomDate?: boolean): void` | `void` | Navigates to the specified view and focused date. |
| `removeDate` | `removeDate(dates: Date \| Date[]): void` | `void` | Removes one or multiple dates from the `values` property. |

### `navigateTo` Example

```jsx
import React, { useRef } from 'react';
import { CalendarComponent } from '@syncfusion/ej2-react-calendars';

export default function NavigateToExample() {
  const calendarRef = useRef(null);

  const goToJuly2026 = () => {
    if (calendarRef.current) {
      // Signature: navigateTo(view, date, isCustomDate?)
      calendarRef.current.navigateTo('Month', new Date(2026, 6, 1));
    }
  };

  return (
    <div>
      <CalendarComponent ref={calendarRef} />
      <button onClick={goToJuly2026}>Go to July 2026</button>
    </div>
  );
}
```

### `addDate` / `removeDate` Example

```jsx
import React, { useRef } from 'react';
import { CalendarComponent } from '@syncfusion/ej2-react-calendars';

export default function AddRemoveDateExample() {
  const calendarRef = useRef(null);

  const addDates = () => {
    calendarRef.current?.addDate([new Date(2026, 10, 5), new Date(2026, 10, 10)]);
  };

  const removeDates = () => {
    calendarRef.current?.removeDate(new Date(2026, 10, 5));
  };

  return (
    <div>
      <CalendarComponent ref={calendarRef} isMultiSelection={true} />
      <button onClick={addDates}>Add Dates</button>
      <button onClick={removeDates}>Remove Date</button>
    </div>
  );
}
```

### `currentView` Example

```jsx
const getView = () => {
  if (calendarRef.current) {
    console.log('Current view:', calendarRef.current.currentView());
    // Returns: 'Month', 'Year', or 'Decade'
  }
};
```

---

## Events

| Event | Type | Description |
|---|---|---|
| `change` | `EmitType<ChangedEventArgs>` | Fires when the Calendar value is changed. |
| `created` | `EmitType<Object>` | Fires when the Calendar is created. |
| `destroyed` | `EmitType<Object>` | Fires when the Calendar is destroyed. |
| `navigated` | `EmitType<NavigatedEventArgs>` | Fires when the Calendar navigates to another level or within the same level. |
| `renderDayCell` | `EmitType<RenderDayCellEventArgs>` | Fires when each day cell is rendered; use to customize cells. |

### `change` Event Example

```jsx
const onChange = (args) => {
  // args.value — the selected Date
  console.log('Selected:', args.value);
};

<CalendarComponent change={onChange} />
```

### `navigated` Event Example

```jsx
const onNavigated = (args) => {
  console.log('Navigated:', args);
};

<CalendarComponent navigated={onNavigated} />
```

### `renderDayCell` Event Example

```jsx
const renderDayCell = (args) => {
  // args.date — Date for this cell
  // args.cellElement — the DOM element
  // args.isDisabled — set true to disable
  const day = args.date.getDay();
  if (day === 0 || day === 6) {
    args.isDisabled = true;
  }
};

<CalendarComponent renderDayCell={renderDayCell} />
```

---

## Enums

### CalendarView

Used for `start`, `depth` props, and the `navigateTo` method.

| Value | Description |
|---|---|
| `'Month'` | Month grid view (default) |
| `'Year'` | Yearly view showing all 12 months |
| `'Decade'` | Decade view showing a 10-year range |

### CalendarType

Used for the `calendarMode` prop.

| Value | Description |
|---|---|
| `'Gregorian'` | Standard Gregorian calendar (default) |
| `'Islamic'` | Islamic (Hijri) calendar |

### DayHeaderFormats

Used for the `dayHeaderFormat` prop.

| Value | Example | Description |
|---|---|---|
| `'Short'` | Su | Short format (default) |
| `'Narrow'` | S | Single character |
| `'Abbreviated'` | Sun | Abbreviated format |
| `'Wide'` | Sunday | Full day name |

### WeekRule

Used for the `weekRule` prop.

| Value | Description |
|---|---|
| `'FirstDay'` | Week 1 starts on the first day of the year (default) |
| `'FirstFourDayWeek'` | Week 1 is the week with ≥ 4 days in the new year |
| `'FirstFullWeek'` | Week 1 is the first week fully in the new year |

---

## Examples

### Controlled Calendar with min/max

```jsx
import React, { useState } from 'react';
import { CalendarComponent } from '@syncfusion/ej2-react-calendars';

export default function ControlledExample() {
  const [value, setValue] = useState(new Date());

  return (
    <CalendarComponent
      value={value}
      min={new Date(2020, 0, 1)}
      max={new Date(2030, 11, 31)}
      change={(e) => setValue(e.value)}
    />
  );
}
```

### Week Numbers + Today Button

```jsx
<CalendarComponent weekNumber={true} showTodayButton={true} />
```

### Islamic Calendar Mode

```jsx
<CalendarComponent calendarMode="Islamic" />
```

### Multi-Selection (controlled)

```jsx
import React, { useState } from 'react';
import { CalendarComponent } from '@syncfusion/ej2-react-calendars';

export default function MultiSelectControlled() {
  const [values, setValues] = useState([
    new Date(2026, 10, 5),
    new Date(2026, 10, 12),
  ]);

  return (
    <CalendarComponent
      isMultiSelection={true}
      values={values}
      change={(args) => {
        if (args.values) setValues(args.values);
      }}
    />
  );
}
```

---

## Official Documentation

- API Reference: https://ej2.syncfusion.com/react/documentation/api/calendar/index-default
- CalendarView enum: https://ej2.syncfusion.com/react/documentation/api/calendar/calendarview
- ChangedEventArgs: https://ej2.syncfusion.com/react/documentation/api/calendar/changedeventargs
- NavigatedEventArgs: https://ej2.syncfusion.com/react/documentation/api/calendar/navigatedeventargs
- RenderDayCellEventArgs: https://ej2.syncfusion.com/react/documentation/api/calendar/renderdaycelleventargs
