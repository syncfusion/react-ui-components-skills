# Dimensions and Responsive Layout

## Table of Contents
- [Dimensions](#dimensions)
  - [Auto Height and Width](#auto-height-and-width)
  - [Height and Width in Pixels](#height-and-width-in-pixels)
  - [Height and Width in Percentages](#height-and-width-in-percentages)
- [Row Auto Height](#row-auto-height)
  - [Calendar Month View](#calendar-month-view-auto-height)
  - [Timeline Views](#timeline-views-auto-height)
  - [Timeline with Multiple Resources](#timeline-with-multiple-resources)
  - [Appointments Occupying Entire Cell](#appointments-occupying-entire-cell)
- [Responsive Layout](#responsive-layout)
- [Mobile Rendering](#mobile-rendering)

## Dimensions

The Scheduler supports three types of dimension values for controlling height and width: `auto`, `pixel`, and `percentage`.

### Auto Height and Width

When height and width are set to `auto`, the Scheduler attempts to match the dimensions of its parent container. By default, both properties are set to `auto`.

```tsx
import * as React from 'react';
import {
  ScheduleComponent, Day, Week, WorkWeek, Month, Agenda, Inject, EventSettingsModel
} from '@syncfusion/ej2-react-schedule';
import { scheduleData } from './datasource';

const App = () => {
  const eventSettings: EventSettingsModel = { dataSource: scheduleData };
  
  return (
    <ScheduleComponent 
      height='auto' 
      width='auto' 
      selectedDate={new Date(2018, 1, 15)} 
      eventSettings={eventSettings}
    >
      <Inject services={[Day, Week, WorkWeek, Month, Agenda]} />
    </ScheduleComponent>
  );
};

export default App;
```

### Height and Width in Pixels

Set exact pixel values for height and width. Values can be numbers (e.g., `500`) or strings with `px` suffix (e.g., `'500px'`).

```tsx
import * as React from 'react';
import {
  ScheduleComponent, Day, Week, WorkWeek, Month, Agenda, Inject, EventSettingsModel
} from '@syncfusion/ej2-react-schedule';
import { scheduleData } from './datasource';

const App = () => {
  const eventSettings: EventSettingsModel = { dataSource: scheduleData };
  
  return (
    <ScheduleComponent 
      height='550px' 
      width='650px' 
      selectedDate={new Date(2018, 1, 15)} 
      eventSettings={eventSettings}
    >
      <Inject services={[Day, Week, WorkWeek, Month, Agenda]} />
    </ScheduleComponent>
  );
};

export default App;
```

### Height and Width in Percentages

Use percentage values to make the Scheduler scale relative to its parent container.

```tsx
import * as React from 'react';
import {
  ScheduleComponent, Day, Week, WorkWeek, Month, Agenda, Inject, EventSettingsModel
} from '@syncfusion/ej2-react-schedule';
import { scheduleData } from './datasource';

const App = () => {
  const eventSettings: EventSettingsModel = { dataSource: scheduleData };

  return (
    <ScheduleComponent 
      height='100%' 
      width='100%' 
      selectedDate={new Date(2018, 1, 15)} 
      eventSettings={eventSettings}
    >
      <Inject services={[Day, Week, WorkWeek, Month, Agenda]} />
    </ScheduleComponent>
  );
};

export default App;
```

## Row Auto Height

The row auto-height feature automatically adjusts row heights in Timeline and Month views based on the number of overlapping appointments, eliminating the `+n more` text indicator.

Enable this feature by setting the `rowAutoHeight` property to `true` (default is `false`).

**Note**: This feature applies only to Timeline views and the calendar Month view.

### Calendar Month View Auto Height

In Month view, the row auto-height feature automatically expands rows to display all appointments.

```tsx
import * as React from 'react';
import {
  ScheduleComponent, Month, Inject,
  ViewsDirective, ViewDirective, EventSettingsModel
} from '@syncfusion/ej2-react-schedule';
import { scheduleData } from './datasource';

const App = () => {
  const eventSettings: EventSettingsModel = { dataSource: scheduleData };

  return (
    <ScheduleComponent 
      width='100%' 
      height='550px' 
      selectedDate={new Date(2018, 1, 15)} 
      eventSettings={eventSettings} 
      rowAutoHeight={true}
    >
      <ViewsDirective>
        <ViewDirective option='Month' />
      </ViewsDirective>
      <Inject services={[Month]} />
    </ScheduleComponent>
  );
};

export default App;
```

### Timeline Views Auto Height

Timeline views automatically adjust row heights based on overlapping appointments.

```tsx
import * as React from 'react';
import {
  ScheduleComponent, TimelineViews, Inject, TimelineMonth, Agenda,
  ViewsDirective, ViewDirective, EventSettingsModel
} from '@syncfusion/ej2-react-schedule';
import { scheduleData } from './datasource';

const App = () => {
  const eventSettings: EventSettingsModel = { dataSource: scheduleData };

  return (
    <ScheduleComponent 
      width='100%' 
      height='550px' 
      selectedDate={new Date(2018, 1, 15)} 
      eventSettings={eventSettings} 
      rowAutoHeight={true}
    >
      <ViewsDirective>
        <ViewDirective option='TimelineDay' />
        <ViewDirective option='TimelineWeek' />
        <ViewDirective option='TimelineWorkWeek' />
        <ViewDirective option='TimelineMonth' />
        <ViewDirective option='Agenda' />
      </ViewsDirective>
      <Inject services={[TimelineViews, TimelineMonth, Agenda]} />
    </ScheduleComponent>
  );
};

export default App;
```

### Timeline with Multiple Resources

Row auto-height works with Timeline views containing multiple resources.

```tsx
import * as React from 'react';
import {
  TimelineViews, ScheduleComponent, ViewsDirective, ViewDirective,
  ResourcesDirective, ResourceDirective, Inject, Resize, 
  DragAndDrop, EventSettingsModel, GroupModel
} from '@syncfusion/ej2-react-schedule';
import { roomData } from './datasource';

const App = () => {
  const ownerData: Object[] = [
    { text: 'Room A', id: 1, color: '#98AFC7' },
    { text: 'Room B', id: 2, color: '#99c68e' },
    { text: 'Room C', id: 3, color: '#C2B280' },
    { text: 'Room D', id: 4, color: '#3090C7' },
    { text: 'Room E', id: 5, color: '#95b9' },
    { text: 'Room F', id: 6, color: '#95b9c7' },
    { text: 'Room G', id: 7, color: '#deb887' },
    { text: 'Room H', id: 8, color: '#3090C7' },
    { text: 'Room I', id: 9, color: '#98AFC7' },
    { text: 'Room J', id: 10, color: '#778899' }
  ];
  
  const fieldsData = {
    id: 'Id',
    subject: { title: 'Summary', name: 'Subject' },
    location: { title: 'Location', name: 'Location' },
    description: { title: 'Comments', name: 'Description' },
    startTime: { title: 'From', name: 'StartTime' },
    endTime: { title: 'To', name: 'EndTime' }
  };
  
  const eventSettings: EventSettingsModel = { 
    dataSource: roomData, 
    fields: fieldsData 
  };
  
  const group: GroupModel = { 
    enableCompactView: false, 
    resources: ['MeetingRoom'] 
  };

  return (
    <ScheduleComponent 
      width='100%' 
      height='550px' 
      selectedDate={new Date(2018, 7, 1)} 
      rowAutoHeight={true} 
      eventSettings={eventSettings} 
      group={group}
    >
      <ResourcesDirective>
        <ResourceDirective 
          field='RoomId' 
          title='Room Type' 
          name='MeetingRoom' 
          allowMultiple={true}
          dataSource={ownerData} 
          textField='text' 
          idField='id' 
          colorField='color'
        />
      </ResourcesDirective>
      <ViewsDirective>
        <ViewDirective option='TimelineDay' />
        <ViewDirective option='TimelineWeek' />
      </ViewsDirective>
      <Inject services={[TimelineViews, Resize, DragAndDrop]} />
    </ScheduleComponent>
  );
};

export default App;
```

### Appointments Occupying Entire Cell

When `rowAutoHeight` is enabled, appointments have whitespace at the bottom by default. Remove this space by setting `ignoreWhitespace` to `true` in `eventSettings`.

```tsx
import * as React from 'react';
import {
  TimelineViews, TimelineMonth, ScheduleComponent, ViewsDirective, 
  ViewDirective, EventSettingsModel, GroupModel, ResourcesDirective, 
  ResourceDirective, Inject
} from '@syncfusion/ej2-react-schedule';
import { resourceData } from './datasource';

const App = () => {
  const eventSettings: EventSettingsModel = { 
    dataSource: resourceData, 
    ignoreWhitespace: true 
  };
  
  const group: GroupModel = { resources: ['Rooms', 'Owners'] };
  
  const roomData: Object[] = [
    { RoomText: 'ROOM 1', Id: 1, RoomColor: '#cb6bb2' },
    { RoomText: 'ROOM 2', Id: 2, RoomColor: '#56ca85' }
  ];
  
  const ownerData: Object[] = [
    { OwnerText: 'Nancy', Id: 1, GroupId: 1, OwnerColor: '#ffaa00' },
    { OwnerText: 'Steven', Id: 2, GroupId: 2, OwnerColor: '#f8a398' },
    { OwnerText: 'Michael', Id: 3, GroupId: 1, OwnerColor: '#7499e1' }
  ];

  return (
    <ScheduleComponent 
      width='100%' 
      height='550px' 
      currentView='TimelineWeek' 
      rowAutoHeight={true} 
      selectedDate={new Date(2021, 7, 4)} 
      eventSettings={eventSettings} 
      group={group}
    >
      <ViewsDirective>
        <ViewDirective option='TimelineDay' />
        <ViewDirective option='TimelineWeek' />
        <ViewDirective option='TimelineMonth' />
      </ViewsDirective>
      <ResourcesDirective>
        <ResourceDirective 
          field='RoomId' 
          title='Room' 
          name='Rooms'
          dataSource={roomData} 
          textField='RoomText' 
          idField='Id' 
          colorField='RoomColor'
        />
        <ResourceDirective 
          field='OwnerId' 
          title='Owner' 
          name='Owners' 
          allowMultiple={true}
          dataSource={ownerData} 
          textField='OwnerText' 
          idField='Id' 
          groupIDField='GroupId' 
          colorField='OwnerColor'
        />
      </ResourcesDirective>
      <Inject services={[TimelineViews, TimelineMonth]} />
    </ScheduleComponent>
  );
};

export default App;
```

**Note**: The `ignoreWhitespace` property only applies when `rowAutoHeight` is enabled.

## Responsive Layout

The Syncfusion React Scheduler is designed to be fully responsive and adapt to various screen sizes automatically. The component adjusts its layout based on the available viewport dimensions, ensuring optimal user experience across devices.

Key responsive features:
- Automatic resizing when parent container dimensions change
- Percentage-based dimensions for fluid layouts
- Built-in adaptive UI mode for mobile devices
- Touch-friendly interface elements on smaller screens

For responsive applications, consider:
- Using percentage values for width and height
- Enabling `enableAdaptiveUI` for mobile-optimized interface
- Testing on various viewport sizes to ensure proper rendering

## Mobile Rendering

The Scheduler provides an adaptive UI mode specifically optimized for mobile devices. Enable this mode using the `enableAdaptiveUI` property set to `true`.

Mobile-specific optimizations include:
- **Compact Header**: View options move to a popup menu
- **Touch-Friendly Controls**: Larger touch targets for better interaction
- **Optimized Event Display**: Improved event rendering for small screens
- **Simplified Navigation**: Streamlined date navigation controls
- **Responsive Dialogs**: Event editor and quick info popup adapt to screen size

Example with adaptive UI enabled:

```tsx
import * as React from 'react';
import {
  ScheduleComponent, Day, Week, Month, Agenda, Inject, EventSettingsModel
} from '@syncfusion/ej2-react-schedule';
import { scheduleData } from './datasource';

const App = () => {
  const eventSettings: EventSettingsModel = { dataSource: scheduleData };

  return (
    <ScheduleComponent 
      width='100%' 
      height='100%' 
      selectedDate={new Date(2018, 1, 15)} 
      enableAdaptiveUI={true} 
      eventSettings={eventSettings}
    >
      <Inject services={[Day, Week, Month, Agenda]} />
    </ScheduleComponent>
  );
};

export default App;
```

When designing for mobile:
- Enable adaptive UI mode for better mobile experience
- Use touch-friendly event handlers
- Consider viewport limitations when customizing templates
- Test on actual mobile devices or emulators
- Ensure custom toolbar items are touch-accessible

---