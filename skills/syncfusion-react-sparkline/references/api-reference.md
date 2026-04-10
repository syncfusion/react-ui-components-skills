# Sparkline API Reference

This file summarizes the main API surface of `SparklineComponent` (from `@syncfusion/ej2-react-charts`) to be used as a quick reference when implementing sparklines.

## Import

```tsx
import { SparklineComponent, RangeBandSettingsDirective, RangeBandSettingDirective } from '@syncfusion/ej2-react-charts';
```

## Key Props (selected)
- `dataSource` (object[]): Data for the sparkline.
- `type` (SparklineType): 'Line' | 'Column' | 'Area' | 'WinLoss' | 'Pie'. See the official API: https://ej2.syncfusion.com/react/documentation/api/sparkline/sparklinetype
- `xName` (string): Field name for x values in `dataSource`.
- `yName` (string): Field name for y values in `dataSource`.
- `height` (string): Height of the sparkline (e.g., '100px').
- `width` (string): Width of the sparkline (e.g., '300px').
- `fill` (string): Primary fill color for the sparkline.
- `valueType` (SparklineValueType): 'Numeric' | 'Category' | 'DateTime'. See: https://ej2.syncfusion.com/react/documentation/api/sparkline/sparklinevaluetype
- `markerSettings` (SparklineMarkerSettingsModel): Configuration for markers (visible points, size, fill, border). See: https://ej2.syncfusion.com/react/documentation/api/sparkline/sparklinemarkersettingsmodel
- `dataLabelSettings` (SparklineDataLabelSettingsModel): Configuration for data labels (visibility, format, style). See: https://ej2.syncfusion.com/react/documentation/api/sparkline/sparklinedatalabelsettingsmodel
- `tooltipSettings` (SparklineTooltipSettingsModel): Tooltip config (visible, format, template). See: https://ej2.syncfusion.com/react/documentation/api/sparkline/sparklinetooltipsettingsmodel
- `axisSettings` (AxisSettingsModel): Axis customization (minX, maxX, minY, maxY, line styles). See: https://ej2.syncfusion.com/react/documentation/api/sparkline/axissettingsmodel
- `rangeBandSettings` (RangeBandSettingsModel[]): Array of range band objects `{ startRange, endRange, color, opacity }`. See: https://ej2.syncfusion.com/react/documentation/api/sparkline/rangebandsettingsmodel
- `padding` (PaddingModel): Padding around container. See: https://ej2.syncfusion.com/react/documentation/api/sparkline/paddingmodel
- `containerArea` (ContainerAreaModel): Configure container border/background. See: https://ej2.syncfusion.com/react/documentation/api/sparkline/containerareamodel
- `border` (SparklineBorderModel): Point/line border settings. See: https://ej2.syncfusion.com/react/documentation/api/sparkline/sparklinebordermodel
- `theme` (SparklineTheme): Theme name (Material, Fabric, Bootstrap, Highcontrast, etc.). See: https://ej2.syncfusion.com/react/documentation/api/sparkline/sparklinetheme
- `useGroupingSeparator` (boolean): Enable grouping separator (thousands separator).
- `enablePersistence` (boolean): Persist state across reloads.
- `enableRtl` (boolean): Enable right-to-left rendering.

## Methods
- Official component API (methods listed on the component page): https://ej2.syncfusion.com/react/documentation/api/sparkline/index-default
- `destroy(): void` — Clean up and remove the component instance. (See methods on the API page)
- `getModuleName(): string` — Returns the module name. (See methods on the API page)
- `renderSparkline(): void` — Re-renders the sparkline (useful after programmatic changes). (See methods on the API page)

## Events (selected)
- `load` — Fired before the sparkline starts loading. See event args: https://ej2.syncfusion.com/react/documentation/api/sparkline/isparklineloadeventargs
- `loaded` — Fired after the sparkline is rendered. See: https://ej2.syncfusion.com/react/documentation/api/sparkline/isparklineloadedeventargs
- `axisRendering` — Fired while axis options are being processed. See: https://ej2.syncfusion.com/react/documentation/api/sparkline/iaxisrenderingeventargs
- `dataLabelRendering` — Fired for each data label before it renders (can change `args.text`). See: https://ej2.syncfusion.com/react/documentation/api/sparkline/idatalabelrenderingeventargs
- `markerRendering` — Fired for each marker before render (can change fill/border). See: https://ej2.syncfusion.com/react/documentation/api/sparkline/imarkerrenderingeventargs
- `pointRegionMouseClick` — Mouse click on a point/region. See: https://ej2.syncfusion.com/react/documentation/api/sparkline/ipointregioneventargs
- `pointRegionMouseMove` — Mouse move over a point/region. See: https://ej2.syncfusion.com/react/documentation/api/sparkline/ipointregioneventargs
- `pointRendering` — Fired for each point before it renders. See: https://ej2.syncfusion.com/react/documentation/api/sparkline/isparklinepointeventargs
- `resize` — Fired when the sparkline container is resized. See: https://ej2.syncfusion.com/react/documentation/api/sparkline/isparklineresizeeventargs
- `seriesRendering` — Fired while series is being prepared. See: https://ej2.syncfusion.com/react/documentation/api/sparkline/iseriesrenderingeventargs
- `sparklineMouseClick` / `sparklineMouseMove` — General mouse events on the sparkline. See: https://ej2.syncfusion.com/react/documentation/api/sparkline/isparklinemouseeventargs
- `tooltipInitialize` — Fired before showing tooltip; can customize content. See: https://ej2.syncfusion.com/react/documentation/api/sparkline/itooltiprenderingeventargs

## Child Directives
- `RangeBandSettingsDirective` / `RangeBandSettingDirective` — Configure one or more range bands.

Example range band directive:

```jsx
<SparklineComponent ...>
  <RangeBandSettingsDirective>
    <RangeBandSettingDirective startRange={2} endRange={4} color="lightblue" opacity={0.4} />
  </RangeBandSettingsDirective>
</SparklineComponent>
```

## Usage Example (condensed)

```jsx
const data = [ { x:1, yval:5 }, { x:2, yval:6 }, { x:3, yval:5 } ];

<SparklineComponent
  id="sparkline"
  dataSource={data}
  xName="x"
  yName="yval"
  height="100px"
  width="300px"
  type="Line"
  markerSettings={{ visible: ['All'] }}
  dataLabelSettings={{ visible: ['End'], format: '${y}' }}
  tooltipSettings={{ visible: true }}
  load={(args) => { /* set theme or locale */ }}
  loaded={(args) => { /* post-render */ }}
/>
```

## Notes & Links
- This is a condensed, curated reference — consult the official API for exhaustive typings and all nested models: https://ej2.syncfusion.com/react/documentation/api/sparkline/index-default
