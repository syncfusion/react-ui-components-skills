---
name: styling-appearance
description: 'Styling and Appearance in React TreeGrid - five built-in themes, CSS customization, CSS variables, dark mode, and accessibility.'
---

# Styling and Appearance

## Table of Contents
- [Built-in Themes](#built-in-themes)
- [CSS Customization](#css-customization)
- [CSS Variables](#css-variables)
- [Dark Mode](#dark-mode)
- [Row and Cell Styling](#row-and-cell-styling)
- [Accessibility](#accessibility)
- [Component Styling](#component-styling)
- [Row and Cell Styling](#row-and-cell-styling)

## Themes

Apply built-in themes:

```tsx
import React from 'react';
import { TreeGridComponent, ColumnsDirective, ColumnDirective } from '@syncfusion/ej2-react-treegrid';

// Import Material theme
import '@syncfusion/ej2-react-treegrid/styles/material.css';
// Alternative themes:
// import '@syncfusion/ej2-react-treegrid/styles/bootstrap.css';
// import '@syncfusion/ej2-react-treegrid/styles/fluent.css';
// import '@syncfusion/ej2-react-treegrid/styles/tailwind.css';
// import '@syncfusion/ej2-react-treegrid/styles/bootstrap5.css';

export default function App() {
  const data = [
    { TaskID: 1, TaskName: 'Planning', Priority: 'High', Children: [] }
  ];

  return (
    <TreeGridComponent
      dataSource={data}
      childMapping="Children"
    >
      <ColumnsDirective>
        <ColumnDirective field="TaskID" headerText="Task ID" width={80} />
        <ColumnDirective field="TaskName" headerText="Task Name" width={160} />
        <ColumnDirective field="Priority" headerText="Priority" width={100} />
      </ColumnsDirective>
    </TreeGridComponent>
  );
}
```

## CSS Customization

Override styles with custom CSS:

```tsx
import React from 'react';
import '@syncfusion/ej2-react-treegrid/styles/material.css';
import './treegrid-custom.css';

export default function App() {
  const data = [{ TaskID: 1, TaskName: 'Planning', Children: [] }];

  return (
    <TreeGridComponent
      dataSource={data}
      childMapping="Children"
      cssClass="custom-treegrid"
    >
      <ColumnsDirective>
        <ColumnDirective field="TaskName" headerText="Task Name" width={160} />
      </ColumnsDirective>
    </TreeGridComponent>
  );
}
```

**treegrid-custom.css:**
```css
.custom-treegrid {
  font-family: 'Arial', sans-serif;
}

.custom-treegrid .e-headercell {
  background-color: #007bff !important;
  color: white !important;
}

.custom-treegrid .e-row {
  background-color: #f8f9fa;
}

.custom-treegrid .e-row:hover {
  background-color: #e9ecef;
}
```

## CSS Variables

Customize with CSS variables:

```tsx
const styles = `
  :root {
    --primary-color: #007bff;
    --header-bg: var(--primary-color);
    --header-text: white;
    --row-hover: #e9ecef;
  }

  .e-treegrid {
    --e-primary: var(--primary-color);
  }

  .e-treegrid .e-headercell {
    background-color: var(--header-bg);
    color: var(--header-text);
  }

  .e-treegrid .e-row:hover {
    background-color: var(--row-hover);
  }
`;

document.head.innerHTML += `<style>${styles}</style>`;
```

## Component Styling

Style specific components:

```tsx

<TreeGridComponent
  dataSource={data}
  childMapping="Children"
  rowHeight={35}
  headerHeight={40}
>
  <ColumnsDirective>
    <ColumnDirective 
      field="TaskName" 
      headerText="Task Name" 
      width={160}
      headerTemplate={<div style={{ color: 'blue', fontWeight: 'bold' }}>Task Name</div>}
    />
  </ColumnsDirective>
</TreeGridComponent>
```

## Row and Cell Styling

Dynamic row and cell styling:

```tsx
<TreeGridComponent
  dataSource={data}
  childMapping="Children"
  rowDataBound={(args) => {
    if (args.data.Priority === 'High') {
      args.row.style.backgroundColor = '#ffcccc';
    }
  }}
  queryCellInfo={(args) => {
    if (args.column.field === 'Priority') {
      const priority = args.data.Priority;
      if (priority === 'High') {
        args.cell.style.color = 'red';
        args.cell.style.fontWeight = 'bold';
      } else if (priority === 'Normal') {
        args.cell.style.color = 'blue';
      }
    }
  }}
>
  <ColumnsDirective>
    <ColumnDirective field="TaskName" headerText="Task Name" width={160} />
    <ColumnDirective field="Priority" headerText="Priority" width={100} />
  </ColumnsDirective>
</TreeGridComponent>
```

## Accessibility Colors

Ensure color contrast for accessibility:

```tsx
const accessibleStyles = `
  .e-treegrid .e-headercell {
    background-color: #003366;  /* Dark blue for contrast */
    color: #ffffff;             /* White text */
  }

  .e-treegrid .e-row {
    color: #333333;             /* Dark text */
  }

  .e-treegrid .e-row:hover {
    background-color: #cccccc;  /* Gray for contrast */
  }

  .e-treegrid .e-selectionbackground {
    background-color: #0066cc;  /* Blue for selection */
  }
`;

document.head.innerHTML += `<style>${accessibleStyles}</style>`;
```

## Key APIs

| Property/Event | Type | Description |
|---|---|---|
| `cssClass` | string | Apply custom CSS class |
| `rowHeight` | number | Row height in pixels |
| `headerHeight` | number | Header row height |
| `rowDataBound` | event | Style rows based on data |
| `queryCellInfo` | event | Style cells dynamically |
| `headerTemplate` | JSX | Custom header styling |

## Built-in Themes

- `Material` - Google Material Design
- `Bootstrap` - Bootstrap 4
- `Bootstrap5` - Bootstrap 5
- `Fluent` - Microsoft Fluent Design
- `Tailwind` - Tailwind CSS

## Common Patterns

1. **Conditional Formatting**: Style rows based on values
2. **Priority Colors**: Red for high, yellow for medium, green for low
3. **Alternating Rows**: Stripe rows for readability
4. **Responsive Layout**: Adjust styles for mobile screens
5. **Dark Mode**: Implement dark theme option

