# Exporting and Importing Schemas

The React Form Builder supports exporting the generated form schema for reuse in Form Renderer and importing an existing schema for further editing.

## Export form schema

Use the **Export** button in the Form Builder toolbar to export the form structure, field configuration, and related settings.

## Disable export

Set `allowExport` to `false`:

```tsx
import { FormBuilderComponent } from '@syncfusion/ej2-react-form-builder';

export default function App() {
  return <FormBuilderComponent allowExport={false} />;
}
```

```jsx
import { FormBuilderComponent } from '@syncfusion/ej2-react-form-builder';

export default function App() {
  return <FormBuilderComponent allowExport={false} />;
}
```

The default value of `allowExport` is `true`.

## Import form schema

Assign an existing schema to the `schema` prop:

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
    },
    password: {
      id: 'textbox_1785491685456_537',
      name: 'password',
      type: 'string',
      label: 'Password',
      textboxType: 'password',
      placeholder: 'Enter your Password',
      required: true,
      minLength: 6,
      widget: 'textbox'
    },
    rememberMe: {
      id: 'checkbox_1785491685456_262',
      name: 'rememberMe',
      type: 'boolean',
      label: 'Remember Me',
      widget: 'checkbox'
    },
    submit: {
      id: 'submit_button_initial',
      name: 'defaultFormsubmit',
      type: 'button',
      label: 'Submit',
      buttonType: 'submit',
      widget: 'button',
      style: 'primary',
      disabled: false
    }
  },
  layout: [
    { type: 'field', propertyId: 'emailAddress' },
    { type: 'field', propertyId: 'password' },
    { type: 'field', propertyId: 'rememberMe' },
    { type: 'field', propertyId: 'submit' }
  ],
  settings: {
    name: 'Untitled Form'
  }
};

export default function App() {
  return <FormBuilderComponent schema={formSchema} />;
}
```

The JavaScript version uses the same `schema` prop without TypeScript annotations. This loads the schema so the builder can continue editing the same form definition.
