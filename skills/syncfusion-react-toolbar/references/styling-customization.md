# Styling & Customization

## Table of Contents
- [CSS Class Structure](#css-class-structure)
- [Toolbar Container Styling](#toolbar-container-styling)
- [Item & Button Styling](#item--button-styling)
- [Icon Styling](#icon-styling)
- [State Styling](#state-styling)
- [Theme Integration](#theme-integration)
- [Popup Customization CSS Classes](#popup-customization-css-classes)
- [Complete Examples](#complete-examples)

---

## CSS Class Structure

Syncfusion Toolbar uses a predictable CSS class structure for styling.

### Main Classes

```css
.e-toolbar              /* Toolbar container */
.e-toolbar-item         /* Individual toolbar item wrapper */
.e-tbar-btn             /* Toolbar button element */
.e-icons                /* Icon element */
.e-separator            /* Separator element */
.e-toolbar-pop          /* Popup dropdown */
.e-overflow-button      /* Overflow dropdown toggle button */
```

### Complete Structure Example

```html
<div class="e-toolbar">
  <div class="e-toolbar-item">
    <button class="e-btn e-tbar-btn">
      <span class="e-icons e-cut-icon"></span>
      Cut
    </button>
  </div>
  <div class="e-toolbar-item">
    <div class="e-separator"></div>
  </div>
</div>
```

---

## Toolbar Container Styling

### Basic Toolbar Styling

```css
.e-toolbar {
  background-color: #f0f0f0;
  border: 2px solid #333;
  border-radius: 4px;
  padding: 8px;
}
```

### Custom Background

```css
.e-toolbar {
  background: linear-gradient(to right, #667eea 0%, #764ba2 100%);
  color: white;
}

.e-toolbar .e-tbar-btn {
  color: white;
}
```

### Bordered Toolbar

```css
.e-toolbar {
  border: 1px solid #ddd;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}
```

### Compact Toolbar

```css
.e-toolbar {
  padding: 4px;
}

.e-toolbar .e-toolbar-item {
  margin: 2px;
}
```

### Full-Width Toolbar

```css
.e-toolbar {
  width: 100%;
  box-sizing: border-box;
}
```

---

## Item & Button Styling

### Item Container

```css
.e-toolbar .e-toolbar-item {
  background: #ffffff;
  border: 1px solid #ddd;
  margin: 0 4px;
  padding: 4px;
}
```

### Button Styling

```css
.e-toolbar .e-tbar-btn {
  background-color: #f5f5f5;
  border: 1px solid #999;
  border-radius: 2px;
  padding: 6px 12px;
  font-size: 14px;
  cursor: pointer;
}
```

### Button with Icons

```css
.e-toolbar .e-tbar-btn {
  display: flex;
  align-items: center;
  gap: 6px;
}

.e-toolbar .e-tbar-btn .e-icons {
  font-size: 18px;
}
```

### Custom Button Colors

```css
.e-toolbar .e-tbar-btn {
  background-color: #007bff;
  color: white;
  border-color: #0056b3;
}
```

### Button Sizing

```css
/* Large buttons */
.e-toolbar .e-tbar-btn {
  padding: 10px 16px;
  font-size: 16px;
}

/* Small buttons */
.e-toolbar .e-tbar-btn {
  padding: 4px 8px;
  font-size: 12px;
}
```

---

## Icon Styling

### Icon Properties

```css
.e-toolbar .e-icons {
  color: #333;
  font-size: 16px;
  font-weight: bold;
}
```

### Icon Colors

```css
/* Blue icons */
.e-toolbar .e-icons {
  color: #0066ff;
}

/* White icons on dark background */
.e-toolbar .e-icons {
  color: #ffffff;
}
```

### Icon Size Variations

```css
/* Small icons */
.e-toolbar .e-icons {
  font-size: 14px;
}

/* Large icons */
.e-toolbar .e-icons {
  font-size: 20px;
}

/* Extra large icons */
.e-toolbar .e-icons {
  font-size: 24px;
}
```

### Icon Spacing

```css
.e-toolbar .e-tbar-btn {
  gap: 8px; /* Space between icon and text */
}
```

---

## State Styling

### Hover State

```css
.e-toolbar .e-tbar-btn:hover {
  background-color: #e8e8e8;
  border-color: #666;
}

.e-toolbar .e-tbar-btn:hover .e-icons {
  color: #0066ff;
}
```

### Focus State

```css
.e-toolbar .e-tbar-btn:focus {
  outline: 2px solid #0066ff;
  outline-offset: 2px;
}

.e-toolbar .e-tbar-btn:focus-visible {
  box-shadow: 0 0 0 3px rgba(0, 102, 255, 0.25);
}
```

### Active/Selected State

```css
.e-toolbar .e-tbar-btn.e-active {
  background-color: #0066ff;
  color: white;
  border-color: #0056b3;
}

.e-toolbar .e-tbar-btn.e-active .e-icons {
  color: white;
}
```

### Disabled State

```css
.e-toolbar .e-tbar-btn:disabled,
.e-toolbar .e-tbar-btn[aria-disabled="true"] {
  opacity: 0.5;
  cursor: not-allowed;
  background-color: #f5f5f5;
}

.e-toolbar .e-tbar-btn:disabled .e-icons {
  color: #999;
}
```

---

## Separator Styling

### Basic Separator

```css
.e-toolbar .e-separator {
  border-left: 1px solid #ccc;
  height: 24px;
  margin: 0 8px;
}
```

### Custom Separator

```css
.e-toolbar .e-separator {
  border-left: 2px solid #0066ff;
  height: 28px;
  margin: 0 12px;
  border-radius: 1px;
}
```

### Colored Separator

```css
.e-toolbar .e-separator {
  border-left-color: #ff6600;
}
```

---

## Theme Integration

### Tailwind Theme Classes

The Toolbar automatically applies Tailwind classes when the theme is imported:

```css
@import '../node_modules/@syncfusion/ej2-react-navigations/styles/tailwind3.css';
```

No additional configuration needed.

### Bootstrap Theme Classes

```css
@import '../node_modules/@syncfusion/ej2-react-navigations/styles/bootstrap5.3.css';
```

Toolbar integrates with Bootstrap styling.

### Theme Override

Override theme colors with custom CSS:

```css
/* After theme import */
.e-toolbar {
  background-color: var(--custom-toolbar-bg);
}

.e-toolbar .e-tbar-btn {
  background-color: var(--custom-btn-bg);
}
```

### CSS Variables

Use CSS variables for consistent theming:

```css
:root {
  --toolbar-bg: #f5f5f5;
  --toolbar-border: #ddd;
  --btn-bg: #ffffff;
  --btn-hover: #e8e8e8;
  --icon-color: #333;
}

.e-toolbar {
  background-color: var(--toolbar-bg);
  border-color: var(--toolbar-border);
}

.e-toolbar .e-tbar-btn {
  background-color: var(--btn-bg);
}

.e-toolbar .e-tbar-btn:hover {
  background-color: var(--btn-hover);
}

.e-toolbar .e-icons {
  color: var(--icon-color);
}
```

---

## Popup Customization CSS Classes

When using Popup overflow mode, customize popup appearance with these CSS classes:

### Popup Container

```css
.e-toolbar-pop {
  /* Main popup dropdown container */
  background-color: #ffffff;
  border: 1px solid #d0d0d0;
  border-radius: 4px;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.15);
  padding: 4px 0;
}
```

### Overflow Button

```css
.e-overflow-button {
  /* The dropdown toggle button at end of toolbar */
  background-color: #f5f5f5;
  border: 1px solid #ccc;
  padding: 6px 10px;
}

.e-overflow-button:hover {
  background-color: #e8e8e8;
}
```

### Item Display Control

```css
.e-overflow-show {
  /* Items with overflow="Show" */
  display: flex !important;
  background-color: #ffffff;
}

.e-overflow-hide {
  /* Items with overflow="Hide" */
  display: flex !important;
  background-color: #f9f9f9;
}
```

### Text Display Modes

```css
.e-popup-text {
  /* Text visible in popup only */
  font-weight: 500;
  color: #1976d2;
}

.e-toolbar-text {
  /* Text visible in toolbar only */
  font-weight: normal;
  color: #333;
}
```

### Complete Popup Customization Example

```jsx
import { ItemDirective, ItemsDirective, ToolbarComponent } from '@syncfusion/ej2-react-navigations';
import './App.css';

const App = () => {
  return (
    <ToolbarComponent overflowMode="Popup" width="350px">
      <ItemsDirective>
        <ItemDirective 
          text="Cut" 
          prefixIcon="e-cut-icon" 
          overflow="Show" 
          showTextOn="Overflow"
        />
        <ItemDirective 
          text="Copy" 
          prefixIcon="e-copy-icon" 
          overflow="Show" 
          showTextOn="Overflow"
        />
        <ItemDirective 
          text="Advanced" 
          overflow="Hide" 
          showTextOn="Overflow"
        />
      </ItemsDirective>
    </ToolbarComponent>
  );
};

export default App;
```

**CSS:**
```css
/* Customize popup appearance */
.e-toolbar-pop {
  background: linear-gradient(to bottom, #ffffff 0%, #f5f5f5 100%);
  border: 2px solid #0066ff;
  box-shadow: 0 8px 24px rgba(0, 102, 255, 0.15);
}

/* Show priority items styling */
.e-overflow-show {
  background-color: #e3f2fd;
  border-left: 3px solid #0066ff;
}

/* Hide priority items styling */
.e-overflow-hide {
  background-color: #fafafa;
  opacity: 0.9;
}

/* Text styling */
.e-popup-text {
  font-size: 14px;
  font-weight: 600;
  color: #0066ff;
}
```

---

## Complete Examples

### Minimal Styled Toolbar

```jsx
import { ItemDirective, ItemsDirective, ToolbarComponent } from '@syncfusion/ej2-react-navigations';
import './App.css';

const App = () => {
  return (
    <ToolbarComponent className="custom-toolbar">
      <ItemsDirective>
        <ItemDirective text="Cut" prefixIcon="e-cut-icon" />
        <ItemDirective text="Copy" prefixIcon="e-copy-icon" />
        <ItemDirective text="Paste" prefixIcon="e-paste-icon" />
      </ItemsDirective>
    </ToolbarComponent>
  );
};

export default App;
```

**CSS:**
```css
.custom-toolbar {
  background-color: #f9f9f9;
  border: 1px solid #e0e0e0;
  border-radius: 6px;
  padding: 8px;
}

.custom-toolbar .e-tbar-btn {
  background-color: #ffffff;
  border: 1px solid #d0d0d0;
  border-radius: 3px;
  padding: 6px 12px;
}

.custom-toolbar .e-tbar-btn:hover {
  background-color: #f5f5f5;
  border-color: #999;
}
```

### Dark Mode Toolbar

```jsx
import { ItemDirective, ItemsDirective, ToolbarComponent } from '@syncfusion/ej2-react-navigations';
import './App.css';

const App = () => {
  return (
    <ToolbarComponent className="dark-toolbar">
      <ItemsDirective>
        <ItemDirective text="New" prefixIcon="e-new-icon" />
        <ItemDirective text="Open" prefixIcon="e-open-icon" />
        <ItemDirective text="Save" prefixIcon="e-save-icon" />
        <ItemDirective type="Separator" />
        <ItemDirective text="Print" prefixIcon="e-print-icon" />
      </ItemsDirective>
    </ToolbarComponent>
  );
};

export default App;
```

**CSS:**
```css
.dark-toolbar {
  background-color: #2d2d2d;
  border: 1px solid #1a1a1a;
  color: #ffffff;
}

.dark-toolbar .e-tbar-btn {
  background-color: #3d3d3d;
  color: #ffffff;
  border-color: #555;
}

.dark-toolbar .e-tbar-btn:hover {
  background-color: #4d4d4d;
  border-color: #888;
}

.dark-toolbar .e-icons {
  color: #ffffff;
}

.dark-toolbar .e-separator {
  border-left-color: #555;
}
```

### Colorful Gradient Toolbar

```jsx
const App = () => {
  return (
    <ToolbarComponent className="gradient-toolbar">
      <ItemsDirective>
        <ItemDirective text="Dashboard" prefixIcon="e-dashboard-icon" />
        <ItemDirective text="Reports" prefixIcon="e-chart-icon" />
        <ItemDirective text="Settings" prefixIcon="e-settings-icon" align="Right" />
      </ItemsDirective>
    </ToolbarComponent>
  );
};

export default App;
```

**CSS:**
```css
.gradient-toolbar {
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  padding: 12px;
  border-radius: 8px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
}

.gradient-toolbar .e-tbar-btn {
  background-color: rgba(255, 255, 255, 0.9);
  color: #333;
  border: none;
  border-radius: 4px;
  padding: 8px 16px;
  font-weight: 500;
}

.gradient-toolbar .e-tbar-btn:hover {
  background-color: rgba(255, 255, 255, 1);
  transform: translateY(-2px);
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);
}
```

### Material Design Toolbar

```jsx
const App = () => {
  return (
    <ToolbarComponent className="material-toolbar">
      <ItemsDirective>
        <ItemDirective text="Create" prefixIcon="e-add-icon" />
        <ItemDirective text="Edit" prefixIcon="e-edit-icon" />
        <ItemDirective text="Delete" prefixIcon="e-delete-icon" />
        <ItemDirective type="Separator" />
        <ItemDirective text="More" prefixIcon="e-more-icon" />
      </ItemsDirective>
    </ToolbarComponent>
  );
};

export default App;
```

**CSS:**
```css
.material-toolbar {
  background-color: #ffffff;
  border-bottom: 1px solid #e0e0e0;
  padding: 12px 16px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.material-toolbar .e-tbar-btn {
  background-color: transparent;
  color: #1976d2;
  border: none;
  padding: 8px 16px;
  border-radius: 2px;
  font-weight: 500;
  text-transform: uppercase;
  font-size: 13px;
}

.material-toolbar .e-tbar-btn:hover {
  background-color: rgba(25, 118, 210, 0.08);
}

.material-toolbar .e-tbar-btn:active {
  background-color: rgba(25, 118, 210, 0.12);
}
```

### Compact Mobile Toolbar

```jsx
const App = () => {
  return (
    <ToolbarComponent className="mobile-toolbar">
      <ItemsDirective>
        <ItemDirective prefixIcon="e-back-icon" />
        <ItemDirective text="Title" />
        <ItemDirective prefixIcon="e-search-icon" align="Right" />
        <ItemDirective prefixIcon="e-more-icon" align="Right" />
      </ItemsDirective>
    </ToolbarComponent>
  );
};

export default App;
```

**CSS:**
```css
.mobile-toolbar {
  background-color: #2196f3;
  color: white;
  padding: 8px;
  display: flex;
  justify-content: space-between;
}

.mobile-toolbar .e-tbar-btn {
  background-color: transparent;
  color: white;
  border: none;
  padding: 8px;
  font-size: 14px;
}

.mobile-toolbar .e-tbar-btn:active {
  background-color: rgba(255, 255, 255, 0.2);
  border-radius: 2px;
}

.mobile-toolbar .e-icons {
  color: white;
  font-size: 20px;
}
```

---

## Tips & Best Practices

### 1. Maintain Visual Hierarchy

```css
/* Primary action */
.e-toolbar .e-tbar-btn.primary {
  background-color: #0066ff;
  color: white;
}

/* Secondary action */
.e-toolbar .e-tbar-btn.secondary {
  background-color: #f5f5f5;
  color: #333;
}
```

### 2. Consistent Spacing

```css
.e-toolbar {
  padding: 12px;
  gap: 8px;
}
```

### 3. Accessible Colors

Ensure sufficient contrast:

```css
/* Good: High contrast */
.e-toolbar .e-tbar-btn {
  background: white;
  color: black;  /* 21:1 ratio */
}

/* Avoid: Low contrast */
.e-toolbar .e-tbar-btn {
  background: #eeeeee;
  color: #cccccc;  /* 1.18:1 - fails WCAG AA */
}
```

### 4. Clear Focus Indicator

```css
.e-toolbar .e-tbar-btn:focus-visible {
  outline: 2px solid #0066ff;
  outline-offset: 2px;
}
```

### 5. Responsive Sizing

```css
@media (max-width: 768px) {
  .e-toolbar .e-tbar-btn {
    padding: 8px 12px;
    font-size: 12px;
  }

  .e-toolbar .e-icons {
    font-size: 14px;
  }
}
```

---

## Performance Considerations

### Minimize Repaints

```css
/* Avoid: Animation on paint properties */
.e-toolbar .e-tbar-btn {
  background-color: #fff;
  transition: background-color 0.3s; /* OK */
}

/* Better: Use transform for animation */
.e-toolbar .e-tbar-btn:hover {
  transform: scale(1.05);
  transition: transform 0.2s;
}
```

### CSS Custom Properties

Use CSS variables to reduce recalculations:

```css
:root {
  --primary-color: #0066ff;
  --padding: 12px;
  --border-radius: 4px;
}

.e-toolbar {
  padding: var(--padding);
}

.e-toolbar .e-tbar-btn {
  background-color: var(--primary-color);
  border-radius: var(--border-radius);
}
```

This enables rapid theme changes without recalculating all values.
