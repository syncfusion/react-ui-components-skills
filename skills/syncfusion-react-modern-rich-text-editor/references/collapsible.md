# Collapsible Sections in React Modern Rich Text Editor

## Table of Contents
- [Available Types](#available-types)
- [Configuring Collapsible Headings](#configuring-collapsible-headings)
- [Commands Support](#commands-support)
- [Indentation Shortcuts](#indentation-shortcuts)

Collapsible sections wrap a block of text in a expandable container that the user can expand or collapse. The Modern Rich Text Editor ships five collapsible variants — `Collapsible Paragraph` and `Collapsible Heading 1` through `Collapsible Heading 4`. Collapsible sections are reached **only** through the slash-command popup; there is no dedicated public toolbar item for them.

## Available Types

| Type | Slash-command entry | Dispatched payload |
| --- | --- | --- |
| Collapsible Paragraph | `Collapsible Paragraph` | `{ triggerType: 'paragraph' }` |
| Collapsible Heading 1 | `Collapsible Heading 1` | `{ triggerType: 'heading', level: 1 }` |
| Collapsible Heading 2 | `Collapsible Heading 2` | `{ triggerType: 'heading', level: 2 }` |
| Collapsible Heading 3 | `Collapsible Heading 3` | `{ triggerType: 'heading', level: 3 }` |
| Collapsible Heading 4 | `Collapsible Heading 4` | `{ triggerType: 'heading', level: 4 }` |

All five entries funnel through the `collapsible` command with a `ToggleCollapsiblePayload` (`{ triggerType: 'paragraph' | 'heading', level?: 1 | 2 | 3 | 4 }`).

## Configuring Collapsible Headings

Enable `slashCommandSettings` and list the five collapsible entries in `slashCommandSettings.items` to expose them through the popup. Each entry is a string identifier from `SlashCommandItems`.

```tsx
import * as React from 'react';
import { RichTextEditorUIComponent, Inject, SlashCommand } from '@syncfusion/ej2-react-richtexteditor-ui';

const slashCommandSettings = {
    enable: true,
    items: [
        'Collapsible Paragraph',
        'Collapsible Heading 1',
        'Collapsible Heading 2',
        'Collapsible Heading 3',
        'Collapsible Heading 4'
    ]
};

function App() {
    return (
        <RichTextEditorUIComponent
            slashCommandSettings={slashCommandSettings}
            placeholder='Type / for collapsible sections...'
        >
            <Inject services={[SlashCommand]} />
        </RichTextEditorUIComponent>
    );
}

export default App;
```

## Commands Support

The `collapsible` command accepts a `ToggleCollapsiblePayload` (`{ triggerType: 'paragraph' | 'heading', level?: 1 | 2 | 3 | 4 }`). The `triggerType` selects between a collapsible paragraph and a collapsible heading; when `triggerType` is `'heading'`, `level` is required and selects the heading level (1–4).

| Slash-command entry | Command | Payload |
| --- | --- | --- |
| `Collapsible Paragraph` | `collapsible` | `{ triggerType: 'paragraph' }` |
| `Collapsible Heading 1` | `collapsible` | `{ triggerType: 'heading', level: 1 }` |
| `Collapsible Heading 2` | `collapsible` | `{ triggerType: 'heading', level: 2 }` |
| `Collapsible Heading 3` | `collapsible` | `{ triggerType: 'heading', level: 3 }` |
| `Collapsible Heading 4` | `collapsible` | `{ triggerType: 'heading', level: 4 }` |

Slash command is a standalone module. If you skip injecting it, the editor renders without the Slash command capability even when `slashCommandSettings.enable` is `true`. See [features.md](features.md) for the full slash command setup and its `itemSelect` event.

## Indentation Shortcuts

| Action | Windows / Linux | macOS | Description |
| :--- | :--- | :--- | :--- |
| **Indent** | <kbd>Tab</kbd> | <kbd>Tab</kbd> | Increases the indentation level of a list item or text. |
| **Outdent** | <kbd>Shift</kbd> + <kbd>Tab</kbd> | <kbd>Shift</kbd> + <kbd>Tab</kbd> | Decreases the indentation level of a list item or text. |

For the other block formats see [block-quote.md](block-quote.md), [callout.md](callout.md), [codeblock.md](codeblock.md), and [toolbar.md](toolbar.md#block-format-toolbar-items).