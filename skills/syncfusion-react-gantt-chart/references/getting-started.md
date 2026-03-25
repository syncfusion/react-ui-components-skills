# Getting Started with Syncfusion React Gantt Chart

## Table of Contents
- [Dependencies](#dependencies)
- [Project Setup](#project-setup)
- [Install the Package](#install-the-package)
- [Add CSS Imports](#add-css-imports)
- [Basic Gantt Implementation](#basic-gantt-implementation)
- [taskFields Mapping](#taskfields-mapping)
- [Module Injection](#module-injection)
- [Common Setup Gotchas](#common-setup-gotchas)

---

## Dependencies

The Gantt component requires the following npm packages:

```bash
npm install @syncfusion/ej2-react-gantt --save
```

This installs the main package along with its peer dependencies:
- `@syncfusion/ej2-grids`
- `@syncfusion/ej2-gantt`
- `@syncfusion/ej2-layouts`
- `@syncfusion/ej2-treegrid`

---

## Project Setup

**Using Vite (recommended):**

```bash
# JavaScript
npm create vite@latest my-app -- --template react
cd my-app
npm install
npm run dev

# TypeScript
npm create vite@latest my-app -- --template react-ts
cd my-app
npm install
npm run dev
```

---

## Install the Package

```bash
npm install @syncfusion/ej2-react-gantt --save
```

---

## Add CSS Imports

Add all required CSS imports to `src/App.css`:

```css
@import "../node_modules/@syncfusion/ej2-base/styles/tailwind3.css";
@import "../node_modules/@syncfusion/ej2-buttons/styles/tailwind3.css";
@import "../node_modules/@syncfusion/ej2-calendars/styles/tailwind3.css";
@import "../node_modules/@syncfusion/ej2-dropdowns/styles/tailwind3.css";
@import "../node_modules/@syncfusion/ej2-gantt/styles/tailwind3.css";
@import "../node_modules/@syncfusion/ej2-grids/styles/tailwind3.css";
@import "../node_modules/@syncfusion/ej2-inputs/styles/tailwind3.css";
@import "../node_modules/@syncfusion/ej2-layouts/styles/tailwind3.css";
@import "../node_modules/@syncfusion/ej2-lists/styles/tailwind3.css";
@import "../node_modules/@syncfusion/ej2-navigations/styles/tailwind3.css";
@import "../node_modules/@syncfusion/ej2-notifications/styles/tailwind3.css";
@import "../node_modules/@syncfusion/ej2-popups/styles/tailwind3.css";
@import "../node_modules/@syncfusion/ej2-richtexteditor/styles/tailwind3.css";
@import "../node_modules/@syncfusion/ej2-treegrid/styles/tailwind3.css";
```

> Replace `tailwind3` with your preferred theme: `material`, `bootstrap5`, `fluent2`, `fabric`, etc.

Then import `App.css` at the top of `src/App.tsx`:

```tsx
import './App.css';
```

---

## Basic Gantt Implementation

Minimal working example with hierarchical data:

```tsx
import * as React from 'react';
import { GanttComponent, ColumnsDirective, ColumnDirective } from '@syncfusion/ej2-react-gantt';
import { TaskFieldsModel } from '@syncfusion/ej2-gantt';
import './App.css';

const data = [
  {
    TaskID: 1,
    TaskName: 'Project Initiation',
    StartDate: new Date('04/02/2024'),
    EndDate: new Date('04/21/2024'),
    subtasks: [
      { TaskID: 2, TaskName: 'Identify Site Location', StartDate: new Date('04/02/2024'), Duration: 4, Progress: 50 },
      { TaskID: 3, TaskName: 'Perform Soil Test',      StartDate: new Date('04/02/2024'), Duration: 4, Progress: 50 },
      { TaskID: 4, TaskName: 'Soil Test Approval',     StartDate: new Date('04/08/2024'), Duration: 0, Progress: 50 },
    ],
  },
  {
    TaskID: 5,
    TaskName: 'Project Estimation',
    StartDate: new Date('04/02/2024'),
    EndDate: new Date('04/21/2024'),
    subtasks: [
      { TaskID: 6, TaskName: 'Develop Floor Plan', StartDate: new Date('04/04/2024'), Duration: 3, Progress: 50 },
      { TaskID: 7, TaskName: 'List Materials',     StartDate: new Date('04/04/2024'), Duration: 3, Progress: 50 },
    ],
  },
];

const taskFields: TaskFieldsModel = {
  id: 'TaskID',
  name: 'TaskName',
  startDate: 'StartDate',
  endDate: 'EndDate',
  duration: 'Duration',
  progress: 'Progress',
  child: 'subtasks',
};

function App() {
  return (
    <GanttComponent dataSource={data} taskFields={taskFields} height="450px">
      <ColumnsDirective>
        <ColumnDirective field="TaskID"   width="80" />
        <ColumnDirective field="TaskName" headerText="Task Name" width="250" />
        <ColumnDirective field="StartDate" />
        <ColumnDirective field="Duration" />
        <ColumnDirective field="Progress" />
      </ColumnsDirective>
    </GanttComponent>
  );
}

export default App;
```

---

## taskFields Mapping

`taskFields` maps your data source fields to Gantt's internal model. All fields are optional except `id` and `name`.

```tsx
const taskFields: TaskFieldsModel = {
  id: 'TaskID',           // Unique task identifier (required)
  name: 'TaskName',       // Task display name (required)
  startDate: 'StartDate', // Task start date
  endDate: 'EndDate',     // Task end date
  duration: 'Duration',   // Duration in days (or with unit: "3 days", "5 hours")
  progress: 'Progress',   // Completion percentage (0-100)
  dependency: 'Predecessor', // Predecessor task IDs for dependencies
  child: 'subtasks',      // Nested child array (hierarchical data)
  parentID: 'ParentID',   // Parent task ID (self-referential/flat data)
  resourceInfo: 'Resources', // Assigned resources
  notes: 'Notes',         // Task notes
  cssClass: 'CssClass',   // Custom CSS class per task
  indicators: 'Indicators', // Task indicator icons/labels
  segments: 'Segments',   // Split task segments
  constraintType: 'ConstraintType', // Task constraint type
  constraintDate: 'ConstraintDate', // Constraint date
  baselineStartDate: 'BaselineStartDate', // Baseline start
  baselineEndDate: 'BaselineEndDate',     // Baseline end
  manual: 'isManual',     // Manual scheduling flag
};
```

**Key rule:** Use either `child` (hierarchical) **or** `parentID` (flat/self-referential), not both.

---

## Module Injection

Gantt features are modular. Inject only what you use to keep bundle size lean:

```tsx
import {
  GanttComponent,
  Inject,
  Edit,         // Cell/dialog/taskbar editing
  Filter,       // Column filtering
  Sort,         // Column sorting
  Toolbar,      // Toolbar items
  Selection,    // Row/cell selection
  ColumnMenu,   // Column header menu
  Reorder,      // Column drag-reorder
  Resize,       // Column resizing
  DayMarkers,   // Event markers and holidays
  ExcelExport,  // Excel export
  PdfExport,    // PDF export
  ContextMenu,  // Right-click context menu
  VirtualScroll, // Virtual scrolling
  UndoRedo,     // Undo/redo support
  RowDD,        // Row drag-and-drop
  CriticalPath, // Critical path highlighting
} from '@syncfusion/ej2-react-gantt';

function App() {
  return (
    <GanttComponent ...>
      <Inject services={[Edit, Filter, Sort, Toolbar, Selection]} />
    </GanttComponent>
  );
}
```

---

## Common Setup Gotchas

**CSS not applied:**
- Ensure all CSS imports are in `App.css` and that file is imported in `App.tsx`.
- All 14 CSS packages are required — missing any will break the UI.

**TaskID 0 or undefined:**
- Gantt treats `TaskID: 0` as a root/invalid record. Start IDs from `1`.

**Tasks not rendering:**
- Verify `taskFields.id` and `taskFields.name` exactly match the field names in your data.
- For hierarchical data, `taskFields.child` must match the array property name.
- For flat data, `taskFields.parentID` must match the parent reference field.

**Height required:**
- Always set `height` prop (e.g., `height="450px"`) — the Gantt won't render without a defined height.

---

## Grid Lines

Control which lines appear in the TreeGrid section:

```tsx
<GanttComponent
  gridLines='Both'   // 'Both' | 'Horizontal' | 'Vertical' | 'None'
  dataSource={data}
  taskFields={taskFields}
  height='450px'
/>
```

| Value | Effect |
|---|---|
| `'Both'` | Horizontal and vertical lines between all cells |
| `'Horizontal'` | Only horizontal lines between rows |
| `'Vertical'` | Only vertical lines between columns |
| `'None'` | No grid lines |

Default is `'Horizontal'`.

**Parent task dates:**
- In Auto mode, parent task dates are calculated from child tasks and cannot be manually set.
- To set parent dates manually, use `taskMode="Manual"` or `taskMode="Custom"`.
