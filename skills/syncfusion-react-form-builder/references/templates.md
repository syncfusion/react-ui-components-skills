# Templates in React Form Builder

Templates display third-party components inside React Form Builder fields. Configure a template in the `toolboxItems` array and map it to a widget with the `type` property.

## Add a template

The template function receives field data. Call `setFieldValue` when the custom control changes:

```tsx
import { useRef } from 'react';
import { FormBuilderComponent } from '@syncfusion/ej2-react-form-builder';
import { FormWidgetType } from '@syncfusion/ej2-form-builder';

const formSchema = {
  version: '0.1.0',
  properties: {
    emailAddress: {
      id: 'textbox_1785491685456_167',
      name: 'emailAddress',
      type: 'string',
      label: 'Email Address',
      textboxType: 'email',
      required: true,
      placeholder: 'Enter your email',
      widget: 'textbox'
    }
  },
  layout: [{ type: 'field', propertyId: 'emailAddress' }],
  settings: { name: 'Untitled Form' }
};

export default function App() {
  const formBuilderRef = useRef<FormBuilderComponent | null>(null);

  function inputTemplate(data: any) {
    const { fieldData } = data;
    return (
      <input
        type={fieldData.textboxType ?? 'text'}
        id={fieldData.id}
        name={fieldData.name}
        placeholder={fieldData.placeholder}
        className="custom-input e-input"
        onChange={(event) => {
          formBuilderRef.current?.setFieldValue(fieldData.id, event.target.value);
        }}
        onBlur={(event) => {
          formBuilderRef.current?.setFieldValue(fieldData.id, event.target.value);
        }}
      />
    );
  }

  return (
    <FormBuilderComponent
      ref={formBuilderRef}
      schema={formSchema}
      allowExport={false}
      toolboxItems={[{ type: FormWidgetType.Textbox, template: inputTemplate }]}
    />
  );
}
```

The JavaScript version uses the same template function without TypeScript type annotations. The custom component renders after its field is dragged onto the design canvas.

## Add template properties

Use `setProperty` to expose third-party component properties in the property panel. See [property-panel.md](property-panel.md).

## Export templates

The template implementation is not serialized in the exported schema. A `templateId` property in `toolboxItems` identifies the template mapped to the corresponding element. Form Renderer requires additional configuration to render the template at runtime.
