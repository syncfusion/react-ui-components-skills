# Member Filtering Reference - Syncfusion React Pivot Table

## Overview

Member filtering enables users to selectively include or exclude specific members (values) from rows, columns, and filter fields in the pivot table. Unlike label filtering which filters based on field headers/text patterns, member filtering works directly with the actual data member values. This feature is essential for focusing analysis on specific subsets of data.

### Key Features
- **Individual Member Selection**: Choose specific values to include or exclude
- **UI-Based Filtering**: Built-in member filter dialog in Field List
- **Programmatic Control**: Filter members via code using `drilledMembers`
- **Multi-Select Support**: Select multiple members at once
- **Hierarchical Support**: Filter members within grouped/hierarchical data
- **Performance**: Efficient handling of large member lists

## Enabling Member Filtering

### Basic Setup

To enable member filtering, set `allowMemberFilter` to **true** and inject the required modules:

```typescript
import { PivotViewComponent, FieldList, MemberFilter, Inject } from '@syncfusion/ej2-react-pivotview';
import { DataSourceSettingsModel } from '@syncfusion/ej2-pivotview/src/model/datasourcesettings-model';
import * as React from 'react';
import { pivotData } from './datasource';

function App() {
  const dataSourceSettings: DataSourceSettingsModel = {
    dataSource: pivotData,
    rows: [{ name: 'Country' }],
    columns: [{ name: 'Product' }],
    values: [{ name: 'Sales' }],
    filters: [{ name: 'Year' }]
  };

  return (
    <PivotViewComponent
      id="PivotView"
      dataSourceSettings={dataSourceSettings}
      allowMemberFilter={true}
      showFieldList={true}
      height={350}
    >
      <Inject services={[FieldList, MemberFilter]} />
    </PivotViewComponent>
  );
}

export default App;
```

## Member Filter Dialog

### Accessing the Filter Dialog

Member filter options appear in the Field List UI. Click the filter icon next to any field to open the member filter dialog:

```typescript
// Member filter dialog automatically shows when you click filter icon in Field List
// Features available in the dialog:
// - Search box to find specific members
// - Select All / Deselect All buttons
// - Checkbox list of all members
// - Apply and Cancel buttons
```

### Filter Dialog Configuration

```typescript
const fieldListSettings = {
  allowDragAndDrop: true,
  allowCalculatedField: true,
  showFilterIcon: true,       // Shows filter icon for each field
  allowSearching: true,       // Enable search in filter dialog
  maxNodeLimitInMemberEditor: 5000  // Max members shown in filter dialog
};

<PivotViewComponent
  fieldListSettings={fieldListSettings}
  allowMemberFilter={true}
  showFieldList={true}
/>
```

## Programmatic Member Filtering

### Using `drilledMembers`

Control member filtering programmatically using the `drilledMembers` property:

```typescript
import { PivotViewComponent } from '@syncfusion/ej2-react-pivotview';

function App() {
  const dataSourceSettings = {
    dataSource: data,
    rows: [{ name: 'Country' }],
    columns: [{ name: 'Product' }],
    values: [{ name: 'Sales' }],
    // Use drilledMembers to select specific members
    drilledMembers: [
      { 
        name: 'Country',
        items: ['USA', 'Canada', 'UK']  // Show only these countries
      },
      {
        name: 'Product',
        items: ['Laptops', 'Mobiles']    // Show only these products
      }
    ]
  };

  return (
    <PivotViewComponent
      dataSourceSettings={dataSourceSettings}
      allowMemberFilter={true}
    />
  );
}
```

## Advanced Member Filtering

### Filter with Hierarchies

For hierarchical data (multiple levels), specify filtered members at each level:

```typescript
const dataSourceSettings = {
  dataSource: hierarchicalData,
  rows: [
    { name: 'Country' },
    { name: 'State' },
    { name: 'City' }
  ],
  columns: [{ name: 'Year' }],
  values: [{ name: 'Sales' }],
  drilledMembers: [
    {
      name: 'Country',
      items: ['USA']  // Filter country first
    },
    {
      name: 'State',
      items: ['California', 'Texas']  // Then filter states within USA
    },
    {
      name: 'City',
      items: ['San Francisco', 'Los Angeles', 'Houston']  // Finally filter cities
    }
  ]
};

<PivotViewComponent
  dataSourceSettings={dataSourceSettings}
  allowMemberFilter={true}
/>
```

### Delimiter Support for Complex Members

When member names contain special characters or hierarchies, use delimiters:

```typescript
const dataSourceSettings = {
  dataSource: data,
  rows: [{ name: 'Country' }],
  columns: [{ name: 'Year' }],
  values: [{ name: 'Sales' }],
  drilledMembers: [
    {
      name: 'Country',
      items: ['USA > California', 'USA > Texas', 'Canada > Ontario'],
      delimiter: ' > '  // Specify delimiter for hierarchical paths
    }
  ]
};
```

## Dynamic Member Management

### Update Members Programmatically

```typescript
function UpdateMembers() {
  let pivotObj: PivotViewComponent;

  const applyFilter = (): void => {
    if (pivotObj && pivotObj.dataSourceSettings) {
      pivotObj.dataSourceSettings.drilledMembers = [
        {
          name: 'Country',
          items: ['USA', 'Canada']  // Update filter members
        }
      ];
      // Trigger update - pivot will re-render with new filter
    }
  };

  const clearFilters = (): void => {
    if (pivotObj && pivotObj.dataSourceSettings) {
      pivotObj.dataSourceSettings.drilledMembers = [];  // Clear all filters
    }
  };

  return (
    <div>
      <button onClick={applyFilter}>Apply Filter</button>
      <button onClick={clearFilters}>Clear Filters</button>
      <PivotViewComponent
        ref={(d: PivotViewComponent) => pivotObj = d}
        dataSourceSettings={dataSourceSettings}
        allowMemberFilter={true}
      />
    </div>
  );
}
```

### Event Handling for Member Changes

```typescript
const onMemberFiltering = (args: any): void => {
  console.log('Member filtering changed');
  console.log('Field:', args.fieldName);
  console.log('Members:', args.memberObjDetail);
};

const onFilterSuccess = (args: any): void => {
  console.log('Filter applied successfully');
  console.log('Current filtered members:', args.drilledMembers);
};

<PivotViewComponent
  dataSourceSettings={dataSourceSettings}
  actionBegin={onMemberFiltering}
  actionComplete={onFilterSuccess}
  allowMemberFilter={true}
/>
```

## Practical Examples

### Example 1: Simple Member Filter

```typescript
function SimpleMemberFilter() {
  const dataSourceSettings = {
    dataSource: [
      { Country: 'USA', Product: 'Laptops', Sales: 5000 },
      { Country: 'USA', Product: 'Mobiles', Sales: 3000 },
      { Country: 'Canada', Product: 'Laptops', Sales: 2500 },
      { Country: 'Canada', Product: 'Mobiles', Sales: 1500 },
      { Country: 'Mexico', Product: 'Laptops', Sales: 1200 },
      { Country: 'Mexico', Product: 'Mobiles', Sales: 800 }
    ],
    rows: [{ name: 'Country' }],
    columns: [{ name: 'Product' }],
    values: [{ name: 'Sales' }],
    // Filter to show only USA and Canada
    drilledMembers: [
      {
        name: 'Country',
        items: ['USA', 'Canada']
      }
    ]
  };

  return (
    <PivotViewComponent
      id="pivot-member-filter"
      dataSourceSettings={dataSourceSettings}
      allowMemberFilter={true}
      showFieldList={true}
      height={400}
    >
      <Inject services={[FieldList, MemberFilter]} />
    </PivotViewComponent>
  );
}

export default SimpleMemberFilter;
```

### Example 2: Interactive Member Selection

```typescript
function InteractiveMemberFilter() {
  const [selectedCountries, setSelectedCountries] = React.useState(['USA', 'Canada']);
  const [selectedProducts, setSelectedProducts] = React.useState(['Laptops', 'Mobiles']);

  const applyFilters = (): void => {
    // Update pivot with selected members
    const config = {
      dataSource: pivotData,
      rows: [{ name: 'Country' }],
      columns: [{ name: 'Product' }],
      values: [{ name: 'Sales' }],
      drilledMembers: [
        {
          name: 'Country',
          items: selectedCountries
        },
        {
          name: 'Product',
          items: selectedProducts
        }
      ]
    };
    // Update pivot with new config
  };

  return (
    <div>
      <div style={{ marginBottom: '20px' }}>
        <label>Countries:</label>
        <select multiple>
          <option>USA</option>
          <option>Canada</option>
          <option>Mexico</option>
          <option>Brazil</option>
        </select>
      </div>
      <div style={{ marginBottom: '20px' }}>
        <label>Products:</label>
        <select multiple>
          <option>Laptops</option>
          <option>Mobiles</option>
          <option>Tablets</option>
        </select>
      </div>
      <button onClick={applyFilters}>Apply Filters</button>
    </div>
  );
}
```

### Example 3: Multi-Level Hierarchical Filter

```typescript
function HierarchicalMemberFilter() {
  const dataSourceSettings = {
    dataSource: [
      { Country: 'USA', State: 'California', City: 'San Francisco', Sales: 5000 },
      { Country: 'USA', State: 'California', City: 'Los Angeles', Sales: 4500 },
      { Country: 'USA', State: 'Texas', City: 'Houston', Sales: 3000 },
      { Country: 'USA', State: 'Texas', City: 'Dallas', Sales: 2500 },
      { Country: 'Canada', State: 'Ontario', City: 'Toronto', Sales: 2000 },
      { Country: 'Canada', State: 'British Columbia', City: 'Vancouver', Sales: 1500 }
    ],
    rows: [
      { name: 'Country' },
      { name: 'State' },
      { name: 'City' }
    ],
    columns: [{ name: 'Year' }],
    values: [{ name: 'Sales' }],
    // Filter on multiple hierarchy levels
    drilledMembers: [
      {
        name: 'Country',
        items: ['USA', 'Canada']
      },
      {
        name: 'State',
        items: ['California', 'Texas', 'Ontario']
      },
      {
        name: 'City',
        items: ['San Francisco', 'Los Angeles', 'Houston', 'Toronto']
      }
    ]
  };

  return (
    <PivotViewComponent
      id="hierarchical-pivot"
      dataSourceSettings={dataSourceSettings}
      allowMemberFilter={true}
      showFieldList={true}
      expandAll={true}  // Show all levels expanded
      height={500}
    >
      <Inject services={[FieldList, MemberFilter]} />
    </PivotViewComponent>
  );
}
```

## Performance Considerations

1. **Large Member Lists**: For fields with 10,000+ members, enable search in filter dialog
2. **Field List Limits**: Use `maxNodeLimitInMemberEditor` to optimize UI performance
3. **Hierarchical Data**: Filter at appropriate hierarchy levels for better performance
4. **Lazy Loading**: Load member lists on-demand when needed

```typescript
// Performance optimization for large datasets
const fieldListSettings = {
  maxNodeLimitInMemberEditor: 1000,  // Limit displayed members
  allowSearching: true               // Enable search for finding members
};

<PivotViewComponent
  fieldListSettings={fieldListSettings}
  allowMemberFilter={true}
/>
```

## Best Practices

✅ **Do:**
- Use member filtering for focused analysis on specific data subsets
- Combine with other filters (label, value) for comprehensive filtering
- Provide UI controls for easy member selection
- Cache filter selections for quick re-application
- Use hierarchical filtering for multi-level data

❌ **Don't:**
- Filter out all members (results in empty pivot)
- Apply excessive member filters on large datasets without optimization
- Forget to update pivot when member selections change
- Use member filter for what should be label filtering

## Common Issues

**No filter icon visible?**
- Verify `allowMemberFilter` is `true`
- Check that `FieldList` or `GroupingBar` is injected
- Ensure `showFieldList={true}` or `showGroupingBar={true}`

**Member filter not applying?**
- Verify `drilledMembers` is correctly configured with matching member values
- Check console for JavaScript errors
- Ensure data source contains the specified members

**Performance slow with many members?**
- Use `maxNodeLimitInMemberEditor` to limit displayed members
- Enable search functionality for navigation
- Consider server-side filtering for very large datasets

## Related Features

- **Label Filtering**: Filter by field headers/text patterns
- **Value Filtering**: Filter by aggregated measure values
- **Field List**: UI for managing fields and filters
- **Grouping Bar**: Alternative UI for field management with filters
