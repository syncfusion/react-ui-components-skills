# Event Customization

## Table of Contents

- [Event Customization](#event-customization)
  - [Event Fields](#event-fields)
    - [Built-in Fields](#built-in-fields)
    - [Custom Fields](#custom-fields)
    - [Field Settings](#field-settings)
  - [Event Customization](#event-customization)
    - [Using Templates](#using-templates)
    - [Using Event Rendered Event](#using-event-rendered-event)
    - [Using CSS Class](#using-css-class)
  - [Advanced Features](#advanced-features)
    - [Block Dates and Times](#block-dates-and-times)
    - [Readonly Events](#readonly-events)
    - [Event Overlapping](#event-overlapping)
    - [Inline Editing](#inline-editing)
    - [Event Tooltips](#event-tooltips)
  - [Troubleshooting and Edge Cases](#troubleshooting-and-edge-cases)

## Event Fields

### Built-in Fields

Map event data to Scheduler using these built-in field properties:

| Field | Description | Required |
|-------|-------------|----------|
| id | Unique identifier for the event | Yes (for CRUD) |
| subject | Summary text of the event | No |
| startTime | Event start time | Yes |
| endTime | Event end time | Yes |
| startTimezone | IANA timezone for start time | No |
| endTimezone | IANA timezone for end time | No |
| location | Location text | No |
| description | Event description | No |
| isAllDay | Whether event is all-day | No |
| recurrenceID | Parent event ID for edited occurrences | No |
| recurrenceRule | Recurrence rule string | No |
| recurrenceException | Exception dates in UTC format | No |
| isReadonly | Make event read-only | No |
| isBlock | Block time ranges | No |

### Custom Fields

Add custom fields beyond default fields without needing to map them in `eventSettings`:

```typescript
const data: Object[] = [{
  Id: 2,
  Subject: 'Meeting',
  StartTime: new Date(2018, 1, 15, 10, 0),
  EndTime: new Date(2018, 1, 15, 12, 30),
  Status: 'Completed',
  Priority: 'High'
}];
```

**Custom field sorting:**

Sort overlapping events by custom fields using `sortComparer`:

```typescript
const comparerFun = (args: Record<string, any>[]): Record<string, any>[] => {
  args.sort((event1, event2) => 
    event1.RankId.localeCompare(event2.RankId, undefined, { numeric: true })
  );
  return args;
};

const eventSettings: EventSettingsModel = { 
  dataSource: data, 
  sortComparer: comparerFun 
};
```

### Field Settings

Customize field properties with additional settings:

```typescript
const fieldsData = {
  id: 'TravelId',
  subject: { 
    name: 'TravelSummary', 
    title: 'Summary', 
    default: 'Add Summary' 
  },
  location: { 
    name: 'Source', 
    default: 'USA',
    validation: { required: true }
  },
  startTime: { name: 'DepartureTime' },
  endTime: { name: 'ArrivalTime' }
};

const eventSettings: EventSettingsModel = { 
  dataSource: data, 
  fields: fieldsData 
};
```

## Event Customization

### Using Templates

Customize event appearance using templates:

```typescript
const eventTemplate = (props) => {
  const getTimeString = (value: Date) => {
    return new Internationalization().formatDate(value, { skeleton: 'hm' });
  };
  
  return (
    <div className="template-wrap" style={{ background: props.SecondaryColor }}>
      <div className="subject" style={{ background: props.PrimaryColor }}>
        {props.Subject}
      </div>
      <div className="time" style={{ background: props.PrimaryColor }}>
        Time: {getTimeString(props.StartTime)} - {getTimeString(props.EndTime)}
      </div>
    </div>
  );
};

const eventSettings: EventSettingsModel = { 
  dataSource: data, 
  template: eventTemplate 
};
```

### Using Event Rendered Event

Customize events before rendering using the `eventRendered` event:

```typescript
const onEventRendered = (args: EventRenderedArgs): void => {
  const categoryColor: string = args.data.CategoryColor;
  if (categoryColor) {
    if (scheduleObj.current.currentView === 'Agenda') {
      (args.element.firstChild as HTMLElement).style.borderLeftColor = categoryColor;
    } else {
      args.element.style.backgroundColor = categoryColor;
    }
  }
};
```

### Using CSS Class

Apply custom styles using the `cssClass` property:

```typescript
<ScheduleComponent cssClass='custom-class' eventSettings={eventSettings}>
  <Inject services={[Day, Week, Month]} />
</ScheduleComponent>
```

```css
.custom-class .e-appointment {
  background-color: #ff6347;
  color: white;
}
```

## Advanced Features

### Block Dates and Times

Block specific time ranges by setting the `isBlock` field to `true`:

```typescript
const blockData: Object[] = [{
  Id: 1,
  Subject: 'Blocked Time',
  StartTime: new Date(2018, 1, 15, 9, 30),
  EndTime: new Date(2018, 1, 15, 11, 0),
  IsBlock: true
}];
```

**Block recurring time ranges:**

```typescript
const blockData: Object[] = [{
  Id: 1,
  Subject: 'Lunch Break',
  StartTime: new Date(2018, 1, 15, 12, 0),
  EndTime: new Date(2018, 1, 15, 13, 0),
  RecurrenceRule: 'FREQ=DAILY;INTERVAL=1;COUNT=5',
  IsBlock: true
}];
```

### Readonly Events

Make the entire Scheduler read-only:

```typescript
<ScheduleComponent readonly={true} eventSettings={eventSettings}>
  <Inject services={[Day, Week, Month]} />
</ScheduleComponent>
```

Make specific events read-only:

```typescript
const readOnlyData: Object[] = [{
  Id: 1,
  Subject: 'Past Event',
  StartTime: new Date(2018, 1, 10, 10, 0),
  EndTime: new Date(2018, 1, 10, 12, 0),
  IsReadonly: true
}];
```

**Restrict CRUD on specific time slots:**

```typescript
const onActionBegin = (args: ActionEventArgs): void => {
  if (args.requestType === 'eventCreate') {
    const eventData = args.data[0];
    const startDate = eventData[scheduleObj.current.eventFields.startTime];
    const endDate = eventData[scheduleObj.current.eventFields.endTime];
    args.cancel = !scheduleObj.current.isSlotAvailable(startDate, endDate);
  }
};
```

### Event Overlapping

Prevent overlapping events using the `allowOverlap` property:

```typescript
<ScheduleComponent allowOverlap={false} eventSettings={eventSettings}>
  <Inject services={[Day, Week, Month, Resize, DragAndDrop]} />
</ScheduleComponent>
```

**Check overlaps beyond visible range:**

```typescript
const checkOverlap = (args: ActionEventArgs): Promise<boolean> => {
  return new Promise((resolve) => {
    const eventsToCheck = Array.isArray(args.data) ? args.data : [args.data];
    const overlappingEvents = allEvents.filter(event =>
      eventsToCheck.some(newEvent =>
        new Date(event.StartTime) < newEvent.EndTime &&
        new Date(event.EndTime) > newEvent.StartTime &&
        event.Id !== newEvent.Id
      )
    );
    
    const result = overlappingEvents.length === 0;
    if (!result) {
      const popupArgs: PopupOpenEventArgs = {
        type: 'OverlapAlert',
        data: eventsToCheck,
        overlapEvents: overlappingEvents
      };
      scheduleObj.current.openOverlapAlert(popupArgs);
    }
    resolve(result);
  });
};

const onActionBegin = (args: ActionEventArgs): void => {
  if (args.requestType === 'eventCreate' || args.requestType === 'eventChange') {
    args.promise = checkOverlap(args);
  }
};
```

### Inline Editing

Enable inline editing for quick subject updates:

```typescript
<ScheduleComponent allowInline={true} eventSettings={eventSettings}>
  <Inject services={[Day, Week, Month]} />
</ScheduleComponent>
```

Single-click on a cell to add an event inline, or single-click on an event subject to edit it inline. Press Enter to save.

### Event Tooltips

Enable tooltips for events:

```typescript
const eventSettings: EventSettingsModel = { 
  dataSource: data, 
  enableTooltip: true 
};
```

**Custom tooltip template:**

```typescript
const tooltipTemplate = (props) => {
  return (
    <div className="tooltip-wrap">
      <div className="name">{props.Subject}</div>
      <div className="city">{props.City}</div>
      <div className="time">From: {props.StartTime.toLocaleString()}</div>
      <div className="time">To: {props.EndTime.toLocaleString()}</div>
    </div>
  );
};

const eventSettings: EventSettingsModel = {
  dataSource: data,
  enableTooltip: true,
  tooltipTemplate: tooltipTemplate
};
```

**Prevent tooltip for specific events:**

```typescript
const onTooltipOpen = (args): void => {
  if (args.data.Subject === 'Vacation') {
    args.cancel = true;
  }
};
```

## Troubleshooting and Edge Cases

### Setting Minimum Height for Short Events

Set minimum height for appointments when duration is less than one slot:

```typescript
const onEventRendered = (args: EventRenderedArgs): void => {
  const cellHeight = scheduleObj.current.element.querySelector('.e-work-cells').offsetHeight;
  const duration = (args.data.EndTime.getTime() - args.data.StartTime.getTime()) / (60 * 1000);
  const appHeight = duration * (cellHeight * scheduleObj.current.timeScale.slotCount) / 
                    scheduleObj.current.timeScale.interval;
  args.element.style.height = appHeight + 'px';
};
```

### Appointments Occupying Entire Cell

Make events occupy full cell height without header:

```typescript
const eventSettings: EventSettingsModel = { 
  dataSource: data, 
  enableMaxHeight: true,
  enableIndicator: false // Hide more indicator
};
```

### Limiting Maximum Events Per Row

Limit concurrent events displayed per row (Month and Timeline views):

```typescript
<ViewsDirective>
  <ViewDirective option='Month' maxEventsPerRow={3} />
</ViewsDirective>
```

### Differentiate Past Time Events

Style past events differently:

```typescript
const onEventRendered = (args: EventRenderedArgs): void => {
  if (args.data.EndTime < scheduleObj.current.selectedDate) {
    args.element.classList.add('e-past-app');
  }
};
```

```css
.e-past-app {
  opacity: 0.5;
}
```

### Retrieve Event Details from UI

Get event details from appointment element:

```typescript
const onEventClick = (args: EventClickArgs): void => {
  const event = scheduleObj.current.getEventDetails(args.element);
  console.log('Subject:', event.Subject);
};
```

### Get Current View Events

Retrieve appointments in current view:

```typescript
const getCurrentEvents = (): void => {
  const events = scheduleObj.current.getCurrentViewEvents();
  console.log('Current view events:', events.length);
};
```

### Get All Events

Get entire appointment collection:

```typescript
const getAllEvents = (): void => {
  const events = scheduleObj.current.getEvents();
  console.log('Total events:', events.length);
};
```

### Refresh Events

Refresh events without re-rendering entire Scheduler:

```typescript
scheduleObj.current.refreshEvents();
```

### Appointment Selection

- Mouse click or single tap: Select single appointment
- Ctrl + Click: Select multiple appointments
- Delete key: Delete selected appointments

### Delete Multiple Appointments

Select multiple appointments and press Delete key to remove them all at once. For recurring events, only selected occurrences are deleted, not the entire series.

Use these comprehensive reference patterns to implement robust appointment and event handling in the Syncfusion React Scheduler component. Always validate event data, handle edge cases appropriately, and provide clear user feedback for CRUD operations.
