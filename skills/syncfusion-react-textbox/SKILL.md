---
name: syncfusion-react-textbox
description: Build single-line and multi-line text input fields using Syncfusion React TextBox. Trigger when user needs text inputs with floating labels, validation states, adornments (icons/buttons via prependTemplate/appendTemplate), sizing options, rounded corners (e-corner), disabled/read-only states, RTL support, or custom styling. Use for forms, user profiles, search bars, comments, addresses, or any text data collection in React applications.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
  category: "Inputs"
---

# Implementing Syncfusion React TextBox Component

The TextBox component is a lightweight input control that captures user text input with support for floating labels, validation states, icons, and advanced features. This skill guides you through implementing, configuring, and customizing the TextBox component in React applications.

## Navigation Guide

> 🛑 **Agentic use:** Do not execute multiple steps autonomously. Confirm with the user before each action (install, run, file creation).

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- Vite setup for React development
- Installing `@syncfusion/ej2-react-inputs` package 🛑 *STOP — Do not install packages autonomously. Ask the user to run: `npm install @syncfusion/ej2-react-inputs`. Pin a specific version (e.g., `@syncfusion/ej2-react-inputs@28.x.x`) and verify with `npm audit`*
- Adding CSS imports and themes
- Creating your first TextBox component
- Adding icons and floating labels
- Running the development server 🛑 *STOP — Do not start the dev server autonomously. Ask the user to run: `npm run dev`*

### Features and Groups
📄 **Read:** [references/features-and-groups.md](references/features-and-groups.md)
- Floating label behavior (Never, Always, Auto)
- Icons with `addIcon()` method (prepend/append)
- Clear button with `showClearButton` property
- Rounded corner with `e-corner` CSS class
- Disabled state with `enabled={false}`
- Multi-line textbox creation
- TextBox with clear button and floating label combinations

### Styling and Sizing
📄 **Read:** [references/styling-and-sizing.md](references/styling-and-sizing.md)
- Three predefined sizes: Normal, Small (`e-small`), Large (`e-bigger`)
- Applying size classes via `cssClass` property
- Rounded corner with `e-corner` CSS class
- CSS customization for TextBox wrapper and floating label
- Custom CSS classes and themes
- Responsive design patterns

### Multiline TextBox
📄 **Read:** [references/multiline-textbox.md](references/multiline-textbox.md)
- Creating multiline/textarea inputs with `multiline={true}`
- Floating labels with multiline
- Auto-resizing textboxes
- Disabling resize functionality
- Limiting text length with `htmlAttributes={{ maxlength: '...' }}`
- Character counting and display

### Validation and States
📄 **Read:** [references/validation-and-states.md](references/validation-and-states.md)
- Error, warning, and success validation states via `cssClass`
- Applying validation classes (`e-error`, `e-warning`, `e-success`)
- Disabled state with `enabled={false}` (not `disabled`)
- Read-only state with `readonly={true}`
- Differences between disabled and read-only
- Dynamic color changes based on values using `input` event

### Advanced Features
📄 **Read:** [references/advanced-features.md](references/advanced-features.md)
- Adornments: `prependTemplate` and `appendTemplate` properties
- Interactive adornments (password toggle, delete button)
- React functional components with hooks
- `useState`, `useEffect`, `useRef`, `useReducer` integration
- Event handling (created, input, change events)
- Form validation patterns

### Accessibility and Migration
📄 **Read:** [references/accessibility-and-migration.md](references/accessibility-and-migration.md)
- WCAG 2.2, Section 508, and WAI-ARIA compliance
- Screen reader support and ARIA attributes
- Right-to-Left (RTL) support with `enableRtl` property
- Keyboard navigation support
- Migrating from CSS TextBox to React component
- Before/after code comparison

### API Reference
📄 **Read:** [references/api.md](references/api.md)
- All properties: `placeholder`, `floatLabelType`, `value`, `type`, `cssClass`, `multiline`, `showClearButton`, `enabled`, `readonly`, `enableRtl`, `enablePersistence`, `autocomplete`, `htmlAttributes`, `locale`, `width`, `prependTemplate`, `appendTemplate`
- Methods: `addIcon`, `addAttributes`, `removeAttributes`, `focusIn`, `focusOut`, `destroy`, `getPersistData`
- Events: `created`, `destroyed`, `change`, `input`, `focus`, `blur`

## Quick Start

### Basic TextBox with Floating Label

```tsx
import { TextBoxComponent } from '@syncfusion/ej2-react-inputs';
import './App.css';

export default function App() {
  return (
    <TextBoxComponent 
      placeholder="Enter your name" 
      floatLabelType="Auto"
    />
  );
}
```

### TextBox with Icon

```tsx
import { TextBoxComponent } from '@syncfusion/ej2-react-inputs';
import { useRef } from 'react';

export default function App() {
  const textboxRef = useRef(null);

  const handleCreate = () => {
    if (textboxRef.current) {
      textboxRef.current.addIcon('append', 'e-icons e-input-popup-date');
    }
  };

  return (
    <TextBoxComponent
      placeholder="Enter date"
      floatLabelType="Auto"
      ref={textboxRef}
      created={handleCreate}
    />
  );
}
```

### TextBox with Clear Button

```tsx
import { TextBoxComponent } from '@syncfusion/ej2-react-inputs';

export default function App() {
  return (
    <TextBoxComponent
      placeholder="Enter your email"
      floatLabelType="Auto"
      showClearButton={true}
    />
  );
}
```

### Multiline TextBox

```tsx
import { TextBoxComponent } from '@syncfusion/ej2-react-inputs';

export default function App() {
  return (
    <TextBoxComponent
      multiline={true}
      placeholder="Enter your address"
      floatLabelType="Auto"
    />
  );
}
```

## Common Patterns

### Form with Validation States

```tsx
import { TextBoxComponent } from '@syncfusion/ej2-react-inputs';
import { useState } from 'react';

export default function ValidationForm() {
  const [cssClass, setCssClass] = useState('');

  return (
    <div>
      <TextBoxComponent
        placeholder="Enter username"
        cssClass={cssClass}
        floatLabelType="Auto"
        input={(e: any) => {
          if (!e.value) setCssClass('');
          else if (e.value.length < 3) setCssClass('e-error');
          else if (e.value.length < 6) setCssClass('e-warning');
          else setCssClass('e-success');
        }}
      />
    </div>
  );
}
```

### Password TextBox with Toggle

```tsx
import * as React from 'react';
import { TextBoxComponent } from '@syncfusion/ej2-react-inputs';
import { useRef, useState } from 'react';

export default function PasswordInput() {
  const textboxRef = useRef<TextBoxComponent>(null);
  const [isVisible, setIsVisible] = useState(false);

  const toggleVisibility = () => {
    if (textboxRef.current) {
      const newVisibility = !isVisible;
      textboxRef.current.type = newVisibility ? 'text' : 'password';
      setIsVisible(newVisibility);
    }
  };

  function appendTemplate(): JSX.Element {
    return (
      <>
        <span className="e-input-separator"></span>
        <span
          className={`e-icons ${isVisible ? 'e-eye-slash' : 'e-eye'}`}
          onClick={toggleVisibility}
          style={{ cursor: 'pointer' }}
        ></span>
      </>
    );
  }

  return (
    <TextBoxComponent
      ref={textboxRef}
      type="password"
      placeholder="Enter password"
      floatLabelType="Auto"
      appendTemplate={appendTemplate}
    />
  );
}
```

### Email Input with Unit Label

```tsx
import * as React from 'react';
import { TextBoxComponent } from '@syncfusion/ej2-react-inputs';

export default function EmailInput() {
  function prependTemplate(): JSX.Element {
    return (
      <>
        <span className="e-icons e-user"></span>
        <span className="e-input-separator"></span>
      </>
    );
  }

  function appendTemplate(): JSX.Element {
    return (
      <>
        <span className="e-input-separator"></span>
        <span>.com</span>
      </>
    );
  }

  return (
    <TextBoxComponent
      type="email"
      placeholder="Enter email"
      floatLabelType="Auto"
      prependTemplate={prependTemplate}
      appendTemplate={appendTemplate}
    />
  );
}
```

### Rounded Corner TextBox

```tsx
import { TextBoxComponent } from '@syncfusion/ej2-react-inputs';

export default function RoundedCornerTextBox() {
  return (
    <TextBoxComponent
      placeholder="Enter Date"
      cssClass="e-corner"
    />
  );
}
```

### Disabled TextBox

```tsx
import { TextBoxComponent } from '@syncfusion/ej2-react-inputs';

export default function DisabledTextBox() {
  return (
    <TextBoxComponent
      placeholder="Enter Name"
      enabled={false}
    />
  );
}
```

### RTL TextBox

```tsx
import { TextBoxComponent } from '@syncfusion/ej2-react-inputs';

export default function RTLTextBox() {
  return (
    <TextBoxComponent
      placeholder="أدخل اسمك"
      floatLabelType="Auto"
      enableRtl={true}
    />
  );
}
```

### Auto-sizing Multiline TextBox

```tsx
import { TextBoxComponent } from '@syncfusion/ej2-react-inputs';
import { useRef } from 'react';

export default function AutoSizeTextbox() {
  const textboxRef = useRef(null);

  const handleInput = () => {
    if (textboxRef.current) {
      const elem = textboxRef.current.respectiveElement;
      elem.style.height = 'auto';
      elem.style.height = elem.scrollHeight + 'px';
    }
  };

  const handleCreate = () => {
    if (textboxRef.current) {
      textboxRef.current.addAttributes({ rows: 1 });
    }
    handleInput();
  };

  return (
    <TextBoxComponent
      multiline={true}
      placeholder="Enter your message"
      floatLabelType="Auto"
      ref={textboxRef}
      created={handleCreate}
      input={handleInput}
    />
  );
}
```

## Key Properties

| Property | Type | Purpose |
|----------|------|---------|
| `placeholder` | `string` | Hint text shown when input is empty |
| `floatLabelType` | `"Never" \| "Always" \| "Auto"` | Label animation behavior |
| `value` | `string` | Sets the content of the TextBox |
| `type` | `string` | Input type (`text`, `password`, `email`, `number`, etc.) |
| `multiline` | `boolean` | Convert to textarea for multi-line input |
| `showClearButton` | `boolean` | Display clear button when input has value |
| `cssClass` | `string` | Apply CSS classes for sizing/validation/appearance (e.g., `"e-error"`, `"e-small"`, `"e-corner"`) |
| `enabled` | `boolean` | Enable (`true`) or disable (`false`) input interaction |
| `readonly` | `boolean` | Allow selection but prevent editing |
| `enableRtl` | `boolean` | Enable right-to-left rendering |
| `enablePersistence` | `boolean` | Persist value state between page reloads ⚠️ *Stores data in browser storage — enable only with explicit user consent* |
| `autocomplete` | `string` | Control browser autocomplete (`"on"` \| `"off"`) |
| `htmlAttributes` | `{ [key: string]: string }` | Pass additional HTML attributes (e.g., `{ maxlength: '200' }`) |
| `locale` | `string` | Override global culture/localization value |
| `width` | `number \| string` | Set component width |
| `prependTemplate` | `() => JSX.Element` | Render element before input |
| `appendTemplate` | `() => JSX.Element` | Render element after input |

## Key Events

| Event | Arguments | Purpose |
|-------|-----------|---------|
| `created` | `Object` | Fires after component initialization |
| `destroyed` | `Object` | Fires when component is destroyed |
| `change` | `ChangedEventArgs` | Fires when value changes on focus-out |
| `input` | `InputEventArgs` | Fires on every keystroke |
| `focus` | `FocusInEventArgs` | Fires when TextBox gains focus |
| `blur` | `FocusOutEventArgs` | Fires when TextBox loses focus |

## Related Documentation

> **ℹ️ External links below are for manual reference only.** Do not auto-fetch these URLs in an agentic pipeline without explicit user consent.

- [Syncfusion React TextBox Component Demo](https://ej2.syncfusion.com/react/demos/#/tailwind3/textboxes/default) *(external)*
- [React TextBox API Reference](https://ej2.syncfusion.com/react/documentation/api/textbox/) *(external)*
- [Syncfusion React Inputs Package](https://www.npmjs.com/package/@syncfusion/ej2-react-inputs) *(external — verify before installing)*
- [React Functional Components](./references/advanced-features.md)
