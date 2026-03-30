# Styling and Customization

## Table of Contents
- [Available Themes](#available-themes)
- [CSS Class Customization](#css-class-customization)
- [Float Label Types](#float-label-types)
- [Placeholder Configuration](#placeholder-configuration)
- [Width and Sizing](#width-and-sizing)
- [Responsive Design](#responsive-design)
- [Full-Screen Mode](#full-screen-mode)
- [RTL Support](#rtl-support)
- [Advanced CSS Customization](#advanced-css-customization)

## Available Themes

Syncfusion provides several pre-built themes. Choose one and import it in your application.

### Theme Files

- **material3.css** - Material Design 3 (modern, default)
- **bootstrap5.css** - Bootstrap 5 theme
- **fluent.css** - Microsoft Fluent Design System
- **tailwind.css** - Tailwind CSS theme
- **fabric.css** - Office Fabric theme
- **material.css** - Original Material Design

### Importing a Theme

```css
/* In your component CSS or global styles */
@import '../node_modules/@syncfusion/ej2-base/styles/material3.css';
@import '../node_modules/@syncfusion/ej2-calendars/styles/material3.css';
```

### Switching Themes Dynamically

```tsx
import React, { useState } from 'react';
import { DateTimePickerComponent } from '@syncfusion/ej2-react-calendars';
import './App.css';

export default function ThemeSwitcher() {
  const [theme, setTheme] = useState('material3');

  const loadTheme = (themeName: string) => {
    // Dynamically load theme CSS
    const link = document.getElementById('theme-link') as HTMLLinkElement;
    if (link) {
      link.href = `../node_modules/@syncfusion/ej2-calendars/styles/${themeName}.css`;
    }
    setTheme(themeName);
  };

  return (
    <div style={{ padding: '20px' }}>
      <select value={theme} onChange={(e) => loadTheme(e.target.value)}>
        <option value="material3">Material 3</option>
        <option value="bootstrap5">Bootstrap 5</option>
        <option value="fluent">Fluent</option>
        <option value="tailwind">Tailwind</option>
        <option value="fabric">Fabric</option>
      </select>

      <link id="theme-link" rel="stylesheet" href={`../node_modules/@syncfusion/ej2-calendars/styles/${theme}.css`} />

      <DateTimePickerComponent placeholder="Select date and time" />
    </div>
  );
}
```

## CSS Class Customization

Use the `cssClass` property to add custom CSS classes to the DateTimePicker root element.

### Basic cssClass Usage

```tsx
<DateTimePickerComponent
  cssClass="my-custom-picker"
  placeholder="Custom styled picker"
/>
```

### CSS for Custom Class

```css
.my-custom-picker {
  background-color: #f5f5f5;
  border-radius: 8px;
}

.my-custom-picker .e-input-group-icon {
  background-color: #2196F3;
  color: white;
}
```

### Multiple Custom Classes

```tsx
<DateTimePickerComponent
  cssClass="custom-picker theme-blue size-large"
  placeholder="Multi-class styling"
/>
```

```css
.custom-picker {
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
}

.theme-blue .e-input-group {
  border-color: #2196F3;
}

.size-large {
  font-size: 16px;
  padding: 12px;
}
```

### Example: Danger/Warning State

```tsx
import React, { useState } from 'react';
import { DateTimePickerComponent } from '@syncfusion/ej2-react-calendars';

export default function StateBasedStyling() {
  const [value, setValue] = useState<Date | null>(null);
  const [state, setState] = useState<'default' | 'warning' | 'error'>('default');

  const getCssClass = () => {
    const baseClass = 'e-datetimepicker';
    switch (state) {
      case 'warning':
        return `${baseClass} state-warning`;
      case 'error':
        return `${baseClass} state-error`;
      default:
        return baseClass;
    }
  };

  const handleChange = (args: any) => {
    setValue(args.value);

    if (!args.value) {
      setState('warning');
    } else if ((args.value as Date) < new Date()) {
      setState('error');
    } else {
      setState('default');
    }
  };

  return (
    <DateTimePickerComponent
      value={value}
      change={handleChange}
      cssClass={getCssClass()}
    />
  );
}
```

```css
.state-warning {
  border-color: #ff9800;
  background-color: #fff3e0;
}

.state-error {
  border-color: #f44336;
  background-color: #ffebee;
}
```

## Float Label Types

Control how the placeholder text floats or behaves.

### Float Label Options

```tsx
// Never: Placeholder stays in place (default)
<DateTimePickerComponent floatLabelType="Never" placeholder="Select date" />

// Always: Placeholder always floats above
<DateTimePickerComponent floatLabelType="Always" placeholder="Select date" />

// Auto: Placeholder floats on focus or input
<DateTimePickerComponent floatLabelType="Auto" placeholder="Select date" />
```

### Styling Float Labels

```css
.e-float-input.e-focus label,
.e-float-input input:not(.e-icon-right) ~ label {
  font-size: 12px;
  transform: translateY(-20px);
  color: #2196F3;
}

.e-float-input input:not(.e-icon-right) ~ label {
  color: #999;
}
```

### Example: Form with Float Labels

```tsx
import React, { useState } from 'react';
import { DateTimePickerComponent } from '@syncfusion/ej2-react-calendars';

export default function FloatLabelForm() {
  const [eventName, setEventName] = useState('');
  const [eventDateTime, setEventDateTime] = useState<Date | null>(null);

  return (
    <form style={{ padding: '20px', maxWidth: '400px' }}>
      <div style={{ marginBottom: '20px' }}>
        <label>Event Name</label>
        <input
          type="text"
          value={eventName}
          onChange={(e) => setEventName(e.target.value)}
          placeholder="Enter event name"
          style={{ width: '100%', padding: '8px' }}
        />
      </div>

      <div style={{ marginBottom: '20px' }}>
        <DateTimePickerComponent
          value={eventDateTime}
          change={(e) => setEventDateTime((e as any).value)}
          floatLabelType="Auto"
          placeholder="Select date and time"
          format="MM/dd/yyyy hh:mm a"
        />
      </div>

      <button type="submit">Create Event</button>
    </form>
  );
}
```

## Placeholder Configuration

Customize the input placeholder and masked input placeholders.

### Input Placeholder

```tsx
<DateTimePickerComponent
  placeholder="Click to select date and time"
/>

<DateTimePickerComponent
  placeholder="MM/DD/YYYY HH:MM AM/PM"
/>
```

### Masked Input Placeholder

When `enableMask` is true, customize placeholder labels:

```tsx
<DateTimePickerComponent
  enableMask={true}
  maskPlaceholder={{
    day: 'dd',
    month: 'mm',
    year: 'yyyy',
    hour: 'hh',
    minute: 'mm',
    second: 'ss',
    dayOfTheWeek: 'ddd'
  }}
/>
```

### Custom Masked Placeholder

```tsx
<DateTimePickerComponent
  enableMask={true}
  maskPlaceholder={{
    day: '##',
    month: '##',
    year: '####',
    hour: '##',
    minute: '##',
    second: '##',
  }}
/>
```

## Width and Sizing

Control the component width.

### Fixed Width

```tsx
// Pixel width
<DateTimePickerComponent width="300px" />

// Percentage
<DateTimePickerComponent width="100%" />

// Number (pixels)
<DateTimePickerComponent width={300} />
```

### Responsive Width

```tsx
import React, { useState, useEffect } from 'react';
import { DateTimePickerComponent } from '@syncfusion/ej2-react-calendars';

export default function ResponsiveWidth() {
  const [width, setWidth] = useState('100%');

  useEffect(() => {
    const handleResize = () => {
      const newWidth = window.innerWidth < 768 ? '100%' : '300px';
      setWidth(newWidth);
    };

    window.addEventListener('resize', handleResize);
    handleResize();

    return () => window.removeEventListener('resize', handleResize);
  }, []);

  return <DateTimePickerComponent width={width} />;
}
```

### Inline Styling

```tsx
<DateTimePickerComponent
  style={{
    width: '100%',
    maxWidth: '350px',
    marginBottom: '10px'
  }}
/>
```

## Responsive Design

Make DateTimePicker responsive across devices.

### Mobile-Friendly Setup

```tsx
import React from 'react';
import { DateTimePickerComponent } from '@syncfusion/ej2-react-calendars';

export default function ResponsiveDateTime() {
  const isMobile = window.innerWidth < 768;

  return (
    <DateTimePickerComponent
      width={isMobile ? '100%' : '300px'}
      fullScreenMode={isMobile}
      placeholder="Select date and time"
    />
  );
}
```

### Media Query Styling

```css
.e-datetimepicker {
  width: 100%;
  max-width: 400px;
}

@media (max-width: 768px) {
  .e-datetimepicker {
    width: 100%;
    max-width: 100%;
  }

  .e-datetimepicker .e-input-group {
    font-size: 16px; /* Prevents mobile zoom on focus */
  }
}

@media (max-width: 480px) {
  .e-datetimepicker {
    font-size: 14px;
  }

  .e-datetimepicker .e-input-group-icon {
    padding: 8px;
  }
}
```

## Full-Screen Mode

Enable full-screen popup on mobile devices.

```tsx
<DateTimePickerComponent
  fullScreenMode={true}
  placeholder="Full-screen on mobile"
/>
```

### Conditional Full-Screen

```tsx
const isMobile = window.innerWidth < 768;

<DateTimePickerComponent
  fullScreenMode={isMobile}
  placeholder="Full-screen on mobile only"
/>
```

## RTL Support

Enable right-to-left layout for Arabic, Hebrew, and other RTL languages.

### Enable RTL

```tsx
<DateTimePickerComponent
  enableRtl={true}
  locale="ar"
  placeholder="اختر التاريخ والوقت"
/>
```

### With RTL CSS

```css
.e-rtl.e-datetimepicker {
  direction: rtl;
}

.e-rtl .e-input-group-icon {
  left: auto;
  right: 0;
}
```

### RTL with Locale

```tsx
import React from 'react';
import { DateTimePickerComponent } from '@syncfusion/ej2-react-calendars';

export default function RTLDateTimePicker() {
  const [locale, setLocale] = React.useState('en');

  return (
    <div>
      <select value={locale} onChange={(e) => setLocale(e.target.value)}>
        <option value="en">English (LTR)</option>
        <option value="ar">العربية (RTL)</option>
        <option value="he">עברית (RTL)</option>
      </select>

      <DateTimePickerComponent
        locale={locale}
        enableRtl={locale !== 'en'}
        format="dd/MM/yyyy hh:mm a"
      />
    </div>
  );
}
```

## Advanced CSS Customization

Deep customization using CSS variables and selectors.

### CSS Custom Properties (Variables)

```css
:root {
  --dt-picker-border-color: #2196F3;
  --dt-picker-bg-color: #ffffff;
  --dt-picker-text-color: #333;
  --dt-picker-hover-bg: #f0f0f0;
}

.e-datetimepicker .e-input-group {
  border-color: var(--dt-picker-border-color);
  background-color: var(--dt-picker-bg-color);
  color: var(--dt-picker-text-color);
}

.e-datetimepicker .e-day-cell:hover {
  background-color: var(--dt-picker-hover-bg);
}
```

### Custom Day Cell Appearance

```css
.e-datetimepicker .e-calendar .e-day-cell {
  border-radius: 4px;
  transition: all 0.2s ease;
}

.e-datetimepicker .e-calendar .e-day-cell.e-selected {
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
  font-weight: bold;
}

.e-datetimepicker .e-calendar .e-day-cell:hover:not(.e-selected) {
  background-color: #f0f0f0;
  transform: scale(1.05);
}
```

### Popup Overlay Customization

```css
.e-datetimepicker .e-datetimepicker-popup {
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.15);
  border-radius: 8px;
}

.e-datetimepicker .e-datetimepicker-popup.e-popup-open {
  animation: slideDown 0.3s ease-out;
}

@keyframes slideDown {
  from {
    opacity: 0;
    transform: translateY(-10px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}
```

### Complete Themed Example

```tsx
import React from 'react';
import { DateTimePickerComponent } from '@syncfusion/ej2-react-calendars';
import './CustomTheme.css';

export default function CustomThemedPicker() {
  return (
    <div className="theme-container">
      <DateTimePickerComponent
        cssClass="custom-purple-theme"
        floatLabelType="Auto"
        placeholder="Select date and time"
        format="dd/MM/yyyy hh:mm a"
      />
    </div>
  );
}
```

```css
/* CustomTheme.css */
.theme-container {
  padding: 20px;
  background: linear-gradient(135deg, #f5f7fa 0%, #c3cfe2 100%);
  min-height: 100vh;
}

.custom-purple-theme {
  background: white;
  border-radius: 8px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
}

.custom-purple-theme .e-input-group {
  border: 2px solid #667eea;
  border-radius: 8px;
  transition: border-color 0.3s;
}

.custom-purple-theme .e-input-group:focus-within {
  border-color: #764ba2;
  box-shadow: 0 0 0 3px rgba(102, 126, 234, 0.1);
}

.custom-purple-theme .e-input-group-icon {
  color: #667eea;
  background: transparent;
}

.custom-purple-theme .e-calendar .e-day-cell.e-selected {
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
}
```

## Summary

- **Themes**: Choose one pre-built theme (material3, bootstrap5, fluent, tailwind, fabric).
- **cssClass**: Add custom styling via CSS classes.
- **floatLabelType**: Control placeholder label behavior.
- **width**: Set fixed or responsive widths.
- **fullScreenMode**: Enable full-screen on mobile.
- **enableRtl**: Support RTL languages.
- **Advanced CSS**: Use custom properties and animations for deep customization.

