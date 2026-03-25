---
name: paging
description: 'Paging in React TreeGrid - pageSizes, currentPage, programmatic navigation, custom pager templates, and server-side paging.'
---

# Paging

## Table of Contents
- [Enable Paging](#enable-paging)
- [Page Configuration](#page-configuration)
- [Pager Types](#pager-types)
- [Programmatic Navigation](#programmatic-navigation)
- [Custom Pager](#custom-pager)
- [Server-side Paging](#server-side-paging)

## Basic Paging

Enable paging with default settings:

```tsx
import React from 'react';
import { TreeGridComponent, ColumnsDirective, ColumnDirective, Inject, Page } from '@syncfusion/ej2-react-treegrid';

export default function App() {
  const data = [
    { TaskID: 1, TaskName: 'Planning', Children: [] },
    { TaskID: 2, TaskName: 'Development', Children: [] },
    // ... more records
  ];

  return (
    <TreeGridComponent
      dataSource={data}
      childMapping="Children"
      allowPaging={true}
      pageSettings={{ pageSize: 10, pageCount: 5 }}
    >
      <ColumnsDirective>
        <ColumnDirective field="TaskID" headerText="Task ID" width={80} />
        <ColumnDirective field="TaskName" headerText="Task Name" width={160} />
      </ColumnsDirective>
      <Inject services={[Page]} />
    </TreeGridComponent>
  );
}
```

## Pager Configuration

Configure paging behavior:

```tsx
<TreeGridComponent
  dataSource={data}
  childMapping="Children"
  allowPaging={true}
  pageSettings={{
    pageSize: 15,
    pageCount: 10,
    currentPage: 1,
    pageSizes: [10, 15, 20, 25]
  }}
  pagerTemplate="{currentPage} of {totalPages} pages"
>
  <ColumnsDirective>
    <ColumnDirective field="TaskID" headerText="Task ID" width={80} />
    <ColumnDirective field="TaskName" headerText="Task Name" width={160} />
  </ColumnsDirective>
  <Inject services={[Page]} />
</TreeGridComponent>
```

## Pager Modes

Different page size modes:

```tsx
<TreeGridComponent
  dataSource={data}
  childMapping="Children"
  allowPaging={true}
  pageSettings={{
    pageSize: 10,
    pageSizeMode: 'Root'  // 'Root', 'All'
  }}
>
  <ColumnsDirective>
    <ColumnDirective field="TaskName" headerText="Task Name" width={160} />
  </ColumnsDirective>
  <Inject services={[Page]} />
</TreeGridComponent>
```

## Programmatic Paging

Navigate via code:

```tsx
const treeGridRef = React.useRef();

const goToPage = (pageNum) => {
  treeGridRef.current.goToPage(pageNum);
};

<TreeGridComponent ref={treeGridRef} dataSource={data} childMapping="Children" allowPaging={true}>
  <ColumnsDirective>
    <ColumnDirective field="TaskName" headerText="Task Name" width={160} />
  </ColumnsDirective>
  <Inject services={[Page]} />
</TreeGridComponent>
```

## Key APIs

| Property/Method | Type | Description |
|---|---|---|
| `allowPaging` | boolean | Enable paging feature |
| `pageSettings` | object | Configure pager properties |
| `pageSize` | number | Records per page |
| `pageCount` | number | Links to display in pager |
| `currentPage` | number | Active page number |
| `goToPage` | method | Navigate to page |

## Common Patterns

1. **Adaptive Page Size**: Adjust page size based on available space
2. **Server-side Paging**: Implement server paging for large datasets
3. **Custom Pager Items**: Add custom elements to pager

