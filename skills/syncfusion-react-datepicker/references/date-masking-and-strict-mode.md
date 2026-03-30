# Date Masking & Strict Mode

## Table of Contents
- [Date Masking with enableMask](#date-masking-with-enablemask)
- [maskPlaceholder Property](#maskplaceholder-property)
- [Date Masking Patterns](#date-masking-patterns)
- [How Masking Works](#how-masking-works)
- [Strict Mode Overview](#strict-mode-overview)
- [Strict Mode vs Normal Mode](#strict-mode-vs-normal-mode)
- [Date Parsing Rules](#date-parsing-rules)
- [Edge Cases & Troubleshooting](#edge-cases--troubleshooting)
- [Best Practices](#best-practices)

## Date Masking with enableMask

The `enableMask` property enables the masked input mode, which provides segment-by-segment date entry with a structured input pattern.

### Basic Masked DatePicker

```jsx
<DatePickerComponent
  enableMask={true}
  format="dd/MM/yyyy"
  placeholder="Select a date"
/>
```

**Default:** `enableMask` is `false` — the DatePicker renders without masked input.
**When `true`:** The input displays the date mask (e.g., `day/month/year`) allowing users to type segment by segment.

### Masked DatePicker with Custom Format

```jsx
<DatePickerComponent
  enableMask={true}
  format="MM/dd/yyyy"
  value={new Date()}
/>
```

**Result:** Input shows a segment-based mask for month, day, and year entry.

## maskPlaceholder Property

The `maskPlaceholder` property customizes the placeholder text shown for each date segment in the masked input.

### Default Placeholder

By default, masked segments show:
```
{ day: 'day', month: 'month', year: 'year', hour: 'hour', minute: 'minute', second: 'second', dayOfTheWeek: 'day of the week' }
```

### Custom maskPlaceholder

```jsx
<DatePickerComponent
  enableMask={true}
  format="MM/dd/yyyy"
  maskPlaceholder={{ day: 'DD', month: 'MM', year: 'YYYY' }}
/>
```

**Result:** Input displays `MM/DD/YYYY` as the empty mask, guiding users to enter each segment.

### Localized maskPlaceholder

```jsx
<DatePickerComponent
  enableMask={true}
  locale="de"
  format="dd.MM.yyyy"
  maskPlaceholder={{ day: 'TT', month: 'MM', year: 'JJJJ' }}
/>
```

## Date Masking Patterns

Date masking provides visual guidance for users typing dates. It shows the expected format as a placeholder or guide.

### Standard Placeholder Masking (without enableMask)

When a format is specified and `enableMask` is not enabled, DatePicker shows a text placeholder:

```jsx
<DatePickerComponent
  value={new Date()}
  format="dd/MM/yyyy"
  placeholder="DD/MM/YYYY"
/>
```

**Result:** Input shows "DD/MM/YYYY" placeholder in light gray until user types.

### Placeholder Text

Use `placeholder` prop for user guidance:

```jsx
<DatePickerComponent
  format="yyyy-MM-dd"
  placeholder="YYYY-MM-DD (e.g., 2026-03-19)"
/>
```

**Shows:** "YYYY-MM-DD (e.g., 2026-03-19)" in light gray until user types

### Input Formats Guidance

Combine format with inputFormats and helpful placeholder:

```jsx
<DatePickerComponent
  value={new Date()}
  format="yyyy-MM-dd"
  inputFormats={['dd/MM/yyyy', 'MM/dd/yyyy', 'yyyy-MM-dd']}
  placeholder="Enter date: DD/MM/YYYY, MM/DD/YYYY, or YYYY-MM-DD"
/>
```

## How Masking Works

### Character-by-Character Masking

DatePicker enforces format as user types:

```
Format: MM/dd/yyyy
User Types:
  "1"     → "1"
  "19"    → "19"
  "19/"   → "19/"
  "19/3"  → "19/3"
  "19/31" → "19/31"
  "19/31/2026" → "19/31/2026"
```

### Format Characters vs Literal Characters

| Type | Examples | Behavior |
|------|----------|----------|
| Format Char | M, d, y | Optional, user enters |
| Literal Char | /, -, space | Auto-filled, required |

**Example format:** `MM/dd/yyyy`
- M: Format character (month digit)
- /: Literal character (auto-filled)
- d: Format character (day digit)
- /: Literal character (auto-filled)
- y: Format character (year digit)

### Auto-Advancement

When user completes a segment, cursor auto-advances:

```
Format: MM/dd/yyyy
User Type: [1][9]
Result: 19|...  (cursor auto-advanced past /)
Continue: [0][3]
Result: 19/03|...  (cursor auto-advanced past /)
Continue: [2][0][2][6]
Result: 19/03/2026 (complete)
```

## Strict Mode Overview

The `strictMode` property controls validation and auto-correction behavior.

### strictMode: false (Default - Permissive)

```jsx
<DatePickerComponent
  value={new Date()}
  strictMode={false}
  format="yyyy-MM-dd"
/>
```

**Behavior:**
- Allows invalid or out-of-range dates
- Shows error styling (red border) if invalid
- Does NOT auto-correct invalid input
- Returns the entered value (valid or invalid)
- Developer handles validation

### strictMode: true (Strict - Auto-Correct)

```jsx
<DatePickerComponent
  value={new Date()}
  strictMode={true}
  format="yyyy-MM-dd"
/>
```

**Behavior:**
- Enforces valid dates only
- Auto-corrects invalid input to nearest valid date
- Shows no error styling
- Returns corrected valid value
- No validation needed

## Strict Mode vs Normal Mode

### Comparison Table

| Scenario | strictMode: false | strictMode: true |
|----------|-------------------|-----------------|
| **Invalid date (Feb 30)** | Accepts, shows error | Rejects, keeps Feb 28 |
| **Out of min/max range** | Accepts, shows error | Auto-corrects to boundary |
| **Invalid format (abc)** | Accepts if format flexible | Rejects, requires valid format |
| **Leap year edge (Feb 29 in non-leap year)** | Accepts as Feb 29, shows error | Auto-corrects to Feb 28 |
| **Validation needed** | Yes, manual validation | No, built-in enforcement |
| **User experience** | See error, fix manually | Seamless, auto-correct |

### Example: February 30 (Invalid Date)

**strictMode: false**
```jsx
<DatePickerComponent
  value={null}
  strictMode={false}
/>
// User types: "2026-02-30"
// Result: Accepts 2026-02-30, shows red border (invalid)
```

**strictMode: true**
```jsx
<DatePickerComponent
  value={null}
  strictMode={true}
/>
// User types: "2026-02-30"
// Result: Auto-corrects to 2026-02-28, displays valid date
```

## Date Parsing Rules

### Parsing Logic (How DatePicker Interprets Input)

1. **Matches inputFormats first**
   - If user input matches any `inputFormats` pattern → parse using that format
   
2. **Matches display format**
   - If input matches `format` → parse as that format
   
3. **Attempts flexible parsing**
   - If `strictMode: false` → try to interpret partial/flexible input
   - Examples: "3/19" → interprets as March 19, "2026-3" → March 2026
   
4. **Rejects invalid input**
   - If no match and `strictMode: true` → reject input
   - If no match and `strictMode: false` → accept and mark as error

### Example: Multiple Input Formats

```jsx
<DatePickerComponent
  value={new Date()}
  format="yyyy-MM-dd"
  inputFormats={['dd/MM/yyyy', 'MM/dd/yyyy', 'yyyyMMdd']}
  strictMode={true}
/>
```

**User Types:**
- "19/03/2026" → Matches inputFormats[0] → Parsed as 2026-03-19 ✓
- "03/19/2026" → Matches inputFormats[1] → Parsed as 2026-03-19 ✓
- "20260319" → Matches inputFormats[2] → Parsed as 2026-03-19 ✓
- "2026-03-19" → Matches format → Parsed as 2026-03-19 ✓
- "abc" → Matches none → Rejected in strict mode ✗

## Edge Cases & Troubleshooting

### Leap Year Validation

**Issue:** User types February 29 in a non-leap year (e.g., 2025)

```jsx
<DatePickerComponent
  value={null}
  strictMode={true}
  format="yyyy-MM-dd"
/>
// User types: "2025-02-29"
// Result: Auto-corrects to "2025-02-28" (Feb 29 doesn't exist in 2025)
```

**With strictMode: false:**
```jsx
<DatePickerComponent
  value={null}
  strictMode={false}
/>
// User types: "2025-02-29"
// Result: Accepts but shows error (invalid date)
```

### Month Boundary Validation

**Issue:** User types day 31 in a month with 30 days

```jsx
<DatePickerComponent
  strictMode={true}
  format="yyyy-MM-dd"
/>
// User types: "2026-04-31" (April has 30 days)
// Result: Auto-corrects to "2026-04-30"
```

### Year Boundaries

**Issue:** User types year outside reasonable range

```jsx
<DatePickerComponent
  strictMode={true}
  min={new Date(1990, 0, 1)}
  max={new Date(2050, 11, 31)}
/>
// User types: "1980-03-15" (before min)
// Result: Auto-corrects to 1990-01-01 (the min date)
```

### Ambiguous Format Parsing

**Issue:** Input could match multiple formats

```jsx
<DatePickerComponent
  format="MM/dd/yyyy"
  inputFormats={['MM/dd/yyyy', 'dd/MM/yyyy']}
/>
// User types: "03/04/2026"
// Result: First matching format wins → MM/dd/yyyy → March 4, 2026
```

**Solution:** Order inputFormats by specificity or user preference

### Partial Input with strictMode

**Issue:** User types incomplete date with strictMode: true

```jsx
<DatePickerComponent
  strictMode={true}
  format="yyyy-MM-dd"
/>
// User types: "2026-03" (incomplete)
// Result: Input not accepted until complete format provided
```

**With strictMode: false:**
```jsx
// User types: "2026-03"
// Result: Accepted as partial input, marked as error if not completed
```

## Best Practices

### 1. Choose Appropriate strictMode

**Use strictMode: true when:**
- You need guaranteed valid dates
- Auto-correction is acceptable
- Seamless UX is priority
- Examples: Booking systems, calendar events

**Use strictMode: false when:**
- You need to show validation errors
- Users must explicitly correct input
- Compliance/audit requirements
- Examples: Forms, data entry, surveys

### 2. Provide Clear Placeholders

```jsx
<DatePickerComponent
  value={new Date()}
  format="dd/MM/yyyy"
  placeholder="DD/MM/YYYY (e.g., 19/03/2026)"
  inputFormats={['dd/MM/yyyy', 'dd-MM-yyyy']}
/>
```

Clear placeholder helps users understand expected format.

### 3. Combine format and inputFormats

```jsx
<DatePickerComponent
  value={new Date()}
  format="yyyy-MM-dd"              // Consistent display
  inputFormats={[
    'dd/MM/yyyy',                  // User convenience
    'MM/dd/yyyy',
    'yyyy-MM-dd',
    'yyyyMMdd'
  ]}
  placeholder="Enter date (multiple formats accepted)"
/>
```

Flexible input with consistent output.

### 4. Validate with min/max

```jsx
<DatePickerComponent
  value={new Date()}
  min={new Date(2020, 0, 1)}
  max={new Date()}
  strictMode={true}
  format="yyyy-MM-dd"
/>
```

min/max with strictMode ensures valid range.

### 5. Handle Edge Cases Explicitly

```jsx
const [date, setDate] = useState(null);
const [error, setError] = useState('');

const handleChange = (e) => {
  const selectedDate = e.value;
  
  // Check for leap year
  const isLeapYear = (selectedDate.getFullYear() % 4 === 0) &&
                     (selectedDate.getFullYear() % 100 !== 0 ||
                      selectedDate.getFullYear() % 400 === 0);
  
  if (selectedDate.getMonth() === 1 &&   // February
      selectedDate.getDate() === 29 &&   // 29th
      !isLeapYear) {
    setError('Selected year does not have February 29');
    return;
  }
  
  setDate(selectedDate);
  setError('');
};

<DatePickerComponent
  value={date}
  change={handleChange}
  strictMode={false}
/>
{error && <p style={{ color: 'red' }}>{error}</p>}
```

### 6. Test Input Edge Cases

```jsx
// Test cases to verify masking and parsing
const testCases = [
  { input: '2026-02-29', desc: 'Leap year', year: 2026, shouldFail: true },
  { input: '2024-02-29', desc: 'Valid leap year', year: 2024, shouldPass: true },
  { input: '2026-04-31', desc: 'Month with 30 days', shouldFail: true },
  { input: '19/03/2026', desc: 'European format', shouldPass: true },
  { input: '03/19/2026', desc: 'US format', shouldPass: true },
  { input: 'abc', desc: 'Invalid text', shouldFail: true }
];
```

### 7. Document User Expectations

In your UI, make expectations clear:

```jsx
<div>
  <label>
    <strong>Start Date</strong>
    <span style={{ color: 'red' }}>*</span>
  </label>
  <DatePickerComponent
    format="dd/MM/yyyy"
    placeholder="DD/MM/YYYY"
    strictMode={true}
    min={new Date()}
  />
  <small>
    Select a future date in DD/MM/YYYY format.
    Past dates will be auto-corrected to today.
  </small>
</div>
```

