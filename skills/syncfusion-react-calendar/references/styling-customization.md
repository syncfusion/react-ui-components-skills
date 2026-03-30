# Styling and Customization

## Table of Contents
- [Theme Selection](#theme-selection)
- [CSS Class Customization](#css-class-customization)
- [Custom Day Cell Rendering](#custom-day-cell-rendering)
- [RTL Support](#rtl-support)
- [CSS Variables and Overrides](#css-variables-and-overrides)
- [Responsive Design](#responsive-design)

---

## Theme Selection

Syncfusion provides multiple built-in themes. Choose one and import it once (preferably in your main `index.js` or global styles).

### Available Themes

- `material3.css` — Material Design 3 (modern, default)
- `bootstrap5.css` — Bootstrap 5 styling
- `fluent.css` — Microsoft Fluent Design
- `tailwind.css` — Tailwind CSS theme
- `fabric.css` — Office Fabric theme

### Import in index.js

```jsx
// index.js
import React from 'react';
import { createRoot } from 'react-dom/client';
import App from './App';

// Import theme once
import '@syncfusion/ej2-base/styles/material3.css';
import '@syncfusion/ej2-calendars/styles/material3.css';

createRoot(document.getElementById('root')).render(<App />);
```

### Switch Themes Dynamically

To change themes at runtime, dynamically add/remove `<link>` tags:

```jsx
import React, { useState } from 'react';
import { CalendarComponent } from '@syncfusion/ej2-react-calendars';

export default function ThemeSwitcher() {
  const [theme, setTheme] = useState('material3');

  const switchTheme = (newTheme) => {
    // Remove old theme link
    const oldLink = document.querySelector(`link[data-theme="${theme}"]`);
    if (oldLink) oldLink.remove();

    // Add new theme link
    const link = document.createElement('link');
    link.rel = 'stylesheet';
    link.href = `/node_modules/@syncfusion/ej2-calendars/styles/${newTheme}.css`;
    link.dataset.theme = newTheme;
    document.head.appendChild(link);

    setTheme(newTheme);
  };

  return (
    <div style={{ padding: 20 }}>
      <div>
        <button onClick={() => switchTheme('material3')}>Material3</button>
        <button onClick={() => switchTheme('bootstrap5')}>Bootstrap5</button>
        <button onClick={() => switchTheme('fluent')}>Fluent</button>
      </div>
      <CalendarComponent />
    </div>
  );
}
```

---

## CSS Class Customization

### Using the `cssClass` prop

Add custom CSS classes to the calendar root element:

```jsx
import React, { useState } from 'react';
import { CalendarComponent } from '@syncfusion/ej2-react-calendars';
import './CustomCalendar.css';

export default function CustomCSSExample() {
  const [value, setValue] = useState(new Date());

  return (
    <div style={{ padding: 20 }}>
      <CalendarComponent
        value={value}
        change={(e) => setValue(e.value)}
        cssClass="my-custom-calendar"
      />
    </div>
  );
}
```

**CustomCalendar.css:**

```css
.my-custom-calendar {
  border: 2px solid #007bff;
  border-radius: 8px;
  padding: 10px;
}

.my-custom-calendar .e-title {
  background-color: #007bff;
  color: white;
  padding: 10px;
  border-radius: 4px;
}

.my-custom-calendar .e-cell {
  font-size: 14px;
  font-weight: 500;
}

.my-custom-calendar .e-selected {
  background-color: #28a745;
  color: white;
}
```

---

## Custom Day Cell Rendering

Use the `renderDayCell` callback to customize the appearance and behavior of individual day cells.

### Example: Highlight Weekends

```jsx
import React, { useState } from 'react';
import { CalendarComponent } from '@syncfusion/ej2-react-calendars';

export default function HighlightWeekendsExample() {
  const [value, setValue] = useState(new Date());

  const renderDayCell = (args) => {
    const dayOfWeek = args.date.getDay();
    if (dayOfWeek === 0 || dayOfWeek === 6) {
      args.cellElement.style.backgroundColor = '#f0f0f0';
      args.cellElement.style.color = '#999';
    }
  };

  return (
    <div style={{ padding: 20 }}>
      <CalendarComponent
        value={value}
        change={(e) => setValue(e.value)}
        renderDayCell={renderDayCell}
      />
    </div>
  );
}
```

### Example: Add Badge for Special Dates

```jsx
import React, { useState } from 'react';
import { CalendarComponent } from '@syncfusion/ej2-react-calendars';

export default function BadgeExample() {
  const [value, setValue] = useState(new Date());
  const eventDates = [
    new Date(2026, 2, 15), // March 15
    new Date(2026, 2, 25), // March 25
  ];

  const renderDayCell = (args) => {
    const dateStr = args.date.toDateString();
    const isEvent = eventDates.some((d) => d.toDateString() === dateStr);

    if (isEvent) {
      // Create a badge overlay
      const badge = document.createElement('span');
      badge.textContent = '●';
      badge.style.color = 'red';
      badge.style.fontSize = '16px';
      badge.style.position = 'absolute';
      badge.style.top = '2px';
      badge.style.right = '2px';
      args.cellElement.style.position = 'relative';
      args.cellElement.appendChild(badge);
    }
  };

  return (
    <div style={{ padding: 20 }}>
      <CalendarComponent
        value={value}
        change={(e) => setValue(e.value)}
        renderDayCell={renderDayCell}
      />
    </div>
  );
}
```

### Example: Disable Past Dates

```jsx
const renderDayCell = (args) => {
  const today = new Date();
  today.setHours(0, 0, 0, 0);

  if (args.date < today) {
    args.isDisabled = true;
    args.cellElement.style.opacity = '0.5';
  }
};

<CalendarComponent renderDayCell={renderDayCell} />
```

---

## RTL Support

Enable right-to-left text direction for Arabic, Hebrew, and other RTL languages.

```jsx
import React, { useState } from 'react';
import { CalendarComponent } from '@syncfusion/ej2-react-calendars';

export default function RTLExample() {
  const [value, setValue] = useState(new Date());

  return (
    <div style={{ padding: 20, direction: 'rtl' }}>
      <CalendarComponent
        value={value}
        change={(e) => setValue(e.value)}
        enableRtl={true}
        locale="ar" // Arabic locale
      />
    </div>
  );
}
```

**Key points:**
- Set `enableRtl={true}` on the component.
- Set `direction: 'rtl'` on the parent container in CSS or inline.
- Use appropriate locale code (e.g., `'ar'` for Arabic, `'he'` for Hebrew).

---

## CSS Variables and Overrides

Syncfusion components often expose CSS variables for theming. You can override them in your CSS:

```css
:root {
  --e-primary-color: #007bff;
  --e-body-font: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  --e-border-radius: 4px;
}

/* Or specifically for calendar */
.e-calendar {
  --e-selected-bg-color: #28a745;
  --e-hover-bg-color: #e9ecef;
}
```

Then apply those variables in your global styles or component CSS files. This allows you to theme multiple components consistently.

---

## Responsive Design

The Calendar adapts to different screen sizes automatically, but you can enhance responsiveness:

```jsx
import React, { useState } from 'react';
import { CalendarComponent } from '@syncfusion/ej2-react-calendars';

export default function ResponsiveExample() {
  const [value, setValue] = useState(new Date());

  const getCalendarStyles = () => {
    const width = window.innerWidth;
    if (width < 600) {
      return { width: '100%', maxWidth: '300px' };
    } else if (width < 1200) {
      return { width: '400px' };
    }
    return { width: '500px' };
  };

  return (
    <div style={{ padding: 20, ...getCalendarStyles() }}>
      <CalendarComponent
        value={value}
        change={(e) => setValue(e.value)}
      />
    </div>
  );
}
```

### Mobile-Friendly Tips

1. **Touch targets:** Ensure day cells are large enough to tap on mobile (minimum 44x44 px).
2. **Font size:** Increase font size on small screens for readability.
3. **Container width:** Constrain the calendar width so it doesn't overflow on phones.

```css
@media (max-width: 600px) {
  .e-calendar {
    font-size: 16px;
  }

  .e-calendar .e-cell {
    height: 50px;
    line-height: 50px;
  }
}

@media (min-width: 601px) {
  .e-calendar {
    font-size: 14px;
  }
}
```

---

## Best Practices

1. **Import CSS once:** Add theme imports to your main `index.js`, not in every component.
2. **Use `cssClass` for scoped changes:** Keeps customizations organized and reusable.
3. **Avoid inline styles in `renderDayCell`:** Instead, add CSS classes and style them externally.
4. **Test RTL:** If supporting RTL, verify layout on mobile and desktop.
5. **Use CSS variables:** For consistent theming across multiple components and screens.

---

## Common Issues

- **Styles not loading:** Ensure the CSS import path is correct relative to `node_modules`.
- **RTL not working:** Remember to set `enableRtl={true}` and `direction: 'rtl'` on the parent container.
- **Custom CSS not applying:** Check CSS specificity; you may need `!important` for overrides, though avoid if possible.
