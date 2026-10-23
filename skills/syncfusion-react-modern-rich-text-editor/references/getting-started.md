# Getting Started with Modern Rich Text Editor

## Table of Contents
- [Prerequisites](#prerequisites)
- [Install the Syncfusion CLI](#install-the-syncfusion-cli)
- [Set up the Vite project using Syncfusion CLI](#set-up-the-vite-project-using-syncfusion-cli)
- [Create a React Application](#create-a-react-application)
- [Adding Syncfusion Modern Rich Text Editor Package](#adding-syncfusion-modern-rich-text-editor-package)
- [Adding CSS Reference](#adding-css-reference)
- [Adding the Modern Rich Text Editor Component](#adding-the-modern-rich-text-editor-component)
- [Run the Application](#run-the-application)
- [Basic Setup](#basic-setup)
- [Set Editor Content](#set-editor-content)
- [Set Content Format](#set-content-format)
- [Configure Toolbar Options](#configure-toolbar-options)
- [Configure Image Settings](#configure-image-settings)
- [Set Placeholder Text](#set-placeholder-text)
- [Configure Auto-Save Behavior](#configure-auto-save-behavior)

The React Modern Rich Text Editor is a WYSIWYG editor that provides a modern editing experience for creating, editing, and formatting rich text content. This section explains how to create a simple Modern Rich Text Editor using a Vite-based React project.

## Prerequisites

- [Node.js 24+](https://nodejs.org/en) (LTS recommended).
- Syncfusion CLI.

This guide uses Vite as the bundler and development environment. Install Node.js `24.13.0` or higher before proceeding. For detailed information about Vite's capabilities and configuration options, refer to the [Vite documentation](https://vitejs.dev/).

## Install the Syncfusion CLI

Install the Syncfusion CLI globally using the following command:

```bash
npm install -g @syncfusion/syncfusion-cli
```

## Set up the Vite project using Syncfusion CLI

You can create a React Vite application using the Syncfusion CLI. The CLI provides two ways to create a project:

### Non-interactive mode

Non-interactive mode allows you to create a project directly using a single command with the required command-line arguments.

```CMD
sf new my-app --framework react --type ts --template rich-text-editor-ui --theme tailwind3
```

In this mode, the project configuration is passed directly in the command. The above command creates a React Vite application configured with the Syncfusion® Modern Rich Text Editor component.

### Interactive mode

Interactive mode guides you through the project creation process with step-by-step prompts.

```CMD
sf
```

When you run the `sf` command, the CLI prompts you to select the required project configuration. To create a React Vite application with the Syncfusion® Modern Rich Text Editor component, select the following options:

```CMD
√ Project name? ... my-app
√ Choose Framework: » React
√ Choose Build Tool: » Vite
√ Choose Language: » Typescript
√ Choose Template: » Rich Text Editor UI
√ Choose Theme: » Tailwind3
√ Choose Style Format: » CSS
√ Would you like to integrate the Syncfusion MCP Server (AI Assistant) into this project? ... no
√ Would you like to install Syncfusion Component Skills for AI-powered development? ... no
√ Install dependencies and start app now? ... no
```

The above selections generate a React Vite application configured with the Syncfusion® Modern Rich Text Editor component. You can choose different values for language, theme, style format, MCP setup, and skills installation based on your project requirements.

The Syncfusion® CLI creates the project with a predefined template. After the project is generated, you can customize or replace the component code based on your application requirements.

Once the project is created, navigate to the project directory and run the following commands in your terminal.

```CMD
cd my-app
npm install
npm run dev
```

---

## Create a React Application

Run the following commands to set up a React application:

```bash
npm create vite@latest my-app -- --template react-ts
```

This command prompts you to configure the React application. When prompted to choose a linter, select either Oxlint or ESLint based on your preference.

As Syncfusion packages are not installed yet, currently, the `No` option will be selected. Then, navigate to the project directory and install the dependencies using the following commands:

```bash
cd my-app
npm install
```

## Adding Syncfusion® Modern Rich Text Editor Package

All the available Essential® JS 2 packages are published in the [`npmjs.com`](https://www.npmjs.com/~syncfusionorg) public registry.
To install Modern Rich Text Editor component, use the following command

```bash
npm install @syncfusion/ej2-react-richtexteditor-ui
```

## Adding CSS Reference

Syncfusion provides multiple themes for the Modern Rich Text Editor component. For a complete list of available themes, refer to the [themes packages](https://ej2.syncfusion.com/react/documentation/appearance/theme#theme-packages).

To apply the [Tailwind 3](https://www.npmjs.com/package/@syncfusion/ej2-tailwind3-theme) theme, install the corresponding theme package by using the following command:

```bash
npm install @syncfusion/ej2-tailwind3-theme
```

The installed theme package includes an `index.css` file that automatically imports all the required dependency styles. Import the following stylesheet into **src/App.css**:

```css
@import '../node_modules/@syncfusion/ej2-tailwind3-theme/styles/richtexteditor-ui/index.css';
```

> **Note:** To apply the application-specific styles correctly, import **App.css** into **src/App.tsx** and remove all the default styles from **src/index.css**.

## Adding the Modern Rich Text Editor Component

Now, you can start adding the React Modern Rich Text Editor component in the application. For getting started, replace the default Vite template content in **src/App.tsx** with the following sample.

```tsx
import * as React from 'react';
import { RichTextEditorUIComponent } from '@syncfusion/ej2-react-richtexteditor-ui';
import './App.css';

function App() {
    return (
        <RichTextEditorUIComponent placeholder='Type something.' />
    );
}

export default App;
```

```css
@import '../node_modules/@syncfusion/ej2-tailwind3-theme/styles/richtexteditor-ui/index.css';
```

## Run the Application

Now run the `npm run dev` command in the console to start the development server. This command compiles your code and serves the application locally, opening it in the browser.

```bash
npm run dev
```

---

## Basic Setup

After initializing the Modern Rich Text Editor (see [Adding the Modern Rich Text Editor Component](#adding-the-modern-rich-text-editor-component)), configure the properties that define its initial content, content format, toolbar options, image handling behavior, and editing experience. These settings establish the editor's default configuration and provide a foundation for further customization based on application requirements.

```tsx
import * as React from 'react';
import { RichTextEditorUIComponent } from '@syncfusion/ej2-react-richtexteditor-ui';

const hostUrl: string = 'https://services.syncfusion.com/js/production/';

const toolbarSettings = {
    items: ['Bold', 'Italic', 'Underline', '|', 'Formats', 'Alignment', '|', 'Link', 'Image', 'Table', '|', 'Undo', 'Redo']
};

const imageSettings = {
    allowedTypes: ['.jpg', '.jpeg', '.png', '.gif', '.bmp', '.webp'],
    maxFileSize: 30000000,
    uploadUrl: hostUrl + 'api/RichTextEditor/SaveFile',
    imageUrl: hostUrl + 'RichTextEditor/'
};

function App() {
    return (
        <RichTextEditorUIComponent
            value='<p>Getting started with the Rich Text Editor UI.</p>'
            valueFormat='html'
            placeholder='Type something.'
            toolbarSettings={toolbarSettings}
            imageSettings={imageSettings}
        />
    );
}

export default App;
```

## Set Editor Content

Configure the `value` property to load content when the editor is initialized. This is commonly used to display existing content for editing or to provide starter content. In React, use state management to track and update the editor content.

```tsx
const [value, setValue] = React.useState('<p>Getting started with the Rich Text Editor UI.</p>');

<RichTextEditorUIComponent
    value={value}
    onChange={(e: any) => setValue(e.value)}
/>
```

## Set Content Format

Before setting editor content, choose the appropriate `valueFormat`. Use `'html'` when working with HTML strings and `'json'` when storing or exchanging content using the editor's structured document model.

```tsx
<RichTextEditorUIComponent
    valueFormat="html"
/>
```

### Supported Values

- `'json'` - Uses the structured document model.
- `'html'` - Uses HTML string content.

> **Note**: `valueFormat` is set to **'json'** by default. Set it to **'html'** when loading editor content as an HTML string.

## Configure Toolbar Options

Configure `toolbarSettings.items` to display only the editing tools required by your application. Keeping the toolbar focused helps simplify the editing experience and reduces unnecessary commands.

```tsx
toolbarSettings={{
    items: [
        'Bold', 'Italic', 'Underline', '|',
        'Formats', 'Alignment', '|',
        'Link', 'Image', 'Table', '|',
        'Undo', 'Redo'
    ]
}}
```

For the full list of built-in toolbar items and layout options, see [toolbar.md](toolbar.md).

## Configure Image Settings

Configure `imageSettings` to control how images are uploaded and validated. You can specify accepted file types, file size limits, and server endpoints used for upload and removal operations.

```tsx
imageSettings={{
    allowedTypes: ['.jpg', '.jpeg', '.png', '.gif', '.bmp', '.webp'],
    maxFileSize: 30000000,
    uploadUrl: 'https://api.example.com/images/upload',
    imageUrl: '/uploads/',
    removeUrl: 'https://api.example.com/images/remove'
}}
```

See [image.md](image.md) for upload, storage, display, and quick toolbar configuration beyond this basic validation.

## Set Placeholder Text

Use the `placeholder` property to display instructional text when the editor is empty. This helps users understand what content is expected before they start typing.

```tsx
<RichTextEditorUIComponent
    placeholder="Type something."
/>
```

## Configure Auto-Save Behavior

When `enableAutoSave` is enabled, use `saveInterval` to control how long the editor waits before automatically saving unsaved changes. The value is specified in milliseconds and is triggered after the user becomes idle.

```tsx
<RichTextEditorUIComponent
    enableAutoSave={true}
    saveInterval={1000}
/>
```