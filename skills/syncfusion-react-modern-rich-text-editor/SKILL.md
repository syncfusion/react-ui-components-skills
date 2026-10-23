---
name: syncfusion-react-modern-rich-text-editor
description: Implement the Syncfusion React Modern Rich Text Editor (RichTextEditorUIComponent) for WYSIWYG rich content editing. Use this skill for editor setup, toolbar and quick toolbar configuration, custom toolbar items, inline and block formatting, fonts and colors, lists and checklists, tables, hyperlinks, images and remote image upload, slash commands, collapsible sections, keyboard shortcuts, editor events, and command-based programmatic control in React applications.
metadata:
  author: "Syncfusion Inc"
  version: "34.1.29"
  category: "File Viewers & Editors"
---

> 📌 **Agent Notice:** The `📄 **Read:**` links in the Documentation and Navigation Guide below are reference pointers for passive file reading only. They do not imply automatic tool invocation, command execution, or action chaining.

# Syncfusion React Modern Rich Text Editor

## Component Overview

The Syncfusion React Modern Rich Text Editor (`RichTextEditorUIComponent` from `@syncfusion/ej2-react-richtexteditor-ui`) is a WYSIWYG editor that provides a modern editing experience for creating, editing, and formatting rich text content.

### Key Capabilities

The Modern Rich Text Editor provides:
- **WYSIWYG rich text editing** - Format content inline and at the block level
- **Configurable toolbar** - Ordered items, layout type (`Auto`, `Expanded`, `Scrollable`, `MultiRow`), position, and floating behaviour
- **Quick Toolbar** - Contextual popups for text selections, links, images, and tables
- **Custom toolbar items** - Your own buttons routed through `toolbarSettings.itemClicked` via `actionId`
- **Slash commands** - A `/`-triggered popup with block, inline, and media entries, plus custom items
- **Rich formatting** - Inline marks, fonts and colors, block quotes, code blocks, callouts, and collapsible sections
- **Lists** - NumberFormat, BulletFormat, and interactive checklists with configurable entries
- **Tables** - Insert, row and column operations, header rows, cell formatting, and resize handles
- **Links** - Auto-linking on paste, protocol normalization, an allow-list, and target configuration
- **Images** - Local and URL insertion, Blob or Base64 save formats, dimensions, display modes, and server-side upload pipelines
- **Fluent command API** - A `commands()` builder for programmatic control, mirrored by toolbar clicks
- **Events** - Lifecycle, focus, content change, and cancelable pre-action hooks

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- Installation of `@syncfusion/ej2-react-richtexteditor-ui` and the Tailwind 3 theme
- Syncfusion CLI project scaffolding (non-interactive and interactive)
- Vite `react-ts` project setup and CSS import
- Basic component implementation
- `value`, `valueFormat` (`'html'` / `'json'`), and `placeholder`
- `enableAutoSave` with `saveInterval`
- `toolbarSettings.items`, `imageSettings`, and reading the value on change

### Toolbar and Quick Toolbar
📄 **Read:** [references/toolbar.md](references/toolbar.md)
- `toolbarSettings.items` and the full built-in toolbar item list
- Toolbar layout `type` (`Auto`, `Expanded`, `Scrollable`, `MultiRow`) and `position`
- Floating toolbar with `enableFloating` and `floatingOffset`
- `quickToolbarSettings` with `enable`, `appendTo`, and the `text` / `image` / `link` / `table` surfaces
- Custom toolbar items via `CustomToolbarItem.actionId` and `itemClicked`
- `updatedToolbarStatus` event and `updateToolbarItems()` at runtime
- Block format toolbar identifiers: `Paragraph`, `Heading 1`–`Heading 4`, `Quote`, `CodeBlock`, `Callout`, `HorizontalLine`
- Keyboard shortcuts for insertions, history, and focus navigation
- `toolbarSettings.keyBindings` for custom shortcut combinations

### Inline Formatting
📄 **Read:** [references/formatting.md](references/formatting.md)
- All 14 inline formats with their toolbar identifiers
- `fontFamily`, `fontSize`, `fontColor`, and `backgroundColor` configuration
- Custom font families, sizes, Google Fonts, and custom colors
- `Palette` / `Picker` modes, `modeSwitcher`, `columns`, and `default`
- The fluent `commands()` builders and their setters
- Keyboard shortcuts for text formatting and styles

### Block Formats
📄 **Read:** [references/block-quote.md](references/block-quote.md)
- The `Quote` toolbar item and the `Blockquote` slash-command entry
- The `blockQuote` toggle command and its payload-free signature

📄 **Read:** [references/codeblock.md](references/codeblock.md)
- The `CodeBlock` toolbar item and the `codeBlock` command
- `CodeBlockCommand` with a `language` value and its common values
- `Ctrl+Shift+B` shortcut and its `keyBindings` override

📄 **Read:** [references/callout.md](references/callout.md)
- The `Callout` split-button and its five variants: Info, Success, Warning, Error, Note
- `CalloutCommand` payloads and the dispatch matrix across toolbar and slash-command surfaces
- Why variants cannot be wired as separate top-level toolbar buttons

📄 **Read:** [references/collapsible.md](references/collapsible.md)
- `Collapsible Paragraph` and `Collapsible Heading 1`–`Heading 4`
- `ToggleCollapsiblePayload` with `triggerType` and `level`
- Slash-command-only reachability and indentation shortcuts

### Lists
📄 **Read:** [references/lists.md](references/lists.md)
- `listSettings` with `numberFormatListItems`, `bulletFormatListItems`, and `checklistType`
- All supported numbering formats and bullet styles
- `NumberFormatList`, `BulletFormatList`, `NumberedList`, `BulletList`, and `Checklist` toolbar items
- `numberedList()`, `bulletList()`, `toggleTaskList()`, and `setListStyle` commands
- Checklist checkbox shapes (`Square`, `Circle`) and runtime changes
- Keyboard shortcuts for lists and indentation

### Table
📄 **Read:** [references/table.md](references/table.md)
- Insert table via toolbar, slash command, and `Ctrl+Shift+E`
- The 3×10 insert grid and the Insert Table dialog limits
- Row, column, header, cell background, alignment, and deletion operations
- `tableSettings.resize`
- Table Quick Toolbar items and their keyboard shortcuts

### Links
📄 **Read:** [references/links.md](references/links.md)
- All five `linkSettings` properties with types and defaults
- `linkOnPaste` behaviour and the URL shapes that trigger auto-linking
- `defaultTarget` values and the Insert-Link dialog checkbox
- `autoPrependProtocol` with `defaultProtocol`
- `allowedProtocols` validation, defaults, and restricting to one protocol
- Link Quick Toolbar items, customization, and open/close behaviour
- `Ctrl+K` shortcut

### Images
📄 **Read:** [references/image.md](references/image.md)
- Inserting images from local storage and web URLs
- `allowedTypes`, `maxFileSize`, and `saveFormat` (`Blob` / `Base64`)
- `dimension` constraints, display modes, and `resize`
- Image Quick Toolbar items: Caption, Alt Text, Replace, WrapText, Align, Display, Dimension, Remove
- Blob vs Base64 trade-offs
- Server-side upload endpoints, `UploadFiles` form field, and filename response handling
- Renaming images on the server and `onFileUploadSuccess`
- Authenticated uploads via `onFileUploading` request headers

### Slash Command
📄 **Read:** [references/features.md](references/features.md)
- Enabling the popup with `slashCommandSettings.enable`
- `SlashCommand` module injection via `<Inject services={[SlashCommand]} />`
- The full built-in items table and the commands each entry dispatches
- Restricting, reordering, and resizing the popup
- Custom items with `ISlashCommandItem`
- The `itemSelect` event, `SlashCommandItemSelectArgs`, and `cancel`
- Caret restoration, filtering, and lifecycle behaviour

### Events
📄 **Read:** [references/editor-events.md](references/editor-events.md)
- Lifecycle: `created`, `destroyed`
- Focus: `focus`, `blur`
- Content: `change` with `args.action` and `args.affectedNodes`
- Action hooks: `actionBegin` and `actionComplete`
- Settings-level events: `itemClicked`, `updatedToolbarStatus`, `itemSelect`
- Image upload events: `onFileUploading`, `onFileUploadSuccess`

### Methods and Commands
📄 **Read:** [references/editor-methods.md](references/editor-methods.md)
- The fluent `commands()` builder and the `apply()` terminator
- Inline, color, typography, block, and list commands with their setters
- `getText()`, `updateToolbarItems()`, and `refresh()`
- Imperative settings access such as `toolbarSettings.type` and `listSettings.checklistType`
- Payload types: `ColorCommand`, `CodeBlockCommand`, `CalloutCommand`, `ToggleCollapsiblePayload`

### API Reference
📄 **Read:** [references/](references/)
- Full `RichTextEditorUIModel` interface: 28 documented properties and 16 documented events
- `toolbarSettings`, `quickToolbarSettings`, `slashCommandSettings`, and `tableSettings` models with defaults
- Enumerations: `ToolbarType`, `ToolbarPosition`, `ValueFormat`, `TextAlignment`, `SlashCommandType`
- Type unions: `ToolbarItem`, `ToolbarItemUpdate`, `TableQuickToolbarItem`, `SlashCommandItems`
- Event argument classes: `ToolbarItemClickedEventArgs`, `UpdatedToolbarStatusEventArgs`, `SlashCommandItemSelectArgs`
- Toolbar reconciliation: `reconcileToolbarItems`, `ShowToolbarItem`, `RemoveToolbarItem`, `ToggleCommandBuilder`
- A documented list of naming conflicts between the API docs and the user guide
- Item types: `CustomToolbarItem`, `ISlashCommandItem`, `SlashCommandItemSelectArgs`, and the quick-toolbar unions

## Quick Start Example

```tsx
import * as React from 'react';
import { RichTextEditorUIComponent } from '@syncfusion/ej2-react-richtexteditor-ui';
import './App.css';

function App() {
    return (
        <RichTextEditorUIComponent
            placeholder='Type something.'
        />
    );
}

export default App;
```

```css
@import '../node_modules/@syncfusion/ej2-tailwind3-theme/styles/richtexteditor-ui/index.css';
```

## Common Patterns

### Read the editor value on change

```tsx
const [value, setValue] = React.useState('');

<RichTextEditorUIComponent
    value={value}
    onChange={(e: any) => setValue(e.value)}
/>
```

### Run a formatting command programmatically

```tsx
const editorRef = React.useRef<RichTextEditorUIComponent>(null);

const makeBold = () => {
    (editorRef.current as any).commands().bold().apply();
};
```

### Apply a custom color to the selection

```tsx
(editorRef.current as any).commands().fontColor().color('#00A3FF').apply();
```

### Remove a formatting mark

```tsx
// Empty string removes the mark
(editorRef.current as any).commands().backgroundColor().color('').apply();
(editorRef.current as any).commands().clearFormat().apply();
```

### Enable slash commands

```tsx
import { RichTextEditorUIComponent, Inject, SlashCommand } from '@syncfusion/ej2-react-richtexteditor-ui';

<RichTextEditorUIComponent
    slashCommandSettings={{ enable: true, items: ['Paragraph', 'Heading 1', 'Blockquote'] }}
>
    <Inject services={[SlashCommand]} />
</RichTextEditorUIComponent>
```

### Show formatting only on text selection

```tsx
<RichTextEditorUIComponent
    toolbarSettings={{ enable: false }}
    quickToolbarSettings={{
        enable: true,
        text: ['Bold', 'Italic', 'Underline', '|', 'Formats', 'Alignment', '|', 'Undo', 'Redo']
    }}
/>
```

### Route a custom toolbar button

```tsx
const toolbarSettings = {
    items: ['Bold', 'Italic', { id: 'Save', actionId: 'save', text: 'Save' }, '|', 'Undo', 'Redo'],
    itemClicked: (args: any) => {
        if (args.item?.actionId === 'save') {
            // your save logic
        }
    }
};
```

### Cancel an action before it runs

```tsx
<RichTextEditorUIComponent
    actionBegin={(args: any) => {
        if (args.action) {
            args.cancel = true;
        }
    }}
/>
```

### Load content as an HTML string

```tsx
<RichTextEditorUIComponent
    value='<p>Getting started with the Rich Text Editor UI.</p>'
    valueFormat='html'
/>
```

## Key Properties

| Property | Type | Description |
|---|---|---|
| `value` | `string` | Content loaded when the editor initializes |
| `valueFormat` | `'html' \| 'json'` | Content format for `value` (default: `'json'`) |
| `placeholder` | `string` | Instructional text shown when the editor is empty |
| `enableAutoSave` | `boolean` | Enables automatic saving of unsaved changes |
| `saveInterval` | `number` | Milliseconds the editor waits before auto-saving, triggered after the user becomes idle |
| `toolbarSettings` | `ToolbarSettingsModel` | Toolbar items, type, position, floating, `keyBindings`, `itemClicked`, `updatedToolbarStatus` |
| `quickToolbarSettings` | `QuickToolbarSettingsModel` | `enable`, `appendTo`, and the `text` / `image` / `link` / `table` arrays |
| `slashCommandSettings` | `SlashCommandSettingsModel` | `enable`, `items`, `popupWidth`, `popupHeight`, `itemSelect` |
| `imageSettings` | `ImageSettingsModel` | `allowedTypes`, `maxFileSize`, `saveFormat`, `uploadUrl`, `imageUrl`, `removeUrl`, `saveUrl`, `path`, `resize`, `dimension` |
| `linkSettings` | `LinkSettingsModel` | `linkOnPaste`, `defaultTarget`, `autoPrependProtocol`, `defaultProtocol`, `allowedProtocols` |
| `tableSettings` | `TableSettingsModel` | `resize` (default: `true`) |
| `listSettings` | `ListSettingsModel` | `numberFormatListItems`, `bulletFormatListItems`, `checklistType` |
| `fontFamily` | `FontFamilyModel` | Custom font names for the `FontName` dropdown |
| `fontSize` | `FontSizeModel` | Custom sizes for the `FontSize` dropdown |
| `fontColor` | `FontColorModel` | Foreground color picker configuration (`default`, `mode`, `columns`, `modeSwitcher`) |
| `backgroundColor` | `BackgroundColorModel` | Highlight color picker configuration (`default`, `mode`, `columns`, `modeSwitcher`) |

## Common Use Cases

**Content Management System** - Build a CMS with structured block content, configurable toolbars, and server-side image pipelines

**Document Authoring** - Create long-form documents with callouts, collapsible sections, code blocks, and tables

**Note-Taking App** - Implement personal notes with formatting toolbars, checklists, and keyboard-first navigation

**Form Input** - Capture rich text submissions with controlled value, character-aware configuration, and auto-save

**Link and Media Workflows** - Centralize media storage with remote upload endpoints, authenticated uploads, and protocol-restricted links