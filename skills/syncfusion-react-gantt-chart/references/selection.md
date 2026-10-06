# Selection in Syncfusion React Gantt Chart

## Table of Contents
- [Enable Selection](#enable-selection)
- [Row Selection](#row-selection)
- [Cell Selection](#cell-selection)
- [Checkbox Selection](#checkbox-selection)
- [Hierarchy Checkbox Selection](#hierarchy-checkbox-selection)
- [Selection Settings](#selection-settings)
- [Programmatic Selection API](#programmatic-selection-api)
- [Selection Events](#selection-events)

---

## Enable Selection

Selection is enabled by injecting the `Selection` module:

```tsx
import { GanttComponent, Inject, Selection } from '@syncfusion/ej2-react-gantt';

<GanttComponent allowSelection={true} ...>
  <Inject services={[Selection]} />
</GanttComponent>
```

---

## Row Selection

Single row selection (default):

```tsx
const selectionSettings = {
  mode: 'Row',
  type: 'Single',
};

<GanttComponent selectionSettings={selectionSettings} ...>
  <Inject services={[Selection]} />
</GanttComponent>
```

Multiple row selection:

```tsx
const selectionSettings = {
  mode: 'Row',
  type: 'Multiple',  // hold Ctrl to select multiple rows
};
```

Checkbox row selection:

```tsx
// Add a checkbox column for selection
<ColumnsDirective>
  <ColumnDirective type="checkbox" width="50" />
  <ColumnDirective field="TaskName" .../>
</ColumnsDirective>
```

---

## Cell Selection

```tsx
const selectionSettings = {
  mode: 'Cell',
  type: 'Multiple',
};

<GanttComponent selectionSettings={selectionSettings} ...>
  <Inject services={[Selection]} />
</GanttComponent>
```

---

## Checkbox Selection

Checkbox selection adds a dedicated checkbox column so users can select rows by clicking the checkbox instead of the row surface.

```tsx
<ColumnsDirective>
  <ColumnDirective field="CheckBox" showCheckbox={true} width="50" />
  <ColumnDirective field="TaskName" headerText="Task Name" width="250" />
</ColumnsDirective>
```

Use checkbox selection when you need a clearer visual affordance for row selection, especially in dense task hierarchies or when row click selection is already used for other interactions.

---

## Hierarchy Checkbox Selection

Hierarchy checkbox selection controls how checkbox state propagates across parent and child records in the task tree.

This feature uses the `hierarchyMode` property to determine whether selection stays local to the clicked row or propagates through the hierarchy.

### Enable Hierarchy Checkbox Selection

Enable the feature by combining checkbox selection with a hierarchy mode configuration:

```tsx
import { GanttComponent, ColumnsDirective, ColumnDirective, Inject, Selection } from '@syncfusion/ej2-react-gantt';

const selectionSettings = {
  mode: 'Row',
  type: 'Multiple',
};

const hierarchyCheckboxMode = 'Hierarchy';

<GanttComponent
  allowSelection={true}
  selectionSettings={selectionSettings}
  hierarchyCheckboxMode={hierarchyCheckboxMode}
  ...
>
  <Inject services={[Selection]} />
  <ColumnsDirective>
    <ColumnDirective field="CheckBox" showCheckbox={true} width="50" />
    <ColumnDirective field="TaskName" headerText="Task Name" width="250" />
  </ColumnsDirective>
</GanttComponent>
```

### Hierarchy Checkbox Mode

The `hierarchyMode` property defines how checkbox selection behaves within the parent-child structure.

| Value | Default | Description |
|---|---|---|
| `Self` | No | Selects only the clicked record |
| `Hierarchy` | Yes | Propagates selection to related parent and child records |
| `FilteredHierarchy` | No | Propagates selection only within the filtered or searched view |

### Mode Behavior

#### Self

- Selects only the current record.
- Parent selection does not affect child records.
- Child selection does not affect ancestors or siblings.
- Useful when each record must be managed independently.

#### Hierarchy

- Selects the current record and propagates selection through its hierarchy.
- Selecting a parent record selects its descendant records.
- Selecting a child record updates ancestor selection state according to the hierarchy selection rules.
- Collapsed descendants are still included because selection is based on the data hierarchy, not only on visible rows.
- This is the default hierarchy behavior.

#### FilteredHierarchy

- Behaves like `Hierarchy` for the visible filtered set.
- Selection propagates only to records currently visible after filtering or searching.
- Hidden records remain unchanged.
- Useful when users need selection to respect the current filtered context.

### Selection Propagation Rules

| Interaction | Self | Hierarchy | FilteredHierarchy |
|---|---|---|---|
| Select parent | Select parent only | Select parent + descendants + related hierarchy state | Select only visible related records |
| Select child | Select child only | Update related hierarchy state for ancestors and descendants | Update only visible related records |
| Collapse parent | No selection impact | No selection loss; hierarchy state is retained | No selection loss for visible filtered rows |
| Expand parent | No selection impact | Descendants remain selected if previously selected | Visible descendants reflect filtered hierarchy state |
| Filter rows | No special behavior | Selection can span filtered records and hidden hierarchy records | Selection affects only filtered results |

### Collapsed and Expanded Records

Hierarchy checkbox selection works consistently across collapsed and expanded nodes:

- Collapsed parents retain their selection state.
- Descendant records can remain selected even when not visible.
- Expanding a parent restores the visible selection state of its descendants.
- Collapsing and expanding rows does not clear hierarchy-driven selection.

### Filtering Behavior

Filtering changes how selection is interpreted only when `FilteredHierarchy` is used:

- In `Hierarchy`, filtering does not change how propagation works across the full hierarchy.
- In `FilteredHierarchy`, checkbox propagation is limited to records currently available in the filtered view.
- Records hidden by filtering or searching are ignored for propagation.
- This avoids selecting tasks that the user cannot currently see.

### Interaction with Other Selection Features

- **Row selection:** Hierarchy checkbox selection is compatible with row selection, but checkbox propagation is controlled independently by `hierarchyMode`.
- **Multiple selection:** Multiple row selection can coexist with checkbox selection when `selectionSettings.type` is `Multiple`.
- **Cell selection:** Cell selection is independent of hierarchy checkbox behavior.
- **Virtualization:** Hierarchy selection should preserve selection state across virtual scrolling because state is data-driven rather than DOM-driven.
- **Paging:** When paging is used, selection should be interpreted against the loaded data subset; use `FilteredHierarchy` when you need page-scoped behavior.
- **Sorting:** Sorting does not change hierarchy relationships, so propagated selection remains tied to the task tree.
- **Filtering/searching:** Selection propagation follows the configured hierarchy mode, with `FilteredHierarchy` limited to visible records.

### Best Practices

- Use `Self` when each task should be selected independently.
- Use `Hierarchy` when parent and child tasks should remain selection-aware as a group.
- Use `FilteredHierarchy` when users work primarily with filtered task subsets.
- Always define a checkbox column with `type="checkbox"` when using hierarchy checkbox selection.
- Test the selection experience with expanded, collapsed, filtered, and virtualized data sets.

### Example

```tsx
const selectionSettings = {
  mode: 'Row',
  type: 'Multiple',
  enableToggle: true,
};

<GanttComponent
  allowSelection={true}
  selectionSettings={selectionSettings}
  hierarchyCheckboxMode="FilteredHierarchy"
  allowFiltering={true}
  ...
>
  <Inject services={[Selection]} />
  <ColumnsDirective>
    <ColumnDirective field="CheckBox" showCheckbox={true} width="50" />
    <ColumnDirective field="TaskName" headerText="Task Name" width="250" />
    <ColumnDirective field="Duration" width="100" />
  </ColumnsDirective>
</GanttComponent>
```

---

## Selection Settings

| Prop | Values | Default | Description |
|---|---|---|---|
| `mode` | `'Row'` \| `'Cell'` | `'Row'` | Selection scope |
| `type` | `'Single'` \| `'Multiple'` | `'Single'` | How many items can be selected |
| `enableToggle` | boolean | `true` | Click selected row again to deselect |
| `persistSelection` | boolean | `false` | Maintain selection across data operations |
| `hierarchyMode` | `'Self'` \| `'Hierarchy'` \| `'FilteredHierarchy'` | `'Hierarchy'` | Controls checkbox selection propagation across hierarchy |

---

## Programmatic Selection API

```tsx
const ganttRef = React.useRef<GanttComponent>(null);

// Select rows by index
ganttRef.current?.selectRow(0);           // select first row
ganttRef.current?.selectRows([0, 2, 4]); // select multiple rows by index

// Select a specific cell: row index, column index
ganttRef.current?.selectCell({ rowIndex: 1, cellIndex: 2 });

// Clear all selections
ganttRef.current?.clearSelection();

// Get selected row indexes
const selectedIndexes = ganttRef.current?.getSelectedRowIndexes();
// Get selected records
const selectedRecords = ganttRef.current?.getSelectedRecords();
```

---

## Selection Events

```tsx
function rowSelected(args: any) {
  console.log('Selected row data:', args.data);
  console.log('Selected row index:', args.rowIndex);
}

function rowDeselected(args: any) {
  console.log('Deselected row data:', args.data);
}

function cellSelected(args: any) {
  console.log('Selected cell:', args.cellIndex, args.data);
}

<GanttComponent
  rowSelected={rowSelected}
  rowDeselected={rowDeselected}
  cellSelected={cellSelected}
  ...
/>
```
