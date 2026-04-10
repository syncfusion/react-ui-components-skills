# Getting Started with Syncfusion React Maps

## Table of Contents
- [Overview](#overview)
- [Dependencies](#dependencies)
- [Installation](#installation)
- [Basic Setup](#basic-setup)
- [Understanding GeoJSON Data](#understanding-geojson-data)
- [Creating Your First Map](#creating-your-first-map)
- [Data Binding](#data-binding)
- [Module Injection](#module-injection)
- [CSS Theme Import](#css-theme-import)
- [Map Projections](#map-projections)
- [Complete Working Example](#complete-working-example)
- [Common Setup Issues](#common-setup-issues)

## Overview

The Syncfusion React Maps component visualizes geographical data using SVG rendering. This guide covers installation, basic setup, and your first map implementation.

## Dependencies

The Maps component requires these packages:

```json
{
  "@syncfusion/ej2-react-maps": "^latest",
  "@syncfusion/ej2-maps": "^latest",
  "@syncfusion/ej2-base": "^latest",
  "@syncfusion/ej2-svg-base": "^latest",
  "@syncfusion/ej2-data": "^latest",
  "@syncfusion/ej2-pdf-export": "^latest",
  "@syncfusion/ej2-react-base": "^latest"
}
```

All dependencies are installed automatically with the main package.

## Installation

### Using npm

```bash
npm install @syncfusion/ej2-react-maps --save
```

### Using Vite (Recommended)

Create a new React project with Vite:

```bash
npm create vite@latest my-maps-app -- --template react
cd my-maps-app
npm install
npm install @syncfusion/ej2-react-maps --save
```

### Using Create React App

```bash
npx create-react-app my-maps-app
cd my-maps-app
npm install @syncfusion/ej2-react-maps --save
```

## Basic Setup

### Project Structure

```
src/
├── App.jsx
├── world-map.ts          # GeoJSON data
└── data.ts               # Optional: custom data
```

### Importing Components

```tsx
import { MapsComponent, LayersDirective, LayerDirective } from '@syncfusion/ej2-react-maps';
```

## Understanding GeoJSON Data

Maps render shapes from GeoJSON format. GeoJSON structure:

```typescript
{
  "type": "FeatureCollection",
  "crs": { 
    "type": "name", 
    "properties": { "name": "urn:ogc:def:crs:OGC:1.3:CRS84" }
  },
  "features": [
    {
      "type": "Feature",
      "properties": { 
        "name": "United States",
        "iso_3166_2": "US"
      },
      "geometry": { 
        "type": "Polygon",
        "coordinates": [[[-125.0, 50.0], [-66.0, 50.0], [-66.0, 25.0], [-125.0, 25.0]]]
      }
    }
  ]
}
```

**Key Components:**
- `features`: Array of geographical features
- `properties`: Metadata (country name, codes, etc.)
- `geometry`: Coordinate data for shapes
- `coordinates`: Longitude/latitude pairs

### Getting GeoJSON Data

**Option 1: Download from Syncfusion**
```bash
# World map GeoJSON
# https://www.syncfusion.com/downloads/support/directtrac/general/ze/world_map1557035892

```

**Option 2: Use Custom GeoJSON**
- Create your own or download from sources like Natural Earth Data
- Save as `.ts` or `.json` file in your project

**Example: world-map.ts**

```typescript
export let world_map = {
  "type": "FeatureCollection",
  "features": [
    // ... feature data
  ]
};
```

## Creating Your First Map

### Minimal Example

```tsx
import * as React from 'react';
import { MapsComponent, LayersDirective, LayerDirective } from '@syncfusion/ej2-react-maps';
import { world_map } from './world-map';

function App() {
  return (
    <MapsComponent id="maps">
      <LayersDirective>
        <LayerDirective shapeData={world_map} />
      </LayersDirective>
    </MapsComponent>
  );
}

export default App;
```

**What's Happening:**
1. `MapsComponent`: Main container
2. `LayersDirective`: Wrapper for layers
3. `LayerDirective`: Individual layer with shape data
4. `shapeData`: GeoJSON data to render

## Data Binding

Bind custom data to map shapes for color-coding, tooltips, and labels.

### Data Source Structure

```typescript
// data.ts
export let countryData = [
  { Country: 'United States', Population: 331002651, Code: 'US' },
  { Country: 'China', Population: 1439323776, Code: 'CN' },
  { Country: 'India', Population: 1380004385, Code: 'IN' },
  { Country: 'Russia', Population: 145934462, Code: 'RU' }
];
```

### Remote Data Binding (DataManager)

React Maps can bind directly to remote data sources using Syncfusion’s DataManager, supporting `OData`, `ODataV4`, `WebAPI`, and custom adapters.
This enables scalable server-driven mapping scenarios.

```tsx
import { DataManager, ODataAdaptor } from '@syncfusion/ej2-data';

const remote = new DataManager({
   url: "https://services.odata.org/V4/Northwind/Northwind.svc/Customers",
   adaptor: new ODataAdaptor()
});

<LayerDirective
    shapeData={world_map}
    dataSource={remote}
    shapeDataPath="Country"
    shapePropertyPath="name"
/>
```

### Binding Data to Shapes

```tsx
import { countryData } from './data';

<LayerDirective
  shapeData={world_map}
  dataSource={countryData}
  shapeDataPath='Country'        // Field in dataSource
  shapePropertyPath='name'       // Field in GeoJSON properties
/>
```

**How It Works:**
- `dataSource`: Your custom data array
- `shapeDataPath`: Field in your data that identifies shapes
- `shapePropertyPath`: Matching field in GeoJSON `properties`
- When values match, data binds to that shape

### Example with Color Mapping

```tsx
import { world_map } from './world-map';
import { countryData } from './data';

function App() {
  return (
    <MapsComponent>
      <LayersDirective>
        <LayerDirective
          shapeData={world_map}
          dataSource={countryData}
          shapeDataPath='Country'
          shapePropertyPath='name'
          shapeSettings={{
            colorValuePath: 'Population',
            colorMapping: [
              { from: 0, to: 100000000, color: '#C3E6CB' },
              { from: 100000000, to: 500000000, color: '#6AB187' },
              { from: 500000000, to: 1500000000, color: '#2F7C4F' }
            ]
          }}
        />
      </LayersDirective>
    </MapsComponent>
  );
}
```

## Module Injection

Maps uses feature-based modules. Inject only what you need to reduce bundle size.

### Available Modules

```typescript
import {
  MapsComponent,
  Inject,
  Legend,         // Legend feature
  DataLabel,      // Data labels
  Marker,         // Markers
  Bubble,         // Bubble visualization
  MapsTooltip,    // Tooltips
  Zoom,           // Zoom and pan
  Highlight,      // Highlight shapes
  Selection,      // Select shapes
  NavigationLine, // Navigation lines
  Annotations,    // Annotations
  Polygon         // Polygon rendering
} from '@syncfusion/ej2-react-maps';
```

### Injecting Modules

```tsx
function App() {
  return (
    <MapsComponent>
      <Inject services={[Legend, MapsTooltip, Zoom]} />
      <LayersDirective>
        <LayerDirective shapeData={world_map} />
      </LayersDirective>
    </MapsComponent>
  );
}
```

**Best Practice**: Only inject modules for features you're actively using.

## CSS Theme Import

Import CSS for the theme you want to use.

### Available Themes

**(18+ including dark & high contrast):**
- Material, MaterialDark, Material3, Material3Dark
- Fabric, FabricDark
- Bootstrap, BootstrapDark, Bootstrap4, Bootstrap5, Bootstrap5Dark, Bootstrap5.3, Bootstrap5.3Dark
- Tailwind, TailwindDark
- Fluent, FluentDark, Fluent2, Fluent2Dark, Fluent2HighContrast
- HighContrast, HighContrastLight

Switch via the `theme` prop:

```tsx
<MapsComponent theme='Material3Dark'>
  {/* Maps content */}
</MapsComponent>
```

**Import at the top of your component or in `main.jsx`/`App.jsx`.**

## Map Projections

React Maps supports multiple geographical projection systems such as `Mercator`, `Miller`, `Winkel3`, and `Eckert` series. Choosing the right projection helps control shape distortion and improves geographical accuracy at different zoom levels.

```tsx
<LayerDirective 
    shapeData={world_map}
    projectionType="Mercator"
/>
```

## Complete Working Example

### App.jsx

```tsx
import * as React from 'react';
import { createRoot } from 'react-dom/client';
import { 
  MapsComponent, 
  LayersDirective, 
  LayerDirective,
  Inject,
  Legend,
  MapsTooltip
} from '@syncfusion/ej2-react-maps';
import { world_map } from './world-map';

function App() {
  const populationData = [
    { Country: 'China', Population: 1439323776 },
    { Country: 'United States', Population: 331002651 },
    { Country: 'India', Population: 1380004385 },
    { Country: 'Indonesia', Population: 273523615 },
    { Country: 'Pakistan', Population: 220892340 }
  ];

  return (
    <div style={{ margin: '20px' }}>
      <h2>World Population Map</h2>
      <MapsComponent
        id="maps"
        titleSettings={{
          text: 'Population by Country',
          textStyle: { size: '16px', fontWeight: '500' }
        }}
        legendSettings={{
          visible: true,
          position: 'Bottom'
        }}
      >
        <Inject services={[Legend, MapsTooltip]} />
        <LayersDirective>
          <LayerDirective
            shapeData={world_map}
            dataSource={populationData}
            shapeDataPath='Country'
            shapePropertyPath='name'
            shapeSettings={{
              fill: '#E5E5E5',
              colorValuePath: 'Population',
              colorMapping: [
                { from: 0, to: 100000000, color: '#C3E6CB', label: '< 100M' },
                { from: 100000000, to: 500000000, color: '#6AB187', label: '100M - 500M' },
                { from: 500000000, to: 1500000000, color: '#2F7C4F', label: '> 500M' }
              ]
            }}
            tooltipSettings={{
              visible: true,
              valuePath: 'Country',
              format: '${Country}: ${Population}'
            }}
          />
        </LayersDirective>
      </MapsComponent>
    </div>
  );
}

export default App;
```

### Running the Application

```bash
npm run dev    # For Vite
# or
npm start      # For Create React App
```

## Common Setup Issues

### Issue: Map not displaying

**Causes:**
- GeoJSON data not imported correctly
- CSS not imported
- Invalid GeoJSON structure

**Solutions:**
```tsx
// ✅ Correct import
import { world_map } from './world-map';

// ❌ Wrong - missing export
const world_map = { ... };
```

### Issue: "Inject is not defined"

**Cause:** Missing import for Inject

**Solution:**
```tsx
import { MapsComponent, Inject, Legend } from '@syncfusion/ej2-react-maps';
```

### Issue: Data not binding to shapes

**Causes:**
- `shapeDataPath` doesn't match data field
- `shapePropertyPath` doesn't match GeoJSON property
- Field name case mismatch

**Solution:**
```tsx
// Check GeoJSON structure
console.log(world_map.features[0].properties);
// Output: { name: 'United States', ... }

// Match the property name exactly
<LayerDirective
  shapePropertyPath='name'  // ✅ Matches GeoJSON
  shapeDataPath='Country'   // ✅ Matches your data
/>
```

### Issue: Module features not working

**Cause:** Forgot to inject the service

**Solution:**
```tsx
// ❌ Legend won't work without injection
<MapsComponent legendSettings={{ visible: true }}>

// ✅ Inject the service
<MapsComponent legendSettings={{ visible: true }}>
  <Inject services={[Legend]} />
</MapsComponent>
```

### Issue: Shapes render but no colors

**Cause:** Color mapping not configured or data not bound

**Solution:**
```tsx
<LayerDirective
  shapeData={world_map}
  dataSource={data}              // ✅ Bind data
  shapeDataPath='Country'        // ✅ Specify matching field
  shapePropertyPath='name'       // ✅ Specify GeoJSON field
  shapeSettings={{
    colorValuePath: 'Population', // ✅ Field to determine color
    colorMapping: [...]           // ✅ Color rules
  }}
/>
```
