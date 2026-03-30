# DateRangePicker API Reference

## Table of Contents
- [Properties](#properties)
- [Methods](#methods)
- [Events](#events)
- [Interfaces](#interfaces)
- [Enums](#enums)

---

## Properties

Complete list of DateRangePicker properties (sourced from the official Syncfusion React API):

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `allowEdit` | boolean | true | Specifies whether the input textbox is editable or not. When false, user can only select from the popup. |
| `cssClass` | string | '' | Sets the root CSS class to the DateRangePicker which allows you to customize the appearance. |
| `dayHeaderFormat` | DayHeaderFormats | Short | Specifies the format of the day displayed in the header (Short, Narrow, Abbreviated, Wide). |
| `depth` | CalendarView | Month | Sets the maximum level of view (Month, Year, Decade) in the Calendar. Depth view should be smaller than the start view. |
| `enablePersistence` | boolean | false | Enable or disable persisting component state (startDate, endDate, value) between page reloads. |
| `enableRtl` | boolean | false | Enable or disable rendering component in right to left direction. |
| `enabled` | boolean | true | Specifies whether the component is enabled. Setting to false disables the DateRangePicker and prevents user interactions. |
| `endDate` | Date | null | Gets or sets the end date of the date range selection. |
| `firstDayOfWeek` | number | null | Gets or sets the Calendar's first day of the week. Defaults to culture-based value. |
| `floatLabelType` | FloatLabelType \| string | Never | Specifies the placeholder text floating behavior (Never, Always, Auto). |
| `format` | string \| RangeFormatObject | null | Sets or gets the required date format for the start and end date strings. |
| `fullScreenMode` | boolean | false | Specifies whether the component popup displays in full screen in mobile devices. |
| `htmlAttributes` | { [key: string]: string } | {} | Adds additional HTML attributes such as disabled, value etc. to the element. |
| `inputFormats` | string[] \| RangeFormatObject[] | null | Specifies an array of acceptable date input formats for parsing user input. |
| `keyConfigs` | { [key: string]: string } | null | Customizes the key actions in DateRangePicker. |
| `locale` | string | 'en-US' | Overrides the global culture and localization value for this component. |
| `max` | Date | new Date(2099, 11, 31) | Gets or sets the maximum date that can be selected in the calendar-popup. |
| `maxDays` | number | null | Specifies the maximum span of days that can be allowed in a date range selection. |
| `min` | Date | new Date(1900, 00, 01) | Gets or sets the minimum date that can be selected in the calendar-popup. |
| `minDays` | number | null | Specifies the minimum span of days that can be allowed in date range selection. |
| `openOnFocus` | boolean | false | Opens the popup while focusing the daterange input when set to true. |
| `placeholder` | string | null | Specifies the placeholder text displayed in the DateRangePicker component. |
| `presets` | PresetsModel[] | null | Sets the predefined ranges which let the user pick a required range easily. |
| `readonly` | boolean | false | Denies editing the ranges in the DateRangePicker component. |
| `separator` | string | '-' | Sets or gets the string used between the start and end date string. |
| `serverTimezoneOffset` | number | null | Specifies the server time zone offset for processing the initial date value. |
| `showClearButton` | boolean | true | Specifies whether to show or hide the clear icon. |
| `start` | CalendarView | Month | Specifies the initial view of the Calendar when it is opened (Month, Year, Decade). |
| `startDate` | Date | null | Gets or sets the start date of the date range selection. |
| `strictMode` | boolean | false | Specifies the component to act as strict, allowing only valid date range input. |
| `value` | Date[] \| DateRange | null | Gets or sets the start and end date of the Calendar. |
| `weekNumber` | boolean | false | Determines whether the week number of the Calendar is displayed or not. |
| `weekRule` | WeekRule | FirstDay | Specifies the rule for defining the first week of the year. |
| `width` | number \| string | '' | Specifies the width of the DateRangePicker component. |
| `zIndex` | number | 1000 | Specifies the z-index value of the dateRangePicker popup element. |

### Key Properties in Context

**Essential for Date Range:**
```typescript
// Core range properties
startDate: Date = new Date(2024, 9, 1);
endDate: Date = new Date(2024, 10, 15);

// Constraints
min: Date = new Date(2024, 0, 1);
max: Date = new Date(2024, 11, 31);

// Min/Max span of days
minDays: number = 2;
maxDays: number = 30;
```

**For Styling:**
```typescript
cssClass: string = "custom-drp primary-theme";
width: string = "100%";
floatLabelType: string = 'Auto';
```

**For Localization:**
```typescript
locale: string = 'de-DE';
format: string = 'd.M.yyyy';
enableRtl: boolean = true;
```

**For Presets:**
```typescript
presets: PresetsModel[] = [
  { label: 'Today', start: new Date(), end: new Date() },
  { label: 'This Month', start: new Date(new Date().setDate(1)), end: new Date() }
];
```

---

## Methods

DateRangePicker component methods for programmatic control:

| Method | Returns | Description |
|--------|---------|-------------|
| `destroy()` | void | Destroys the widget. |
| `focusIn()` | void | Sets the focus to the widget for interaction. |
| `focusOut()` | void | Removes focus from widget, if the widget is in focus state. |
| `getPersistData()` | string | Returns the properties that are maintained upon browser refresh. |
| `getSelectedRange()` | Object | Returns the selected range and day span in the DateRangePicker. |
| `hide()` | void | Closes the Popup container in the DateRangePicker component. |
| `show()` | void | Opens the Popup container in the DateRangePicker component. |

### Method Usage Examples

**Open/Close Popup:**
```typescript
dateRangeRef.current.show();  // Open popup
dateRangeRef.current.hide();  // Close popup
```

**Focus Management:**
```typescript
dateRangeRef.current.focusIn();   // Set focus to widget
dateRangeRef.current.focusOut();  // Remove focus from widget
```

**Get Selected Range:**
```typescript
// Returns selected range object and day span
const rangeInfo = dateRangeRef.current.getSelectedRange();
console.log(rangeInfo);
```

**Get Persist Data:**
```typescript
// Returns JSON string of persistent state properties (startDate, endDate, value)
const persistData = dateRangeRef.current.getPersistData();
console.log(persistData);
```

**Destroy:**
```typescript
dateRangeRef.current.destroy();
```

---

## Events

Complete list of DateRangePicker events:

| Event | Arguments | Description |
|-------|-----------|-------------|
| `blur` | BlurEventArgs | Triggers when the control loses the focus. |
| `change` | RangeEventArgs | Triggers when the Calendar value is changed. |
| `cleared` | ClearedEventArgs | Triggers when daterangepicker value is cleared using the clear button. |
| `close` | Object | Triggers when the DateRangePicker is closed. |
| `created` | Object | Triggers when Calendar is created. |
| `destroyed` | Object | Triggers when Calendar is destroyed. |
| `focus` | FocusEventArgs | Triggers when the control gets focus. |
| `navigated` | NavigatedEventArgs | Triggers when the Calendar is navigated to another view or within the same level of view. |
| `open` | Object | Triggers when the DateRangePicker is opened. |
| `renderDayCell` | RenderDayCellEventArgs | Triggers when each day cell of the Calendar is rendered. |
| `select` | Object | Triggers on selecting the start and end date. |

### Event Handler Signatures

```typescript
// Change event - most commonly used
onChange(args: RangeEventArgs): void {
  console.log('Start:', args.startDate);
  console.log('End:', args.endDate);
  console.log('Value:', args.value);
  console.log('Was user interaction:', args.isInteracted);
}

// Select event - triggered on selecting start and end date
onSelect(args: any): void {
  console.log('Date range selected');
}

// Cleared event - triggered when clear button is clicked
onCleared(args: ClearedEventArgs): void {
  console.log('Value cleared by user');
}

// Open/Close events
onOpen(): void { console.log('Popup opened'); }
onClose(): void { console.log('Popup closed'); }

// Focus/Blur events
onFocus(args: FocusEventArgs): void { }
onBlur(args: BlurEventArgs): void { }

// Lifecycle events
onCreated(): void { console.log('Component created'); }
onDestroyed(): void { console.log('Component destroyed'); }

// Navigation event
onNavigated(args: NavigatedEventArgs): void {
  console.log('View changed:', args.view);
}

// Render day cell event (customize individual cells)
onRenderDayCell(args: RenderDayCellEventArgs): void {
  // Disable weekends
  if (args.date.getDay() === 0 || args.date.getDay() === 6) {
    args.isDisabled = true;
  }
}
```

---

## Interfaces

### RangeEventArgs

Fired when the Calendar value is changed:

```typescript
interface RangeEventArgs {
  startDate: Date;
  endDate: Date;
  value: Date[] | DateRange;
  isInteracted: boolean;
  event?: Event;
  element: HTMLInputElement | HTMLElement;
}
```

### ClearedEventArgs

Fired when the daterangepicker value is cleared using the clear button:

```typescript
interface ClearedEventArgs {
  event: Event;
}
```

### FocusEventArgs

Fired when the control gets focus:

```typescript
interface FocusEventArgs {
  model?: Object;
}
```

### BlurEventArgs

Fired when the control loses focus:

```typescript
interface BlurEventArgs {
  model?: Object;
}
```

### NavigatedEventArgs

Fired when the Calendar is navigated to another view:

```typescript
interface NavigatedEventArgs {
  view: string;
  date: Date;
  event?: Event;
}
```

### RenderDayCellEventArgs

Fired when each day cell of the Calendar is rendered:

```typescript
interface RenderDayCellEventArgs {
  date: Date;
  isDisabled: boolean;
  element: HTMLElement;
}
```

### PresetsModel

For predefined date range options:

```typescript
interface PresetsModel {
  label: string;
  start: Date;
  end: Date;
}
```

### DateRange

Used by the `value` property:

```typescript
interface DateRange {
  start?: Date;
  end?: Date;
}
```

---

## Enums

### FloatLabelType

```typescript
enum FloatLabelType {
  Never = 'Never',    // Label stays as placeholder, never floats
  Always = 'Always',  // Label always floats above the input
  Auto = 'Auto'       // Label floats after focusing or entering a value
}
```

### CalendarView

Used for `start` and `depth` properties:

```typescript
enum CalendarView {
  Month = 'Month',    // Month view (default)
  Year = 'Year',      // Year view
  Decade = 'Decade'   // Decade view
}
```

### DayHeaderFormats

Used for `dayHeaderFormat` property:

```typescript
enum DayHeaderFormats {
  Short = 'Short',              // Short format (e.g., Su)
  Narrow = 'Narrow',            // Single character (e.g., S)
  Abbreviated = 'Abbreviated',  // Abbreviated format (e.g., Sun)
  Wide = 'Wide'                 // Full day name (e.g., Sunday)
}
```

### WeekRule

Used for `weekRule` property:

```typescript
enum WeekRule {
  FirstDay = 'FirstDay',
  FirstFullWeek = 'FirstFullWeek',
  FirstFourDayWeek = 'FirstFourDayWeek'
}
```

---

## Common API Patterns

### Pattern 1: Basic Setup with Key Properties

```tsx
import { DateRangePickerComponent } from '@syncfusion/ej2-react-calendars';
import * as React from 'react';

function BasicSetup() {
  return (
    <DateRangePickerComponent
      id="daterangepicker"
      startDate={new Date(2024, 9, 1)}
      endDate={new Date(2024, 10, 15)}
      min={new Date(2024, 0, 1)}
      max={new Date(2024, 11, 31)}
      minDays={2}
      maxDays={30}
      format="M/d/yyyy"
      locale="en-US"
      placeholder="Select date range"
      allowEdit={true}
      showClearButton={true}
      openOnFocus={false}
      strictMode={false}
      cssClass="custom-drp"
      width="100%"
      htmlAttributes={{ 'aria-label': 'Date range selection' }}
      change={(e) => console.log(e.startDate, e.endDate)}
    />
  );
}
```

### Pattern 2: Controlled Component

```tsx
import { DateRangePickerComponent } from '@syncfusion/ej2-react-calendars';
import * as React from 'react';

function ControlledDateRange() {
  const [startDate, setStartDate] = React.useState<Date | null>(null);
  const [endDate, setEndDate] = React.useState<Date | null>(null);

  return (
    <DateRangePickerComponent
      startDate={startDate}
      endDate={endDate}
      change={(e) => {
        setStartDate(e.startDate);
        setEndDate(e.endDate);
      }}
    />
  );
}
```

### Pattern 3: Preset Ranges

```tsx
import { DateRangePickerComponent, PresetsModel } from '@syncfusion/ej2-react-calendars';
import * as React from 'react';

function PresetRanges() {
  const presets: PresetsModel[] = [
    { label: 'Today', start: new Date(), end: new Date() },
    {
      label: 'Last 7 Days',
      start: new Date(new Date().setDate(new Date().getDate() - 7)),
      end: new Date()
    },
    {
      label: 'This Month',
      start: new Date(new Date().setDate(1)),
      end: new Date()
    }
  ];

  return (
    <DateRangePickerComponent
      presets={presets}
      placeholder="Select date range"
    />
  );
}
```

### Pattern 4: Imperative Method Usage

```tsx
import { DateRangePickerComponent } from '@syncfusion/ej2-react-calendars';
import * as React from 'react';

function ImperativeControl() {
  const dateRangeRef = React.useRef<DateRangePickerComponent>(null);

  return (
    <div>
      <DateRangePickerComponent ref={dateRangeRef} id="daterangepicker" />
      <button onClick={() => dateRangeRef.current?.show()}>Open</button>
      <button onClick={() => dateRangeRef.current?.hide()}>Close</button>
      <button onClick={() => dateRangeRef.current?.focusIn()}>Focus</button>
      <button onClick={() => {
        const range = dateRangeRef.current?.getSelectedRange();
        console.log('Selected range:', range);
      }}>Get Range</button>
      <button onClick={() => {
        if (dateRangeRef.current) dateRangeRef.current.value = null;
      }}>Clear</button>
    </div>
  );
}
```

### Pattern 5: renderDayCell to Disable Weekends

```tsx
import { DateRangePickerComponent, RenderDayCellEventArgs } from '@syncfusion/ej2-react-calendars';
import * as React from 'react';

function DisableWeekends() {
  const handleRenderDayCell = (args: RenderDayCellEventArgs) => {
    const day = args.date.getDay();
    if (day === 0 || day === 6) {
      args.isDisabled = true;
    }
  };

  return (
    <DateRangePickerComponent
      id="daterangepicker"
      placeholder="Select weekdays only"
      renderDayCell={handleRenderDayCell}
    />
  );
}

export default DisableWeekends;
```

