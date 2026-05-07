# Item Configuration

## Table of Contents
- [ItemModel API Reference](#itemmodel-api-reference)
- [Button Type](#button-type)
- [Button Properties](#button-properties)
  - [text](#text)
  - [id](#id)
  - [prefixIcon](#prefixicon)
  - [suffixIcon](#suffixicon)
  - [width](#width)
  - [align](#align)
  - [disabled](#disabled)
  - [visible](#visible)
  - [cssClass](#cssclass)
  - [htmlAttributes](#htmlattributes)
  - [overflow](#overflow)
  - [showAlwaysInPopup](#showalwaysinpopup)
  - [showTextOn](#showtexton)
- [Separator Type](#separator-type)
- [Input Type](#input-type)
- [Tab Navigation](#tab-navigation)
- [Complete Examples](#complete-examples)

---

## ItemModel API Reference

The `ItemModel` interface defines the structure for each toolbar item. Understanding this API is essential for configuring toolbar items programmatically.

### ItemModel Interface Structure

```typescript
interface ItemModel {
  // Identification
  id?: string;                              // Unique identifier
  
  // Content
  text?: string;                            // Display text
  template?: Function | JSX.Element;        // Custom template
  
  // Icons
  prefixIcon?: string;                      // Icon before text
  suffixIcon?: string;                      // Icon after text
  tooltipText?: string;                     // Hover tooltip
  
  // Appearance
  width?: string;                           // Item width (px, %, em)
  align?: 'Left' | 'Center' | 'Right';      // Alignment in toolbar
  cssClass?: string;                        // Custom CSS classes
  
  // State
  disabled?: boolean;                       // Is item disabled
  visible?: boolean;                        // Is item visible
  
  // Type & Behavior
  type?: 'Button' | 'Separator' | 'Input';  // Item type (default: Button)
  overflow?: 'Show' | 'Hide' | 'None';      // Popup priority (Popup mode)
  showAlwaysInPopup?: boolean;              // Always show in popup
  showTextOn?: 'Both' | 'Overflow' | 'Toolbar'; // Text visibility
  
  // Attributes & Navigation
  htmlAttributes?: { [key: string]: any };  // HTML attributes
  tabIndex?: number;                        // Tab order
}
```

### Items Array Configuration

Configure items as an array in the ToolbarComponent:

```jsx
import { ItemDirective, ItemsDirective, ToolbarComponent } from '@syncfusion/ej2-react-navigations';

const App = () => {
  // Define items as array (recommended for dynamic data)
  const toolbarItems = [
    {
      id: 'cut',
      text: 'Cut',
      prefixIcon: 'e-cut-icon',
      tooltipText: 'Cut (Ctrl+X)'
    },
    {
      id: 'copy',
      text: 'Copy',
      prefixIcon: 'e-copy-icon',
      tooltipText: 'Copy (Ctrl+C)'
    },
    {
      id: 'paste',
      text: 'Paste',
      prefixIcon: 'e-paste-icon',
      tooltipText: 'Paste (Ctrl+V)',
      overflow: 'Show'  // Always visible
    },
    {
      type: 'Separator'
    },
    {
      id: 'bold',
      text: 'Bold',
      prefixIcon: 'e-bold-icon',
      overflow: 'Hide'  // Goes to popup
    }
  ];

  return (
    <ToolbarComponent>
      <ItemsDirective>
        {toolbarItems.map((item, index) => (
          <ItemDirective key={index} {...item} />
        ))}
      </ItemsDirective>
    </ToolbarComponent>
  );
};

export default App;
```

### Using items Property Directly

Pass items array directly to ToolbarComponent:

```jsx
import { ToolbarComponent } from '@syncfusion/ej2-react-navigations';
import { useRef } from 'react';

const App = () => {
  const toolbarRef = useRef(null);

  const items = [
    { text: 'New', prefixIcon: 'e-new-icon' },
    { text: 'Open', prefixIcon: 'e-open-icon' },
    { text: 'Save', prefixIcon: 'e-save-icon' },
    { type: 'Separator' },
    { text: 'Print', prefixIcon: 'e-print-icon' }
  ];

  return (
    <ToolbarComponent ref={toolbarRef} items={items}>
    </ToolbarComponent>
  );
};

export default App;
```

### Complete ItemModel Example

Create a fully configured item with all properties:

```jsx
const complexItem = {
  // Identification
  id: 'advanced-settings',
  
  // Content
  text: 'Settings',
  tooltipText: 'Configure advanced settings (Alt+S)',
  
  // Icons
  prefixIcon: 'e-settings-icon',
  suffixIcon: 'e-dropdown',
  
  // Appearance
  width: '140px',
  align: 'Right',
  cssClass: 'custom-btn highlight-border',
  
  // State
  disabled: false,
  visible: true,
  
  // Type & Behavior
  type: 'Button',
  overflow: 'Show',
  showAlwaysInPopup: false,
  showTextOn: 'Both',
  
  // Attributes
  htmlAttributes: {
    'aria-label': 'Advanced Settings',
    'data-id': 'settings-btn',
    'role': 'button'
  },
  tabIndex: 5
};

<ItemDirective {...complexItem} />
```

### ItemModel Property Types Reference

```typescript
interface ItemModel {
  id?: string;
  // Type: string
  // Purpose: Unique identifier for the item
  // Example: "save-btn", "format-bold", "menu-file"
  
  text?: string;
  // Type: string
  // Purpose: Display text shown on button or in popup
  // Example: "Save", "Format", "Edit"
  // If omitted with only icon: just icon displays
  
  prefixIcon?: string;
  // Type: string
  // Purpose: CSS class for icon positioned before text
  // Example: "e-save-icon", "e-icons e-copy-icon"
  
  suffixIcon?: string;
  // Type: string
  // Purpose: CSS class for icon positioned after text
  // Example: "e-dropdown", "e-arrow-right"
  // Note: prefixIcon takes priority if both set
  
  width?: string;
  // Type: string (CSS length)
  // Purpose: Set custom item width
  // Examples: "100px", "50%", "auto", "8em"
  
  align?: 'Left' | 'Center' | 'Right';
  // Type: 'Left' | 'Center' | 'Right'
  // Purpose: Align item within toolbar
  // Default: 'Left'
  // Right alignment useful for Help, Settings buttons
  
  disabled?: boolean;
  // Type: boolean
  // Purpose: Disable item interaction
  // Default: false
  // When true: grayed out, no click events
  
  visible?: boolean;
  // Type: boolean
  // Purpose: Control item visibility
  // Default: true
  // When false: hidden but DOM element exists
  
  type?: 'Button' | 'Separator' | 'Input';
  // Type: 'Button' | 'Separator' | 'Input'
  // Purpose: Define item rendering type
  // Default: 'Button'
  // Separator: vertical divider line
  // Input: container for form components
  
  overflow?: 'Show' | 'Hide' | 'None';
  // Type: 'Show' | 'Hide' | 'None'
  // Purpose: Priority for Popup overflow mode
  // 'Show': always visible in toolbar (priority)
  // 'Hide': always moves to popup
  // 'None': default - moves based on space
  
  showAlwaysInPopup?: boolean;
  // Type: boolean
  // Purpose: Force item to always display in popup
  // Default: false
  // Requires overflow mode: 'Popup'
  
  showTextOn?: 'Both' | 'Overflow' | 'Toolbar';
  // Type: 'Both' | 'Overflow' | 'Toolbar'
  // Purpose: Control where button text displays
  // 'Both': visible everywhere (default)
  // 'Overflow': only in popup
  // 'Toolbar': only in main toolbar
  
  tooltipText?: string;
  // Type: string
  // Purpose: HTML title attribute tooltip
  // Example: "Save (Ctrl+S)", "Download file"
  
  template?: Function | JSX.Element;
  // Type: Function | JSX.Element
  // Purpose: Custom rendering template
  // Example: () => <CustomComponent />
  
  htmlAttributes?: { [key: string]: any };
  // Type: object
  // Purpose: HTML attributes applied to element
  // Examples: { 'aria-label': '...', 'data-test-id': '...' }
  
  tabIndex?: number;
  // Type: number
  // Purpose: Tab navigation order
  // Positive: visited in order (1, 2, 3...)
  // 0: visited in DOM order
  // Negative: skipped from tab navigation
  
  cssClass?: string;
  // Type: string
  // Purpose: Custom CSS class(es)
  // Example: "custom-btn highlight-red"
}
```

### Configuration Patterns

**Pattern 1: Simple Button Array**
```jsx
const items = [
  { text: 'Cut', prefixIcon: 'e-cut-icon' },
  { text: 'Copy', prefixIcon: 'e-copy-icon' },
  { text: 'Paste', prefixIcon: 'e-paste-icon' }
];
```

**Pattern 2: With Separators & Grouping**
```jsx
const items = [
  // File operations
  { text: 'New', prefixIcon: 'e-new-icon' },
  { text: 'Save', prefixIcon: 'e-save-icon' },
  { type: 'Separator' },
  // Edit operations
  { text: 'Undo', prefixIcon: 'e-undo-icon' },
  { text: 'Redo', prefixIcon: 'e-redo-icon' }
];
```

**Pattern 3: With State & Conditions**
```jsx
const items = [
  {
    id: 'delete',
    text: 'Delete',
    prefixIcon: 'e-delete-icon',
    disabled: !hasSelection,  // Disabled if nothing selected
    overflow: hasSpace ? 'Show' : 'Hide'
  }
];
```

**Pattern 4: Right-Aligned Items**
```jsx
const items = [
  // Left-aligned
  { text: 'File', prefixIcon: 'e-file-icon' },
  { type: 'Separator' },
  // Right-aligned
  { text: 'Help', prefixIcon: 'e-help-icon', align: 'Right' }
];
```

**Pattern 5: Mixed Content**
```jsx
const items = [
  { text: 'Bold', prefixIcon: 'e-bold-icon', type: 'Button' },
  { type: 'Separator' },
  { type: 'Input', template: fontSizeDropdown },
  { type: 'Input', template: colorPicker }
];
```

**Pattern 6: Accessible Items**
```jsx
const items = [
  {
    id: 'save-btn',
    text: 'Save',
    prefixIcon: 'e-save-icon',
    tooltipText: 'Save document (Ctrl+S)',
    htmlAttributes: {
      'aria-label': 'Save document',
      'data-shortcut': 'Ctrl+S'
    }
  }
];
```

---

## Button Type

**Button** is the default item type in Toolbar. It renders as a clickable command button.

### Basic Button

```jsx
<ItemDirective text="Save" />
```

This renders as a simple button with text "Save".

### Button with Icon

```jsx
<ItemDirective text="Save" prefixIcon="e-save-icon" />
```

- `prefixIcon` - Icon positioned before the text
- If text is omitted, only the icon displays

---

## Button Properties

### text
The display text for the button:

```jsx
<ItemDirective text="Download" />
```

### id
Unique identifier for the button:

```jsx
<ItemDirective text="Save" id="save-button" />
```

If not provided, an ID is auto-generated.

### prefixIcon
Icon positioned before the text:

```jsx
<ItemDirective text="Upload" prefixIcon="e-upload-icon" />
```

Output: 📤 Upload

### suffixIcon
Icon positioned after the text:

```jsx
<ItemDirective text="Settings" suffixIcon="e-dropdown" />
```

**Note:** If both `prefixIcon` and `suffixIcon` are provided, only `prefixIcon` is used.

### width
Set button width:

```jsx
<ItemDirective text="Custom Width" width="120px" />
```

Common values: "auto", "80px", "100%", "150px"

### align
Position the item in the toolbar:

```jsx
<ItemDirective text="Left Aligned" align="Left" />
<ItemDirective text="Center" align="Center" />
<ItemDirective text="Right Aligned" align="Right" />
```

**Alignment options:**
- `"Left"` - Default, aligns at start
- `"Center"` - Centers in toolbar
- `"Right"` - Aligns at end (useful for help buttons)

### disabled
Disable or enable the button:

```jsx
<ItemDirective text="Inactive Button" disabled={true} />
<ItemDirective text="Active Button" disabled={false} />
```

When `disabled={true}`, the button appears grayed out and click events don't trigger.

### visible
Control item visibility:

```jsx
<ItemDirective text="Visible Item" visible={true} />
<ItemDirective text="Hidden Item" visible={false} />
```

When `visible={false}`, the item is hidden from the toolbar but still exists in the DOM.

### cssClass
Add custom CSS classes to the button:

```jsx
<ItemDirective 
  text="Styled Button" 
  cssClass="custom-class highlight-btn" 
/>
```

**CSS:**
```css
.custom-class {
  background-color: #e3f2fd;
  border-radius: 4px;
}

.highlight-btn {
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
}
```

### htmlAttributes
Add HTML attributes to the button element:

```jsx
<ItemDirective 
  text="Accessible Button"
  htmlAttributes={{
    'aria-label': 'Save document',
    'data-tooltip': 'Click to save',
    'data-test-id': 'save-btn'
  }}
/>
```

Renders as:
```html
<button aria-label="Save document" data-tooltip="Click to save" data-test-id="save-btn">
  Accessible Button
</button>
```

### overflow
Control where item displays (Popup mode):

```jsx
<ItemDirective text="Always Show" overflow="Show" />
<ItemDirective text="Always Hide" overflow="Hide" />
<ItemDirective text="Auto" overflow="None" />
```

**Options:**
- `"Show"` - Always in toolbar (priority display)
- `"Hide"` - Always in popup (secondary display)
- `"None"` - Default, moves to popup based on space

### showAlwaysInPopup
Force item to always display in popup (Popup mode only):

```jsx
<ItemDirective 
  text="Advanced" 
  showAlwaysInPopup={true} 
  overflow="Hide"
/>
```

Even if space is available, item appears in popup dropdown.

### showTextOn
Control where button text displays:

```jsx
<ItemDirective 
  text="Cut" 
  prefixIcon="e-cut-icon" 
  showTextOn="Overflow" 
/>
<ItemDirective 
  text="Copy" 
  prefixIcon="e-copy-icon" 
  showTextOn="Toolbar" 
/>
```

**Options:**
- `"Both"` - Text visible everywhere
- `"Overflow"` - Text only in popup
- `"Toolbar"` - Text only in toolbar

---

## Separator Type

**Separator** adds a vertical line to visually group related commands:

```jsx
<ToolbarComponent>
  <ItemsDirective>
    <ItemDirective text="Cut" />
    <ItemDirective text="Copy" />
    <ItemDirective type="Separator" />
    <ItemDirective text="Bold" />
  </ItemsDirective>
</ToolbarComponent>
```

**Separator rules:**
- Separators at the beginning or end are not visible
- Used to group related buttons
- No properties can be set for separators

---

## Input Type

**Input** type is for embedding Syncfusion input-based components:

```jsx
<ItemDirective type="Input" template={<NumericTextBoxComponent value={10} />} />
```

### Using NumericTextBox

```jsx
import { NumericTextBoxComponent } from '@syncfusion/ej2-react-inputs';

<ItemDirective 
  type="Input" 
  template={() => (
    <NumericTextBoxComponent format='c2' value={1} />
  )} 
/>
```

### Using DropDownList

```jsx
import { DropDownListComponent } from '@syncfusion/ej2-react-dropdowns';

const data = ['Badminton', 'Basketball', 'Cricket', 'Golf', 'Hockey'];

<ItemDirective 
  type="Input" 
  template={() => (
    <DropDownListComponent dataSource={data} width={120} index={0} />
  )} 
/>
```

### Using CheckBox

```jsx
import { CheckBoxComponent } from '@syncfusion/ej2-react-buttons';

<ItemDirective 
  type="Input" 
  template={() => (
    <CheckBoxComponent label='Enable Feature' checked={true} />
  )} 
/>
```

### Using RadioButton

```jsx
import { RadioButtonComponent } from '@syncfusion/ej2-react-buttons';

<ItemDirective 
  type="Input" 
  template={() => (
    <RadioButtonComponent label='Option 1' name='group1' checked={true} />
  )} 
/>
```

---

## Tab Navigation

### tabIndex Property

Enable tab key navigation with `tabIndex`:

```jsx
<ItemDirective text="Item 1" tabIndex={1} />
<ItemDirective text="Item 2" tabIndex={2} />
<ItemDirective text="Item 3" tabIndex={3} />
```

Users can navigate using Tab and Shift+Tab keys.

### tabIndex Rules

- **Positive values (1, 2, 3...):** Items are navigated in ascending order
- **0:** Items are navigated based on DOM order
- **Negative values:** Item is skipped from tab navigation

### Example: Mixed Navigation Order

```jsx
<ItemDirective text="First" tabIndex={1} />
<ItemDirective text="Third" tabIndex={3} />
<ItemDirective text="Second" tabIndex={2} />
```

Navigation order: First → Second → Third (despite DOM order)

### Example: Full Toolbar Navigation

```jsx
<ToolbarComponent width="400" overflowMode="Scrollable">
  <ItemsDirective>
    <ItemDirective text="Cut" prefixIcon="e-cut-icon" tabIndex={0} />
    <ItemDirective text="Copy" prefixIcon="e-copy-icon" tabIndex={0} />
    <ItemDirective text="Paste" prefixIcon="e-paste-icon" tabIndex={0} />
    <ItemDirective type="Separator" />
    <ItemDirective text="Bold" prefixIcon="e-bold-icon" tabIndex={0} />
    <ItemDirective text="Underline" prefixIcon="e-underline-icon" tabIndex={0} />
    <ItemDirective text="Italic" prefixIcon="e-italic-icon" tabIndex={0} />
  </ItemsDirective>
</ToolbarComponent>
```

All items use `tabIndex={0}` for DOM-based navigation order.

---

## Complete Examples

### Multi-Type Toolbar

Combining different item types in one toolbar:

```jsx
import { ItemDirective, ItemsDirective, ToolbarComponent } from '@syncfusion/ej2-react-navigations';
import { CheckBoxComponent, RadioButtonComponent } from '@syncfusion/ej2-react-buttons';
import { DropDownListComponent } from '@syncfusion/ej2-react-dropdowns';
import { NumericTextBoxComponent } from '@syncfusion/ej2-react-inputs';

const App = () => {
  const sportList = ['Badminton', 'Basketball', 'Cricket', 'Golf', 'Hockey', 'Rugby'];

  const dropDownTemplate = () => (
    <DropDownListComponent dataSource={sportList} width={120} index={2} />
  );

  const numericTemplate = () => (
    <NumericTextBoxComponent format='c2' value={1} />
  );

  const checkBoxTemplate = () => (
    <CheckBoxComponent label='Checkbox' checked={true} />
  );

  const radioTemplate = () => (
    <RadioButtonComponent label='Radio' name='default' checked={true} />
  );

  return (
    <ToolbarComponent>
      <ItemsDirective>
        {/* Button group */}
        <ItemDirective text="Cut" prefixIcon="e-cut-icon" />
        <ItemDirective text="Copy" prefixIcon="e-copy-icon" />
        <ItemDirective type="Separator" />
        
        {/* Format group */}
        <ItemDirective text="Bold" prefixIcon="e-bold-icon" />
        <ItemDirective text="Italic" prefixIcon="e-italic-icon" />
        <ItemDirective text="Underline" prefixIcon="e-underline-icon" />
        <ItemDirective type="Separator" />
        
        {/* Input components */}
        <ItemDirective type="Input" template={numericTemplate} />
        <ItemDirective type="Input" template={dropDownTemplate} />
        <ItemDirective type="Input" template={checkBoxTemplate} />
        <ItemDirective type="Input" template={radioTemplate} />
      </ItemsDirective>
    </ToolbarComponent>
  );
};

export default App;
```

### Right-Aligned Help Button

Use `align="Right"` to position items at the end:

```jsx
<ToolbarComponent>
  <ItemsDirective>
    <ItemDirective text="Save" prefixIcon="e-save-icon" />
    <ItemDirective text="Print" prefixIcon="e-print-icon" />
    <ItemDirective text="Export" prefixIcon="e-export-icon" />
    <ItemDirective 
      text="Help" 
      prefixIcon="e-help-icon" 
      align="Right" 
      id="help-button"
    />
  </ItemsDirective>
</ToolbarComponent>
```

The Help button appears on the right side of the toolbar.

### Icon-Only Toolbar

Buttons with icons only (no text):

```jsx
<ToolbarComponent>
  <ItemsDirective>
    <ItemDirective prefixIcon="e-cut-icon" />
    <ItemDirective prefixIcon="e-copy-icon" />
    <ItemDirective prefixIcon="e-paste-icon" />
    <ItemDirective type="Separator" />
    <ItemDirective prefixIcon="e-bold-icon" />
    <ItemDirective prefixIcon="e-italic-icon" />
    <ItemDirective prefixIcon="e-underline-icon" />
  </ItemsDirective>
</ToolbarComponent>
```

Cleaner, more compact appearance.

---

## Common Patterns

### Text Editor Toolbar

```jsx
<ToolbarComponent width="500">
  <ItemsDirective>
    {/* File operations */}
    <ItemDirective text="New" prefixIcon="e-new-icon" />
    <ItemDirective text="Open" prefixIcon="e-open-icon" />
    <ItemDirective text="Save" prefixIcon="e-save-icon" />
    <ItemDirective type="Separator" />
    
    {/* Edit operations */}
    <ItemDirective text="Undo" prefixIcon="e-undo-icon" />
    <ItemDirective text="Redo" prefixIcon="e-redo-icon" />
    <ItemDirective type="Separator" />
    
    {/* Formatting */}
    <ItemDirective text="Bold" prefixIcon="e-bold-icon" />
    <ItemDirective text="Italic" prefixIcon="e-italic-icon" />
    <ItemDirective text="Underline" prefixIcon="e-underline-icon" />
  </ItemsDirective>
</ToolbarComponent>
```

### Form Toolbar with Inputs

```jsx
<ToolbarComponent>
  <ItemsDirective>
    <ItemDirective text="Filter" />
    <ItemDirective type="Input" template={() => (
      <DropDownListComponent dataSource={['All', 'Active', 'Inactive']} />
    )} />
    <ItemDirective type="Input" template={() => (
      <NumericTextBoxComponent placeholder="Items per page" />
    )} />
    <ItemDirective text="Search" prefixIcon="e-search-icon" align="Right" />
  </ItemsDirective>
</ToolbarComponent>
```

---

## Edge Cases

### Separator Position
Separators at start or end are not displayed:

```jsx
{/* These separators are hidden */}
<ItemDirective type="Separator" />
<ItemDirective text="Item 1" />
<ItemDirective type="Separator" />
```

### Item Without Text and Icon
An empty button renders but has no content:

```jsx
{/* Not recommended - invisible button */}
<ItemDirective />
```

Always provide either `text` or `prefixIcon`.

### Mixed Tab Index Values
If you mix specific numbers and 0:

```jsx
<ItemDirective text="First" tabIndex={1} />
<ItemDirective text="Second" tabIndex={0} />
<ItemDirective text="Third" tabIndex={2} />
```

Items with tabIndex > 0 are visited first in order, then items with tabIndex={0} in DOM order.
