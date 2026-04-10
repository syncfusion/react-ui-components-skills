# Appearance Customization

## Table of contents

- [Overview](#overview)
- [Sparkline Border](#sparkline-border)
  - [Basic Border Configuration](#basic-border-configuration)
  - [Border Properties](#border-properties)
  - [Custom Border Styles](#custom-border-styles)
  - [No Border (Default)](#no-border-default)
- [Sparkline Padding](#sparkline-padding)
  - [Setting Uniform Padding](#setting-uniform-padding)
  - [Individual Padding Control](#individual-padding-control)
  - [Zero Padding](#zero-padding)
- [Sparkline Area Background](#sparkline-area-background)
  - [Setting Background Color](#setting-background-color)
  - [Background with Border](#background-with-border)
  - [Transparent Background (Default)](#transparent-background-default)
- [Sparkline Themes](#sparkline-themes)
  - [Applying Themes](#applying-themes)
  - [Highcontrast Theme for Dark Mode](#highcontrast-theme-for-dark-mode)
  - [Bootstrap Theme](#bootstrap-theme)
  - [Fabric Theme](#fabric-theme)
- [Color Customization](#color-customization)
  - [Fill Color](#fill-color)
  - [Line Width (for Line type)](#line-width-for-line-type)
  - [Opacity (for Area type)](#opacity-for-area-type)
  - [Border for Area/Column](#border-for-areacolumn)
- [Complete Styling Examples](#complete-styling-examples)
  - [Professional Light Theme](#professional-light-theme)
  - [Dark Mode Sparkline](#dark-mode-sparkline)
  - [Minimal Clean Design](#minimal-clean-design)
  - [Vibrant Gradient-Style](#vibrant-gradient-style)
- [Best Practices](#best-practices)
  - [Consistent Design Language](#consistent-design-language)
  - [Contrast for Readability](#contrast-for-readability)
  - [Padding for Breathing Room](#padding-for-breathing-room)
  - [Theme-Based Styling](#theme-based-styling)
  - [Color Meaning](#color-meaning)
- [Summary](#summary)

## Overview

The appearance of the Syncfusion React Sparkline can be extensively customized to match your application's design system. This includes:

- **Border:** Customize the container area border
- **Padding:** Control spacing between container and sparkline
- **Background:** Set custom background colors
- **Themes:** Apply predefined themes (Material, Fabric, Bootstrap, Highcontrast)
- **Colors:** Customize fill colors, point colors, and more

## Sparkline Border

The `containerArea` property with its `border` sub-property controls the border around the sparkline area.

### Basic Border Configuration

```typescript
import { SparklineComponent } from '@syncfusion/ej2-react-charts';
import * as React from 'react';

function SparklineWithBorder() {
  const data: number[] = [0, 6, 4, 1, 3, 2, 5];
  
  return (
    <SparklineComponent
      dataSource={data}
      type="Line"
      height="150px"
      width="300px"
      containerArea={{
        border: {
          color: '#033e96',
          width: 2
        }
      }}
    />
  );
}

export default SparklineWithBorder;
```

### Border Properties

| Property | Type | Description | Default |
|----------|------|-------------|---------|
| `color` | string | Border color (hex, rgb, or color name) | - |
| `width` | number | Border width in pixels | 0 |

### Custom Border Styles

```typescript
function CustomBorderSparkline() {
  const data: number[] = [3, 6, 4, 1, 3, 2, 5];
  
  return (
    <SparklineComponent
      dataSource={data}
      type="Column"
      height="150px"
      width="300px"
      fill="#4A90E2"
      containerArea={{
        border: {
          color: '#2C5F8D',
          width: 3
        }
      }}
    />
  );
}
```

### No Border (Default)

```typescript
function NoBorderSparkline() {
  const data: number[] = [0, 6, 4, 1, 3, 2, 5];
  
  return (
    <SparklineComponent
      dataSource={data}
      type="Area"
      height="150px"
      width="300px"
      // No containerArea specified = no border
    />
  );
}
```

## Sparkline Padding

Padding controls the space between the container edge and the sparkline itself. By default, padding is set to 5 pixels on all sides.

### Setting Uniform Padding

```typescript
import { SparklineComponent } from '@syncfusion/ej2-react-charts';
import * as React from 'react';

function SparklineWithPadding() {
  const data: number[] = [0, 6, 4, 1, 3, 2, 5];
  
  return (
    <SparklineComponent
      dataSource={data}
      type="Line"
      height="150px"
      width="300px"
      padding={{
        left: 20,
        right: 20,
        top: 20,
        bottom: 20
      }}
      containerArea={{
        border: { color: '#999', width: 1 }
      }}
    />
  );
}

export default SparklineWithPadding;
```

### Individual Padding Control

```typescript
function CustomPaddingSparkline() {
  const data: number[] = [3, 6, 4, 1, 3, 2, 5];
  
  return (
    <SparklineComponent
      dataSource={data}
      type="Column"
      height="200px"
      width="400px"
      padding={{
        left: 10,
        right: 10,
        top: 30,    // More space at top
        bottom: 15  // Less space at bottom
      }}
      containerArea={{
        border: { color: '#4CAF50', width: 2 },
        background: '#f5f5f5'
      }}
    />
  );
}
```

### Zero Padding

```typescript
function NoPaddingSparkline() {
  const data: number[] = [0, 6, 4, 1, 3, 2, 5];
  
  return (
    <SparklineComponent
      dataSource={data}
      type="Area"
      height="150px"
      width="300px"
      padding={{
        left: 0,
        right: 0,
        top: 0,
        bottom: 0
      }}
    />
  );
}
```

**Use case for zero padding:** When you want the sparkline to fill the entire container, useful for grid cell integrations.

## Sparkline Area Background

The background color of the sparkline container can be customized using the `containerArea.background` property. By default, the sparkline background is transparent.

### Setting Background Color

```typescript
import { SparklineComponent } from '@syncfusion/ej2-react-charts';
import * as React from 'react';

function SparklineWithBackground() {
  const data: number[] = [0, 6, 4, 1, 3, 2, 5];
  
  return (
    <SparklineComponent
      dataSource={data}
      type="Line"
      height="150px"
      width="300px"
      fill="#e3165b"
      containerArea={{
        background: '#ffffcc'
      }}
    />
  );
}

export default SparklineWithBackground;
```

### Background with Border

```typescript
function BackgroundWithBorder() {
  const data: number[] = [3, 6, 4, 1, 3, 2, 5];
  
  return (
    <SparklineComponent
      dataSource={data}
      type="Column"
      height="150px"
      width="300px"
      fill="#2196F3"
      containerArea={{
        background: '#E3F2FD',
        border: {
          color: '#2196F3',
          width: 1
        }
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
```

### Transparent Background (Default)

```typescript
function TransparentBackground() {
  const data: number[] = [0, 6, 4, 1, 3, 2, 5];
  
  return (
    <SparklineComponent
      dataSource={data}
      type="Area"
      height="150px"
      width="300px"
      fill="#9C27B0"
      opacity={0.6}
      // No background specified = transparent
    />
  );
}
```

## Sparkline Themes

The sparkline supports four predefined themes that control the colors of data labels and track lines:

- **Material** (default)
- **Fabric**
- **Bootstrap**
- **Highcontrast**

### Applying Themes

```typescript
import { SparklineComponent, Inject, SparklineTooltip } from '@syncfusion/ej2-react-charts';
import * as React from 'react';

function MaterialThemeSparkline() {
  const data: number[] = [0, 6, 4, 1, 3, 2, 5];
  
  return (
    <SparklineComponent
      dataSource={data}
      type="Line"
      height="150px"
      width="300px"
      theme="Material"
      tooltipSettings={{
        visible: true
      }}
      dataLabelSettings={{
        visible: ['All']
      }}
    >
      <Inject services={[SparklineTooltip]} />
    </SparklineComponent>
  );
}

export default MaterialThemeSparkline;
```

### Highcontrast Theme for Dark Mode

The Highcontrast theme sets data label and track line colors to white for dark backgrounds:

```typescript
function HighcontrastThemeSparkline() {
  const data: number[] = [0, 6, 4, 1, 3, 2, 5];
  
  return (
    <div style={{ backgroundColor: '#000', padding: '20px' }}>
      <SparklineComponent
        dataSource={data}
        type="Line"
        height="150px"
        width="300px"
        theme="Highcontrast"
        fill="#00BCD4"
        dataLabelSettings={{
          visible: ['Start', 'End'],
          textStyle: { size: '12px' }
        }}
        tooltipSettings={{
          visible: true
        }}
      >
        <Inject services={[SparklineTooltip]} />
      </SparklineComponent>
    </div>
  );
}
```

### Bootstrap Theme

```typescript
function BootstrapThemeSparkline() {
  const data: number[] = [3, 6, 4, 1, 3, 2, 5];
  
  return (
    <SparklineComponent
      dataSource={data}
      type="Column"
      height="150px"
      width="300px"
      theme="Bootstrap"
      fill="#0275d8"
      dataLabelSettings={{
        visible: ['All'],
        textStyle: { size: '10px' }
      }}
    />
  );
}
```

### Fabric Theme

```typescript
function FabricThemeSparkline() {
  const data: number[] = [0, 6, 4, 1, 3, 2, 5];
  
  return (
    <SparklineComponent
      dataSource={data}
      type="Area"
      height="150px"
      width="300px"
      theme="Fabric"
      fill="#0078d4"
      opacity={0.5}
      dataLabelSettings={{
        visible: ['High', 'Low']
      }}
    />
  );
}
```

**Theme Impact:**
- Themes primarily affect data label text colors and track line colors
- For light themes (Material, Fabric, Bootstrap): Uses black text
- For Highcontrast theme: Uses white text
- Does not affect sparkline fill colors (customize separately)

## Color Customization

Beyond themes, you can customize individual colors for various sparkline elements.

### Fill Color

The primary color of the sparkline:

```typescript
function CustomFillColor() {
  const data: number[] = [0, 6, 4, 1, 3, 2, 5];
  
  return (
    <SparklineComponent
      dataSource={data}
      type="Line"
      height="150px"
      width="300px"
      fill="#E91E63"
      lineWidth={3}
    />
  );
}
```

### Line Width (for Line type)

```typescript
function CustomLineWidth() {
  const data: number[] = [0, 6, 4, 1, 3, 2, 5];
  
  return (
    <SparklineComponent
      dataSource={data}
      type="Line"
      height="150px"
      width="300px"
      fill="#4CAF50"
      lineWidth={4}
    />
  );
}
```

### Opacity (for Area type)

```typescript
function CustomOpacity() {
  const data: number[] = [0, 6, 4, 1, 3, 2, 5];
  
  return (
    <SparklineComponent
      dataSource={data}
      type="Area"
      height="150px"
      width="300px"
      fill="#9C27B0"
      opacity={0.3}
    />
  );
}
```

### Border for Area/Column

```typescript
function CustomBorder() {
  const data: number[] = [3, 6, 4, 1, 3, 2, 5];
  
  return (
    <SparklineComponent
      dataSource={data}
      type="Column"
      height="150px"
      width="300px"
      fill="#FF9800"
      border={{
        color: '#E65100',
        width: 2
      }}
    />
  );
}
```

## Complete Styling Examples

### Professional Light Theme

```typescript
function ProfessionalLightSparkline() {
  const data = [
    { month: 'Jan', value: 45 },
    { month: 'Feb', value: 52 },
    { month: 'Mar', value: 48 },
    { month: 'Apr', value: 60 },
    { month: 'May', value: 58 },
    { month: 'Jun', value: 68 }
  ];
  
  return (
    <SparklineComponent
      dataSource={data}
      xName="month"
      yName="value"
      type="Area"
      height="200px"
      width="100%"
      theme="Material"
      fill="#4A90E2"
      opacity={0.4}
      border={{ color: '#4A90E2', width: 2 }}
      containerArea={{
        background: '#F8FAFC',
        border: { color: '#E2E8F0', width: 1 }
      }}
      padding={{
        left: 15,
        right: 15,
        top: 15,
        bottom: 15
      }}
      markerSettings={{
        visible: ['High', 'Low'],
        size: 6,
        fill: '#4A90E2',
        border: { color: '#fff', width: 2 }
      }}
    />
  );
}
```

### Dark Mode Sparkline

```typescript
function DarkModeSparkline() {
  const data: number[] = [3, 6, 4, 1, 3, 2, 5, 4, 6];
  
  return (
    <div style={{ backgroundColor: '#1E1E1E', padding: '20px' }}>
      <SparklineComponent
        dataSource={data}
        type="Column"
        height="150px"
        width="100%"
        theme="Highcontrast"
        fill="#00E676"
        containerArea={{
          background: '#2D2D2D',
          border: { color: '#404040', width: 1 }
        }}
        padding={{
          left: 12,
          right: 12,
          top: 12,
          bottom: 12
        }}
        highPointColor="#00E676"
        lowPointColor="#FF5252"
      />
    </div>
  );
}
```

### Minimal Clean Design

```typescript
function MinimalSparkline() {
  const data: number[] = [0, 6, 4, 1, 3, 2, 5];
  
  return (
    <SparklineComponent
      dataSource={data}
      type="Line"
      height="80px"
      width="200px"
      fill="#000000"
      lineWidth={2}
      padding={{
        left: 5,
        right: 5,
        top: 5,
        bottom: 5
      }}
      // No border, no background = minimal look
    />
  );
}
```

### Vibrant Gradient-Style

```typescript
function VibrantGradientSparkline() {
  const data: number[] = [2, 8, 5, 3, 7, 4, 9, 6];
  
  return (
    <SparklineComponent
      dataSource={data}
      type="Area"
      height="180px"
      width="100%"
      fill="#FF6B6B"
      opacity={0.6}
      border={{ color: '#C92A2A', width: 3 }}
      containerArea={{
        background: 'linear-gradient(to bottom, #FFF5F5, #FFC9C9)',
        border: { color: '#FF6B6B', width: 2 }
      }}
      padding={{
        left: 20,
        right: 20,
        top: 20,
        bottom: 20
      }}
      markerSettings={{
        visible: ['All'],
        size: 5,
        fill: '#C92A2A',
        border: { color: '#fff', width: 1 }
      }}
    />
  );
}
```

## Best Practices

### 1. Consistent Design Language

Match your sparkline theme and colors to your application's design system:

```typescript
// Define theme colors
const brandColors = {
  primary: '#4A90E2',
  secondary: '#50C878',
  background: '#F8FAFC',
  border: '#E2E8F0'
};

function BrandedSparkline({ data }: { data: number[] }) {
  return (
    <SparklineComponent
      dataSource={data}
      type="Area"
      height="150px"
      width="100%"
      fill={brandColors.primary}
      opacity={0.4}
      containerArea={{
        background: brandColors.background,
        border: { color: brandColors.border, width: 1 }
      }}
    />
  );
}
```

### 2. Contrast for Readability

Ensure sufficient contrast, especially for data labels:

```typescript
function AccessibleSparkline() {
  const data: number[] = [0, 6, 4, 1, 3, 2, 5];
  
  return (
    <SparklineComponent
      dataSource={data}
      type="Line"
      height="150px"
      width="300px"
      fill="#0066CC"
      lineWidth={3}
      dataLabelSettings={{
        visible: ['Start', 'End'],
        fill: '#FFFFFF',
        border: { color: '#0066CC', width: 1 },
        textStyle: {
          color: '#0066CC',
          fontWeight: 'bold',
          size: '12px'
        }
      }}
    />
  );
}
```

### 3. Padding for Breathing Room

Use adequate padding to prevent sparkline from touching container edges:

```typescript
function WellPaddedSparkline() {
  const data: number[] = [3, 6, 4, 1, 3, 2, 5];
  
  return (
    <SparklineComponent
      dataSource={data}
      type="Column"
      height="150px"
      width="300px"
      padding={{
        left: 15,
        right: 15,
        top: 15,
        bottom: 15
      }}
      containerArea={{
        border: { color: '#ccc', width: 1 }
      }}
    />
  );
}
```

### 4. Theme-Based Styling

Adapt colors based on theme for dark mode support:

```typescript
function ThemeAwareSparkline({ isDarkMode }: { isDarkMode: boolean }) {
  const data: number[] = [0, 6, 4, 1, 3, 2, 5];
  
  const colors = isDarkMode
    ? {
        fill: '#00E676',
        background: '#2D2D2D',
        border: '#404040',
        theme: 'Highcontrast' as const
      }
    : {
        fill: '#4CAF50',
        background: '#F5F5F5',
        border: '#E0E0E0',
        theme: 'Material' as const
      };
  
  return (
    <SparklineComponent
      dataSource={data}
      type="Line"
      height="150px"
      width="300px"
      theme={colors.theme}
      fill={colors.fill}
      lineWidth={2}
      containerArea={{
        background: colors.background,
        border: { color: colors.border, width: 1 }
      }}
    />
  );
}
```

### 5. Color Meaning

Use colors purposefully to communicate meaning:

```typescript
function SemanticColorSparkline() {
  const data: number[] = [3, 6, -2, 4, -1, 3, 5];
  
  return (
    <SparklineComponent
      dataSource={data}
      type="Column"
      height="150px"
      width="300px"
      fill="#4CAF50"              // Green for positive
      negativePointColor="#F44336" // Red for negative
      highPointColor="#2196F3"     // Blue for highest
      lowPointColor="#FF9800"      // Orange for lowest
    />
  );
}
```

## Summary

- **Border:** Use `containerArea.border` to add borders around sparkline
- **Padding:** Use `padding` object to control spacing (default: 5px all sides)
- **Background:** Use `containerArea.background` to set background colors
- **Themes:** Choose from Material, Fabric, Bootstrap, or Highcontrast
- **Colors:** Customize `fill`, `lineWidth`, `opacity`, and point colors
- **Best Practices:** Maintain consistency, ensure contrast, use adequate padding
