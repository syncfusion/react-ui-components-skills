# Advanced Features & Template Configuration

## Table of Contents
- [Template Configuration](#template-configuration)
- [Item-Wise Custom Templates](#item-wise-custom-templates)
- [Stateless Templates](#stateless-templates)
- [Toggle Buttons](#toggle-buttons)
- [Link Items](#link-items)
- [Tooltips](#tooltips)
- [Rendering Other Components](#rendering-other-components)
- [Command Customization](#command-customization)
- [Dynamic Item Management](#dynamic-item-management)
- [Event Handling](#event-handling)
- [Scroll Step Customization](#scroll-step-customization)

---

## Template Configuration

### Overview

Templates allow you to customize the appearance and content of toolbar items beyond basic buttons. Templates provide complete control over HTML structure, styling, and content.

### Template Basics

Templates are defined as functions or JSX components:

```jsx
const customTemplate = () => {
  return (
    <div className="custom-item">
      Custom Content Here
    </div>
  );
};

<ItemDirective template={customTemplate} />
```

### Template Properties

```jsx
<ItemDirective
  template={customTemplate}     // Function returning JSX
  type="Button"                 // Item type
  align="Left"                  // Alignment
  overflow="None"               // Overflow behavior
/>
```

### When to Use Templates

- **Complex layouts** - More than simple button + icon
- **Custom styling** - Unique appearance needs
- **Mixed content** - Combine text, icons, badges
- **Interactive elements** - Dropdowns, inputs, checkboxes
- **Conditional rendering** - Show/hide based on state

---

## Item-Wise Custom Templates

### Single Item Template

Create a unique template for one toolbar item:

```jsx
import { ItemDirective, ItemsDirective, ToolbarComponent } from '@syncfusion/ej2-react-navigations';

const App = () => {
  // Template for search bar
  const searchTemplate = () => {
    return (
      <div style={{ display: 'flex', alignItems: 'center', gap: '8px' }}>
        <input
          type="text"
          placeholder="Search..."
          style={{
            padding: '6px 12px',
            border: '1px solid #ddd',
            borderRadius: '4px',
            width: '200px'
          }}
        />
        <button className="e-btn">Go</button>
      </div>
    );
  };

  return (
    <ToolbarComponent>
      <ItemsDirective>
        <ItemDirective text="File" />
        <ItemDirective text="Edit" />
        <ItemDirective template={searchTemplate} align="Right" />
      </ItemsDirective>
    </ToolbarComponent>
  );
};

export default App;
```

### Multiple Different Templates

Use different templates for different items:

```jsx
const App = () => {
  // Template 1: Search
  const searchTemplate = () => (
    <input type="text" placeholder="Search..." style={{ width: '150px' }} />
  );

  // Template 2: Dropdown
  const dropdownTemplate = () => (
    <select style={{ padding: '6px' }}>
      <option>View</option>
      <option>List</option>
      <option>Grid</option>
      <option>Cards</option>
    </select>
  );

  // Template 3: Badge Counter
  const badgeTemplate = () => (
    <div style={{ position: 'relative' }}>
      <button className="e-btn">Notifications</button>
      <span style={{
        position: 'absolute',
        top: '-8px',
        right: '-8px',
        backgroundColor: 'red',
        color: 'white',
        borderRadius: '50%',
        width: '20px',
        height: '20px',
        display: 'flex',
        alignItems: 'center',
        justifyContent: 'center',
        fontSize: '12px'
      }}>
        5
      </span>
    </div>
  );

  return (
    <ToolbarComponent>
      <ItemsDirective>
        <ItemDirective template={searchTemplate} />
        <ItemDirective template={dropdownTemplate} />
        <ItemDirective template={badgeTemplate} align="Right" />
      </ItemsDirective>
    </ToolbarComponent>
  );
};

export default App;
```

### Template with State

Use React state within templates:

```jsx
import { useState } from 'react';
import { ItemDirective, ItemsDirective, ToolbarComponent } from '@syncfusion/ej2-react-navigations';

const App = () => {
  const [fontSize, setFontSize] = useState(14);

  const fontSizeTemplate = () => (
    <div>
      <label>Font Size: </label>
      <input
        type="number"
        value={fontSize}
        onChange={(e) => setFontSize(Number(e.target.value))}
        min={8}
        max={72}
        style={{ width: '50px', padding: '4px' }}
      />
    </div>
  );

  return (
    <ToolbarComponent>
      <ItemsDirective>
        <ItemDirective text="Bold" />
        <ItemDirective text="Italic" />
        <ItemDirective template={fontSizeTemplate} />
      </ItemsDirective>
    </ToolbarComponent>
  );
};

export default App;
```

### Template Styling

Apply custom CSS to templates:

```jsx
const App = () => {
  const customTemplate = () => (
    <div className="toolbar-custom-item">
      <span className="custom-label">Custom Item</span>
    </div>
  );

  return (
    <ToolbarComponent>
      <ItemsDirective>
        <ItemDirective template={customTemplate} />
      </ItemsDirective>
    </ToolbarComponent>
  );
};

export default App;
```

**CSS:**
```css
.toolbar-custom-item {
  display: flex;
  align-items: center;
  padding: 0 12px;
  height: 100%;
  background: linear-gradient(to right, #667eea, #764ba2);
  border-radius: 4px;
}

.custom-label {
  color: white;
  font-weight: bold;
  font-size: 14px;
}
```

---

## Stateless Templates

### Overview

Stateless templates allow rendering content without maintaining component state. This is useful for performance optimization when templates don't need to react to state changes.

### Using statelessTemplates Property

Configure stateless templates in ToolbarComponent:

```jsx
import { ItemDirective, ItemsDirective, ToolbarComponent } from '@syncfusion/ej2-react-navigations';

const App = () => {
  const statelessItems = [
    {
      text: 'Cut',
      template: () => (
        <div style={{ display: 'flex', alignItems: 'center', gap: '8px' }}>
          <span className="e-icons e-cut-icon"></span>
          <span>Cut</span>
        </div>
      )
    }
  ];

  return (
    <ToolbarComponent statelessTemplates={true}>
      <ItemsDirective>
        {/* Templates render without state management */}
      </ItemsDirective>
    </ToolbarComponent>
  );
};

export default App;
```

### CSS Classes for Popup Customization

When using stateless templates with Popup mode, customize using CSS classes:

```css
/* Show items in popup */
.e-overflow-show {
  /* Applies to items with overflow="Show" */
  display: flex !important;
}

/* Hide items from popup */
.e-overflow-hide {
  /* Applies to items with overflow="Hide" */
  display: none !important;
}

/* Text display in popup */
.e-popup-text {
  /* Label text styling in popup */
  font-weight: 500;
}

/* Text display in toolbar */
.e-toolbar-text {
  /* Label text styling in toolbar */
  font-weight: normal;
}
```

### Popup Customization Example

```jsx
<ToolbarComponent overflowMode="Popup" statelessTemplates={true}>
  <ItemsDirective>
    <ItemDirective text="Edit" overflow="Show" />
    <ItemDirective text="Delete" overflow="Show" />
    <ItemDirective text="Archive" overflow="Hide" />
    <ItemDirective text="Share" overflow="Hide" />
  </ItemsDirective>
</ToolbarComponent>
```

**CSS:**
```css
.e-overflow-show {
  background-color: #e3f2fd;
}

.e-overflow-hide {
  background-color: #f5f5f5;
}

.e-popup-text {
  color: #1976d2;
  font-weight: bold;
}
```

---

## Toggle Buttons

### Basic Toggle Button

Create a button that maintains pressed/unpressed state:

```jsx
import { useState } from 'react';
import { ItemDirective, ItemsDirective, ToolbarComponent } from '@syncfusion/ej2-react-navigations';

const App = () => {
  const [isBold, setIsBold] = useState(false);

  const toggleTemplate = () => (
    <button
      className={`e-btn toggle-btn ${isBold ? 'active' : ''}`}
      onClick={() => setIsBold(!isBold)}
    >
      <span className="e-icons e-bold-icon"></span>
      Bold
    </button>
  );

  return (
    <ToolbarComponent>
      <ItemsDirective>
        <ItemDirective template={toggleTemplate} />
        <ItemDirective text="Italic" />
      </ItemsDirective>
    </ToolbarComponent>
  );
};

export default App;
```

**CSS:**
```css
.toggle-btn {
  background-color: #f5f5f5;
  border: 1px solid #ccc;
}

.toggle-btn.active {
  background-color: #0066ff;
  color: white;
  border-color: #0056b3;
}
```

### Toggle Group (Like Radio Buttons)

Multiple toggles where only one is active:

```jsx
import { useState } from 'react';
import { ItemDirective, ItemsDirective, ToolbarComponent } from '@syncfusion/ej2-react-navigations';

const App = () => {
  const [alignment, setAlignment] = useState('left');

  const alignmentTemplate = () => (
    <div className="toggle-group">
      <button
        className={`e-btn toggle-btn ${alignment === 'left' ? 'active' : ''}`}
        onClick={() => setAlignment('left')}
        title="Align Left"
      >
        ↤
      </button>
      <button
        className={`e-btn toggle-btn ${alignment === 'center' ? 'active' : ''}`}
        onClick={() => setAlignment('center')}
        title="Align Center"
      >
        ⇄
      </button>
      <button
        className={`e-btn toggle-btn ${alignment === 'right' ? 'active' : ''}`}
        onClick={() => setAlignment('right')}
        title="Align Right"
      >
        ⤥
      </button>
    </div>
  );

  return (
    <ToolbarComponent>
      <ItemsDirective>
        <ItemDirective template={alignmentTemplate} />
      </ItemsDirective>
    </ToolbarComponent>
  );
};

export default App;
```

**CSS:**
```css
.toggle-group {
  display: flex;
  gap: 2px;
}

.toggle-btn {
  background-color: #f5f5f5;
  border: 1px solid #ccc;
  padding: 6px 12px;
  cursor: pointer;
}

.toggle-btn.active {
  background-color: #0066ff;
  color: white;
  border-color: #0056b3;
}
```

---

## Link Items

### Link as Toolbar Item

Render a link or anchor element in toolbar:

```jsx
<ToolbarComponent>
  <ItemsDirective>
    <ItemDirective text="Home" />
    <ItemDirective>
      <a href="https://example.com" className="e-btn e-tbar-btn">
        External Link
      </a>
    </ItemDirective>
  </ItemsDirective>
</ToolbarComponent>
```

### Navigation Links

Create navigation toolbar:

```jsx
import { Link } from 'react-router-dom';

const App = () => {
  const navTemplate = () => (
    <nav style={{ display: 'flex', gap: '16px' }}>
      <Link to="/" className="e-btn e-tbar-btn">Home</Link>
      <Link to="/about" className="e-btn e-tbar-btn">About</Link>
      <Link to="/services" className="e-btn e-tbar-btn">Services</Link>
      <Link to="/contact" className="e-btn e-tbar-btn">Contact</Link>
    </nav>
  );

  return (
    <ToolbarComponent>
      <ItemsDirective>
        <ItemDirective template={navTemplate} />
      </ItemsDirective>
    </ToolbarComponent>
  );
};

export default App;
```

### Breadcrumb-Style Links

```jsx
const App = () => {
  const breadcrumbTemplate = () => (
    <div style={{ display: 'flex', gap: '4px', alignItems: 'center' }}>
      <a href="/">Home</a>
      <span>/</span>
      <a href="/documents">Documents</a>
      <span>/</span>
      <span>Current Page</span>
    </div>
  );

  return (
    <ToolbarComponent>
      <ItemsDirective>
        <ItemDirective template={breadcrumbTemplate} />
      </ItemsDirective>
    </ToolbarComponent>
  );
};

export default App;
```

---

## Tooltips

### Add Tooltips to Buttons

Use HTML title attribute for tooltips:

```jsx
<ItemDirective 
  text="Save" 
  prefixIcon="e-save-icon"
  title="Save document (Ctrl+S)"
/>
```

### Template with Tooltip

```jsx
const App = () => {
  const saveTemplate = () => (
    <button 
      className="e-btn e-tbar-btn"
      title="Save document (Ctrl+S)"
    >
      <span className="e-icons e-save-icon"></span>
      Save
    </button>
  );

  return (
    <ToolbarComponent>
      <ItemsDirective>
        <ItemDirective template={saveTemplate} />
      </ItemsDirective>
    </ToolbarComponent>
  );
};

export default App;
```

### Custom Tooltip Component

```jsx
import { useState } from 'react';

const App = () => {
  const [showTooltip, setShowTooltip] = useState(false);

  const tooltipTemplate = () => (
    <div
      className="tooltip-container"
      onMouseEnter={() => setShowTooltip(true)}
      onMouseLeave={() => setShowTooltip(false)}
    >
      <button className="e-btn e-tbar-btn">
        <span className="e-icons e-help-icon"></span>
      </button>
      {showTooltip && (
        <div className="tooltip">
          Click here for help. Press F1 for more information.
        </div>
      )}
    </div>
  );

  return (
    <ToolbarComponent>
      <ItemsDirective>
        <ItemDirective template={tooltipTemplate} />
      </ItemsDirective>
    </ToolbarComponent>
  );
};

export default App;
```

**CSS:**
```css
.tooltip-container {
  position: relative;
  display: inline-block;
}

.tooltip {
  position: absolute;
  bottom: 100%;
  left: 50%;
  transform: translateX(-50%);
  background-color: #333;
  color: white;
  padding: 8px 12px;
  border-radius: 4px;
  white-space: nowrap;
  font-size: 12px;
  margin-bottom: 8px;
  z-index: 1000;
}

.tooltip::after {
  content: '';
  position: absolute;
  top: 100%;
  left: 50%;
  transform: translateX(-50%);
  border: 6px solid transparent;
  border-top-color: #333;
}
```

---

## Rendering Other Components

### Syncfusion Components in Toolbar

Render Syncfusion controls as toolbar items:

```jsx
import { ItemDirective, ItemsDirective, ToolbarComponent } from '@syncfusion/ej2-react-navigations';
import { DropDownListComponent } from '@syncfusion/ej2-react-dropdowns';
import { NumericTextBoxComponent } from '@syncfusion/ej2-react-inputs';
import { ColorPickerComponent } from '@syncfusion/ej2-react-inputs';

const App = () => {
  const fonts = ['Arial', 'Calibri', 'Courier New', 'Georgia', 'Times New Roman'];
  const sizes = [8, 10, 12, 14, 16, 18, 20, 24, 28, 32];

  const fontTemplate = () => (
    <DropDownListComponent
      dataSource={fonts}
      index={0}
      width={120}
    />
  );

  const sizeTemplate = () => (
    <NumericTextBoxComponent
      value={12}
      min={6}
      max={72}
      step={1}
      width={60}
    />
  );

  return (
    <ToolbarComponent>
      <ItemsDirective>
        <ItemDirective text="Bold" prefixIcon="e-bold-icon" />
        <ItemDirective type="Separator" />
        <ItemDirective type="Input" template={fontTemplate} />
        <ItemDirective type="Input" template={sizeTemplate} />
      </ItemsDirective>
    </ToolbarComponent>
  );
};

export default App;
```

### React Component in Toolbar

Render any React component:

```jsx
import { ItemDirective, ItemsDirective, ToolbarComponent } from '@syncfusion/ej2-react-navigations';

// Custom component
const UserProfile = () => {
  const [user, setUser] = useState({ name: 'John Doe', avatar: '👤' });

  return (
    <div style={{ display: 'flex', alignItems: 'center', gap: '8px' }}>
      <span style={{ fontSize: '20px' }}>{user.avatar}</span>
      <span>{user.name}</span>
    </div>
  );
};

const App = () => {
  const profileTemplate = () => <UserProfile />;

  return (
    <ToolbarComponent>
      <ItemsDirective>
        <ItemDirective text="File" />
        <ItemDirective template={profileTemplate} align="Right" />
      </ItemsDirective>
    </ToolbarComponent>
  );
};

export default App;
```

---

## Command Customization

### Add Click Handlers

Handle button clicks:

```jsx
import { useState } from 'react';
import { ItemDirective, ItemsDirective, ToolbarComponent } from '@syncfusion/ej2-react-navigations';

const App = () => {
  const [messages, setMessages] = useState([]);

  const handleSave = () => {
    setMessages([...messages, 'Document saved!']);
  };

  const handlePrint = () => {
    window.print();
    setMessages([...messages, 'Print dialog opened']);
  };

  const saveTemplate = () => (
    <button className="e-btn e-tbar-btn" onClick={handleSave}>
      <span className="e-icons e-save-icon"></span>
      Save
    </button>
  );

  const printTemplate = () => (
    <button className="e-btn e-tbar-btn" onClick={handlePrint}>
      <span className="e-icons e-print-icon"></span>
      Print
    </button>
  );

  return (
    <>
      <ToolbarComponent>
        <ItemsDirective>
          <ItemDirective template={saveTemplate} />
          <ItemDirective template={printTemplate} />
        </ItemsDirective>
      </ToolbarComponent>
      <div>
        {messages.map((msg, i) => (
          <div key={i}>{msg}</div>
        ))}
      </div>
    </>
  );
};

export default App;
```

### Context Menu Integration

Show context menu on button click:

```jsx
import { useState } from 'react';
import { ItemDirective, ItemsDirective, ToolbarComponent } from '@syncfusion/ej2-react-navigations';

const App = () => {
  const [menuVisible, setMenuVisible] = useState(false);

  const menuTemplate = () => (
    <div style={{ position: 'relative' }}>
      <button 
        className="e-btn e-tbar-btn"
        onClick={() => setMenuVisible(!menuVisible)}
      >
        Menu ▼
      </button>
      {menuVisible && (
        <div style={{
          position: 'absolute',
          top: '100%',
          left: 0,
          backgroundColor: 'white',
          border: '1px solid #ccc',
          borderRadius: '4px',
          minWidth: '150px',
          zIndex: 1000,
          marginTop: '4px'
        }}>
          <div style={{ padding: '8px 12px', cursor: 'pointer' }}>New</div>
          <div style={{ padding: '8px 12px', cursor: 'pointer' }}>Open</div>
          <div style={{ padding: '8px 12px', cursor: 'pointer' }}>Save</div>
          <hr style={{ margin: '4px 0' }} />
          <div style={{ padding: '8px 12px', cursor: 'pointer' }}>Exit</div>
        </div>
      )}
    </div>
  );

  return (
    <ToolbarComponent>
      <ItemsDirective>
        <ItemDirective template={menuTemplate} />
      </ItemsDirective>
    </ToolbarComponent>
  );
};

export default App;
```

---

## Dynamic Item Management

### Add Items at Runtime

Add new items to the toolbar using the `addItems()` method:

```jsx
import { useState, useRef } from 'react';
import { ItemDirective, ItemsDirective, ToolbarComponent } from '@syncfusion/ej2-react-navigations';

const App = () => {
  const toolbarRef = useRef(null);
  const [itemCount, setItemCount] = useState(0);

  const addNewItem = () => {
    const newItems = [
      {
        text: `New Item ${itemCount + 1}`,
        prefixIcon: 'e-new-icon'
      }
    ];
    
    // Add item at the end
    toolbarRef.current.addItems(newItems);
    setItemCount(itemCount + 1);
  };

  const addItemAtPosition = () => {
    const newItems = [
      {
        text: 'Insert Here',
        prefixIcon: 'e-insert-icon'
      }
    ];
    
    // Add item at index 2
    toolbarRef.current.addItems(newItems, 2);
  };

  return (
    <>
      <button onClick={addNewItem}>Add Item at End</button>
      <button onClick={addItemAtPosition}>Add Item at Position 2</button>
      
      <ToolbarComponent ref={toolbarRef}>
        <ItemsDirective>
          <ItemDirective text="Cut" prefixIcon="e-cut-icon" />
          <ItemDirective text="Copy" prefixIcon="e-copy-icon" />
          <ItemDirective text="Paste" prefixIcon="e-paste-icon" />
        </ItemsDirective>
      </ToolbarComponent>
    </>
  );
};

export default App;
```

### Remove Items

Remove items using the `removeItems()` method:

```jsx
const removeByIndex = () => {
  // Remove item at index 1
  toolbarRef.current.removeItems([1]);
};

const removeByID = () => {
  // Remove item with specific ID
  toolbarRef.current.removeItems(['copy-btn']);
};

const removeMultiple = () => {
  // Remove multiple items
  toolbarRef.current.removeItems([0, 2, 4]);
};
```

Example with remove buttons:

```jsx
<ItemDirective 
  id="copy-btn"
  text="Copy" 
  prefixIcon="e-copy-icon" 
/>

<button onClick={removeByID}>Remove Copy Button</button>
```

### Enable/Disable Items

Control item enabled state using `enableItems()`:

```jsx
const enableAllItems = () => {
  // Enable all items
  toolbarRef.current.enableItems(null, true);
};

const disableSpecificItems = () => {
  // Disable items at indices 1 and 3
  toolbarRef.current.enableItems([1, 3], false);
};

const disableByID = () => {
  // Disable item by ID
  toolbarRef.current.enableItems(['delete-btn'], false);
};
```

Complete example with enable/disable:

```jsx
<ItemDirective 
  id="delete-btn"
  text="Delete" 
  prefixIcon="e-delete-icon" 
/>

<button onClick={disableByID}>Disable Delete</button>
<button onClick={enableAllItems}>Enable All</button>
```

### Hide/Show Items

Control item visibility using `hideItem()`:

```jsx
const hideItem = () => {
  // Hide item at index 2
  toolbarRef.current.hideItem(2, true);
};

const showItem = () => {
  // Show item at index 2
  toolbarRef.current.hideItem(2, false);
};

const hideByID = () => {
  // Hide item by ID
  const itemIndex = toolbarRef.current.element.querySelector('#backup-btn');
  const index = Array.from(toolbarRef.current.element.querySelectorAll('.e-toolbar-item')).indexOf(itemIndex?.closest('.e-toolbar-item'));
  toolbarRef.current.hideItem(index, true);
};
```

### Refresh Overflow Layout

When adding/removing items in Scrollable or Popup modes, refresh the layout:

```jsx
const refreshLayout = () => {
  // Recalculate overflow behavior after dynamic changes
  toolbarRef.current.refreshOverflow();
};
```

### Destroy Toolbar

Clean up the component when no longer needed:

```jsx
const destroyToolbar = () => {
  toolbarRef.current.destroy();
};

// Typically done in useEffect cleanup
useEffect(() => {
  return () => {
    if (toolbarRef.current) {
      toolbarRef.current.destroy();
    }
  };
}, []);
```

### Complete Dynamic Management Example

```jsx
import { useState, useRef } from 'react';
import { ItemDirective, ItemsDirective, ToolbarComponent } from '@syncfusion/ej2-react-navigations';

const App = () => {
  const toolbarRef = useRef(null);
  const [items, setItems] = useState(['Cut', 'Copy', 'Paste']);

  const addItem = () => {
    const newItem = { text: `Item ${items.length + 1}` };
    toolbarRef.current.addItems([newItem]);
    setItems([...items, newItem.text]);
  };

  const removeLastItem = () => {
    if (items.length > 0) {
      toolbarRef.current.removeItems([items.length - 1]);
      setItems(items.slice(0, -1));
    }
  };

  const disableFirst = () => {
    toolbarRef.current.enableItems([0], false);
  };

  const enableAll = () => {
    toolbarRef.current.enableItems(null, true);
  };

  return (
    <>
      <div style={{ marginBottom: '16px' }}>
        <button onClick={addItem} style={{ marginRight: '8px' }}>Add Item</button>
        <button onClick={removeLastItem} style={{ marginRight: '8px' }}>Remove Last</button>
        <button onClick={disableFirst} style={{ marginRight: '8px' }}>Disable First</button>
        <button onClick={enableAll}>Enable All</button>
      </div>
      
      <ToolbarComponent ref={toolbarRef}>
        <ItemsDirective>
          <ItemDirective text="Cut" prefixIcon="e-cut-icon" />
          <ItemDirective text="Copy" prefixIcon="e-copy-icon" />
          <ItemDirective text="Paste" prefixIcon="e-paste-icon" />
        </ItemsDirective>
      </ToolbarComponent>
    </>
  );
};

export default App;
```

---

## Event Handling

### Available Events

Toolbar provides several events for monitoring user interactions and lifecycle:

| Event | Triggers | Arguments |
|-------|----------|-----------|
| `beforeCreate` | Before component initialization | BeforeCreateArgs |
| `created` | After component initialization | Event |
| `clicked` | When item clicked | ClickEventArgs |
| `keyDown` | When key pressed in toolbar | KeyDownEventArgs |
| `destroyed` | When component destroyed | Event |

### beforeCreate Event

Fired before the Toolbar component is initialized. Useful for configuration modifications:

```jsx
const handleBeforeCreate = (args) => {
  console.log('Toolbar initializing');
  console.log('enableCollision:', args.enableCollision); // Get/set collision handling
  console.log('scrollStep:', args.scrollStep);           // Get/set scroll distance
  
  // Modify configuration before creation
  args.scrollStep = 100; // Increase scroll step
};

<ToolbarComponent beforeCreate={handleBeforeCreate}>
  <ItemsDirective>
    {/* items */}
  </ItemsDirective>
</ToolbarComponent>
```

### clicked Event

Fired when a toolbar item is clicked:

```jsx
const handleClick = (args) => {
  console.log('Item clicked:', args.item.text);
  console.log('Item ID:', args.item.id);
  console.log('Original event:', args.originalEvent); // Browser click event
  
  // Prevent default action
  args.cancel = true; // Cancel if needed
};

<ToolbarComponent clicked={handleClick}>
  <ItemsDirective>
    <ItemDirective text="Save" />
  </ItemsDirective>
</ToolbarComponent>
```

### keyDown Event

Fired when a keyboard key is pressed in the toolbar:

```jsx
const handleKeyDown = (args) => {
  console.log('Key pressed');
  console.log('Current item:', args.currentItem);   // Currently focused item
  console.log('Next item:', args.nextItem);         // Item that would be selected
  console.log('Original event:', args.originalEvent); // Browser keyboard event
  
  // Implement custom keyboard navigation
  if (args.originalEvent.key === 'Enter') {
    console.log('Enter pressed on:', args.currentItem?.textContent);
  }
  
  // Prevent default navigation
  args.cancel = true;
};

<ToolbarComponent keyDown={handleKeyDown}>
  <ItemsDirective>
    {/* items */}
  </ItemsDirective>
</ToolbarComponent>
```

### created Event

Fired after the Toolbar component is fully initialized:

```jsx
const handleCreated = () => {
  console.log('Toolbar created successfully');
  // Initialize any post-creation logic
};

<ToolbarComponent created={handleCreated}>
  <ItemsDirective>
    {/* items */}
  </ItemsDirective>
</ToolbarComponent>
```

### destroyed Event

Fired when the Toolbar component is destroyed:

```jsx
const handleDestroyed = () => {
  console.log('Toolbar destroyed');
  // Cleanup resources
};

<ToolbarComponent destroyed={handleDestroyed}>
  <ItemsDirective>
    {/* items */}
  </ItemsDirective>
</ToolbarComponent>
```

### Complete Event Handling Example

```jsx
import { useState } from 'react';
import { ItemDirective, ItemsDirective, ToolbarComponent } from '@syncfusion/ej2-react-navigations';

const App = () => {
  const [messages, setMessages] = useState([]);

  const addMessage = (msg) => {
    setMessages([...messages, msg]);
  };

  const handleBeforeCreate = (args) => {
    addMessage('beforeCreate: Initializing...');
  };

  const handleCreated = () => {
    addMessage('created: Component ready');
  };

  const handleClick = (args) => {
    addMessage(`clicked: ${args.item.text}`);
  };

  const handleKeyDown = (args) => {
    addMessage(`keyDown: ${args.originalEvent.key} on ${args.currentItem?.textContent}`);
  };

  return (
    <>
      <ToolbarComponent
        beforeCreate={handleBeforeCreate}
        created={handleCreated}
        clicked={handleClick}
        keyDown={handleKeyDown}
      >
        <ItemsDirective>
          <ItemDirective text="Cut" prefixIcon="e-cut-icon" />
          <ItemDirective text="Copy" prefixIcon="e-copy-icon" />
          <ItemDirective text="Paste" prefixIcon="e-paste-icon" />
        </ItemsDirective>
      </ToolbarComponent>
      
      <div style={{ marginTop: '20px' }}>
        <h3>Event Log:</h3>
        {messages.map((msg, i) => (
          <div key={i}>{msg}</div>
        ))}
      </div>
    </>
  );
};

export default App;
```

---

## Scroll Step Customization

### Control Scroll Distance

Customize how much the toolbar scrolls:

```jsx
<ToolbarComponent 
  scrollStep={100}
  overflowMode="Scrollable"
>
  <ItemsDirective>
    {/* items */}
  </ItemsDirective>
</ToolbarComponent>
```

Default `scrollStep` is 50 pixels. Increase for larger jumps, decrease for finer control.

### Dynamic Scroll Step

Adjust based on device type:

```jsx
const App = () => {
  const isMobile = window.innerWidth < 768;
  const scrollStep = isMobile ? 150 : 50;

  return (
    <ToolbarComponent 
      scrollStep={scrollStep}
      overflowMode="Scrollable"
    >
      <ItemsDirective>
        {/* items */}
      </ItemsDirective>
    </ToolbarComponent>
  );
};

export default App;
```

### Touch Swipe Distance

Touch swipe behavior is automatic and uses the scrollStep property.

---

## Complete Advanced Example

Comprehensive toolbar with multiple advanced features:

```jsx
import { useState } from 'react';
import { ItemDirective, ItemsDirective, ToolbarComponent } from '@syncfusion/ej2-react-navigations';
import { DropDownListComponent } from '@syncfusion/ej2-react-dropdowns';

const App = () => {
  const [alignment, setAlignment] = useState('left');
  const [isBold, setIsBold] = useState(false);
  const [isItalic, setIsItalic] = useState(false);
  const [fontSize, setFontSize] = useState(14);

  // Alignment toggle template
  const alignmentTemplate = () => (
    <div style={{ display: 'flex', gap: '2px' }}>
      {['left', 'center', 'right'].map(align => (
        <button
          key={align}
          className={`e-btn ${alignment === align ? 'active' : ''}`}
          onClick={() => setAlignment(align)}
          style={{
            padding: '6px 10px',
            backgroundColor: alignment === align ? '#0066ff' : '#f5f5f5',
            color: alignment === align ? 'white' : 'black',
            border: 'none',
            cursor: 'pointer'
          }}
        >
          {align.charAt(0).toUpperCase()}
        </button>
      ))}
    </div>
  );

  // Font selector template
  const fontTemplate = () => (
    <DropDownListComponent
      dataSource={['Arial', 'Calibri', 'Courier']}
      index={0}
      width={120}
    />
  );

  // Toggle button template
  const toggleTemplate = (label, state, setState) => () => (
    <button
      className="e-btn"
      onClick={() => setState(!state)}
      style={{
        padding: '6px 12px',
        backgroundColor: state ? '#0066ff' : '#f5f5f5',
        color: state ? 'white' : 'black',
        border: 'none',
        cursor: 'pointer',
        fontWeight: state ? 'bold' : 'normal'
      }}
    >
      {label}
    </button>
  );

  return (
    <ToolbarComponent overflowMode="Popup">
      <ItemsDirective>
        {/* File operations */}
        <ItemDirective text="New" prefixIcon="e-new-icon" />
        <ItemDirective text="Open" prefixIcon="e-open-icon" />
        <ItemDirective text="Save" prefixIcon="e-save-icon" />
        <ItemDirective type="Separator" />

        {/* Format operations */}
        <ItemDirective template={toggleTemplate('Bold', isBold, setIsBold)} />
        <ItemDirective template={toggleTemplate('Italic', isItalic, setIsItalic)} />
        <ItemDirective type="Separator" />

        {/* Alignment */}
        <ItemDirective template={alignmentTemplate} />
        <ItemDirective type="Separator" />

        {/* Font selection */}
        <ItemDirective type="Input" template={fontTemplate} />

        {/* Help at end */}
        <ItemDirective 
          text="Help" 
          prefixIcon="e-help-icon" 
          align="Right"
          title="Press F1 for help"
        />
      </ItemsDirective>
    </ToolbarComponent>
  );
};

export default App;
```

---

## Summary

Advanced features enable powerful customizations:
- **Templates** - Complete control over item appearance
- **Toggle buttons** - State-based button behavior
- **Links** - Navigation and external links
- **Tooltips** - Contextual help for users
- **Component rendering** - Embed Syncfusion or custom components
- **Command handlers** - Custom actions and logic
- **Scroll customization** - Fine-tune scroll behavior

Combine these features to build sophisticated, interactive toolbars tailored to your application's needs.
