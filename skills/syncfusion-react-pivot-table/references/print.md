# Print Reference - Syncfusion React Pivot Table

## Overview

The printing feature in the Syncfusion React Pivot Table enables users to generate print-friendly output of their pivot table and pivot charts. The component captures the current state of the pivot table (including all applied filters, sorting, and formatting) and provides flexible printing options. Printing can be triggered through the toolbar or programmatically from your application code.

### Key Features
- **Pivot Table Printing**: Print the complete pivot table with all configurations
- **Pivot Chart Printing**: Print visualizations alongside or separately from data
- **State Preservation**: Maintains all filters, sorting, and formatting during print
- **Formatting Support**: Colors, fonts, and styling are preserved in print output
- **Flexible Triggers**: Both toolbar button and programmatic API support

## Printing Pivot Table

### How to Print

#### Method 1: Using the Grid Print Method

Access the underlying Grid component through the PivotView reference and call its print method:

```typescript
import { PivotViewComponent, Inject, Toolbar } from '@syncfusion/ej2-react-pivotview';
import { DataSourceSettingsModel } from '@syncfusion/ej2-pivotview/src/model/datasourcesettings-model';
import * as React from 'react';
import { pivotData } from './datasource';

function App() {
  const dataSourceSettings: DataSourceSettingsModel = {
    columns: [{ name: 'Year' }, { name: 'Quarter' }],
    rows: [{ name: 'Country' }, { name: 'Products' }],
    values: [{ name: 'Sales' }, { name: 'Amount' }],
    dataSource: pivotData
  };

  let pivotObj: PivotViewComponent;

  const printPivot = () => {
    if (pivotObj && pivotObj.grid) {
      pivotObj.grid.print();
    }
  };

  return (
    <>
      <button onClick={printPivot}>Print Pivot Table</button>
      <PivotViewComponent
        ref={(d: PivotViewComponent) => pivotObj = d}
        id='PivotView'
        height={350}
        dataSourceSettings={dataSourceSettings}
      >
        <Inject services={[Toolbar]} />
      </PivotViewComponent>
    </>
  );
}

export default App;
```

#### Method 2: Using Print Method with Options

The print method accepts parameters for customization:

```typescript
const printWithOptions = () => {
  if (pivotObj && pivotObj.grid) {
    pivotObj.grid.print({
      type: 'CurrentPage',  // Print current page or all rows
      hierarchy: false      // Include hierarchy indicators
    });
  }
};
```

## Printing Pivot Charts

### Basic Chart Printing

To print the pivot chart visualization, access the Chart component and call its print method:

```typescript
import { PivotViewComponent, PivotChart, Inject } from '@syncfusion/ej2-react-pivotview';
import { DataSourceSettingsModel } from '@syncfusion/ej2-pivotview/src/model/datasourcesettings-model';
import * as React from 'react';
import { pivotData } from './datasource';

function App() {
  const dataSourceSettings: DataSourceSettingsModel = {
    columns: [{ name: 'Year' }, { name: 'Quarter' }],
    rows: [{ name: 'Country' }, { name: 'Products' }],
    values: [{ name: 'Sales' }],
    dataSource: pivotData
  };

  let pivotObj: PivotViewComponent;

  const printChart = () => {
    if (pivotObj && pivotObj.chart) {
      pivotObj.chart.print();
    }
  };

  return (
    <>
      <button onClick={printChart}>Print Chart</button>
      <PivotViewComponent
        ref={(d: PivotViewComponent) => pivotObj = d}
        id='PivotView'
        height={350}
        dataSourceSettings={dataSourceSettings}
        displayOption={{ view: 'Chart' }}
      >
        <Inject services={[PivotChart]} />
      </PivotViewComponent>
    </>
  );
}

export default App;
```

### Printing Both Table and Chart

To print both pivot table and chart together:

```typescript
const printBoth = () => {
  if (pivotObj && pivotObj.grid) {
    pivotObj.grid.print();
  }
  setTimeout(() => {
    if (pivotObj && pivotObj.chart) {
      pivotObj.chart.print();
    }
  }, 1000); // Delay chart print to ensure grid print dialog closes
};
```

## Print Configuration

### Display Option Settings

Control what is shown on the page when printing:

```typescript
import { DisplayOption } from '@syncfusion/ej2-react-pivotview';

// Display only table
const displayOption: DisplayOption = {
  view: 'Table'
};

// Display only chart
const displayOption: DisplayOption = {
  view: 'Chart'
};

// Display both table and chart
const displayOption: DisplayOption = {
  view: 'Both'
};

<PivotViewComponent
  displayOption={displayOption}
  dataSourceSettings={dataSourceSettings}
>
  <Inject services={[PivotChart]} />
</PivotViewComponent>
```

## Print Event Handler

Handle the print event to customize printing behavior:

```typescript
const onActionBegin = (args: PivotActionBeginEventArgs): void => {
  if (args.actionName === 'Print Grid' || args.actionName === 'Print Chart') {
    console.log('Print initiated');
    // Perform any pre-print operations
  }
};

const onActionComplete = (args: PivotActionCompleteEventArgs): void => {
  if (args.actionName === 'Grid Printing Completed' || args.actionName === 'Chart Printing Completed') {
    console.log('Print completed');
    // Perform any post-print operations
  }
};

<PivotViewComponent
  actionBegin={onActionBegin}
  actionComplete={onActionComplete}
  dataSourceSettings={dataSourceSettings}
>
  <Inject services={[PivotChart]} />
</PivotViewComponent>
```

## Print with Toolbar

Enable print functionality through the toolbar:

```typescript
import { PivotViewComponent, Inject, Toolbar, PivotChart } from '@syncfusion/ej2-react-pivotview';

const App = () => {
  return (
    <PivotViewComponent
      id='PivotView'
      height={350}
      dataSourceSettings={dataSourceSettings}
      toolbar={['Print']}
      displayOption={{ view: 'Both' }}
    >
      <Inject services={[Toolbar, PivotChart]} />
    </PivotViewComponent>
  );
};
```

**Available Toolbar Options:**
- `'Print'` - Print the current view (table or chart)
- Combine with other options: `['Print', 'Export', 'Excel Export', 'PDF Export']`

## Print Styles and Formatting

The pivot table preserves all formatting during print:

```typescript
import { DataSourceSettingsModel, IDataSet } from '@syncfusion/ej2-react-pivotview';

const dataSourceSettings: DataSourceSettingsModel = {
  columns: [{ name: 'Year' }],
  rows: [{ name: 'Country' }],
  values: [{ name: 'Amount', caption: 'Sales Amount' }],
  dataSource: pivotData,
  formatSettings: [
    {
      name: 'Amount',
      format: 'C2',  // Currency format preserved in print
      useGrouping: true
    }
  ],
  conditionalFormatSettings: [
    {
      measure: 'Amount',
      condition: 'GreaterThan',
      value1: 5000,
      style: {
        backgroundColor: '#80FF80',
        color: '#000000',
        fontWeight: 'bold'
      }
    }
  ]
};

// Formatting (colors, fonts) is maintained in print output
<PivotViewComponent
  dataSourceSettings={dataSourceSettings}
  // ... other props
>
  <Inject services={[Toolbar, PivotChart]} />
</PivotViewComponent>
```

## Print Page Settings

Configure page orientation and sizing for print:

```typescript
const printPageSettings = () => {
  if (pivotObj && pivotObj.grid) {
    // Access grid print method with options
    pivotObj.grid.print({
      type: 'AllPages'  // Print all pages or specific range
    });
  }
};
```

**Print Page Options:**
- `'CurrentPage'` - Print only the visible page
- `'AllPages'` - Print all pages in the dataset

## Practical Examples

### Example 1: Simple Print Button

```typescript
import { PivotViewComponent, Inject } from '@syncfusion/ej2-react-pivotview';
import { DataSourceSettingsModel } from '@syncfusion/ej2-pivotview/src/model/datasourcesettings-model';
import * as React from 'react';

function SimplePrint() {
  const dataSourceSettings: DataSourceSettingsModel = {
    columns: [{ name: 'Year' }],
    rows: [{ name: 'Country' }, { name: 'Products' }],
    values: [{ name: 'Sales' }],
    dataSource: pivotData
  };

  let pivotObj: PivotViewComponent;

  return (
    <div>
      <button onClick={() => pivotObj?.grid?.print()}>Print Table</button>
      <PivotViewComponent
        ref={(d: PivotViewComponent) => pivotObj = d}
        dataSourceSettings={dataSourceSettings}
        height={350}
      />
    </div>
  );
}

export default SimplePrint;
```

### Example 2: Print with Toolbar Integration

```typescript
import { PivotViewComponent, Inject, Toolbar, PivotChart } from '@syncfusion/ej2-react-pivotview';

function PrintWithToolbar() {
  const dataSourceSettings: DataSourceSettingsModel = {
    columns: [{ name: 'Year' }],
    rows: [{ name: 'Country' }],
    values: [{ name: 'Sales' }, { name: 'Amount' }],
    dataSource: pivotData
  };

  return (
    <PivotViewComponent
      id='PivotView'
      dataSourceSettings={dataSourceSettings}
      height={350}
      toolbar={['Print', 'Export']}
      displayOption={{ view: 'Both' }}
    >
      <Inject services={[Toolbar, PivotChart]} />
    </PivotViewComponent>
  );
}

export default PrintWithToolbar;
```

### Example 3: Custom Print Handler

```typescript
function CustomPrint() {
  const dataSourceSettings: DataSourceSettingsModel = {
    columns: [{ name: 'Year' }],
    rows: [{ name: 'Country' }],
    values: [{ name: 'Sales' }, { name: 'Amount' }],
    dataSource: pivotData,
    formatSettings: [{ name: 'Amount', format: 'C2' }]
  };

  let pivotObj: PivotViewComponent;

  const customPrint = () => {
    // Pre-print operations
    console.log('Preparing to print...');
    
    // Trigger print
    if (pivotObj?.grid) {
      pivotObj.grid.print();
    }
  };

  const onActionComplete = (args: PivotActionCompleteEventArgs) => {
    if (args.actionName === 'Grid Printing Completed') {
      console.log('Print completed successfully');
    }
  };

  return (
    <div>
      <button onClick={customPrint}>Print Report</button>
      <PivotViewComponent
        ref={(d: PivotViewComponent) => pivotObj = d}
        dataSourceSettings={dataSourceSettings}
        height={350}
        actionComplete={onActionComplete}
      />
    </div>
  );
}

export default CustomPrint;
```

## Print Considerations

1. **Browser Compatibility**: Works on all modern browsers with print dialog support
2. **Large Datasets**: For very large pivot tables, consider using page-wise printing
3. **Chart Resolution**: Ensure chart is rendered before printing for best quality
4. **Styling**: Custom CSS styles may not transfer to print output
5. **Headers/Footers**: Browser print dialog allows adding headers and footers

## Troubleshooting

**Print dialog not appearing?**
- Check if Grid module is properly injected
- Verify pivotObj reference is correctly set
- Check browser console for JavaScript errors
- Ensure `height` property is set on PivotViewComponent

**Chart not printing?**
- Verify `PivotChart` module is injected
- Check that `displayOption.view` includes 'Chart'
- Ensure chart is fully rendered before printing
- Check browser print settings

**Formatting not preserved?**
- Verify `formatSettings` is correctly configured
- Check `conditionalFormatSettings` for proper column references
- Some browser-specific CSS may not print correctly
- Test print preview before final printing

## Related Features

- **Export to Excel**: Export data in spreadsheet format
- **PDF Export**: Generate professional PDF reports  
- **Toolbar**: Provides print button in UI
- **Display Options**: Control what views are available for printing
