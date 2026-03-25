# Advanced Tutorials & Patterns

## Table of Contents
- [Overview](#overview)
- [Real-Time Data Updates](#real-time-data-updates)
- [Master-Detail with Filtering](#master-detail-with-filtering)
- [Complex Calculations](#complex-calculations)
- [Advanced Filtering](#advanced-filtering)
- [Custom Themes](#custom-themes)
- [Performance Monitoring](#performance-monitoring)
- [Testing Grids](#testing-grids)

## Overview

This guide covers advanced patterns, real-world scenarios, and best practices for production grids.

---

## Real-Time Data Updates

### Auto-Refresh Grid Data

```tsx
import React, { useRef, useEffect, useState } from 'react';
import { GridComponent } from '@syncfusion/ej2-react-grids';

function GridWithAutoRefresh() {
  const gridRef = useRef(null);
  const [refreshInterval, setRefreshInterval] = useState(5000);  // 5 seconds

  useEffect(() => {
    const interval = setInterval(async () => {
      try {
        const response = await fetch('/api/orders');
        const newData = await response.json();
        
        // Update grid data
        gridRef.current.dataSource = newData;
      } catch (error) {
        console.error('Refresh error:', error);
      }
    }, refreshInterval);

    return () => clearInterval(interval);
  }, [refreshInterval]);

  return (
    <div>
      <label>Refresh every:
        <select 
          value={refreshInterval} 
          onChange={(e) => setRefreshInterval(parseInt(e.target.value))}
        >
          <option value='1000'>1 second</option>
          <option value='5000'>5 seconds</option>
          <option value='10000'>10 seconds</option>
          <option value='30000'>30 seconds</option>
        </select>
      </label>

      <GridComponent ref={gridRef} dataSource={[]} allowPaging={true}>
        {/* columns */}
      </GridComponent>
    </div>
  );
}

export default GridWithAutoRefresh;
```

### WebSocket Real-Time Updates

```tsx
import React, { useRef, useEffect } from 'react';
import { GridComponent } from '@syncfusion/ej2-react-grids';

function GridWithWebSocket() {
  const gridRef = useRef(null);

  useEffect(() => {
    const ws = new WebSocket('ws://api.example.com/orders');

    ws.onmessage = (event) => {
      const newData = JSON.parse(event.data);
      
      // Update specific row or add new row
      const data = gridRef.current.dataSource;
      const index = data.findIndex(item => item.OrderID === newData.OrderID);
      
      if (index >= 0) {
        // Update existing row
        data[index] = newData;
      } else {
        // Add new row
        data.push(newData);
      }
      
      gridRef.current.refresh();
    };

    ws.onerror = (error) => {
      console.error('WebSocket error:', error);
    };

    return () => ws.close();
  }, []);

  return (
    <GridComponent ref={gridRef} dataSource={[]} allowPaging={true}>
      {/* columns */}
    </GridComponent>
  );
}

export default GridWithWebSocket;
```

### SignalR Real-Time Updates

```tsx
import React, { useRef, useEffect } from 'react';
import * as signalR from '@microsoft/signalr';
import { GridComponent } from '@syncfusion/ej2-react-grids';

function GridWithSignalR() {
  const gridRef = useRef(null);
  const connectionRef = useRef(null);

  useEffect(() => {
    const connection = new signalR.HubConnectionBuilder()
      .withUrl('/orderHub')
      .withAutomaticReconnect()
      .build();

    connectionRef.current = connection;

    connection.on('OrderUpdated', (updatedOrder) => {
      const data = gridRef.current.dataSource;
      const index = data.findIndex(o => o.OrderID === updatedOrder.OrderID);
      
      if (index >= 0) {
        data[index] = updatedOrder;
        gridRef.current.setCellValue(index, 'Freight', updatedOrder.Freight);
      }
    });

    connection.on('OrderAdded', (newOrder) => {
      gridRef.current.addRecord(newOrder);
    });

    connection.start().catch(err => console.error('Connection error:', err));

    return () => connection.stop();
  }, []);

  return (
    <GridComponent ref={gridRef} dataSource={[]} allowPaging={true}>
      {/* columns */}
    </GridComponent>
  );
}

export default GridWithSignalR;
```

---

## Master-Detail with Filtering

### Filtered Detail Grid

```tsx
import React, { useState } from 'react';
import { GridComponent, ColumnsDirective, ColumnDirective } from '@syncfusion/ej2-react-grids';

function MasterDetailWithFiltering() {
  const [selectedOrder, setSelectedOrder] = useState(null);

  const detailTemplate = (props) => {
    return (
      <GridComponent
        dataSource={getOrderDetails(props.OrderID)}
        allowPaging={true}
        pageSettings={{ pageSize: 5 }}
      >
        <ColumnsDirective>
          <ColumnDirective field='ProductID' headerText='Product ID' width='100' />
          <ColumnDirective field='ProductName' headerText='Product' width='150' />
          <ColumnDirective field='Quantity' headerText='Quantity' width='100' />
          <ColumnDirective field='UnitPrice' headerText='Unit Price' width='100' format='C2' />
        </ColumnsDirective>
      </GridComponent>
    );
  };

  const handleMasterRowClick = (args) => {
    setSelectedOrder(args.rowData);
  };

  return (
    <GridComponent
      dataSource={orders}
      detailTemplate={detailTemplate}
      recordClick={handleMasterRowClick}
    >
      <ColumnsDirective>
        <ColumnDirective field='OrderID' headerText='Order ID' width='100' />
        <ColumnDirective field='CustomerID' headerText='Customer' width='120' />
        <ColumnDirective field='OrderDate' headerText='Order Date' type='date' format='yMd' />
        <ColumnDirective field='Freight' headerText='Freight' width='100' format='C2' />
      </ColumnsDirective>
    </GridComponent>
  );
}

export default MasterDetailWithFiltering;
```

---

## Complex Calculations

### Calculated Columns with Aggregates

```tsx
function GridWithCalculations() {
  const data = orders.map(order => ({
    ...order,
    Tax: order.Freight * 0.1,
    Total: order.Freight + (order.Freight * 0.1)
  }));

  return (
    <GridComponent dataSource={data}>
      <ColumnsDirective>
        <ColumnDirective field='OrderID' headerText='Order ID' width='100' />
        <ColumnDirective field='Freight' headerText='Freight' width='100' format='C2' />
        <ColumnDirective
          field='Tax'
          headerText='Tax (10%)'
          template={(props) => `$${(props.Freight * 0.1).toFixed(2)}`}
          width='100'
        />
        <ColumnDirective
          field='Total'
          headerText='Total'
          template={(props) => `$${(props.Freight * 1.1).toFixed(2)}`}
          width='100'
        />
      </ColumnsDirective>
      <AggregatesDirective>
        <AggregateDirective>
          <AggregateColumnsDirective>
            <AggregateColumnDirective field='Freight' type='Sum' />
            <AggregateColumnDirective field='Tax' type='Sum' />
            <AggregateColumnDirective field='Total' type='Sum' />
          </AggregateColumnsDirective>
        </AggregateDirective>
      </AggregatesDirective>
      <Inject services={[Aggregates]} />
    </GridComponent>
  );
}
```

### Running Totals

```tsx
function GridWithRunningTotal() {
  const [runningTotal, setRunningTotal] = useState(0);

  const getRunningTotal = (currentFreight) => {
    const newTotal = runningTotal + currentFreight;
    setRunningTotal(newTotal);
    return newTotal;
  };

  return (
    <GridComponent dataSource={orders}>
      <ColumnsDirective>
        <ColumnDirective field='OrderID' headerText='Order ID' width='100' />
        <ColumnDirective field='Freight' headerText='Freight' width='100' format='C2' />
        <ColumnDirective
          headerText='Running Total'
          template={(props) => `$${getRunningTotal(props.Freight).toFixed(2)}`}
          width='150'
        />
      </ColumnsDirective>
    </GridComponent>
  );
}
```

---

## Advanced Filtering

### Multi-Column Complex Filter

```tsx
function GridWithComplexFilter() {
  const gridRef = useRef(null);

  const applyAdvancedFilter = () => {
    const gridInstance = gridRef.current;
    
    // Create complex filter: (Freight > 50 AND Status = 'Active') OR (CustomerID = 'VINET')
    const predicate = new Predicate('Freight', 'greaterThan', 50);
    predicate.and('Status', 'equal', 'Active');
    predicate.or('CustomerID', 'equal', 'VINET');

    gridInstance.query = new Query().where(predicate);
    gridInstance.refresh();
  };

  return (
    <div>
      <button onClick={applyAdvancedFilter}>Apply Complex Filter</button>

      <GridComponent
        ref={gridRef}
        dataSource={orders}
        allowFiltering={true}
        filterSettings={{ type: 'Menu' }}
      >
        <ColumnsDirective>
          <ColumnDirective field='OrderID' headerText='Order ID' width='100' />
          <ColumnDirective field='CustomerID' headerText='Customer' width='120' />
          <ColumnDirective field='Freight' headerText='Freight' width='100' format='C2' />
          <ColumnDirective field='Status' headerText='Status' width='100' />
        </ColumnsDirective>
        <Inject services={[Filter]} />
      </GridComponent>
    </div>
  );
}

import { Predicate, Query } from '@syncfusion/ej2-data';
```

### Date Range Filtering

```tsx
function GridWithDateRangeFilter() {
  const gridRef = useRef(null);
  const [startDate, setStartDate] = useState(null);
  const [endDate, setEndDate] = useState(null);

  const applyDateFilter = () => {
    const predicate = new Predicate('OrderDate', 'greaterthanorequal', startDate);
    predicate.and('OrderDate', 'lessthanorequal', endDate);

    gridRef.current.query = new Query().where(predicate);
    gridRef.current.refresh();
  };

  return (
    <div>
      <div>
        <label>Start Date:
          <input
            type='date'
            onChange={(e) => setStartDate(new Date(e.target.value))}
          />
        </label>
        <label>End Date:
          <input
            type='date'
            onChange={(e) => setEndDate(new Date(e.target.value))}
          />
        </label>
        <button onClick={applyDateFilter}>Filter</button>
      </div>

      <GridComponent ref={gridRef} dataSource={orders}>
        <ColumnsDirective>
          <ColumnDirective field='OrderID' headerText='Order ID' width='100' />
          <ColumnDirective
            field='OrderDate'
            headerText='Order Date'
            type='date'
            format='yMd'
          />
          <ColumnDirective field='Freight' headerText='Freight' width='100' format='C2' />
        </ColumnsDirective>
      </GridComponent>
    </div>
  );
}

import { Predicate, Query } from '@syncfusion/ej2-data';
```

---

## Custom Themes

### Create Custom Theme

```tsx
const customTheme = `
  /* Header styling */
  .e-grid .e-headercell {
    background-color: #2c3e50;
    color: #ecf0f1;
    font-weight: 600;
    padding: 12px;
  }

  /* Row styling */
  .e-grid .e-row {
    border-bottom: 1px solid #ecf0f1;
  }

  .e-grid .e-row:hover {
    background-color: #f8f9fa;
  }

  /* Selected row */
  .e-grid .e-selectionbackground {
    background-color: #3498db;
    color: white;
  }

  /* Alternating row colors */
  .e-grid .e-row:nth-child(even) {
    background-color: #f8f9fa;
  }

  /* Cell padding and text */
  .e-grid .e-gridcontent td {
    padding: 12px;
    color: #2c3e50;
  }

  /* Pager styling */
  .e-pagercontainer {
    background-color: #ecf0f1;
    padding: 8px;
  }

  .e-pagercontainer .e-pagerjump input {
    border: 1px solid #95a5a6;
    padding: 6px;
    border-radius: 4px;
  }

  /* Edit cell styling */
  .e-grid .e-editedrow .e-gridcell {
    background-color: #fffacd;
  }

  /* Sort indicator */
  .e-grid .e-headercell .e-sortnumber {
    background-color: #3498db;
    color: white;
    border-radius: 50%;
    padding: 2px 6px;
  }
`;

<style>{customTheme}</style>
<GridComponent dataSource={data}>
  {/* columns */}
</GridComponent>
```

### Dynamic Theme Switching

```tsx
import React, { useState } from 'react';

function GridWithThemeSwitcher() {
  const [theme, setTheme] = useState('light');

  const themes = {
    light: { background: '#ffffff', text: '#000000' },
    dark: { background: '#1e1e1e', text: '#ffffff' },
    blue: { background: '#e3f2fd', text: '#1565c0' }
  };

  const currentTheme = themes[theme];
  const themeStyles = `
    .themed-grid {
      background-color: ${currentTheme.background};
      color: ${currentTheme.text};
    }

    .themed-grid .e-headercell {
      background-color: ${theme === 'dark' ? '#333333' : '#f5f5f5'};
      color: ${currentTheme.text};
    }
  `;

  return (
    <div>
      <select value={theme} onChange={(e) => setTheme(e.target.value)}>
        <option value='light'>Light</option>
        <option value='dark'>Dark</option>
        <option value='blue'>Blue</option>
      </select>

      <style>{themeStyles}</style>
      <div className='themed-grid'>
        <GridComponent dataSource={data}>
          {/* columns */}
        </GridComponent>
      </div>
    </div>
  );
}

export default GridWithThemeSwitcher;
```

---

## Performance Monitoring

### Measure Grid Performance

```tsx
function GridWithPerformanceMonitoring() {
  const gridRef = useRef(null);
  const [metrics, setMetrics] = useState({});

  useEffect(() => {
    const observer = new PerformanceObserver((list) => {
      for (const entry of list.getEntries()) {
        console.log(`${entry.name}: ${entry.duration}ms`);
      }
    });

    observer.observe({ entryTypes: ['measure'] });

    return () => observer.disconnect();
  }, []);

  const measureInitialLoad = () => {
    performance.mark('grid-load-start');
    
    // Grid renders here
    
    performance.mark('grid-load-end');
    performance.measure('grid-load', 'grid-load-start', 'grid-load-end');
  };

  const countDataFetches = () => {
    const navigationTiming = performance.getEntriesByType('navigation')[0];
    console.log('Network timing:', {
      dnsLookup: navigationTiming.domainLookupEnd - navigationTiming.domainLookupStart,
      tcpConnect: navigationTiming.connectEnd - navigationTiming.connectStart,
      ttfb: navigationTiming.responseStart - navigationTiming.requestStart,
      download: navigationTiming.responseEnd - navigationTiming.responseStart
    });
  };

  return (
    <div>
      <button onClick={measureInitialLoad}>Measure Load Time</button>
      <button onClick={countDataFetches}>Show Network Metrics</button>

      <GridComponent
        ref={gridRef}
        dataSource={data}
        actionComplete={(args) => {
          console.log(`Action '${args.requestType}' completed in ${performance.now()}ms`);
        }}
      >
        {/* columns */}
      </GridComponent>
    </div>
  );
}
```

---


## Testing Grids

### Unit Tests

```tsx
import { render, screen, waitFor } from '@testing-library/react';
import '@testing-library/jest-dom';
import GridComponent from './GridComponent';

describe('Grid Component', () => {
  test('renders grid with data', async () => {
    const { container } = render(<GridComponent dataSource={mockData} />);
    expect(container.querySelector('.e-grid')).toBeInTheDocument();
  });

  test('displays correct number of rows', async () => {
    const { container } = render(<GridComponent dataSource={mockData} />);
    await waitFor(() => {
      const rows = container.querySelectorAll('.e-row');
      expect(rows).toHaveLength(mockData.length);
    });
  });

  test('sorting works correctly', async () => {
    const { container } = render(<GridComponent dataSource={mockData} allowSorting={true} />);
    const headerCell = container.querySelector('[data-field="OrderID"]');
    headerCell.click();
    
    await waitFor(() => {
      const firstRow = container.querySelector('.e-row');
      expect(firstRow.textContent).toContain('10248');
    });
  });

  test('filtering works correctly', async () => {
    const { container } = render(<GridComponent dataSource={mockData} allowFiltering={true} />);
    const filterInput = container.querySelector('.e-filterinput');
    filterInput.value = 'VINET';
    fireEvent.change(filterInput);
    
    await waitFor(() => {
      const rows = container.querySelectorAll('.e-row');
      expect(rows.length).toBeLessThan(mockData.length);
    });
  });

  test('editing saves data correctly', async () => {
    const { container } = render(
      <GridComponent dataSource={mockData} editSettings={{ mode: 'Inline' }} />
    );
    
    const editButton = container.querySelector('[aria-label="Edit"]');
    fireEvent.click(editButton);
    
    const input = container.querySelector('input');
    fireEvent.change(input, { target: { value: 'NewValue' } });
    
    const saveButton = container.querySelector('[aria-label="Save"]');
    fireEvent.click(saveButton);
    
    await waitFor(() => {
      expect(mockData[0].CustomerID).toBe('NewValue');
    });
  });
});

import { render, screen, waitFor, fireEvent } from '@testing-library/react';
```

### Integration Tests

```tsx
describe('Grid Integration Tests', () => {
  test('pagination, sorting, and filtering work together', async () => {
    const { container } = render(
      <GridComponent
        dataSource={largeDataset}
        allowPaging={true}
        allowSorting={true}
        allowFiltering={true}
      />
    );

    // Apply filter
    const filterInput = container.querySelector('.e-filterinput');
    filterInput.value = 'Berlin';
    fireEvent.change(filterInput);

    // Sort
    const sortHeader = container.querySelector('[data-field="OrderDate"]');
    fireEvent.click(sortHeader);

    // Change page
    const pageInput = container.querySelector('.e-pagejump input');
    fireEvent.change(pageInput, { target: { value: '2' } });

    await waitFor(() => {
      const rows = container.querySelectorAll('.e-row');
      expect(rows.length).toBeGreaterThan(0);
    });
  });

  test('edit and save with validation', async () => {
    const onSave = jest.fn();
    const { container } = render(
      <GridComponent
        dataSource={mockData}
        editSettings={{ mode: 'Dialog', allowEditing: true }}
        actionComplete={onSave}
      />
    );

    // Start edit
    const editButton = container.querySelector('[aria-label="Edit"]');
    fireEvent.click(editButton);

    // Fill form
    const form = container.querySelector('.e-dialog-component');
    const inputs = form.querySelectorAll('input');
    fireEvent.change(inputs[0], { target: { value: 'UpdatedValue' } });

    // Save
    const saveButton = form.querySelector('.e-primary');
    fireEvent.click(saveButton);

    await waitFor(() => {
      expect(onSave).toHaveBeenCalled();
    });
  });
});
```

---

## Best Practices Summary

1. **Real-time data**: Use WebSocket or SignalR for live updates
2. **Master-detail**: Filter child data based on parent selection
3. **Calculations**: Pre-compute complex values before binding
4. **Filtering**: Use Predicate for complex queries
5. **Themes**: Create reusable theme CSS modules
6. **Performance**: Monitor with Performance API
7. **Testing**: Test user interactions and data scenarios
8. **Error handling**: Gracefully handle API failures
9. **Accessibility**: Ensure keyboard navigation works
10. **Documentation**: Document custom configurations
