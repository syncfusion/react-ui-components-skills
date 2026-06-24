---
name: syncfusion-react-troubleshooting
description: Resolve common issues in Syncfusion React components and general React applications. Use this skill when debugging installation issues, styling problems, rendering errors, licensing issues, globalization problems, state persistence failures, or runtime bugs in React apps.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
  category: "Troubleshooting"
---

## Troubleshooting Guide for Syncfusion React Components

This skill helps identify and resolve issues encountered while working with Syncfusion React components and React applications. It provides structured debugging steps, common problems, and solutions.

---

## 📚 Navigation Guide

### 🔧 Installation & Setup Issues
📄 **Read:** [references/installation-issues.md](references/installation-issues.md)  
Use when:
- Components are not rendering
- Styles are missing or broken
- Package installation errors occur

---

### 🎨 Styling & Theme Issues
📄 **Read:** [references/styling-issues.md](references/styling-issues.md)  
Use when:
- UI looks unstyled
- Theme or CSS is not applied
- Tailwind/Bootstrap conflicts occur

---

### 🔑 Licensing Issues
📄 **Read:** [references/license-issues.md](references/license-issues.md)  
Use when:
- License warnings appear
- Components show trial message

---

## 🚀 Quick Troubleshooting Checklist

Before deep debugging, verify:

- ✅ Packages installed correctly
- ✅ CSS imported properly
- ✅ License activated
- ✅ Correct component imports
- ✅ No console errors
- ✅ React hooks used properly
- ✅ Environment variables configured

---

# Common React Issues in Syncfusion Context

## 1. State not updating

**Cause:** Incorrect use of React hooks  

**Resolution:** Use `useState` and `setState` properly

---

## 2. Component re-render issues

**Cause:** Unnecessary re-renders or improper state usage  

**Resolution:** Use memoization and avoid inline functions

---

## 3. SSR (Next.js) rendering issues

**Cause:** Components rendered on server  

**Resolution:** Initialize Syncfusion components inside `useEffect`

## Installation & Setup Issues

### Issue: Component not found or module error

**Symptoms:**
- `Module not found`
- Import errors

**Solution:**
```bash
npm install @syncfusion/ej2-react-grids --save
```

**Verify import:**

import { GridComponent } from '@syncfusion/ej2-react-grids';

### Rendering Issues

### Issue: Component renders empty or blank

**Common Causes:**

- Missing dataSource
- Incorrect JSX structure
- Missing required directives (e.g., ColumnsDirective)

**Fix**
```jsx
<GridComponent dataSource={data}>
  <ColumnsDirective>
    <ColumnDirective field="OrderID" />
  </ColumnsDirective>
</GridComponent>
```

### Issue: Component not appearing at all

**Checklist:**

- Is the parent container rendered?
- Is component inside return statement?
- Any conditional rendering preventing display?

## Styling & Theme Issues

### Issue: No styles applied (unstyled component)

**Cause:**
CSS files not imported

```css
@import '../node_modules/@syncfusion/ej2-base/styles/tailwind3.css';  
@import '../node_modules/@syncfusion/ej2-buttons/styles/tailwind3.css';  
@import '../node_modules/@syncfusion/ej2-calendars/styles/tailwind3.css';  
@import '../node_modules/@syncfusion/ej2-dropdowns/styles/tailwind3.css';  
@import '../node_modules/@syncfusion/ej2-inputs/styles/tailwind3.css';  
@import '../node_modules/@syncfusion/ej2-navigations/styles/tailwind3.css';
@import '../node_modules/@syncfusion/ej2-popups/styles/tailwind3.css';
@import '../node_modules/@syncfusion/ej2-splitbuttons/styles/tailwind3.css';
@import '../node_modules/@syncfusion/ej2-notifications/styles/tailwind3.css';
@import "../node_modules/@syncfusion/ej2-react-grids/styles/tailwind3.css";
```

### Issue: Layout broken or UI misaligned

**Fixes:**

- Ensure all required CSS packages are imported
- Avoid mixing multiple themes
- Check container width/height

## Performance Issues

### Issue: Slow rendering

**Fixes:**

- Use pagination
- Avoid large datasets in memory
- Enable virtualization (if available)
- Memoize components

### Issue: Frequent re-renders

**Fix:**

- Use React.memo
- Avoid inline functions

## Debugging Best Practices

**1. Use Console Logs**

```ts
console.log(props, state);
```

**2. Inspect DOM**

Use DevTools to verify:
- Component presence
- Applied classes
- Layout issues

**3. Check Network Requests**
- Ensure APIs return data correctly.

**4. Isolate Components**
- Test minimal example before integrating.

## Common Error Messages & Fixes

| Error                               | Cause                     | Resolution                     |
|------------------------------------|---------------------------|---------------------------------|
| Module not found                   | Package missing           | Run `npm install`              |
| Cannot read property of undefined  | Invalid data              | Check `dataSource`             |
| Styles missing                     | CSS not imported          | Add required CSS imports       |
| Event not working                  | Wrong handler             | Bind correctly                 |
| Blank component                    | Missing configuration     | Add required props             |