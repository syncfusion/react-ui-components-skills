# Advanced Features

## Table of Contents
- [Overview](#overview)
- [Axis Customization](#axis-customization)
  - [Value Types](#value-types)
  - [Min and Max Values](#min-and-max-values)
  - [Axis Line Customization](#axis-line-customization)
- [Range Bands](#range-bands)
  - [Single Range Band](#single-range-band)
  - [Multiple Range Bands](#multiple-range-bands)
- [Special Points Customization](#special-points-customization)
  - [Point Colors](#point-colors)
  - [Tie Point Color](#tie-point-color)
- [Sparkline Dimensions](#sparkline-dimensions)
  - [Container Size](#container-size)
  - [Fixed Dimensions](#fixed-dimensions)
  - [Percentage Dimensions](#percentage-dimensions)
- [Localization](#localization)

## Overview

This guide covers advanced sparkline features including axis customization, range bands, special point colors, dimension control, and localization. These features provide fine-grained control over sparkline behavior and appearance.

## Axis Customization

### Value Types

You can change the sparkline axis value type to handle different data formats.

#### Available Value Types

| Value Type | Description | Use Case |
|------------|-------------|----------|
| `Numeric` | Numeric values (default) | Sales figures, counts, measurements |
| `Category` | String categories | Product names, regions, labels |
| `DateTime` | Date and time values | Time-series data, trends over time |

#### DateTime Value Type

```typescript
import { SparklineComponent } from '@syncfusion/ej2-react-charts';
import * as React from 'react';

function DateTimeSparkline() {
  const data = [
    { date: new Date(2024, 0, 1), value: 100 },
    { date: new Date(2024, 1, 1), value: 120 },
    { date: new Date(2024, 2, 1), value: 110 },
    { date: new Date(2024, 3, 1), value: 140 },
    { date: new Date(2024, 4, 1), value: 130 },
    { date: new Date(2024, 5, 1), value: 160 }
  ];
  
  return (
    <SparklineComponent
      dataSource={data}
      xName="date"
      yName="value"
      valueType="DateTime"
      type="Line"
      height="150px"
      width="400px"
    />
  );
}

export default DateTimeSparkline;
```

#### Category Value Type

```typescript
function CategorySparkline() {
  const data = [
    { product: 'Product A', sales: 35 },
    { product: 'Product B', sales: 28 },
    { product: 'Product C', sales: 42 },
    { product: 'Product D', sales: 38 },
    { product: 'Product E', sales: 50 }
  ];
  
  return (
    <SparklineComponent
      dataSource={data}
      xName="product"
      yName="sales"
      valueType="Category"
      type="Column"
      height="150px"
      width="400px"
    />
  );
}
```

#### Numeric Value Type (Default)

```typescript
function NumericSparkline() {
  const data = [
    { x: 0, y: 100 },
    { x: 1, y: 120 },
    { x: 2, y: 110 },
    { x: 3, y: 140 },
    { x: 4, y: 130 }
  ];
  
  return (
    <SparklineComponent
      dataSource={data}
      xName="x"
      yName="y"
      valueType="Numeric"
      type="Area"
      height="150px"
      width="300px"
    />
  );
}
```

### Min and Max Values

Control the axis range by setting minimum and maximum values for both X and Y axes.

#### Setting Y-Axis Range

```typescript
import { SparklineComponent } from '@syncfusion/ej2-react-charts';
import * as React from 'react';

function YAxisRangeSparkline() {
  const data: number[] = [15, 23, 18, 30, 28, 35, 32];
  
  return (
    <SparklineComponent
      dataSource={data}
      type="Line"
      height="150px"
      width="300px"
      axisSettings={{
        minY: 0,
        maxY: 50
      }}
    />
  );
}

export default YAxisRangeSparkline;
```

#### Setting X-Axis Range

```typescript
function XAxisRangeSparkline() {
  const data = [
    { x: 5, y: 100 },
    { x: 10, y: 120 },
    { x: 15, y: 110 },
    { x: 20, y: 140 },
    { x: 25, y: 130 }
  ];
  
  return (
    <SparklineComponent
      dataSource={data}
      xName="x"
      yName="y"
      type="Area"
      height="150px"
      width="300px"
      axisSettings={{
        minX: 0,
        maxX: 30,
        minY: 80,
        maxY: 160
      }}
    />
  );
}
```

#### Use Cases for Min/Max

```typescript
// Normalize different datasets to same scale
function NormalizedComparison() {
  const dataset1: number[] = [15, 23, 18, 30, 28];
  
  const commonAxisSettings = {
    minY: 0,
    maxY: 100
  };
  
  return (
    <div style={{ display: 'flex', gap: '20px' }}>
      <SparklineComponent
        dataSource={dataset1}
        type="Line"
        height="150px"
        width="200px"
        axisSettings={commonAxisSettings}
      />
    </div>
  );
}
```

### Axis Line Customization

Customize or hide the axis line with the `axisSettings.lineSettings` property.

**Note:** Axis line customization is not applicable for Win-Loss sparklines.

#### Setting Axis Line Value

Define where the horizontal axis line appears:

```typescript
import { SparklineComponent } from '@syncfusion/ej2-react-charts';
import * as React from 'react';

function AxisLineValue() {
  const data: number[] = [3, 6, -2, 4, -1, 3, 5];
  
  return (
    <SparklineComponent
      dataSource={data}
      type="Column"
      height="150px"
      width="300px"
      axisSettings={{
        value: 0  // Axis line at y=0
      }}
    />
  );
}

export default AxisLineValue;
```

#### Custom Axis Line Appearance

```typescript
function CustomAxisLine() {
  const data: number[] = [5, 8, -3, 10, -2, 7, -5, 12];
  
  return (
    <SparklineComponent
      dataSource={data}
      type="Line"
      height="150px"
      width="300px"
      axisSettings={{
        value: 0,
        lineSettings: {
          visible: true,
          color: '#F44336',
          width: 2,
          opacity: 0.8
        }
      }}
    />
  );
}
```

#### Dashed Axis Line

```typescript
function DashedAxisLine() {
  const data: number[] = [3, 6, -2, 4, -1, 3, 5];
  
  return (
    <SparklineComponent
      dataSource={data}
      type="Area"
      height="150px"
      width="300px"
      axisSettings={{
        value: 0,
        lineSettings: {
          visible: true,
          color: '#666666',
          width: 1,
          dashArray: '5,5'  // Dashed line
        }
      }}
    />
  );
}
```

#### Hide Axis Line

```typescript
function HiddenAxisLine() {
  const data: number[] = [3, 6, 4, 1, 3, 2, 5];
  
  return (
    <SparklineComponent
      dataSource={data}
      type="Column"
      height="150px"
      width="300px"
      axisSettings={{
        lineSettings: {
          visible: false  // Hide axis line
        }
      }}
    />
  );
}
```

## Range Bands

Range bands highlight specific value ranges along the Y-axis, useful for showing target zones, acceptable ranges, or critical thresholds.

### Single Range Band

```typescript
import { SparklineComponent } from '@syncfusion/ej2-react-charts';
import * as React from 'react';

function SingleRangeBand() {
  const data: number[] = [0, 6, 4, 1, 3, 2, 5];
  
  return (
    <SparklineComponent
      dataSource={data}
      type="Area"
      height="150px"
      width="300px"
      fill="#4A90E2"
      opacity={0.6}
      rangeBandSettings={[
        {
          startRange: 1,
          endRange: 3,
          color: '#bfd4fc',
          opacity: 0.4
        }
      ]}
    />
  );
}

export default SingleRangeBand;
```

### Range Band Properties

| Property | Type | Description |
|----------|------|-------------|
| `startRange` | number | Starting Y-axis value for range band |
| `endRange` | number | Ending Y-axis value for range band |
| `color` | string | Range band fill color |
| `opacity` | number | Range band opacity (0-1) |

### Target Zone Range Band

```typescript
function TargetZone() {
  const performanceData: number[] = [65, 72, 68, 80, 85, 78, 92, 75, 88];
  
  return (
    <SparklineComponent
      dataSource={performanceData}
      type="Line"
      height="150px"
      width="400px"
      fill="#2196F3"
      lineWidth={2}
      rangeBandSettings={[
        {
          startRange: 70,
          endRange: 90,
          color: '#4CAF50',
          opacity: 0.2
        }
      ]}
    />
  );
}
```

### Multiple Range Bands

Define multiple range bands to highlight different zones:

```typescript
import { SparklineComponent } from '@syncfusion/ej2-react-charts';
import * as React from 'react';

function MultipleRangeBands() {
  const data: number[] = [0, 6, 4, 1, 3, 2, 5, 4, 7, 3, 8];
  
  return (
    <SparklineComponent
      dataSource={data}
      type="Area"
      height="180px"
      width="400px"
      fill="#2196F3"
      opacity={0.5}
      rangeBandSettings={[
        {
          startRange: 0,
          endRange: 3,
          color: '#F44336',
          opacity: 0.3
        },
        {
          startRange: 3,
          endRange: 6,
          color: '#FFC107',
          opacity: 0.3
        },
        {
          startRange: 6,
          endRange: 10,
          color: '#4CAF50',
          opacity: 0.3
        }
      ]}
    />
  );
}

export default MultipleRangeBands;
```

### Performance Zones Example

```typescript
function PerformanceZones() {
  const scores: number[] = [45, 62, 55, 78, 85, 92, 68, 88, 75, 95];
  
  return (
    <div style={{ padding: '20px' }}>
      <h3 style={{ margin: '0 0 10px 0' }}>
        Performance Scores
      </h3>
      <div style={{ display: 'flex', gap: '10px', marginBottom: '10px', fontSize: '12px' }}>
        <span style={{ color: '#F44336' }}>🔴 Below Average (0-60)</span>
        <span style={{ color: '#FFC107' }}>🟡 Average (60-80)</span>
        <span style={{ color: '#4CAF50' }}>🟢 Excellent (80-100)</span>
      </div>
      <SparklineComponent
        dataSource={scores}
        type="Line"
        height="150px"
        width="100%"
        fill="#2196F3"
        lineWidth={3}
        rangeBandSettings={[
          {
            startRange: 0,
            endRange: 60,
            color: '#F44336',
            opacity: 0.15
          },
          {
            startRange: 60,
            endRange: 80,
            color: '#FFC107',
            opacity: 0.15
          },
          {
            startRange: 80,
            endRange: 100,
            color: '#4CAF50',
            opacity: 0.15
          }
        ]}
        axisSettings={{
          minY: 0,
          maxY: 100
        }}
        markerSettings={{
          visible: ['All'],
          size: 6,
          fill: '#2196F3',
          border: { color: '#FFFFFF', width: 2 }
        }}
      />
    </div>
  );
}
```

## Special Points Customization

Customize colors for special data points to differentiate start, end, high, low, positive, and negative values.

**Note:** Special points customization is only applicable for Line, Column, and Area sparkline types.

### Point Colors

```typescript
import { SparklineComponent } from '@syncfusion/ej2-react-charts';
import * as React from 'react';

function SpecialPointColors() {
  const data: number[] = [5, 12, -3, 18, 8, -5, 15, 10, 20];
  
  return (
    <SparklineComponent
      dataSource={data}
      type="Line"
      height="150px"
      width="400px"
      fill="#673AB7"
      lineWidth={2}
      startPointColor="#2196F3"
      endPointColor="#FF9800"
      highPointColor="#4CAF50"
      lowPointColor="#F44336"
      negativePointColor="#E91E63"
      markerSettings={{
        visible: ['Start', 'End', 'High', 'Low', 'Negative'],
        size: 8
      }}
    />
  );
}

export default SpecialPointColors;
```

### Available Point Color Properties

| Property | Description | Applies To |
|----------|-------------|------------|
| `startPointColor` | Color for the first data point | Line, Column, Area |
| `endPointColor` | Color for the last data point | Line, Column, Area |
| `highPointColor` | Color for the highest value point | Line, Column, Area |
| `lowPointColor` | Color for the lowest value point | Line, Column, Area |
| `negativePointColor` | Color for negative value points | Line, Column, Area |
| `positivePointColor` | Color for positive value points | Line, Column, Area |

### Column with Positive/Negative Colors

```typescript
function PositiveNegativeColumn() {
  const data: number[] = [5, 8, -3, 10, -2, 7, -5, 12, 3, -1];
  
  return (
    <SparklineComponent
      dataSource={data}
      type="Column"
      height="150px"
      width="350px"
      positivePointColor="#4CAF50"
      negativePointColor="#F44336"
    />
  );
}
```

### Area with High/Low Emphasis

```typescript
function HighLowArea() {
  const data: number[] = [15, 23, 18, 30, 28, 35, 32, 25, 38];
  
  return (
    <SparklineComponent
      dataSource={data}
      type="Area"
      height="150px"
      width="400px"
      fill="#9C27B0"
      opacity={0.5}
      highPointColor="#4CAF50"
      lowPointColor="#F44336"
      markerSettings={{
        visible: ['High', 'Low'],
        size: 10,
        border: { color: '#FFFFFF', width: 2 }
      }}
    />
  );
}
```

### Tie Point Color

For Win-Loss sparklines, use `tiePointColor` to customize the color of zero-value points.

```typescript
import { SparklineComponent } from '@syncfusion/ej2-react-charts';
import * as React from 'react';

function WinLossTiePoints() {
  const results: number[] = [12, 15, -10, 13, 15, 6, -12, 17, 13, 0, 8, -10];
  
  return (
    <SparklineComponent
      dataSource={results}
      type="WinLoss"
      height="120px"
      width="400px"
      fill="#4CAF50"
      negativePointColor="#F44336"
      tiePointColor="#2196F3"
    />
  );
}

export default WinLossTiePoints;
```

### Win-Loss Color Explanation

```typescript
function WinLossExplained() {
  const data: number[] = [5, -3, 8, -2, 0, 7, -5, 10, 0, 3];
  
  return (
    <div style={{ padding: '20px' }}>
      <h3 style={{ margin: '0 0 10px 0' }}>Game Results</h3>
      <div style={{ display: 'flex', gap: '15px', marginBottom: '10px', fontSize: '12px' }}>
        <span>🟢 Win (positive)</span>
        <span>🔴 Loss (negative)</span>
        <span>🔵 Tie (zero)</span>
      </div>
      <SparklineComponent
        dataSource={data}
        type="WinLoss"
        height="120px"
        width="100%"
        fill="#4CAF50"
        negativePointColor="#F44336"
        tiePointColor="#2196F3"
      />
    </div>
  );
}
```

## Sparkline Dimensions

Control sparkline size through container dimensions, fixed pixel values, or percentage values.

### Container Size

Sparkline can adapt to its container's dimensions:

```typescript
import { SparklineComponent } from '@syncfusion/ej2-react-charts';
import * as React from 'react';

function ContainerSizeSparkline() {
  const data: number[] = [0, 6, 4, 1, 3, 2, 5];
  
  return (
    <div style={{ width: '650px', height: '350px' }}>
      <SparklineComponent
        dataSource={data}
        type="Line"
      />
    </div>
  );
}

export default ContainerSizeSparkline;
```

### Fixed Dimensions

#### Pixel Values

Set exact dimensions using pixel values:

```typescript
function PixelDimensions() {
  const data: number[] = [0, 6, 4, 1, 3, 2, 5];
  
  return (
    <SparklineComponent
      dataSource={data}
      type="Area"
      width="500px"
      height="200px"
    />
  );
}
```

#### Small Sparkline

```typescript
function SmallSparkline() {
  const data: number[] = [3, 6, 4, 1, 3, 2, 5];
  
  return (
    <SparklineComponent
      dataSource={data}
      type="Line"
      width="150px"
      height="60px"
      lineWidth={2}
    />
  );
}
```

#### Large Dashboard Sparkline

```typescript
function LargeDashboardSparkline() {
  const data: number[] = [15, 23, 18, 30, 28, 35, 32];
  
  return (
    <SparklineComponent
      dataSource={data}
      type="Area"
      width="800px"
      height="300px"
      fill="#4A90E2"
      opacity={0.4}
    />
  );
}
```

### Percentage Dimensions

Set dimensions as percentages of the container:

```typescript
function PercentageDimensions() {
  const data: number[] = [0, 6, 4, 1, 3, 2, 5];
  
  return (
    <div style={{ width: '800px', height: '400px' }}>
      <SparklineComponent
        dataSource={data}
        type="Column"
        width="100%"
        height="50%"
      />
    </div>
  );
}
```

**Percentage behavior:** Sparkline calculates its size relative to the container. For example, `height="50%"` renders the sparkline at half the container's height.

### Responsive Sparkline

```typescript
function ResponsiveSparkline() {
  const data: number[] = [3, 6, 4, 1, 3, 2, 5];
  
  return (
    <div style={{ width: '100%', maxWidth: '600px', height: '200px' }}>
      <SparklineComponent
        dataSource={data}
        type="Area"
        width="100%"
        height="100%"
        fill="#9C27B0"
        opacity={0.5}
      />
    </div>
  );
}
```

### Grid Cell Sparkline

```typescript
function GridCellSparkline() {
  const data: number[] = [5, 8, 3, 10, 6];
  
  return (
    <div style={{ 
      display: 'inline-block', 
      width: '120px', 
      height: '40px',
      verticalAlign: 'middle'
    }}>
      <SparklineComponent
        dataSource={data}
        type="Line"
        width="100%"
        height="100%"
        fill="#4CAF50"
        lineWidth={2}
        padding={{ left: 0, right: 0, top: 0, bottom: 0 }}
      />
    </div>
  );
}
```

## Localization

The sparkline component supports localization, particularly useful for tooltip formatting based on culture.

### Tooltip with Currency Format

```typescript
import {
  SparklineComponent,
  Inject,
  SparklineTooltip,
} from '@syncfusion/ej2-react-charts';
import { setCulture, L10n } from '@syncfusion/ej2-base';
import * as React from 'react';
import * as ReactDOM from 'react-dom';

function LocalizedSparkline() {
  // Set culture to German
  setCulture('de');

  const salesData = [
    { month: 'Jan', sales: 1200 },
    { month: 'Feb', sales: 1500 },
    { month: 'Mar', sales: 1300 },
    { month: 'Apr', sales: 1800 },
    { month: 'May', sales: 1600 },
  ];

  return (
    <SparklineComponent
      id="sparkline"
      height="200px"
      width="350px"
      containerArea={{
        border: { color: '#033e96', width: 2 },
      }}
      tooltipSettings={{
        visible: true,
      }}
      // To specify currency format
      format={'c0'}
      useGroupingSeparator={true}
      lineWidth={3}
      padding={{ left: 20, right: 20, bottom: 20, top: 20 }}
      border={{ color: '#033e96', width: 2 }}
      type="Area"
      fill="#b2cfff"
      dataSource={[30000, 60000, 40000, 10000, 30000, 20000, 50000]}
    >
      <Inject services={[SparklineTooltip]} />
    </SparklineComponent>
  );
}

export default LocalizedSparkline;
ReactDOM.render(<LocalizedSparkline />, document.getElementById('charts'));
```

### Multiple Cultures

```typescript
function MultiCultureExample() {
  const data: number[] = [1200, 1500, 1300, 1800, 1600];
  
  // For US locale
  setCulture('en-US');
  
  return (
    <SparklineComponent
      dataSource={data}
      type="Line"
      height="150px"
      width="300px"
      tooltipSettings={{
        visible: true
      }}
    >
      <Inject services={[SparklineTooltip]} />
    </SparklineComponent>
  );
}
```

## Summary

**Axis Customization:**
- Value types: Numeric, Category, DateTime
- Min/max control: axisSettings (minX, maxX, minY, maxY)
- Axis line: Customize color, width, opacity, dashArray

**Range Bands:**
- Highlight value ranges with startRange, endRange
- Multiple bands for different zones
- Useful for targets, thresholds, performance zones

**Special Points:**
- Customize colors: start, end, high, low, positive, negative
- Tie point color for Win-Loss sparklines
- Works with Line, Column, Area types

**Dimensions:**
- Container-based: Adapts to parent element
- Fixed: Pixel values (width="300px")
- Percentage: Relative to container (width="100%")

**Localization:**
- Culture-specific formatting in tooltips
- Currency and number format support
