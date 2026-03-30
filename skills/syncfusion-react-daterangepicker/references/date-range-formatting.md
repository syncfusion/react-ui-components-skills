# Date Range Formatting

## Table of Contents
- [Overview](#overview)
- [Format String Options](#format-string-options)
- [Locale-Based Formatting](#locale-based-formatting)
- [Custom Display Format](#custom-display-format)
- [Separator Configuration](#separator-configuration)
- [Float Label Types](#float-label-types)
- [Placeholder Management](#placeholder-management)
- [Common Patterns](#common-patterns)

## Overview

Date formatting controls how dates are displayed in the DateRangePicker input and calendar. The component supports:
- Standard format strings (M/d/yyyy, dd-MMM-yyyy, etc.)
- Locale-specific formatting
- Custom separators between start and end dates
- Float label positioning
- HTML attributes for customization

## Format String Options

### Common Format Patterns

| Pattern | Example | Description |
|---------|---------|-------------|
| `M/d/yyyy` | 3/15/2024 | Month/Day/Year (US) |
| `d/M/yyyy` | 15/3/2024 | Day/Month/Year (EU) |
| `dd-MMM-yyyy` | 15-Mar-2024 | Day-Month-Year with abbreviated month |
| `dd-MMMM-yyyy` | 15-March-2024 | Day-Month-Year with full month |
| `yyyy-MM-dd` | 2024-03-15 | ISO format (sortable) |
| `MM/dd/yy` | 03/15/24 | Short year format |
| `MMM d, yyyy` | Mar 15, 2024 | Abbreviated month with comma |
| `MMMM d, yyyy` | March 15, 2024 | Full month with comma |
| `E, MMM d, yyyy` | Fri, Mar 15, 2024 | Weekday with date |

### Basic Format Implementation

```tsx
import { DateRangePickerComponent } from '@syncfusion/ej2-react-calendars';
import * as React from 'react';

function FormatExamples() {
  const [format, setFormat] = React.useState<string>('M/d/yyyy');

  const formats = {
    'US Format': 'M/d/yyyy',
    'EU Format': 'd/M/yyyy',
    'ISO Format': 'yyyy-MM-dd',
    'Long Format': 'MMMM d, yyyy',
    'Short Format': 'MMM d, yy',
    'With Weekday': 'E, MMM d, yyyy',
  };

  return (
    <div style={{ padding: '20px' }}>
      <h3>Date Format Examples</h3>

      <div style={{ marginBottom: '15px' }}>
        {Object.entries(formats).map(([label, fmt]) => (
          <label key={label} style={{ display: 'block', marginBottom: '8px' }}>
            <input
              type="radio"
              name="format"
              value={fmt}
              checked={format === fmt}
              onChange={(e) => setFormat(e.target.value)}
            />
            {' '}{label}: {fmt}
          </label>
        ))}
      </div>

      <DateRangePickerComponent
        id="daterangepicker"
        placeholder="Select date range"
        format={format}
      />

      <p style={{ marginTop: '10px', fontSize: '12px', color: '#666' }}>
        Format: <code>{format}</code>
      </p>
    </div>
  );
}

export default FormatExamples;
```

## Locale-Based Formatting

### Format Based on Language/Region

```tsx
import { DateRangePickerComponent } from '@syncfusion/ej2-react-calendars';
import * as React from 'react';

function LocaleFormatting() {
  const [locale, setLocale] = React.useState<string>('en-US');

  const locales = {
    'English (US)': 'en-US',
    'English (GB)': 'en-GB',
    'German': 'de-DE',
    'French': 'fr-FR',
    'Spanish': 'es-ES',
    'Japanese': 'ja-JP',
    'Chinese': 'zh-CN',
    'Arabic': 'ar-SA',
  };

  const getFormatForLocale = (loc: string): string => {
    switch (loc) {
      case 'en-US':
        return 'M/d/yyyy';
      case 'en-GB':
      case 'de-DE':
      case 'fr-FR':
      case 'es-ES':
        return 'd/M/yyyy';
      case 'ja-JP':
      case 'zh-CN':
        return 'yyyy/M/d';
      case 'ar-SA':
        return 'd/M/yyyy';
      default:
        return 'M/d/yyyy';
    }
  };

  return (
    <div style={{ padding: '20px' }}>
      <h3>Locale-Based Date Formatting</h3>

      <div style={{ marginBottom: '15px' }}>
        <label style={{ display: 'block', marginBottom: '5px' }}>
          <strong>Select Locale:</strong>
        </label>
        <select
          value={locale}
          onChange={(e) => setLocale(e.target.value)}
          style={{ padding: '8px', fontSize: '14px' }}
        >
          {Object.entries(locales).map(([label, value]) => (
            <option key={value} value={value}>
              {label}
            </option>
          ))}
        </select>
      </div>

      <DateRangePickerComponent
        id="daterangepicker"
        placeholder="Select date range"
        locale={locale}
        format={getFormatForLocale(locale)}
      />

      <p style={{ marginTop: '15px', fontSize: '12px', color: '#666' }}>
        Locale: <code>{locale}</code> | Format: <code>{getFormatForLocale(locale)}</code>
      </p>
    </div>
  );
}

export default LocaleFormatting;
```

## Custom Display Format

### Format with Internationalization API

```tsx
import { DateRangePickerComponent, ChangedEventArgs } from '@syncfusion/ej2-react-calendars';
import * as React from 'react';

function CustomDateDisplay() {
  const [range, setRange] = React.useState<{
    start: Date | null;
    end: Date | null;
  }>({ start: null, end: null });

  const [displayFormat, setDisplayFormat] = React.useState<'short' | 'long' | 'relative'>('short');

  const handleDateChange = (args: ChangedEventArgs) => {
    setRange({ start: args.startDate, end: args.endDate });
  };

  const formatDateRange = () => {
    if (!range.start || !range.end) return '';

    switch (displayFormat) {
      case 'short':
        return `${range.start.toLocaleDateString()} - ${range.end.toLocaleDateString()}`;
      case 'long':
        return `${range.start.toLocaleDateString('en-US', {
          weekday: 'long',
          year: 'numeric',
          month: 'long',
          day: 'numeric',
        })} - ${range.end.toLocaleDateString('en-US', {
          weekday: 'long',
          year: 'numeric',
          month: 'long',
          day: 'numeric',
        })}`;
      case 'relative': {
        const today = new Date();
        today.setHours(0, 0, 0, 0);
        const startDaysFromToday = Math.ceil(
          (range.start.getTime() - today.getTime()) / (1000 * 60 * 60 * 24)
        );
        const endDaysFromToday = Math.ceil(
          (range.end.getTime() - today.getTime()) / (1000 * 60 * 60 * 24)
        );
        return `${startDaysFromToday} days from now - ${endDaysFromToday} days from now`;
      }
      default:
        return '';
    }
  };

  return (
    <div style={{ padding: '20px', maxWidth: '600px' }}>
      <h3>Custom Date Display Format</h3>

      <DateRangePickerComponent
        id="daterangepicker"
        placeholder="Select date range"
        format="M/d/yyyy"
        change={handleDateChange}
      />

      <div style={{ marginTop: '20px' }}>
        <label style={{ display: 'block', marginBottom: '10px' }}>
          <strong>Display Format:</strong>
        </label>
        {['short', 'long', 'relative'].map((fmt) => (
          <label key={fmt} style={{ display: 'block', marginBottom: '8px' }}>
            <input
              type="radio"
              name="displayFormat"
              value={fmt}
              checked={displayFormat === fmt as any}
              onChange={(e) => setDisplayFormat(e.target.value as any)}
            />
            {' '}{fmt.charAt(0).toUpperCase() + fmt.slice(1)}
          </label>
        ))}
      </div>

      {range.start && range.end && (
        <div style={{
          marginTop: '20px',
          padding: '15px',
          backgroundColor: '#f5f5f5',
          borderRadius: '4px',
        }}>
          <h4 style={{ margin: '0 0 10px 0' }}>Formatted Output:</h4>
          <p style={{ margin: 0 }}>{formatDateRange()}</p>
        </div>
      )}
    </div>
  );
}

export default CustomDateDisplay;
```

## Separator Configuration

### Custom Separator Between Dates

```tsx
import { DateRangePickerComponent } from '@syncfusion/ej2-react-calendars';
import * as React from 'react';

function SeparatorCustomization() {
  const [separator, setSeparator] = React.useState<string>(' - ');

  const separators = {
    'Dash': ' - ',
    'To': ' to ',
    'Arrow': ' → ',
    'Pipe': ' | ',
    'Comma': ', ',
    'Slash': ' / ',
  };

  return (
    <div style={{ padding: '20px' }}>
      <h3>Date Range Separator Options</h3>

      <div style={{ marginBottom: '15px' }}>
        <label style={{ display: 'block', marginBottom: '10px' }}>
          <strong>Select Separator:</strong>
        </label>
        {Object.entries(separators).map(([label, sep]) => (
          <label key={label} style={{ display: 'block', marginBottom: '8px' }}>
            <input
              type="radio"
              name="separator"
              value={sep}
              checked={separator === sep}
              onChange={(e) => setSeparator(e.target.value)}
            />
            {' '}{label} (<code>{sep}</code>)
          </label>
        ))}
      </div>

      <DateRangePickerComponent
        id="daterangepicker"
        placeholder="Select date range"
        separator={separator}
      />

      <p style={{ marginTop: '15px', fontSize: '12px', color: '#666' }}>
        Example: 3/15/2024{separator}3/25/2024
      </p>
    </div>
  );
}

export default SeparatorCustomization;
```

## Float Label Types

### Label Positioning

The `floatLabelType` property controls placeholder label behavior:

```tsx
import { DateRangePickerComponent } from '@syncfusion/ej2-react-calendars';
import * as React from 'react';

function FloatLabelDemo() {
  const [labelType, setLabelType] = React.useState<'Never' | 'Always' | 'Auto'>('Auto');

  const labelTypes: Array<'Never' | 'Always' | 'Auto'> = ['Never', 'Always', 'Auto'];

  return (
    <div style={{ padding: '20px' }}>
      <h3>Float Label Types</h3>

      <div style={{ marginBottom: '30px' }}>
        <label style={{ display: 'block', marginBottom: '10px' }}>
          <strong>Label Type:</strong>
        </label>
        {labelTypes.map((type) => (
          <label key={type} style={{ display: 'block', marginBottom: '8px' }}>
            <input
              type="radio"
              name="floatLabelType"
              value={type}
              checked={labelType === type}
              onChange={(e) => setLabelType(e.target.value as any)}
            />
            {' '}{type}
          </label>
        ))}
      </div>

      <div style={{
        padding: '20px',
        backgroundColor: '#f9f9f9',
        borderRadius: '4px',
        minHeight: '150px',
      }}>
        <DateRangePickerComponent
          id="daterangepicker"
          placeholder="Select date range"
          floatLabelType={labelType}
        />
      </div>

      <div style={{ marginTop: '20px', fontSize: '12px', color: '#666' }}>
        <p><strong>Never:</strong> Label stays as placeholder</p>
        <p><strong>Always:</strong> Label floats above input</p>
        <p><strong>Auto:</strong> Label floats on focus/value</p>
      </div>
    </div>
  );
}

export default FloatLabelDemo;
```

## Placeholder Management

### Custom Placeholder Configuration

```tsx
import { DateRangePickerComponent } from '@syncfusion/ej2-react-calendars';
import * as React from 'react';

function PlaceholderCustomization() {
  return (
    <div style={{ padding: '20px', maxWidth: '600px' }}>
      <h3>Placeholder Configuration</h3>

      <div style={{ marginBottom: '20px' }}>
        <h4>Default Placeholder</h4>
        <DateRangePickerComponent
          id="daterangepicker1"
          placeholder="Select a date range"
        />
      </div>

      <div style={{ marginBottom: '20px' }}>
        <h4>Custom Placeholder</h4>
        <DateRangePickerComponent
          id="daterangepicker2"
          placeholder="From date - To date"
        />
      </div>

      <div style={{ marginBottom: '20px' }}>
        <h4>Semantic Placeholder</h4>
        <DateRangePickerComponent
          id="daterangepicker3"
          placeholder="Check-in - Check-out"
        />
      </div>

      <div style={{ marginBottom: '20px' }}>
        <h4>With Float Label</h4>
        <DateRangePickerComponent
          id="daterangepicker4"
          placeholder="Select date range"
          floatLabelType="Always"
        />
      </div>
    </div>
  );
}

export default PlaceholderCustomization;
```

## HTML Attributes

### Custom HTML Attributes

```tsx
import { DateRangePickerComponent } from '@syncfusion/ej2-react-calendars';
import * as React from 'react';

function HTMLAttributesExample() {
  const htmlAttributes = {
    title: 'Select a date range for your booking',
    'data-testid': 'hotel-date-range-picker',
    'aria-label': 'Hotel booking date range',
  };

  return (
    <div style={{ padding: '20px' }}>
      <h3>Custom HTML Attributes</h3>

      <DateRangePickerComponent
        id="daterangepicker"
        placeholder="Select date range"
        htmlAttributes={htmlAttributes}
      />

      <div style={{ marginTop: '20px', padding: '10px', backgroundColor: '#f0f0f0' }}>
        <h4>Applied Attributes:</h4>
        <pre style={{ margin: 0, fontSize: '12px' }}>
          {JSON.stringify(htmlAttributes, null, 2)}
        </pre>
      </div>
    </div>
  );
}

export default HTMLAttributesExample;
```

## Common Patterns

### Pattern: Multi-Locale Picker

```tsx
import { DateRangePickerComponent, ChangedEventArgs } from '@syncfusion/ej2-react-calendars';
import * as React from 'react';

function MultiLocaleExample() {
  const [selectedLocale, setSelectedLocale] = React.useState<string>('en-US');
  const [dateRange, setDateRange] = React.useState<{
    start: Date | null;
    end: Date | null;
  }>({ start: null, end: null });

  interface LocaleConfig {
    format: string;
    locale: string;
    direction?: 'ltr' | 'rtl';
  }

  const localeConfigs: Record<string, LocaleConfig> = {
    'en-US': { locale: 'en-US', format: 'M/d/yyyy' },
    'de-DE': { locale: 'de-DE', format: 'd.M.yyyy' },
    'fr-FR': { locale: 'fr-FR', format: 'd/M/yyyy' },
    'ja-JP': { locale: 'ja-JP', format: 'yyyy/M/d' },
    'ar-SA': { locale: 'ar-SA', format: 'd/M/yyyy', direction: 'rtl' },
  };

  const config = localeConfigs[selectedLocale];

  return (
    <div style={{ padding: '20px', maxWidth: '600px', direction: config.direction }}>
      <h3>Multi-Locale Date Range Picker</h3>

      <div style={{ marginBottom: '20px' }}>
        <label style={{ display: 'block', marginBottom: '8px' }}>
          <strong>Select Language:</strong>
        </label>
        <select
          value={selectedLocale}
          onChange={(e) => setSelectedLocale(e.target.value)}
          style={{ padding: '8px', fontSize: '14px' }}
        >
          {Object.keys(localeConfigs).map((locale) => (
            <option key={locale} value={locale}>
              {locale}
            </option>
          ))}
        </select>
      </div>

      <DateRangePickerComponent
        id="daterangepicker"
        placeholder="Select date range"
        locale={config.locale}
        format={config.format}
        enableRtl={config.direction === 'rtl'}
        change={(e: ChangedEventArgs) =>
          setDateRange({ start: e.startDate, end: e.endDate })
        }
      />

      {dateRange.start && dateRange.end && (
        <div style={{
          marginTop: '20px',
          padding: '15px',
          backgroundColor: '#f5f5f5',
          borderRadius: '4px',
        }}>
          <p>
            <strong>Selected:</strong> {dateRange.start.toLocaleDateString(config.locale)} - {dateRange.end.toLocaleDateString(config.locale)}
          </p>
        </div>
      )}
    </div>
  );
}

export default MultiLocaleExample;
```

### Pattern: Format Switcher

```tsx
import { DateRangePickerComponent } from '@syncfusion/ej2-react-calendars';
import * as React from 'react';

function FormatSwitcher() {
  const [format, setFormat] = React.useState<string>('M/d/yyyy');
  const [locale, setLocale] = React.useState<string>('en-US');

  const formatOptions: Record<string, Record<string, string>> = {
    'en-US': {
      'Short': 'M/d/yy',
      'Medium': 'MMM d, yyyy',
      'Long': 'MMMM d, yyyy',
      'ISO': 'yyyy-MM-dd',
    },
    'de-DE': {
      'Short': 'd.M.yy',
      'Medium': 'd. MMM yyyy',
      'Long': 'd. MMMM yyyy',
      'ISO': 'yyyy-MM-dd',
    },
    'fr-FR': {
      'Short': 'd/M/yy',
      'Medium': 'd MMM yyyy',
      'Long': 'd MMMM yyyy',
      'ISO': 'yyyy-MM-dd',
    },
  };

  return (
    <div style={{ padding: '20px', maxWidth: '600px' }}>
      <h3>Format & Locale Switcher</h3>

      <div style={{ marginBottom: '15px', display: 'grid', gridTemplateColumns: '1fr 1fr', gap: '15px' }}>
        <div>
          <label style={{ display: 'block', marginBottom: '5px' }}>
            <strong>Locale:</strong>
          </label>
          <select
            value={locale}
            onChange={(e) => {
              setLocale(e.target.value);
              setFormat(formatOptions[e.target.value]['Medium']);
            }}
            style={{ width: '100%', padding: '8px' }}
          >
            {Object.keys(formatOptions).map((loc) => (
              <option key={loc} value={loc}>
                {loc}
              </option>
            ))}
          </select>
        </div>

        <div>
          <label style={{ display: 'block', marginBottom: '5px' }}>
            <strong>Format:</strong>
          </label>
          <select
            value={format}
            onChange={(e) => setFormat(e.target.value)}
            style={{ width: '100%', padding: '8px' }}
          >
            {Object.entries(formatOptions[locale]).map(([name, fmt]) => (
              <option key={name} value={fmt}>
                {name}
              </option>
            ))}
          </select>
        </div>
      </div>

      <DateRangePickerComponent
        id="daterangepicker"
        placeholder="Select date range"
        locale={locale}
        format={format}
      />

      <p style={{ marginTop: '15px', fontSize: '12px', color: '#666' }}>
        Locale: <code>{locale}</code> | Format: <code>{format}</code>
      </p>
    </div>
  );
}

export default FormatSwitcher;
```
