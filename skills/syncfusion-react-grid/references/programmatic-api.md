# Programmatic Control

## Table of Contents
- [gridRef.current — Complete Method Catalog](#gridrefcurrent--complete-method-catalog)
- [Data Methods](#data-methods)
- [Row Methods](#row-methods)
- [Column Methods](#column-methods)
- [Sort Methods](#sort-methods)
- [Filter Methods](#filter-methods)
- [Search Methods](#search-methods)
- [Group Methods](#group-methods)
- [Page Methods](#page-methods)
- [Edit Methods](#edit-methods)
- [Export Methods](#export-methods)
- [Print Method](#print-method)
- [Toolbar Methods](#toolbar-methods)
- [Hierarchy / Detail Row Methods](#hierarchy--detail-row-methods)
- [State Methods](#state-methods)
- [Related References](#related-references)

---
## Mandatory Rules for Inbuilt API
**CRITICAL**: Follow these rules when using Grid's

### Rule 1: Grid Ref is MANDATORY for Method Calls
```tsx
// ❌ WRONG - No ref
<GridComponent dataSource={data} />

// ✅ CORRECT - Use ref for method access
const gridRef = useRef(null);
<GridComponent ref={gridRef} dataSource={data} />
// Access methods: gridRef.current.addRecord(data)
```

### Rule 2: Primary Key Required for Data Manipulation Methods
Methods like `setCellValue()`, `setRowData()`, `deleteRecord()` require `isPrimaryKey={true}` on ID column.

```tsx
// ❌ WRONG - Methods fail silently
<ColumnDirective field='OrderID' />

// ✅ CORRECT
<ColumnDirective field='OrderID' isPrimaryKey={true} />

```

### Rule 3: Async Methods Must Be Awaited
Export and some data methods return Promises.

```tsx
// ❌ WRONG - Export might not complete
const handleExport = () => {
  gridRef.current.excelExport();
};

// ✅ CORRECT - Wait for completion
const handleExport = async () => {
  await gridRef.current.excelExport();
  console.log('Export complete');
};
```

### Rule 4: Use Correct Method Parameters
Check method signatures - wrong parameters = silent failure.

```tsx
// ❌ WRONG - Missing required parameters
gridRef.current.filterByColumn('Freight', 100);

// ✅ CORRECT - All 3 parameters required
gridRef.current.filterByColumn('Freight', 'greaterThan', 100);

// ❌ WRONG - Wrong parameter type
gridRef.current.selectRows(0); // Number instead of array

// ✅ CORRECT
gridRef.current.selectRows([0, 2, 4]); // Array required
```

### Rule 5: Check Return Values from Getter Methods
Don't assume methods return data - always validate.

```tsx
// ❌ WRONG - Might be undefined/empty
const selected = gridRef.current.getSelectedRecords();
console.log(selected[0].OrderID); // Crashes if empty

// ✅ CORRECT - Check before use
const selected = gridRef.current.getSelectedRecords();
if (selected && selected.length > 0) {
  console.log(selected[0].OrderID);
}
```

### Rule 5: Edit Methods Require editSettings Configuration
```tsx
// ❌ WRONG - addRecord() fails silently
<GridComponent dataSource={data} />
gridRef.current.addRecord(newData);

// ✅ CORRECT
<GridComponent 
  dataSource={data}
  editSettings={{ allowAdding: true, allowEditing: true, allowDeleting: true }}
>
  <Inject services={[Edit]} />
</GridComponent>
gridRef.current.addRecord(newData);
```

## gridRef.current — Complete Method Catalog

Always attach a ref to access the grid instance programmatically:

```tsx
const gridRef = useRef(null);
<GridComponent ref={gridRef} ... >
```

## Data Methods
```tsx
gridRef.current.refresh()                             // Reload data from source
gridRef.current.setProperties({ dataSource: newData })// Change data source at runtime
gridRef.current.getCurrentViewRecords()               // Get currently visible rows (after filter/page)
gridRef.current.getDataModule().dataManager           // Access underlying DataManager
```

## Row Methods
```tsx
gridRef.current.getRowByIndex(rowIndex)               // Get DOM row element by index
gridRef.current.getRowInfo(rowElement)                // Get row data from a DOM element
gridRef.current.clearSelection()                      // Deselect all rows
gridRef.current.selectRows([0, 2, 4])                 // Select rows by index array
gridRef.current.getSelectedRows()                     // Get selected DOM row elements
gridRef.current.getSelectedRecords()                  // Get selected data records (objects)
```

## Column Methods
```tsx
gridRef.current.getColumns()                          // Get all column objects
gridRef.current.getVisibleColumns()                   // Get only visible column objects
gridRef.current.getColumnByField('OrderID')           // Get column by field name
gridRef.current.getColumnByUid(uid)                   // Get column by UID
gridRef.current.showColumns('Order ID')               // Show column by headerText
gridRef.current.showColumns(['Order ID', 'Freight'])  // Show multiple columns
gridRef.current.hideColumns('Internal Notes')         // Hide column by headerText
gridRef.current.hideColumns(['Region', 'EmployeeID']) // Hide multiple columns
gridRef.current.autoFitColumns()                      // Auto-fit all columns to content
gridRef.current.autoFitColumns(['OrderID', 'Freight'])// Auto-fit specific columns
```

## Sort Methods
```tsx
gridRef.current.sortColumn('OrderID', 'Ascending')   // Sort a column programmatically
gridRef.current.sortColumn('Freight', 'Descending')  // Descending sort
gridRef.current.clearSorting()                       // Remove all sorts
gridRef.current.sortSettings.columns                 // Read current sort state
```

## Filter Methods
```tsx
gridRef.current.filterByColumn('CustomerID', 'StartsWith', 'VIN') // Filter column
gridRef.current.filterByColumn('Freight', 'GreaterThan', 50)       // Numeric filter
gridRef.current.clearFiltering()                                   // Remove all filters
gridRef.current.clearFiltering(['CustomerID'])                     // Remove one column filter
gridRef.current.filterSettings.columns                             // Read current filter state
```

## Search Methods
```tsx
gridRef.current.search('searchTerm')                 // Perform global search
gridRef.current.search('')                           // Clear search
```

## Group Methods
```tsx
gridRef.current.groupColumn('ShipCountry')           // Group by a column
gridRef.current.ungroupColumn('ShipCountry')         // Remove group
gridRef.current.groupSettings.columns               // Read currently grouped columns
```

## Page Methods
```tsx
gridRef.current.goToPage(3)                          // Navigate to specific page
gridRef.current.pageSettings.currentPage            // Current page number
gridRef.current.pageSettings.pageSize               // Current page size
gridRef.current.pageSettings.totalRecordsCount      // Total record count
```

## Edit Methods
```tsx
gridRef.current.addRecord({ OrderID: 99, CustomerID: 'NEW' }) // Add record programmatically
gridRef.current.startEdit()                          // Begin editing selected row
gridRef.current.endEdit()                            // Confirm / save current edit
gridRef.current.closeEdit()                          // Cancel current edit
gridRef.current.deleteRecord('OrderID', recordObj)  // Delete by key field + record
gridRef.current.updateRow(rowIndex, updatedData)    // Update a specific row
gridRef.current.setCellValue(rowIndex, field, value)// Update a single cell value
```

## Export Methods
```tsx
gridRef.current.excelExport()                        // Export to Excel
gridRef.current.excelExport({ fileName: 'data.xlsx' }) // With options
gridRef.current.pdfExport()                          // Export to PDF
gridRef.current.pdfExport({ pageOrientation: 'Landscape' }) // With options
gridRef.current.csvExport()                          // Export to CSV
```

## Print Method
```tsx
gridRef.current.print()                              // Print the grid
```

## Toolbar Methods
```tsx
gridRef.current.toolbarModule.enableItems(['Add', 'Edit'], true)     // Enable items
gridRef.current.toolbarModule.enableItems(['Delete', 'Update'], false) // Disable items
```

## Hierarchy / Detail Row Methods
```tsx
gridRef.current.detailRowModule.detailsExpand([0, 1, 2])   // Expand detail rows by index
gridRef.current.detailRowModule.detailsCollapse([0, 1, 2]) // Collapse detail rows
```