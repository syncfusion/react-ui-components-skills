# Code Block in React Modern Rich Text Editor

## Table of Contents
- [Overview](#overview)
- [Commands Support](#commands-support)
- [Keyboard Shortcut](#keyboard-shortcut)
- [Custom Key Binding](#custom-key-binding)

A code block inserts a `<pre>` element at the current selection and tags it with a language identifier for syntax highlighting. Code blocks are reached through the `CodeBlock` built-in toolbar identifier in the Modern Rich Text Editor. There is no slash-command entry for the code block; users reach it through the toolbar or through the `codeBlock` command.

## Overview

| Public surface | Identifier | Dispatched command |
| --- | --- | --- |
| Toolbar item | `CodeBlock` | `codeBlock` |

The `CodeBlock` toolbar item is also accepted in the Text Quick Toolbar array — see [toolbar.md](toolbar.md#text-quick-toolbar-items).

## Commands Support

The `codeBlock` command accepts a `CodeBlockCommand` payload (`{ language: string }`). The `language` value is the language identifier applied to the block — common values include `"javascript"`, `"typescript"`, `"html"`, `"css"`, `"python"`, and `"plaintext"`. An empty string applies a plain code block with no specific language annotation.

| Command | Payload | Toolbar item | Keyboard shortcut |
| --- | --- | --- | --- |
| `codeBlock` | `CodeBlockCommand` (`{ language: string }`) | `CodeBlock` | `Ctrl+Shift+B` (Windows) / `⌘ ⇧ B` (macOS) |

Add the `CodeBlock` toolbar item next to the other block-format buttons to expose the command.

```tsx
import * as React from 'react';
import { RichTextEditorUIComponent } from '@syncfusion/ej2-react-richtexteditor-ui';

const toolbarSettings = {
    items: [
        'Bold', 'Italic', 'Underline', '|',
        'Formats', 'Alignment', '|',
        'CodeBlock', 'Quote', 'HorizontalLine', '|',
        'Undo', 'Redo'
    ]
};

function App() {
    return (
        <RichTextEditorUIComponent
            toolbarSettings={toolbarSettings}
            placeholder='Insert code snippets...'
        />
    );
}

export default App;
```

## Keyboard Shortcut

| Action | Windows / Linux | macOS | Description |
| :--- | :--- | :--- | :--- |
| **Code Block** | <kbd>Ctrl</kbd> + <kbd>Shift</kbd> + <kbd>B</kbd> | <kbd>⌘</kbd> + <kbd>Shift</kbd> + <kbd>B</kbd> | Converts the block into a preformatted code snippet. |

## Custom Key Binding

The default code block shortcut can be overridden through `toolbarSettings.keyBindings`:

```tsx
const toolbarSettings = {
    items: [
        'Bold', 'Italic', 'Underline', 'Strikethrough', '|',
        'BulletList', 'NumberedList', '|',
        'Link', 'Image', 'Table', 'CodeBlock', '|',
        'Undo', 'Redo'
    ],
    keyBindings: {
        link: 'ctrl+alt+k',
        image: 'ctrl+alt+i',
        'code-block': 'ctrl+shift+c'
    }
} as any;
```

Modifiers and key values are separated with a `+` symbol. Supported modifier tokens are `ctrl`, `shift`, `alt`, and `meta`. The editor automatically updates the corresponding toolbar button tooltips and accessibility attributes to match your custom key bindings. See [toolbar.md](toolbar.md#configuring-keyboard-shortcuts) for the full set of built-in shortcuts.

For the other block formats see [block-quote.md](block-quote.md), [callout.md](callout.md), [collapsible.md](collapsible.md), and [toolbar.md](toolbar.md#block-format-toolbar-items).