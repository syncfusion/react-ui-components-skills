# Syncfusion React Grid — Events Catalog

## Table of Contents
- [actionBegin vs actionComplete — Decision Rule](#actionbegin-vs-actioncomplete--decision-rule)
- [requestType Full Catalog](#requesttype-full-catalog)
  - [CRUD / Edit](#crud--edit)
  - [Navigation / Data](#navigation--data)
  - [Export / Print](#export--print)
  - [Scroll](#scroll)
- [args.cancel = true — Interception Patterns](#argscancel--true--interception-patterns)
  - [Cancellable Events](#cancellable-events)
- [All Event Props Catalog](#all-event-props-catalog)
  - [Lifecycle](#lifecycle)
  - [Universal Action](#universal-action)
  - [Row](#row)
  - [Cell](#cell)
  - [Edit (Granular)](#edit-granular)
  - [Sort / Filter / Group / Search / Page](#sort--filter--group--search--page)
  - [Column](#column)
  - [Export / Print](#export--print-1)
  - [Hierarchy / Detail Row](#hierarchy--detail-row)
  - [Context Menu](#context-menu)
  - [Toolbar](#toolbar)
  - [Scroll](#scroll-1)
- [Related References](#related-references)

---

## Mandatory Rules for Inbuilt API

**CRITICAL**: Follow these rules when using Grid's

### Rule 1: Event Handlers Must Match Exact Signature
Wrong event signatures cause silent failures or crashes.

```tsx
// ❌ WRONG - Missing args parameter
<GridComponent actionComplete={() => console.log('done')} />

// ✅ CORRECT - Accept args parameter
<GridComponent actionComplete={(args) => {
  console.log('Request type:', args.requestType);
}} />
```

### Rule 2: Use actionBegin for Cancellation, actionComplete for Reactions
```tsx
<GridComponent
  actionBegin={(args) => {
    // Cancel action before it executes
    if (args.requestType === 'save' && !isValid) {
      args.cancel = true;
    }
  }}
  actionComplete={(args) => {
    // React after action completes
    if (args.requestType === 'save') {
      toast.success('Saved!');
    }
  }}
  actionFailure={(args) => {
    // MANDATORY - Handle errors
    console.error(args.error);
  }}
/>
```

### Rule 3: Event Timing Matters
Know when events fire relative to action execution:

| Event | Timing | Can Cancel? | Use For |
|-------|--------|-------------|---------|
| `actionBegin` | Before action starts | ✅ Yes | Validation, cancellation |
| `actionComplete` | After action finishes | ❌ No | Success handling, API calls |
| `actionFailure` | On error | ❌ No | Error handling |
| `rowSelecting` | Before selection | ✅ Yes | Prevent selection |
| `rowSelected` | After selection | ❌ No | React to selection |

### Rule 4: Don't Call Methods Inside Render Events
```tsx
// ❌ FORBIDDEN - Causes infinite loops
<GridComponent
  queryCellInfo={(args) => {
    gridRef.current.refresh(); // NEVER DO THIS
  }}
  rowDataBound={(args) => {
    gridRef.current.selectRow(0); // NEVER DO THIS
  }}
/>

// ✅ CORRECT - Use proper events
<GridComponent
  actionComplete={(args) => {
    if (args.requestType === 'refresh') {
      // Safe to call methods here
    }
  }}
/>
```


## actionBegin vs actionComplete — Decision Rule

| Use `actionBegin` when you need to | Use `actionComplete` when you need to |
|---|---|
| Cancel / stop the action (`args.cancel = true`) | Call backend API after save/delete |
| Modify data before save (`args.data.field = value`) | Show toast / notification |
| Confirm before delete | Refresh grid data |
| Block unauthorized actions | Sync external React state |
| Customize export before start | Re-enable toolbar buttons |

```tsx
// actionBegin � intercept and mutate
const actionBegin = (args) => {
  if (args.requestType === 'delete')
    args.cancel = !window.confirm(`Delete record ${args.data[0]?.OrderID}?`);

  if (args.requestType === 'save') {
    args.data.UpdatedAt = new Date().toISOString();
    args.data.UpdatedBy = currentUser.id;
  }

  if (['add', 'beginEdit'].includes(args.requestType)) {
    gridRef.current.toolbarModule.enableItems(['Add', 'Edit', 'Delete'], false);
    gridRef.current.toolbarModule.enableItems(['Update', 'Cancel'], true);
  }
};

// actionComplete � react after action
const actionComplete = async (args) => {
  if (args.requestType === 'save') {
    await apiService.syncRecord(args.data);
    showToast('Record saved');
    gridRef.current.clearSelection();
    gridRef.current.toolbarModule.enableItems(['Add', 'Edit', 'Delete'], true);
    gridRef.current.toolbarModule.enableItems(['Update', 'Cancel'], false);
  }
  if (args.requestType === 'delete') {
    await apiService.deleteRecord(args.data[0]);
    gridRef.current.refresh();
  }
};
```

---

## requestType Full Catalog

### CRUD / Edit

| `requestType` | When | Available in |
|---|---|---|
| `'add'` | Add clicked / addRecord() called | `actionBegin` |
| `'beginEdit'` | Row double-clicked / Edit clicked | `actionBegin` |
| `'save'` | Record saved (add or edit) | `actionBegin`, `actionComplete` |
| `'delete'` | Record deleted | `actionBegin`, `actionComplete` |
| `'cancel'` | Edit cancelled | `actionBegin`, `actionComplete` |
| `'batchsave'` | Batch mode all changes saved | `actionBegin`, `actionComplete` |

### Navigation / Data

| `requestType` | When | Available in |
|---|---|---|
| `'sorting'` | Sort applied/removed | both |
| `'filtering'` | Filter applied/cleared | both |
| `'searching'` | Global search | both |
| `'grouping'` | Column grouped | both |
| `'ungrouping'` | Column ungrouped | both |
| `'paging'` | Page changed | both |
| `'refresh'` | Grid refreshed | both |
| `'reorder'` | Column reordered | `actionComplete` |

### Export / Print

| `requestType` | When | Available in |
|---|---|---|
| `'excel'` | Excel export | `actionBegin` |
| `'pdfexport'` | PDF export | `actionBegin` |
| `'csvexport'` | CSV export | `actionBegin` |
| `'print'` | Print | `actionBegin` |

### Scroll

| `requestType` | When | Available in |
|---|---|---|
| `'virtualscroll'` | Virtual scroll chunk loaded | `actionComplete` |
| `'infiniteScroll'` | Infinite scroll next block | `actionComplete` |

---

## args.cancel = true � Interception Patterns

Only works in `actionBegin` and specific before-events.

```tsx
// Cancel delete with confirmation
actionBegin={(args) => {
  if (args.requestType === 'delete')
    args.cancel = !window.confirm('Delete this record?');
}}

// Cancel add � permission check
actionBegin={(args) => {
  if (args.requestType === 'add' && !userHasRole('editor'))
    args.cancel = true;
}}

// Cancel row selection � locked records
rowSelecting={(args) => {
  if (args.data?.Status === 'Locked') args.cancel = true;
}}

// Cancel cell edit � protect primary key in Batch mode
cellEdit={(args) => {
  if (args.columnName === 'OrderID') args.cancel = true;
}}

// Cancel batch save � custom validation
beforeBatchSave={(args) => {
  const errors = validateBatchChanges(args.batchChanges);
  if (errors.length > 0) { args.cancel = true; showValidationDialog(errors); }
}}
```

### Cancellable Events

| Event | Cancellable | Use Case |
|---|---|---|
| `actionBegin` | ? | Cancel any CRUD, sort, filter, group, page, export |
| `rowSelecting` | ? | Prevent selection of locked rows |
| `cellEdit` (Batch) | ? | Prevent editing specific cells |
| `beforeBatchAdd` | ? | Block batch add conditionally |
| `beforeBatchDelete` | ? | Block batch delete conditionally |
| `beforeBatchSave` | ? | Block batch save on validation failure |
| `actionComplete` | ? | Action already done |
| `rowSelected` | ? | Selection already done |

---

## All Event Props Catalog

Wire directly on `<GridComponent>` as props.

### Lifecycle
```tsx
created={fn}           // Grid initialized and rendered
load={fn}              // Before data binding
dataBound={fn}         // After data bound and rows rendered
destroyed={fn}         // Component destroyed
```

### Universal Action
```tsx
actionBegin={fn}       // Before ANY action � check args.requestType
actionComplete={fn}    // After ANY action
actionFailure={fn}     // On error � always wire for error handling
```

### Row
```tsx
rowDataBound={fn}      // Per row during render � use for row styling
rowSelecting={fn}      // Before selection � cancellable
rowSelected={fn}       // After selection � update external state
rowDeselecting={fn}    // Before deselect � cancellable
rowDeselected={fn}     // After deselect
recordClick={fn}       // Single click
recordDoubleClick={fn} // Double click
```

### Cell
```tsx
queryCellInfo={fn}     // Per cell during render � use for cell styling
cellSelecting={fn}     // Before cell select � cancellable
cellSelected={fn}      // After cell select
cellDeselected={fn}    // After cell deselect
```

### Edit (Granular)
```tsx
beginEdit={fn}         // Edit mode started (Inline/Dialog)
cellEdit={fn}          // Cell entered edit (Batch) � cancellable
cellSave={fn}          // Cell saved (Batch)
batchAdd={fn}          // Batch: new row added
batchDelete={fn}       // Batch: row deleted
beforeBatchAdd={fn}    // Cancellable
beforeBatchDelete={fn} // Cancellable
beforeBatchSave={fn}   // Cancellable � use for validation
```

### Column
```tsx
columnDragStart={fn}
columnDrag={fn}
columnDrop ={fn}
resizeStart={fn}
resizing={fn}
resizeStop={fn}                  // auto-save state
```

### Export / Print
```tsx
beforeExcelExport={fn}    // Modify workbook (args.workbook)
excelExportComplete={fn}
beforePdfExport={fn}      // Modify document (args.pdfDocument)
pdfExportComplete={fn}
beforePrint={fn}
printComplete={fn}
```

### Hierarchy / Detail Row
```tsx
detailDataBound={fn}
detailExpand={fn}
detailCollapse={fn}
```

### Context Menu
```tsx
contextMenuClick={fn}  // args.item.id to identify item
contextMenuOpen={fn}   // modify items dynamically
```

### Toolbar
```tsx
toolbarClick={fn}      // args.item.id or args.item.text
```