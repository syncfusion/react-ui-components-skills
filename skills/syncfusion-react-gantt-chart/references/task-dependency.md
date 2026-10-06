# Task Dependency in Syncfusion React Gantt Chart

## Table of Contents
- [Overview](#overview)
- [Dependency Types](#dependency-types)
- [Configuring Dependencies in Data Source](#configuring-dependencies-in-data-source)
- [Full Example](#full-example)
- [Predecessor Offsets](#predecessor-offsets)
- [Drawing Dependencies via UI](#drawing-dependencies-via-ui)
- [Programmatic API](#programmatic-api)
- [Connector Line Customization](#connector-line-customization)
- [Validation and Conflict Handling](#validation-and-conflict-handling)
- [Allowed Dependency Types](#allowed-dependency-types)
- [Common Gotchas](#common-gotchas)

---

## Overview

Task dependencies in the Gantt Chart establish scheduling relationships between tasks. When a predecessor task's dates change, successor tasks automatically shift to maintain the defined relationship. Dependencies are mapped via `taskFields.dependency` and rendered as connector lines between taskbars.

The Gantt Chart supports both dependency parsing during data loading and dependency editing through the UI. By default, all supported dependency types are processed and can be created or updated. The `allowedDependencyTypes` property lets you restrict which relationship types are accepted during loading and editing.

---

## Dependency Types

Four relationship types are supported:

| Type | String Code | Meaning |
|---|---|---|
| Finish to Start | `FS` | Successor starts after predecessor finishes (most common) |
| Start to Start | `SS` | Successor starts when predecessor starts |
| Finish to Finish | `FF` | Successor finishes when predecessor finishes |
| Start to Finish | `SF` | Successor finishes when predecessor starts |

Specify the type in the dependency string: `"2FS"`, `"3SS"`, `"4FF"`, `"5SF"`.

These types are used consistently in data binding, dependency dialogs, taskbar editing, and programmatic predecessor APIs.

---

## Configuring Dependencies in Data Source

Add a `Predecessor` field to each task. The value is a string with one or more predecessor references:

```tsx
const data = [
  { TaskID: 1, TaskName: 'Requirements', StartDate: new Date('04/02/2024'), Duration: 5, Progress: 100, ParentID: null },
  { TaskID: 2, TaskName: 'Design', StartDate: new Date('04/09/2024'), Duration: 7, Progress: 60, ParentID: null, Predecessor: '1FS' },
  { TaskID: 3, TaskName: 'Development', StartDate: new Date('04/18/2024'), Duration: 10, Progress: 30, ParentID: null, Predecessor: '2FS' },
  { TaskID: 4, TaskName: 'Testing', StartDate: new Date('04/30/2024'), Duration: 5, Progress: 0, ParentID: null, Predecessor: '3FS' },
  // Multiple predecessors:
  { TaskID: 5, TaskName: 'Launch Review', StartDate: new Date('05/07/2024'), Duration: 2, Progress: 0, ParentID: null, Predecessor: '3FS,4FS' },
];
```

Map the field in `taskFields`:

```tsx
const taskFields: TaskFieldsModel = {
  id: 'TaskID',
  name: 'TaskName',
  startDate: 'StartDate',
  duration: 'Duration',
  progress: 'Progress',
  dependency: 'Predecessor',   // <-- maps the dependency string field
  parentID: 'ParentID',
};
```

If `allowedDependencyTypes` is configured, the Gantt validates the dependency type portion of each predecessor string during initial load. Strings whose relationship type is not allowed are ignored or rejected according to the current data operation path, and the corresponding relationship is not maintained in the chart.

---

## Full Example

```tsx
import * as React from 'react';
import {
  GanttComponent, ColumnsDirective, ColumnDirective, Inject, Edit, Toolbar, Selection
} from '@syncfusion/ej2-react-gantt';
import { TaskFieldsModel, EditSettingsModel } from '@syncfusion/ej2-gantt';

const data = [
  { TaskID: 1, TaskName: 'Site Analysis', StartDate: new Date('04/02/2024'), Duration: 4, Progress: 80, ParentID: null },
  { TaskID: 2, TaskName: 'Soil Test', StartDate: new Date('04/08/2024'), Duration: 4, Progress: 50, ParentID: null, Predecessor: '1FS' },
  { TaskID: 3, TaskName: 'Test Approval', StartDate: new Date('04/08/2024'), Duration: 0, Progress: 0, ParentID: null, Predecessor: '2FS' },
  { TaskID: 4, TaskName: 'Floor Plan', StartDate: new Date('04/04/2024'), Duration: 3, Progress: 50, ParentID: null, Predecessor: '1SS' },
  { TaskID: 5, TaskName: 'Cost Estimation', StartDate: new Date('04/16/2024'), Duration: 3, Progress: 0, ParentID: null, Predecessor: '3FS,4FS' },
];

const taskFields: TaskFieldsModel = {
  id: 'TaskID',
  name: 'TaskName',
  startDate: 'StartDate',
  duration: 'Duration',
  progress: 'Progress',
  dependency: 'Predecessor',
  parentID: 'ParentID',
};

const editSettings: EditSettingsModel = {
  allowEditing: true,
  allowAdding: true,
  allowDeleting: true,
  allowTaskbarEditing: true,
};

function App() {
  return (
    <GanttComponent
      dataSource={data}
      taskFields={taskFields}
      editSettings={editSettings}
      allowParentDependency={true}
      allowedDependencyTypes={['FS', 'SF']}
      toolbar={['Add', 'Edit', 'Update', 'Delete', 'Cancel']}
      height="450px"
    >
      <ColumnsDirective>
        <ColumnDirective field="TaskID" width="80" />
        <ColumnDirective field="TaskName" headerText="Task Name" width="200" />
        <ColumnDirective field="Predecessor" headerText="Dependency" width="150" />
        <ColumnDirective field="StartDate" />
        <ColumnDirective field="Duration" />
        <ColumnDirective field="Progress" />
      </ColumnsDirective>
      <Inject services={[Edit, Toolbar, Selection]} />
    </GanttComponent>
  );
}

export default App;
```

---

## Predecessor Offsets

Add a lag (positive offset) or lead (negative offset) to a dependency using `+` or `-` with a unit:

| Format | Meaning |
|---|---|
| `"2FS"` | Finish-to-Start, no offset |
| `"2FS+3"` | Finish-to-Start, 3-day lag |
| `"2FS-1"` | Finish-to-Start, 1-day lead (starts 1 day early) |
| `"2FS+3h"` | Finish-to-Start, 3-hour lag |
| `"2FS+30m"` | Finish-to-Start, 30-minute lag |
| `"2SS+2d"` | Start-to-Start, 2-day lag |

```tsx
// Examples in data source:
{ TaskID: 3, Predecessor: '2FS+2' }   // starts 2 days after task 2 finishes
{ TaskID: 4, Predecessor: '3FS-1d' }  // starts 1 day before task 3 finishes (lead)
{ TaskID: 5, Predecessor: '4SS+3h' }  // starts 3 hours after task 4 starts
```

When `allowedDependencyTypes` excludes a type, offsets for that type are not interpreted as active relationships. For example, if only `['SF']` is allowed, an entry such as `2FS+3d` is treated as disallowed because the `FS` relationship type itself is not allowed.

### Auto-update Predecessor Offset

When `autoUpdatePredecessorOffset={true}`, Gantt recalculates offset values on initial load to account for calendar rules (weekends, holidays):

```tsx
<GanttComponent autoUpdatePredecessorOffset={true} ... />
```

This property works with `allowedDependencyTypes` during initial load. Allowed relationships can still be normalized for calendar rules, while disallowed relationship types remain excluded.

---

## Drawing Dependencies via UI

Users can create dependencies interactively by enabling taskbar editing:

```tsx
const editSettings: EditSettingsModel = {
  allowTaskbarEditing: true,  // enables dragging connector points
};

<GanttComponent editSettings={editSettings} ...>
  <Inject services={[Edit]} />
</GanttComponent>
```

Users hover over a taskbar to see connection points (left/right sides), then drag from one point to another taskbar to create a dependency.

If `allowedDependencyTypes` is configured, the Gantt only allows connector creation for the permitted relationship types. Attempting to create a disallowed type is prevented by the component.

---

## Programmatic API

```tsx
const ganttRef = React.useRef<GanttComponent>(null);

// Add a predecessor: task 4 Finish-to-Start depends on task 3, with 2-day lag
ganttRef.current?.addPredecessor(4, '3FS+2');

// Remove all predecessors for task 4
ganttRef.current?.removePredecessor(4);

// Update predecessor for task 4
ganttRef.current?.updatePredecessor(4, '3SS');

<GanttComponent ref={ganttRef} ... />
```

Programmatic dependency updates obey `allowedDependencyTypes`.
If the supplied dependency string uses a type outside the configured collection, the update is blocked or ignored based on the current API path, and the Gantt does not persist the unsupported dependency.

---

## Connector Line Customization

Customize the appearance of dependency lines:

```tsx
<GanttComponent
  connectorLineWidth={2}              // line thickness in px
  connectorLineBackground="blue"      // CSS color value
  ...
/>
```

For dynamic per-connector styling, use `queryTaskbarInfo`:

```tsx
function queryTaskbarInfo(args: any) {
  if (args.data.taskData.Predecessor) {
    args.connectorLineColor = 'red';
  }
}

<GanttComponent queryTaskbarInfo={queryTaskbarInfo} ... />
```

Connector styling is visual only and does not override `allowedDependencyTypes` restrictions.

---

## Validation and Conflict Handling

When a user creates a dependency or edits a task that would cause a circular dependency or conflict, Gantt raises an `actionBegin` event:

```tsx
function actionBegin(args: any) {
  if (args.requestType === 'validateDependency') {
    // args.validateMode contains the conflict details
    // Set args.cancel = true to prevent the action
    if (/* your custom validation */ false) {
      args.cancel = true;
    }
  }
}

<GanttComponent actionBegin={actionBegin} ... />
```

### allowParentDependency

By default, `allowParentDependency={true}` allows dependency lines between parent and child tasks across different groups. Set to `false` to restrict dependencies to only sibling tasks:

```tsx
<GanttComponent allowParentDependency={false} ... />
```

## Predecessor Offset Auto-Synchronization

The `autoUpdatePredecessorOffset` property controls whether Gantt automatically recalculates and synchronizes predecessor offset values during initial data load, accounting for calendar rules (weekends, holidays):

```tsx
<GanttComponent
  autoUpdatePredecessorOffset={true}  // default: true — recalculates offsets on load
  ...
/>
```

**Behavior:**
- **Enabled:** Gantt recalculates offsets based on rendered dates after applying holidays/weekends. The predecessor column updates to reflect accurate offsets.
- **Disabled:** Predecessor column shows original offsets, even if they no longer match rendered dependency lines due to calendar adjustments.

---

## Disable Automatic Offset Updates During Taskbar Editing

The `updateOffsetOnTaskbarEdit` property controls whether offsets are automatically recalculated when users drag taskbars:

```tsx
<GanttComponent
  updateOffsetOnTaskbarEdit={false}  // default: true — prevents auto-updates during drag
  ...
/>
```

When set to `false`, users must manually update offsets via the dependency dialog or predecessor column.

---

## Dependency Validation Modes

When editing creates a dependency conflict, the `actionBegin` event fires with `requestType: 'validateLinkedTask'`. Three validation modes handle conflicts:

| Mode | Behavior |
|---|---|
| `respectLink` | Reverts invalid edits to maintain dependency integrity |
| `removeLink` | Removes conflicting dependencies to allow edits |
| `preserveLinkWithEditing` | Updates offsets to maintain links (default) |

```tsx
const actionBegin = (args: any) => {
  if (args.requestType === 'validateLinkedTask') {
    // Choose conflict resolution mode:
    args.validateMode.respectLink = true;        // revert edit
    // OR
    args.validateMode.removeLink = true;         // remove link
    // OR (default)
    args.validateMode.preserveLinkWithEditing = true; // update offset
  }
};

<GanttComponent actionBegin={actionBegin} .../>
```

---

## Validation Dialog

When all validation modes are disabled in `actionBegin`, Gantt displays a dialog prompting users to choose:
- Cancel the edit (keep link)
- Remove the link and move the task
- Adjust the link with offset

```tsx
const actionBegin = (args: any) => {
  if (args.requestType === 'validateLinkedTask') {
    args.validateMode.preserveLinkWithEditing = false;  // disable all modes → show dialog
  }
};
```

---

## Parent Dependencies

By default, `allowParentDependency={true}` allows dependencies between parent-parent, child-child, parent-child, and child-parent tasks. Set to `false` to restrict dependencies to sibling tasks only:

```tsx
<GanttComponent
  allowParentDependency={false}  // restrict to siblings only
  ...
/>
```

---

## Multiple Predecessors

A single task can have multiple predecessors. Specify them as a comma-separated string:

```tsx
const data = [
  { TaskID: 1, ... },
  { TaskID: 2, ... },
  { TaskID: 3, ... },
  // Task 4 depends on both task 2 and task 3:
  { TaskID: 4, TaskName: 'Approval', Predecessor: '2FS,3FS', ... },
];
```

---

## Show/Hide Dependency Lines Dynamically

Hide or show dependency lines by toggling CSS visibility on `.e-gantt-dependency-view-container`:

```tsx
const showDependencies = () => {
  const container = document.querySelector('.e-gantt-dependency-view-container') as HTMLElement;
  if (container) {
    container.style.visibility = 'visible';
  }
};

const hideDependencies = () => {
  const container = document.querySelector('.e-gantt-dependency-view-container') as HTMLElement;
  if (container) {
    container.style.visibility = 'hidden';
  }
};
```

---

## Disable Predecessor Validation

Set `enablePredecessorValidation={false}` to disable automatic date validation based on predecessor relationships:

```tsx
<GanttComponent enablePredecessorValidation={false} ... />
```

This property controls validation behavior after dependency relationships have already been accepted. It does not bypass `allowedDependencyTypes` filtering.

---

## Allowed Dependency Types

The `allowedDependencyTypes` property controls which dependency relationship types are accepted by the Gantt Chart during data loading, editing, and dependency maintenance.

### API

```tsx
<GanttComponent allowedDependencyTypes={['FS', 'SF']} ... />
```

### Accepted Values

The property accepts an array of dependency codes:

- `FS` — Finish to Start
- `SS` — Start to Start
- `FF` — Finish to Finish
- `SF` — Start to Finish

### Default Behavior

- If `allowedDependencyTypes` is not specified, all dependency types are allowed.
- If `allowedDependencyTypes` is set to an empty array, it behaves the same as not specifying the property.
- Only supported dependency types are considered; unsupported strings are ignored.

### Behavior

When the property is configured:

- dependency parsing checks the type portion of each predecessor string
- initial data binding keeps only allowed dependency relationships
- dependency drawing via the UI is limited to the configured types
- editing through the dependency dialog respects the configured types
- taskbar drag editing respects the configured types
- programmatic predecessor changes are validated against the same rules
- validation logic preserves only relationships that are allowed

### Supported Scenarios

#### Allow only Finish-to-Start

```tsx
<GanttComponent allowedDependencyTypes={['FS']} ... />
```

Only `FS` links can be created or preserved. `SS`, `FF`, and `SF` links are blocked.

#### Allow only Start-to-Finish

```tsx
<GanttComponent allowedDependencyTypes={['SF']} ... />
```

Only `SF` links are accepted. This is useful when a workflow requires a task to finish when its predecessor begins.

#### Allow multiple types

```tsx
<GanttComponent allowedDependencyTypes={['FS', 'SS']} ... />
```

Only `FS` and `SS` relationships are allowed. `FF` and `SF` dependencies are not processed.

#### Allow all types explicitly

```tsx
<GanttComponent allowedDependencyTypes={['FS', 'SS', 'FF', 'SF']} ... />
```

This is equivalent to the default behavior.

### Existing Dependency Validation

When a restricted configuration is applied, existing dependency strings are validated against the allowed list:

- Allowed relationships remain active and render as dependency lines.
- Disallowed relationships are not maintained in the chart.
- If a task has multiple predecessors, allowed links are retained while disallowed links are ignored.
- During editing, a user cannot change a task in a way that introduces a disallowed dependency type.
- When validation or dependency maintenance recalculates offsets, only allowed relationships participate in the recalculation.

### Dependency Editing Interactions

`allowedDependencyTypes` works alongside editing features:

- **Dependency dialog editing**: only permitted types should be selectable or committed.
- **Taskbar editing**: connector-based creation follows the allowed list.
- **Predecessor validation**: conflict resolution only applies to accepted links.
- **Programmatic APIs**: `addPredecessor`, `removePredecessor`, and `updatePredecessor` should be used with strings that match the allowed types.
- **Initial data parsing**: dependency strings in the data source are parsed, but only accepted if the type is allowed.

### Best Practices

- Define `allowedDependencyTypes` only when the workflow requires stricter dependency rules.
- Keep the array small and explicit so developers know which relationship types are supported.
- Use the same codes consistently in the data source, UI, and programmatic APIs.
- Test mixed-predecessor tasks to confirm that allowed links are retained and disallowed links are ignored.

---

## Common Gotchas

| Issue | Cause | Fix |
|---|---|---|
| Dependency not drawn | `dependency` not in `taskFields` | Add `dependency: 'Predecessor'` to `taskFields` |
| Circular dependency error | Task A → B → A | Redesign dependencies to avoid cycles |
| Offset not applied | Wrong format in data string | Use format `"2FS+3"` or `"2FS+3d"` |
| Offsets don't recalculate on load | `autoUpdatePredecessorOffset` false | Set to `true` for automatic recalculation |
| Offsets auto-update on drag (unwanted) | `updateOffsetOnTaskbarEdit` true | Set to `false` for manual updates only |
| Validation dialog not appearing | All validation modes enabled in actionBegin | Disable all modes: `args.validateMode = { respectLink: false, removeLink: false, preserveLinkWithEditing: false }` |
| Parent task dependency not working | `allowParentDependency` false | Set `allowParentDependency={true}` |
| Dependencies removed unexpectedly | `enablePredecessorValidation` true with conflicting edits | Set `enablePredecessorValidation={false}` if strict validation not needed |
| Dependency lines not visible | Container visibility hidden | Check CSS; unhide `.e-gantt-dependency-view-container` |
| Disallowed dependency missing | `allowedDependencyTypes` excludes the type | Add the dependency code to `allowedDependencyTypes` |

---

## Limitation

> When `allowedDependencyTypes` is configured, unsupported dependency relationships are filtered during parsing and editing. If a task depends on a mix of allowed and disallowed types, only the allowed relationships remain active.

## See also
- [How to configure task constraints?](https://ej2.syncfusion.com/react/documentation/gantt/task-constraints)
- [How to customize taskbars?](https://ej2.syncfusion.com/react/documentation/gantt/taskbar)
- [How to enable critical path?](https://ej2.syncfusion.com/react/documentation/gantt/critical-path)
