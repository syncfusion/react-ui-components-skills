# Customization and Styling

## Table of Contents
- [Overview](#overview)
- [CSS Class Customization](#css-class-customization)
- [Theme Options](#theme-options)
- [RTL Support](#rtl-support)
- [Full-Screen Mode](#full-screen-mode)
- [Width and Height](#width-and-height)
- [Custom CSS Variables](#custom-css-variables)
- [Accessibility Features](#accessibility-features)
- [Common Patterns](#common-patterns)

## Overview

DateRangePicker provides extensive customization options:
- Built-in themes (Material, Bootstrap, Fluent, Tailwind)
- CSS class customization via `cssClass` property
- CSS variables for theme customization
- RTL (right-to-left) support
- Mobile full-screen mode
- Width, height, and z-index configuration
- Accessibility features (ARIA, keyboard navigation)

## CSS Class Customization

### Apply Custom CSS Class

```tsx
import { DateRangePickerComponent } from '@syncfusion/ej2-react-calendars';
import * as React from 'react';
import './CustomDateRangePicker.css';

function CSSClassExample() {
  return (
    <div style={{ padding: '20px' }}>
      <h3>Custom CSS Styling</h3>
      
      <DateRangePickerComponent
        id="daterangepicker"
        placeholder="Select date range"
        cssClass="custom-drp"
      />
    </div>
  );
}

export default CSSClassExample;
```

**CustomDateRangePicker.css:**

```css
/* Custom styling for DateRangePicker */
.e-daterangepicker.custom-drp.e-input-group {
  border: 2px solid #1976d2;
  border-radius: 8px;
  background-color: #f8f9fa;
}

.e-daterangepicker.custom-drp .e-input {
  font-size: 14px;
  font-weight: 500;
  padding: 10px 12px;
  color: #333;
}

.e-daterangepicker.custom-drp .e-input:focus {
  border-color: #1976d2;
  box-shadow: 0 0 0 3px rgba(25, 118, 210, 0.1);
}

/* Popup styling */
.e-daterangepicker.custom-drp .e-popup {
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.15);
  border-radius: 8px;
}

/* Calendar header */
.e-daterangepicker.custom-drp .e-calendar .e-header {
  background: linear-gradient(135deg, #1976d2 0%, #1565c0 100%);
}
```

### Multiple CSS Classes

```tsx
import { DateRangePickerComponent } from '@syncfusion/ej2-react-calendars';
import * as React from 'react';

function MultipleClassesExample() {
  return (
    <div style={{ padding: '20px' }}>
      <DateRangePickerComponent
        id="daterangepicker"
        placeholder="Select date range"
        cssClass="primary-theme rounded-corners elevated"
      />
    </div>
  );
}

export default MultipleClassesExample;
```

**Stylesheet:**

```css
/* Primary theme */
.e-daterangepicker.primary-theme .e-input {
  color: #1976d2;
  font-weight: 600;
}

/* Rounded corners */
.e-daterangepicker.rounded-corners .e-input-group {
  border-radius: 12px;
}

/* Elevated shadow */
.e-daterangepicker.elevated .e-input-group {
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}
```

## Theme Options

### Built-In Themes

Syncfusion provides five built-in themes:

```tsx
import { DateRangePickerComponent } from '@syncfusion/ej2-react-calendars';
import * as React from 'react';
import '@syncfusion/ej2-base/styles/material3.css';
import '@syncfusion/ej2-calendars/styles/material3.css';
// For other themes, import corresponding CSS files

function ThemeExample() {
  const [theme, setTheme] = React.useState<string>('material3');

  const themes = [
    { name: 'Material 3', value: 'material3' },
    { name: 'Bootstrap 5', value: 'bootstrap5' },
    { name: 'Fluent', value: 'fluent' },
    { name: 'Tailwind', value: 'tailwind3' },
    { name: 'Fabric', value: 'fabric' },
  ];

  return (
    <div style={{ padding: '20px' }}>
      <h3>Theme Selection</h3>

      <div style={{ marginBottom: '20px' }}>
        {themes.map((t) => (
          <label key={t.value} style={{ display: 'block', marginBottom: '8px' }}>
            <input
              type="radio"
              name="theme"
              value={t.value}
              checked={theme === t.value}
              onChange={(e) => setTheme(e.target.value)}
            />
            {' '}{t.name}
          </label>
        ))}
      </div>

      <DateRangePickerComponent
        id="daterangepicker"
        placeholder="Select date range"
        cssClass={`theme-${theme}`}
      />
    </div>
  );
}

export default ThemeExample;
```

### Theme CSS Imports

```tsx
// Material 3 (Modern, recommended)
import '@syncfusion/ej2-base/styles/material3.css';
import '@syncfusion/ej2-calendars/styles/material3.css';

// Bootstrap 5
import '@syncfusion/ej2-base/styles/bootstrap5.css';
import '@syncfusion/ej2-calendars/styles/bootstrap5.css';

// Fluent (Microsoft)
import '@syncfusion/ej2-base/styles/fluent.css';
import '@syncfusion/ej2-calendars/styles/fluent.css';

// Tailwind 3
import '@syncfusion/ej2-base/styles/tailwind3.css';
import '@syncfusion/ej2-calendars/styles/tailwind3.css';

// Fabric (Office)
import '@syncfusion/ej2-base/styles/fabric.css';
import '@syncfusion/ej2-calendars/styles/fabric.css';
```

## RTL Support

### Enable Right-to-Left

```tsx
import { DateRangePickerComponent } from '@syncfusion/ej2-react-calendars';
import * as React from 'react';

function RTLExample() {
  const [isRTL, setIsRTL] = React.useState<boolean>(false);

  return (
    <div style={{ padding: '20px' }}>
      <label style={{ marginBottom: '20px', display: 'block' }}>
        <input
          type="checkbox"
          checked={isRTL}
          onChange={(e) => setIsRTL(e.target.checked)}
        />
        {' '}Enable RTL
      </label>

      <div style={{ direction: isRTL ? 'rtl' : 'ltr' }}>
        <DateRangePickerComponent
          id="daterangepicker"
          placeholder="اختر نطاق التاريخ"
          enableRtl={isRTL}
          locale="ar-SA"
        />
      </div>

      <p style={{ marginTop: '20px', fontSize: '12px', color: '#666' }}>
        {isRTL ? 'RTL enabled - Arabic layout' : 'LTR enabled - English layout'}
      </p>
    </div>
  );
}

export default RTLExample;
```

### RTL with Custom Styling

```css
/* RTL-specific styles */
.e-daterangepicker[dir="rtl"] .e-input-group {
  direction: rtl;
}

.e-daterangepicker[dir="rtl"] .e-input-group-icon {
  order: -1;
  margin-left: 12px;
  margin-right: 0;
}

.e-daterangepicker[dir="rtl"] .e-popup {
  left: auto !important;
  right: 0;
}
```

## Full-Screen Mode

### Mobile Full-Screen Mode

```tsx
import { DateRangePickerComponent } from '@syncfusion/ej2-react-calendars';
import * as React from 'react';

function FullScreenModeExample() {
  const [fullScreen, setFullScreen] = React.useState<boolean>(false);

  return (
    <div style={{ padding: '20px' }}>
      <label style={{ marginBottom: '20px', display: 'block' }}>
        <input
          type="checkbox"
          checked={fullScreen}
          onChange={(e) => setFullScreen(e.target.checked)}
        />
        {' '}Full-Screen Mode
      </label>

      <DateRangePickerComponent
        id="daterangepicker"
        placeholder="Select date range"
        fullScreenMode={fullScreen}
      />

      <p style={{ marginTop: '20px', fontSize: '12px', color: '#666' }}>
        {fullScreen
          ? 'Full-screen mode enabled (mobile-optimized)'
          : 'Normal mode (popup)'}
      </p>
    </div>
  );
}

export default FullScreenModeExample;
```

### Auto Full-Screen for Mobile

```tsx
import { DateRangePickerComponent } from '@syncfusion/ej2-react-calendars';
import * as React from 'react';

function AutoFullScreenExample() {
  const [isMobile, setIsMobile] = React.useState<boolean>(window.innerWidth < 768);

  React.useEffect(() => {
    const handleResize = () => {
      setIsMobile(window.innerWidth < 768);
    };

    window.addEventListener('resize', handleResize);
    return () => window.removeEventListener('resize', handleResize);
  }, []);

  return (
    <div style={{ padding: '20px' }}>
      <DateRangePickerComponent
        id="daterangepicker"
        placeholder="Select date range"
        fullScreenMode={isMobile}
      />

      <p style={{ marginTop: '20px', fontSize: '12px', color: '#666' }}>
        {isMobile ? '📱 Mobile view (full-screen enabled)' : '💻 Desktop view (popup mode)'}
      </p>
    </div>
  );
}

export default AutoFullScreenExample;
```

## Width and Height

### Fixed Dimensions

```tsx
import { DateRangePickerComponent } from '@syncfusion/ej2-react-calendars';
import * as React from 'react';

function DimensionExample() {
  return (
    <div style={{ padding: '20px' }}>
      <h3>Component Dimensions</h3>

      <div style={{ marginBottom: '30px' }}>
        <h4>Fixed Width (400px)</h4>
        <DateRangePickerComponent
          id="daterangepicker1"
          placeholder="Select date range"
          width={400}
        />
      </div>

      <div style={{ marginBottom: '30px' }}>
        <h4>Percentage Width (100%)</h4>
        <DateRangePickerComponent
          id="daterangepicker2"
          placeholder="Select date range"
          width="100%"
        />
      </div>

      <div style={{ marginBottom: '30px' }}>
        <h4>Custom Width (300px)</h4>
        <DateRangePickerComponent
          id="daterangepicker3"
          placeholder="Select date range"
          width="300px"
        />
      </div>
    </div>
  );
}

export default DimensionExample;
```

### Responsive Width

```tsx
import { DateRangePickerComponent } from '@syncfusion/ej2-react-calendars';
import * as React from 'react';

function ResponsiveWidthExample() {
  const [width, setWidth] = React.useState<string>('100%');

  React.useEffect(() => {
    const handleResize = () => {
      if (window.innerWidth < 480) {
        setWidth('100%');
      } else if (window.innerWidth < 768) {
        setWidth('90%');
      } else {
        setWidth('500px');
      }
    };

    handleResize();
    window.addEventListener('resize', handleResize);
    return () => window.removeEventListener('resize', handleResize);
  }, []);

  return (
    <div style={{ padding: '20px' }}>
      <DateRangePickerComponent
        id="daterangepicker"
        placeholder="Select date range"
        width={width}
      />
      <p style={{ marginTop: '10px', fontSize: '12px', color: '#666' }}>
        Current width: {width}
      </p>
    </div>
  );
}

export default ResponsiveWidthExample;
```

## Custom CSS Variables

### Theme Variables Customization

```css
/* Custom CSS variables for theme customization */
:root {
  /* Primary colors */
  --primary-color: #1976d2;
  --primary-light: #bbdefb;
  --primary-dark: #0d47a1;

  /* Input styling */
  --input-border-color: #e0e0e0;
  --input-focus-color: #1976d2;
  --input-background: #ffffff;

  /* Calendar styling */
  --calendar-background: #ffffff;
  --calendar-selected: #1976d2;
  --calendar-hover: #e3f2fd;

  /* Text colors */
  --text-primary: #212121;
  --text-secondary: #757575;
  --text-disabled: #bdbdbd;
}

.e-daterangepicker .e-input {
  border-color: var(--input-border-color);
  background-color: var(--input-background);
  color: var(--text-primary);
}

.e-daterangepicker .e-input:focus {
  border-color: var(--input-focus-color);
  color: var(--text-primary);
}

.e-calendar .e-cell.e-selected {
  background-color: var(--calendar-selected);
}

.e-calendar .e-cell:hover {
  background-color: var(--calendar-hover);
}
```

## Accessibility Features

### ARIA Attributes

```tsx
import { DateRangePickerComponent } from '@syncfusion/ej2-react-calendars';
import * as React from 'react';

function AccessibilityExample() {
  const htmlAttributes = {
    'aria-label': 'Hotel booking date range',
    'aria-describedby': 'date-range-help',
    'role': 'group',
  };

  return (
    <div style={{ padding: '20px' }}>
      <label htmlFor="daterangepicker" style={{ display: 'block', marginBottom: '8px' }}>
        <strong>Select Your Travel Dates</strong>
      </label>

      <DateRangePickerComponent
        id="daterangepicker"
        placeholder="Select date range"
        htmlAttributes={htmlAttributes}
      />

      <p id="date-range-help" style={{ fontSize: '12px', color: '#666', marginTop: '8px' }}>
        Select check-in and check-out dates for your hotel booking
      </p>
    </div>
  );
}

export default AccessibilityExample;
```

### Keyboard Navigation

DateRangePicker supports standard keyboard navigation:

- **Tab** - Move focus to component
- **Enter/Space** - Open/close popup
- **Arrow Keys** - Navigate calendar
- **Escape** - Close popup
- **Ctrl+Left/Right** - Navigate months

```tsx
import { DateRangePickerComponent } from '@syncfusion/ej2-react-calendars';
import * as React from 'react';

function KeyboardNavigationExample() {
  const [keyboardLog, setKeyboardLog] = React.useState<string[]>([]);

  const handleKeyDown = (e: React.KeyboardEvent) => {
    setKeyboardLog((prev) => [...prev.slice(-4), `Pressed: ${e.key}`]);
  };

  return (
    <div style={{ padding: '20px', display: 'grid', gridTemplateColumns: '1fr 1fr', gap: '20px' }}>
      <div>
        <h3>Keyboard Navigation</h3>
        <DateRangePickerComponent
          id="daterangepicker"
          placeholder="Select date range"
        />
        <p style={{ marginTop: '10px', fontSize: '12px', color: '#666' }}>
          Use Tab, Enter, Arrow Keys, and Escape
        </p>
      </div>

      <div style={{ padding: '10px', border: '1px solid #ccc', borderRadius: '4px' }}>
        <h4>Last Keys Pressed:</h4>
        <ul style={{ margin: 0, paddingLeft: '20px', fontSize: '12px' }}>
          {keyboardLog.map((key, idx) => (
            <li key={idx}>{key}</li>
          ))}
        </ul>
      </div>
    </div>
  );
}

export default KeyboardNavigationExample;
```

## Common Patterns

### Pattern: Dark Mode Theme

```tsx
import { DateRangePickerComponent } from '@syncfusion/ej2-react-calendars';
import * as React from 'react';
import './DarkModeTheme.css';

function DarkModeExample() {
  const [isDarkMode, setIsDarkMode] = React.useState<boolean>(false);

  return (
    <div
      style={{
        padding: '20px',
        backgroundColor: isDarkMode ? '#121212' : '#ffffff',
        color: isDarkMode ? '#ffffff' : '#000000',
        minHeight: '100vh',
        transition: 'all 0.3s ease',
      }}
    >
      <label style={{ marginBottom: '20px', display: 'block' }}>
        <input
          type="checkbox"
          checked={isDarkMode}
          onChange={(e) => setIsDarkMode(e.target.checked)}
        />
        {' '}Dark Mode
      </label>

      <DateRangePickerComponent
        id="daterangepicker"
        placeholder="Select date range"
        cssClass={isDarkMode ? 'dark-mode-drp' : 'light-mode-drp'}
      />
    </div>
  );
}

export default DarkModeExample;
```

**DarkModeTheme.css:**

```css
/* Light Mode */
.e-daterangepicker.light-mode-drp .e-input-group {
  background-color: #ffffff;
  border-color: #e0e0e0;
}

.e-daterangepicker.light-mode-drp .e-input {
  color: #212121;
}

/* Dark Mode */
.e-daterangepicker.dark-mode-drp .e-input-group {
  background-color: #242424;
  border-color: #424242;
}

.e-daterangepicker.dark-mode-drp .e-input {
  color: #ffffff;
}

.e-daterangepicker.dark-mode-drp .e-popup {
  background-color: #242424;
  color: #ffffff;
}

.e-daterangepicker.dark-mode-drp .e-calendar .e-cell {
  color: #ffffff;
}

.e-daterangepicker.dark-mode-drp .e-calendar .e-cell.e-selected {
  background-color: #1976d2;
}
```

### Pattern: Custom Styled Preset Ranges

```tsx
import { DateRangePickerComponent } from '@syncfusion/ej2-react-calendars';
import * as React from 'react';

function StyledPresetRangesExample() {
  const [range, setRange] = React.useState<{
    start: Date | null;
    end: Date | null;
  }>({ start: null, end: null });

  const today = new Date();

  const presets = [
    {
      label: 'Last 7 Days',
      color: '#FF6B6B',
      value: {
        start: new Date(today.getTime() - 7 * 86400000),
        end: new Date(today),
      },
    },
    {
      label: 'Last 30 Days',
      color: '#4ECDC4',
      value: {
        start: new Date(today.getTime() - 30 * 86400000),
        end: new Date(today),
      },
    },
    {
      label: 'This Month',
      color: '#45B7D1',
      value: {
        start: new Date(today.getFullYear(), today.getMonth(), 1),
        end: new Date(today),
      },
    },
    {
      label: 'Last Quarter',
      color: '#96CEB4',
      value: {
        start: new Date(today.getTime() - 90 * 86400000),
        end: new Date(today),
      },
    },
  ];

  return (
    <div style={{ padding: '20px' }}>
      <DateRangePickerComponent
        id="daterangepicker"
        placeholder="Select date range"
        startDate={range.start}
        endDate={range.end}
        change={(e: any) => setRange({ start: e.startDate, end: e.endDate })}
      />

      <div style={{
        display: 'grid',
        gridTemplateColumns: 'repeat(auto-fit, minmax(120px, 1fr))',
        gap: '10px',
        marginTop: '20px',
      }}>
        {presets.map((preset) => (
          <button
            key={preset.label}
            onClick={() => setRange(preset.value)}
            style={{
              padding: '12px',
              backgroundColor: preset.color,
              color: 'white',
              border: 'none',
              borderRadius: '8px',
              cursor: 'pointer',
              fontWeight: 'bold',
              transition: 'transform 0.2s',
              transform: range.start === preset.value.start ? 'scale(1.05)' : 'scale(1)',
            }}
            onMouseEnter={(e) => {
              e.currentTarget.style.transform = 'scale(1.08)';
            }}
            onMouseLeave={(e) => {
              e.currentTarget.style.transform =
                range.start === preset.value.start ? 'scale(1.05)' : 'scale(1)';
            }}
          >
            {preset.label}
          </button>
        ))}
      </div>
    </div>
  );
}

export default StyledPresetRangesExample;
```
