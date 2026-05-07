# Item Management (CRUD Operations)

## Table of Contents
- [Adding Items](#adding-items)
- [Removing Items](#removing-items)
- [Finding Items](#finding-items)
- [Updating Items](#updating-items)
- [Batch Operations](#batch-operations)
- [Managing Nested Items](#managing-nested-items)

## Adding Items

### Add Single Item

```tsx
import { useRef } from 'react';
import { ListViewComponent } from '@syncfusion/ej2-react-lists';

export function AddItemExample() {
  const listViewRef = useRef<ListViewComponent>(null);

  const data = [
    { id: '1', text: 'Item 1' },
    { id: '2', text: 'Item 2' }
  ];

  const handleAddItem = () => {
    const newItem = {
      id: Date.now().toString(),
      text: `New Item ${Date.now()}`
    };

    if (listViewRef.current) {
      listViewRef.current.addItem([newItem]);
    }
  };

  return (
    <div>
      <button onClick={handleAddItem}>Add Item</button>
      <ListViewComponent
        ref={listViewRef}
        dataSource={data}
        fields={{ id: 'id', text: 'text' }}
      />
    </div>
  );
}
```

### Add with Object Properties

```tsx
interface Product {
  id: string;
  name: string;
  price: number;
  category: string;
}

const handleAddProduct = () => {
  const newProduct: Product = {
    id: `prod-${Date.now()}`,
    name: 'New Product',
    price: 99.99,
    category: 'Electronics'
  };

  listViewRef.current.addItem([newProduct]);
};
```

### Add Item at Specific Position

```tsx
// ListView doesn't support "addAt" position, but you can:
// 1. Manipulate state and update dataSource
// 2. Use removeItem to remove, then addItem to insert

const handleInsertAtPosition = (index: number) => {
  const allData = [...data];
  const newItem = { id: `id-${Date.now()}`, text: 'Inserted Item' };
  
  // Insert at position
  allData.splice(index, 0, newItem);
  
  // Update state (requires managed state)
  setData(allData);
};
```

### Add Multiple Items

```tsx
const handleAddMultipleItems = () => {
  const newItems = [
    { id: '10', text: 'Item 10' },
    { id: '11', text: 'Item 11' },
    { id: '12', text: 'Item 12' }
  ];

  if (listViewRef.current) {
    listViewRef.current.addItem(newItems);  // Pass array
  }
};
```

### Validate Before Adding

```tsx
const handleAddValidated = (itemText: string) => {
  // Validation
  if (!itemText || itemText.trim() === '') {
    alert('Item text cannot be empty');
    return;
  }

  if (itemText.length > 100) {
    alert('Item text too long');
    return;
  }

  // Check for duplicates
  const isDuplicate = data.some(item => item.text === itemText);
  if (isDuplicate) {
    alert('Item already exists');
    return;
  }

  // Add item
  const newItem = {
    id: Date.now().toString(),
    text: itemText
  };

  listViewRef.current.addItem([newItem]);
};
```

## Removing Items

### Remove by Reference

```tsx
const handleRemoveItem = () => {
  // Get the selected item
  const selected = listViewRef.current.getSelectedItems();
  
  if (selected) {
    listViewRef.current.removeItem(selected.element);
  }
};
```

### Remove by Element

```tsx
const handleRemoveByElement = (itemId: string) => {
  // Get element by id
  const element = document.getElementById(itemId);
  
  if (element) {
    listViewRef.current.removeItem(element);
  }
};
```

### Remove by Fields

```tsx
const handleRemoveByFields = (itemId: string, itemText: string) => {
  const itemFields = {
    id: itemId,
    text: itemText
  };

  listViewRef.current.removeItem(itemFields);
};
```

### Remove Multiple Items

```tsx
const handleRemoveMultiple = () => {
  const itemsToRemove = [
    { id: '1', text: 'Item 1' },
    { id: '2', text: 'Item 2' }
  ];

  itemsToRemove.forEach(item => {
    listViewRef.current.removeItem(item);
  });
};
```

### Remove All Items

```tsx
const handleRemoveAllItems = () => {
  if (confirm('Remove all items?')) {
    data.forEach(item => {
      listViewRef.current.removeItem(item);
    });
  }
};
```

### Remove with Confirmation

```tsx
const handleRemoveWithConfirm = () => {
  const selected = listViewRef.current.getSelectedItems();
  
  if (selected && confirm(`Remove "${selected.text}"?`)) {
    listViewRef.current.removeItem(selected.element);
  } else if (!selected) {
    alert('No item selected');
  }
};
```

## Finding Items

### Find Item by Reference

```tsx
const handleFindItem = (itemId: string) => {
  const itemFields = { id: itemId };
  const foundItem = listViewRef.current.findItem(itemFields);
  
  console.log('Found:', foundItem);
  // Returns: { id, text, element, data }
};
```

### Find in Current Data

```tsx
const handleSearchInData = (searchText: string) => {
  const found = data.find(item => item.text === searchText);
  
  if (found) {
    console.log('Found item:', found);
  } else {
    console.log('Item not found');
  }
};
```

### Find Multiple Matching Items

```tsx
const handleSearchMultiple = (searchText: string) => {
  const matches = data.filter(item =>
    item.text.toLowerCase().includes(searchText.toLowerCase())
  );
  
  console.log('Matching items:', matches);
};
```

### Get Item Index

```tsx
const handleGetItemIndex = (itemId: string) => {
  const index = data.findIndex(item => item.id === itemId);
  
  if (index !== -1) {
    console.log(`Item at index: ${index}`);
  } else {
    console.log('Item not found');
  }
};
```

## Updating Items

### Update Item Text

```tsx
import { useState } from 'react';

export function UpdateItemExample() {
  const [data, setData] = useState([
    { id: '1', text: 'Item 1' },
    { id: '2', text: 'Item 2' }
  ]);

  const handleUpdateItem = (itemId: string, newText: string) => {
    const updated = data.map(item =>
      item.id === itemId ? { ...item, text: newText } : item
    );
    setData(updated);
  };

  return (
    <div>
      <button onClick={() => handleUpdateItem('1', 'Updated Item 1')}>
        Update Item 1
      </button>
      <ListViewComponent dataSource={data} />
    </div>
  );
}
```

### Update with Dialog

```tsx
const handleUpdateWithDialog = (itemId: string) => {
  const oldItem = data.find(item => item.id === itemId);
  
  if (!oldItem) return;
  
  const newText = prompt('Edit item:', oldItem.text);
  
  if (newText !== null && newText.trim() !== '') {
    const updated = data.map(item =>
      item.id === itemId ? { ...item, text: newText } : item
    );
    setData(updated);
  }
};
```

### Update Multiple Fields

```tsx
interface Item {
  id: string;
  text: string;
  category?: string;
  isActive?: boolean;
}

const handleUpdateMultipleFields = (itemId: string, updates: Partial<Item>) => {
  const updated: Item[] = data.map(item =>
    item.id === itemId ? { ...item, ...updates } : item
  );
  setData(updated);
};

// Usage:
handleUpdateMultipleFields('1', {
  text: 'Updated Text',
  category: 'NewCategory',
  isActive: false
});
```

### Refresh After Update

```tsx
const handleUpdateAndRefresh = (itemId: string, newText: string) => {
  // Update the item
  const updated = data.map(item =>
    item.id === itemId ? { ...item, text: newText } : item
  );
  setData(updated);
  
  // Refresh ListView if needed
  listViewRef.current?.refresh?.();
};
```

## Batch Operations

### Add Multiple & Remove Multiple

```tsx
const handleBatchUpdate = async () => {
  // Remove old items
  const itemsToRemove = [
    { id: '1', text: 'Item 1' },
    { id: '2', text: 'Item 2' }
  ];

  itemsToRemove.forEach(item => {
    listViewRef.current.removeItem(item);
  });

  // Add new items
  const newItems = [
    { id: '10', text: 'New Item 1' },
    { id: '11', text: 'New Item 2' }
  ];

  listViewRef.current.addItem(newItems);
};
```

### Bulk Update

```tsx
const handleBulkUpdate = (updates: Record<string, string>) => {
  const updated = data.map(item =>
    updates[item.id] ? { ...item, text: updates[item.id] } : item
  );
  setData(updated);
};

// Usage:
handleBulkUpdate({
  '1': 'Updated Item 1',
  '2': 'Updated Item 2',
  '3': 'Updated Item 3'
});
```

### Clear & Reload

```tsx
const handleClearAndReload = async () => {
  // Get all current items
  const allItems = data;
  
  // Remove each item
  allItems.forEach(item => {
    listViewRef.current.removeItem(item);
  });
  
  // Fetch new data and add
  const newData = await fetchItemsFromAPI();
  listViewRef.current.addItem(newData);
};
```

## Managing Nested Items

### Add Nested Item

```tsx
interface NestedItem {
  id: string;
  text: string;
  child?: NestedItem[];
}

const handleAddNestedItem = (parentId: string) => {
  const newNestedItem: NestedItem = {
    id: `nested-${Date.now()}`,
    text: 'Nested Item'
  };

  // Find parent
  const parent = findItemRecursive(data, parentId);
  
  if (parent) {
    if (!parent.child) {
      parent.child = [];
    }
    parent.child.push(newNestedItem);
    setData([...data]);  // Trigger re-render
  }
};

const findItemRecursive = (items: NestedItem[], id: string): NestedItem | null => {
  for (const item of items) {
    if (item.id === id) return item;
    if (item.child) {
      const found = findItemRecursive(item.child, id);
      if (found) return found;
    }
  }
  return null;
};
```

### Remove Nested Item

```tsx
const handleRemoveNestedItem = (parentId: string, childId: string) => {
  const parent = findItemRecursive(data, parentId);
  
  if (parent && parent.child) {
    parent.child = parent.child.filter(child => child.id !== childId);
    setData([...data]);
  }
};
```

## Complete Item Management Example

```tsx
import { useState, useRef } from 'react';
import { ListViewComponent } from '@syncfusion/ej2-react-lists';

interface Item {
  id: string;
  text: string;
}

export function ItemManagementComplete() {
  const [items, setItems] = useState<Item[]>([
    { id: '1', text: 'Item 1' },
    { id: '2', text: 'Item 2' }
  ]);

  const [inputValue, setInputValue] = useState('');
  const listViewRef = useRef<ListViewComponent>(null);

  // Create
  const addItem = () => {
    if (inputValue.trim()) {
      const newItem: Item = {
        id: Date.now().toString(),
        text: inputValue
      };
      setItems([...items, newItem]);
      setInputValue('');
    }
  };

  // Update
  const updateItem = (id: string) => {
    const newText = prompt('Edit item:');
    if (newText) {
      setItems(items.map(item =>
        item.id === id ? { ...item, text: newText } : item
      ));
    }
  };

  // Delete
  const deleteItem = (id: string) => {
    setItems(items.filter(item => item.id !== id));
  };

  // Find
  const searchItem = (searchText: string) => {
    const found = items.filter(item =>
      item.text.toLowerCase().includes(searchText.toLowerCase())
    );
    console.log('Found:', found);
  };

  return (
    <div style={{ maxWidth: '500px' }}>
      <div style={{ marginBottom: '15px' }}>
        <input
          value={inputValue}
          onChange={(e) => setInputValue(e.target.value)}
          onKeyPress={(e) => e.key === 'Enter' && addItem()}
          placeholder="Enter new item..."
          style={{ padding: '8px', width: '70%' }}
        />
        <button onClick={addItem} style={{ marginLeft: '5px' }}>
          Add
        </button>
      </div>

      <ListViewComponent
        ref={listViewRef}
        dataSource={items}
        fields={{ id: 'id', text: 'text' }}
        height="300px"
      />

      <div style={{ marginTop: '15px' }}>
        {items.map(item => (
          <div key={item.id} style={{ marginBottom: '8px' }}>
            <span style={{ marginRight: '10px' }}>{item.text}</span>
            <button onClick={() => updateItem(item.id)} style={{ marginRight: '5px' }}>
              Edit
            </button>
            <button onClick={() => deleteItem(item.id)}>
              Delete
            </button>
          </div>
        ))}
      </div>
    </div>
  );
}
```

