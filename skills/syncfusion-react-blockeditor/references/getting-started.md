# Getting Started with Block Editor

## Table of Contents
- [Create a React Application](#create-a-react-application)
- [Adding Syncfusion Block Editor Packages](#adding-syncfusion-block-editor-packages)
- [Adding a CSS Reference](#adding-a-css-reference)
- [Add the Block Editor Component](#add-the-block-editor-component)
- [Basic Implementation](#basic-implementation)
- [Creating Your First Document](#creating-your-first-document)
- [Setting Initial Content](#setting-initial-content)
- [Component Sizing](#component-sizing)
- [Running the Application](#running-the-application)

## Create a React Application

Run the following commands to set up a React application:

```bash
npm create vite@latest my-app -- --template react-ts
```

This command prompts you to configure the React application. Select ESLint and for install with npm, choose No.

As Syncfusion packages are not installed yet, currently, the `No` option will be selected. Then, navigate to the project directory and install the dependencies using the following commands:

```
cd my-app
npm install
```

> **Note:** To set up a React application with Nextjs or Remix, refer to this [documentation](https://ej2.syncfusion.com/react/documentation/getting-started/quick-start) for more details.

## Adding Syncfusion Block Editor Packages

All available Essential JS 2 packages are published in the [`npmjs.com`](https://www.npmjs.com/~syncfusionorg) public registry.

To install the Block Editor component package, use the following command:

```bash
npm install @syncfusion/ej2-react-blockeditor --save
```

## Adding a CSS Reference

Syncfusion provides multiple themes for the Block Editor component. For a complete list of available themes, refer to the [themes packages](https://ej2.syncfusion.com/react/documentation/appearance/theme#theme-packages).

To install the [tailwind3](https://www.npmjs.com/package/@syncfusion/ej2-tailwind3-theme) theme package, use the following command:

```bash
npm install @syncfusion/ej2-tailwind3-theme --save
```

Import the required CSS theme files for the Block Editor and its dependencies in your `src/App.css` file.

```css
@import '../node_modules/@syncfusion/ej2-tailwind3-theme/styles/blockeditor/index.css';
```
> To apply the application-specific styles correctly, import **App.css** into **src/App.tsx** and remove all the default styles from **src/index.css**.

## Add the Block Editor Component

Add the BlockEditorComponent to your application. Replace the contents of `src/App.tsx` with the following code:

```typescript
// Import the BlockEditor.
import { BlockEditorComponent } from '@syncfusion/ej2-react-blockeditor';
import * as React from 'react';
import './App.css';
// To render BlockEditor.
function App() {
  return (
    <BlockEditorComponent id="block-editor"></BlockEditorComponent>
  );
}
export default App;
```

```css
@import '../node_modules/@syncfusion/ej2-tailwind3-theme/styles/blockeditor/index.css';
```

> **Note:** This guide assumes React 18+. The Vite template mounts the app into a `<div id="root">` element in `index.html` via `src/main.tsx` using `createRoot`. Ensure your `main.tsx` renders the `App` component into that node.

## Basic Implementation

The basic setup above provides a functional Block Editor component. Both functional and class components are supported. The `id` prop is required and serves as the unique identifier for the component instance.

This creates an empty block editor where users can start typing or use the "/" command to insert blocks.

## Creating Your First Document

Create a simple React component with the BlockEditor:

## Setting Initial Content

### Using Block Data

To pre-populate the editor with content, use the `blocks` property with an array of BlockModel objects:

```tsx
import { BlockEditorComponent, BlockModel, ContentType } from '@syncfusion/ej2-react-blockeditor';
import * as React from 'react';

function App() {
    const initialBlocks: BlockModel[] = [
        {
            id: 'block-1',
            blockType: 'Heading',
            properties: { level: 1 },
            content: [
                {
                    contentType: ContentType.Text,
                    content: 'Welcome to Block Editor'
                }
            ]
        },
        {
            id: 'block-2',
            blockType: 'Paragraph',
            content: [
                {
                    contentType: ContentType.Text,
                    content: 'This is a sample paragraph with initial content.'
                }
            ]
        },
        {
            id: 'block-3',
            blockType: 'BulletList',
            content: [
                {
                    contentType: ContentType.Text,
                    content: 'First list item'
                }
            ]
        },
        {
            id: 'block-4',
            blockType: 'BulletList',
            content: [
                {
                    contentType: ContentType.Text,
                    content: 'Second list item'
                }
            ]
        }
    ];

    return (
        <BlockEditorComponent
            id="block-editor"
            blocks={initialBlocks}
        />
    );
}

export default App;
```

### Block Properties Explained

Each block requires:
- **blockType** - The type of content (Paragraph, Heading, BulletList, etc.)
- **content** - Array of ContentModel objects containing the text and formatting
- **id** (optional) - Unique identifier for the block
- **properties** (optional) - Type-specific settings (e.g., heading level)

### Content Type Options

The `ContentType` enum defines the four valid content types for any `ContentModel.contentType` field:

```tsx
enum ContentType {
    Text = 'Text',       // Plain or inline-formatted text
    Link = 'Link',       // Hyperlink — requires ILinkContentSettings in properties
    Mention = 'Mention', // User mention — requires IMentionContentSettings in properties
    Label = 'Label'      // Label/tag — requires ILabelContentSettings in properties
}
```

> ⚠️ `Image`, `Code`, and `Embed` are **not** valid `ContentType` values. Images and code are represented as `blockType` values (`'Image'`, `'Code'`), not as content types.

### Block Type Reference

All valid `blockType` values (from the `BlockType` enum):

| Value | Description |
|---|---|
| `'Paragraph'` | Standard text paragraph |
| `'Heading'` | Heading (configure `level: 1–4` in `properties`) |
| `'BulletList'` | Unordered bullet list |
| `'NumberedList'` | Ordered numbered list |
| `'Checklist'` | To-do list with checkboxes (use `properties.isChecked`) |
| `'Code'` | Code block with syntax highlighting (use `properties.language`) |
| `'Table'` | Data table |
| `'Quote'` | Blockquote (supports nested `properties.children`) |
| `'Callout'` | Highlighted callout box |
| `'Image'` | Image block (configure via `imageBlockSettings` prop) |
| `'Divider'` | Horizontal separator line |
| `'CollapsibleHeading'` | Collapsible heading (use `properties.isExpanded`, `properties.children`) |
| `'CollapsibleParagraph'` | Collapsible paragraph (use `properties.isExpanded`, `properties.children`) |
| `'Template'` | Custom template block (use `BlockModel.template`) |

## Component Sizing

Use `width` and `height` props to control the editor container dimensions. Both accept CSS string values or numeric pixel values.

```tsx
import { BlockEditorComponent } from '@syncfusion/ej2-react-blockeditor';
import * as React from 'react';

function App() {
    return (
        <BlockEditorComponent
            id="block-editor"
            width="800px"
            height="600px"
        />
    );
}

export default App;
```

| Prop | Type | Default | Description |
|---|---|---|---|
| `width` | `string \| number` | `'100%'` | Width of the editor container |
| `height` | `string \| number` | `'auto'` | Height of the editor container |

## Run the application

Now, run the `npm run dev` command in your terminal to start the development server.

```
npm run dev
```

Your BlockEditor should now be ready to use! Users can:
- Type directly in the editor
- Press "/" to open the command menu
- Right-click for context menu options
- Drag blocks to reorder them
- Use keyboard shortcuts for formatting
