# Template Customization

## Table of Contents

- [Template Customization](#template-customization)
  - [Cell Template Customization](#cell-template-customization)
    - [Date Header Template](#date-header-template)
    - [Cell Template](#cell-template)
    - [Month Cell Template](#month-cell-template)
    - [Resource Header Template](#resource-header-template)
  - [Event Rendering Templates](#event-rendering-templates)
  - [Quick Info Templates](#quick-info-templates)
  - [Tooltip Customization](#tooltip-customization)

## Cell Template Customization

### Date Header Template

Cells can be customized in all views using templates or the `renderCell` event.

**Setting Cell Dimensions**

Customize cell height and width using the `cssClass` property:

```tsx
<ScheduleComponent cssClass='schedule-cell-dimension'>
  {/* ... */}
</ScheduleComponent>
```

```css
.schedule-cell-dimension.e-schedule .e-vertical-view .e-time-cells-wrap table td,
.schedule-cell-dimension.e-schedule .e-vertical-view .e-work-cells {
  height: 50px;
}

.schedule-cell-dimension.e-schedule .e-month-view .e-work-cells {
  height: 80px;
}
```

**Checking Cell Availability**

Check if time slots are available using `isSlotAvailable`:

```tsx
import { useRef } from 'react';

const scheduleObj = useRef(null);

const onActionBegin = (args) => {
  if (args.requestType === 'eventCreate' && args.data.length > 0) {
    let eventData = args.data[0];
    let eventField = scheduleObj.current.eventFields;
    let startDate = eventData[eventField.startTime];
    let endDate = eventData[eventField.endTime];
    args.cancel = !scheduleObj.current.isSlotAvailable(startDate, endDate);
  }
};
```

### Cell Template

Customize cell appearance using the `cellTemplate` property:

```tsx
const getWorkCellText = (date) => {
  let weekEnds = [0, 6];
  if (weekEnds.indexOf(date.getDay()) >= 0) {
    return "<img src='weekend-icon.svg' />";
  }
  return '';
};

const cellTemplate = (props) => {
  if (props.type === "workCells") {
    return (
      <div 
        className="templatewrap" 
        dangerouslySetInnerHTML={{ __html: getWorkCellText(props.date) }}
      />
    );
  }
  return <div></div>;
};

<ScheduleComponent cellTemplate={cellTemplate}>
  {/* ... */}
</ScheduleComponent>
```

**Using renderCell Event**

An alternative approach using the `renderCell` event:

```tsx
import { createElement } from '@syncfusion/ej2-base';

const onRenderCell = (args) => {
  if (args.elementType === 'workCells' || args.elementType === 'monthCells') {
    let weekEnds = [0, 6];
    if (weekEnds.indexOf(args.date.getDay()) >= 0) {
      let ele = createElement('div', {
        innerHTML: "<img src='weekend-icon.svg' />",
        className: 'templatewrap'
      });
      args.element.appendChild(ele);
    }
  }
};

<ScheduleComponent renderCell={onRenderCell}>
  {/* ... */}
</ScheduleComponent>
```

**RenderCell Element Types**

The `renderCell` event provides access to different element types:

| Element Type | Description |
|--------------|-------------|
| dateHeader | Header cell rendering |
| monthDay | Header cell in month view |
| resourceHeader | Resource header cell |
| alldayCells | All-day cell rendering |
| emptyCells | Empty cell on header bar |
| resourceGroupCells | Work cells for parent resource |
| workCells | Work cell rendering |
| monthCells | Month cell rendering |
| majorSlot | Major time slot cell |
| minorSlot | Minor time slot cell |
| weekNumberCell | Week number cell |

### Month Cell Template

Customize month cells with specific content:

```tsx
const getMonthCellContent = (date) => {
  if (date.getMonth() === 11 && date.getDate() === 25) {
    return '<img src="christmas.svg" />';
  } else if (date.getMonth() === 0 && date.getDate() === 1) {
    return '<img src="newyear.svg" />';
  }
  return '';
};

const cellTemplate = (props) => {
  if (props.type === "monthCells") {
    return (
      <div 
        className="templatewrap" 
        dangerouslySetInnerHTML={{ __html: getMonthCellContent(props.date) }}
      />
    );
  }
  return <div></div>;
};
```

**Customizing Month Cell Header**

Use `cellHeaderTemplate` to customize month cell headers:

```tsx
import { Internationalization } from '@syncfusion/ej2-base';

const instance = new Internationalization();

const getDateHeaderText = (props) => {
  return (
    <div>
      {instance.formatDate(props.date, { skeleton: "Ed" })}
    </div>
  );
};

<ScheduleComponent cellHeaderTemplate={getDateHeaderText}>
  {/* ... */}
</ScheduleComponent>
```

**Customizing Weekend Cell Background**

Customize weekend cell colors:

```tsx
const onRenderCell = (args) => {
  if (args.elementType === "workCells") {
    if (args.date && (args.date.getDay() === 0 || args.date.getDay() === 6)) {
      args.element.style.background = '#ffdea2';
    }
  }
};
```

For month view, use CSS:

```css
.schedule-cell-customization.e-schedule .e-month-view .e-work-cells:not(.e-work-days) {
  background-color: #f08080;
}
```

**Setting Min/Max Date Range**

Restrict date navigation using `minDate` and `maxDate`:

```tsx
<ScheduleComponent 
  minDate={new Date(2017, 4, 17)}
  maxDate={new Date(2018, 5, 17)}
>
  {/* ... */}
</ScheduleComponent>
```

### Resource Header Template

When using resources, you can customize resource headers using the `resourceHeaderTemplate` within the resource configuration:

```tsx
import { ResourcesDirective, ResourceDirective } from '@syncfusion/ej2-react-schedule';

const resourceHeaderTemplate = (props) => {
  return (
    <div className="resource-header">
      <div className="resource-image">
        <img src={props.resourceData.Image} alt={props.resourceData.Text} />
      </div>
      <div className="resource-name">{props.resourceData.Text}</div>
    </div>
  );
};

<ScheduleComponent>
  <ResourcesDirective>
    <ResourceDirective 
      field='OwnerId' 
      title='Owner' 
      name='Owners' 
      dataSource={ownerData} 
      textField='Text' 
      idField='Id' 
      resourceHeaderTemplate={resourceHeaderTemplate}
    />
  </ResourcesDirective>
  {/* ... */}
</ScheduleComponent>
```

## Event Rendering Templates

Customize how events are rendered on the scheduler using the `eventTemplate` property:

```tsx
const eventTemplate = (props) => {
  return (
    <div className="template-wrap" style={{ background: props.Color }}>
      <div className="subject">{props.Subject}</div>
      <div className="time">
        {props.StartTime.toLocaleString()} - {props.EndTime.toLocaleString()}
      </div>
    </div>
  );
};

<ScheduleComponent eventSettings={{ template: eventTemplate }}>
  {/* ... */}
</ScheduleComponent>
```

## Quick Info Templates

Quick info popups appear on single-clicking cells or events. Customize them using the `quickInfoTemplates` property.

**Disabling Quick Info Popups**

```tsx
<ScheduleComponent showQuickInfo={false}>
  {/* ... */}
</ScheduleComponent>
```

**Opening Quick Info on Multiple Cell Selection**

```tsx
<ScheduleComponent quickInfoOnSelectionEnd={true}>
  {/* ... */}
</ScheduleComponent>
```

**Customizing Quick Info Watermark**

```tsx
L10n.load({
  'en-US': {
    'schedule': {
      'addTitle': 'New Title'
    }
  }
});
```

**Custom Quick Info Templates**

```tsx
import { useRef } from 'react';
import { isNullOrUndefined } from '@syncfusion/ej2-base';

const scheduleObj = useRef(null);

const header = (props) => {
  return (
    <div>
      {props.elementType === "cell" ? (
        <div className="e-cell-header e-popup-header">
          <div className="e-header-icon-wrapper">
            <button 
              id="close" 
              className="e-close e-close-icon e-icons" 
              title="Close"
              onClick={buttonClickActions}
            />
          </div>
        </div>
      ) : (
        <div className="e-event-header e-popup-header">
          <div className="e-header-icon-wrapper">
            <button 
              id="close" 
              className="e-close e-close-icon e-icons" 
              title="CLOSE"
              onClick={buttonClickActions}
            />
          </div>
        </div>
      )}
    </div>
  );
};

const content = (props) => {
  return (
    <div>
      {props.elementType === "cell" ? (
        <div className="e-cell-content e-template">
          <form className="e-schedule-form">
            <div>
              <input 
                className="subject e-field e-input" 
                type="text" 
                name="Subject" 
                placeholder="Title" 
              />
            </div>
            <div>
              <input 
                className="location e-field e-input" 
                type="text" 
                name="Location" 
                placeholder="Location" 
              />
            </div>
          </form>
        </div>
      ) : (
        <div className="e-event-content e-template">
          <div className="e-subject-wrap">
            {props.Subject && <div className="subject">{props.Subject}</div>}
            {props.Location && <div className="location">{props.Location}</div>}
            {props.Description && <div className="description">{props.Description}</div>}
          </div>
        </div>
      )}
    </div>
  );
};

const footer = (props) => {
  return (
    <div>
      {props.elementType === "cell" ? (
        <div className="e-cell-footer">
          <div className="left-button">
            <button 
              id="more-details" 
              className="e-event-details" 
              title="Extra Details"
              onClick={buttonClickActions}
            >
              Extra Details
            </button>
          </div>
          <div className="right-button">
            <button 
              id="add" 
              className="e-event-create" 
              title="Add"
              onClick={buttonClickActions}
            >
              Add
            </button>
          </div>
        </div>
      ) : (
        <div className="e-event-footer">
          <div className="left-button">
            <button 
              id="edit" 
              className="e-event-edit" 
              title="Edit"
              onClick={buttonClickActions}
            >
              Edit
            </button>
            {!isNullOrUndefined(props.RecurrenceRule) && props.RecurrenceRule !== "" && (
              <button 
                id="edit-series" 
                className="e-edit-series" 
                title="Edit Series"
                onClick={buttonClickActions}
              >
                Edit Series
              </button>
            )}
          </div>
          <div className="right-button">
            <button 
              id="delete" 
              className="e-event-delete" 
              title="Delete"
              onClick={buttonClickActions}
            >
              Delete
            </button>
            {!isNullOrUndefined(props.RecurrenceRule) && props.RecurrenceRule !== "" && (
              <button 
                id="delete-series" 
                className="e-delete-series" 
                title="Delete Series"
                onClick={buttonClickActions}
              >
                Delete Series
              </button>
            )}
          </div>
        </div>
      )}
    </div>
  );
};

const buttonClickActions = (e) => {
  const action = e.target.id;
  
  switch (action) {
    case "add":
      const cellDetails = scheduleObj.current.getCellDetails(
        scheduleObj.current.getSelectedElements()
      );
      const eventData = scheduleObj.current.eventWindow.getObjectFromFormData(
        "e-quick-popup-wrapper"
      );
      scheduleObj.current.addEvent({
        Subject: eventData.Subject || "Add title",
        StartTime: cellDetails.startTime,
        EndTime: cellDetails.endTime,
        Location: eventData.Location
      });
      break;
    case "edit":
    case "edit-series":
      const event = scheduleObj.current.activeEventData.event;
      const actionType = event.RecurrenceRule ? 
        (action === "edit" ? "EditOccurrence" : "EditSeries") : "Save";
      scheduleObj.current.openEditor(event, actionType);
      break;
    case "delete":
    case "delete-series":
      const deleteEvent = scheduleObj.current.activeEventData.event;
      const deleteType = deleteEvent.RecurrenceRule ? 
        (action === "delete" ? "DeleteOccurrence" : "DeleteSeries") : "Delete";
      scheduleObj.current.deleteEvent(deleteEvent, deleteType);
      break;
    case "more-details":
      const moreDetails = scheduleObj.current.getCellDetails(
        scheduleObj.current.getSelectedElements()
      );
      scheduleObj.current.openEditor(moreDetails, "Add", true);
      break;
  }
  
  scheduleObj.current.closeQuickInfoPopup();
};

const quickInfoTemplates = { 
  header: header, 
  content: content, 
  footer: footer 
};

<ScheduleComponent 
  ref={scheduleObj}
  quickInfoTemplates={quickInfoTemplates}
>
  {/* ... */}
</ScheduleComponent>
```

**Manually Opening Quick Info Popup**

```tsx
const onCellClickButton = () => {
  const cellData = {
    Subject: 'Review Meeting',
    StartTime: new Date(2023, 2, 5, 9, 0, 0),
    EndTime: new Date(2023, 2, 5, 10, 0, 0)
  };
  scheduleObj.current.openQuickInfoPopup(cellData, 'Add');
};

const onEventClickButton = () => {
  const eventData = {
    Id: 1,
    Subject: 'Review Meeting',
    StartTime: new Date(2023, 2, 5, 9, 0, 0),
    EndTime: new Date(2023, 2, 5, 10, 0, 0)
  };
  scheduleObj.current.openQuickInfoPopup(eventData, 'Save');
};
```

**Manually Closing Quick Info Popup**

```tsx
const onCloseQuickInfo = () => {
  scheduleObj.current.closeQuickInfoPopup();
};
```

## Tooltip Customization

Customize tooltips that appear when hovering over events using the `eventRendered` event:

```tsx
import { Tooltip } from '@syncfusion/ej2-popups';

const onEventRendered = (args) => {
  const tooltip = new Tooltip({
    content: `${args.data.Subject}<br/>Start: ${args.data.StartTime.toLocaleString()}<br/>End: ${args.data.EndTime.toLocaleString()}`,
    target: '.e-appointment',
    position: 'TopCenter'
  });
  tooltip.appendTo(args.element);
};

<ScheduleComponent eventRendered={onEventRendered}>
  {/* ... */}
</ScheduleComponent>
```

**More Events Indicator Popup**

When multiple appointments exceed cell height, a "+more" indicator appears. Customize this behavior:

**Preventing More Indicator Popup**

```tsx
const onPopupOpen = (args) => {
  if (args.type === 'EventContainer') {
    args.cancel = true;
  }
};
```

**Customizing More Indicator Popup**

```tsx
import { Internationalization } from '@syncfusion/ej2-base';

const onPopupOpen = (args) => {
  if (args.type === 'EventContainer') {
    let instance = new Internationalization();
    let date = instance.formatDate(args.data.date, { skeleton: 'MMMEd' });
    args.element.querySelector('.e-header-date').innerText = date;
    args.element.querySelector('.e-header-day').innerText = 
      'Event count: ' + args.data.event.length;
  }
};
```

**Preventing More Indicator Popup Display**

```tsx
const onMoreEventsClick = (args) => {
  args.cancel = true;
};

<ScheduleComponent moreEventsClick={onMoreEventsClick}>
  {/* ... */}
</ScheduleComponent>
```

**Navigating to Day View on More Indicator Click**

```tsx
const onMoreEventsClick = (args) => {
  args.isPopupOpen = false;
};

<ScheduleComponent moreEventsClick={onMoreEventsClick}>
  <ViewsDirective>
    <ViewDirective option='Day' />
    <ViewDirective option='Month' />
  </ViewsDirective>
  <Inject services={[Day, Month]} />
</ScheduleComponent>
```
