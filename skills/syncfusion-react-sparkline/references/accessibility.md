# Accessibility

## Table of Contents
- [Overview](#overview)
- [Accessibility Standards Compliance](#accessibility-standards-compliance)
- [WAI-ARIA Attributes](#wai-aria-attributes)
- [Keyboard Interaction](#keyboard-interaction)
- [Screen Reader Support](#screen-reader-support)
- [Right-to-Left (RTL) Support](#right-to-left-rtl-support)
- [Color Contrast](#color-contrast)
- [Mobile Device Support](#mobile-device-support)
- [Accessibility Testing](#accessibility-testing)
- [Best Practices](#best-practices)

## Overview

The Syncfusion React Sparkline component follows accessibility guidelines and standards to ensure usability for all users, including those with disabilities. The component implements WAI-ARIA patterns, supports keyboard navigation, and provides screen reader compatibility.

**Supported Standards:**
- ADA (Americans with Disabilities Act)
- Section 508
- WCAG 2.2 (Web Content Accessibility Guidelines)
- WAI-ARIA (Web Accessibility Initiative - Accessible Rich Internet Applications)

## Accessibility Standards Compliance

The Sparkline component's compliance with various accessibility standards:

| Accessibility Criteria | Support Level | Description |
|------------------------|---------------|-------------|
| **WCAG 2.2 Support** | Partial | Core accessibility features implemented |
| **Section 508 Support** | Partial | Government accessibility requirements |
| **Screen Reader Support** | Partial | Compatible with major screen readers |
| **Right-To-Left Support** | Full | Complete RTL language support |
| **Color Contrast** | Full | Meets contrast requirements |
| **Mobile Device Support** | Full | Touch-friendly interactions |
| **Keyboard Navigation** | Partial | Basic keyboard support |
| **Accessibility Checker** | Full | Passes automated validation |
| **Axe-core Validation** | Full | Passes axe-core accessibility tests |

**Support Levels:**
- **Full (✓):** All features meet the requirement
- **Partial (⚠):** Some features meet the requirement
- **Not Supported (✗):** Component does not meet the requirement

### Compliance Notes

**Full Support:**
- Right-to-left (RTL) language display
- Color contrast ratios meet WCAG AA standards
- Mobile touch interactions
- Automated accessibility testing validation

**Partial Support:**
- WCAG 2.2: Core features comply, advanced features in progress
- Section 508: Basic requirements met
- Screen readers: Works with major readers, continuous improvements
- Keyboard navigation: Print functionality available

## WAI-ARIA Attributes

The Sparkline component implements WAI-ARIA attributes to provide semantic information to assistive technologies.

### Implemented ARIA Attributes

| ARIA Attribute | Purpose | Usage |
|----------------|---------|-------|
| `role="img"` | Identifies sparkline as an image | Applied to SVG container |
| `aria-label` | Provides descriptive label | Describes sparkline content |
| `aria-hidden` | Hides decorative elements | Applied to non-essential elements |

### ARIA Label Example

```typescript
import { SparklineComponent } from '@syncfusion/ej2-react-charts';
import * as React from 'react';

function AccessibleSparkline() {
  const salesData: number[] = [120, 150, 130, 180, 160, 200];
  
  return (
    <div role="region" aria-label="Sales Trend Visualization">
      <h3 id="sparkline-title">Monthly Sales Trend</h3>
      <SparklineComponent
        dataSource={salesData}
        type="Area"
        height="150px"
        width="400px"
        fill="#4A90E2"
        opacity={0.4}
      />
      <p className="sr-only">
        Sales data shows an upward trend from 120 to 200 over 6 months,
        with the highest value of 200 in the final month.
      </p>
    </div>
  );
}

export default AccessibleSparkline;
```

### Screen Reader Text

Include descriptive text for screen readers:

```typescript
function SparklineWithDescription() {
  const performanceScores: number[] = [65, 72, 68, 80, 85, 78, 92];
  const average = performanceScores.reduce((a, b) => a + b) / performanceScores.length;
  const max = Math.max(...performanceScores);
  const min = Math.min(...performanceScores);
  
  return (
    <div>
      <h3>Weekly Performance</h3>
      <SparklineComponent
        dataSource={performanceScores}
        type="Line"
        height="150px"
        width="400px"
        fill="#4CAF50"
        lineWidth={2}
      />
      <div className="sr-only">
        Performance scores range from {min} to {max}, 
        with an average of {average.toFixed(1)}. 
        The trend shows improvement over the 7-week period.
      </div>
    </div>
  );
}
```

**CSS for screen reader only text:**
```css
.sr-only {
  position: absolute;
  width: 1px;
  height: 1px;
  padding: 0;
  margin: -1px;
  overflow: hidden;
  clip: rect(0, 0, 0, 0);
  white-space: nowrap;
  border-width: 0;
}
```

## Keyboard Interaction

The Sparkline component follows keyboard interaction guidelines for accessibility.

### Supported Keyboard Shortcuts

| Key Combination | Action | Description |
|-----------------|--------|-------------|
| `Ctrl + P` | Print | Prints the sparkline |

### Print Functionality Example

```typescript
import { SparklineComponent } from '@syncfusion/ej2-react-charts';
import * as React from 'react';

function PrintableSparkline() {
  const data: number[] = [0, 6, 4, 1, 3, 2, 5];
  
  return (
    <div>
      <p>Press <kbd>Ctrl + P</kbd> to print this sparkline</p>
      <SparklineComponent
        dataSource={data}
        type="Line"
        height="150px"
        width="300px"
      />
    </div>
  );
}

export default PrintableSparkline;
```

### Focus Management

Ensure sparkline containers are keyboard accessible:

```typescript
function FocusableSparkline() {
  const data: number[] = [3, 6, 4, 1, 3, 2, 5];
  
  return (
    <div 
      tabIndex={0}
      role="img"
      aria-label="Sales trend showing fluctuating values"
      style={{ 
        border: '2px solid transparent',
        padding: '10px',
        borderRadius: '4px'
      }}
      onFocus={(e) => e.currentTarget.style.borderColor = '#4A90E2'}
      onBlur={(e) => e.currentTarget.style.borderColor = 'transparent'}
    >
      <SparklineComponent
        dataSource={data}
        type="Area"
        height="150px"
        width="300px"
      />
    </div>
  );
}
```

## Screen Reader Support

The sparkline is compatible with major screen readers including JAWS, NVDA, and VoiceOver.

### Screen Reader Best Practices

#### 1. Provide Context

```typescript
function ContextualSparkline() {
  const data: number[] = [120, 150, 130, 180, 160, 200];
  
  return (
    <section aria-labelledby="sales-heading">
      <h2 id="sales-heading">Q1 Sales Performance</h2>
      <p>
        Sales have increased from $120K in January to $200K in June,
        representing 67% growth.
      </p>
      <SparklineComponent
        dataSource={data}
        type="Column"
        height="150px"
        width="400px"
        fill="#4CAF50"
      />
    </section>
  );
}
```

#### 2. Data Tables for Complex Data

Provide a data table alternative for screen reader users:

```typescript
function SparklineWithTable() {
  const monthlyData = [
    { month: 'Jan', sales: 120 },
    { month: 'Feb', sales: 150 },
    { month: 'Mar', sales: 130 },
    { month: 'Apr', sales: 180 },
    { month: 'May', sales: 160 },
    { month: 'Jun', sales: 200 }
  ];
  
  return (
    <div>
      <h3>Monthly Sales</h3>
      <SparklineComponent
        dataSource={monthlyData}
        xName="month"
        yName="sales"
        type="Area"
        height="150px"
        width="400px"
        fill="#4A90E2"
        opacity={0.4}
      />
      
      <details>
        <summary>View data table</summary>
        <table>
          <caption>Monthly Sales Data (in thousands)</caption>
          <thead>
            <tr>
              <th scope="col">Month</th>
              <th scope="col">Sales ($K)</th>
            </tr>
          </thead>
          <tbody>
            {monthlyData.map((item, index) => (
              <tr key={index}>
                <td>{item.month}</td>
                <td>{item.sales}</td>
              </tr>
            ))}
          </tbody>
        </table>
      </details>
    </div>
  );
}
```

#### 3. Announce Dynamic Changes

```typescript
function DynamicSparkline() {
  const [data, setData] = React.useState([3, 6, 4, 1, 3, 2, 5]);
  const [announcement, setAnnouncement] = React.useState('');
  
  const refreshData = () => {
    const newData = Array.from({ length: 7 }, () => Math.floor(Math.random() * 10));
    setData(newData);
    setAnnouncement(`Data updated. New average: ${(newData.reduce((a, b) => a + b) / newData.length).toFixed(1)}`);
  };
  
  return (
    <div>
      <button onClick={refreshData}>Refresh Data</button>
      <SparklineComponent
        dataSource={data}
        type="Line"
        height="150px"
        width="300px"
      />
      <div aria-live="polite" aria-atomic="true" className="sr-only">
        {announcement}
      </div>
    </div>
  );
}
```

## Right-to-Left (RTL) Support

The Sparkline component fully supports RTL languages like Arabic and Hebrew.

### Enabling RTL

```typescript
import { SparklineComponent, Inject, SparklineTooltip } from '@syncfusion/ej2-react-charts';
import * as React from 'react';
import * as ReactDOM from 'react-dom';

function AccessibleSparkline() {
  const salesData = [120, 150, 130, 180, 160, 200];
  
  return (
    <div role="region" aria-label="Sales Trend Visualization">
      <h3 id="sparkline-title">Monthly Sales Trend</h3>
      <SparklineComponent
        enableRtl={true}
        dataSource={salesData}
        type="Area"
        height="150px"
        width="400px"
        fill="#4A90E2"
        opacity={0.4}
        tooltipSettings={{
          visible: true
        }}
      />
      <p className="sr-only">
        Sales data shows an upward trend from 120 to 200 over 6 months,
        with the highest value of 200 in the final month.
      </p>
    </div>
  );
}

export default AccessibleSparkline;
ReactDOM.render(<AccessibleSparkline />, document.getElementById('charts'));
```

## Color Contrast

The Sparkline component meets WCAG AA color contrast requirements.

### Contrast Guidelines

**WCAG AA Requirements:**
- Normal text: Minimum 4.5:1 contrast ratio
- Large text: Minimum 3:1 contrast ratio
- UI components: Minimum 3:1 contrast ratio

### High Contrast Examples

```typescript
function HighContrastSparkline() {
  const data: number[] = [0, 6, 4, 1, 3, 2, 5];
  
  return (
    <SparklineComponent
      dataSource={data}
      type="Line"
      height="150px"
      width="300px"
      fill="#0066CC"      // Dark blue - good contrast
      lineWidth={3}        // Thicker for visibility
      markerSettings={{
        visible: ['All'],
        size: 8,
        fill: '#0066CC',
        border: {
          color: '#FFFFFF',
          width: 2          // White border for contrast
        }
      }}
    />
  );
}
```

### Dark Mode Accessibility

```typescript
function DarkModeAccessible() {
  const data: number[] = [3, 6, 4, 1, 3, 2, 5];
  
  return (
    <div style={{ backgroundColor: '#1E1E1E', padding: '20px' }}>
      <SparklineComponent
        dataSource={data}
        type="Area"
        height="150px"
        width="300px"
        theme="Highcontrast"
        fill="#00E676"      // Bright green for dark background
        opacity={0.6}
        border={{ color: '#00E676', width: 2 }}
        dataLabelSettings={{
          visible: ['High', 'Low'],
          fill: '#FFFFFF',
          textStyle: {
            color: '#1E1E1E',
            fontWeight: 'bold'
          }
        }}
      />
    </div>
  );
}
```

### Color Blind Friendly

Use patterns in addition to color:

```typescript
function ColorBlindFriendly() {
  const data: number[] = [5, 8, -3, 10, -2, 7, -5, 12];
  
  return (
    <SparklineComponent
      dataSource={data}
      type="Column"
      height="150px"
      width="300px"
      fill="#0077BB"          // Blue (distinguishable)
      negativePointColor="#CC3311"  // Red (distinct from blue)
      markerSettings={{
        visible: ['All'],
        size: 6,
        fill: '#FFFFFF',
        border: { color: '#000000', width: 2 }
      }}
    />
  );
}
```

## Mobile Device Support

The Sparkline component is fully optimized for mobile devices and touch interactions.

### Touch-Friendly Sparkline

```typescript
function TouchFriendlySparkline() {
  const data: number[] = [0, 6, 4, 1, 3, 2, 5];
  
  return (
    <SparklineComponent
      dataSource={data}
      type="Area"
      height="180px"      // Larger for mobile
      width="100%"
      fill="#4A90E2"
      opacity={0.4}
      markerSettings={{
        visible: ['All'],
        size: 10          // Larger markers for touch
      }}
      tooltipSettings={{
        visible: true,
        format: 'Value: ${y}',
        textStyle: { size: '14px' }  // Larger text for mobile
      }}
    />
  );
}
```

### Responsive Design

```typescript
function ResponsiveAccessibleSparkline() {
  const data: number[] = [3, 6, 4, 1, 3, 2, 5];
  
  return (
    <div style={{ 
      width: '100%', 
      maxWidth: '600px',
      minHeight: '150px',
      padding: '15px'
    }}>
      <SparklineComponent
        dataSource={data}
        type="Line"
        height="150px"
        width="100%"
        fill="#2196F3"
        lineWidth={3}
        markerSettings={{
          visible: ['All'],
          size: 8
        }}
      />
    </div>
  );
}
```

## Accessibility Testing

The Sparkline component passes validation from industry-standard accessibility testing tools.

### Supported Testing Tools

1. **Accessibility Checker:** NPM package for automated validation
2. **Axe-core:** Industry-standard accessibility testing

### Testing Example

```bash
# Install accessibility testing tools
npm install --save-dev accessibility-checker axe-core
```

## Best Practices

### 1. Always Provide Context

```typescript
// ✅ Good: Clear context
function ContextualSparkline() {
  return (
    <section aria-labelledby="trend-heading">
      <h3 id="trend-heading">Website Traffic Trend</h3>
      <p>Showing daily visitors for the past week</p>
      <SparklineComponent dataSource={data} type="Line" height="150px" width="300px" />
    </section>
  );
}

// ❌ Bad: No context
function NoContextSparkline() {
  return <SparklineComponent dataSource={data} type="Line" height="150px" width="300px" />;
}
```

### 2. Include Text Alternatives

Provide screen reader descriptions for visual-only information:

```typescript
function TextAlternativeSparkline() {
  const data: number[] = [120, 150, 130, 180, 160, 200];
  const trend = data[data.length - 1] > data[0] ? 'increasing' : 'decreasing';
  
  return (
    <div>
      <SparklineComponent dataSource={data} type="Area" height="150px" width="300px" />
      <p className="sr-only">
        Sales trend is {trend}, from {data[0]} to {data[data.length - 1]}
      </p>
    </div>
  );
}
```

### 3. Ensure Sufficient Color Contrast

Use tools to verify contrast ratios:

```typescript
// ✅ Good: High contrast (4.5:1+)
<SparklineComponent
  fill="#0066CC"
  dataLabelSettings={{
    fill: "#FFFFFF",
    textStyle: { color: "#000000" }
  }}
/>

// ❌ Bad: Low contrast
<SparklineComponent
  fill="#DDDDDD"
  dataLabelSettings={{
    fill: "#EEEEEE",
    textStyle: { color: "#CCCCCC" }
  }}
/>
```

### 4. Test with Assistive Technologies

Regularly test with:
- Screen readers (JAWS, NVDA, VoiceOver)
- Keyboard-only navigation
- Mobile screen readers (TalkBack, VoiceOver)
- Browser zoom (200%+)

### 5. Provide Data Tables for Complex Sparklines

When sparklines show critical data, provide tabular alternatives:

```typescript
function AccessibleComplexSparkline() {
  const data = [...]; // Complex dataset
  
  return (
    <div>
      <SparklineComponent dataSource={data} type="Line" height="150px" width="300px" />
      <button onClick={() => showDataTable()}>View as table</button>
    </div>
  );
}
```

## Summary

- **Standards:** Complies with ADA, Section 508, WCAG 2.2, WAI-ARIA
- **ARIA:** Uses role="img", aria-label, aria-hidden attributes
- **Keyboard:** Supports Ctrl+P for printing
- **Screen Readers:** Compatible with JAWS, NVDA, VoiceOver
- **RTL:** Full support for right-to-left languages
- **Contrast:** Meets WCAG AA color contrast requirements
- **Mobile:** Touch-friendly with responsive design
- **Testing:** Passes accessibility-checker and axe-core validation
- **Best Practices:** Provide context, text alternatives, and test regularly
