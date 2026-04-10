# Data Labels

## Table of contents

- [Overview](#overview)
- [Enabling Data Labels](#enabling-data-labels)
  - [Show All Data Labels](#show-all-data-labels)
  - [Show Start and End Labels](#show-start-and-end-labels)
- [Visibility Options](#visibility-options)
  - [Available Visibility Options](#available-visibility-options)
  - [Combining Multiple Visibility Options](#combining-multiple-visibility-options)
  - [High and Low Points Only](#high-and-low-points-only)
  - [Negative Points Only](#negative-points-only)
- [Customizing Data Labels](#customizing-data-labels)
  - [Customization Properties](#customization-properties)
  - [Custom Fill and Border](#custom-fill-and-border)
  - [Transparent Background with Border](#transparent-background-with-border)
  - [Different Styles for Different Points](#different-styles-for-different-points)
- [Formatting Data Label Text](#formatting-data-label-text)
  - [Format Placeholders](#format-placeholders)
  - [Displaying X and Y Values](#displaying-x-and-y-values)
  - [Currency Format](#currency-format)
  - [Percentage Format](#percentage-format)
  - [Unit Suffix](#unit-suffix)
  - [Custom Prefix and Suffix](#custom-prefix-and-suffix)
- [Best Practices](#best-practices)
- [Summary](#summary)

## Overview

Data labels display values of data points directly on the sparkline, improving readability and providing immediate access to exact values. They are particularly useful when precise values matter more than just visual trends.

**Key capabilities:**
- Show values at specific points (All, Start, End, High, Low, Negative)
- Customize appearance (fill, border, opacity, text styles)
- Format text to display custom values and units
- Multiple visibility options for targeted display

## Enabling Data Labels

To enable data labels, configure the `dataLabelSettings` property with the `visible` option.

### Show All Data Labels

```typescript
import { SparklineComponent } from '@syncfusion/ej2-react-charts';
import * as React from 'react';
import * as ReactDOM from 'react-dom';

function SparklineWithAllLabels() {
  const data: number[] = [0, 6, 4, 1, 3, 2, 5];
  
  return (
    <SparklineComponent
      dataSource={data}
      type="Line"
      height="150px"
      width="300px"
      dataLabelSettings={{
        visible: ['All']
      }}
    />
  );
}

export default SparklineWithAllLabels;
ReactDOM.render(<SparklineWithAllLabels />, document.getElementById('charts'));
```

### Show Start and End Labels

```typescript
import { SparklineComponent } from '@syncfusion/ej2-react-charts';
import * as React from 'react';
import * as ReactDOM from 'react-dom';

function SparklineWithStartEnd() {
  const data: number[] = [0, 6, 4, 1, 3, 2, 5];
  
  return (
    <SparklineComponent
      dataSource={data}
      type="Area"
      height="150px"
      width="300px"
      dataLabelSettings={{
        visible: ['Start', 'End']
      }}
    />
  );
}

export default SparklineWithStartEnd;
ReactDOM.render(<SparklineWithStartEnd />, document.getElementById('charts'));
```

## Visibility Options

The `visible` property accepts an array of visibility options. You can combine multiple options:

### Available Visibility Options

| Option | Description | Use Case |
|--------|-------------|----------|
| `All` | Shows labels for all data points | When every value is important |
| `Start` | Shows label for the first point | Highlight starting value |
| `End` | Shows label for the last point | Highlight final value |
| `High` | Shows label for the highest value point | Emphasize peak performance |
| `Low` | Shows label for the lowest value point | Highlight minimum values |
| `Negative` | Shows labels for all negative value points | Identify losses or declines |

### Combining Multiple Visibility Options

```typescript
import { SparklineComponent } from '@syncfusion/ej2-react-charts';
import * as React from 'react';
import * as ReactDOM from 'react-dom';

function SparklineWithMultipleLabels() {
  const data: number[] = [3, 6, 4, 1, 3, 2, 5];
  
  return (
    <SparklineComponent
      dataSource={data}
      type="Column"
      height="150px"
      width="300px"
      dataLabelSettings={{
        visible: ['High', 'Low', 'Start', 'End']
      }}
    />
  );
}

export default SparklineWithMultipleLabels;
ReactDOM.render(<SparklineWithMultipleLabels />, document.getElementById('charts'));
```

### High and Low Points Only

```typescript
import { SparklineComponent } from '@syncfusion/ej2-react-charts';
import * as React from 'react';
import * as ReactDOM from 'react-dom';

function HighLowLabels() {
  const data: number[] = [12, 18, 8, 25, 15, 10, 22];
  
  return (
    <SparklineComponent
      dataSource={data}
      type="Line"
      height="150px"
      width="300px"
      dataLabelSettings={{
        visible: ['High', 'Low']
      }}
      highPointColor="#4CAF50"
      lowPointColor="#F44336"
    />
  );
}
export default HighLowLabels;
ReactDOM.render(<HighLowLabels />, document.getElementById('charts'));
```

### Negative Points Only

```typescript
import { SparklineComponent } from '@syncfusion/ej2-react-charts';
import * as React from 'react';
import * as ReactDOM from 'react-dom';

function NegativeLabels() {
  const data: number[] = [5, 8, -3, 10, -2, 7, -5, 12];
  
  return (
    <SparklineComponent
      dataSource={data}
      type="Column"
      height="150px"
      width="300px"
      dataLabelSettings={{
        visible: ['Negative']
      }}
      fill="#2196F3"
      negativePointColor="#F44336"
    />
  );
}

export default NegativeLabels;
ReactDOM.render(<NegativeLabels />, document.getElementById('charts'));
```

## Customizing Data Labels

Data labels can be extensively customized to match your design requirements.

### Customization Properties

| Property | Type | Description | Default |
|----------|------|-------------|---------|
| `fill` | string | Background color of label | - |
| `opacity` | number | Opacity of label background (0-1) | 1 |
| `border` | object | Border configuration | - |
| `border.color` | string | Border color | - |
| `border.width` | number | Border width in pixels | 0 |
| `textStyle` | object | Text styling options | - |
| `textStyle.size` | string | Font size (e.g., '12px') | - |
| `textStyle.color` | string | Text color | - |
| `textStyle.fontWeight` | string | Font weight (normal, bold) | - |
| `textStyle.fontFamily` | string | Font family | - |

### Custom Fill and Border

```typescript
import { SparklineComponent } from '@syncfusion/ej2-react-charts';
import * as React from 'react';
import * as ReactDOM from 'react-dom';

function CustomStyledLabels() {
  const data: number[] = [0, 6, 4, 1, 3, 2, 5];
  
  return (
    <SparklineComponent
      dataSource={data}
      type="Line"
      height="150px"
      width="300px"
      dataLabelSettings={{
        visible: ['All'],
        fill: '#2196F3',
        opacity: 0.6,
        border: {
          color: '#1976D2',
          width: 2
        },
        textStyle: {
          color: '#FFFFFF',
          size: '12px',
          fontWeight: 'bold'
        }
      }}
    />
  );
}

export default CustomStyledLabels;
ReactDOM.render(<CustomStyledLabels />, document.getElementById('charts'));
```

### Transparent Background with Border

```typescript
import { SparklineComponent } from '@syncfusion/ej2-react-charts';
import * as React from 'react';
import * as ReactDOM from 'react-dom';

function TransparentLabels() {
  const data = [3, 6, 4, 1, 3, 2, 5];
  
  return (
    <SparklineComponent
      dataSource={data}
      type="Column"
      height="150px"
      width="300px"
      dataLabelSettings={{
        visible: ['High', 'Low'],
        fill: 'transparent',
        border: {
          color: '#4CAF50',
          width: 1
        },
        textStyle: {
          color: '#4CAF50',
          size: '14px',
          fontWeight: 'bold'
        }
      }}
    />
  );
}

export default TransparentLabels;
ReactDOM.render(<TransparentLabels />, document.getElementById('charts'));
```

### Different Styles for Different Points

```typescript
import { SparklineComponent } from '@syncfusion/ej2-react-charts';
import * as React from 'react';
import * as ReactDOM from 'react-dom';

function MultiColorLabels() {
  const data = [5, 8, 3, 10, 6, 2, 9];
  
  return (
    <SparklineComponent
      dataSource={data}
      type="Area"
      height="150px"
      width="300px"
      dataLabelSettings={{
        visible: ['Start', 'End', 'High', 'Low'],
        fill: '#FFFFFF',
        opacity: 0.9,
        border: {
          color: '#333333',
          width: 1
        },
        textStyle: {
          color: '#333333',
          size: '11px',
          fontWeight: 'bold'
        }
      }}
      highPointColor="#4CAF50"
      lowPointColor="#F44336"
      startPointColor="#2196F3"
      endPointColor="#FF9800"
    />
  );
}

export default MultiColorLabels;
ReactDOM.render(<MultiColorLabels />, document.getElementById('charts'));
```

## Formatting Data Label Text

The `format` property allows you to customize how values are displayed in data labels, including units, prefixes, and custom formats.

### Format Placeholders

- `${x}` - X-axis value
- `${y}` - Y-axis value

### Displaying X and Y Values

```typescript
import { SparklineComponent } from '@syncfusion/ej2-react-charts';
import * as React from 'react';

import { SparklineComponent } from '@syncfusion/ej2-react-charts';
import * as React from 'react';
import * as ReactDOM from 'react-dom';

function FormattedLabels() {
  const data = [
    { month: 0, sales: 100 },
    { month: 1, sales: 150 },
    { month: 2, sales: 130 },
    { month: 3, sales: 180 },
    { month: 4, sales: 160 },
    { month: 5, sales: 200 }
  ];
  
  return (
    <SparklineComponent
      dataSource={data}
      xName="month"
      yName="sales"
      type="Line"
      height="150px"
      width="400px"
      dataLabelSettings={{
        visible: ['All'],
        format: 'M${month}: $${sales}K',
        fill: '#4A90E2',
        opacity: 0.8,
        textStyle: {
          color: '#FFFFFF',
          size: '10px'
        }
      }}
    />
  );
}

export default FormattedLabels;
ReactDOM.render(<FormattedLabels />, document.getElementById('charts'));
export default FormattedLabels;
```

**Output:** Labels show as "M0: $100K", "M1: $150K", etc.

### Currency Format

```typescript
import { SparklineComponent } from '@syncfusion/ej2-react-charts';
import * as React from 'react';
import * as ReactDOM from 'react-dom';

function CurrencyLabels() {
  const data = [
    { x: 'Mon', y: 3 },
    { x: 'Tue', y: 5 },
    { x: 'Wed', y: 2 },
    { x: 'Thu', y: 4 },
    { x: 'Fri', y: 6 },
  ];

  return (
    <SparklineComponent
      axisSettings={{
        minX: -1,
        maxX: 7,
        maxY: 7,
        minY: -1,
      }}
      dataSource={data}
      fill="blue"
      valueType="Category"
      xName="x"
      yName="y"
      type="Column"
      height="200px"
      width="500px"
      dataLabelSettings={{
        visible: ['All'],
        format: '$${y}',
        textStyle: {
          color: '#2E7D32',
          size: '12px',
          fontWeight: 'bold',
        },
      }}
    />
  );
}

export default CurrencyLabels;
ReactDOM.render(<CurrencyLabels />, document.getElementById('charts'));
```

### Percentage Format

```typescript
import { SparklineComponent } from '@syncfusion/ej2-react-charts';
import * as React from 'react';
import * as ReactDOM from 'react-dom';

function PercentageLabels() {
  const data = [
    { x: 'Mon', y: 3 },
    { x: 'Tue', y: 5 },
    { x: 'Wed', y: 2 },
    { x: 'Thu', y: 4 },
    { x: 'Fri', y: 6 },
  ];

  return (
    <SparklineComponent
      axisSettings={{
        minX: -1,
        maxX: 7,
        maxY: 7,
        minY: -1,
      }}
      dataSource={data}
      fill="blue"
      valueType="Category"
      xName="x"
      yName="y"
      type="Pie"
      height="200px"
      width="500px"
      dataLabelSettings={{
        visible: ['All'],
        format: '${y}%',
        textStyle: {
          color: '#2E7D32',
          size: '12px',
          fontWeight: 'bold',
        },
      }}
    />
  );
}

export default PercentageLabels;
ReactDOM.render(<PercentageLabels />, document.getElementById('charts'));
```

### Unit Suffix

```typescript
import { SparklineComponent } from '@syncfusion/ej2-react-charts';
import * as React from 'react';
import * as ReactDOM from 'react-dom';

function UnitLabels() {
  const data = [
    { x: 'Mon', y: 3 },
    { x: 'Tue', y: 5 },
    { x: 'Wed', y: 2 },
    { x: 'Thu', y: 4 },
    { x: 'Fri', y: 6 },
  ];

  return (
    <SparklineComponent
      axisSettings={{
        minX: -1,
        maxX: 7,
        maxY: 7,
        minY: -1,
      }}
      dataSource={data}
      fill="blue"
      valueType="Category"
      xName="x"
      yName="y"
      type="Area"
      height="200px"
      width="500px"
      dataLabelSettings={{
        visible: ['All'],
        format: '${y}°C',
        textStyle: {
          color: '#2E7D32',
          size: '12px',
          fontWeight: 'bold',
        },
      }}
    />
  );
}

export default UnitLabels;
ReactDOM.render(<UnitLabels />, document.getElementById('charts'));
```

### Custom Prefix and Suffix

```typescript
import { SparklineComponent } from '@syncfusion/ej2-react-charts';
import * as React from 'react';
import * as ReactDOM from 'react-dom';

function CustomFormatLabels() {
  const data = [
    { x: 'Mon', y: 3 },
    { x: 'Tue', y: 5 },
    { x: 'Wed', y: 2 },
    { x: 'Thu', y: 4 },
    { x: 'Fri', y: 6 },
  ];

  return (
    <SparklineComponent
      axisSettings={{
        minX: -1,
        maxX: 7,
        maxY: 7,
        minY: -1,
      }}
      dataSource={data}
      fill="blue"
      valueType="Category"
      xName="x"
      yName="y"
      type="Column"
      height="200px"
      width="500px"
      dataLabelSettings={{
        visible: ['All'],
        format: '${x}: ${y} visitors',
        textStyle: {
          color: '#2E7D32',
          size: '12px',
          fontWeight: 'bold',
        },
      }}
    />
  );
}

export default CustomFormatLabels;
ReactDOM.render(<CustomFormatLabels />, document.getElementById('charts'));
```

## Best Practices

### 1. Avoid Label Overcrowding

Don't show all labels if you have many data points - it creates visual clutter:

```typescript
// ❌ Bad: Too many labels
function TooManyLabels() {
  const data = Array.from({ length: 50 }, (_, i) => Math.random() * 100);
  
  return (
    <SparklineComponent
      dataSource={data}
      type="Line"
      height="150px"
      width="300px"
      dataLabelSettings={{ visible: ['All'] }} // Cluttered!
    />
  );
}

// ✅ Good: Selective labels
function SelectiveLabels() {
  const data = Array.from({ length: 50 }, (_, i) => Math.random() * 100);
  
  return (
    <SparklineComponent
      dataSource={data}
      type="Line"
      height="150px"
      width="300px"
      dataLabelSettings={{ visible: ['High', 'Low', 'End'] }} // Clear!
    />
  );
}
```

### 2. Ensure Readability

Use sufficient contrast and appropriate font sizes:

```typescript
function ReadableLabels() {
  const data: number[] = [3, 6, 4, 1, 3, 2, 5];
  
  return (
    <SparklineComponent
      dataSource={data}
      type="Column"
      height="150px"
      width="300px"
      fill="#2196F3"
      dataLabelSettings={{
        visible: ['All'],
        fill: '#FFFFFF',
        border: { color: '#1976D2', width: 1 },
        textStyle: {
          color: '#1976D2',
          size: '12px',  // Large enough to read
          fontWeight: 'bold'
        }
      }}
    />
  );
}
```

### 3. Use Format for Context

Always add units or context to make values meaningful:

```typescript
// ✅ Good: Clear units
<SparklineComponent
  dataSource={data}
  dataLabelSettings={{
    visible: ['All'],
    format: '${y} ms'  // Milliseconds
  }}
/>

// ❌ Bad: No context
<SparklineComponent
  dataSource={data}
  dataLabelSettings={{
    visible: ['All']  // Just numbers, unclear meaning
  }}
/>
```

### 4. Match Theme

Coordinate label colors with your sparkline theme:

```typescript
function ThemeMatchedLabels() {
  const data: number[] = [0, 6, 4, 1, 3, 2, 5];
  
  return (
    <SparklineComponent
      dataSource={data}
      type="Area"
      height="150px"
      width="300px"
      fill="#9C27B0"
      opacity={0.5}
      dataLabelSettings={{
        visible: ['High', 'Low'],
        fill: '#9C27B0',  // Match fill color
        opacity: 0.9,
        textStyle: {
          color: '#FFFFFF',
          size: '11px'
        }
      }}
    />
  );
}
```

### 5. Strategic Placement

Show labels only where they add value:

```typescript
// ✅ Good: Labels at key points
function StrategicLabels() {
  const data: number[] = [100, 105, 98, 110, 108, 115, 120];
  
  return (
    <SparklineComponent
      dataSource={data}
      type="Line"
      height="150px"
      width="300px"
      dataLabelSettings={{
        visible: ['Start', 'End'],  // Show trend: start vs end
        format: '${y}%'
      }}
    />
  );
}
```

## Summary

- **Enable:** Set `visible` in `dataLabelSettings` with options like All, Start, End, High, Low, Negative
- **Customize:** Use `fill`, `border`, `opacity`, and `textStyle` for appearance
- **Format:** Use `format` property with `${x}` and `${y}` placeholders for custom text
- **Best Practices:** Avoid overcrowding, ensure readability, add units, match theme
- **Strategic Use:** Show labels only where they provide value
