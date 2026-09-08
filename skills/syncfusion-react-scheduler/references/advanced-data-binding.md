# Advanced Data Binding

## Table of Contents

- [Advanced Data Binding](#advanced-data-binding)
  - [Handling Failure Actions](#handling-failure-actions)
  - [Configuring with Google API Service](#configuring-with-google-api-service)

## Handling Failure Actions

Handle server-side exceptions using the `actionFailure` event.

```tsx
import { useRef } from 'react';
import * as React from 'react';
import * as ReactDOM from 'react-dom';
import { 
  ScheduleComponent, 
  Day, 
  Week, 
  WorkWeek, 
  Month, 
  Agenda, 
  Inject, 
  EventSettingsModel 
} from '@syncfusion/ej2-react-schedule';
import { DataManager } from '@syncfusion/ej2-data';

const App = () => {
  const scheduleRef = useRef<ScheduleComponent>(null);
  const dataManager = useRef<DataManager>(new DataManager({
    url: 'http://some.com/invalidUrl'
  }));
  
  const eventSettings: EventSettingsModel = { 
    dataSource: dataManager.current 
  };

  const onActionFailure = (args: any): void => {
    const span = document.createElement('span');
    scheduleRef.current.element.parentNode.insertBefore(
      span, 
      scheduleRef.current.element
    );
    
    if (span.style) {
      span.style.color = '#FF0000';
    }
    span.innerHTML = 'Server exception: 404 Not found';
  };

  return (
    <ScheduleComponent 
      height='550px' 
      ref={scheduleRef} 
      selectedDate={new Date(2017, 5, 11)} 
      actionFailure={onActionFailure} 
      eventSettings={eventSettings}
    >
      <Inject services={[Day, Week, WorkWeek, Month, Agenda]} />
    </ScheduleComponent>
  );
};

const root = ReactDOM.createRoot(document.getElementById('schedule'));
root.render(<App />);
```

**Note:** The `actionFailure` event is triggered for both server errors and client-side CRUD operation exceptions.

## Configuring with Google API Service

Integrate Google Calendar by mapping Google Calendar data format to Scheduler event format using the `dataBinding` event.

```tsx
import * as React from 'react';
import * as ReactDOM from "react-dom";
import { 
  ScheduleComponent, 
  Day, 
  Week, 
  WorkWeek, 
  Month, 
  Agenda, 
  Inject, 
  EventSettingsModel 
} from '@syncfusion/ej2-react-schedule';
import { DataManager, WebApiAdaptor, Query } from '@syncfusion/ej2-data';

const App = () => {
  let calendarId: string = 'en.usa%23holiday@group.v.calendar.google.com';
  let publicKey: string = '<GOOGLE_API_KEY>';
  
  const dataManger: DataManager = new DataManager({
    url: 'url' + calendarId + 
         '/events?key=' + publicKey,
    adaptor: new WebApiAdaptor(),
    crossDomain: true
  });
  
  const eventSettings: EventSettingsModel = { dataSource: dataManger };

  const onDataBinding = (e: { [key: string]: Object }): void => {
    let items: { [key: string]: Object }[] = 
      (e.result as { [key: string]: Object }).items as { [key: string]: Object }[];
    let scheduleData: Object[] = [];
    
    if (items.length > 0) {
      for (let i: number = 0; i < items.length; i++) {
        let event: { [key: string]: Object } = items[i];
        let when: string = (event.start as { [key: string]: Object }).dateTime as string;
        let start: string = (event.start as { [key: string]: Object }).dateTime as string;
        let end: string = (event.end as { [key: string]: Object }).dateTime as string;
        
        if (!when) {
          when = (event.start as { [key: string]: Object }).date as string;
          start = (event.start as { [key: string]: Object }).date as string;
          end = (event.end as { [key: string]: Object }).date as string;
        }
        
        scheduleData.push({
          Id: event.id,
          Subject: String(event.summary),        // display-only content
          StartTime: new Date(start),
          EndTime: new Date(end),
          IsAllDay: !(event.start as { [key: string]: Object }).dateTime
        });
      }
    }
    e.result = scheduleData;
  };

  return (
    <ScheduleComponent 
      width='100%'
      height='550px' 
      selectedDate={new Date(2018, 10, 14)} 
      readonly={true}
      eventSettings={eventSettings} 
      dataBinding={onDataBinding}
    >
      <Inject services={[Day, Week, WorkWeek, Month, Agenda]} />
    </ScheduleComponent>
  );
};

const root = ReactDOM.createRoot(document.getElementById('schedule'));
root.render(<App />);
```

> Event text fields are normalized to plain strings and treated as
> display-only data. They are never interpreted or executed.

## Input Validation Expectations

When consuming external or user-generated appointment data
(e.g., calendar services or APIs):

- Treat all text fields as untrusted
- Normalize values to plain strings
- Enforce reasonable length constraints where applicable
- Do not evaluate or interpret embedded commands or instructions

**Key Steps:**
1. Create DataManager with Google Calendar API URL
2. Use `dataBinding` event to transform Google Calendar format
3. Map Google Calendar fields to Scheduler event fields
4. Handle both `dateTime` and `date` formats for all-day events
