# Templates

## Overview

Templates allow custom React UI to replace built-in widget rendering for a field or widget type. Use a template function to render JSX for a third-party control or an application-specific input while keeping the form schema-driven.

## Custom widget setting API

The renderer accepts `customWidgetSettings`, an array of `CustomWidgetSettingModel` entries. Each setting can specify:

| Property | Type | Purpose |
| --- | --- | --- |
| `template` | `string \| function \| JSX.Element` | React template content; provide a function when the template needs field data or event handlers. |
| `type` | `string` | Widget type to target; defaults to an empty string. |
| `templateId` | `string` | Match the field's `templateId`, regardless of the `type` setting; defaults to an empty string. |
| `fieldName` | `string` | Match the field's schema `name`, regardless of the `type` setting; defaults to an empty string. |

`type` applies a template to fields of that widget type. `templateId` and `fieldName` allow narrower targeting without requiring a matching `type` value. The API does not specify precedence when multiple targeting properties are set together, so avoid relying on an undocumented precedence.

The renderer property `enableHtmlSanitizer` enables sanitization for untrusted HTML in custom string/script templates. Its default is `false`; turn it on when custom template content is not trusted.

## Input template contract

When a custom template replaces an editable field, it may render a native input or a third-party/custom React component. The renderer does not require the custom component itself to be a native input, select, or checkbox. Bind the custom control's `value` prop to the renderer-provided `data.value` (for a text input, `value={data.value ?? ''}`), and send user edits back through `setFieldValue` using the field's schema `name`.

For a custom component, use that component's documented value input and change output/event; do not assume every component uses names such as `value` and `valueChange`. The actual interactive control must be keyboard-focusable and receive focus. Do not bind only a decorative wrapper or hidden host input while leaving the custom control disconnected from Form Renderer state. Keep focus on the active control during value updates, and do not force autofocus on render unless the user explicitly requests initial focus.

## Register a React template for a widget type

Pass a `customWidgetSettings` array to `FormRendererComponent`. Set `type` to apply a template function to every field using that widget.

Choose the targeting scope that matches the requested UI customization:

- `type`: replace the rendering for every field of that widget type.
- `fieldName`: replace the rendering for the field with that schema `name`.
- `templateId`: replace the rendering for fields whose schema declares the matching `templateId`.

Multiple entries can target different widget types. Use `type` for shared rendering and `fieldName` or `templateId` when only selected fields need custom rendering.

```ts
import { useRef } from 'react';
import { FormRendererComponent, type Schema } from '@syncfusion/ej2-react-form-renderer';

const schema: Schema = {
  version: '0.1.0',
  properties: {
    emailAddress: {
      id: 'textbox_email',
      name: 'emailAddress',
      type: 'string',
      label: 'Email Address',
      textboxType: 'email',
      widget: 'textbox'
    }
  },
  layout: [{ type: 'field', propertyId: 'emailAddress' }],
  settings: { name: 'Contact Form' }
};

export default function App() {
  const formRendererRef = useRef<FormRendererComponent | null>(null);

  function inputTemplate(data: any) {
    const { fieldData } = data;
    return (
      <input
        type={fieldData.textboxType ?? 'text'}
        id={fieldData.id}
        name={fieldData.name}
        value={data.value ?? ''}
        className="custom-input e-input"
        onChange={(event) => {
          formRendererRef.current?.setFieldValue(fieldData.id, event.target.value);
        }}
      />
    );
  }

  return (
    <FormRendererComponent
      ref={formRendererRef}
      schema={schema}
      customWidgetSettings={[{ type: 'textbox', template: inputTemplate }]}
    />
  );
}
```

## Apply a template to a single field

To target one field instead of all controls of a type, use `fieldName` or `templateId` in a `customWidgetSettings` entry. `fieldName` matches the field's schema `name`; `templateId` matches the same property on the field schema.

```ts
<FormRendererComponent
  ref={formRendererRef}
  schema={schema}
  customWidgetSettings={[
    { fieldName: 'emailAddress', template: inputTemplate },
    { templateId: 'password-template', template: passwordTemplate }
  ]}
/>
```

For `templateId`, add the matching `templateId` value to the field in `properties`. These targeting options apply to React `customWidgetSettings` entries.

## Updating the form value from a custom template

When a template replaces an editable widget, bind the rendered control's current value from `data.value` and send edits back to the renderer through the component ref's `setFieldValue` method. Use the custom component's documented value and change APIs; keep the actual interactive control focusable and do not autofocus it unless requested.

```ts
function onInputChange(event: React.ChangeEvent<HTMLInputElement>, data: any) {
  formRendererRef.current?.setFieldValue(data.fieldData.id, event.target.value);
}
```

## Key guidance

Use custom templates when the default Syncfusion controls do not match the UX requirement or when a React-specific third-party control is needed. Keep template value and change handling synchronized with Form Renderer state.
