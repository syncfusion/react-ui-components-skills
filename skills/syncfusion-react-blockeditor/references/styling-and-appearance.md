# Styling and Appearance

## Table of Contents
- [CSS Theming](#css-theming)
- [Available Themes](#available-themes)
- [Theme Customization](#theme-customization)
- [Block Editor CSS Classes and IDs](#block-editor-css-classes-and-ids)
- [Block Editor CSS Variables](#block-editor-css-variables)
- [Block Styling](#block-styling)
- [Typography Options](#typography-options)
- [Indentation and Nesting](#indentation-and-nesting)
- [Placeholder Customization](#placeholder-customization)
- [Dark Mode Support](#dark-mode-support)
- [Using Theme Studio](#using-theme-studio)
- [Custom Styling Examples](#custom-styling-examples)

## CSS Theming

The BlockEditor uses Syncfusion's centralized theme system. Import theme CSS files to style the component.

### Basic Theme Import

```css
/* Material theme */
@import "../node_modules/@syncfusion/ej2-base/styles/material.css";
@import "../node_modules/@syncfusion/ej2-inputs/styles/material.css";
@import "../node_modules/@syncfusion/ej2-popups/styles/material.css";
@import "../node_modules/@syncfusion/ej2-buttons/styles/material.css";
@import "../node_modules/@syncfusion/ej2-splitbuttons/styles/material.css";
@import "../node_modules/@syncfusion/ej2-navigations/styles/material.css";
@import "../node_modules/@syncfusion/ej2-dropdowns/styles/material.css";
@import "../node_modules/@syncfusion/ej2-react-blockeditor/styles/material.css";
```

### Theme Import in React Component

```tsx
import '@syncfusion/ej2-base/styles/material.css';
import '@syncfusion/ej2-inputs/styles/material.css';
import '@syncfusion/ej2-popups/styles/material.css';
import '@syncfusion/ej2-buttons/styles/material.css';
import '@syncfusion/ej2-splitbuttons/styles/material.css';
import '@syncfusion/ej2-navigations/styles/material.css';
import '@syncfusion/ej2-dropdowns/styles/material.css';
import '@syncfusion/ej2-react-blockeditor/styles/material.css';

import { BlockEditorComponent } from '@syncfusion/ej2-react-blockeditor';

function App() {
    return <BlockEditorComponent id="editor" />;
}
```

## Available Themes

Syncfusion provides multiple built-in themes. Replace the theme name in imports:

### Theme Options

| Theme | Import Path | Best For |
|-------|-------------|----------|
| Material | `material.css` | Google Material Design |
| Bootstrap 5 | `bootstrap5.css` | Bootstrap ecosystem |
| Fluent | `fluent.css` | Microsoft Fluent Design |
| Tailwind | `tailwind3.css` | Tailwind CSS projects |
| Material Dark | `material-dark.css` | Dark mode Material |
| Bootstrap Dark | `bootstrap5-dark.css` | Dark mode Bootstrap |
| Tailwind Dark | `tailwind3-dark.css` | Dark mode Tailwind |

### Switching Themes

```tsx
// Theme selector component
function ThemeSwitcher() {
    const [theme, setTheme] = React.useState('material');

    const handleThemeChange = (newTheme: string) => {
        setTheme(newTheme);
        // Dynamically update theme imports
        document.documentElement.dataset.theme = newTheme;
    };

    return (
        <div>
            <select onChange={(e) => handleThemeChange(e.target.value)}>
                <option value="material">Material</option>
                <option value="bootstrap5">Bootstrap 5</option>
                <option value="fluent">Fluent</option>
                <option value="tailwind3">Tailwind</option>
            </select>
        </div>
    );
}
```

## Theme Customization

The Block Editor provides flexible theme customization options to help match the editor appearance with your application design. You can customize built-in themes by overriding CSS variables, targeting specific CSS classes/IDs, or create a fully customized theme using Syncfusion Theme Studio.

### Default CSS Override

The Block Editor uses CSS variables with the unified `--sf` naming convention. These variables control colors, typography, backgrounds, borders, icons, and other visual elements across the editor.

### Block Editor CSS Classes and IDs

| Selector                      | Purpose                            |
| ----------------------------- | ---------------------------------- |
| `.e-blockeditor`              | Main Block Editor element          |
| `.e-active`                   | Active Block Editor element        |
| `.e-selected`                 | Selected Block Editor element      |
| `.e-list-item`                | Dropdown or list item              |
| `.e-dropdownbase`             | Dropdown container                 |
| `.e-table-element`            | Block Editor table element         |
| `#blockeditor_contextmenu`    | Block Editor context menu          |
| `#blockeditor_code-ddl_popup` | Code block language dropdown popup |

### Block Editor CSS Variables

| Variable                               | Purpose                  |
| -------------------------------------- | ------------------------ |
| `--color-sf-content-bg-color`          | Content background       |
| `--color-sf-content-bg-color-hover`    | Hover background         |
| `--color-sf-content-bg-color-selected` | Selected background      |
| `--color-sf-content-bg-color-pressed`  | Pressed background       |
| `--color-sf-content-text-color`        | Content text color       |
| `--color-sf-content-text-color-alt1`   | Alternate text color     |
| `--color-sf-placeholder-text-color`    | Placeholder text color   |
| `--color-sf-border-light`              | Light border color       |
| `--color-sf-border`                    | Border color             |
| `--color-sf-border-hover`              | Hover border color       |
| `--color-sf-border-selected`           | Selected border color    |
| `--color-sf-primary`                   | Primary color            |
| `--color-sf-primary-bg-color`          | Primary background color |
| `--color-sf-primary-border-color`      | Primary border color     |
| `--color-sf-primary-text-color`        | Primary text color       |
| `--color-sf-icon-color`                | Icon color               |

### Theme Customization Example

The following example demonstrates how to customize the Block Editor appearance using CSS variable overrides with multiple built-in themes.

```tsx
import * as React from 'react';
import * as ReactDOM from 'react-dom';
import { useState } from 'react';
import { BlockEditorComponent, ContentType } from '@syncfusion/ej2-react-blockeditor';

const blockData = [
  {
    blockType: 'Heading',
    properties: { level: 1 },
    content: [{ contentType: ContentType.Text, content: 'Block Editor - All Block Types' }]
  },
  {
    blockType: 'Paragraph',
    content: [
      { contentType: ContentType.Text, content: 'This sample shows every block type used once, including a ' },
      { contentType: ContentType.Text, content: 'bold', properties: { styles: { bold: true } } },
      { contentType: ContentType.Text, content: ' word, a ' },
      {
        contentType: ContentType.Link,
        content: 'link',
        properties: { url: 'https://ej2.syncfusion.com/documentation/block-editor/getting-started' }
      },
      { contentType: ContentType.Text, content: ', a mention ' },
      { contentType: ContentType.Mention, properties: { userId: 'user1' } },
      { contentType: ContentType.Text, content: ', and a label ' },
      { contentType: ContentType.Label, properties: { labelId: 'progress' } },
      { contentType: ContentType.Text, content: '.' }
    ]
  },
  {
    blockType: 'BulletList',
    content: [{ contentType: ContentType.Text, content: 'Bullet list item' }]
  },
  {
    blockType: 'NumberedList',
    content: [{ contentType: ContentType.Text, content: 'Numbered list item' }]
  },
  {
    blockType: 'Checklist',
    properties: { isChecked: true },
    content: [{ contentType: ContentType.Text, content: 'Checklist item' }]
  },
  {
    blockType: 'Quote',
    properties: {
      children: [
        {
          blockType: 'Paragraph',
          content: [{ contentType: ContentType.Text, content: 'Quote block example.' }]
        }
      ]
    }
  },
  {
    blockType: 'Code',
    content: [{ contentType: ContentType.Text, content: 'function greet(name) {\n  return `Hello, ${name}!`;\n}' }]
  },
  {
    blockType: 'CollapsibleParagraph',
    content: [{ contentType: ContentType.Text, content: 'Toggle block example', properties: { styles: { bold: true } } }],
    properties: {
      isExpanded: false,
      children: [
        {
          blockType: 'Paragraph',
          content: [{ contentType: ContentType.Text, content: 'Collapsed content goes here.' }]
        }
      ]
    }
  },
  {
    blockType: 'Table',
    properties: {
      columns: [{ headerText: 'Name' }, { headerText: 'Role' }, { headerText: 'Status' }],
      rows: [
        {
          cells: [
            { blocks: [{ blockType: 'Paragraph', content: [{ contentType: ContentType.Text, content: 'Arun Kumar' }] }] },
            { blocks: [{ blockType: 'Paragraph', content: [{ contentType: ContentType.Text, content: 'Developer' }] }] },
            { blocks: [{ blockType: 'Paragraph', content: [{ contentType: ContentType.Text, content: 'Active' }] }] }
          ]
        },
        {
          cells: [
            { blocks: [{ blockType: 'Paragraph', content: [{ contentType: ContentType.Text, content: 'Divya Sharma' }] }] },
            { blocks: [{ blockType: 'Paragraph', content: [{ contentType: ContentType.Text, content: 'Designer' }] }] },
            { blocks: [{ blockType: 'Paragraph', content: [{ contentType: ContentType.Text, content: 'Active' }] }] }
          ]
        }
      ]
    }
  },
  { blockType: 'Divider' }
];

const users = [{ id: 'user1', user: 'Andrews', avatarUrl: 'https://ej2.syncfusion.com/demos/src/block-editor/images/andrew.png' }];

function App() {
  const [theme, setTheme] = useState('slateLightTheme');

  return (
    <div className={theme}>
      <div className="container">
        <h2>Block Editor - Theme Customization</h2>

        <div className="theme-switcher">
          <button onClick={() => setTheme('slateLightTheme')}>Slate Light</button>
          <button onClick={() => setTheme('sageGreenTheme')}>Sage Green</button>
          <button onClick={() => setTheme('darkProTheme')}>Dark Pro</button>
        </div>

        <BlockEditorComponent
          id="block-editor"
          width="100%"
          height="600px"
          blocks={blockData}
          users={users}
        />
      </div>
    </div>
  );
}
export default App;

ReactDOM.render(<App />, document.getElementById('container'));
```

```css
/* Theme Customization Styles */

body {
  margin: 0;
  font-family: "Segoe UI", sans-serif;
  background: #f4f5f7;
  color: #1f2937;
  transition: background-color 0.3s ease, color 0.3s ease;
}

#container {
  padding: 40px 20px;
  min-height: 100vh;
  box-sizing: border-box;
}

#loader {
  color: #008cff;
  font-size: 14px;
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
}

.container {
  max-width: 1100px;
  margin: 0 auto;
  padding: 24px;
  border-radius: 22px;
  background: rgba(255, 255, 255, 0.5);
  backdrop-filter: blur(10px);
  transition: all 0.3s ease;
}

h2 {
  margin: 0 0 16px;
  font-size: 2rem;
  color: inherit;
  font-weight: 600;
}

/* =====================================
   THEME SWITCHER
   ===================================== */

/* Theme button container */
.theme-switcher {
  margin-bottom: 15px;
  display: flex;
  gap: 10px;
  flex-wrap: wrap;
}

/* Theme buttons */
.theme-switcher button {
  padding: 10px 16px;
  border: 1px solid #ccc;
  border-radius: 8px;
  cursor: pointer;
  background: #fff;
  color: #303030;
  font-weight: 600;
  transition: all 0.3s ease;
}

/* Theme button hover state */
.theme-switcher button:hover {
  background-color: #f0f0f0;
  border-color: #999;
}

/* Active theme button */
.theme-switcher button.active {
  background: inherit;
  border-color: inherit;
}

/* =====================================
   SLATE LIGHT THEME
   ===================================== */

/* Block Editor appearance */
.slateLightTheme .e-blockeditor {
  /* Background Colors */
  --color-sf-content-bg-color: #f8f7f4;
  --color-sf-content-bg-color-hover: #efede7;
  --color-sf-content-bg-color-selected: #e7e5df;
  --color-sf-content-bg-color-pressed: #dcd9d2;

  /* Text Colors */
  --color-sf-content-text-color: #2f3437;
  --color-sf-content-text-color-alt1: #1f2328;
  --color-sf-placeholder-text-color: #787774;

  /* Border Colors */
  --color-sf-border-light: #e7e5df;
  --color-sf-border: #d0cdc7;
  --color-sf-border-hover: #8b8b8b;
  --color-sf-border-selected: #5f5f5f;

  /* Primary Accent */
  --color-sf-primary: #2563eb;
  --color-sf-primary-bg-color: #2563eb;
  --color-sf-primary-border-color: #2563eb;
  --color-sf-primary-text-color: #ffffff;

  /* Icons */
  --color-sf-icon-color: #6b7280;

  background: #f8f7f4 !important;
  border: 1px solid #d0cdc7;
  border-radius: 20px;
  box-shadow: 0 8px 24px rgba(0, 0, 0, 0.08);
}

/* Context menu and popup styling */
body.slateLightTheme #blockeditor_contextmenu,
body.slateLightTheme .e-content .e-dropdownbase,
body.slateLightTheme #blockeditor_code-ddl_popup {
  background: #ffffff;
  border: 1px solid #d0cdc7;
  color: #2f3437;
}

/* Dropdown items */
body.slateLightTheme #blockeditor_code-ddl_popup .e-list-item {
  background: #ffffff !important;
  color: #2f3437 !important;
}

/* Dropdown hover state */
body.slateLightTheme #blockeditor_code-ddl_popup .e-list-item:hover {
  background: #efede7 !important;
  color: #1f2328 !important;
}

/* Selected dropdown item */
body.slateLightTheme #blockeditor_code-ddl_popup .e-selected,
body.slateLightTheme #blockeditor_code-ddl_popup .e-active {
  background: #2563eb !important;
  color: #ffffff !important;
}

/* Table headers */
.slateLightTheme .e-blockeditor .e-table-element thead th {
  background: #2563eb !important;
  color: #ffffff !important;
}

/* =====================================
   SAGE GREEN THEME
   ===================================== */

/* Block Editor appearance */
.sageGreenTheme .e-blockeditor {
  /* Background Colors */
  --color-sf-content-bg-color: #f6f8f4;
  --color-sf-content-bg-color-hover: #edf2e8;
  --color-sf-content-bg-color-selected: #dfe8d5;
  --color-sf-content-bg-color-pressed: #d2dfc4;

  /* Text Colors */
  --color-sf-content-text-color: #2f3e2f;
  --color-sf-content-text-color-alt1: #243224;
  --color-sf-placeholder-text-color: #6b7a61;

  /* Border Colors */
  --color-sf-border-light: #dfe8d5;
  --color-sf-border: #c8d5b9;
  --color-sf-border-hover: #7a9b5f;
  --color-sf-border-selected: #6b8e23;

  /* Primary Accent */
  --color-sf-primary: #6b8e23;
  --color-sf-primary-bg-color: #6b8e23;
  --color-sf-primary-border-color: #6b8e23;
  --color-sf-primary-text-color: #ffffff;

  /* Icons */
  --color-sf-icon-color: #5f7d3b;

  background: #f6f8f4 !important;
  border: 1px solid #c8d5b9;
  border-radius: 20px;
  box-shadow: 0 8px 24px rgba(107, 142, 35, 0.12);
}

/* Context menu and popup styling */
body.sageGreenTheme #blockeditor_contextmenu,
body.sageGreenTheme .e-content .e-dropdownbase,
body.sageGreenTheme #blockeditor_code-ddl_popup {
  background: #fbfcfa;
  border: 1px solid #c8d5b9;
  color: #2f3e2f;
}

/* Dropdown items */
body.sageGreenTheme #blockeditor_code-ddl_popup .e-list-item {
  background: #fbfcfa !important;
  color: #2f3e2f !important;
}

/* Dropdown hover state */
body.sageGreenTheme #blockeditor_code-ddl_popup .e-list-item:hover {
  background: #edf2e8 !important;
  color: #4d6530 !important;
}

/* Selected dropdown item */
body.sageGreenTheme #blockeditor_code-ddl_popup .e-selected,
body.sageGreenTheme #blockeditor_code-ddl_popup .e-active {
  background: #6b8e23 !important;
  color: #ffffff !important;
}

/* Table headers */
.sageGreenTheme .e-blockeditor .e-table-element thead th {
  background: #6b8e23 !important;
  color: #ffffff !important;
}

/* =====================================
   DARK PRO THEME
   ===================================== */

/* Block Editor appearance */
.darkProTheme .e-blockeditor {
  /* Background Colors */
  --color-sf-content-bg-color: #0d1117;
  --color-sf-content-bg-color-hover: #161b22;
  --color-sf-content-bg-color-selected: #21262d;
  --color-sf-content-bg-color-pressed: #30363d;

  /* Text Colors */
  --color-sf-content-text-color: #e6edf3;
  --color-sf-content-text-color-alt1: #ffffff;
  --color-sf-placeholder-text-color: #7d8590;

  /* Border Colors */
  --color-sf-border-light: #21262d;
  --color-sf-border: #30363d;
  --color-sf-border-hover: #58a6ff;
  --color-sf-border-selected: #58a6ff;

  /* Primary Accent */
  --color-sf-primary: #1f6feb;
  --color-sf-primary-bg-color: #1f6feb;
  --color-sf-primary-border-color: #1f6feb;
  --color-sf-primary-text-color: #ffffff;

  /* Icons */
  --color-sf-icon-color: #8b949e;

  background: #0d1117 !important;
  border: 1px solid #30363d;
  border-radius: 20px;
  box-shadow: 0 12px 32px rgba(0, 0, 0, 0.55);
}

/* Context menu and popup styling */
body.darkProTheme #blockeditor_contextmenu,
body.darkProTheme .e-content .e-dropdownbase,
body.darkProTheme #blockeditor_code-ddl_popup {
  background: #161b22;
  border: 1px solid #30363d;
  color: #e6edf3;
  border-radius: 12px;
  box-shadow: 0 8px 24px rgba(0, 0, 0, 0.4);
}

/* Dropdown items */
body.darkProTheme #blockeditor_code-ddl_popup .e-list-item {
  background: #161b22 !important;
  color: #e6edf3 !important;
}

/* Dropdown hover state */
body.darkProTheme #blockeditor_code-ddl_popup .e-list-item:hover {
  background: #21262d !important;
  color: #ffffff !important;
}

/* Selected dropdown item */
body.darkProTheme #blockeditor_code-ddl_popup .e-selected,
body.darkProTheme #blockeditor_code-ddl_popup .e-active {
  background: #1f6feb !important;
  color: #ffffff !important;
}

/* Table headers */
.darkProTheme .e-blockeditor .e-table-element thead th {
  background: #1f6feb !important;
  color: #ffffff !important;
}

/* Active editor elements */
.darkProTheme .e-blockeditor .e-active,
.darkProTheme .e-blockeditor .e-selected {
  color: #58a6ff !important;
}

/* Links */
.darkProTheme .e-blockeditor a {
  color: #58a6ff !important;
}
```

## Block Styling

### CSS Class Property

Apply custom CSS classes to blocks using the `cssClass` property:

```tsx
import { BlockEditorComponent, BlockModel, ContentType } from '@syncfusion/ej2-react-blockeditor';
import * as React from 'react';

function App() {
    const blocks: BlockModel[] = [
        {
            id: 'block-1',
            blockType: 'Paragraph',
            cssClass: 'default-block',
            content: [{
                contentType: ContentType.Text,
                content: 'Default styling'
            }]
        },
        {
            id: 'block-2',
            blockType: 'Paragraph',
            cssClass: 'highlight-block important',
            content: [{
                contentType: ContentType.Text,
                content: 'Highlighted and important'
            }]
        },
        {
            id: 'block-3',
            blockType: 'Paragraph',
            cssClass: 'warning-block',
            content: [{
                contentType: ContentType.Text,
                content: 'Warning message'
            }]
        }
    ];

    return (
        <div>
            <style>{`
                .highlight-block {
                    background-color: #fff3cd;
                    border-left: 4px solid #ffc107;
                    padding: 10px 15px;
                }
                
                .important {
                    font-weight: bold;
                    color: #cc0000;
                }
                
                .warning-block {
                    background-color: #f8d7da;
                    border-left: 4px solid #f5c6cb;
                    color: #721c24;
                    padding: 10px 15px;
                }
                
                .default-block {
                    padding: 8px 0;
                }
            `}</style>
            <BlockEditorComponent id="editor" blocks={blocks} />
        </div>
    );
}

export default App;
```

### Block Type-Specific Styling

```css
/* Style all paragraph blocks */
.e-block[data-block-type="Paragraph"] {
    line-height: 1.6;
    color: #333;
}

/* Style heading blocks */
.e-block[data-block-type="Heading"] {
    margin-top: 20px;
    margin-bottom: 10px;
    color: #1a1a1a;
}

/* Style list items */
.e-block[data-block-type="BulletList"],
.e-block[data-block-type="NumberedList"] {
    margin-left: 20px;
}

/* Style code blocks */
.e-block[data-block-type="Code"] {
    background-color: #f5f5f5;
    border-radius: 4px;
    font-family: 'Courier New', monospace;
    padding: 10px;
}

/* Style table blocks */
.e-block[data-block-type="Table"] {
    margin: 10px 0;
}
```

## Typography Options

### Text Formatting

The BlockEditor supports inline formatting through the toolbar:

```tsx
import { BlockEditorComponent, BlockModel, ContentType } from '@syncfusion/ej2-react-blockeditor';
import * as React from 'react';

function App() {
    const blocks: BlockModel[] = [
        {
            id: 'block-1',
            blockType: 'Paragraph',
            content: [{
                contentType: ContentType.Text,
                content: 'Select text and use the toolbar to: Bold, Italic, Underline'
            }]
        }
    ];

    return <BlockEditorComponent id="editor" blocks={blocks} />;
}
```

### Custom Font Styling

```css
/* Apply custom fonts */
.e-block-editor {
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    font-size: 16px;
    line-height: 1.5;
}

/* Heading styles */
.e-block[data-block-type="Heading"] h1 {
    font-size: 2.5rem;
    font-weight: 700;
}

.e-block[data-block-type="Heading"] h2 {
    font-size: 2rem;
    font-weight: 700;
}

.e-block[data-block-type="Heading"] h3 {
    font-size: 1.5rem;
    font-weight: 600;
}

.e-block[data-block-type="Heading"] h4 {
    font-size: 1.25rem;
    font-weight: 600;
}

/* Quote styling */
.e-block[data-block-type="Quote"] {
    border-left: 4px solid #ccc;
    padding-left: 15px;
    color: #666;
    font-style: italic;
    margin-left: 0;
}

/* Code block styling */
.e-block[data-block-type="Code"] code {
    font-family: 'Courier New', Courier, monospace;
    font-size: 14px;
    line-height: 1.4;
}
```

## Indentation and Nesting

### Indentation Styling

```tsx
const indentedBlocks: BlockModel[] = [
    {
        id: 'level-0',
        blockType: 'BulletList',
        indent: 0,
        content: [{
            contentType: ContentType.Text,
            content: 'Level 0 - Main item'
        }]
    },
    {
        id: 'level-1',
        blockType: 'BulletList',
        indent: 1,
        content: [{
            contentType: ContentType.Text,
            content: 'Level 1 - Nested item'
        }]
    },
    {
        id: 'level-2',
        blockType: 'BulletList',
        indent: 2,
        content: [{
            contentType: ContentType.Text,
            content: 'Level 2 - Deeper nesting'
        }]
    }
];
```

### Nested Block Styling

```css
/* Indent based on nesting level */
.e-block {
    margin-left: 0;
}

.e-block[data-indent="1"] {
    margin-left: 20px;
}

.e-block[data-indent="2"] {
    margin-left: 40px;
}

.e-block[data-indent="3"] {
    margin-left: 60px;
}

/* Nested content styling */
.e-block-children {
    margin-top: 5px;
    margin-bottom: 5px;
}
```

## Placeholder Customization

### Empty Block Placeholders

```tsx
import { BlockEditorComponent, BlockModel } from '@syncfusion/ej2-react-blockeditor';
import * as React from 'react';

function App() {
    const blocks: BlockModel[] = [
        {
            id: 'block-1',
            blockType: 'Paragraph',
            properties: {
                placeholder: 'Start typing your content here...'
            },
            content: []
        }
    ];

    return <BlockEditorComponent id="editor" blocks={blocks} />;
}
```

### Placeholder Styling

```css
/* Global placeholder styling */
.e-block-editor [data-placeholder]::before {
    content: attr(data-placeholder);
    color: #999;
    font-style: italic;
    pointer-events: none;
}

/* Heading placeholder */
.e-block[data-block-type="Heading"][data-placeholder]::before {
    color: #ccc;
    font-size: 1.5rem;
}

/* Code block placeholder */
.e-block[data-block-type="Code"][data-placeholder]::before {
    color: #bbb;
    font-family: 'Courier New', monospace;
}
```

## Dark Mode Support

### Dark Theme Import

```tsx
import '@syncfusion/ej2-base/styles/material-dark.css';
import '@syncfusion/ej2-inputs/styles/material-dark.css';
import '@syncfusion/ej2-popups/styles/material-dark.css';
import '@syncfusion/ej2-buttons/styles/material-dark.css';
import '@syncfusion/ej2-splitbuttons/styles/material-dark.css';
import '@syncfusion/ej2-navigations/styles/material-dark.css';
import '@syncfusion/ej2-dropdowns/styles/material-dark.css';
import '@syncfusion/ej2-react-blockeditor/styles/material-dark.css';

import { BlockEditorComponent } from '@syncfusion/ej2-react-blockeditor';

function App() {
    return <BlockEditorComponent id="editor" />;
}
```

### System Dark Mode Support

```tsx
function AppWithDarkMode() {
    const [isDark, setIsDark] = React.useState(false);

    React.useEffect(() => {
        // Detect system dark mode preference
        const prefersDark = window.matchMedia('(prefers-color-scheme: dark)').matches;
        setIsDark(prefersDark);
        
        // Update document class
        if (prefersDark) {
            document.documentElement.classList.add('dark-mode');
        }
    }, []);

    const toggleDarkMode = () => {
        setIsDark(!isDark);
        document.documentElement.classList.toggle('dark-mode');
    };

    return (
        <div>
            <button onClick={toggleDarkMode}>
                {isDark ? 'Light Mode' : 'Dark Mode'}
            </button>
            <BlockEditorComponent id="editor" />
        </div>
    );
}
```

### Custom Dark Mode Styles

```css
:root {
    --bg-primary: #ffffff;
    --text-primary: #000000;
    --border-color: #cccccc;
}

.dark-mode {
    --bg-primary: #1e1e1e;
    --text-primary: #e0e0e0;
    --border-color: #444444;
}

.e-block-editor {
    background-color: var(--bg-primary);
    color: var(--text-primary);
}

.e-block {
    border-color: var(--border-color);
}

.dark-mode .e-block[data-block-type="Code"] {
    background-color: #2d2d2d;
    color: #e0e0e0;
}
```

## Using Theme Studio

Syncfusion Theme Studio provides an advanced way to create a fully customized theme for the Block Editor and other EJ2 components.

1. Visit the [Syncfusion<sup style="font-size:70%">&reg;</sup> Theme Studio](https://ej2.syncfusion.com/themestudio/).
2. Select a base theme such as Material 3, Fluent 2, Bootstrap 5.3, or Tailwind 3.
3. Customize colors, typography, borders, and component styles.
4. Download the generated CSS file.
5. Include the generated theme in your application.

This approach ensures consistent styling across all Syncfusion components in your application.

## Custom Styling Examples

### Example 1: Info/Warning/Success Blocks

```tsx
function StyledBlocksExample() {
    const blocks: BlockModel[] = [
        {
            id: 'info',
            blockType: 'Paragraph',
            cssClass: 'block-info',
            content: [{
                contentType: ContentType.Text,
                content: 'ℹ️ This is an informational message.'
            }]
        },
        {
            id: 'warning',
            blockType: 'Paragraph',
            cssClass: 'block-warning',
            content: [{
                contentType: ContentType.Text,
                content: '⚠️ This is a warning message.'
            }]
        },
        {
            id: 'success',
            blockType: 'Paragraph',
            cssClass: 'block-success',
            content: [{
                contentType: ContentType.Text,
                content: '✓ Operation completed successfully.'
            }]
        }
    ];

    return (
        <div>
            <style>{`
                .block-info {
                    background-color: #e3f2fd;
                    border-left: 4px solid #2196f3;
                    color: #1976d2;
                    padding: 12px 15px;
                    border-radius: 2px;
                }
                
                .block-warning {
                    background-color: #fff3e0;
                    border-left: 4px solid #ff9800;
                    color: #e65100;
                    padding: 12px 15px;
                    border-radius: 2px;
                }
                
                .block-success {
                    background-color: #e8f5e9;
                    border-left: 4px solid #4caf50;
                    color: #2e7d32;
                    padding: 12px 15px;
                    border-radius: 2px;
                }
            `}</style>
            <BlockEditorComponent id="editor" blocks={blocks} />
        </div>
    );
}
```

### Example 2: Responsive Styling

```css
/* Default (desktop) */
.e-block-editor {
    font-size: 16px;
    line-height: 1.6;
}

/* Tablet */
@media (max-width: 768px) {
    .e-block-editor {
        font-size: 14px;
        padding: 10px;
    }
    
    .e-block {
        margin-left: 0 !important;
    }
}

/* Mobile */
@media (max-width: 480px) {
    .e-block-editor {
        font-size: 13px;
    }
    
    .e-block-header {
        display: none;
    }
}
```

### Example 3: High Contrast Accessibility

```css
/* High contrast mode */
@media (prefers-contrast: more) {
    .e-block-editor {
        border: 2px solid #000;
    }
    
    .e-block {
        border: 1px solid #000;
        padding: 8px;
    }
    
    .block-info {
        background-color: #ffffff;
        border: 2px solid #0066cc;
        font-weight: bold;
    }
}
```
