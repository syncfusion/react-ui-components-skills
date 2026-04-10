# Sparkline Types

## Table of contents

- [Overview](#overview)
- [Line Type](#line-type)
  - [When to Use Line Type](#when-to-use-line-type)
  - [Basic Line Sparkline](#basic-line-sparkline)
  - [Line with Object Data](#line-with-object-data)
- [Column Type](#column-type)
  - [When to Use Column Type](#when-to-use-column-type)
  - [Basic Column Sparkline](#basic-column-sparkline)
  - [Column with Positive/Negative Values](#column-with-positivenegative-values)
  - [Column with Custom Colors](#column-with-custom-colors)
- [Area Type](#area-type)
  - [When to Use Area Type](#when-to-use-area-type)
  - [Basic Area Sparkline](#basic-area-sparkline)
  - [Area with Border](#area-with-border)
  - [Area with Gradient Effect](#area-with-gradient-effect)
- [Win-Loss Type](#win-loss-type)
  - [When to Use Win-Loss Type](#when-to-use-win-loss-type)
  - [Basic Win-Loss Sparkline](#basic-win-loss-sparkline)
  - [Win-Loss with Custom Colors](#win-loss-with-custom-colors)
  - [Win-Loss for Performance Tracking](#win-loss-for-performance-tracking)
- [Pie Type](#pie-type)
  - [When to Use Pie Type](#when-to-use-pie-type)
  - [Basic Pie Sparkline](#basic-pie-sparkline)
  - [Pie with Object Data](#pie-with-object-data)
- [Type Selection Guide](#type-selection-guide)
  - [Decision Tree](#decision-tree)
  - [Common Combinations](#common-combinations)
- [Summary](#summary)

## Overview

The Syncfusion React Sparkline supports five different types of visualizations, each optimized for specific data presentation needs. You can change the sparkline type by setting the `type` property.

**Supported types:**
- **Line:** Continuous trend visualization
- **Column:** Discrete value comparison
- **Area:** Filled trend visualization  
- **Win-Loss:** Binary outcome representation
- **Pie:** Proportional data display

Choose the appropriate type based on your data characteristics and the story you want to tell.

## Line Type

The Line type renders the sparkline series as a continuous line, ideal for showing trends over time or continuous data series.

### When to Use Line Type

- **Time-series data:** Stock prices, temperature readings, website traffic
- **Continuous trends:** Sales performance, user growth, server metrics
- **Smooth transitions:** Data where continuity between points matters
- **Change over time:** Any metric that evolves gradually

### Basic Line Sparkline

```typescript
import { SparklineComponent } from '@syncfusion/ej2-react-charts';
import * as React from 'react';
import * as ReactDOM from 'react-dom';

function LineSparkline() {
  const data: number[] = [0, 6, 4, 1, 3, 2, 5];
  
  return (
    <SparklineComponent
      dataSource={data}
      type="Line"
      height="150px"
      width="300px"
      fill="#2196F3"
      lineWidth={2}
    />
  );
}

export default LineSparkline;
ReactDOM.render(<LineSparkline />, document.getElementById('charts'));
```

### Line with Object Data

```javascript
import { SparklineComponent } from '@syncfusion/ej2-react-charts';
import * as React from 'react';
import * as ReactDOM from 'react-dom';

function LineSparklineAdvanced() {
  const data = [
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
      dataSource={data}
      xName="xval"
      yName="yval"
      type="Line"
      height="150px"
      width="100%"
      fill="#4CAF50"
      lineWidth={3}
    />
  );
}

export default LineSparklineAdvanced;
ReactDOM.render(<LineSparklineAdvanced />, document.getElementById('charts'));
```

## Column Type

The Column type renders the sparkline series as vertical bars, perfect for comparing discrete values.

### When to Use Column Type

- **Comparative analysis:** Monthly sales, quarterly revenue
- **Discrete data points:** Individual product performance
- **Category comparison:** Sales by region, items by category
- **Magnitude emphasis:** When exact values at each point matter

### Basic Column Sparkline

```typescript
import { SparklineComponent } from '@syncfusion/ej2-react-charts';
import * as React from 'react';
import * as ReactDOM from 'react-dom';

function ColumnSparkline() {
  const data = [3, 6, 4, 1, 3, 2, 5];
  
  return (
    <SparklineComponent
      dataSource={data}
      type="Column"
      height="150px"
      width="300px"
      fill="#FF9800"
    />
  );
}

export default ColumnSparkline;
ReactDOM.render(<ColumnSparkline />, document.getElementById('charts'));
```

### Column with Positive/Negative Values

```typescript
import { SparklineComponent } from '@syncfusion/ej2-react-charts';
import * as React from 'react';
import * as ReactDOM from 'react-dom';

function ColumnSparklineWithNegatives() {
  const data: number[] = [3, 6, -2, 4, -1, 3, 2];
  
  return (
    <SparklineComponent
      dataSource={data}
      type="Column"
      height="150px"
      width="300px"
      fill="#2196F3"
      negativePointColor="#F44336"
    />
  );
}

export default ColumnSparklineWithNegatives;
ReactDOM.render(<ColumnSparklineWithNegatives />, document.getElementById('charts'));
```

### Column with Custom Colors

```javascript
import { SparklineComponent } from '@syncfusion/ej2-react-charts';
import * as React from 'react';
import * as ReactDOM from 'react-dom';

function ColoredColumnSparkline() {
  const salesData = [
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
      dataSource={salesData}
      xName="xval"
      yName="yval"
      type="Column"
      height="150px"
      width="100%"
      fill="#9C27B0"
      highPointColor="#4CAF50"
      lowPointColor="#F44336"
    />
  );
}

export default ColoredColumnSparkline;
ReactDOM.render(<ColoredColumnSparkline />, document.getElementById('charts'));
```

## Area Type

The Area type renders the sparkline series as a filled area beneath a line, combining the benefits of line charts with visual emphasis on volume.

### When to Use Area Type

- **Volume emphasis:** When the magnitude of change is important
- **Cumulative data:** Total sales, accumulated metrics
- **Trend with context:** Showing both trend and scale
- **Visual impact:** More visually prominent than line charts

### Basic Area Sparkline

```typescript
import { SparklineComponent } from '@syncfusion/ej2-react-charts';
import * as React from 'react';
import * as ReactDOM from 'react-dom';

function AreaSparkline() {
  const data: number[] = [0, 6, 4, 1, 3, 2, 5];
  
  return (
    <SparklineComponent
      dataSource={data}
      type="Area"
      height="150px"
      width="300px"
      fill="#673AB7"
      opacity={0.6}
    />
  );
}

export default AreaSparkline;
ReactDOM.render(<AreaSparkline />, document.getElementById('charts'));
```

### Area with Border

```typescript
import { SparklineComponent } from '@syncfusion/ej2-react-charts';
import * as React from 'react';
import * as ReactDOM from 'react-dom';

function AreaSparklineWithBorder() {
  const data = [2, 8, 5, 3, 7, 4, 9];
  
  return (
    <SparklineComponent
      dataSource={data}
      type="Area"
      height="150px"
      width="300px"
      fill="#00BCD4"
      opacity={0.4}
      border={{ color: '#00BCD4', width: 2 }}
    />
  );
}

export default AreaSparklineWithBorder;
ReactDOM.render(<AreaSparklineWithBorder />, document.getElementById('charts'));
```

### Area with Gradient Effect

```typescript
import { SparklineComponent } from '@syncfusion/ej2-react-charts';
import * as React from 'react';
import * as ReactDOM from 'react-dom';

function GradientAreaSparkline() {
  const performanceData = [
    { week: 1, performance: 65 },
    { week: 2, performance: 72 },
    { week: 3, performance: 68 },
    { week: 4, performance: 80 },
    { week: 5, performance: 85 },
    { week: 6, performance: 78 },
    { week: 7, performance: 92 }
  ];
  
  return (
    <SparklineComponent
      dataSource={performanceData}
      xName="week"
      yName="performance"
      type="Area"
      height="150px"
      width="100%"
      fill="#E91E63"
      opacity={0.5}
    />
  );
}

export default GradientAreaSparkline;
ReactDOM.render(<GradientAreaSparkline />, document.getElementById('charts'));
```

## Win-Loss Type

The Win-Loss type renders the sparkline as a binary representation, showing positive values as wins and negative values as losses. Zero values are represented as ties.

### When to Use Win-Loss Type

- **Binary outcomes:** Win/loss records, pass/fail results
- **Performance tracking:** Above/below target performance
- **Gain/loss analysis:** Stock market gains and losses
- **Success metrics:** Hit/miss ratio, goal achievement

### Basic Win-Loss Sparkline

```typescript
import { SparklineComponent } from '@syncfusion/ej2-react-charts';
import * as React from 'react';
import * as ReactDOM from 'react-dom';

function WinLossSparkline() {
  const data: number[] = [12, 15, -10, 13, 15, 6, -12, 17, 13, 0, 8, -10];
  
  return (
    <SparklineComponent
      dataSource={data}
      type="WinLoss"
      height="150px"
      width="300px"
    />
  );
}

export default WinLossSparkline;
ReactDOM.render(<WinLossSparkline />, document.getElementById('charts'));
```

### Win-Loss with Custom Colors

```typescript
import { SparklineComponent } from '@syncfusion/ej2-react-charts';
import * as React from 'react';
import * as ReactDOM from 'react-dom';

function CustomWinLossSparkline() {
  const gameResults = [5, -3, 8, -2, 0, 7, -5, 10, 3, -1, 0, 6];
  
  return (
    <SparklineComponent
      dataSource={gameResults}
      type="WinLoss"
      height="150px"
      width="400px"
      fill="#4CAF50"          // Win color (positive values)
      negativePointColor="#F44336"  // Loss color (negative values)
      tiePointColor="#2196F3"       // Tie color (zero values)
    />
  );
}

export default CustomWinLossSparkline;
ReactDOM.render(<CustomWinLossSparkline />, document.getElementById('charts'));
```

### Win-Loss for Performance Tracking

```javascript
import { SparklineComponent } from '@syncfusion/ej2-react-charts';
import * as React from 'react';
import * as ReactDOM from 'react-dom';

function PerformanceWinLoss() {
  const dailyPerformance = [
    { x: 0, xval: '2005', yval: 20090440 },
    { x: 1, xval: '2006', yval: 20264080 },
    { x: 2, xval: '2007', yval: -20434180 },
    { x: 3, xval: '2008', yval: 21007310 },
    { x: 4, xval: '2009', yval: 21262640 },
    { x: 5, xval: '2010', yval: -21515750 },
    { x: 6, xval: '2011', yval: 21766710 },
    { x: 7, xval: '2012', yval: 22015580 },
    { x: 8, xval: '2013', yval: -22262500 },
    { x: 9, xval: '2014', yval: 22507620 },
];
  
  return (
    <SparklineComponent
      dataSource={dailyPerformance}
      xName="xval"
      yName="yval"
      type="WinLoss"
      height="150px"
      width="100%"
      fill="#8BC34A"
      negativePointColor="#FF5722"
      tiePointColor="#607D8B"
    />
  );
}

export default PerformanceWinLoss;
ReactDOM.render(<PerformanceWinLoss />, document.getElementById('charts'));
```

**Important Notes:**
- Positive values (> 0) are rendered as "win" bars
- Negative values (< 0) are rendered as "loss" bars
- Zero values (= 0) are rendered as "tie" bars
- All bars have equal height regardless of actual value magnitude

## Pie Type

The Pie type renders the sparkline as a circular chart, showing proportional relationships between values.

### When to Use Pie Type

- **Proportional data:** Market share, budget allocation
- **Part-to-whole relationships:** Component breakdown
- **Category distribution:** Product mix, demographic splits
- **Percentage representation:** Completion status, resource allocation

### Basic Pie Sparkline

```typescript
import { SparklineComponent } from '@syncfusion/ej2-react-charts';
import * as React from 'react';
import * as ReactDOM from 'react-dom';

function PieSparkline() {
  const data = [3, 6, 4, 1, 3];
  
  return (
    <SparklineComponent
      dataSource={data}
      type="Pie"
      height="150px"
      width="150px"
    />
  );
}

export default PieSparkline;
ReactDOM.render(<PieSparkline />, document.getElementById('charts'));
```

### Pie with Object Data

```javascript
import { SparklineComponent } from '@syncfusion/ej2-react-charts';
import * as React from 'react';
import * as ReactDOM from 'react-dom';

function PieSparklineWithCategories() {
  const marketShare = [
    { category: 'Product A', value: 35 },
    { category: 'Product B', value: 28 },
    { category: 'Product C', value: 20 },
    { category: 'Product D', value: 17 }
  ];
  
  return (
    <SparklineComponent
      dataSource={marketShare}
      xName="category"
      yName="value"
      type="Pie"
      height="200px"
      width="200px"
    />
  );
}

export default PieSparklineWithCategories;
ReactDOM.render(<PieSparklineWithCategories />, document.getElementById('charts'));
```

**Note:** Pie sparklines work best with 3-7 data points. Too many segments can make the chart hard to read in a small space.

## Type Selection Guide

Use this guide to choose the right sparkline type for your data:

| Sparkline Type | Best For | Data Characteristics | Visual Impact |
|----------------|----------|---------------------|---------------|
| **Line** | Trends over time | Continuous, time-series | Subtle, focuses on trend |
| **Column** | Discrete comparisons | Individual values matter | Moderate, emphasizes magnitude |
| **Area** | Volume and trends | Cumulative, volume-based | Strong, fills space |
| **Win-Loss** | Binary outcomes | Win/loss, pass/fail, above/below | Clear, binary distinction |
| **Pie** | Proportional relationships | Part-to-whole, percentages | Strong, circular emphasis |

### Decision Tree

**Is your data binary (win/loss, yes/no)?**
- Yes → Use **Win-Loss**
- No → Continue

**Do you need to show proportions of a whole?**
- Yes → Use **Pie**
- No → Continue

**Is your data continuous over time?**
- Yes → Use **Line** or **Area**
  - Need subtle trend → **Line**
  - Need emphasis on volume → **Area**
- No → Use **Column**

### Common Combinations

You can display multiple sparkline types together for comprehensive analysis:

```typescript
/**
 * Sparkline sample for series types
 */
import * as React from 'react';
import * as ReactDOM from 'react-dom';
import {
  SparklineComponent,
  Inject,
  SparklineTooltip,
} from '@syncfusion/ej2-react-charts';

const SAMPLE_CSS = `
      .control-fluid {
          padding: 0px !important;
      }
      td {
          font-family: "Roboto", "Segoe UI", "GeezaPro", "DejaVu Serif", "sans-serif";
          font-weight: 600;
      }
      .spark {
          border: 1px solid rgb(209, 209, 209);
          border-radius: 2px;
          width: 100%;
      }    
      .sparent {
          height: 110px;
          margin-left: 3%;
          margin-right: 0%;
      }
      .sparkpie {
          float: left;
          margin-top: 15px;
          margin-left: 2%;
      }
      .pieparent {
          width: 93%;
      } 
      .pietext {
          float: left;
          margin-left: 5%;
      }
      .sparktext {
           text-align: center;
           width: 100%;
      }`;

function Series() {
  return (
    <div className="control-pane">
      <style>{SAMPLE_CSS}</style>
      <div className="control-section">
        <div id="spark-container" className="row">
          <div className="cols-sample-area" style={{ marginTop: '8%' }}>
            <div className="col-lg-2 col-md-3 col-sm-5 sparent">
              <div className="spark" id="line">
                <SparklineComponent
                  id="spark1-container"
                  height="80px"
                  width="90%"
                  lineWidth={1}
                  type="Line"
                  valueType="Numeric"
                  fill="#3C78EF"
                  tooltipSettings={{
                    visible: true,
                    format: '${x} : ${yval}',
                    trackLineSettings: {
                      visible: true,
                    },
                  }}
                  markerSettings={{ visible: ['All'], size: 2.5, fill: 'blue' }}
                  dataSource={[
                    { x: 1, yval: 5 },
                    { x: 2, yval: 6 },
                    { x: 3, yval: 5 },
                    { x: 4, yval: 7 },
                    { x: 5, yval: 4 },
                    { x: 6, yval: 3 },
                    { x: 7, yval: 9 },
                    { x: 8, yval: 5 },
                    { x: 9, yval: 6 },
                    { x: 10, yval: 5 },
                    { x: 11, yval: 7 },
                    { x: 12, yval: 8 },
                    { x: 13, yval: 4 },
                    { x: 14, yval: 5 },
                    { x: 15, yval: 3 },
                    { x: 16, yval: 4 },
                    { x: 17, yval: 11 },
                    { x: 18, yval: 10 },
                    { x: 19, yval: 2 },
                    { x: 20, yval: 12 },
                    { x: 21, yval: 4 },
                    { x: 22, yval: 7 },
                    { x: 23, yval: 6 },
                    { x: 24, yval: 8 },
                  ]}
                  xName="x"
                  yName="yval"
                >
                  <Inject services={[SparklineTooltip]} />
                  <div
                    className="sparktext"
                    style={{
                      position: 'absolute',
                      marginTop: '90px',
                      marginLeft: '2px',
                      width: '100%',
                    }}
                  >
                    Power production for a day
                  </div>
                </SparklineComponent>
              </div>
            </div>
            <div className="col-lg-2 col-md-3 col-sm-5 sparent">
              <div className="spark" id="area">
                <SparklineComponent
                  id="spark2-container"
                  height="80px"
                  width="90%"
                  lineWidth={1}
                  type="Area"
                  valueType="Category"
                  fill="#b2cfff"
                  opacity={1}
                  border={{ color: '#3C78EF', width: 2 }}
                  axisSettings={{
                    lineSettings: {
                      visible: true,
                    },
                  }}
                  tooltipSettings={{
                    visible: true,
                    format: '${xval} : ${yval}°C',
                    trackLineSettings: {
                      visible: true,
                    },
                  }}
                  dataSource={[
                    { x: 1, xval: 'Jan', yval: 34 },
                    { x: 2, xval: 'Feb', yval: 36 },
                    { x: 3, xval: 'Mar', yval: 32 },
                    { x: 4, xval: 'Apr', yval: 35 },
                    { x: 5, xval: 'May', yval: 40 },
                    { x: 6, xval: 'Jun', yval: 38 },
                    { x: 7, xval: 'Jul', yval: 33 },
                    { x: 8, xval: 'Aug', yval: 37 },
                    { x: 9, xval: 'Sep', yval: 34 },
                    { x: 10, xval: 'Oct', yval: 31 },
                    { x: 11, xval: 'Nov', yval: 30 },
                    { x: 12, xval: 'Dec', yval: 29 },
                  ]}
                  xName="xval"
                  yName="yval"
                >
                  <Inject services={[SparklineTooltip]} />
                  <div
                    className="sparktext"
                    style={{
                      position: 'absolute',
                      marginTop: '90px',
                      marginLeft: '3px',
                      width: '100%',
                    }}
                  >
                    Average weather comparision
                  </div>
                </SparklineComponent>
              </div>
            </div>
            <div className="col-lg-2 col-md-3 col-sm-5 sparent">
              <div className="spark" id="column">
                <SparklineComponent
                  id="spark3-container"
                  height="80px"
                  width="90%"
                  lineWidth={1}
                  type="Column"
                  valueType="Category"
                  fill="#3C78EF"
                  highPointColor="#14aa21"
                  tooltipSettings={{
                    visible: true,
                    format: '${xval} : ${yval}',
                  }}
                  dataSource={[
                    { x: 1, xval: 'Jan', yval: 10 },
                    { x: 2, xval: 'Feb', yval: 6 },
                    { x: 3, xval: 'Mar', yval: 8 },
                    { x: 4, xval: 'Apr', yval: -5 },
                    { x: 5, xval: 'May', yval: 11 },
                    { x: 6, xval: 'Jun', yval: 5 },
                    { x: 7, xval: 'Jul', yval: -2 },
                    { x: 8, xval: 'Aug', yval: 7 },
                    { x: 9, xval: 'Sep', yval: -3 },
                    { x: 10, xval: 'Oct', yval: 6 },
                    { x: 11, xval: 'Nov', yval: 8 },
                    { x: 12, xval: 'Dec', yval: 10 },
                  ]}
                  xName="xval"
                  yName="yval"
                >
                  <Inject services={[SparklineTooltip]} />
                  <div
                    className="sparktext"
                    style={{
                      position: 'absolute',
                      marginTop: '90px',
                      marginLeft: '10px',
                      width: '100%',
                    }}
                  >
                    Revenue status
                  </div>
                </SparklineComponent>
              </div>
            </div>
            <div className="col-lg-2 col-md-5 col-sm-5 sparent">
              <div className="spark" id="winloss">
                <SparklineComponent
                  id="spark4-container"
                  height="80px"
                  width="90%"
                  lineWidth={1}
                  type="WinLoss"
                  valueType="Numeric"
                  fill="#3C78EF"
                  negativePointColor="#fc5070"
                  tooltipSettings={{
                    format: '${x} : ${y}',
                  }}
                  dataSource={[12, 15, -10, 13, 15, 6, -12, 17, 13, 0, 8, -10]}
                >
                  <Inject services={[SparklineTooltip]} />
                  <div
                    className="sparktext"
                    style={{
                      position: 'absolute',
                      marginTop: '90px',
                      marginLeft: '5px',
                      width: '100%',
                    }}
                  >
                    Customer satisfaction score
                  </div>
                </SparklineComponent>
              </div>
            </div>
            <div className="col-lg-2 col-md-5 col-sm-10 sparent">
              <div className="spark" style={{ height: '87px' }}>
                <SparklineComponent
                  className="sparkpie"
                  id="pie1"
                  style={{ height: '40px', width: '29%' }}
                  height="40px"
                  width="100%"
                  lineWidth={1}
                  type="Pie"
                  valueType="Category"
                  tooltipSettings={{
                    visible: true,
                    format: '${x} : ${y}',
                  }}
                  xName="x"
                  yName="y"
                  dataSource={[
                    { x: 'Gold', y: 46 },
                    { x: 'Silver', y: 37 },
                    { x: 'Bronze', y: 38 },
                  ]}
                >
                  <Inject services={[SparklineTooltip]} />
                  <div
                    className="pietext"
                    style={{
                      position: 'absolute',
                      marginTop: '40px',
                      width: '50%',
                    }}
                  >
                    USA
                  </div>
                </SparklineComponent>
                <SparklineComponent
                  className="sparkpie"
                  id="pie2"
                  style={{ height: '40px', width: '29%' }}
                  height="40px"
                  width="100%"
                  lineWidth={1}
                  type="Pie"
                  valueType="Category"
                  tooltipSettings={{
                    visible: true,
                    format: '${x} : ${y}',
                  }}
                  xName="x"
                  yName="y"
                  dataSource={[
                    { x: 'Gold', y: 27 },
                    { x: 'Silver', y: 23 },
                    { x: 'Bronze', y: 17 },
                  ]}
                >
                  <Inject services={[SparklineTooltip]} />
                  <div
                    className="pietext"
                    style={{
                      position: 'absolute',
                      marginTop: '40px',
                      width: '50%',
                    }}
                  >
                    GBR
                  </div>
                </SparklineComponent>
                <SparklineComponent
                  className="sparkpie"
                  id="pie3"
                  style={{ height: '40px', width: '29%' }}
                  height="40px"
                  width="100%"
                  lineWidth={1}
                  type="Pie"
                  valueType="Category"
                  tooltipSettings={{
                    visible: true,
                    format: '${x} : ${y}',
                  }}
                  xName="x"
                  yName="y"
                  dataSource={[
                    { x: 'Gold', y: 26 },
                    { x: 'Silver', y: 18 },
                    { x: 'Bronze', y: 26 },
                  ]}
                >
                  <Inject services={[SparklineTooltip]} />
                  <div
                    className="pietext"
                    style={{
                      position: 'absolute',
                      marginTop: '40px',
                      width: '50%',
                    }}
                  >
                    CHN
                  </div>
                </SparklineComponent>
                <div
                  className="sparktext"
                  style={{
                    position: 'absolute',
                    marginTop: '90px',
                    width: '90%',
                  }}
                >
                  Olympics medal details{' '}
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  );
}
export default Series;
ReactDOM.render(<Series />, document.getElementById('charts'));
```

## Summary

- **Line:** Best for continuous trends and time-series data
- **Column:** Best for discrete value comparisons
- **Area:** Best when volume and trend both matter
- **Win-Loss:** Best for binary outcomes and above/below comparisons
- **Pie:** Best for proportional and part-to-whole relationships

Choose the type that best communicates your data story to your users.
