# Getting Started with Toolbar

## Table of Contents
- [Dependencies](#dependencies)
- [Project Setup](#project-setup)
- [Adding Syncfusion Packages](#adding-syncfusion-packages)
- [CSS Configuration](#css-configuration)
- [Basic Implementation](#basic-implementation)
- [HTML Element Initialization](#html-element-initialization)
- [Global Configuration](#global-configuration)
  - [Locale Support](#locale-support)
  - [Right-to-Left (RTL) Support](#right-to-left-rtl-support)
  - [Enable Persistence](#enable-persistence)
  - [Enable Collision Detection](#enable-collision-detection)
  - [Enable HTML Sanitizer](#enable-html-sanitizer)
- [Running Your Application](#running-your-application)

---

## Dependencies

The Toolbar component requires the following minimum dependencies:

```
@syncfusion/ej2-react-navigations (main package)
├── @syncfusion/ej2-base
├── @syncfusion/ej2-react-base
└── @syncfusion/ej2-navigations
    ├── @syncfusion/ej2-buttons
    └── @syncfusion/ej2-popups
```

These packages handle the core functionality, button rendering, and popup components.

---

## Project Setup

### Using Vite (Recommended)

Vite provides faster development builds and optimized production output. Create a new React application:

```bash
npm create vite@latest my-toolbar-app -- --template react
cd my-toolbar-app
npm install
```

For TypeScript support:

```bash
npm create vite@latest my-toolbar-app -- --template react-ts
cd my-toolbar-app
npm install
```

### Using Create React App

If you prefer Create React App:

```bash
npx create-react-app my-toolbar-app
cd my-toolbar-app
npm start
```

---

## Adding Syncfusion Packages

Install the Toolbar component package:

```bash
npm install @syncfusion/ej2-react-navigations --save
```

This automatically installs all peer dependencies including buttons and popups.

---

## CSS Configuration

### Step 1: Import Theme Styles

Add the following CSS imports to your `src/App.css` file:

```css
@import '../node_modules/@syncfusion/ej2-base/styles/tailwind3.css';
@import '../node_modules/@syncfusion/ej2-buttons/styles/tailwind3.css';
@import '../node_modules/@syncfusion/ej2-popups/styles/tailwind3.css';
@import '../node_modules/@syncfusion/ej2-react-navigations/styles/tailwind3.css';
```

**Theme Options:**
- `tailwind3.css` - Tailwind theme (recommended)
- `bootstrap5.3.css` - Bootstrap theme
- `fluent2.css` - Microsoft Fluent theme
- `material3.css` - Material Design theme

### Step 2: Import CSS in Application

In your `src/App.tsx` or `src/App.jsx`:

```jsx
import './App.css';
```

---

## Basic Implementation

### Simple Button Toolbar

Create a toolbar with basic button items:

```jsx
import { ItemDirective, ItemsDirective, ToolbarComponent } from '@syncfusion/ej2-react-navigations';
import './App.css';

const App = () => {
  return (
    <ToolbarComponent id='toolbar'>
      <ItemsDirective>
        <ItemDirective text="Cut" />
        <ItemDirective text="Copy" />
        <ItemDirective text="Paste" />
        <ItemDirective type="Separator" />
        <ItemDirective text="Bold" />
        <ItemDirective text="Italic" />
        <ItemDirective text="Underline" />
      </ItemsDirective>
    </ToolbarComponent>
  );
};

export default App;
```

**What happens:**
- `ToolbarComponent` creates the toolbar container
- `ItemsDirective` wraps all toolbar items
- `ItemDirective` defines individual items
- Default item type is `Button`
- `type="Separator"` creates a visual divider

### Toolbar with Icons

Add icons to buttons using `prefixIcon`:

```jsx
<ToolbarComponent id='toolbar'>
  <ItemsDirective>
    <ItemDirective text="Cut" prefixIcon="e-cut-icon" />
    <ItemDirective text="Copy" prefixIcon="e-copy-icon" />
    <ItemDirective text="Paste" prefixIcon="e-paste-icon" />
    <ItemDirective type="Separator" />
    <ItemDirective text="Bold" prefixIcon="e-bold-icon" />
    <ItemDirective text="Italic" prefixIcon="e-italic-icon" />
    <ItemDirective text="Underline" prefixIcon="e-underline-icon" />
  </ItemsDirective>
</ToolbarComponent>
```

The icon is positioned before the text. If no text is provided, only the icon displays.

### Minimal Working Example

Minimal setup with required imports:

```jsx
import { ToolbarComponent, ItemsDirective, ItemDirective } from '@syncfusion/ej2-react-navigations';
import './App.css';

export default function App() {
  return (
    <ToolbarComponent>
      <ItemsDirective>
        <ItemDirective text="File" />
        <ItemDirective text="Edit" />
        <ItemDirective text="View" />
      </ItemsDirective>
    </ToolbarComponent>
  );
}
```

This is the absolute minimum to render a working toolbar.

---

## HTML Element Initialization

The Toolbar can also be rendered from HTML structure:

### HTML Structure

```html
<div id="root"></div>
```

### React Component

```jsx
import { ToolbarComponent } from '@syncfusion/ej2-react-navigations';
import './App.css';

const App = () => {
  return (
    <ToolbarComponent>
      <div>
        <div><button className='e-btn e-tbar-btn'>Cut</button></div>
        <div><button className='e-btn e-tbar-btn'>Copy</button></div>
        <div><button className='e-btn e-tbar-btn'>Paste</button></div>
        <div className='e-separator'></div>
        <div><button className='e-btn e-tbar-btn'>Bold</button></div>
        <div><button className='e-btn e-tbar-btn'>Italic</button></div>
      </div>
    </ToolbarComponent>
  );
};

export default App;
```

**Structure:**
- `ToolbarComponent` - Root toolbar element
- Direct `<div>` children - Items container
- Nested `<div>` elements - Individual items
- `e-btn e-tbar-btn` classes - Button styling
- `e-separator` class - Separator styling

---

## Global Configuration

### Locale Support

Support multiple languages by setting the `locale` property:

```jsx
import { ItemDirective, ItemsDirective, ToolbarComponent } from '@syncfusion/ej2-react-navigations';

const App = () => {
  return (
    <ToolbarComponent locale="de-DE">
      <ItemsDirective>
        <ItemDirective text="Speichern" prefixIcon="e-save-icon" />
        <ItemDirective text="Drucken" prefixIcon="e-print-icon" />
      </ItemsDirective>
    </ToolbarComponent>
  );
};

export default App;
```

**Common locale codes:**
- `"en-US"` - English (United States)
- `"en-GB"` - English (Great Britain)
- `"de-DE"` - German
- `"fr-FR"` - French
- `"es-ES"` - Spanish
- `"ja-JP"` - Japanese
- `"ar-AE"` - Arabic
- `"zh-CN"` - Chinese (Simplified)

### Right-to-Left (RTL) Support

Enable RTL layout for languages that read right-to-left:

```jsx
<div dir="rtl">
  <ToolbarComponent enableRtl={true}>
    <ItemsDirective>
      <ItemDirective text="حفظ" prefixIcon="e-save-icon" /> {/* Arabic: Save */}
      <ItemDirective text="طباعة" prefixIcon="e-print-icon" /> {/* Arabic: Print */}
    </ItemsDirective>
  </ToolbarComponent>
</div>
```

**Features with RTL:**
- All items automatically reverse order
- Text aligns right to left
- Icons maintain position but layout reverses
- Scrollable arrows reverse direction
- Popup alignment adjusts

### RTL with Locale

Combine RTL and locale for complete internationalization:

```jsx
<div dir="rtl">
  <ToolbarComponent enableRtl={true} locale="ar-AE">
    <ItemsDirective>
      {/* Arabic toolbar with RTL layout */}
      <ItemDirective text="جديد" prefixIcon="e-new-icon" />
      <ItemDirective text="فتح" prefixIcon="e-open-icon" />
      <ItemDirective text="حفظ" prefixIcon="e-save-icon" />
    </ItemsDirective>
  </ToolbarComponent>
</div>
```

### Dynamic Locale Change

Update locale at runtime:

```jsx
import { useState } from 'react';
import { ItemDirective, ItemsDirective, ToolbarComponent } from '@syncfusion/ej2-react-navigations';

const App = () => {
  const [locale, setLocale] = useState('en-US');

  const changeLocale = (newLocale) => {
    setLocale(newLocale);
  };

  return (
    <>
      <div style={{ marginBottom: '16px' }}>
        <button onClick={() => changeLocale('en-US')}>English</button>
        <button onClick={() => changeLocale('de-DE')}>Deutsch</button>
        <button onClick={() => changeLocale('fr-FR')}>Français</button>
      </div>

      <ToolbarComponent locale={locale}>
        <ItemsDirective>
          <ItemDirective 
            text={locale === 'de-DE' ? 'Speichern' : locale === 'fr-FR' ? 'Enregistrer' : 'Save'} 
            prefixIcon="e-save-icon" 
          />
          <ItemDirective 
            text={locale === 'de-DE' ? 'Drucken' : locale === 'fr-FR' ? 'Imprimer' : 'Print'} 
            prefixIcon="e-print-icon" 
          />
        </ItemsDirective>
      </ToolbarComponent>
    </>
  );
};

export default App;
```

### Enable Persistence

Save toolbar state (active items, visibility) between sessions:

```jsx
<ToolbarComponent enablePersistence={true}>
  <ItemsDirective>
    {/* Toolbar state is saved and restored */}
  </ItemsDirective>
</ToolbarComponent>
```

### Enable Collision Detection

Automatically prevent UI elements from colliding:

```jsx
<ToolbarComponent 
  enableCollision={true}
  overflowMode="Popup"
>
  <ItemsDirective>
    {/* Items automatically adjust to prevent overlap */}
  </ItemsDirective>
</ToolbarComponent>
```

### Enable HTML Sanitizer

Sanitize HTML content to prevent XSS attacks:

```jsx
<ToolbarComponent enableHtmlSanitizer={true}>
  <ItemsDirective>
    {/* HTML content is sanitized */}
  </ItemsDirective>
</ToolbarComponent>
```

---

## Running Your Application

### For Vite Projects

```bash
npm run dev
```

Your application will start at `http://localhost:5173`

### For Create React App

```bash
npm start
```

Your application will start at `http://localhost:3000`

### Development Server

The dev server provides:
- Hot module replacement (HMR) - instant updates when you save
- Source map support for debugging
- Error overlay for build errors
- Automatic browser refresh

---

## Troubleshooting

### Styles Not Applied
- Ensure CSS imports are in `App.css` before using the component
- Check that the correct theme file is imported
- Verify the path is correct relative to node_modules

### Component Not Rendering
- Check that `@syncfusion/ej2-react-navigations` is installed
- Verify imports: `ToolbarComponent`, `ItemsDirective`, `ItemDirective`
- Ensure `ItemsDirective` wraps all items

### Icons Not Displaying
- Verify icon class names (e.g., `e-cut-icon`, `e-copy-icon`)
- Check that font dependencies are loaded with CSS
- Use prefixIcon or suffixIcon properties

### Port Already in Use
For Vite:
```bash
npm run dev -- --port 3000
```

For Create React App:
```bash
PORT=3000 npm start
```

---

## Next Steps

Once you have a basic toolbar running:
1. **Add different item types** → See [item-configuration.md](item-configuration.md)
2. **Handle responsive overflow** → See [responsive-modes.md](responsive-modes.md)
3. **Add interactions** → See [advanced-features.md](advanced-features.md)
4. **Improve accessibility** → See [accessibility.md](accessibility.md)
