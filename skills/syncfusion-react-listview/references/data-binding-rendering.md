# Data Binding & Rendering

## Table of Contents
- [Local Data Sources](#local-data-sources)
- [Object Data Mapping](#object-data-mapping)
- [Remote Data with DataManager](#remote-data-with-datamanager)
- [Query Filtering](#query-filtering)
- [Pagination](#pagination)
- [Dynamic Data Updates](#dynamic-data-updates)

## Local Data Sources

### String Array

```tsx
import { ListViewComponent } from '@syncfusion/ej2-react-lists';

const data = ['Apple', 'Banana', 'Orange', 'Mango'];

<ListViewComponent
  id="string-list"
  dataSource={data}
/>
```

### Number Array

```tsx
const numbers = [1, 2, 3, 4, 5];

<ListViewComponent
  id="number-list"
  dataSource={numbers}
/>
```

### Simple Objects

```tsx
const items = [
  { id: '1', text: 'Item 1' },
  { id: '2', text: 'Item 2' },
  { id: '3', text: 'Item 3' }
];

<ListViewComponent
  id="object-list"
  dataSource={items}
  fields={{ id: 'id', text: 'text' }}
/>
```

## Object Data Mapping

### Basic Field Mapping

```tsx
interface Product {
  productId: number;
  productName: string;
  category: string;
  price: number;
}

const products: Product[] = [
  { productId: 1, productName: 'Laptop', category: 'Electronics', price: 999 },
  { productId: 2, productName: 'Desk', category: 'Furniture', price: 299 },
  { productId: 3, productName: 'Chair', category: 'Furniture', price: 199 }
];

<ListViewComponent
  dataSource={products}
  fields={{
    id: 'productId',
    text: 'productName',
    tooltip: 'category'
  }}
/>
```

### Field Mapping for Features

```tsx
interface ComplexItem {
  itemId: string;
  itemName: string;
  description: string;
  isActive: boolean;
  iconClass: string;
  imageUrl: string;
  childItems?: ComplexItem[];
  showCheckBox?: boolean;
  sortField?: string;
}

const data: ComplexItem[] = [
  {
    itemId: '1',
    itemName: 'Documents',
    description: 'My documents folder',
    isActive: true,
    iconClass: 'e-icons e-folder',
    imageUrl: '/images/folder.png',
    childItems: [
      { itemId: '1-1', itemName: 'Report.pdf', description: 'Q4 Report', isActive: true, iconClass: 'e-icons e-file', imageUrl: '', sortField: 'Report' }
    ],
    sortField: 'Documents'
  }
];

<ListViewComponent
  dataSource={data}
  fields={{
    id: 'itemId',
    text: 'itemName',
    tooltip: 'description',
    enabled: 'isActive',
    iconCss: 'iconClass',
    image: 'imageUrl',
    child: 'childItems',
    sortBy: 'sortField'
  }}
/>
```

### Nested/Hierarchical Data

```tsx
interface TreeItem {
  id: string;
  text: string;
  child?: TreeItem[];
}

const hierarchyData: TreeItem[] = [
  {
    id: '1',
    text: 'Asia',
    child: [
      { id: '1.1', text: 'India', child: [
        { id: '1.1.1', text: 'Delhi' },
        { id: '1.1.2', text: 'Mumbai' }
      ]},
      { id: '1.2', text: 'China' }
    ]
  },
  {
    id: '2',
    text: 'Europe',
    child: [
      { id: '2.1', text: 'Germany' },
      { id: '2.2', text: 'France' }
    ]
  }
];

<ListViewComponent
  dataSource={hierarchyData}
  fields={{
    id: 'id',
    text: 'text',
    child: 'child'
  }}
/>
```

### Grouped Data

```tsx
interface GroupedItem {
  id: string;
  text: string;
  category: string;
}

const groupedData: GroupedItem[] = [
  { id: '1', text: 'Apple', category: 'Fruit' },
  { id: '2', text: 'Carrot', category: 'Vegetable' },
  { id: '3', text: 'Banana', category: 'Fruit' },
  { id: '4', text: 'Tomato', category: 'Vegetable' }
];

<ListViewComponent
  dataSource={groupedData}
  fields={{
    id: 'id',
    text: 'text',
    groupBy: 'category'  // ← Group by category field
  }}
/>
```

## Remote Data with DataManager

### Basic Remote Data

```tsx
import { ListViewComponent } from '@syncfusion/ej2-react-lists';
import { DataManager, UrlAdaptor } from '@syncfusion/ej2-data';

const data = new DataManager({
  url: 'https://jsonplaceholder.typicode.com/users',
  adaptor: new UrlAdaptor()
});

<ListViewComponent
  dataSource={data}
  fields={{
    id: 'id',
    text: 'name'
  }}
  headerTitle="Users"
  showHeader={true}
/>
```

### With CORS Proxy

```tsx
const data = new DataManager({
  url: 'https://api.example.com/items',
  adaptor: new UrlAdaptor(),
  crossDomain: true
});
```

### Custom API Response Mapping

```tsx
const data = new DataManager({
  url: 'https://api.example.com/products',
  adaptor: new UrlAdaptor()
});

<ListViewComponent
  dataSource={data}
  fields={{
    id: 'productId',
    text: 'productName',
    tooltip: 'description'
  }}
/>
```

### Error Handling

```tsx
const handleActionFailure = (args: any) => {
  console.error('Data fetch failed:', args);
  alert('Failed to load data');
};

<ListViewComponent
  dataSource={remoteData}
  actionFailure={handleActionFailure}
/>
```

## Query Filtering

### Basic Query

```tsx
import { Query } from '@syncfusion/ej2-data';

const data = new DataManager({
  url: 'https://jsonplaceholder.typicode.com/users'
});

const query = new Query()
  .select(['id', 'name', 'email'])
  .take(5);

<ListViewComponent
  dataSource={data}
  query={query}
  fields={{ id: 'id', text: 'name' }}
/>
```

### Filter Results

```tsx
// WHERE clause
const query = new Query()
  .where('status', '==', 'active');

// Multiple conditions (AND)
const query = new Query()
  .where('category', '==', 'Electronics')
  .and('price', '<', 1000);

// OR condition
const query = new Query()
  .where('status', '==', 'active')
  .or('priority', '==', 'high');
```

### Search Filter Example

```tsx
import { useState } from 'react';
import { Query } from '@syncfusion/ej2-data';

export function FilteredListView() {
  const [searchText, setSearchText] = useState('');
  
  const data = new DataManager({
    url: 'https://api.example.com/items'
  });

  // Build dynamic query based on search
  let query = new Query();
  
  if (searchText) {
    query = query.where('name', 'startswith', searchText);
  }

  return (
    <div>
      <input
        type="text"
        placeholder="Search items..."
        value={searchText}
        onChange={(e) => setSearchText(e.target.value)}
        style={{ marginBottom: '10px', padding: '8px' }}
      />
      
      <ListViewComponent
        dataSource={data}
        query={query}
        fields={{ id: 'id', text: 'name' }}
      />
    </div>
  );
}
```

### Sort & Order

```tsx
const query = new Query()
  .sortBy('name', 'ascending')
  .take(10);

// Multiple sort
const query = new Query()
  .sortBy('category', 'ascending')
  .sortBy('name', 'ascending');
```

### Paging with Query

```tsx
const query = new Query()
  .skip(0)      // Start from record 0
  .take(10);    // Get 10 records

// For page 2 with 10 items per page:
const page = 2;
const pageSize = 10;
const skipCount = (page - 1) * pageSize;

const query = new Query()
  .skip(skipCount)
  .take(pageSize);
```

## Pagination

### Manual Pagination

```tsx
import { useState } from 'react';
import { ListViewComponent } from '@syncfusion/ej2-react-lists';

export function PaginatedListView() {
  const [currentPage, setCurrentPage] = useState(1);
  const itemsPerPage = 10;
  const totalItems = 100;

  const allData = Array.from({ length: totalItems }, (_, i) => ({
    id: (i + 1).toString(),
    text: `Item ${i + 1}`
  }));

  // Calculate paginated data
  const startIndex = (currentPage - 1) * itemsPerPage;
  const endIndex = startIndex + itemsPerPage;
  const paginatedData = allData.slice(startIndex, endIndex);

  const totalPages = Math.ceil(totalItems / itemsPerPage);

  return (
    <div>
      <ListViewComponent
        dataSource={paginatedData}
        fields={{ id: 'id', text: 'text' }}
        height="400px"
      />

      <div style={{ marginTop: '20px', textAlign: 'center' }}>
        <button
          disabled={currentPage === 1}
          onClick={() => setCurrentPage(currentPage - 1)}
        >
          Previous
        </button>

        <span style={{ margin: '0 10px' }}>
          Page {currentPage} of {totalPages}
        </span>

        <button
          disabled={currentPage === totalPages}
          onClick={() => setCurrentPage(currentPage + 1)}
        >
          Next
        </button>
      </div>
    </div>
  );
}
```

### Load More Pattern

```tsx
import { useState } from 'react';

export function LoadMoreListView() {
  const [items, setItems] = useState(
    Array.from({ length: 10 }, (_, i) => ({
      id: (i + 1).toString(),
      text: `Item ${i + 1}`
    }))
  );

  const handleLoadMore = () => {
    const nextItems = Array.from(
      { length: 10 },
      (_, i) => ({
        id: (items.length + i + 1).toString(),
        text: `Item ${items.length + i + 1}`
      })
    );
    setItems([...items, ...nextItems]);
  };

  return (
    <div>
      <ListViewComponent
        dataSource={items}
        fields={{ id: 'id', text: 'text' }}
        height="400px"
      />

      <button
        onClick={handleLoadMore}
        style={{ marginTop: '10px', width: '100%', padding: '10px' }}
      >
        Load More
      </button>
    </div>
  );
}
```

## Dynamic Data Updates

### Updating Entire DataSource

```tsx
import { useState } from 'react';
import { ListViewComponent } from '@syncfusion/ej2-react-lists';

export function DynamicListView() {
  const [data, setData] = useState([
    { id: '1', text: 'Item 1' },
    { id: '2', text: 'Item 2' }
  ]);

  const handleRefresh = () => {
    // Fetch new data
    const newData = [
      { id: '3', text: 'Item 3' },
      { id: '4', text: 'Item 4' },
      { id: '5', text: 'Item 5' }
    ];
    setData(newData);
  };

  return (
    <div>
      <button onClick={handleRefresh} style={{ marginBottom: '10px' }}>
        Refresh Data
      </button>

      <ListViewComponent
        dataSource={data}
        fields={{ id: 'id', text: 'text' }}
      />
    </div>
  );
}
```

### Adding Items Dynamically

```tsx
const handleAddItem = () => {
  const newData = [
    ...data,
    { id: Date.now().toString(), text: `New Item ${Date.now()}` }
  ];
  setData(newData);
};
```

### Updating Specific Item

```tsx
const handleUpdateItem = (itemId: string, newText: string) => {
  const updatedData = data.map(item =>
    item.id === itemId ? { ...item, text: newText } : item
  );
  setData(updatedData);
};
```

### Removing Items

```tsx
const handleRemoveItem = (itemId: string) => {
  const filteredData = data.filter(item => item.id !== itemId);
  setData(filteredData);
};
```

### Complete CRUD Example

```tsx
import { useState } from 'react';
import { ListViewComponent } from '@syncfusion/ej2-react-lists';

interface ListItem {
  id: string;
  text: string;
}

export function CRUDListView() {
  const [items, setItems] = useState<ListItem[]>([
    { id: '1', text: 'Task 1' },
    { id: '2', text: 'Task 2' }
  ]);

  const [inputValue, setInputValue] = useState('');

  // Create
  const add = () => {
    if (inputValue.trim()) {
      setItems([
        ...items,
        { id: Date.now().toString(), text: inputValue }
      ]);
      setInputValue('');
    }
  };

  // Read (items are displayed)
  // Update
  const update = (id: string) => {
    const newText = prompt('Enter new text:');
    if (newText) {
      setItems(items.map(item =>
        item.id === id ? { ...item, text: newText } : item
      ));
    }
  };

  // Delete
  const remove = (id: string) => {
    setItems(items.filter(item => item.id !== id));
  };

  return (
    <div style={{ maxWidth: '400px' }}>
      <div style={{ marginBottom: '10px' }}>
        <input
          value={inputValue}
          onChange={(e) => setInputValue(e.target.value)}
          onKeyPress={(e) => e.key === 'Enter' && add()}
          placeholder="Add new item..."
          style={{ width: '70%', padding: '5px' }}
        />
        <button onClick={add} style={{ marginLeft: '5px' }}>
          Add
        </button>
      </div>

      <ListViewComponent
        dataSource={items}
        fields={{ id: 'id', text: 'text' }}
        height="300px"
      />

      <div style={{ marginTop: '10px' }}>
        {items.map(item => (
          <div key={item.id} style={{ marginBottom: '5px' }}>
            <button onClick={() => update(item.id)} style={{ marginRight: '5px' }}>
              Edit
            </button>
            <button onClick={() => remove(item.id)}>
              Delete
            </button>
          </div>
        ))}
      </div>
    </div>
  );
}
```

## Performance Tips

- Use `DataManager` with remote data to avoid loading all items at once
- Enable `enableVirtualization` for large datasets
- Use `Query().take(n)` to limit initial data
- Implement pagination for better UX with large datasets
- Avoid frequent re-renders by using `useMemo` for field mappings

