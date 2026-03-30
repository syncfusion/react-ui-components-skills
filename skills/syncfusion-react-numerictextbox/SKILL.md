---
name: syncfusion-react-numerictextbox
description: Implement Syncfusion React NumericTextBox with comprehensive guidance on validation, formatting, and accessibility. Use this when working with numeric input fields, currency/percentage formatting, decimal precision, range validation, or spin button controls. This skill covers form integration and numeric UI requirements in React applications.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
  category: "Inputs"
---

# Implementing NumericTextBox

## Component Overview

The **NumericTextBox** is a Syncfusion React input component designed specifically for numeric data entry. It provides:

- **Formatted input** - Automatic number formatting (currency, percentage, scientific notation)
- **Validation** - Built-in min/max range validation with strict mode
- **Spin buttons** - Visual increment/decrement controls with customizable steps
- **Decimal control** - Manage decimal places and precision during input
- **Accessibility** - Full WCAG 2.2 compliance with ARIA attributes and keyboard navigation
- **Internationalization** - Locale support, RTL (Right-to-Left) languages
- **Form ready** - Integrates seamlessly with React forms and state management

## Documentation & Navigation Guide

When the user needs help with NumericTextBox, guide them to the appropriate reference:

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- Installation and package setup
- CSS imports and themes
- React component import and basic JSX
- Creating your first NumericTextBox
- Running the application

### Formats & Validation
📄 **Read:** [references/formats-and-validation.md](references/formats-and-validation.md)
- Standard format specifiers (currency, percentage, number, scientific)
- Custom number formats with # and 0 patterns
- Range validation with min and max properties
- strictMode for enforcing valid ranges
- Real-world formatting examples

### Spin Buttons & Step Control
📄 **Read:** [references/spin-buttons-and-step.md](references/spin-buttons-and-step.md)
- Enabling/disabling spin button arrows
- Step property for increment values
- Customizing spin button appearance and behavior
- Precision with step increments
- Keyboard shortcuts (Arrow Up/Down)

### Adornments & Styling
📄 **Read:** [references/adornments-and-styling.md](references/adornments-and-styling.md)
- Prefix and suffix text (units, currency symbols)
- CSS classes and custom styling
- Placeholder, disabled, and readonly states
- Focus and blur event handling
- Theme customization and appearance options

### Precision & Decimals
📄 **Read:** [references/precision-decimals.md](references/precision-decimals.md)
- decimals property for controlling decimal places
- validateDecimalOnType for real-time precision validation
- Maintaining trailing zeros in display
- Rounding behavior and edge cases
- Precision during input vs display

### Two-Way Binding & Forms
📄 **Read:** [references/two-way-binding-forms.md](references/two-way-binding-forms.md)
- Two-way value binding in React (value prop + onChange)
- Controlled component patterns
- React state management
- React Hook Form integration
- Form validation with NumericTextBox

### Globalization & Accessibility
📄 **Read:** [references/globalization-accessibility.md](references/globalization-accessibility.md)
- Internationalization and locale support
- Right-to-Left (RTL) language support
- WCAG 2.2 accessibility compliance
- Keyboard navigation and shortcuts
- Screen reader support with ARIA attributes
- Focus management and color contrast

### How-To Guide
📄 **Read:** [references/how-to-guide.md](references/how-to-guide.md)
- Custom validation patterns
- Preventing nullable input
- Maintaining trailing zeros
- Customizing spin button icons
- Common troubleshooting and edge cases

### API Reference
📄 **Read:** [references/api.md](references/api.md)
- Complete properties reference (value, min, max, step, format, decimals, etc.)
- Methods reference (increment, decrement, getText, focusIn, focusOut, destroy, etc.)
- Events reference with full argument types (change, blur, focus, created, destroyed)

## Quick Start Example

Here's a minimal working example to get started:

```jsx
import React, { useState } from 'react';
import { NumericTextBoxComponent } from '@syncfusion/ej2-react-inputs';
import '@syncfusion/ej2-base/styles/material3.css';
import '@syncfusion/ej2-buttons/styles/material3.css';
import '@syncfusion/ej2-inputs/styles/material3.css';

export default function App() {
  const [value, setValue] = useState(10);

  return (
    <div style={{ padding: '20px' }}>
      <h3>Enter a Number</h3>
      <NumericTextBoxComponent
        value={value}
        onChange={(e) => setValue(e.value)}
        min={0}
        max={100}
        step={1}
      />
      <p>Current Value: {value}</p>
    </div>
  );
}
```

**Key points:**
- Import `NumericTextBoxComponent` from `@syncfusion/ej2-react-inputs`
- Import required CSS themes (material3 in this example)
- Use `value` prop for the current numeric value
- Use `onChange` event to update React state
- Add `min`, `max`, `step` for validation and controls

## Common Patterns

### 1. Currency Input
```jsx
<NumericTextBoxComponent
  value={99.99}
  format="c2"
  min={0}
  placeholder="Enter amount"
/>
```

### 2. Percentage Input
```jsx
<NumericTextBoxComponent
  value={50}
  format="p"
  min={0}
  max={100}
/>
```

### 3. Integer-Only Input
```jsx
<NumericTextBoxComponent
  value={10}
  decimals={0}
  step={1}
  min={0}
/>
```

### 4. Bounded Range with Validation
```jsx
<NumericTextBoxComponent
  value={25}
  min={0}
  max={100}
  strictMode={true}
  placeholder="0-100"
/>
```

### 5. Form Field with Label
```jsx
<div>
  <label>Product Quantity:</label>
  <NumericTextBoxComponent
    value={qty}
    onChange={(e) => setQty(e.value)}
    min={1}
    step={1}
    prefix="Units: "
  />
</div>
```

## Key Properties Reference

| Property | Type | Purpose |
|----------|------|---------|
| `value` | number | Current numeric value |
| `min` | number | Minimum allowed value |
| `max` | number | Maximum allowed value |
| `step` | number | Increment/decrement step (default: 1) |
| `decimals` | number | Number of decimal places when focused |
| `format` | string | Number format (n2, c2, p2, e2, etc.) |
| `currency` | string | ISO 4217 currency code (e.g., 'USD', 'EUR') |
| `placeholder` | string | Placeholder text when empty |
| `floatLabelType` | FloatLabelType | Float label behavior ('Never', 'Always', 'Auto') |
| `readonly` | boolean | Prevent user input |
| `enabled` | boolean | Enable or disable the control (default: true) |
| `strictMode` | boolean | Enforce min/max validation (default: true) |
| `validateDecimalOnType` | boolean | Restrict decimal length during typing |
| `showSpinButton` | boolean | Show/hide spinner arrows (default: true) |
| `showClearButton` | boolean | Show/hide clear icon |
| `allowMouseWheel` | boolean | Enable mouse wheel increment/decrement (default: true) |
| `cssClass` | string | Additional CSS classes for custom styling |
| `width` | number \| string | Width of the component |

## Common Use Cases

**1. Shopping Cart - Quantity Input**
- Integer-only, min=1, step=1, spinner for easy adjustment

**2. Price Calculator - Currency Field**
- format="c2", min=0, prefix="$", two decimal places

**3. Rating or Score - 0-100 Range**
- min=0, max=100, strictMode=true, no decimals

**4. Discount Percentage**
- format="p", min=0, max=100, two decimal places

**5. Measurement Input**
- decimals=2, suffix=" cm", min=0, spinner for precision

**6. Financial Form**
- format="c2", validation, form integration, accessibility

## Next Steps

1. **Package requirement:** The packages `@syncfusion/ej2-react-inputs`, `@syncfusion/ej2-base`, and `@syncfusion/ej2-buttons` must be present in your project's `package.json`. Confirm they are already installed and that your lockfile (e.g., `package-lock.json` or `yarn.lock`) pins their versions for supply-chain integrity. When adding them, use an explicit version range such as `@syncfusion/ej2-react-inputs@^27.x.x` to avoid unpinned dependency risks.
2. **Getting Started reference:** For installation details and basic setup, see [references/getting-started.md](references/getting-started.md).
3. **Choose your reference:** Based on your use case (formatting, validation, forms, etc.), navigate to the relevant reference section above.
4. **Review examples:** Each reference contains ready-to-use code samples that can be adapted to your requirements.
5. **Customize:** Modify the examples to fit your specific use case and application needs.

---

For detailed implementation guidance, navigate to the appropriate reference file above.
