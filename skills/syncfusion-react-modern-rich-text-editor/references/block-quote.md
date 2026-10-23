# Block Quote in React Modern Rich Text Editor

## Table of Contents
- [Overview](#overview)
- [Commands Support](#commands-support)
- [Exposing Block Quote Through the Slash Command](#exposing-block-quote-through-the-slash-command)

A block quote wraps the current block in a `<blockquote>` element to visually set it apart from surrounding text. The Modern Rich Text Editor exposes the block quote through the `Quote` built-in toolbar identifier and through the `Blockquote` slash-command entry.

## Overview

| Public surface | Identifier | Dispatched command |
| --- | --- | --- |
| Toolbar item | `Quote` | `blockQuote` |
| Slash command | `Blockquote` | `blockQuote` |

## Commands Support

The `blockQuote` command does not require a payload. It toggles a blockquote on the current selection: invoking it on a plain paragraph wraps it in a blockquote, and invoking it again on the resulting blockquote unwraps it.

| Command | Payload | Toolbar item | Slash-command entry |
| --- | --- | --- | --- |
| `blockQuote` | — | `Quote` | `Blockquote` |

Add the `Quote` toolbar item alongside the other block-format buttons, and add `Blockquote` to `slashCommandSettings.items` if you want to expose it through the slash-command popup as well.

```tsx
import * as React from 'react';
import { RichTextEditorUIComponent, Inject, SlashCommand } from '@syncfusion/ej2-react-richtexteditor-ui';

const toolbarSettings = {
    items: [
        'Bold', 'Italic', 'Underline', '|',
        'Formats', 'Alignment', '|',
        'Quote', 'CodeBlock', '|',
        'Undo', 'Redo'
    ]
};

const slashCommandSettings = {
    enable: true,
    items: ['Blockquote']
};

function App() {
    return (
        <RichTextEditorUIComponent
            toolbarSettings={toolbarSettings}
            slashCommandSettings={slashCommandSettings}
            placeholder='Quote a passage...'
        >
            <Inject services={[SlashCommand]} />
        </RichTextEditorUIComponent>
    );
}

export default App;
```

## Exposing Block Quote Through the Slash Command

To expose the same block formats through the slash-command popup, set `slashCommandSettings.enable` to `true` and list the matching entries in `slashCommandSettings.items`. The popup reuses the toolbar identifiers for formats that share a name and adds dedicated entries for the callout variants and collapsible headings.

```tsx
import * as React from 'react';
import { RichTextEditorUIComponent, Inject, SlashCommand } from '@syncfusion/ej2-react-richtexteditor-ui';

const toolbarSettings = {
    items: ['Formats', 'Alignment', 'Quote', 'CodeBlock', 'HorizontalLine', 'Callout']
};

const slashCommandSettings = {
    enable: true,
    items: [
        'Paragraph',
        'Heading 1', 'Heading 2', 'Heading 3', 'Heading 4',
        'Blockquote',
        'Info', 'Success', 'Warning', 'Error', 'Note',
        'Collapsible Paragraph',
        'Collapsible Heading 1', 'Collapsible Heading 2',
        'Collapsible Heading 3', 'Collapsible Heading 4'
    ]
};

function App() {
    return (
        <RichTextEditorUIComponent
            toolbarSettings={toolbarSettings}
            slashCommandSettings={slashCommandSettings}
            placeholder='Type something...'
        >
            <Inject services={[SlashCommand]} />
        </RichTextEditorUIComponent>
    );
}

export default App;
```

Slash command is a standalone module. If you skip injecting it, the editor renders without the Slash command capability even when `slashCommandSettings.enable` is `true`. See [features.md](features.md) for the full slash command setup and its `itemSelect` event.

For the other block formats see [codeblock.md](codeblock.md), [callout.md](callout.md), [collapsible.md](collapsible.md), and [toolbar.md](toolbar.md#block-format-toolbar-items).