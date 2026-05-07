# Styling and Appearance

## Table of Contents
- [CSS Class Structure](#css-class-structure)
- [Basic CSS Customization](#basic-css-customization)
- [Theme Studio Integration](#theme-studio-integration)
- [Custom CSS Overrides](#custom-css-overrides)
- [Icon Styling](#icon-styling)
- [Visual States](#visual-states)

## CSS Class Structure

The ContextMenu component uses a structured hierarchy of CSS classes for customization:

| CSS Class | Purpose |
|-----------|---------|
| `.e-contextmenu-wrapper` | Main container for the context menu |
| `.e-contextmenu-wrapper .e-menu-parent` | Parent menu items container |
| `.e-menu-item` | Individual menu item |
| `.e-menu-item.e-selected` | Selected menu item state |
| `.e-menu-item.e-disabled` | Disabled menu item state |
| `.e-menu-item .e-menu-icon` | Icon element in menu item |
| `.e-menu-item .e-caret` | Arrow icon for nested menus |
| `.e-ul` | Unordered list of menu items |

## Basic CSS Customization

Override default styles by targeting these classes in your CSS file:

```css
/* Customize the main wrapper */
.e-contextmenu-wrapper {
  border-radius: 8px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.15);
  background-color: #fff;
}

/* Customize menu items */
.e-contextmenu-wrapper .e-menu-item {
  padding: 10px 16px;
  font-size: 14px;
  color: #333;
}

/* Hover state */
.e-contextmenu-wrapper .e-menu-item:hover {
  background-color: #f0f0f0;
  color: #000;
}

/* Selected state */
.e-contextmenu-wrapper .e-menu-item.e-selected {
  background-color: #e3f2fd;
  color: #1976d2;
  font-weight: 500;
}

/* Disabled state */
.e-contextmenu-wrapper .e-menu-item.e-disabled {
  color: #ccc;
  cursor: not-allowed;
  opacity: 0.6;
}

/* Icons */
.e-contextmenu-wrapper .e-menu-item .e-menu-icon {
  margin-right: 8px;
  font-size: 16px;
}

/* Caret for nested menus */
.e-contextmenu-wrapper .e-menu-item .e-caret::before {
  margin-left: 8px;
}
```

## Theme Studio Integration

Syncfusion Theme Studio allows visual theme creation without code. To customize themes:

1. Visit [Syncfusion Theme Studio](https://ej2.syncfusion.com/themestudio/?theme=material)
2. Select your base theme (Material, Bootstrap, Fluent, Tailwind)
3. Customize colors, typography, and spacing
4. Download the custom CSS file
5. Import in your application

Example custom theme import:

```ts
// In App.css
@import './custom-theme.css';
@import '../node_modules/@syncfusion/ej2-navigations/styles/tailwind3.css';
```

## Custom CSS Overrides

### Example: Modern Dark Theme

```css
/* Dark theme styling */
.e-contextmenu-wrapper {
  background-color: #2d2d2d;
  border: 1px solid #444;
  border-radius: 8px;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.4);
}

.e-contextmenu-wrapper .e-menu-item {
  color: #e0e0e0;
  padding: 10px 14px;
}

.e-contextmenu-wrapper .e-menu-item:hover {
  background-color: #3d3d3d;
  color: #fff;
}

.e-contextmenu-wrapper .e-menu-item.e-selected {
  background-color: #1976d2;
  color: #fff;
}

.e-contextmenu-wrapper .e-menu-item.e-disabled {
  color: #666;
  opacity: 0.5;
}

/* Separator styling */
.e-contextmenu-wrapper .e-separator {
  background-color: #444;
  margin: 4px 0;
  height: 1px;
}
```

### Example: Compact Theme

```css
/* Compact spacing */
.e-contextmenu-wrapper {
  padding: 2px 0;
}

.e-contextmenu-wrapper .e-menu-item {
  padding: 6px 12px;
  font-size: 12px;
}

.e-contextmenu-wrapper .e-menu-icon {
  font-size: 14px;
  margin-right: 6px;
}

.e-contextmenu-wrapper .e-ul {
  min-width: 150px;
}
```

### Example: Gradient Background

```css
.e-contextmenu-wrapper {
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
}

.e-contextmenu-wrapper .e-menu-item {
  color: white;
}

.e-contextmenu-wrapper .e-menu-item:hover {
  background-color: rgba(255, 255, 255, 0.1);
}

.e-contextmenu-wrapper .e-menu-item.e-selected {
  background-color: rgba(255, 255, 255, 0.2);
  font-weight: bold;
}
```

## Icon Styling

### Customize Icon Colors

```css
/* Default icon color */
.e-contextmenu-wrapper .e-menu-icon::before {
  color: #666;
}

/* Hover icon color */
.e-contextmenu-wrapper .e-menu-item:hover .e-menu-icon::before {
  color: #1976d2;
}

/* Selected icon color */
.e-contextmenu-wrapper .e-menu-item.e-selected .e-menu-icon::before {
  color: #fff;
}

/* Custom icon size */
.e-contextmenu-wrapper .e-menu-icon {
  font-size: 18px;
}
```

### Add Custom Icons

Use Font Awesome or other icon libraries:

```html
<!-- Add Font Awesome CDN -->
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css">
```

```ts
const menuItems = [
  { text: 'Edit', iconCss: 'fas fa-edit' },
  { text: 'Delete', iconCss: 'fas fa-trash' },
  { text: 'Download', iconCss: 'fas fa-download' }
];
```

## Visual States

### Focus State (Keyboard Navigation)

```css
.e-contextmenu-wrapper .e-menu-item:focus {
  outline: 2px solid #1976d2;
  outline-offset: -2px;
}

.e-contextmenu-wrapper .e-menu-item:focus-visible {
  box-shadow: inset 0 0 0 2px #1976d2;
}
```

### Submenu Arrow

```css
/* Right-pointing arrow for submenus */
.e-contextmenu-wrapper .e-menu-item .e-caret::before {
  content: '\e76e';
  margin-left: auto;
  color: #999;
}

/* Custom arrow color on hover */
.e-contextmenu-wrapper .e-menu-item:hover .e-caret::before {
  color: #1976d2;
}
```

### Separator Styles

```css
/* Line separator */
.e-contextmenu-wrapper .e-separator {
  height: 1px;
  background-color: #e0e0e0;
  margin: 4px 0;
}

/* Thick separator */
.e-contextmenu-wrapper .e-separator.thick {
  height: 2px;
  background-color: #ccc;
  margin: 6px 0;
}
```

## Advanced Styling Examples

### Material Design Style

```css
.e-contextmenu-wrapper {
  background: white;
  border-radius: 4px;
  box-shadow: 0 3px 1px -2px rgba(0,0,0,.2),
              0 2px 2px 0 rgba(0,0,0,.14),
              0 1px 5px 0 rgba(0,0,0,.12);
  min-width: 112px;
}

.e-contextmenu-wrapper .e-menu-item {
  padding: 12px 16px;
  height: 48px;
  display: flex;
  align-items: center;
  transition: background-color 0.2s ease;
}

.e-contextmenu-wrapper .e-menu-item:hover {
  background-color: #f5f5f5;
}

.e-contextmenu-wrapper .e-menu-item.e-selected {
  background-color: #f5f5f5;
  color: #000;
}
```

### Fluent Design Style

```css
.e-contextmenu-wrapper {
  background: rgba(255, 255, 255, 0.95);
  border: 1px solid #e0e0e0;
  border-radius: 2px;
  box-shadow: 0 0.63px 2.52px rgba(0, 0, 0, 0.132),
              0 1.5px 6px rgba(0, 0, 0, 0.108);
  backdrop-filter: blur(1px);
}

.e-contextmenu-wrapper .e-menu-item {
  padding: 8px 12px;
  height: 32px;
  font-size: 13px;
}

.e-contextmenu-wrapper .e-menu-item:hover {
  background-color: #f3f3f3;
}
```

## Best Practices

1. **Consistency:** Match context menu styling with your application theme
2. **Contrast:** Ensure sufficient color contrast for accessibility (WCAG AA minimum)
3. **Spacing:** Use consistent padding and margins for visual hierarchy
4. **Performance:** Avoid complex CSS animations that could slow rendering
5. **Responsiveness:** Test styling on different screen sizes
6. **Dark Mode:** Provide both light and dark theme variants
