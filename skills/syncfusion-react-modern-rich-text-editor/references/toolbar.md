# Toolbar and Quick Toolbar in React Modern Rich Text Editor

## Table of Contents
- [Toolbar Items and Types](#toolbar-items-and-types)
- [Available Toolbar Items](#available-toolbar-items)
- [Layout Configuration](#layout-configuration)
- [Floating Toolbar](#floating-toolbar)
- [Custom Toolbar Items](#custom-toolbar-items)
- [itemClicked Event](#itemclicked-event)
- [updatedToolbarStatus Event](#updatedtoolbarstatus-event)
- [Updating Toolbar Items at Runtime](#updating-toolbar-items-at-runtime)
- [Quick Toolbar Settings](#quick-toolbar-settings)
- [Enable Append To Body](#enable-append-to-body)
- [Text Quick Toolbar](#text-quick-toolbar)
- [When the Text Quick Toolbar Opens](#when-the-text-quick-toolbar-opens)
- [Custom Toolbar Item Interface](#custom-toolbar-item-interface)
- [Available Quick Toolbar Items](#available-quick-toolbar-items)
- [Block Format Toolbar Items](#block-format-toolbar-items)
- [Keyboard Shortcuts for Insertions and Blocks](#keyboard-shortcuts-for-insertions-and-blocks)
- [Keyboard Shortcuts for History and Focus Navigation](#keyboard-shortcuts-for-history-and-focus-navigation)
- [Configuring Keyboard Shortcuts](#configuring-keyboard-shortcuts)

The main toolbar is the primary interaction surface for formatting commands, and its layout and contents are fully configurable through `toolbarSettings`. Alongside it, the **Quick Toolbar** is a contextual popup toolbar that appears next to the current selection inside the editable area, giving fast access to the most relevant commands without moving focus to the top of the editor. Both are configured on `RichTextEditorUIComponent`.

- [API Reference](#api-reference)
## Toolbar Items and Types

Set `toolbarSettings.items` to the ordered list of buttons (and `'|'` separators) you want to show. The default set already covers the common cases, so you only need to set this when you want to trim it down or add other built-in tools.

If you don't want a toolbar at all set `toolbarSettings.enable` to `false` and no toolbar is rendered; the editor stays fully usable programmatically.

> **Note**: Below are the default toolbar items:
> `['Bold','Italic','Underline','Strikethrough','|','Formats','Alignment','BulletList','NumberedList','|','Link','Image','Table','|','Undo','Redo']`

```tsx
import * as React from 'react';
import { RichTextEditorUIComponent } from '@syncfusion/ej2-react-richtexteditor-ui';

function App() {
    const handleItemClicked = (args: any) => {
        console.log('Toolbar item clicked:', args);
    };

    const handleToolbarStatusUpdated = (args: any) => {
        console.log('Toolbar status updated:', args);
    };

    const toolbarSettings = {
        items: [
            'Bold', 'Italic', 'Underline', 'Strikethrough', '|',
            'Formats', 'Alignment', 'BulletList', 'NumberedList', '|',
            'Link', 'Image', 'Table', '|',
            'Undo', 'Redo'
        ],
        type: 'MultiRow' as any,
        position: 'Top' as any,
        enableFloating: true,
        floatingOffset: 0,
        itemClicked: handleItemClicked,
        updatedToolbarStatus: handleToolbarStatusUpdated
    };

    return (
        <RichTextEditorUIComponent
            toolbarSettings={toolbarSettings}
        />
    );
}

export default App;
```

## Available Toolbar Items

The built-in items you can list in `toolbarSettings.items`:

- **History** — `Undo`, `Redo`
- **Text formatting** — `Bold`, `Italic`, `Underline`, `Strikethrough`, `Subscript`, `Superscript`, `InlineCode`, `ClearFormat`
- **Font and color** — `FontName`, `FontSize`, `FontColor`, `BackgroundColor`
- **Structure** — `Formats`, `Paragraph`, `Quote`, `Callout`, `CodeBlock`, `Indent`, `Outdent`, `HorizontalLine`
- **Alignment** — `Alignment`, `AlignLeft`, `AlignCenter`, `AlignRight`, `AlignJustify`
- **Lists** — `BulletFormatList`, `BulletList`, `NumberFormatList`, `NumberedList`
- **Insert** — `Link`, `Image`, `Table`

For text formatting items see [formatting.md](formatting.md), for list items see [lists.md](lists.md), for `Link`, `Image`, and `Table` see [links.md](links.md), [image.md](image.md), and [table.md](table.md), and for the block format identifiers `Quote`, `Callout`, `CodeBlock`, and `HorizontalLine` see [Block Format Toolbar Items](#block-format-toolbar-items).

## Layout Configuration

### Type

When your item list is longer than the available width, `toolbarSettings.type` controls how overflow items are displayed. The default value is `'Auto'`, which automatically uses `'Expanded'` mode on web/desktop devices and `'Scrollable'` mode on mobile devices. In `'Expanded'` mode, all toolbar items remain visible by expanding the toolbar to accommodate the available commands without introducing horizontal scrolling. Use `'Scrollable'` to always enable horizontal scrolling, or use `'MultiRow'` to wrap overflow items onto additional rows.

### Position

By default the toolbar sits at the top of the editor (`toolbarSettings.position: 'Top'`); set it to `'Bottom'` if your layout calls for the toolbar underneath the content instead.

## Floating Toolbar

For a long document, losing the toolbar off-screen as the user scrolls down is disruptive. `toolbarSettings.enableFloating` is `true` by default, so the toolbar automatically detaches and sticks in place once the editor scrolls out of view. Set it to `false` if you'd rather the toolbar just scroll away with the content. If your page includes a sticky header, use `toolbarSettings.floatingOffset` to add a top offset (in pixels) and prevent the floating toolbar from overlapping the header. The default value is `0`.

```tsx
import * as React from 'react';
import { RichTextEditorUIComponent } from '@syncfusion/ej2-react-richtexteditor-ui';

function App() {
    const editorRef = React.useRef<RichTextEditorUIComponent>(null);
    const [toolbarType, setToolbarType] = React.useState('Expanded');
    const [position, setPosition] = React.useState('Top');
    const [floating, setFloating] = React.useState(true);

    const handleTypeChange = (e: React.ChangeEvent<HTMLSelectElement>) => {
        setToolbarType(e.target.value);
        if (editorRef.current) {
            (editorRef.current as any).toolbarSettings.type = e.target.value;
            (editorRef.current as any).refresh();
        }
    };

    const handlePositionChange = (e: React.ChangeEvent<HTMLSelectElement>) => {
        setPosition(e.target.value);
        if (editorRef.current) {
            (editorRef.current as any).toolbarSettings.position = e.target.value;
            (editorRef.current as any).refresh();
        }
    };

    const handleFloatingChange = (e: React.ChangeEvent<HTMLInputElement>) => {
        setFloating(e.target.checked);
        if (editorRef.current) {
            (editorRef.current as any).toolbarSettings.enableFloating = e.target.checked;
            (editorRef.current as any).refresh();
        }
    };

    const toolbarSettings = {
        items: [
            'Undo', 'Redo', '|',
            'Bold', 'Italic', 'Underline', '|',
            'FontName', 'FontSize', 'FontColor', 'BackgroundColor', '|',
            'Formats', 'Alignment', '|',
            'BulletList', 'NumberedList', '|',
            'Undo', 'Redo'
        ],
        type: toolbarType as any,
        position: position as any,
        enableFloating: floating
    };

    return (
        <div>
            <div className="toolbar-config">
                <label>Toolbar Type:</label>
                <select value={toolbarType} onChange={handleTypeChange}>
                    <option>Expanded</option>
                    <option>MultiRow</option>
                    <option>Scrollable</option>
                </select>

                <label>Position:</label>
                <select value={position} onChange={handlePositionChange}>
                    <option>Top</option>
                    <option>Bottom</option>
                </select>

                <label>
                    <input
                        type="checkbox"
                        checked={floating}
                        onChange={handleFloatingChange}
                    />
                    Enable Floating
                </label>
            </div>

            <RichTextEditorUIComponent
                ref={editorRef}
                toolbarSettings={toolbarSettings}
            />
        </div>
    );
}

export default App;
```

---

## Custom Toolbar Items

Alongside the built-in strings, `toolbarSettings.items` also accepts item objects for tools of your own. Give each one an `actionId` — a unique identifier you'll check for in the `itemClicked` handler — plus the usual presentation properties (`prefixIcon`/`suffixIcon`, `tooltipText`, `align`, `cssClass`, `disabled`, and so on, the same set the underlying EJ2 Toolbar item model uses).

```tsx
import * as React from 'react';
import { RichTextEditorUIComponent } from '@syncfusion/ej2-react-richtexteditor-ui';

const wordCountItem = {
    id: 'WordCount',
    actionId: 'wordCount',
    prefixIcon: 'e-icons e-numbering-list',
    tooltipText: 'Word count',
    align: 'Left'
} as any;

function App() {
    const editorRef = React.useRef<RichTextEditorUIComponent>(null);

    const handleItemClicked = (args: any): void => {
        if (args.item) {
            const editor = editorRef.current;
            if (editor && args.item.id === 'WordCount') {
                const words: number = (editor as any).getText().trim().split(/\s+/).filter(Boolean).length;
                alert('Word count: ' + words);
            }
        }
    };

    const toolbarSettings = {
        items: [
            'Bold', 'Italic', 'Underline', '|',
            wordCountItem,
            '|', 'Undo', 'Redo'
        ],
        itemClicked: handleItemClicked
    };

    return (
        <RichTextEditorUIComponent
            ref={editorRef}
            toolbarSettings={toolbarSettings}
        />
    );
}

export default App;
```

## itemClicked Event

Listen for `itemClicked` to know whenever any toolbar button is clicked, before its command runs — this is also how you handle a custom item's action, since custom items don't have a built-in command of their own. The event args' `item` field is the toolbar item's model, so check `args.item.actionId` to identify which custom item was clicked.

Refer to the sample above: clicking the word-count button reads `args.item.actionId`, and if it matches `'wordCount'`, calls `editor.getText()` to count and display the words in the document.

## updatedToolbarStatus Event

The `updatedToolbarStatus` event is raised after the toolbar synchronizes its visual state with the current editor selection or cursor formatting. Its event arguments provide the active inline marks, block-level formats, and resolved font and color styles.

The following example demonstrates how to handle the `updatedToolbarStatus` event and display which text formatting styles (bold, italic, underline, strikethrough) are currently active at the cursor position.

```tsx
import * as React from 'react';
import { RichTextEditorUIComponent } from '@syncfusion/ej2-react-richtexteditor-ui';

const toolbarSettings = {
    items: [
        'Bold', 'Italic', 'Underline', 'Strikethrough', '|',
        'BulletList', 'NumberedList', '|',
        'Undo', 'Redo'
    ]
};

function App() {
    const [eventStatus, setEventStatus] = React.useState('');

    const handleUpdatedToolbarStatus = () => {
        const timestamp = new Date().toLocaleTimeString();
        setEventStatus(`Toolbar status updated at ${timestamp}`);
    };

    return (
        <div className="container">
            <RichTextEditorUIComponent
                toolbarSettings={toolbarSettings}
                placeholder='Use toolbar buttons to trigger updateToolbarStatus event...'
                updatedToolbarStatus={handleUpdatedToolbarStatus}
            />
            <div className="event-status">
                {eventStatus || 'No toolbar status update yet'}
            </div>
        </div>
    );
}

export default App;
```

## Updating Toolbar Items at Runtime

If your toolbar needs to change based on application state — for example, showing a different set of tools depending on the user's permissions — call `editorRef.current?.updateToolbarItems(updates)` with a batch of add/remove/reorder operations. The editor reconciles only what actually changed rather than re-rendering the whole toolbar, so things like an open dropdown or current focus aren't disrupted.

---

## Quick Toolbar Settings

The Quick Toolbar is a contextual popup toolbar that appears next to the current selection inside the editable area. It is wired through `quickToolbarSettings` on `RichTextEditorUIComponent`.

The Modern Rich Text Editor exposes the Quick Toolbar through one configuration object with five sub-surfaces:

| Property | Type | Default | Purpose |
| --- | --- | --- | --- |
| `enable` | `boolean` | `true` | Master switch for all quick toolbars. |
| `appendTo` | `string` | `null` | Mounts the popup to the document body or specified selector to escape clipping inside narrow or scroll-constrained containers. |
| `text` | `ToolbarItem[]` | `null` | Items shown when text is selected. |
| `image` | `ImageQuickToolbarItem[]` | `['AltText', 'Caption', '\|', 'Align', 'Display', 'WrapText', '\|', 'Dimension', 'Replace', 'Remove']` | Items shown when an image is selected. |
| `link` | `LinkQuickToolbarItem[]` | `['Open', 'Copy', 'Edit', 'Remove']` | Items shown when the caret is inside a link. |
| `table` | `TableQuickToolbarItem[]` | `['Header', 'Remove', '\|', 'Row', 'Column', '\|', 'CellBackgroundColor', 'Align', 'VerticalAlign']` | Items shown when the caret is inside a table. |

```tsx
import * as React from 'react';
import { RichTextEditorUIComponent } from '@syncfusion/ej2-react-richtexteditor-ui';

const toolbarSettings = {
    items: [
        'Bold', 'Italic', 'Underline', 'Strikethrough', '|',
        'BulletList', 'NumberedList', '|',
        'Link', 'Image', 'Table', '|',
        'Undo', 'Redo'
    ]
};

const quickToolbarSettings = {
    enable: true,
    text: ['Undo', 'Redo', 'Bold', 'Italic', 'Underline', 'Strikethrough', 'FontColor', 'BackgroundColor', 'Formats', 'BulletList', 'NumberedList'],
    image: ['AltText', 'Caption', '|', 'Align', 'Display', 'WrapText', '|', 'Dimension', 'Replace', 'Remove'],
    link: ['Open', 'Copy', 'Edit', 'Remove'],
    table: ['Header', 'Remove', '|', 'Row', 'Column', '|', 'CellBackgroundColor', 'Align', 'VerticalAlign']
} as any;

function App() {
    return (
        <RichTextEditorUIComponent
            toolbarSettings={toolbarSettings}
            placeholder='Type something or insert content to see quick toolbar...'
            quickToolbarSettings={quickToolbarSettings}
        />
    );
}

export default App;
```

When `quickToolbarSettings.enable` is `true` and at least one of `text`, `image`, `link`, or `table` is non-empty, the corresponding popup is built and bound to the relevant editor surface. Any sub-surface whose array is empty (or `null`) is **not** instantiated — set `text` to `null` to disable the text quick toolbar while keeping the image / link / table ones.

> **Note:** Inline mode can be achieved by disabling the main toolbar and adding the text quick-toolbar items on the `quickToolbarSettings.text` surface:
>
> ```tsx
> <RichTextEditorUIComponent
>     toolbarSettings={{ enable: false }}
>     quickToolbarSettings={{
>         enable: true,
>         text: [
>             'Bold', 'Italic', 'Underline', '|',
>             'Formats', 'Alignment', '|',
>             'NumberedList', 'BulletList', '|',
>             'Undo', 'Redo'
>         ]
>     }}
> />
> ```

## Enable Append To Body

`appendTo` controls whether the quick toolbar popup is mounted on `document.body` or on the editor container.

- When set to `'body'`, the popup is appended to the document body and remains visible over the editor area.
- When set to `null` (default), the popup is appended to the editor area and always remains inside the editor area.

```tsx
import * as React from 'react';
import { RichTextEditorUIComponent } from '@syncfusion/ej2-react-richtexteditor-ui';

const toolbarSettings = {
    items: [
        'Undo', 'Redo', '|',
        'Bold', 'Italic', 'Underline', '|',
        'Image', 'Link', 'Table'
    ]
};

const quickToolbarSettings = {
    enable: true,
    appendTo: 'body'
};

function App() {
    return (
        <RichTextEditorUIComponent
            toolbarSettings={toolbarSettings}
            quickToolbarSettings={quickToolbarSettings}
            placeholder='Type something...'
        />
    );
}

export default App;
```

> With `appendTo: 'body'`, the popup is parented to `document.body` and its visibility is clamped to the editor's bounds — the popup hides itself off-screen when the target block scrolls out of the editor's visible region.

## Text Quick Toolbar

`text` is the array of items shown when a non-collapsed text selection exists inside the editor. The popup opens on `mouseup` and `keyup` (when a non-collapsed selection exists), and closes on `Escape`, on outside interaction, or on `selectionchange` that empties the selection.

The default is `null`, which means the Text Quick Toolbar is **not** rendered unless you supply a non-empty `text` array. The Text Quick Toolbar is the only quick-toolbar surface that accepts the full `ToolbarItem` union — built-in identifiers, built-in-with-config objects, custom items, and the `'|'` separator.

```tsx
import * as React from 'react';
import { RichTextEditorUIComponent } from '@syncfusion/ej2-react-richtexteditor-ui';

const toolbarSettings = {
    items: [
        'Undo', 'Redo', '|',
        'Bold', 'Italic', 'Underline', '|',
        'Image', 'Link', 'Table'
    ]
};

const quickToolbarSettings = {
    text: [
        'Bold', 'Italic', 'Underline', 'StrikeThrough', '|',
        'ClearFormat'
    ]
};

function App() {
    return (
        <RichTextEditorUIComponent
            toolbarSettings={toolbarSettings}
            quickToolbarSettings={quickToolbarSettings}
            placeholder='Select text to see quick toolbar...'
        />
    );
}

export default App;
```

### When the Text Quick Toolbar Opens

The Text Quick Toolbar opens under three triggers:

- **Mouse selection (`mouseup`)** — after the user finishes a drag-select inside the editable area.
- **Keyboard selection (`keyup`)** — when the user extends a selection with Shift + arrow keys.
- **Triple-click (`trippleclick`)** — when the user triple-clicks a word or block; the toolbar opens centred above the block.

The popup is dismissed on:

- `Escape` key.
- Editor blur (configurable).

---

## Custom Toolbar Item Interface

A **Custom Toolbar Item** is a user-supplied toolbar entry that does not map to a built-in editor command. It is declared through the `CustomToolbarItem` interface and is dispatched through the editor's `toolbarSettings.itemClicked` event.

The Text Quick Toolbar (`quickToolbarSettings.text`) accepts the full `ToolbarItem` union — built-in identifiers, built-in-with-config objects, custom items, and the `'|'` separator. The item shapes that can be mixed into any Text Quick Toolbar array are:

| Item form | Example | Purpose |
| --- | --- | --- |
| Built-in identifier | `'Bold'` | Use the editor's built-in handler. |
| Built-in with config | `{ item: 'Bold', align: 'Right' }` | Override the label of a built-in. |
| Custom item | `{ actionId: 'save', id: 'save', text: 'Save' }` | Wire a user-defined action (this section). |
| Separator | `\|` | Visual divider between groups. |

Add `actionId` (and optional shortcut labels) to the toolbar item to make it route through `toolbarSettings.itemClicked`:

```ts
export interface CustomToolbarItem extends ItemModel {
    actionId: string;
    windowsShortcutText?: string;
    macShortcutText?: string;
}
```

| Field | Type | Required | Purpose |
| --- | --- | --- | --- |
| `actionId` | `string` | Yes | Stable identifier — read off the clicked item in `toolbarSettings.itemClicked` to route the action. |
| `windowsShortcutText` | `string` | No | Tooltip text for the Windows keyboard shortcut. |
| `macShortcutText` | `string` | No | Tooltip text for the macOS keyboard shortcut. |

Any other field inherited from `ItemModel` (`text`, `id`, `iconCss`, `prefixIcon`, `tooltipText`, `htmlAttributes`, etc.) is also accepted.

A custom item can appear in any of the four quick-toolbar arrays — `text`, `image`, `link`, `table` — because each of those arrays is a union that includes `CustomToolbarItem`.

```tsx
import * as React from 'react';
import { RichTextEditorUIComponent } from '@syncfusion/ej2-react-richtexteditor-ui';

const toolbarSettings = {
    items: [
        'Undo', 'Redo', '|',
        'Bold', 'Italic', 'Underline', '|',
        'Image', 'Link', 'Table'
    ]
};

const quickToolbarSettings = {
    text: [
        'Bold', 'Italic', 'Underline', 'StrikeThrough', '|',
        'ClearFormat', 'Superscript', 'Subscript'
    ],
    image: [
        'AltText', 'Caption', '|',
        'Align', 'Display', 'WrapText', '|',
        'Dimension', 'Replace', 'Remove'
    ],
    link: [
        'Open', 'Edit', 'Remove'
    ]
};

function App() {
    return (
        <RichTextEditorUIComponent
            toolbarSettings={toolbarSettings}
            quickToolbarSettings={quickToolbarSettings}
            placeholder='Type something...'
        />
    );
}

export default App;
```

> `actionId` is the routing key. `toolbarSettings.itemClicked` is fired for every toolbar item (main and quick) — branch on `args.item.actionId` to dispatch the right command or run your custom handler. The public top-level `itemClick` event you may see in older guides does not exist on `RichTextEditorUI`; the public surface is `toolbarSettings.itemClicked`.

Custom items are accepted by `image`, `link`, and `table` as well. The `link` and `image` arrays are unions that include `CustomToolbarItem`, so the same shape works in every quick-toolbar context.

## Available Quick Toolbar Items

The built-in quick-toolbar items are declared as typed unions. Each quick-toolbar surface accepts a different union — the union narrows the available identifiers per surface so the compiler flags unsupported items.

### Link Quick Toolbar Items

```ts
type LinkQuickToolbarItem = 'Open' | 'Copy' | 'Edit' | 'Remove' | '|' | CustomToolbarItem;
```

| Identifier | Behavior |
| --- | --- |
| `Open` | Opens the link `href` in a new tab using the configured `target`. |
| `Copy` | Copies the link `href` to the clipboard. |
| `Edit` | Opens the Insert-Link dialog pre-filled with the current link. |
| `Remove` | Removes the link while keeping the link text. |
| `\|` | Visual separator used to split action groups inside the popup. |
| `CustomToolbarItem` | A user-defined item routed through `toolbarSettings.itemClicked`. |

See [links.md](links.md) for the Link Quick Toolbar behaviour and customization.

### Table Quick Toolbar Items

```ts
type TableQuickToolbarItem = 'Row' | 'Column' | 'Header' | 'CellBackgroundColor' | 'VerticalAlign' | 'Align' | 'Remove' | '|' | CustomToolbarItem;
```

| Identifier | Behavior |
| --- | --- |
| `Row` | Opens the row sub-popup (insert above / below, delete row). |
| `Column` | Opens the column sub-popup (insert left / right, delete column). |
| `Header` | Toggles the table header row. |
| `CellBackgroundColor` | Opens the background-color picker for the active cell. |
| `VerticalAlign` | Opens the vertical-align picker (top / middle / bottom). |
| `Align` | Opens the horizontal-align picker (left / center / right). |
| `Remove` | Deletes the table. |
| `\|` | Visual separator. |
| `CustomToolbarItem` | A user-defined item routed through `toolbarSettings.itemClicked`. |

See [table.md](table.md) for the table operations behind these items.

### Image Quick Toolbar Items

```ts
type ImageQuickToolbarItem = 'AltText' | 'Caption' | 'Align' | 'Display' | 'WrapText' | 'Dimension' | 'Replace' | 'Remove' | '|' | CustomToolbarItem;
```

| Identifier | Behavior |
| --- | --- |
| `AltText` | Opens the Alt-Text editor for the selected image. |
| `Caption` | Toggles the image caption. |
| `Align` | Opens the alignment picker (left / center / right). |
| `Display` | Toggles the image display mode (`inline` / `block`). |
| `WrapText` | Toggles text-wrap around the image. |
| `Dimension` | Opens the dimension editor (width / height). |
| `Replace` | Opens the file picker to replace the image source. |
| `Remove` | Deletes the image. |
| `\|` | Visual separator. |
| `CustomToolbarItem` | A user-defined item routed through `toolbarSettings.itemClicked`. |

See [image.md](image.md) for the image quick toolbar in detail.

### Text Quick Toolbar Items

The Text Quick Toolbar accepts the full `ToolbarItem` union — every built-in toolbar identifier, built-in-with-config objects, custom items, and the `'|'` separator.

| Identifier | Behavior |
| --- | --- |
| `Bold`, `Italic`, `Underline`, `Strikethrough` | Toggle inline marks. |
| `Subscript`, `Superscript` | Toggle inline marks. |
| `UpperCase`, `LowerCase` | Transform the selection. |
| `InlineCode` | Toggle inline code mark. |
| `HorizontalLine` | Insert a horizontal rule at the cursor. |
| `Formats` | Open the paragraph / heading dropdown. |
| `FontSize`, `FontColor`, `BackgroundColor`, `FontName` | Open the matching picker. |
| `BulletList`, `NumberedList`, `NumberFormatList`, `BulletFormatList`, `Checklist` | List controls. |
| `Alignment` | Open the alignment picker. |
| `Quote`, `CodeBlock`, `Callout` | Block-format split buttons. |
| `Link`, `Image`, `Table` | Insert-link / image / table popups. |
| `ClearFormat` | Remove inline marks from the selection. |
| `Paragraph`, `Heading 1`–`Heading 4` | Block-format converters. |
| `Indent`, `Outdent` | Indent controls. |
| `AlignLeft`, `AlignCenter`, `AlignRight`, `AlignJustify` | Direct alignment controls. |
| `CalloutInfo`, `CalloutSuccess`, `CalloutWarning`, `CalloutError`, `CalloutNote` | Callout-variant children of the `Callout` split button. |
| `\|` | Visual separator. |

> Built-in identifiers can also be wrapped in a `BuiltInToolbarItemConfig` to override the visible label: `{ item: 'Bold', align: 'Right' }`.

---

## Block Format Toolbar Items

Block formats in the Modern Rich Text Editor change the block-level structure of the document rather than inline text. The editor exposes block formats through two public surfaces: built-in toolbar identifiers in `toolbarSettings.items`, and slash-command entries in `slashCommandSettings.items`. There is no separate public `format` configuration object — every block format is wired through one of those two arrays.

Use the built-in toolbar identifiers from `BuiltInToolbarItem` to add block formats to the editor toolbar. The following example adds `Quote`, `CodeBlock`, `HorizontalLine`, and the `Callout` split-button alongside the default formatting controls.

```tsx
import * as React from 'react';
import { RichTextEditorUIComponent } from '@syncfusion/ej2-react-richtexteditor-ui';

const toolbarSettings = {
    items: [
        'Bold', 'Italic', 'Underline', 'Strikethrough', '|',
        'Formats', 'Alignment', 'BulletList', 'NumberedList', '|',
        'Quote', 'CodeBlock', 'HorizontalLine', 'Callout', '|',
        'Undo', 'Redo'
    ]
};

function App() {
    return (
        <RichTextEditorUIComponent
            toolbarSettings={toolbarSettings}
            placeholder='Type something...'
        />
    );
}

export default App;
```

The editor recognizes the following block-level formats. Each entry lists the toolbar identifier (where one exists) and the slash-command identifier (where one exists).

### Paragraph and Headings

- **Paragraph** — toolbar `Paragraph`, slash command `Paragraph`, command `paragraph`.
- **Heading 1–4** — toolbar `Heading 1` … `Heading 4`, slash commands `Heading 1` … `Heading 4`, commands `heading1` … `heading4`.

Headings and paragraph can also be reached through the `Formats` dropdown toolbar item, which exposes `Paragraph` and `Heading 1`–`Heading 4` as a single control.

### Horizontal Line

Inserts an `<hr>` element at the cursor. Toolbar `HorizontalLine`, command `horizontalRule`.

### Dedicated Block Format Pages

- **Block quote** — see [block-quote.md](block-quote.md).
- **Code block** — see [codeblock.md](codeblock.md).
- **Callouts** — see [callout.md](callout.md).
- **Collapsible headings** — see [collapsible.md](collapsible.md).

---

## Keyboard Shortcuts for Insertions and Blocks

| Action | Windows / Linux | macOS | Description |
| :--- | :--- | :--- | :--- |
| **Insert Link** | <kbd>Ctrl</kbd> + <kbd>K</kbd> | <kbd>⌘</kbd> + <kbd>K</kbd> | Opens the dialog to insert or edit a hyperlink. |
| **Insert Image** | <kbd>Ctrl</kbd> + <kbd>Shift</kbd> + <kbd>I</kbd> | <kbd>⌘</kbd> + <kbd>Shift</kbd> + <kbd>I</kbd> | Opens the image upload dialog. |
| **Insert Table** | <kbd>Ctrl</kbd> + <kbd>Shift</kbd> + <kbd>E</kbd> | <kbd>⌘</kbd> + <kbd>Shift</kbd> + <kbd>E</kbd> | Opens the table insertion grid/dialog. |
| **Code Block** | <kbd>Ctrl</kbd> + <kbd>Shift</kbd> + <kbd>B</kbd> | <kbd>⌘</kbd> + <kbd>Shift</kbd> + <kbd>B</kbd> | Converts the block into a preformatted code snippet. |

## Keyboard Shortcuts for History and Focus Navigation

| Action | Windows / Linux | macOS | Description |
| :--- | :--- | :--- | :--- |
| **Undo** | <kbd>Ctrl</kbd> + <kbd>Z</kbd> | <kbd>⌘</kbd> + <kbd>Z</kbd> | Reverts the previous editing action. |
| **Redo** | <kbd>Ctrl</kbd> + <kbd>Shift</kbd> + <kbd>Z</kbd> | <kbd>⌘</kbd> + <kbd>Shift</kbd> + <kbd>Z</kbd> | Reapplies the previously undone action. |
| **Toolbar Focus** | <kbd>Alt</kbd> + <kbd>F10</kbd> | <kbd>Option</kbd> + <kbd>F10</kbd> | Shifts focus directly to the first active toolbar item. |
| **Escape** | <kbd>Esc</kbd> | <kbd>Esc</kbd> | Closes open dropdowns, popups, or dialog windows. |

## Configuring Keyboard Shortcuts

You can override default shortcut key combinations or assign custom combinations using the `keyBindings` property on `toolbarSettings`.

### Syntax Rules

- Modifiers and key values are separated with a `+` symbol (e.g., `'ctrl+alt+k'`).
- Supported modifier tokens: `ctrl`, `shift`, `alt`, and `meta`.
- Key values can be written in lowercase (e.g., `'ctrl+b'`).
- The Modern Rich Text Editor automatically updates corresponding toolbar button tooltips and accessibility attributes to match your custom key bindings.

### Configuration Example

Use the keyBindings property to customize or override the default keyboard shortcuts. In the following example, the built-in shortcuts for link insertion, code blocks, and image insertion are remapped to custom key combinations.

```tsx
import * as React from 'react';
import { RichTextEditorUIComponent } from '@syncfusion/ej2-react-richtexteditor-ui';

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

function App() {
    return (
        <RichTextEditorUIComponent
            toolbarSettings={toolbarSettings}
        />
    );
}

export default App;
```

## API Reference


## UpdatedToolbarStatusEventArgs

Event arguments for the `updatedToolbarStatus` event raised after the toolbar synchronizes its visual state (active buttons, dropdown values, color pickers) with the current editor selection / cursor formatting.

### Properties

| Property | Type | Description |
| --- | --- | --- |
| `activeMarks` | `Object` | Snapshot of the active marks at the current selection. |
| `blockFormats` | `Object` | Snapshot of block-level formats at the current selection. |
| `styles` | `Object` | Resolved font / color values at the current selection. |

## ShowToolbarItem

Shows a toolbar item by ID.

### Properties

| Property | Type | Description |
| --- | --- | --- |
| `itemId` | `string` | Stable ID of the item to show |

## RemoveToolbarItem

Removes a toolbar item by ID.

### Properties

| Property | Type | Description |
| --- | --- | --- |
| `index` | `number` | Index of the item to remove |
| `itemId` | `string` | Stable ID of the item to remove |

## EnableToolbarItem

**Toolbar Command** — Enables a toolbar item by ID.

### Properties

#### `itemId` (string)
Stable ID of the item to enable

---

## DisableToolbarItem

**Toolbar Command** — Disables a toolbar item by ID.

### Properties

#### `itemId` (string)
Stable ID of the item to disable

---

## CustomToolbarItem

**Toolbar Item Type** — Custom toolbar item with user-defined action. Extends the standard toolbar configuration with additional customization options.

### Properties

#### `actionId` (string)
Unique identifier for the custom toolbar item. Used to distinguish items and handle itemClick events.

#### `align` (ItemAlign)
Specifies the location for aligning Toolbar items on the Toolbar. Each command will be aligned according to the `align` property. Possible values:
- `Left`: To align commands to the left side of the Toolbar.
- `Center`: To align commands at the center of the Toolbar.
- `Right`: To align commands to the right side of the Toolbar.

#### `cssClass` (string)
Defines single/multiple classes (separated by space) to be used for customization of commands.

#### `disabled` (boolean)
Specifies whether an item should be disabled or not.

#### `htmlAttributes` ({ [key: string]: string })
Defines htmlAttributes used to add custom attributes to Toolbar command. Supports HTML attributes such as style, class, etc.

#### `id` (string)
Specifies the unique ID to be used with button or input element of Toolbar items.

#### `macShortcutText` (string)
Keyboard shortcut tooltip for macOS.

#### `overflow` (OverflowOption)
Specifies the Toolbar command display area when an element's content is too large to fit available space. This is applicable only to `popup` mode. Possible values:
- `Show`: Always shows the item as the primary priority on the Toolbar.
- `Hide`: Always shows the item as the secondary priority on the popup.
- `None`: No priority for display, and as per normal order moves to popup when content exceeds.

#### `prefixIcon` (string)
Defines single/multiple classes separated by space used to specify an icon for the button. The icon will be positioned before the text content if text is available, otherwise the icon alone will be rendered.

#### `showAlwaysInPopup` (boolean)
Defines the priority of items to display it in popup always. It allows to maintain toolbar item on popup always but it does not work for toolbar priority items.

#### `showTextOn` (DisplayMode)
Specifies where the button text will be displayed on popup mode of the Toolbar. Possible values:
- `Toolbar`: Text will be displayed on Toolbar only.
- `Overflow`: Text will be displayed only when content overflows to popup.
- `Both`: Text will be displayed on popup and Toolbar.

#### `suffixIcon` (string)
Defines single/multiple classes separated by space used to specify an icon for the button. The icon will be positioned after the text content if text is available.

#### `tabIndex` (number)
Specifies the tab order of the Toolbar items. When positive values assigned, it allows to switch focus to the next/previous toolbar items with Tab/ShiftTab keys. By default, user can able to switch between items only via arrow keys. If the value is set to 0 for all tool bar items, then tab switches based on element order.

#### `template` (string|function|JSX.Element)
Specifies the HTML element/element ID as a string that can be added as a Toolbar command. E.g - `items: [{ template: '<input placeholder="Search"/>' },{ template: '#checkbox1' }]`

#### `text` (string)
Specifies the text to be displayed on the Toolbar button.

#### `tooltipText` (string)
Specifies the text to be displayed on hovering the Toolbar button.

#### `type` (ItemType)
Specifies the types of command to be rendered in the Toolbar. Supported types are:
- `Button`: Creates the Button control with its given properties like text, prefixIcon, etc.
- `Separator`: Adds a horizontal line that separates the Toolbar commands.
- `Input`: Creates an input element that is applicable to template rendering with Syncfusion controls like DropDownList, AutoComplete, etc.

#### `visible` (boolean)
Specifies whether an item should be hidden or not.

#### `width` (number|string)
Specifies the width of the Toolbar button commands.

#### `windowsShortcutText` (string)
Keyboard shortcut tooltip for Windows.

### Events

#### `click` (EmitType<ClickEventArgs>)
Event triggers when `click` the toolbar item.

---

## AddToolbarItem

**Operation Class** — Adds a toolbar item at a specified index.

### Properties

#### `index` (number)
Index at which to insert the item

#### `item` (ToolbarItem)
The item to add

## BuiltInToolbarItem

**Enumeration** — Built-in toolbar item names for common editor commands.

- `Undo`, `Redo`
- `Bold`, `Italic`, `Underline`, `Strikethrough`, `Subscript`, `Superscript`
- `UpperCase`, `LowerCase`
- `HorizontalLine`, `InlineCode`
- `Formats`, `FontSize`, `FontColor`, `BackgroundColor`, `FontName`
- `BulletList`, `NumberedList`, `NumberFormatList`, `BulletFormatList`, `Checklist`
- `Alignment` (with directional variants: `AlignLeft`, `AlignCenter`, `AlignRight`, `AlignJustify`)
- `Quote`, `CodeBlock`, `Callout`
- `Link`, `Image`, `Table`, `ClearFormat`
- Heading levels: `Heading 1`, `Heading 2`, `Heading 3`, `Heading 4`, `Paragraph`
- `Indent`, `Outdent`
- Callout variants: `CalloutInfo`, `CalloutSuccess`, `CalloutWarning`, `CalloutError`, `CalloutNote`

---

## BuiltInToolbarItemConfig

**Interface** — Configuration for a built-in toolbar item. Helps align built-in items with ItemModel properties.

### Properties

#### `align` (ItemAlign)
Specifies alignment: `Left`, `Center`, or `Right`

#### `cssClass` (string)
Single/multiple classes (space-separated) for customization

#### `disabled` (boolean)
Specifies whether the item should be disabled

#### `htmlAttributes` ({ [key: string]: string })
Custom HTML attributes (style, class, etc.)

#### `id` (string)
Unique ID for the button or input element

#### `item` (BuiltInToolbarItem)
The built-in toolbar item name

#### `overflow` (OverflowOption)
Display area when content overflows: `Show`, `Hide`, or `None`

#### `prefixIcon` (string)
Icon classes positioned before the text

#### `showAlwaysInPopup` (boolean)
Priority for popup display

#### `showTextOn` (DisplayMode)
Where button text displays: `Toolbar`, `Overflow`, or `Both`

#### `suffixIcon` (string)
Icon classes positioned after the text

#### `tabIndex` (number)
Tab order of toolbar items

#### `template` (string | function | JSX.Element)
HTML element or element ID to add as a command

#### `text` (string)
Text displayed on the button

#### `tooltipText` (string)
Text displayed on hover

#### `type` (ItemType)
Command type: `Button`, `Separator`, or `Input`

#### `visible` (boolean)
Whether item should be hidden

#### `width` (number | string)
Width of the button command

### Events

#### `click` (EmitType<ClickEventArgs>)
Event triggers when the toolbar item is clicked

---

## QuickToolbarSettingsModel

Interface for a class `QuickToolbarSettings`.

### Properties

| Property | Type | Description |
| --- | --- | --- |
| `enable` | `boolean` | Specifies whether the quick toolbar is enabled in the RichTextEditor. |
| `enableAppendToBody` | `boolean` | Specifies whether to append the quick toolbar popup to the document body instead of the editor container. Enable this to avoid clipping in containers with constrained width or overflow. Disable it to keep the popup positioned within the editor container. |
| `image` | `ImageQuickToolbarItem[]` | Specifies the toolbar items shown when an image is selected. |
| `link` | `LinkQuickToolbarItem[]` | Specifies the toolbar items shown when the cursor is inside a link. If no items are configured, the link quick toolbar is not initialized. Default order: Open, Copy, Edit, Remove. |
| `table` | `TableQuickToolbarItem[]` | Items displayed in the Table Quick Toolbar. |
| `text` | `ToolbarItem[]` | Specifies the toolbar items shown when text is selected. If no items are configured, the text quick toolbar is not rendered. |

## TableQuickToolbarItem

- `Row`
- `Column`
- `Header`
- `CellBackgroundColor`
- `VerticalAlign`
- `Align`
- `Remove`
- (separator entry — the source page renders this entry without a value)
- `CustomToolbarItem`

## TableSettingsModel

Interface for a class `TableSettings`.

### Properties

| Property | Type | Description |
| --- | --- | --- |
| `resize` | `boolean` | Enables table resize drag handles when new tables are inserted into the editor. |

## TableSettings

Configures the table insertion behavior of the Rich Text Editor.

### Properties

| Property | Type | Default | Description |
| --- | --- | --- | --- |
| `resize` | `boolean` | `true` | Enables table resize drag handles when new tables are inserted into the editor. |

## ToggleCommandBuilder

Base builder for toggle commands (bold, italic, etc.).

### Methods

#### apply

Executes the toggle command.

Returns `void`.

## API Reference

## ToolbarSettings

Configures the Toolbar settings of the RichTextEditor.

### Properties

| Property | Type | Default | Description |
| --- | --- | --- | --- |
| `enable` | `boolean` | `true` | Specifies whether to render the Toolbar in the Editor. |
| `enableFloating` | `boolean` | `true` | Whether the toolbar remains visible while its scroll owner is scrolled. |
| `floatingOffset` | `number` | `0` | Non-negative vertical offset, in pixels, applied while floating. |
| `items` | `ToolbarItem[]` | `['Bold', 'Italic', 'Underline', 'Strikethrough', '\|', 'Formats', 'Alignment', 'BulletList', 'NumberedList', '\|', 'Link', 'Image', 'Table', '\|', 'Undo', 'Redo']` | Defines the items displayed in the toolbar. |
| `position` | `ToolbarPosition` | `'Top'` | Defines whether the toolbar is rendered above or below the editor. |
| `type` | `ToolbarType` | `'Auto'` | Defines the toolbar overflow and layout behavior. |

### Events

| Event | Type | Description |
| --- | --- | --- |
| `itemClicked` | `EmitType<ToolbarItemClickedEventArgs>` | Event raised when a toolbar item is clicked in the component. |
| `updatedToolbarStatus` | `EmitType<UpdatedToolbarStatusEventArgs>` | Raised after the toolbar item status (active states, dropdown values, color picker selections) has been synchronized with the current editor selection. This event fires after formatting states are updated to reflect the user's current cursor position or selection in the editor. |
## ToolbarSettingsModel

Interface for a class `ToolbarSettings`.

### Properties

| Property | Type | Description |
| --- | --- | --- |
| `enable` | `boolean` | Specifies whether to render the Toolbar in the Editor. |
| `enableFloating` | `boolean` | Whether the toolbar remains visible while its scroll owner is scrolled. |
| `floatingOffset` | `number` | Non-negative vertical offset, in pixels, applied while floating. |
| `items` | `ToolbarItem[]` | Defines the items displayed in the toolbar. |
| `position` | `ToolbarPosition` | Defines whether the toolbar is rendered above or below the editor. |
| `type` | `ToolbarType` | Defines the toolbar overflow and layout behavior. |

### Events

| Event | Type | Description |
| --- | --- | --- |
| `itemClicked` | `EmitType<ToolbarItemClickedEventArgs>` | Event raised when a toolbar item is clicked in the component. |
| `updatedToolbarStatus` | `EmitType<UpdatedToolbarStatusEventArgs>` | Raised after the toolbar item status (active states, dropdown values, color picker selections) has been synchronized with the current editor selection. This event fires after formatting states are updated to reflect the user's current cursor position or selection in the editor. |
## ToolbarType

Defines the toolbar overflow and layout behavior.

- `Auto`
- `Expanded`
- `MultiRow`
- `Scrollable`
## ToolbarPosition

Defines the toolbar vertical position relative to editor.

- `Top`
- `Bottom`
## ToolbarItem

Union type for toolbar items. Accepts built-in items by name, custom configurations, custom items, or separators.

- `BuiltInToolbarItem`
- `BuiltInToolbarItemConfig`
- `CustomToolbarItem`
- (separator entry — the source page renders this entry without a value)
## ToolbarItemUpdate

Discriminated union for toolbar item update operations. Used during reconciliation of items array changes and direct toolbar updates.

- `AddToolbarItem`
- `RemoveToolbarItem`
- `ShowToolbarItem`
- `HideToolbarItem`
- `EnableToolbarItem`
- `DisableToolbarItem`
## ToolbarItemClickedEventArgs

Represents the event arguments for a toolbar item click event in the component.

### Properties

| Property | Type | Description |
| --- | --- | --- |
| `dataIndex` | `number` | Specifies the index of the message data associated with the toolbar item click event. This property is not applicable for header toolbar item click. |
| `event` | `Event` | Specifies the event object associated with the toolbar item click. Represents the underlying event that triggered the click action, providing details about the event. |
| `item` | `ToolbarItemModel` | Specifies the toolbar item that was clicked. Represents the model of the toolbar item that triggered the click event. |
| `name` | `string` | Specifies name of the event. |
## UpdatedToolbarStatusEventArgs

Event arguments for the `updatedToolbarStatus` event raised after the toolbar synchronizes its visual state (active buttons, dropdown values, color pickers) with the current editor selection / cursor formatting.

### Properties

| Property | Type | Description |
| --- | --- | --- |
| `activeMarks` | `Object` | Snapshot of the active marks at the current selection. |
| `blockFormats` | `Object` | Snapshot of block-level formats at the current selection. |
| `styles` | `Object` | Resolved font / color values at the current selection. |
## ShowToolbarItem

Shows a toolbar item by ID.

### Properties

| Property | Type | Description |
| --- | --- | --- |
| `itemId` | `string` | Stable ID of the item to show |
## RemoveToolbarItem

Removes a toolbar item by ID.

### Properties

| Property | Type | Description |
| --- | --- | --- |
| `index` | `number` | Index of the item to remove |
| `itemId` | `string` | Stable ID of the item to remove |
## EnableToolbarItem

**Toolbar Command** — Enables a toolbar item by ID.

### Properties

#### `itemId` (string)
Stable ID of the item to enable

---
## DisableToolbarItem

**Toolbar Command** — Disables a toolbar item by ID.

### Properties

#### `itemId` (string)
Stable ID of the item to disable

---
## CustomToolbarItem

**Toolbar Item Type** — Custom toolbar item with user-defined action. Extends the standard toolbar configuration with additional customization options.

### Properties

#### `actionId` (string)
Unique identifier for the custom toolbar item. Used to distinguish items and handle itemClick events.

#### `align` (ItemAlign)
Specifies the location for aligning Toolbar items on the Toolbar. Each command will be aligned according to the `align` property. Possible values:
- `Left`: To align commands to the left side of the Toolbar.
- `Center`: To align commands at the center of the Toolbar.
- `Right`: To align commands to the right side of the Toolbar.

#### `cssClass` (string)
Defines single/multiple classes (separated by space) to be used for customization of commands.

#### `disabled` (boolean)
Specifies whether an item should be disabled or not.

#### `htmlAttributes` ({ [key: string]: string })
Defines htmlAttributes used to add custom attributes to Toolbar command. Supports HTML attributes such as style, class, etc.

#### `id` (string)
Specifies the unique ID to be used with button or input element of Toolbar items.

#### `macShortcutText` (string)
Keyboard shortcut tooltip for macOS.

#### `overflow` (OverflowOption)
Specifies the Toolbar command display area when an element's content is too large to fit available space. This is applicable only to `popup` mode. Possible values:
- `Show`: Always shows the item as the primary priority on the Toolbar.
- `Hide`: Always shows the item as the secondary priority on the popup.
- `None`: No priority for display, and as per normal order moves to popup when content exceeds.

#### `prefixIcon` (string)
Defines single/multiple classes separated by space used to specify an icon for the button. The icon will be positioned before the text content if text is available, otherwise the icon alone will be rendered.

#### `showAlwaysInPopup` (boolean)
Defines the priority of items to display it in popup always. It allows to maintain toolbar item on popup always but it does not work for toolbar priority items.

#### `showTextOn` (DisplayMode)
Specifies where the button text will be displayed on popup mode of the Toolbar. Possible values:
- `Toolbar`: Text will be displayed on Toolbar only.
- `Overflow`: Text will be displayed only when content overflows to popup.
- `Both`: Text will be displayed on popup and Toolbar.

#### `suffixIcon` (string)
Defines single/multiple classes separated by space used to specify an icon for the button. The icon will be positioned after the text content if text is available.

#### `tabIndex` (number)
Specifies the tab order of the Toolbar items. When positive values assigned, it allows to switch focus to the next/previous toolbar items with Tab/ShiftTab keys. By default, user can able to switch between items only via arrow keys. If the value is set to 0 for all tool bar items, then tab switches based on element order.

#### `template` (string|function|JSX.Element)
Specifies the HTML element/element ID as a string that can be added as a Toolbar command. E.g - `items: [{ template: '<input placeholder="Search"/>' },{ template: '#checkbox1' }]`

#### `text` (string)
Specifies the text to be displayed on the Toolbar button.

#### `tooltipText` (string)
Specifies the text to be displayed on hovering the Toolbar button.

#### `type` (ItemType)
Specifies the types of command to be rendered in the Toolbar. Supported types are:
- `Button`: Creates the Button control with its given properties like text, prefixIcon, etc.
- `Separator`: Adds a horizontal line that separates the Toolbar commands.
- `Input`: Creates an input element that is applicable to template rendering with Syncfusion controls like DropDownList, AutoComplete, etc.

#### `visible` (boolean)
Specifies whether an item should be hidden or not.

#### `width` (number|string)
Specifies the width of the Toolbar button commands.

#### `windowsShortcutText` (string)
Keyboard shortcut tooltip for Windows.

### Events

#### `click` (EmitType<ClickEventArgs>)
Event triggers when `click` the toolbar item.

---
## AddToolbarItem

**Operation Class** — Adds a toolbar item at a specified index.

### Properties

#### `index` (number)
Index at which to insert the item

#### `item` (ToolbarItem)
The item to add
## BuiltInToolbarItem

**Enumeration** — Built-in toolbar item names for common editor commands.

- `Undo`, `Redo`
- `Bold`, `Italic`, `Underline`, `Strikethrough`, `Subscript`, `Superscript`
- `UpperCase`, `LowerCase`
- `HorizontalLine`, `InlineCode`
- `Formats`, `FontSize`, `FontColor`, `BackgroundColor`, `FontName`
- `BulletList`, `NumberedList`, `NumberFormatList`, `BulletFormatList`, `Checklist`
- `Alignment` (with directional variants: `AlignLeft`, `AlignCenter`, `AlignRight`, `AlignJustify`)
- `Quote`, `CodeBlock`, `Callout`
- `Link`, `Image`, `Table`, `ClearFormat`
- Heading levels: `Heading 1`, `Heading 2`, `Heading 3`, `Heading 4`, `Paragraph`
- `Indent`, `Outdent`
- Callout variants: `CalloutInfo`, `CalloutSuccess`, `CalloutWarning`, `CalloutError`, `CalloutNote`

---
## BuiltInToolbarItemConfig

**Interface** — Configuration for a built-in toolbar item. Helps align built-in items with ItemModel properties.

### Properties

#### `align` (ItemAlign)
Specifies alignment: `Left`, `Center`, or `Right`

#### `cssClass` (string)
Single/multiple classes (space-separated) for customization

#### `disabled` (boolean)
Specifies whether the item should be disabled

#### `htmlAttributes` ({ [key: string]: string })
Custom HTML attributes (style, class, etc.)

#### `id` (string)
Unique ID for the button or input element

#### `item` (BuiltInToolbarItem)
The built-in toolbar item name

#### `overflow` (OverflowOption)
Display area when content overflows: `Show`, `Hide`, or `None`

#### `prefixIcon` (string)
Icon classes positioned before the text

#### `showAlwaysInPopup` (boolean)
Priority for popup display

#### `showTextOn` (DisplayMode)
Where button text displays: `Toolbar`, `Overflow`, or `Both`

#### `suffixIcon` (string)
Icon classes positioned after the text

#### `tabIndex` (number)
Tab order of toolbar items

#### `template` (string | function | JSX.Element)
HTML element or element ID to add as a command

#### `text` (string)
Text displayed on the button

#### `tooltipText` (string)
Text displayed on hover

#### `type` (ItemType)
Command type: `Button`, `Separator`, or `Input`

#### `visible` (boolean)
Whether item should be hidden

#### `width` (number | string)
Width of the button command

### Events

#### `click` (EmitType<ClickEventArgs>)
Event triggers when the toolbar item is clicked

---
## QuickToolbarSettingsModel

Interface for a class `QuickToolbarSettings`.

### Properties

| Property | Type | Description |
| --- | --- | --- |
| `enable` | `boolean` | Specifies whether the quick toolbar is enabled in the RichTextEditor. |
| `enableAppendToBody` | `boolean` | Specifies whether to append the quick toolbar popup to the document body instead of the editor container. Enable this to avoid clipping in containers with constrained width or overflow. Disable it to keep the popup positioned within the editor container. |
| `image` | `ImageQuickToolbarItem[]` | Specifies the toolbar items shown when an image is selected. |
| `link` | `LinkQuickToolbarItem[]` | Specifies the toolbar items shown when the cursor is inside a link. If no items are configured, the link quick toolbar is not initialized. Default order: Open, Copy, Edit, Remove. |
| `table` | `TableQuickToolbarItem[]` | Items displayed in the Table Quick Toolbar. |
| `text` | `ToolbarItem[]` | Specifies the toolbar items shown when text is selected. If no items are configured, the text quick toolbar is not rendered. |
