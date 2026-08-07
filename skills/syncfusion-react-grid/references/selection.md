# Selection in React Grid

## Table of Contents
- [Overview](#overview)
- [When to Use This Skill](#when-to-use-this-skill)
- [Enable Selection](#enable-selection)
- [Selection Settings](#selection-settings)
- [Row Selection](#row-selection)
- [Cell Selection](#cell-selection)
- [Column Selection](#column-selection)
- [Checkbox Selection](#checkbox-selection)
- [Persist Selection](#persist-selection)
- [Programmatic Selection](#programmatic-selection)
- [Selection Events](#selection-events)
- [Important Notes](#important-notes)

## Overview

Selection in the Syncfusion React Grid lets users highlight rows, cells, or columns. Use `selectionSettings` to switch between selection modes, enable checkbox-based row selection, and control behavior for mouse, keyboard, and touch interactions.

By default, selection is enabled (`allowSelection: true`) and the grid uses `mode: 'Row'` with `type: 'Single'` unless you override those settings.

## When to Use This Skill

Use this skill when you need to:
- enable row selection for records or bulk actions
- allow cell selection for data entry or inspection workflows
- enable column selection for analysis or formatting tasks
- add checkbox-based multi-row selection
- persist selection across paging or refresh
- select, clear, or inspect items programmatically
- respond to row, cell, or column selection events

## Enable Selection

### Basic setup

```tsx
import { GridComponent, ColumnsDirective, ColumnDirective } from '@syncfusion/ej2-react-grids';

const selectionSettings = {
  mode: 'Row',
  type: 'Multiple'
};

<GridComponent dataSource={data} allowSelection={true} selectionSettings={selectionSettings}>
  <ColumnsDirective>
    <ColumnDirective field='OrderID' headerText='Order ID' width='120' />
    <ColumnDirective field='CustomerID' headerText='Customer ID' width='150' />
    <ColumnDirective field='Freight' headerText='Freight' width='100' format='C2' />
  </ColumnsDirective>
</GridComponent>
```

## Selection Settings

Use these properties inside `selectionSettings` to control selection behavior:

- `mode`: `'Row' | 'Cell' | 'Both'` (default is `Row`)
- `type`: `'Single' | 'Multiple'`
- `allowColumnSelection`: enables column header selection
- `checkboxOnly`: allows selection only when clicking the checkbox column
- `checkboxMode`: `'Default' | 'ResetOnRowClick'`
- `enableToggle`: allows a selected item to be deselected by clicking it again
- `enableSimpleMultiRowSelection`: enables multiple row selection with a single click
- `persistSelection`: keeps selection across paging and refresh for row/column selection
- `cellSelectionMode`: `'Flow' | 'Box' | 'BoxWithBorder'`
- `isRowSelectable`: callback used to conditionally allow or block row selection

```tsx
const selectionSettings = {
  mode: 'Row',
  type: 'Multiple',
  allowColumnSelection: false,
  checkboxOnly: false,
  checkboxMode: 'Default',
  enableToggle: true,
  enableSimpleMultiRowSelection: true,
  persistSelection: true,
  cellSelectionMode: 'Flow'
};
```

Common interactions:
- `Ctrl + Click` adds or removes separate rows, cells, or columns.
- `Shift + Click` selects a range of rows, cells, or columns.

## Row Selection

Use row selection when you want users to select records rather than individual cells.

```tsx
const selectionSettings = {
  mode: 'Row',
  type: 'Multiple'
};
```

### Single row selection

```tsx
const selectionSettings = {
  mode: 'Row',
  type: 'Single'
};
```

### Multiple row selection by single click

```tsx
const selectionSettings = {
  mode: 'Row',
  type: 'Multiple',
  enableSimpleMultiRowSelection: true
};
```

### Toggle selection with a single click

Use this option when you want a clicked item to toggle between selected and deselected states. It gives a simple one-click interaction for rows or cells.

```tsx
const selectionSettings = {
  mode: 'Row',
  type: 'Multiple',
  enableToggle: true
};
```

### Preselect a row

```tsx
<GridComponent dataSource={data} selectedRowIndex={1} selectionSettings={{ mode: 'Row', type: 'Single' }}>
  <ColumnsDirective>
    <ColumnDirective field='OrderID' headerText='Order ID' width='120' />
    <ColumnDirective field='CustomerID' headerText='Customer ID' width='150' />
  </ColumnsDirective>
</GridComponent>
```

### Conditional row selection

Use this when only some rows should be selectable. For example, you can block selection for canceled or locked records by returning `false` for those rows.

```tsx
const isRowSelectable = (data: any) => data.Status !== 'Cancelled';
```

## Cell Selection

Use cell selection when you want users to act on individual cells or cell ranges.

```tsx
const selectionSettings = {
  mode: 'Cell',
  type: 'Multiple'
};
```

### Cell selection mode

```tsx
const selectionSettings = {
  mode: 'Cell',
  type: 'Multiple',
  cellSelectionMode: 'Box'
};
```

Supported values are `Flow`, `Box`, and `BoxWithBorder`.

Use arrow keys to move between cells and `Shift + Click` to select a range. Cell selection requires `mode` to be `Cell` or `Both` and `type` to be `Multiple` for range selection.

### Programmatic cell selection

```tsx
const selectSingleCell = () => gridRef.current?.selectCell({ rowIndex: 1, cellIndex: 2 });
const selectCells = () => gridRef.current?.selectCells([{ rowIndex: 1, cellIndexes: [2] }]);
const selectRange = () => gridRef.current?.selectCellsByRange(
  { rowIndex: 1, cellIndex: 0 },
  { rowIndex: 3, cellIndex: 2 }
);
const clearCellSelection = () => gridRef.current?.clearCellSelection();
const selectedCellIndexes = () => gridRef.current?.getSelectedRowCellIndexes();
```

## Column Selection

Use column selection to highlight one or more columns by clicking their headers.

```tsx
const selectionSettings = {
  allowColumnSelection: true,
  type: 'Multiple'
};
```

Click a column header to select it. For multiple columns, use `Ctrl + Click` for non-consecutive columns or `Shift + Click`/`Shift + Arrow` for a range. Press `Esc` to clear the current column selection.

### Programmatic column selection

```tsx
const selectColumn = () => gridRef.current?.selectColumn(1);
const selectColumns = () => gridRef.current?.selectColumns([0, 2]);
const selectColumnsByRange = () => gridRef.current?.selectColumnsByRange(0, 2);
const selectColumnWithExisting = () => gridRef.current?.selectColumnWithExisting(3);
const clearColumnSelection = () => gridRef.current?.clearColumnSelection();
```

## Checkbox Selection

Render a checkbox column to enable row selection with checkboxes.

```tsx
<GridComponent dataSource={data} allowSelection={true} selectionSettings={{ mode: 'Row', type: 'Multiple' }}>
  <ColumnsDirective>
    <ColumnDirective type='checkbox' width='50' />
    <ColumnDirective field='OrderID' headerText='Order ID' width='120' />
    <ColumnDirective field='CustomerID' headerText='Customer ID' width='150' />
  </ColumnsDirective>
</GridComponent>
```

### Checkbox selection modes

Use this when you want checkboxes to behave differently. `Default` is the usual behavior, while `ResetOnRowClick` clears the previous selection when the user clicks another row.

```tsx
const selectionSettings = {
  mode: 'Row',
  type: 'Multiple',
  checkboxMode: 'ResetOnRowClick'
};
```

- `Default`: standard checkbox selection behavior.
- `ResetOnRowClick`: a new row click resets the previous selection.

### Restrict selection to checkbox clicks

Use this when you want the user to select rows only by clicking the checkboxes, not by clicking the row itself. This is helpful for stricter bulk-selection UIs.

```tsx
const selectionSettings = {
  mode: 'Row',
  type: 'Multiple',
  checkboxOnly: true
};
```

### Hide the select-all checkbox

```tsx
const headerTemplate = () => <div />;

<ColumnDirective type='checkbox' width='50' headerTemplate={headerTemplate} />
```

### Single-row checkbox selection

```tsx
const rowSelecting = (args: any) => {
  if (args.target?.classList.contains('e-icons')) {
    gridRef.current?.clearSelection();
  }
};
```

## Persist Selection

Persist selection keeps selected items across paging and refresh operations.

```tsx
const selectionSettings = {
  mode: 'Row',
  type: 'Multiple',
  persistSelection: true
};
```

For persistence, define a primary key with `isPrimaryKey={true}` on at least one column.

```tsx
<ColumnDirective field='OrderID' isPrimaryKey={true} />
```

Persist selection is supported for row and column selection with `type: 'Multiple'`, and it is not supported for cell selection.

## Programmatic Selection

Use refs to select rows, cells, columns, or clear the current selection from code. This is useful when a button click or another event should update selection automatically.

```tsx
import { useRef } from 'react';

function App() {
  const gridRef = useRef<any>(null);

  const selectRows = () => gridRef.current?.selectRows([0, 2, 4]);
  const selectCell = () => gridRef.current?.selectCell({ rowIndex: 1, cellIndex: 2 });
  const selectColumn = () => gridRef.current?.selectColumn(1);
  const clearSelection = () => gridRef.current?.clearSelection();
  const selectAll = () => gridRef.current?.selectAll();

  return (
    <div>
      <button onClick={selectRows}>Select rows</button>
      <button onClick={selectCell}>Select cell</button>
      <button onClick={selectColumn}>Select column</button>
      <button onClick={selectAll}>Select all</button>
      <button onClick={clearSelection}>Clear selection</button>

      <GridComponent ref={gridRef} dataSource={data} allowSelection={true}>
        <ColumnsDirective>
          <ColumnDirective field='OrderID' headerText='Order ID' width='120' />
          <ColumnDirective field='CustomerID' headerText='Customer ID' width='150' />
        </ColumnsDirective>
      </GridComponent>
    </div>
  );
}
```

You can also read current state with `getSelectedRecords()`, `getSelectedRowIndexes()`, `getSelectedRows()`, and `getSelectedRowCellIndexes()`.

## Selection Events

Hook into selection events to validate user actions or respond to changes. Use these events to block selection, show a message, or run custom logic after a selection happens.

```tsx
const rowSelecting = (args: any) => {
  if (args.data.CustomerID === 'VINET') {
    args.cancel = true;
  }
};

const rowSelected = (args: any) => {
  console.log('Row selected', args.data);
};

<GridComponent
  dataSource={data}
  selectionSettings={{ mode: 'Row', type: 'Multiple' }}
  rowSelecting={rowSelecting}
  rowSelected={rowSelected}
  allowSelection={true}
>
  <ColumnsDirective>
    <ColumnDirective field='OrderID' headerText='Order ID' width='120' />
    <ColumnDirective field='CustomerID' headerText='Customer ID' width='150' />
  </ColumnsDirective>
</GridComponent>
```

Useful event pairs include:
- `rowSelecting`, `rowSelected`, `rowDeselecting`, `rowDeselected`
- `cellSelecting`, `cellSelected`, `cellDeselecting`, `cellDeselected`
- `columnSelecting`, `columnSelected`, `columnDeselecting`, `columnDeselected`

## Important Notes

- Set `allowSelection={false}` to disable selection completely.
- Use `isPrimaryKey={true}` when you want persisted selection across paging or refresh.
- `persistSelection` is not supported for cell selection.
- `checkboxOnly` restricts selection to checkbox clicks.
- `enableToggle` allows single-click select and deselect behavior.
- `isRowSelectable` can be used to prevent selection for specific rows.
- `Ctrl + Click` and `Shift + Click` are the standard multi-selection interactions for rows, cells, and columns.

