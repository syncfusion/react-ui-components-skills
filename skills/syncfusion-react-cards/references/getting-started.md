# Getting Started with Card Component

## Installation & Setup

### Package Installation

Install the Syncfusion layouts package which contains the Card component:

```bash
npm install @syncfusion/ej2-layouts --save
```

### Import CSS Styles

Add the Card CSS styles to your React application. Choose the theme that matches your design system:

```css
/* Using Tailwind CSS theme (recommended) */
@import '@syncfusion/ej2-layouts/styles/tailwind3.css';

/* Or use other available themes: */
/* @import '@syncfusion/ej2-layouts/styles/bootstrap5.css'; */
/* @import '@syncfusion/ej2-layouts/styles/material.css'; */
/* @import '@syncfusion/ej2-layouts/styles/fluent.css'; */
```

Place this import in your main CSS file or at the top of your component file.

## Basic Card Structure

A Card component is built using HTML div elements with specific CSS classes. The Card is purely CSS-based—no JavaScript component instance is required.

### Minimal Card

The simplest card requires just the root element:

```jsx
<div className="e-card">
  Sample Card Content
</div>
```

### Card with Sections

A typical card includes header, content, and optional footer:

```jsx
<div className="e-card">
  {/* Header section (optional) */}
  <div className="e-card-header">
    <div className="e-card-header-caption">
      <div className="e-card-header-title">Header Title</div>
    </div>
  </div>

  {/* Content section */}
  <div className="e-card-content">
    Card content goes here
  </div>

  {/* Actions section (optional) */}
  <div className="e-card-actions">
    <button className="e-card-btn">Action</button>
  </div>
</div>
```

## Simple Examples

### Example 1: Text-Only Card

```jsx
import React from 'react';
import '@syncfusion/ej2-layouts/styles/tailwind3.css';

export default function SimpleCard() {
  return (
    <div className="e-card">
      <div className="e-card-content">
        This is a simple card with just text content. You can add any HTML elements inside.
      </div>
    </div>
  );
}
```

### Example 2: Card with Title

```jsx
export default function CardWithTitle() {
  return (
    <div className="e-card">
      <div className="e-card-header-title">My Card Title</div>
      <div className="e-card-content">
        This card displays a title at the top.
      </div>
    </div>
  );
}
```

### Example 3: Card with Header and Content

```jsx
export default function CardWithHeader() {
  return (
    <div className="e-card">
      <div className="e-card-header">
        <div className="e-card-header-caption">
          <div className="e-card-header-title">Professional Card</div>
          <div className="e-card-sub-title">With subtitle</div>
        </div>
      </div>
      <div className="e-card-content">
        Well-organized card with a header section containing title and subtitle.
      </div>
    </div>
  );
}
```

## CSS Classes Overview

### Root Element

| Class | Purpose |
|-------|---------|
| `e-card` | Main card container (required) |
| `e-card-horizontal` | Makes card layout horizontal (optional) |

### Header Section

| Class | Purpose |
|-------|---------|
| `e-card-header` | Header container |
| `e-card-header-caption` | Wrapper for text content in header |
| `e-card-header-title` | Main title text |
| `e-card-sub-title` | Subtitle text below title |
| `e-card-header-image` | Image in header section |

### Content Sections

| Class | Purpose |
|-------|---------|
| `e-card-content` | Main content area |
| `e-card-image` | Full-width image section |
| `e-card-title` | Title overlay on images |
| `e-card-separator` | Visual divider between sections |

### Action Section

| Class | Purpose |
|-------|---------|
| `e-card-actions` | Button container |
| `e-card-btn` | Individual button styling |
| `e-card-vertical` | Stack buttons vertically |

## Important Notes

1. **CSS-Based**: The Card component is entirely CSS-based. No JavaScript initialization is needed.

2. **Flexible Structure**: You only need to include the sections you require. A card can have just content, or a full header with images and actions.

3. **Theme Support**: Choose a theme during import that matches your application's design system.

4. **Responsive**: Card layouts are responsive by default and adapt to container width.

5. **Class Naming**: All Card classes use the `e-card` prefix followed by the section name (e.g., `e-card-header`, `e-card-content`).

## Common Patterns

**Card with all sections:**

```jsx
<div className="e-card">
  <div className="e-card-header">
    <div className="e-card-header-caption">
      <div className="e-card-header-title">Complete Card</div>
      <div className="e-card-sub-title">With all sections</div>
    </div>
  </div>
  <div className="e-card-content">
    Main content area with detailed information
  </div>
  <div className="e-card-actions">
    <button className="e-card-btn">Action 1</button>
    <button className="e-card-btn">Action 2</button>
  </div>
</div>
```

**Next:** Read about [card structure and headers](./card-structure-headers.md) to add more complex header layouts with images and captions.
