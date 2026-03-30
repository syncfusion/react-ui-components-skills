# Advanced Features

## Table of Contents
- [Masked Input Support](#masked-input-support)
- [Strict Mode Validation](#strict-mode-validation)
- [Calendar Modes](#calendar-modes)
- [Server Timezone Handling](#server-timezone-handling)
- [Day Header Formats](#day-header-formats)
- [Week Number Display](#week-number-display)
- [Keyboard Configuration](#keyboard-configuration)
- [Persistence](#persistence)
- [Allow/Disable Edit](#allowdisable-edit)

## Masked Input Support

Enable masked input for guided date/time entry. Users enter values in placeholder format.

### Basic Masked Input

```tsx
<DateTimePickerComponent
  enableMask={true}
  placeholder="Enter date and time"
/>
```

When enabled, the input follows mask format:
- Format: **DD/MM/YYYY HH:MM**
- User types: **2 5 1 2 2 0 2 5 1 4 3 0** → converts to **25/12/2025 14:30**

### Custom Mask Placeholders

```tsx
<DateTimePickerComponent
  enableMask={true}
  maskPlaceholder={{
    day: 'dd',      // Two-digit day
    month: 'mm',    // Two-digit month
    year: 'yyyy',   // Four-digit year
    hour: 'hh',     // Two-digit hour
    minute: 'mm',   // Two-digit minute
    second: 'ss',   // Two-digit second
    dayOfTheWeek: 'ddd'  // Day name
  }}
/>
```

### Example: Masked Input for Data Entry

```tsx
import React, { useState } from 'react';
import { DateTimePickerComponent } from '@syncfusion/ej2-react-calendars';

export default function MaskedInputExample() {
  const [value, setValue] = useState<Date | null>(null);

  return (
    <div style={{ padding: '20px', maxWidth: '400px' }}>
      <h3>Appointment Booking (Masked Entry)</h3>
      <DateTimePickerComponent
        value={value}
        change={(e) => setValue((e as any).value)}
        enableMask={true}
        maskPlaceholder={{
          day: 'dd',
          month: 'mm',
          year: 'yyyy',
          hour: 'hh',
          minute: 'mm'
        }}
        format="dd/MM/yyyy hh:mm"
      />
      <p>Selected: {value?.toLocaleString()}</p>
    </div>
  );
}
```

## Strict Mode Validation

In strict mode, invalid or out-of-range values are rejected and reset to the previous valid value.

### Enabling Strict Mode

```tsx
// Without strict mode (default): invalid input is highlighted but accepted
<DateTimePickerComponent strictMode={false} />

// With strict mode: invalid input reverts to previous value
<DateTimePickerComponent strictMode={true} />
```

### Example: Strict Mode with Constraints

```tsx
import React, { useState } from 'react';
import { DateTimePickerComponent } from '@syncfusion/ej2-react-calendars';

export default function StrictModeExample() {
  const [value, setValue] = useState<Date | null>(new Date(2025, 0, 15, 10, 0));

  return (
    <div style={{ padding: '20px' }}>
      <h3>Strict Validation</h3>
      <DateTimePickerComponent
        value={value}
        change={(e) => setValue((e as any).value)}
        min={new Date(2025, 0, 1)}
        max={new Date(2025, 11, 31)}
        minTime={new Date(0, 0, 0, 9, 0)}
        maxTime={new Date(0, 0, 0, 17, 0)}
        strictMode={true}
        format="MM/dd/yyyy HH:mm"
        placeholder="2025 dates, 9 AM - 5 PM only"
      />
      <p>Current value: {value?.toLocaleString()}</p>
      <p style={{ fontSize: '12px', color: '#666' }}>
        Try entering an invalid date (e.g., 01/01/2024 or 8:00 AM) — it will revert automatically.
      </p>
    </div>
  );
}
```

### When to Use Strict Mode

- **Enable (true)** for critical data entry (financial transactions, medical records)
- **Disable (false)** for casual inputs or when you want to allow user corrections

## Calendar Modes

Support different calendar systems (Gregorian, Islamic, etc.).

### Gregorian Calendar (Default)

```tsx
<DateTimePickerComponent
  calendarMode="Gregorian"
  placeholder="Gregorian calendar"
/>
```

### Islamic Calendar

```tsx
<DateTimePickerComponent
  calendarMode="Islamic"
  placeholder="Islamic calendar"
  locale="ar"
/>
```

### Example: Switchable Calendar Modes

```tsx
import React, { useState } from 'react';
import { DateTimePickerComponent } from '@syncfusion/ej2-react-calendars';

export default function CalendarModeSwitch() {
  const [mode, setMode] = useState<'Gregorian' | 'Islamic'>('Gregorian');

  return (
    <div style={{ padding: '20px' }}>
      <label>
        Calendar Mode:
        <select
          value={mode}
          onChange={(e) => setMode(e.target.value as 'Gregorian' | 'Islamic')}
        >
          <option value="Gregorian">Gregorian</option>
          <option value="Islamic">Islamic</option>
        </select>
      </label>

      <DateTimePickerComponent
        calendarMode={mode}
        locale={mode === 'Islamic' ? 'ar' : 'en'}
        format="dd/MM/yyyy hh:mm a"
      />
    </div>
  );
}
```

## Server Timezone Handling

Process dates using server timezone offset instead of local browser timezone.

### Basic Server Timezone Offset

```tsx
// Assume server is in UTC+5 and client is in UTC-5 (10-hour difference)
const serverTimezoneOffset = 300; // in minutes (UTC+5)

<DateTimePickerComponent
  value={new Date()}
  serverTimezoneOffset={serverTimezoneOffset}
/>
```

### Example: Multi-Timezone Scheduling

```tsx
import React, { useState } from 'react';
import { DateTimePickerComponent } from '@syncfusion/ej2-react-calendars';

export default function MultiTimezonePicker() {
  const [timezone, setTimezone] = useState('UTC+0');

  const getTimezoneOffset = (tz: string): number => {
    const offsets: Record<string, number> = {
      'UTC-8': -480,  // PST
      'UTC-5': -300,  // EST
      'UTC+0': 0,     // GMT
      'UTC+1': 60,    // CET
      'UTC+5': 300,   // PKT
      'UTC+9': 540,   // JST
    };
    return offsets[tz] || 0;
  };

  return (
    <div style={{ padding: '20px' }}>
      <label>
        Timezone:
        <select value={timezone} onChange={(e) => setTimezone(e.target.value)}>
          <option value="UTC-8">Pacific Time (UTC-8)</option>
          <option value="UTC-5">Eastern Time (UTC-5)</option>
          <option value="UTC+0">UTC</option>
          <option value="UTC+1">Central European (UTC+1)</option>
          <option value="UTC+5">Pakistan (UTC+5)</option>
          <option value="UTC+9">Japan (UTC+9)</option>
        </select>
      </label>

      <DateTimePickerComponent
        serverTimezoneOffset={getTimezoneOffset(timezone)}
        format="MM/dd/yyyy hh:mm a"
        placeholder={`Time in ${timezone}`}
      />
    </div>
  );
}
```

## Day Header Formats

Customize how day names appear in the calendar header.

### Available Formats

```tsx
// Short: Su, Mo, Tu, etc. (default)
<DateTimePickerComponent dayHeaderFormat="Short" />

// Abbreviated: Sun, Mon, Tue, etc.
<DateTimePickerComponent dayHeaderFormat="Abbreviated" />

// Narrow: S, M, T, etc. (single character)
<DateTimePickerComponent dayHeaderFormat="Narrow" />

// Wide: Sunday, Monday, Tuesday, etc. (full names)
<DateTimePickerComponent dayHeaderFormat="Wide" />
```

### Example: Format Comparison

```tsx
import React, { useState } from 'react';
import { DateTimePickerComponent } from '@syncfusion/ej2-react-calendars';

export default function DayHeaderFormatExample() {
  const [format, setFormat] = useState<'Short' | 'Abbreviated' | 'Narrow' | 'Wide'>('Short');

  return (
    <div style={{ padding: '20px' }}>
      <label>
        Day Header Format:
        <select value={format} onChange={(e) => setFormat(e.target.value as any)}>
          <option value="Short">Short (Su, Mo)</option>
          <option value="Abbreviated">Abbreviated (Sun, Mon)</option>
          <option value="Narrow">Narrow (S, M)</option>
          <option value="Wide">Wide (Sunday, Monday)</option>
        </select>
      </label>

      <DateTimePickerComponent
        dayHeaderFormat={format}
        placeholder="See header format change"
      />
    </div>
  );
}
```

## Week Number Display

Show or hide week numbers in the calendar.

### Enable Week Numbers

```tsx
<DateTimePickerComponent
  weekNumber={true}
  placeholder="Calendar with week numbers"
/>
```

```tsx
// Disable (default)
<DateTimePickerComponent
  weekNumber={false}
  placeholder="Calendar without week numbers"
/>
```

### Example: Week Selection Use Case

```tsx
import React, { useState } from 'react';
import { DateTimePickerComponent } from '@syncfusion/ej2-react-calendars';

export default function WeekNumberExample() {
  const [showWeeks, setShowWeeks] = useState(true);

  return (
    <div style={{ padding: '20px' }}>
      <label>
        <input
          type="checkbox"
          checked={showWeeks}
          onChange={(e) => setShowWeeks(e.target.checked)}
        />
        Show Week Numbers
      </label>

      <DateTimePickerComponent
        weekNumber={showWeeks}
        placeholder="Select date (note week numbers)"
      />

      <p style={{ fontSize: '12px', color: '#666' }}>
        Week numbers help with planning and scheduling.
      </p>
    </div>
  );
}
```

## Keyboard Configuration

Customize keyboard shortcuts for navigation and selection.

### Default Keyboard Actions

| Action | Key | Description |
|--------|-----|-------------|
| altUpArrow | Alt + ↑ | Expand/collapse popup |
| altDownArrow | Alt + ↓ | Open/close popup |
| moveUp | ↑ | Navigate up in calendar |
| moveDown | ↓ | Navigate down in calendar |
| moveLeft | ← | Navigate left in calendar |
| moveRight | → | Navigate right in calendar |
| select | Enter | Select date |
| home | Home | Jump to first day of month |
| end | End | Jump to last day of month |
| pageUp | PageUp | Previous month |
| pageDown | PageDown | Next month |

### Custom Keyboard Configuration

```tsx
<DateTimePickerComponent
  keyConfigs={{
    select: 'space',           // Use spacebar to select
    home: 'ctrl+home',         // Custom home key
    end: 'ctrl+end',           // Custom end key
    moveUp: 'w',               // Use W key to go up
    moveDown: 's',             // Use S key to go down
    moveLeft: 'a',             // Use A key to go left
    moveRight: 'd',            // Use D key to go right
  }}
/>
```

### Example: Custom Keyboard Navigation

```tsx
import React from 'react';
import { DateTimePickerComponent } from '@syncfusion/ej2-react-calendars';

export default function CustomKeyboardExample() {
  const customKeys = {
    select: 'space',
    home: 'ctrl+home',
    end: 'ctrl+end',
    moveUp: 'arrowup',
    moveDown: 'arrowdown',
    moveLeft: 'arrowleft',
    moveRight: 'arrowright',
    pageUp: 'pageup',
    pageDown: 'pagedown',
  };

  return (
    <div style={{ padding: '20px' }}>
      <h3>Custom Keyboard Navigation</h3>
      <DateTimePickerComponent
        keyConfigs={customKeys}
        placeholder="Use arrow keys to navigate"
      />
      <ul style={{ fontSize: '12px' }}>
        <li>↑↓←→ to navigate</li>
        <li>Space to select</li>
        <li>Ctrl+Home for first day</li>
        <li>Ctrl+End for last day</li>
      </ul>
    </div>
  );
}
```

## Persistence

Enable state persistence across page reloads.

### Enable Persistence

```tsx
<DateTimePickerComponent
  enablePersistence={true}
  id="myDateTimePicker"
/>
```

When enabled, the following state is persisted to localStorage:
- `value` - The selected date and time

### Example: Persistence with ID

```tsx
import React, { useState } from 'react';
import { DateTimePickerComponent } from '@syncfusion/ej2-react-calendars';

export default function PersistenceExample() {
  const [value, setValue] = useState<Date | null>(new Date());

  return (
    <div style={{ padding: '20px' }}>
      <h3>Persistent DateTimePicker</h3>
      <DateTimePickerComponent
        id="persistentPicker"
        value={value}
        change={(e) => setValue((e as any).value)}
        enablePersistence={true}
        placeholder="Selection persists on reload"
      />
      <p>Reload the page — the selected value should remain!</p>
    </div>
  );
}
```

## Allow/Disable Edit

Control whether users can manually edit the input or must select from the popup.

### Allow Edit (Default)

```tsx
// User can type or select from popup
<DateTimePickerComponent
  allowEdit={true}
  placeholder="Can type or use popup"
/>
```

### Disable Edit

```tsx
// User can only select from popup, input is read-only
<DateTimePickerComponent
  allowEdit={false}
  placeholder="Popup only"
/>
```

### Example: Conditional Edit Mode

```tsx
import React, { useState } from 'react';
import { DateTimePickerComponent } from '@syncfusion/ej2-react-calendars';

export default function EditModeToggle() {
  const [allowEdit, setAllowEdit] = useState(true);

  return (
    <div style={{ padding: '20px' }}>
      <label>
        <input
          type="checkbox"
          checked={allowEdit}
          onChange={(e) => setAllowEdit(e.target.checked)}
        />
        Allow Manual Edit
      </label>

      <DateTimePickerComponent
        allowEdit={allowEdit}
        placeholder={allowEdit ? 'Can type or select' : 'Popup only'}
      />
    </div>
  );
}
```

## Summary

- **Masked Input**: Guided entry with `enableMask` and `maskPlaceholder`.
- **Strict Mode**: Reject invalid input and revert to previous value.
- **Calendar Modes**: Support Gregorian and Islamic calendars.
- **Timezone Handling**: Use `serverTimezoneOffset` for server-side dates.
- **Day Headers**: Customize with `dayHeaderFormat` (Short, Abbreviated, Narrow, Wide).
- **Week Numbers**: Enable with `weekNumber={true}`.
- **Keyboard Config**: Customize shortcuts with `keyConfigs`.
- **Persistence**: Enable with `enablePersistence` to save across reloads.
- **Edit Control**: Use `allowEdit` to toggle manual entry vs popup-only mode.

