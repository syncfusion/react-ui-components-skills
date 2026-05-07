# Expand Modes

## Table of Contents
- [Overview](#overview)
- [Single Expand Mode](#single-expand-mode)
- [Multiple Expand Mode](#multiple-expand-mode)
- [Setting Initial Expanded State](#setting-initial-expanded-state)
- [Toggle Behavior](#toggle-behavior)
- [Use Cases](#use-cases)
- [Comparison](#comparison)

## Overview

The Accordion component supports two expand modes that control how many panels can be expanded simultaneously:

- **Single** - Only one panel can be open at a time
- **Multiple** - Multiple panels can be open at the same time (default)

Choose the mode based on your UI/UX requirements and content organization strategy.

## Single Expand Mode

In Single mode, expanding a new panel automatically collapses the previously expanded panel.

### Basic Implementation

```jsx
import React from 'react';
import { 
  AccordionComponent, 
  AccordionItemDirective, 
  AccordionItemsDirective 
} from '@syncfusion/ej2-react-navigations';

export default function App() {
  return (
    <AccordionComponent expandMode='Single'>
      <AccordionItemsDirective>
        <AccordionItemDirective header='Question 1' content='Answer to question 1' />
        <AccordionItemDirective header='Question 2' content='Answer to question 2' />
        <AccordionItemDirective header='Question 3' content='Answer to question 3' />
      </AccordionItemsDirective>
    </AccordionComponent>
  );
}
```

### Behavior Example

1. User clicks "Question 1" → Panel 1 expands
2. User clicks "Question 2" → Panel 1 collapses, Panel 2 expands
3. User clicks "Question 2" again → Panel 2 collapses (toggle)
4. No panels are forced to stay expanded

### Single Mode Characteristics

- **One panel at a time** - Only single panel can be expanded
- **Auto-collapse** - Previous panel collapses when new one opens
- **Toggle support** - Click expanded header again to collapse it
- **Space efficient** - Shows minimal content, maximizes available space
- **Focused UX** - Guides user through content sequentially

### When User Needs Single Mode

Use Single expand mode for:
- **FAQ sections** - One question/answer visible at a time
- **Guided tours** - Present information step-by-step
- **Menu navigation** - Tree-like hierarchical menus
- **Sequential workflows** - Multi-step processes
- **Mobile-friendly layouts** - Conserves screen real estate

## Multiple Expand Mode

In Multiple mode (default), multiple panels can be expanded at the same time. Users can expand additional panels without collapsing previously expanded ones.

### Basic Implementation

```jsx
import React from 'react';
import { 
  AccordionComponent, 
  AccordionItemDirective, 
  AccordionItemsDirective 
} from '@syncfusion/ej2-react-navigations';

export default function App() {
  return (
    <AccordionComponent expandMode='Multiple'>
      <AccordionItemsDirective>
        <AccordionItemDirective header='Feature 1' content='Details about feature 1' />
        <AccordionItemDirective header='Feature 2' content='Details about feature 2' />
        <AccordionItemDirective header='Feature 3' content='Details about feature 3' />
      </AccordionItemsDirective>
    </AccordionComponent>
  );
}
```

### Behavior Example

1. User clicks "Feature 1" → Panel 1 expands
2. User clicks "Feature 2" → Panel 1 stays expanded, Panel 2 also expands
3. User clicks "Feature 2" again → Panel 2 collapses (Panel 1 remains expanded)
4. Multiple panels can remain expanded indefinitely

### Multiple Mode Characteristics

- **Multiple panels expanded** - Any number of panels can be open
- **Independence** - Each panel expands/collapses independently
- **Toggle support** - Click expanded header to collapse without affecting others
- **Content visibility** - Show all related content simultaneously
- **Natural interaction** - Users control what they view

### When User Needs Multiple Mode

Use Multiple expand mode for:
- **Settings panels** - Review multiple configuration options at once
- **Feature showcase** - Display multiple capabilities together
- **Dashboard sections** - View multiple data widgets simultaneously
- **Comparison layouts** - See multiple items side-by-side
- **Form sections** - Preview related form fields together
- **Content organization** - Browse topics without forced navigation

## Setting Initial Expanded State

Use the `expanded` property to pre-expand specific panels on component load.

### Single Pre-expanded Panel

```jsx
<AccordionComponent expandMode='Single'>
  <AccordionItemsDirective>
    <AccordionItemDirective 
      expanded={true}
      header='Getting Started' 
      content='This section opens by default when component loads.' 
    />
    <AccordionItemDirective header='Advanced Usage' content='Revealed when user clicks.' />
    <AccordionItemDirective header='API Reference' content='Revealed when user clicks.' />
  </AccordionItemsDirective>
</AccordionComponent>
```

**Result:** "Getting Started" is expanded on page load; others are collapsed

### Multiple Pre-expanded Panels

```jsx
<AccordionComponent expandMode='Multiple'>
  <AccordionItemsDirective>
    <AccordionItemDirective 
      expanded={true}
      header='Installation' 
      content='Installation instructions...' 
    />
    <AccordionItemDirective 
      expanded={true}
      header='Quick Start' 
      content='Quick start guide...' 
    />
    <AccordionItemDirective 
      header='Examples' 
      content='More examples...' 
    />
  </AccordionItemsDirective>
</AccordionComponent>
```

**Result:** "Installation" and "Quick Start" are expanded on load; "Examples" collapsed

### Conditional Expanded State

Pre-expand panels based on component state:

```jsx
import React, { useState } from 'react';
import { 
  AccordionComponent, 
  AccordionItemDirective, 
  AccordionItemsDirective 
} from '@syncfusion/ej2-react-navigations';

export default function App() {
  const [isNewUser, setIsNewUser] = useState(true);

  return (
    <AccordionComponent>
      <AccordionItemsDirective>
        <AccordionItemDirective 
          expanded={isNewUser}
          header='New User Guide' 
          content='Step-by-step guide for new users...' 
        />
        <AccordionItemDirective 
          expanded={!isNewUser}
          header='Advanced Options' 
          content='For experienced users...' 
        />
      </AccordionItemsDirective>
    </AccordionComponent>
  );
}
```

**Behavior:** Shows different sections based on user type

## Toggle Behavior

Toggle functionality allows users to click an expanded header to collapse it.

### Default Toggle Behavior

```jsx
<AccordionComponent>
  <AccordionItemsDirective>
    <AccordionItemDirective 
      expanded={true}
      header='Click Me' 
      content='Content here' 
    />
  </AccordionItemsDirective>
</AccordionComponent>
```

**User interactions:**
- First click on header → Expands if collapsed
- Second click on header → Collapses if expanded
- Third click on header → Expands again

This works in both Single and Multiple modes.

### Toggle in Single Mode

In Single mode, toggle behavior is enhanced:

```jsx
<AccordionComponent expandMode='Single'>
  <AccordionItemsDirective>
    <AccordionItemDirective expanded={true} header='Section 1' content='...' />
    <AccordionItemDirective header='Section 2' content='...' />
    <AccordionItemDirective header='Section 3' content='...' />
  </AccordionItemsDirective>
</AccordionComponent>
```

**Interaction flow:**
1. Section 1 starts expanded
2. Click Section 2 → Section 1 collapses, Section 2 expands
3. Click Section 1 → Section 2 collapses, Section 1 expands
4. Click Section 1 again → Section 1 collapses (all sections closed)

### Toggle in Multiple Mode

In Multiple mode, each panel toggles independently:

```jsx
<AccordionComponent expandMode='Multiple'>
  <AccordionItemsDirective>
    <AccordionItemDirective expanded={true} header='Section 1' content='...' />
    <AccordionItemDirective expanded={true} header='Section 2' content='...' />
    <AccordionItemDirective header='Section 3' content='...' />
  </AccordionItemsDirective>
</AccordionComponent>
```

**Interaction flow:**
1. Sections 1 and 2 start expanded
2. Click Section 1 → Section 1 collapses (Section 2 stays expanded)
3. Click Section 3 → Section 3 expands (Sections 1 and 2 remain in their states)
4. Multiple panels can remain expanded

## Use Cases

### FAQ Section (Single Mode Recommended)

```jsx
<AccordionComponent expandMode='Single'>
  <AccordionItemsDirective>
    <AccordionItemDirective 
      expanded={true}
      header='How do I get started?' 
      content='Visit our getting started guide...' 
    />
    <AccordionItemDirective 
      header='What are the pricing options?' 
      content='We offer three pricing tiers...' 
    />
    <AccordionItemDirective 
      header='Is there a free trial?' 
      content='Yes, we offer a 14-day free trial...' 
    />
  </AccordionItemsDirective>
</AccordionComponent>
```

**Why Single Mode:** Users browse one question at a time, focusing on relevant information

### Settings Panel (Multiple Mode Recommended)

```jsx
<AccordionComponent expandMode='Multiple'>
  <AccordionItemsDirective>
    <AccordionItemDirective 
      expanded={true}
      header='Account Settings' 
      content='Email, password, 2FA options...' 
    />
    <AccordionItemDirective 
      expanded={true}
      header='Notification Preferences' 
      content='Email alerts, SMS notifications...' 
    />
    <AccordionItemDirective 
      header='Privacy & Security' 
      content='Data sharing, security logs...' 
    />
  </AccordionItemsDirective>
</AccordionComponent>
```

**Why Multiple Mode:** Users review multiple settings simultaneously before saving changes

## Comparison

| Aspect | Single Mode | Multiple Mode |
|--------|-------------|----------------|
| **Panels Open** | One at a time | Multiple simultaneously |
| **Default Behavior** | Auto-collapse previous | Independent panels |
| **Use Case** | FAQs, Step-by-step guides | Settings, Dashboards |
| **Space Efficient** | Yes | No (shows more content) |
| **User Control** | Guided (one section) | Full control (view all) |
| **Toggle Click** | Can collapse to empty state | Toggles individual panels |
| **Initial Expanded** | Set one default | Can set multiple defaults |

---

## Troubleshooting

**Issue: Multiple panels collapse unexpectedly**
- Verify `expandMode='Multiple'` is set correctly
- Check that `expanded={true}` doesn't override mode behavior
- Inspect component state for conflicts

**Issue: Single mode not auto-collapsing**
- Confirm `expandMode='Single'` is specified
- Check for custom event handlers overriding default behavior

**Issue: Can't collapse all panels in Single mode**
- This is by-design - toggle behavior allows all-collapsed state
- Click expanded header again to collapse

**Issue: Pre-expanded state not working**
- Ensure `expanded={true}` is on `AccordionItemDirective`
- Verify it's not being overridden by component state
- Check browser console for React state conflicts
