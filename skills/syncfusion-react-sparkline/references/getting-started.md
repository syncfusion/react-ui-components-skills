# Getting Started with React Sparkline

## Table of Contents
- [Overview](#overview)
- [Dependencies](#dependencies)
- [Installation and Configuration](#installation-and-configuration)
- [Creating a React Application](#creating-a-react-application)
- [Installing Syncfusion Sparkline Package](#installing-syncfusion-sparkline-package)
- [Adding Sparkline to Your Project](#adding-sparkline-to-your-project)
- [Module Injection](#module-injection)
- [Binding Data Source](#binding-data-source)
- [Changing Sparkline Type](#changing-sparkline-type)
- [Enabling Tooltip](#enabling-tooltip)
- [Complete Working Example](#complete-working-example)

## Overview

The Syncfusion React Sparkline is a lightweight chart component designed to visualize data trends in minimal space without axes or coordinates. This guide walks through the complete setup process from installation to creating your first functional sparkline.

**Key characteristics:**
- Consumes minimal space
- Simple, highly condensed data visualization
- Easy to interpret at a glance
- Perfect for dashboards and grid integration
- Supports 5 types: Line, Column, Area, Win-Loss, and Pie

## Dependencies

The Sparkline component requires the following minimum dependencies:

```
|-- @syncfusion/ej2-react-charts
    |-- @syncfusion/ej2-data
    |-- @syncfusion/ej2-svg-base
    |-- @syncfusion/ej2-react-base
    |-- @syncfusion/ej2-pdf-export
    |-- @syncfusion/ej2-file-utils
    |-- @syncfusion/ej2-compression
```

**Note:** Once you install `@syncfusion/ej2-react-charts`, all required dependencies will be installed automatically along with the main package.

## Installation and Configuration

### Creating a React Application

The recommended approach is to use **Vite** for React application setup, which provides:
- Faster development environment
- Smaller bundle sizes
- Optimized builds compared to create-react-app

#### TypeScript Environment

To set up a React application with TypeScript:

```bash
npm create vite@latest my-app -- --template react-ts
cd my-app
npm install
npm run dev
```

#### JavaScript Environment

To set up a React application with JavaScript:

```bash
npm create vite@latest my-app -- --template react
cd my-app
npm install
npm run dev
```

**Alternative:** You can also use `create-react-app` if preferred, but Vite is recommended for better performance.

### Installing Syncfusion Sparkline Package

All Syncfusion Essential JS 2 packages are published on the npmjs.com public registry.

To install the Sparkline package:

```bash
npm install @syncfusion/ej2-react-charts --save
```

**What the --save flag does:** Instructs NPM to include the Sparkline package inside the `dependencies` section of your `package.json` file.

## Adding Sparkline to Your Project

### Basic Component Setup

Add the SparklineComponent to your `src/App.tsx` (TypeScript) or `src/App.jsx` (JavaScript):

**TypeScript:**
```typescript
import { SparklineComponent } from '@syncfusion/ej2-react-charts';
import * as React from 'react';

function App() {
  return (<SparklineComponent></SparklineComponent>);
}

export default App;
```

**JavaScript:**
```javascript
import { SparklineComponent } from '@syncfusion/ej2-react-charts';
import * as React from 'react';

function App() {
  return (<SparklineComponent></SparklineComponent>);
}

export default App;
```

### Running the Application

Start the development server:

```bash
npm run dev
```

This command:
- Compiles your code
- Serves the application locally
- Opens it in the browser (typically at `http://localhost:5173`)

**Current state:** Since no data source has been specified, only an empty SVG element is rendered. The sparkline container exists but displays no shapes.

## Module Injection

Sparkline components are segregated into individual feature-wise modules. To use specific features, inject their corresponding service modules.

### Available Feature Services

| Service Name | Description |
|--------------|-------------|
| `SparklineTooltip` | Enables tooltip functionality for sparkline |

### Injecting Tooltip Module

To use tooltips, inject the `SparklineTooltip` module:

**TypeScript:**
```typescript
import { SparklineComponent, Inject, SparklineTooltip } from '@syncfusion/ej2-react-charts';
import * as React from "react";

function App() {
  return (
    <SparklineComponent>
      <Inject services={[SparklineTooltip]} />
    </SparklineComponent>
  );
}

export default App;
```

**JavaScript:**
```javascript
import { SparklineComponent, Inject, SparklineTooltip } from '@syncfusion/ej2-react-charts';
import * as React from "react";

function App() {
  return (
    <SparklineComponent>
      <Inject services={[SparklineTooltip]} />
    </SparklineComponent>
  );
}

export default App;
```

**Important:** Module injection must be done for each feature you want to use. Without injection, the feature will not be available.

## Binding Data Source

The `dataSource` property enables data binding for the sparkline. It accepts a collection of values as input.

### Simple Numeric Array

**TypeScript:**
```typescript
import { SparklineComponent } from '@syncfusion/ej2-react-charts';
import * as React from 'react';

function App() {
  const data: number[] = [0, 6, 4, 1, 3, 2, 5];
  
  return (
    <SparklineComponent
      dataSource={data}
      height="150px"
      width="300px"
    />
  );
}

export default App;
```

### Object Array with xName and yName

For more complex data structures, use `xName` and `yName` to specify which properties to use:

**TypeScript:**
```typescript
import { SparklineComponent } from '@syncfusion/ej2-react-charts';
import * as React from 'react';
import * as ReactDOM from 'react-dom';

interface DataPoint {
  x: number;
  y: number;
}

function App() {
  const data: DataPoint[] = [
    { x: 0, y: 0 },
    { x: 1, y: 6 },
    { x: 2, y: 4 },
    { x: 3, y: 1 },
    { x: 4, y: 3 },
    { x: 5, y: 2 },
    { x: 6, y: 5 }
  ];
  
  return (
    <SparklineComponent
      dataSource={data}
      xName="x"
      yName="y"
      height="150px"
      width="300px"
    />
  );
}

export default App;
ReactDOM.render(<App />, document.getElementById('charts'));
```

**JavaScript:**
```javascript
import { SparklineComponent } from '@syncfusion/ej2-react-charts';
import * as React from 'react';
import * as ReactDOM from 'react-dom';

function App() {
  const data = [
    { x: 0, y: 0 },
    { x: 1, y: 6 },
    { x: 2, y: 4 },
    { x: 3, y: 1 },
    { x: 4, y: 3 },
    { x: 5, y: 2 },
    { x: 6, y: 5 }
  ];
  
  return (
    <SparklineComponent
      dataSource={data}
      xName="x"
      yName="y"
      height="150px"
      width="300px"
    />
  );
}

export default App;
ReactDOM.render(<App />, document.getElementById('charts'));
```

## Changing Sparkline Type

The sparkline type can be configured using the `type` property. Supported types:
- `Line` - Continuous line chart (default)
- `Column` - Vertical bars
- `Area` - Filled area chart
- `WinLoss` - Binary outcome representation
- `Pie` - Circular proportional chart

### Area Type Example

**TypeScript:**
```typescript
import { SparklineComponent } from '@syncfusion/ej2-react-charts';
import * as React from 'react';
import * as ReactDOM from 'react-dom';

function App() {
  const data: number[] = [0, 6, 4, 1, 3, 2, 5];
  
  return (
    <SparklineComponent
      dataSource={data}
      type="Area"
      height="150px"
      width="300px"
      fill="#b2cfff"
    />
  );
}

export default App;
ReactDOM.render(<App />, document.getElementById('charts'));
```

**JavaScript:**
```javascript
import { SparklineComponent } from '@syncfusion/ej2-react-charts';
import * as React from 'react';
import * as ReactDOM from 'react-dom';

function App() {
  const data = [0, 6, 4, 1, 3, 2, 5];
  
  return (
    <SparklineComponent
      dataSource={data}
      type="Area"
      height="150px"
      width="300px"
      fill="#b2cfff"
    />
  );
}

export default App;
ReactDOM.render(<App />, document.getElementById('charts'));
```

### Column Type Example

```typescript
<SparklineComponent
  dataSource={[3, 6, 4, 1, 3, 2, 5]}
  type="Column"
  height="150px"
  width="300px"
/>
```

## Enabling Tooltip

The sparkline provides additional information through tooltips that appear when hovering over data points.

### Basic Tooltip Configuration

To enable tooltips:
1. Set `visible` to `true` in `tooltipSettings`
2. Inject the `SparklineTooltip` module

**TypeScript:**
```typescript
import { SparklineComponent, Inject, SparklineTooltip } from '@syncfusion/ej2-react-charts';
import * as React from 'react';
import * as ReactDOM from 'react-dom';

function App() {
  const data: number[] = [0, 6, 4, 1, 3, 2, 5];
  
  return (
    <SparklineComponent
      dataSource={data}
      type="Line"
      height="150px"
      width="300px"
      tooltipSettings={{
        visible: true,
        format: '${x} : ${y}'
      }}
    >
      <Inject services={[SparklineTooltip]} />
    </SparklineComponent>
  );
}

export default App;
ReactDOM.render(<App />, document.getElementById('charts'));
```

**JavaScript:**
```javascript
import { SparklineComponent, Inject, SparklineTooltip } from '@syncfusion/ej2-react-charts';
import * as React from 'react';
import * as ReactDOM from 'react-dom';

function App() {
  const data = [0, 6, 4, 1, 3, 2, 5];
  
  return (
    <SparklineComponent
      dataSource={data}
      type="Line"
      height="150px"
      width="300px"
      tooltipSettings={{
        visible: true,
        format: '${x} : ${y}'
      }}
    >
      <Inject services={[SparklineTooltip]} />
    </SparklineComponent>
  );
}

export default App;
ReactDOM.render(<App />, document.getElementById('charts'));
```

**Format placeholders:**
- `${x}` - X-axis value
- `${y}` - Y-axis value

## Complete Working Example

Here's a complete, production-ready example combining all the concepts:

**TypeScript:**
```typescript
import { SparklineComponent } from '@syncfusion/ej2-react-charts';
import * as React from 'react';
import * as ReactDOM from 'react-dom';

interface DataPoint {
  x: number;
  y: number;
}

function App() {
  const data: DataPoint[] = [
    { x: 0, y: 0 },
    { x: 1, y: 6 },
    { x: 2, y: 4 },
    { x: 3, y: 1 },
    { x: 4, y: 3 },
    { x: 5, y: 2 },
    { x: 6, y: 5 }
  ];
  
  return (
    <SparklineComponent
      dataSource={data}
      xName="x"
      yName="y"
      height="150px"
      width="300px"
    />
  );
}

export default App;
ReactDOM.render(<App />, document.getElementById('charts'));
```

**JavaScript:**
```javascript
import { SparklineComponent } from '@syncfusion/ej2-react-charts';
import * as React from 'react';
import * as ReactDOM from 'react-dom';

function App() {
  const data = [
    { x: 0, y: 0 },
    { x: 1, y: 6 },
    { x: 2, y: 4 },
    { x: 3, y: 1 },
    { x: 4, y: 3 },
    { x: 5, y: 2 },
    { x: 6, y: 5 }
  ];
  
  return (
    <SparklineComponent
      dataSource={data}
      xName="x"
      yName="y"
      height="150px"
      width="300px"
    />
  );
}

export default App;
ReactDOM.render(<App />, document.getElementById('charts'));
```

## Key Takeaways

1. **Installation:** Use `npm install @syncfusion/ej2-react-charts --save`
2. **Import:** Import `SparklineComponent` from `@syncfusion/ej2-react-charts`
3. **Data Binding:** Use `dataSource` property with arrays or objects
4. **Type Selection:** Set `type` property to Line, Column, Area, WinLoss, or Pie
5. **Module Injection:** Inject `SparklineTooltip` for tooltip functionality
6. **Sizing:** Use `height` and `width` properties for dimensions
7. **Tooltips:** Configure with `tooltipSettings` object

## Next Steps

- Learn about all 5 sparkline types and when to use each
- Explore appearance customization options (themes, colors, padding)
- Add data labels to highlight specific values
- Configure markers for start, end, high, low, and negative points
- Implement advanced features like range bands and axis customization
