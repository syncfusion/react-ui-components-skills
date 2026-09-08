# Editor Customization

## Table of Contents
- [Editor Customization](#editor-customization)
  - [Overview](#overview)
  - [Event Editor Customization](#event-editor-customization)
    - [Default Editor Fields](#default-editor-fields)
    - [Custom Editor Template](#custom-editor-template)
    - [Adding Custom Fields](#adding-custom-fields)
    - [Editor Validation](#editor-validation)

## Overview

The Syncfusion React Scheduler provides extensive customization options for event editors. You can customize editor windows, add custom fields, apply validation rules, and extend editor functionality to match your application's requirements and design standards.

Key customization areas include:
- **Event Editor**: Customize fields, templates, validation, and header/footer sections
- **Custom Fields**: Add custom data fields and resource fields to the editor
- **Validation**: Apply custom validation rules to editor fields

## Event Editor Customization

### Default Editor Fields

The Scheduler displays a detailed event editor window when cells or events are double-clicked. The editor contains default fields like Subject, Location, Start Time, End Time, and Description.

**Changing Editor Window Labels**

You can customize field labels using the `fields` option within `eventSettings`:

```tsx
import * as React from 'react';
import { ScheduleComponent, Day, Week, Month, Inject } from '@syncfusion/ej2-react-schedule';

const App = () => {
  const fieldsData = {
    id: 'Id',
    subject: { name: 'Subject', title: 'Event Name' },
    location: { name: 'Location', title: 'Event Location' },
    description: { name: 'Description', title: 'Event Description' },
    startTime: { name: 'StartTime', title: 'Start Duration' },
    endTime: { name: 'EndTime', title: 'End Duration' }
  };
  
  const eventSettings = { 
    dataSource: scheduleData, 
    fields: fieldsData 
  };

  return (
    <ScheduleComponent eventSettings={eventSettings}>
      <Inject services={[Day, Week, Month]} />
    </ScheduleComponent>
  );
};
```

**Changing Header and Footer Button Text**

Customize the editor window header title and footer button text using localization:

```tsx
import { L10n } from '@syncfusion/ej2-base';

L10n.load({
  'en-US': {
    'schedule': {
      'saveButton': 'Add',
      'cancelButton': 'Close',
      'deleteButton': 'Remove',
      'newEvent': 'Add Event',
    },
  }
});
```

**Customizing Time Duration**

Change the default time duration in the editor window by modifying the `duration` property in the `popupOpen` event:

```tsx
const onPopupOpen = (args) => {
  if (args.type === 'Editor') {
    args.duration = 60; // Set to 60 minutes
  }
};

<ScheduleComponent popupOpen={onPopupOpen}>
  {/* ... */}
</ScheduleComponent>
```

**Preventing Editor Display**

Prevent the editor window from opening by setting `cancel` to `true`:

```tsx
const onPopupOpen = (args) => {
  if (args.type === 'Editor') {
    args.cancel = true;
  }
};
```

**Customizing Timezone Collection**

Customize timezone collections in the editor using the `timezoneDataSource` property:

```tsx
<ScheduleComponent 
  timezoneDataSource={[
    { Value: 'Pacific/Niue', Text: 'Niue' },
    { Value: 'Pacific/Honolulu', Text: 'Hawaii Time' },
    { Value: 'Pacific/Tahiti', Text: 'Tahiti' },
  ]}
>
  {/* ... */}
</ScheduleComponent>
```

### Custom Editor Template

Create a fully customized editor window using the `editorTemplate` property:

```tsx
import { DateTimePickerComponent } from '@syncfusion/ej2-react-calendars';
import { DropDownListComponent } from '@syncfusion/ej2-react-dropdowns';

const editorTemplate = (props) => {
  return (
    props !== undefined ? (
      <table className="custom-event-editor" style={{ width: '100%', padding: '5' }}>
        <tbody>
          <tr>
            <td className="e-textlabel">Summary</td>
            <td colSpan={4}>
              <input 
                id="Summary" 
                className="e-field e-input" 
                type="text" 
                name="Subject" 
                style={{ width: '100%' }} 
              />
            </td>
          </tr>
          <tr>
            <td className="e-textlabel">Status</td>
            <td colSpan={4}>
              <DropDownListComponent 
                id="EventType" 
                placeholder='Choose status' 
                data-name="EventType" 
                className="e-field" 
                style={{ width: '100%' }} 
                dataSource={['New', 'Requested', 'Confirmed']} 
                value={props.EventType || null}
              />
            </td>
          </tr>
          <tr>
            <td className="e-textlabel">From</td>
            <td colSpan={4}>
              <DateTimePickerComponent 
                format='dd/MM/yy hh:mm a' 
                id="StartTime" 
                data-name="StartTime" 
                value={new Date(props.startTime || props.StartTime)} 
                className="e-field"
              />
            </td>
          </tr>
          <tr>
            <td className="e-textlabel">To</td>
            <td colSpan={4}>
              <DateTimePickerComponent 
                format='dd/MM/yy hh:mm a' 
                id="EndTime" 
                data-name="EndTime" 
                value={new Date(props.endTime || props.EndTime)} 
                className="e-field"
              />
            </td>
          </tr>
          <tr>
            <td className="e-textlabel">Reason</td>
            <td colSpan={4}>
              <textarea 
                id="Description" 
                className="e-field e-input" 
                name="Description" 
                rows={3} 
                cols={50} 
                style={{ width: '100%', height: '60px', resize: 'vertical' }}
              />
            </td>
          </tr>
        </tbody>
      </table>
    ) : <div></div>
  );
};

<ScheduleComponent editorTemplate={editorTemplate} showQuickInfo={false}>
  {/* ... */}
</ScheduleComponent>
```

> **Note**: Fields with the `e-field` class are processed automatically for DropDownList, DateTimePicker, MultiSelect, DatePicker, CheckBox, and TextBox components.

**Customizing Header and Footer Templates**

Customize the editor header and footer using `editorHeaderTemplate` and `editorFooterTemplate`:

```tsx
const editorHeaderTemplate = (props) => {
  return (
    <div id="event-header">
      {props !== undefined ? 
        (props.Subject ? <div>{props.Subject}</div> : <div>Create New Event</div>) 
        : <div></div>
      }
    </div>
  );
};

const editorFooterTemplate = () => {
  return (
    <div id="event-footer">
      <div id="verify">
        <input type="checkbox" id="check-box" value="unchecked" />
        <label htmlFor="check-box">Verified</label>
      </div>
      <div id="right-button">
        <button id="Save" className="e-control e-btn e-primary" disabled>
          Save
        </button>
        <button id="Cancel" className="e-control e-btn e-primary">
          Cancel
        </button>
      </div>
    </div>
  );
};

<ScheduleComponent 
  editorHeaderTemplate={editorHeaderTemplate}
  editorFooterTemplate={editorFooterTemplate}
>
  {/* ... */}
</ScheduleComponent>
```

### Adding Custom Fields

Add custom fields to the default editor using the `popupOpen` event:

```tsx
import { createElement } from '@syncfusion/ej2-base';
import { DropDownList } from '@syncfusion/ej2-dropdowns';

const onPopupOpen = (args) => {
  if (args.type === 'Editor') {
    if (!args.element.querySelector('.custom-field-row')) {
      let row = createElement('div', { className: 'custom-field-row' });
      let formElement = args.element.querySelector('.e-schedule-form');
      formElement.firstChild.insertBefore(row, formElement.firstChild.firstChild);
      
      let container = createElement('div', { className: 'custom-field-container' });
      let inputEle = createElement('input', {
        className: 'e-field', 
        attrs: { name: 'EventType' }
      });
      container.appendChild(inputEle);
      row.appendChild(container);
      
      let dropDownList = new DropDownList({
        dataSource: [
          { text: 'Public Event', value: 'public-event' },
          { text: 'Maintenance', value: 'maintenance' },
          { text: 'Commercial Event', value: 'commercial-event' }
        ],
        fields: { text: 'text', value: 'value' },
        value: args.data.EventType,
        floatLabelType: 'Always',
        placeholder: 'Event Type'
      });
      dropDownList.appendTo(inputEle);
    }
  }
};
```

**Adding Resource Fields**

Include resource fields with multiple selection support:

```tsx
import { MultiSelectComponent } from '@syncfusion/ej2-react-dropdowns';

const ownerData = [
  { OwnerText: 'Nancy', Id: 1, OwnerColor: '#ffaa00' },
  { OwnerText: 'Steven', Id: 2, OwnerColor: '#f8a398' },
  { OwnerText: 'Michael', Id: 3, OwnerColor: '#7499e1' }
];

const editorTemplate = (props) => {
  return (
    <table className="custom-event-editor">
      <tbody>
        <tr>
          <td className="e-textlabel">Owner</td>
          <td colSpan={4}>
            <MultiSelectComponent 
              className="e-field" 
              placeholder='Choose owner' 
              data-name="OwnerId" 
              dataSource={ownerData} 
              fields={{ text: 'OwnerText', value: 'Id' }} 
              value={props.OwnerId} 
            />
          </td>
        </tr>
        {/* Other fields */}
      </tbody>
    </table>
  );
};
```

**Adding Recurrence Options**

Include recurrence editor in custom template:

```tsx
import { RecurrenceEditorComponent } from '@syncfusion/ej2-react-schedule';

const editorTemplate = (props) => {
  return (
    <table className="custom-event-editor">
      <tbody>
        {/* Other fields */}
        <tr>
          <td className="e-textlabel">Recurrence</td>
          <td colSpan={4}>
            <RecurrenceEditorComponent id="RecurrenceEditor" />
          </td>
        </tr>
      </tbody>
    </table>
  );
};

const onPopupClose = (args) => {
  if (args.type === 'Editor' && args.data) {
    args.data.RecurrenceRule = recurrObject.current.value;
  }
};
```

### Editor Validation

Apply validation rules to editor fields using the `fields` property:

```tsx
const minValidation = (args) => {
  return args['value'].length >= 5;
};

const fieldsData = {
  id: 'Id',
  subject: { 
    name: 'Subject', 
    validation: { required: true } 
  },
  location: { 
    name: 'Location', 
    validation: { required: true } 
  },
  description: {
    name: 'Description', 
    validation: {
      required: true, 
      minLength: [minValidation, 'Need at least 5 letters']
    }
  },
  startTime: { 
    name: 'StartTime', 
    validation: { required: true } 
  },
  endTime: { 
    name: 'EndTime', 
    validation: { required: true } 
  }
};

const eventSettings = { 
  dataSource: scheduleData, 
  fields: fieldsData 
};
```

**Validating Custom Template Fields**

Apply validation to custom template fields:

```tsx
import { FormValidator } from '@syncfusion/ej2-inputs';
import { isNullOrUndefined } from '@syncfusion/ej2-base';

const onPopupOpen = (args) => {
  if (args.type === 'Editor') {
    let statusElement = args.element.querySelector('#EventType');
    if (statusElement) {
      statusElement.setAttribute('name', 'EventType');
    }
    
    if (!isNullOrUndefined(document.getElementById("EventType_Error"))) {
      document.getElementById("EventType_Error").style.display = "none";
    }
    
    let formElement = args.element.querySelector('.e-schedule-form');
    let validator = formElement.ej2_instances[0];
    validator.addRules('EventType', { required: true });
  }
};
```

**Saving Custom Editor Data**

Use the `popupClose` event to save customized editor data:

```tsx
const onPopupClose = (args) => {
  if (args.type === 'Editor' && !isNullOrUndefined(args.data)) {
    let subjectElement = args.element.querySelector('#Summary');
    if (subjectElement) {
      args.data.Subject = subjectElement.value;
    }
    
    let statusElement = args.element.querySelector('#EventType');
    if (statusElement) {
      args.data.EventType = statusElement.value;
    }
    
    args.data.StartTime = startObj.current.value;
    args.data.EndTime = endObj.current.value;
    
    let descriptionElement = args.element.querySelector('#Description');
    if (descriptionElement) {
      args.data.Description = descriptionElement.value;
    }
  }
};
```

**Manually Closing Editor**

Close the editor programmatically using the `closeEditor` method:

```tsx
import { useRef } from 'react';

const scheduleObj = useRef(null);

const onCloseEditor = () => {
  scheduleObj.current.closeEditor();
};

<ScheduleComponent ref={scheduleObj}>
  {/* ... */}
</ScheduleComponent>
```
