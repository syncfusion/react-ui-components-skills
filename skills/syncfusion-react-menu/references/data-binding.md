# Data Binding in React Menu Component

## Table of Contents
- [Data Binding Overview](#data-binding-overview)
- [Hierarchical Data Binding](#hierarchical-data-binding)
- [Self-Referential Data Binding](#self-referential-data-binding)
- [FieldSettingsModel Configuration](#fieldsettingsmodel-configuration)
- [JSON Data Mapping](#json-data-mapping)
- [Complex Nested Hierarchies](#complex-nested-hierarchies)
- [Dynamic Data Updates](#dynamic-data-updates)

## Data Binding Overview

The Menu component supports two primary data binding approaches:

1. **Hierarchical Data:** Direct nested structure with parent-child relationships
2. **Self-Referential Data:** Flat structure with ID/parent ID relationships

Both methods map custom data properties to Menu properties using `FieldSettingsModel`.

## Hierarchical Data Binding

Hierarchical data uses nested arrays to represent menu levels. This is the most intuitive approach for organizing menu structures.

### Basic Hierarchical Example

```jsx
import { FieldSettingsModel, MenuComponent } from '@syncfusion/ej2-react-navigations';
import * as React from 'react';
import './App.css';

function App() {
  // Data source with nested structure
  const dataSource = [
    {
      continent: 'Asia',
      countries: [
        { country: 'China', capital: 'Beijing' },
        { country: 'India', capital: 'New Delhi' },
        { country: 'Japan', capital: 'Tokyo' }
      ]
    },
    {
      continent: 'North America',
      countries: [
        { country: 'Canada', capital: 'Ottawa' },
        { country: 'USA', capital: 'Washington' },
        { country: 'Mexico', capital: 'Mexico City' }
      ]
    },
    {
      continent: 'Europe',
      countries: [
        { country: 'France', capital: 'Paris' },
        { country: 'Germany', capital: 'Berlin' },
        { country: 'UK', capital: 'London' }
      ]
    }
  ];

  // Field mapping configuration
  const fields: FieldSettingsModel = {
    text: 'continent',
    children: 'countries'
  };

  return (
    <MenuComponent items={dataSource} fields={fields}></MenuComponent>
  );
}

export default App;
```

### Multi-Level Hierarchical Structure

```jsx
const dataSource = [
  {
    continent: 'Asia',
    countries: [
      {
        country: 'China',
        cities: [
          { city: 'Beijing' },
          { city: 'Shanghai' },
          { city: 'Guangzhou' }
        ]
      },
      {
        country: 'India',
        cities: [
          { city: 'Delhi' },
          { city: 'Mumbai' },
          { city: 'Bangalore' }
        ]
      }
    ]
  }
];

const fields: FieldSettingsModel = {
  text: 'continent',
  children: 'countries'
};

// For the second level, use nested fields configuration
const countriesFields: FieldSettingsModel = {
  text: 'country',
  children: 'cities'
};
```

## Self-Referential Data Binding

Self-referential data uses a flat structure where parent-child relationships are defined by ID/parent ID fields. This is useful for database-driven menus.

### Self-Referential Structure

```jsx
const dataSource = [
  {
    id: 1,
    text: 'File',
    parentId: null,
    iconCss: 'e-icons e-file'
  },
  {
    id: 2,
    text: 'New',
    parentId: 1,
    iconCss: 'e-icons e-new'
  },
  {
    id: 3,
    text: 'Open',
    parentId: 1,
    iconCss: 'e-icons e-open'
  },
  {
    id: 4,
    text: 'Save',
    parentId: 1,
    iconCss: 'e-icons e-save'
  },
  {
    id: 5,
    text: 'Edit',
    parentId: null,
    iconCss: 'e-icons e-edit'
  },
  {
    id: 6,
    text: 'Cut',
    parentId: 5,
    iconCss: 'e-icons e-cut'
  },
  {
    id: 7,
    text: 'Copy',
    parentId: 5,
    iconCss: 'e-icons e-copy'
  }
];

const fields: FieldSettingsModel = {
  text: 'text',
  parentId: 'parentId',
  children: 'children',
  iconCss: 'iconCss'
};

<MenuComponent items={dataSource} fields={fields}></MenuComponent>
```

## FieldSettingsModel Configuration

FieldSettingsModel maps custom data properties to Menu properties:

```jsx
interface FieldSettingsModel {
  // Property name for display text
  text?: string;

  // Property name for sub-items (hierarchical)
  children?: string;

  // Property name for parent ID (self-referential)
  parentId?: string;

  // Property name for item ID (self-referential)
  itemId?: string;

  // Property name for icon CSS class
  iconCss?: string;

  // Property name for URL (navigation link)
  url?: string;

  // Property name for separator flag
  separator?: string;

  // Property name for disabled flag
  disabled?: string;

  // Property name for hidden flag
  hidden?: string;

  // Property name for HTML attributes
  htmlAttributes?: string;
}
```

### Mapping with Custom Property Names

```jsx
// Data source with custom property names
const dataSource = [
  {
    label: 'File',           // Custom text property
    subItems: [              // Custom children property
      { label: 'New', icon: 'file-icon', isDisabled: false }
    ]
  }
];

// Map custom properties
const fields: FieldSettingsModel = {
  text: 'label',             // Maps 'label' to menu text
  children: 'subItems',      // Maps 'subItems' to children
  iconCss: 'icon',           // Maps 'icon' to iconCss
  disabled: 'isDisabled'     // Maps 'isDisabled' to disabled
};

<MenuComponent items={dataSource} fields={fields}></MenuComponent>
```

## JSON Data Mapping

Complete example with comprehensive JSON data mapping:

```jsx
import { FieldSettingsModel, MenuComponent } from '@syncfusion/ej2-react-navigations';
import * as React from 'react';

function App() {
  // Comprehensive JSON data source
  const dataSource = [
    {
      menuId: 1,
      menuText: 'File',
      menuParent: null,
      menuIcon: 'e-icons e-file',
      submenus: [
        {
          menuId: 2,
          menuText: 'New',
          menuParent: 1,
          menuIcon: 'e-icons e-new'
        },
        {
          menuId: 3,
          menuText: 'Open',
          menuParent: 1,
          menuIcon: 'e-icons e-open'
        },
        {
          menuId: 4,
          menuText: 'Save',
          menuParent: 1,
          menuIcon: 'e-icons e-save'
        }
      ]
    },
    {
      menuId: 5,
      menuText: 'Edit',
      menuParent: null,
      menuIcon: 'e-icons e-edit',
      submenus: [
        {
          menuId: 6,
          menuText: 'Cut',
          menuParent: 5,
          menuIcon: 'e-icons e-cut'
        },
        {
          menuId: 7,
          menuText: 'Copy',
          menuParent: 5,
          menuIcon: 'e-icons e-copy'
        },
        {
          menuId: 8,
          menuText: 'Paste',
          menuParent: 5,
          menuIcon: 'e-icons e-paste'
        }
      ]
    }
  ];

  // Field mapping
  const fields: FieldSettingsModel = {
    text: 'menuText',
    children: 'submenus',
    iconCss: 'menuIcon',
    itemId: 'menuId',
    parentId: 'menuParent'
  };

  return (
    <MenuComponent 
      items={dataSource} 
      fields={fields}
    ></MenuComponent>
  );
}

export default App;
```

## Complex Nested Hierarchies

Example with deeply nested menu structures (3+ levels):

```jsx
const dataSource = [
  {
    category: 'Products',
    items: [
      {
        subcategory: 'Electronics',
        products: [
          {
            productName: 'Laptops',
            models: [
              { model: 'XPS 15' },
              { model: 'ThinkPad X1' }
            ]
          },
          {
            productName: 'Phones',
            models: [
              { model: 'iPhone 14' },
              { model: 'Galaxy S23' }
            ]
          }
        ]
      },
      {
        subcategory: 'Accessories',
        products: [
          {
            productName: 'Cables',
            models: [
              { model: 'USB-C' },
              { model: 'HDMI' }
            ]
          }
        ]
      }
    ]
  }
];

// Handle multiple nesting levels with recursive field mapping
const fields: FieldSettingsModel = {
  text: 'category',
  children: 'items'
};
```

## Dynamic Data Updates

Update menu data dynamically without recreating the component:

```jsx
import { MenuComponent } from '@syncfusion/ej2-react-navigations';
import * as React from 'react';

function App() {
  let menuObj: MenuComponent;
  const [items, setItems] = React.useState([
    {
      text: 'File',
      items: [
        { text: 'New' },
        { text: 'Open' }
      ]
    }
  ]);

  // Update menu items
  const addNewCategory = () => {
    const newItems = [...items, {
      text: 'New Category',
      items: [
        { text: 'Item 1' },
        { text: 'Item 2' }
      ]
    }];
    setItems(newItems);
  };

  // Modify existing data
  const modifyMenuItem = () => {
    const updatedItems = [...items];
    updatedItems[0].items.push({ text: 'Save' });
    setItems(updatedItems);
  };

  return (
    <div>
      <button onClick={addNewCategory}>Add Category</button>
      <button onClick={modifyMenuItem}>Add Save Option</button>
      <MenuComponent 
        ref={(menu) => (menuObj = menu)}
        items={items}
      ></MenuComponent>
    </div>
  );
}

export default App;
```

### Real-Time Data from API

```jsx
function App() {
  const [menuData, setMenuData] = React.useState([]);

  React.useEffect(() => {
    // Fetch menu structure from API
    fetch('/api/menu-structure')
      .then(res => res.json())
      .then(data => {
        // Transform API response if needed
        const transformedData = data.map(item => ({
          text: item.name,
          items: item.children
        }));
        setMenuData(transformedData);
      });
  }, []);

  return <MenuComponent items={menuData}></MenuComponent>;
}
```

## Related Topics
- [Menu Items Customization](menu-items-customization.md) - Add/remove items dynamically
- [Events and Interactions](events-and-interactions.md) - Respond to data changes
