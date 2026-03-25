# Grid Component - Properties, Methods, and Events Reference

## Table of Contents
- [Properties Overview](#properties-overview)
- [Core Data Properties](#core-data-properties)
- [Layout & Display Properties](#layout--display-properties)
- [Feature Enablement Properties](#feature-enablement-properties)
- [Selection & Editing Properties](#selection--editing-properties)
- [Methods Overview](#methods-overview)
- [Data Manipulation Methods](#data-manipulation-methods)
- [Selection Methods](#selection-methods)
- [Column Management Methods](#column-management-methods)
- [Navigation & Paging Methods](#navigation--paging-methods)
- [Events Overview](#events-overview)
- [Data Events](#data-events)
- [Interaction Events](#interaction-events)
- [Practical Examples](#practical-examples)

---

## Properties Overview

The Grid component has 95+ configurable properties. Here are the most commonly used ones organized by category.

### Quick Reference Table

| Category | Key Properties |
|----------|----------------|
| **Data Binding** | `dataSource`, `columns`, `query` |
| **Paging** | `allowPaging`, `pageSettings` |
| **Sorting** | `allowSorting`, `allowMultiSorting`, `sortSettings` |
| **Filtering** | `allowFiltering`, `filterSettings` |
| **Selection** | `allowSelection`, `selectionSettings` |
| **Editing** | `editSettings` |
| **Layout** | `height`, `width`, `gridLines`, `rowHeight` |
| **Export** | `allowExcelExport`, `allowPdfExport` |
| **Grouping** | `allowGrouping`, `groupSettings` |

---

## Core Data Properties

### dataSource
**Type:** `Object | DataManager | DataResult`  
**Default:** `[]`  
**Description:** Primary data source for rendering grid rows

```tsx
import { GridComponent, ColumnsDirective, ColumnDirective } from '@syncfusion/ej2-react-grids';

const data = [
  { OrderID: 10001, CustomerID: 'ALFKI', Freight: 32.38 },
  { OrderID: 10002, CustomerID: 'ANATR', Freight: 11.61 },
];

<GridComponent dataSource={data}>
  <ColumnsDirective>
    <ColumnDirective field='OrderID' headerText='Order ID' width='100' />
    <ColumnDirective field='CustomerID' headerText='Customer' width='100' />
    <ColumnDirective field='Freight' headerText='Freight' width='100' format='C2' />
  </ColumnsDirective>
</GridComponent>
```

### columns
**Type:** `Column[] | string[] | ColumnModel[]`  
**Default:** `[]`  
**Description:** Schema definition for grid columns. Auto-generated from dataSource if empty

```tsx
<GridComponent dataSource={data}>
  <ColumnsDirective>
    <ColumnDirective field='OrderID' headerText='Order ID' width='100' />
    <ColumnDirective field='CustomerID' headerText='Customer Name' width='150' />
    <ColumnDirective field='ShipCity' headerText='Ship City' width='150' />
  </ColumnsDirective>
</GridComponent>
```

### query
**Type:** `Query`  
**Default:** `null`  
**Description:** External Query to be executed along with data processing

```tsx
import { GridComponent, Inject, Page } from '@syncfusion/ej2-react-grids';
import { Query, DataManager, UrlAdaptor } from '@syncfusion/ej2-data';

const query = new Query()
  .select(['OrderID', 'CustomerID', 'Freight'])
  .where('Freight', 'greaterThan', 100);

<GridComponent 
  dataSource={new DataManager({ url: 'api/orders', adaptor: new UrlAdaptor() })}
  query={query}
>
  {/* columns */}
</GridComponent>
```

---

## Layout & Display Properties

### height
**Type:** `string | number`  
**Default:** `'auto'`  
**Description:** Scrollable height of grid content

```tsx
<GridComponent dataSource={data} height={400}>
  {/* columns */}
</GridComponent>

// Or with string value
<GridComponent dataSource={data} height='500px'>
  {/* columns */}
</GridComponent>
```

### width
**Type:** `string | number`  
**Default:** `'auto'`  
**Description:** Grid width

```tsx
<GridComponent dataSource={data} width='100%'>
  {/* columns */}
</GridComponent>
```

### rowHeight
**Type:** `number`  
**Default:** `null` (auto-calculated)  
**Description:** Height for all grid rows

```tsx
<GridComponent dataSource={data} rowHeight={40}>
  {/* columns */}
</GridComponent>
```

### gridLines
**Type:** `GridLine` (Both | None | Horizontal | Vertical | Default)  
**Default:** `Default`  
**Description:** Grid line display mode

```tsx
<GridComponent dataSource={data} gridLines='Both'>
  {/* columns */}
</GridComponent>
```

### allowTextWrap
**Type:** `boolean`  
**Default:** `false`  
**Description:** Enables text wrapping in cells

```tsx
<GridComponent dataSource={data} allowTextWrap={true}>
  {/* columns */}
</GridComponent>
```

### enableAltRow
**Type:** `boolean`  
**Default:** `true`  
**Description:** Renders alternating row styling

```tsx
<GridComponent dataSource={data} enableAltRow={true}>
  {/* columns */}
</GridComponent>
```

### enableHover
**Type:** `boolean`  
**Default:** `true`  
**Description:** Enables row hover effect

```tsx
<GridComponent dataSource={data} enableHover={true}>
  {/* columns */}
</GridComponent>
```

### enableStickyHeader
**Type:** `boolean`  
**Default:** `false`  
**Description:** Makes column headers visible while scrolling

```tsx
<GridComponent dataSource={data} enableStickyHeader={true}>
  {/* columns */}
</GridComponent>
```

---

## Feature Enablement Properties

### allowPaging
**Type:** `boolean`  
**Default:** `false`  
**Description:** Enables pager rendering at grid footer for page navigation

```tsx
import { GridComponent, Inject, Page } from '@syncfusion/ej2-react-grids';

<GridComponent dataSource={data} allowPaging={true}>
  {/* columns */}
  <Inject services={[Page]} />
</GridComponent>
```

### pageSettings
**Type:** `PageSettingsModel`  
**Default:** `{currentPage: 1, pageSize: 12, pageCount: 8, ...}`  
**Description:** Configures pager behavior

```tsx
<GridComponent 
  dataSource={data} 
  allowPaging={true}
  pageSettings={{ pageSize: 20, currentPage: 1 }}
>
  {/* columns */}
</GridComponent>
```

### allowSorting
**Type:** `boolean`  
**Default:** `false`  
**Description:** Enables sorting when clicking column headers

```tsx
import { GridComponent, Inject, Sort } from '@syncfusion/ej2-react-grids';

<GridComponent dataSource={data} allowSorting={true}>
  {/* columns */}
  <Inject services={[Sort]} />
</GridComponent>
```

### allowMultiSorting
**Type:** `boolean`  
**Default:** `false`  
**Description:** Allows sorting multiple columns

```tsx
<GridComponent 
  dataSource={data} 
  allowSorting={true}
  allowMultiSorting={true}
>
  {/* columns */}
</GridComponent>
```

### allowFiltering
**Type:** `boolean`  
**Default:** `false`  
**Description:** Displays filter bar for record filtering

```tsx
import { GridComponent, Inject, Filter } from '@syncfusion/ej2-react-grids';

<GridComponent dataSource={data} allowFiltering={true}>
  {/* columns */}
  <Inject services={[Filter]} />
</GridComponent>
```

### allowSelection
**Type:** `boolean`  
**Default:** `true`  
**Description:** Enables row/cell selection with highlighting

```tsx
<GridComponent dataSource={data} allowSelection={true}>
  {/* columns */}
</GridComponent>
```

### allowGrouping
**Type:** `boolean`  
**Default:** `false`  
**Description:** Enables dynamic column grouping via drag-drop

```tsx
import { GridComponent, Inject, Group } from '@syncfusion/ej2-react-grids';

<GridComponent dataSource={data} allowGrouping={true}>
  {/* columns */}
  <Inject services={[Group]} />
</GridComponent>
```

### allowExcelExport
**Type:** `boolean`  
**Default:** `false`  
**Description:** Enables Excel export functionality

```tsx
import { GridComponent, Inject, ExcelExport } from '@syncfusion/ej2-react-grids';

<GridComponent dataSource={data} allowExcelExport={true}>
  {/* columns */}
  <Inject services={[ExcelExport]} />
</GridComponent>
```

### allowPdfExport
**Type:** `boolean`  
**Default:** `false`  
**Description:** Enables PDF export functionality

```tsx
import { GridComponent, Inject, PdfExport } from '@syncfusion/ej2-react-grids';

<GridComponent dataSource={data} allowPdfExport={true}>
  {/* columns */}
  <Inject services={[PdfExport]} />
</GridComponent>
```

---

## Selection & Editing Properties

### selectionSettings
**Type:** `SelectionSettingsModel`  
**Description:** Configures selection mode, cellSelectionMode, and type

```tsx
<GridComponent 
  dataSource={data} 
  selectionSettings={{
    mode: 'Row',           // Row, Cell, or Column
    type: 'Multiple',      // Single or Multiple
    cellSelectionMode: 'Flow' // Flow or Box
  }}
>
  {/* columns */}
</GridComponent>
```

### editSettings
**Type:** `EditSettingsModel`  
**Description:** Configures editing behavior, modes, and dialogs

```tsx
import { GridComponent, Inject, Edit } from '@syncfusion/ej2-react-grids';

<GridComponent 
  dataSource={data} 
  editSettings={{
    allowAdding: true,
    allowEditing: true,
    allowDeleting: true,
    mode: 'Dialog'  // Dialog, Batch, InlineAdd, InlineEdit, etc.
  }}
>
  {/* columns */}
  <Inject services={[Edit]} />
</GridComponent>
```

---

## Methods Overview

The Grid component provides 137+ methods organized by functionality. Here are the most commonly used ones.

---

## Data Manipulation Methods

### addRecord()
**Parameters:** `data?: Object, index?: number`  
**Return:** `void`  
**Description:** Adds new record(s) to grid (requires `editSettings.allowEditing: true`)

```tsx
import React, { useRef } from 'react';
import { GridComponent, Inject, Edit } from '@syncfusion/ej2-react-grids';

function GridWithAddRecord() {
  const gridRef = useRef(null);

  const handleAddRecord = () => {
    const newRecord = {
      OrderID: 10003,
      CustomerID: 'BLONP',
      Freight: 25.00
    };
    gridRef.current.addRecord(newRecord);
  };

  return (
    <div>
      <button onClick={handleAddRecord}>Add Record</button>
      
      <GridComponent 
        ref={gridRef} 
        dataSource={data}
        editSettings={{ allowAdding: true, allowEditing: true }}
      >
        {/* columns */}
        <Inject services={[Edit]} />
      </GridComponent>
    </div>
  );
}
```

### setCellValue()
**Parameters:** `key: string | number, field: string, value: string | number | boolean | Date | null`  
**Return:** `void`  
**Description:** Updates single cell value by primary key

```tsx
const handleUpdateCell = () => {
  gridRef.current.setCellValue(10001, 'Freight', 50.00);
};
```

### setRowData()
**Parameters:** `key: string | number, rowData?: Object`  
**Return:** `void`  
**Description:** Updates entire row by primary key

```tsx
const handleUpdateRow = () => {
  const updatedData = {
    OrderID: 10001,
    CustomerID: 'UPDATED',
    Freight: 100.00
  };
  gridRef.current.setRowData(10001, updatedData);
};
```

### deleteRecord()
**Parameters:** `fieldname?: string, data?: Object`  
**Return:** `void`  
**Description:** Deletes record (requires `editSettings.allowDeleting: true`)

```tsx
const handleDeleteRecord = () => {
  gridRef.current.deleteRecord('OrderID', { OrderID: 10001 });
};
```

### getBatchChanges()
**Return:** `Object`  
**Description:** Gets added, edited, deleted data before bulk save

```tsx
const getBatchUpdates = () => {
  const changes = gridRef.current.getBatchChanges();
  console.log('Added:', changes.addedRecords);
  console.log('Modified:', changes.changedRecords);
  console.log('Deleted:', changes.deletedRecords);
};
```

### changeDataSource()
**Parameters:** `dataSource?: Object | DataManager | DataResult, columns?: Column[]`  
**Return:** `void`  
**Description:** Changes datasource and columns completely

```tsx
const newData = [
  { OrderID: 20001, CustomerID: 'NEWCUST', Freight: 50 }
];

gridRef.current.changeDataSource(newData);
```

---

## Selection Methods

### selectRow()
**Parameters:** `index: number, isToggle?: boolean`  
**Return:** `void`  
**Description:** Selects row by index

```tsx
const handleSelectRow = () => {
  gridRef.current.selectRow(0);  // Select first row
};
```

### selectRows()
**Parameters:** `rowIndexes: number[]`  
**Return:** `void`  
**Description:** Selects multiple rows by indexes

```tsx
const handleSelectMultipleRows = () => {
  gridRef.current.selectRows([0, 2, 4]);  // Select rows at index 0, 2, 4
};
```

### getSelectedRowIndexes()
**Return:** `number[]`  
**Description:** Gets selected row indexes

```tsx
const getSelectedIndexes = () => {
  const indexes = gridRef.current.getSelectedRowIndexes();
  console.log('Selected rows:', indexes);
};
```

### getSelectedRecords()
**Return:** `Object[]`  
**Description:** Gets selected row data

```tsx
const getSelectedData = () => {
  const selectedRecords = gridRef.current.getSelectedRecords();
  console.log('Selected data:', selectedRecords);
};
```

### clearRowSelection()
**Return:** `void`  
**Description:** Deselects all rows

```tsx
const handleClearSelection = () => {
  gridRef.current.clearRowSelection();
};
```

### selectCell()
**Parameters:** `cellIndex: IIndex, isToggle?: boolean`  
**Return:** `void`  
**Description:** Selects cell by row/column index

```tsx
const handleSelectCell = () => {
  gridRef.current.selectCell({ rowIndex: 0, cellIndex: 1 });
};
```

---

## Column Management Methods

### getColumns()
**Parameters:** `isRefresh?: boolean`  
**Return:** `Column[]`  
**Description:** Gets column definitions

```tsx
const getGridColumns = () => {
  const columns = gridRef.current.getColumns();
  console.log('Grid columns:', columns);
};
```

### getColumnByField()
**Parameters:** `field: string`  
**Return:** `Column`  
**Description:** Gets column by field name

```tsx
const getColumn = () => {
  const column = gridRef.current.getColumnByField('CustomerID');
  console.log('Column:', column);
};
```

### getColumnFieldNames()
**Return:** `string[]`  
**Description:** Gets all column field names

```tsx
const getFieldNames = () => {
  const fields = gridRef.current.getColumnFieldNames();
  console.log('Field names:', fields);
};
```

### hideColumns()
**Parameters:** `keys: string | string[], hideBy?: string`  
**Return:** `void`  
**Description:** Hides columns by name

```tsx
const handleHideColumns = () => {
  gridRef.current.hideColumns('Freight');  // Hide single column
  // Or multiple
  gridRef.current.hideColumns(['Freight', 'ShipCity']);
};
```

### showColumns()
**Parameters:** `keys: string | string[], showBy?: string`  
**Return:** `void`  
**Description:** Shows columns by name

```tsx
const handleShowColumns = () => {
  gridRef.current.showColumns('Freight');
};
```

### autoFitColumns()
**Parameters:** `fieldNames?: string | string[]`  
**Return:** `void`  
**Description:** Auto-fits columns to content

```tsx
const handleAutoFit = () => {
  gridRef.current.autoFitColumns();  // Auto-fit all columns
};
```

### reorderColumns()
**Parameters:** `fromFName: string | string[], toFName: string`  
**Return:** `void`  
**Description:** Reorders columns by field names

```tsx
const handleReorderColumns = () => {
  gridRef.current.reorderColumns('OrderID', 'Freight');
};
```

---

## Navigation & Paging Methods

### goToPage()
**Parameters:** `pageNo: number`  
**Return:** `void`  
**Description:** Navigates to specified page

```tsx
const handleGoToPage = () => {
  gridRef.current.goToPage(3);  // Go to page 3
};
```

### search()
**Parameters:** `searchString: string`  
**Return:** `void`  
**Description:** Searches records with key

```tsx
const handleSearch = (searchValue) => {
  gridRef.current.search(searchValue);
};
```

### sortColumn()
**Parameters:** `columnName: string, direction: SortDirection, isMultiSort?: boolean`  
**Return:** `void`  
**Description:** Sorts column with direction (Ascending or Descending)

```tsx
const handleSortColumn = () => {
  gridRef.current.sortColumn('Freight', 'Ascending');
};
```

### clearSorting()
**Return:** `void`  
**Description:** Clears all sorting

```tsx
const handleClearSorting = () => {
  gridRef.current.clearSorting();
};
```

---

## Filtering Methods

### filterByColumn()
**Parameters:** `fieldName: string, filterOperator: string, filterValue: string | number | Date | boolean`  
**Return:** `void`  
**Description:** Filters by column with operators

```tsx
const handleFilterByColumn = () => {
  // Filter Freight > 100
  gridRef.current.filterByColumn('Freight', 'greaterThan', 100);
};
```

### clearFiltering()
**Parameters:** `fields?: string[]`  
**Return:** `void`  
**Description:** Clears all filters or specific fields

```tsx
const handleClearFilters = () => {
  gridRef.current.clearFiltering();
};
```

### getFilteredRecords()
**Return:** `Object[] | Promise`  
**Description:** Gets all filtered records

```tsx
const getFiltered = () => {
  const filtered = gridRef.current.getFilteredRecords();
  console.log('Filtered records:', filtered);
};
```

---

## Export Methods

### excelExport()
**Parameters:** `excelExportProperties?: ExcelExportProperties`  
**Return:** `Promise`  
**Description:** Exports grid to Excel file (.xlsx)

```tsx
const handleExcelExport = async () => {
  await gridRef.current.excelExport();
};
```

### csvExport()
**Parameters:** `excelExportProperties?: ExcelExportProperties`  
**Return:** `Promise`  
**Description:** Exports grid to CSV file

```tsx
const handleCsvExport = async () => {
  await gridRef.current.csvExport();
};
```

### pdfExport()
**Parameters:** `pdfExportProperties?: PdfExportProperties`  
**Return:** `Promise`  
**Description:** Exports grid to PDF document

```tsx
const handlePdfExport = async () => {
  await gridRef.current.pdfExport();
};
```

### print()
**Return:** `void`  
**Description:** Prints all pages

```tsx
const handlePrint = () => {
  gridRef.current.print();
};
```

---

## Events Overview

The Grid component provides 65+ events. Here are the most commonly used ones.

---

## Data Events

### beforeDataBound
**Arguments:** `BeforeDataBoundArgs`  
**Description:** Triggers before data is bound to grid

```tsx
const onBeforeDataBound = (args) => {
  console.log('Before data binding:', args);
};

<GridComponent dataSource={data} beforeDataBound={onBeforeDataBound}>
  {/* columns */}
</GridComponent>
```

### dataBound
**Arguments:** `Object`  
**Description:** Triggers when data source is populated

```tsx
const onDataBound = (args) => {
  console.log('Data bound completed');
};

<GridComponent dataSource={data} dataBound={onDataBound}>
  {/* columns */}
</GridComponent>
```

### dataSourceChanged
**Arguments:** `DataSourceChangedEventArgs`  
**Description:** Triggers when grid data is added/deleted/updated

```tsx
const onDataSourceChanged = (args) => {
  console.log('Data source changed:', args.action);
  console.log('Data:', args.data);
};

<GridComponent dataSource={data} dataSourceChanged={onDataSourceChanged}>
  {/* columns */}
</GridComponent>
```

---

## Interaction Events

### rowSelecting
**Arguments:** `RowSelectingEventArgs`  
**Description:** Triggers before row selection

```tsx
const onRowSelecting = (args) => {
  console.log('Row index:', args.rowIndex);
  if (args.rowIndex === 0) {
    args.cancel = true;  // Prevent selection
  }
};

<GridComponent dataSource={data} rowSelecting={onRowSelecting}>
  {/* columns */}
</GridComponent>
```

### rowSelected
**Arguments:** `RowSelectedEventArgs`  
**Description:** Triggers after row selection

```tsx
const onRowSelected = (args) => {
  console.log('Selected row data:', args.data);
};

<GridComponent dataSource={data} rowSelected={onRowSelected}>
  {/* columns */}
</GridComponent>
```

### rowDeselecting
**Arguments:** `RowDeselectingEventArgs`  
**Description:** Triggers before row deselection

```tsx
const onRowDeselecting = (args) => {
  console.log('Deselecting row:', args.rowIndex);
};

<GridComponent dataSource={data} rowDeselecting={onRowDeselecting}>
  {/* columns */}
</GridComponent>
```

### rowDeselected
**Arguments:** `RowDeselectedEventArgs`  
**Description:** Triggers after row deselection

```tsx
const onRowDeselected = (args) => {
  console.log('Deselected row data:', args.data);
};

<GridComponent dataSource={data} rowDeselected={onRowDeselected}>
  {/* columns */}
</GridComponent>
```

### cellSelecting
**Arguments:** `CellSelectingEventArgs`  
**Description:** Triggers before cell selection

```tsx
const onCellSelecting = (args) => {
  console.log('Selecting cell at:', args.cellIndex);
};

<GridComponent dataSource={data} cellSelecting={onCellSelecting}>
  {/* columns */}
</GridComponent>
```

### cellSelected
**Arguments:** `CellSelectedEventArgs`  
**Description:** Triggers after cell selection

```tsx
const onCellSelected = (args) => {
  console.log('Selected cell value:', args.value);
};

<GridComponent dataSource={data} cellSelected={onCellSelected}>
  {/* columns */}
</GridComponent>
```

---

## Editing Events

### actionBegin
**Arguments:** `ActionEventArgs`  
**Description:** Triggers before grid action begins (add, edit, delete, sort, filter, etc.)

```tsx
const onActionBegin = (args) => {
  console.log('Action type:', args.requestType);
  // requestType can be: 'save', 'cancel', 'delete', 'add', etc.
};

<GridComponent dataSource={data} actionBegin={onActionBegin}>
  {/* columns */}
</GridComponent>
```

### actionComplete
**Arguments:** `ActionEventArgs`  
**Description:** Triggers after grid action completes

```tsx
const onActionComplete = (args) => {
  if (args.requestType === 'save') {
    console.log('Save completed');
    console.log('New data:', args.data);
  }
};

<GridComponent dataSource={data} actionComplete={onActionComplete}>
  {/* columns */}
</GridComponent>
```

### actionFailure
**Arguments:** `FailureEventArgs`  
**Description:** Triggers when grid action fails

```tsx
const onActionFailure = (args) => {
  console.error('Action failed:', args.error);
};

<GridComponent dataSource={data} actionFailure={onActionFailure}>
  {/* columns */}
</GridComponent>
```

### beforeEdit
**Arguments:** `BeforeEditEventArgs`  
**Description:** Triggers before entering edit mode

```tsx
const onBeforeEdit = (args) => {
  console.log('Edit starting for:', args.data);
  // Can prevent edit by setting args.cancel = true
};

<GridComponent dataSource={data} beforeEdit={onBeforeEdit}>
  {/* columns */}
</GridComponent>
```

### beginEdit
**Arguments:** `BeginEditEventArgs`  
**Description:** Triggers after entering edit mode

```tsx
const onBeginEdit = (args) => {
  console.log('Now in edit mode for:', args.data);
};

<GridComponent dataSource={data} beginEdit={onBeginEdit}>
  {/* columns */}
</GridComponent>
```

### endEdit
**Arguments:** `EndEditEventArgs`  
**Description:** Triggers after edit completes

```tsx
const onEndEdit = (args) => {
  console.log('Edit completed');
  console.log('Data:', args.data);
};

<GridComponent dataSource={data} endEdit={onEndEdit}>
  {/* columns */}
</GridComponent>
```

---

## Query Cell Info Event

### queryCellInfo
**Arguments:** `QueryCellInfoEventArgs`  
**Description:** Triggers for each cell rendering

```tsx
const onQueryCellInfo = (args) => {
  // Highlight cells based on value
  if (args.cell.index === 0 && args.data.Freight > 100) {
    args.cell.classList.add('high-freight');
  }
};

<GridComponent dataSource={data} queryCellInfo={onQueryCellInfo}>
  {/* columns */}
</GridComponent>
```

### queryRowInfo
**Arguments:** `QueryRowInfoEventArgs`  
**Description:** Triggers for each row rendering

```tsx
const onQueryRowInfo = (args) => {
  // Highlight entire row based on condition
  if (args.data.Freight > 100) {
    args.row.classList.add('high-value-row');
  }
};

<GridComponent dataSource={data} queryRowInfo={onQueryRowInfo}>
  {/* columns */}
</GridComponent>
```

---

## Column Events

### columnSelecting
**Arguments:** `ColumnSelectingEventArgs`  
**Description:** Triggers before column selection

```tsx
const onColumnSelecting = (args) => {
  console.log('Selecting column:', args.column.field);
};

<GridComponent dataSource={data} columnSelecting={onColumnSelecting}>
  {/* columns */}
</GridComponent>
```

### columnSelected
**Arguments:** `ColumnSelectedEventArgs`  
**Description:** Triggers after column selection

```tsx
const onColumnSelected = (args) => {
  console.log('Selected column:', args.column.field);
};

<GridComponent dataSource={data} columnSelected={onColumnSelected}>
  {/* columns */}
</GridComponent>
```

### columnWidthChange
**Arguments:** `ColumnWidthChangeEventArgs`  
**Description:** Triggers when column width changes

```tsx
const onColumnWidthChange = (args) => {
  console.log('Column width changed:', args.column.field, args.width);
};

<GridComponent dataSource={data} columnWidthChange={onColumnWidthChange}>
  {/* columns */}
</GridComponent>
```

---

## Practical Examples

### Complete CRUD Example

```tsx
import React, { useRef, useState } from 'react';
import {
  GridComponent,
  ColumnsDirective,
  ColumnDirective,
  Inject,
  Page,
  Sort,
  Filter,
  Edit,
  Toolbar
} from '@syncfusion/ej2-react-grids';

function GridCRUDExample() {
  const gridRef = useRef(null);
  const [data, setData] = useState([
    { OrderID: 10001, CustomerID: 'ALFKI', Freight: 32.38, ShipCity: 'Berlin' },
    { OrderID: 10002, CustomerID: 'ANATR', Freight: 11.61, ShipCity: 'Madrid' },
    { OrderID: 10003, CustomerID: 'ANTON', Freight: 65.10, ShipCity: 'Marseille' },
  ]);

  const handleAddRecord = () => {
    const newRecord = {
      OrderID: Math.max(...data.map(d => d.OrderID)) + 1,
      CustomerID: 'NEWCUST',
      Freight: 50.00,
      ShipCity: 'Paris'
    };
    gridRef.current.addRecord(newRecord);
  };

  const handleUpdateSelected = () => {
    const selected = gridRef.current.getSelectedRecords();
    if (selected.length > 0) {
      gridRef.current.setCellValue(selected[0].OrderID, 'Freight', 99.99);
    }
  };

  const handleDeleteSelected = () => {
    const selected = gridRef.current.getSelectedRecords();
    if (selected.length > 0) {
      gridRef.current.deleteRecord('OrderID', selected[0]);
    }
  };

  return (
    <div>
      <div style={{ marginBottom: '10px' }}>
        <button onClick={handleAddRecord}>Add New Record</button>
        <button onClick={handleUpdateSelected}>Update Selected</button>
        <button onClick={handleDeleteSelected}>Delete Selected</button>
      </div>

      <GridComponent
        ref={gridRef}
        dataSource={data}
        allowPaging={true}
        allowSorting={true}
        allowFiltering={true}
        editSettings={{
          allowAdding: true,
          allowEditing: true,
          allowDeleting: true,
          mode: 'Dialog'
        }}
        selectionSettings={{
          type: 'Single',
          mode: 'Row'
        }}
      >
        <ColumnsDirective>
          <ColumnDirective field='OrderID' headerText='Order ID' width='100' isPrimaryKey={true} />
          <ColumnDirective field='CustomerID' headerText='Customer' width='120' />
          <ColumnDirective field='Freight' headerText='Freight' width='100' format='C2' />
          <ColumnDirective field='ShipCity' headerText='Ship City' width='120' />
        </ColumnsDirective>
        <Inject services={[Page, Sort, Filter, Edit, Toolbar]} />
      </GridComponent>
    </div>
  );
}

export default GridCRUDExample;
```

### Grid with Custom Events

```tsx
function GridWithEvents() {
  const gridRef = useRef(null);

  const onActionBegin = (args) => {
    console.log(`Action started: ${args.requestType}`);
  };

  const onActionComplete = (args) => {
    console.log(`Action completed: ${args.requestType}`);
    if (args.requestType === 'save') {
      console.log('Data saved:', args.data);
    }
  };

  const onRowSelected = (args) => {
    console.log('Row selected:', args.data);
  };

  const onQueryCellInfo = (args) => {
    if (args.column.field === 'Freight' && args.data.Freight > 50) {
      args.cell.style.backgroundColor = '#ffcccc';
    }
  };

  return (
    <GridComponent
      ref={gridRef}
      dataSource={data}
      allowPaging={true}
      editSettings={{ allowEditing: true, allowAdding: true, allowDeleting: true }}
      actionBegin={onActionBegin}
      actionComplete={onActionComplete}
      rowSelected={onRowSelected}
      queryCellInfo={onQueryCellInfo}
    >
      <ColumnsDirective>
        <ColumnDirective field='OrderID' headerText='Order ID' width='100' />
        <ColumnDirective field='CustomerID' headerText='Customer' width='120' />
        <ColumnDirective field='Freight' headerText='Freight' width='100' />
      </ColumnsDirective>
      <Inject services={[Page, Edit]} />
    </GridComponent>
  );
}

export default GridWithEvents;
```

### Dynamic Column Management

```tsx
function GridWithDynamicColumns() {
  const gridRef = useRef(null);

  const handleHideFreight = () => {
    gridRef.current.hideColumns('Freight');
  };

  const handleShowFreight = () => {
    gridRef.current.showColumns('Freight');
  };

  const handleAutoFit = () => {
    gridRef.current.autoFitColumns();
  };

  const handleReorder = () => {
    gridRef.current.reorderColumns('OrderID', 'Freight');
  };

  return (
    <div>
      <div style={{ marginBottom: '10px' }}>
        <button onClick={handleHideFreight}>Hide Freight</button>
        <button onClick={handleShowFreight}>Show Freight</button>
        <button onClick={handleAutoFit}>Auto Fit</button>
        <button onClick={handleReorder}>Reorder Columns</button>
      </div>

      <GridComponent ref={gridRef} dataSource={data}>
        <ColumnsDirective>
          <ColumnDirective field='OrderID' headerText='Order ID' width='100' />
          <ColumnDirective field='CustomerID' headerText='Customer' width='120' />
          <ColumnDirective field='Freight' headerText='Freight' width='100' />
          <ColumnDirective field='ShipCity' headerText='City' width='120' />
        </ColumnsDirective>
      </GridComponent>
    </div>
  );
}

export default GridWithDynamicColumns;
```

---

## Common Operator Values for Filtering

| Operator | Description | Example |
|----------|-------------|---------|
| `equal` | Equals | `'equal'` |
| `notequal` | Not equals | `'notequal'` |
| `contains` | Contains | `'contains'` |
| `startswith` | Starts with | `'startswith'` |
| `endswith` | Ends with | `'endswith'` |
| `greaterthan` | Greater than | `'greaterthan'` |
| `lessthan` | Less than | `'lessthan'` |
| `greaterthanorequal` | Greater than or equal | `'greaterthanorequal'` |
| `lessthanorequal` | Less than or equal | `'lessthanorequal'` |

---

## Sort Direction Values

| Value | Description |
|-------|-------------|
| `Ascending` | Ascending sort order |
| `Descending` | Descending sort order |

---

## Common Patterns

### Check if Grid has data
```tsx
const hasData = gridRef.current.currentViewData?.length > 0;
```

### Get total records count
```tsx
const totalRecords = gridRef.current.pageSettings?.totalRecordsCount || data.length;
```

### Refresh grid after external data change
```tsx
gridRef.current.refresh();
```

### Get current page records
```tsx
const currentPageRecords = gridRef.current.getCurrentViewRecords();
```

