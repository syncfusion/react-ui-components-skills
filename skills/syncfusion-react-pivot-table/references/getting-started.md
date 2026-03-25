# Getting Started with PivotView

## Overview

The Syncfusion React PivotView component is a powerful tool for data analysis and reporting. This guide covers installation, setup, and creating your first pivot table.

## Installation

Install the required Syncfusion packages via npm:

```bash
npm install @syncfusion/ej2-react-pivotview
npm install @syncfusion/ej2-react-base
npm install @syncfusion/ej2-base
```

## Basic Component Setup

### Step 1: Import Required Modules

```typescript
import { PivotViewComponent } from '@syncfusion/ej2-react-pivotview';
import '@syncfusion/ej2-react-pivotview/styles/material.css';
```

### Step 2: Create Data Source

```typescript
export const pivotData = [
  { Country: 'USA', Region: 'North', Product: 'Laptop', Sales: 5000, Year: 2020, Quarter: 'Q1' },
  { Country: 'USA', Region: 'South', Product: 'Desktop', Sales: 3000, Year: 2020, Quarter: 'Q1' },
  { Country: 'Canada', Region: 'East', Product: 'Laptop', Sales: 2500, Year: 2020, Quarter: 'Q1' },
  { Country: 'Canada', Region: 'West', Product: 'Mobile', Sales: 1500, Year: 2020, Quarter: 'Q1' }
];
```

### Step 3: Configure Data Source Settings

```typescript
const dataSourceSettings = {
  dataSource: pivotData,
  rows: [{ name: 'Country' }],
  columns: [{ name: 'Product' }],
  values: [{ name: 'Sales', caption: 'Total Sales' }]
};
```

### Step 4: Create the Component

```typescript
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

## CSS Imports and Theming

Include the appropriate theme stylesheet:

```typescript
// Material theme
import '@syncfusion/ej2-react-pivotview/styles/material.css';

// Bootstrap theme
import '@syncfusion/ej2-react-pivotview/styles/bootstrap.css';

// Fabric theme
import '@syncfusion/ej2-react-pivotview/styles/fabric.css';

// High contrast theme
import '@syncfusion/ej2-react-pivotview/styles/highcontrast.css';
```

## NextJS Integration

### Installation

```bash
npm install @syncfusion/ej2-react-pivotview
```

### Dynamic Import

Use dynamic imports for server-side rendering compatibility:

```typescript
import dynamic from 'next/dynamic';
import { Suspense } from 'react';

const DynamicPivotView = dynamic(() => import('./PivotViewComponent'), { 
  ssr: false,
  loading: () => <p>Loading Pivot Table...</p>
});

export default function Page() {
  return <DynamicPivotView />;
}
```

### Pages Router Example

```typescript
// pages/pivot.tsx
import { PivotViewComponent, Inject, GroupingBar } from '@syncfusion/ej2-react-pivotview';
import '@syncfusion/ej2-react-pivotview/styles/material.css';

const pivotData = [...];

export default function PivotPage() {
  return (
    <PivotViewComponent
      dataSourceSettings={{ dataSource: pivotData, rows: [{ name: 'Country' }], columns: [{ name: 'Product' }], values: [{ name: 'Sales' }] }}
      showGroupingBar={true}
      height={400}
    >
      <Inject services={[GroupingBar]} />
    </PivotViewComponent>
  );
}
```

## Complete First Example

```typescript
import { PivotViewComponent, Inject, GroupingBar, FieldList } from '@syncfusion/ej2-react-pivotview';
import '@syncfusion/ej2-react-pivotview/styles/material.css';
import React from 'react';

const sampleData = [
  { Country: 'USA', Product: 'Laptops', Sales: 5000 },
  { Country: 'USA', Product: 'Mobiles', Sales: 3000 },
  { Country: 'Canada', Product: 'Laptops', Sales: 2500 },
  { Country: 'Canada', Product: 'Mobiles', Sales: 1500 }
];

function PivotTableDemo() {
  const dataSourceSettings = {
    dataSource: sampleData,
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
      showGroupingBar={true}
      showFieldList={true}
      height={400}
    >
      <Inject services={[GroupingBar, FieldList]} />
    </PivotViewComponent>
  );
}

export default PivotTableDemo;
```

## Troubleshooting

### Module not found errors
Ensure all required Syncfusion packages are installed. Run `npm list @syncfusion/ej2-react-pivotview` to verify.

### Styles not showing
Import the CSS file before component usage: `import '@syncfusion/ej2-react-pivotview/styles/material.css';`

### NextJS SSR issues
Use dynamic imports with `ssr: false` for PivotView components to avoid server-side rendering issues.
