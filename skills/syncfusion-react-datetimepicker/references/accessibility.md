# Accessibility

## Table of Contents
- [WCAG Compliance](#wcag-compliance)
- [ARIA Implementation](#aria-implementation)
- [Keyboard Navigation](#keyboard-navigation)
- [Screen Reader Support](#screen-reader-support)
- [Focus Management](#focus-management)
- [Color Contrast](#color-contrast)
- [Disabled and Readonly States](#disabled-and-readonly-states)
- [Testing Accessibility](#testing-accessibility)

## WCAG Compliance

DateTimePicker implements WCAG 2.1 Level AA accessibility standards.

### Compliant Features

- ✅ Keyboard accessible (Level A)
- ✅ Semantic HTML structure (Level A)
- ✅ Sufficient color contrast (Level AA)
- ✅ Focus indicators visible (Level AA)
- ✅ Error messaging and remediation (Level AA)
- ✅ Responsive to scaling up to 200% (Level AA)

### Non-Compliant Usage Patterns

- ❌ DON'T: Remove focus styles — keep a visible outline on focus.
- ❌ DON'T: Use color alone to indicate errors; include text or icons as well.
- ❌ DON'T: Use inline `style` on the component to force very small font sizes.

### Compliant Usage Patterns

```css
/* ✅ DO: Keep visible focus styles */
.e-datetimepicker:focus {
  outline: 2px solid #2196F3;
  outline-offset: 2px;
}

/* ✅ DO: Use readable font sizes via CSS classes */
.my-datetimepicker { font-size: 14px; }
```

```tsx
// Apply CSS using the supported `cssClass` property
<DateTimePickerComponent cssClass="my-datetimepicker" />

// Use descriptive text + color for errors
<div style={{ color: 'red' }}>
  ⚠️ Date required: Select a date from the calendar
</div>
```

## ARIA Implementation

Add ARIA attributes to enhance semantic meaning for assistive technologies.

### ARIA Attributes

```tsx
// aria-label: Label for screen readers
<DateTimePickerComponent
  aria-label="Select appointment date and time"
/>

// aria-describedby: Link to description text
<div>
  <DateTimePickerComponent
    aria-describedby="date-help"
  />
  <span id="date-help">
    Select a date and time between 9 AM and 5 PM
  </span>
</div>

// aria-required: Indicate required field
<DateTimePickerComponent
  aria-required="true"
  placeholder="Date is required"
/>

// aria-invalid: Indicate error state
<DateTimePickerComponent
  aria-invalid={hasError}
  aria-describedby={hasError ? "error-msg" : undefined}
/>
```

### Complete ARIA Example

```tsx
import React, { useState } from 'react';
import { DateTimePickerComponent } from '@syncfusion/ej2-react-calendars';

export default function AccessibleForm() {
  const [appointmentDate, setAppointmentDate] = useState<Date | null>(null);
  const [hasError, setHasError] = useState(false);

  const handleChange = (args: any) => {
    const selected = args.value as Date | null;
    setAppointmentDate(selected);
    setHasError(!selected);
  };

  return (
    <div>
      <label htmlFor="appointment-datetime">
        Appointment Date and Time <span aria-label="required">*</span>
      </label>

      <DateTimePickerComponent
        id="appointment-datetime"
        value={appointmentDate}
        change={handleChange}
        aria-label="Select appointment date and time"
        aria-required="true"
        aria-invalid={hasError}
        aria-describedby={hasError ? "error-msg" : "date-help"}
        min={new Date()}
        minTime={new Date(0, 0, 0, 9, 0)}
        maxTime={new Date(0, 0, 0, 17, 0)}
        format="MM/dd/yyyy hh:mm a"
      />

      {hasError && (
        <span id="error-msg" style={{ color: 'red' }}>
          ⚠️ Please select an appointment date and time
        </span>
      )}

      {!hasError && (
        <span id="date-help" style={{ fontSize: '12px', color: '#666' }}>
          Select a date and time between 9 AM and 5 PM
        </span>
      )}
    </div>
  );
}
```

## Keyboard Navigation

DateTimePicker is fully keyboard accessible.

### Keyboard Shortcuts Reference

#### Input Navigation

| Key | Action |
|-----|--------|
| Alt + ↓ | Open popup |
| Alt + ↑ | Close popup |
| Escape | Close popup |
| Tab | Move to next element |
| Shift + Tab | Move to previous element |

#### Calendar Navigation (Popup Open)

| Key | Action |
|-----|--------|
| ↑ | Previous day row |
| ↓ | Next day row |
| ← | Previous day |
| → | Next day |
| Home | First day of month |
| End | Last day of month |
| Page Up | Previous month |
| Page Down | Next month |
| Ctrl + Home | First day of decade |
| Ctrl + End | Last day of decade |
| Enter / Space | Select date |

#### Time Picker Navigation (Popup Open)

| Key | Action |
|-----|--------|
| ↑ | Previous time |
| ↓ | Next time |
| ← | Previous time |
| → | Next time |

### Keyboard Example

```tsx
import React, { useState } from 'react';
import { DateTimePickerComponent } from '@syncfusion/ej2-react-calendars';

export default function KeyboardExample() {
  const [value, setValue] = useState<Date | null>(new Date());

  return (
    <div style={{ padding: '20px' }}>
      <h2>Keyboard Navigation Demo</h2>

      <DateTimePickerComponent
        value={value}
        change={(e) => setValue((e as any).value)}
        format="MM/dd/yyyy hh:mm a"
        placeholder="Press Alt+Down to open, then use arrow keys"
      />

      <h3>Keyboard Controls:</h3>
      <ul>
        <li><kbd>Alt</kbd> + <kbd>↓</kbd> - Open picker</li>
        <li><kbd>↑</kbd> <kbd>↓</kbd> <kbd>←</kbd> <kbd>→</kbd> - Navigate</li>
        <li><kbd>Enter</kbd> - Select</li>
        <li><kbd>Escape</kbd> - Close</li>
      </ul>
    </div>
  );
}
```

## Screen Reader Support

DateTimePicker announces changes and state to screen readers like NVDA, JAWS, and VoiceOver.

### Screen Reader Announcements

- ✅ Component labels
- ✅ Selected date announcement
- ✅ Error messages
- ✅ Calendar navigation
- ✅ Day/month/year values
- ✅ Day of week
- ✅ Disabled states

### Screen-Reader-Friendly Implementation

```tsx
import React, { useState } from 'react';
import { DateTimePickerComponent } from '@syncfusion/ej2-react-calendars';

export default function ScreenReaderFriendly() {
  const [meeting, setMeeting] = useState({
    title: '',
    dateTime: null as Date | null,
    attendees: '',
  });

  const [announceMessage, setAnnounceMessage] = useState('');

  const handleDateChange = (args: any) => {
    const newDate = args.value as Date;
    setMeeting({ ...meeting, dateTime: newDate });

    // Announce to screen readers
    const announcement = `Meeting scheduled for ${newDate.toLocaleDateString()} at ${newDate.toLocaleTimeString()}`;
    setAnnounceMessage(announcement);
  };

  return (
    <div>
      {/* Hidden live region for announcements */}
      <div
        role="status"
        aria-live="polite"
        aria-atomic="true"
        style={{ position: 'absolute', left: '-10000px' }}
      >
        {announceMessage}
      </div>

      <form>
        <div>
          <label htmlFor="meeting-title">Meeting Title</label>
          <input
            id="meeting-title"
            type="text"
            value={meeting.title}
            onChange={(e) => setMeeting({ ...meeting, title: e.target.value })}
            placeholder="Enter meeting title"
          />
        </div>

        <div>
          <label htmlFor="meeting-datetime">Date and Time</label>
          <DateTimePickerComponent
            id="meeting-datetime"
            value={meeting.dateTime}
            onChange={handleDateChange}
            aria-label="Select meeting date and time"
            format="MM/dd/yyyy hh:mm a"
          />
        </div>

        <div>
          <label htmlFor="attendees">Attendees</label>
          <input
            id="attendees"
            type="email"
            value={meeting.attendees}
            onChange={(e) => setMeeting({ ...meeting, attendees: e.target.value })}
            placeholder="attendee@example.com"
          />
        </div>

        <button type="submit">Schedule Meeting</button>
      </form>
    </div>
  );
}
```

## Focus Management

Proper focus management ensures keyboard navigation works smoothly.

### Focus Indicators

```css
/* Ensure visible focus indicators */
.e-datetimepicker:focus,
.e-datetimepicker .e-input-group:focus-within {
  outline: 2px solid #2196F3;
  outline-offset: 2px;
}

.e-datetimepicker .e-day-cell:focus {
  outline: 2px solid #2196F3;
  outline-offset: 1px;
}
```

### Focus Trap in Modal

If DateTimePicker is in a modal, trap focus within it:

```tsx
import React, { useRef, useEffect } from 'react';
import { DateTimePickerComponent } from '@syncfusion/ej2-react-calendars';

export default function FocusTrapModal() {
  const dateTimeRef = useRef<HTMLDivElement>(null);

  useEffect(() => {
    const handleKeyDown = (e: KeyboardEvent) => {
      if (e.key === 'Tab') {
        // Trap focus within modal
        const focusableElements = dateTimeRef.current?.querySelectorAll(
          'button, [href], input, select, textarea, [tabindex]:not([tabindex="-1"])'
        );
        if (!focusableElements?.length) return;

        const firstElement = focusableElements[0] as HTMLElement;
        const lastElement = focusableElements[focusableElements.length - 1] as HTMLElement;

        if (e.shiftKey) {
          if (document.activeElement === firstElement) {
            e.preventDefault();
            lastElement.focus();
          }
        } else {
          if (document.activeElement === lastElement) {
            e.preventDefault();
            firstElement.focus();
          }
        }
      }
    };

    dateTimeRef.current?.addEventListener('keydown', handleKeyDown);
    return () => dateTimeRef.current?.removeEventListener('keydown', handleKeyDown);
  }, []);

  return (
    <div ref={dateTimeRef} role="dialog" aria-labelledby="modal-title">
      <h2 id="modal-title">Schedule Appointment</h2>
      <DateTimePickerComponent placeholder="Select date and time" />
      <button>Submit</button>
    </div>
  );
}
```

## Color Contrast

Ensure sufficient color contrast between text and background (WCAG AA requires 4.5:1 for normal text).

### Contrast-Compliant Styling

```css
/* ✅ Good contrast (dark text on light background) */
.e-datetimepicker .e-input-group {
  color: #212121;           /* Text: Dark gray/black */
  background-color: #ffffff; /* Background: White */
  /* Contrast ratio: 12.6:1 (WCAG AAA) */
}

/* ❌ Poor contrast */
.e-datetimepicker .e-input-group {
  color: #999999;           /* Text: Light gray */
  background-color: #f0f0f0; /* Background: Very light gray */
  /* Contrast ratio: 3.1:1 (FAILS WCAG AA) */
}
```

### Check Contrast

Use online tools to verify:
- [WebAIM Contrast Checker](https://webaim.org/resources/contrastchecker/)
- [Colour Contrast Analyser](https://www.tpgi.com/color-contrast-checker/)

## Disabled and Readonly States

Indicate disabled/readonly states clearly.

### Disabled State

```tsx
<DateTimePickerComponent
  enabled={false}
  aria-disabled="true"
  placeholder="This field is disabled"
/>
```

### Readonly State

```tsx
<DateTimePickerComponent
  readonly={true}
  value={new Date()}
  aria-readonly="true"
  placeholder="This field is readonly"
/>
```

### Visual Indicators

```css
/* Disabled state */
.e-datetimepicker:disabled,
.e-datetimepicker[aria-disabled="true"] {
  opacity: 0.6;
  background-color: #f5f5f5;
  cursor: not-allowed;
}

/* Readonly state */
.e-datetimepicker[readonly],
.e-datetimepicker[aria-readonly="true"] {
  background-color: #f9f9f9;
  cursor: default;
}
```

## Testing Accessibility

### Manual Testing Checklist

- [ ] **Keyboard**: Can navigate and select using only keyboard (Tab, Arrow keys, Enter)
- [ ] **Focus**: Focus indicator clearly visible at all times
- [ ] **Labels**: All inputs have associated labels
- [ ] **Errors**: Error messages are announced and associated with inputs
- [ ] **Screen Reader**: Test with at least one screen reader (NVDA, JAWS, VoiceOver)
- [ ] **Contrast**: Color contrast meets WCAG AA (4.5:1)
- [ ] **Zoom**: Works correctly when zoomed to 200%
- [ ] **Disabled**: Disabled state is clearly indicated

### Automated Testing Tools

```bash
# Install accessibility testing tools
npm install --save-dev axe-core jest-axe

# Run accessibility tests
npm test
```

### Automated Test Example

```tsx
import React from 'react';
import { render } from '@testing-library/react';
import { axe, toHaveNoViolations } from 'jest-axe';
import { DateTimePickerComponent } from '@syncfusion/ej2-react-calendars';

expect.extend(toHaveNoViolations);

test('DateTimePicker should not have accessibility violations', async () => {
  const { container } = render(
    <DateTimePickerComponent
      aria-label="Select date and time"
      placeholder="Select date and time"
    />
  );

  const results = await axe(container);
  expect(results).toHaveNoViolations();
});
```

### Screen Reader Testing

**With NVDA (Windows):**
1. Download [NVDA](https://www.nvaccess.org/download/)
2. Start NVDA before testing
3. Tab through the component
4. Listen to announcements

**With VoiceOver (macOS/iOS):**
1. Enable: Cmd + F5 or System Preferences → Accessibility → VoiceOver
2. Navigate with arrow keys
3. Listen to announcements

## Summary

- **WCAG Compliance**: Meets Level AA standards out-of-the-box
- **ARIA**: Add labels, descriptions, and required/invalid indicators
- **Keyboard**: Full navigation with Tab, Arrow keys, Enter, Escape
- **Screen Readers**: Announces dates, events, errors
- **Focus**: Clear focus indicators and proper focus management
- **Contrast**: Use 4.5:1 ratio for normal text
- **Testing**: Use automated tools (axe-core) and manual screen reader testing

