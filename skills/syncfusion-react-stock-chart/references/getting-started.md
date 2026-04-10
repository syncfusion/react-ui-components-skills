# Getting Started with Stock Charts

This guide covers the initial setup and basic implementation of Syncfusion React Stock Chart component.

> See the concise API summary in [API Reference](./api.md) for full prop/event details.

## Table of contents for Getting Started

- [Getting Started with Stock Charts](getting-started.md#getting-started-with-stock-charts)
- [Installation and Dependencies](getting-started.md#installation-and-dependencies)
  - [Required Package](getting-started.md#required-package)
- [Project Setup](getting-started.md#project-setup)
  - [Using Vite (Recommended)](getting-started.md#using-vite-recommended)
  - [Using Create React App (Alternative)](getting-started.md#using-create-react-app-alternative)
- [Basic Stock Chart Implementation](getting-started.md#basic-stock-chart-implementation)
  - [Minimal Example](getting-started.md#minimal-example)
- [Module Injection](getting-started.md#module-injection)
  - [Essential Modules for Stock Charts](getting-started.md#essential-modules-for-stock-charts)
- [Understanding Stock Data Structure](getting-started.md#understanding-stock-data-structure)
  - [Data Format](getting-started.md#data-format)
- [Creating Your First Stock Chart with Data](getting-started.md#creating-your-first-stock-chart-with-data)
  - [Complete Working Example](getting-started.md#complete-working-example)
  - [Key Configuration Points](getting-started.md#key-configuration-points)
- [Adding Chart Title](getting-started.md#adding-chart-title)
- [Adding Crosshair for Precise Reading](getting-started.md#adding-crosshair-for-precise-reading)
- [Adding Trackball for Multi-Series](getting-started.md#adding-trackball-for-multi-series)
- [Complete Starter Template](getting-started.md#complete-starter-template)
- [Next Steps](getting-started.md#next-steps)
- [Troubleshooting](getting-started.md#troubleshooting)

## Installation and Dependencies

### Required Package

Install the Stock Chart package from npm:

```bash
npm install @syncfusion/ej2-react-charts --save
```

Note: you may also need peer packages (installed automatically), and to import required CSS for chosen Syncfusion themes when using in a real app.

### Dependencies Tree

The `@syncfusion/ej2-react-charts` package includes these dependencies:

- `@syncfusion/ej2-base` - Base library
- `@syncfusion/ej2-data` - Data management
- `@syncfusion/ej2-charts` - Core charting library
- `@syncfusion/ej2-react-base` - React base
- `@syncfusion/ej2-pdf-export` - PDF export
- `@syncfusion/ej2-file-utils` - File utilities
- `@syncfusion/ej2-compression` - Compression
- `@syncfusion/ej2-svg-base` - SVG rendering
- `@syncfusion/ej2-navigations` - Navigation components
- `@syncfusion/ej2-calendars` - Calendar components
- `@syncfusion/ej2-popups` - Popup components
- `@syncfusion/ej2-lists` - List components
- `@syncfusion/ej2-inputs` - Input components
- `@syncfusion/ej2-buttons` - Button components
- `@syncfusion/ej2-splitbuttons` - Split button components

All dependencies are automatically installed with the main package.

## Project Setup

### Using Vite (Recommended)

Vite provides faster development and smaller bundle sizes. Create a new React project:

```bash
# Create new Vite project
npm create vite@latest my-stock-app

# Follow prompts to select React and JavaScript/TypeScript
# Navigate to project
cd my-stock-app

# Install dependencies
npm install

# Install Syncfusion Stock Chart
npm install @syncfusion/ej2-react-charts --save

# Start development server
npm run dev
```

**For TypeScript:**
```bash
npm create vite@latest my-stock-app -- --template react-ts
cd my-stock-app
npm install
npm install @syncfusion/ej2-react-charts --save
npm run dev
```

**For JavaScript:**
```bash
npm create vite@latest my-stock-app -- --template react
cd my-stock-app
npm install
npm install @syncfusion/ej2-react-charts --save
npm run dev
```

### Using Create React App (Alternative)

```bash
npx create-react-app my-stock-app
cd my-stock-app
npm install @syncfusion/ej2-react-charts --save
npm start
```

## Basic Stock Chart Implementation

### Minimal Example

Add the Stock Chart component to your `src/App.tsx` or `src/App.jsx`:

```typescript
import { StockChartComponent } from '@syncfusion/ej2-react-charts';
import * as React from 'react';
import * as ReactDOM from 'react-dom';

function App() {
  return <StockChartComponent />;
}

export default App;

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(<App />);
```

This creates an empty stock chart container. Now let's add data and features.

## Module Injection

Stock Chart features are segregated into individual modules. You must inject the modules you want to use.

### Essential Modules for Stock Charts

```typescript
import { 
  StockChartComponent, 
  Inject,
  CandleSeries,    // For candle series visualization
  Tooltip,         // For tooltips
  DataLabel,       // For data labels
  DateTime         // For datetime axis (required for stock data)
} from '@syncfusion/ej2-react-charts';

function App() {
  return (
    <StockChartComponent id='stockchart'>
      <Inject services={[CandleSeries, Tooltip, DataLabel, DateTime]} />
    </StockChartComponent>
  );
}
```

**Common modules to inject:**
- `CandleSeries` - Candle/candlestick charts
- `LineSeries` - Line charts
- `SplineSeries` - Spline/smooth line charts
- `HiloSeries` - High-Low range bars
- `HiloOpenCloseSeries` - OHLC bars
- `DateTime` - DateTime axis (essential for stock data)
- `Tooltip` - Hover tooltips
- `RangeTooltip` - Range selector tooltips
- `Crosshair` - Crosshair lines
- `DataLabel` - Data labels on points

## Understanding Stock Data Structure

Stock Chart expects data in a specific format with datetime and OHLC (Open, High, Low, Close) values.

### Data Format

```typescript
const stockData = [
  {
    x: new Date('2012-04-02T00:00:00.000Z'),  // Date/time
    open: 320.705719,                          // Opening price
    high: 324.074066,                          // Highest price
    low: 317.737732,                           // Lowest price
    close: 323.783783,                         // Closing price
    volume: 45638000                           // Trading volume
  },
  {
    x: new Date('2012-04-03T00:00:00.000Z'),
    open: 323.028015,
    high: 324.299286,
    low: 319.639648,
    close: 321.631622,
    volume: 40857000
  },
  // ... more data points
];
```

**Required fields for different series types:**
- **Candle/HiloOpenClose:** x, open, high, low, close
- **Hilo:** x, high, low
- **Line/Spline:** x, close (or any y-value field)
- **Volume (optional):** volume field for volume visualization

## Creating Your First Stock Chart with Data

### Complete Working Example

```typescript
import { 
  StockChartComponent, 
  StockChartSeriesCollectionDirective,
  StockChartSeriesDirective,
  Inject, 
  DateTime, 
  Tooltip, 
  RangeTooltip,
  Crosshair,
  CandleSeries 
} from '@syncfusion/ej2-react-charts';
import * as React from 'react';

function App() {
  const data = [
    {
      x: new Date('2012-04-02'),
      open: 320.705719,
      high: 324.074066,
      low: 317.737732,
      close: 323.783783,
      volume: 45638000
    },
    {
      x: new Date('2012-04-03'),
      open: 323.028015,
      high: 324.299286,
      low: 319.639648,
      close: 321.631622,
      volume: 40857000
    },
    {
      x: new Date('2012-04-04'),
      open: 319.544556,
      high: 319.819824,
      low: 315.865875,
      close: 317.892883,
      volume: 32519000
    },
    {
      x: new Date('2012-04-05'),
      open: 316.436432,
      high: 318.533539,
      low: 314.599609,
      close: 316.476471,
      volume: 46327000
    }
  ];

  return (
    <StockChartComponent
      id='stockchart'
      primaryXAxis={{ 
        valueType: 'DateTime'  // Essential for datetime data
      }}
    >
      <Inject services={[DateTime, Tooltip, RangeTooltip, Crosshair, CandleSeries]} />
      <StockChartSeriesCollectionDirective>
        <StockChartSeriesDirective
          dataSource={data}
          type='Candle'
          xName='x'      // Date field
          high='high'    // High price field
          low='low'      // Low price field
          open='open'    // Open price field
          close='close'  // Close price field
        />
      </StockChartSeriesCollectionDirective>
    </StockChartComponent>
  );
}

export default App;
```

### Key Configuration Points

1. **primaryXAxis with DateTime:** Always set `valueType: 'DateTime'` for stock data
2. **Series dataSource:** Bind your data array
3. **Series type:** Choose from Candle, Line, Spline, Hilo, HiloOpenClose
4. **Field mapping:** Map data fields to chart properties (xName, high, low, open, close)

## Adding Chart Title

Provide context with a descriptive title:

```typescript
<StockChartComponent
  id='stockchart'
  title='AAPL Stock Price'
  primaryXAxis={{ valueType: 'DateTime' }}
>
  {/* ... series configuration ... */}
</StockChartComponent>
```

## Adding Crosshair for Precise Reading

Crosshair adds vertical and horizontal lines to help users read exact values:

```typescript
<StockChartComponent
  id='stockchart'
  title='Stock Price with Crosshair'
  primaryXAxis={{ valueType: 'DateTime' }}
  crosshair={{ 
    enable: true,
    lineType: 'Both'  // Both vertical and horizontal lines
  }}
>
  <Inject services={[DateTime, Tooltip, RangeTooltip, Crosshair, CandleSeries]} />
  {/* ... series ... */}
</StockChartComponent>
```

## Adding Trackball for Multi-Series

Trackball shows a shared tooltip across all series at the mouse position:

```typescript
<StockChartComponent
  id='stockchart'
  primaryXAxis={{ valueType: 'DateTime' }}
  crosshair={{ enable: true }}
  tooltip={{ enable: true, shared: true }}  // Shared tooltip for trackball
>
  <Inject services={[DateTime, Tooltip, RangeTooltip, Crosshair, CandleSeries]} />
  {/* ... series ... */}
</StockChartComponent>
```

## Complete Starter Template

Here's a complete, production-ready starter template:

```typescript
import { 
  StockChartComponent, 
  StockChartSeriesCollectionDirective,
  StockChartSeriesDirective,
  Inject, 
  DateTime, 
  Tooltip, 
  RangeTooltip,
  Crosshair,
  LineSeries,
  SplineSeries,
  CandleSeries,
  HiloOpenCloseSeries,
  HiloSeries,
  RangeAreaSeries,
  Trendlines
} from '@syncfusion/ej2-react-charts';
import * as React from 'react';

function StockChartApp() {
  // Your stock data
  const stockData = [
    { x: new Date('2012-04-02'), open: 85.97, high: 90.58, low: 85.97, close: 90.58, volume: 660187068 },
    { x: new Date('2012-04-09'), open: 89.02, high: 92.50, low: 88.50, close: 92.90, volume: 490033264 },
    { x: new Date('2012-04-16'), open: 92.52, high: 93.00, low: 88.50, close: 92.52, volume: 450167016 },
    // Add more data points...
  ];

  return (
    <div style={{ padding: '20px' }}>
      <StockChartComponent
        id='stockchart'
        title='Stock Price Analysis'
        primaryXAxis={{ valueType: 'DateTime' }}
        crosshair={{ enable: true }}
        tooltip={{ enable: true }}
        height='450px'
      >
        <Inject services={[
          DateTime, Tooltip, RangeTooltip, Crosshair,
          LineSeries, SplineSeries, CandleSeries, 
          HiloOpenCloseSeries, HiloSeries, 
          RangeAreaSeries, Trendlines
        ]} />
        <StockChartSeriesCollectionDirective>
          <StockChartSeriesDirective
            dataSource={stockData}
            type='Candle'
            xName='x'
            high='high'
            low='low'
            open='open'
            close='close'
            volume='volume'
          />
        </StockChartSeriesCollectionDirective>
      </StockChartComponent>
    </div>
  );
}

export default StockChartApp;
```

## Next Steps

After getting your basic stock chart running:

1. **Explore different series types** - Try Line, Spline, Hilo, HiloOpenClose, Candle
2. **Add period selector** - Quick navigation buttons for date ranges (1D, 5D, 1M, etc.)
3. **Configure range selector** - Thumb-based range selection
4. **Add technical indicators** - SMA, EMA, RSI, MACD, Bollinger Bands
5. **Customize appearance** - Colors, themes, gradients
6. **Add stock events** - Mark significant events on the chart
7. **Enable export/print** - Allow users to save or print charts

## Troubleshooting

**Chart not displaying:**
- Ensure `DateTime` module is injected
- Verify `primaryXAxis.valueType` is set to `'DateTime'`
- Check data format (x field should be Date objects)
- Confirm series type module is injected (e.g., `CandleSeries` for candle charts)

**Data not showing:**
- Verify field mappings (xName, high, low, open, close)
- Check that data array is not empty
- Ensure date values are valid Date objects

**Console errors about modules:**
- Add missing module to Inject services array
- Import module from `@syncfusion/ej2-react-charts`
