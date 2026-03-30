# DateTimePicker API Reference

## Table of Contents
- [Properties](#properties)
- [Methods](#methods)
- [Events](#events)
- [Type summaries](#type-summaries)
- [Usage examples](#usage-examples)

## Properties

Below are the most commonly used properties. For each property the typical default is shown.

- `allowEdit` (boolean) — Defaults: `true`.
  - If `false`, input textbox is read-only and users must pick from the popup.

- `calendarMode` (CalendarType) — Defaults: `Gregorian`.
  - Choose calendar system (gregorian, islamic, etc.).

- `cssClass` (string) — Defaults: `null`.
  - Adds a custom root CSS class for styling.

- `dayHeaderFormat` (DayHeaderFormats) — Defaults: `Short`.
  - Controls day header format (Short, Narrow, Abbreviated, Wide).

- `depth` (CalendarView) — Defaults: `Month`.
  - Max navigation depth (Month / Year / Decade).

- `enableMask` (boolean) — Defaults: `false`.
  - When `true`, renders with masked input behavior.

- `enablePersistence` (boolean) — Defaults: `false`.
  - Persist state (value) across reloads.

- `enableRtl` (boolean) — Defaults: `false`.
  - Render layout right-to-left when `true`.

- `enabled` (boolean) — Defaults: `true`.
  - Component enabled/disabled state.

- `firstDayOfWeek` (number) — Defaults: `0`.
  - Day index used as the first day in calendar (0 = Sunday).

- `floatLabelType` (FloatLabelType | string) — Defaults: `Never`.
  - Controls floating placeholder label behavior (Never, Always, Auto).

- `format` (string | FormatObject) — Defaults: `null`.
  - Display/parse format for the value (e.g., `dd/MM/yyyy hh:mm` or skeleton objects).

- `fullScreenMode` (boolean) — Defaults: `false`.
  - Popup displays full screen on mobile when `true`.

- `htmlAttributes` ({ [key: string]: string }) — Defaults: `{}`.
  - Additional attributes applied to the input element.

- `inputFormats` (string[] | FormatObject[]) — Defaults: `null`.
  - Array of acceptable parse formats.

- `keyConfigs` ({ [key: string]: string }) — Defaults: `null`.
  - Customize keyboard action shortcuts.

- `locale` (string) — Defaults: `''`.
  - Override global culture for this component.

- `maskPlaceholder` (MaskPlaceholderModel) — Defaults: built-in labels.
  - Placeholder texts for masked input parts.

- `max` (Date) — Defaults: `new Date(2099, 11, 31)`.
  - Maximum selectable date.

- `maxTime` (Date) — Defaults: `null`.
  - Maximum selectable time inside time picker.

- `min` (Date) — Defaults: `new Date(1900, 0, 1)`.
  - Minimum selectable date.

- `minTime` (Date) — Defaults: `null`.
  - Minimum selectable time inside time picker.

- `openOnFocus` (boolean) — Defaults: `false`.
  - When true, popup opens on input focus.

- `placeholder` (string) — Defaults: `null`.
  - Placeholder text in the input.

- `readonly` (boolean) — Defaults: `false`.
  - Make component readonly.

- `scrollTo` (Date) — Defaults: `null`.
  - Scroll time list to this time when opened if no value selected.

- `serverTimezoneOffset` (number) — Defaults: `null`.
  - Process initial value using server timezone offset.

- `showClearButton` (boolean) — Defaults: `true`.
  - Show/hide clear icon.

- `showTodayButton` (boolean) — Defaults: `true`.
  - Show/hide "Today" button in popup.

- `start` (CalendarView) — Defaults: `Month`.
  - Initial calendar view when popup opens.

- `step` (number) — Defaults: `30`.
  - Time interval in minutes between adjacent time entries.

- `strictMode` (boolean) — Defaults: `false`.
  - When `true` rejects invalid/out-of-range input and resets to previous value.

- `timeFormat` (string) — Defaults: `null`.
  - Format used for times in the time popup list.

- `value` (Date) — Defaults: `null`.
  - Selected date-time value.

- `weekNumber` (boolean) — Defaults: `false`.
  - Show week numbers in calendar.

- `weekRule` (WeekRule) — Defaults: `FirstDay`.
  - Rule for determining first week of year.

- `width` (number | string) — Defaults: `null`.
  - Component width.

- `zIndex` (number) — Defaults: `1000`.
  - z-index for popup element.


## Methods

- `currentView(): string` — Returns current calendar view string ("Month", "Year", "Decade").
- `destroy(): void` — Destroys the widget and removes event listeners.
- `focusIn(): void` — Sets focus to the input element.
- `focusOut(): void` — Removes focus from the input element.
- `getPersistData(): string` — Returns a serialized string of persistable properties.
- `navigateTo(view: CalendarView, date: Date): void` — Navigate calendar to specified view/date.
- `onPropertyChanged(newProp, oldProp): void` — Internal; invoked when properties change.
- `removeDate(dates: Date | Date[]): void` — Remove single/multiple dates (Calendar values API).


## Events

- `blur` — Fires when the input loses focus.
- `change` (EmitType<ChangedEventArgs>) — Fires when selected value changes; handler receives `{ value, element, model, ... }`.
- `cleared` (EmitType<ClearedEventArgs>) — Fires when value is cleared using clear button.
- `close` — Fires when popup closes.
- `created` — Fires after component is created.
- `destroyed` — Fires after component is destroyed.
- `focus` — Fires when input gains focus.
- `navigated` (EmitType<NavigatedEventArgs>) — Fires when calendar view navigates.
- `open` — Fires when popup opens.
- `renderDayCell` (EmitType<RenderDayCellEventArgs>) — Fired for each day cell render; allows customizing day cell (disable, add class, template).


## Type Summaries

- `ChangedEventArgs` — available properties typically include `value: Date`, `event: Event`, `element`, `model`.
- `NavigatedEventArgs` — includes `view`, `element`, `date` for the navigated view.
- `RenderDayCellEventArgs` — includes `date`, `view`, and `cell` to allow DOM customization.


## Usage Examples

### Basic controlled functional component

```tsx
import React, { useState } from 'react';
import { DateTimePickerComponent } from '@syncfusion/ej2-react-calendars';

export default function DateTimeExample() {
  const [value, setValue] = useState<Date | null>(new Date());

  const handleChange = (args: any) => {
    setValue(args.value);
    console.log('change', args.value);
  };

  return (
    <DateTimePickerComponent
      value={value}
      change={handleChange}
      format="MM/dd/yyyy hh:mm a"
      step={15}
      placeholder="Select date and time"
    />
  );
}
```

### Example: min/max and time constraints

```tsx
<DateTimePickerComponent
  min={new Date(2023, 0, 1)}
  max={new Date(2024, 11, 31)}
  minTime={new Date(0,0,0,8,0)}  // 8:00 AM
  maxTime={new Date(0,0,0,18,0)} // 6:00 PM
  step={30}
/>
```

### Example: masked input and strict mode

```tsx
<DateTimePickerComponent
  enableMask={true}
  maskPlaceholder={{ day: 'dd', month: 'mm', year: 'yyyy', hour: 'hh', minute: 'mm' }}
  strictMode={true}
/>
```

### Example: listen to open/close and customize day cell

```tsx
<DateTimePickerComponent
  open={() => console.log('opened')}
  close={() => console.log('closed')}
  renderDayCell={(args: any) => {
    // disable weekends
    const day = args.date.getDay();
    if (day === 0 || day === 6) {
      (args as any).isDisabled = true;
    }
  }}
/>
```

## Notes & References
- This file summarizes the official API. For any updates, refer to the Syncfusion docs:
  https://ej2.syncfusion.com/react/documentation/api/datetimepicker/index-default

