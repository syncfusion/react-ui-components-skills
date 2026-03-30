# Time Format and Display Configuration

## Table of Contents
- [Time Format Strings](#time-format-strings)
- [TimeFormatObject with Skeleton](#timeformatobject-with-skeleton)
- [Locale-Based Formatting](#locale-based-formatting)
- [Placeholder and Float Labels](#placeholder-and-float-labels)
- [HTML Attributes](#html-attributes)
- [Masked Input Mode](#masked-input-mode)
- [Format Examples](#format-examples)

## Time Format Strings

The TimePicker supports standard time format patterns for displaying and parsing time values.

### Common Format Patterns

| Pattern | Description | Example |
|---------|-------------|---------|
| `HH` | 24-hour format (00-23) | 14 |
| `hh` | 12-hour format (01-12) | 02 |
| `mm` | Minutes (00-59) | 30 |
| `ss` | Seconds (00-59) | 45 |
| `a` | AM/PM indicator | AM/PM |
| `A` | Uppercase AM/PM | AM/PM |

### Format String Examples

```tsx
import { TimePickerComponent } from '@syncfusion/ej2-react-calendars';
import * as React from 'react';

function FormatExamples() {
  return (
    <div style={{ padding: '20px' }}>
      <h3>Different Time Formats</h3>

      {/* 24-hour format: 14:30 */}
      <div>
        <label>24-Hour Format (HH:mm):</label>
        <TimePickerComponent
          value={new Date('1/1/2018 2:30 PM')}
          format="HH:mm"
          placeholder="14:30"
        />
      </div>

      {/* 12-hour format with seconds: 02:30:45 PM */}
      <div style={{ marginTop: '15px' }}>
        <label>12-Hour with Seconds (hh:mm:ss a):</label>
        <TimePickerComponent
          value={new Date('1/1/2018 2:30:45 PM')}
          format="hh:mm:ss a"
          placeholder="02:30:45 PM"
        />
      </div>

      {/* Compact format: 2:30 PM */}
      <div style={{ marginTop: '15px' }}>
        <label>Compact Format (h:mm a):</label>
        <TimePickerComponent
          value={new Date('1/1/2018 2:30 PM')}
          format="h:mm a"
          placeholder="2:30 PM"
        />
      </div>

      {/* With seconds: 14:30:45 */}
      <div style={{ marginTop: '15px' }}>
        <label>24-Hour with Seconds (HH:mm:ss):</label>
        <TimePickerComponent
          value={new Date('1/1/2018 2:30:45 PM')}
          format="HH:mm:ss"
          placeholder="14:30:45"
        />
      </div>
    </div>
  );
}

export default FormatExamples;
```

## TimeFormatObject with Skeleton

For more advanced formatting, use the `TimeFormatObject` with the `skeleton` property:

### Skeleton Patterns

| Skeleton | Description | Example Output |
|----------|-------------|-----------------|
| `hm` | Hour and minute (12-hour) | 2:30 PM |
| `Hm` | Hour and minute (24-hour) | 14:30 |
| `hms` | Hour, minute, second (12-hour) | 2:30:45 PM |
| `Hms` | Hour, minute, second (24-hour) | 14:30:45 |

### Using Skeleton Property

```tsx
import { TimePickerComponent } from '@syncfusion/ej2-react-calendars';
import * as React from 'react';

function SkeletonFormatExample() {
  const format12Hour = { skeleton: 'hm' };
  const format24Hour = { skeleton: 'Hm' };
  const formatWithSeconds = { skeleton: 'Hms' };

  return (
    <div style={{ padding: '20px' }}>
      <h3>Skeleton-Based Formats</h3>

      {/* 12-hour: 2:30 PM */}
      <div>
        <label>12-Hour (skeleton: 'hm'):</label>
        <TimePickerComponent
          value={new Date('1/1/2018 2:30 PM')}
          format={format12Hour}
          placeholder="2:30 PM"
        />
      </div>

      {/* 24-hour: 14:30 */}
      <div style={{ marginTop: '15px' }}>
        <label>24-Hour (skeleton: 'Hm'):</label>
        <TimePickerComponent
          value={new Date('1/1/2018 2:30 PM')}
          format={format24Hour}
          placeholder="14:30"
        />
      </div>

      {/* With seconds: 2:30:45 PM */}
      <div style={{ marginTop: '15px' }}>
        <label>With Seconds (skeleton: 'Hms'):</label>
        <TimePickerComponent
          value={new Date('1/1/2018 2:30:45 PM')}
          format={formatWithSeconds}
          placeholder="14:30:45"
        />
      </div>
    </div>
  );
}

export default SkeletonFormatExample;
```

## Locale-Based Formatting

TimePicker automatically formats time based on the component's locale. By default, it uses 'en-US', but you can change it:

### Setting Custom Locale

```tsx
import { TimePickerComponent } from '@syncfusion/ej2-react-calendars';
import * as React from 'react';

function LocaleFormattingExample() {
  const [selectedTime, setSelectedTime] = React.useState(new Date('1/1/2018 2:30 PM'));

  return (
    <div style={{ padding: '20px' }}>
      <h3>Locale-Based Time Formatting</h3>

      {/* English (US) - 2:30 PM */}
      <div>
        <label>English (US) locale:</label>
        <TimePickerComponent
          value={selectedTime}
          locale="en-US"
          change={(e: any) => setSelectedTime(e.value)}
        />
      </div>

      {/* German - 14:30 */}
      <div style={{ marginTop: '15px' }}>
        <label>German locale:</label>
        <TimePickerComponent
          value={selectedTime}
          locale="de-DE"
          format="HH:mm"
        />
      </div>

      {/* French - 14h30 */}
      <div style={{ marginTop: '15px' }}>
        <label>French locale:</label>
        <TimePickerComponent
          value={selectedTime}
          locale="fr-FR"
        />
      </div>

      {/* Arabic - with RTL support */}
      <div style={{ marginTop: '15px' }}>
        <label>Arabic locale (RTL):</label>
        <TimePickerComponent
          value={selectedTime}
          locale="ar-SA"
          enableRtl={true}
        />
      </div>
    </div>
  );
}

export default LocaleFormattingExample;
```

### Locale Format Auto-Detection

| Locale | Default Format | Display |
|--------|-----------------|---------|
| en-US | h:mm a | 2:30 PM |
| de-DE | HH:mm | 14:30 |
| fr-FR | HH:mm | 14:30 |
| ja-JP | H:mm | 14:30 |
| ar-SA | HH:mm | 14:30 |

## Placeholder and Float Labels

### Placeholder Text

The `placeholder` property shows hint text when the TimePicker is empty:

```tsx
import { TimePickerComponent } from '@syncfusion/ej2-react-calendars';
import * as React from 'react';

function PlaceholderExample() {
  return (
    <div style={{ padding: '20px' }}>
      <h3>Placeholder Examples</h3>

      <div>
        <label>With Placeholder:</label>
        <TimePickerComponent
          placeholder="Select appointment time"
        />
      </div>

      <div style={{ marginTop: '15px' }}>
        <label>Without Placeholder:</label>
        <TimePickerComponent />
      </div>

      <div style={{ marginTop: '15px' }}>
        <label>Custom Placeholder:</label>
        <TimePickerComponent
          placeholder="HH:MM AM/PM"
        />
      </div>
    </div>
  );
}

export default PlaceholderExample;
```

### Float Label Types

The `floatLabelType` property controls label behavior in floating label mode:

```tsx
import { TimePickerComponent, FloatLabelType } from '@syncfusion/ej2-react-calendars';
import * as React from 'react';

function FloatLabelExample() {
  return (
    <div style={{ padding: '20px' }}>
      <h3>Float Label Types</h3>

      {/* Never: Label stays in input */}
      <div>
        <label>FloatLabelType.Never:</label>
        <TimePickerComponent
          placeholder="Enter time"
          floatLabelType="Never"
        />
      </div>

      {/* Always: Label always floats above */}
      <div style={{ marginTop: '15px' }}>
        <label>FloatLabelType.Always:</label>
        <TimePickerComponent
          placeholder="Enter time"
          floatLabelType="Always"
        />
      </div>

      {/* Auto: Label floats on focus */}
      <div style={{ marginTop: '15px' }}>
        <label>FloatLabelType.Auto:</label>
        <TimePickerComponent
          placeholder="Enter time"
          floatLabelType="Auto"
        />
      </div>
    </div>
  );
}

export default FloatLabelExample;
```

## HTML Attributes

Use `htmlAttributes` to add custom DOM attributes to the input element:

```tsx
import { TimePickerComponent } from '@syncfusion/ej2-react-calendars';
import * as React from 'react';

function HtmlAttributesExample() {
  const customAttributes = {
    name: 'appointment_time',
    placeholder: 'Select time',
    'data-testid': 'time-picker-input',
    'aria-label': 'Select appointment time',
    title: 'Choose time between 9 AM and 5 PM'
  };

  return (
    <div style={{ padding: '20px' }}>
      <h3>Custom HTML Attributes</h3>

      <TimePickerComponent
        htmlAttributes={customAttributes}
        min={new Date('1/1/2018 9:00 AM')}
        max={new Date('1/1/2018 5:00 PM')}
      />

      <p>Check the input element attributes in browser inspector</p>
    </div>
  );
}

export default HtmlAttributesExample;
```

## Masked Input Mode

Masked input helps users enter time in the correct format by providing visual guidance:

### Enabling Mask

```tsx
import { TimePickerComponent, TimeMaskPlaceholderModel } from '@syncfusion/ej2-react-calendars';
import * as React from 'react';

function MaskedInputExample() {
  const [maskedTime, setMaskedTime] = React.useState(new Date('1/1/2018 10:30 AM'));

  const maskPlaceholder: TimeMaskPlaceholderModel = {
    hour: 'HH',
    minute: 'MM',
    second: 'SS',
  };

  return (
    <div style={{ padding: '20px' }}>
      <h3>Masked Time Input</h3>

      {/* Basic masked input */}
      <div>
        <label>Masked Time Entry (hh:mm a):</label>
        <TimePickerComponent
          value={maskedTime}
          enableMask={true}
          format="hh:mm a"
          maskPlaceholder={maskPlaceholder}
          change={(e: any) => setMaskedTime(e.value)}
        />
      </div>

      {/* 24-hour masked input */}
      <div style={{ marginTop: '15px' }}>
        <label>24-Hour Masked (HH:mm:ss):</label>
        <TimePickerComponent
          value={new Date('1/1/2018 14:30:45')}
          enableMask={true}
          format="HH:mm:ss"
        />
      </div>

      <p style={{ marginTop: '15px', color: '#666' }}>
        Masked input provides guidance for entering time in correct format
      </p>
    </div>
  );
}

export default MaskedInputExample;
```

### Custom Mask Placeholders

```tsx
import { TimePickerComponent } from '@syncfusion/ej2-react-calendars';
import * as React from 'react';

function CustomMaskPlaceholderExample() {
  const customPlaceholder = {
    hour: '00',
    minute: '00',
    second: '00',
  };

  return (
    <div>
      <TimePickerComponent
        enableMask={true}
        format="HH:mm:ss"
        maskPlaceholder={customPlaceholder}
        placeholder="Enter time (HH:MM:SS)"
      />
    </div>
  );
}

export default CustomMaskPlaceholderExample;
```

## Format Examples

### Practical Format Scenarios

#### Scenario 1: Appointment Booking (12-hour with AM/PM)

```tsx
import { TimePickerComponent } from '@syncfusion/ej2-react-calendars';
import * as React from 'react';

function AppointmentBooking() {
  const [appointmentTime, setAppointmentTime] = React.useState<Date | null>(null);

  return (
    <div style={{ padding: '20px' }}>
      <h3>Book Appointment</h3>
      <label>Select Time:</label>
      <TimePickerComponent
        value={appointmentTime}
        format="hh:mm a"
        placeholder="2:30 PM"
        step={30}
        change={(e: any) => setAppointmentTime(e.value)}
      />
      {appointmentTime && (
        <p>
          Appointment booked at: {appointmentTime.toLocaleTimeString('en-US', { hour: '2-digit', minute: '2-digit' })}
        </p>
      )}
    </div>
  );
}

export default AppointmentBooking;
```

#### Scenario 2: 24-Hour Military Time

```tsx
import { TimePickerComponent } from '@syncfusion/ej2-react-calendars';
import * as React from 'react';

function MilitaryTimeFormat() {
  return (
    <div style={{ padding: '20px' }}>
      <h3>Military Time (24-Hour)</h3>
      <label>Enter Time:</label>
      <TimePickerComponent
        value={new Date('1/1/2018 14:30')}
        format="HH:mm"
        placeholder="14:30"
      />
      <p>Format: 24-hour time (00:00 - 23:59)</p>
    </div>
  );
}

export default MilitaryTimeFormat;
```

#### Scenario 3: Duration Display (with seconds)

```tsx
import { TimePickerComponent } from '@syncfusion/ej2-react-calendars';
import * as React from 'react';

function DurationTimeFormat() {
  return (
    <div style={{ padding: '20px' }}>
      <h3>Time Duration with Seconds</h3>
      <label>Start Time:</label>
      <TimePickerComponent
        value={new Date('1/1/2018 9:30:15 AM')}
        format="hh:mm:ss a"
        placeholder="09:30:15 AM"
      />
    </div>
  );
}

export default DurationTimeFormat;
```

#### Scenario 4: Locale-Aware International Format

```tsx
import { TimePickerComponent } from '@syncfusion/ej2-react-calendars';
import * as React from 'react';

function InternationalTimeFormat() {
  const [selectedLocale, setSelectedLocale] = React.useState('en-US');
  const [selectedTime, setSelectedTime] = React.useState(new Date('1/1/2018 2:30 PM'));

  const locales: { [key: string]: string } = {
    'en-US': 'English (US)',
    'de-DE': 'German',
    'fr-FR': 'French',
    'ja-JP': 'Japanese',
  };

  return (
    <div style={{ padding: '20px' }}>
      <h3>International Time Formatting</h3>

      <div>
        <label>Select Locale:</label>
        <select
          value={selectedLocale}
          onChange={(e) => setSelectedLocale(e.target.value)}
          style={{ padding: '5px' }}
        >
          {Object.entries(locales).map(([key, label]) => (
            <option key={key} value={key}>
              {label}
            </option>
          ))}
        </select>
      </div>

      <div style={{ marginTop: '15px' }}>
        <label>Time ({locales[selectedLocale]}):</label>
        <TimePickerComponent
          value={selectedTime}
          locale={selectedLocale}
          change={(e: any) => setSelectedTime(e.value)}
        />
      </div>

      <p style={{ marginTop: '15px', color: '#666' }}>
        Current locale: {locales[selectedLocale]}
      </p>
    </div>
  );
}

export default InternationalTimeFormat;
```

## Best Practices

1. **Consistency:** Use the same format throughout your application
2. **User Expectations:** Use 12-hour format for consumer apps, 24-hour for technical apps
3. **Accessibility:** Always provide a placeholder that shows the expected format
4. **Mobile:** Consider masked input for mobile devices
5. **Internationalization:** Use locale-aware formatting for multi-language apps
6. **Clarity:** For ambiguous times, include AM/PM or use 24-hour format

## Edge Cases

### Empty Value Handling

```tsx
const handleChange = (e: any) => {
  if (e.value === null) {
    console.log('Time cleared');
  } else {
    console.log('Time selected:', e.value.toLocaleTimeString());
  }
};
```

### Invalid Format Fallback

```tsx
// If format is invalid, TimePicker falls back to culture-specific format
<TimePickerComponent
  format="invalid-format"  // Falls back to en-US default
/>
```

## Related Topics

- [Time Range and Selection](time-range-and-selection.md) - Learn about min/max constraints
- [Events and Methods](events-and-methods.md) - Handle format-related events
- [API Reference](api-reference.md) - Complete format property documentation
