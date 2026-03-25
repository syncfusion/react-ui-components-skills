# Getting Started with Syncfusion React Charts

## Installation and Package Setup

### Install Required Packages

```bash
npm install @syncfusion/ej2-react-charts @syncfusion/ej2-base
```

### Import Required Modules

```jsx
import { ChartComponent, SeriesCollectionDirective, SeriesDirective, Inject, LineSeries, Category, Legend, Tooltip } from '@syncfusion/ej2-react-charts';
```

The `Inject` component is crucial—it registers the services (like `LineSeries`, `Category`, `Legend`, `Tooltip`) that your chart needs. Without injecting services, those features won't work.

**Available themes:**
- `Material` - Material Design
- `Bootstrap` - Bootstrap theme
- `Bootstrap4` - Bootstrap 4 theme
- `Tailwind` - Tailwind CSS theme

Choose the theme that matches your application's design system. You can only have one theme imported at a time.

---

## Creating Your First Chart

### Minimal Chart Example

```jsx
import React from 'react';
import { ChartComponent, SeriesCollectionDirective, SeriesDirective, Inject, LineSeries, Category } from '@syncfusion/ej2-react-charts';

export default function MyChart() {
  const chartData = [
    { month: 'Jan', sales: 35 },
    { month: 'Feb', sales: 28 },
    { month: 'Mar', sales: 34 },
    { month: 'Apr', sales: 32 },
    { month: 'May', sales: 40 }
  ];

  return (
    <ChartComponent>
      <Inject services={[LineSeries, Category]} />
      <SeriesCollectionDirective>
        <SeriesDirective
          dataSource={chartData}
          xName='month'
          yName='sales'
          type='Line'
        />
      </SeriesCollectionDirective>
    </ChartComponent>
  );
}
```

**What this does:**
- `ChartComponent`: Main chart container
- `SeriesCollectionDirective`: Container for all chart series
- `SeriesDirective`: Single data series with X/Y mapping
- `Inject`: Registers LineSeries and Category axis services

### Chart with Title and Axes Labels

```jsx
<ChartComponent
  id='sales-chart'
  primaryXAxis={{ valueType: 'Category', labelFormat: '{value}' }}
  primaryYAxis={{ labelFormat: '${value}K' }}
  title='Monthly Sales'
  subTitle='Q1 2024 Performance'
>
  <Inject services={[LineSeries, Category]} />
  <SeriesCollectionDirective>
    <SeriesDirective
      dataSource={chartData}
      xName='month'
      yName='sales'
      type='Line'
      name='Sales'
    />
  </SeriesCollectionDirective>
</ChartComponent>
```

**Key additions:**
- `id`: Unique identifier for the chart (required for multiple charts)
- `primaryXAxis`: Configure category values with label formatting
- `primaryYAxis`: Format Y-axis labels (e.g., currency with $ symbol)
- `title`: Main chart heading displayed at the top
- `subTitle`: Additional context displayed below title
- `name`: Series name for legend display

---

## Basic Chart Configuration

### Chart Sizing

Set explicit dimensions to control layout:

```jsx
<ChartComponent
  width='800px'
  height='400px'
>
  {/* chart content */}
</ChartComponent>
```

**Responsive sizing** (recommended for modern apps):

```jsx
<ChartComponent
  width='100%'
  height='400px'
>
  {/* chart content */}
</ChartComponent>
```

This makes the chart responsive to container width while maintaining fixed height.

### Adding Legend

```jsx
<ChartComponent
  legendSettings={{ visible: true, position: 'Right' }}
>
  <Inject services={[LineSeries, Category, Legend]} />
  {/* series */}
</ChartComponent>
```

**Legend positions:** Top, Bottom, Left, Right, Custom

### Adding Tooltip

```jsx
<ChartComponent
  tooltip={{ enable: true, shared: true }}
>
  <Inject services={[LineSeries, Category, Tooltip]} />
  {/* series */}
</ChartComponent>
```

**Tooltip props:**
- `enable`: Show tooltips on hover
- `shared`: Display all series values at once (for multi-series charts)
- `template`: Custom HTML template for tooltip content

---

## Common Setup Issues

### Issue: Chart Not Rendering
**Cause:** Missing CSS imports or undeclared services
**Solution:**
1. Verify CSS files are imported at the top of your component file
2. Check that all services used in the chart are included in `<Inject services={[...]}`
3. Ensure each service is imported from `@syncfusion/ej2-react-charts`

### Issue: Data Not Displaying
**Cause:** Incorrect property mapping (xName/yName)
**Solution:**
1. Verify `xName` and `yName` match your data object property names exactly (case-sensitive)
2. Confirm `dataSource` array contains objects with these properties
3. Check that `type` matches the series type imported and injected

### Issue: Labels Not Showing on Axes
**Cause:** Category axis not injected or valueType not set correctly
**Solution:**
1. Ensure `Category` service is injected for string-based axes
2. For numeric data, use `valueType='Numeric'` or numeric axis type
3. Verify data contains the mapped properties

---

## Full Example: Sales Chart with Multiple Features

```jsx
import React from 'react';
import { ChartComponent, SeriesCollectionDirective, SeriesDirective, Inject, LineSeries, Category, Legend, Tooltip, DataLabel } from '@syncfusion/ej2-react-charts';

export default function SalesChart() {
  const data = [
    { month: 'Jan', revenue: 35, profit: 8 },
    { month: 'Feb', revenue: 28, profit: 6 },
    { month: 'Mar', revenue: 34, profit: 7 },
    { month: 'Apr', revenue: 32, profit: 7 },
    { month: 'May', revenue: 40, profit: 9 }
  ];

  return (
    <ChartComponent
      id='sales-chart'
      primaryXAxis={{
        valueType: 'Category',
        majorGridLines: { width: 0 }
      }}
      primaryYAxis={{
        labelFormat: '${value}K',
        edgeLabelPlacement: 'Shift'
      }}
      title='2024 Sales Performance'
      width='100%'
      height='420px'
      tooltip={{ enable: true, shared: true }}
      legendSettings={{ visible: true, position: 'Top' }}
    >
      <Inject services={[LineSeries, Category, Legend, Tooltip, DataLabel]} />
      <SeriesCollectionDirective>
        <SeriesDirective
          dataSource={data}
          xName='month'
          yName='revenue'
          name='Revenue'
          width={2}
          type='Line'
          marker={{
            visible: true,
            width: 7,
            height: 7,
            dataLabel: { visible: true, position: 'Top' }
          }}
        />
        <SeriesDirective
          dataSource={data}
          xName='month'
          yName='profit'
          name='Profit'
          width={2}
          type='Line'
          marker={{
            visible: true,
            width: 7,
            height: 7
          }}
        />
      </SeriesCollectionDirective>
    </ChartComponent>
  );
}
```

This example demonstrates:
- Multiple series for comparison
- Custom axis label formatting
- Legend and tooltip configuration
- Responsive width and fixed height
- Professional styling with proper spacing

---

## Additional Configuration Options

### Theme Selection

Choose from built-in themes to match your application design:

```jsx
<ChartComponent
  theme='Bootstrap5'  // or 'Material', 'Fabric', 'Tailwind', 'Bootstrap', etc.
>
  {/* chart content */}
</ChartComponent>
```

**Available themes:**
- Material (default), MaterialDark
- Fabric, FabricDark
- Bootstrap, BootstrapDark, Bootstrap4, Bootstrap5, Bootstrap5Dark
- Tailwind, TailwindDark
- Fluent, FluentDark, Fluent2, Fluent2Dark, Fluent2HighContrast
- Material3, Material3Dark
- HighContrast, HighContrastLight

### Background and Borders

```jsx
<ChartComponent
  background='#f5f5f5'
  backgroundImage='/pattern.png'
  border={{
    color: '#e0e0e0',
    width: 2
  }}
  chartArea={{
    border: { color: '#cccccc', width: 1 },
    backgroundColor: '#ffffff'
  }}
>
  {/* chart content */}
</ChartComponent>
```

### Animation Control

```jsx
<ChartComponent
  enableAnimation={true}  // Enable/disable series animations
>
  {/* chart content */}
</ChartComponent>
```

### No Data Template

Display custom content when chart has no data:

```jsx
<ChartComponent
  noDataTemplate='<div style="padding: 20px;">No data available to display</div>'
>
  {/* chart content */}
</ChartComponent>
```

---

## Quick Reference

For complete API documentation and all available properties, see:
- **API Reference:** https://ej2.syncfusion.com/react/documentation/api/chart/overview
- **Official Documentation:** https://ej2.syncfusion.com/react/documentation/api/chart/

---

