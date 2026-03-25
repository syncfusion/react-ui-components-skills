# Getting Started with React Accumulation Charts

This guide covers the installation, setup, and basic implementation of Syncfusion React Accumulation Charts.

## Installation

Install the Syncfusion React Charts package via npm:

```bash
npm install @syncfusion/ej2-react-charts --save
```

This package includes all chart types, including accumulation charts (Pie, Doughnut, Funnel, Pyramid).

## Dependencies

The accumulation chart component depends on these Syncfusion packages (installed automatically):

```
@syncfusion/ej2-react-charts
  ├── @syncfusion/ej2-data
  ├── @syncfusion/ej2-pdf-export
  ├── @syncfusion/ej2-file-utils
  ├── @syncfusion/ej2-compression
  ├── @syncfusion/ej2-react-popups
  ├── @syncfusion/ej2-react-buttons
  └── @syncfusion/ej2-react-base
```

## Project Setup

### Using Vite (Recommended)

Create a new React project with Vite:

```bash
npm create vite@latest my-chart-app -- --template react
cd my-chart-app
npm install
npm install @syncfusion/ej2-react-charts --save
```

### Using Create React App

```bash
npx create-react-app my-chart-app
cd my-chart-app
npm install @syncfusion/ej2-react-charts --save
```

## Importing Components

Import the necessary components and modules:

```tsx
import { 
  AccumulationChartComponent,
  AccumulationSeriesCollectionDirective,
  AccumulationSeriesDirective,
  Inject,
  PieSeries
} from '@syncfusion/ej2-react-charts';
```

## Basic AccumulationChartComponent Usage

The `AccumulationChartComponent` is the root container for all accumulation charts:

```tsx
import React from 'react';
import { AccumulationChartComponent } from '@syncfusion/ej2-react-charts';

function App() {
  return (
    <AccumulationChartComponent id='chart'>
      {/* Series configuration goes here */}
    </AccumulationChartComponent>
  );
}

export default App;
```

## First Pie Chart with Data Binding

Here's a complete example creating a basic pie chart:

```tsx
import React from 'react';
import { 
  AccumulationChartComponent,
  AccumulationSeriesCollectionDirective,
  AccumulationSeriesDirective,
  Inject,
  PieSeries
} from '@syncfusion/ej2-react-charts';


function App() {
  // Sample data
  const data = [
    { month: 'Jan', sales: 35 },
    { month: 'Feb', sales: 28 },
    { month: 'Mar', sales: 34 },
    { month: 'Apr', sales: 32 },
    { month: 'May', sales: 40 }
  ];

  return (
    <AccumulationChartComponent 
      id='pie-chart'
      title='Monthly Sales Distribution'
    >
      <Inject services={[PieSeries]} />
      <AccumulationSeriesCollectionDirective>
        <AccumulationSeriesDirective
          dataSource={data}
          xName='month'
          yName='sales'
          radius='90%'
        />
      </AccumulationSeriesCollectionDirective>
    </AccumulationChartComponent>
  );
}

export default App;
```

## AccumulationSeriesCollectionDirective Pattern

The series configuration uses a directive pattern:

```tsx
<AccumulationSeriesCollectionDirective>
  <AccumulationSeriesDirective
    dataSource={yourData}    // Array of objects
    xName='categoryField'    // Field name for categories
    yName='valueField'       // Field name for values
    type='Pie'              // Chart type (default)
  />
</AccumulationSeriesCollectionDirective>
```

**Key Points:**
- `dataSource`: Array of data objects
- `xName`: Property name for category/label (x-axis)
- `yName`: Property name for numeric value (y-axis)
- Each series represents one accumulation chart

## Data Structure Requirements

Your data should be an array of objects with at least two properties:

```tsx
const data = [
  { category: 'Product A', value: 45 },
  { category: 'Product B', value: 30 },
  { category: 'Product C', value: 25 }
];

// Use with:
<AccumulationSeriesDirective
  dataSource={data}
  xName='category'  // Maps to 'category' property
  yName='value'     // Maps to 'value' property
/>
```

## Module Injection

Syncfusion uses dependency injection for optional features. Always inject required modules:

```tsx
<Inject services={[PieSeries]} />
```

**Common modules:**
- `PieSeries`: Required for Pie/Doughnut charts
- `FunnelSeries`: Required for Funnel charts
- `PyramidSeries`: Required for Pyramid charts
- `AccumulationDataLabel`: For data labels
- `AccumulationLegend`: For legend
- `AccumulationTooltip`: For tooltips

## Complete Starter Template

```tsx
import React from 'react';
import { 
  AccumulationChartComponent,
  AccumulationSeriesCollectionDirective,
  AccumulationSeriesDirective,
  Inject,
  AccumulationLegend,
  AccumulationDataLabel,
  AccumulationTooltip,
  PieSeries
} from '@syncfusion/ej2-react-charts';


function App() {
  const data = [
    { x: 'Chrome', y: 61.3, text: 'Chrome: 61.3%' },
    { x: 'Safari', y: 24.6, text: 'Safari: 24.6%' },
    { x: 'Edge', y: 5.0, text: 'Edge: 5.0%' },
    { x: 'Firefox', y: 4.8, text: 'Firefox: 4.8%' },
    { x: 'Others', y: 4.3, text: 'Others: 4.3%' }
  ];

  return (
    <div style={{ padding: '20px' }}>
      <AccumulationChartComponent 
        id='browser-chart'
        title='Browser Market Share 2024'
        legendSettings={{ visible: true }}
        tooltip={{ enable: true }}
      >
        <Inject services={[
          PieSeries, 
          AccumulationLegend, 
          AccumulationDataLabel, 
          AccumulationTooltip
        ]} />
        <AccumulationSeriesCollectionDirective>
          <AccumulationSeriesDirective
            dataSource={data}
            xName='x'
            yName='y'
            radius='80%'
            dataLabel={{
              visible: true,
              name: 'text',
              position: 'Outside'
            }}
          />
        </AccumulationSeriesCollectionDirective>
      </AccumulationChartComponent>
    </div>
  );
}

export default App;
```

## TypeScript Support

For TypeScript projects, add type definitions:

```tsx
import React from 'react';
import { 
  AccumulationChartComponent,
  AccumulationSeriesCollectionDirective,
  AccumulationSeriesDirective,
  Inject,
  PieSeries
} from '@syncfusion/ej2-react-charts';

interface ChartData {
  category: string;
  value: number;
}

function App(): JSX.Element {
  const data: ChartData[] = [
    { category: 'A', value: 25 },
    { category: 'B', value: 35 },
    { category: 'C', value: 40 }
  ];

  return (
    <AccumulationChartComponent id='chart'>
      <Inject services={[PieSeries]} />
      <AccumulationSeriesCollectionDirective>
        <AccumulationSeriesDirective
          dataSource={data}
          xName='category'
          yName='value'
        />
      </AccumulationSeriesCollectionDirective>
    </AccumulationChartComponent>
  );
}

export default App;
```

## Common Setup Issues

### Issue: Chart not rendering
**Solution:** Ensure you've:
1. Imported CSS theme
2. Injected required modules
3. Provided valid data with xName/yName fields
4. Set a container height if parent has no height

### Issue: Module not found error
**Solution:** Install the package: `npm install @syncfusion/ej2-react-charts`

### Issue: Types not found (TypeScript)
**Solution:** Types are included in the package. Ensure proper import paths.

## Next Steps

Now that you have a basic chart running, you can:
- Customize chart types (Pie, Doughnut, Funnel, Pyramid)
- Add data labels for better readability
- Enable legends for categorical information
- Add tooltips for interactive data exploration
- Customize colors and styling
- Handle user interactions with events
