# User Interaction

## Table of contents

- [Overview](#overview)
- [Tooltip Configuration](#tooltip-configuration)
  - [Basic Tooltip Setup](#basic-tooltip-setup)
  - [Tooltip with Format](#tooltip-with-format)
  - [Tooltip for Object Data](#tooltip-for-object-data)
- [Tooltip Customization](#tooltip-customization)
  - [Customization Properties](#customization-properties)
  - [Custom Tooltip Colors](#custom-tooltip-colors)
  - [Tooltip with Border](#tooltip-with-border)
  - [Professional Tooltip Styling](#professional-tooltip-styling)
- [Tooltip Templates](#tooltip-templates)
  - [Basic Tooltip Template](#basic-tooltip-template)
  - [Template with Icons](#template-with-icons)
  - [Template with Multiple Data Points](#template-with-multiple-data-points)
- [Track Line Feature](#track-line-feature)
  - [Basic Track Line](#basic-track-line)
  - [Track Line with Custom Color](#track-line-with-custom-color)
  - [Track Line Without Tooltip](#track-line-without-tooltip)
  - [Track Line Customization Properties](#track-line-customization-properties)
- [Complete Examples](#complete-examples)
  - [Interactive Dashboard Sparkline](#interactive-dashboard-sparkline)
  - [Column Chart with Tooltip](#column-chart-with-tooltip)
- [Best Practices](#best-practices)
- [Summary](#summary)

## Overview

The Syncfusion React Sparkline provides two primary user interaction features that enhance data exploration:

1. **Tooltip:** Display detailed information when hovering over data points
2. **Track Line:** Visual line that tracks mouse movement across the sparkline

Both features require the `SparklineTooltip` module to be injected.

## Tooltip Configuration

Tooltips provide additional context by showing data values when users hover over the sparkline.

### Basic Tooltip Setup

To enable tooltips, you must:
1. Inject the `SparklineTooltip` module
2. Set `visible` to `true` in `tooltipSettings`

```typescript
import { SparklineComponent, Inject, SparklineTooltip } from '@syncfusion/ej2-react-charts';
import * as React from 'react';
import * as ReactDOM from 'react-dom';

function SparklineWithTooltip() {
  const data: number[] = [0, 6, 4, 1, 3, 2, 5];
  
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

export default SparklineWithTooltip;
ReactDOM.render(<SparklineWithTooltip />, document.getElementById('charts'));
```

### Tooltip with Format

Use the `format` property to customize tooltip content:

```javascript
import { SparklineComponent, Inject, SparklineTooltip } from '@syncfusion/ej2-react-charts';
import * as React from 'react';
import * as ReactDOM from 'react-dom';

function TooltipWithFormat() {
  const dataSource=[
    { x: 0, xval: '2005', yval: 20090440 },
    { x: 1, xval: '2006', yval: 20264080 },
    { x: 2, xval: '2007', yval: 20434180 },
    { x: 3, xval: '2008', yval: 21007310 },
    { x: 4, xval: '2009', yval: 21262640 },
    { x: 5, xval: '2010', yval: 21515750 },
    { x: 6, xval: '2011', yval: 21766710 },
    { x: 7, xval: '2012', yval: 22015580 },
    { x: 8, xval: '2013', yval: 22262500 },
    { x: 9, xval: '2014', yval: 22507620 },
];
  
  return (
    <SparklineComponent
      dataSource={dataSource}
      xName='xval' yName='yval'
      type="Area"
      height="150px"
      width="300px"
      tooltipSettings={{
        visible: true,
        format: '${xval} : ${yval}'
      }}
    >
      <Inject services={[SparklineTooltip]} />
    </SparklineComponent>
  );
}

export default TooltipWithFormat;
ReactDOM.render(<TooltipWithFormat />, document.getElementById('charts'));
```

### Tooltip for Object Data

```javascript
import { SparklineComponent, Inject, SparklineTooltip } from '@syncfusion/ej2-react-charts';
import * as React from 'react';
import * as ReactDOM from 'react-dom';

function TooltipWithObjectData() {
  const data = [0, 6, 4, 1, 3, 2, 5];
  
  return (
    <SparklineComponent
      dataSource={data}
      xName="month"
      yName="sales"
      type="Column"
      height="150px"
      width="100%"
      tooltipSettings={{
        visible: true,
        format: '${month}: $${sales}K'
      }}
    >
      <Inject services={[SparklineTooltip]} />
    </SparklineComponent>
  );
}

export default TooltipWithObjectData;
ReactDOM.render(<TooltipWithObjectData />, document.getElementById('charts'));
```

## Tooltip Customization

Tooltips can be extensively customized to match your application's design.

### Customization Properties

| Property | Type | Description |
|----------|------|-------------|
| `visible` | boolean | Enable/disable tooltip |
| `format` | string | Format string with ${x} and ${y} placeholders |
| `fill` | string | Tooltip background color |
| `border` | object | Border configuration |
| `border.color` | string | Border color |
| `border.width` | number | Border width in pixels |
| `textStyle` | object | Text styling options |
| `textStyle.size` | string | Font size (e.g., '12px') |
| `textStyle.color` | string | Text color |
| `textStyle.fontWeight` | string | Font weight |
| `textStyle.fontFamily` | string | Font family |

### Custom Tooltip Colors

```javascript
import { SparklineComponent, Inject, SparklineTooltip } from '@syncfusion/ej2-react-charts';
import * as React from 'react';
import * as ReactDOM from 'react-dom';

function CustomColorTooltip() {
  const data = [0, 6, 4, 1, 3, 2, 5];
  
  return (
    <SparklineComponent
      dataSource={data}
      type="Line"
      height="150px"
      width="300px"
      fill="#4A90E2"
      lineWidth={2}
      tooltipSettings={{
        visible: true,
        format: 'Value: ${y}',
        fill: 'red',
        textStyle: {
          color: '#FFFFFF',
          size: '12px'
        }
      }}
    >
      <Inject services={[SparklineTooltip]} />
    </SparklineComponent>
  );
}

export default CustomColorTooltip;
ReactDOM.render(<CustomColorTooltip />, document.getElementById('charts'));
```

### Tooltip with Border

```typescript
function TooltipWithBorder() {
  const data: number[] = [3, 6, 4, 1, 3, 2, 5];
  
  return (
    <SparklineComponent
      dataSource={data}
      type="Column"
      height="150px"
      width="300px"
      fill="#4CAF50"
      tooltipSettings={{
        visible: true,
        format: '${x}: ${y}',
        fill: '#FFFFFF',
        border: {
          color: '#4CAF50',
          width: 2
        },
        textStyle: {
          color: '#333333',
          size: '13px',
          fontWeight: 'bold'
        }
      }}
    >
      <Inject services={[SparklineTooltip]} />
    </SparklineComponent>
  );
}
```

### Professional Tooltip Styling

```typescript
function ProfessionalTooltip() {
  const data = [3, 6, 4, 1, 3, 2, 5];

  return (
    <SparklineComponent
      dataSource={data}
      xName="week"
      yName="score"
      type="Area"
      height="150px"
      width="100%"
      fill="#4A90E2"
      opacity={0.4}
      tooltipSettings={{
        visible: true,
        format: '${week}: ${score}%',
        fill: '#2C3E50',
        border: {
          color: '#4A90E2',
          width: 1
        },
        textStyle: {
          color: '#ECF0F1',
          size: '12px',
          fontWeight: '500',
          fontFamily: 'Segoe UI, sans-serif'
        }
      }}
    >
      <Inject services={[SparklineTooltip]} />
    </SparklineComponent>
  );
}
```

## Tooltip Templates

For advanced customization, use tooltip templates to display custom HTML content.

### Basic Tooltip Template

```javascript
import {
  SparklineComponent,
  Inject,
  SparklineTooltip,
} from '@syncfusion/ej2-react-charts';
import * as React from 'react';
import * as ReactDOM from 'react-dom';

function TooltipTemplate() {
  const data = [0, 6, 4, 1, 3, 2, 5];

  return (
    <SparklineComponent
      id="sparkline"
      height="200px"
      width="500px"
      axisSettings={{
        minX: -1,
        maxX: 7,
        maxY: 8,
        minY: -1,
      }}
      fill="#033e96"
      valueType="Category"
      xName="x"
      yName="y"
      dataSource={[
        { x: 'Mon', y: 3 },
        { x: 'Tue', y: 5 },
        { x: 'Wed', y: 2 },
        { x: 'Thu', y: 4 },
        { x: 'Fri', y: 6 },
      ]}
      tooltipSettings={{
        visible: true,
        template:
          '<div id="sparktooltip" style="border-radius: 5px;background: #008cff;' +
          'color: #FFFFFF !important;font-size: 16px;font-style: italic;padding: 8px;">' +
          '${x} : ${y} </div>',
      }}
    >
      <Inject services={[SparklineTooltip]} />
    </SparklineComponent>
  );
}

export default TooltipTemplate;
ReactDOM.render(<TooltipTemplate />, document.getElementById('charts'));
```

### Template with Icons

```typescript
function TooltipWithIcon() {
  const revenueData = [
    { month: 'Jan', revenue: 45000, growth: 5 },
    { month: 'Feb', revenue: 52000, growth: 15 },
    { month: 'Mar', revenue: 48000, growth: -8 },
    { month: 'Apr', revenue: 60000, growth: 25 },
    { month: 'May', revenue: 58000, growth: -3 },
    { month: 'Jun', revenue: 68000, growth: 17 }
  ];
  
  const tooltipTemplate = (args: any) => {
    const point = revenueData[args.x];
    const isPositive = point.growth >= 0;
    
    return (
      <div style={{
        padding: '12px',
        background: '#FFFFFF',
        border: '2px solid #4A90E2',
        borderRadius: '6px',
        boxShadow: '0 4px 12px rgba(0,0,0,0.15)',
        minWidth: '180px'
      }}>
        <div style={{ 
          fontWeight: 'bold', 
          fontSize: '14px', 
          color: '#2C3E50',
          marginBottom: '8px'
        }}>
          {point.month} 2024
        </div>
        <div style={{ fontSize: '12px', color: '#555', marginBottom: '4px' }}>
          Revenue: <strong>${(point.revenue / 1000).toFixed(0)}K</strong>
        </div>
        <div style={{ 
          fontSize: '12px', 
          color: isPositive ? '#4CAF50' : '#F44336',
          fontWeight: 'bold'
        }}>
          {isPositive ? '▲' : '▼'} {Math.abs(point.growth)}%
        </div>
      </div>
    );
  };
  
  return (
    <SparklineComponent
      dataSource={revenueData}
      xName="month"
      yName="revenue"
      type="Area"
      height="150px"
      width="100%"
      fill="#4A90E2"
      opacity={0.4}
      tooltipSettings={{
        visible: true,
        template: tooltipTemplate
      }}
    >
      <Inject services={[SparklineTooltip]} />
    </SparklineComponent>
  );
}
```

### Template with Multiple Data Points

```typescript
function MultiDataTooltip() {
  const complexData = [
    { day: 'Mon', sales: 120, target: 100, satisfaction: 85 },
    { day: 'Tue', sales: 150, target: 120, satisfaction: 88 },
    { day: 'Wed', sales: 130, target: 110, satisfaction: 82 },
    { day: 'Thu', sales: 180, target: 150, satisfaction: 90 },
    { day: 'Fri', sales: 200, target: 180, satisfaction: 92 }
  ];
  
  const tooltipTemplate = (args: any) => {
    const point = complexData[args.x];
    const metTarget = point.sales >= point.target;
    
    return (
      <div style={{
        padding: '12px',
        background: '#2C3E50',
        color: '#ECF0F1',
        borderRadius: '6px',
        minWidth: '160px'
      }}>
        <div style={{ 
          fontWeight: 'bold', 
          fontSize: '14px', 
          marginBottom: '8px',
          borderBottom: '1px solid #ECF0F1',
          paddingBottom: '6px'
        }}>
          {point.day}
        </div>
        <div style={{ fontSize: '11px', marginBottom: '4px' }}>
          Sales: <strong>{point.sales}</strong>
        </div>
        <div style={{ fontSize: '11px', marginBottom: '4px' }}>
          Target: <strong>{point.target}</strong>
        </div>
        <div style={{ 
          fontSize: '11px', 
          marginBottom: '4px',
          color: metTarget ? '#4CAF50' : '#F44336'
        }}>
          Status: <strong>{metTarget ? '✓ Met' : '✗ Below'}</strong>
        </div>
        <div style={{ fontSize: '11px' }}>
          Satisfaction: <strong>{point.satisfaction}%</strong>
        </div>
      </div>
    );
  };
  
  return (
    <SparklineComponent
      dataSource={complexData}
      xName="day"
      yName="sales"
      type="Column"
      height="150px"
      width="100%"
      fill="#4A90E2"
      tooltipSettings={{
        visible: true,
        template: tooltipTemplate
      }}
    >
      <Inject services={[SparklineTooltip]} />
    </SparklineComponent>
  );
}
```

## Track Line Feature

The track line provides a visual indicator that follows the mouse position, helping users identify specific data points more easily.

### Basic Track Line

```typescript
import * as React from 'react';
import * as ReactDOM from 'react-dom';
import { SparklineComponent, Inject, SparklineTooltip } from '@syncfusion/ej2-react-charts';

function SparklineWithTrackLine() {
  const data = [0, 6, 4, 1, 3, 2, 5];
  
  return (
    <SparklineComponent
      dataSource={data}
      type="Line"
      height="150px"
      width="300px"
      tooltipSettings={{
        visible: true,
        trackLineSettings: {
          visible: true
        }
      }}
    >
      <Inject services={[SparklineTooltip]} />
    </SparklineComponent>
  );
}

export default SparklineWithTrackLine;
ReactDOM.render(<SparklineWithTrackLine />, document.getElementById('charts'));
```

**Note:** Track line requires the `SparklineTooltip` module to be injected, even if you're not showing tooltips.

### Track Line with Custom Color

```typescript
function CustomTrackLine() {
  const data: number[] = [3, 6, 4, 1, 3, 2, 5];
  
  return (
    <SparklineComponent
      dataSource={data}
      type="Area"
      height="150px"
      width="300px"
      fill="#4CAF50"
      opacity={0.4}
      tooltipSettings={{
        visible: true,
        format: '${x}: ${y}',
        trackLineSettings: {
          visible: true,
          color: '#F44336',
          width: 2
        }
      }}
    >
      <Inject services={[SparklineTooltip]} />
    </SparklineComponent>
  );
}
```

### Track Line Without Tooltip

```typescript
function TrackLineOnly() {
  const data: number[] = [0, 6, 4, 1, 3, 2, 5];
  
  return (
    <SparklineComponent
      dataSource={data}
      type="Line"
      height="150px"
      width="300px"
      fill="#2196F3"
      lineWidth={2}
      tooltipSettings={{
        visible: false,  // Tooltip disabled
        trackLineSettings: {
          visible: true,
          color: '#FF9800',
          width: 1
        }
      }}
    >
      <Inject services={[SparklineTooltip]} />
    </SparklineComponent>
  );
}
```

### Track Line Customization Properties

| Property | Type | Description | Default |
|----------|------|-------------|---------|
| `visible` | boolean | Enable/disable track line | false |
| `color` | string | Track line color | Theme-based (black for light themes, white for dark) |
| `width` | number | Track line width in pixels | 1 |

## Complete Examples

### Interactive Dashboard Sparkline

```typescript
function InteractiveDashboard() {
  const metrics = [
    { day: 'Mon', value: 150 },
    { day: 'Tue', value: 180 },
    { day: 'Wed', value: 165 },
    { day: 'Thu', value: 200 },
    { day: 'Fri', value: 195 },
    { day: 'Sat', value: 210 },
    { day: 'Sun', value: 185 }
  ];
  
  return (
    <div style={{ 
      padding: '20px', 
      background: '#F5F7FA', 
      borderRadius: '8px',
      boxShadow: '0 2px 8px rgba(0,0,0,0.1)'
    }}>
      <h3 style={{ margin: '0 0 15px 0', fontSize: '16px', color: '#2C3E50' }}>
        Weekly Metrics
      </h3>
      <SparklineComponent
        dataSource={metrics}
        xName="day"
        yName="value"
        type="Area"
        height="150px"
        width="100%"
        fill="#4A90E2"
        opacity={0.3}
        border={{ color: '#4A90E2', width: 2 }}
        markerSettings={{
          visible: ['All'],
          size: 6,
          fill: '#4A90E2',
          border: { color: '#FFFFFF', width: 2 }
        }}
        tooltipSettings={{
          visible: true,
          format: '${day}: ${value} units',
          fill: '#2C3E50',
          border: { color: '#4A90E2', width: 2 },
          textStyle: {
            color: '#ECF0F1',
            size: '12px',
            fontWeight: '500'
          },
          trackLineSettings: {
            visible: true,
            color: '#4A90E2',
            width: 2
          }
        }}
      >
        <Inject services={[SparklineTooltip]} />
      </SparklineComponent>
    </div>
  );
}
```

### Column Chart with Tooltip

```typescript
function DetailedColumnTooltip() {
  const data = [3, 6, 4, 1, 3, 2, 5];
  
  return (
    <SparklineComponent
      dataSource={data}
      type="Column"
      height="150px"
      width="300px"
      fill="#4CAF50"
      tooltipSettings={{
        visible: true,
        fill: '#FFFFFF',
        border: {
          color: '#4CAF50',
          width: 2
        },
        textStyle: {
          color: '#333333',
          size: '13px',
          fontWeight: 'bold'
        }
      }}
    >
      <Inject services={[SparklineTooltip]} />
    </SparklineComponent>
  );
}
```

## Best Practices

### 1. Always Inject SparklineTooltip

Both tooltips and track lines require module injection:

```typescript
// ✅ Correct
<SparklineComponent tooltipSettings={{ visible: true }}>
  <Inject services={[SparklineTooltip]} />
</SparklineComponent>

// ❌ Wrong - will not work
<SparklineComponent tooltipSettings={{ visible: true }}>
  {/* Missing Inject! */}
</SparklineComponent>
```

### 2. Provide Meaningful Tooltip Content

Add context and units to tooltip values:

```typescript
// ✅ Good: Clear context
tooltipSettings={{
  visible: true,
  format: '${month}: $${sales}K revenue'
}}

// ❌ Bad: No context
tooltipSettings={{
  visible: true,
  format: '${y}'
}}
```

### 3. Ensure Tooltip Readability

Use sufficient contrast between background and text:

```typescript
// ✅ Good: High contrast
tooltipSettings={{
  fill: '#333333',
  textStyle: { color: '#FFFFFF' }
}}

// ❌ Bad: Low contrast
tooltipSettings={{
  fill: '#CCCCCC',
  textStyle: { color: '#DDDDDD' }
}}
```

### 4. Use Templates for Complex Data

When showing multiple values, use templates instead of format strings:

```typescript
// ✅ Better: Template for complex data
const tooltipTemplate = (args: any) => (
  <div>
    <div>Sales: {args.sales}</div>
    <div>Target: {args.target}</div>
    <div>Variance: {args.variance}%</div>
  </div>
);

// ❌ Less effective: Format string for complex data
format: 'Sales: ${sales}, Target: ${target}, Variance: ${variance}%'
```

### 5. Coordinate Track Line with Theme

Match track line color to your sparkline theme:

```typescript
function ThemedTrackLine() {
  return (
    <SparklineComponent
      dataSource={data}
      fill="#4A90E2"
      tooltipSettings={{
        visible: true,
        trackLineSettings: {
          visible: true,
          color: '#4A90E2'  // Matches fill color
        }
      }}
    >
      <Inject services={[SparklineTooltip]} />
    </SparklineComponent>
  );
}
```

## Summary

- **Tooltip:** Enable with `tooltipSettings.visible` and inject `SparklineTooltip`
- **Format:** Use `format` property with `${x}` and `${y}` placeholders
- **Customize:** Control colors, borders, and text styles
- **Templates:** Create custom HTML tooltips for advanced scenarios
- **Track Line:** Enable with `trackLineSettings.visible` for visual guidance
- **Best Practices:** Inject module, provide context, ensure readability, use templates for complexity
