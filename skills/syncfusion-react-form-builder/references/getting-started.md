# Getting started with React Form Builder

The Syncfusion React Form Builder is a visual form designer. You can drag fields onto a design canvas, configure field properties, preview the result, and export the generated schema for use with the Form Renderer.

This guide uses a Vite + React + TypeScript project.

## Prerequisites

- Install a supported Node.js LTS release and npm or another Node package manager.
- Ensure the React and Syncfusion package versions are compatible.
- This walkthrough assumes React 18 or later.

## Set up the Vite project

Create a React application with JavaScript or TypeScript:

```bash
npm create vite@latest my-project -- --template react
npm create vite@latest my-project -- --template react-ts
```

Navigate to the project directory:

```bash
cd my-project
```

## Install the Form Builder package

```bash
npm install @syncfusion/ej2-react-form-builder
```

With Yarn:

```bash
yarn add @syncfusion/ej2-react-form-builder
```

## Install and import a theme

Install the Material 3 theme:

```bash
npm install @syncfusion/ej2-material3-theme
```

Clear the default content from `src/index.css` if it conflicts with Syncfusion styles. Import the theme from `src/App.css`:

```css
@import "@syncfusion/ej2-material3-theme/styles/material3.css";
```

## Add the Form Builder component

Import `FormBuilderComponent` into `src/App.tsx`:

```tsx
import { FormBuilderComponent } from '@syncfusion/ej2-react-form-builder';
import './App.css';

function App() {
  return <FormBuilderComponent />;
}

export default App;
```

The JavaScript version uses the same component without TypeScript annotations:

```jsx
import { FormBuilderComponent } from '@syncfusion/ej2-react-form-builder';
import './App.css';

function App() {
  return <FormBuilderComponent />;
}

export default App;
```

## Run the application

```bash
npm run dev
```

Vite reports a local URL, typically `http://localhost:5173`. Open it in a browser to view the Form Builder.

## Load an initial form schema

Bind a schema object to the `schema` prop when the customer asks for a Form Builder with an initial form:

```tsx
import { FormBuilderComponent } from '@syncfusion/ej2-react-form-builder';

const formSchema = {
  version: '0.1.0',
  properties: {
    emailAddress: {
      id: 'textbox_1785491685456_167',
      name: 'emailAddress',
      type: 'string',
      label: 'Email Address',
      textboxType: 'email',
      placeholder: 'Enter your email',
      required: true,
      widget: 'textbox'
    }
  },
  layout: [
    { type: 'field', propertyId: 'emailAddress' }
  ],
  settings: {
    name: 'Untitled Form'
  }
};

export default function App() {
  return <FormBuilderComponent schema={formSchema} />;
}
```

The schema contains field definitions in `properties`, the rendering tree in `layout`, and form-wide options in `settings`. Read [form-schema.md](form-schema.md) for the complete schema model.

## Form Builder areas

The Form Builder contains:

1. **Left pane / toolbox** - supported fields that can be dragged onto the canvas.
2. **Design canvas** - the area where fields are arranged.
3. **Right pane** - form settings and selected field properties.
4. **Code view** - the generated form schema.
5. **Preview** - an interactive view of the generated form.

After dropping the first field, use the canvas context menu to add more fields. See [field-components-panel.md](field-components-panel.md), [property-panel.md](property-panel.md), and [preview-tab.md](preview-tab.md) for the next steps.

## Form Builder properties

The React Form Builder exposes these component properties. Use them as props on `FormBuilderComponent`.

| Property | Type | Default | Purpose |
|---|---|---|---|
| `locale` | `string` | `'en-US'` | Sets the locale used by the Form Builder. |
| `allowExport` | `boolean` | `true` | Enables or disables exporting the form schema as JSON. |
| `enablePreview` | `boolean` | `true` | Enables or disables the end-user form preview. |
| `enablePersistence` | `boolean` | `false` | Persists the builder state and form schema across sessions when enabled. |
| `enableRtl` | `boolean` | `false` | Enables right-to-left rendering for RTL languages and locales. |
| `mode` | `'Simple' \| 'Developer'` | `'Developer'` | Selects the builder experience. Simple mode provides basic form creation; Developer mode exposes advanced features and full schema control. |
| `formTemplates` | `CustomFormTemplate[] \| null` | `null` | Supplies custom form templates that can be dragged onto the canvas. |
| `toolboxCategories` | `ToolboxCategory[] \| null` | `null` | Customizes which toolbox components are available and how they are grouped. |
| `toolboxItems` | `ToolboxItemSettingModel[]` | `[]` | Defines the toolbox items shown in the component palette. Use this to customize items and render custom components with templates. |
| `schema` | `Schema \| string \| Record<string, unknown> \| null` | `null` | Loads the form schema to be edited by the Form Builder. The value can be a schema object, JSON string, or `Schema` interface instance. |
| `enableHtmlSanitizer` | `boolean` | `false` | Sanitizes untrusted HTML in custom widget string or script templates when enabled. |

```tsx
<FormBuilderComponent
  locale="en-US"
  allowExport={true}
  enablePreview={true}
  enablePersistence={false}
  enableRtl={false}
  mode="Developer"
  schema={formSchema}
  formTemplates={formTemplates}
  toolboxCategories={toolboxCategories}
  toolboxItems={toolboxItems}
  enableHtmlSanitizer={true}
/>
```

The JavaScript version uses the same props without TypeScript annotations.

### Builder modes

`mode` accepts `Simple` or `Developer`:

- **Simple** provides a simplified interface for basic form creation.
- **Developer** provides advanced features and full control over the form schema.

The Templates tab is available in Developer mode. See [field-components-panel.md](field-components-panel.md) for toolbox and template behavior.

## Form Builder events

### `onFieldDragStarted`

Triggered when a field drag operation starts. The event arguments contain `componentName` (`string | null`), `componentType` (`string`), `sourceContainerId` (`string`), and `isNested` (`boolean`).

### `onFieldDragOver`

Triggered while a field is dragged over a valid drop target. The event arguments contain `isNested` (`boolean`).

### `onFieldDropped`

Triggered when a field is dropped onto a target container. The event arguments contain `componentName` (`string`), `isReordered` (`boolean`), `componentType` (`string`), `sourceContainerId` (`string`), and `targetContainerId` (`string`).

### `onFieldPropertyChanged`

Triggered when a form field property changes. The event arguments contain `propertyName` (`string`), `propertyValue` (`any`), and `fieldName` (`string`).

```tsx
import { FormBuilderComponent } from '@syncfusion/ej2-react-form-builder';

export default function App() {
  function onFieldDragStarted(args: FieldDragStartEventArgs) {
    console.log(args.componentName, args.componentType, args.sourceContainerId, args.isNested);
  }

  function onFieldDragOver(args: FieldDragOverEventArgs) {
    console.log(args.isNested);
  }

  function onFieldDropped(args: FieldDroppedEventArgs) {
    console.log(args.componentName, args.componentType, args.sourceContainerId, args.targetContainerId, args.isReordered);
  }

  function onFieldPropertyChanged(args: PropertyChangedEventArgs) {
    console.log(args.fieldName, args.propertyName, args.propertyValue);
  }

  return (
    <FormBuilderComponent
      onFieldDragStarted={onFieldDragStarted}
      onFieldDragOver={onFieldDragOver}
      onFieldDropped={onFieldDropped}
      onFieldPropertyChanged={onFieldPropertyChanged}
    />
  );
}
```

These event payloads describe builder editing state. They are separate from Form Renderer value-change and submit events.
