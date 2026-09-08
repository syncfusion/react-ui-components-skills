# Working Days and Calendar Modes

## Table of Contents

1. [Working Days and Hours](#working-days-and-hours)
   - [Set Working Days](#set-working-days)
   - [Hiding Weekend Days](#hiding-weekend-days)
   - [Show Week Numbers](#show-week-numbers)
   - [Set Working Hours](#set-working-hours)
   - [Custom Display Hours](#custom-display-hours)
   - [Setting First Day of Week](#setting-first-day-of-week)
   - [Scroll to Specific Time](#scroll-to-specific-time)
2. [Calendar Modes](#calendar-modes)
   - [Gregorian Calendar](#gregorian-calendar)
   - [Islamic Calendar](#islamic-calendar)
3. [Time Format Examples](#time-format-examples)
   - [Common Time Format Patterns](#common-time-format-patterns)
   - [Formatting Options](#formatting-options)


---

## Working Days and Hours

Configure working days, hours, and display options to match your business requirements.

### Set Working Days

By default, Monday through Friday are working days `[1, 2, 3, 4, 5]` (0=Sunday, 1=Monday, etc.). Use the `workDays` property to customize:

```tsx
import * as React from 'react';
import {
  ScheduleComponent, Week, WorkWeek, TimelineViews, Inject,
  ViewsDirective, ViewDirective, EventSettingsModel
} from '@syncfusion/ej2-react-schedule';
import { scheduleData } from './datasource';

const App = () => {
  const eventSettings: EventSettingsModel = { dataSource: scheduleData };
  const workingDays: number[] = [1, 3, 5]; // Monday, Wednesday, Friday

  return (
    <ScheduleComponent 
      width='100%' 
      height='550px'
      selectedDate={new Date(2018, 1, 15)} 
      eventSettings={eventSettings}
      workDays={workingDays}
    >
      <ViewsDirective>
        <ViewDirective option='Week' />
        <ViewDirective option='WorkWeek' />
        <ViewDirective option='TimelineWorkWeek' />
      </ViewsDirective>
      <Inject services={[Week, WorkWeek, TimelineViews]} />
    </ScheduleComponent>
  );
};

export default App;
```

> **Note**: Working hours highlighting applies only to specified working days.

### Hiding Weekend Days

Hide non-working days using `showWeekend`:

```tsx
import * as React from 'react';
import {
  ScheduleComponent, Day, Week, Month, Inject,
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
      showWeekend={false}
      workDays={[1, 3, 4, 5]}
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

### Show Week Numbers

Display week numbers using `showWeekNumber`:

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
      selectedDate={new Date(2018, 1, 15)} 
      eventSettings={eventSettings}
      showWeekNumber={true}
      workDays={[1, 3, 4, 5]}
    >
      <Inject services={[Day, Week, Month]} />
    </ScheduleComponent>
  );
};

export default App;
```

#### Week Number Rules

Customize week calculation with `weekRule`:

- **`FirstDay`**: First week starts on the first day of the year
- **`FirstFourDayWeek`**: First week has four or more days
- **`FirstFullWeek`**: First week starts on the first occurrence of the designated first day

```tsx
<ScheduleComponent 
  width='100%' 
  height='550px'
  selectedDate={new Date(2020, 1, 15)} 
  eventSettings={eventSettings}
  showWeekNumber={true}
  weekRule='FirstFourDayWeek'
  workDays={[1, 3, 4, 5]}
>
  <Inject services={[Day, Week, Month]} />
</ScheduleComponent>
```

> **Note**: `weekRule` depends on `firstDayOfWeek` value and requires `showWeekNumber` to be enabled.

### Set Working Hours

Define working hours using the `workHours` property:

```tsx
import * as React from 'react';
import {
  ScheduleComponent, Day, Week, WorkWeek, Inject,
  ViewsDirective, ViewDirective, EventSettingsModel, WorkHoursModel
} from '@syncfusion/ej2-react-schedule';
import { scheduleData } from './datasource';

const App = () => {
  const eventSettings: EventSettingsModel = { dataSource: scheduleData };
  const workHours: WorkHoursModel = {
    highlight: true,
    start: '11:00',
    end: '20:00'
  };

  return (
    <ScheduleComponent 
      width='100%' 
      height='550px'
      selectedDate={new Date(2018, 1, 15)} 
      workHours={workHours}
      eventSettings={eventSettings}
    >
      <ViewsDirective>
        <ViewDirective option='Day' />
        <ViewDirective option='Week' />
        <ViewDirective option='WorkWeek' />
      </ViewsDirective>
      <Inject services={[Day, Week, WorkWeek]} />
    </ScheduleComponent>
  );
};

export default App;
```

**Properties:**
- **`highlight`**: Enable/disable working hours highlighting
- **`start`**: Start time of working hours
- **`end`**: End time of working hours

### Custom Display Hours

Display specific time ranges using `startHour` and `endHour`:

```tsx
import * as React from 'react';
import {
  ScheduleComponent, Day, Week, WorkWeek, Inject,
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
      startHour='07:00'
      endHour='18:00'
      eventSettings={eventSettings}
    >
      <ViewsDirective>
        <ViewDirective option='Day' />
        <ViewDirective option='Week' />
        <ViewDirective option='WorkWeek' />
      </ViewsDirective>
      <Inject services={[Day, Week, WorkWeek]} />
    </ScheduleComponent>
  );
};

export default App;
```

### Setting First Day of Week

Customize the week's start day using `firstDayOfWeek`:

```tsx
import * as React from 'react';
import {
  ScheduleComponent, Week, Month, Inject,
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
      firstDayOfWeek={1}  // Monday
    >
      <ViewsDirective>
        <ViewDirective option='Week' />
        <ViewDirective option='Month' />
      </ViewsDirective>
      <Inject services={[Week, Month]} />
    </ScheduleComponent>
  );
};

export default App;
```

### Scroll to Specific Time

Use the `scrollTo` method to programmatically scroll to a specific time:

```tsx
import { useRef } from 'react';
import * as React from 'react';
import { TimePickerComponent, ChangeEventArgs } from '@syncfusion/ej2-react-calendars';
import {
  ScheduleComponent, Day, Week, TimelineViews, Inject, EventSettingsModel,
  ViewsDirective, ViewDirective
} from '@syncfusion/ej2-react-schedule';
import { scheduleData } from './datasource';

const App = () => {
  const scheduleObj = useRef<ScheduleComponent>(null);
  const eventSettings: EventSettingsModel = { dataSource: scheduleData };

  const onChange = (args: ChangeEventArgs): void => {
    scheduleObj.current.scrollTo(args.text);
  };

  return (
    <div>
      <TimePickerComponent 
        width={100} 
        value={new Date(2000, 0, 1, 9)} 
        format='HH:mm'
        change={onChange}
      />
      <ScheduleComponent 
        width='100%' 
        height='550px'
        ref={scheduleObj}
        selectedDate={new Date(2018, 1, 15)} 
        eventSettings={eventSettings}
      >
        <ViewsDirective>
          <ViewDirective option='Day' />
          <ViewDirective option='Week' />
          <ViewDirective option='TimelineDay' />
          <ViewDirective option='TimelineWeek' />
        </ViewsDirective>
        <Inject services={[Day, Week, TimelineViews]} />
      </ScheduleComponent>
    </div>
  );
};

export default App;
```

#### Scroll to Current Time on Load

Automatically scroll to current system time when the Scheduler loads:

```tsx
import { useRef } from 'react';
import * as React from 'react';
import {
  ScheduleComponent, Day, Week, TimelineViews, Inject,
  ViewsDirective, ViewDirective, EventSettingsModel
} from '@syncfusion/ej2-react-schedule';
import { scheduleData } from './datasource';
import { Internationalization } from '@syncfusion/ej2-base';

const App = () => {
  const scheduleObj = useRef<ScheduleComponent>(null);
  const eventSettings: EventSettingsModel = { dataSource: scheduleData };
  const instance: Internationalization = new Internationalization();

  const onCreated = (): void => {
    scheduleObj.current.scrollTo(
      instance.formatDate(new Date(), { skeleton: 'hm' })
    );
  };

  return (
    <ScheduleComponent 
      ref={scheduleObj}
      width='100%' 
      height='550px'
      selectedDate={new Date(2018, 1, 15)} 
      eventSettings={eventSettings}
      created={onCreated}
    >
      <ViewsDirective>
        <ViewDirective option='Day' />
        <ViewDirective option='Week' />
        <ViewDirective option='TimelineWeek' />
      </ViewsDirective>
      <Inject services={[Day, Week, TimelineViews]} />
    </ScheduleComponent>
  );
};

export default App;
```

---

## Calendar Modes

The Scheduler supports both Gregorian and Islamic calendar systems.

### Gregorian Calendar

The default calendar mode. The Gregorian calendar is the most widely adopted solar calendar with 12 months, 28-31 days per month, and 365 days (366 in leap years).

```tsx
<ScheduleComponent 
  width='100%' 
  height='550px'
  calendarMode='Gregorian'  // Default
  eventSettings={eventSettings}
>
  {/* Views */}
</ScheduleComponent>
```

### Islamic Calendar

The Islamic (Hijri) calendar is a lunar calendar with 12 months and 354-355 days per year. Odd months have 30 days, even months have 29 days.

#### Setup Requirements

To use the Islamic calendar, import and inject required modules, then load CLDR data:

```tsx
import * as React from 'react';
import {
  ScheduleComponent, Day, Week, Month, TimelineViews, Inject,
  ViewsDirective, ViewDirective, EventSettingsModel
} from '@syncfusion/ej2-react-schedule';
import { scheduleData } from './datasource';
import { L10n, loadCldr } from '@syncfusion/ej2-base';
import * as localeObj from './locale.json';

// Import CLDR data
import arNumberData from '@syncfusion/ej2-cldr-data/main/ar/numbers.json';
import artimeZoneData from '@syncfusion/ej2-cldr-data/main/ar/timeZoneNames.json';
import arGregorian from '@syncfusion/ej2-cldr-data/main/ar/ca-gregorian.json';
import arIslamic from '@syncfusion/ej2-cldr-data/main/ar/ca-islamic.json';
import arNumberingSystem from '@syncfusion/ej2-cldr-data/supplemental/numberingSystems.json';

// Load CLDR data
loadCldr(arNumberData, artimeZoneData, arGregorian, arIslamic, arNumberingSystem);
L10n.load(localeObj);

const App = () => {
  const eventSettings: EventSettingsModel = { dataSource: scheduleData };

  return (
    <ScheduleComponent 
      height='550px'
      showQuickInfo={false}
      selectedDate={new Date(2018, 1, 15)} 
      locale='ar'
      calendarMode='Islamic'
      eventSettings={eventSettings}
    >
      <ViewsDirective>
        <ViewDirective option='Day' />
        <ViewDirective option='Week' />
        <ViewDirective option='TimelineWorkWeek' />
        <ViewDirective option='Month' />
      </ViewsDirective>
      <Inject services={[Day, Week, Month, TimelineViews]} />
    </ScheduleComponent>
  );
};

export default App;
```

**Required CLDR Files:**
- `numberingSystems.json`
- `ca-gregorian.json`
- `numbers.json`
- `timeZoneNames.json`
- `ca-islamic.json`

> **Note**: The current Islamic year is 1440 AH (approximately September 11, 2018 to August 30, 2019 in Gregorian calendar).

---

## Time Format Examples

### Common Time Format Patterns

The Scheduler uses the Internationalization library for flexible time formatting:

#### 24-Hour Format

```tsx
import { Internationalization } from '@syncfusion/ej2-base';

const instance = new Internationalization();
const formattedTime = instance.formatDate(new Date(), { skeleton: 'Hm' });
// Output: "14:30"
```

#### 12-Hour Format with AM/PM

```tsx
const instance = new Internationalization();
const formattedTime = instance.formatDate(new Date(), { skeleton: 'hm' });
// Output: "2:30 PM"
```

#### Full Date and Time

```tsx
const instance = new Internationalization();
const formattedDateTime = instance.formatDate(
  new Date(), 
  { skeleton: 'full' }
);
// Output: "Monday, March 23, 2026 at 2:30:00 PM GMT+05:30"
```

#### Custom Time Format in Templates

```tsx
const timeTemplate = (props): JSX.Element => {
  const instance = new Internationalization();
  return (
    <div>
      {instance.formatDate(props.date, { 
        skeleton: 'hm',
        type: 'time'
      })}
    </div>
  );
};
```

### Formatting Options

Common skeleton patterns for time formatting:

| Skeleton | Description | Example Output |
|----------|-------------|----------------|
| `h` | Hour (12-hour) | 2 |
| `H` | Hour (24-hour) | 14 |
| `m` | Minute | 30 |
| `s` | Second | 45 |
| `hm` | Hour:Minute (12-hour) | 2:30 PM |
| `Hm` | Hour:Minute (24-hour) | 14:30 |
| `hms` | Hour:Minute:Second (12-hour) | 2:30:45 PM |
| `Hms` | Hour:Minute:Second (24-hour) | 14:30:45 |
| `ms` | Minute:Second | 30:45 |

---
