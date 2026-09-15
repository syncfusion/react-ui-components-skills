# Pie and Doughnut Charts

## Table of Contents
- [Overview](#overview)
- [Basic Pie Chart](#basic-pie-chart)
- [Radius Customization](#radius-customization)
- [Pie Center Positioning](#pie-center-positioning)
- [Various Radius Pie Chart](#various-radius-pie-chart)
- [Doughnut Chart](#doughnut-chart)
- [Start and End Angles](#start-and-end-angles)
- [Color and Text Mapping](#color-and-text-mapping)
- [Border Radius](#border-radius)
- [Point Customization](#point-customization)
- [Hide Border on Hover](#hide-border-on-hover)
- [Pattern Fills](#pattern-fills)
- [Multi-Level Drill-Down](#multi-level-drill-down)
- [Multiple Pie Series](#multiple-pie-series)
  - [Mapping Related Points with mappingKey](#mapping-related-points-with-mappingkey)

## Overview

Pie and Doughnut charts are circular visualizations that display data as proportional slices. Use them when you need to show:
- Part-to-whole relationships
- Percentage distributions
- Categorical data comparisons
- Market share analysis

## Basic Pie Chart

To render a pie chart, inject `PieSeries` module and set series type to 'Pie' (default):

```tsx
import { 
  AccumulationChartComponent,
  AccumulationSeriesCollectionDirective,
  AccumulationSeriesDirective,
  Inject,
  PieSeries
} from '@syncfusion/ej2-react-charts';

function App() {
  const data = [
    { x: 'Jan', y: 3 }, { x: 'Feb', y: 3.5 },
    { x: 'Mar', y: 7 }, { x: 'Apr', y: 13.5 }
  ];

  return (
    <AccumulationChartComponent id='pie-chart'>
      <Inject services={[PieSeries]} />
      <AccumulationSeriesCollectionDirective>
        <AccumulationSeriesDirective 
          dataSource={data} 
          xName='x' 
          yName='y' 
          type='Pie'  // Optional, 'Pie' is default
        />
      </AccumulationSeriesCollectionDirective>
    </AccumulationChartComponent>
  );
}
```

**Note:** If `PieSeries` module is not explicitly injected, it will be loaded by default.

## Radius Customization

By default, the pie chart radius is 80% of the container size (minimum of width and height). Customize using the `radius` property:

```tsx
<AccumulationSeriesDirective 
  dataSource={data} 
  xName='x' 
  yName='y'
  radius='100%'  // Full container size
/>

// Other examples:
// radius='60%'  - Smaller pie
// radius='90%'  - Standard size
// radius='50%'  - Half size
```

**Common radius values:**
- `'80%'`: Default, provides padding around chart
- `'90%'`: Larger, less padding
- `'100%'`: Maximum size, touches edges
- `'60%'`: Smaller, more white space

## Pie Center Positioning

Change the pie chart's center position using the `center` property. Default is 50% for both x and y:

```tsx
<AccumulationChartComponent 
  id='pie-chart'
  center={{ x: '60%', y: '60%' }}  // Move center to lower-right
>
  <Inject services={[PieSeries, AccumulationDataLabel]} />
  <AccumulationSeriesCollectionDirective>
    <AccumulationSeriesDirective 
      dataSource={data} 
      xName='x' 
      yName='y'
      radius='70%'
    />
  </AccumulationSeriesCollectionDirective>
</AccumulationChartComponent>
```

**Use cases:**
- Offset chart to make room for legends or labels
- Create asymmetric layouts
- Position chart in specific areas of the container

## Various Radius Pie Chart

Create pie charts where each slice has a different radius using the `radius` property with a data field:

```tsx
function App() {
  const data = [
    { x: 'Argentina', y: 505370, r: '50%' },
    { x: 'Belgium', y: 551500, r: '70%' },
    { x: 'Cuba', y: 312685, r: '84%' },
    { x: 'Egypt', y: 357022, r: '90%' }
  ];

  return (
    <AccumulationChartComponent 
      id='varied-radius-pie'
      legendSettings={{ visible: true }}
      enableSmartLabels={true}
    >
      <Inject services={[PieSeries, AccumulationDataLabel, AccumulationLegend]} />
      <AccumulationSeriesCollectionDirective>
        <AccumulationSeriesDirective 
          dataSource={data} 
          xName='x' 
          yName='y'
          radius='r'  // Use 'r' field from data for radius
          innerRadius='20%'
          dataLabel={{ visible: true, position: 'Outside', name: 'x' }}
        />
      </AccumulationSeriesCollectionDirective>
    </AccumulationChartComponent>
  );
}
```

This creates a visually interesting effect where slice importance is shown by both angle and radius.

## Doughnut Chart

Create a doughnut chart by setting the `innerRadius` property to a value greater than 0%:

```tsx
<AccumulationSeriesDirective 
  dataSource={data} 
  xName='x' 
  yName='y'
  innerRadius='40%'  // Creates doughnut hole
  radius='80%'
/>
```

**InnerRadius values:**
- `'0%'`: Regular pie chart (no hole)
- `'20%'`-`'40%'`: Thin doughnut
- `'50%'`-`'70%'`: Wide doughnut
- `'80%'`: Very thin ring

**Complete doughnut example:**

```tsx
function DoughnutChart() {
  const data = [
    { category: 'Mobile', value: 45, text: 'Mobile: 45%' },
    { category: 'Desktop', value: 35, text: 'Desktop: 35%' },
    { category: 'Tablet', value: 20, text: 'Tablet: 20%' }
  ];

  return (
    <AccumulationChartComponent 
      id='doughnut-chart'
      title='Device Usage'
      legendSettings={{ visible: true, position: 'Right' }}
    >
      <Inject services={[PieSeries, AccumulationLegend]} />
      <AccumulationSeriesCollectionDirective>
        <AccumulationSeriesDirective 
          dataSource={data}
          xName='category'
          yName='value'
          innerRadius='40%'
          radius='100%'
        />
      </AccumulationSeriesCollectionDirective>
    </AccumulationChartComponent>
  );
}
```

### Center Label (Doughnut Charts)

Display a configurable label at the center of a doughnut chart (commonly used for totals or summaries).

```tsx
<AccumulationChartComponent id='center-label-chart'>
  <Inject services={[PieSeries]} />
  <AccumulationSeriesCollectionDirective>
    <AccumulationSeriesDirective
      dataSource={data}
      xName='x'
      yName='y'
      innerRadius='50%'
      centerLabel={{
        text: 'Total: 100M',
        textStyle: { size: '24px', fontWeight: 'bold', color: '#333' }
      }}
    />
  </AccumulationSeriesCollectionDirective>
</AccumulationChartComponent>
```

**Note**: Use the `centerLabel` property or an annotation at the center for dynamic totals. Its is specifically designed for doughnut charts and automatically stays centered even during resize or animation.

## Start and End Angles

Customize the pie chart's angular range using `startAngle` and `endAngle` properties. Default is 0° to 360° (full circle):

```tsx
<AccumulationSeriesDirective 
  dataSource={data}
  xName='x'
  yName='y'
  startAngle={270}  // Start from bottom
  endAngle={90}     // End at bottom (semi-circle)
  dataLabel={{ visible: true, position: 'Outside' }}
/>
```

**Common angle patterns:**

**Semi-pie (gauge style):**
```tsx
startAngle={270}
endAngle={90}
// Creates bottom-to-bottom semi-circle
```

**Quarter-pie:**
```tsx
startAngle={0}
endAngle={90}
// Creates top-right quarter
```

**Three-quarter pie:**
```tsx
startAngle={0}
endAngle={270}
// Creates three-quarter circle
```

**Reverse direction:**
```tsx
startAngle={360}
endAngle={0}
// Draws slices counter-clockwise
```

## Color and Text Mapping

Map fill colors and text from your data source using `pointColorMapping` and data label `name` property:

```tsx
function ColorMappedChart() {
  const data = [
    { x: 'Jan', y: 3, text: 'January: 3%', fill: '#FF6384' },
    { x: 'Feb', y: 3.5, text: 'February: 3.5%', fill: '#36A2EB' },
    { x: 'Mar', y: 7, text: 'March: 7%', fill: '#FFCE56' },
    { x: 'Apr', y: 13.5, text: 'April: 13.5%', fill: '#4BC0C0' }
  ];

  return (
    <AccumulationChartComponent id='color-mapped-pie'>
      <Inject services={[PieSeries, AccumulationDataLabel]} />
      <AccumulationSeriesCollectionDirective>
        <AccumulationSeriesDirective 
          dataSource={data}
          xName='x'
          yName='y'
          pointColorMapping='fill'  // Use 'fill' field for colors
          dataLabel={{
            visible: true,
            name: 'text',  // Use 'text' field for labels
            position: 'Outside'
          }}
        />
      </AccumulationSeriesCollectionDirective>
    </AccumulationChartComponent>
  );
}
```

This gives you complete control over slice colors directly from your data.

## Border Radius

Add rounded corners to pie slices using the `borderRadius` property for a modern look:

```tsx
<AccumulationSeriesDirective 
  dataSource={data}
  xName='x'
  yName='y'
  borderRadius={8}  // Rounds corners by 8px
  startAngle={0}
  endAngle={360}
/>
```

**Border radius values:**
- `0`: Sharp corners (default)
- `4-8`: Subtle rounding
- `10-15`: Noticeable rounding
- `20+`: Heavy rounding

**Example with border radius:**

```tsx
function RoundedPieChart() {
  const data = [
    { x: 'Product A', y: 30 },
    { x: 'Product B', y: 25 },
    { x: 'Product C', y: 45 }
  ];

  return (
    <AccumulationChartComponent 
      id='rounded-pie'
      legendSettings={{ visible: false }}
    >
      <Inject services={[PieSeries]} />
      <AccumulationSeriesCollectionDirective>
        <AccumulationSeriesDirective 
          dataSource={data}
          xName='x'
          yName='y'
          borderRadius={12}
          radius='90%'
        />
      </AccumulationSeriesCollectionDirective>
    </AccumulationChartComponent>
  );
}
```

## Point Customization

Customize individual slices using the `pointRender` event:

```tsx
import { IAccPointRenderEventArgs } from '@syncfusion/ej2-react-charts';

function CustomizedPieChart() {
  const data = [
    { x: 'Jan', y: 3 },
    { x: 'Feb', y: 3.5 },
    { x: 'Mar', y: 7 },
    { x: 'Apr', y: 13.5 }
  ];

  const onPointRender = (args: IAccPointRenderEventArgs): void => {
    // Highlight April slice
    if (args.point.x === 'Apr') {
      args.fill = '#f4bc42';  // Golden color
      args.border = { color: '#333', width: 2 };
    } else {
      args.fill = '#597cf9';  // Blue for others
    }
  };

  return (
    <AccumulationChartComponent 
      id='custom-pie'
      pointRender={onPointRender}
    >
      <Inject services={[PieSeries]} />
      <AccumulationSeriesCollectionDirective>
        <AccumulationSeriesDirective 
          dataSource={data}
          xName='x'
          yName='y'
        />
      </AccumulationSeriesCollectionDirective>
    </AccumulationChartComponent>
  );
}
```

**Use cases for pointRender:**
- Highlight specific slices
- Apply conditional coloring based on values
- Add custom borders to important points
- Implement dynamic theming

## Hide Border on Hover

By default, hovering over a slice shows a border. Disable this using `enableBorderOnMouseMove`:

```tsx
<AccumulationChartComponent 
  id='no-hover-border'
  enableBorderOnMouseMove={false}  // Disable hover border
>
  <Inject services={[PieSeries]} />
  <AccumulationSeriesCollectionDirective>
    <AccumulationSeriesDirective 
      dataSource={data}
      xName='x'
      yName='y'
    />
  </AccumulationSeriesCollectionDirective>
</AccumulationChartComponent>
```

Use this when:
- You want a cleaner hover effect
- Borders conflict with your design
- You're using custom hover states

## Pattern Fills

Apply patterns (stripes, dots, grids) to pie slices using the `applyPattern` property and `pointRender` event:

```tsx
function PatternPieChart() {
  const data = [
    { x: 'Chrome', y: 61.3 },
    { x: 'Safari', y: 24.6 },
    { x: 'Edge', y: 5.0 },
    { x: 'Firefox', y: 4.8 }
  ];

  return (
    <AccumulationChartComponent 
      id='pattern-pie'
      title='Browser Statistics'
      legendSettings={{ visible: false }}
    >
      <Inject services={[PieSeries]} />
      <AccumulationSeriesCollectionDirective>
        <AccumulationSeriesDirective 
          dataSource={data}
          xName='x'
          yName='y'
          applyPattern={true}  // Enable patterns
          startAngle={0}
          endAngle={360}
        />
      </AccumulationSeriesCollectionDirective>
    </AccumulationChartComponent>
  );
}
```

Patterns are useful for:
- Black and white printing
- Accessibility (color-blind users)
- Visual distinction without color dependency

## Multi-Level Drill-Down

Implement drill-down functionality using `pointClick` and `chartMouseClick` events:

```tsx
import { useState } from 'react';
import { IAccPointEventArgs, IMouseEventArgs } from '@syncfusion/ej2-react-charts';

function DrillDownChart() {
  const [currentData, setCurrentData] = useState(mainData);
  const [level, setLevel] = useState(0);

  const mainData = [
    { x: 'USA', y: 46, drillData: usaData },
    { x: 'China', y: 26, drillData: chinaData },
    { x: 'India', y: 18, drillData: indiaData }
  ];

  const usaData = [
    { x: 'California', y: 20 },
    { x: 'Texas', y: 15 },
    { x: 'Florida', y: 11 }
  ];

  const onPointClick = (args: IAccPointEventArgs): void => {
    if (level === 0 && args.point.drillData) {
      setCurrentData(args.point.drillData);
      setLevel(1);
    }
  };

  const onChartClick = (args: IMouseEventArgs): void => {
    // Click outside to drill-up
    if (level > 0) {
      setCurrentData(mainData);
      setLevel(0);
    }
  };

  return (
    <AccumulationChartComponent 
      id='drill-down'
      pointClick={onPointClick}
      chartMouseClick={onChartClick}
      title={level === 0 ? 'Countries' : 'States'}
    >
      <Inject services={[PieSeries]} />
      <AccumulationSeriesCollectionDirective>
        <AccumulationSeriesDirective 
          dataSource={currentData}
          xName='x'
          yName='y'
        />
      </AccumulationSeriesCollectionDirective>
    </AccumulationChartComponent>
  );
}
```

**Drill-down tips:**
- Store drill data in each point
- Track navigation level with state
- Provide visual cue for drill-up (back button or click outside)
- Show breadcrumb for current level

## Multiple Pie Series

Render multiple pie or doughnut series in a single Accumulation Chart to compare related datasets as concentric rings. Each series can have its own data source, radius, inner radius, labels, and styling.

```tsx
import {
  AccumulationChartComponent,
  AccumulationSeriesCollectionDirective,
  AccumulationSeriesDirective,
  Inject,
  PieSeries,
  AccumulationDataLabel,
  AccumulationLegend,
  AccumulationTooltip
} from '@syncfusion/ej2-react-charts';

function MultiplePieSeriesChart() {
  const currentYearData = [
    { category: 'Mobile', value: 45 },
    { category: 'Desktop', value: 35 },
    { category: 'Tablet', value: 20 }
  ];

  const previousYearData = [
    { category: 'Mobile', value: 38 },
    { category: 'Desktop', value: 42 },
    { category: 'Tablet', value: 20 }
  ];

  return (
    <AccumulationChartComponent
      id='multiple-pie-series'
      title='Device Usage Comparison'
      legendSettings={{ visible: true }}
      tooltip={{
        enable: true,
        format: '${series.name}<br/>${point.x}: <b>${point.y}%</b>'
      }}
    >
      <Inject
        services={[
          PieSeries,
          AccumulationDataLabel,
          AccumulationLegend,
          AccumulationTooltip
        ]}
      />
      <AccumulationSeriesCollectionDirective>
        <AccumulationSeriesDirective
          dataSource={currentYearData}
          xName='category'
          yName='value'
          name='Current Year'
          type='Pie'
          radius='100%'
          innerRadius='70%'
          dataLabel={{
            visible: true,
            name: 'category',
            position: 'Outside'
          }}
        />
        <AccumulationSeriesDirective
          dataSource={previousYearData}
          xName='category'
          yName='value'
          name='Previous Year'
          type='Pie'
          radius='60%'
          innerRadius='30%'
          dataLabel={{
            visible: true,
            name: 'category',
            position: 'Inside'
          }}
        />
      </AccumulationSeriesCollectionDirective>
    </AccumulationChartComponent>
  );
}

export default MultiplePieSeriesChart;
```

Configure `radius` and `innerRadius` for each series so that the series render as separate concentric rings without overlapping.

- The outer series uses a larger `radius`.
- The inner series uses a smaller `radius`.
- The `innerRadius` determines the thickness of each ring.
- Each series can use a separate data source and visual configuration.
- Tooltips identify the hovered point and its corresponding series.

### Mapping Related Points with mappingKey

Use the `mappingKey` property in `legendSettings` to associate corresponding points across multiple pie series. The property specifies the point field used to group related legend items across the series.

```tsx
function MappedMultiplePieSeriesChart() {
  const currentYearData = [
    { id: 'mobile', category: 'Mobile', value: 45 },
    { id: 'desktop', category: 'Desktop', value: 35 },
    { id: 'tablet', category: 'Tablet', value: 20 }
  ];

  const previousYearData = [
    { id: 'mobile', category: 'Mobile', value: 38 },
    { id: 'desktop', category: 'Desktop', value: 42 },
    { id: 'tablet', category: 'Tablet', value: 20 }
  ];

  return (
    <AccumulationChartComponent
      id='multiple-pie-series'
      title='Device Usage Comparison'
      legendSettings={{
        visible: true,
        mappingKey: 'x'
      }}
      tooltip={{
        enable: true,
        format: '${series.name}<br/>${point.x}: <b>${point.y}%</b>'
      }}
    >
      <Inject
        services={[
          PieSeries,
          AccumulationDataLabel,
          AccumulationLegend,
          AccumulationTooltip
        ]}
      />
      <AccumulationSeriesCollectionDirective>
        <AccumulationSeriesDirective
          dataSource={currentYearData}
          xName='category'
          yName='value'
          name='Current Year'
          type='Pie'
          radius='100%'
          innerRadius='70%'
          dataLabel={{
            visible: true,
            name: 'category',
            position: 'Outside'
          }}
        />
        <AccumulationSeriesDirective
          dataSource={previousYearData}
          xName='category'
          yName='value'
          name='Previous Year'
          type='Pie'
          radius='60%'
          innerRadius='30%'
          dataLabel={{
            visible: true,
            name: 'category',
            position: 'Inside'
          }}
        />
      </AccumulationSeriesCollectionDirective>
    </AccumulationChartComponent>
  );
}

export default MappedMultiplePieSeriesChart;
```

In this example:

- `mappingKey: 'x'` is configured within `legendSettings`.
- The `x` value represents the category mapped through the `xName` property of each series.
- Points with the same category value across multiple series are represented by a common legend item.
- Interacting with a mapped legend item affects the corresponding points in all related pie series.
- The point mapping does not depend on the order of items in each data source.

For example, the following points are associated because both use `Mobile` as their category value:

```tsx
// Current year
{ id: 'mobile', category: 'Mobile', value: 45 }

// Previous year
{ id: 'mobile', category: 'Mobile', value: 38 }
```

Both series configure `xName='category'`. Therefore, the chart maps the `category` field to the internal `x` value used by `mappingKey`.

**Requirements for `mappingKey`:**

- Configure `mappingKey` inside the `legendSettings` property.
- Use `'x'` to associate points based on the field mapped through each series' `xName` property.
- Related points across the series must have identical X values.
- Each series should use a consistent category mapping.
- The mapped values should uniquely identify categories within each series.

**Note:** Multiple pie series require the `PieSeries` module. Inject `AccumulationLegend` to display and interact with the mapped legend items. Inject `AccumulationDataLabel` and `AccumulationTooltip` only when their corresponding features are enabled.

## Common Issues and Solutions

**Issue: Pie chart too small**
- Solution: Increase `radius` to '90%' or '100%'

**Issue: Slices not showing expected colors**
- Solution: Use `pointColorMapping` or `pointRender` event

**Issue: Doughnut hole too large/small**
- Solution: Adjust `innerRadius` value (0%-80%)

**Issue: Labels overlapping**
- Solution: Enable `enableSmartLabels={true}` on AccumulationChartComponent

**Issue: Semi-pie not aligned correctly**
- Solution: Adjust `startAngle` and `endAngle` (angles are in degrees, 0° is top)
