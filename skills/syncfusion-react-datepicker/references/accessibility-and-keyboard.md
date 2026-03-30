# Accessibility & Keyboard Navigation

## Table of Contents
- [WCAG 2.2 Compliance](#wcag-22-compliance)
- [Keyboard Navigation](#keyboard-navigation)
- [ARIA Attributes](#aria-attributes)
- [Screen Reader Support](#screen-reader-support)
- [Focus Management](#focus-management)
- [Color Contrast](#color-contrast)
- [Mobile Accessibility](#mobile-accessibility)
- [Testing Accessibility](#testing-accessibility)

## WCAG 2.2 Compliance

DatePicker meets WCAG 2.2 level AA standards:

### Supported Standards

| Standard | Status | Details |
|----------|--------|---------|
| WCAG 2.2 Level AA | ✅ Supported | Most features compliant |
| Section 508 (US Law) | ✅ Partial | Key features supported |
| ADA (Americans with Disabilities) | ✅ Supported | Accessible to all users |
| ARIA 1.2 Specification | ✅ Supported | WAI-ARIA patterns implemented |

### Key WCAG Principles

1. **Perceivable** - Content visible to all (text, color, icons)
2. **Operable** - Keyboard navigation, sufficient time, no seizure triggers
3. **Understandable** - Clear labels, predictable behavior, error prevention
4. **Robust** - Compatible with assistive technologies

## Keyboard Navigation

### Input Field (Before Calendar Opens)

| Key | Action |
|-----|--------|
| Alt + Down Arrow | Open calendar popup |
| Alt + Up Arrow | Close calendar popup |
| Escape | Close calendar (if open) |
| Tab | Move focus to next element |
| Shift + Tab | Move focus to previous element |

### Calendar Navigation (Month View)

| Key | Action |
|-----|--------|
| Arrow Up | Focus previous week date |
| Arrow Down | Focus next week date |
| Arrow Left | Focus previous day |
| Arrow Right | Focus next day |
| Home | Focus first date in month |
| End | Focus last date in month |
| Page Up | Previous month |
| Page Down | Next month |
| Enter | Select focused date, close calendar |
| Escape | Close calendar without selecting |
| Space | Select focused date |

### Calendar Navigation (Year View)

| Key | Action |
|-----|--------|
| Arrow Keys | Move between months |
| Page Up | Previous year |
| Page Down | Next year |
| Home | First month (January) |
| End | Last month (December) |
| Enter | Select month, go to Month view |

### Calendar Navigation (Decade View)

| Key | Action |
|-----|--------|
| Arrow Keys | Move between years |
| Page Up | Previous decade |
| Page Down | Next decade |
| Home | First year of decade |
| End | Last year of decade |
| Enter | Select year, go to Year view |

### Complete Keyboard Journey

```
1. User focuses DatePicker input
2. User presses Tab to reach DatePicker
3. User presses Alt+Down to open calendar
4. DatePicker opens in Month view with today highlighted
5. User presses Up Arrow 3 times to go back 3 weeks
6. User presses Right Arrow 2 times to select 2 days forward
7. User presses Enter to select and close
```

## ARIA Attributes

DatePicker automatically applies accessibility attributes:

### Applied to Input Element

| Attribute | Value | Purpose |
|-----------|-------|---------|
| `aria-label` | "DatePicker" | Identifies purpose to screen readers |
| `aria-expanded` | true/false | Indicates calendar open/closed state |
| `aria-disabled` | true/false | Indicates disabled state |
| `aria-readonly` | true/false | Indicates readonly state |
| `aria-activedescendant` | ID of focused cell | Indicates currently focused day |
| `role` | "textbox" | Identifies as input element |

### Applied to Calendar Popup

| Attribute | Value | Purpose |
|-----------|-------|---------|
| `role` | "dialog" | Popup is a modal dialog |
| `aria-modal` | true | Only popup can be interacted with |
| `aria-label` | Month/Year | Shows current view context |
| `aria-hidden` | false | Popup is visible to screen readers |

### Applied to Day Cells

| Attribute | Value | Purpose |
|-----------|-------|---------|
| `aria-disabled` | true/false | Indicates if day is selectable |
| `aria-selected` | true/false | Indicates if day is selected |
| `role` | "button" | Accessible as clickable button |
| `aria-label` | "March 19, 2026" | Full date description |

### Example: Checking ARIA in Browser DevTools

```html
<!-- Open calendar in browser DevTools Elements panel -->
<input 
  aria-label="DatePicker"
  aria-expanded="true"
  aria-activedescendant="datepicker_cell_19"
  role="textbox"
/>

<!-- Calendar popup -->
<div role="dialog" aria-modal="true" aria-label="March 2026">
  <button 
    id="datepicker_cell_19"
    role="button"
    aria-disabled="false"
    aria-selected="false"
    aria-label="Thursday, March 19, 2026"
  >
    19
  </button>
</div>
```

## Screen Reader Support

### Testing with NVDA (Windows)

1. Install NVDA: https://www.nvaccess.org/
2. Open browser with DatePicker page
3. Press Ctrl+Alt+N to start NVDA
4. Tab to DatePicker input
5. NVDA announces: "DatePicker input, collapsed"
6. Press Alt+Down to open calendar
7. NVDA announces: "Calendar popup, March 2026, Month view"
8. Navigate with arrows, NVDA announces each date

### Testing with JAWS (Windows)

1. Tab to DatePicker input
2. JAWS reads: "Date picker textbox, collapsed"
3. Press Alt+Down to open
4. JAWS announces: "Calendar opened, March 2026"
5. Use arrow keys, JAWS reads each date

### Testing with VoiceOver (Mac)

1. Enable VoiceOver: Cmd+F5
2. Tab to DatePicker input
3. VoiceOver announces purpose and state
4. Press VO+Space to open calendar (may vary)
5. Navigate with arrow keys

### What Screen Reader Users Hear

**Default state:**
```
"Date Picker input, collapsed, select to open calendar"
```

**After opening calendar:**
```
"Calendar dialog, March 2026, Month view. 
Currently focused on today, March 19, 2026. 
Highlighted as current selection."
```

**Navigating with arrow keys:**
```
(Down Arrow)
"Friday, March 20, 2026, not selected, clickable"

(Down Arrow)
"Friday, March 27, 2026, disabled, outside range"
```

## Focus Management

### Default Focus Behavior

1. **Initial Focus:** Today's date when calendar opens
2. **Selection Focus:** After selecting, focus returns to input
3. **Tab Order:** DatePicker fits naturally in page tab order

### Visible Focus Indicator

```css
/* Focus outline is visible on input */
.e-datepicker:focus {
  outline: 2px solid #0066cc;
  outline-offset: 2px;
}

/* Focus visible on calendar day */
.e-calendar .e-day:focus {
  box-shadow: inset 0 0 0 2px #0066cc;
}
```

### Managing Focus Programmatically

```jsx
import React, { useRef } from 'react';
import { DatePickerComponent } from '@syncfusion/ej2-react-calendars';

export default function App() {
  const datePickerRef = useRef(null);

  const handleFocus = () => {
    // Focus the DatePicker using focusIn() method
    if (datePickerRef.current) {
      datePickerRef.current.focusIn();
    }
  };

  const handleBlur = () => {
    // Remove focus from DatePicker using focusOut() method
    if (datePickerRef.current) {
      datePickerRef.current.focusOut();
    }
  };

  const handleOpen = () => {
    // Open calendar using show() method
    if (datePickerRef.current) {
      datePickerRef.current.show();
    }
  };

  const handleClose = () => {
    // Close calendar using hide() method
    if (datePickerRef.current) {
      datePickerRef.current.hide();
    }
  };

  return (
    <div>
      <button onClick={handleFocus}>Focus DatePicker</button>
      <button onClick={handleBlur}>Remove Focus</button>
      <button onClick={handleOpen}>Open Calendar</button>
      <button onClick={handleClose}>Close Calendar</button>
      
      <DatePickerComponent
        ref={datePickerRef}
        value={new Date()}
        placeholder="Select date"
        focus={(e) => console.log('Focused:', e)}
        blur={(e) => console.log('Blurred:', e)}
        open={(e) => console.log('Calendar opened')}
        close={(e) => console.log('Calendar closed')}
        change={(e) => console.log('Value changed:', e.value)}
        navigated={(e) => console.log('Navigated to:', e.view)}
      />
    </div>
  );
}
```

## Color Contrast

### Compliance with WCAG Standards

DatePicker meets color contrast requirements:

| Element | Foreground | Background | Ratio | WCAG Level |
|---------|-----------|-----------|-------|-----------|
| Text | #333333 | #FFFFFF | 12.6:1 | AAA |
| Selected Date | #FFFFFF | #0066cc | 8.6:1 | AAA |
| Disabled Date | #999999 | #FFFFFF | 4.5:1 | AA |
| Hover | #0066cc | #F0F0F0 | 5.5:1 | AA |

### Testing Color Contrast

Use WebAIM Contrast Checker: https://webaim.org/resources/contrastchecker/

```
Foreground: #333333 (Text)
Background: #FFFFFF (White)
Contrast Ratio: 12.6:1 ✅ WCAG AAA Pass
```

### High Contrast Mode Support

For users with vision impairments, DatePicker supports high contrast:

```jsx
import '@syncfusion/ej2-react-calendars/styles/highcontrast.css';

<DatePickerComponent value={new Date()} />
```

**High Contrast Theme:**
- Increased contrast ratios (typically >7:1)
- Stronger borders and outlines
- No reliance on color alone
- Supports Windows High Contrast mode

## Mobile Accessibility

### Touch Keyboard Navigation

On mobile devices, DatePicker adapts:
- Touch opens calendar popup
- Touch keyboard for text input (supports ime)
- Touch-friendly calendar grid (larger touch targets)
- Calendar layout optimized for small screens

### Mobile Testing

```jsx
<DatePickerComponent
  value={new Date()}
  placeholder="Select date"
  // Mobile automatically adjusts layout
/>
```

**Mobile user experience:**
1. Focus DatePicker input
2. Tap to open calendar
3. Calendar appears at appropriate size
4. Touch dates to select
5. Calendar closes, date populates

### Responsive Sizing

```css
/* Automatically responsive */
.e-datepicker {
  width: 100%;
  max-width: 100%;
}

@media (max-width: 600px) {
  .e-popup-wrapper {
    width: 100vw;
    height: auto;
  }
}
```

## Testing Accessibility

### Automated Testing

Use tools to check accessibility:

```bash
# Install accessibility checker
npm install axe-core --save-dev
```

```jsx
import { axe, toHaveNoViolations } from 'jest-axe';

test('DatePicker has no accessibility violations', async () => {
  const { container } = render(
    <DatePickerComponent value={new Date()} />
  );
  
  const results = await axe(container);
  expect(results).toHaveNoViolations();
});
```

### Manual Testing Checklist

```
Keyboard Navigation:
☐ Can reach DatePicker with Tab
☐ Alt+Down opens calendar
☐ Arrow keys navigate dates
☐ Enter selects date
☐ Escape closes without selecting
☐ Tab order is logical

Screen Reader:
☐ NVDA announces purpose
☐ JAWS reads state correctly
☐ VoiceOver provides feedback
☐ Dates announced with full description
☐ Disabled dates announced

Focus Visible:
☐ Focus outline visible on all focused elements
☐ Focus outline has sufficient contrast
☐ Focus indicator not removed

Color & Contrast:
☐ Text readable without color reliance
☐ Links and buttons distinguishable
☐ Color contrast >4.5:1 for normal text
☐ Color contrast >7:1 for large text

Mobile:
☐ Touch targets sufficient size (44x44px minimum)
☐ No horizontal scrolling required
☐ Zoom functionality works
☐ Landscape and portrait work
```

### Complete Accessibility Example

```jsx
import React, { useState } from 'react';
import { DatePickerComponent } from '@syncfusion/ej2-react-calendars';
import '@syncfusion/ej2-react-calendars/styles/highcontrast.css';

export default function AccessibleApp() {
  const [date, setDate] = useState(new Date());

  return (
    <div style={{ padding: '20px' }}>
      <h1>Accessible DatePicker Form</h1>
      
      <form>
        <fieldset>
          <legend>Event Registration</legend>
          
          <div>
            <label htmlFor="event-date">
              <strong>Event Date:</strong>
              <span style={{ color: 'red' }}>*</span>
              <span className="sr-only"> (Required)</span>
            </label>
            
            <DatePickerComponent
              id="event-date"
              value={date}
              change={(e) => setDate(e.value)}
              min={new Date()}
              max={new Date(Date.now() + 365 * 24 * 60 * 60 * 1000)}
              placeholder="Choose an event date (YYYY-MM-DD)"
              format="yyyy-MM-dd"
              aria-required="true"
              aria-describedby="date-help"
            />
            
            <small id="date-help" style={{ display: 'block', marginTop: '5px' }}>
              Select a date within the next year.
            </small>
          </div>
          
          <button type="submit">Register Event</button>
        </fieldset>
      </form>

      <style>{`
        .sr-only {
          position: absolute;
          width: 1px;
          height: 1px;
          overflow: hidden;
          clip: rect(0, 0, 0, 0);
        }
      `}</style>
    </div>
  );
}
```

