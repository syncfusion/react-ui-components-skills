# Editor Methods and Commands in React Modern Rich Text Editor

## Table of Contents
- [About This Reference](#about-this-reference)
- [The Command Builder](#the-command-builder)
- [Inline Format Commands](#inline-format-commands)
- [Color Commands](#color-commands)
- [Typography Commands](#typography-commands)
- [Block Format Commands](#block-format-commands)
- [List Commands](#list-commands)
- [Command and Payload Summary](#command-and-payload-summary)
- [Instance Members](#instance-members)
- [Accessing the Editor Instance](#accessing-the-editor-instance)
- [Settings Object Property Access](#settings-object-property-access)
- [Documented Types](#documented-types)

- [API Reference](#api-reference)
## About This Reference

This page consolidates every programmatic entry point that appears in the Modern Rich Text Editor documentation. Every builder, setter, payload type, and instance member listed here is taken directly from a documented example or table — nothing is extrapolated.

Formatting commands are reachable through two equivalent surfaces: a toolbar click and the fluent `commands()` chain. Both routes converge on the same `actionBegin` / `actionComplete` pipeline and share identical cancellation and history semantics.

## The Command Builder

All formatting commands are reached through the editor's fluent command builder and terminated with `apply()`:

```ts
editorRef.current?.commands().<builder>().<setter>().apply();
```

Toggle commands carry no payload — invoking them flips the mark for the selection, or sets the active typing mark when the caret is collapsed. Payload-taking commands chain a setter between the builder and `apply()`.

## Inline Format Commands

```ts
editorRef.current?.commands().bold().apply();
editorRef.current?.commands().italic().apply();
editorRef.current?.commands().underline().apply();
editorRef.current?.commands().strikethrough().apply();
editorRef.current?.commands().subscript().apply();
editorRef.current?.commands().superscript().apply();
editorRef.current?.commands().inlineCode().apply();
editorRef.current?.commands().clearFormat().apply();
editorRef.current?.commands().uppercase().apply();
editorRef.current?.commands().lowercase().apply();
```

| Builder | Setter | Effect |
| --- | --- | --- |
| `bold()` | — | Toggles bold on the selection. |
| `italic()` | — | Toggles italic on the selection. |
| `underline()` | — | Toggles underline on the selection. |
| `strikethrough()` | — | Toggles strikethrough on the selection. |
| `subscript()` | — | Toggles subscript on the selection. |
| `superscript()` | — | Toggles superscript on the selection. |
| `inlineCode()` | — | Toggles the inline code mark. |
| `clearFormat()` | — | Removes all inline marks from the selection. |
| `uppercase()` | — | Transforms the selection to uppercase. |
| `lowercase()` | — | Transforms the selection to lowercase. |

`clearFormat` strips every inline mark — bold, italic, underline, strikethrough, subscript, superscript, code, color, highlight, font size, and font family — from the current selection without altering the block structure.

For a full end-to-end wiring example see [formatting.md](formatting.md#programmatic-end-to-end-example).

## Color Commands

Both color commands share the `ColorCommand { color: string }` payload. An empty string removes the corresponding mark from the selection.

```ts
// Apply a color
editorRef.current?.commands().fontColor().color('#DC2626').apply();
editorRef.current?.commands().backgroundColor().color('rgba(255, 247, 199, 1)').apply();

// Remove a color
editorRef.current?.commands().fontColor().color('').apply();
editorRef.current?.commands().backgroundColor().color('').apply();
```

| Builder | Setter | Payload | Effect |
| --- | --- | --- | --- |
| `fontColor()` | `color(color)` | `ColorCommand` | Applies or removes a foreground color. |
| `backgroundColor()` | `color(color)` | `ColorCommand` | Applies or removes a highlight (background) color. |

The empty-string signal is the documented convention for removing a color mark. Any other "no color" string is treated as a literal color.

## Typography Commands

### Font Size

```ts
editorRef.current?.commands().fontSize().size('16px').apply();
editorRef.current?.commands().fontSize().size('').apply(); // remove the font size
```

`size` accepts any CSS size string — `'12px'`, `'1.5em'`, `'small'`, `'120%'`. The default dropdown item sends `{ size: 'Default' }`, which is treated as remove the font size.

### Font Family

```ts
editorRef.current?.commands().fontName().family('Georgia, serif').apply();
editorRef.current?.commands().fontName().family('').apply(); // remove the font family
```

The toolbar item label is `FontName`, but the command name and mark type are both `fontFamily`. The fluent builder is exposed as `.fontName()` to match the toolbar label.

## Block Format Commands

```ts
editorRef.current?.commands().paragraph().apply();
editorRef.current?.commands().heading1().apply();
editorRef.current?.commands().heading2().apply();
editorRef.current?.commands().heading3().apply();
editorRef.current?.commands().heading4().apply();
editorRef.current?.commands().blockQuote().apply();
editorRef.current?.commands().horizontalRule().apply();
```

| Command | Payload | Effect |
| --- | --- | --- |
| `paragraph` | — | Converts the current block to a paragraph. |
| `heading1` | — | Converts the current block to a level-1 heading. |
| `heading2` | — | Converts the current block to a level-2 heading. |
| `heading3` | — | Converts the current block to a level-3 heading. |
| `heading4` | — | Converts the current block to a level-4 heading. |
| `blockQuote` | — | Toggles blockquote on the current block. |
| `horizontalRule` | — | Inserts an `<hr>` element at the cursor. |
| `codeBlock` | `CodeBlockCommand` (`{ language: string }`) | Inserts or converts to a code block with the given language. |
| `callout` | `CalloutCommand` (`{ callout: 'info' \| 'success' \| 'warning' \| 'error' \| 'note' }`) | Toggles a callout block of the given variant. |
| `collapsible` | `ToggleCollapsiblePayload` (`{ triggerType: 'paragraph' \| 'heading', level?: 1 \| 2 \| 3 \| 4 }`) | Toggles a collapsible section of the given type and level. |

For `codeBlock`, see [codeblock.md](codeblock.md). For `callout`, see [callout.md](callout.md). For `collapsible`, see [collapsible.md](collapsible.md). For `blockQuote`, see [block-quote.md](block-quote.md).

## List Commands

```ts
// Create a numbered list with the default decimal format
editorRef.current?.commands().numberedList().apply();

// Create a numbered list with a specific format
editorRef.current?.commands().numberedList().options({ listType: 'upper-alpha' }).apply();

// Create a bulleted list with the default disc bullets
editorRef.current?.commands().bulletList().apply();

// Create a bulleted list with a specific style
editorRef.current?.commands().bulletList().options({ listType: 'square' }).apply();

// Toggle a checklist
editorRef.current?.commands().toggleTaskList().apply();
```

| Builder | Setter | Payload | Effect |
| --- | --- | --- | --- |
| `numberedList()` | `options({ listType })` | `{ listType?: string }` | Inserts a numbered list or applies numbering to the selection. |
| `bulletList()` | `options({ listType })` | `{ listType?: string }` | Inserts a bulleted list or applies bullets to the selection. |
| `toggleTaskList()` | — | — | Inserts a checklist, or removes checklist formatting when the selection is already a checklist. |

`setListStyle` changes the numbering or bullet format of an existing list without creating a new one.

**Valid `listType` values:**

| List | Values |
| --- | --- |
| Numbered | `'decimal'`, `'lower-alpha'`, `'upper-alpha'`, `'lower-roman'`, `'upper-roman'`, `'lower-greek'` |
| Bulleted | `'disc'`, `'circle'`, `'square'` |

See [lists.md](lists.md) for the toolbar and `listSettings` configuration.

## Command and Payload Summary

| Command | Payload type | Payload shape |
| --- | --- | --- |
| `bold`, `italic`, `underline`, `strikethrough`, `subscript`, `superscript`, `inlineCode`, `clearFormat`, `uppercase`, `lowercase` | — | No payload. |
| `fontColor`, `backgroundColor` | `ColorCommand` | `{ color: string }` — empty string removes the mark. |
| `fontSize` | — | `size(cssSize)` — `'12px'`, `'1.5em'`, `'small'`, `'120%'`; empty string removes it. |
| `fontName` | — | `family(fontFamily)` — empty string removes it. Mark and command name are `fontFamily`. |
| `paragraph`, `heading1`–`heading4`, `blockQuote`, `horizontalRule` | — | No payload. |
| `codeBlock` | `CodeBlockCommand` | `{ language: string }` |
| `callout` | `CalloutCommand` | `{ callout: 'info' \| 'success' \| 'warning' \| 'error' \| 'note' }` |
| `collapsible` | `ToggleCollapsiblePayload` | `{ triggerType: 'paragraph' \| 'heading', level?: 1 \| 2 \| 3 \| 4 }` |
| `numberedList`, `bulletList` | — | `options({ listType })` |
| `toggleTaskList` | — | No payload. |

The block-format command names above are members of `EditorCommandMap` and are dispatched by the editor when the matching toolbar identifier or slash-command entry is activated.

## Instance Members

### Accessing the Editor Instance

Attach a `useRef` to the component to reach the instance:

```tsx
const editorRef = React.useRef<RichTextEditorUIComponent>(null);
```

```tsx
const editorRef = useRef(null);
```

### getText()

Returns the editor content as text. It is used in the custom toolbar item example to count words:

```tsx
const words: number = (editor as any).getText().trim().split(/\s+/).filter(Boolean).length;
```

See [toolbar.md](toolbar.md#custom-toolbar-items) for the full word-count custom item.

### updateToolbarItems(updates)

Applies a batch of add/remove/reorder operations to the toolbar. The editor reconciles only what actually changed rather than re-rendering the whole toolbar, so things like an open dropdown or current focus aren't disrupted.

```ts
editorRef.current?.updateToolbarItems(updates);
```

### refresh()

Re-renders the component after a settings property is changed imperatively. It is used together with direct `toolbarSettings` mutation:

```tsx
const handleTypeChange = (e: React.ChangeEvent<HTMLSelectElement>) => {
    setToolbarType(e.target.value);
    if (editorRef.current) {
        (editorRef.current as any).toolbarSettings.type = e.target.value;
        (editorRef.current as any).refresh();
    }
};
```

## Settings Object Property Access

Settings properties can also be read and written imperatively on the instance.

| Property | Example |
| --- | --- |
| `toolbarSettings.type` | `(editorRef.current as any).toolbarSettings.type = e.target.value;` |
| `toolbarSettings.position` | `(editorRef.current as any).toolbarSettings.position = e.target.value;` |
| `toolbarSettings.enableFloating` | `(editorRef.current as any).toolbarSettings.enableFloating = e.target.checked;` |
| `listSettings.checklistType` | `editorRef.current.listSettings.checklistType = 'Square';` |

## Documented Types

| Type | Shape |
| --- | --- |
| `ColorCommand` | `{ color: string }` |
| `CodeBlockCommand` | `{ language: string }` |
| `CalloutCommand` | `{ callout: 'info' \| 'success' \| 'warning' \| 'error' \| 'note' }` |
| `ToggleCollapsiblePayload` | `{ triggerType: 'paragraph' \| 'heading', level?: 1 \| 2 \| 3 \| 4 }` |
| `ISlashCommandItem` | `{ text: string; command: string; iconCss: string; description: string; type: string }` |
| `SlashCommandItemSelectArgs` | `{ isInteracted: boolean; item: HTMLLIElement; itemData: ISlashCommandItem; originalEvent: MouseEvent \| KeyboardEvent \| TouchEvent; cancel: boolean }` |
| `CustomToolbarItem` | Extends `ItemModel` with `actionId: string`, `windowsShortcutText?: string`, `macShortcutText?: string` |
| `LinkQuickToolbarItem` | `'Open' \| 'Copy' \| 'Edit' \| 'Remove' \| '\|' \| CustomToolbarItem` |
| `TableQuickToolbarItem` | `'Row' \| 'Column' \| 'Header' \| 'CellBackgroundColor' \| 'VerticalAlign' \| 'Align' \| 'Remove' \| '\|' \| CustomToolbarItem` |
| `ImageQuickToolbarItem` | `'AltText' \| 'Caption' \| 'Align' \| 'Display' \| 'WrapText' \| 'Dimension' \| 'Replace' \| 'Remove' \| '\|' \| CustomToolbarItem` |
| `BuiltInToolbarItem` | The union of built-in toolbar identifiers |
| `BuiltInToolbarItemConfig` | Wrapper for a built-in identifier, e.g. `{ item: 'Bold', align: 'Right' }` |
| `EditorCommandMap` | The map of command names to their payload types |
| `SlashCommandItems` | The union of built-in slash-command entry strings |
| `ToolbarItem` | The full toolbar item union — built-in identifiers, built-in-with-config objects, custom items, and the `'\|'` separator |

See [toolbar.md](toolbar.md) for the quick-toolbar unions and the `CustomToolbarItem` field table, and [features.md](features.md) for `ISlashCommandItem` and `SlashCommandItemSelectArgs` in context.

## API Reference


## CommandExecutor

**Service Class** — Command executor providing chainable builders for all supported commands.

### Methods

#### `backgroundColor()`
Get a builder for the backgroundColor command  
**Returns:** `ColorCommandBuilder`

#### `blockQuote()`
Get a builder for the blockQuote command  
**Returns:** `ToggleCommandBuilder`

#### `bold()`
Get a builder for the bold command  
**Returns:** `ToggleCommandBuilder`

#### `bulletList()`
Get a builder for the bulletList command  
**Returns:** `ListCommandBuilder`

#### `callout()`
Get a builder for the callout command  
**Returns:** `CalloutCommandBuilder`

#### `clearFormat()`
Get a builder for the clearFormat command  
**Returns:** `ToggleCommandBuilder`

#### `codeBlock()`
Get a builder for the codeBlock command  
**Returns:** `CodeBlockCommandBuilder`

#### `collapsible()`
Get a builder for the collapsible command  
**Returns:** `CollapsibleCommandBuilder`

#### `fontColor()`
Get a builder for the fontColor command  
**Returns:** `ColorCommandBuilder`

#### `fontName()`
Get a builder for the fontName command  
**Returns:** `FontFamilyCommandBuilder`

#### `fontSize()`
Get a builder for the fontSize command  
**Returns:** `FontSizeCommandBuilder`

#### `heading1()` / `heading2()` / `heading3()` / `heading4()`
Get builders for heading commands  
**Returns:** `ToggleCommandBuilder`

#### `horizontalRule()`
Get a builder for the horizontalRule command  
**Returns:** `HorizontalRuleCommandBuilder`

#### `indent()`
Get a builder for the indent command  
**Returns:** `ToggleCommandBuilder`

#### `inlineCode()`
Get a builder for the inlineCode command  
**Returns:** `ToggleCommandBuilder`

#### `italic()`
Get a builder for the italic command  
**Returns:** `ToggleCommandBuilder`

#### `link()`
Get a builder for the link command  
**Returns:** `LinkCommandBuilder`

#### `lowercase()`
Get a builder for the lowercase command  
**Returns:** `ToggleCommandBuilder`

#### `numberedList()`
Get a builder for the numberedList command  
**Returns:** `ListCommandBuilder`

#### `outdent()`
Get a builder for the outdent command  
**Returns:** `ToggleCommandBuilder`

#### `paragraph()`
Get a builder for the paragraph command  
**Returns:** `ToggleCommandBuilder`

#### `redo()` / `undo()`
Get builders for undo/redo commands  
**Returns:** `ToggleCommandBuilder`

#### `setListStyle()`
Get a builder for the setListStyle command  
**Returns:** `ListStyleCommandBuilder`

#### `setTextAlign()`
Get a builder for the setTextAlign command  
**Returns:** `AlignmentCommandBuilder`

#### `strikethrough()`
Get a builder for the strikethrough command  
**Returns:** `ToggleCommandBuilder`

#### `subscript()` / `superscript()`
Get builders for subscript/superscript commands  
**Returns:** `ToggleCommandBuilder`

#### `toggleTaskList()`
Get a builder for the toggleTaskList command  
**Returns:** `ChecklistCommandBuilder`

#### `underline()`
Get a builder for the underline command  
**Returns:** `ToggleCommandBuilder`

#### `uppercase()`
Get a builder for the uppercase command  
**Returns:** `ToggleCommandBuilder`

---

## HorizontalRuleCommandBuilder

**Builder Class** — Builder for horizontal line command

### Methods

#### `apply() → void`
Executes the toggle command

---

## CodeBlockCommandBuilder

**Builder Class** — Builder for code block command.

### Methods

#### `language(lang: string)`
Set the code language  
**Returns:** `this`

#### `apply()`
Execute the code block command with set options  
**Returns:** `void`

---

## CollapsibleCommandBuilder

**Builder Class** — Builder for collapsible command.

### Methods

#### `level(level: 1 | 2 | 3 | 4)`
Set heading level  
**Returns:** `this`

#### `triggerType(type: string)`
Set trigger type  
**Returns:** `this`

#### `apply()`
Execute the collapsible command  
**Returns:** `void`

---

## API Reference

## CommandExecutor

**Service Class** — Command executor providing chainable builders for all supported commands.

### Methods

#### `backgroundColor()`
Get a builder for the backgroundColor command  
**Returns:** `ColorCommandBuilder`

#### `blockQuote()`
Get a builder for the blockQuote command  
**Returns:** `ToggleCommandBuilder`

#### `bold()`
Get a builder for the bold command  
**Returns:** `ToggleCommandBuilder`

#### `bulletList()`
Get a builder for the bulletList command  
**Returns:** `ListCommandBuilder`

#### `callout()`
Get a builder for the callout command  
**Returns:** `CalloutCommandBuilder`

#### `clearFormat()`
Get a builder for the clearFormat command  
**Returns:** `ToggleCommandBuilder`

#### `codeBlock()`
Get a builder for the codeBlock command  
**Returns:** `CodeBlockCommandBuilder`

#### `collapsible()`
Get a builder for the collapsible command  
**Returns:** `CollapsibleCommandBuilder`

#### `fontColor()`
Get a builder for the fontColor command  
**Returns:** `ColorCommandBuilder`

#### `fontName()`
Get a builder for the fontName command  
**Returns:** `FontFamilyCommandBuilder`

#### `fontSize()`
Get a builder for the fontSize command  
**Returns:** `FontSizeCommandBuilder`

#### `heading1()` / `heading2()` / `heading3()` / `heading4()`
Get builders for heading commands  
**Returns:** `ToggleCommandBuilder`

#### `horizontalRule()`
Get a builder for the horizontalRule command  
**Returns:** `HorizontalRuleCommandBuilder`

#### `indent()`
Get a builder for the indent command  
**Returns:** `ToggleCommandBuilder`

#### `inlineCode()`
Get a builder for the inlineCode command  
**Returns:** `ToggleCommandBuilder`

#### `italic()`
Get a builder for the italic command  
**Returns:** `ToggleCommandBuilder`

#### `link()`
Get a builder for the link command  
**Returns:** `LinkCommandBuilder`

#### `lowercase()`
Get a builder for the lowercase command  
**Returns:** `ToggleCommandBuilder`

#### `numberedList()`
Get a builder for the numberedList command  
**Returns:** `ListCommandBuilder`

#### `outdent()`
Get a builder for the outdent command  
**Returns:** `ToggleCommandBuilder`

#### `paragraph()`
Get a builder for the paragraph command  
**Returns:** `ToggleCommandBuilder`

#### `redo()` / `undo()`
Get builders for undo/redo commands  
**Returns:** `ToggleCommandBuilder`

#### `setListStyle()`
Get a builder for the setListStyle command  
**Returns:** `ListStyleCommandBuilder`

#### `setTextAlign()`
Get a builder for the setTextAlign command  
**Returns:** `AlignmentCommandBuilder`

#### `strikethrough()`
Get a builder for the strikethrough command  
**Returns:** `ToggleCommandBuilder`

#### `subscript()` / `superscript()`
Get builders for subscript/superscript commands  
**Returns:** `ToggleCommandBuilder`

#### `toggleTaskList()`
Get a builder for the toggleTaskList command  
**Returns:** `ChecklistCommandBuilder`

#### `underline()`
Get a builder for the underline command  
**Returns:** `ToggleCommandBuilder`

#### `uppercase()`
Get a builder for the uppercase command  
**Returns:** `ToggleCommandBuilder`

---
## HorizontalRuleCommandBuilder

**Builder Class** — Builder for horizontal line command

### Methods

#### `apply() → void`
Executes the toggle command

---
## CodeBlockCommandBuilder

**Builder Class** — Builder for code block command.

### Methods

#### `language(lang: string)`
Set the code language  
**Returns:** `this`

#### `apply()`
Execute the code block command with set options  
**Returns:** `void`

---
## CollapsibleCommandBuilder

**Builder Class** — Builder for collapsible command.

### Methods

#### `level(level: 1 | 2 | 3 | 4)`
Set heading level  
**Returns:** `this`

#### `triggerType(type: string)`
Set trigger type  
**Returns:** `this`

#### `apply()`
Execute the collapsible command  
**Returns:** `void`

---
