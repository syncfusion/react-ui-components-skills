# Date Formats & Input

## Table of Contents
- [Display Format](#display-format)
- [Format Pattern Syntax](#format-pattern-syntax)
- [Common Format Examples](#common-format-examples)
- [Input Formats](#input-formats)
- [Automatic Format Conversion](#automatic-format-conversion)
- [Culture-Based Formatting](#culture-based-formatting)
- [Format and Input Formats Together](#format-and-input-formats-together)

## Display Format

The `format` property controls how the selected date is displayed in the input field.

### Default Format

By default, DatePicker uses the culture's default format:

```jsx
<DatePickerComponent
  value={new Date()}
  // format not specified, uses culture default (e.g., "M/d/yyyy" for en-US)
/>
```

### Custom Format

Set a custom display format using the `format` property:

```jsx
<DatePickerComponent
  id="datepicker"
  value={new Date()}
  format="yyyy-MM-dd"  // ISO format: 2026-03-19
  placeholder="YYYY-MM-DD"
/>
```

**When user selects March 19, 2026:**
- Input shows: `2026-03-19`
- The format applies only to display, not the underlying Date object

## Format Pattern Syntax

DatePicker uses this pattern syntax for custom formats:

| Character | Meaning | Example |
|-----------|---------|---------|
| `y` or `yy` | 2-digit year | 26 |
| `yyyy` | 4-digit year | 2026 |
| `M` | Month without leading zero | 3 (for March) |
| `MM` | Month with leading zero | 03 (for March) |
| `MMM` | Month abbreviation | Mar |
| `MMMM` | Full month name | March |
| `d` | Day without leading zero | 9 |
| `dd` | Day with leading zero | 09 |
| `ddd` | Day abbreviation | Thu |
| `dddd` | Full day name | Thursday |
| `/` or `-` | Separator (literal) | - or / |

### Pattern Construction

Combine characters to build formats:

```
yyyy-MM-dd        → 2026-03-19
dd/MM/yyyy        → 19/03/2026
MMMM d, yyyy      → March 19, 2026
dddd, MMMM d      → Thursday, March 19
MM/dd/yy          → 03/19/26
d MMM yyyy        → 19 Mar 2026
```

## Common Format Examples

### US Format (Month-Day-Year)
```jsx
<DatePickerComponent
  value={new Date()}
  format="MM/dd/yyyy"  // 03/19/2026
  placeholder="MM/DD/YYYY"
/>
```

### European Format (Day-Month-Year)
```jsx
<DatePickerComponent
  value={new Date()}
  format="dd/MM/yyyy"  // 19/03/2026
  placeholder="DD/MM/YYYY"
/>
```

### ISO Format (Year-Month-Day)
```jsx
<DatePickerComponent
  value={new Date()}
  format="yyyy-MM-dd"  // 2026-03-19
  placeholder="YYYY-MM-DD"
/>
```

### Long Format with Day Name
```jsx
<DatePickerComponent
  value={new Date()}
  format="dddd, MMMM d, yyyy"  // Thursday, March 19, 2026
  placeholder="Day, Month Date, Year"
/>
```

### Short Format with Month Name
```jsx
<DatePickerComponent
  value={new Date()}
  format="d MMM yyyy"  // 19 Mar 2026
  placeholder="D Mon YYYY"
/>
```

## Input Formats

The `inputFormats` property lets users enter dates in multiple formats, which are automatically converted to the display format.

### Single Input Format

```jsx
<DatePickerComponent
  value={new Date()}
  format="yyyy-MM-dd"           // Display format
  inputFormats={['dd/MM/yyyy']}  // What users can type
  placeholder="Type: DD/MM/YYYY (converts to YYYY-MM-DD)"
/>
```

**User types:** `19/03/2026` → **Displays:** `2026-03-19`

### Multiple Input Formats

Accept dates in various formats:

```jsx
<DatePickerComponent
  value={new Date()}
  format="yyyy-MM-dd"
  inputFormats={[
    'dd/MM/yyyy',     // 19/03/2026
    'MM/dd/yyyy',     // 03/19/2026
    'yyyy-MM-dd',     // 2026-03-19
    'yyyyMMdd',       // 20260319
    'd MMM yyyy'      // 19 Mar 2026
  ]}
  placeholder="Enter date (multiple formats accepted)"
/>
```

**Users can type any of these and DatePicker will parse correctly:**
- `19/03/2026` (European)
- `03/19/2026` (US)
- `2026-03-19` (ISO)
- `20260319` (No separators)
- `19 Mar 2026` (Month name)

### Real-World Example: Birth Date

```jsx
<DatePickerComponent
  id="birthDate"
  value={null}
  format="MM/dd/yyyy"
  inputFormats={['MM/dd/yyyy', 'dd/MM/yyyy', 'yyyy-MM-dd']}
  placeholder="Birth Date (MM/DD/YYYY)"
/>
```

This accepts multiple formats, converting all to US format for consistency.

## Automatic Format Conversion

When `inputFormats` is defined and user types a date:

1. User types a date matching one of the `inputFormats`
2. On blur or Enter key press, DatePicker parses the input
3. Parsed date converts to the `format` display format
4. Input field updates to show formatted date

**Example flow:**

```
User input: "19-03-2026"
Matches inputFormat: "dd-MM-yyyy" ✓
Parses to: Date(2026, 2, 19)
Converts to format: "yyyy-MM-dd"
Display: "2026-03-19"
```

### When Parsing Fails

If input doesn't match any `inputFormats`:

```jsx
<DatePickerComponent
  value={new Date()}
  format="yyyy-MM-dd"
  inputFormats={['dd/MM/yyyy']}
  placeholder="Enter as DD/MM/YYYY"
/>
```

**User types:** `invalid-date` → Field shows error styling (red border), value remains unchanged

## Culture-Based Formatting

Different cultures use different date formats. Syncfusion auto-applies culture defaults:

### English (en-US)
```jsx
<DatePickerComponent locale="en" />  // M/d/yyyy (3/19/2026)
```

### German (de)
```jsx
<DatePickerComponent locale="de" />  // d.M.yyyy (19.3.2026)
```

### French (fr)
```jsx
<DatePickerComponent locale="fr" />  // dd/MM/yyyy (19/03/2026)
```

### British English (en-GB)
```jsx
<DatePickerComponent locale="en-GB" />  // dd/MM/yyyy (19/03/2026)
```

## Format and Input Formats Together

Combine `format` and `inputFormats` for maximum flexibility:

```jsx
<DatePickerComponent
  id="datepicker"
  value={new Date()}
  format="dd MMM yyyy"              // Display: 19 Mar 2026
  inputFormats={[
    'dd/MM/yyyy',                   // User can type: 19/03/2026
    'MM/dd/yyyy',                   // User can type: 03/19/2026
    'yyyy-MM-dd',                   // User can type: 2026-03-19
    'dd MMM yyyy'                   // User can type: 19 Mar 2026
  ]}
  placeholder="Enter date: 19/03/2026 or 03/19/2026 or 2026-03-19"
/>
```

**Result:**
- Input accepts 4 different formats
- All convert to "19 Mar 2026" format for display
- Provides best user experience: flexible input, consistent display

### Complete Application Example

```jsx
import React, { useState } from 'react';
import { DatePickerComponent } from '@syncfusion/ej2-react-calendars';

export default function App() {
  const [date, setDate] = useState(new Date());
  const [formattedString, setFormattedString] = useState('');

  const handleChange = (e) => {
    setDate(e.value);
    // Get the formatted date string
    if (e.value) {
      const day = String(e.value.getDate()).padStart(2, '0');
      const month = String(e.value.getMonth() + 1).padStart(2, '0');
      const year = e.value.getFullYear();
      setFormattedString(`${day}/${month}/${year}`);
    }
  };

  return (
    <div style={{ padding: '20px' }}>
      <h3>Date Format Example</h3>
      <DatePickerComponent
        id="datepicker"
        value={date}
        change={handleChange}
        format="dddd, MMMM d, yyyy"
        inputFormats={['dd/MM/yyyy', 'MM/dd/yyyy', 'yyyy-MM-dd']}
        placeholder="Enter date (dd/MM/yyyy, MM/dd/yyyy, or YYYY-MM-DD)"
      />
      <p>Formatted Display: {formattedString}</p>
      <p>Underlying Date Object: {date?.toString()}</p>
    </div>
  );
}
```

