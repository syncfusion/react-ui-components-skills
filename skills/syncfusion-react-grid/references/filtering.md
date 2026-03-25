# Filtering in React Grid

## Table of Contents
- [Overview](#overview)
- [Enable Filtering](#enable-filtering)
- [Filter Types](#filter-types)
- [Initial Filters](#initial-filters)
- [Filter Conditions](#filter-conditions)
- [Advanced Filtering](#advanced-filtering)

## Overview

Filtering allows users to narrow down data by applying criteria to columns. It supports multiple filter modes, operators, and custom filter logic.

## Enable Filtering

### Basic Filtering Setup

```tsx
import { GridComponent, ColumnsDirective, ColumnDirective, Inject, Filter } from '@syncfusion/ej2-react-grids';

<GridComponent dataSource={data} allowFiltering={true}>
  <ColumnsDirective>
    <ColumnDirective field='OrderID' headerText='Order ID' width='100' />
    <ColumnDirective field='CustomerID' headerText='Customer' width='120' />
    <ColumnDirective field='Freight' headerText='Freight' width='100' format='C2' />
  </ColumnsDirective>
  <Inject services={[Filter]} />
</GridComponent>
```

## Filter Types

### Filter Bar (Default)

Users type in cells below column headers:

```tsx
const filterSettings = {
  type: 'FilterBar'
};

<GridComponent
  dataSource={data}
  allowFiltering={true}
  filterSettings={filterSettings}
>
  {/* columns */}
  <Inject services={[Filter]} />
</GridComponent>
```

### Filter Menu

Dropdown menu in column headers:

```tsx
const filterSettings = {
  type: 'Menu'
};

<GridComponent
  dataSource={data}
  filterSettings={filterSettings}
  allowFiltering={true}
>
  {/* columns */}
  <Inject services={[Filter]} />
</GridComponent>
```

### Excel-Like Filter

Checkbox grid for filtering:

```tsx
const filterSettings = {
  type: 'Excel'
};

<GridComponent
  dataSource={data}
  filterSettings={filterSettings}
  allowFiltering={true}
>
  {/* columns */}
  <Inject services={[Filter]} />
</GridComponent>
```

## Initial Filters

Apply filters automatically on grid load:

```tsx
const filterSettings = {
  columns: [
    { field: 'CustomerID', operator: 'StartsWith', value: 'VIN' },
    { field: 'Freight', operator: 'GreaterThan', value: 30 }
  ]
};

<GridComponent
  dataSource={data}
  filterSettings={filterSettings}
  allowFiltering={true}
>
  {/* columns */}
  <Inject services={[Filter]} />
</GridComponent>
```

## Filter Conditions

### Common Filter Operators

| Operator | Usage | Example |
|----------|-------|---------|
| `StartsWith` | Text begins with | `'StartsWith', 'VIN'` |
| `EndsWith` | Text ends with | `'EndsWith', 'NET'` |
| `Contains` | Text contains | `'Contains', 'ING'` |
| `Equal` | Exact match | `'Equal', 'VINET'` |
| `NotEqual` | Not equal to | `'NotEqual', 'USA'` |
| `GreaterThan` | Greater than | `'GreaterThan', 100` |
| `GreaterThanOrEqual` | ≥ | `'GreaterThanOrEqual', 100` |
| `LessThan` | Less than | `'LessThan', 50` |
| `LessThanOrEqual` | ≤ | `'LessThanOrEqual', 50` |
| `Between` | Within range | `'Between', 10, 100` |
| `NotContains` | Does not contain | `'NotContains', 'test'` |

### Multiple Filter Conditions

```tsx
const filterSettings = {
  columns: [
    { field: 'CustomerID', operator: 'StartsWith', value: 'A', matchCase: false },
    { field: 'Freight', operator: 'GreaterThan', value: 20 },
    { field: 'ShipCountry', operator: 'Equal', value: 'USA' }
  ],
  mode: 'Immediate'  // Apply filter as you type
};

<GridComponent filterSettings={filterSettings}>
  {/* columns */}
</GridComponent>
```

## Advanced Filtering

### Programmatic Filtering

```tsx
import React, { useRef } from 'react';

function App() {
  const gridRef = useRef(null);

  const filterData = () => {
    gridRef.current.filterByColumn('CustomerID', 'StartsWith', 'VIN');
  };

  const clearFilters = () => {
    gridRef.current.clearFiltering();
  };

  return (
    <div>
      <button onClick={filterData}>Filter VINET</button>
      <button onClick={clearFilters}>Clear Filter</button>

      <GridComponent ref={gridRef} dataSource={data} allowFiltering={true}>
        {/* columns */}
        <Inject services={[Filter]} />
      </GridComponent>
    </div>
  );
}

export default App;
```

### Disable Filtering for Specific Column

```tsx
<ColumnDirective
  field='InternalNotes'
  headerText='Notes'
  allowFiltering={false}
  width='200'
/>
```

### Custom Filter Templates

```tsx
const filterTemplate = () => {
  return (
    <input
      type='text'
      placeholder='Enter name...'
      onChange={(e) => {
        gridRef.current.filterByColumn('CustomerID', 'Contains', e.target.value);
      }}
    />
  );
};

<ColumnDirective
  field='CustomerID'
  filterTemplate={filterTemplate}
/>
```

### Handle Filter Events

```tsx
const onActionBegin = (args) => {
  console.log('Filter column:', args.column.field);
  console.log('Filter operator:', args.operator);
  console.log('Filter value:', args.value);
};

<GridComponent
  dataSource={data}
  allowFiltering={true}
  actionBegin={onActionBegin}
>
  {/* columns */}
  <Inject services={[Filter]} />
</GridComponent>
```

### Case-Insensitive Filtering

```tsx
const filterSettings = {
  columns: [
    {
      field: 'CustomerID',
      operator: 'StartsWith',
      value: 'vin',
      matchCase: false  // Case-insensitive
    }
  ]
};

<GridComponent filterSettings={filterSettings}>
  {/* columns */}
</GridComponent>
```

### Composite Filtering (Multiple Values)

```tsx
const filterSettings = {
  columns: [
    { field: 'ShipCountry', operator: 'Equal', value: 'USA' },
    { field: 'ShipCountry', operator: 'Equal', value: 'UK' }
  ]
};

<GridComponent filterSettings={filterSettings}>
  {/* columns */}
</GridComponent>
```

### Remote Filtering

Apply filters on server-side for large datasets:

```tsx
import { DataManager, UrlAdaptor, Query } from '@syncfusion/ej2-data';
import { GridComponent, Inject, Filter } from '@syncfusion/ej2-react-grids';

function GridWithRemoteFilter() {
  const data = new DataManager({
    url: 'https://api.example.com/orders',
    adaptor: new UrlAdaptor()
  });

  const onActionComplete = (args) => {
    // Filter applied, server handles filtering automatically
    console.log('Filter applied:', args);
  };

  return (
    <GridComponent
      dataSource={data}
      allowFiltering={true}
      filterSettings={{ type: 'Menu' }}
      actionComplete={onActionComplete}
      allowPaging={true}
      pageSettings={{ pageSize: 12 }}
    >
      <ColumnsDirective>
        <ColumnDirective field='OrderID' headerText='Order ID' width='100' />
        <ColumnDirective field='CustomerID' headerText='Customer' width='120' />
        <ColumnDirective field='Freight' headerText='Freight' format='C2' />
      </ColumnsDirective>
      <Inject services={[Filter, Page]} />
    </GridComponent>
  );
}

export default GridWithRemoteFilter;
```

### Advanced Predicate Filtering

Use Predicate for complex filter logic:

```tsx
import { GridComponent, Inject, Filter } from '@syncfusion/ej2-react-grids';
import { Predicate, Query } from '@syncfusion/ej2-data';
import React, { useRef } from 'react';

function GridWithPredicateFilter() {
  const gridRef = useRef(null);

  const applyComplexFilter = () => {
    // (Freight > 50 AND Status = 'Active') OR (CustomerID STARTS WITH 'VIN')
    let predicate = new Predicate('Freight', 'greaterThan', 50);
    predicate = predicate.and('Status', 'equal', 'Active');
    predicate = predicate.or('CustomerID', 'startswith', 'VIN');

    gridRef.current.query = new Query().where(predicate);
    gridRef.current.refresh();
  };

  return (
    <div>
      <button onClick={applyComplexFilter}>Apply Complex Filter</button>
      <GridComponent ref={gridRef} dataSource={data} allowFiltering={true}>
        <ColumnsDirective>
          <ColumnDirective field='OrderID' headerText='Order ID' width='100' />
          <ColumnDirective field='Freight' headerText='Freight' format='C2' />
          <ColumnDirective field='Status' headerText='Status' width='100' />
          <ColumnDirective field='CustomerID' headerText='Customer' width='120' />
        </ColumnsDirective>
        <Inject services={[Filter]} />
      </GridComponent>
    </div>
  );
}

export default GridWithPredicateFilter;
```

### Filter with Date Range

```tsx
import React, { useState, useRef } from 'react';
import { Predicate, Query } from '@syncfusion/ej2-data';

function GridWithDateRangeFilter() {
  const gridRef = useRef(null);
  const [startDate, setStartDate] = useState(null);
  const [endDate, setEndDate] = useState(null);

  const applyDateRange = () => {
    const predicate = new Predicate('OrderDate', 'greaterthanorequal', startDate);
    predicate.and('OrderDate', 'lessthanorequal', endDate);

    gridRef.current.query = new Query().where(predicate);
    gridRef.current.refresh();
  };

  return (
    <div>
      <div>
        <label>Start Date:
          <input
            type='date'
            onChange={(e) => setStartDate(new Date(e.target.value))}
          />
        </label>
        <label>End Date:
          <input
            type='date'
            onChange={(e) => setEndDate(new Date(e.target.value))}
          />
        </label>
        <button onClick={applyDateRange}>Filter</button>
      </div>

      <GridComponent ref={gridRef} dataSource={data} allowFiltering={true}>
        <ColumnsDirective>
          <ColumnDirective field='OrderID' headerText='Order ID' width='100' />
          <ColumnDirective
            field='OrderDate'
            headerText='Order Date'
            type='date'
            format='yMd'
          />
        </ColumnsDirective>
        <Inject services={[Filter]} />
      </GridComponent>
    </div>
  );
}

export default GridWithDateRangeFilter;
```

### Custom Filter UI

```tsx
function GridWithCustomFilter() {
  const gridRef = useRef(null);

  const handleCustomFilter = (e) => {
    const searchTerm = e.target.value;
    const predicate = new Predicate('CustomerID', 'contains', searchTerm, false);
    const predicate2 = predicate.or('ShipCity', 'contains', searchTerm, false);

    gridRef.current.query = new Query().where(predicate2);
    gridRef.current.refresh();
  };

  return (
    <div>
      <div>
        <input
          type='text'
          placeholder='Search by customer or city...'
          onChange={handleCustomFilter}
          style={{ padding: '10px', width: '300px' }}
        />
      </div>

      <GridComponent ref={gridRef} dataSource={data} allowFiltering={true}>
        <ColumnsDirective>
          <ColumnDirective field='OrderID' headerText='Order ID' width='100' />
          <ColumnDirective field='CustomerID' headerText='Customer' width='120' />
          <ColumnDirective field='ShipCity' headerText='City' width='120' />
        </ColumnsDirective>
        <Inject services={[Filter]} />
      </GridComponent>
    </div>
  );
}
```

### Filter Templates

```tsx
const dropdownFilterTemplate = (props) => {
  return (
    <select
      onChange={(e) => {
        gridRef.current.filterByColumn(props.column.field, 'equal', e.target.value);
      }}
    >
      <option value=''>--Select--</option>
      <option value='Active'>Active</option>
      <option value='Inactive'>Inactive</option>
      <option value='Pending'>Pending</option>
    </select>
  );
};

const numberRangeTemplate = (props) => {
  return (
    <div>
      <input
        type='number'
        placeholder='Min'
        onChange={(e) => {
          gridRef.current.filterByColumn(props.column.field, 'greaterthanorequal', e.target.value);
        }}
      />
      <input
        type='number'
        placeholder='Max'
        onChange={(e) => {
          gridRef.current.filterByColumn(props.column.field, 'lessthanorequal', e.target.value);
        }}
      />
    </div>
  );
};

<GridComponent dataSource={data} allowFiltering={true}>
  <ColumnsDirective>
    <ColumnDirective field='Status' filterTemplate={dropdownFilterTemplate} />
    <ColumnDirective field='Freight' filterTemplate={numberRangeTemplate} />
  </ColumnsDirective>
  <Inject services={[Filter]} />
</GridComponent>
```

### Filter State Persistence

```tsx
import React, { useEffect, useRef } from 'react';

function GridWithFilterPersistence() {
  const gridRef = useRef(null);

  useEffect(() => {
    // Load saved filters from localStorage
    const savedFilters = localStorage.getItem('gridFilters');
    if (savedFilters) {
      const filters = JSON.parse(savedFilters);
      gridRef.current.filterSettings.columns = filters;
      gridRef.current.refresh();
    }
  }, []);

  const saveFilterState = () => {
    const filters = gridRef.current.filterSettings.columns;
    localStorage.setItem('gridFilters', JSON.stringify(filters));
    alert('Filters saved');
  };

  return (
    <div>
      <button onClick={saveFilterState}>Save Filters</button>
      <GridComponent
        ref={gridRef}
        dataSource={data}
        allowFiltering={true}
        filterSettings={{ type: 'Menu' }}
      >
        <ColumnsDirective>
          <ColumnDirective field='OrderID' headerText='Order ID' width='100' />
          <ColumnDirective field='CustomerID' headerText='Customer' width='120' />
          <ColumnDirective field='Freight' headerText='Freight' format='C2' />
        </ColumnsDirective>
        <Inject services={[Filter]} />
      </GridComponent>
    </div>
  );
}

export default GridWithFilterPersistence;
```
