# PDF Export Reference - Syncfusion React Pivot Table

## Overview

The PDF export feature in the Syncfusion React Pivot Table allows you to export pivot table data to Adobe PDF format. This feature is essential for creating professional, portable reports that can be easily shared, archived, and printed. PDF export preserves formatting, styling, page layouts, and all visual elements defined in your pivot table, ensuring consistent document appearance across different devices and operating systems.

### Key Features
- **Professional Formatting**: Maintains fonts, colors, borders, and styling
- **Page Layout Control**: Configure page orientation, size, and margins
- **Multi-page Support**: Automatically handles large tables across pages
- **Headers and Footers**: Customizable page headers and footers with page numbers
- **Document Properties**: Set title, author, subject, and keywords
- **Watermarks**: Add text or image watermarks to pages
- **Bookmarks**: Generate PDF bookmarks for navigation
- **Print-Ready**: Optimized for high-quality printing

## Enabling PDF Export

### Basic Setup

To enable PDF export, set `allowPdfExport` to **true** and inject the `PdfExport` module:

```typescript
import { PivotViewComponent, PdfExport, Inject, Toolbar } from '@syncfusion/ej2-react-pivotview';
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
      allowPdfExport={true}
      toolbar={['Pdf Export']}
    >
      <Inject services={[PdfExport, Toolbar]} />
    </PivotViewComponent>
  );
}

export default App;
```

## Triggering PDF Export

### Method 1: Toolbar Button

Add 'Pdf Export' to the toolbar for automatic export button:

```typescript
<PivotViewComponent
  toolbar={['Pdf Export']}
  allowPdfExport={true}
>
  <Inject services={[PdfExport, Toolbar]} />
</PivotViewComponent>
```

### Method 2: Programmatic Export

Export on-demand using code:

```typescript
const exportToPdf = (): void => {
  if (pivotObj) {
    pivotObj.pdfExport();
  }
};

// In your UI
<button onClick={exportToPdf}>Export to PDF</button>
```

### Method 3: Custom Export with Options

```typescript
const exportWithOptions = (): void => {
  if (pivotObj) {
    pivotObj.pdfExport({
      fileName: 'sales-report.pdf'  // Custom file name
    });
  }
};
```

## PDF Export Configuration

### File Name and Properties

```typescript
const exportToPdf = (): void => {
  if (pivotObj) {
    pivotObj.pdfExport({
      fileName: 'Q1-Sales-Report.pdf',
      dataSourceSettings: pivotObj.dataSourceSettings,
      orientation: 'Landscape',     // Landscape or Portrait
      pageSize: 'A4',               // A4, Letter, Legal, etc.
      theme: 'Material'             // PDF theme
    });
  }
};
```

### Page Configuration

```typescript
const pdfExportSettings = {
  fileName: 'report.pdf',
  orientation: 'Landscape',         // Wide layout for better table fit
  pageSize: 'A4',                   // Standard page size
  pageMargins: {
    top: 0.5,      // inches
    bottom: 0.5,
    left: 0.5,
    right: 0.5
  },
  pageBreak: true,                  // Break large tables across pages
  fitToWidth: true                  // Scale table to fit page width
};

pivotObj?.pdfExport(pdfExportSettings);
```

## Customizing PDF Export

### PDF Export Events

Use events to customize export behavior:

```typescript
const onActionBegin = (args: PivotActionBeginEventArgs): void => {
  if (args.actionName === 'PDF Export') {
    console.log('Starting PDF export...');
    // Show loading indicator
  }
};

const onActionComplete = (args: PivotActionCompleteEventArgs): void => {
  if (args.actionName === 'PDF Export') {
    console.log('PDF export completed');
    // Hide loading indicator
  }
};

<PivotViewComponent
  actionBegin={onActionBegin}
  actionComplete={onActionComplete}
  allowPdfExport={true}
>
  <Inject services={[PdfExport]} />
</PivotViewComponent>
```

### Column Customization

```typescript
const onPdfExport = (args: PdfExportEventArgs): void => {
  // Include only specific columns
  args.columns = [
    { field: 'Country', headerText: 'Country', width: 100 },
    { field: 'Year', headerText: 'Year', width: 80 },
    { field: 'Amount', headerText: 'Total', format: 'C2', width: 120 }
  ];
};

<PivotViewComponent
  onPdfExport={onPdfExport}
  allowPdfExport={true}
>
  <Inject services={[PdfExport]} />
</PivotViewComponent>
```

## PDF Styling and Appearance

### Apply Themes

```typescript
const exportWithTheme = (): void => {
  if (pivotObj) {
    const theme = {
      name: 'Material',              // Material, Office, Bootstrap, etc.
      header: {
        fontSize: 14,
        bold: true,
        color: '#FFFFFF',
        backgroundColor: '#1F4E78'
      },
      record: {
        fontSize: 12,
        color: '#000000',
        backgroundColor: '#FFFFFF'
      }
    };

    pivotObj.pdfExport({
      fileName: 'themed-report.pdf',
      theme: theme
    });
  }
};
```

### Cell Styling

```typescript
const onPdfExport = (args: PdfExportEventArgs): void => {
  // Apply custom styling
  args.style = {
    headerFontSize: 14,
    headerFontName: 'Calibri',
    recordFontSize: 11,
    recordFontName: 'Calibri'
  };

  // Format specific columns
  args.columns.forEach(col => {
    if (col.field === 'Amount') {
      col.format = 'C2';              // Currency
      col.textAlign = 'Right';
      col.width = 120;
    } else if (col.field === 'Percentage') {
      col.format = 'P2';              // Percentage
      col.textAlign = 'Center';
    }
  });
};
```

## Advanced Export Features

### Headers and Footers

```typescript
const exportWithHeaderFooter = (): void => {
  if (pivotObj) {
    pivotObj.pdfExport({
      fileName: 'formal-report.pdf',
      orientation: 'Landscape',
      pageSize: 'A4',
      // Custom header
      header: {
        fromTop: 0.4,
        height: 60,
        contents: [
          {
            type: 'Text',
            value: 'Q1 2024 Sales Analysis',
            position: { x: 250, y: 50 },
            style: { fontSize: 20, bold: true }
          }
        ]
      },
      // Custom footer
      footer: {
        fromBottom: 160,
        height: 60,
        contents: [
          {
            type: 'PageNumber',
            value: 'Page # of $',
            position: { x: 250, y: 20 },
            style: { fontSize: 10 }
          },
          {
            type: 'Text',
            value: 'Confidential - For Internal Use Only',
            position: { x: 200, y: 5 },
            style: { fontSize: 9, italic: true }
          }
        ]
      }
    });
  }
};
```

### Document Properties

```typescript
const exportWithDocumentInfo = (): void => {
  if (pivotObj) {
    pivotObj.pdfExport({
      fileName: 'report.pdf',
      // Set document metadata
      documentInfo: {
        title: 'Sales Report Q1 2024',
        author: 'Finance Department',
        subject: 'Quarterly Sales Analysis',
        keywords: 'sales, q1, 2024, analysis',
        creator: 'Syncfusion Pivot Table'
      }
    });
  }
};
```

### Watermarks

```typescript
const exportWithWatermark = (): void => {
  if (pivotObj) {
    pivotObj.pdfExport({
      fileName: 'draft-report.pdf',
      watermark: {
        type: 'Text',
        value: 'DRAFT',
        angle: -45,
        opacity: 0.3,
        fontSize: 100,
        color: '#E0E0E0'
      }
    });
  }
};
```

### Multi-Page Control

```typescript
const exportLargeTable = (): void => {
  if (pivotObj) {
    pivotObj.pdfExport({
      fileName: 'large-report.pdf',
      orientation: 'Landscape',        // Wide orientation
      pageSize: 'A3',                  // Larger page size
      pageBreak: true,                 // Break across pages
      fitToPage: true,                 // Scale to fit
      repeatHeader: true               // Repeat headers on each page
    });
  }
};
```

## Export Events

### PDF Export Event

Customize during export:

```typescript
const onPdfExport = (args: PdfExportEventArgs): void => {
  // Modify export settings
  args.cancel = false;               // Cancel export if needed
  
  // Customize columns
  args.columns.forEach(col => {
    if (col.field === 'Amount') {
      col.format = 'C2';
    }
  });
};
```

### Action Events

```typescript
const onActionBegin = (args: PivotActionBeginEventArgs): void => {
  if (args.actionName === 'PDF Export') {
    // Show progress indicator
    console.log('PDF export started');
  }
};

const onActionComplete = (args: PivotActionCompleteEventArgs): void => {
  if (args.actionName === 'PDF Export') {
    // Hide progress, show success
    console.log('PDF export completed successfully');
  }
};

const onActionFailure = (args: PivotActionFailureEventArgs): void => {
  if (args.actionName === 'PDF Export') {
    console.error('PDF export failed:', args.error);
  }
};
```

## Practical Examples

### Example 1: Simple PDF Export

```typescript
import { PivotViewComponent, PdfExport, Toolbar, Inject } from '@syncfusion/ej2-react-pivotview';

function SimplePdfExport() {
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

  const exportPdf = (): void => {
    if (pivotObj) {
      pivotObj.pdfExport({
        fileName: 'sales-report.pdf'
      });
    }
  };

  return (
    <div>
      <button onClick={exportPdf}>Export to PDF</button>
      <PivotViewComponent
        ref={(d: PivotViewComponent) => pivotObj = d}
        dataSourceSettings={dataSourceSettings}
        allowPdfExport={true}
        height={350}
      >
        <Inject services={[PdfExport]} />
      </PivotViewComponent>
    </div>
  );
}

export default SimplePdfExport;
```

### Example 2: Professional Report with Headers and Watermark

```typescript
function ProfessionalPdfExport() {
  let pivotObj: PivotViewComponent;

  const exportProfessionalReport = (): void => {
    if (pivotObj) {
      pivotObj.pdfExport({
        fileName: 'sales-analysis-q1-2024.pdf',
        orientation: 'Landscape',
        pageSize: 'A4',
        pageMargins: { top: 0.75, bottom: 0.75, left: 0.5, right: 0.5 },
        header: {
          fromTop: 0.3,
          height: 50,
          contents: [
            {
              type: 'Text',
              value: 'Q1 2024 Sales Analysis Report',
              position: { x: 200, y: 30 },
              style: { fontSize: 18, bold: true, color: '#1F4E78' }
            }
          ]
        },
        footer: {
          fromBottom: 150,
          height: 40,
          contents: [
            {
              type: 'PageNumber',
              value: 'Page # of $',
              position: { x: 250, y: 25 },
              style: { fontSize: 10 }
            },
            {
              type: 'Text',
              value: 'Generated: ' + new Date().toLocaleDateString(),
              position: { x: 40, y: 25 },
              style: { fontSize: 9 }
            }
          ]
        }
      });
    }
  };

  return (
    <div>
      <button onClick={exportProfessionalReport}>Export Professional Report</button>
      <PivotViewComponent
        ref={(d: PivotViewComponent) => pivotObj = d}
        dataSourceSettings={dataSourceSettings}
        allowPdfExport={true}
        toolbar={['Pdf Export']}
        height={350}
      >
        <Inject services={[PdfExport, Toolbar]} />
      </PivotViewComponent>
    </div>
  );
}

export default ProfessionalPdfExport;
```

### Example 3: Customized Columns and Formatting

```typescript
function CustomPdfExport() {
  let pivotObj: PivotViewComponent;

  const onPdfExport = (args: PdfExportEventArgs): void => {
    // Customize columns and formatting
    args.columns = [
      { field: 'Country', headerText: 'Country', width: 90 },
      { field: 'Year', headerText: 'Year', width: 70, textAlign: 'Center' },
      { field: 'Amount', headerText: 'Amount', format: 'C2', width: 110, textAlign: 'Right' },
      { field: 'Sales', headerText: 'Units', format: 'N0', width: 90, textAlign: 'Right' }
    ];
  };

  const onActionComplete = (args: PivotActionCompleteEventArgs): void => {
    if (args.actionName === 'PDF Export') {
      console.log('PDF export successful');
    }
  };

  return (
    <PivotViewComponent
      ref={(d: PivotViewComponent) => pivotObj = d}
      dataSourceSettings={dataSourceSettings}
      allowPdfExport={true}
      toolbar={['Pdf Export']}
      onPdfExport={onPdfExport}
      onActionComplete={onActionComplete}
      height={350}
    >
      <Inject services={[PdfExport, Toolbar]} />
    </PivotViewComponent>
  );
}

export default CustomPdfExport;
```

## Page Size Options

| Size | Dimensions | Usage |
|------|-----------|-------|
| Letter | 8.5" x 11" | North America standard |
| A4 | 210mm x 297mm | International standard |
| A3 | 297mm x 420mm | Large tables, landscape |
| Legal | 8.5" x 14" | Legal documents |
| Tabloid | 11" x 17" | Large format |
| Ledger | 11" x 17" | Wide tables |

## Orientation Guidelines

**Portrait** (8.5" x 11"): Best for narrow tables, text-heavy reports
**Landscape** (11" x 8.5"): Best for wide pivot tables with many columns

## Performance Considerations

1. **Large Datasets**: Very large tables may take time to render
2. **File Size**: PDF files with images/formatting are larger than plain text
3. **Memory Usage**: Complex layouts require more memory
4. **Browser Performance**: Large exports may freeze UI briefly

## Best Practices

1. **File Naming**: Use descriptive, timestamped names
2. **Page Sizing**: Test page sizes for your typical data dimensions
3. **Margins**: Set appropriate margins for printing (0.5-0.75 inches)
4. **Headers/Footers**: Include company branding and page numbers
5. **Fonts**: Use standard fonts that display consistently
6. **Print Testing**: Always test PDF output on actual printer

## Troubleshooting

**Export button not appearing?**
- Verify `allowPdfExport` is `true`
- Check that `Toolbar` and `PdfExport` modules are injected
- Ensure toolbar array includes 'Pdf Export'

**PDF is blank?**
- Verify data source is populated
- Check pivot table renders correctly
- Verify no JavaScript errors in console

**Page layout issues?**
- Adjust pageSize and orientation for your data
- Modify margins if content is cut off
- Use fitToPage option to auto-scale

**Export takes too long?**
- Consider reducing data size
- Export specific columns only
- Use asynchronous export method
- Check for large complex formatting

## Related Features

- **Excel Export**: Export to Excel format
- **Print**: Print pivot table directly
- **Data Formatting**: Format numbers, dates, currencies
- **Grid Customization**: Customize column widths, alignment, styling
