# Accessibility & Keyboard Support

## Table of Contents
- [Overview](#overview)
  - [Compliance Standards](#compliance-standards)
- [ARIA Attributes](#aria-attributes)
- [Keyboard Navigation](#keyboard-navigation)
  - [allowKeyboard Property](#allowkeyboard-property)
  - [Navigation Keys](#navigation-keys)
  - [keyDown Event](#keydown-event)
- [Screen Reader Support](#screen-reader-support)
- [Color Contrast](#color-contrast)
- [Mobile & Touch Accessibility](#mobile--touch-accessibility)
- [Right-to-Left (RTL) Support](#right-to-left-rtl-support)
- [ARIA Best Practices](#aria-best-practices)
- [Testing Accessibility](#testing-accessibility)
- [Common Accessibility Issues & Fixes](#common-accessibility-issues--fixes)
- [Summary](#summary)

## Overview

Syncfusion Toolbar is designed following **WAI-ARIA** specifications to ensure accessibility for all users, including those using assistive technologies like screen readers and keyboard navigation.

### Compliance Standards

| Standard | Support | Details |
|----------|---------|---------|
| WCAG 2.2 | ✅ Full | Web Content Accessibility Guidelines Level AA compliance |
| Section 508 | ✅ Full | U.S. federal accessibility requirements |
| Screen Readers | ✅ Full | NVDA, JAWS, VoiceOver compatibility |
| Keyboard Navigation | ✅ Full | Full keyboard control without mouse |
| Right-to-Left (RTL) | ✅ Full | Bidirectional text and layout support |
| Color Contrast | ✅ Full | WCAG AA color contrast ratios met |
| Mobile Device Support | ✅ Full | Touch and assistive technology support |

---

## ARIA Attributes

Toolbar implements WAI-ARIA attributes automatically. These provide semantic information to assistive technologies.

### Toolbar Container Attributes

**role="toolbar"**
```html
<div role="toolbar" aria-label="Document Formatting">
  <!-- toolbar items -->
</div>
```
Identifies the element as a toolbar control.

**aria-orientation**
```html
<div role="toolbar" aria-orientation="horizontal">
  <!-- items -->
</div>
```
- `"horizontal"` - Default, items arranged left to right
- `"vertical"` - Items arranged top to bottom

**aria-label**
```html
<div role="toolbar" aria-label="Text Formatting Toolbar">
  <!-- items -->
</div>
```
Describes the purpose of the toolbar. Essential for screen reader users.

**aria-disabled**
```html
<div role="toolbar" aria-disabled="false">
  <!-- items -->
</div>
```
Indicates if the entire toolbar is disabled.

### Popup Attributes

**aria-expanded**
```html
<button aria-expanded="false">More Options ▼</button>
```
- `"true"` - Popup is open
- `"false"` - Popup is closed

**aria-haspopup**
```html
<button aria-haspopup="true">Menu ▼</button>
```
Indicates that the element opens a popup dropdown.

### Example: Accessible Toolbar

```jsx
import { ItemDirective, ItemsDirective, ToolbarComponent } from '@syncfusion/ej2-react-navigations';

const App = () => {
  return (
    <ToolbarComponent 
      aria-label="Text Editor Formatting"
      role="toolbar"
    >
      <ItemsDirective>
        <ItemDirective text="Cut" prefixIcon="e-cut-icon" />
        <ItemDirective text="Copy" prefixIcon="e-copy-icon" />
        <ItemDirective text="Paste" prefixIcon="e-paste-icon" />
        <ItemDirective type="Separator" />
        <ItemDirective text="Bold" prefixIcon="e-bold-icon" />
        <ItemDirective text="Italic" prefixIcon="e-italic-icon" />
      </ItemsDirective>
    </ToolbarComponent>
  );
};

export default App;
```

---

## Keyboard Navigation

Keyboard navigation is enabled by default via the `allowKeyboard` property. Users can navigate and interact using keyboard shortcuts.

### allowKeyboard Property

Enable or disable keyboard navigation:

```jsx
<ToolbarComponent allowKeyboard={true}>
  {/* Keyboard navigation enabled (default) */}
</ToolbarComponent>

<ToolbarComponent allowKeyboard={false}>
  {/* Keyboard navigation disabled */}
</ToolbarComponent>
```

**Default:** `true` - keyboard navigation is enabled

### Navigation Keys

| Key | Action |
|-----|--------|
| **Left Arrow** | Focus previous toolbar item |
| **Right Arrow** | Focus next toolbar item |
| **Home** | Focus first toolbar item |
| **End** | Focus last toolbar item |

### Interaction Keys

| Key | Action |
|-----|--------|
| **Enter** | Activate focused button or open popup |
| **Space** | Activate focused button (alternative) |
| **Escape** | Close open popup |
| **Down Arrow** | Navigate to next popup item |
| **Up Arrow** | Navigate to previous popup item |

### Tab Navigation

| Key | Action |
|-----|--------|
| **Tab** | Move focus through interactive elements |
| **Shift + Tab** | Move focus backward through elements |

Tab order follows the `tabIndex` property when set, otherwise follows DOM order.

### Example: Full Keyboard Navigation

```jsx
<ToolbarComponent width="400" overflowMode="Scrollable" allowKeyboard={true}>
  <ItemsDirective>
    <ItemDirective text="Cut" prefixIcon="e-cut-icon" tabIndex={0} />
    <ItemDirective text="Copy" prefixIcon="e-copy-icon" tabIndex={0} />
    <ItemDirective text="Paste" prefixIcon="e-paste-icon" tabIndex={0} />
    <ItemDirective type="Separator" />
    <ItemDirective text="Bold" prefixIcon="e-bold-icon" tabIndex={0} />
    <ItemDirective text="Underline" prefixIcon="e-underline-icon" tabIndex={0} />
    <ItemDirective text="Italic" prefixIcon="e-italic-icon" tabIndex={0} />
    <ItemDirective text="Color-Picker" prefixIcon="e-color-icon" tabIndex={0} />
    <ItemDirective type="Separator" />
    <ItemDirective text="A-Z Sort" prefixIcon="e-ascending-icon" tabIndex={0} />
    <ItemDirective text="Z-A Sort" prefixIcon="e-descending-icon" tabIndex={0} />
    <ItemDirective text="Clear" prefixIcon="e-clear-icon" tabIndex={0} />
  </ItemsDirective>
</ToolbarComponent>
```

**Usage:**
1. Press Tab to enter toolbar
2. Use Left/Right arrows to navigate items
3. Press Enter to activate
4. Press Tab again to exit toolbar

### keyDown Event

Handle keyboard events using the `keyDown` event. This event fires when a key is pressed within the toolbar and can be used to implement custom keyboard shortcuts or navigation logic.

**Event Parameters:**
- `currentItem` - HTMLElement of the currently focused toolbar item
- `nextItem` - HTMLElement of the next item that would be focused
- `originalEvent` - Browser KeyboardEvent with key information
- `cancel` - Set to `true` to prevent default keyboard action
- `name` - Event name ("keyDown")

### keyDown Event Example

```jsx
import { ItemDirective, ItemsDirective, ToolbarComponent } from '@syncfusion/ej2-react-navigations';
import { useState } from 'react';

const App = () => {
  const [lastKey, setLastKey] = useState('');

  const handleKeyDown = (args) => {
    // Log keyboard interaction
    console.log('Key:', args.originalEvent.key);
    console.log('Current item:', args.currentItem?.textContent);
    console.log('Next item:', args.nextItem?.textContent);

    setLastKey(args.originalEvent.key);

    // Implement custom keyboard shortcuts
    if (args.originalEvent.key === 'Enter') {
      console.log('Activating item:', args.currentItem?.textContent);
    }

    if (args.originalEvent.key === 'Escape') {
      console.log('Closing toolbar');
    }
  };

  return (
    <>
      <ToolbarComponent keyDown={handleKeyDown} allowKeyboard={true}>
        <ItemsDirective>
          <ItemDirective text="Cut" prefixIcon="e-cut-icon" />
          <ItemDirective text="Copy" prefixIcon="e-copy-icon" />
          <ItemDirective text="Paste" prefixIcon="e-paste-icon" />
        </ItemsDirective>
      </ToolbarComponent>
      <p>Last key pressed: {lastKey}</p>
    </>
  );
};

export default App;
```

### Custom Navigation with keyDown

Implement custom keyboard behavior:

```jsx
const handleCustomKeyboard = (args) => {
  const event = args.originalEvent;

  // Custom shortcut: Ctrl+S for save
  if (event.ctrlKey && event.key === 's') {
    event.preventDefault();
    console.log('Save shortcut triggered');
    // Trigger save action
  }

  // Custom shortcut: Ctrl+Z for undo
  if (event.ctrlKey && event.key === 'z') {
    event.preventDefault();
    console.log('Undo shortcut triggered');
    // Trigger undo action
  }

  // Prevent default if handling custom shortcut
  if (event.ctrlKey || event.metaKey) {
    args.cancel = true;
  }
};

<ToolbarComponent keyDown={handleCustomKeyboard} allowKeyboard={true}>
  {/* items */}
</ToolbarComponent>
```

### Preventing Default Keyboard Navigation

```jsx
const handlePreventDefault = (args) => {
  // Disable arrow key navigation, handle manually
  if (args.originalEvent.key === 'ArrowLeft' || args.originalEvent.key === 'ArrowRight') {
    args.cancel = true;
    // Implement custom navigation logic
  }
};

<ToolbarComponent keyDown={handlePreventDefault} allowKeyboard={false}>
  {/* items */}
</ToolbarComponent>
```

---

## Screen Reader Support

Screen readers announce toolbar content and navigation options.

### What Screen Readers Announce

**Toolbar Container:**
- "Document Formatting toolbar"
- "Button, Cut"
- "Button, Copy"
- etc.

**Keyboard Help:**
- "Use arrow keys to navigate toolbar items"
- "Press Enter to activate"

### Testing with Screen Readers

**Popular Screen Readers:**
- JAWS (Windows)
- NVDA (Windows, free)
- VoiceOver (macOS, iOS)
- TalkBack (Android)

### Screen Reader Best Practices

1. **Always set aria-label:**
   ```jsx
   <ToolbarComponent aria-label="Editor Commands">
     {/* items */}
   </ToolbarComponent>
   ```

2. **Use meaningful button text:**
   ```jsx
   {/* Good */}
   <ItemDirective text="Bold" prefixIcon="e-bold-icon" />
   
   {/* Avoid: just icon */}
   <ItemDirective prefixIcon="e-bold-icon" />
   ```

3. **Group related items with separators:**
   ```jsx
   <ItemDirective text="Cut" />
   <ItemDirective text="Copy" />
   <ItemDirective type="Separator" /> {/* Clear grouping */}
   <ItemDirective text="Bold" />
   ```

---

## Color Contrast

Toolbar components meet WCAG AA color contrast requirements (4.5:1 for text).

### Default Themes

All Syncfusion themes provide compliant color contrasts:
- Tailwind3: ✅ Compliant
- Bootstrap5.3: ✅ Compliant
- Fluent2: ✅ Compliant
- Material3: ✅ Compliant

### Custom Colors

When customizing colors, ensure:

```css
/* Good: High contrast */
.e-toolbar {
  background: #ffffff;
  color: #000000;  /* 21:1 contrast ratio */
}

/* Avoid: Low contrast */
.e-toolbar {
  background: #eeeeee;
  color: #cccccc;  /* 1.18:1 - too low */
}
```

Use tools like WebAIM contrast checker to verify.

---

## Mobile & Touch Accessibility

### Touch Interactions

- **Tap** - Activate button (same as click)
- **Double-tap** - Alternative activation
- **Long-press** - In scrollable mode, scroll continuously
- **Swipe** - In scrollable mode, navigate hidden items

### Mobile Screen Readers

- **VoiceOver (iOS):** Works natively with ARIA
- **TalkBack (Android):** Full support for keyboard events
- **Magnification:** Works with standard zoom

### Mobile-Optimized Example

```jsx
<ToolbarComponent 
  overflowMode="Popup"
  width="100%"
  aria-label="Mobile Toolbar"
>
  <ItemsDirective>
    {/* Touch-friendly button sizes */}
    <ItemDirective 
      text="Save" 
      prefixIcon="e-save-icon" 
      overflow="Show"
    />
    <ItemDirective 
      text="Edit" 
      prefixIcon="e-edit-icon" 
      overflow="Show"
    />
    
    {/* Less important in popup */}
    <ItemDirective text="Share" prefixIcon="e-share-icon" />
    <ItemDirective text="Settings" prefixIcon="e-settings-icon" />
  </ItemsDirective>
</ToolbarComponent>
```

---

## Right-to-Left (RTL) Support

Toolbar automatically adapts for RTL languages.

### RTL Implementation

```jsx
import { ToolbarComponent, ItemsDirective, ItemDirective } from '@syncfusion/ej2-react-navigations';

const App = () => {
  return (
    <div dir="rtl">
      <ToolbarComponent>
        <ItemsDirective>
          <ItemDirective text="قص" prefixIcon="e-cut-icon" /> {/* Arabic: Cut */}
          <ItemDirective text="نسخ" prefixIcon="e-copy-icon" /> {/* Arabic: Copy */}
          <ItemDirective text="لصق" prefixIcon="e-paste-icon" /> {/* Arabic: Paste */}
        </ItemsDirective>
      </ToolbarComponent>
    </div>
  );
};

export default App;
```

**Automatic RTL Adjustments:**
- Items reversed in order
- Text aligned right to left
- Icons mirrored where appropriate
- Arrows reversed in scrollable mode

---

## ARIA Best Practices

### 1. Label All Toolbars

```jsx
<ToolbarComponent aria-label="Main Navigation">
  {/* items */}
</ToolbarComponent>
```

### 2. Use Descriptive Button Text

```jsx
{/* Good */}
<ItemDirective text="Download File" prefixIcon="e-download-icon" />

{/* Avoid: Too vague */}
<ItemDirective text="Click" prefixIcon="e-icon" />
```

### 3. Group Related Items

```jsx
<ItemDirective text="New" />
<ItemDirective text="Open" />
<ItemDirective text="Save" />
<ItemDirective type="Separator" />
<ItemDirective text="Print" />
```

### 4. Indicate Item Status

```jsx
{/* Disabled item */}
<ItemDirective text="Paste" disabled={true} />

{/* Selected/Active item */}
<ItemDirective text="Bold" active={true} />
```

### 5. Provide Tooltips

Use title attribute for additional context:

```jsx
<ItemDirective 
  text="Save" 
  prefixIcon="e-save-icon"
  title="Save document (Ctrl+S)"
/>
```

---

## Testing Accessibility

### Automated Tools

1. **axe-core** - Automated accessibility testing
   ```bash
   npm install axe-core
   ```

2. **accessibility-checker** - Real-time validation
   ```bash
   npm install accessibility-checker
   ```

### Manual Testing Checklist

- [ ] Keyboard navigation works (Tab, arrow keys, Enter)
- [ ] Screen reader announces toolbar purpose
- [ ] All buttons have descriptive labels
- [ ] Color contrast is sufficient (4.5:1)
- [ ] Focus indicator is visible
- [ ] Mobile touch interactions work
- [ ] RTL layout is correct

### Focus Indicator

Ensure focus is visible:

```css
.e-toolbar .e-tbar-btn:focus {
  outline: 2px solid #0066cc;
  outline-offset: 2px;
}
```

---

## Common Accessibility Issues & Fixes

### Issue: Icon-only buttons unclear
**Fix:** Add text or tooltip
```jsx
{/* Before: Unclear */}
<ItemDirective prefixIcon="e-save-icon" />

{/* After: Clear */}
<ItemDirective text="Save" prefixIcon="e-save-icon" title="Save document" />
```

### Issue: Focus not visible
**Fix:** Add focus styling
```css
.e-toolbar .e-tbar-btn:focus {
  box-shadow: 0 0 0 2px #0066cc;
}
```

### Issue: Toolbar not labeled
**Fix:** Add aria-label
```jsx
<ToolbarComponent aria-label="Document Editor">
  {/* items */}
</ToolbarComponent>
```

### Issue: Keyboard navigation skipped item
**Fix:** Set tabIndex correctly
```jsx
<ItemDirective text="Important" tabIndex={0} />
```

---

## Summary

The Toolbar component provides:
- ✅ Full WCAG 2.2 compliance
- ✅ Complete keyboard navigation
- ✅ Screen reader support
- ✅ ARIA attributes
- ✅ Mobile/touch support
- ✅ RTL language support
- ✅ High color contrast

Focus on adding descriptive labels and testing with assistive technologies to ensure your implementation is fully accessible.
