# Responsive Modes

## Table of Contents
- [Scrollable Mode](#scrollable-mode)
- [Popup Mode](#popup-mode)
- [MultiRow Mode](#multirow-mode)
- [Extended Mode](#extended-mode)
- [Command Priority](#command-priority)
- [Text Display Options](#text-display-options)
- [Mode Comparison](#mode-comparison-table)
- [Choosing the Right Mode](#choosing-the-right-mode)
- [Examples](#examples)

---

## Scrollable Mode

**Scrollable** is the default overflow mode. Items display in a single line with horizontal scrolling when space is limited.

### How It Works

- All items stay visible in a horizontal row
- Left/right navigation arrows appear when items overflow
- Touch swipe and keyboard arrow keys navigate hidden items
- Arrows become disabled when reaching start/end

### Basic Scrollable Toolbar

```jsx
<ToolbarComponent overflowMode="Scrollable">
  <ItemsDirective>
    <ItemDirective text="Cut" prefixIcon="e-cut-icon" />
    <ItemDirective text="Copy" prefixIcon="e-copy-icon" />
    <ItemDirective text="Paste" prefixIcon="e-paste-icon" />
    <ItemDirective type="Separator" />
    <ItemDirective text="Bold" prefixIcon="e-bold-icon" />
    <ItemDirective text="Italic" prefixIcon="e-italic-icon" />
    <ItemDirective text="Underline" prefixIcon="e-underline-icon" />
    <ItemDirective text="Color-Picker" prefixIcon="e-color-icon" />
    <ItemDirective type="Separator" />
    <ItemDirective text="A-Z Sort" prefixIcon="e-ascending-icon" />
    <ItemDirective text="Z-A Sort" prefixIcon="e-descending-icon" />
  </ItemsDirective>
</ToolbarComponent>
```

### Navigation Interactions

**Mouse clicks:**
- Click left arrow → Previous items become visible
- Click right arrow → Next items become visible
- Hold arrow continuously → Continuous scrolling

**Touch devices:**
- Swipe left → Scroll right
- Swipe right → Scroll left
- Hold and drag → Continuous scroll

**Keyboard:**
- Left arrow key → Previous item
- Right arrow key → Next item

### Limited Width Example

```jsx
<ToolbarComponent width="300px" overflowMode="Scrollable">
  <ItemsDirective>
    <ItemDirective text="Cut" prefixIcon="e-cut-icon" />
    <ItemDirective text="Copy" prefixIcon="e-copy-icon" />
    <ItemDirective text="Paste" prefixIcon="e-paste-icon" />
    <ItemDirective type="Separator" />
    <ItemDirective text="Bold" prefixIcon="e-bold-icon" />
    <ItemDirective text="Italic" prefixIcon="e-italic-icon" />
    <ItemDirective text="Underline" prefixIcon="e-underline-icon" />
  </ItemsDirective>
</ToolbarComponent>
```

With width="300px", navigation arrows appear to scroll through items.

---

## Popup Mode

**Popup** mode hides overflow items in a dropdown container, keeping the toolbar compact.

### How It Works

- Items that fit in available space display normally
- Overflow items move to a popup dropdown
- Dropdown icon appears at toolbar end
- Click dropdown to view overflow items

### Basic Popup Toolbar

```jsx
<ToolbarComponent overflowMode="Popup" width="380px">
  <ItemsDirective>
    <ItemDirective text="Cut" prefixIcon="e-cut-icon" />
    <ItemDirective text="Copy" prefixIcon="e-copy-icon" />
    <ItemDirective text="Paste" prefixIcon="e-paste-icon" />
    <ItemDirective type="Separator" />
    <ItemDirective text="Bold" prefixIcon="e-bold-icon" />
    <ItemDirective text="Italic" prefixIcon="e-italic-icon" />
    <ItemDirective text="Underline" prefixIcon="e-underline-icon" />
    <ItemDirective type="Separator" />
    <ItemDirective text="A-Z Sort" prefixIcon="e-ascending-icon" />
    <ItemDirective text="Z-A Sort" prefixIcon="e-descending-icon" />
  </ItemsDirective>
</ToolbarComponent>
```

First few items stay visible; overflow items go to popup.

### Popup Display Behavior

- Popup opens on dropdown icon click
- If popup height exceeds page height, additional items are hidden
- Items in popup maintain their visual representation (icons, text)
- Pressing Escape closes the popup

---

## Command Priority

Control which items display in toolbar vs popup using `overflow` property.

### Priority Options

| Value | Behavior |
|-------|----------|
| `"Show"` | Always display in toolbar (primary priority) |
| `"Hide"` | Always display in popup (secondary priority) |
| `"None"` | Default behavior - move to popup based on space |

### Show Priority Example

Force important commands to stay visible:

```jsx
<ToolbarComponent overflowMode="Popup" width="300px">
  <ItemsDirective>
    {/* Always visible */}
    <ItemDirective text="Cut" prefixIcon="e-cut-icon" overflow="Show" />
    <ItemDirective text="Copy" prefixIcon="e-copy-icon" overflow="Show" />
    <ItemDirective text="Paste" prefixIcon="e-paste-icon" overflow="Show" />
    <ItemDirective type="Separator" />
    
    {/* Will move to popup */}
    <ItemDirective text="Bold" prefixIcon="e-bold-icon" />
    <ItemDirective text="Italic" prefixIcon="e-italic-icon" />
    <ItemDirective text="Underline" prefixIcon="e-underline-icon" />
    
    {/* Always in popup */}
    <ItemDirective text="Advanced..." prefixIcon="e-more-icon" overflow="Hide" />
  </ItemsDirective>
</ToolbarComponent>
```

With `overflow="Show"`, Cut, Copy, Paste stay in toolbar even when space is limited.

### Hide Priority Example

Move less-used commands to popup:

```jsx
<ToolbarComponent overflowMode="Popup" width="400px">
  <ItemsDirective>
    <ItemDirective text="Save" prefixIcon="e-save-icon" overflow="Show" />
    <ItemDirective text="Print" prefixIcon="e-print-icon" overflow="Show" />
    <ItemDirective type="Separator" overflow="Show" />
    
    <ItemDirective text="Spell Check" overflow="Hide" />
    <ItemDirective text="Grammar Check" overflow="Hide" />
    <ItemDirective text="Word Count" overflow="Hide" />
  </ItemsDirective>
</ToolbarComponent>
```

Spell/Grammar/Word Count commands go to popup first.

### Behavior When Show Priority Overflows

If Show priority items exceed available space:

```jsx
{/* All Show items: too many for toolbar */}
<ItemDirective text="Item 1" overflow="Show" />
<ItemDirective text="Item 2" overflow="Show" />
<ItemDirective text="Item 3" overflow="Show" />
<ItemDirective text="Item 4" overflow="Show" /> {/* This might move to popup */}
```

Excess Show items move to popup top position before Hide items.

---

## MultiRow Mode

**MultiRow** mode wraps overflow items to multiple rows instead of hiding them. Items display in a grid-like structure within the toolbar.

### How It Works

- Items fill the available width
- When space is exceeded, items wrap to the next row
- All items remain visible (no popup)
- Toolbar height adjusts based on number of rows needed
- No horizontal scrolling or navigation arrows

### Basic MultiRow Toolbar

```jsx
<ToolbarComponent overflowMode="MultiRow" width="300px" height="auto">
  <ItemsDirective>
    <ItemDirective text="Cut" prefixIcon="e-cut-icon" />
    <ItemDirective text="Copy" prefixIcon="e-copy-icon" />
    <ItemDirective text="Paste" prefixIcon="e-paste-icon" />
    <ItemDirective type="Separator" />
    <ItemDirective text="Bold" prefixIcon="e-bold-icon" />
    <ItemDirective text="Italic" prefixIcon="e-italic-icon" />
    <ItemDirective text="Underline" prefixIcon="e-underline-icon" />
    <ItemDirective text="Color-Picker" prefixIcon="e-color-icon" />
    <ItemDirective text="A-Z Sort" prefixIcon="e-ascending-icon" />
    <ItemDirective text="Z-A Sort" prefixIcon="e-descending-icon" />
  </ItemsDirective>
</ToolbarComponent>
```

**Result:** First row shows Cut, Copy, Paste; second row shows Bold, Italic, Underline, etc.

### MultiRow with Responsive Width

```jsx
<ToolbarComponent 
  overflowMode="MultiRow" 
  width="100%" 
  height="auto"
>
  <ItemsDirective>
    {/* Items wrap based on available width */}
    <ItemDirective text="New" prefixIcon="e-new-icon" />
    <ItemDirective text="Open" prefixIcon="e-open-icon" />
    <ItemDirective text="Save" prefixIcon="e-save-icon" />
    <ItemDirective type="Separator" />
    <ItemDirective text="Print" prefixIcon="e-print-icon" />
    <ItemDirective text="Export" prefixIcon="e-export-icon" />
    <ItemDirective type="Separator" />
    <ItemDirective text="Undo" prefixIcon="e-undo-icon" />
    <ItemDirective text="Redo" prefixIcon="e-redo-icon" />
  </ItemsDirective>
</ToolbarComponent>
```

On wide screens, more items fit per row. On narrow screens, fewer items display per row and more rows appear.

### MultiRow with Priority

Control which items display using `overflow` property:

```jsx
<ToolbarComponent overflowMode="MultiRow" width="250px" height="auto">
  <ItemsDirective>
    {/* Show priority items in first row */}
    <ItemDirective text="Save" prefixIcon="e-save-icon" overflow="Show" />
    <ItemDirective text="Print" prefixIcon="e-print-icon" overflow="Show" />
    
    {/* Hide priority items in subsequent rows */}
    <ItemDirective text="Export PDF" overflow="Hide" />
    <ItemDirective text="Export Excel" overflow="Hide" />
    <ItemDirective text="Export Word" overflow="Hide" />
  </ItemsDirective>
</ToolbarComponent>
```

### Use Cases for MultiRow

- **Mobile-first design** - Stack items naturally on narrow screens
- **Variable width containers** - Adapts to available space
- **All items visible** - No hidden menus or dropdowns
- **Dashboard toolbars** - Multi-row command layouts
- **Responsive forms** - Toolbar adapts to form width

---

## Extended Mode

**Extended** mode combines scrolling and multi-row behavior. Items wrap to multiple rows with horizontal scrolling when needed.

### How It Works

- Items wrap to multiple rows when space is limited
- If all rows don't fit, horizontal scroll arrows appear
- Users can scroll through rows instead of items
- Provides both vertical and horizontal overflow handling
- Best for toolbars with many items and limited width

### Basic Extended Toolbar

```jsx
<ToolbarComponent overflowMode="Extended" width="300px" height="auto">
  <ItemsDirective>
    <ItemDirective text="Cut" prefixIcon="e-cut-icon" />
    <ItemDirective text="Copy" prefixIcon="e-copy-icon" />
    <ItemDirective text="Paste" prefixIcon="e-paste-icon" />
    <ItemDirective type="Separator" />
    <ItemDirective text="Bold" prefixIcon="e-bold-icon" />
    <ItemDirective text="Italic" prefixIcon="e-italic-icon" />
    <ItemDirective text="Underline" prefixIcon="e-underline-icon" />
    <ItemDirective type="Separator" />
    <ItemDirective text="Font Color" prefixIcon="e-color-icon" />
    <ItemDirective text="Highlight" prefixIcon="e-highlight-icon" />
    <ItemDirective text="A-Z Sort" prefixIcon="e-ascending-icon" />
    <ItemDirective text="Z-A Sort" prefixIcon="e-descending-icon" />
  </ItemsDirective>
</ToolbarComponent>
```

### Extended with Multiple Rows

```jsx
<ToolbarComponent 
  overflowMode="Extended" 
  width="280px" 
  height="80px"
>
  <ItemsDirective>
    <ItemDirective text="Item 1" />
    <ItemDirective text="Item 2" />
    <ItemDirective text="Item 3" />
    <ItemDirective type="Separator" />
    <ItemDirective text="Item 4" />
    <ItemDirective text="Item 5" />
    <ItemDirective text="Item 6" />
    <ItemDirective type="Separator" />
    <ItemDirective text="Item 7" />
    <ItemDirective text="Item 8" />
  </ItemsDirective>
</ToolbarComponent>
```

If items don't fit in 2 rows of 280px width, horizontal scroll arrows appear to navigate between rows.

### Extended with Priority Control

```jsx
<ToolbarComponent overflowMode="Extended" width="300px">
  <ItemsDirective>
    {/* Essential items always visible */}
    <ItemDirective text="New" overflow="Show" />
    <ItemDirective text="Save" overflow="Show" />
    <ItemDirective text="Delete" overflow="Show" />
    
    {/* Secondary items may wrap */}
    <ItemDirective text="Copy" />
    <ItemDirective text="Paste" />
    
    {/* Hidden by default if space limited */}
    <ItemDirective text="Advanced Options" overflow="Hide" />
    <ItemDirective text="Settings" overflow="Hide" />
  </ItemsDirective>
</ToolbarComponent>
```

### Use Cases for Extended

- **Rich text editors** - Many formatting options with limited width
- **Complex dashboards** - Multiple tool categories in one bar
- **Data grids** - Large toolbar with many actions
- **Document editors** - Professional tools with wrapping
- **Responsive design** - Adapts from wide to narrow viewports

---

## Mode Comparison Table

| Feature | Scrollable | Popup | MultiRow | Extended |
|---------|-----------|-------|----------|----------|
| **Wrapping** | No | No | Yes | Yes |
| **Horizontal Scroll** | Yes | No | No | Yes |
| **All Items Visible** | Yes* | No | Yes | Yes** |
| **Compact** | Yes | Most | No | Moderate |
| **Space Responsive** | No | Yes | Yes | Yes |
| **Use Arrow Keys** | Yes | No | No | Yes |
| **Good for Many Items** | No | Yes | Yes | Yes |
| **Desktop Preference** | Yes | Yes | No | No |
| **Mobile Preference** | No | Yes | Yes | Yes |

*With horizontal scrolling arrows
**With horizontal scroll arrows if needed

---

## Always in Popup Items

Use `showAlwaysInPopup` to keep items always in popup dropdown:

```jsx
<ItemDirective text="Advanced Settings" overflow="Show" showAlwaysInPopup={true} />
```

**Note:** This property doesn't work with `overflow="Show"` - item must use `overflow="Hide"`.

```jsx
<ItemDirective 
  text="Advanced Settings" 
  showAlwaysInPopup={true}
/>
```

Item always appears in popup, regardless of space.

---

## Text Display Options

Control where button text displays using `showTextOn`.

### Display Modes

| Value | Behavior |
|-------|----------|
| `"Both"` | Text visible in toolbar AND popup |
| `"Overflow"` | Text only visible in popup |
| `"Toolbar"` | Text only visible in toolbar |

### Icon + Text in Popup Only

Show icons in toolbar, full text in popup:

```jsx
<ToolbarComponent overflowMode="Popup" width="330px">
  <ItemsDirective>
    <ItemDirective 
      text="Cut" 
      prefixIcon="e-cut-icon" 
      showTextOn="Overflow" 
      overflow="Show" 
    />
    <ItemDirective 
      text="Copy" 
      prefixIcon="e-copy-icon" 
      showTextOn="Overflow" 
      overflow="Show" 
    />
    <ItemDirective 
      text="Paste" 
      prefixIcon="e-paste-icon" 
      showTextOn="Overflow" 
      overflow="Show" 
    />
    <ItemDirective type="Separator" />
    <ItemDirective 
      text="Bold" 
      prefixIcon="e-bold-icon" 
      showTextOn="Overflow" 
    />
    <ItemDirective 
      text="Italic" 
      prefixIcon="e-italic-icon" 
      showTextOn="Overflow" 
    />
  </ItemsDirective>
</ToolbarComponent>
```

**Result:**
- Toolbar: Shows icons only (compact)
- Popup: Shows icons + text (informative)

### Text Always Visible

Show text in both toolbar and popup:

```jsx
<ItemDirective 
  text="Settings" 
  prefixIcon="e-settings-icon" 
  showTextOn="Both" 
/>
```

### Text Only in Toolbar

Hide text in popup, show in toolbar:

```jsx
<ItemDirective 
  text="Help" 
  showTextOn="Toolbar" 
  overflow="Hide" 
/>
```

---

## Choosing the Right Mode

### Use Scrollable When:
- You want all items always accessible
- Users prefer continuous horizontal scroll
- Toolbar has moderate number of items (8-12)
- Screen width is variable but usually large
- You want familiar scroll behavior

**Example:**
```jsx
<ToolbarComponent overflowMode="Scrollable" width="100%">
  {/* Rich text editor with many formatting options */}
</ToolbarComponent>
```

### Use Popup When:
- You want a compact toolbar
- Important items fit in visible area
- Less-used items can hide in dropdown
- Space is constrained (mobile, narrow panels)
- You want clean, minimal appearance

**Example:**
```jsx
<ToolbarComponent overflowMode="Popup" width="400px">
  {/* Mobile toolbar with priority commands visible */}
</ToolbarComponent>
```

---

## Examples

### Mobile-Optimized Toolbar

```jsx
<ToolbarComponent overflowMode="Popup" width="100%">
  <ItemsDirective>
    {/* Essential commands always visible */}
    <ItemDirective 
      text="New" 
      prefixIcon="e-new-icon" 
      overflow="Show" 
      showTextOn="Toolbar"
    />
    <ItemDirective 
      text="Save" 
      prefixIcon="e-save-icon" 
      overflow="Show" 
      showTextOn="Toolbar"
    />
    <ItemDirective type="Separator" overflow="Show" />
    
    {/* Secondary commands in popup with full text */}
    <ItemDirective 
      text="Print" 
      prefixIcon="e-print-icon" 
      showTextOn="Overflow"
    />
    <ItemDirective 
      text="Export" 
      prefixIcon="e-export-icon" 
      showTextOn="Overflow"
    />
    <ItemDirective 
      text="Share" 
      prefixIcon="e-share-icon" 
      showTextOn="Overflow"
    />
  </ItemsDirective>
</ToolbarComponent>
```

### Rich Text Editor Toolbar

```jsx
<ToolbarComponent overflowMode="Scrollable" width="100%">
  <ItemsDirective>
    {/* File operations */}
    <ItemDirective text="New" prefixIcon="e-new-icon" />
    <ItemDirective text="Open" prefixIcon="e-open-icon" />
    <ItemDirective text="Save" prefixIcon="e-save-icon" />
    <ItemDirective text="Print" prefixIcon="e-print-icon" />
    <ItemDirective type="Separator" />
    
    {/* Edit operations */}
    <ItemDirective text="Undo" prefixIcon="e-undo-icon" />
    <ItemDirective text="Redo" prefixIcon="e-redo-icon" />
    <ItemDirective type="Separator" />
    
    {/* Format operations */}
    <ItemDirective text="Bold" prefixIcon="e-bold-icon" />
    <ItemDirective text="Italic" prefixIcon="e-italic-icon" />
    <ItemDirective text="Underline" prefixIcon="e-underline-icon" />
    <ItemDirective text="Strikethrough" prefixIcon="e-strikethrough-icon" />
    
    {/* Color picker and font size at end */}
    <ItemDirective text="Font Color" prefixIcon="e-color-icon" align="Right" />
  </ItemsDirective>
</ToolbarComponent>
```

### Compact Dashboard Toolbar

```jsx
<ToolbarComponent overflowMode="Popup" width="500px">
  <ItemsDirective>
    {/* Always visible */}
    <ItemDirective 
      text="Refresh" 
      prefixIcon="e-refresh-icon" 
      overflow="Show" 
    />
    <ItemDirective 
      text="Filter" 
      prefixIcon="e-filter-icon" 
      overflow="Show" 
    />
    <ItemDirective type="Separator" overflow="Show" />
    
    {/* Visible if space */}
    <ItemDirective text="Export to Excel" prefixIcon="e-export-icon" />
    <ItemDirective text="Export to PDF" prefixIcon="e-pdf-icon" />
    <ItemDirective type="Separator" />
    
    {/* Settings always in popup */}
    <ItemDirective 
      text="Column Settings" 
      showAlwaysInPopup={true}
    />
    <ItemDirective 
      text="View Options" 
      showAlwaysInPopup={true}
    />
  </ItemsDirective>
</ToolbarComponent>
```

---

## Edge Cases

### Empty Overflow
If all items fit in toolbar, no popup appears:

```jsx
<ToolbarComponent overflowMode="Popup" width="1000px">
  {/* Few items that fit easily - no popup needed */}
</ToolbarComponent>
```

### All Hide Priority
If all items have `overflow="Hide"`, they all move to popup:

```jsx
<ToolbarComponent overflowMode="Popup">
  <ItemsDirective>
    <ItemDirective text="Item 1" overflow="Hide" />
    <ItemDirective text="Item 2" overflow="Hide" />
    <ItemDirective text="Item 3" overflow="Hide" />
  </ItemsDirective>
</ToolbarComponent>
```

Toolbar becomes empty except dropdown arrow.

### Separator in Popup
Separators move to popup when items overflow:

```jsx
<ToolbarComponent overflowMode="Popup" width="200px">
  <ItemsDirective>
    <ItemDirective text="Item 1" />
    <ItemDirective type="Separator" />
    <ItemDirective text="Item 2" />
    <ItemDirective text="Item 3" />
    <ItemDirective text="Item 4" />
  </ItemsDirective>
</ToolbarComponent>
```

Separator moves to popup with overflowing items, maintaining visual grouping.
