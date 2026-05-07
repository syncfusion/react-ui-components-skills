# Animation Effects

## Table of Contents
- [Overview](#overview)
- [Default Animations](#default-animations)
- [Available Animation Effects](#available-animation-effects)
- [Configuring Animation Properties](#configuring-animation-properties)
- [Custom Expand/Collapse Animations](#custom-expandcollapse-animations)
- [Disabling Animations](#disabling-animations)
- [Performance Considerations](#performance-considerations)
- [Common Patterns](#common-patterns)

## Overview

The Accordion component provides smooth animations when panels expand and collapse. By default, panels use `SlideDown` for expand and `SlideUp` for collapse animations. You can customize animations with various effects, durations, and easing functions.

Animation configuration includes:
- **Effect** - Type of animation (SlideDown, FadeIn, ZoomIn, etc.)
- **Duration** - How long animation runs in milliseconds
- **Easing** - Animation timing function (ease-in, ease-out, etc.)

## Default Animations

By default, the Accordion uses predefined animations for smooth expand/collapse:

### Default Configuration

```jsx
import React from 'react';
import { 
  AccordionComponent, 
  AccordionItemDirective, 
  AccordionItemsDirective 
} from '@syncfusion/ej2-react-navigations';

export default function App() {
  return (
    <AccordionComponent>
      <AccordionItemsDirective>
        <AccordionItemDirective header='Section 1' content='This uses default animations' />
        <AccordionItemDirective header='Section 2' content='SlideDown on expand, SlideUp on collapse' />
      </AccordionItemsDirective>
    </AccordionComponent>
  );
}
```

**Default Behavior:**
- **Expand:** SlideDown animation
- **Collapse:** SlideUp animation
- **Duration:** 400ms
- **Easing:** ease-out

These defaults provide natural, smooth transitions without additional configuration.

## Available Animation Effects

The Accordion supports multiple animation effects for both expand and collapse actions:

### Effect Types

| Effect | Description | Best For |
|--------|-------------|----------|
| `SlideDown` | Panel slides down smoothly | Default, natural expand |
| `SlideUp` | Panel slides up smoothly | Default, natural collapse |
| `FadeIn` | Panel fades in gradually | Subtle, minimalist UI |
| `FadeOut` | Panel fades out gradually | Subtle, minimalist UI |
| `FadeZoomIn` | Panel fades while growing | Modern, engaging |
| `FadeZoomOut` | Panel fades while shrinking | Modern, engaging |
| `ZoomIn` | Panel grows from center | Eye-catching expand |
| `ZoomOut` | Panel shrinks to center | Eye-catching collapse |
| `None` | No animation | Fast interaction |

### Choosing Effects

**For professional/minimal UI:**
```jsx
expand: { effect: 'FadeIn', duration: 300 }
collapse: { effect: 'FadeOut', duration: 300 }
```

**For modern/engaging UI:**
```jsx
expand: { effect: 'FadeZoomIn', duration: 400 }
collapse: { effect: 'FadeZoomOut', duration: 400 }
```

**For fast/responsive UI:**
```jsx
expand: { effect: 'None' }
collapse: { effect: 'None' }
```

## Configuring Animation Properties

### Basic Animation Configuration

Set animation properties on the Accordion component:

```jsx
import React from 'react';
import { 
  AccordionComponent, 
  AccordionItemDirective, 
  AccordionItemsDirective 
} from '@syncfusion/ej2-react-navigations';

export default function App() {
  const animationSettings = {
    expand: { effect: 'SlideDown', duration: 500 },
    collapse: { effect: 'SlideUp', duration: 500 }
  };

  return (
    <AccordionComponent animation={animationSettings}>
      <AccordionItemsDirective>
        <AccordionItemDirective header='Custom Animation' content='Slides at 500ms' />
        <AccordionItemDirective header='Another Item' content='Same animation effect' />
      </AccordionItemsDirective>
    </AccordionComponent>
  );
}
```

### Animation Properties

Each animation object accepts:

```js
{
  effect: 'SlideDown' | 'FadeIn' | 'ZoomIn' | 'None',  // Animation type
  duration: 400,                                          // Milliseconds
  easing: 'ease-out'                                      // CSS easing function
}
```

## Custom Expand/Collapse Animations

Set different animations for expand and collapse actions:

### Expand and Collapse with Different Effects

```jsx
const customAnimation = {
  expand: { 
    effect: 'FadeZoomIn', 
    duration: 400, 
    easing: 'ease-out' 
  },
  collapse: { 
    effect: 'FadeZoomOut', 
    duration: 300, 
    easing: 'ease-in' 
  }
};

<AccordionComponent animation={customAnimation}>
  <AccordionItemsDirective>
    <AccordionItemDirective header='Item' content='Different expand/collapse animations' />
  </AccordionItemsDirective>
</AccordionComponent>
```

**Behavior:**
- Expanding uses FadeZoomIn (400ms, ease-out)
- Collapsing uses FadeZoomOut (300ms, ease-in)
- Creates asymmetric animation experience

### Interactive Animation Selection

Change animations based on user preferences:

```jsx
import React, { useState, useRef } from 'react';
import { 
  AccordionComponent, 
  AccordionItemDirective, 
  AccordionItemsDirective 
} from '@syncfusion/ej2-react-navigations';
import { DropDownListComponent } from '@syncfusion/ej2-react-dropdowns';

export default function App() {
  const accordionRef = useRef(null);
  const [expandEffect, setExpandEffect] = useState('SlideDown');
  const [collapseEffect, setCollapseEffect] = useState('SlideUp');

  const effectOptions = ['SlideDown', 'SlideUp', 'FadeIn', 'FadeOut', 'ZoomIn', 'ZoomOut', 'None'];

  const handleExpandChange = (e) => {
    if (accordionRef.current) {
      accordionRef.current.animation.expand = { effect: e.value };
      setExpandEffect(e.value);
    }
  };

  const handleCollapseChange = (e) => {
    if (accordionRef.current) {
      accordionRef.current.animation.collapse = { effect: e.value };
      setCollapseEffect(e.value);
    }
  };

  return (
    <div>
      <div style={{ marginBottom: '20px' }}>
        <label>Expand Animation: </label>
        <DropDownListComponent 
          dataSource={effectOptions} 
          value={expandEffect} 
          change={handleExpandChange}
        />
      </div>

      <div style={{ marginBottom: '20px' }}>
        <label>Collapse Animation: </label>
        <DropDownListComponent 
          dataSource={effectOptions} 
          value={collapseEffect} 
          change={handleCollapseChange}
        />
      </div>

      <AccordionComponent ref={accordionRef} animation={{
        expand: { effect: expandEffect },
        collapse: { effect: collapseEffect }
      }}>
        <AccordionItemsDirective>
          <AccordionItemDirective header='Try it' content='Expand/collapse animations change above' />
          <AccordionItemDirective header='Test effects' content='Select different effects to see them in action' />
        </AccordionItemsDirective>
      </AccordionComponent>
    </div>
  );
}
```

## Disabling Animations

For performance-critical scenarios or minimal UI, disable animations entirely:

### Disable All Animations

```jsx
const noAnimation = {
  expand: { effect: 'None' },
  collapse: { effect: 'None' }
};

<AccordionComponent animation={noAnimation}>
  <AccordionItemsDirective>
    <AccordionItemDirective header='Instant Expand' content='No animation overhead' />
  </AccordionItemsDirective>
</AccordionComponent>
```

**Result:** Panels expand/collapse instantly without visual transition

### Disable via CSS

Alternatively, override animations with CSS:

```css
.e-accordion .e-expand {
  animation: none !important;
}

.e-accordion .e-collapse {
  animation: none !important;
}
```

## Performance Considerations

### Animation Duration Impact

- **Shorter durations** (100-300ms) - Snappier feel, better for frequent interactions
- **Standard duration** (300-500ms) - Good balance for most UIs
- **Longer durations** (500ms+) - Smooth, premium feel, may feel slow

### Effect Complexity

**Performant animations:**
```js
{ effect: 'SlideDown', duration: 300 }  // Hardware-accelerated
{ effect: 'FadeIn', duration: 300 }     // Simple opacity
```

**Complex animations (use sparingly):**
```js
{ effect: 'FadeZoomIn', duration: 800 }  // Combined effects
{ effect: 'ZoomIn', duration: 800 }      // Scale transformation
```

### Best Practices

1. **Keep durations under 500ms** for responsive feel
2. **Match animations to content load** - Don't animate while loading
3. **Consider mobile devices** - Disable animations on low-end devices
4. **Test on various hardware** - Smooth on desktop may lag on mobile
5. **Use `None` for large lists** - Many animated items can impact performance

### Mobile-Friendly Pattern

```jsx
const isSlowDevice = () => {
  // Simple device detection
  return navigator.deviceMemory < 4;
};

const animationSettings = isSlowDevice() 
  ? { expand: { effect: 'None' }, collapse: { effect: 'None' } }
  : { expand: { effect: 'SlideDown', duration: 300 }, collapse: { effect: 'SlideUp', duration: 300 } };

<AccordionComponent animation={animationSettings}>
  {/* accordion items */}
</AccordionComponent>
```

## Common Patterns

### Pattern 1: Professional/Minimal UI

Subtle animations for business applications:

```jsx
const minimalAnimation = {
  expand: { effect: 'FadeIn', duration: 250, easing: 'ease-out' },
  collapse: { effect: 'FadeOut', duration: 250, easing: 'ease-in' }
};
```

### Pattern 2: Modern/Engaging UI

Pronounced animations for contemporary applications:

```jsx
const modernAnimation = {
  expand: { effect: 'FadeZoomIn', duration: 400, easing: 'ease-out' },
  collapse: { effect: 'FadeZoomOut', duration: 400, easing: 'ease-in' }
};
```

### Pattern 3: Fast/Responsive UI

Quick animations for fast-paced interactions:

```jsx
const fastAnimation = {
  expand: { effect: 'SlideDown', duration: 150, easing: 'ease-out' },
  collapse: { effect: 'SlideUp', duration: 150, easing: 'ease-in' }
};
```

### Pattern 4: No Animation (Accessible/Performance)

Instant expand/collapse for accessibility and performance:

```jsx
const noAnimation = {
  expand: { effect: 'None' },
  collapse: { effect: 'None' }
};
```

---

## Troubleshooting

**Issue: Animations not working**
- Verify CSS imports are included (animation requires base styles)
- Check that `animation` prop is properly formatted
- Ensure effect names match exactly (case-sensitive)

**Issue: Animations are stuttering/laggy**
- Reduce animation duration (try 200-300ms)
- Use simpler effects like 'FadeIn' instead of 'FadeZoomIn'
- Check browser DevTools performance tab
- Test on different devices/browsers

**Issue: Animation property changes not applying**
- Use `ref` to access accordion instance directly
- Update `animation` prop on component
- Force re-render if needed

**Issue: Animations disabled but still animating**
- Verify effect is set to 'None', not disabled
- Check for CSS overrides forcing animations
- Clear browser cache to reload styles

**Issue: Duration not changing**
- Ensure duration is in milliseconds (not seconds)
- Verify prop update triggers component refresh
- Check that animation object is being passed correctly

