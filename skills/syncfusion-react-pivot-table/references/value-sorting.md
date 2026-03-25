# Value Sorting Reference - Syncfusion React Pivot Table

## Overview

Value sorting enables sorting pivot table rows and columns based on aggregated measure values (Sum, Average, Count, etc.) instead of alphabetically by field labels. This feature is essential for ranking analysis, identifying top/bottom performers, and organizing data by magnitude rather than label order.

### Key Features
- **Sort by Aggregated Values**: Order by Sum, Average, Count, Min, Max, etc.
- **Ascending/Descending**: Flexible sort direction control
- **Multi-Level Sorting**: Sort by primary and secondary measures
- **Interactive UI**: Click column/row headers to sort
- **Programmatic Control**: Configure sorts via code
- **Performance Optimized**: Efficient sorting on calculated values

## Enabling Value Sorting

### Basic Setup

Value sorting is enabled by default when value fields are configured:

```typescript
import { PivotViewComponent, GroupingBar, Inject } from '@syncfusion/ej2-react-pivotview';
import { DataSourceSettingsModel } from '@syncfusion/ej2-pivotview/src/model/datasourcesettings-model';
import * as React from 'react';
import { pivotData } from './datasource';

function App() {
  const dataSourceSettings: DataSourceSettingsModel = {
    dataSource: pivotData,
    rows: [{ name: 'Country' }],
    columns: [{ name: 'Product' }],
    values: [{ name: 'Sales', caption: 'Total Sales' }]
  };

  return (
    <PivotViewComponent
      id="PivotView"
      dataSourceSettings={dataSourceSettings}
      showGroupingBar={true}  // Enable sorting via header clicks
      height={350}
    >
      <Inject services={[GroupingBar]} />
    </PivotViewComponent>
  );
}

export default App;
```

## Interactive Value Sorting

### Sort by Header Click

Users can click on value column/row headers to sort:

```typescript
// Steps for user:
// 1. Click on a value field header (e.g., "Total Sales")
// 2. A sort icon appears
// 3. Click again to toggle sort direction (↑ Ascending, ↓ Descending)
// 4. Pivot automatically re-sorts based on that measure
```

### Sort Direction Indicators

| Icon | Direction | Meaning |
|------|-----------|---------|
| ↑ | Ascending | Sort lowest to highest |
| ↓ | Descending | Sort highest to lowest |
| ⊼ | None | No sort applied |

## Programmatic Value Sorting

### Define Sorts in Configuration

```typescript
function App() {
  const dataSourceSettings = {
    dataSource: data,
    rows: [{ name: 'Country' }],
    columns: [{ name: 'Product' }],
    values: [{ name: 'Sales' }],
    // Configure sorting
    valueAxis: 'Row',  // Sort rows by value
    sortSettings: [
      {
        name: 'Sales',
        axis: 'Row',
        direction: 'Descending'  // Sort highest sales first
      }
    ]
  };

  return (
    <PivotViewComponent dataSourceSettings={dataSourceSettings} />
  );
}
```

## Sort Configuration

### Sort by Single Value

```typescript
const dataSourceSettings = {
  dataSource: data,
  rows: [{ name: 'Country' }],
  columns: [{ name: 'Year' }],
  values: [
    { name: 'Sales', caption: 'Total Sales' },
    { name: 'Units', caption: 'Units Sold' }
  ],
  // Sort rows descending by Sales value
  sortSettings: [
    {
      name: 'Sales',
      axis: 'Row',
      direction: 'Descending'
    }
  ]
};

<PivotViewComponent dataSourceSettings={dataSourceSettings} />
```

### Sort Ascending vs Descending

```typescript
// Ascending - Low to High
const sortAscending = {
  sortSettings: [
    {
      name: 'Sales',
      axis: 'Row',
      direction: 'Ascending'  // Smallest first
    }
  ]
};

// Descending - High to Low  
const sortDescending = {
  sortSettings: [
    {
      name: 'Sales',
      axis: 'Row',
      direction: 'Descending'  // Largest first
    }
  ]
};
```

## Advanced Sorting

### Multi-Level Sort Configuration

```typescript
function MultiLevelSort() {
  const dataSourceSettings = {
    dataSource: data,
    rows: [{ name: 'Country' }, { name: 'Region' }],
    columns: [{ name: 'Product' }],
    values: [
      { name: 'Sales', caption: 'Total Sales' },
      { name: 'Units', caption: 'Units Sold' }
    ],
    // Sort by multiple criteria
    sortSettings: [
      {
        name: 'Sales',
        axis: 'Row',
        direction: 'Descending'  // Primary sort: High sales first
      },
      {
        name: 'Units',
        axis: 'Row',
        direction: 'Ascending'   // Secondary sort: Low units first
      }
    ]
  };

  return (
    <PivotViewComponent dataSourceSettings={dataSourceSettings} />
  );
}
```

### Sort Rows and Columns

```typescript
const dataSourceSettings = {
  dataSource: data,
  rows: [{ name: 'Country' }],
  columns: [{ name: 'Product' }],
  values: [{ name: 'Sales' }],
  sortSettings: [
    {
      name: 'Sales',
      axis: 'Row',       // Sort row fields
      direction: 'Descending'
    },
    {
      name: 'Sales',
      axis: 'Column',    // Also sort column fields
      direction: 'Ascending'
    }
  ]
};
```

### Sort by Specific Column Value

```typescript
function SortBySpecificProduct() {
  const dataSourceSettings = {
    dataSource: data,
    rows: [{ name: 'Country' }],
    columns: [{ name: 'Product' }],
    values: [{ name: 'Sales' }],
    // Sort countries by sales for a specific product
    sortSettings: [
      {
        name: 'Sales',
        axis: 'Row',
        direction: 'Descending',
        comparedField: 'Laptops'  // Sort by Laptops sales specifically
      }
    ]
  };

  return <PivotViewComponent dataSourceSettings={dataSourceSettings} />;
}
```

## Dynamic Sort Management

### Update Sorting Programmatically

```typescript
function DynamicSorting() {
  let pivotObj: PivotViewComponent;
  const [sortField, setSortField] = React.useState('Sales');
  const [sortDirection, setSortDirection] = React.useState('Descending');

  const applySorting = (): void => {
    if (pivotObj && pivotObj.dataSourceSettings) {
      pivotObj.dataSourceSettings.sortSettings = [
        {
          name: sortField,
          axis: 'Row',
          direction: sortDirection
        }
      ];
    }
  };

  const toggleDirection = (): void => {
    setSortDirection(sortDirection === 'Ascending' ? 'Descending' : 'Ascending');
  };

  return (
    <div>
      <div style={{ marginBottom: '15px' }}>
        <label>Sort By: </label>
        <select value={sortField} onChange={(e) => setSortField(e.target.value)}>
          <option value="Sales">Sales</option>
          <option value="Units">Units</option>
          <option value="Amount">Amount</option>
        </select>
      </div>
      <div style={{ marginBottom: '15px' }}>
        <label>Direction: </label>
        <button onClick={toggleDirection}>
          {sortDirection === 'Ascending' ? '↑ Ascending' : '↓ Descending'}
        </button>
      </div>
      <button onClick={applySorting}>Apply Sort</button>
      <PivotViewComponent
        ref={(d: PivotViewComponent) => pivotObj = d}
        dataSourceSettings={dataSourceSettings}
      />
    </div>
  );
}
```

### Remove Sorting

```typescript
function ClearSorting() {
  let pivotObj: PivotViewComponent;

  const removeSorting = (): void => {
    if (pivotObj && pivotObj.dataSourceSettings) {
      pivotObj.dataSourceSettings.sortSettings = [];  // Clear all sorts
    }
  };

  return (
    <div>
      <button onClick={removeSorting}>Clear Sorting</button>
      <PivotViewComponent
        ref={(d: PivotViewComponent) => pivotObj = d}
        dataSourceSettings={dataSourceSettings}
      />
    </div>
  );
}
```

## Event Handling

### Sort Event Capture

```typescript
const onActionComplete = (args: any): void => {
  if (args.actionName === 'Sort') {
    console.log('Sorting applied');
    console.log('Sort settings:', args.sortSettings);
  }
};

const onActionBegin = (args: any): void => {
  if (args.actionName === 'Sort') {
    console.log('Starting sort operation');
  }
};

<PivotViewComponent
  actionBegin={onActionBegin}
  actionComplete={onActionComplete}
  dataSourceSettings={dataSourceSettings}
/>
```

## Practical Examples

### Example 1: Rank Countries by Total Sales

```typescript
function RankCountries() {
  const dataSourceSettings = {
    dataSource: [
      { Country: 'USA', Sales: 550000 },
      { Country: 'Canada', Sales: 320000 },
      { Country: 'Mexico', Sales: 145000 },
      { Country: 'Brazil', Sales: 420000 },
      { Country: 'Argentina', Sales: 95000 }
    ],
    rows: [{ name: 'Country' }],
    values: [{ name: 'Sales', caption: 'Total Sales', format: 'C0' }],
    // Sort countries by sales descending (highest first)
    sortSettings: [
      {
        name: 'Sales',
        axis: 'Row',
        direction: 'Descending'  // Ranking from top to bottom
      }
    ]
  };

  return (
    <PivotViewComponent
      id="country-ranking"
      dataSourceSettings={dataSourceSettings}
      height={400}
    />
  );
}

export default RankCountries;
```

### Example 2: Sort Products by Average Rating

```typescript
function SortByAverage() {
  const dataSourceSettings = {
    dataSource: productData,
    rows: [{ name: 'Product' }],
    columns: [{ name: 'Category' }],
    values: [
      { name: 'Amount', caption: 'Total Amount', format: 'C2' },
      { name: 'Rating', caption: 'Avg Rating', type: 'Avg' }
    ],
    // Sort products by average rating (highest first)
    sortSettings: [
      {
        name: 'Rating',
        axis: 'Row',
        direction: 'Descending'
      }
    ]
  };

  return (
    <PivotViewComponent
      dataSourceSettings={dataSourceSettings}
      showFieldList={true}
      height={500}
    />
  );
}

export default SortByAverage;
```

### Example 3: Multi-Criteria Sorting

```typescript
function MultiCriteriaSorting() {
  const dataSourceSettings = {
    dataSource: salesData,
    rows: [{ name: 'Region' }, { name: 'Salesperson' }],
    columns: [{ name: 'Year' }],
    values: [
      { name: 'Sales', caption: 'Total Sales', format: 'C2' },
      { name: 'Commission', caption: 'Commission' }
    ],
    // Sort regions by sales, then salespeople by commission
    sortSettings: [
      {
        name: 'Sales',
        axis: 'Row',
        direction: 'Descending'  // Regions: highest sales first
      },
      {
        name: 'Commission',
        axis: 'Row',
        direction: 'Descending'  // Salespeople: highest commission first
      }
    ]
  };

  return (
    <PivotViewComponent
      id="multi-sort"
      dataSourceSettings={dataSourceSettings}
      height={500}
    />
  );
}

export default MultiCriteriaSorting;
```

## Performance Considerations

1. **Large Datasets**: Value sorting may require full data scan
2. **Multiple Sort Criteria**: Additional overhead per criterion
3. **Real-Time Sorting**: User clicks on headers trigger re-sort
4. **Server-Side Sorting**: OLAP data sorted server-side automatically

## Combining with Other Features

### Value Sorting + Value Filtering

```typescript
function SortAndFilter() {
  const dataSourceSettings = {
    dataSource: data,
    rows: [{ name: 'Country' }],
    columns: [{ name: 'Product' }],
    values: [{ name: 'Sales' }],
    // First filter to show only high values
    filterSettings: [
      {
        name: 'Sales',
        type: 'Value',
        condition: 'GreaterThan',
        value1: '50000'
      }
    ],
    // Then sort remaining by value
    sortSettings: [
      {
        name: 'Sales',
        axis: 'Row',
        direction: 'Descending'
      }
    ]
  };

  return <PivotViewComponent dataSourceSettings={dataSourceSettings} />;
}
```

### Value Sorting + Conditional Formatting

```typescript
function SortWithConditionalFormatting() {
  const dataSourceSettings = {
    dataSource: data,
    rows: [{ name: 'Country' }],
    values: [{ name: 'Sales' }],
    sortSettings: [
      {
        name: 'Sales',
        axis: 'Row',
        direction: 'Descending'  // Sorted from high to low
      }
    ]
  };

  function cellTemplate(props: any): JSX.Element {
    const value = parseFloat(props.cellInfo?.value);
    const isHigh = value > 100000;
    
    return (
      <span style={{ color: isHigh ? '#4CAF50' : '#FF9800' }}>
        {value.toLocaleString('en-US', { style: 'currency', currency: 'USD' })}
      </span>
    );
  }

  return (
    <PivotViewComponent
      dataSourceSettings={dataSourceSettings}
      cellTemplate={cellTemplate}
    />
  );
}
```

## Best Practices

✅ **Do:**
- Sort by most relevant measure for analysis
- Combine sorting with filtering for focused insights
- Use descending sort for identifying top performers
- Sort after aggregations are fully configured
- Allow users to toggle sort direction interactively

❌ **Don't:**
- Sort by labels when value sorting is needed
- Apply contradictory sorts to same axis
- Change sorting frequently during analysis (impacts performance)
- Forget to communicate sort order to users
- Sort unmeaningful aggregations

## Common Issues

**Sort not applying?**
- Verify sort field name matches value field exactly
- Check data contains numeric values to sort
- Ensure sort is applied after data binding complete

**Unexpected sort order?**
- Verify sort direction (Ascending vs Descending)
- Check if nulls/zeros are included in sort
- Review data types (might be text vs number)

**Performance degradation?**
- Simplify multi-level sorts
- Sort only necessary fields
- Use smaller datasets for testing
- Consider pagination with sorting

## Related Features

- **Label Sorting**: Sort by field names alphabetically
- **Value Filtering**: Filter by aggregated values
- **Member Filtering**: Filter by specific members
- **Conditional Formatting**: Visually highlight sorted data
