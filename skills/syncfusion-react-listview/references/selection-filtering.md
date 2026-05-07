# Selection & Filtering

## Table of Contents
- [Single Item Selection](#single-item-selection)
- [Multiple Item Selection](#multiple-item-selection)
- [Checkbox Selection](#checkbox-selection)
- [Selection Events](#selection-events)
- [Programmatic Selection](#programmatic-selection)
- [Filtering Data](#filtering-data)
- [Search Functionality](#search-functionality)

## Single Item Selection

### Basic Selection

```tsx
import { ListViewComponent } from '@syncfusion/ej2-react-lists';
import { useState } from 'react';

export function SingleSelectExample() {
  const [selectedItem, setSelectedItem] = useState<any>(null);

  const data = [
    { id: '1', text: 'Item 1' },
    { id: '2', text: 'Item 2' },
    { id: '3', text: 'Item 3' }
  ];

  const handleSelect = (args: any) => {
    setSelectedItem(args);
    console.log('Selected:', args.text);
  };

  return (
    <div>
      <ListViewComponent
        dataSource={data}
        fields={{ id: 'id', text: 'text' }}
        select={handleSelect}
      />
      {selectedItem && <p>Selected: {selectedItem.text}</p>}
    </div>
  );
}
```

### Disable Selection

```tsx
<ListViewComponent
  dataSource={data}
  fields={{ id: 'id', text: 'text' }}
  select={(args) => {
    // Prevent selection for certain items
    if (args.id === 'restricted-id') {
      args.preventDefault?.();
    }
  }}
/>
```

## Multiple Item Selection

### Enable Multiple Selection

```tsx
export function MultiSelectExample() {
  const [selectedItems, setSelectedItems] = useState<any[]>([]);

  const handleSelect = (args: any) => {
    if (args.isChecked === undefined) {
      // Toggle selection
      setSelectedItems(prev => {
        const isSelected = prev.find(item => item.id === args.id);
        if (isSelected) {
          return prev.filter(item => item.id !== args.id);
        } else {
          return [...prev, args];
        }
      });
    }
  };

  return (
    <div>
      <ListViewComponent
        dataSource={data}
        fields={{ id: 'id', text: 'text' }}
        select={handleSelect}
      />
      <p>Selected: {selectedItems.map(item => item.text).join(', ')}</p>
    </div>
  );
}
```

### Get Selected Items

```tsx
const handleGetSelection = () => {
  const selected = listViewRef.current.getSelectedItems();
  console.log('Selected items:', selected);
};
```

## Checkbox Selection

### Show Checkboxes

```tsx
<ListViewComponent
  dataSource={data}
  fields={{ id: 'id', text: 'text' }}
  showCheckBox={true}
/>
```

### Checkbox Position

```tsx
// Left (default)
<ListViewComponent
  showCheckBox={true}
  checkBoxPosition="Left"
/>

// Right
<ListViewComponent
  showCheckBox={true}
  checkBoxPosition="Right"
/>
```

### Checkbox with Data Binding

```tsx
interface Item {
  id: string;
  text: string;
  checked?: boolean;
}

const data: Item[] = [
  { id: '1', text: 'Item 1', checked: false },
  { id: '2', text: 'Item 2', checked: true },
  { id: '3', text: 'Item 3', checked: false }
];

<ListViewComponent
  dataSource={data}
  fields={{
    id: 'id',
    text: 'text',
    isChecked: 'checked'
  }}
  showCheckBox={true}
/>
```

### Check/Uncheck Items

```tsx
const handleCheckItem = (itemId: string) => {
  const item = { id: itemId, text: 'Item' };
  listViewRef.current.checkItem(item);
};

const handleUncheckItem = (itemId: string) => {
  const item = { id: itemId, text: 'Item' };
  listViewRef.current.uncheckItem(item);
};

const handleCheckAll = () => {
  listViewRef.current.checkAllItems();
};

const handleUncheckAll = () => {
  listViewRef.current.uncheckAllItems();
};
```

### Get Checked Items

```tsx
const handleGetChecked = () => {
  const selected = listViewRef.current.getSelectedItems();
  console.log('Checked items:', selected);
};
```

## Selection Events

### Select Event

```tsx
const handleSelect = (args: any) => {
  console.log('Event triggered:', {
    id: args.id,
    text: args.text,
    element: args.element,
    isChecked: args.isChecked,
    data: args.data
  });
};

<ListViewComponent
  dataSource={data}
  select={handleSelect}
/>
```

### Action Begin/Complete Events

```tsx
const handleActionBegin = (args: any) => {
  console.log('Action started:', args);
};

const handleActionComplete = (args: any) => {
  console.log('Action completed:', args);
};

const handleActionFailure = (args: any) => {
  console.log('Action failed:', args);
};

<ListViewComponent
  dataSource={data}
  actionBegin={handleActionBegin}
  actionComplete={handleActionComplete}
  actionFailure={handleActionFailure}
/>
```

### Scroll Event

```tsx
const handleScroll = (args: any) => {
  console.log('Scrolled:', args);
  if (args.distanceY > 500) {
    // Load more items
    console.log('Reached bottom');
  }
};

<ListViewComponent
  dataSource={data}
  scroll={handleScroll}
/>
```

## Programmatic Selection

### Select Item by ID

```tsx
const handleSelectById = (itemId: string) => {
  const item = { id: itemId };
  listViewRef.current.selectItem(item);
};
```

### Select Item by Element

```tsx
const handleSelectByElement = (itemId: string) => {
  const element = document.querySelector(`[data-id="${itemId}"]`);
  if (element) {
    listViewRef.current.selectItem(element);
  }
};
```

### Select Multiple Items

```tsx
const handleSelectMultiple = (itemIds: string[]) => {
  const items = itemIds.map(id => ({
    id: id,
    text: 'Item'
  }));
  
  listViewRef.current.selectMultipleItems(items);
};
```

### Unselect Item

```tsx
const handleUnselectItem = (itemId: string) => {
  const item = { id: itemId };
  listViewRef.current.unselectItem(item);
};

// Unselect all
const handleUnselectAll = () => {
  listViewRef.current.unselectItem();
};
```

## Filtering Data

### Client-Side Filtering

```tsx
import { useState } from 'react';

export function FilteredListView() {
  const [filteredData, setFilteredData] = useState(data);
  const [filterText, setFilterText] = useState('');

  const handleFilter = (text: string) => {
    setFilterText(text);
    
    if (text.trim() === '') {
      setFilteredData(data);
    } else {
      const filtered = data.filter(item =>
        item.text.toLowerCase().includes(text.toLowerCase())
      );
      setFilteredData(filtered);
    }
  };

  return (
    <div>
      <input
        type="text"
        placeholder="Filter..."
        value={filterText}
        onChange={(e) => handleFilter(e.target.value)}
        style={{ marginBottom: '10px', padding: '8px' }}
      />
      
      <ListViewComponent
        dataSource={filteredData}
        fields={{ id: 'id', text: 'text' }}
      />
    </div>
  );
}
```

### Filter by Property

```tsx
const handleFilterByCategory = (category: string) => {
  const filtered = data.filter(item => item.category === category);
  setFilteredData(filtered);
};

// Usage:
handleFilterByCategory('Electronics');
```

### Advanced Filtering

```tsx
const handleAdvancedFilter = (filters: Record<string, any>) => {
  const filtered = data.filter(item => {
    for (const [key, value] of Object.entries(filters)) {
      if (item[key] !== value) return false;
    }
    return true;
  });
  setFilteredData(filtered);
};

// Usage:
handleAdvancedFilter({
  category: 'Electronics',
  inStock: true
});
```

## Search Functionality

### Real-Time Search

```tsx
export function SearchListView() {
  const [searchText, setSearchText] = useState('');
  const [filteredData, setFilteredData] = useState(data);

  const handleSearch = (text: string) => {
    setSearchText(text);
    
    if (!text) {
      setFilteredData(data);
      return;
    }

    const results = data.filter(item =>
      item.text.toLowerCase().includes(text.toLowerCase()) ||
      item.description?.toLowerCase().includes(text.toLowerCase())
    );
    
    setFilteredData(results);
  };

  return (
    <div>
      <div style={{ marginBottom: '10px' }}>
        <input
          type="text"
          placeholder="Search items..."
          value={searchText}
          onChange={(e) => handleSearch(e.target.value)}
          style={{ padding: '8px', width: '100%' }}
        />
        {searchText && (
          <p style={{ fontSize: '12px', color: '#666' }}>
            Found {filteredData.length} results
          </p>
        )}
      </div>

      <ListViewComponent
        dataSource={filteredData}
        fields={{ id: 'id', text: 'text' }}
        height="400px"
      />
    </div>
  );
}
```

### Case-Insensitive Search

```tsx
const handleCaseInsensitiveSearch = (searchText: string) => {
  const lowerSearch = searchText.toLowerCase();
  
  const filtered = data.filter(item =>
    item.text.toLowerCase().includes(lowerSearch)
  );
  
  setFilteredData(filtered);
};
```

### Partial Match Search

```tsx
// Matches beginning
const searchBeginning = (text: string) =>
  data.filter(item => item.text.toLowerCase().startsWith(text.toLowerCase()));

// Matches end
const searchEnd = (text: string) =>
  data.filter(item => item.text.toLowerCase().endsWith(text.toLowerCase()));

// Matches anywhere
const searchAnywhere = (text: string) =>
  data.filter(item => item.text.toLowerCase().includes(text.toLowerCase()));

// Exact match
const searchExact = (text: string) =>
  data.filter(item => item.text === text);

// Regex match
const searchRegex = (pattern: string) =>
  data.filter(item => new RegExp(pattern, 'i').test(item.text));
```

### Highlight Search Results

```tsx
export function HighlightedSearchListView() {
  const [searchText, setSearchText] = useState('');
  const [filteredData, setFilteredData] = useState(data);

  const handleSearch = (text: string) => {
    setSearchText(text);
    setFilteredData(
      text ? data.filter(item =>
        item.text.toLowerCase().includes(text.toLowerCase())
      ) : data
    );
  };

  const highlightTemplate = (props: any) => {
    if (!searchText) return <span>{props.text}</span>;

    const parts = props.text.split(
      new RegExp(`(${searchText})`, 'gi')
    );

    return (
      <span>
        {parts.map((part: string, index: number) =>
          part.toLowerCase() === searchText.toLowerCase() ? (
            <mark key={index}>{part}</mark>
          ) : (
            <span key={index}>{part}</span>
          )
        )}
      </span>
    );
  };

  return (
    <div>
      <input
        type="text"
        placeholder="Search..."
        value={searchText}
        onChange={(e) => handleSearch(e.target.value)}
        style={{ marginBottom: '10px', padding: '8px', width: '100%' }}
      />

      <ListViewComponent
        dataSource={filteredData}
        fields={{ id: 'id', text: 'text' }}
        template={highlightTemplate}
      />
    </div>
  );
}
```

### Autocomplete Search

```tsx
export function AutocompleteSearchListView() {
  const [searchText, setSearchText] = useState('');
  const [suggestions, setSuggestions] = useState<string[]>([]);

  const handleSearch = (text: string) => {
    setSearchText(text);

    if (text.length > 0) {
      const matches = data
        .map(item => item.text)
        .filter(text => text.toLowerCase().startsWith(searchText.toLowerCase()))
        .slice(0, 5);  // Top 5 suggestions
      
      setSuggestions(matches);
    } else {
      setSuggestions([]);
    }
  };

  return (
    <div style={{ position: 'relative', marginBottom: '20px' }}>
      <input
        type="text"
        placeholder="Search..."
        value={searchText}
        onChange={(e) => handleSearch(e.target.value)}
        style={{ padding: '8px', width: '100%' }}
      />

      {suggestions.length > 0 && (
        <ul style={{
          position: 'absolute',
          top: '40px',
          left: 0,
          right: 0,
          border: '1px solid #ccc',
          listStyle: 'none',
          padding: 0,
          margin: 0,
          background: 'white'
        }}>
          {suggestions.map(suggestion => (
            <li
              key={suggestion}
              onClick={() => setSearchText(suggestion)}
              style={{ padding: '8px', cursor: 'pointer' }}
            >
              {suggestion}
            </li>
          ))}
        </ul>
      )}
    </div>
  );
}
```

## Complete Selection & Filtering Example

```tsx
import { useState, useRef } from 'react';
import { ListViewComponent } from '@syncfusion/ej2-react-lists';

interface Item {
  id: string;
  text: string;
  category: string;
}

export function CompleteSelectionFiltering() {
  const [selectedItems, setSelectedItems] = useState<Item[]>([]);
  const [searchText, setSearchText] = useState('');
  const [filterCategory, setFilterCategory] = useState('');
  const listViewRef = useRef<ListViewComponent>(null);

  const data: Item[] = [
    { id: '1', text: 'Laptop', category: 'Electronics' },
    { id: '2', text: 'Desk', category: 'Furniture' },
    { id: '3', text: 'Mouse', category: 'Electronics' }
  ];

  // Filter data
  let filtered = data;
  
  if (searchText) {
    filtered = filtered.filter(item =>
      item.text.toLowerCase().includes(searchText.toLowerCase())
    );
  }
  
  if (filterCategory) {
    filtered = filtered.filter(item => item.category === filterCategory);
  }

  const handleSelect = (args: any) => {
    setSelectedItems([...selectedItems, args]);
  };

  return (
    <div style={{ maxWidth: '500px' }}>
      <div style={{ marginBottom: '15px' }}>
        <input
          type="text"
          placeholder="Search..."
          value={searchText}
          onChange={(e) => setSearchText(e.target.value)}
          style={{ padding: '8px', marginBottom: '10px', width: '100%' }}
        />

        <select
          value={filterCategory}
          onChange={(e) => setFilterCategory(e.target.value)}
          style={{ padding: '8px', width: '100%' }}
        >
          <option value="">All Categories</option>
          <option value="Electronics">Electronics</option>
          <option value="Furniture">Furniture</option>
        </select>
      </div>

      <ListViewComponent
        ref={listViewRef}
        dataSource={filtered}
        fields={{ id: 'id', text: 'text' }}
        select={handleSelect}
        height="300px"
      />

      <div style={{ marginTop: '15px', padding: '10px', background: '#f5f5f5' }}>
        <p><strong>Selected Items:</strong></p>
        {selectedItems.length === 0 ? (
          <p>No items selected</p>
        ) : (
          <ul>
            {selectedItems.map(item => (
              <li key={item.id}>{item.text} ({item.category})</li>
            ))}
          </ul>
        )}
      </div>
    </div>
  );
}
```

