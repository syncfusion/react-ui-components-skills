# Globalization & Localization

## Table of Contents
- [What is Globalization](#what-is-globalization)
- [Setting Locale](#setting-locale)
- [CLDR Data](#cldr-data)
- [Common Cultures](#common-cultures)
- [Right-to-Left Support](#right-to-left-support)
- [Locale Customization](#locale-customization)
- [Complete Examples](#complete-examples)

## What is Globalization

Globalization in DatePicker includes:
- **Internationalization (i18n):** Parsing and formatting dates according to regional standards
- **Localization (l10n):** Translating text and applying culture-specific formats
- **RTL Support:** Right-to-Left languages (Arabic, Hebrew, Urdu)
- **Week Configuration:** Different cultures start weeks on different days

## Setting Locale

The `locale` property sets the culture for date formatting and display.

### Default Behavior

```jsx
<DatePickerComponent
  value={new Date()}
  placeholder="Select date"
/>
```

**Default:** Uses "en-US" (American English)
- Format: M/d/yyyy (3/19/2026)
- Week starts: Sunday
- Month names: English

### Setting a Locale

```jsx
<DatePickerComponent
  locale="de"  // German
  value={new Date()}
  placeholder="Datum eingeben"
/>
```

**German (de):**
- Format: d.M.yyyy (19.3.2026)
- Week starts: Monday
- Month names: Deutsch (Januar, Februar, etc.)

### Locale Code Format

- **Language only:** "de", "fr", "es" (uses default region)
- **Language-Region:** "en-GB", "en-US", "zh-CN" (specific region)
- **Full:** "zh-Hans-CN" (script + region)

## CLDR Data

CLDR (Common Locale Data Repository) contains locale-specific date/number formats.

### Install CLDR Data

```bash
npm install cldr-data --save
```

### Import CLDR for Specific Cultures

```jsx
import { loadCldr } from '@syncfusion/ej2-base';

// German locale
import * as gregorian from 'cldr-data/main/de/ca-gregorian.json';
import * as numbers from 'cldr-data/main/de/numbers.json';
import * as timeZoneNames from 'cldr-data/main/de/timeZoneNames.json';
import * as numberingSystems from 'cldr-data/supplemental/numberingSystems.json';
import * as weekData from 'cldr-data/supplemental/weekData.json';

loadCldr(numberingSystems, gregorian, numbers, timeZoneNames, weekData);

// Now use German locale
<DatePickerComponent locale="de" />
```

### Why weekData.json?

The `weekData` defines which day starts the week for each culture:
- en-US: Sunday (0)
- de, fr, es: Monday (1)
- ar: Saturday (6)

### Load Multiple Cultures

```jsx
import { loadCldr } from '@syncfusion/ej2-base';

// English
import * as enGregorian from 'cldr-data/main/en/ca-gregorian.json';
import * as enNumbers from 'cldr-data/main/en/numbers.json';

// German
import * as deGregorian from 'cldr-data/main/de/ca-gregorian.json';
import * as deNumbers from 'cldr-data/main/de/numbers.json';

// French
import * as frGregorian from 'cldr-data/main/fr/ca-gregorian.json';
import * as frNumbers from 'cldr-data/main/fr/numbers.json';

import * as numberingSystems from 'cldr-data/supplemental/numberingSystems.json';
import * as weekData from 'cldr-data/supplemental/weekData.json';

loadCldr(
  numberingSystems,
  enGregorian, enNumbers,
  deGregorian, deNumbers,
  frGregorian, frNumbers,
  weekData
);
```

## Common Cultures

### English Variants

| Locale | Format | Week Start | Region |
|--------|--------|-----------|--------|
| en-US | M/d/yyyy | Sunday | United States |
| en-GB | dd/MM/yyyy | Monday | United Kingdom |
| en-AU | d/MM/yyyy | Monday | Australia |
| en-IN | dd-MM-yyyy | Sunday | India |

### European Cultures

| Locale | Format | Week Start | Country |
|--------|--------|-----------|---------|
| de | dd.MM.yyyy | Monday | Germany |
| fr | dd/MM/yyyy | Monday | France |
| es | d/M/yyyy | Monday | Spain |
| it | dd/MM/yyyy | Monday | Italy |
| pt | dd/MM/yyyy | Monday | Portugal |

### Asian Cultures

| Locale | Format | Week Start | Country |
|--------|--------|-----------|---------|
| zh-CN | yyyy/M/d | Monday | China (Simplified) |
| ja | yyyy/M/d | Sunday | Japan |
| ko | yyyy.M.d | Sunday | Korea |
| th | d/M/yyyy | Sunday | Thailand |

### Middle Eastern (RTL)

| Locale | Format | Week Start | Direction |
|--------|--------|-----------|-----------|
| ar | d/M/yyyy | Saturday | RTL |
| he | d.M.yyyy | Sunday | RTL |
| ur | d/M/yyyy | Sunday | RTL |

## Right-to-Left Support

### Enable RTL

RTL is automatic when using RTL locale:

```jsx
<DatePickerComponent
  locale="ar"  // Arabic - automatic RTL
  value={new Date()}
  placeholder="اختر تاريخا"
/>
```

**Result:**
- Calendar flows right-to-left
- Navigation arrows reverse
- Text aligns right
- Week starts Saturday (Arab convention)

### Manual RTL Enablement

```jsx
import React from 'react';
import { DatePickerComponent } from '@syncfusion/ej2-react-calendars';
import { enableRtl } from '@syncfusion/ej2-base';

// Enable RTL globally
enableRtl(true);

export default function App() {
  return (
    <DatePickerComponent
      locale="ar"
      value={new Date()}
      placeholder="اختر تاريخا"
    />
  );
}
```

### RTL on Specific Container

```jsx
<div style={{ direction: 'rtl' }}>
  <DatePickerComponent
    locale="ar"
    value={new Date()}
    placeholder="اختر تاريخا"
  />
</div>
```

## Locale Customization

### Custom Locale Text

Customize specific text like "Today" button:

```jsx
import { L10n } from '@syncfusion/ej2-base';

// Define German translations
L10n.load({
  'de': {
    'datepicker': {
      'today': 'Heute',
      'placeholder': 'Datum eingeben'
    }
  }
});

export default function App() {
  return (
    <DatePickerComponent
      locale="de"
      value={new Date()}
      placeholder="Datum eingeben"
    />
  );
}
```

### Define Multiple Custom Locales

```jsx
import { L10n } from '@syncfusion/ej2-base';

L10n.load({
  'de': {
    'datepicker': {
      'today': 'Heute',
      'placeholder': 'Datum eingeben'
    }
  },
  'fr': {
    'datepicker': {
      'today': 'Aujourd\'hui',
      'placeholder': 'Entrez la date'
    }
  },
  'es': {
    'datepicker': {
      'today': 'Hoy',
      'placeholder': 'Ingrese la fecha'
    }
  }
});
```

## Week Start Configuration

Different cultures start the week on different days.

### Automatic by Culture

Syncfusion automatically sets week start based on culture:

```jsx
<DatePickerComponent locale="en-US" />   // Week starts Sunday
<DatePickerComponent locale="de" />      // Week starts Monday
<DatePickerComponent locale="ar" />      // Week starts Saturday
```

**Result:** Calendar header shows appropriate first day

### Manual Override with `firstDayOfWeek`

Use the `firstDayOfWeek` property to manually override the first day of the week regardless of culture:

```jsx
// 0=Sunday, 1=Monday, 2=Tuesday, ..., 6=Saturday
<DatePickerComponent
  locale="en-US"
  firstDayOfWeek={1}   // Force Monday as first day
  value={new Date()}
/>
```

```jsx
import { loadCldr } from '@syncfusion/ej2-base';
import * as customWeekData from 'cldr-data/supplemental/weekData.json';

loadCldr(customWeekData);

<DatePickerComponent locale="de" />  // Uses week data from CLDR for German culture
```

## Complete Examples

### Example 1: Multi-Language Application

```jsx
import React, { useState } from 'react';
import { DatePickerComponent } from '@syncfusion/ej2-react-calendars';
import { loadCldr } from '@syncfusion/ej2-base';

// Import locales
import * as enGregorian from 'cldr-data/main/en/ca-gregorian.json';
import * as deGregorian from 'cldr-data/main/de/ca-gregorian.json';
import * as frGregorian from 'cldr-data/main/fr/ca-gregorian.json';
import * as numberingSystems from 'cldr-data/supplemental/numberingSystems.json';
import * as weekData from 'cldr-data/supplemental/weekData.json';

loadCldr(
  numberingSystems,
  enGregorian, deGregorian, frGregorian,
  weekData
);

export default function App() {
  const [locale, setLocale] = useState('en');

  return (
    <div style={{ padding: '20px' }}>
      <h3>Multi-Language DatePicker</h3>
      
      <div>
        <label>Language: </label>
        <select value={locale} onChange={(e) => setLocale(e.target.value)}>
          <option value="en">English</option>
          <option value="de">Deutsch</option>
          <option value="fr">Français</option>
        </select>
      </div>

      <DatePickerComponent
        locale={locale}
        value={new Date()}
        placeholder={locale === 'en' ? 'Select date' : 
                     locale === 'de' ? 'Datum eingeben' : 
                     'Sélectionner la date'}
      />
    </div>
  );
}
```

### Example 2: Arabic (RTL) Support

```jsx
import React from 'react';
import { DatePickerComponent } from '@syncfusion/ej2-react-calendars';
import { loadCldr } from '@syncfusion/ej2-base';
import { L10n } from '@syncfusion/ej2-base';

import * as arGregorian from 'cldr-data/main/ar/ca-gregorian.json';
import * as arNumbers from 'cldr-data/main/ar/numbers.json';
import * as numberingSystems from 'cldr-data/supplemental/numberingSystems.json';
import * as weekData from 'cldr-data/supplemental/weekData.json';

loadCldr(numberingSystems, arGregorian, arNumbers, weekData);

L10n.load({
  'ar': {
    'datepicker': {
      'today': 'اليوم',
      'placeholder': 'اختر التاريخ'
    }
  }
});

export default function App() {
  return (
    <div style={{ direction: 'rtl', padding: '20px' }}>
      <h3>تاريخ الاختيار</h3>
      <DatePickerComponent
        locale="ar"
        value={new Date()}
        placeholder="اختر التاريخ"
      />
    </div>
  );
}
```

### Example 3: US and UK Date Formats

```jsx
import React, { useState } from 'react';
import { DatePickerComponent } from '@syncfusion/ej2-react-calendars';

export default function App() {
  const [country, setCountry] = useState('us');

  const dateFormats = {
    us: { locale: 'en-US', format: 'M/d/yyyy', placeholder: 'MM/DD/YYYY' },
    uk: { locale: 'en-GB', format: 'dd/MM/yyyy', placeholder: 'DD/MM/YYYY' }
  };

  const config = dateFormats[country];

  return (
    <div style={{ padding: '20px' }}>
      <h3>Regional Date Formats</h3>
      
      <div>
        <label>Region: </label>
        <select value={country} onChange={(e) => setCountry(e.target.value)}>
          <option value="us">United States</option>
          <option value="uk">United Kingdom</option>
        </select>
      </div>

      <DatePickerComponent
        locale={config.locale}
        value={new Date()}
        format={config.format}
        placeholder={config.placeholder}
      />

      <p style={{ marginTop: '10px' }}>
        Format Example: {new Date().toLocaleDateString(config.locale)}
      </p>
    </div>
  );
}
```

### Example 4: Asian Date Formats

```jsx
import React, { useState } from 'react';
import { DatePickerComponent } from '@syncfusion/ej2-react-calendars';
import { loadCldr } from '@syncfusion/ej2-base';

import * as zhGregorian from 'cldr-data/main/zh/ca-gregorian.json';
import * as jaGregorian from 'cldr-data/main/ja/ca-gregorian.json';
import * as koGregorian from 'cldr-data/main/ko/ca-gregorian.json';
import * as numberingSystems from 'cldr-data/supplemental/numberingSystems.json';
import * as weekData from 'cldr-data/supplemental/weekData.json';

loadCldr(numberingSystems, zhGregorian, jaGregorian, koGregorian, weekData);

export default function App() {
  const [locale, setLocale] = useState('zh-CN');

  return (
    <div style={{ padding: '20px' }}>
      <h3>Asian Date Formats</h3>
      
      <div>
        <label>Language: </label>
        <select value={locale} onChange={(e) => setLocale(e.target.value)}>
          <option value="zh-CN">Chinese (Simplified)</option>
          <option value="ja">日本語 (Japanese)</option>
          <option value="ko">한국어 (Korean)</option>
        </select>
      </div>

      <DatePickerComponent
        locale={locale}
        value={new Date()}
        placeholder={`Select date (${locale})`}
      />
    </div>
  );
}
```

## Tips & Best Practices

1. **Load Only Needed Locales:** Don't load all 200+ cultures if you only need a few
2. **Include weekData:** Always load `weekData.json` for proper week start configuration
3. **Test RTL Thoroughly:** RTL involves not just direction but cultural conventions
4. **Cache CLDR Data:** Load CLDR data once at app startup, not per component
5. **User Preference:** Store locale in localStorage or user profile for persistence
6. **Fallback Locale:** Always have en-US as fallback for missing translations

