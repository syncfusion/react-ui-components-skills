# Excel Export Reference - Syncfusion React Pivot Table

## Overview

The Excel export feature in the Syncfusion React Pivot Table allows you to export pivot table data, formatting, and styling to Microsoft Excel format (.xlsx). This feature is essential for creating professional reports, sharing data with stakeholders, and integrating with business intelligence tools. The Excel export preserves all applied filters, sorting, conditional formatting, number formats, and visual styling from your pivot table.

### Key Features
- **Complete Formatting**: Preserves colors, fonts, borders, and conditional formatting
- **Number Formats**: Maintains currency, percentage, and custom number formats
- **Hierarchical Structure**: Exports grouped data with proper hierarchy
- **Multiple Sheets**: Export different configurations to separate sheets
- **Themed Export**: Apply Excel themes to exported files
- **Dynamic Headers**: Customizable headers and footers
- **Performance**: Efficient export even for large datasets

## Enabling Excel Export

### Basic Setup

To enable Excel export, set `allowExcelExport` to **true** and inject the `ExcelExport` module:

```typescript
import { PivotViewComponent, ExcelExport, Inject, IDataSet } from '@syncfusion/ej2-react-pivotview';
import { DataSourceSettingsModel } from '@syncfusion/ej2-pivotview/src/model/datasourcesettings-model';
import * as React from 'react';
import { pivotData } from './datasource';

function App() {
  const dataSourceSettings: DataSourceSettingsModel = {
    columns: [{ name: 'Year' }, { name: 'Quarter' }],
    rows: [{ name: 'Country' }, { name: 'Products' }],
    values: [{ name: 'Sales' }, { name: 'Amount' }],
    dataSource: pivotData,
    formatSettings: [{ name: 'Amount', format: 'C2' }]
  };

  let pivotObj: PivotViewComponent;

  return (
    <PivotViewComponent
      ref={(d: PivotViewComponent) => pivotObj = d}
      id='PivotView'
      height={350}
      dataSourceSettings={dataSourceSettings}
      allowExcelExport={true}
      toolbar={['Excel Export']}
    >
      <Inject services={[ExcelExport, Toolbar]} />
    </PivotViewComponent>
  );
}

export default App;
```

## Triggering Excel Export

### Method 1: Toolbar Button

Add 'Excel Export' to the toolbar for automatic export button:

```typescript
<PivotViewComponent
  toolbar={['Excel Export']}
  allowExcelExport={true}
>
  <Inject services={[ExcelExport, Toolbar]} />
</PivotViewComponent>
```

### Method 2: Programmatic Export

Export on-demand from your code:

```typescript
const exportToExcel = (): void => {
  if (pivotObj) {
    pivotObj.excelExport();
  }
};

// In your UI
<button onClick={exportToExcel}>Export Pivot Table</button>
```

### Method 3: Custom Export with Options

```typescript
const exportWithOptions = (): void => {
  if (pivotObj) {
    pivotObj.excelExport({
      fileName: 'sales-report.xlsx'  // Custom file name
    });
  }
};
```

## Export Configuration

### File Name and Path

```typescript
const exportToExcel = (): void => {
  if (pivotObj) {
    pivotObj.excelExport({
      fileName: 'Q1-Sales-Report.xlsx',
      dataSourceSettings: pivotObj.dataSourceSettings
    });
  }
};
```

### Export Format Options

```typescript
const excelExportSettings = {
  fileName: 'report.xlsx',
  hierarchyExport: 'All',       // Export complete hierarchy
  expandedLevelColumns: [],      // Specific columns to expand
  theme: {
    name: 'Material',            // Excel theme: Material, Office, etc.
    colors: ['#1976D2', '#4CAF50', '#FF9800']
  }
};

pivotObj?.excelExport(excelExportSettings);
```

## Customizing Excel Export

### Excel Export Event

Use the `excelExportProperties` to customize export behavior:

```typescript
const onActionBegin = (args: PivotActionBeginEventArgs): void => {
  if (args.actionName === 'Excel Export') {
    // Customize export properties
    const excelExportProperties = {
      fileName: 'sales-analysis.xlsx',
      dataSourceSettings: args.dataSourceSettings,
      columns: [
        {
          field: 'Amount',
          headerText: 'Total Amount',
          format: 'C2',
          width: 150
        },
        {
          field: 'Sales',
          headerText: 'Units Sold',
          format: 'N0',
          width: 150
        }
      ]
    };
  }
};

<PivotViewComponent
  actionBegin={onActionBegin}
  allowExcelExport={true}
>
  <Inject services={[ExcelExport]} />
</PivotViewComponent>
```

### Applying Themes

```typescript
const exportWithTheme = (): void => {
  if (pivotObj) {
    const theme = {
      name: 'Office',              // Built-in theme
      header: {
        bold: true,
        color: '#FFFFFF',
        backColor: '#1F4E78'
      },
      record: {
        color: '#000000',
        backColor: '#FFFFFF'
      }
    };

    pivotObj.excelExport({
      fileName: 'themed-report.xlsx',
      theme: theme
    });
  }
};
```

## Column Customization

### Control Which Columns to Export

```typescript
const onExcelExport = (args: ExcelExportEventArgs): void => {
  // Include only specific columns
  args.columns = [
    { field: 'Country', headerText: 'Country', width: 100 },
    { field: 'Year', headerText: 'Year', width: 80 },
    { field: 'Amount', headerText: 'Total', format: 'C2', width: 120 }
  ];
};

<PivotViewComponent
  onExcelExport={onExcelExport}
  allowExcelExport={true}
>
  <Inject services={[ExcelExport]} />
</PivotViewComponent>
```

### Format Columns in Export

```typescript
const onExcelExport = (args: ExcelExportEventArgs): void => {
  args.columns.forEach(col => {
    if (col.field === 'Amount') {
      col.format = 'C2';                    // Currency format
      col.textAlign = 'Right';
      col.width = 150;
    } else if (col.field === 'Percentage') {
      col.format = 'P2';                    // Percentage format
      col.textAlign = 'Right';
    } else if (col.field === 'Date') {
      col.format = 'MMM dd, yyyy';          // Date format
    }
  });
};
```

## Styling and Formatting

### Cell Styling in Excel

```typescript
const onExcelExport = (args: ExcelExportEventArgs): void => {
  // Apply styles to header cells
  args.style = [
    {
      name: 'header',
      bold: true,
      color: '#FFFFFF',
      backColor: '#4472C4',
      borders: {
        left: { lineStyle: 'Thin' },
        right: { lineStyle: 'Thin' },
        top: { lineStyle: 'Thin' },
        bottom: { lineStyle: 'Thin' }
      }
    },
    {
      name: 'total',
      bold: true,
      color: '#000000',
      backColor: '#E8E8E8'
    }
  ];
};
```

### Conditional Cell Formatting

```typescript
const onExcelExport = (args: ExcelExportEventArgs): void => {
  // Format cells based on value
  args.cellformatting = (args: any) => {
    if (args.cellValue > 5000) {
      args.style.backColor = '#90EE90';  // Green for high values
      args.style.bold = true;
    } else if (args.cellValue < 1000) {
      args.style.backColor = '#FFB6C6';  // Red for low values
    }
  };
};
```

## Advanced Export Features

### Multiple Data Export

Export multiple views to separate sheets:

```typescript
const exportMultipleSheets = async (): Promise<void> => {
  if (pivotObj) {
    // First sheet - current state
    await pivotObj.excelExport({
      fileName: 'multi-report.xlsx',
      sheet: 'Current View'
    });
    
    // Second sheet - summarized data
    const summarySettings = { ...dataSourceSettings, expandAll: false };
    const tempPivot = new PivotViewComponent({
      dataSourceSettings: summarySettings
    });
    // Export second sheet...
  }
};
```

### Export with Headers and Footers

```typescript
const exportWithHeaderFooter = (): void => {
  if (pivotObj) {
    pivotObj.excelExport({
      fileName: 'formal-report.xlsx',
      dataSourceSettings: pivotObj.dataSourceSettings,
      theme: {
        name: 'Office'
      },
      headerRows: [
        'Q1 2024 Sales Analysis Report',
        'Generated: ' + new Date().toLocaleDateString()
      ],
      footerRows: [
        'Confidential - For Internal Use Only'
      ]
    });
  }
};
```

## Export Events

### Action Begin

Triggered when export begins:

```typescript
const onActionBegin = (args: PivotActionBeginEventArgs): void => {
  if (args.actionName === 'Excel Export') {
    console.log('Starting Excel export...');
    // Show loading indicator
  }
};
```

### Action Complete

Triggered when export completes:

```typescript
const onActionComplete = (args: PivotActionCompleteEventArgs): void => {
  if (args.actionName === 'Excel Export') {
    console.log('Excel export completed');
    // Hide loading indicator, show success message
  }
};
```

## Practical Examples

### Example 1: Simple Export with Formatting

```typescript
import { PivotViewComponent, ExcelExport, Toolbar, Inject } from '@syncfusion/ej2-react-pivotview';

function SimpleExcelExport() {
  const dataSourceSettings = {
    columns: [{ name: 'Year' }, { name: 'Quarter' }],
    rows: [{ name: 'Country' }, { name: 'Products' }],
    values: [
      { name: 'Sales', caption: 'Total Sales' },
      { name: 'Amount', caption: 'Amount' }
    ],
    dataSource: pivotData,
    formatSettings: [
      { name: 'Amount', format: 'C2' },
      { name: 'Sales', format: 'N0' }
    ]
  };

  let pivotObj: PivotViewComponent;

  const exportExcel = (): void => {
    if (pivotObj) {
      pivotObj.excelExport({
        fileName: 'sales-report.xlsx'
      });
    }
  };

  return (
    <div>
      <button onClick={exportExcel}>Export to Excel</button>
      <PivotViewComponent
        ref={(d: PivotViewComponent) => pivotObj = d}
        dataSourceSettings={dataSourceSettings}
        allowExcelExport={true}
        height={350}
      >
        <Inject services={[ExcelExport]} />
      </PivotViewComponent>
    </div>
  );
}

export default SimpleExcelExport;
```

### Example 2: Customized Export with Styling

```typescript
function CustomizedExcelExport() {
  let pivotObj: PivotViewComponent;

  const onExcelExport = (args: ExcelExportEventArgs): void => {
    // Customize export columns
    args.columns = [
      { field: 'Country', headerText: 'Country', width: 120 },
      { field: 'Products', headerText: 'Product Line', width: 120 },
      { field: 'Amount', headerText: 'Total Revenue', format: 'C2', width: 150, textAlign: 'Right' }
    ];
  };

  const onActionComplete = (args: PivotActionCompleteEventArgs): void => {
    if (args.actionName === 'Excel Export') {
      console.log('Report exported successfully');
    }
  };

  return (
    <PivotViewComponent
      ref={(d: PivotViewComponent) => pivotObj = d}
      dataSourceSettings={dataSourceSettings}
      allowExcelExport={true}
      toolbar={['Excel Export']}
      onExcelExport={onExcelExport}
      onActionComplete={onActionComplete}
      height={350}
    >
      <Inject services={[ExcelExport, Toolbar]} />
    </PivotViewComponent>
  );
}

export default CustomizedExcelExport;
```

## Performance Considerations

1. **Large Datasets**: Exporting very large pivot tables may take time
2. **File Size**: Format-rich exports (with styling) create larger files
3. **Memory Usage**: Export is processed in-browser for relational data
4. **Browser Compatibility**: Requires modern browser with Blob support

## Best Practices

1. **File Naming**: Use descriptive, timestamped file names
2. **Number Formats**: Always apply appropriate formats before export
3. **Conditional Formatting**: Use sparingly to keep file sizes reasonable
4. **User Feedback**: Show progress/confirmation during export
5. **Error Handling**: Catch and handle export errors gracefully

## Troubleshooting

**Export button not appearing?**
- Verify `allowExcelExport` is `true`
- Check that `Toolbar` and `ExcelExport` modules are injected
- Ensure toolbar array includes 'Excel Export'

**Exported file is empty?**
- Verify data source is properly configured
- Check that pivot table renders correctly before export
- Verify no JavaScript errors in console

**Formatting not preserved?**
- Check `formatSettings` is configured
- Verify Excel theme is properly applied
- Ensure conditional formatting rules are valid

**File size too large?**
- Reduce number of columns/rows
- Remove unnecessary conditional formatting
- Use simpler theme or no theme

## Related Features

- **PDF Export**: Export to PDF format
- **Print**: Print pivot table directly
- **Data Formatting**: Format numbers, dates, currencies
- **Conditional Formatting**: Apply color-based formatting rules
