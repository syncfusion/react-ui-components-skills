# Field Components Panel

The Field Components Panel is the left pane of the React Form Builder. It contains the draggable palette of form fields, layout containers, and pre-built templates.

The panel has **Fields** and **Templates** tabs. The Templates tab is available in Developer mode. Search filters components by label or type; while searching, the accordion becomes a flat result list.

## Toolbox categories

### Basic

`textbox`, `textarea`, `checkbox`, `radio`, `number`, `inputMask`, `dropdown`, `multiselect`, `date`, and `button`.

### Advanced

`checkboxGroup`, `dateTime`, `time`, `dateRange`, `switch`, `rating`, `splitButton`, `rangeSlider`, `signature`, `imageEditor`, `fileUpload`, `colorPicker`, `richTextEditor`, and `dataGrid`.

### Layout

`message`, `panel`, `table`, `tabs`, `card`, and `staticHtml`. Layout components are stored in the schema `layout[]` array rather than in `properties`.

## Templates tab

Pre-built schemas can be dragged from the Templates tab onto the canvas. Configure them with the `formTemplates` prop:

```tsx
import { FormBuilderComponent } from '@syncfusion/ej2-react-form-builder';

const formTemplates = [
  {
    id: 'login form',
    title: 'login',
    schema: {
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
      layout: [{ type: 'field', propertyId: 'emailAddress' }],
      settings: { name: 'Untitled Form' }
    }
  }
];

export default function App() {
  return <FormBuilderComponent formTemplates={formTemplates} />;
}
```

The JavaScript version uses the same `formTemplates` configuration. Dropping a template inserts its schema; submit buttons inside templates are filtered to prevent duplicates.

## Customize toolbox items

Use `toolboxCategories` to control which fields appear in each category:

```tsx
import { FormBuilderComponent } from '@syncfusion/ej2-react-form-builder';
import { ComponentCategory, FormWidgetType } from '@syncfusion/ej2-form-builder';

const toolboxCategories = [
  {
    category: ComponentCategory.Basic,
    items: [FormWidgetType.Textbox, FormWidgetType.Textarea]
  },
  {
    category: ComponentCategory.Advanced,
    items: [FormWidgetType.Date, FormWidgetType.DateRange]
  },
  {
    category: ComponentCategory.Layout,
    items: [FormWidgetType.Panel, FormWidgetType.Card]
  }
];

export default function App() {
  return <FormBuilderComponent toolboxCategories={toolboxCategories} />;
}
```

The JavaScript version uses the same categories and widget values without TypeScript annotations. Fields and templates can be reordered within layout containers through drag and drop.
