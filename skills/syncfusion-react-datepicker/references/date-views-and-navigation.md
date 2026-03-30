# Date Views & Navigation

## Table of Contents
- [Calendar Views](#calendar-views)
- [Start Property](#start-property)
- [Depth Property](#depth-property)
- [Navigation Behavior](#navigation-behavior)
- [Start and Depth Together](#start-and-depth-together)
- [Common Use Cases](#common-use-cases)
- [Keyboard Navigation](#keyboard-navigation)

## Calendar Views

DatePicker supports three levels of date navigation:

### Month View (Default)
- Shows individual days in a calendar grid
- User selects a specific date (e.g., March 19)
- Click arrows to move between months
- Ideal for: daily date selection

```jsx
<DatePickerComponent
  start="Month"  // Default - explicit for clarity
  placeholder="Select a day"
/>
```

**Display:** Calendar grid with 28-31 days visible

### Year View
- Shows 12 months of the selected year
- User selects a month (e.g., March 2026)
- Click arrows to move between years
- Ideal for: month-level precision

```jsx
<DatePickerComponent
  start="Year"
  placeholder="Select a month"
/>
```

**Display:** 12 month tiles (Jan, Feb, Mar, etc.)

### Decade View
- Shows 10 years of the decade
- User selects a year (e.g., 2026)
- Click arrows to move between decades
- Ideal for: year selection, birth dates, historical data

```jsx
<DatePickerComponent
  start="Decade"
  placeholder="Select a year"
/>
```

**Display:** 10 year tiles (2020-2029)

## Start Property

The `start` property determines the initial view when the calendar opens.

### Start Month (Default)

```jsx
<DatePickerComponent
  start="Month"
  value={new Date(2026, 2, 15)}  // March 15, 2026
  placeholder="Select a date"
/>
```

**When user clicks the input:**
1. Calendar opens in Month view
2. Shows March 2026 calendar
3. March 15 is highlighted (current value)
4. User can click a specific day

### Start Year

```jsx
<DatePickerComponent
  start="Year"
  value={new Date(2026, 2, 15)}  // March 15, 2026
  placeholder="Select a date"
/>
```

**When user clicks the input:**
1. Calendar opens in Year view
2. Shows 12 months of 2026
3. March is highlighted (current value's month)
4. User clicks a month to go to Month view
5. Then selects a specific day

### Start Decade

```jsx
<DatePickerComponent
  start="Decade"
  value={new Date(2026, 2, 15)}
  placeholder="Select a date"
/>
```

**When user clicks the input:**
1. Calendar opens in Decade view
2. Shows years 2020-2029
3. 2026 is highlighted (current value's year)
4. User clicks a year to go to Year view
5. Clicks a month to go to Month view
6. Selects a specific day

### Flow: Decade → Year → Month → Day

```
Start Decade:  2020-2029 tiles
              ↓ (user clicks 2026)
              Jan-Dec 2026 tiles
              ↓ (user clicks March)
              March 1-31 calendar
              ↓ (user clicks 15)
              Returns: March 15, 2026
```

## Depth Property

The `depth` property restricts the deepest (most detailed) view users can navigate to.

### Requirements
- `depth` value must be **less detailed** than `start` value
- Example: If `start="Decade"`, then `depth` can be "Year" or "Month"
- If `depth >= start`, the restriction doesn't apply

### Depth: Month (Most Detailed)

```jsx
<DatePickerComponent
  start="Decade"
  depth="Month"
  placeholder="Select a month"
/>
```

**Navigation allowed:**
- Decade view → Year view → Month view (stops here)
- User selects: March 2026 (not a specific day)
- Returns: Date(2026, 2, 1) - first day of selected month

### Depth: Year

```jsx
<DatePickerComponent
  start="Decade"
  depth="Year"
  placeholder="Select a year"
/>
```

**Navigation allowed:**
- Decade view → Year view (stops here)
- User selects: 2026 (not specific month or day)
- Returns: Date(2026, 0, 1) - first day of selected year

## Start and Depth Together

### Example 1: Birth Date (Start Decade, Depth Month)

```jsx
<DatePickerComponent
  start="Decade"
  depth="Month"
  value={null}
  placeholder="Select birth month/year"
/>
```

**User journey:**
1. Opens with Decade view (2020-2029)
2. Clicks 1995
3. Goes to Year view (1995)
4. Clicks March
5. Stops at Month view (cannot select specific day)
6. Returns: Date(1995, 2, 1)

**Use case:** Birth month/year for insurance, general demographics

### Example 2: Report Month (Start Year, Depth Month)

```jsx
<DatePickerComponent
  start="Year"
  depth="Month"
  value={new Date()}
  placeholder="Select report month"
/>
```

**User journey:**
1. Opens with Year view (12 months of 2026)
2. Clicks March
3. Stops at Month view (cannot select specific day)
4. Returns: Date(2026, 2, 1)

**Use case:** Monthly reports, billing cycles

### Example 3: Historical Year Selection (Start Decade, Depth Year)

```jsx
<DatePickerComponent
  start="Decade"
  depth="Year"
  value={null}
  placeholder="Select year"
/>
```

**User journey:**
1. Opens with Decade view (2020-2029)
2. Clicks 2024
3. Stops at Year view (cannot go deeper)
4. Returns: Date(2024, 0, 1)

**Use case:** Year filtering, historical reports, census data

## Navigation Behavior

### Arrow Button Navigation

**Month view:**
- Left arrow: Previous month
- Right arrow: Next month
- Click month/year header: Goes to Year view

**Year view:**
- Left arrow: Previous year
- Right arrow: Next year
- Click year header: Goes to Decade view

**Decade view:**
- Left arrow: Previous decade (goes back 10 years)
- Right arrow: Next decade (goes forward 10 years)
- No header click (already at top level)

### Header Click Navigation

Click the title at the top to drill up:

```
Month view: Click "March 2026" → Year view
Year view:  Click "2026" → Decade view
Decade view: Cannot click (top level)
```

## Common Use Cases

### 1. Flexible Birth Date Selection (Any Day)

```jsx
<DatePickerComponent
  start="Decade"
  placeholder="Select birth date"
/>
```

- Starts with decades for fast year selection
- Can drilldown to month and then specific day
- Efficient for historical dates (1980s, 1990s, etc.)

### 2. Month-Level Reporting

```jsx
<DatePickerComponent
  start="Year"
  depth="Month"
  format="MMMM yyyy"  // March 2026
  placeholder="Report month"
/>
```

- Shows 12 months of the year
- User selects month (not day)
- Returns first day of month
- Useful for: monthly sales reports, billing, forecasts

### 3. Year-Only Selection

```jsx
<DatePickerComponent
  start="Decade"
  depth="Year"
  format="yyyy"  // 2026
  placeholder="Select year"
/>
```

- Shows decades for fast navigation
- User selects year (not month or day)
- Returns January 1 of selected year
- Useful for: annual reports, budget planning, archiving

### 4. Standard Day Selection (Default)

```jsx
<DatePickerComponent
  start="Month"  // or omit - default
  placeholder="Select date"
/>
```

- Standard calendar view
- Most intuitive for users
- Select any specific day
- Default behavior

### 5. Quick Date Selection (Recent Dates)

```jsx
<DatePickerComponent
  start="Month"
  value={new Date()}
  placeholder="Select date"
/>
```

- Opens showing current month
- Pre-selected to today
- User can navigate months quickly
- Useful for: recent date selection, quick picks

## Keyboard Navigation

### In Month View

| Key | Action |
|-----|--------|
| Arrow Keys | Move between days |
| Page Up | Previous month |
| Page Down | Next month |
| Home | First day of month |
| End | Last day of month |
| Enter | Select focused day |
| Escape | Close calendar |

### In Year View

| Key | Action |
|-----|--------|
| Arrow Keys | Move between months |
| Page Up | Previous year |
| Page Down | Next year |
| Home | First month (January) |
| End | Last month (December) |
| Enter | Select focused month |

### In Decade View

| Key | Action |
|-----|--------|
| Arrow Keys | Move between years |
| Page Up | Previous decade |
| Page Down | Next decade |
| Home | First year of decade |
| End | Last year of decade |
| Enter | Select focused year |

### Example: Complete Keyboard Navigation

```
1. Focus DatePicker input
2. Press Alt+Down → Calendar opens (Month view)
3. Press Page Up 3 times → Navigates 3 months back
4. Press Home → Jumps to 1st of month
5. Press Arrow Right 5 times → Selects 6th day
6. Press Enter → Closes calendar, date selected
```

## Complete Example

```jsx
import React, { useState } from 'react';
import { DatePickerComponent } from '@syncfusion/ej2-react-calendars';

export default function App() {
  const [dateInfo, setDateInfo] = useState({
    value: null,
    view: 'Month'
  });

  const handleChange = (e) => {
    setDateInfo(prev => ({
      ...prev,
      value: e.value
    }));
  };

  return (
    <div style={{ padding: '20px' }}>
      <h3>Date Views Demo</h3>
      
      <div>
        <label>Standard (Day Selection):</label>
        <DatePickerComponent
          start="Month"
          change={handleChange}
          placeholder="Select a day"
        />
      </div>

      <div style={{ marginTop: '20px' }}>
        <label>Month Selection (Birth Date):</label>
        <DatePickerComponent
          start="Decade"
          depth="Month"
          format="MMMM yyyy"
          change={handleChange}
          placeholder="Select month/year"
        />
      </div>

      <div style={{ marginTop: '20px' }}>
        <label>Year Selection:</label>
        <DatePickerComponent
          start="Decade"
          depth="Year"
          format="yyyy"
          change={handleChange}
          placeholder="Select year"
        />
      </div>

      {dateInfo.value && (
        <p style={{ marginTop: '20px' }}>
          Selected: {dateInfo.value.toString()}
        </p>
      )}
    </div>
  );
}
```

