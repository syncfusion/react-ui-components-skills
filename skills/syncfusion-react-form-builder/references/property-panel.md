# Property Panel

The Property Panel is the right pane of the React Form Builder. It appears when a field is selected on the design canvas. When no field is selected, the right pane displays form settings.

## When the property panel appears

The property panel opens when:

1. A new field is dropped onto the canvas.
2. An existing field is edited.
3. The widget type of a selected field is switched in Developer mode.

Closing the editor returns the right pane to the Form Settings panel.

## Elements in the property panel

The available tabs depend on the selected field:

- **Display** - labels, placeholders, default values, layout, and styling.
- **Validation** - required, length, pattern, and custom validation settings.
- **Conditions** - conditional visibility, required, disabled, read-only, set-value, choice-based, and conditional-data rules.
- **Layout** - field size and HTML attributes.
- **Data** - data source and columns for Data Grid fields.

## Real-time auto-save

Property changes are applied immediately. There is no Save button; the design canvas reflects each change as it is made.

## Add a property

Use `setProperty` on a Form Builder ref to add a property for a built-in control or a custom template:

```tsx
import { useEffect, useRef } from 'react';
import { FormBuilderComponent } from '@syncfusion/ej2-react-form-builder';
import { FormWidgetType } from '@syncfusion/ej2-form-builder';

export default function App() {
  const formBuilderRef = useRef<FormBuilderComponent | null>(null);

  useEffect(() => {
    formBuilderRef.current?.setProperty(FormWidgetType.Textarea, {
      key: 'enableRtl',
      label: 'Enable RTL',
      type: 'boolean',
      default: false
    });
  }, []);

  return <FormBuilderComponent ref={formBuilderRef} />;
}
```

The JavaScript version uses `useRef(null)` and the same `setProperty` call.

## Hide a property

Use `getProperty`, set `visible` to `false`, and call `refresh()`:

```tsx
import { useEffect, useRef } from 'react';
import { FormBuilderComponent } from '@syncfusion/ej2-react-form-builder';
import { FormWidgetType } from '@syncfusion/ej2-form-builder';

export default function App() {
  const formBuilderRef = useRef<FormBuilderComponent | null>(null);

  useEffect(() => {
    const labelProperty = formBuilderRef.current?.getProperty(FormWidgetType.Textarea, 'label');
    if (labelProperty) {
      labelProperty.visible = false;
      formBuilderRef.current?.refresh();
    }
  }, []);

  return <FormBuilderComponent ref={formBuilderRef} />;
}
```
