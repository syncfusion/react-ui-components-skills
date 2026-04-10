# Markers

## Table of contents

- [Overview](#overview)
- [Adding Markers to Sparkline](#adding-markers-to-sparkline)
  - [Show All Markers](#show-all-markers)
  - [Show Start and End Markers](#show-start-and-end-markers)
- [Marker Visibility Options](#marker-visibility-options)
  - [Available Visibility Options](#available-visibility-options)
  - [Combining Multiple Visibility Options](#combining-multiple-visibility-options)
  - [High and Low Markers Only](#high-and-low-markers-only)
  - [Negative Point Markers](#negative-point-markers)
- [Special Point Markers](#special-point-markers)
  - [High and Low Point Emphasis](#high-and-low-point-emphasis)
  - [Start and End Point Markers](#start-and-end-point-markers)
  - [All Special Points](#all-special-points)
- [Customizing Markers](#customizing-markers)
  - [Customization Properties](#customization-properties)
  - [Custom Size and Fill](#custom-size-and-fill)
  - [Markers with Border](#markers-with-border)
  - [Large Highlighted Markers](#large-highlighted-markers)
  - [Semi-Transparent Markers](#semi-transparent-markers)
- [Complete Examples](#complete-examples)
  - [Professional Dashboard Markers](#professional-dashboard-markers)
  - [Minimal Line with End Marker](#minimal-line-with-end-marker)
  - [Column with High/Low Markers](#column-with-highlow-markers)
  - [Win-Loss with Markers](#win-loss-with-markers)
  - [Area Chart with Gradient Effect](#area-chart-with-gradient-effect)
- [Best Practices](#best-practices)
- [Summary](#summary)

## Overview

Markers are visual indicators placed on data points in the sparkline to highlight specific values. They are particularly useful for emphasizing key points like start, end, high, low, and negative values.

**Key capabilities:**
- Add markers to specific points or all points
- Customize marker size, fill color, border, and opacity
- Highlight special points (start, end, high, low, negative)
- Combine markers with other features like data labels

## Adding Markers to Sparkline

To add markers, configure the `markerSettings` property with the `visible` option.

### Show All Markers

```typescript
import { SparklineComponent } from '@syncfusion/ej2-react-charts';
import * as React from 'react';
import * as ReactDOM from 'react-dom';

function SparklineWithAllMarkers() {
  const data: number[] = [0, 6, 4, 1, 3, 2, 5];
  
  return (
    <SparklineComponent
      dataSource={data}
      type="Line"
      height="150px"
      width="300px"
      markerSettings={{
        visible: ['All']
      }}
    />
  );
}

export default SparklineWithAllMarkers;
ReactDOM.render(<SparklineWithAllMarkers />, document.getElementById('charts'));
```

### Show Start and End Markers

```typescript
import { SparklineComponent } from '@syncfusion/ej2-react-charts';
import * as React from 'react';
import * as ReactDOM from 'react-dom';

function SparklineWithStartEndMarkers() {
  const data: number[] = [0, 6, 4, 1, 3, 2, 5];
  
  return (
    <SparklineComponent
      dataSource={data}
      type="Area"
      height="150px"
      width="300px"
      fill="#4A90E2"
      opacity={0.4}
      markerSettings={{
        visible: ['Start', 'End']
      }}
    />
  );
}

export default SparklineWithStartEndMarkers;
ReactDOM.render(<SparklineWithStartEndMarkers />, document.getElementById('charts'));
```

## Marker Visibility Options

The `visible` property accepts an array of visibility options. You can combine multiple options to show markers at different points.

### Available Visibility Options

| Option | Description | Use Case |
|--------|-------------|----------|
| `All` | Shows markers for all data points | When every value needs emphasis |
| `Start` | Shows marker for the first point | Highlight starting value |
| `End` | Shows marker for the last point | Highlight final value |
| `High` | Shows marker for the highest value point | Emphasize peak performance |
| `Low` | Shows marker for the lowest value point | Highlight minimum values |
| `Negative` | Shows markers for all negative value points | Identify losses or declines |

### Combining Multiple Visibility Options

```typescript
import { SparklineComponent } from '@syncfusion/ej2-react-charts';
import * as React from 'react';
import * as ReactDOM from 'react-dom';

function SparklineWithMultipleMarkers() {
  const data: number[] = [3, 6, 4, 1, 3, 2, 5];
  
  return (
    <SparklineComponent
      dataSource={data}
      type="Line"
      height="150px"
      width="300px"
      markerSettings={{
        visible: ['High', 'Low', 'Start', 'End']
      }}
    />
  );
}

export default SparklineWithMultipleMarkers;
ReactDOM.render(<SparklineWithMultipleMarkers />, document.getElementById('charts'));
```

### High and Low Markers Only

```typescript
import { SparklineComponent } from '@syncfusion/ej2-react-charts';
import * as React from 'react';
import * as ReactDOM from 'react-dom';

function HighLowMarkers() {
  const data: number[] = [12, 18, 8, 25, 15, 10, 22];
  
  return (
    <SparklineComponent
      dataSource={data}
      type="Line"
      height="150px"
      width="300px"
      fill="#2196F3"
      lineWidth={2}
      markerSettings={{
        visible: ['High', 'Low']
      }}
      highPointColor="#4CAF50"
      lowPointColor="#F44336"
    />
  );
}

export default HighLowMarkers;
ReactDOM.render(<HighLowMarkers />, document.getElementById('charts'));
```

### Negative Point Markers

```typescript
import { SparklineComponent } from '@syncfusion/ej2-react-charts';
import * as React from 'react';
import * as ReactDOM from 'react-dom';

function NegativeMarkers() {
  const data: number[] = [5, 8, -3, 10, -2, 7, -5, 12];
  
  return (
    <SparklineComponent
      dataSource={data}
      type="Column"
      height="150px"
      width="300px"
      fill="#2196F3"
      negativePointColor="#F44336"
      markerSettings={{
        visible: ['Negative']
      }}
    />
  );
}

export default NegativeMarkers;
ReactDOM.render(<NegativeMarkers />, document.getElementById('charts'));
```

## Special Point Markers

Special point markers highlight specific data characteristics by applying different markers to start, end, high, low, and negative points.

### High and Low Point Emphasis

```typescript
import { SparklineComponent } from '@syncfusion/ej2-react-charts';
import * as React from 'react';
import * as ReactDOM from 'react-dom';

function HighLowEmphasis() {
  const salesData = [15, 23, 18, 30, 28, 35, 32];
  
  return (
    <SparklineComponent
      dataSource={salesData}
      type="Line"
      height="150px"
      width="300px"
      fill="#4A90E2"
      lineWidth={2}
      markerSettings={{
        visible: ['High', 'Low'],
        size: 8
      }}
      highPointColor="#4CAF50"
      lowPointColor="#F44336"
    />
  );
}

export default HighLowEmphasis;
ReactDOM.render(<HighLowEmphasis />, document.getElementById('charts'));
```

### Start and End Point Markers

```typescript
import { SparklineComponent } from '@syncfusion/ej2-react-charts';
import * as React from 'react';
import * as ReactDOM from 'react-dom';

function StartEndMarkers() {
  const data = [
    { month: 'Jan', value: 100 },
    { month: 'Feb', value: 120 },
    { month: 'Mar', value: 110 },
    { month: 'Apr', value: 140 },
    { month: 'May', value: 130 },
    { month: 'Jun', value: 160 }
  ];
  
  return (
    <SparklineComponent
      dataSource={data}
      xName="month"
      yName="value"
      type="Area"
      height="150px"
      width="100%"
      fill="#9C27B0"
      opacity={0.5}
      markerSettings={{
        visible: ['Start', 'End'],
        size: 10
      }}
      startPointColor="#2196F3"
      endPointColor="#FF9800"
    />
  );
}

export default StartEndMarkers;
ReactDOM.render(<StartEndMarkers />, document.getElementById('charts'));
```

### All Special Points

```typescript
import { SparklineComponent } from '@syncfusion/ej2-react-charts';
import * as React from 'react';
import * as ReactDOM from 'react-dom';

function AllSpecialPoints() {
  const data: number[] = [5, 12, -3, 18, 8, -5, 15, 10, 20];
  
  return (
    <SparklineComponent
      dataSource={data}
      type="Line"
      height="150px"
      width="400px"
      fill="#673AB7"
      lineWidth={2}
      markerSettings={{
        visible: ['Start', 'End', 'High', 'Low', 'Negative'],
        size: 7
      }}
      startPointColor="#2196F3"
      endPointColor="#FF9800"
      highPointColor="#4CAF50"
      lowPointColor="#F44336"
      negativePointColor="#E91E63"
    />
  );
}

export default AllSpecialPoints;
ReactDOM.render(<AllSpecialPoints />, document.getElementById('charts'));
```

## Customizing Markers

Markers can be extensively customized to match your design requirements.

### Customization Properties

| Property | Type | Description | Default |
|----------|------|-------------|---------|
| `size` | number | Marker size in pixels | 5 |
| `fill` | string | Marker fill color | - |
| `opacity` | number | Marker opacity (0-1) | 1 |
| `border` | object | Border configuration | - |
| `border.color` | string | Border color | - |
| `border.width` | number | Border width in pixels | 0 |

### Custom Size and Fill

```typescript
import { SparklineComponent } from '@syncfusion/ej2-react-charts';
import * as React from 'react';
import * as ReactDOM from 'react-dom';

function CustomMarkers() {
  const data: number[] = [0, 6, 4, 1, 3, 2, 5];
  
  return (
    <SparklineComponent
      dataSource={data}
      type="Line"
      height="150px"
      width="300px"
      fill="#2196F3"
      lineWidth={2}
      markerSettings={{
        visible: ['All'],
        size: 8,
        fill: '#FF9800',
        opacity: 0.8
      }}
    />
  );
}

export default CustomMarkers;
ReactDOM.render(<CustomMarkers />, document.getElementById('charts'));
```

### Markers with Border

```typescript
import { SparklineComponent } from '@syncfusion/ej2-react-charts';
import * as React from 'react';
import * as ReactDOM from 'react-dom';

function MarkersWithBorder() {
  const data: number[] = [3, 6, 4, 1, 3, 2, 5];
  
  return (
    <SparklineComponent
      dataSource={data}
      type="Area"
      height="150px"
      width="300px"
      fill="#4CAF50"
      opacity={0.4}
      markerSettings={{
        visible: ['All'],
        size: 10,
        fill: '#4CAF50',
        border: {
          color: '#FFFFFF',
          width: 2
        }
      }}
    />
  );
}

export default MarkersWithBorder;
ReactDOM.render(<MarkersWithBorder />, document.getElementById('charts'));
```

### Large Highlighted Markers

```typescript
import { SparklineComponent } from '@syncfusion/ej2-react-charts';
import * as React from 'react';
import * as ReactDOM from 'react-dom';

function LargeMarkers() {
  const data: number[] = [12, 18, 8, 25, 15, 10, 22];
  
  return (
    <SparklineComponent
      dataSource={data}
      type="Line"
      height="150px"
      width="300px"
      fill="#E91E63"
      lineWidth={3}
      markerSettings={{
        visible: ['High', 'Low'],
        size: 12,
        fill: '#FFFFFF',
        border: {
          color: '#E91E63',
          width: 3
        }
      }}
      highPointColor="#4CAF50"
      lowPointColor="#F44336"
    />
  );
}

export default LargeMarkers;
ReactDOM.render(<LargeMarkers />, document.getElementById('charts'));
```

### Semi-Transparent Markers

```typescript
import { SparklineComponent } from '@syncfusion/ej2-react-charts';
import * as React from 'react';
import * as ReactDOM from 'react-dom';

function TransparentMarkers() {
  const data: number[] = [5, 8, 3, 10, 6, 2, 9];
  
  return (
    <SparklineComponent
      dataSource={data}
      type="Column"
      height="150px"
      width="300px"
      fill="#2196F3"
      markerSettings={{
        visible: ['All'],
        size: 15,
        fill: '#FFC107',
        opacity: 0.6,
        border: {
          color: '#FF9800',
          width: 2
        }
      }}
    />
  );
}

export default TransparentMarkers;
ReactDOM.render(<TransparentMarkers />, document.getElementById('charts'));
```

## Complete Examples

### Professional Dashboard Markers

```typescript
import { SparklineComponent } from '@syncfusion/ej2-react-charts';
import * as React from 'react';
import * as ReactDOM from 'react-dom';

function DashboardSparkline() {
  const performanceData = [
    { week: 1, score: 65 },
    { week: 2, score: 72 },
    { week: 3, score: 68 },
    { week: 4, score: 80 },
    { week: 5, score: 85 },
    { week: 6, score: 78 },
    { week: 7, score: 92 }
  ];
  
  return (
    <div style={{ padding: '20px', background: '#F5F7FA', borderRadius: '8px' }}>
      <h3 style={{ margin: '0 0 10px 0', fontSize: '14px', color: '#333' }}>
        Weekly Performance Score
      </h3>
      <SparklineComponent
        dataSource={performanceData}
        xName="week"
        yName="score"
        type="Area"
        height="120px"
        width="100%"
        fill="#4A90E2"
        opacity={0.3}
        border={{ color: '#4A90E2', width: 2 }}
        markerSettings={{
          visible: ['All'],
          size: 6,
          fill: '#4A90E2',
          border: {
            color: '#FFFFFF',
            width: 2
          }
        }}
        highPointColor="#4CAF50"
        lowPointColor="#F44336"
      />
    </div>
  );
}

export default DashboardSparkline;
ReactDOM.render(<DashboardSparkline />, document.getElementById('charts'));
```

### Minimal Line with End Marker

```typescript
import { SparklineComponent } from '@syncfusion/ej2-react-charts';
import * as React from 'react';
import * as ReactDOM from 'react-dom';

function MinimalEndMarker() {
  const data: number[] = [8, 12, 6, 15, 10, 4, 14];
  
  return (
    <SparklineComponent
      dataSource={data}
      type="Line"
      height="100px"
      width="250px"
      fill="#000000"
      lineWidth={2}
      markerSettings={{
        visible: ['End'],
        size: 8,
        fill: '#000000',
        border: {
          color: '#FFFFFF',
          width: 2
        }
      }}
    />
  );
}

export default MinimalEndMarker;
ReactDOM.render(<MinimalEndMarker />, document.getElementById('charts'));
```

### Column with High/Low Markers

```typescript
import { SparklineComponent } from '@syncfusion/ej2-react-charts';
import * as React from 'react';
import * as ReactDOM from 'react-dom';

function ColumnHighLow() {
  const monthlyData: number[] = [35, 42, 28, 50, 38, 45, 32, 48, 40, 55, 36, 52];
  
  return (
    <SparklineComponent
      dataSource={monthlyData}
      type="Column"
      height="150px"
      width="100%"
      fill="#673AB7"
      markerSettings={{
        visible: ['High', 'Low'],
        size: 10,
        fill: '#FFFFFF',
        opacity: 1,
        border: {
          color: '#673AB7',
          width: 2
        }
      }}
      highPointColor="#4CAF50"
      lowPointColor="#F44336"
    />
  );
}

export default ColumnHighLow;
ReactDOM.render(<ColumnHighLow />, document.getElementById('charts'));
```

### Win-Loss with Markers

```typescript
import { SparklineComponent } from '@syncfusion/ej2-react-charts';
import * as React from 'react';
import * as ReactDOM from 'react-dom';

function WinLossWithMarkers() {
  const results: number[] = [5, -3, 8, -2, 0, 7, -5, 10, 3, -1];
  
  return (
    <SparklineComponent
      dataSource={results}
      type="WinLoss"
      height="120px"
      width="350px"
      fill="#4CAF50"
      negativePointColor="#F44336"
      tiePointColor="#FFC107"
      markerSettings={{
        visible: ['All'],
        size: 6,
        fill: '#333333',
        opacity: 0.7
      }}
    />
  );
}

export default WinLossWithMarkers;
ReactDOM.render(<WinLossWithMarkers />, document.getElementById('charts'));
```

### Area Chart with Gradient Effect

```typescript
import { SparklineComponent } from '@syncfusion/ej2-react-charts';
import * as React from 'react';
import * as ReactDOM from 'react-dom';

function GradientAreaWithMarkers() {
  const trendData: number[] = [15, 22, 18, 28, 32, 25, 35, 30, 42];
  
  return (
    <SparklineComponent
      dataSource={trendData}
      type="Area"
      height="180px"
      width="100%"
      fill="#E91E63"
      opacity={0.5}
      border={{ color: '#C2185B', width: 2 }}
      markerSettings={{
        visible: ['Start', 'End', 'High'],
        size: 9,
        fill: '#FFFFFF',
        border: {
          color: '#C2185B',
          width: 2
        }
      }}
      highPointColor="#4CAF50"
      startPointColor="#2196F3"
      endPointColor="#FF9800"
      containerArea={{
        background: '#FCE4EC',
        border: { color: '#F06292', width: 1 }
      }}
      padding={{
        left: 15,
        right: 15,
        top: 15,
        bottom: 15
      }}
    />
  );
}

export default GradientAreaWithMarkers;
ReactDOM.render(<GradientAreaWithMarkers />, document.getElementById('charts'));
```

## Best Practices

### 1. Strategic Marker Placement

Don't show markers for every point when you have many data points:

```typescript
// ❌ Bad: Too many markers
function TooManyMarkers() {
  const data = Array.from({ length: 100 }, (_, i) => Math.random() * 100);
  
  return (
    <SparklineComponent
      dataSource={data}
      type="Line"
      height="150px"
      width="300px"
      markerSettings={{ visible: ['All'] }} // Cluttered!
    />
  );
}

// ✅ Good: Strategic markers
function StrategicMarkers() {
  const data = Array.from({ length: 100 }, (_, i) => Math.random() * 100);
  
  return (
    <SparklineComponent
      dataSource={data}
      type="Line"
      height="150px"
      width="300px"
      markerSettings={{ visible: ['High', 'Low', 'End'] }} // Clear!
    />
  );
}
```

### 2. Size Proportional to Sparkline

Choose marker sizes appropriate for your sparkline dimensions:

```typescript
// Small sparkline = smaller markers
function SmallSparkline() {
  const data: number[] = [3, 6, 4, 1, 3, 2, 5];
  
  return (
    <SparklineComponent
      dataSource={data}
      type="Line"
      height="60px"
      width="150px"
      markerSettings={{
        visible: ['High', 'Low'],
        size: 4  // Small markers for small sparkline
      }}
    />
  );
}

// Large sparkline = larger markers
function LargeSparkline() {
  const data: number[] = [3, 6, 4, 1, 3, 2, 5];
  
  return (
    <SparklineComponent
      dataSource={data}
      type="Line"
      height="200px"
      width="500px"
      markerSettings={{
        visible: ['High', 'Low'],
        size: 12  // Larger markers for large sparkline
      }}
    />
  );
}
```

### 3. Contrast for Visibility

Ensure markers stand out against the sparkline:

```typescript
// ✅ Good: High contrast
function HighContrastMarkers() {
  const data: number[] = [0, 6, 4, 1, 3, 2, 5];
  
  return (
    <SparklineComponent
      dataSource={data}
      type="Area"
      height="150px"
      width="300px"
      fill="#2196F3"
      opacity={0.4}
      markerSettings={{
        visible: ['All'],
        size: 8,
        fill: '#2196F3',
        border: {
          color: '#FFFFFF',  // White border for visibility
          width: 2
        }
      }}
    />
  );
}
```

### 4. Combine with Point Colors

Use special point colors to enhance marker visibility:

```typescript
function ColoredMarkers() {
  const data: number[] = [5, 12, 3, 18, 8, 2, 15];
  
  return (
    <SparklineComponent
      dataSource={data}
      type="Line"
      height="150px"
      width="300px"
      fill="#673AB7"
      lineWidth={2}
      markerSettings={{
        visible: ['Start', 'End', 'High', 'Low'],
        size: 8,
        fill: '#FFFFFF',
        border: { width: 2 }  // Border color from point colors
      }}
      startPointColor="#2196F3"
      endPointColor="#FF9800"
      highPointColor="#4CAF50"
      lowPointColor="#F44336"
    />
  );
}
```

### 5. Accessibility Considerations

Ensure markers are visible for users with color vision deficiencies:

```typescript
function AccessibleMarkers() {
  const data: number[] = [3, 6, 4, 1, 3, 2, 5];
  
  return (
    <SparklineComponent
      dataSource={data}
      type="Line"
      height="150px"
      width="300px"
      fill="#0066CC"
      lineWidth={3}
      markerSettings={{
        visible: ['All'],
        size: 10,
        fill: '#FFFFFF',
        border: {
          color: '#0066CC',
          width: 3  // Thick border for visibility
        }
      }}
    />
  );
}
```

### 6. Combine Markers with Data Labels

For maximum clarity, combine markers with data labels:

```typescript
import { SparklineComponent } from '@syncfusion/ej2-react-charts';
import * as React from 'react';
import * as ReactDOM from 'react-dom';

function MarkersWithLabels() {
  const data = [0, 6, 4, 1, 3, 2, 5];
  
  return (
    <SparklineComponent
      dataSource={data}
      type="Area"
      height="150px"
      width="300px"
      fill="#4A90E2"
      opacity={0.4}
      markerSettings={{
        visible: ['High', 'Low'],
        size: 8,
        fill: '#4A90E2',
        border: { color: '#FFFFFF', width: 2 }
      }}
      dataLabelSettings={{
        visible: ['High', 'Low'],
        fill: '#FFFFFF',
        border: { color: '#4A90E2', width: 1 },
        textStyle: { color: '#4A90E2', size: '11px', fontWeight: 'bold' }
      }}
      highPointColor="#4CAF50"
      lowPointColor="#F44336"
    />
  );
}

export default MarkersWithLabels;
ReactDOM.render(<MarkersWithLabels />, document.getElementById('charts'));
```

## Summary

- **Enable:** Set `visible` in `markerSettings` with options like All, Start, End, High, Low, Negative
- **Customize:** Use `size`, `fill`, `opacity`, `border` for appearance
- **Special Points:** Combine with point colors (highPointColor, lowPointColor, etc.)
- **Best Practices:** Strategic placement, proportional sizing, high contrast, accessibility
- **Combinations:** Works well with data labels, tooltips, and other features
