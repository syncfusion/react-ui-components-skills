# Getting Started with PivotView

## Overview

The Syncfusion React PivotView component is a powerful tool for data analysis and reporting. This guide covers installation, setup, and creating your first pivot table.

## Installation

Install the required Syncfusion packages via npm:

```bash
npm install @syncfusion/ej2-react-pivotview
```

## CSS Imports and Theming

Themes for Syncfusion React components can be applied using CSS or SASS files from the [npm theme packages](https://ej2.syncfusion.com/react/documentation/appearance/theme#theme-packages), CDN, CRG, or [Theme Studio](https://ej2.syncfusion.com/react/documentation/appearance/theme-studio). For more information, see the [themes documentation](https://ej2.syncfusion.com/react/documentation/appearance/theme).

This guide uses the `Tailwind 3` theme as an example, sourced from the theme package. In this package, each component includes an `index.css` file that automatically loads all the required dependency styles. To install the [Tailwind 3](https://www.npmjs.com/package/@syncfusion/ej2-tailwind3-theme) theme package, use the following command:

```bash
npm install @syncfusion/ej2-tailwind3-theme --save
```

By default, Vite projects include a `index.css` file with default styles. These default styles may conflict with Syncfusion component styles. Clear all content from the `index.css` file to prevent style conflicts.

The required styles for the Pivot Table component are imported in the **src/App.css** file:

```css
/* App.css */
@import '../node_modules/@syncfusion/ej2-tailwind3-theme/styles/pivotview/index.css';
```

> **Note:** The `node_modules` path assumes Syncfusion packages are installed in your project's node_modules directory. If you're using a different path or bundler configuration, adjust the path accordingly.

Import the CSS file in your main App component:

```tsx
import './App.css';
```

## Basic Component Setup

### Step 1: Import Required Modules

```typescript
/* App.tsx */
import { IDataSet, PivotViewComponent } from '@syncfusion/ej2-react-pivotview';
import { DataSourceSettingsModel } from '@syncfusion/ej2-pivotview/src/model/datasourcesettings-model';
import * as React from 'react';
```

### Step 2: Create Data Source

```typescript
/* App.tsx */
export const pivotData: IDataSet[] = [
  { Country: 'USA', Region: 'North', Product: 'Laptop', Sales: 5000, Year: 2020, Quarter: 'Q1' },
  { Country: 'USA', Region: 'South', Product: 'Desktop', Sales: 3000, Year: 2020, Quarter: 'Q1' },
  { Country: 'Canada', Region: 'East', Product: 'Laptop', Sales: 2500, Year: 2020, Quarter: 'Q1' },
  { Country: 'Canada', Region: 'West', Product: 'Mobile', Sales: 1500, Year: 2020, Quarter: 'Q1' }
];
```

### Step 3: Configure Data Source Settings

```typescript
/* App.tsx */
const dataSourceSettings: DataSourceSettingsModel = {
  dataSource: pivotData as IDataSet[],
  rows: [{ name: 'Country' }],
  columns: [{ name: 'Product' }],
  values: [{ name: 'Sales', caption: 'Total Sales' }]
};
```

### Step 4: Create the Component

```typescript
/* App.tsx */
function App() {
  return (
    <PivotViewComponent
      id="pivotview"
      dataSourceSettings={dataSourceSettings}
      height={350}
    />
  );
}

export default App;
```

## Minimal PivotTable Component

```typescript
/* App.tsx */
import { PivotViewComponent, Inject, IDataSet } from '@syncfusion/ej2-react-pivotview';
import { DataSourceSettingsModel } from '@syncfusion/ej2-pivotview/src/model/datasourcesettings-model';
import React from 'react';

const sampleData: IDataSet[] = [
  { Country: 'USA', Product: 'Laptops', Sales: 5000 },
  { Country: 'USA', Product: 'Mobiles', Sales: 3000 },
  { Country: 'Canada', Product: 'Laptops', Sales: 2500 },
  { Country: 'Canada', Product: 'Mobiles', Sales: 1500 }
];

function PivotTableDemo() {
  const dataSourceSettings: DataSourceSettingsModel = {
    dataSource: sampleData as IDataSet[],
    rows: [{ name: 'Country' }],
    columns: [{ name: 'Product' }],
    values: [{ name: 'Sales', caption: 'Total Sales' }],
    expandAll: false,
    showGrandTotals: true
  };

  return (
    <PivotViewComponent
      id="pivotview"
      dataSourceSettings={dataSourceSettings}
      height={400}
    >
    </PivotViewComponent>
  );
}

export default PivotTableDemo;
```

## Module Injection

React PivotView features are modular and require injection to enable them. This reduces bundle size by loading only needed features.

### Inject Services

```tsx
import { Inject, GroupingBar, FieldList } from '@syncfusion/ej2-react-pivotview';

<PivotViewComponent dataSourceSettings={dataSourceSettings}>
  <Inject services={[GroupingBar, FieldList]} />
</PivotViewComponent>
```

### Common Modules

| Module | Purpose | Import |
|--------|---------|--------|
| `GroupingBar` | Enable grouping bar | `import { GroupingBar } from '@syncfusion/ej2-react-pivotview'` |
| `FieldList` | Enable field list | `import { FieldList } from '@syncfusion/ej2-react-pivotview'` |
| `ConditionalFormatting` | Enable conditional formatting | `import { ConditionalFormatting } from '@syncfusion/ej2-react-pivotview'` |
| `NumberFormatting` | Enable number formatting | `import { NumberFormatting } from '@syncfusion/ej2-react-pivotview'` |
| `CalculatedField` | Enable calculated field | `import { CalculatedField } from '@syncfusion/ej2-react-pivotview'` |
| `Toolbar` | Enable toolbar | `import { Toolbar } from '@syncfusion/ej2-react-pivotview'` |
| `ExcelExport` | Enable Excel export | `import { ExcelExport } from '@syncfusion/ej2-react-pivotview'` |
| `PDFExport` | Enable PDF export | `import { PDFExport } from '@syncfusion/ej2-react-pivotview'` |
| `VirtualScroll` | Enable virtual scrolling | `import { VirtualScroll } from '@syncfusion/ej2-react-pivotview'` |
| `Pager` | Enable paging | `import { Pager } from '@syncfusion/ej2-react-pivotview'` |
| `PivotChart` | Enable pivot chart visualization | `import { PivotChart } from '@syncfusion/ej2-react-pivotview'` |
| `Grouping` | Enable grouping | `import { Grouping } from '@syncfusion/ej2-react-pivotview'` |
| `DrillThrough` | Enable drill-through | `import { DrillThrough } from '@syncfusion/ej2-react-pivotview'` |

### Example with GroupingBar and FieldList

```tsx
import { PivotViewComponent, Inject, GroupingBar, FieldList, IDataSet } from '@syncfusion/ej2-react-pivotview';
import { DataSourceSettingsModel } from '@syncfusion/ej2-pivotview/src/model/datasourcesettings-model';
import React from 'react';

const sampleData: IDataSet[] = [
  { Country: 'USA', Region: 'North', Product: 'Laptops', Sales: 5000, Year: 2020 },
  { Country: 'USA', Region: 'South', Product: 'Desktops', Sales: 3000, Year: 2020 },
  { Country: 'Canada', Region: 'East', Product: 'Laptops', Sales: 2500, Year: 2020 },
  { Country: 'Canada', Region: 'West', Product: 'Mobiles', Sales: 1500, Year: 2020 }
];

function PivotTableWithModules() {
  const dataSourceSettings: DataSourceSettingsModel = {
    dataSource: sampleData as IDataSet[],
    rows: [{ name: 'Country' }],
    columns: [{ name: 'Product' }],
    values: [{ name: 'Sales', caption: 'Total Sales' }],
    expandAll: false
  };

  return (
    <PivotViewComponent
      id="pivotview"
      dataSourceSettings={dataSourceSettings}
      showGroupingBar={true}
      showFieldList={true}
      height={400}
    >
      <Inject services={[GroupingBar, FieldList]} />
    </PivotViewComponent>
  );
}

export default PivotTableWithModules;
```

## Troubleshooting

### Module not found errors
Ensure all required Syncfusion packages are installed. Run `npm list @syncfusion/ej2-react-pivotview` to verify.

### Styles not showing
Ensure the Tailwind 3 theme package is installed: `npm install @syncfusion/ej2-tailwind3-theme --save`, and that the theme CSS is imported in `App.css` (for Vite) or `globals.css` (for Next.js): `@import '../node_modules/@syncfusion/ej2-tailwind3-theme/styles/pivotview/index.css';`. Also confirm that any default `index.css` provided by the bundler has been cleared to prevent style conflicts.
