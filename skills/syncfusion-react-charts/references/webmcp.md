# React Chart WebMCP AI Sample Generation Guide

## Table of Contents

- [Purpose](#purpose)
- [Required rules](#required-rules)
- [Complete React feature-tour sample](#complete-react-feature-tour-sample)
- [How to use WebMCP in React](#how-to-use-webmcp-in-react)
- [Complete sample: Enable all built-in WebMCP tools](#complete-sample-enable-all-built-in-webmcp-tools)
- [Complete sample: Enable only specific built-in WebMCP tools](#complete-sample-enable-only-specific-built-in-webmcp-tools)
- [Complete sample: Add custom WebMCP tools](#complete-sample-add-custom-webmcp-tools)
- [Complete sample: Use built-in tools and custom tools together](#complete-sample-use-built-in-tools-and-custom-tools-together)
- [Directly retrieve and execute a WebMCP tool](#directly-retrieve-and-execute-a-webmcp-tool)
- [Confirmation, cancellation, and validation](#confirmation-cancellation-and-validation)
- [Module-selection rules](#module-selection-rules)
- [Required generation scenarios](#required-generation-scenarios)
- [Exact built-in tool names](#exact-built-in-tool-names)
- [Spoon-fed tool-by-tool React implementation](#spoon-fed-tool-by-tool-react-implementation)
- [Final copy rules](#final-copy-rules)
- [AI validation checklist](#ai-validation-checklist)

## Purpose

Use this reference to generate complete React Chart samples with WebMCP. Registration is automatic after rendering. Do not generate a manual registration button or call `registerWebMcpTools()`.

## Required rules

- Import and inject `WebMcpChart`.
- Set `enableWebMcp={true}`.
- Give `ChartComponent` a stable ID.
- Omit `webMcpSettings.tools` to register all built-ins.
- Use a string array to restrict registration to selected built-in names.
- Use the same `webMcpSettings.tools` property for custom tools. There is no separate `customTools` setting.
- To combine selected built-in and custom tools, retrieve the built-in tool objects with `getWebMcpTools()`, append the custom tool objects, and assign the combined array to `webMcpSettings.tools`.
- Use `getWebMcpTools()` for diagnostics, testing, direct execution, or to retrieve built-in tool objects before combining them with custom tools.
- Use `beforeWebMcpToolExecute` to validate, cancel, or control confirmation.
- Inject the complete Chart module set in the WebMCP generation sample. Customers can then change series types, axes, indicators, interactions, annotations, and export settings without first fixing missing-module errors.
- Do not reduce the comprehensive injection list merely because the initial rendered sample uses only Line and Column series.

## Complete React feature-tour sample

```tsx
import * as React from 'react';
import { useRef } from 'react';
import {
  AreaSeries, AtrIndicator, BarSeries, BollingerBands, BoxAndWhiskerSeries, BubbleSeries, CandleSeries,
  Category, ChartAnnotation, ChartComponent, ColumnSeries, Crosshair, DataEditing, DataLabel, DateTime,
  DateTimeCategory, EmaIndicator, ErrorBar, Export, Highlight, HiloOpenCloseSeries, HiloSeries,
  HistogramSeries, Inject, Legend, LineSeries, Logarithmic, MacdIndicator, MomentumIndicator,
  MultiColoredAreaSeries, MultiColoredLineSeries, MultiLevelLabel, ParetoSeries, PolarSeries, RadarSeries,
  RangeAreaSeries, RangeColumnSeries, RangeStepAreaSeries, RsiIndicator, ScatterSeries, ScrollBar, Selection,
  SeriesCollectionDirective, SeriesDirective, SmaIndicator, SplineAreaSeries, SplineRangeAreaSeries,
  SplineSeries, StackingArea100Series, StackingAreaSeries, StackingBar100Series, StackingBarSeries,
  StackingColumn100Series, StackingColumnSeries, StackingLine100Series, StackingLineSeries,
  StackingStepAreaSeries, StepAreaSeries, StepLineSeries, StochasticIndicator, StripLine, TmaIndicator,
  Tooltip, Trendlines, WaterfallSeries, WebMcpChart, Zoom
} from '@syncfusion/ej2-react-charts';

const data: Object[] = [
  { x: 1, y: 32, comparison: 24 }, { x: 2, y: 48, comparison: 36 },
  { x: 3, y: 42, comparison: 34 }, { x: 4, y: 65, comparison: 52 },
  { x: 5, y: 58, comparison: 46 }, { x: 6, y: 74, comparison: 61 }
];

const allToolNames: string[] = [
  'inspectChart',
  'getSeriesData',
  'addChartSeries',
  'removeChartSeries',
  'clearChartSeries',
  'updateChartAnnotation',
  'showChartTooltip',
  'showChartCrosshair',
  'exportChart',
  'printChart',
  'updateChartDataSource',
  'toggleChartSeriesVisibility',
  'configureChartZoom',
  'resetChartZoom',
  'selectChartPoints',
  'clearChartSelection',
  'addChartAxis',
  'updateChartAxis',
  'updateChartTitle',
  'hideChartTooltip',
  'hideChartCrosshair',
  'updateChartProperties',
  'updateChartSeries',
  'updateChartPoint',
  'appendChartData',
  'removeChartPoints',
  'getChartPoint',
  'getSelectedChartData',
  'getChartAxes',
  'getChartIndicators',
  'getChartStriplines',
  'removeChartAxis',
  'getChartAnnotations',
  'addChartAnnotation',
  'removeChartAnnotation',
  'animateChart',
  'getChartLegend'
];

export default function WebMcpFeatureTour(): JSX.Element {
  const chartRef = useRef<ChartComponent | null>(null);

  const beforeWebMcpToolExecute = (args: any): void => {
    if (args.toolName.endsWith('_printChart')) {
      args.showConfirmationDialog = true;
    }
  };

  const executeTool = async (name: string, input: Object): Promise<Object> => {
    if (!chartRef.current) {
      throw new Error('Chart is not available.');
    }
    const tools: any[] = chartRef.current.getWebMcpTools([name]);
    const tool: any = tools.find((item: any) => item.name === name);
    if (!tool || typeof tool.execute !== 'function') {
      throw new Error(`WebMCP tool "${name}" is unavailable.`);
    }
    return tool.execute(input);
  };

  return (
    <ChartComponent
      ref={chartRef}
      id='feature-tour-chart'
      title='Chart WebMCP Feature Tour'
      subTitle='All 37 built-in Chart WebMCP tools'
      enableWebMcp={true}
      enableExport={true}
      webMcpSettings={{ name: 'featureTourChart', tools: allToolNames, exposedTo: ['assistant'] }}
      beforeWebMcpToolExecute={beforeWebMcpToolExecute}
      primaryXAxis={{ title: 'Sample index', minimum: 0, maximum: 8, interval: 1 }}
      primaryYAxis={{
        title: 'Value', minimum: 0, maximum: 100, interval: 20,
        stripLines: [{ start: 60, end: 80, text: 'Target range', color: '#DCFCE7', opacity: 0.5, visible: true }]
      }}
      tooltip={{ enable: true, shared: true, header: '<b>${point.x}</b>', format: '${series.name}: ${point.y}' }}
      crosshair={{ enable: true, lineType: 'Both' }}
      legendSettings={{ visible: true }}
      zoomSettings={{ enableSelectionZooming: true, enableMouseWheelZooming: true, enablePan: true }}
      selectionMode='Point'
      annotations={[{ content: '<div>Peak value</div>', x: 6, y: 74, coordinateUnits: 'Point', region: 'Series' }]}
    >
      <Inject services={[
        AreaSeries, AtrIndicator, BarSeries, BollingerBands, BoxAndWhiskerSeries, BubbleSeries,
        CandleSeries, Category, ChartAnnotation, ColumnSeries, Crosshair, DataEditing, DataLabel, DateTime,
        DateTimeCategory, EmaIndicator, ErrorBar, Export, Highlight, HiloOpenCloseSeries, HiloSeries,
        HistogramSeries, Legend, LineSeries, Logarithmic, MacdIndicator, MomentumIndicator,
        MultiColoredAreaSeries, MultiColoredLineSeries, MultiLevelLabel, ParetoSeries, PolarSeries,
        RadarSeries, RangeAreaSeries, RangeColumnSeries, RangeStepAreaSeries, RsiIndicator, ScatterSeries,
        ScrollBar, Selection, SmaIndicator, SplineAreaSeries, SplineRangeAreaSeries, SplineSeries,
        StackingArea100Series, StackingAreaSeries, StackingBar100Series, StackingBarSeries,
        StackingColumn100Series, StackingColumnSeries, StackingLine100Series, StackingLineSeries,
        StackingStepAreaSeries, StepAreaSeries, StepLineSeries, StochasticIndicator, StripLine, TmaIndicator,
        Tooltip, Trendlines, WaterfallSeries, WebMcpChart, Zoom
      ]} />
      <SeriesCollectionDirective>
        <SeriesDirective dataSource={data} type='Line' xName='x' yName='y' name='Observed values'
          marker={{ visible: true }} animation={{ enable: false }} />
        <SeriesDirective dataSource={data} type='Column' xName='x' yName='comparison' name='Comparison values'
          animation={{ enable: false }} />
      </SeriesCollectionDirective>
    </ChartComponent>
  );
}
```

The helper is intentionally available for diagnostics and direct testing. Do not add execution buttons unless the requested UI needs them.

The full injection list is intentional. WebMCP tools can add or update series, axes, indicators, annotations, zoom settings, selection, tooltip, crosshair, striplines, and export behavior at runtime. Injecting the complete Chart module set allows the customer to change the generated sample according to their requirements without encountering a missing-module warning. The AI must preserve this full list in WebMCP feature-tour samples unless the customer explicitly requests a smaller module set.


## How to use WebMCP in React

This section is mandatory in generated guidance. It explains the complete setup rather than only listing modules.

### Step 1: Install the React Chart package

```bash
npm install @syncfusion/ej2-react-charts
```

### Step 2: Import the Chart, directives, rendered-series modules, and `WebMcpChart`

```tsx
import * as React from 'react';
import {
  ChartComponent, Inject, LineSeries, SeriesCollectionDirective, SeriesDirective, WebMcpChart
} from '@syncfusion/ej2-react-charts';
```

### Step 3: Inject `WebMcpChart`

Every WebMCP-enabled Chart must inject `WebMcpChart`. Also inject the modules required by the rendered Chart and by any configuration the exposed tools may create.

```tsx
<Inject services={[LineSeries, WebMcpChart]} />
```

### Step 4: Enable WebMCP on the Chart

Set `enableWebMcp={true}` and give the Chart a stable `id`.

```tsx
<ChartComponent id='sales-chart' enableWebMcp={true}>
  <Inject services={[LineSeries, WebMcpChart]} />
</ChartComponent>
```

After the Chart is rendered, its WebMCP tools are registered automatically. Do not add `registerWebMcpTools()` and do not add a registration button.

### Step 5: Configure `webMcpSettings`

Use `name` to provide the Chart's WebMCP registration prefix.

```tsx
webMcpSettings={{ name: 'salesChart' }}
```

Use `tools` only when registration must be restricted to a selected set of built-in tools.

```tsx
webMcpSettings={{
  name: 'salesChart',
  tools: ['inspectChart', 'getSeriesData', 'showChartTooltip', 'hideChartTooltip']
}}
```

Add `exposedTo` only when the user or host environment supplies the required exposure values. Do not invent exposure values.

### Step 6: Handle execution when validation, cancellation, or confirmation is required

```tsx
const beforeWebMcpToolExecute = (args: any): void => {
  if (args.toolName.endsWith('_exportChart') || args.toolName.endsWith('_printChart')) {
    args.showConfirmationDialog = true;
  }

  if (args.toolName.endsWith('_clearChartSeries') && !canClearSeries) {
    args.cancel = true;
  }
};
```

Attach the handler to the Chart:

```tsx
<ChartComponent
  id='sales-chart'
  enableWebMcp={true}
  beforeWebMcpToolExecute={beforeWebMcpToolExecute}
>
  {/* Chart configuration */}
</ChartComponent>
```

The event tool name may include the `webMcpSettings.name` prefix. Use `endsWith()` when checking a built-in operation.

## Complete sample: Enable all built-in WebMCP tools

Omit `webMcpSettings.tools` to register every built-in Chart WebMCP tool.

```tsx
import * as React from 'react';
import {
  ChartComponent, Inject, LineSeries, SeriesCollectionDirective, SeriesDirective, Tooltip, WebMcpChart
} from '@syncfusion/ej2-react-charts';

const data: Object[] = [
  { x: 1, y: 32 },
  { x: 2, y: 48 },
  { x: 3, y: 42 }
];

export default function AllWebMcpToolsChart(): JSX.Element {
  return (
    <ChartComponent
      id='all-webmcp-tools-chart'
      enableWebMcp={true}
      webMcpSettings={{ name: 'allToolsChart' }}
      tooltip={{ enable: true }}
    >
      <Inject services={[LineSeries, Tooltip, WebMcpChart]} />
      <SeriesCollectionDirective>
        <SeriesDirective dataSource={data} type='Line' xName='x' yName='y' name='Sales' />
      </SeriesCollectionDirective>
    </ChartComponent>
  );
}
```

This registers all built-in tools. However, a tool can only use a Chart feature when its corresponding React module is injected. For an unrestricted AI-generation sample, use the complete feature import and injection list from the feature-tour sample.

## Complete sample: Enable only specific built-in WebMCP tools

Add the exact built-in names to `webMcpSettings.tools`.

```tsx
import * as React from 'react';
import {
  ChartComponent, Inject, LineSeries, SeriesCollectionDirective, SeriesDirective, Tooltip, WebMcpChart
} from '@syncfusion/ej2-react-charts';

const data: Object[] = [
  { x: 1, y: 32 },
  { x: 2, y: 48 },
  { x: 3, y: 42 }
];

export default function SpecificWebMcpToolsChart(): JSX.Element {
  const beforeWebMcpToolExecute = (args: any): void => {
    if (args.toolName.endsWith('_updateChartDataSource')) {
      args.showConfirmationDialog = true;
    }
  };

  return (
    <ChartComponent
      id='specific-webmcp-tools-chart'
      enableWebMcp={true}
      webMcpSettings={{
        name: 'specificToolsChart',
        tools: [
          'inspectChart',
          'getSeriesData',
          'showChartTooltip',
          'hideChartTooltip',
          'updateChartDataSource'
        ]
      }}
      beforeWebMcpToolExecute={beforeWebMcpToolExecute}
      tooltip={{ enable: true }}
    >
      <Inject services={[LineSeries, Tooltip, WebMcpChart]} />
      <SeriesCollectionDirective>
        <SeriesDirective dataSource={data} type='Line' xName='x' yName='y' name='Sales' />
      </SeriesCollectionDirective>
    </ChartComponent>
  );
}
```

Rules for specific-tool registration:

- Use only verified built-in tool names.
- Tool names are case-sensitive.
- Include every module required by the initial Chart.
- Include every module required by the selected tools.
- Include every module required by configurations that mutation tools may create.
- Keep `WebMcpChart` imported and injected.
- Do not manually register the selected tools.

## Complete sample: Add custom WebMCP tools

A custom tool requires a unique name, description, JSON input schema, execution function, input validation, and JSON-safe output.

```tsx
import * as React from 'react';
import { useRef } from 'react';
import {
  ChartComponent, Inject, LineSeries, SeriesCollectionDirective, SeriesDirective, WebMcpChart
} from '@syncfusion/ej2-react-charts';

const data: Object[] = [
  { x: 1, y: 32 },
  { x: 2, y: 48 },
  { x: 3, y: 42 }
];

export default function CustomWebMcpChart(): JSX.Element {
  const chartRef = useRef<ChartComponent | null>(null);

  const customTools: any[] = [
    {
      name: 'getHighestPoint',
      description: 'Returns the highest point from the requested Chart series.',
      inputSchema: {
        type: 'object',
        properties: {
          seriesIndex: {
            type: 'number',
            description: 'Zero-based index of the Chart series.'
          }
        },
        required: ['seriesIndex'],
        additionalProperties: false
      },
      execute: async (input: { seriesIndex: number }): Promise<Object> => {
        const chart: any = chartRef.current;

        if (!chart) {
          return { success: false, message: 'The Chart is not available.' };
        }

        if (!Number.isInteger(input.seriesIndex) || input.seriesIndex < 0) {
          return { success: false, message: 'Provide a valid non-negative series index.' };
        }

        const series: any = chart.series?.[input.seriesIndex];
        const points: any[] = series?.points || [];

        if (!series) {
          return { success: false, message: 'The requested series does not exist.' };
        }

        if (points.length === 0) {
          return { success: false, message: 'The requested series has no points.' };
        }

        const highestPoint: any = points.reduce((highest: any, current: any): any => {
          return current.yValue > highest.yValue ? current : highest;
        });

        return {
          success: true,
          seriesIndex: input.seriesIndex,
          point: {
            index: highestPoint.index,
            x: highestPoint.x,
            y: highestPoint.yValue
          }
        };
      }
    }
  ];

  return (
    <ChartComponent
      ref={chartRef}
      id='custom-webmcp-chart'
      enableWebMcp={true}
      webMcpSettings={{
        name: 'customToolsChart',
        tools: customTools
      }}
    >
      <Inject services={[LineSeries, WebMcpChart]} />
      <SeriesCollectionDirective>
        <SeriesDirective dataSource={data} type='Line' xName='x' yName='y' name='Sales' />
      </SeriesCollectionDirective>
    </ChartComponent>
  );
}
```

Custom-tool rules:

- Use a unique custom-tool name that does not conflict with a built-in name.
- Give the tool a precise description so an AI agent knows when to call it.
- Define a valid JSON input schema.
- Mark mandatory inputs in `required`.
- Set `additionalProperties: false` when undeclared inputs must be rejected.
- Validate input again inside `execute`.
- Return plain JSON-safe objects, arrays, strings, numbers, booleans, or `null`.
- Do not return the Chart instance, DOM elements, functions, events, circular objects, or class instances.
- Do not fabricate a successful result when validation fails.

## Complete sample: Use built-in tools and custom tools together

Use the `tools` property for both selected built-in tools and application-specific custom tools. There is no separate `customTools` property. Retrieve the selected built-in tool objects with `getWebMcpTools()`, append the custom tool objects, and assign the combined array to `webMcpSettings.tools` after the Chart is loaded.

```tsx
import * as React from 'react';
import { useRef } from 'react';
import {
  ChartComponent, Inject, LineSeries, SeriesCollectionDirective, SeriesDirective, Tooltip, WebMcpChart
} from '@syncfusion/ej2-react-charts';

const data: Object[] = [
  { x: 1, y: 32 },
  { x: 2, y: 48 },
  { x: 3, y: 42 }
];

const selectedToolNames: string[] = [
  'inspectChart',
  'getSeriesData',
  'showChartTooltip',
  'hideChartTooltip'
];

export default function BuiltInAndCustomWebMcpChart(): JSX.Element {
  const chartRef = useRef<ChartComponent | null>(null);
  const toolsInitialized = useRef<boolean>(false);

  const customTool: any = {
    name: 'getPointCount',
    description: 'Returns the number of points in the requested Chart series.',
    inputSchema: {
      type: 'object',
      properties: {
        seriesIndex: { type: 'number', description: 'Zero-based series index.' }
      },
      required: ['seriesIndex'],
      additionalProperties: false
    },
    execute: async (input: { seriesIndex: number }): Promise<Object> => {
      const series: any = (chartRef.current as any)?.series?.[input.seriesIndex];

      if (!series) {
        return { success: false, message: 'The requested series does not exist.' };
      }

      return {
        success: true,
        seriesIndex: input.seriesIndex,
        pointCount: series.points?.length || 0
      };
    }
  };

  const configureTools = (): void => {
    const chart: any = chartRef.current;
    if (!chart || toolsInitialized.current) {
      return;
    }

    const tools: any[] = chart.getWebMcpTools(selectedToolNames);
    tools.push(customTool);
    toolsInitialized.current = true;

    chart.setProperties({
      webMcpSettings: {
        name: 'combinedToolsChart',
        tools
      }
    }, false);
  };

  return (
    <ChartComponent
      ref={chartRef}
      id='built-in-custom-webmcp-chart'
      enableWebMcp={true}
      webMcpSettings={{ name: 'combinedToolsChart', tools: [] }}
      loaded={configureTools}
      tooltip={{ enable: true }}
    >
      <Inject services={[LineSeries, Tooltip, WebMcpChart]} />
      <SeriesCollectionDirective>
        <SeriesDirective dataSource={data} type='Line' xName='x' yName='y' name='Sales' />
      </SeriesCollectionDirective>
    </ChartComponent>
  );
}
```

Rules for combining built-in and custom tools:

- Do not use or generate a `customTools` property.
- Start with `webMcpSettings.tools: []` to prevent automatic registration before the combined list is ready.
- Call `getWebMcpTools(selectedToolNames)` only after the Chart is loaded.
- Append each custom tool object to the returned built-in tool array.
- Assign the combined array to `webMcpSettings.tools` by using `setProperties()`.
- Guard the loaded handler so the tools are configured only once.

## Directly retrieve and execute a WebMCP tool

Use `getWebMcpTools()` only for diagnostics, tests, or an explicitly requested UI workflow. It is not required for automatic registration.

```tsx
import * as React from 'react';
import { useRef } from 'react';
import {
  ChartComponent, Inject, LineSeries, SeriesCollectionDirective, SeriesDirective, WebMcpChart
} from '@syncfusion/ej2-react-charts';

const data: Object[] = [
  { x: 1, y: 32 },
  { x: 2, y: 48 }
];

export default function DirectWebMcpExecutionChart(): JSX.Element {
  const chartRef = useRef<ChartComponent | null>(null);

  const executeTool = async (name: string, input: Object): Promise<Object> => {
    if (!chartRef.current) {
      throw new Error('The Chart is not available.');
    }

    const tools: any[] = chartRef.current.getWebMcpTools([name]);
    const tool: any = tools.find((item: any): boolean => item.name === name);

    if (!tool || typeof tool.execute !== 'function') {
      throw new Error(`The WebMCP tool "${name}" is unavailable.`);
    }

    return tool.execute(input);
  };

  const inspectChart = async (): Promise<void> => {
    const result: Object = await executeTool('inspectChart', {});
    console.log(result);
  };

  return (
    <div>
      <button type='button' onClick={inspectChart}>Inspect Chart</button>
      <ChartComponent
        ref={chartRef}
        id='direct-execution-chart'
        enableWebMcp={true}
        webMcpSettings={{ name: 'directExecutionChart', tools: ['inspectChart'] }}
      >
        <Inject services={[LineSeries, WebMcpChart]} />
        <SeriesCollectionDirective>
          <SeriesDirective dataSource={data} type='Line' xName='x' yName='y' name='Sales' />
        </SeriesCollectionDirective>
      </ChartComponent>
    </div>
  );
}
```

Do not add a direct-execution button unless the requested UI explicitly needs one.

## Confirmation, cancellation, and validation

### Keep default confirmation behavior

Do not override confirmation unless the application has a specific requirement. Write operations should retain their built-in confirmation behavior.

### Force confirmation for selected operations

```tsx
const beforeWebMcpToolExecute = (args: any): void => {
  if (args.toolName.endsWith('_exportChart') || args.toolName.endsWith('_printChart')) {
    args.showConfirmationDialog = true;
  }
};
```

### Cancel an invalid or disallowed operation

```tsx
const beforeWebMcpToolExecute = (args: any): void => {
  if (args.toolName.endsWith('_clearChartSeries') && !canClearSeries) {
    args.cancel = true;
  }
};
```

### Validate execution targets

Before execution, validate all applicable values:

- Chart and element IDs
- Series indexes
- Point indexes
- Annotation indexes
- Axis names
- Data field mappings
- Export types
- Tool-specific configuration objects
- Application permissions

## Module-selection rules

### Read-only tool

Import and inject `WebMcpChart` plus every module required to render the current Chart.

### Feature-specific tool

Import and inject `WebMcpChart`, the modules needed by the current Chart, and the module required by the tool. Examples:

```tsx
<Inject services={[LineSeries, Tooltip, WebMcpChart]} />
```

```tsx
<Inject services={[LineSeries, Crosshair, WebMcpChart]} />
```

```tsx
<Inject services={[LineSeries, Zoom, WebMcpChart]} />
```

### Mutation tool

For tools such as `addChartSeries`, `updateChartSeries`, `addChartAxis`, `updateChartAxis`, and `updateChartProperties`, import and inject every module for every configuration the tool is permitted to create.

### Unrestricted AI-generation sample

Use the complete import and injection list from the feature-tour sample. Do not reduce modules based only on the initially rendered series.

## Required generation scenarios

An AI using this guide must be able to generate all of the following without additional assumptions:

1. A Chart with all built-in WebMCP tools.
2. A Chart with selected built-in WebMCP tools.
3. A Chart with one or more custom WebMCP tools.
4. A Chart with selected built-in tools and custom tools together.
5. A Chart with `beforeWebMcpToolExecute` validation.
6. A Chart that cancels a disallowed operation.
7. A Chart that explicitly requests confirmation for selected operations.
8. A diagnostic or test sample that retrieves and directly executes a tool.
9. A comprehensive sample with every required feature module.
10. A focused sample with only the modules required by its rendered configuration and permitted tool behavior.

## Exact built-in tool names

- `inspectChart`
- `getSeriesData`
- `addChartSeries`
- `removeChartSeries`
- `clearChartSeries`
- `updateChartAnnotation`
- `showChartTooltip`
- `showChartCrosshair`
- `exportChart`
- `printChart`
- `updateChartDataSource`
- `toggleChartSeriesVisibility`
- `configureChartZoom`
- `resetChartZoom`
- `selectChartPoints`
- `clearChartSelection`
- `addChartAxis`
- `updateChartAxis`
- `updateChartTitle`
- `hideChartTooltip`
- `hideChartCrosshair`
- `updateChartProperties`
- `updateChartSeries`
- `updateChartPoint`
- `appendChartData`
- `removeChartPoints`
- `getChartPoint`
- `getSelectedChartData`
- `getChartAxes`
- `getChartIndicators`
- `getChartStriplines`
- `removeChartAxis`
- `getChartAnnotations`
- `addChartAnnotation`
- `removeChartAnnotation`
- `animateChart`
- `getChartLegend`


## Spoon-fed tool-by-tool React implementation

This section is intentionally explicit. For every tool, copy the exact React package import, copy the exact `<Inject>` block, add the listed Chart properties, and use the supplied execution input. Do not expect the generator to infer a missing module.

### Shared direct-execution helper

Add this once in every sample that directly executes a WebMCP tool:

```tsx
const chartRef = useRef<ChartComponent | null>(null);

const executeTool = async (name: string, input: Object): Promise<Object> => {
  if (!chartRef.current) {
    throw new Error('Chart is not available.');
  }

  const tools: any[] = chartRef.current.getWebMcpTools([name]);
  const tool: any = tools.find((item: any): boolean => item.name === name);

  if (!tool || typeof tool.execute !== 'function') {
    throw new Error(`WebMCP tool "${name}" is unavailable.`);
  }

  return tool.execute(input);
};
```

Attach the same ref to the Chart:

```tsx
<ChartComponent ref={chartRef} id='feature-tour-chart' enableWebMcp={true}>
  {/* Chart configuration */}
</ChartComponent>
```

### Shared React component/directive imports

When the sample declares series in JSX, include these React items in addition to the tool-specific feature modules:

```tsx
import {
  ChartComponent, Inject, SeriesCollectionDirective, SeriesDirective
} from '@syncfusion/ej2-react-charts';
```

### 1. `inspectChart`

**Copy this package import:**

```tsx
import {
  WebMcpChart
} from '@syncfusion/ej2-react-charts';
```

**Copy this injection block inside `ChartComponent`:**

```tsx
<Inject services={[
  WebMcpChart
]} />
```

**Additional required Chart properties:** None beyond the shared stable `id`, `enableWebMcp={true}`, and `webMcpSettings` configuration.

**Direct execution:**

```tsx
await executeTool('inspectChart', {});
```

**Preconditions and notes:**

- A rendered Chart is required.

### 2. `getSeriesData`

**Copy this package import:**

```tsx
import {
  LineSeries, WebMcpChart
} from '@syncfusion/ej2-react-charts';
```

**Copy this injection block inside `ChartComponent`:**

```tsx
<Inject services={[
  LineSeries, WebMcpChart
]} />
```

**Additional required Chart properties:** None beyond the shared stable `id`, `enableWebMcp={true}`, and `webMcpSettings` configuration.

**Direct execution:**

```tsx
await executeTool('getSeriesData', { seriesIndex: 0 });
```

**Preconditions and notes:**

- The target series index must exist.
- Replace `LineSeries` with the actual rendered series module when needed.

### 3. `addChartSeries`

**Copy this package import:**

```tsx
import {
  AreaSeries, BarSeries, BoxAndWhiskerSeries, BubbleSeries, CandleSeries, ColumnSeries, HiloOpenCloseSeries,
  HiloSeries, HistogramSeries, LineSeries, MultiColoredAreaSeries, MultiColoredLineSeries, ParetoSeries,
  PolarSeries, RadarSeries, RangeAreaSeries, RangeColumnSeries, RangeStepAreaSeries, ScatterSeries,
  SplineAreaSeries, SplineRangeAreaSeries, SplineSeries, StackingArea100Series, StackingAreaSeries,
  StackingBar100Series, StackingBarSeries, StackingColumn100Series, StackingColumnSeries,
  StackingLine100Series, StackingLineSeries, StackingStepAreaSeries, StepAreaSeries, StepLineSeries,
  WaterfallSeries, AtrIndicator, BollingerBands, EmaIndicator, MacdIndicator, MomentumIndicator, RsiIndicator,
  SmaIndicator, StochasticIndicator, TmaIndicator, DataLabel, ErrorBar, Trendlines, WebMcpChart
} from '@syncfusion/ej2-react-charts';
```

**Copy this injection block inside `ChartComponent`:**

```tsx
<Inject services={[
  AreaSeries, BarSeries, BoxAndWhiskerSeries, BubbleSeries, CandleSeries, ColumnSeries, HiloOpenCloseSeries,
  HiloSeries, HistogramSeries, LineSeries, MultiColoredAreaSeries, MultiColoredLineSeries, ParetoSeries,
  PolarSeries, RadarSeries, RangeAreaSeries, RangeColumnSeries, RangeStepAreaSeries, ScatterSeries,
  SplineAreaSeries, SplineRangeAreaSeries, SplineSeries, StackingArea100Series, StackingAreaSeries,
  StackingBar100Series, StackingBarSeries, StackingColumn100Series, StackingColumnSeries,
  StackingLine100Series, StackingLineSeries, StackingStepAreaSeries, StepAreaSeries, StepLineSeries,
  WaterfallSeries, AtrIndicator, BollingerBands, EmaIndicator, MacdIndicator, MomentumIndicator, RsiIndicator,
  SmaIndicator, StochasticIndicator, TmaIndicator, DataLabel, ErrorBar, Trendlines, WebMcpChart
]} />
```

**Additional required Chart properties:** None beyond the shared stable `id`, `enableWebMcp={true}`, and `webMcpSettings` configuration.

**Direct execution:**

```tsx
await executeTool('addChartSeries', { series: { type: 'Line', dataSource: [{ x: 1, y: 20 }, { x: 2, y: 35 }], xName: 'x', yName: 'y', name: 'Added series' } });
```

**Preconditions and notes:**

- Keep all listed series and optional series-feature modules because the tool may add or configure them.

### 4. `removeChartSeries`

**Copy this package import:**

```tsx
import {
  WebMcpChart
} from '@syncfusion/ej2-react-charts';
```

**Copy this injection block inside `ChartComponent`:**

```tsx
<Inject services={[
  WebMcpChart
]} />
```

**Additional required Chart properties:** None beyond the shared stable `id`, `enableWebMcp={true}`, and `webMcpSettings` configuration.

**Direct execution:**

```tsx
await executeTool('removeChartSeries', { seriesIndex: 1 });
```

**Preconditions and notes:**

- The target series index must exist.
- Keep modules required by the remaining rendered series.

### 5. `clearChartSeries`

**Copy this package import:**

```tsx
import {
  WebMcpChart
} from '@syncfusion/ej2-react-charts';
```

**Copy this injection block inside `ChartComponent`:**

```tsx
<Inject services={[
  WebMcpChart
]} />
```

**Additional required Chart properties:** None beyond the shared stable `id`, `enableWebMcp={true}`, and `webMcpSettings` configuration.

**Direct execution:**

```tsx
await executeTool('clearChartSeries', {});
```

**Preconditions and notes:**

- At least one series should exist before execution.
- Keep modules required to render the initial series.

### 6. `updateChartAnnotation`

**Copy this package import:**

```tsx
import {
  ChartAnnotation, WebMcpChart
} from '@syncfusion/ej2-react-charts';
```

**Copy this injection block inside `ChartComponent`:**

```tsx
<Inject services={[
  ChartAnnotation, WebMcpChart
]} />
```

**Additional required Chart properties:** None beyond the shared stable `id`, `enableWebMcp={true}`, and `webMcpSettings` configuration.

**Direct execution:**

```tsx
await executeTool('updateChartAnnotation', { annotationIndex: 0, annotation: { content: '<div>Updated peak</div>', x: 5, y: 58 } });
```

**Preconditions and notes:**

- The annotation index must exist.

### 7. `showChartTooltip`

**Copy this package import:**

```tsx
import {
  Tooltip, WebMcpChart
} from '@syncfusion/ej2-react-charts';
```

**Copy this injection block inside `ChartComponent`:**

```tsx
<Inject services={[
  Tooltip, WebMcpChart
]} />
```

**Add these required Chart properties:**

```tsx
tooltip={{ enable: true }}
```

**Direct execution:**

```tsx
await executeTool('showChartTooltip', { seriesIndex: 0, pointIndex: 2 });
```

**Preconditions and notes:**

- The series and point indexes must exist.

### 8. `showChartCrosshair`

**Copy this package import:**

```tsx
import {
  Crosshair, WebMcpChart
} from '@syncfusion/ej2-react-charts';
```

**Copy this injection block inside `ChartComponent`:**

```tsx
<Inject services={[
  Crosshair, WebMcpChart
]} />
```

**Add these required Chart properties:**

```tsx
crosshair={{ enable: true, lineType: 'Both' }}
```

**Direct execution:**

```tsx
await executeTool('showChartCrosshair', { x: 250, y: 180 });
```

**Preconditions and notes:**

- The supplied coordinates must be within the intended Chart area.

### 9. `exportChart`

**Copy this package import:**

```tsx
import {
  Export, WebMcpChart
} from '@syncfusion/ej2-react-charts';
```

**Copy this injection block inside `ChartComponent`:**

```tsx
<Inject services={[
  Export, WebMcpChart
]} />
```

**Add these required Chart properties:**

```tsx
enableExport={true}
```

**Direct execution:**

```tsx
await executeTool('exportChart', { type: 'PNG', fileName: 'chart-export' });
```

**Preconditions and notes:**

- Use a supported export type.

### 10. `printChart`

**Copy this package import:**

```tsx
import {
  Export, WebMcpChart
} from '@syncfusion/ej2-react-charts';
```

**Copy this injection block inside `ChartComponent`:**

```tsx
<Inject services={[
  Export, WebMcpChart
]} />
```

**Additional required Chart properties:** None beyond the shared stable `id`, `enableWebMcp={true}`, and `webMcpSettings` configuration.

**Direct execution:**

```tsx
await executeTool('printChart', { elementIds: 'feature-tour-chart' });
```

**Preconditions and notes:**

- The supplied element ID must exist.

### 11. `updateChartDataSource`

**Copy this package import:**

```tsx
import {
  LineSeries, WebMcpChart
} from '@syncfusion/ej2-react-charts';
```

**Copy this injection block inside `ChartComponent`:**

```tsx
<Inject services={[
  LineSeries, WebMcpChart
]} />
```

**Additional required Chart properties:** None beyond the shared stable `id`, `enableWebMcp={true}`, and `webMcpSettings` configuration.

**Direct execution:**

```tsx
await executeTool('updateChartDataSource', { seriesIndex: 0, dataSource: [{ x: 1, y: 40 }, { x: 2, y: 55 }] });
```

**Preconditions and notes:**

- The target series must exist.
- The new data must match the series `xName` and `yName` fields.

### 12. `toggleChartSeriesVisibility`

**Copy this package import:**

```tsx
import {
  Legend, LineSeries, WebMcpChart
} from '@syncfusion/ej2-react-charts';
```

**Copy this injection block inside `ChartComponent`:**

```tsx
<Inject services={[
  Legend, LineSeries, WebMcpChart
]} />
```

**Add these required Chart properties:**

```tsx
legendSettings={{ visible: true }}
```

**Direct execution:**

```tsx
await executeTool('toggleChartSeriesVisibility', { seriesIndex: 0 });
```

**Preconditions and notes:**

- The target series must exist.
- `Legend` is included so visibility can also be represented through the legend.

### 13. `configureChartZoom`

**Copy this package import:**

```tsx
import {
  Zoom, WebMcpChart
} from '@syncfusion/ej2-react-charts';
```

**Copy this injection block inside `ChartComponent`:**

```tsx
<Inject services={[
  Zoom, WebMcpChart
]} />
```

**Additional required Chart properties:** None beyond the shared stable `id`, `enableWebMcp={true}`, and `webMcpSettings` configuration.

**Direct execution:**

```tsx
await executeTool('configureChartZoom', { zoomSettings: { enableSelectionZooming: true, enableMouseWheelZooming: true, enablePan: true } });
```

**Preconditions and notes:**

- No additional tool-specific precondition beyond a rendered WebMCP-enabled Chart.

### 14. `resetChartZoom`

**Copy this package import:**

```tsx
import {
  Zoom, WebMcpChart
} from '@syncfusion/ej2-react-charts';
```

**Copy this injection block inside `ChartComponent`:**

```tsx
<Inject services={[
  Zoom, WebMcpChart
]} />
```

**Additional required Chart properties:** None beyond the shared stable `id`, `enableWebMcp={true}`, and `webMcpSettings` configuration.

**Direct execution:**

```tsx
await executeTool('resetChartZoom', {});
```

**Preconditions and notes:**

- The Chart can be reset even when no zoom is currently applied.

### 15. `selectChartPoints`

**Copy this package import:**

```tsx
import {
  Selection, WebMcpChart
} from '@syncfusion/ej2-react-charts';
```

**Copy this injection block inside `ChartComponent`:**

```tsx
<Inject services={[
  Selection, WebMcpChart
]} />
```

**Add these required Chart properties:**

```tsx
selectionMode='Point'
```

**Direct execution:**

```tsx
await executeTool('selectChartPoints', { points: [{ seriesIndex: 0, pointIndex: 1 }, { seriesIndex: 0, pointIndex: 3 }] });
```

**Preconditions and notes:**

- Every supplied series and point index must exist.

### 16. `clearChartSelection`

**Copy this package import:**

```tsx
import {
  Selection, WebMcpChart
} from '@syncfusion/ej2-react-charts';
```

**Copy this injection block inside `ChartComponent`:**

```tsx
<Inject services={[
  Selection, WebMcpChart
]} />
```

**Add these required Chart properties:**

```tsx
selectionMode='Point'
```

**Direct execution:**

```tsx
await executeTool('clearChartSelection', {});
```

**Preconditions and notes:**

- No additional tool-specific precondition beyond a rendered WebMCP-enabled Chart.

### 17. `addChartAxis`

**Copy this package import:**

```tsx
import {
  Category, DateTime, DateTimeCategory, Logarithmic, MultiLevelLabel, ScrollBar, StripLine, WebMcpChart
} from '@syncfusion/ej2-react-charts';
```

**Copy this injection block inside `ChartComponent`:**

```tsx
<Inject services={[
  Category, DateTime, DateTimeCategory, Logarithmic, MultiLevelLabel, ScrollBar, StripLine, WebMcpChart
]} />
```

**Additional required Chart properties:** None beyond the shared stable `id`, `enableWebMcp={true}`, and `webMcpSettings` configuration.

**Direct execution:**

```tsx
await executeTool('addChartAxis', { axis: { name: 'secondaryAxis', title: 'Secondary value', opposedPosition: true } });
```

**Preconditions and notes:**

- Use a unique additional-axis name.
- Keep all axis modules because the tool may create different value types and axis features.

### 18. `updateChartAxis`

**Copy this package import:**

```tsx
import {
  Category, DateTime, DateTimeCategory, Logarithmic, MultiLevelLabel, ScrollBar, StripLine, WebMcpChart
} from '@syncfusion/ej2-react-charts';
```

**Copy this injection block inside `ChartComponent`:**

```tsx
<Inject services={[
  Category, DateTime, DateTimeCategory, Logarithmic, MultiLevelLabel, ScrollBar, StripLine, WebMcpChart
]} />
```

**Additional required Chart properties:** None beyond the shared stable `id`, `enableWebMcp={true}`, and `webMcpSettings` configuration.

**Direct execution:**

```tsx
await executeTool('updateChartAxis', { axisName: 'primaryYAxis', axis: { title: 'Updated value', minimum: 0, maximum: 120, interval: 20 } });
```

**Preconditions and notes:**

- The target primary or additional axis must exist.

### 19. `updateChartTitle`

**Copy this package import:**

```tsx
import {
  WebMcpChart
} from '@syncfusion/ej2-react-charts';
```

**Copy this injection block inside `ChartComponent`:**

```tsx
<Inject services={[
  WebMcpChart
]} />
```

**Additional required Chart properties:** None beyond the shared stable `id`, `enableWebMcp={true}`, and `webMcpSettings` configuration.

**Direct execution:**

```tsx
await executeTool('updateChartTitle', { title: 'Updated Chart title', subTitle: 'Updated subtitle' });
```

**Preconditions and notes:**

- No additional tool-specific precondition beyond a rendered WebMCP-enabled Chart.

### 20. `hideChartTooltip`

**Copy this package import:**

```tsx
import {
  Tooltip, WebMcpChart
} from '@syncfusion/ej2-react-charts';
```

**Copy this injection block inside `ChartComponent`:**

```tsx
<Inject services={[
  Tooltip, WebMcpChart
]} />
```

**Add these required Chart properties:**

```tsx
tooltip={{ enable: true }}
```

**Direct execution:**

```tsx
await executeTool('hideChartTooltip', {});
```

**Preconditions and notes:**

- No additional tool-specific precondition beyond a rendered WebMCP-enabled Chart.

### 21. `hideChartCrosshair`

**Copy this package import:**

```tsx
import {
  Crosshair, WebMcpChart
} from '@syncfusion/ej2-react-charts';
```

**Copy this injection block inside `ChartComponent`:**

```tsx
<Inject services={[
  Crosshair, WebMcpChart
]} />
```

**Add these required Chart properties:**

```tsx
crosshair={{ enable: true, lineType: 'Both' }}
```

**Direct execution:**

```tsx
await executeTool('hideChartCrosshair', {});
```

**Preconditions and notes:**

- No additional tool-specific precondition beyond a rendered WebMCP-enabled Chart.

### 22. `updateChartProperties`

**Copy this package import:**

```tsx
import {
  AreaSeries, BarSeries, BoxAndWhiskerSeries, BubbleSeries, CandleSeries, ColumnSeries, HiloOpenCloseSeries,
  HiloSeries, HistogramSeries, LineSeries, MultiColoredAreaSeries, MultiColoredLineSeries, ParetoSeries,
  PolarSeries, RadarSeries, RangeAreaSeries, RangeColumnSeries, RangeStepAreaSeries, ScatterSeries,
  SplineAreaSeries, SplineRangeAreaSeries, SplineSeries, StackingArea100Series, StackingAreaSeries,
  StackingBar100Series, StackingBarSeries, StackingColumn100Series, StackingColumnSeries,
  StackingLine100Series, StackingLineSeries, StackingStepAreaSeries, StepAreaSeries, StepLineSeries,
  WaterfallSeries, AtrIndicator, BollingerBands, EmaIndicator, MacdIndicator, MomentumIndicator, RsiIndicator,
  SmaIndicator, StochasticIndicator, TmaIndicator, Category, DateTime, DateTimeCategory, Logarithmic,
  MultiLevelLabel, ScrollBar, StripLine, ChartAnnotation, Crosshair, DataEditing, DataLabel, ErrorBar, Export,
  Highlight, Legend, Selection, Tooltip, Trendlines, WebMcpChart, Zoom
} from '@syncfusion/ej2-react-charts';
```

**Copy this injection block inside `ChartComponent`:**

```tsx
<Inject services={[
  AreaSeries, BarSeries, BoxAndWhiskerSeries, BubbleSeries, CandleSeries, ColumnSeries, HiloOpenCloseSeries,
  HiloSeries, HistogramSeries, LineSeries, MultiColoredAreaSeries, MultiColoredLineSeries, ParetoSeries,
  PolarSeries, RadarSeries, RangeAreaSeries, RangeColumnSeries, RangeStepAreaSeries, ScatterSeries,
  SplineAreaSeries, SplineRangeAreaSeries, SplineSeries, StackingArea100Series, StackingAreaSeries,
  StackingBar100Series, StackingBarSeries, StackingColumn100Series, StackingColumnSeries,
  StackingLine100Series, StackingLineSeries, StackingStepAreaSeries, StepAreaSeries, StepLineSeries,
  WaterfallSeries, AtrIndicator, BollingerBands, EmaIndicator, MacdIndicator, MomentumIndicator, RsiIndicator,
  SmaIndicator, StochasticIndicator, TmaIndicator, Category, DateTime, DateTimeCategory, Logarithmic,
  MultiLevelLabel, ScrollBar, StripLine, ChartAnnotation, Crosshair, DataEditing, DataLabel, ErrorBar, Export,
  Highlight, Legend, Selection, Tooltip, Trendlines, WebMcpChart, Zoom
]} />
```

**Additional required Chart properties:** None beyond the shared stable `id`, `enableWebMcp={true}`, and `webMcpSettings` configuration.

**Direct execution:**

```tsx
await executeTool('updateChartProperties', { properties: { background: '#FFFFFF', title: 'Updated properties', legendSettings: { visible: true }, tooltip: { enable: true, shared: true } } });
```

**Preconditions and notes:**

- Use the complete feature import and injection lists below because this tool may enable multiple Chart features.

### 23. `updateChartSeries`

**Copy this package import:**

```tsx
import {
  AreaSeries, BarSeries, BoxAndWhiskerSeries, BubbleSeries, CandleSeries, ColumnSeries, HiloOpenCloseSeries,
  HiloSeries, HistogramSeries, LineSeries, MultiColoredAreaSeries, MultiColoredLineSeries, ParetoSeries,
  PolarSeries, RadarSeries, RangeAreaSeries, RangeColumnSeries, RangeStepAreaSeries, ScatterSeries,
  SplineAreaSeries, SplineRangeAreaSeries, SplineSeries, StackingArea100Series, StackingAreaSeries,
  StackingBar100Series, StackingBarSeries, StackingColumn100Series, StackingColumnSeries,
  StackingLine100Series, StackingLineSeries, StackingStepAreaSeries, StepAreaSeries, StepLineSeries,
  WaterfallSeries, AtrIndicator, BollingerBands, EmaIndicator, MacdIndicator, MomentumIndicator, RsiIndicator,
  SmaIndicator, StochasticIndicator, TmaIndicator, DataLabel, ErrorBar, Trendlines, WebMcpChart
} from '@syncfusion/ej2-react-charts';
```

**Copy this injection block inside `ChartComponent`:**

```tsx
<Inject services={[
  AreaSeries, BarSeries, BoxAndWhiskerSeries, BubbleSeries, CandleSeries, ColumnSeries, HiloOpenCloseSeries,
  HiloSeries, HistogramSeries, LineSeries, MultiColoredAreaSeries, MultiColoredLineSeries, ParetoSeries,
  PolarSeries, RadarSeries, RangeAreaSeries, RangeColumnSeries, RangeStepAreaSeries, ScatterSeries,
  SplineAreaSeries, SplineRangeAreaSeries, SplineSeries, StackingArea100Series, StackingAreaSeries,
  StackingBar100Series, StackingBarSeries, StackingColumn100Series, StackingColumnSeries,
  StackingLine100Series, StackingLineSeries, StackingStepAreaSeries, StepAreaSeries, StepLineSeries,
  WaterfallSeries, AtrIndicator, BollingerBands, EmaIndicator, MacdIndicator, MomentumIndicator, RsiIndicator,
  SmaIndicator, StochasticIndicator, TmaIndicator, DataLabel, ErrorBar, Trendlines, WebMcpChart
]} />
```

**Additional required Chart properties:** None beyond the shared stable `id`, `enableWebMcp={true}`, and `webMcpSettings` configuration.

**Direct execution:**

```tsx
await executeTool('updateChartSeries', { seriesIndex: 0, series: { type: 'Column', name: 'Updated series', marker: { visible: true } } });
```

**Preconditions and notes:**

- The target series must exist.
- Keep all listed modules because the tool may change the series type or optional features.

### 24. `updateChartPoint`

**Copy this package import:**

```tsx
import {
  DataEditing, LineSeries, WebMcpChart
} from '@syncfusion/ej2-react-charts';
```

**Copy this injection block inside `ChartComponent`:**

```tsx
<Inject services={[
  DataEditing, LineSeries, WebMcpChart
]} />
```

**Additional required Chart properties:** None beyond the shared stable `id`, `enableWebMcp={true}`, and `webMcpSettings` configuration.

**Direct execution:**

```tsx
await executeTool('updateChartPoint', { seriesIndex: 0, pointIndex: 2, point: { x: 3, y: 70 } });
```

**Preconditions and notes:**

- The series and point indexes must exist.
- The point object must match the configured field mappings.

### 25. `appendChartData`

**Copy this package import:**

```tsx
import {
  LineSeries, WebMcpChart
} from '@syncfusion/ej2-react-charts';
```

**Copy this injection block inside `ChartComponent`:**

```tsx
<Inject services={[
  LineSeries, WebMcpChart
]} />
```

**Additional required Chart properties:** None beyond the shared stable `id`, `enableWebMcp={true}`, and `webMcpSettings` configuration.

**Direct execution:**

```tsx
await executeTool('appendChartData', { seriesIndex: 0, data: [{ x: 7, y: 80 }, { x: 8, y: 76 }] });
```

**Preconditions and notes:**

- The appended objects must match the series field mappings.

### 26. `removeChartPoints`

**Copy this package import:**

```tsx
import {
  LineSeries, WebMcpChart
} from '@syncfusion/ej2-react-charts';
```

**Copy this injection block inside `ChartComponent`:**

```tsx
<Inject services={[
  LineSeries, WebMcpChart
]} />
```

**Additional required Chart properties:** None beyond the shared stable `id`, `enableWebMcp={true}`, and `webMcpSettings` configuration.

**Direct execution:**

```tsx
await executeTool('removeChartPoints', { seriesIndex: 0, pointIndexes: [1, 3] });
```

**Preconditions and notes:**

- Every point index must exist.

### 27. `getChartPoint`

**Copy this package import:**

```tsx
import {
  LineSeries, WebMcpChart
} from '@syncfusion/ej2-react-charts';
```

**Copy this injection block inside `ChartComponent`:**

```tsx
<Inject services={[
  LineSeries, WebMcpChart
]} />
```

**Additional required Chart properties:** None beyond the shared stable `id`, `enableWebMcp={true}`, and `webMcpSettings` configuration.

**Direct execution:**

```tsx
await executeTool('getChartPoint', { seriesIndex: 0, pointIndex: 2 });
```

**Preconditions and notes:**

- The series and point indexes must exist.

### 28. `getSelectedChartData`

**Copy this package import:**

```tsx
import {
  Selection, WebMcpChart
} from '@syncfusion/ej2-react-charts';
```

**Copy this injection block inside `ChartComponent`:**

```tsx
<Inject services={[
  Selection, WebMcpChart
]} />
```

**Add these required Chart properties:**

```tsx
selectionMode='Point'
```

**Direct execution:**

```tsx
await executeTool('getSelectedChartData', {});
```

**Preconditions and notes:**

- A selection is required when selected data is expected.

### 29. `getChartAxes`

**Copy this package import:**

```tsx
import {
  Category, DateTime, DateTimeCategory, Logarithmic, MultiLevelLabel, ScrollBar, StripLine, WebMcpChart
} from '@syncfusion/ej2-react-charts';
```

**Copy this injection block inside `ChartComponent`:**

```tsx
<Inject services={[
  Category, DateTime, DateTimeCategory, Logarithmic, MultiLevelLabel, ScrollBar, StripLine, WebMcpChart
]} />
```

**Additional required Chart properties:** None beyond the shared stable `id`, `enableWebMcp={true}`, and `webMcpSettings` configuration.

**Direct execution:**

```tsx
await executeTool('getChartAxes', {});
```

**Preconditions and notes:**

- The full axis list keeps all supported value types and axis features available.

### 30. `getChartIndicators`

**Copy this package import:**

```tsx
import {
  AtrIndicator, BollingerBands, EmaIndicator, MacdIndicator, MomentumIndicator, RsiIndicator, SmaIndicator,
  StochasticIndicator, TmaIndicator, LineSeries, WebMcpChart
} from '@syncfusion/ej2-react-charts';
```

**Copy this injection block inside `ChartComponent`:**

```tsx
<Inject services={[
  AtrIndicator, BollingerBands, EmaIndicator, MacdIndicator, MomentumIndicator, RsiIndicator, SmaIndicator,
  StochasticIndicator, TmaIndicator, LineSeries, WebMcpChart
]} />
```

**Additional required Chart properties:** None beyond the shared stable `id`, `enableWebMcp={true}`, and `webMcpSettings` configuration.

**Direct execution:**

```tsx
await executeTool('getChartIndicators', {});
```

**Preconditions and notes:**

- Keep the source series module required by each configured indicator.

### 31. `getChartStriplines`

**Copy this package import:**

```tsx
import {
  StripLine, WebMcpChart
} from '@syncfusion/ej2-react-charts';
```

**Copy this injection block inside `ChartComponent`:**

```tsx
<Inject services={[
  StripLine, WebMcpChart
]} />
```

**Additional required Chart properties:** None beyond the shared stable `id`, `enableWebMcp={true}`, and `webMcpSettings` configuration.

**Direct execution:**

```tsx
await executeTool('getChartStriplines', {});
```

**Preconditions and notes:**

- Configure striplines on a Chart axis when stripline data is expected.

### 32. `removeChartAxis`

**Copy this package import:**

```tsx
import {
  Category, DateTime, DateTimeCategory, Logarithmic, MultiLevelLabel, ScrollBar, StripLine, WebMcpChart
} from '@syncfusion/ej2-react-charts';
```

**Copy this injection block inside `ChartComponent`:**

```tsx
<Inject services={[
  Category, DateTime, DateTimeCategory, Logarithmic, MultiLevelLabel, ScrollBar, StripLine, WebMcpChart
]} />
```

**Additional required Chart properties:** None beyond the shared stable `id`, `enableWebMcp={true}`, and `webMcpSettings` configuration.

**Direct execution:**

```tsx
await executeTool('removeChartAxis', { axisName: 'secondaryAxis' });
```

**Preconditions and notes:**

- Only an existing removable additional axis should be targeted.

### 33. `getChartAnnotations`

**Copy this package import:**

```tsx
import {
  ChartAnnotation, WebMcpChart
} from '@syncfusion/ej2-react-charts';
```

**Copy this injection block inside `ChartComponent`:**

```tsx
<Inject services={[
  ChartAnnotation, WebMcpChart
]} />
```

**Additional required Chart properties:** None beyond the shared stable `id`, `enableWebMcp={true}`, and `webMcpSettings` configuration.

**Direct execution:**

```tsx
await executeTool('getChartAnnotations', {});
```

**Preconditions and notes:**

- No additional tool-specific precondition beyond a rendered WebMCP-enabled Chart.

### 34. `addChartAnnotation`

**Copy this package import:**

```tsx
import {
  ChartAnnotation, WebMcpChart
} from '@syncfusion/ej2-react-charts';
```

**Copy this injection block inside `ChartComponent`:**

```tsx
<Inject services={[
  ChartAnnotation, WebMcpChart
]} />
```

**Additional required Chart properties:** None beyond the shared stable `id`, `enableWebMcp={true}`, and `webMcpSettings` configuration.

**Direct execution:**

```tsx
await executeTool('addChartAnnotation', { annotation: { content: '<div>New annotation</div>', x: 4, y: 65, coordinateUnits: 'Point', region: 'Series' } });
```

**Preconditions and notes:**

- The annotation coordinates and units must be valid for the Chart.

### 35. `removeChartAnnotation`

**Copy this package import:**

```tsx
import {
  ChartAnnotation, WebMcpChart
} from '@syncfusion/ej2-react-charts';
```

**Copy this injection block inside `ChartComponent`:**

```tsx
<Inject services={[
  ChartAnnotation, WebMcpChart
]} />
```

**Additional required Chart properties:** None beyond the shared stable `id`, `enableWebMcp={true}`, and `webMcpSettings` configuration.

**Direct execution:**

```tsx
await executeTool('removeChartAnnotation', { annotationIndex: 0 });
```

**Preconditions and notes:**

- The annotation index must exist.

### 36. `animateChart`

**Copy this package import:**

```tsx
import {
  LineSeries, WebMcpChart
} from '@syncfusion/ej2-react-charts';
```

**Copy this injection block inside `ChartComponent`:**

```tsx
<Inject services={[
  LineSeries, WebMcpChart
]} />
```

**Additional required Chart properties:** None beyond the shared stable `id`, `enableWebMcp={true}`, and `webMcpSettings` configuration.

**Direct execution:**

```tsx
await executeTool('animateChart', {});
```

**Preconditions and notes:**

- Keep the module for every rendered series type.

### 37. `getChartLegend`

**Copy this package import:**

```tsx
import {
  Legend, WebMcpChart
} from '@syncfusion/ej2-react-charts';
```

**Copy this injection block inside `ChartComponent`:**

```tsx
<Inject services={[
  Legend, WebMcpChart
]} />
```

**Add these required Chart properties:**

```tsx
legendSettings={{ visible: true }}
```

**Direct execution:**

```tsx
await executeTool('getChartLegend', {});
```

**Preconditions and notes:**

- No additional tool-specific precondition beyond a rendered WebMCP-enabled Chart.

## Final copy rules

- Copy `ChartComponent`, `Inject`, `SeriesCollectionDirective`, and `SeriesDirective` when the JSX uses them.
- Copy `useRef` from React when direct execution uses `chartRef`.
- Copy every feature module shown for the selected tool from `@syncfusion/ej2-react-charts`.
- Copy the same feature modules into `<Inject services={[...]}/>`.
- Never place `ChartComponent`, `Inject`, `SeriesCollectionDirective`, or `SeriesDirective` inside the `services` array.
- Always retain `WebMcpChart` in both the import and injection list.
- Use the full import and injection lists for `addChartSeries`, `updateChartSeries`, `addChartAxis`, `updateChartAxis`, `getChartAxes`, `removeChartAxis`, and `updateChartProperties`.
- Keep modules required by the initially rendered Chart even if a tool only reads or removes content.
- Do not use Angular `*Service` names in React.
- Do not use `Chart.Inject(...)` in React. Use `<Inject services={[...]}/>`.
- Do not add `registerWebMcpTools()` or a registration button. Registration remains automatic.
- Do not invent execution results or model-context responses.

## AI validation checklist

- [ ] `WebMcpChart` is injected.
- [ ] The Chart has a stable ID.
- [ ] `enableWebMcp` is true.
- [ ] Automatic registration is used.
- [ ] No registration button or `registerWebMcpTools()` call is generated.
- [ ] Explicit names belong to the verified 37-tool list.
- [ ] The complete Chart module list is imported and injected so customers can change the series type and enabled features.
- [ ] `WebMcpChart` remains in the injection list.
- [ ] The AI has not reduced modules based only on the initial Line and Column series.
- [ ] Write tools retain confirmation unless an intentional override is requested.
- [ ] No fake model-context UI or fabricated response is generated.
