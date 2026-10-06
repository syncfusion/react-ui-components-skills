# Calendar Settings

## Table of Contents
- [Overview](#overview)
- [Project Calendar](#project-calendar)
- [Task Calendars](#task-calendars)
- [Hours Per Day](#hours-per-day)
- [Calendar Behavior and Scheduling Impact](#calendar-behavior-and-scheduling-impact)
- [Supported Scenarios](#supported-scenarios)
- [Feature Limitations](#feature-limitations)
- [Best Practices](#best-practices)
- [See Also](#see-also)

---

## Overview

The Syncfusion React Gantt Chart supports calendar-driven scheduling through the `calendarSettings` property. Calendar settings define working time blocks, holidays, and task-specific calendar rules that affect how durations are calculated and how tasks are scheduled.

Calendar configuration is split into two levels:

- **Project calendar** — the default calendar applied to the entire project (applies globally)
- **Task calendars** — custom calendars assigned to specific tasks through `taskFields.calendarId` (applies per-task)

Calendar settings affect:

- Task start and end date calculation
- Duration conversion between hours and days
- Dependency-based scheduling and offset calculations
- Weekend and holiday handling (when combined with `workWeek` property)
- Working time rules for project and task scopes
- How display duration in days relates to actual working hours via `hoursPerDay`

### Key APIs

| Property | Type | Purpose |
|---|---|---|
| `calendarSettings.projectCalendar` | `ProjectCalendarModel` | Defines default working time, holidays, and exceptions for the project |
| `calendarSettings.taskCalendar` | `TaskCalendarModel[]` | Defines custom calendars for specific tasks |
| `taskFields.calendarId` | `string` | Maps a task's calendar ID field to bind it to a task calendar |
| `hoursPerDay` | `number` | Defines the working hours that equal one display day (default: 8) |
| `workWeek` | `string[]` | Defines which days of the week are working days (default: Mon–Fri) |
| `includeWeekend` | `boolean` | Includes weekends in scheduling if set to `true` |
| `dayWorkingTime` | `DayWorkingTimeModel[]` | Defines global daily working hours (overridden by calendar working time) |

---

## Project Calendar

The [calendarSettings.projectCalendar](https://ej2.syncfusion.com/react/documentation/api/gantt/calendarSettingsModel#projectcalendar) property defines the default working calendar for the project. Tasks that do not specify a task calendar use this calendar.

### Typical project calendar configuration

```tsx
import * as React from 'react';
import { GanttComponent } from '@syncfusion/ej2-react-gantt';
import { ProjectCalendarModel } from '@syncfusion/ej2-gantt';

function App() {
  const projectCalendar: ProjectCalendarModel = {
    workingTime: [
      { from: 9, to: 12 },
      { from: 13, to: 17 }
    ]
  };

  return (
    <GanttComponent
      dataSource={data}
      taskFields={taskFields}
      calendarSettings={{ projectCalendar }}
      height='450px'
    />
  );
}
```

### Project calendar behavior

- Defines the default working time blocks for all tasks
- Applies when a task does not have an assigned task calendar via `calendarId`
- Works together with global Gantt settings such as `workWeek` (defines working days) and `includeWeekend` where applicable
- Holiday dates are treated as non-working days for the project
- Calendar exceptions override the default working time for specific dates

### Project calendar exceptions

Calendar exceptions allow overriding normal working rules for specific dates. Use exceptions when the project has special working days, partial work days, or date-specific schedule adjustments that do not fit the standard working pattern.

```tsx
const projectCalendar: ProjectCalendarModel = {
  workingTime: [{ from: 9, to: 17 }],
  holidays: [from: new Date('2024-12-25'), to: new Date('2024-12-25'), label: 'Local Holiday 1'],
  exceptions: [
    { from: new Date('2024-04-10'), to: new Date('2024-04-10'), isWorking: false },
    { from: new Date('2024-04-12'), to: new Date('2024-04-12'), workingTime: [{ from: 10, to: 14 }] }
  ]
};

<GanttComponent
  calendarSettings={{ projectCalendar }}
  ...
/>
```

**Exception properties:**
- `date` — the specific date to override
- `isWorking` — set to `false` to make the date non-working
- `workingTime` — override the working hours for that date

---

## Task Calendars

Task calendars enable different tasks to follow different working rules than the project calendar. This is useful for:

- Split-shift teams
- Region-specific working patterns
- External vendors with different holidays
- Tasks that follow a fixed calendar separate from the main project calendar

### Assign task-specific calendars

Define the task calendar collection in `calendarSettings.taskCalendar`, then map a calendar identifier to each task using `taskFields.calendarId`.

```tsx
import { TaskFieldsModel, TaskCalendarModel } from '@syncfusion/ej2-gantt';

const taskFields: TaskFieldsModel = {
  id: 'TaskID',
  name: 'TaskName',
  startDate: 'StartDate',
  duration: 'Duration',
  progress: 'Progress',
  calendarId: 'CalendarId'  // maps to the calendar identifier
};

const dayShiftCalendar: TaskCalendarModel = {
  calendarId: 'day-shift',
  holidays: [],
  exceptions: []
};

const nightShiftCalendar: TaskCalendarModel = {
  calendarId: 'night-shift',
  holidays: [],
  exceptions: []
};

const calendarSettings = {
  projectCalendar: {
    workingTime: [{ from: 9, to: 17 }]
  },
  taskCalendars: [dayShiftCalendar, nightShiftCalendar]
};
```

**Configuration steps:**
1. Add `calendarId` to `taskFields` to map your task data field
2. Define task calendars in `calendarSettings.taskCalendar` array, each with a unique `calendarId`
3. Set the `calendarId` value in your task data to bind a task to its calendar

### Task calendar precedence

When `calendarId` is assigned to a task:

- The task uses the assigned task calendar only
- The assigned task calendar overrides the project calendar for that task
- Working days, holidays, and exceptions from the task calendar control the task schedule
- Other task calendars are not used for that task

If `calendarId` is not provided, the task falls back to the project calendar.

### Task calendar example

```tsx
// Data with calendar assignments
const data = [
  { TaskID: 1, TaskName: 'Project Planning', StartDate: new Date('04/02/2024'), Duration: 5, CalendarId: 'day-shift' },
  { TaskID: 2, TaskName: 'Night Deployment', StartDate: new Date('04/08/2024'), Duration: 2, CalendarId: 'night-shift' },
  { TaskID: 3, TaskName: 'General Review', StartDate: new Date('04/10/2024'), Duration: 3 }  // no CalendarId, uses project calendar
];

// Calendars with different working times
const dayShift: TaskCalendarModel = {
  calendarId: 'day-shift',
  holidays: [from: new Date('2024-04-15'), to: new Date('2024-04-15') label: 'Holiday 1'],
  exceptions: []
};

const nightShift: TaskCalendarModel = {
  calendarId: 'night-shift',
  holidays: [from: new Date('2024-04-10'), to: new Date('2024-04-10'), label: 'Holiday 2'],  // different holiday for night team
  exceptions: [{ from: new Date('2024-04-16'), to: new Date('2024-04-16'), workingTime: [{ from: 20, to: 24 }] }]
};

const calendarSettings = {
  projectCalendar: {
    workingTime: [{ from: 9, to: 17 }]
  },
  taskCalendars: [dayShift, nightShift]
};

<GanttComponent
  dataSource={data}
  taskFields={taskFields}
  calendarSettings={calendarSettings}
  height='450px'
/>
```

**Result:**
- Task 1 uses day-shift calendar (8 AM–4 PM working time)
- Task 2 uses night-shift calendar (10 PM–6 AM working time)
- Task 3 uses project calendar (9 AM–5 PM working time)

### Task calendar holidays

Task calendars can define holidays that override project holidays for tasks assigned to that calendar. This lets a task follow a different holiday schedule without changing the project-wide calendar.

```tsx
const apacCalendar: TaskCalendarModel = {
  calendarId: 'apac-calendar',
  holidays: [
    { from: new Date('2024-04-10'), to: new Date('2024-04-10'), label: 'Local Holiday 1' },
    { from: new Date('2024-04-17'), to: new Date('2024-04-17'), label: 'Local Holiday 2' }
  ],
  exceptions: []
};

const calendarSettings = {
  projectCalendar: {
    workingTime: [{ from: 9, to: 17 }]
  },
  taskCalendars: [apacCalendar]
};

<GanttComponent
  calendarSettings={calendarSettings}
  ...
/>
```

**Holiday properties:**
- `from` — start date of the holiday (required)
- `to` — end date for multi-day holidays (optional)
- `label` — text displayed in the timeline (optional)
- `cssClass` — CSS class for custom styling (optional)

### Task calendar exceptions

Use task calendar exceptions when a task needs a special working day or a custom working-time range on specific dates.

```tsx
const supportCalendar: TaskCalendarModel = {
  calendarId: 'support-shift',
  holidays: [],
  exceptions: [
    { from: new Date('2024-04-15'), to: new Date('2024-04-15'), isWorking: false },
    { from: new Date('2024-04-16'), to: new Date('2024-04-16'),  workingTime: [{ from: 12, to: 20 }] }
  ]
};

<GanttComponent
  calendarSettings={{ taskCalendars: [supportCalendar] }}
  ...
/>
```

**Exception properties:**
- `date` — the specific date to override (required)
- `isWorking` — set to `false` to make the date non-working
- `workingTime` — custom working-time blocks for that date

---

## Hours Per Day

The `hoursPerDay` property defines how many working hours equal one day when the Gantt displays day-based durations. This affects how duration is displayed, not how tasks are scheduled.

```tsx
<GanttComponent
  dataSource={data}
  taskFields={taskFields}
  hoursPerDay={8}
  height='450px'
/>
```

**Default:** `8` hours per day

### Duration conversion behavior

- Day-based durations are converted using the configured `hoursPerDay` value
- Changing `hoursPerDay` updates the displayed duration in days
- Changing `hoursPerDay` does not change the underlying working duration of the task
- Start and end dates remain unchanged when only the displayed day conversion changes
- The task's actual scheduled duration is determined by `calendarSettings` and working time rules

### Example

If a task has 32 working hours of actual duration:

- With `hoursPerDay={8}`, the task displays as 4 days
- With `hoursPerDay={16}`, the same task displays as 2 days
- The task's start and end dates do not change; only the day-based display value changes

If the calendar's working time is configured as 24 hours (0:00 to 24:00), a task with 24 hours duration may occupy a single calendar day visually, even though the duration is specified as 4 days at `hoursPerDay={6}`.

---

## Calendar Behavior and Scheduling Impact

Calendar settings directly influence task scheduling and duration calculations.

### Working time

- Task durations are computed using only the working time defined in the active calendar
- Non-working time is skipped during duration calculation
- Working time blocks are defined per calendar and applied to all working days
- Partial-day work can span multiple calendar dates if not completed within a single day's working time

### Weekends

- Weekend handling depends on the global `workWeek` property and the active calendar's working time
- If weekends are excluded from `workWeek`, tasks skip Saturday and Sunday during scheduling
- If weekends are included in `workWeek` and the calendar defines working time for them, weekend dates count toward duration
- Calendar-level working time overrides `dayWorkingTime` for tasks using that calendar

### Holidays

- Holidays are treated as non-working days for the active calendar
- Tasks skip holiday dates when calculating end dates
- Project calendar holidays apply to tasks without an assigned task calendar
- Task calendars can define their own holidays, overriding project holidays for assigned tasks

### Dependencies

- Dependency calculations use the active calendar of the predecessor and successor tasks
- Successor tasks are scheduled using the predecessor relationship and the successor's calendar rules
- Calendar differences can change the visible gap between linked tasks on the timeline
- Predecessor offset updates account for calendar changes if `autoUpdatePredecessorOffset={true}`

### Duration calculations

- Day-based duration values are converted using `hoursPerDay`
- The displayed duration in days changes if `hoursPerDay` is updated
- The underlying working duration is not affected by `hoursPerDay` changes
- Changing calendar rules may change the computed end date even if duration stays the same
- A task calendar only affects tasks assigned via `taskFields.calendarId`

### Project vs task calendar fallback

- Tasks without `calendarId` use the project calendar's working time rules
- Tasks with `calendarId` use the assigned task calendar exclusively
- Task calendar rules do not merge with project calendar rules for the same task
- If `calendarId` references a non-existent calendar, the task falls back to the project calendar

---

## Supported Scenarios

Task Calendar is useful for:

- A project that has a default weekday calendar, but specific tasks run on night shifts
- Teams in different regions with different working days or holidays
- Vendor or subcontractor tasks that use their own schedule rules
- Projects that require special working dates for selected milestones or work packages
- Mixed scheduling where some tasks use the project calendar and others use a task calendar

---

## Feature Limitations

- A task can follow only one assigned task calendar at a time
- Task calendars do not merge with the project calendar for the same task
- Invalid or missing `calendarId` values should be handled in the data source before binding
- Calendar changes can affect calculated dates and dependency offsets, so verify schedules after updating working rules
- Day-based duration display depends on the configured `hoursPerDay` value and the calendar's working hours

---

## Best Practices

- Use the project calendar for the default schedule and reserve task calendars for exceptions only
- Keep calendar identifiers (`calendarId` values) stable and predictable in the data source
- Define working time consistently across calendar definitions to avoid ambiguous duration results
- Use calendar exceptions for date-specific changes rather than creating separate calendars for minor overrides
- Validate dependency-driven tasks after changing calendars, as calculated start or end dates may shift
- Document task calendar assignments in the task data model and data dictionary for maintainability
- Verify task calendar holidays do not conflict with project holidays unintentionally
- Test calendar changes with a sample set of dependent tasks to ensure scheduling behaves as expected
- Avoid nesting multiple levels of exceptions; keep exceptions clear and limited in scope
- Review `autoUpdatePredecessorOffset` and `updateOffsetOnTaskbarEdit` settings when using task calendars with dependencies

---

---

## Complete Example: Multi-Calendar Project

Here's a full example with project and task calendars working together:

```tsx
import * as React from 'react';
import {
  GanttComponent,
  ColumnsDirective,
  ColumnDirective,
  Inject,
  Edit,
  Toolbar,
  Selection
} from '@syncfusion/ej2-react-gantt';
import {
  TaskFieldsModel,
  ProjectCalendarModel,
  TaskCalendarModel,
  EditSettingsModel
} from '@syncfusion/ej2-gantt';

function App() {
  // Task data with calendar assignments
  const data = [
    {
      TaskID: 1,
      TaskName: 'Project Initiation',
      StartDate: new Date('2024-04-02'),
      Duration: 4,
      Progress: 50,
      CalendarId: null  // uses project calendar
    },
    {
      TaskID: 2,
      TaskName: 'Day Shift Development',
      StartDate: new Date('2024-04-08'),
      Duration: 5,
      Progress: 30,
      CalendarId: 'day-shift'
    },
    {
      TaskID: 3,
      TaskName: 'Night Shift Deployment',
      StartDate: new Date('2024-04-15'),
      Duration: 3,
      Progress: 0,
      CalendarId: 'night-shift'
    },
    {
      TaskID: 4,
      TaskName: 'Extended Hours Support',
      StartDate: new Date('2024-04-20'),
      Duration: 2,
      Progress: 0,
      CalendarId: 'extended-hours'
    }
  ];

  // Task fields mapping
  const taskFields: TaskFieldsModel = {
    id: 'TaskID',
    name: 'TaskName',
    startDate: 'StartDate',
    duration: 'Duration',
    progress: 'Progress',
    calendarId: 'CalendarId'
  };

  // Project calendar: standard 9-5 working hours
  const projectCalendar: ProjectCalendarModel = {
    workingTime: [
      { from: 9, to: 12 },   // 9 AM – 12 PM
      { from: 13, to: 17 }   // 1 PM – 5 PM
    ],
    holidays: [
      { from: new Date('2024-04-10'), to: new Date('2024-04-10'), label: 'Company Holiday' }
    ],
    exceptions: []
  };

  // Task calendar: day shift 8 AM – 4 PM
  const dayShiftCalendar: TaskCalendarModel = {
    calendarId: 'day-shift',
    holidays: [
      { from: new Date('2024-04-10'), to: new Date('2024-04-10'), label: 'Holiday (Day Shift)' }
    ],
    exceptions: [
      {
        from: new Date('2024-04-12'),
        to: new Date('2024-04-12'),
        workingTime: [{ from: 8, to: 16 }]  // full 8 hours
      }
    ]
  };

  // Task calendar: night shift 10 PM – 6 AM
  const nightShiftCalendar: TaskCalendarModel = {
    calendarId: 'night-shift',
    holidays: [
      { from: new Date('2024-04-15'), to: new Date('2024-04-15'), label: 'Holiday (Night Shift)' }
    ],
    exceptions: []
  };

  // Task calendar: extended hours 7 AM – 7 PM
  const extendedHoursCalendar: TaskCalendarModel = {
    calendarId: 'extended-hours',
    holidays: [],
    exceptions: []
  };

  // Edit settings
  const editSettings: EditSettingsModel = {
    allowEditing: true,
    allowAdding: true,
    allowDeleting: true,
    allowTaskbarEditing: true
  };

  return (
    <GanttComponent
      dataSource={data}
      taskFields={taskFields}
      editSettings={editSettings}
      calendarSettings={{
        projectCalendar,
        taskCalendars: [dayShiftCalendar, nightShiftCalendar, extendedHoursCalendar]
      }}
      hoursPerDay={8}
      workWeek={['Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday']}
      toolbar={['Add', 'Edit', 'Delete', 'Update', 'Cancel']}
      height='450px'
    >
      <ColumnsDirective>
        <ColumnDirective field='TaskID' width='80' />
        <ColumnDirective field='TaskName' headerText='Task Name' width='250' />
        <ColumnDirective field='CalendarId' headerText='Calendar' width='120' />
        <ColumnDirective field='StartDate' />
        <ColumnDirective field='Duration' />
        <ColumnDirective field='Progress' />
      </ColumnsDirective>
      <Inject services={[Edit, Toolbar, Selection]} />
    </GanttComponent>
  );
}

export default App;
```

---

## API Properties Reference

### ProjectCalendarModel

```typescript
export interface ProjectCalendarModel {
  // Working time blocks for each day (e.g., 9-12, 13-17)
  workingTime?: DayWorkingTimeModel[];

  // Holiday dates that are non-working
  holidays?: HolidayModel[];

  // Exceptions that override default working time for specific dates
  exceptions?: CalendarExceptionModel[];
}
```

### TaskCalendarModel

```typescript
export interface TaskCalendarModel {
  // Unique identifier for this calendar (referenced via taskFields.calendarId)
  calendarId?: string;

  // Holiday dates for this calendar (overrides project holidays)
  holidays?: HolidayModel[];

  // Exceptions that override working time for specific dates
  exceptions?: CalendarExceptionModel[];
}
```

### DayWorkingTimeModel

```typescript
export interface DayWorkingTimeModel {
  // Start hour in 24-hour format (0–23)
  from?: number;

  // End hour in 24-hour format (0–23)
  to?: number;
}
```

### HolidayModel

```typescript
export interface HolidayModel {
  // Start date of the holiday (required)
  from?: Date;

  // End date for multi-day holidays (optional)
  to?: Date;

  // Text label displayed in the timeline
  label?: string;

  // CSS class for styling
  cssClass?: string;
}
```

### CalendarExceptionModel

```typescript
export interface CalendarExceptionModel {
  // Specific date to override (required)
  date?: Date;

  // Set to false to mark as non-working; omit for working day
  isWorking?: boolean;

  // Custom working time blocks for this date
  workingTime?: DayWorkingTimeModel[];
}
```

---

## See Also

- [Calendar Settings in React Gantt Chart Component | Syncfusion](https://help.syncfusion.com/gantt-sdk/react/gantt-chart/calendar-settings)
- [Holidays, Working Time & Timezone](holidays-working-time.md)
- [Task Scheduling](task-scheduling.md)
- [Task Dependency](task-dependency.md)
- [Resources](resources.md)