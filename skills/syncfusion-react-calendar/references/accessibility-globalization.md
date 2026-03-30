# Accessibility & Globalization

## Table of Contents
- [Accessibility Overview](#accessibility-overview)
- [WCAG Compliance](#wcag-compliance)
- [Keyboard Navigation](#keyboard-navigation)
- [ARIA Attributes](#aria-attributes)
- [Screen Reader Support](#screen-reader-support)
- [Globalization (Locales)](#globalization-locales)
- [RTL (Right-to-Left) Languages](#rtl-right-to-left-languages)
- [Testing for Accessibility](#testing-for-accessibility)

---

## Accessibility Overview

The Syncfusion Calendar component is built with accessibility in mind, but you should verify and enhance it for your specific use case:

- Built-in ARIA labels and roles
- Keyboard navigation (Tab, Arrow keys, Enter)
- Focus indicators and state management
- Locale support for multiple languages
- RTL rendering for right-to-left languages

---

## WCAG Compliance

The Calendar aims to comply with WCAG 2.1 Level AA standards:

- **Perceivable:** Color contrast, text alternatives, distinguishable elements
- **Operable:** Keyboard accessible, sufficient time, seizure prevention
- **Understandable:** Readable text, predictable behavior, clear labels
- **Robust:** Valid HTML, compatible with assistive technologies

### Verification Checklist

- [ ] Text color contrast ≥ 4.5:1 for normal text, ≥ 3:1 for large text
- [ ] All interactive elements are keyboard accessible
- [ ] Focus indicators are visible (≥ 3px border or outline)
- [ ] Error messages are descriptive and announced to screen readers
- [ ] Date format is clear and consistent

---

## Keyboard Navigation

The Calendar supports standard keyboard shortcuts:

| Key | Action |
|-----|--------|
| `Tab` | Move focus to/from the calendar |
| `Arrow Up` | Select date in previous week |
| `Arrow Down` | Select date in next week |
| `Arrow Left` | Select previous day |
| `Arrow Right` | Select next day |
| `Enter` / `Space` | Confirm selection |
| `Page Up` | Navigate to previous month |
| `Page Down` | Navigate to next month |
| `Ctrl` + `Page Up` | Navigate to previous year |
| `Ctrl` + `Page Down` | Navigate to next year |

### Testing Keyboard Navigation

```jsx
import React, { useState } from 'react';
import { CalendarComponent } from '@syncfusion/ej2-react-calendars';

export default function KeyboardA11yExample() {
  const [value, setValue] = useState(new Date());

  const onChange = (args) => {
    console.log('Date selected via keyboard or mouse:', args.value);
  };

  return (
    <div style={{ padding: 20 }}>
      <label htmlFor="calendar-input" style={{ display: 'block', marginBottom: 10 }}>
        Select a date (use keyboard arrows to navigate):
      </label>
      <CalendarComponent
        id="calendar-input"
        value={value}
        change={onChange}
      />
      <p>Selected: {value.toDateString()}</p>
    </div>
  );
}
```

---

## ARIA Attributes

The Calendar includes default ARIA attributes, but you should enhance them for context:

```jsx
import React, { useState } from 'react';
import { CalendarComponent } from '@syncfusion/ej2-react-calendars';

export default function ARIAExample() {
  const [value, setValue] = useState(new Date());

  return (
    <div style={{ padding: 20 }}>
      {/* Container with ARIA labels */}
      <div
        role="region"
        aria-labelledby="calendar-title"
        aria-describedby="calendar-desc"
      >
        <h2 id="calendar-title">Event Date Picker</h2>
        <p id="calendar-desc">
          Use arrow keys to navigate dates. Press Enter to select.
        </p>
        {/* Wrap the calendar in a labelled container for accessibility context */}
        <CalendarComponent
          value={value}
          change={(e) => setValue(e.value)}
        />
      </div>
    </div>
  );
}
```

### Common ARIA Attributes (applied to the wrapper container)

- `aria-label` — Provides an accessible name for the container region
- `aria-describedby` — Links to a description element for the container
- `role="region"` — Marks the container as a significant section
- `aria-live="polite"` — On a separate live region element to announce date changes

> **Note:** `aria-label`, `tabIndex`, and similar ARIA attributes are not official props of `CalendarComponent`. Apply them to a wrapping `<div>` or use a live region element alongside the component, as shown in the examples.

---

## Screen Reader Support

### Testing with NVDA (Windows) or JAWS

1. **Enable screen reader:** Start NVDA (Ctrl+Alt+N) or JAWS.
2. **Navigate the calendar:** Use Tab to focus, arrow keys to select dates.
3. **Listen for announcements:** Screen reader should announce:
   - Component role ("calendar")
   - Current date being selected
   - Month/year navigation changes
   - Disabled date status

### Announcing Date Changes

Enhance the change event with live region updates:

```jsx
import React, { useState, useRef } from 'react';
import { CalendarComponent } from '@syncfusion/ej2-react-calendars';

export default function ScreenReaderExample() {
  const [value, setValue] = useState(new Date());
  const announceRef = useRef(null);

  const onChange = (args) => {
    setValue(args.value);
    // Announce the selected date to screen readers
    if (announceRef.current) {
      announceRef.current.textContent = `Date selected: ${args.value.toDateString()}`;
    }
  };

  return (
    <div style={{ padding: 20 }}>
      <CalendarComponent value={value} change={onChange} />
      {/* Live region for screen reader announcements */}
      <div
        ref={announceRef}
        aria-live="polite"
        aria-atomic="true"
        style={{ position: 'absolute', left: '-10000px' }}
      />
    </div>
  );
}
```

---

## Globalization (Locales)

The Calendar supports 100+ locales. Set the locale via the `locale` prop:

### Common Locales

```jsx
<CalendarComponent locale="en-US" />    // English (US)
<CalendarComponent locale="en-GB" />    // English (UK)
<CalendarComponent locale="de-DE" />    // German
<CalendarComponent locale="fr-FR" />    // French
<CalendarComponent locale="es-ES" />    // Spanish
<CalendarComponent locale="it-IT" />    // Italian
<CalendarComponent locale="ja-JP" />    // Japanese
<CalendarComponent locale="zh-CN" />    // Chinese (Simplified)
<CalendarComponent locale="hi-IN" />    // Hindi
```

### Example: Multi-Language Picker

```jsx
import React, { useState } from 'react';
import { CalendarComponent } from '@syncfusion/ej2-react-calendars';

export default function LocalePickerExample() {
  const [locale, setLocale] = useState('en-US');
  const [value, setValue] = useState(new Date());

  const locales = [
    { code: 'en-US', name: 'English (US)' },
    { code: 'de-DE', name: 'Deutsch' },
    { code: 'fr-FR', name: 'Français' },
    { code: 'es-ES', name: 'Español' },
    { code: 'ja-JP', name: '日本語' },
  ];

  return (
    <div style={{ padding: 20 }}>
      <label htmlFor="locale-select">Select Language:</label>
      <select
        id="locale-select"
        value={locale}
        onChange={(e) => setLocale(e.target.value)}
      >
        {locales.map((l) => (
          <option key={l.code} value={l.code}>
            {l.name}
          </option>
        ))}
      </select>

      <div style={{ marginTop: 20 }}>
        <CalendarComponent
          locale={locale}
          value={value}
          change={(e) => setValue(e.value)}
        />
        <p>Selected: {value.toLocaleDateString(locale)}</p>
      </div>
    </div>
  );
}
```

### Date Formatting by Locale

```jsx
const date = new Date(2026, 2, 15); // March 15, 2026

console.log(date.toLocaleDateString('en-US'));   // 3/15/2026
console.log(date.toLocaleDateString('de-DE'));   // 15.3.2026
console.log(date.toLocaleDateString('fr-FR'));   // 15/03/2026
console.log(date.toLocaleDateString('ja-JP'));   // 2026/3/15
```

---

## RTL (Right-to-Left) Languages

For Arabic, Hebrew, Persian, and other RTL languages, set both the locale and RTL flag:

```jsx
import React, { useState } from 'react';
import { CalendarComponent } from '@syncfusion/ej2-react-calendars';

export default function RTLExample() {
  const [value, setValue] = useState(new Date());

  return (
    <div style={{ padding: 20, direction: 'rtl', textAlign: 'right' }}>
      <h3>تقويم يومي</h3>
      <CalendarComponent
        locale="ar"
        enableRtl={true}
        value={value}
        change={(e) => setValue(e.value)}
      />
      <p>التاريخ المختار: {value.toLocaleDateString('ar-EG')}</p>
    </div>
  );
}
```

### CSS for RTL Containers

```css
/* Global RTL support */
[dir='rtl'] {
  direction: rtl;
  text-align: right;
}

/* Calendar-specific RTL */
.e-calendar[data-rtl="true"] {
  flex-direction: row-reverse;
}
```

### RTL Testing Checklist

- [ ] Calendar layout is mirrored (left/right flipped)
- [ ] Day-of-week headers are in correct RTL order
- [ ] Navigation buttons (prev/next) are in correct positions
- [ ] Text and numbers are right-aligned
- [ ] Week start day matches locale (Sunday vs. Monday vs. Saturday)

---

## Testing for Accessibility

### Automated Tools

1. **axe DevTools** — Chrome extension for instant accessibility audits
   - Right-click → Inspect → axe DevTools → Scan
2. **Lighthouse** — Built into Chrome DevTools
   - DevTools → Lighthouse → Audit for accessibility
3. **WAVE** — WebAIM's web accessibility tool
   - Browser extension or online validator

### Manual Testing

1. **Keyboard-only navigation:** Unplug mouse and navigate the calendar using Tab, arrows, Enter.
2. **Screen reader testing:** Use NVDA (Windows) or VoiceOver (Mac) to verify announcements.
3. **Color contrast:** Use Color Contrast Analyzer to verify sufficient ratios.
4. **Focus indicators:** Tab through; ensure focus outline is visible at all times.

### Example Test Checklist

```jsx
// A11y Test Suite (pseudo-code)
describe('Calendar Accessibility', () => {
  it('should be keyboard navigable', () => {
    // Simulate Tab, Arrow keys, Enter
    // Verify date changes and focus moves
  });

  it('should announce changes to screen readers', () => {
    // Enable screen reader
    // Change date
    // Verify live region announces
  });

  it('should have sufficient color contrast', () => {
    // Check computed style of selected date
    // Verify contrast ≥ 4.5:1
  });

  it('should have visible focus indicators', () => {
    // Tab to calendar
    // Verify outline or border is visible
  });

  it('should support RTL locales', () => {
    // Set locale to 'ar', enableRtl={true}
    // Verify layout is mirrored
  });
});
```

---

## Best Practices

1. **Always provide labels:** Use `<label>` elements or `aria-label` for context.
2. **Test with actual assistive technologies:** Don't rely only on browser DevTools.
3. **Provide alternative date input:** Offer a text input alongside the calendar for users who prefer typing.
4. **Ensure sufficient spacing:** Touch targets should be at least 44x44 pixels on mobile.
5. **Test with real users:** Especially those who rely on assistive technologies.
6. **Document locale and RTL support:** Inform users which languages and scripts are supported.

---

## Resources

- [WCAG 2.1 Guidelines](https://www.w3.org/WAI/WCAG21/quickref/)
- [MDN: ARIA](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA)
- [Syncfusion Accessibility](https://www.syncfusion.com/products/react/accessibility)
- [WebAIM](https://webaim.org/)

