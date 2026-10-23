# Lists in React Modern Rich Text Editor

## Table of Contents
- [List Types Overview](#list-types-overview)
- [Configuring List Items in the Toolbar](#configuring-list-items-in-the-toolbar)
- [Common Commands](#common-commands)
- [List Type Details](#list-type-details)
- [NumberFormat List Overview](#numberformat-list-overview)
- [BulletFormat List Overview](#bulletformat-list-overview)
- [Checklist Overview](#checklist-overview)
- [NumberFormat List](#numberformat-list)
- [Supported Number Formats](#supported-number-formats)
- [Custom Number List Items](#custom-number-list-items)
- [Create Numbered List](#create-numbered-list)
- [Change Number Format](#change-number-format)
- [BulletFormat List](#bulletformat-list)
- [Supported Bullet Styles](#supported-bullet-styles)
- [Custom Bullet List Items](#custom-bullet-list-items)
- [Create Bulleted List](#create-bulleted-list)
- [Change Bullet Style](#change-bullet-style)
- [Checklist](#checklist)
- [Checkbox Shape](#checkbox-shape)
- [Create Checklist](#create-checklist)
- [Keyboard Shortcuts for Lists and Indentation](#keyboard-shortcuts-for-lists-and-indentation)

The List feature in the Rich Text Editor enables users to create and format different types of lists for organizing and presenting content. Lists provide structured ways to display information and improve content readability. The Rich Text Editor supports three primary list types: numbered (NumberFormat), bulleted (BulletFormat), and checklist items.

> **Note:** List format items are configured in the toolbar item configuration. Refer to [toolbar.md](toolbar.md) for toolbar setup instructions.

- [API Reference](#api-reference)
## List Types Overview

The Rich Text Editor provides three distinct list types, each designed for different content organization needs:

| List Type | Purpose | Use Case |
| --- | --- | --- |
| **NumberFormat List** | Ordered lists with numeric formatting | Step-by-step instructions, sequences, ranked items |
| **BulletFormat List** | Unordered lists with bullet markers | Key points, feature lists, general items |
| **Checklist** | Interactive checkbox items | Task lists, requirements, verification items |

## Configuring List Items in the Toolbar

To configure which list format options appear in the toolbar, use the toolbar item configuration. List items are configured through the `listSettings` property when setting up toolbar items.

**Configure in Toolbar Settings:**

```tsx
<RichTextEditorUIComponent
  toolbarSettings={{
    items: [
      'NumberFormatList',  // Displays configured number formats
      'BulletFormatList',  // Displays configured bullet styles
      'Checklist'          // Displays checklist button
    ]
  }}
  listSettings={{
    numberFormatListItems: [
      { text: 'Decimal', listType: 'decimal' },
      { text: 'Roman', listType: 'upper-roman' }
    ],
    bulletFormatListItems: [
      { text: 'Disc', listType: 'disc' },
      { text: 'Circle', listType: 'circle' }
    ],
    checklistType: 'Square'
  }}
/>
```

The `NumberFormatList`, `BulletFormatList`, `NumberedList`, `BulletList`, and `Checklist` identifiers are also accepted in the Text Quick Toolbar array — see [toolbar.md](toolbar.md#text-quick-toolbar-items).

## Common Commands

All list operations use the following commands:
- **`numberedList`** - Create a numbered list
- **`bulletList`** - Create a bulleted list
- **`toggleTaskList`** - Create a checklist
- **`setListStyle`** - Change the current list style

**Example:**

```ts
// Create a bullet list
editorRef.current?.commands().bulletList().apply();

// Change to Roman numerals
editorRef.current?.commands().numberedList().options({ listType: 'upper-alpha' }).apply();

// Toggle checklist
editorRef.current?.commands().toggleTaskList().apply();
```

## List Type Details

### NumberFormat List Overview

Numbered lists display items in sequential order using numeric markers (1, 2, 3, etc.) or custom numbering formats (Roman numerals, letters, etc.).

**Key Properties:**
- List type configuration
- Custom list item definitions
- Numbering format options

**Supported Operations:**
- Create numbered lists
- Apply custom numbering formats
- Modify list properties at runtime

### BulletFormat List Overview

Bulleted lists display items with bullet markers (•, ○, ■, etc.) without sequential ordering.

**Key Properties:**
- List type configuration
- Custom list item definitions
- Bullet style options

**Supported Operations:**
- Create bulleted lists
- Apply custom bullet styles
- Modify list properties at runtime

### Checklist Overview

Checklists display items with interactive checkboxes, allowing users to mark items as complete or incomplete.

**Key Properties:**
- Checkbox state management
- Custom checkbox representations

**Supported Operations:**
- Create checklists
- Toggle checkbox states
- Apply custom checkbox styling

---

## NumberFormat List

The NumberFormat List feature enables users to create and manage ordered lists with numeric markers. Numbered lists are essential for presenting sequential information, step-by-step instructions, ranked items, and any content that requires a specific order.

### Supported Number Formats

The Rich Text Editor supports the following numbering formats for ordered lists:

**Available Numbering Formats:**
- `'decimal'` - Standard numeric (1, 2, 3, ...)
- `'lower-alpha'` - Lowercase alphabetic (a, b, c, ...)
- `'upper-alpha'` - Uppercase alphabetic (A, B, C, ...)
- `'lower-roman'` - Lowercase Roman numerals (i, ii, iii, ...)
- `'upper-roman'` - Uppercase Roman numerals (I, II, III, ...)
- `'lower-greek'` - Lowercase Greek letters (α, β, γ, ...)

**Default Formats:**
By default, the Rich Text Editor provides six standard number formats accessible from the toolbar dropdown:
- Number
- Lower Greek
- Lower Roman
- Upper Alpha
- Lower Alpha
- Upper Roman

The following example demonstrates how to add the number format list in toolbar of the Modern Rich Text Editor.

```tsx
import * as React from 'react';
import { RichTextEditorUIComponent } from '@syncfusion/ej2-react-richtexteditor-ui';

const toolbarSettings = {
    items: ['NumberFormatList']
};

function App() {
    return (
        <RichTextEditorUIComponent
            toolbarSettings={toolbarSettings}
            placeholder='Type something ...'
        />
    );
}

export default App;
```

### Custom Number List Items

You can customize which number formats are available in the toolbar by modifying the `numberFormatListItems` property. This allows you to define a subset of supported formats or add custom ones tailored to your application's needs.

**Property:** `listSettings.numberFormatListItems`

See [Configuring List Items in the Toolbar](#configuring-list-items-in-the-toolbar) for a full `listSettings` configuration example.

### Create Numbered List

**Command:** `numberedList`

**Description:** Inserts a numbered list at the current cursor position or applies numbering to selected content.

**Options:**
- `listType` (optional) - Specify the numbering format (e.g., `'decimal'`, `'lower-roman'`, `'upper-alpha'`)

**Example:**

```ts
// Insert a numbered list with default decimal format
editorRef.current?.commands().numberedList().apply();
```

### Change Number Format

**Command:** `setListStyle`

**Description:** Changes the numbering format of an existing numbered list without creating a new list.

**Options:**
- `listType` (required) - The new numbering format to apply

**Available Formats:**
- `'decimal'` - Standard numbering (1, 2, 3, ...)
- `'lower-alpha'` - Lowercase letters (a, b, c, ...)
- `'upper-alpha'` - Uppercase letters (A, B, C, ...)
- `'lower-roman'` - Lowercase Roman numerals (i, ii, iii, ...)
- `'upper-roman'` - Uppercase Roman numerals (I, II, III, ...)
- `'lower-greek'` - Greek letters (α, β, γ, ...)

**Example:**

```ts
// Change current list to Uppercase letters
editorRef.current?.commands().numberedList().options({ listType: 'upper-alpha' }).apply();
```

---

## BulletFormat List

The BulletFormat List feature enables users to create and manage unordered lists with bullet markers. Bulleted lists are ideal for presenting key points, feature lists, and any content that does not require a specific order.

### Supported Bullet Styles

The Rich Text Editor supports the following bullet styles for unordered lists:

**Available Bullet Styles:**
- `'disc'` - Filled circle bullet (●)
- `'circle'` - Hollow circle bullet (○)
- `'square'` - Square bullet (■)

**Default Styles:**
By default, the Rich Text Editor provides three standard bullet styles accessible from the toolbar dropdown:
- Disc
- Circle
- Square

The following example demonstrates how to add the bullet format list in toolbar of the Modern Rich Text Editor.

```tsx
import * as React from 'react';
import { RichTextEditorUIComponent } from '@syncfusion/ej2-react-richtexteditor-ui';

const toolbarSettings = {
    items: ['BulletFormatList']
};

function App() {
    return (
        <RichTextEditorUIComponent
            toolbarSettings={toolbarSettings}
            placeholder='Type something ...'
        />
    );
}

export default App;
```

### Custom Bullet List Items

You can customize which bullet styles are available in the toolbar by modifying the `bulletFormatListItems` property. This allows you to define a subset of supported styles or add custom ones tailored to your application's needs.

**Property:** `listSettings.bulletFormatListItems`

See [Configuring List Items in the Toolbar](#configuring-list-items-in-the-toolbar) for a full `listSettings` configuration example.

### Create Bulleted List

**Command:** `bulletList`

**Description:** Inserts a bulleted list at the current cursor position or applies bullet formatting to selected content.

**Options:**
- `listType` (optional) - Specify the bullet style (e.g., `'disc'`, `'circle'`, `'square'`)

**Example:**

```ts
// Insert a bulleted list with default disc bullets
editorRef.current?.commands().bulletList().apply();
```

### Change Bullet Style

**Command:** `setListStyle`

**Options:**
- `listType` (required) - The new bullet style to apply

**Example:**

```ts
// Change current list to square bullets
editorRef.current?.commands().bulletList().options({ listType: 'square' }).apply();
```

---

## Checklist

The Checklist feature enables users to create and manage interactive checkbox lists within the Rich Text Editor. Checklists are ideal for task management, requirements tracking, and scenarios where items need to be marked as complete or incomplete.

### Checkbox Shape

The `checklistType` property defines the visual shape of checkboxes in checklist items.

**Property Name:** `listSettings.checklistType`

**Available Shapes:**
- `'Square'` - Square-shaped checkbox (□ / ☑) — **default**
- `'Circle'` - Circular checkbox (○ / ◉)

**Example:**

```ts
// Create a Rich Text Editor with default square checkboxes
<RichTextEditorUIComponent
  toolbarSettings={{
    items: ['Checklist']
  }}
/>

// Use circular checkboxes
<RichTextEditorUIComponent
  listSettings={{
    checklistType: 'Circle'
  }}
/>

// Change checkbox shape at runtime
editorRef.current.listSettings.checklistType = 'Square';
```

### Create Checklist

Commands provide programmatic control over Checklist operations. Use these commands to create, modify, and manage checklists within the editor.

**Description:** Inserts a checklist at the current cursor position or applies checklist formatting to the selected content. If the selected content is already formatted as a checklist, executing the same command again removes the checklist formatting and reverts it to a normal paragraph.

**Example:**

```ts
// Insert a checklist with default settings
editorRef.current?.commands().toggleTaskList().apply();
```

---

## Keyboard Shortcuts for Lists and Indentation

| Action | Windows / Linux | macOS | Description |
| :--- | :--- | :--- | :--- |
| **Ordered List** | <kbd>Ctrl</kbd> + <kbd>Shift</kbd> + <kbd>O</kbd> | <kbd>⌘</kbd> + <kbd>Shift</kbd> + <kbd>O</kbd> | Inserts or toggles a numbered list. |
| **Unordered List** | <kbd>Ctrl</kbd> + <kbd>Alt</kbd> + <kbd>O</kbd> | <kbd>⌘</kbd> + <kbd>Option</kbd> + <kbd>O</kbd> | Inserts or toggles a bulleted list. |
| **Checklist** | <kbd>Ctrl</kbd> + <kbd>Shift</kbd> + <kbd>7</kbd> | <kbd>⌘</kbd> + <kbd>Shift</kbd> + <kbd>7</kbd> | Inserts or toggles an interactive checklist. |
| **Indent** | <kbd>Tab</kbd> | <kbd>Tab</kbd> | Increases the indentation level of a list item or text. |
| **Outdent** | <kbd>Shift</kbd> + <kbd>Tab</kbd> | <kbd>Shift</kbd> + <kbd>Tab</kbd> | Decreases the indentation level of a list item or text. |

For the remaining built-in shortcuts see [formatting.md](formatting.md#keyboard-shortcuts-for-text-formatting-and-styles) and [toolbar.md](toolbar.md#keyboard-shortcuts-for-insertions-and-blocks).

## API Reference


## ListSettings

**Configuration Class** — Configures list settings

### Properties

#### `bulletFormatListItems` (BulletFormatListItem[])
Defines the items displayed in the BulletFormatList dropdown.  
**Default:** `BulletFormatLists`

#### `numberFormatListItems` (NumberFormatListItem[])
Defines the items displayed in the NumberFormatList dropdown.  
**Default:** `NumberFormatLists`

---

## ListSettingsModel

**Interface** — Model for list settings configuration

### Properties

#### `bulletFormatListItems` (BulletFormatListItem[])
Defines the items displayed in the BulletFormatList dropdown.

#### `numberFormatListItems` (NumberFormatListItem[])
Defines the items displayed in the NumberFormatList dropdown.

---

## ListCommandBuilder

**Builder Class** — Builder for list commands (numberedList, bulletList)

### Methods

#### `apply() → void`
Execute the list command with set options

#### `options(opts: Partial<ListCommandOptions>) → this`
Set list options

| Parameter | Type | Description |
|---|---|---|
| `opts` | Partial | The list command options |

---

## ListStyleCommandBuilder

**Builder Class** — Builder for list style command

### Methods

#### `apply() → void`
Execute the list style command with set options

#### `listType(styleType: string) → this`
Set the list style type

| Parameter | Type | Description |
|---|---|---|
| `styleType` | string | The list style type to set |

---

## NumberFormatList

**Model Class** — Reusable definition of a numbered (ordered) list style. Mirrors the CSS list-style-type value of the same name and is the canonical mapping consumed by the toolbar layer when rendering a "Numbered List" dropdown.

### Properties

#### `command` (string)
Editor command name dispatched when the entry is selected.

#### `id` (string)
Stable identifier used by the toolbar renderer.

#### `listType` (NumberListType)
Underlying list-style type passed to the headless editor.

#### `text` (string)
Display label shown in the toolbar dropdown.

---

## NumberFormatListItem

**Union Type** — `NumberListType | CustomFormatListItem`

Defines items available in the numbered (ordered) list dropdown.

---

## BulletFormatList

**Interface** — Reusable definition of a bulleted (unordered) list style. Mirrors the CSS `list-style-type` value of the same name and is the canonical mapping consumed by the toolbar layer when rendering a "Bullet List" dropdown.

### Properties

#### `command` (string)
Editor command name dispatched when the entry is selected.

#### `id` (string)
Stable identifier used by the toolbar renderer.

#### `listType` (BulletListType)
Underlying list-style type passed to the headless editor.

#### `text` (string)
Display label shown in the toolbar dropdown.

---

## BulletFormatListItem

**Union Type** — Union of bullet list item types.

- `BulletListType`
- `CustomFormatListItem`

---

## ChecklistCommandBuilder

**Builder Class** — Builder for checklist command.

### Methods

#### `apply()`
Execute the checklist command  
**Returns:** `void`

---

## API Reference

## ListSettings

**Configuration Class** — Configures list settings

### Properties

#### `bulletFormatListItems` (BulletFormatListItem[])
Defines the items displayed in the BulletFormatList dropdown.  
**Default:** `BulletFormatLists`

#### `numberFormatListItems` (NumberFormatListItem[])
Defines the items displayed in the NumberFormatList dropdown.  
**Default:** `NumberFormatLists`

---
## ListSettingsModel

**Interface** — Model for list settings configuration

### Properties

#### `bulletFormatListItems` (BulletFormatListItem[])
Defines the items displayed in the BulletFormatList dropdown.

#### `numberFormatListItems` (NumberFormatListItem[])
Defines the items displayed in the NumberFormatList dropdown.

---
## ListCommandBuilder

**Builder Class** — Builder for list commands (numberedList, bulletList)

### Methods

#### `apply() → void`
Execute the list command with set options

#### `options(opts: Partial<ListCommandOptions>) → this`
Set list options

| Parameter | Type | Description |
|---|---|---|
| `opts` | Partial | The list command options |

---
## ListStyleCommandBuilder

**Builder Class** — Builder for list style command

### Methods

#### `apply() → void`
Execute the list style command with set options

#### `listType(styleType: string) → this`
Set the list style type

| Parameter | Type | Description |
|---|---|---|
| `styleType` | string | The list style type to set |

---
## NumberFormatList

**Model Class** — Reusable definition of a numbered (ordered) list style. Mirrors the CSS list-style-type value of the same name and is the canonical mapping consumed by the toolbar layer when rendering a "Numbered List" dropdown.

### Properties

#### `command` (string)
Editor command name dispatched when the entry is selected.

#### `id` (string)
Stable identifier used by the toolbar renderer.

#### `listType` (NumberListType)
Underlying list-style type passed to the headless editor.

#### `text` (string)
Display label shown in the toolbar dropdown.

---
## NumberFormatListItem

**Union Type** — `NumberListType | CustomFormatListItem`

Defines items available in the numbered (ordered) list dropdown.

---
## BulletFormatList

**Interface** — Reusable definition of a bulleted (unordered) list style. Mirrors the CSS `list-style-type` value of the same name and is the canonical mapping consumed by the toolbar layer when rendering a "Bullet List" dropdown.

### Properties

#### `command` (string)
Editor command name dispatched when the entry is selected.

#### `id` (string)
Stable identifier used by the toolbar renderer.

#### `listType` (BulletListType)
Underlying list-style type passed to the headless editor.

#### `text` (string)
Display label shown in the toolbar dropdown.

---
## BulletFormatListItem

**Union Type** — Union of bullet list item types.

- `BulletListType`
- `CustomFormatListItem`

---
## ChecklistCommandBuilder

**Builder Class** — Builder for checklist command.

### Methods

#### `apply()`
Execute the checklist command  
**Returns:** `void`

---
