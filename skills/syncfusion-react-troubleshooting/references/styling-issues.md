# Overview

This document provides common theme-related troubleshooting scenarios in Syncfusion React components, including built-in themes, dark mode, and CSS variable customization. Each issue includes a clear resolution with short explanations to help fix styling and theme problems quickly.

## Table of Contents

- [Theme not applied](#theme-not-applied)
- [Styles missing or broken](#styles-missing-or-broken)
- [Wrong theme loaded](#wrong-theme-loaded)
- [CDN theme mismatch](#cdn-theme-mismatch)
- [Dark mode not working](#dark-mode-not-working)
- [Dark mode not toggling dynamically](#dark-mode-not-toggling-dynamically)
- [Component-specific dark mode not applied](#component-specific-dark-mode-not-applied)
- [CSS variables not working](#css-variables-not-working)
- [Incorrect color rendering in Material 3](#incorrect-color-rendering-in-material-3)
- [Custom theme override not applied](#custom-theme-override-not-applied)
- [Multiple theme imports conflict](#multiple-theme-imports-conflict)
- [Dependency styles missing](#dependency-styles-missing)
- [Layout/UI misalignment due to theme](#layoutui-misalignment-due-to-theme)
- [Theme not applied](#theme-not-applied)
- [Icon not displaying](#icon-not-displaying)
- [Dark mode or size mode not working](#dark-mode-not-working)
- [CSS customization not applied](#css-customization-not-applied)
- [Inconsistent UI or style conflicts](#inconsistent-ui-or-style-conflicts)

---

## Theme not applied

**Resolution:** Ensure the correct theme CSS file is imported in your project (e.g., `tailwind3.css` or `material3.css`).

If CSS is missing, components render without styling because themes provide all UI appearance definitions.

---

## Styles missing or broken

**Resolution:** Import base styles and required component styles in the correct order starting with `ej2-base`.

Missing dependencies cause incomplete styling and broken layouts across components.

---

## Wrong theme loaded

**Resolution:** Verify the imported theme matches the expected design system (Material, Fluent, Bootstrap).

Using the wrong theme file may lead to unexpected UI appearance and inconsistent styling.

---

## CDN theme mismatch

**Resolution:** Ensure the CDN version matches the installed Syncfusion npm package version.

Version mismatch causes rendering issues because CSS and component scripts become incompatible.

---

## Dark mode not working

**Resolution:** Apply the `e-dark-mode` class to body or container to enable dark theme.

Without this class, dark styles will not be activated even if the theme supports dark mode.

---

## Dark mode not toggling dynamically

**Resolution:** Update the DOM class (`e-dark-mode`) inside React state changes or `useEffect`.

Failing to update DOM state prevents real-time switching between light and dark modes.

---

## Component-specific dark mode not applied

**Resolution:** Wrap the specific component container with the `e-dark-mode` class.

Dark mode applies only to elements inside the class scope, not globally unless configured.

---

## CSS variables not working

**Resolution:** Use correct syntax such as `var(--variable)` and ensure variables are defined at root or container level.

Undefined or incorrectly scoped variables will not be applied in components.

---

## Incorrect color rendering in Material 3

**Resolution:** Use RGB format values for Material 3 variables instead of hex values.

Material 3 strictly requires RGB format and improper values result in incorrect colors.

---

## Custom theme override not applied

**Resolution:** Ensure custom CSS variables are loaded after the theme file and scoped correctly.

Overrides fail if defined before theme import or outside the applied container.

---

## Multiple theme imports conflict

**Resolution:** Import only one theme file at a time to avoid conflicting styles.

Multiple theme files override each other and cause unpredictable UI rendering.

---

## Dependency styles missing

**Resolution:** Import all required dependency styles for complex components like Grid.

Components rely on multiple internal modules and missing styles break their layout.

---

## Layout/UI misalignment due to theme

**Resolution:** Ensure consistent theme usage and do not mix different framework styles (e.g., Bootstrap + Tailwind).

Mixing styles leads to CSS conflicts and improper UI alignment across components.

---

## Icon not displaying

**Resolution:** Install and import the `@syncfusion/ej2-icons` package and include the correct icon CSS file.

Icons require both `e-icons` base class and specific icon class, otherwise they will not render.

---

## Dark mode or size mode not working

**Resolution:** Apply required utility classes like `e-dark-mode` or `e-bigger` to the body or container dynamically.

Without these classes, dark theme or touch size mode will not activate even if supported by the theme.

---

## CSS customization not applied

**Resolution:** Ensure custom styles target correct Syncfusion classes like `.e-btn`, `.e-grid` and are loaded after theme CSS.

Incorrect selectors or load order prevents overrides from applying to the components.

---

## Inconsistent UI or style conflicts

**Resolution:** Avoid importing multiple themes or mixing frameworks like Bootstrap and Tailwind together.

Conflicting styles override each other and lead to broken layouts and unpredictable UI behavior.