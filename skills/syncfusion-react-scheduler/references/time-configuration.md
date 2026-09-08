# Time Configuration

## Table of Contents

1. [Overview](#overview)
2. [Timescale Configuration](#timescale-configuration)
   - [Setting Time Slot Duration](#setting-time-slot-duration)
   - [Customizing Time Cells with Templates](#customizing-time-cells-with-templates)
   - [Hiding the Timescale](#hiding-the-timescale)
   - [Current Time Indicator](#current-time-indicator)
3. [Timezone Management](#timezone-management)
   - [Understanding Date Manipulation](#understanding-date-manipulation)
   - [Scheduler Without Specific Timezone](#scheduler-without-specific-timezone)
   - [Setting Specific Timezone](#setting-specific-timezone)
   - [Display Events with No Time Difference](#display-events-with-no-time-difference)
   - [Assign Specific Timezones to Events](#assign-specific-timezones-to-events)
   - [Customizing Timezone Collection](#customizing-timezone-collection)
   - [Timezone Methods](#timezone-methods)


---

## Overview

The Syncfusion React Scheduler provides comprehensive time configuration options to customize how time is displayed and managed in your scheduling application. This includes:

- **Timescale**: Control time slot intervals and visual appearance
- **Timezone**: Support for single and multiple timezone scenarios
- **Working Days/Hours**: Define business hours and working days
- **Calendar Modes**: Support for Gregorian and Islamic calendars
- **Time Formats**: Flexible time display formatting

These features work together to create a flexible scheduling experience that can adapt to various business requirements and international needs.

---

## Timescale Configuration

Time slots are the cells displayed in the Day, Week, and Work Week views of the Scheduler. The `timeScale` property controls the duration and appearance of these slots.

### Key Properties

- **`enable`**: When `true`, displays appointments accurately against exact time duration. Default: `true`
- **`interval`**: Time duration in minutes for each time axis segment. Default: `60` (1 hour)
- **`slotCount`**: Number of slots to split the interval into. Default: `2` (30-minute slots)

> **Note**: The maximum slots per day is 1000 for Timeline views (TimelineDay, TimelineWeek, TimelineWorkWeek).

### Setting Time Slot Duration

Configure custom time slot durations by combining `interval` and `slotCount`. The following example creates six 10-minute slots per hour:

```tsx
import * as React from 'react';
import {
  ScheduleComponent, Day, Week, WorkWeek, TimelineViews,
  ViewsDirective, ViewDirective, Inject, EventSettingsModel, TimeScaleModel
} from '@syncfusion/ej2-react-schedule';
import { scheduleData } from './datasource';

const App = () => {
  const eventSettings: EventSettingsModel = { dataSource: scheduleData };
  const timeScale: TimeScaleModel = { 
    enable: true, 
    interval: 60, 
    slotCount: 6 
  };

  return (
    <ScheduleComponent 
      width='100%' 
      height='550px'
      selectedDate={new Date(2018, 1, 15)} 
      eventSettings={eventSettings}
      timeScale={timeScale}
    >
      <ViewsDirective>
        <ViewDirective option='Day' />
        <ViewDirective option='Week' />
        <ViewDirective option='WorkWeek' />
        <ViewDirective option='TimelineDay' />
      </ViewsDirective>
      <Inject services={[Day, Week, WorkWeek, TimelineViews]} />
    </ScheduleComponent>
  );
};

export default App;
```

### Customizing Time Cells with Templates

Use templates to customize the appearance of time cells with `majorSlotTemplate` and `minorSlotTemplate`:

```tsx
import * as React from 'react';
import {
  ScheduleComponent, Day, Week, WorkWeek, TimelineViews,
  ViewsDirective, ViewDirective, Inject, EventSettingsModel, TimeScaleModel
} from '@syncfusion/ej2-react-schedule';
import { scheduleData } from './datasource';
import { Internationalization } from '@syncfusion/ej2-base';

const App = () => {
  const eventSettings: EventSettingsModel = { dataSource: scheduleData };
  const instance = new Internationalization();

  const minorSlotTemplate = (props): JSX.Element => {
    return (
      <div style={{ textAlign: 'right', marginRight: '15px' }}>
        {instance.formatDate(props.date, { skeleton: 'ms' }).replace(':00', '')}
      </div>
    );
  };

  const majorSlotTemplate = (props): JSX.Element => {
    return (
      <div>{instance.formatDate(props.date, { skeleton: 'hm' })}</div>
    );
  };

  const timeScale: TimeScaleModel = {
    enable: true, 
    interval: 60, 
    slotCount: 6,
    majorSlotTemplate: majorSlotTemplate,
    minorSlotTemplate: minorSlotTemplate
  };

  return (
    <ScheduleComponent 
      width='100%' 
      height='550px'
      selectedDate={new Date(2018, 1, 15)} 
      eventSettings={eventSettings}
      timeScale={timeScale}
    >
      <ViewsDirective>
        <ViewDirective option='Day' />
        <ViewDirective option='Week' />
        <ViewDirective option='TimelineWeek' />
        <ViewDirective option='WorkWeek' />
      </ViewsDirective>
      <Inject services={[Day, Week, WorkWeek, TimelineViews]} />
    </ScheduleComponent>
  );
};

export default App;
```

### Hiding the Timescale

Disable grid lines by setting `enable` to `false`. All appointments display one below another without time indicators:

```tsx
const timeScale: TimeScaleModel = { enable: false };

<ScheduleComponent 
  width='100%' 
  height='550px'
  selectedDate={new Date(2018, 1, 15)} 
  eventSettings={eventSettings}
  timeScale={timeScale}
>
  {/* Views */}
</ScheduleComponent>
```

### Current Time Indicator

The Scheduler highlights the current time by default. Control this with `showTimeIndicator`:

```tsx
<ScheduleComponent 
  width='100%' 
  height='550px'
  showTimeIndicator={true}  // Default: true
>
  <ViewsDirective>
    <ViewDirective option='Day' />
    <ViewDirective option='Week' />
    <ViewDirective option='TimelineWeek' />
  </ViewsDirective>
  <Inject services={[Day, Week, TimelineViews]} />
</ScheduleComponent>
```

---

## Timezone Management

The Scheduler supports comprehensive timezone management for global scheduling scenarios.

### Understanding Date Manipulation

JavaScript's `new Date()` constructor returns the current date with complete time and timezone information:

```javascript
// Example output:
// Wed Dec 12 2018 05:23:27 GMT+0530 (India Standard Time)
```

### Scheduler Without Specific Timezone

When the `timezone` property is not set, appointments display based on the client's system timezone. The same appointment may appear at different times for users in different timezones.

```tsx
import * as React from 'react';
import {
  ScheduleComponent, Day, Week, WorkWeek, Month, Agenda, Inject, EventSettingsModel
} from '@syncfusion/ej2-react-schedule';

const App = () => {
  const scheduleData: Object[] = [{
    Id: 3,
    Subject: 'Paris',
    StartTime: new Date(2018, 1, 15, 9, 0),
    EndTime: new Date(2018, 1, 15, 10, 0)
  }];
  
  const eventSettings: EventSettingsModel = { dataSource: scheduleData };

  return (
    <ScheduleComponent 
      width='100%' 
      height='550px'
      selectedDate={new Date(2018, 1, 11)} 
      eventSettings={eventSettings}
    >
      <Inject services={[Day, Week, WorkWeek, Month, Agenda]} />
    </ScheduleComponent>
  );
};

export default App;
```

### Setting Specific Timezone

Set a specific timezone using the `timezone` property. Appointments display according to the Scheduler's timezone regardless of the user's system timezone:

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
      width='100%' 
      height='550px'
      selectedDate={new Date(2018, 1, 17)} 
      eventSettings={eventSettings}
      timezone='America/New_York'  // Eastern Time UTC-05:00
    >
      <Inject services={[Day, Week, WorkWeek, Month, Agenda]} />
    </ScheduleComponent>
  );
};

export default App;
```

### Display Events with No Time Difference

Setting timezone to `UTC` displays appointments at the same time for all users globally:

```tsx
import * as React from 'react';
import {
  ScheduleComponent, Day, Week, Month, Timezone, Inject,
  ViewsDirective, ViewDirective, EventSettingsModel
} from '@syncfusion/ej2-react-schedule';
import { fifaEventsData } from './datasource';

const App = () => {
  const eventSettings: EventSettingsModel = { dataSource: fifaEventsData };
  let timezone: Timezone = new Timezone();

  const onCreate = (): void => {
    for (let fifaEvent of fifaEventsData) {
      let event: { [key: string]: Object } = fifaEvent as { [key: string]: Object };
      event.StartTime = timezone.removeLocalOffset(event.StartTime as Date);
      event.EndTime = timezone.removeLocalOffset(event.EndTime as Date);
    }
  };

  return (
    <ScheduleComponent 
      width='100%' 
      height='550px'
      selectedDate={new Date(2018, 5, 17)} 
      created={onCreate}
      eventSettings={eventSettings}
      timezone='UTC'
    >
      <ViewsDirective>
        <ViewDirective option='Day' />
        <ViewDirective option='Week' />
        <ViewDirective option='Month' />
      </ViewsDirective>
      <Inject services={[Day, Week, Month]} />
    </ScheduleComponent>
  );
};

export default App;
```

### Assign Specific Timezones to Events

Individual appointments can have their own timezones using `startTimezone` and `endTimezone` properties:

```tsx
import * as React from 'react';
import {
  ScheduleComponent, Day, Week, WorkWeek, Month, Agenda, Inject, EventSettingsModel
} from '@syncfusion/ej2-react-schedule';

const App = () => {
  const scheduleData: Object[] = [{
    Id: 3,
    Subject: 'Paris',
    StartTime: new Date(2018, 1, 15, 10, 0),
    EndTime: new Date(2018, 1, 15, 12, 30),
    StartTimezone: 'Europe/Moscow',
    EndTimezone: 'Europe/Moscow'
  }];

  const eventSettings: EventSettingsModel = { dataSource: scheduleData };

  return (
    <ScheduleComponent 
      width='100%' 
      height='550px'
      selectedDate={new Date(2018, 1, 11)} 
      eventSettings={eventSettings}
    >
      <Inject services={[Day, Week, WorkWeek, Month, Agenda]} />
    </ScheduleComponent>
  );
};

export default App;
```

### Customizing Timezone Collection

Customize the timezone collection shown in the editor window:

```tsx
import * as React from 'react';
import { useEffect } from 'react';
import {
  ScheduleComponent, Day, Week, Month, timezoneData, Inject, EventSettingsModel
} from '@syncfusion/ej2-react-schedule';
import { scheduleData } from './datasource';

const App = () => {
  const eventSettings: EventSettingsModel = { dataSource: scheduleData };

  const timeZones: { [key: string]: Object }[] = [
    { Value: 'America/New_York', Text: '(UTC-05:00) Eastern Time' },
    { Value: 'UTC', Text: 'UTC' },
    { Value: 'Asia/Kolkata', Text: '(UTC+05:30) India Standard Time' }
  ];

  useEffect(() => {
    timezoneData.splice(0, timezoneData.length, ...timeZones as any);
  }, []);

  return (
    <ScheduleComponent 
      width='100%' 
      height='550px'
      selectedDate={new Date(2018, 1, 1)} 
      eventSettings={eventSettings}
    >
      <Inject services={[Day, Week, Month]} />
    </ScheduleComponent>
  );
};

export default App;
```

### Timezone Methods

The Scheduler provides utility methods for timezone operations:

#### offset

Calculates the difference in minutes between a UTC date and a target timezone:

```typescript
let timezone: Timezone = new Timezone();
let date: Date = new Date(2018, 11, 5, 15, 25, 11);
let timeZoneOffset: number = timezone.offset(date, "Europe/Paris");
console.log(timeZoneOffset); // -60
```

**Parameters:**
- `Date` (Date): UTC time as date object
- `Timezone` (String): Target timezone

**Returns:** `number` (offset in minutes)

#### convert

Converts a date from one timezone to another:

```typescript
let timezone: Timezone = new Timezone();
let date: Date = new Date(2018, 11, 5, 15, 25, 11);
let convertedDate: Date = timezone.convert(date, "Europe/Paris", "Asia/Tokyo");
let convertedDate1: Date = timezone.convert(date, 60, -360);
console.log(convertedDate);  // 2018-12-05T08:55:11.000Z
console.log(convertedDate1); // 2018-12-05T16:55:11.000Z
```

**Parameters:**
- `Date` (Date): UTC time as date object
- `fromOffset` (number/string): Source timezone
- `toOffset` (number/string): Target timezone

**Returns:** `Date`

#### add

Adds the time difference between UTC and a timezone to the date:

```typescript
let timezone: Timezone = new Timezone();
let date: Date = new Date(2018, 11, 5, 15, 25, 11);
let convertedDate: Date = timezone.add(date, "Europe/Paris");
console.log(convertedDate); // 2018-12-05T05:25:11.000Z
```

**Parameters:**
- `Date` (Date): UTC time as date object
- `Timezone` (String): Target timezone

**Returns:** `Date`

#### remove

Removes the time difference between UTC and a timezone:

```typescript
let timezone: Timezone = new Timezone();
let date: Date = new Date(2018, 11, 5, 15, 25, 11);
let convertedDate: Date = timezone.remove(date, "Europe/Paris");
console.log(convertedDate); // 2018-12-05T14:25:11.000Z
```

**Parameters:**
- `Date` (Date): UTC time as date object
- `Timezone` (String): Target timezone

**Returns:** `Date`

#### removeLocalOffset

Removes the local timezone offset from a date:

```typescript
let timezone: Timezone = new Timezone();
let date: Date = new Date(2018, 11, 5, 15, 25, 11);
let convertedDate: Date = timezone.removeLocalOffset(date);
console.log(convertedDate); // 2018-12-05T15:25:11.000Z
```

**Parameters:**
- `Date` (Date): UTC time as date object

**Returns:** `Date`

---
