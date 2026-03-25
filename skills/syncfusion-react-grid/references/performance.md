# Performance Optimization in React Grid

## Table of Contents
- [Overview](#overview)
- [Virtual Scrolling](#virtual-scrolling)
- [Infinite Scrolling](#infinite-scrolling)
- [Data Loading Strategies](#data-loading-strategies)
- [Rendering Optimization](#rendering-optimization)
- [Memory Management](#memory-management)
- [Event Optimization](#event-optimization)
- [Bundle Size Optimization](#bundle-size-optimization)
- [Benchmarking](#benchmarking)

## Overview

Performance optimization is critical when handling large datasets. Syncfusion Grid provides multiple strategies for managing thousands or millions of records efficiently.

### Key Performance Metrics

- **Initial Load Time**: Time to display first page of data
- **Time to Interactive (TTI)**: When user can interact
- **Memory Usage**: RAM consumed by the grid
- **Render Time**: Time to render/re-render rows
- **Scroll Smoothness**: FPS during scrolling

---

## Virtual Scrolling

Virtual scrolling renders only visible rows, not entire dataset.

### When to Use

- **10,000+ records** and visible rows <= 100
- Smooth scrolling through massive datasets
- Cannot use pagination

### Setup

```tsx
import { GridComponent, ColumnsDirective, ColumnDirective, Inject, VirtualScroll } from '@syncfusion/ej2-react-grids';

<GridComponent
  dataSource={largeData}  // 100,000+ records
  enableVirtualization={true}
  height='500px'
>
  <ColumnsDirective>
    <ColumnDirective field='OrderID' headerText='Order ID' width='100' />
    <ColumnDirective field='CustomerID' headerText='Customer' width='120' />
  </ColumnsDirective>
  <Inject services={[VirtualScroll]} />
</GridComponent>
```

### Configuration Options

```tsx
const settings = {
  enableVirtualization: true,
  height: '500px',                    // Set explicit height
  rowHeight: 36,                       // Match actual row height
  pageSettings: { pageSize: 12 }       // Viewport size
};

<GridComponent dataSource={largeData} {...settings}>
  {/* columns */}
  <Inject services={[VirtualScroll]} />
</GridComponent>
```

### Virtual Scrolling with Sorting/Filtering

The whole dataset is virtualized even with operations:

```tsx
<GridComponent
  dataSource={largeDataManager}
  enableVirtualization={true}
  allowSorting={true}
  allowFiltering={true}
  height='500px'
>
  <ColumnsDirective>
    <ColumnDirective field='OrderID' headerText='Order ID' width='100' />
    <ColumnDirective field='CustomerID' headerText='Customer' width='120' />
  </ColumnsDirective>
  <Inject services={[VirtualScroll, Sort, Filter]} />
</GridComponent>
```

### Performance: ~1 Million Rows

```
Without Virtual Scroll:
- Initial load: 8-10 seconds
- Memory: 500+ MB
- GPU crash at 500K rows

With Virtual Scroll:
- Initial load: 1-2 seconds
- Memory: 50-100 MB
- Smooth scrolling through 1M rows
```

### Virtual Scrolling Limitations

❌ **Not supported with:**
- Row grouping
- Row spanning
- Batch editing
- Print
- Export (must use server-side)

---

## Infinite Scrolling

Load data on-demand as user scrolls to bottom.

### When to Use

- Unknown total record count
- Real-time streaming data
- Memory constraints
- Mobile applications

### Setup

```tsx
import { GridComponent, Inject, InfiniteScroll } from '@syncfusion/ej2-react-grids';

<GridComponent
  dataSource={data}
  enableInfiniteScrolling={true}
  pageSettings={{ pageSize: 50 }}
  height='500px'
>
  {/* columns */}
  <Inject services={[InfiniteScroll]} />
</GridComponent>
```

### With Remote Data

```tsx
import { DataManager, UrlAdaptor } from '@syncfusion/ej2-data';

const data = new DataManager({
  url: 'https://api.example.com/orders',
  adaptor: new UrlAdaptor(),
});

<GridComponent
  dataSource={data}
  enableInfiniteScrolling={true}
>
  {/* columns */}
  <Inject services={[InfiniteScroll]} />
</GridComponent>
```

### Load More Indicator

```tsx
const gridRef = useRef(null);

return (
  <div>
    <GridComponent
      ref={gridRef}
      dataSource={data}
      enableInfiniteScrolling={true}
      pageSettings={{ pageSize: 50 }}
      height='500px'
      actionBegin={(args) => {
        if (args.requestType === 'infiniteScroll') {
          // Show loading indicator
          setIsLoading(true);
        }
      }}
      actionComplete={(args) => {
        if (args.requestType === 'infiniteScroll') {
          setIsLoading(false);
        }
      }}
    >
      {/* columns */}
    </GridComponent>
    {isLoading && <div>Loading more...</div>}
  </div>
);
```

### Performance Comparison

```
10 pages x 50 rows (Infinite Scroll):
- Memory: ~10 MB
- Smooth scrolling
- One request at start, one per scroll

Pagination:
- Memory: 2-3 MB per page
- No continuous scroll
- One request per page click
```

---

## Data Loading Strategies

### Progressive Loading with Spinners

```tsx
import { GridComponent, Inject, Page } from '@syncfusion/ej2-react-grids';

function App() {
  const [isLoading, setIsLoading] = useState(true);

  return (
    <div>
      {isLoading && <div className='spinner'>Loading...</div>}
      
      <GridComponent
        dataSource={data}
        allowPaging={true}
        pageSettings={{ pageSize: 12 }}
        actionComplete={() => setIsLoading(false)}
      >
        {/* columns */}
        <Inject services={[Page]} />
      </GridComponent>
    </div>
  );
}
```

### Shimmer Loading (Skeleton)

```tsx
function App() {
  const [isLoading, setIsLoading] = useState(true);

  return (
    <div>
      {isLoading && (
        <div className='shimmer-grid'>
          {[...Array(12)].map((_, i) => (
            <div key={i} className='shimmer-row'>
              <div className='shimmer-cell' />
              <div className='shimmer-cell' />
              <div className='shimmer-cell' />
            </div>
          ))}
        </div>
      )}
      
      <GridComponent
        dataSource={data}
        style={{ display: isLoading ? 'none' : 'block' }}
        actionComplete={() => setIsLoading(false)}
      >
        {/* columns */}
      </GridComponent>
    </div>
  );
}
```

```css
.shimmer-row {
  display: flex;
  gap: 10px;
  padding: 10px;
  background: linear-gradient(90deg, #f0f0f0 25%, #e0e0e0 50%, #f0f0f0 75%);
  background-size: 200% 100%;
  animation: shimmer 1.5s infinite;
}

@keyframes shimmer {
  0% { background-position: 200% 0; }
  100% { background-position: -200% 0; }
}
```

### Lazy Load with Defer

```tsx
import React, { Suspense, lazy } from 'react';

const Grid = lazy(() => import('./Grid'));

function App() {
  return (
    <Suspense fallback={<div>Loading grid...</div>}>
      <Grid />
    </Suspense>
  );
}
```

---

## Rendering Optimization

### Column Reordering & Resizing

Disable if not needed to reduce overhead:

```tsx
<GridComponent
  dataSource={data}
  allowReordering={false}  // Disable if not needed
  allowResizing={false}    // Disable if not needed
>
  {/* columns */}
</GridComponent>
```

### Template Optimization

Avoid heavy computations in templates:

```tsx
// ❌ Bad: Heavy computation in template
<ColumnDirective
  field='Freight'
  template={(props) => {
    const result = expensiveCalculation(props);  // Called on each render!
    return <span>{result}</span>;
  }}
/>

// ✅ Good: Pre-compute with useMemo
const freight = useMemo(() => {
  return data.map(item => ({
    ...item,
    freightCategory: item.Freight > 50 ? 'High' : 'Low'
  }));
}, [data]);

<GridComponent dataSource={freight}>
  <ColumnsDirective>
    <ColumnDirective
      field='freightCategory'
      template={(props) => <span>{props.freightCategory}</span>}
    />
  </ColumnsDirective>
</GridComponent>
```

### Avoid Inline Functions

```tsx
// ❌ Bad: Creates new function on each render
<GridComponent
  actionBegin={(args) => {
    console.log(args);
  }}
/>

// ✅ Good: Define outside or use useCallback
const handleActionBegin = useCallback((args) => {
  console.log(args);
}, []);

<GridComponent actionBegin={handleActionBegin} />
```

### Row Height Fixed Value

Set explicit `rowHeight` for virtual scrolling:

```tsx
<GridComponent
  dataSource={data}
  enableVirtualization={true}
  rowHeight={36}  // Match CSS height exactly
  height='500px'
>
  {/* columns */}
</GridComponent>
```

---

## Memory Management

### Clear Data When Hidden

```tsx
const [showGrid, setShowGrid] = useState(true);
const gridRef = useRef(null);

const closeGrid = () => {
  gridRef.current.dataSource = [];  // Clear data
  setShowGrid(false);
};

return (
  showGrid && (
    <GridComponent ref={gridRef} dataSource={largeData}>
      {/* columns */}
    </GridComponent>
  )
);
```

### Cleanup on Unmount

```tsx
import React, { useEffect, useRef } from 'react';

function GridComponent() {
  const gridRef = useRef(null);

  useEffect(() => {
    return () => {
      // Cleanup
      if (gridRef.current) {
        gridRef.current.destroy();
      }
    };
  }, []);

  return <GridComponent ref={gridRef} dataSource={data} />;
}
```

### Batch Updates

```tsx
const gridRef = useRef(null);

const updateMultipleRows = (newData) => {
  gridRef.current.dataSource = newData;  // Single update
};

// Instead of:
// data.forEach(item => gridRef.current.addRecord(item));  // Multiple updates
```

---

## Event Optimization

### Debounce Search

```tsx
import { useCallback, useRef } from 'react';

function GridWithSearch() {
  const gridRef = useRef(null);
  const debounceTimer = useRef(null);

  const handleSearch = useCallback((searchValue) => {
    clearTimeout(debounceTimer.current);
    
    debounceTimer.current = setTimeout(() => {
      gridRef.current.search(searchValue);
    }, 300);  // Wait 300ms after typing stops
  }, []);

  return (
    <div>
      <input
        type='text'
        placeholder='Search...'
        onChange={(e) => handleSearch(e.target.value)}
      />
      <GridComponent ref={gridRef} dataSource={data}>
        {/* columns */}
      </GridComponent>
    </div>
  );
}
```

### Throttle Scroll Events

```tsx
const throttle = (func, limit) => {
  let inThrottle;
  return function() {
    if (!inThrottle) {
      func.apply(this, arguments);
      inThrottle = true;
      setTimeout(() => inThrottle = false, limit);
    }
  };
};

const handleGridScroll = throttle((args) => {
  console.log('Scrolled');
}, 100);

<GridComponent actionBegin={handleGridScroll}>
  {/* columns */}
</GridComponent>
```

### Conditional Event Handlers

```tsx
<GridComponent
  dataSource={data}
  recordDoubleClick={showDetailOnly ? (args) => showDetail(args) : null}
>
  {/* columns */}
</GridComponent>
```

---

## Bundle Size Optimization

### Tree Shaking

Only import what you use:

```tsx
// ❌ Imports all modules
import * from '@syncfusion/ej2-react-grids';

// ✅ Import only needed
import {
  GridComponent,
  ColumnsDirective,
  ColumnDirective,
  Inject,
  Page,
  Sort,
  Filter
} from '@syncfusion/ej2-react-grids';
```

### Selective Module Injection

```tsx
// Minimal bundle
<Inject services={[Page]} />

// vs.

// Large bundle
<Inject services={[Page, Sort, Filter, Group, Edit, Toolbar, ExcelExport, PdfExport]} />
```

### Lazy Load Grid

```tsx
import { lazy, Suspense } from 'react';

const GridWithExports = lazy(() => 
  import('./GridWithExports')  // Only load when needed
);

<Suspense fallback={<div>Loading...</div>}>
  <GridWithExports />
</Suspense>
```

### Bundle Comparison

```
Core Grid Only:        ~45 KB
+ Paging:              ~55 KB (+10 KB)
+ Sorting:             ~65 KB (+10 KB)
+ All Data Ops:        ~90 KB (+25 KB)
+ All with Export:     ~200 KB (+110 KB)
```

---

## Benchmarking

### Measure Render Time

```tsx
useEffect(() => {
  const startTime = performance.now();

  // Grid renders here

  const endTime = performance.now();
  console.log(`Grid rendered in ${endTime - startTime}ms`);
}, []);
```

### Monitor Memory Usage

```tsx
useEffect(() => {
  const logMemory = () => {
    if (performance.memory) {
      console.log('Used Memory:', Math.round(performance.memory.usedJSHeapSize / 1048576) + ' MB');
      console.log('Total Memory:', Math.round(performance.memory.jsHeapSizeLimit / 1048576) + ' MB');
    }
  };

  const interval = setInterval(logMemory, 5000);
  return () => clearInterval(interval);
}, []);
```

### Profiling with React DevTools

1. Install React DevTools browser extension
2. Open DevTools > Profiler tab
3. Record interaction
4. Analyze render times and component updates

### Network Monitoring

Use browser DevTools Network tab to:
- Monitor API response times
- Check for unnecessary requests
- Verify pagination/lazy loading
- Monitor bundle sizes

---

## Performance Checklist

- [ ] Use virtual scrolling for 10K+ records
- [ ] Use infinite scroll for real-time/mobile
- [ ] Implement server-side filtering/sorting
- [ ] Set explicit row height for virtual scroll
- [ ] Use debouncing for search/filtering
- [ ] Optimize templates (avoid heavy computations)
- [ ] Lazy load grid component if not immediately needed
- [ ] Only inject necessary modules
- [ ] Enable pagination for medium datasets (1K-10K)
- [ ] Disable features not used (reordering, resizing, etc.)
- [ ] Monitor memory and render times
- [ ] Test with realistic data size
- [ ] Use CSS-based styling instead of inline
- [ ] Implement loading indicators for UX
- [ ] Profile bundle size regularly
