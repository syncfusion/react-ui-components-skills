# Getting Started with React Form Renderer

## Table of Contents
- [Overview](#overview)
- [Prerequisites](#prerequisites)
- [Create a Vite project](#create-a-vite-project)
- [Install the packages](#install-the-packages)
- [Import the theme](#import-the-theme)
- [Add the Form Renderer](#add-the-form-renderer)
- [Run the app](#run-the-app)

## Overview

The React Form Renderer builds forms from a JSON schema. The schema declares fields, layout, and behavior; render it with `FormRendererComponent` from `@syncfusion/ej2-react-form-renderer`.

This setup follows the source guide's Vite + React workflow and assumes React 18 or later with TypeScript. The package component API is the same for JavaScript projects; omit TypeScript annotations when using JavaScript.

## Prerequisites

- Install a supported Node.js LTS release and npm or another Node package manager.
- Check the Syncfusion React system requirements and React/package version compatibility for the project.

## Create a Vite project

Choose the JavaScript or TypeScript Vite template:

```bash
npm create vite@latest my-project -- --template react
npm create vite@latest my-project -- --template react-ts
```

Yarn alternatives:

```bash
yarn create vite my-project --template react
yarn create vite my-project --template react-ts
```

During setup, select ESLint if prompted. After creation, enter the project directory:

```bash
cd my-project
```

## Install the packages

Install the React Form Renderer package:

```bash
npm install @syncfusion/ej2-react-form-renderer
```

With Yarn:

```bash
yarn add @syncfusion/ej2-react-form-renderer
```

## Import the theme

This example uses the Material 3 theme package:

```bash
npm install @syncfusion/ej2-material3-theme
```

With Yarn:

```bash
yarn add @syncfusion/ej2-material3-theme
```

Vite's default `src/index.css` styles may conflict with Syncfusion styles; clear its default contents. Import the theme from `src/App.css`:

```css
@import "@syncfusion/ej2-material3-theme/styles/material3.css";
```

## Add the Form Renderer

In `src/App.tsx`, import `FormRendererComponent` and `Schema`, define a unified form schema, and pass it to the component. Listen for successful or unsuccessful submission with the `submit` prop; its event args provide `data` and `isValid`.

```tsx
import { FormRendererComponent, type Schema } from '@syncfusion/ej2-react-form-renderer';
import './App.css';

export default function App() {
  const schema: Schema = {
    version: '0.1.0',
    properties: {
      emailAddress: {
        id: 'textbox_email',
        name: 'emailAddress',
        type: 'string',
        label: 'Email Address',
        textboxType: 'email',
        required: true,
        widget: 'textbox'
      },
      rememberMe: {
        id: 'checkbox_remember',
        name: 'rememberMe',
        type: 'boolean',
        label: 'Remember Me',
        widget: 'checkbox'
      },
      submit: {
        id: 'submit_button',
        name: 'defaultSubmit',
        type: 'button',
        label: 'Submit',
        buttonType: 'submit',
        widget: 'button'
      }
    },
    layout: [
      { type: 'field', propertyId: 'emailAddress' },
      { type: 'field', propertyId: 'rememberMe' },
      { type: 'field', propertyId: 'submit' }
    ],
    settings: { name: 'Contact Form' }
  };

  const onSubmit = (args: { data: Record<string, unknown>; isValid: boolean }) => {
    console.log(args.data, args.isValid);
  };

  return <FormRendererComponent schema={schema} submit={onSubmit} />;
}
```

For a JavaScript `src/App.jsx`, use the same schema and render API without the `Schema` type annotation:

```jsx
import { FormRendererComponent } from '@syncfusion/ej2-react-form-renderer';
import './App.css';

const schema = {
  version: '0.1.0',
  properties: {
    emailAddress: {
      id: 'textbox_email',
      name: 'emailAddress',
      type: 'string',
      label: 'Email Address',
      textboxType: 'email',
      required: true,
      widget: 'textbox'
    },
    submit: {
      id: 'submit_button',
      name: 'defaultSubmit',
      type: 'button',
      label: 'Submit',
      buttonType: 'submit',
      widget: 'button'
    }
  },
  layout: [
    { type: 'field', propertyId: 'emailAddress' },
    { type: 'field', propertyId: 'submit' }
  ],
  settings: { name: 'Contact Form' }
};

export default function App() {
  const onSubmit = (args) => {
    console.log(args.data, args.isValid);
  };

  return <FormRendererComponent schema={schema} submit={onSubmit} />;
}
```

## Run the app

Start Vite:

```bash
npm run dev
```

Vite reports the local URL, usually `http://localhost:5173`. Open it in the browser to view the form. If that port is in use, Vite selects another available port and prints it in the terminal.
