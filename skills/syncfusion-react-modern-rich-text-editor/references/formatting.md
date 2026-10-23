# Inline Formatting in React Modern Rich Text Editor

## Table of Contents
- [Available Inline Formats](#available-inline-formats)
- [Toggle Text Styles](#toggle-text-styles)
- [Font and Styling](#font-and-styling)
- [Clear All Inline Formatting](#clear-all-inline-formatting)
- [Commands](#commands)
- [Fluent Builder](#fluent-builder)
- [Color Commands](#color-commands)
- [Typography Commands](#typography-commands)
- [Programmatic End-to-End Example](#programmatic-end-to-end-example)
- [Font Family Configuration](#font-family-configuration)
- [Built-in Font Family](#built-in-font-family)
- [Custom Font Family](#custom-font-family)
- [Google Font Support](#google-font-support)
- [Font Size Configuration](#font-size-configuration)
- [Built-in Font Size](#built-in-font-size)
- [Custom Font Size](#custom-font-size)
- [Font and Background Color Configuration](#font-and-background-color-configuration)
- [Custom Font and Background Colors](#custom-font-and-background-colors)
- [Keyboard Shortcuts for Text Formatting and Styles](#keyboard-shortcuts-for-text-formatting-and-styles)

The Modern Rich Text Editor ships a curated set of inline formats that apply character-level marks to the selected text without changing the surrounding block. The toolbar palette exposes these formats through built-in items, and the same formats are reachable from code through the fluent `commands()` builder.

Every inline format is exposed through two equivalent surfaces — a toolbar click and the fluent `editorRef.current?.commands().<builder>().<setter>().apply()` chain. Both routes converge on the same `actionBegin` / `actionComplete` pipeline and share identical cancellation and history semantics.

- [API Reference](#api-reference)
## Available Inline Formats

The table below lists the inline formats available in the Modern Rich Text Editor's toolbar.

| Name | Summary | Initialization |
|----------------|---------|------------------------------------------|
| Bold | Makes the selected text thicker and darker. | toolbarSettings: { items: ['Bold'] } |
| Italic | Slants the selected text to the right. | toolbarSettings: { items: ['Italic'] } |
| Underline | Adds a line beneath the selected text. | toolbarSettings: { items: ['Underline'] } |
| Strikethrough | Applies a line through the selected text. | toolbarSettings: { items: ['Strikethrough'] } |
| Subscript | Positions the selected text slightly below the normal line. | toolbarSettings: { items: ['Subscript'] } |
| Superscript | Positions the selected text slightly above the normal line. | toolbarSettings: { items: ['Superscript'] } |
| Lowercase | Converts the selected text to lowercase. | toolbarSettings: { items: ['Lowercase'] } |
| Uppercase | Converts the selected text to uppercase. | toolbarSettings: { items: ['Uppercase'] } |
| InlineCode | Formats the selected text as inline code. | toolbarSettings: { items: ['InlineCode'] } |
| FontName | Defines the font family applied to the selected text. | toolbarSettings: { items: ['FontName'] } |
| FontSize | Defines the font size applied to the selected text. | toolbarSettings: { items: ['FontSize'] } |
| FontColor | Applies a foreground color to the selected text. | toolbarSettings: { items: ['FontColor'] } |
| BackgroundColor | Applies a highlight (background) color to the selected text. | toolbarSettings: { items: ['BackgroundColor'] } |
| ClearFormat | Removes all inline formatting from the selected text. | toolbarSettings: { items: ['ClearFormat'] } |

```tsx
import * as React from 'react';
import { RichTextEditorUIComponent } from '@syncfusion/ej2-react-richtexteditor-ui';

const toolbarSettings = {
    items: [
        'FontColor', 'BackgroundColor'
    ]
};

const fontColor = {
    default: '#DC2626',
    mode: 'Palette',
    columns: 5,
    modeSwitcher: true
} as any;

const backgroundColor = {
    default: '#FFF7C7',
    mode: 'Picker',
    columns: 5,
    modeSwitcher: false
} as any;

function App() {
    return (
        <RichTextEditorUIComponent
            toolbarSettings={toolbarSettings}
            placeholder='Type something ...'
            fontColor={fontColor}
            backgroundColor={backgroundColor}
        />
    );
}

export default App;
```

## Toggle Text Styles

The toggle styles include `Bold`, `Italic`, `Underline`, `Strikethrough`, `Subscript`, `Superscript`, `Lowercase`, `Uppercase`, and `InlineCode`. They carry no value payload — clicking the toolbar item, or invoking the command without arguments, flips the mark for the selection or sets the active typing mark for a collapsed caret.

## Font and Styling

The `FontName`, `FontSize`, `FontColor`, and `BackgroundColor` toolbar items are bound to a `FontName` dropdown, a `FontSize` dropdown, and the foreground and background Color Picker controls respectively. Each accepts a typed payload and the empty string removes the corresponding mark from the selection. For property-by-property configuration, see [Font Family Configuration](#font-family-configuration), [Font Size Configuration](#font-size-configuration), and [Font and Background Color Configuration](#font-and-background-color-configuration).

## Clear All Inline Formatting

The `ClearFormat` toolbar item strips every inline mark (bold, italic, underline, strikethrough, subscript, superscript, code, color, highlight, font size, and font family) from the current selection without altering the block structure. For a programmatic equivalent, use `clearFormat` from [Commands](#commands).

---

## Commands

## Fluent Builder

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

Toggle commands carry no payload. They flip the mark for the selection or set the active typing mark for a collapsed caret.

## Color Commands

Both color commands share the `ColorCommand { color: string }` payload. An empty string removes the corresponding mark from the selection.

### Apply a Color

```ts
editorRef.current?.commands().fontColor().color('#DC2626').apply();
editorRef.current?.commands().backgroundColor().color('rgba(255, 247, 199, 1)').apply();
```

### Remove a Color

```ts
editorRef.current?.commands().fontColor().color('').apply();
editorRef.current?.commands().backgroundColor().color('').apply();
```

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

## Programmatic End-to-End Example

```tsx
import * as React from 'react';
import { RichTextEditorUIComponent } from '@syncfusion/ej2-react-richtexteditor-ui';

const toolbarSettings = {
    items: [
        'Bold', 'Italic', 'Underline', 'Strikethrough',
        '|',
        'FontSize', 'FontName', 'FontColor', 'BackgroundColor',
        '|',
        'InlineCode', 'ClearFormat'
    ]
};

function App() {
    const editorRef = React.useRef<RichTextEditorUIComponent>(null);

    React.useEffect(() => {
        const editor = editorRef.current;
        if (!editor) return;

        const boldBtn = document.getElementById('boldBtn');
        const underlineBtn = document.getElementById('underlineBtn');
        const fontSizeBtn = document.getElementById('fontSizeBtn');
        const fontColorBtn = document.getElementById('fontColorBtn');
        const highlightBtn = document.getElementById('highlightBtn');
        const clearFormatBtn = document.getElementById('clearFormatBtn');

        const handleBold = () => {
            (editor as any).commands().bold().apply();
        };

        const handleUnderline = () => {
            (editor as any).commands().underline().apply();
        };

        const handleFontSize = () => {
            (editor as any).commands().fontSize().size('18px').apply();
        };

        const handleFontColor = () => {
            (editor as any).commands().fontColor().color('#00A3FF').apply();
        };

        const handleHighlight = () => {
            (editor as any).commands().backgroundColor().color('#FFF7C7').apply();
        };

        const handleClearFormat = () => {
            (editor as any).commands().clearFormat().apply();
        };

        boldBtn?.addEventListener('click', handleBold);
        underlineBtn?.addEventListener('click', handleUnderline);
        fontSizeBtn?.addEventListener('click', handleFontSize);
        fontColorBtn?.addEventListener('click', handleFontColor);
        highlightBtn?.addEventListener('click', handleHighlight);
        clearFormatBtn?.addEventListener('click', handleClearFormat);

        return () => {
            boldBtn?.removeEventListener('click', handleBold);
            underlineBtn?.removeEventListener('click', handleUnderline);
            fontSizeBtn?.removeEventListener('click', handleFontSize);
            fontColorBtn?.removeEventListener('click', handleFontColor);
            highlightBtn?.removeEventListener('click', handleHighlight);
            clearFormatBtn?.removeEventListener('click', handleClearFormat);
        };
    }, []);

    return (
        <RichTextEditorUIComponent
            ref={editorRef}
            toolbarSettings={toolbarSettings}
            placeholder='Type something ...'
        />
    );
}

export default App;
```

---

## Font Family Configuration

The Modern Rich Text Editor exposes the configuration surface for inline formats through the `fontSize`, `fontFamily`, `fontColor`, and `backgroundColor` settings models. These options control which values appear in the toolbar dropdowns and Color Pickers and how those controls are rendered.

### Built-in Font Family

The Rich Text Editor initializes with a default font family, which inherits the font family of the parent element. You can change the font for selected text using the font family dropdown in the toolbar. When the default font family is selected, the toolbar displays `Font Name`. For other font families, the toolbar shows the name of the selected font.

You can add the `FontName` tool to the Modern Rich Text Editor toolbar using the `toolbarSettings` [items](https://ej2.syncfusion.com/documentation/api/richtexteditor-ui/toolbarSettings#items) property.

```tsx
import * as React from 'react';
import { RichTextEditorUIComponent } from '@syncfusion/ej2-react-richtexteditor-ui';

const toolbarSettings = {
    items: ['FontName']
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

### Custom Font Family

The Modern Rich Text Editor supports providing custom fonts along with the existing list. To add additional font names to the font dropdown, configure the `items` field of the [fontFamily](https://ej2.syncfusion.com/documentation/api/richtexteditor-ui/fontFamily#items) property.

```tsx
import * as React from 'react';
import { RichTextEditorUIComponent } from '@syncfusion/ej2-react-richtexteditor-ui';

const toolbarSettings = {
    items: ['FontName']
};

const fontFamily = {
    items: [
        { text: 'Default', value: 'Default' },
        { text: 'Segoe UI', value: 'Segoe UI, sans-serif' },
        { text: 'Roboto', value: 'Roboto, sans-serif' },
        { text: 'Georgia', value: 'Georgia, serif' },
        { text: 'Courier New', value: 'Courier New, monospace' }
    ]
};

function App() {
    return (
        <RichTextEditorUIComponent
            value='<p>Customize the font family using the FontName dropdown in the toolbar.</p>'
            valueFormat='html'
            placeholder='Type something...'
            toolbarSettings={toolbarSettings}
            fontFamily={fontFamily}
        />
    );
}

export default App;
```

### Google Font Support

To use web fonts in the Modern Rich Text Editor, the web fonts do not need to be present on the local machine. To add web fonts to the editor, refer the web font links in your page and add the font names to the [fontFamily](https://ej2.syncfusion.com/documentation/api/richtexteditor-ui/fontFamily#items) property.

```tsx
import { RichTextEditorUIComponent } from '@syncfusion/ej2-react-richtexteditor-ui';
import React, { useRef } from 'react';

function App() {
    const editorRef = useRef<RichTextEditorUIComponent>(null);

    return (
        <RichTextEditorUIComponent
            ref={editorRef}
            toolbarSettings={{
                items: ['FontName']
            }}
            fontFamily={{
                items: [
                    { text: 'Default', value: 'Default' },
                    { text: 'Roboto', value: 'Roboto, sans-serif' },
                    { text: 'Great Vibes', value: '"Great Vibes", cursive' }
                ]
            }}
        />
    );
}

export default App;
```

## Font Size Configuration

The Modern Rich Text Editor initializes with a default font size, which inherits the font size of the parent element. You can change the size for the selected text using the font size dropdown in the toolbar. When the default font size is selected, the toolbar displays `Font Size`. For other font sizes, the toolbar shows the selected size.

### Built-in Font Size

You can add the `FontSize` tool to the Modern Rich Text Editor toolbar using the `toolbarSettings` [items](https://ej2.syncfusion.com/documentation/api/richtexteditor-ui/toolbarSettings#items) property.

```tsx
import * as React from 'react';
import { RichTextEditorUIComponent } from '@syncfusion/ej2-react-richtexteditor-ui';

const toolbarSettings = {
    items: ['FontSize']
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

### Custom Font Size

The Modern Rich Text Editor supports providing custom sizes along with the existing list. To add additional sizes to the font size dropdown, configure the `items` field of the [fontSize](https://ej2.syncfusion.com/documentation/api/richtexteditor-ui/fontSize#items) property.

```tsx
import * as React from 'react';
import { RichTextEditorUIComponent } from '@syncfusion/ej2-react-richtexteditor-ui';

const toolbarSettings = {
    items: ['FontSize']
};

const fontSize = {
    items: [
        { text: 'Default', value: 'Default' },
        { text: '10', value: '10px' },
        { text: '12', value: '12px' },
        { text: '14', value: '14px' },
        { text: '16', value: '16px' },
        { text: '18', value: '18px' },
        { text: '24', value: '24px' },
        { text: '32', value: '32px' }
    ]
};

function App() {
    return (
        <RichTextEditorUIComponent
            toolbarSettings={toolbarSettings}
            placeholder='Type something ...'
            fontSize={fontSize}
        />
    );
}

export default App;
```

## Font and Background Color Configuration

You can add the `FontColor` and `BackgroundColor` tools to the Modern Rich Text Editor toolbar using the `toolbarSettings` [items](https://ej2.syncfusion.com/documentation/api/richtexteditor-ui/toolbarSettings#items) property.

```tsx
import * as React from 'react';
import { RichTextEditorUIComponent } from '@syncfusion/ej2-react-richtexteditor-ui';

const toolbarSettings = {
    items: ['FontColor', 'BackgroundColor']
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

### Custom Font and Background Colors

The `FontColor` and `BackgroundColor` properties offer two modes — `Picker` and `Palette`. The `Palette` mode provides a predefined set of colors, while the `Picker` mode includes a color scheme to choose custom colors. You can switch between these options using the [modeSwitcher](https://ej2.syncfusion.com/documentation/api/richtexteditor-ui/fontColorModel#modeSwitcher) feature.

The number of columns in the color palette for both `FontColor` and `BackgroundColor` can be configured using the [columns](https://ej2.syncfusion.com/documentation/api/richtexteditor-ui/backgroundColorModel#columns) property. The [default](https://ej2.syncfusion.com/documentation/api/richtexteditor-ui/backgroundColorModel#default) property specifies the default color that is applied when no color is explicitly selected by the user.

```tsx
import * as React from 'react';
import { RichTextEditorUIComponent } from '@syncfusion/ej2-react-richtexteditor-ui';

const toolbarSettings = {
    items: [
        'Bold', 'Italic', 'Underline', 'Strikethrough', 'Subscript', 'Superscript', 'LowerCase', 'UpperCase', '|',
        'InlineCode', '|',
        'FontName', 'FontSize', 'FontColor', 'BackgroundColor', '|',
        'ClearFormat'
    ]
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

---

## Keyboard Shortcuts for Text Formatting and Styles

| Action | Windows / Linux | macOS | Description |
| :--- | :--- | :--- | :--- |
| **Bold** | <kbd>Ctrl</kbd> + <kbd>B</kbd> | <kbd>⌘</kbd> + <kbd>B</kbd> | Toggles bold styling on the selected text. |
| **Italic** | <kbd>Ctrl</kbd> + <kbd>I</kbd> | <kbd>⌘</kbd> + <kbd>I</kbd> | Toggles italic styling on the selected text. |
| **Underline** | <kbd>Ctrl</kbd> + <kbd>U</kbd> | <kbd>⌘</kbd> + <kbd>U</kbd> | Applies or removes underline formatting. |
| **Strikethrough** | <kbd>Ctrl</kbd> + <kbd>Shift</kbd> + <kbd>S</kbd> | <kbd>⌘</kbd> + <kbd>Shift</kbd> + <kbd>S</kbd> | Strikes through the selected text. |
| **Subscript** | <kbd>Ctrl</kbd> + <kbd>=</kbd> | <kbd>⌘</kbd> + <kbd>=</kbd> | Converts the selected text to subscript. |
| **Superscript** | <kbd>Ctrl</kbd> + <kbd>Shift</kbd> + <kbd>=</kbd> | <kbd>⌘</kbd> + <kbd>Shift</kbd> + <kbd>=</kbd> | Converts the selected text to superscript. |
| **Inline Code** | <kbd>Ctrl</kbd> + <kbd>E</kbd> | <kbd>⌘</kbd> + <kbd>E</kbd> | Formats the selected text as inline code. |
| **Clear Format** | <kbd>Ctrl</kbd> + <kbd>Shift</kbd> + <kbd>R</kbd> | <kbd>⌘</kbd> + <kbd>Shift</kbd> + <kbd>R</kbd> | Clears inline formatting from the selection. |
| **Uppercase** | <kbd>Ctrl</kbd> + <kbd>Shift</kbd> + <kbd>U</kbd> | <kbd>⌘</kbd> + <kbd>Shift</kbd> + <kbd>U</kbd> | Transforms the selection to uppercase. |
| **Lowercase** | <kbd>Ctrl</kbd> + <kbd>Shift</kbd> + <kbd>L</kbd> | <kbd>⌘</kbd> + <kbd>Shift</kbd> + <kbd>L</kbd> | Transforms the selection to lowercase. |

For the remaining built-in shortcuts see [toolbar.md](toolbar.md#keyboard-shortcuts-for-insertions-and-blocks), [lists.md](lists.md#keyboard-shortcuts-for-lists-and-indentation), and [codeblock.md](codeblock.md#keyboard-shortcut).

## API Reference


## ColorCommandBuilder

**Builder Class** — Builder for color commands (fontColor, backgroundColor)

### Methods

#### `color(colorValue: string)`
Set the color value  
**Returns:** `this`

#### `apply()`
Execute the color command with set options  
**Returns:** `void`

---

## ColorModeType

**Enumeration** — Enumerates the color picker display modes supported by the font color and background color pickers.

- `Palette` — Renders a fixed swatch grid.
- `Picker` — Renders an inline color picker only (no palette).

---

## ColorPickerType

**Enumeration** — Identifies which editor color picker triggered an event or is being referenced in a payload.

- `FontColor`
- `BackgroundColor`

---

## FontColor

**Configuration Class** — Defines the configuration for the font color picker. Controls the default color, color palette, mode, number of columns, mode switcher, and recent-colors visibility shown in the FontColor toolbar item.

### Properties

#### `columns` (number)
Specifies the number of columns in the color palette.  
**Default:** `5`

#### `default` (string)
Specifies the default font color.  
**Default:** `'#DC2626'`

#### `mode` (ColorModeType)
Specifies the color mode.  
**Default:** `'Palette'`

#### `modeSwitcher` (boolean)
Enables or disables the mode switcher button.  
**Default:** `false`

#### `preset` ({ [key: string]: string[] })
Specifies custom color codes keyed by group label.  
**Default:** `{ Custom: ['#000000', '#FFFFFF', '#DC2626', '#B8590D', '#8C7000', '#5B21B6', '#4D700F', '#1F7333', '#146B52', '#0D6666', '#0A6185', '#1A5499', '#2640A6', '#383399', '#612E9E', '#732494', '#941F6B', '#9E2447', '#992938', '#704724', '#7A612E', '#5C611F', '#404C61', '#4D4D4D', '#242947'] }`

#### `showRecentColors` (boolean)
Indicates whether the recent colors section is shown in the picker.  
**Default:** `true`

---

## FontColorModel

**Interface** — Interface for a class FontColor

### Properties

#### `columns` (number)
Specifies the number of columns in the color palette.

#### `default` (string)
Specifies the default font color.

#### `mode` (ColorModeType)
Specifies the color mode.

#### `modeSwitcher` (boolean)
Enables or disables the mode switcher button.

#### `preset` ({ [key: string]: string[] })
Specifies custom color codes keyed by group label.

#### `showRecentColors` (boolean)
Indicates whether the recent colors section is shown in the picker.

---

## FontFamily

**Configuration Class** — Font family settings for the editor. Configures the font family dropdown control within the toolbar.

### Properties

#### `items` (FontFamilyItem[])
Specifies the default font family items for the dropdown. Each item includes id, text, command, and typed value payload.  
**Default:** `DEFAULT_FONT_FAMILY_ITEMS`

#### `width` (string)
Specifies the width of the dropdown content area.  
**Default:** `'72px'`

---

## FontFamilyCommandBuilder

**Builder Class** — Builder for font family command

### Methods

#### `apply()`
Execute the font family command with set options  
**Returns:** `void`

#### `family(fontName: string)`
Set the font family name

| Parameter | Type | Description |
|-----------|------|-------------|
| `fontName` | string | The font family name to set |

**Returns:** `this`

---

## FontFamilyItem

**Interface** — Typed interface for font family dropdown items. Represents a single font family option with family name and editor command.

### Properties

#### `command` (string)
Editor command to execute (must be `'fontName'`)

#### `id` (string)
Unique identifier for the dropdown menu item

#### `text` (string)
Display text for the item (e.g., `'Arial'`)

#### `value` (string)
Font family value payload with font name (e.g., `{ family: 'Arial' }`)

---

## FontFamilyModel

**Interface** — Interface for a class FontFamily

### Properties

#### `items` (FontFamilyItem[])
Specifies the default font family items for the dropdown. Each item includes id, text, command, and typed value payload.

#### `width` (string)
Specifies the width of the dropdown content area.

---

## FontSize

**Configuration Class** — Font size settings for the editor. Configures the font size dropdown control within the toolbar.

### Properties

#### `items` (FontSizeItem[])
Specifies the default font size items for the dropdown. Each item includes id, text, command, and typed value payload.  
**Default:** `DEFAULT_FONT_SIZE_ITEMS`

#### `width` (string)
Specifies the width of the dropdown content area.  
**Default:** `'60px'`

---

## FontSizeCommandBuilder

**Builder Class** — Builder for font size command

### Methods

#### `apply()`
Execute the font size command with set options  
**Returns:** `void`

#### `size(sizeValue: string)`
Set the font size

| Parameter | Type | Description |
|-----------|------|-------------|
| `sizeValue` | string | The font size value to set |

**Returns:** `this`

---

## FontSizeItem

**Interface** — Typed interface for font size dropdown items. Represents a single font size option with numeric value and editor command.

### Properties

#### `command` (string)
Editor command to execute (must be `'fontSize'`)

#### `id` (string)
Unique identifier for the dropdown menu item

#### `text` (string)
Display text for the item (e.g., `'14'`)

#### `value` (string)
Font size value payload with CSS unit (e.g., `{ size: '14px' }`)

---

## FontSizeModel

**Interface** — Interface for a class FontSize

### Properties

#### `items` (FontSizeItem[])
Specifies the default font size items for the dropdown. Each item includes id, text, command, and typed value payload.

#### `width` (string)
Specifies the width of the dropdown content area.

---

## BackgroundColor

**Configuration Class** — Defines the configuration for the background color (text highlight color) picker.

### Properties

#### `columns` (number)
Specifies the number of columns in the color palette.  
**Default:** `5`

#### `default` (string)
Specifies the default background color.  
**Default:** `'#FFF7C7'`

#### `mode` (ColorModeType)
Specifies the color mode.  
**Default:** `'Palette'`

#### `modeSwitcher` (boolean)
Enables or disables the mode switcher button.  
**Default:** `false`

#### `preset` ({ [key: string]: string[] })
Specifies custom color codes keyed by group label.  
**Default:** 25-color palette starting with `#000000`, `#FFFFFF`, `#FCE3E0`, etc.

#### `showRecentColors` (boolean)
Indicates whether the recent colors section is shown in the picker.  
**Default:** `true`

---

## BackgroundColorModel

**Interface** — Interface for a class BackgroundColor.

### Properties

#### `columns` (number)
Specifies the number of columns in the color palette.

#### `default` (string)
Specifies the default background color.

#### `mode` (ColorModeType)
Specifies the color mode.

#### `modeSwitcher` (boolean)
Enables or disables the mode switcher button.

#### `preset` ({ [key: string]: string[] })
Specifies custom color codes keyed by group label.

#### `showRecentColors` (boolean)
Indicates whether the recent colors section is shown in the picker.

---

## Format

**Configuration Class** — Configuration for the Format toolbar item.

### Properties

#### `items` (ToolbarSubItem[])
Specifies the items displayed in the Format dropdown.  
**Default:** `DEFAULT_FORMAT_ITEMS`

#### `width` (string)
Specifies the width of the Format dropdown.  
**Default:** `'75px'`

---

## FormatModel

**Interface** — Model for format configuration

### Properties

#### `items` (ToolbarSubItem[])
Specifies the items displayed in the Format dropdown.

#### `width` (string)
Specifies the width of the Format dropdown.

---

**End of Part 2**

## AlignmentCommandBuilder

**Builder Class** — Builder for text alignment command.

### Methods

#### `align(alignment: string)`
Set the text alignment  
**Returns:** `this`

#### `apply()`
Execute the alignment command with set options  
**Returns:** `void`

---

## API Reference

## ColorCommandBuilder

**Builder Class** — Builder for color commands (fontColor, backgroundColor)

### Methods

#### `color(colorValue: string)`
Set the color value  
**Returns:** `this`

#### `apply()`
Execute the color command with set options  
**Returns:** `void`

---
## ColorModeType

**Enumeration** — Enumerates the color picker display modes supported by the font color and background color pickers.

- `Palette` — Renders a fixed swatch grid.
- `Picker` — Renders an inline color picker only (no palette).

---
## ColorPickerType

**Enumeration** — Identifies which editor color picker triggered an event or is being referenced in a payload.

- `FontColor`
- `BackgroundColor`

---
## FontColor

**Configuration Class** — Defines the configuration for the font color picker. Controls the default color, color palette, mode, number of columns, mode switcher, and recent-colors visibility shown in the FontColor toolbar item.

### Properties

#### `columns` (number)
Specifies the number of columns in the color palette.  
**Default:** `5`

#### `default` (string)
Specifies the default font color.  
**Default:** `'#DC2626'`

#### `mode` (ColorModeType)
Specifies the color mode.  
**Default:** `'Palette'`

#### `modeSwitcher` (boolean)
Enables or disables the mode switcher button.  
**Default:** `false`

#### `preset` ({ [key: string]: string[] })
Specifies custom color codes keyed by group label.  
**Default:** `{ Custom: ['#000000', '#FFFFFF', '#DC2626', '#B8590D', '#8C7000', '#5B21B6', '#4D700F', '#1F7333', '#146B52', '#0D6666', '#0A6185', '#1A5499', '#2640A6', '#383399', '#612E9E', '#732494', '#941F6B', '#9E2447', '#992938', '#704724', '#7A612E', '#5C611F', '#404C61', '#4D4D4D', '#242947'] }`

#### `showRecentColors` (boolean)
Indicates whether the recent colors section is shown in the picker.  
**Default:** `true`

---
## FontColorModel

**Interface** — Interface for a class FontColor

### Properties

#### `columns` (number)
Specifies the number of columns in the color palette.

#### `default` (string)
Specifies the default font color.

#### `mode` (ColorModeType)
Specifies the color mode.

#### `modeSwitcher` (boolean)
Enables or disables the mode switcher button.

#### `preset` ({ [key: string]: string[] })
Specifies custom color codes keyed by group label.

#### `showRecentColors` (boolean)
Indicates whether the recent colors section is shown in the picker.

---
## FontFamily

**Configuration Class** — Font family settings for the editor. Configures the font family dropdown control within the toolbar.

### Properties

#### `items` (FontFamilyItem[])
Specifies the default font family items for the dropdown. Each item includes id, text, command, and typed value payload.  
**Default:** `DEFAULT_FONT_FAMILY_ITEMS`

#### `width` (string)
Specifies the width of the dropdown content area.  
**Default:** `'72px'`

---
## FontFamilyCommandBuilder

**Builder Class** — Builder for font family command

### Methods

#### `apply()`
Execute the font family command with set options  
**Returns:** `void`

#### `family(fontName: string)`
Set the font family name

| Parameter | Type | Description |
|-----------|------|-------------|
| `fontName` | string | The font family name to set |

**Returns:** `this`

---
## FontFamilyItem

**Interface** — Typed interface for font family dropdown items. Represents a single font family option with family name and editor command.

### Properties

#### `command` (string)
Editor command to execute (must be `'fontName'`)

#### `id` (string)
Unique identifier for the dropdown menu item

#### `text` (string)
Display text for the item (e.g., `'Arial'`)

#### `value` (string)
Font family value payload with font name (e.g., `{ family: 'Arial' }`)

---
## FontFamilyModel

**Interface** — Interface for a class FontFamily

### Properties

#### `items` (FontFamilyItem[])
Specifies the default font family items for the dropdown. Each item includes id, text, command, and typed value payload.

#### `width` (string)
Specifies the width of the dropdown content area.

---
## FontSize

**Configuration Class** — Font size settings for the editor. Configures the font size dropdown control within the toolbar.

### Properties

#### `items` (FontSizeItem[])
Specifies the default font size items for the dropdown. Each item includes id, text, command, and typed value payload.  
**Default:** `DEFAULT_FONT_SIZE_ITEMS`

#### `width` (string)
Specifies the width of the dropdown content area.  
**Default:** `'60px'`

---
## FontSizeCommandBuilder

**Builder Class** — Builder for font size command

### Methods

#### `apply()`
Execute the font size command with set options  
**Returns:** `void`

#### `size(sizeValue: string)`
Set the font size

| Parameter | Type | Description |
|-----------|------|-------------|
| `sizeValue` | string | The font size value to set |

**Returns:** `this`

---
## FontSizeItem

**Interface** — Typed interface for font size dropdown items. Represents a single font size option with numeric value and editor command.

### Properties

#### `command` (string)
Editor command to execute (must be `'fontSize'`)

#### `id` (string)
Unique identifier for the dropdown menu item

#### `text` (string)
Display text for the item (e.g., `'14'`)

#### `value` (string)
Font size value payload with CSS unit (e.g., `{ size: '14px' }`)

---
## FontSizeModel

**Interface** — Interface for a class FontSize

### Properties

#### `items` (FontSizeItem[])
Specifies the default font size items for the dropdown. Each item includes id, text, command, and typed value payload.

#### `width` (string)
Specifies the width of the dropdown content area.

---
## BackgroundColor

**Configuration Class** — Defines the configuration for the background color (text highlight color) picker.

### Properties

#### `columns` (number)
Specifies the number of columns in the color palette.  
**Default:** `5`

#### `default` (string)
Specifies the default background color.  
**Default:** `'#FFF7C7'`

#### `mode` (ColorModeType)
Specifies the color mode.  
**Default:** `'Palette'`

#### `modeSwitcher` (boolean)
Enables or disables the mode switcher button.  
**Default:** `false`

#### `preset` ({ [key: string]: string[] })
Specifies custom color codes keyed by group label.  
**Default:** 25-color palette starting with `#000000`, `#FFFFFF`, `#FCE3E0`, etc.

#### `showRecentColors` (boolean)
Indicates whether the recent colors section is shown in the picker.  
**Default:** `true`

---
## BackgroundColorModel

**Interface** — Interface for a class BackgroundColor.

### Properties

#### `columns` (number)
Specifies the number of columns in the color palette.

#### `default` (string)
Specifies the default background color.

#### `mode` (ColorModeType)
Specifies the color mode.

#### `modeSwitcher` (boolean)
Enables or disables the mode switcher button.

#### `preset` ({ [key: string]: string[] })
Specifies custom color codes keyed by group label.

#### `showRecentColors` (boolean)
Indicates whether the recent colors section is shown in the picker.

---
## Format

**Configuration Class** — Configuration for the Format toolbar item.

### Properties

#### `items` (ToolbarSubItem[])
Specifies the items displayed in the Format dropdown.  
**Default:** `DEFAULT_FORMAT_ITEMS`

#### `width` (string)
Specifies the width of the Format dropdown.  
**Default:** `'75px'`

---
## FormatModel

**Interface** — Model for format configuration

### Properties

#### `items` (ToolbarSubItem[])
Specifies the items displayed in the Format dropdown.

#### `width` (string)
Specifies the width of the Format dropdown.

---

**End of Part 2**

# API References (Part 3)

## Table of Contents

- [Format](#format)
- [FontSizeModel](#fontsizemodel)
- [FontSizeItem](#fontsizeitem)
- [FontSizeCommandBuilder](#fontsizecommandbuilder)
- [FontSize](#fontsize)
- [FontFamilyModel](#fontfamilymodel)
- [FontFamilyItem](#fontfamilyitem)
- [FontFamilyCommandBuilder](#fontfamilycommandbuilder)
- [FontFamily](#fontfamily)
- [FontColorModel](#fontcolormodel)
- [FontColor](#fontcolor)
- [FileUploadingEventArgs](#fileuploadingeventargs)
- [FileUploadSuccessEventArgs](#fileuploadsuccesseventargs)
- [FileUploadFailedEventArgs](#fileuploadfailedeventargs)
- [FileSelectedEventArgs](#fileselectedeventargs)
- [FileRemovingEventArgs](#fileremovingeventargs)
- [EnableToolbarItem](#enabletoolbaritem)
- [EditorMark](#editormark)
- [EditorKeyBindingAction](#editorkeybindingaction)
- [EditorDocument](#editordocument)
- [EditorComponentEventArgs](#editorcomponenteventargs)
- [DisableToolbarItem](#disabletoolbaritem)
- [DialogType](#dialogtype)
- [CustomToolbarItem](#customtoolbaritem)
- [CustomNodeName](#customnodename)

---
## ToggleCommandBuilder

Base builder for toggle commands (bold, italic, etc.).

### Methods

#### apply

Executes the toggle command.

Returns `void`.
## AlignmentCommandBuilder

**Builder Class** — Builder for text alignment command.

### Methods

#### `align(alignment: string)`
Set the text alignment  
**Returns:** `this`

#### `apply()`
Execute the alignment command with set options  
**Returns:** `void`

---
