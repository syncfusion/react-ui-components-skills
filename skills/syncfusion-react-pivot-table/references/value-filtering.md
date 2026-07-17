# Value Filtering Reference - Syncfusion React Pivot Table

## Overview

Value filtering enables filtering of pivot table rows and columns based on aggregated measure values (Sum, Average, Count, etc.) rather than field labels or member names. This feature is powerful for isolating data that meets specific numeric criteria, such as showing only regions with sales above a threshold or products with average quantities below a target.

### Key Features
- **Filter by Aggregated Values**: Filter based on Sum, Average, Count, Min, Max, etc.
- **Flexible Operators**: Supports range and comparison operators
- **Multi-Field Support**: Filter by one or more value fields
- **UI-Based Filtering**: Built-in filter dialog in Field List
- **Conditional Analysis**: Quickly identify outliers and trends
- **Performance Optimized**: Efficient filtering on calculated aggregates

## Enabling Value Filtering

### Basic Setup

To enable value filtering, set `allowValueFilter` to **true**:

```typescript
import { PivotViewComponent, FieldList, ValueFilter, Inject } from '@syncfusion/ej2-react-pivotview';
import { DataSourceSettingsModel } from '@syncfusion/ej2-pivotview/src/model/datasourcesettings-model';
import * as React from 'react';
import { pivotData } from './datasource';

function App() {
  const dataSourceSettings: DataSourceSettingsModel = {
    dataSource: pivotData as IDataSet[],
    rows: [{ name: 'Country' }],
    columns: [{ name: 'Product' }],
    values: [{ name: 'Sales', caption: 'Total Sales' }]
  };

  return (
    <PivotViewComponent
      id="PivotView"
      dataSourceSettings={dataSourceSettings}
      allowValueFilter={true}
      showFieldList={true}
      height={350}
    >
      <Inject services={[FieldList, ValueFilter]} />
    </PivotViewComponent>
  );
}

export default App;
```

## Value Filtering Operators

| Operator | Description | Use Case |
|----------|-------------|----------|
| `Equals` | Value equals exactly | Sales = $50,000 |
| `DoesNotEquals` | Value does not equal | Sales ≠ $25,000 |
| `GreaterThan` | Value greater than | Sales > $100,000 |
| `GreaterThanOrEqualTo` | Value >= threshold | Sales >= $50,000 |
| `LessThan` | Value less than | Sales < $10,000 |
| `LessThanOrEqualTo` | Value <= threshold | Sales <= $100,000 |
| `Between` | Value within range | Sales between $25K-$100K |
| `NotBetween` | Value outside range | Sales not between $0-$5K |
| `Top` | Top N members by highest values (client-side only) | Top 10 countries by sales |
| `Bottom` | Bottom N members by lowest values (client-side only) | Bottom 5 products by average |

## Basic Value Filtering

### Filter by Single Value Threshold

```typescript
import { PivotViewComponent, FieldList, IDataSet, Inject } from '@syncfusion/ej2-react-pivotview';
import { DataSourceSettingsModel } from '@syncfusion/ej2-pivotview/src/model/datasourcesettings-model';

function App() {
  const dataSourceSettings: DataSourceSettingsModel = {
    dataSource: pivotData as IDataSet[],
    expandAll: false,
    allowValueFilter: true,
    rows: [{ name: 'Country' }],
    columns: [{ name: 'Year' }],
    values: [{ name: 'Sold', caption: 'Units Sold' }],
    // Filter rows where total units sold per country > 2000
    filterSettings: [
      {
        name: 'Country',
        measure: 'Sold',
        type: 'Value',
        condition: 'GreaterThan',
        value1: '2000'
      }
    ]
  };

  return (
    <PivotViewComponent
      id='PivotView'
      height={350}
      dataSourceSettings={dataSourceSettings}
      showFieldList={true}
    >
      <Inject services={[FieldList]} />
    </PivotViewComponent>
  );
}

export default App;
```

### Range Value Filtering

```typescript
const dataSourceSettings: DataSourceSettingsModel = {
  dataSource: pivotData as IDataSet[],
  expandAll: false,
  allowValueFilter: true,
  rows: [{ name: 'Country' }],
  columns: [{ name: 'Year' }],
  values: [{ name: 'Sold', caption: 'Units Sold' }, { name: 'Amount', caption: 'Sold Amount' }],
  // Filter to show only countries with units sold between 1500 and 5000
  filterSettings: [
    {
      name: 'Country',
      measure: 'Sold',
      type: 'Value',
      condition: 'Between',
      value1: '1500',
      value2: '5000'
    }
  ]
};
```

## Top/Bottom N Filtering

The `Top` and `Bottom` operators allow you to display only the top N or bottom N members based on the aggregated value of a measure field. Use `value1` to specify the count N. **Note:** Top/Bottom filtering is performed client-side only.

### Top N Values

```typescript
function TopCountriesByRevenue() {
  const dataSourceSettings: DataSourceSettingsModel = {
    dataSource: pivotData as IDataSet[],
    expandAll: false,
    allowValueFilter: true,
    rows: [{ name: 'Country' }],
    columns: [{ name: 'Year', caption: 'Production Year' }, { name: 'Quarter' }],
    values: [{ name: 'Sold', caption: 'Units Sold' }, { name: 'Amount', caption: 'Sold Amount' }],
    filters: [],
    // Show top 5 countries by total units sold
    filterSettings: [
      {
        name: 'Country',
        measure: 'Sold',
        type: 'Value',
        condition: 'Top',
        value1: '5'
      }
    ]
  };

  return (
    <PivotViewComponent
      id='PivotView'
      height={350}
      dataSourceSettings={dataSourceSettings}
      showFieldList={true}
    >
      <Inject services={[FieldList]} />
    </PivotViewComponent>
  );
}

export default TopCountriesByRevenue;
```

### Bottom N Values

```typescript
const dataSourceSettings: DataSourceSettingsModel = {
  dataSource: pivotData as IDataSet[],
  expandAll: false,
  allowValueFilter: true,
  rows: [{ name: 'Country' }, { name: 'Products' }],
  columns: [{ name: 'Year', caption: 'Production Year' }, { name: 'Quarter' }],
  values: [{ name: 'Sold', caption: 'Units Sold' }, { name: 'Amount', caption: 'Sold Amount' }],
  filters: [],
  // Show bottom 5 countries by total units sold
  filterSettings: [
    {
      name: 'Country',
      measure: 'Sold',
      type: 'Value',
      condition: 'Bottom',
      value1: '5'
    }
  ]
};
```

## Clearing the Existing Value Filter

You can clear the applied value filter by clicking the **Clear** option at the bottom of the filter dialog under the **Value** tab.

## Advanced Value Filtering

### Multiple Field Filtering

```typescript
function MultiFieldValueFilter() {
  const dataSourceSettings: DataSourceSettingsModel = {
    dataSource: data as IDataSet[],
    rows: [{ name: 'Country' }],
    columns: [{ name: 'Product' }],
    values: [
      { name: 'Sales', caption: 'Total Sales' },
      { name: 'Units', caption: 'Units Sold' }
    ],
    // Filter by multiple value columns
    filterSettings: [
      {
        name: 'Sales',
        type: 'Value',
        condition: 'GreaterThan',
        value1: '50000'  // Sales > 50K
      },
      {
        name: 'Units',
        type: 'Value',
        condition: 'GreaterThan',
        value1: '1000'   // Units > 1000
      }
    ]
  };

  return (
    <PivotViewComponent
      dataSourceSettings={dataSourceSettings}
      allowValueFilter={true}
    />
  );
}
```

### Column-Level Value Filtering

To filter values on a specific axis, place the field on the desired axis (row or column). Value filtering is applied to the field on that axis automatically, and the `filterSettings.name` should reference a row or column field:

```typescript
const dataSourceSettings: DataSourceSettingsModel = {
  dataSource: data as IDataSet[],
  rows: [{ name: 'Country' }],
  columns: [{ name: 'Year' }, { name: 'Quarter' }],
  values: [{ name: 'Sales' }],
  // Filter the Country field (row axis) by total sales
  filterSettings: [
    {
      name: 'Country',
      measure: 'Sales',
      type: 'Value',
      condition: 'GreaterThan',
      value1: 100000
    }
  ]
};
```

## Value Filter Dialog

### Interactive Filter Customization

```typescript
function InteractiveValueFilter() {
  let pivotObj: PivotViewComponent;
  const [filterValue, setFilterValue] = React.useState(50000);
  const [operator, setOperator] = React.useState('GreaterThan');

  const applyFilter = (): void => {
    if (pivotObj && pivotObj.dataSourceSettings) {
      pivotObj.dataSourceSettings.filterSettings = [
        {
          name: 'Sales',
          type: 'Value',
          condition: operator,
          value1: filterValue.toString()
        }
      ];
      // Required for the change to take effect
      pivotObj.refresh();
    }
  };

  return (
    <div>
      <div style={{ marginBottom: '15px' }}>
        <label>Operator: </label>
        <select value={operator} onChange={(e) => setOperator(e.target.value)}>
          <option value="GreaterThan">Greater Than</option>
          <option value="LessThan">Less Than</option>
          <option value="Between">Between</option>
          <option value="Top">Top N</option>
          <option value="Bottom">Bottom N</option>
        </select>
      </div>
      <div style={{ marginBottom: '15px' }}>
        <label>Value: </label>
        <input
          type="number"
          value={filterValue}
          onChange={(e) => setFilterValue(parseInt(e.target.value))}
        />
      </div>
      <button onClick={applyFilter}>Apply Filter</button>
      <PivotViewComponent
        ref={(d: PivotViewComponent) => pivotObj = d}
        dataSourceSettings={dataSourceSettings}
        allowValueFilter={true}
      />
    </div>
  );
}
```

## Practical Examples

### Example 1: Show Only High-Performing Regions

```typescript
import { IDataSet } from '@syncfusion/ej2-react-pivotview';

function HighPerformingRegions() {
  const dataSourceSettings: DataSourceSettingsModel = {
    dataSource: [
      { Region: 'North', Sales: 150000 },
      { Region: 'South', Sales: 45000 },
      { Region: 'East', Sales: 120000 },
      { Region: 'West', Sales: 30000 },
      { Region: 'Central', Sales: 200000 }
    ] as IDataSet[],
    rows: [{ name: 'Region' }],
    values: [{ name: 'Sales', caption: 'Total Sales' }],
    // Show only regions with sales > $100,000
    filterSettings: [
      {
        name: 'Sales',
        type: 'Value',
        condition: 'GreaterThan',
        value1: '100000'
      }
    ]
  };

  return (
    <PivotViewComponent
      id="high-performers"
      dataSourceSettings={dataSourceSettings}
      allowValueFilter={true}
      height={300}
    >
      <Inject services={[ValueFilter]} />
    </PivotViewComponent>
  );
}

export default HighPerformingRegions;
```

### Example 2: Top 10 Products by Revenue

```typescript
function TopProductsReport() {
  const dataSourceSettings: DataSourceSettingsModel = {
    dataSource: productData as IDataSet[],
    rows: [{ name: 'Product' }],
    columns: [{ name: 'Category' }],
    values: [
      { name: 'Revenue', caption: 'Total Revenue', format: 'C2' },
      { name: 'Units', caption: 'Units Sold', format: 'N0' }
    ],
    // Show top 10 products by revenue
    filterSettings: [
      {
        name: 'Revenue',
        type: 'Value',
        condition: 'Top',
        value1: '10'
      }
    ]
  };

  return (
    <PivotViewComponent
      id="top-products"
      dataSourceSettings={dataSourceSettings}
      allowValueFilter={true}
      showFieldList={true}
      height={500}
    >
      <Inject services={[FieldList, ValueFilter]} />
    </PivotViewComponent>
  );
}

export default TopProductsReport;
```

### Example 3: Filter by Average Value Range

```typescript
function AverageValueRange() {
  const dataSourceSettings: DataSourceSettingsModel = {
    dataSource: salesData as IDataSet[],
    rows: [{ name: 'Country' }],
    columns: [{ name: 'Product' }],
    values: [
      { name: 'Amount', caption: 'Sales Amount' }
    ],
    // Filter to show countries averaging between $50K-$150K per product
    filterSettings: [
      {
        name: 'Amount',
        type: 'Value',
        condition: 'Between',
        value1: '50000',
        value2: '150000'
      }
    ]
  };

  return (
    <PivotViewComponent
      id="avg-range"
      dataSourceSettings={dataSourceSettings}
      height={400}
    />
  );
}

export default AverageValueRange;
```

## Performance Considerations

1. **Top/Bottom Filtering**: Computed efficiently even on large datasets
2. **Range Filters**: Generally faster than multiple conditions
3. **Percentage Filtering**: Requires full data scan but still efficient
4. **Multiple Value Fields**: Each adds slight overhead

```typescript
// Optimize by filtering data before pivot
const filteredData = rawData.filter(item => item.Sales > 50000);

<PivotViewComponent
  dataSourceSettings={{
    dataSource: filteredData as IDataSet[],  // Pre-filtered data
    rows: [{ name: 'Country' }],
    values: [{ name: 'Sales' }]
  }}
  allowValueFilter={true}
/>
```

## Best Practices

✅ **Do:**
- Use Top/Bottom N for comparative analysis (outlier identification)
- Combine with member filtering for multi-criteria analysis
- Use range filtering for excluding outliers
- Apply value filters after evaluating all aggregations
- Use clear thresholds aligned with business metrics

❌ **Don't:**
- Filter out all data with overly restrictive thresholds
- Use value filtering where label filtering is more appropriate
- Apply excessive value filters without understanding data distribution
- Forget to consider which value field is being filtered

## Common Issues

**Filter excludes all rows?**
- Verify threshold values are within data range
- Check data aggregation (Sum vs Avg vs Count)
- Review format settings (currency might have $ sign)

**Unexpected results with Top/Bottom?**
- Verify N value is appropriate for your dataset
- Check if ties exist (multiple values at threshold)
- Consider total row/column counts

**Performance slow?**
- Simplify filter conditions
- Use pre-filtered data when possible
- Consider server-side filtering for OLAP data

## Related Features

- **Label Filtering**: Filter by field headers/text
- **Member Filtering**: Filter by exact member values
- **Value Sorting**: Sort by measure values
- **Conditional Formatting**: Highlight cells meeting value criteria
