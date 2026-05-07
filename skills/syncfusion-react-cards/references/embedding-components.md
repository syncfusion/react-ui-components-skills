# Embedding Components in Cards

The Card component provides a flexible container that can host any other component within its content area, enabling the creation of rich, interactive interfaces. By combining the structured layout benefits of cards with the functionality of other Syncfusion components, you can build sophisticated UIs.

## Basic Component Integration

### Simple Component Embedding

Any React component can be placed inside the card's content section:

```jsx
import React from 'react';
import { ListViewComponent } from '@syncfusion/ej2-react-lists';

export default function CardWithList() {
  const todoList = [
    { todoList: 'Pay Bills' },
    { todoList: 'Call Chris' },
    { todoList: 'Meet Andrew' }
  ];

  const fields = { text: 'todoList' };

  return (
    <div className="e-card">
      <div className="e-card-title">To-Do List</div>
      <div className="e-card-separator" />
      <div className="e-card-content">
        <ListViewComponent 
          dataSource={todoList}
          fields={fields}
          showCheckBox={true} />
      </div>
    </div>
  );
}
```

### Component with Header

```jsx
<div className="e-card">
  <div className="e-card-header">
    <div className="e-card-header-caption">
      <div className="e-card-header-title">Component Header</div>
      <div className="e-card-sub-title">Embedded within card</div>
    </div>
  </div>
  <div className="e-card-separator" />
  <div className="e-card-content">
    <YourComponentHere />
  </div>
</div>
```

## ListView Integration

### To-Do List Card

```jsx
import { ListViewComponent } from '@syncfusion/ej2-react-lists';

export default function TodoCard() {
  const todoList = [
    { todoList: 'Pay Bills' },
    { todoList: 'Call Chris' },
    { todoList: 'Meet Andrew' },
    { todoList: 'Visit Manager' },
    { todoList: 'Customer Meeting' }
  ];

  const fields = { text: 'todoList' };

  return (
    <div className="e-card">
      <div className="e-card-title">To-Do List</div>
      <div className="e-card-separator" />
      <div className="e-card-content">
        <ListViewComponent 
          dataSource={todoList}
          fields={fields}
          showCheckBox={true} />
      </div>
    </div>
  );
}
```

**Output:** A card containing an interactive to-do list with checkboxes.

### Advanced ListView Card

```jsx
import React, { useState } from 'react';
import { ListViewComponent } from '@syncfusion/ej2-react-lists';

export default function AdvancedListCard() {
  const [items, setItems] = useState([
    { id: 1, text: 'Pending Task 1', status: 'pending' },
    { id: 2, text: 'Pending Task 2', status: 'pending' },
    { id: 3, text: 'Completed Task 1', status: 'completed' }
  ]);

  const fields = { text: 'text' };

  const onSelect = (args) => {
    console.log('Selected item:', args.data);
  };

  return (
    <div className="e-card" style={{ maxWidth: '400px' }}>
      <div className="e-card-header">
        <div className="e-card-header-caption">
          <div className="e-card-header-title">Project Tasks</div>
          <div className="e-card-sub-title">{items.length} items</div>
        </div>
      </div>
      <div className="e-card-separator" />
      <div className="e-card-content">
        <ListViewComponent 
          dataSource={items}
          fields={fields}
          select={onSelect}
          showCheckBox={true} />
      </div>
      <div className="e-card-actions">
        <button className="e-card-btn">Add Task</button>
        <button className="e-card-btn">Clear Completed</button>
      </div>
    </div>
  );
}
```

## Other Component Integration

### Card with Data Grid

```jsx
import { GridComponent, ColumnsDirective, ColumnDirective, Inject, Page, Sort, Filter } from '@syncfusion/ej2-react-grids';

export default function CardWithGrid() {
  const data = [
    { OrderID: 10248, CustomerName: 'VINET', TotalAmount: 32.38 },
    { OrderID: 10249, CustomerName: 'TOMSP', TotalAmount: 11.61 },
    { OrderID: 10250, CustomerName: 'HANAR', TotalAmount: 65.83 }
  ];

  return (
    <div className="e-card">
      <div className="e-card-header-title">Sales Data</div>
      <div className="e-card-separator" />
      <div className="e-card-content">
        <GridComponent dataSource={data} allowPaging={true} pageSettings={{ pageSize: 5 }}>
          <ColumnsDirective>
            <ColumnDirective field='OrderID' headerText='Order ID' width='100' />
            <ColumnDirective field='CustomerName' headerText='Customer' width='150' />
            <ColumnDirective field='TotalAmount' headerText='Amount' width='100' />
          </ColumnsDirective>
          <Inject services={[Page, Sort, Filter]} />
        </GridComponent>
      </div>
    </div>
  );
}
```

### Card with Chart

```jsx
import { ChartComponent, SeriesCollectionDirective, SeriesDirective, Inject, LineSeries, DateTime, Legend, Tooltip } from '@syncfusion/ej2-react-charts';

export default function CardWithChart() {
  const data = [
    { x: new Date(2024, 0, 1), yValue: 21 },
    { x: new Date(2024, 1, 1), yValue: 24 },
    { x: new Date(2024, 2, 1), yValue: 36 },
    { x: new Date(2024, 3, 1), yValue: 38 }
  ];

  return (
    <div className="e-card">
      <div className="e-card-header-title">Sales Trend</div>
      <div className="e-card-separator" />
      <div className="e-card-content">
        <ChartComponent>
          <SeriesCollectionDirective>
            <SeriesDirective dataSource={data} xName='x' yName='yValue' type='Line' />
          </SeriesCollectionDirective>
          <Inject services={[LineSeries, DateTime, Legend, Tooltip]} />
        </ChartComponent>
      </div>
    </div>
  );
}
```

### Card with Buttons Component

```jsx
import { ButtonComponent } from '@syncfusion/ej2-react-buttons';

export default function CardWithButtons() {
  return (
    <div className="e-card">
      <div className="e-card-header-title">Settings</div>
      <div className="e-card-separator" />
      <div className="e-card-content" style={{ display: 'flex', flexDirection: 'column', gap: '10px' }}>
        <ButtonComponent className='e-primary'>Save Changes</ButtonComponent>
        <ButtonComponent className='e-outline'>Cancel</ButtonComponent>
      </div>
    </div>
  );
}
```

### Card with Dropdown

```jsx
import { DropDownListComponent } from '@syncfusion/ej2-react-dropdowns';

export default function CardWithDropdown() {
  const countries = [
    { text: 'United States', value: 'USA' },
    { text: 'Canada', value: 'CAN' },
    { text: 'United Kingdom', value: 'UK' },
    { text: 'Australia', value: 'AUS' }
  ];

  const fields = { text: 'text', value: 'value' };

  return (
    <div className="e-card">
      <div className="e-card-header-title">Select Country</div>
      <div className="e-card-separator" />
      <div className="e-card-content">
        <DropDownListComponent 
          dataSource={countries}
          fields={fields}
          placeholder="Choose a country"
          width="100%" />
      </div>
    </div>
  );
}
```

## Dynamic Content Patterns

### Card with State Management

```jsx
import React, { useState } from 'react';
import { ListViewComponent } from '@syncfusion/ej2-react-lists';

export default function DynamicCard() {
  const [items, setItems] = useState([
    { id: 1, name: 'Item 1' },
    { id: 2, name: 'Item 2' }
  ]);
  
  const [newItem, setNewItem] = useState('');

  const addItem = () => {
    if (newItem.trim()) {
      setItems([...items, { id: items.length + 1, name: newItem }]);
      setNewItem('');
    }
  };

  const fields = { text: 'name' };

  return (
    <div className="e-card">
      <div className="e-card-header-title">Dynamic List</div>
      <div className="e-card-separator" />
      <div className="e-card-content">
        <ListViewComponent 
          dataSource={items}
          fields={fields} />
      </div>
      <div className="e-card-separator" />
      <div className="e-card-content">
        <div style={{ display: 'flex', gap: '5px' }}>
          <input 
            type="text"
            value={newItem}
            onChange={(e) => setNewItem(e.target.value)}
            placeholder="Add new item"
            style={{ flex: 1, padding: '8px', borderRadius: '4px', border: '1px solid #ccc' }} />
          <button 
            onClick={addItem}
            style={{ padding: '8px 16px', background: '#007bff', color: 'white', border: 'none', borderRadius: '4px', cursor: 'pointer' }}>
            Add
          </button>
        </div>
      </div>
    </div>
  );
}
```

### Card with Conditional Content

```jsx
import React, { useState } from 'react';

export default function ConditionalCard() {
  const [expanded, setExpanded] = useState(false);
  const [selectedTab, setSelectedTab] = useState('overview');

  return (
    <div className="e-card">
      <div className="e-card-header-title">Content Switcher</div>
      <div className="e-card-separator" />
      
      <div className="e-card-content">
        <div style={{ display: 'flex', gap: '10px', marginBottom: '15px' }}>
          <button 
            onClick={() => setSelectedTab('overview')}
            style={{
              padding: '8px 12px',
              background: selectedTab === 'overview' ? '#007bff' : '#e9ecef',
              color: selectedTab === 'overview' ? 'white' : 'black',
              border: 'none',
              borderRadius: '4px',
              cursor: 'pointer'
            }}>
            Overview
          </button>
          <button 
            onClick={() => setSelectedTab('details')}
            style={{
              padding: '8px 12px',
              background: selectedTab === 'details' ? '#007bff' : '#e9ecef',
              color: selectedTab === 'details' ? 'white' : 'black',
              border: 'none',
              borderRadius: '4px',
              cursor: 'pointer'
            }}>
            Details
          </button>
        </div>
      </div>

      <div className="e-card-separator" />

      <div className="e-card-content">
        {selectedTab === 'overview' && (
          <div>
            <h3>Overview</h3>
            <p>General information about the item</p>
          </div>
        )}
        {selectedTab === 'details' && (
          <div>
            <h3>Details</h3>
            <p>Detailed specification and information</p>
          </div>
        )}
      </div>
    </div>
  );
}
```

## Use Cases

### Dashboard Widget Card

Combine charts, grids, and metrics in a single card:

```jsx
export default function DashboardCard() {
  return (
    <div className="e-card">
      <div className="e-card-header-title">Sales Dashboard</div>
      <div className="e-card-separator" />
      <div className="e-card-content">
        <div style={{ display: 'grid', gridTemplateColumns: '1fr 1fr', gap: '10px', marginBottom: '15px' }}>
          <div style={{ padding: '10px', background: '#f0f0f0', borderRadius: '4px' }}>
            <p style={{ margin: '0 0 5px 0', fontSize: '12px', color: '#666' }}>Total Sales</p>
            <h3 style={{ margin: 0 }}>$45,290</h3>
          </div>
          <div style={{ padding: '10px', background: '#f0f0f0', borderRadius: '4px' }}>
            <p style={{ margin: '0 0 5px 0', fontSize: '12px', color: '#666' }}>Growth</p>
            <h3 style={{ margin: 0, color: '#28a745' }}>+12.5%</h3>
          </div>
        </div>
        {/* Chart component would go here */}
      </div>
    </div>
  );
}
```

### Form Card

Combine form elements with validation:

```jsx
export default function FormCard() {
  const [formData, setFormData] = React.useState({ name: '', email: '' });

  const handleChange = (e) => {
    setFormData({ ...formData, [e.target.name]: e.target.value });
  };

  const handleSubmit = () => {
    console.log('Form submitted:', formData);
  };

  return (
    <div className="e-card">
      <div className="e-card-header-title">Contact Form</div>
      <div className="e-card-separator" />
      <div className="e-card-content">
        <input 
          type="text"
          name="name"
          placeholder="Your name"
          value={formData.name}
          onChange={handleChange}
          style={{ width: '100%', padding: '8px', marginBottom: '10px', borderRadius: '4px', border: '1px solid #ccc', boxSizing: 'border-box' }} />
        <input 
          type="email"
          name="email"
          placeholder="Your email"
          value={formData.email}
          onChange={handleChange}
          style={{ width: '100%', padding: '8px', marginBottom: '10px', borderRadius: '4px', border: '1px solid #ccc', boxSizing: 'border-box' }} />
      </div>
      <div className="e-card-actions">
        <button className="e-card-btn" onClick={handleSubmit}>Submit</button>
      </div>
    </div>
  );
}
```

## Best Practices

1. **Performance**: Lazy load heavy components inside cards to improve initial render time

2. **Data binding**: Pass data to embedded components through props for maintainability

3. **Event handling**: Properly handle component events and communicate up/down the hierarchy

4. **Responsive design**: Ensure embedded components respond to card container width

5. **Accessibility**: Maintain semantic HTML and ARIA labels for embedded components

6. **Styling**: Consider card styling when embedding to maintain consistent appearance

7. **Memory management**: Clean up subscriptions and event listeners in useEffect cleanup

**Return to:** [SKILL.md](../SKILL.md) for navigation to other topics.
