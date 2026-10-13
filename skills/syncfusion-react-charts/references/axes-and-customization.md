# Axes and Customization

## Table of Contents
- [Overview](#overview)
- [Axis Types](#axis-types)
- [Primary Axes Configuration](#primary-axes-configuration)
- [Secondary Axes](#secondary-axes)
- [Axis Labels and Formatting](#axis-labels-and-formatting)
- [Multiple Panes](#multiple-panes)
- [Common Patterns](#common-patterns)

---

## Overview

Axes define how your chart interprets and displays data. Syncfusion React Charts supports multiple axis types, each suited for different data formats:

- **Category:** String or categorical values
- **Numeric:** Numerical values with mathematical scaling
- **DateTime:** Date/time values with temporal scaling
- **Logarithmic:** Exponential scale (useful for wide value ranges)

Most charts have `primaryXAxis` and `primaryYAxis`. Financial charts may have additional `indicator` axes.

---

## Axis Types

### Category Axis

Use for string-based categorical data (month names, product names, regions, etc.).

```jsx
<ChartComponent
  primaryXAxis={{ valueType: 'Category' }}
  primaryYAxis={{ valueType: 'Double' }}
>
  {/* series with xName mapping to category values */}
</ChartComponent>
```

**When to use:**
- Product comparisons
- Monthly/quarterly data
- Regional breakdowns
- Any discrete, non-numeric categories

**Common configurations:**
```jsx
primaryXAxis={{
  valueType: 'Category',
  labelFormat: '{value}',
  majorTickLines: { width: 2 },
  minorTickLines: { width: 1 },
  labelIntersectAction: 'Rotate45' // or 'Wrap', 'MultipleRows'
}}
```

### Numeric Axis

Use for numerical data with automatic scaling and intervals.

```jsx
<ChartComponent
  primaryXAxis={{ valueType: 'Double' }}
  primaryYAxis={{ valueType: 'Double' }}
>
  {/* scatter or other XY charts */}
</ChartComponent>
```

**When to use:**
- Scatter plots
- Correlation analysis
- Weight vs. height comparisons
- Any XY coordinate data

**Common configurations:**
```jsx
primaryYAxis={{
  valueType: 'Double',
  minimum: 0,
  maximum: 100,
  interval: 10,
  labelFormat: '${value}K'
}}
```

### DateTime Axis

Use for time-series data with automatic date/time scaling.

```jsx
<ChartComponent
  primaryXAxis={{
    valueType: 'DateTime',
    intervalType: 'Months', // or 'Years', 'Days', 'Hours'
    interval: 1
  }}
>
  {/* time-based data */}
</ChartComponent>
```

**When to use:**
- Stock prices over time
- Website traffic trends
- Sensor readings
- Any time-series analysis

**intervalType options:** Seconds, Minutes, Hours, Days, Months, Years

**Common configurations:**
```jsx
primaryXAxis={{
  valueType: 'DateTime',
  intervalType: 'Days',
  interval: 7, // weekly intervals
  labelFormat: 'MMM dd', // e.g., "Mar 15"
}}
```

### DateTimeCategory Axis
Use for date-time values treated as categories (non-linear intervals, useful for irregular dates like stock market holidays).

```jsx
<ChartComponent
  primaryXAxis={{
    valueType: 'DateTimeCategory',
    intervalType: 'Months',
    labelFormat: 'MMM yyyy'
  }}
>
  {/* series */}
</ChartComponent>
```

**When to use:** Stock or event data with missing dates.

### Logarithmic Axis

Use for data with exponential growth or very large value ranges.

```jsx
<ChartComponent
  primaryYAxis={{
    valueType: 'Logarithmic',
    logBase: 10 // or 2, e, etc.
  }}
>
  {/* data with exponential scale */}
</ChartComponent>
```

**When to use:**
- Financial charts (stock growth)
- Scientific data (exponential growth)
- Wide value ranges (1 to 1,000,000)

---

## Primary Axes Configuration

The two main axes that define the chart's coordinate system.

### Basic Configuration

```jsx
<ChartComponent
  primaryXAxis={{
    valueType: 'Category',
    labelFormat: '{value}',
    title: 'Months'
  }}
  primaryYAxis={{
    valueType: 'Double',
    labelFormat: '${value}K',
    title: 'Sales Revenue',
    minimum: 0,
    maximum: 100
  }}
>
  {/* chart content */}
</ChartComponent>
```

### Label Formatting

Format axis labels for readability:

```jsx
// Currency formatting
primaryYAxis={{ labelFormat: '${value}' }}

// Percentage formatting
primaryYAxis={{ labelFormat: '{value}%' }}

// Thousand separators
primaryYAxis={{ labelFormat: '{value:n0}' }} // 1,000

// Decimal places
primaryYAxis={{ labelFormat: '{value:n2}' }} // 1,000.00

// Date formatting
primaryXAxis={{ labelFormat: 'MMM dd' }} // Mar 15

// Scientific notation
primaryYAxis={{ labelFormat: '{value:e2}' }} // 1.23e+5
```

### Axis Range Configuration

Control the min/max values and interval spacing:

```jsx
primaryYAxis={{
  minimum: 0,
  maximum: 100,
  interval: 10, // distance between labels
  intervalType: 'Days', // or 'Days', 'Months', etc.
  edgelabelPlacement: 'Shift' // or 'Hide', 'None'
}}
```

### Axis Crossing

Control where axes intersect:

```jsx
primaryYAxis={{
  crossesAt: 50, // Y-axis crosses X-axis at x=50
  opposedPosition: false // true moves axis to opposite side
}}

primaryXAxis={{
  crossesAt: 0, // X-axis crosses Y-axis at y=0
  opposedPosition: true // Moves to top of chart
}}
```

### Label Angle and Rotation

Rotate axis labels for long text:

```jsx
primaryXAxis={{
  labelRotation: 45 // rotate labels 45 degrees
}}

// Or use automatic action
primaryXAxis={{
  labelIntersectAction: 'Rotate45' // auto-rotate if needed
  // Also: 'Wrap', 'MultipleRows', 'None'
}}
```



##### Axis label intersection configuration

`labelIntersectAction` belongs directly inside `primaryXAxis`, `primaryYAxis`, or an additional axis object. It controls collisions between axis labels and is different from point data-label collision handling.

```tsx
<ChartComponent
  primaryXAxis={{
    valueType: 'Category',
    labelIntersectAction: 'Rotate45WithTrim',
    maximumLabelWidth: 90
  }}
>
  <Inject services={[Category, ColumnSeries]} />
</ChartComponent>
```

Use the actions as follows:

- `None`: Leaves labels unchanged even when they overlap.
- `Hide`: Hides labels that collide with previously rendered labels.
- `Trim`: Trims intersecting labels and displays an ellipsis.
- `Wrap`: Wraps intersecting labels into multiple lines.
- `MultipleRows`: Moves colliding horizontal-axis labels into additional rows.
- `Rotate45`: Rotates intersecting labels by 45 degrees.
- `Rotate90`: Rotates intersecting labels by 90 degrees.
- `Rotate45WithTrim`: Rotates labels by 45 degrees and trims text that exceeds the available width.
- `Rotate90WithTrim`: Rotates labels by 90 degrees and trims text that exceeds the available width.

Use `maximumLabelWidth` with trim or wrap actions when label bounds must remain predictable. Use `labelRotation` only when a fixed angle must be applied regardless of whether labels intersect.

Do not nest the action under a `label` object:

```tsx
// Incorrect
primaryXAxis={{
  label: {
    labelIntersectAction: 'Rotate45WithTrim'
  }
}}
```

Axis intersection does not require a dedicated React module. Inject the module required by the axis value type, such as `Category`, `DateTime`, `DateTimeCategory`, or `Logarithmic`, plus every rendered series module.


#### Rotation with Trimming

`labelIntersectAction` additionally supports the following actions for long intersecting axis labels:

- `Rotate45WithTrim`: Rotates intersecting labels by 45 degrees and trims text that exceeds the available label bounds.
- `Rotate90WithTrim`: Rotates intersecting labels by 90 degrees and trims text that exceeds the available label bounds.

Use these actions when rotation is required but long labels must remain constrained to the available axis-label area. Unlike `Rotate45` and `Rotate90`, the trim variants apply trimming after rotation.

##### Complete `Rotate45WithTrim` sample

```tsx
import * as React from 'react';
import {
  Category, ChartComponent, ColumnSeries, Inject, SeriesCollectionDirective, SeriesDirective, Tooltip
} from '@syncfusion/ej2-react-charts';

const data: Object[] = [
  { department: 'Consumer Electronics and Accessories', revenue: 72 },
  { department: 'Home Furniture and Interior Products', revenue: 58 },
  { department: 'Sports Equipment and Outdoor Recreation', revenue: 64 },
  { department: 'Personal Care and Beauty Products', revenue: 49 },
  { department: 'Books, Education and Office Supplies', revenue: 55 },
  { department: 'Automotive Parts and Maintenance', revenue: 46 }
];

export default function Rotate45WithTrimChart(): JSX.Element {
  return (
    <ChartComponent
      id='rotate45-with-trim-chart'
      title='Department Revenue with 45-Degree Rotation and Trimming'
      primaryXAxis={{
        valueType: 'Category',
        title: 'Department',
        labelIntersectAction: 'Rotate45WithTrim',
        maximumLabelWidth: 90,
        majorGridLines: { width: 0 }
      }}
      primaryYAxis={{ title: 'Revenue (Millions)', minimum: 0, maximum: 80, interval: 20, labelFormat: '${value}M' }}
      tooltip={{ enable: true, format: '${point.x}: ${point.y}M' }}
      legendSettings={{ visible: false }}
    >
      <Inject services={[Category, ColumnSeries, Tooltip]} />
      <SeriesCollectionDirective>
        <SeriesDirective dataSource={data} type='Column' xName='department' yName='revenue' name='Revenue' />
      </SeriesCollectionDirective>
    </ChartComponent>
  );
}
```

##### Complete `Rotate90WithTrim` sample

```tsx
import * as React from 'react';
import {
  Category, ChartComponent, ColumnSeries, Inject, SeriesCollectionDirective, SeriesDirective, Tooltip
} from '@syncfusion/ej2-react-charts';

const data: Object[] = [
  { product: 'Professional Wireless Noise-Cancelling Headphones', units: 42 },
  { product: 'Ultra-Wide High-Resolution Desktop Monitor', units: 36 },
  { product: 'Ergonomic Adjustable Standing Workstation Desk', units: 31 },
  { product: 'Smart Home Security and Monitoring System', units: 39 },
  { product: 'Portable High-Capacity Power Station', units: 28 },
  { product: 'Automatic Robotic Floor Cleaning System', units: 34 }
];

export default function Rotate90WithTrimChart(): JSX.Element {
  return (
    <ChartComponent
      id='rotate90-with-trim-chart'
      title='Product Sales with 90-Degree Rotation and Trimming'
      primaryXAxis={{
        valueType: 'Category',
        title: 'Product',
        labelIntersectAction: 'Rotate90WithTrim',
        maximumLabelWidth: 100,
        majorGridLines: { width: 0 }
      }}
      primaryYAxis={{ title: 'Units Sold', minimum: 0, maximum: 50, interval: 10 }}
      tooltip={{ enable: true, format: '${point.x}: ${point.y} units' }}
      legendSettings={{ visible: false }}
    >
      <Inject services={[Category, ColumnSeries, Tooltip]} />
      <SeriesCollectionDirective>
        <SeriesDirective dataSource={data} type='Column' xName='product' yName='units' name='Units Sold' />
      </SeriesCollectionDirective>
    </ChartComponent>
  );
}
```

#### Extended Axis Scrollbar Settings

##### React module requirements and configuration placement

Inject `ScrollBar` for Chart scrollbars and `Zoom` when scrolling participates in zooming or panning. Also inject the rendered series module and the module required by the axis value type.

```tsx
import {
  Category, ChartComponent, Inject, LineSeries, ScrollBar, Zoom
} from '@syncfusion/ej2-react-charts';
```

Two related configurations are available:

- `zoomSettings.enableScrollbar` enables the standard scrollbar that follows the Chart zoom state.
- `axis.scrollbarSettings` controls axis-specific appearance, placement, lazy-load range, and zoom behavior.

```tsx
<ChartComponent
  id='scrollbar-chart'
  primaryXAxis={{
    valueType: 'Category',
    zoomFactor: 0.5,
    zoomPosition: 0,
    scrollbarSettings: {
      enable: true,
      height: 16,
      enableZoom: true,
      position: 'PlaceNextToAxisLine',
      scrollbarRadius: 4,
      trackRadius: 4,
      scrollbarColor: '#6B7280',
      trackColor: '#E5E7EB',
      gripColor: '#FFFFFF'
    }
  }}
  zoomSettings={{
    enableScrollbar: true,
    enableSelectionZooming: true,
    enableMouseWheelZooming: true,
    enablePan: true,
    mode: 'X'
  }}
>
  <Inject services={[Category, LineSeries, ScrollBar, Zoom]} />
</ChartComponent>
```

Use the settings as follows:

- `enable`: Enables an axis scrollbar for lazy-loading workflows.
- `pointsLength`: Defines the complete logical point count for numeric or logarithmic lazy loading.
- `range`: Defines the complete minimum and maximum DateTime range.
- `height`: Sets scrollbar thickness. Use `12` or greater when the grip-circle visual and grab cursor are required.
- `enableZoom`: Enables thumb resizing, arrows, and scrollbar wheel zoom. Set it to `false` for pan-only behavior.
- `position`: Use `Top` or `Bottom` for horizontal axes, `Left` or `Right` for vertical axes, or `PlaceNextToAxisLine` for axis-relative placement.
- `scrollbarColor`, `trackColor`, and `gripColor`: Configure thumb, track, and grip colors.
- `scrollbarRadius` and `trackRadius`: Configure thumb and track corner radii.

For DateTime lazy loading, use `range` and inject `DateTime`:

```tsx
primaryXAxis={{
  valueType: 'DateTime',
  minimum: new Date(2025, 0, 1),
  maximum: new Date(2025, 2, 31),
  scrollbarSettings: {
    enable: true,
    range: {
      minimum: new Date(2024, 0, 1),
      maximum: new Date(2026, 11, 31)
    },
    enableZoom: true,
    height: 16
  }
}}
```

For numeric or logarithmic lazy loading, use `pointsLength`:

```tsx
primaryXAxis={{
  minimum: 0,
  maximum: 100,
  scrollbarSettings: {
    enable: true,
    pointsLength: 1000,
    enableZoom: false,
    height: 16
  }
}}
```

Use `scrollStart` for interaction start, `scrollChanged` for continuous range changes, and `scrollEnd` to request or apply lazy-loaded data after the final range is known.


The axis scrollbar supports `enableZoom`, `scrollbarRadius`, `trackRadius`, `scrollbarColor`, `trackColor`, `gripColor`, and `position`. `enableZoom` defaults to `true`. When enabled, resize handles and wheel zoom are available. When disabled, the scrollbar is pan-only. A scrollbar height of 12 or greater enables the grip-circle visual and grab cursor.

```tsx
<ChartComponent
  primaryXAxis={{
    valueType: 'Category',
    scrollbarSettings: {
      enable: true,
      height: 16,
      enableZoom: true,
      position: 'PlaceNextToAxisLine',
      scrollbarRadius: 4,
      trackRadius: 4,
      scrollbarColor: '#6B7280',
      trackColor: '#E5E7EB',
      gripColor: '#FFFFFF'
    }
  }}
>
  <Inject services={[ScrollBar]} />
</ChartComponent>
```

### Grid Lines Configuration

Control major and minor grid lines:

```jsx
primaryYAxis={{
  majorGridLines: {
    width: 1,
    color: '#e0e0e0',
    dashArray: '5,5' // dashed
  },
  minorGridLines: {
    visible: false
  }
}}
```

### Axis Scrollbar
Enable scrollbar on axis for large datasets or zooming navigation.

```jsx
<ChartComponent
  primaryXAxis={{
    valueType: 'Category',
    scrollbarSettings: {
      enable: true,
      pointsLength: 50  // optional
    }
  }}
>
  <Inject services={[ScrollBar]} />
  {/* series */}
</ChartComponent>
```

**Note:** Requires injecting `ScrollBar` module.

---

## Secondary Axes

Add additional axes for comparing different scales or units.

### Basic Secondary Axis

```jsx
<ChartComponent
  primaryYAxis={{ /* config */ }}
  axes={[{
    name: 'secondaryYAxis',
    valueType: 'Double',
    labelFormat: '${value}',
    opposedPosition: true // right side
  }]}
>
  <SeriesCollectionDirective>
    <SeriesDirective
      yAxisName='secondaryYAxis' // attach to secondary
      type='Line'
      ...
    />
  </SeriesCollectionDirective>
</ChartComponent>
```

### Use Case: Comparing Revenue and Count

```jsx
const chartData = [
  { month: 'Jan', revenue: 35000, customers: 120 },
  { month: 'Feb', revenue: 28000, customers: 105 }
];

<ChartComponent
  primaryYAxis={{
    labelFormat: '${value}',
    title: 'Revenue',
    minimum: 0,
    maximum: 40000
  }}
  axes={[{
    name: 'countAxis',
    valueType: 'Double',
    title: 'Customer Count',
    minimum: 0,
    maximum: 200,
    opposedPosition: true
  }]}
>
  <SeriesCollectionDirective>
    {/* Revenue series on primary axis */}
    <SeriesDirective
      dataSource={chartData}
      xName='month'
      yName='revenue'
      type='Column'
      name='Revenue'
    />
    {/* Customer count on secondary axis */}
    <SeriesDirective
      dataSource={chartData}
      xName='month'
      yName='customers'
      yAxisName='countAxis'
      type='Line'
      name='Customers'
    />
  </SeriesCollectionDirective>
</ChartComponent>
```

---

## Axis Labels and Formatting

### Custom Label Formatting

```jsx
primaryYAxis={{
  labelFormat: (args) => {
    // Custom formatting logic
    if (args.value >= 1000) {
      return (args.value / 1000).toFixed(1) + 'K';
    }
    return args.value;
  }
}}
```

### Axis Titles

```jsx
<ChartComponent
  primaryXAxis={{
    title: 'Months',
    titleStyle: {
      fontFamily: 'Arial',
      fontStyle: 'Normal',
      fontWeight: '400',
      size: '14px',
      color: '#424242'
    }
  }}
  primaryYAxis={{
    title: 'Sales Revenue ($)',
    titleStyle: { size: '14px', color: '#424242' }
  }}
>
  {/* chart */}
</ChartComponent>
```

### Tick Marks

Control major and minor tick marks:

```jsx
primaryXAxis={{
  majorTickLines: {
    width: 2,
    size: 5,
    color: '#000'
  },
  minorTickLines: {
    visible: true,
    width: 1,
    size: 3
  }
}}
```

---

## Multiple Panes

Split chart into multiple panes for complex comparisons.

### Basic Multiple Panes

```jsx
import { StripLine } from '@syncfusion/ej2-react-charts';

<ChartComponent
  rows={[
    { height: '50%' },
    { height: '50%' }
  ]}
>
  <SeriesCollectionDirective>
    {/* Series for top pane */}
    <SeriesDirective
      xAxisName='topXAxis'
      yAxisName='topYAxis'
      type='Column'
      ...
    />
    {/* Series for bottom pane */}
    <SeriesDirective
      xAxisName='bottomXAxis'
      yAxisName='bottomYAxis'
      type='Line'
      ...
    />
  </SeriesCollectionDirective>
</ChartComponent>
```

### Use Case: Price and Volume

```jsx
<ChartComponent
  rows={[
    { height: '70%' }, // price chart
    { height: '30%' }  // volume chart
  ]}
  axes={[
    { name: 'priceYAxis', rowIndex: 0, ... },
    { name: 'volumeYAxis', rowIndex: 1, ... }
  ]}
>
  {/* price series */}
  {/* volume series */}
</ChartComponent>
```

---

## Common Patterns

### Pattern 1: Financial Chart with Two Y-Axes

```jsx
<ChartComponent
  primaryYAxis={{
    labelFormat: '${value}',
    title: 'Stock Price',
    minimum: 50,
    maximum: 150
  }}
  axes={[{
    name: 'volumeAxis',
    labelFormat: '{value}M',
    title: 'Trading Volume',
    opposedPosition: true
  }]}
>
  <SeriesCollectionDirective>
    <SeriesDirective type='Candle' yName='close' />
    <SeriesDirective type='Column' yAxisName='volumeAxis' yName='volume' />
  </SeriesCollectionDirective>
</ChartComponent>
```

### Pattern 2: DateTime Axis with Proper Intervals

```jsx
primaryXAxis={{
  valueType: 'DateTime',
  intervalType: 'Months',
  interval: 1,
  labelFormat: 'MMM yyyy',
  majorGridLines: { width: 0 }
}}
```

### Pattern 3: Category Axis with Rotated Labels

```jsx
primaryXAxis={{
  valueType: 'Category',
  labelRotation: -45,
  labelIntersectAction: 'Rotate45',
  minorTickLines: { width: 0 },
  labelStyle: { angle: -45 }
}}
```

### Pattern 4: Logarithmic Axis for Wide Ranges

```jsx
primaryYAxis={{
  valueType: 'Logarithmic',
  logBase: 10,
  title: 'Log Scale',
  labelFormat: '10^{value}'
}}
```

---

## Edge Cases and Troubleshooting

### Issue: Labels Overlapping
**Solution:** Use `labelIntersectAction: 'Rotate45'` or `'Wrap'`

### Issue: Axis Range Too Small
**Solution:** Explicitly set `minimum` and `maximum` on axis

### Issue: DateTime Axis Not Formatting Correctly
**Solution:** Ensure data property is actual Date object, not string

### Issue: Secondary Axis Not Appearing
**Solution:** Verify series has `yAxisName` matching secondary axis name

---

## Performance Tips

- Use `majorGridLines: { width: 0 }` to hide grid for cleaner look
- Limit number of grid lines with `interval` setting
- Use `labelIntersectAction: 'Hide'` instead of rotating for dense labels
- For large datasets, use category axis instead of datetime (faster rendering)
