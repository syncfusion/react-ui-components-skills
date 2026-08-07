# Styling and Appearance in React Data Grid

The Syncfusion React Data Grid provides flexible styling and appearance customization options to align with your application's design requirements. You can choose from a wide range of built-in themes, customize visual elements using CSS, and create consistent experiences through theme-based styling.

## Table of Contents

- [Built-in Themes](#built-in-themes)
- [Theme Packages](#theme-packages)
- [Size Modes](#size-modes)
- [Theme Customization](#theme-customization)
- [Default CSS Override](#default-css-override)
- [Common Patterns](#common-patterns)
- [Quick Start](#quick-start)

---

## Quick Start

### Install a Theme Package

```bash
npm install @syncfusion/ej2-tailwidn3-theme
```

### Import Theme in Your App

```css
@import "../node_modules/@syncfusion/ej2-tailwind3-theme/styles/grid/index.css";
```

```jsx
import { GridComponent, ColumnsDirective, ColumnDirective, Inject, Page } from '@syncfusion/ej2-react-grids';

export default function App() {
  const data = [
    { OrderID: 10248, CustomerName: 'VINET', TotalAmount: 32.38 },
    { OrderID: 10249, CustomerName: 'TOMSP', TotalAmount: 11.61 }
  ];

  return (
    <GridComponent dataSource={data} allowPaging={true}>
      <ColumnsDirective>
        <ColumnDirective field='OrderID' headerText='Order ID' width='100' />
        <ColumnDirective field='CustomerName' headerText='Customer Name' width='150' />
        <ColumnDirective field='TotalAmount' headerText='Total Amount' width='120' />
      </ColumnsDirective>
      <Inject services={[Page]} />
    </GridComponent>
  );
}
```

### Enable Touch Mode (Larger UI Elements)

```jsx
// Add to body in index.html or apply CSS class dynamically
<body className="e-bigger">
  <div id="root"></div>
</body>
```

---

## Built-in Themes

The DataGrid components include a comprehensive collection of themes to match popular design systems and provide both light and dark variants.

### Available Themes

| Theme | Style Sheet | Use Case |
|-------|------------|----------|
| **Tailwind 3.4** | `tailwind3.css` / `tailwind3.scss` | Modern utility-first design system |
| **Tailwind 3.4 Dark** | `tailwind3-dark.css` | Dark mode variant of Tailwind 3.4 |
| **Bootstrap 5.3** | `bootstrap5.3.css` / `bootstrap5.3.scss` | Latest Bootstrap framework integration |
| **Bootstrap 5.3 Dark** | `bootstrap5.3-dark.css` | Dark mode for Bootstrap 5.3 |
| **Fluent 2** | `fluent2.css` / `fluent2.scss` | Microsoft Fluent Design System 2 |
| **Fluent 2 Dark** | `fluent2-dark.css` | Dark mode variant of Fluent 2 |
| **Material 3** | `material3.css` / `material3.scss` | Latest Google Material Design 3 |
| **Material 3 Dark** | `material3-dark.css` | Dark mode for Material Design 3 |
| **Bootstrap 5** | `bootstrap5.css` / `bootstrap5.scss` | Standard Bootstrap 5 |
| **Bootstrap 5 Dark** | `bootstrap5-dark.css` | Dark variant of Bootstrap 5 |
| **Fluent** | `fluent.css` / `fluent.scss` | Microsoft Fluent Design |
| **Fluent Dark** | `fluent-dark.css` | Dark variant of Fluent |
| **Material** | `material.css` / `material.scss` | Google Material Design |
| **Material Dark** | `material-dark.css` | Dark variant of Material Design |
| **Tailwind** | `tailwind.css` / `tailwind.scss` | Standard Tailwind CSS |
| **Tailwind Dark** | `tailwind-dark.css` | Dark variant of Tailwind |
| **Office Fabric** | `fabric.css` / `fabric.scss` | Microsoft Office Fabric design |
| **Office Fabric Dark** | `fabric-dark.css` | Dark variant of Office Fabric |
| **High Contrast** | `highcontrast.css` | Accessible high contrast theme |

### Choosing a Theme

- **Material 3** — Best for modern, clean designs with vibrant colors
- **Fluent 2** — Ideal for enterprise applications following Microsoft design
- **Bootstrap 5.3** — Great for Bootstrap ecosystem integration
- **Tailwind 3.4** — Recommended for utility-first workflows
- **High Contrast** — Required for accessibility compliance (WCAG AA/AAA)

---

## Theme Packages

Syncfusion® built-in themes are distributed as npm packages. Install the theme package that matches your design requirements.

### Installation Examples

**Material 3 Theme (Light + Dark variants)**

```bash
# Light theme
npm install @syncfusion/ej2-material3-theme

# Dark theme
npm install @syncfusion/ej2-material3-dark-theme
```

**Bootstrap 5.3 Theme**

```bash
npm install @syncfusion/ej2-bootstrap5.3-theme
npm install @syncfusion/ej2-bootstrap5.3-dark-theme
```

**Tailwind 3 Theme**

```bash
npm install @syncfusion/ej2-tailwind3-theme
npm install @syncfusion/ej2-tailwind3-dark-theme
```

### All Available Theme Packages

| Theme | Light Package | Dark Package |
|-------|---|---|
| Tailwind 3 | `@syncfusion/ej2-tailwind3-theme` | `@syncfusion/ej2-tailwind3-dark-theme` |
| Bootstrap 5.3 | `@syncfusion/ej2-bootstrap5.3-theme` | `@syncfusion/ej2-bootstrap5.3-dark-theme` |
| Fluent 2 | `@syncfusion/ej2-fluent2-theme` | `@syncfusion/ej2-fluent2-dark-theme` |
| Material 3 | `@syncfusion/ej2-material3-theme` | `@syncfusion/ej2-material3-dark-theme` |
| Bootstrap 5 | `@syncfusion/ej2-bootstrap5-theme` | `@syncfusion/ej2-bootstrap5-dark-theme` |
| Bootstrap 4 | `@syncfusion/ej2-bootstrap4-theme` | — |
| Bootstrap | `@syncfusion/ej2-bootstrap-theme` | `@syncfusion/ej2-bootstrap-dark-theme` |
| Material | `@syncfusion/ej2-material-theme` | `@syncfusion/ej2-material-dark-theme` |
| Tailwind | `@syncfusion/ej2-tailwind-theme` | `@syncfusion/ej2-tailwind-dark-theme` |
| Fluent | `@syncfusion/ej2-fluent-theme` | `@syncfusion/ej2-fluent-dark-theme` |
| Fabric | `@syncfusion/ej2-fabric-theme` | `@syncfusion/ej2-fabric-dark-theme` |
| High Contrast | `@syncfusion/ej2-highcontrast-theme` | — |

---

## Size Modes

The DataGrid components support two size modes to optimize user experience across different devices and input methods.

### Normal Mode (Default)

Standard sizing optimized for mouse and keyboard interactions. This is the default mode for desktop applications.

```jsx
// No additional configuration needed - normal mode is default
<GridComponent dataSource={data}>
  {/* Your grid configuration */}
</GridComponent>
```

### Touch Mode (Bigger)

Larger elements with increased padding, font sizes, and touch targets for improved touch interaction and accessibility. Use this for mobile applications or touch-enabled interfaces.

```jsx
// Option 1: Apply globally via CSS class on body
// In index.html or App.tsx/jsx:
<body className="e-bigger">
  <div id="root"></div>
</body>

// Option 2: Apply dynamically with JavaScript
document.body.classList.add('e-bigger');

// Option 3: Apply to specific container
<div className="e-bigger">
  <GridComponent dataSource={data}>
    {/* Grid will use larger size mode */}
  </GridComponent>
</div>
```

### Size Mode Comparison

| Aspect | Normal Mode | Touch Mode |
|--------|-------------|-----------|
| **Target Device** | Desktop, mouse, keyboard | Mobile, tablet, touch input |
| **Element Size** | Standard | Larger (increased padding/height) |
| **Font Size** | Standard | Increased for better readability |
| **Touch Targets** | Moderate | Larger for easier interaction |
| **Row Height** | ~30-40px | ~48-56px |
| **Column Headers** | Standard height | Increased height |
| **Padding** | Compact | Spacious |

---

## Theme Customization

The DataGrid provides flexible theme customization options to help align the control appearance with your application's design requirements. You can customize built-in themes either by overriding CSS variables or by creating customized themes.

### Default CSS Override (CSS Variables)

The DataGrid themes use CSS variables with the unified `--sf` naming convention. This ensures visual consistency, simple customization, and global updates. Centralized variables allow quick adjustments to colors, backgrounds, and borders across the grid.

#### Material 3 Theme Color Variables

The Material 3 theme applies scalable CSS custom properties to maintain consistency. Here are commonly used color-related variables:

| Variable | Purpose | Example Value |
|----------|---------|----------------|
| `--e-font-name` | Default font family for DataGrid | `Segoe UI, sans-serif` |
| `--color-sf-content-bg-color-alt1` | Background of DataGrid header | `#f3f3f3` |
| `--color-sf-primary` | Primary brand color | `#6200ee` |
| `--color-sf-surface` | Surface/card background color | `#ffffff` |
| `--color-sf-on-surface` | Text color on surfaces | `#1c1b1f` |
| `--color-sf-error` | Error/alert color | `#b3261e` |
| `--color-sf-warning` | Warning color | `#f57c00` |
| `--color-sf-success` | Success color | `#388e3c` |

#### Customize CSS Variables

Create a custom stylesheet to override theme variables:

```css
@import "../node_modules/@syncfusion/ej2-tailwind3-theme/styles/grid/index.css";
/* custom-theme.css */

/* Override Material 3 theme variables */
:root {
  /* Header customization */
  --color-sf-content-bg-color-alt1: #e8eaf6;
  
  /* Primary brand color */
  --color-sf-primary: #5e35b1;
  
  /* Row hover effect */
  --color-sf-surface-hover: #f3e5f5;
  
  /* Border color */
  --color-sf-outline: #ede7f6;
  
  /* Text color */
  --color-sf-on-surface: #311b92;
  
  /* Selection color */
  --color-sf-secondary-container: #e1bee7;
}
```

```jsx
import './custom-theme.css';

export default function App() {
  return (
    <GridComponent dataSource={data}>
      {/* Grid with customized colors */}
    </GridComponent>
  );
}
```

#### Common Customizations

**Change Header Background Color**

```css
:root {
  --color-sf-content-bg-color-alt1: #2c3e50;
  --color-sf-on-content-text: #ffffff;
}
```

**Customize Row Hover Effect**

```css
:root {
  --color-sf-surface-hover: #eceff1;
}
```

**Modify Grid Lines Color**

```css
:root {
  --color-sf-outline: #b0bec5;
}
```

**Change Selected Row Appearance**

```css
:root {
  --color-sf-secondary-container: #c5e1a5;
  --color-sf-on-secondary-container: #33691e;
}
```

---

## Common Patterns

### Pattern 1: Switching Themes Dynamically

Allow users to switch between light and dark themes at runtime:

```css
@import "../node_modules/@syncfusion/ej2-tailwind3-theme/styles/grid/index.css";
```

```jsx
import { useState } from 'react';
import { GridComponent, ColumnsDirective, ColumnDirective, Inject, Page } from '@syncfusion/ej2-react-grids';

export default function App() {
  const [isDarkMode, setIsDarkMode] = useState(false);

  const handleThemeChange = () => {
    setIsDarkMode(!isDarkMode);
    if (!isDarkMode) {
      document.body.classList.add('e-dark');
      // Remove light theme and add dark theme
      import('../node_modules/@syncfusion/ej2-tailwind3-dark-theme/styles/grid/index.css');
    } else {
      document.body.classList.remove('e-dark');
      import('../node_modules/@syncfusion/ej2-tailwind3-theme/styles/grid/index.css');
    }
  };

  return (
    <>
      <button onClick={handleThemeChange}>
        Toggle {isDarkMode ? 'Light' : 'Dark'} Mode
      </button>
      <GridComponent dataSource={data} allowPaging={true}>
        <ColumnsDirective>
          <ColumnDirective field='OrderID' headerText='Order ID' width='100' />
          <ColumnDirective field='CustomerName' headerText='Customer Name' width='150' />
        </ColumnsDirective>
        <Inject services={[Page]} />
      </GridComponent>
    </>
  );
}
```

### Pattern 2: Responsive Design with Multiple Themes

Apply different themes based on screen size:

```css
@import "../node_modules/@syncfusion/ej2-bootstrap5.3-theme/styles/grid/index.css";
```

```jsx
import { useEffect, useState } from 'react';
import { GridComponent } from '@syncfusion/ej2-react-grids';

export default function App() {
  const [theme, setTheme] = useState('bootstrap');

  useEffect(() => {
    const handleResize = () => {
      if (window.innerWidth < 768) {
        // Mobile: Use touch mode and simpler theme
        document.body.classList.add('e-bigger');
        setTheme('touch-optimized');
      } else {
        // Desktop: Use normal mode
        document.body.classList.remove('e-bigger');
        setTheme('desktop');
      }
    };

    window.addEventListener('resize', handleResize);
    handleResize(); // Call once on mount
    return () => window.removeEventListener('resize', handleResize);
  }, []);

  return <GridComponent dataSource={data}>{/* Grid content */}</GridComponent>;
}
```

### Pattern 3: Custom Brand Colors

Override theme colors to match brand identity:

```css
@import "../node_modules/@syncfusion/ej2-material3-theme/styles/grid/index.css";
/* brand-theme.css */
:root {
  /* Company brand color (instead of Material purple) */
  --color-sf-primary: #1976d2; /* Blue brand */
  --color-sf-secondary: #dc004e; /* Pink accent */
  
  /* Header styling */
  --color-sf-content-bg-color-alt1: #f5f5f5;
  
  /* Selection and hover states */
  --color-sf-surface-hover: #e3f2fd;
  --color-sf-secondary-container: #f3e5f5;
  
  /* Success/Error states */
  --color-sf-success: #4caf50;
  --color-sf-error: #f44336;
}
```

```jsx
import './brand-theme.css';
```

### Pattern 4: Accessibility with High Contrast Theme

Implement high contrast theme for accessibility compliance:

```jsx
import { useState } from 'react';
import '@syncfusion/ej2-material3-theme/styles/material3.css';

export default function App() {
  const [highContrast, setHighContrast] = useState(false);

  useEffect(() => {
    if (highContrast) {
      document.body.classList.add('e-highcontrast');
      import('@syncfusion/ej2-highcontrast-theme/styles/highcontrast.css');
    } else {
      document.body.classList.remove('e-highcontrast');
    }
  }, [highContrast]);

  return (
    <>
      <label>
        <input 
          type="checkbox" 
          checked={highContrast}
          onChange={(e) => setHighContrast(e.target.checked)}
        />
        Enable High Contrast Mode (Accessibility)
      </label>
      <GridComponent dataSource={data}>
        {/* Grid content */}
      </GridComponent>
    </>
  );
}
```

---

## When to Use Each Approach

| Scenario | Recommendation | Method |
|----------|---|---|
| **Static theme** | Use one theme throughout app | Import CSS in main app file |
| **User preference** | Let users choose theme | Use CSS class switching + state |
| **Brand matching** | Override default colors | CSS variable customization |
| **Accessibility** | Support high contrast | Provide high contrast theme toggle |
| **Mobile-first** | Optimize for touch | Apply `e-bigger` class on mobile |
| **Team design system** | Consistent styling | Create custom theme package |

---

## Edge Cases & Troubleshooting

### Issue: Custom CSS variables not overriding theme

**Problem:** CSS variable changes ignored by theme

**Solution:** Ensure custom styles load AFTER theme CSS and use `!important` if needed

```jsx
// Order matters!
import '@syncfusion/ej2-material3-theme/styles/material3.css';
import './custom-overrides.css'; // Load custom styles AFTER theme

// In custom-overrides.css:
:root {
  --color-sf-primary: #1976d2 !important; /* Use !important if necessary */
}
```

### Issue: Touch mode not activating

**Problem:** `e-bigger` class applied but UI not enlarging

**Solution:** 
1. Verify class is applied to `<body>` or parent container
2. Check theme includes touch mode styles
3. Ensure theme CSS is loaded correctly

```jsx
// ✅ Correct: Add to body in index.html or main layout
<body className="e-bigger">
  <div id="root"></div>
</body>

// Or dynamically:
useEffect(() => {
  if (isTouchDevice) {
    document.body.classList.add('e-bigger');
  }
}, []);
```

---

## Best Practices

1. **Choose theme once at startup** — Switching themes frequently impacts performance
2. **Keep CSS variable overrides minimal** — Override only what you need to customize
3. **Test accessibility** — Verify color contrast meets WCAG AA standards
4. **Mobile consideration** — Use `e-bigger` class for better touch UX
5. **Cache management** — Clear cache when switching themes during development
6. **Use dark theme package** — Don't manually create dark themes; use official packages