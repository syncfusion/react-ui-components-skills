# Form Actions

## Table of Contents
- [Overview](#overview)
- [Button types](#button-types)
- [Submit button](#submit-button)
- [Reset button](#reset-button)
- [Custom button action](#custom-button-action)
- [Button group](#button-group)
- [Detecting field changes](#detecting-field-changes)
- [Failure and created events](#failure-and-created-events)
- [Key guidance](#key-guidance)

## Overview

Forms often include action buttons. The Form Renderer follows the schema pattern for actions as well: each button is a field in `properties` with a `buttonType`.

## Button types

| `buttonType` | Behavior | Event |
| --- | --- | --- |
| `submit` | validates and submits the form | `submit` |
| `reset` | restores the form to its initial state | no event |
| `button` | run custom logic | `buttonClick` |

## Submit button

```json
{
  "submit": {
    "id": "submit_button_initial",
    "name": "defaultFormsubmit",
    "type": "button",
    "label": "Submit",
    "buttonType": "submit",
    "widget": "button",
    "style": "primary"
  }
}
```

When the form validates, the `submit` event fires with `args.data` and `args.isValid`.

## Reset button

```json
{
  "reset": {
    "id": "reset_button_initial",
    "name": "defaultFormreset",
    "type": "button",
    "label": "Reset",
    "buttonType": "reset",
    "widget": "button",
    "style": "primary"
  }
}
```

The reset button restores the form to its initial values through native reset behavior.

## Custom button action

Use a plain button for custom business logic and wire it to a React `buttonClick` handler.

```json
{
  "cancel": {
    "id": "cancel_button_initial",
    "name": "defaultFormcancel",
    "type": "button",
    "label": "Cancel",
    "buttonType": "button",
    "widget": "button",
    "style": "secondary"
  }
}
```

## Button group

To render multiple button actions together, define a button field with `allowMultiple` and a `buttonsGroups` array. Each item can set its own action `type`, `label`, `style`, `iconCss`, `iconPosition`, and `disabled` state.

```json
{
  "version": "0.1.0",
  "properties": {
    "button": {
      "id": "button_1790832556007_795",
      "name": "button_270",
      "type": "button",
      "label": "Button",
      "buttonType": "button",
      "widget": "button",
      "allowMultiple": true,
      "style": "primary",
      "buttonsGroups": [
        {
          "label": "Submit",
          "type": "submit",
          "style": "success",
          "iconCss": "e-icons e-check",
          "iconPosition": "Right",
          "disabled": false
        },
        {
          "label": "Reset",
          "type": "reset",
          "style": "error",
          "iconCss": "e-icons e-close",
          "iconPosition": "Right",
          "disabled": false
        },
        {
          "label": "Action",
          "type": "button",
          "style": "information",
          "iconCss": "",
          "iconPosition": "Left",
          "disabled": false
        }
      ]
    },
    "submit": {
      "id": "default-submit-button",
      "name": "defaultSubmit",
      "type": "button",
      "label": "Submit",
      "buttonType": "submit",
      "widget": "button"
    }
  },
  "layout": [
    { "type": "field", "propertyId": "button" },
    { "type": "field", "propertyId": "submit" }
  ],
  "settings": {
    "name": "Untitled Form"
  }
}
```

Include the group field in `layout` as usual. In this example, the group contains submit, reset, and plain button actions; the separate submit field is also included in the form layout.

## Detecting field changes

The `change` event fires when a field value changes due to user input or a program-driven expression update. The event argument contains:

| Member | Description |
| --- | --- |
| `fieldName` | The changed field's unique name / identifier. |
| `label` | The changed field's display label. |
| `value` | The new value. It follows the field type: commonly a string for text, number for numeric fields, boolean for checkbox / switch, and an array for `multiselect` / `checkboxGroup`. |
| `dataModel` | All current form field values. |

On change, the renderer updates form state, reevaluates `expressionValue` expressions that depend on the changed field, reruns dependent `customValidation` rules, then emits the event with `{ fieldName, label, value }`.

```ts
import { FormRendererComponent, type Schema } from '@syncfusion/ej2-react-form-renderer';

export default function App() {
  const schema: Schema = {
    version: '0.1.0',
    properties: {
      emailAddress: {
        id: 'textbox_email',
        name: 'emailAddress',
        type: 'string',
        label: 'Email Address',
        widget: 'textbox'
      }
    },
    layout: [{ type: 'field', propertyId: 'emailAddress' }],
    settings: { name: 'Contact Form' }
  };

  const onChange = (args: any) => {
    console.log(args.fieldName, args.label, args.value, args.dataModel);
  };

  return <FormRendererComponent schema={schema} change={onChange} />;
}
```

`change` does not fire for layout-only nodes (`panel`, `table`, `tabs`, `card`), presentation-only widgets (`staticHtml`, `message`), or buttons. Buttons instead raise `submit` or `buttonClick` according to their action type; reset does not raise an event.

## Key guidance

Use individual button fields for standalone actions or `buttonsGroups` when related actions should be presented together. Use `submit` for final validation, `reset` for clearing user input, and `button` for custom behavior such as navigation or API requests. Keep action fields in the `layout` array so they render in the form.
