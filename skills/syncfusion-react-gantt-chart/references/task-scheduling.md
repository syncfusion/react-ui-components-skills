# Task Scheduling in Syncfusion React Gantt Chart

## Table of Contents
- [Scheduling Modes Overview](#scheduling-modes-overview)
- [Auto Scheduling (Default)](#auto-scheduling-default)
- [Manual Scheduling](#manual-scheduling)
- [Custom Scheduling (Mixed Mode)](#custom-scheduling-mixed-mode)
- [Duration Units](#duration-units)
- [Week and Month Duration Calculation](#week-and-month-duration-calculation)
- [daysPerWeek and daysPerMonth](#daysperweek-and-dayspermonth)
- [Unscheduled Tasks](#unscheduled-tasks)
- [Project Start and End Dates](#project-start-and-end-dates)
- [Choosing the Right Mode](#choosing-the-right-mode)

---

## Scheduling Modes Overview

The `taskMode` prop controls how task dates are calculated and validated:

| Mode | Value | Description |
|---|---|---|
| **Auto** | `"Auto"` (default) | Dates auto-validated from dependencies, working time, holidays, weekends |
| **Manual** | `"Manual"` | Dates fixed as given in data source; no auto-recalculation |
| **Custom** | `"Custom"` | Per-task: each task can be auto or manual, driven by a boolean field |

---

## Auto Scheduling (Default)

In **Auto** mode:
- Parent task dates are calculated from child tasks (min start → max end)
- Dates adjust automatically when predecessors change
- You cannot manually edit parent task dates/progress (they are derived)
- Dependencies, holidays, weekends, and working time all affect calculated dates

```tsx
import { GanttComponent, ColumnsDirective, ColumnDirective, Inject, Edit, Toolbar, Selection } from '@syncfusion/ej2-react-gantt';
import { TaskFieldsModel, EditSettingsModel } from '@syncfusion/ej2-gantt';

const taskFields: TaskFieldsModel = {
  id: 'TaskID',
  name: 'TaskName',
  startDate: 'StartDate',
  endDate: 'EndDate',
  duration: 'Duration',
  progress: 'Progress',
  parentID: 'ParentID',
};

const editSettings: EditSettingsModel = {
  allowEditing: true,
  allowDeleting: true,
  allowTaskbarEditing: true,
};

function App() {
  return (
    <GanttComponent
      dataSource={projectData}
      taskFields={taskFields}
      taskMode="Auto"
      editSettings={editSettings}
      toolbar={['Add', 'Edit', 'Update', 'Delete', 'Cancel', 'ExpandAll', 'CollapseAll']}
      height="450px"
    >
      <ColumnsDirective>
        <ColumnDirective field="TaskID"   headerText="Task ID"   width="100" />
        <ColumnDirective field="TaskName" headerText="Task Name" width="250" />
        <ColumnDirective field="StartDate" />
        <ColumnDirective field="Duration" />
        <ColumnDirective field="Progress" />
      </ColumnsDirective>
      <Inject services={[Edit, Toolbar, Selection]} />
    </GanttComponent>
  );
}
```

---

## Manual Scheduling

In **Manual** mode:
- Start and end dates are taken directly from the data source, with no auto-recalculation
- Parent task dates are also taken from data (not derived from children)
- Predecessors do NOT automatically shift successor dates
- Use `validateManualTasksOnLinking={true}` to enable predecessor-based validation only (dates adjust on linking, but not on other changes)

```tsx
function App() {
  return (
    <GanttComponent
      dataSource={data}
      taskFields={taskFields}
      taskMode="Manual"
      validateManualTasksOnLinking={true}  // optional: enable predecessor shifts
      editSettings={editSettings}
      toolbar={['Add', 'Edit', 'Update', 'Delete', 'Cancel']}
      height="450px"
    >
      <ColumnsDirective>
        <ColumnDirective field="TaskName"  headerText="Task Name" width="250" />
        <ColumnDirective field="StartDate" />
        <ColumnDirective field="Duration" />
        <ColumnDirective field="Progress" />
      </ColumnsDirective>
      <Inject services={[Edit, Toolbar, Selection]} />
    </GanttComponent>
  );
}
```

> **When to use Manual mode:** When your project data has fixed dates that should not be recalculated — e.g., contractually fixed milestones or imported schedules from external tools.

---

## Custom Scheduling (Mixed Mode)

In **Custom** mode, each task independently decides whether it is auto or manually scheduled, driven by a boolean field in the data source.

Map the boolean field using `taskFields.manual`:

```tsx
// Data example:
const data = [
  { TaskID: 1, TaskName: 'Design Phase',    StartDate: new Date('04/02/2024'), Duration: 5, isManual: false }, // auto
  { TaskID: 2, TaskName: 'Fixed Milestone', StartDate: new Date('04/10/2024'), Duration: 0, isManual: true  }, // manual
  { TaskID: 3, TaskName: 'Development',     StartDate: new Date('04/11/2024'), Duration: 7, isManual: false }, // auto
];

const taskFields: TaskFieldsModel = {
  id: 'TaskID',
  name: 'TaskName',
  startDate: 'StartDate',
  duration: 'Duration',
  progress: 'Progress',
  manual: 'isManual',   // <-- boolean field per task
};

function App() {
  return (
    <GanttComponent
      dataSource={data}
      taskFields={taskFields}
      taskMode="Custom"
      validateManualTasksOnLinking={true}
      editSettings={editSettings}
      height="450px"
    >
      <ColumnsDirective>
        <ColumnDirective field="TaskName" headerText="Task Name" width="250" />
        <ColumnDirective field="isManual" headerText="Manual"    width="100" />
        <ColumnDirective field="StartDate" />
        <ColumnDirective field="Duration" />
      </ColumnsDirective>
      <Inject services={[Edit, Toolbar, Selection]} />
    </GanttComponent>
  );
}
```

> **When to use Custom mode:** When a project has a mix — some tasks are fixed (milestones, contractual deliverables) and others should auto-schedule around dependencies.

---

## Duration Units

Duration units define how the scheduler interprets a task’s duration field before it calculates start and end dates. In this component, the unit can come from the global setting, from the task record, or directly from the duration text.

### Supported duration units

| Unit | Typical use | Scheduling effect |
|---|---|---|
| `Day` | General planning | Moves the task in whole-day blocks |
| `Hour` | Short or shift-based work | Uses daily working hours and time slots |
| `Minute` | Very short tasks | Useful for precision work, meetings, and burst tasks |
| `Week` | Sprint planning, iteration planning | Converts the duration using `daysPerWeek` before scheduling |
| `Month` | Long phases, roadmap planning | Converts the duration using `daysPerMonth` before scheduling |

### How duration is resolved

The scheduler checks duration input in this order:

1. A unit embedded in the duration text, such as `5 days`, `2 weeks`, or `1 month`
2. `taskFields.durationUnit` on the task row
3. The global `durationUnit` property on the component

This order lets a project keep a single default while still supporting imported rows or mixed-unit planning.

### What changes when Week or Month is used

`Week` and `Month` are not treated as fixed calendar spans. They are translated into working-day equivalents and then passed through the normal calendar rules.

- `Week` uses the `daysPerWeek` value as the conversion base
- `Month` uses the `daysPerMonth` value as the conversion base
- Weekend, holiday, and task-calendar rules are still applied after the conversion
- Taskbar dragging can change the displayed dates even when the original duration value stays the same

### Example — mixed duration sources

```tsx
const taskFields: TaskFieldsModel = {
  id: 'TaskID',
  name: 'TaskName',
  startDate: 'StartDate',
  duration: 'Duration',
  durationUnit: 'DurationUnit',
  progress: 'Progress',
  parentID: 'ParentID',
};

const data = [
  { TaskID: 1, TaskName: 'Planning', StartDate: new Date('04/02/2024'), Duration: '5 days' },
  { TaskID: 2, TaskName: 'Execution', StartDate: new Date('04/09/2024'), Duration: 2, DurationUnit: 'Week' },
  { TaskID: 3, TaskName: 'Closure', StartDate: new Date('04/30/2024'), Duration: 1, DurationUnit: 'Month' },
];
```

### Example — global duration unit with overrides

```tsx
<GanttComponent
  dataSource={data}
  taskFields={taskFields}
  durationUnit="Day"
  height="450px"
/>
```

---

## Week and Month Duration Calculation

When a task uses `Week` or `Month`, the scheduler converts the value to working days before it derives the end date. The conversion is calendar-aware, so the final result still respects weekends, holidays, and the configured work week.

### Week duration behavior

- `1 Week` equals `daysPerWeek` working days.
- The conversion happens before dependency logic is evaluated.
- If the project uses a 5-day schedule, a 2-week task behaves like 10 working days, not 14 calendar days.
- This is useful for sprint planning because the estimate stays in work units rather than calendar units.

### Month duration behavior

- `1 Month` equals `daysPerMonth` working days.
- The conversion is applied before the task is placed on the timeline.
- A 1-month task behaves like a fixed planning month, not a calendar-month hop.
- This keeps month-based estimates stable across different calendar lengths.

### Scheduling flow

For both `Week` and `Month`, the scheduler follows this flow:

1. Read the task duration and unit
2. Convert the value to working days using the configured base
3. Apply work week, weekends, holidays, and working time rules
4. Resolve predecessor links and parent rollups
5. Recalculate the final visible start and end dates

### Example — week and month tasks in one project

```tsx
const taskFields: TaskFieldsModel = {
  id: 'TaskID',
  name: 'TaskName',
  startDate: 'StartDate',
  duration: 'Duration',
  durationUnit: 'DurationUnit',
  progress: 'Progress',
};

const data = [
  { TaskID: 1, TaskName: 'Sprint Cycle', StartDate: new Date('04/01/2024'), Duration: 2, DurationUnit: 'Week' },
  { TaskID: 2, TaskName: 'Release Phase', StartDate: new Date('04/15/2024'), Duration: 1, DurationUnit: 'Month' },
];

<GanttComponent
  dataSource={data}
  taskFields={taskFields}
  daysPerWeek={5}
  daysPerMonth={20}
  height="450px"
/>
```

### Limits and edge cases

- A `Week` value is not always 7 calendar days.
- A `Month` value is not the same as a calendar month boundary.
- A task may still end later than the converted duration suggests if it crosses non-working periods.
- Editing or dragging a taskbar can change the resulting dates even if the original duration text stays the same.
- If the schedule uses manual task dates, the conversion still matters once the user edits the task or a dependency recalculates it.

---

## daysPerWeek and daysPerMonth

`daysPerWeek` and `daysPerMonth` are the conversion controls used by `Week` and `Month` duration units.

### `daysPerWeek`

`daysPerWeek` defines how many working days count as one week in the schedule.

- Used when the task duration or task unit is `Week`
- Affects successor movement when dependency links recalculate the schedule
- Impacts parent task rollups when child tasks are week-based
- Should match the project’s work-week rule

### `daysPerMonth`

`daysPerMonth` defines how many working days count as one month in the schedule.

- Used when the task duration or task unit is `Month`
- Affects end-date derivation and schedule extension for month-based tasks
- Impacts parent rollups and dependency validation for long-range plans
- Should match the organization’s planning policy rather than the calendar month length

### How they affect scheduling

These values are read during duration conversion, so they influence the whole scheduling pipeline:

- Start date placement for new tasks
- End date calculation after conversion
- Duration recalculation during edit and drag operations
- Dependency validation when a linked task shifts
- Parent task date rollups in auto scheduling
- Rescheduling after the working calendar changes

### Example — project conversion settings

```tsx
<GanttComponent
  dataSource={data}
  taskFields={taskFields}
  daysPerWeek={5}
  daysPerMonth={20}
  includeWeekend={false}
  dayWorkingTime={[{ from: 8, to: 17 }]}
  height="450px"
/>
```

### Recommended values

- `daysPerWeek = 5` for Monday-to-Friday planning
- `daysPerWeek = 6` for six-day operating schedules
- `daysPerMonth = 20` for many project-tracking scenarios
- Keep the same values across all views and exports that represent the same schedule

---

## Unscheduled Tasks

A task is "unscheduled" when its date/duration information is incomplete. Gantt renders these as special taskbars.

| Scenario | Data | Rendering |
|---|---|---|
| Duration only | `Duration: 3` (no dates) | Taskbar positioned at project start |
| Start date only | `StartDate: ...` (no duration/end) | Open-ended taskbar |
| End date only | `EndDate: ...` (no start/duration) | Taskbar ending at given date |
| No dates at all | No start, end, or duration | Milestone at project start |

Enable unscheduled task support by providing `allowUnscheduledTasks={true}`:

```tsx
<GanttComponent
  dataSource={data}
  taskFields={taskFields}
  allowUnscheduledTasks={true}
  height="450px"
/>
```

---

## Project Start and End Dates

Explicitly set the visible timeline range using `projectStartDate` and `projectEndDate`:

```tsx
<GanttComponent
  dataSource={data}
  taskFields={taskFields}
  projectStartDate={new Date('03/25/2024')}
  projectEndDate={new Date('07/28/2024')}
  height="450px"
/>
```

Without these, Gantt auto-calculates the range from the earliest start and latest end date in your data.

---

## Choosing the Right Mode

| Need | Use |
|---|---|
| Standard project plan that auto-adjusts when tasks change | `taskMode="Auto"` |
| Fixed schedule imported from another tool (e.g., Excel) | `taskMode="Manual"` |
| Mix of fixed milestones and flexible tasks | `taskMode="Custom"` |
| Predecessor changes should still shift manual tasks | Add `validateManualTasksOnLinking={true}` |
| Tasks with incomplete date info | Add `allowUnscheduledTasks={true}` |
