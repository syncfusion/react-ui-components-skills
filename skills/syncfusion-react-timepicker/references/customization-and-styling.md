# Customization and Styling

## Table of Contents
- [CSS Class Customization](#css-class-customization)
- [Theme Options](#theme-options)
- [Full-Screen Mode for Mobile](#full-screen-mode-for-mobile)
- [RTL (Right-to-Left) Support](#rtl-right-to-left-support)
- [Strict Mode Validation](#strict-mode-validation)
- [Z-Index Management](#z-index-management)
- [Width and Height Configuration](#width-and-height-configuration)
- [Accessibility Features](#accessibility-features)
- [Theme Studio Integration](#theme-studio-integration)

## CSS Class Customization

Use the `cssClass` property to apply custom CSS classes and styling:

### Basic CSS Customization

```tsx
import { TimePickerComponent } from '@syncfusion/ej2-react-calendars';
import * as React from 'react';
import './custom-timepicker.css';

function CssClassExample() {
  return (
    <div style={{ padding: '20px' }}>
      <h3>CSS Class Customization</h3>

      {/* Apply custom CSS class */}
      <TimePickerComponent
        value={new Date('1/1/2018 10:00 AM')}
        cssClass="custom-timepicker"
        placeholder="Select time"
      />
    </div>
  );
}

export default CssClassExample;
```

**custom-timepicker.css:**
```css
/* Custom TimePicker Styling */
.custom-timepicker .e-input-group {
  border: 2px solid #4CAF50;
  border-radius: 8px;
}

.custom-timepicker .e-input {
  font-size: 16px;
  font-weight: 500;
  color: #333;
}

.custom-timepicker .e-input:hover {
  border-color: #45a049;
}

.custom-timepicker .e-input:focus {
  outline: none;
  box-shadow: 0 0 5px rgba(76, 175, 80, 0.5);
}
```

### Multiple CSS Classes

```tsx
import { TimePickerComponent } from '@syncfusion/ej2-react-calendars';
import * as React from 'react';

function MultipleCssClassesExample() {
  return (
    <div style={{ padding: '20px' }}>
      <h3>Multiple CSS Classes</h3>

      <TimePickerComponent
        value={new Date('1/1/2018 10:00 AM')}
        cssClass="custom-timepicker large-input"
        placeholder="Select time"
      />
    </div>
  );
}

export default MultipleCssClassesExample;
```

## Theme Options

Syncfusion provides multiple pre-built themes. Choose one during installation:

### Available Themes

| Theme | File | Best For | Appearance |
|-------|------|----------|-----------|
| Material 3 | material3.css | Modern, recommended | Clean, minimal |
| Bootstrap 5 | bootstrap5.css | Bootstrap projects | Familiar Bootstrap look |
| Fluent | fluent.css | Microsoft apps | Microsoft Fluent Design |
| Tailwind | tailwind.css | Tailwind projects | Tailwind aesthetic |
| Fabric | fabric.css | Office apps | Office Fabric theme |
| Material | material.css | Material Design | Google Material |
| Highcontrast | highcontrast.css | Accessibility | High contrast for vision impaired |

### Applying Themes

```tsx
import { TimePickerComponent } from '@syncfusion/ej2-react-calendars';
import * as React from 'react';

// Material 3 (recommended)
import '@syncfusion/ej2-base/styles/material3.css';
import '@syncfusion/ej2-calendars/styles/material3.css';

function ThemeExample() {
  return (
    <div style={{ padding: '20px' }}>
      <h3>Material 3 Theme</h3>

      <TimePickerComponent
        value={new Date('1/1/2018 10:00 AM')}
        placeholder="Select time"
      />
    </div>
  );
}

export default ThemeExample;
```

### Theme Comparison

```tsx
// For Bootstrap 5 theme, replace imports:
// import '@syncfusion/ej2-base/styles/bootstrap5.css';
// import '@syncfusion/ej2-calendars/styles/bootstrap5.css';

// For Fluent theme:
// import '@syncfusion/ej2-base/styles/fluent.css';
// import '@syncfusion/ej2-calendars/styles/fluent.css';

// For Tailwind theme:
// import '@syncfusion/ej2-base/styles/tailwind.css';
// import '@syncfusion/ej2-calendars/styles/tailwind.css';
```

## Full-Screen Mode for Mobile

Enable `fullScreenMode` for mobile devices to show TimePicker in full-screen mode:

```tsx
import { TimePickerComponent } from '@syncfusion/ej2-react-calendars';
import * as React from 'react';

function FullScreenModeExample() {
  const [selectedTime, setSelectedTime] = React.useState<Date | null>(new Date('1/1/2018 10:00 AM'));

  return (
    <div style={{ padding: '20px' }}>
      <h3>Full-Screen Mode for Mobile</h3>

      <TimePickerComponent
        value={selectedTime}
        fullScreenMode={true}
        change={(e: any) => setSelectedTime(e.value)}
        placeholder="Select time"
      />

      <p style={{ marginTop: '15px', color: '#666' }}>
        On mobile devices, the time picker will appear in full-screen mode
      </p>
    </div>
  );
}

export default FullScreenModeExample;
```

### Responsive Full-Screen Setup

```tsx
import { TimePickerComponent } from '@syncfusion/ej2-react-calendars';
import * as React from 'react';

function ResponsiveFullScreenExample() {
  const [selectedTime, setSelectedTime] = React.useState<Date | null>(new Date('1/1/2018 10:00 AM'));

  // Detect if mobile device
  const isMobile = () => window.innerWidth < 768;

  return (
    <div style={{ padding: '20px' }}>
      <h3>Responsive Full-Screen</h3>

      <TimePickerComponent
        value={selectedTime}
        fullScreenMode={isMobile()}
        change={(e: any) => setSelectedTime(e.value)}
        placeholder="Select time"
      />

      <p>Full-screen: {isMobile() ? 'Yes' : 'No'}</p>
    </div>
  );
}

export default ResponsiveFullScreenExample;
```

## RTL (Right-to-Left) Support

Enable `enableRtl` for right-to-left languages like Arabic, Hebrew:

```tsx
import { TimePickerComponent } from '@syncfusion/ej2-react-calendars';
import * as React from 'react';

function RtlExample() {
  const [selectedTime, setSelectedTime] = React.useState<Date | null>(new Date('1/1/2018 10:00 AM'));
  const [isRtl, setIsRtl] = React.useState(false);

  return (
    <div style={{ padding: '20px' }}>
      <h3>RTL Support</h3>

      <label>
        <input
          type="checkbox"
          checked={isRtl}
          onChange={(e) => setIsRtl(e.target.checked)}
        />
        {' '}Enable RTL
      </label>

      <div style={{ marginTop: '15px' }}>
        <TimePickerComponent
          value={selectedTime}
          enableRtl={isRtl}
          locale={isRtl ? 'ar-SA' : 'en-US'}
          change={(e: any) => setSelectedTime(e.value)}
          placeholder="Select time"
        />
      </div>

      <p style={{ marginTop: '15px', direction: isRtl ? 'rtl' : 'ltr' }}>
        Direction: {isRtl ? 'Right to Left (Arabic)' : 'Left to Right (English)'}
      </p>
    </div>
  );
}

export default RtlExample;
```

### Multi-Language RTL Setup

```tsx
import { TimePickerComponent } from '@syncfusion/ej2-react-calendars';
import * as React from 'react';

function MultiLanguageRtlExample() {
  const [locale, setLocale] = React.useState('en-US');
  const [selectedTime, setSelectedTime] = React.useState<Date | null>(new Date('1/1/2018 10:00 AM'));

  const locales = {
    'en-US': { name: 'English (US)', rtl: false },
    'ar-SA': { name: 'Arabic (Saudi)', rtl: true },
    'he-IL': { name: 'Hebrew (Israel)', rtl: true },
    'de-DE': { name: 'German', rtl: false },
  };

  return (
    <div style={{ padding: '20px' }}>
      <h3>Multi-Language RTL Support</h3>

      <label>Select Language:</label>
      <select
        value={locale}
        onChange={(e) => setLocale(e.target.value)}
        style={{ padding: '5px', marginLeft: '10px' }}
      >
        {Object.entries(locales).map(([key, value]) => (
          <option key={key} value={key}>
            {value.name}
          </option>
        ))}
      </select>

      <div style={{ marginTop: '15px' }}>
        <TimePickerComponent
          value={selectedTime}
          locale={locale}
          enableRtl={locales[locale as keyof typeof locales].rtl}
          change={(e: any) => setSelectedTime(e.value)}
          placeholder="Select time"
        />
      </div>
    </div>
  );
}

export default MultiLanguageRtlExample;
```

## Strict Mode Validation

Enable `strictMode` to validate input and restrict to valid times:

```tsx
import { TimePickerComponent } from '@syncfusion/ej2-react-calendars';
import * as React from 'react';

function StrictModeExample() {
  const [selectedTime, setSelectedTime] = React.useState<Date | null>(new Date('1/1/2018 10:00 AM'));
  const [strictMode, setStrictMode] = React.useState(false);

  return (
    <div style={{ padding: '20px' }}>
      <h3>Strict Mode Validation</h3>

      <label>
        <input
          type="checkbox"
          checked={strictMode}
          onChange={(e) => setStrictMode(e.target.checked)}
        />
        {' '}Enable Strict Mode
      </label>

      <div style={{ marginTop: '15px' }}>
        <TimePickerComponent
          value={selectedTime}
          strictMode={strictMode}
          min={new Date('1/1/2018 9:00 AM')}
          max={new Date('1/1/2018 5:00 PM')}
          change={(e: any) => setSelectedTime(e.value)}
          placeholder="Enter valid time (9 AM - 5 PM)"
        />
      </div>

      <p style={{ marginTop: '15px', color: '#666' }}>
        {strictMode ? '✓ Only valid times within range are accepted' : '○ Any time can be entered'}
      </p>
    </div>
  );
}

export default StrictModeExample;
```

## Z-Index Management

Control popup stacking with `zIndex` property:

```tsx
import { TimePickerComponent } from '@syncfusion/ej2-react-calendars';
import * as React from 'react';

function ZIndexExample() {
  return (
    <div style={{ padding: '20px' }}>
      <h3>Z-Index Management</h3>

      {/* Default z-index (1000) */}
      <div>
        <label>Default Z-Index (1000):</label>
        <TimePickerComponent
          value={new Date('1/1/2018 10:00 AM')}
          placeholder="Select time"
        />
      </div>

      {/* Custom z-index for modals */}
      <div style={{ marginTop: '20px' }}>
        <label>High Z-Index (9999 - for modals):</label>
        <TimePickerComponent
          value={new Date('1/1/2018 10:00 AM')}
          zIndex={9999}
          placeholder="Select time"
        />
      </div>

      <p style={{ marginTop: '15px', color: '#666' }}>
        Increase z-index if popup appears behind other elements
      </p>
    </div>
  );
}

export default ZIndexExample;
```

## Width and Height Configuration

Control component dimensions with `width` property:

```tsx
import { TimePickerComponent } from '@syncfusion/ej2-react-calendars';
import * as React from 'react';

function WidthConfigExample() {
  return (
    <div style={{ padding: '20px' }}>
      <h3>Width Configuration</h3>

      {/* Default width */}
      <div>
        <label>Default Width:</label>
        <TimePickerComponent
          value={new Date('1/1/2018 10:00 AM')}
          placeholder="Select time"
        />
      </div>

      {/* Custom width in pixels */}
      <div style={{ marginTop: '20px' }}>
        <label>Width: 300px:</label>
        <TimePickerComponent
          value={new Date('1/1/2018 10:00 AM')}
          width="300px"
          placeholder="Select time"
        />
      </div>

      {/* Full width */}
      <div style={{ marginTop: '20px' }}>
        <label>Full Width (100%):</label>
        <TimePickerComponent
          value={new Date('1/1/2018 10:00 AM')}
          width="100%"
          placeholder="Select time"
        />
      </div>

      {/* Width in percentage */}
      <div style={{ marginTop: '20px' }}>
        <label>Width: 80%:</label>
        <TimePickerComponent
          value={new Date('1/1/2018 10:00 AM')}
          width="80%"
          placeholder="Select time"
        />
      </div>
    </div>
  );
}

export default WidthConfigExample;
```

## Accessibility Features

### Keyboard Navigation

```tsx
import { TimePickerComponent } from '@syncfusion/ej2-react-calendars';
import * as React from 'react';

function AccessibilityExample() {
  const [selectedTime, setSelectedTime] = React.useState<Date | null>(new Date('1/1/2018 10:00 AM'));

  return (
    <div style={{ padding: '20px' }}>
      <h3>Accessibility Features</h3>

      <label htmlFor="timepicker">Select Time:</label>
      <TimePickerComponent
        id="timepicker"
        value={selectedTime}
        change={(e: any) => setSelectedTime(e.value)}
        placeholder="Use arrow keys to select time"
        htmlAttributes={{
          'aria-label': 'Select appointment time',
          'aria-describedby': 'time-help',
          tabIndex: 0,
        }}
      />

      <div id="time-help" style={{ marginTop: '10px', fontSize: '12px', color: '#666' }}>
        Use arrow keys to navigate times, Enter to select, Escape to close
      </div>
    </div>
  );
}

export default AccessibilityExample;
```

### Custom Keyboard Shortcuts

```tsx
import { TimePickerComponent } from '@syncfusion/ej2-react-calendars';
import * as React from 'react';

function KeyboardShortcutsExample() {
  const customKeyConfigs = {
    enter: 'space',
    home: 'ctrl+home',
    end: 'ctrl+end',
    up: 'uparrow',
    down: 'downarrow',
  };

  return (
    <div style={{ padding: '20px' }}>
      <h3>Custom Keyboard Shortcuts</h3>

      <TimePickerComponent
        value={new Date('1/1/2018 10:00 AM')}
        keyConfigs={customKeyConfigs}
        placeholder="Select time"
      />

      <div style={{ marginTop: '20px', padding: '10px', backgroundColor: '#f5f5f5' }}>
        <h4>Keyboard Shortcuts:</h4>
        <ul style={{ fontSize: '12px' }}>
          <li>Space: Open/Close popup</li>
          <li>Ctrl+Home: Go to first time</li>
          <li>Ctrl+End: Go to last time</li>
          <li>Up/Down Arrow: Navigate times</li>
        </ul>
      </div>
    </div>
  );
}

export default KeyboardShortcutsExample;
```

## Theme Studio Integration

Syncfusion Theme Studio allows creating custom themes without coding:

1. Visit: https://ej2.syncfusion.com/themestudio/
2. Select TimePicker component
3. Customize colors, fonts, spacing
4. Download custom CSS
5. Import in your app

```tsx
// After downloading custom theme from Theme Studio
import '@/custom-theme.css';
import { TimePickerComponent } from '@syncfusion/ej2-react-calendars';
import * as React from 'react';

function ThemedExample() {
  return (
    <div style={{ padding: '20px' }}>
      <h3>Custom Theme (From Theme Studio)</h3>

      <TimePickerComponent
        value={new Date('1/1/2018 10:00 AM')}
        placeholder="Using custom theme"
      />
    </div>
  );
}

export default ThemedExample;
```

## Complete Customization Example

```tsx
import { TimePickerComponent } from '@syncfusion/ej2-react-calendars';
import * as React from 'react';
import './fully-customized.css';

function FullyCustomizedExample() {
  const [selectedTime, setSelectedTime] = React.useState<Date | null>(new Date('1/1/2018 10:00 AM'));
  const [isRtl, setIsRtl] = React.useState(false);

  return (
    <div style={{ padding: '20px', maxWidth: '500px' }}>
      <h3>Fully Customized TimePicker</h3>

      <label>
        <input
          type="checkbox"
          checked={isRtl}
          onChange={(e) => setIsRtl(e.target.checked)}
        />
        {' '}Enable RTL
      </label>

      <div style={{ marginTop: '20px' }}>
        <TimePickerComponent
          value={selectedTime}
          cssClass="custom-styled-timepicker"
          enableRtl={isRtl}
          width="100%"
          format="hh:mm a"
          min={new Date('1/1/2018 9:00 AM')}
          max={new Date('1/1/2018 5:00 PM')}
          step={15}
          strictMode={true}
          showClearButton={true}
          placeholder="Select time (9 AM - 5 PM)"
          change={(e: any) => setSelectedTime(e.value)}
        />
      </div>

      <div style={{ marginTop: '20px', padding: '15px', backgroundColor: '#f5f5f5', borderRadius: '4px' }}>
        <p>
          <strong>Selected:</strong> {selectedTime?.toLocaleTimeString()}
        </p>
        <p>
          <strong>RTL:</strong> {isRtl ? 'Enabled' : 'Disabled'}
        </p>
      </div>
    </div>
  );
}

export default FullyCustomizedExample;
```

**fully-customized.css:**
```css
/* Fully Customized TimePicker */
.custom-styled-timepicker .e-input-group {
  border: 2px solid #3f51b5;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(63, 81, 181, 0.1);
  transition: all 0.3s ease;
}

.custom-styled-timepicker .e-input-group:hover {
  box-shadow: 0 4px 8px rgba(63, 81, 181, 0.2);
}

.custom-styled-timepicker .e-input-group.e-focus {
  border-color: #1a237e;
  box-shadow: 0 4px 12px rgba(63, 81, 181, 0.3);
}

.custom-styled-timepicker .e-input {
  font-size: 15px;
  padding: 10px;
  color: #333;
}

.custom-styled-timepicker .e-icon-btn {
  color: #3f51b5;
}
```

## Related Topics

- [Time Format and Display](time-format-and-display.md) - Format customization
- [Events and Methods](events-and-methods.md) - Event handling
- [API Reference](api-reference.md) - Complete styling properties
