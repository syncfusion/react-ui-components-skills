# Callouts in React Modern Rich Text Editor

## Table of Contents
- [Available Types](#available-types)
- [Configuring Callouts](#configuring-callouts)
- [Commands Support](#commands-support)

Callouts highlight a block of text with a status indicator. The Modern Rich Text Editor ships five callout variants — `Info`, `Success`, `Warning`, `Error`, and `Note` — each available through two public surfaces: the `Callout` split-button toolbar item (which exposes all five variants together) and the slash-command popup (which exposes each variant as its own entry). The variants cannot be wired as separate top-level toolbar buttons; the public `callout` command only fires through the `Callout` split-button child or through a slash-command selection.

- [API Reference](#api-reference)
## Available Types

| Variant | Slash-command entry |
| --- | --- |
| Info | `Info` |
| Success | `Success` |
| Warning | `Warning` |
| Error | `Error` |
| Note | `Note` |

All five variants funnel through the same `callout` command with a `CalloutCommand` payload (`{ callout: 'info' | 'success' | 'warning' | 'error' | 'note' }`).

## Configuring Callouts

Add the `Callout` split-button to expose all five variants through a single toolbar control. To surface the variants through the slash-command popup, enable `slashCommandSettings` and list the matching entries in `slashCommandSettings.items`.

```tsx
import * as React from 'react';
import { RichTextEditorUIComponent, Inject, SlashCommand } from '@syncfusion/ej2-react-richtexteditor-ui';

const toolbarSettings = {
    items: [
        'Bold', 'Italic', 'Underline', '|',
        'Callout',
        '|',
        'Undo', 'Redo'
    ]
};

const slashCommandSettings = {
    enable: true,
    items: [
        'Info', 'Success', 'Warning', 'Error', 'Note'
    ]
};

function App() {
    return (
        <RichTextEditorUIComponent
            toolbarSettings={toolbarSettings}
            slashCommandSettings={slashCommandSettings}
            placeholder='Type / for slash commands...'
        >
            <Inject services={[SlashCommand]} />
        </RichTextEditorUIComponent>
    );
}

export default App;
```

## Commands Support

The `callout` command accepts a `CalloutCommand` payload (`{ callout: 'info' | 'success' | 'warning' | 'error' | 'note' }`). The following table shows how every public surface dispatches the same command with the matching variant.

| Public surface | Identifier | Dispatched command | Payload `callout` |
| --- | --- | --- | --- |
| Toolbar split-button | `Callout` (child: `CalloutInfo`) | `callout` | `'info'` |
| Toolbar split-button | `Callout` (child: `CalloutSuccess`) | `callout` | `'success'` |
| Toolbar split-button | `Callout` (child: `CalloutWarning`) | `callout` | `'warning'` |
| Toolbar split-button | `Callout` (child: `CalloutError`) | `callout` | `'error'` |
| Toolbar split-button | `Callout` (child: `CalloutNote`) | `callout` | `'note'` |
| Slash command | `Info` | `callout` | `'info'` |
| Slash command | `Success` | `callout` | `'success'` |
| Slash command | `Warning` | `callout` | `'warning'` |
| Slash command | `Error` | `callout` | `'error'` |
| Slash command | `Note` | `callout` | `'note'` |

The callout-variant children `CalloutInfo`, `CalloutSuccess`, `CalloutWarning`, `CalloutError`, and `CalloutNote` are also accepted as identifiers in the Text Quick Toolbar array — see [toolbar.md](toolbar.md#text-quick-toolbar-items).

Slash command is a standalone module. If you skip injecting it, the editor renders without the Slash command capability even when `slashCommandSettings.enable` is `true`. See [features.md](features.md) for the full slash command setup.

For the other block formats see [block-quote.md](block-quote.md), [codeblock.md](codeblock.md), [collapsible.md](collapsible.md), and [toolbar.md](toolbar.md#block-format-toolbar-items).

## API Reference


## CalloutCommandBuilder

**Builder Class** — Builder for the callout command. Sets the callout variant (info | warning | error | success | note) before dispatching the toggle to the headless editor.

### Methods

#### `callout(variant: string)`
Set the callout variant  
**Returns:** `this`

#### `apply()`
Execute the callout command with set options  
**Returns:** `void`

---

## API Reference

## CalloutCommandBuilder

**Builder Class** — Builder for the callout command. Sets the callout variant (info | warning | error | success | note) before dispatching the toggle to the headless editor.

### Methods

#### `callout(variant: string)`
Set the callout variant  
**Returns:** `this`

#### `apply()`
Execute the callout command with set options  
**Returns:** `void`

---
