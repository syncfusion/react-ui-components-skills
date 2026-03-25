# Accessibility, Styling & Localization

## Table of Contents
- [Accessibility Compliance](#accessibility-compliance)
- [WAI-ARIA Attributes](#wai-aria-attributes)
- [Keyboard Navigation](#keyboard-navigation)
- [CSS Customization](#css-customization)
  - [Wrapper Element](#wrapper-element)
  - [Dropdown Icon](#dropdown-icon)
  - [Popup List Container](#popup-list-container)
  - [List Items](#list-items)
  - [Placeholder Text](#placeholder-text)
- [Theming](#theming)
- [Localization](#localization)
- [RTL Layout](#rtl-layout)
- [Gotchas](#gotchas)

---

## Accessibility Compliance

The DropDownList meets the following standards:

| Criteria | Support |
|---|---|
| WCAG 2.2 | ✅ Full |
| Section 508 | ✅ Full |
| Screen Reader | ✅ Full |
| RTL Support | ✅ Full |
| Color Contrast | ✅ Full |
| Mobile Device | ✅ Full |
| Keyboard Navigation | ✅ Full |
| Accessibility Checker | ✅ Full |
| Axe-core Validation | ✅ Full |

---

## WAI-ARIA Attributes

The DropDownList follows the [WAI-ARIA combobox pattern](https://www.w3.org/WAI/ARIA/apg/patterns/combobox/):

| Attribute | Purpose |
|---|---|
| `aria-haspopup` | Indicates the input has an associated popup list |
| `aria-expanded` | Reflects whether the popup is open |
| `aria-selected` | Marks the currently selected option |
| `aria-readonly` | Reflects the readonly state |
| `aria-disabled` | Reflects the disabled state |
| `aria-activedescendent` | Points to the active item inside the popup for screen readers |
| `aria-owns` | Associates the popup list as a child of the input |

These attributes are managed automatically — no manual configuration needed.

---

## Keyboard Navigation

| Key | Action |
|---|---|
| `Arrow Down` | Selects the first item (if none selected), or the next item |
| `Arrow Up` | Selects the previous item |
| `Page Down` | Scrolls down one page and selects the first item |
| `Page Up` | Scrolls up one page and selects the first item |
| `Enter` | Confirms selection and closes popup; or opens popup if closed |
| `Tab` | Closes popup and moves focus to the next tab stop |
| `Shift + Tab` | Closes popup and moves focus to the previous tab stop |
| `Alt + Down` | Opens the popup |
| `Alt + Up` | Closes the popup |
| `Escape` | Closes the popup, retaining the current selection |
| `Home` | Selects the first item |
| `End` | Selects the last item |

---

## CSS Customization

Target Syncfusion CSS class selectors to override default styles. Always scope to `.e-ddl` to avoid affecting other components.

### Wrapper Element

```css
.e-ddl.e-input-group.e-control-wrapper .e-input {
  font-size: 16px;
  font-family: 'Inter', sans-serif;
  color: #333;
  background: #f9f9f9;
}
```

### Dropdown Icon

```css
/* Change the dropdown arrow icon color */
.e-ddl .e-input-group-icon.e-ddl-icon::before {
  color: #6200ea;
}
```

### Popup List Container

```css
/* Style the popup container */
.e-dropdownbase .e-list-parent.e-ul {
  background-color: #ffffff;
  border: 1px solid #e0e0e0;
}
```

### List Items

```css
/* Hover state */
.e-dropdownbase .e-list-item.e-hover {
  background-color: #f3e5f5;
  color: #6200ea;
}

/* Selected state */
.e-dropdownbase .e-list-item.e-active {
  background-color: #6200ea;
  color: #ffffff;
}
```

### Placeholder Text

```css
.e-ddl.e-input-group input::placeholder {
  color: #9e9e9e;
  font-style: italic;
}
```

---

## Theming

Syncfusion provides built-in themes. Switch by changing the CSS import in `App.css`:

```css
/* Tailwind 3 (current default — recommended) */
@import "../node_modules/@syncfusion/ej2-react-dropdowns/styles/tailwind3.css";

/* Material 3 */
@import "../node_modules/@syncfusion/ej2-react-dropdowns/styles/material3.css";

/* Bootstrap 5 */
@import "../node_modules/@syncfusion/ej2-react-dropdowns/styles/bootstrap5.css";

/* Fluent 2 */
@import "../node_modules/@syncfusion/ej2-react-dropdowns/styles/fluent2.css";
```

> Always import the base and inputs CSS alongside the dropdowns CSS:
> ```css
> @import "../node_modules/@syncfusion/ej2-base/styles/tailwind3.css";
> @import "../node_modules/@syncfusion/ej2-inputs/styles/tailwind3.css";
> @import "../node_modules/@syncfusion/ej2-react-dropdowns/styles/tailwind3.css";
> ```

For a custom theme, use [Syncfusion Theme Studio](https://ej2.syncfusion.com/themestudio/).

---

## Localization

Override the default English text for no-records and request failure messages using `L10n`:

| Locale key | Default (en-US) |
|---|---|
| `noRecordsTemplate` | `No records found` |
| `actionFailureTemplate` | `The request failed` |

```tsx
import { L10n } from '@syncfusion/ej2-base';
import { DropDownListComponent } from '@syncfusion/ej2-react-dropdowns';

// Load French translations before rendering
L10n.load({
  'fr-BE': {
    'dropdowns': {
      noRecordsTemplate: 'Aucun enregistrement trouvé',
      actionFailureTemplate: 'La requête a échoué',
    }
  }
});

export default function App() {
  return (
    <DropDownListComponent
      dataSource={[]}
      locale="fr-BE"
      placeholder="Sélectionnez un élément"
    />
  );
}
```

> `L10n.load()` must be called before the component renders. Do this at app initialization level.

---

## RTL Layout

```tsx
<DropDownListComponent
  dataSource={sportsData}
  enableRtl={true}
  placeholder="Select a game"
/>
```

RTL flips the popup and icon direction for right-to-left languages (Arabic, Hebrew, Urdu, etc.). Combine with `locale` for full internationalization.

---

## Gotchas

- **CSS specificity** — Syncfusion styles use specific selectors. If overrides don't work, increase specificity or use `!important` sparingly.
- **Theme consistency** — all three CSS imports (`ej2-base`, `ej2-inputs`, `ej2-react-dropdowns`) must use the same theme name. Mixing themes causes visual glitches.
- **`L10n.load()` timing** — must run before the component first renders; placing it inside a component's render function causes a race condition.
- **Screen reader testing** — ARIA attributes are managed automatically but verify with an actual screen reader (NVDA, VoiceOver) in your specific browser environment.

## See Also

- [Getting Started](getting-started.md) — CSS import setup
- [Grouping & Templates](grouping-and-templates.md) — `noRecordsTemplate` and `actionFailureTemplate` as JSX
- [Features & Configuration](features-and-configuration.md) — `enableRtl`, `readonly`, `enabled`
