# API Reference

## Table of Contents
- [Properties](#properties)
- [Methods](#methods)
- [Events](#events)
- [Field Mapping](#field-mapping)
- [Type Definitions](#type-definitions)

## Properties

### animation

**Type:** `AnimationSettings`  
**Default:** `{ effect: 'None', duration: 400, easing: 'ease-in' }`

Controls item animation when rendered or updated.

```tsx
<ListViewComponent
  animation={{
    effect: 'SlideDown',      // FadeIn, SlideUp, SlideDown, SlideLeft, SlideRight, Zoom
    duration: 500,             // milliseconds
    easing: 'ease-out'         // ease, ease-in, ease-out, ease-in-out, linear
  }}
/>
```

### checkBoxPosition

**Type:** `'Left' | 'Right'`  
**Default:** `'Left'`

Position of checkbox when `showCheckBox` is enabled.

```tsx
// Checkbox on left
<ListViewComponent checkBoxPosition="Left" showCheckBox={true} />

// Checkbox on right
<ListViewComponent checkBoxPosition="Right" showCheckBox={true} />
```

### cssClass

**Type:** `string`  
**Default:** `''`

CSS class applied to the ListView container.

```tsx
<ListViewComponent
  cssClass="my-custom-list e-primary"
/>

// In CSS:
// .my-custom-list { padding: 20px; }
// .my-custom-list .e-list-item { color: blue; }
```

### dataSource

**Type:** `any[] | DataManager`  
**Default:** `[]`

Data to display in the ListView.

```tsx
// Array of objects
<ListViewComponent
  dataSource={[
    { id: 1, text: 'Item 1' },
    { id: 2, text: 'Item 2' }
  ]}
/>

// DataManager for remote data
const dm = new DataManager({
  url: 'https://api.example.com/items',
  adaptor: new UrlAdaptor()
});

<ListViewComponent dataSource={dm} />
```

### disableHtmlEncode

**Type:** `boolean`  
**Default:** `true`

Enable rendering of raw text content in the ListView component without HTML encoding. When set to true, the text will be displayed exactly as provided (including HTML tags or special characters), instead of being encoded or truncated.

**Note:** To preserve and render raw HTML content correctly, `enableHtmlSanitizer` must also be set to false.

```tsx
// Display raw HTML content (both enableHtmlSanitizer=false AND disableHtmlEncode=true)
<ListViewComponent
  disableHtmlEncode={true}
  enableHtmlSanitizer={false}
  template={(props: any) => `<div><strong>${props.text}</strong></div>`}
  dataSource={[
    { id: 1, text: 'Product <em>Name</em>' },
    { id: 2, text: 'Price: $99.99' }
  ]}
/>

// With HTML encoding (default behavior)
<ListViewComponent
  disableHtmlEncode={false}
  enableHtmlSanitizer={true}
  dataSource={[
    { id: 1, text: 'Item with <tag>' },  // Will show as: Item with &lt;tag&gt;
    { id: 2, text: 'Text & symbols' }     // Will show as: Text &amp; symbols
  ]}
/>

// Display special characters as-is
<ListViewComponent
  disableHtmlEncode={true}
  dataSource={[
    { id: 1, text: 'hiiih<hihi' },  // Shows exactly as: hiiih<hihi
    { id: 2, text: 'Price: $50 & up' }
  ]}
/>
```

### enable

**Type:** `boolean`  
**Default:** `true`

Enable or disable the ListView component.

```tsx
<ListViewComponent enable={isEnabled} />
```

### enableHtmlSanitizer

**Type:** `boolean`  
**Default:** `true`

Sanitize HTML in templates to prevent XSS attacks.

```tsx
// Safe: HTML is sanitized
<ListViewComponent
  enableHtmlSanitizer={true}
  template={(props: any) => `<div>${props.text}</div>`}
/>

// Unsafe: Allows any HTML
<ListViewComponent
  enableHtmlSanitizer={false}
  template={(props: any) => `<div>${props.dangerousContent}</div>`}
/>
```

### enablePersistence

**Type:** `boolean`  
**Default:** `false`

Persist ListView state (scroll position, selection, checked items) in localStorage.

```tsx
<ListViewComponent
  enablePersistence={true}
/>

// State persisted to: localStorage['ComponentName']
```

### enableRtl

**Type:** `boolean`  
**Default:** `false`

Enable right-to-left layout for RTL languages.

```tsx
<ListViewComponent enableRtl={true} />
```

### enableVirtualization

**Type:** `boolean`  
**Default:** `false`

Enable virtual scrolling for large datasets (1000+ items).

```tsx
<ListViewComponent
  dataSource={largeArray}  // 5000+ items
  enableVirtualization={true}
  height="400px"
/>
```

### fields

**Type:** `FieldSettingsModel`  
**Default:** `{ id: 'id', text: 'text' }`

Maps data source fields to ListView properties.

```tsx
<ListViewComponent
  fields={{
    id: 'itemId',           // Unique identifier
    text: 'itemName',       // Display text
    child: 'subItems',      // Nested items
    groupBy: 'category',    // Grouping field
    image: 'imageUrl',      // Image field
    iconCss: 'iconClass',   // Icon CSS class
    sortBy: 'sortField',    // Sort field
    isChecked: 'checked',   // Checkbox state
    enabled: 'isEnabled'    // Enable/disable state
  }}
/>
```

### groupTemplate

**Type:** `string | Function`  
**Default:** `null`

Custom template for group headers when data is grouped.

```tsx
const groupTemplate = (props: any) => (
  <div style={{ fontWeight: 'bold', backgroundColor: '#f0f0f0', padding: '5px' }}>
    {props.text} ({props.count} items)
  </div>
);

<ListViewComponent
  groupTemplate={groupTemplate}
  fields={{ groupBy: 'category' }}
/>
```

### headerTemplate

**Type:** `string | Function`  
**Default:** `null`

Custom template for ListView header.

```tsx
const headerTemplate = () => (
  <div style={{ padding: '10px', backgroundColor: '#2196F3', color: 'white' }}>
    <h3>My List</h3>
  </div>
);

<ListViewComponent
  headerTemplate={headerTemplate}
  showHeader={true}
/>
```

### headerTitle

**Type:** `string`  
**Default:** `''`

Title for the ListView header.

```tsx
<ListViewComponent
  headerTitle="Inbox"
  showHeader={true}
/>
```

### height

**Type:** `string | number`  
**Default:** `'100%'`

Height of the ListView container.

```tsx
// Pixel value
<ListViewComponent height="500px" />

// Percentage
<ListViewComponent height="100%" />

// Number (assumes px)
<ListViewComponent height={400} />
```

### htmlAttributes

**Type:** `{ [key: string]: string }`  
**Default:** `{}`

Additional HTML attributes for the ListView container.

```tsx
<ListViewComponent
  htmlAttributes={{
    'data-test': 'my-list',
    'aria-label': 'Item list',
    role: 'list'
  }}
/>
```

### locale

**Type:** `string`  
**Default:** `'en-US'`

Localization culture for the ListView.

```tsx
<ListViewComponent locale="de-DE" />
// German localization

<ListViewComponent locale="ar-AE" />
// Arabic with RTL support
```

### query

**Type:** `Query`  
**Default:** `null`

DataManager query for filtering, sorting, and pagination.

```tsx
import { Query } from '@syncfusion/ej2-data';

<ListViewComponent
  dataSource={dataManager}
  query={new Query()
    .where('category', 'equal', 'Electronics')
    .take(10)
    .sortBy('price')
  }
/>
```

### showCheckBox

**Type:** `boolean`  
**Default:** `false`

Show checkboxes for multi-select functionality.

```tsx
<ListViewComponent showCheckBox={true} />
```

### showHeader

**Type:** `boolean`  
**Default:** `false`

Display the ListView header.

```tsx
<ListViewComponent
  showHeader={true}
  headerTitle="My Items"
/>
```

### showIcon

**Type:** `boolean`  
**Default:** `false`

Show icons next to items (requires iconCss field).

```tsx
<ListViewComponent
  showIcon={true}
  fields={{ iconCss: 'icon' }}
/>
```

### sortOrder

**Type:** `'Ascending' | 'Descending' | 'None'`  
**Default:** `'None'`

Sort order for list items.

```tsx
// Sort ascending
<ListViewComponent sortOrder="Ascending" />

// Sort descending
<ListViewComponent sortOrder="Descending" />

// No sorting
<ListViewComponent sortOrder="None" />
```

### template

**Type:** `string | Function`  
**Default:** `null`

Custom template for list items.

```tsx
// JSX template
const template = (props: any) => (
  <div>
    <strong>{props.text}</strong>
    <p>{props.description}</p>
  </div>
);

<ListViewComponent template={template} />
```

### width

**Type:** `string | number`  
**Default:** `'100%'`

Width of the ListView container.

```tsx
// Pixel value
<ListViewComponent width="500px" />

// Percentage
<ListViewComponent width="100%" />

// Number (assumes px)
<ListViewComponent width={500} />
```

## Methods

### addItem

**Signature:** `addItem(data: any, fields?: FieldSettingsModel): void`

Add single or multiple items to the ListView.

```tsx
const listViewRef = useRef<ListViewComponent>(null);

const handleAddItem = () => {
  // Add single item
  listViewRef.current?.addItem({
    id: '1',
    text: 'New Item'
  });
};

const handleAddMultiple = () => {
  // Add multiple items
  listViewRef.current?.addItem([
    { id: '1', text: 'Item 1' },
    { id: '2', text: 'Item 2' }
  ]);
};
```

### back

**Signature:** `back(): void`

Navigate back from nested ListView to parent.

```tsx
const handleBackClick = () => {
  listViewRef.current?.back?.();
};

<button onClick={handleBackClick}>← Back</button>
```

### checkAllItems

**Signature:** `checkAllItems(): void`

Check all checkboxes in the ListView.

```tsx
const handleCheckAll = () => {
  listViewRef.current?.checkAllItems?.();
};

<button onClick={handleCheckAll}>Check All</button>
```

### checkItem

**Signature:** `checkItem(item: any): void`

Check specific item checkbox.

```tsx
const handleCheckItem = (itemId: string) => {
  listViewRef.current?.checkItem?.({ id: itemId });
};
```

### destroy

**Signature:** `destroy(): void`

Destroy the ListView component and release all resources.

```tsx
const handleDestroyListView = () => {
  // Clean up the ListView before unmounting
  listViewRef.current?.destroy?.();
};

// Typically used in useEffect cleanup
useEffect(() => {
  return () => {
    listViewRef.current?.destroy?.();
  };
}, []);
```

### disableItem

**Signature:** `disableItem(item: any): void`

Disable an item.

```tsx
const handleDisableItem = (itemId: string) => {
  listViewRef.current?.disableItem?.({ id: itemId });
};
```

### enableItem

**Signature:** `enableItem(item: any): void`

Enable a disabled item.

```tsx
const handleEnableItem = (itemId: string) => {
  listViewRef.current?.enableItem?.({ id: itemId });
};
```

### findItem

**Signature:** `findItem(fields: any): HTMLElement | null`

Find item element by field values.

```tsx
const handleFindItem = () => {
  const element = listViewRef.current?.findItem?.({
    id: '5',
    text: 'Item 5'
  });

  if (element) {
    console.log('Item found:', element);
  }
};
```

### getSelectedItems

**Signature:** `getSelectedItems(): SelectedCollection`

Get all selected items.

```tsx
const handleGetSelected = () => {
  const selected = listViewRef.current?.getSelectedItems?.();
  console.log('Selected items:', selected?.data);
  console.log('Selected text:', selected?.text);
};
```

### hideItem

**Signature:** `hideItem(item: any): void`

Hide an item from display.

```tsx
const handleHideItem = (itemId: string) => {
  listViewRef.current?.hideItem?.({ id: itemId });
};
```

### refresh

**Signature:** `refresh(): void`

Refresh the ListView with current data.

```tsx
const handleRefresh = () => {
  listViewRef.current?.refresh?.();
};

<button onClick={handleRefresh}>Refresh</button>
```

### refreshItemHeight

**Signature:** `refreshItemHeight(): void`

Recalculate item heights for virtual scrolling.

```tsx
const handleUpdateHeights = () => {
  listViewRef.current?.refreshItemHeight?.();
};
```

### removeItem

**Signature:** `removeItem(item: any): void`

Remove item(s) from ListView.

```tsx
const handleRemoveItem = (itemId: string) => {
  // Remove single item
  listViewRef.current?.removeItem?.({ id: itemId });
};

const handleRemoveMultiple = () => {
  // Remove multiple items
  listViewRef.current?.removeItem?.([
    { id: '1' },
    { id: '2' }
  ]);
};
```

### selectItem

**Signature:** `selectItem(item: any): void`

Select specific item.

```tsx
const handleSelectItem = (itemId: string) => {
  listViewRef.current?.selectItem?.({ id: itemId });
};
```

### selectMultipleItems

**Signature:** `selectMultipleItems(items: any[]): void`

Select multiple items at once.

```tsx
const handleSelectMultiple = () => {
  listViewRef.current?.selectMultipleItems?.([
    { id: '1' },
    { id: '2' },
    { id: '3' }
  ]);
};
```

### showItem

**Signature:** `showItem(item: any): void`

Show hidden item.

```tsx
const handleShowItem = (itemId: string) => {
  listViewRef.current?.showItem?.({ id: itemId });
};
```

### uncheckAllItems

**Signature:** `uncheckAllItems(): void`

Uncheck all checkboxes.

```tsx
const handleUncheckAll = () => {
  listViewRef.current?.uncheckAllItems?.();
};

<button onClick={handleUncheckAll}>Uncheck All</button>
```

### uncheckItem

**Signature:** `uncheckItem(item: any): void`

Uncheck specific item.

```tsx
const handleUncheckItem = (itemId: string) => {
  listViewRef.current?.uncheckItem?.({ id: itemId });
};
```

### unselectItem

**Signature:** `unselectItem(item?: any): void`

Deselect item.

```tsx
const handleUnselectItem = (itemId: string) => {
  listViewRef.current?.unselectItem?.({ id: itemId });
};
```

### removeMultipleItems

**Signature:** `removeMultipleItems(items: any[]): void`

Remove multiple items from the ListView at once.

```tsx
const handleRemoveMultiple = () => {
  listViewRef.current?.removeMultipleItems?.([
    { id: '1' },
    { id: '2' },
    { id: '3' }
  ]);
};
```

## Events

### select

**Type:** `(args: SelectEventArgs) => void`

Fired when item is selected.

```tsx
interface SelectEventArgs {
  text: string;               // Item text
  data: any;                  // Item data
  index: number;              // Item index
  isInteracted: boolean;      // User interacted
  eventType: string;          // Event type
  isCtrlKey: boolean;         // Ctrl key pressed
  isShiftKey: boolean;        // Shift key pressed
  isTouchEnd: boolean;        // Touch event
  element: HTMLElement;       // DOM element
}

const handleSelect = (args: SelectEventArgs) => {
  console.log('Selected:', args.text, args.data);
};

<ListViewComponent select={handleSelect} />
```

### actionBegin

**Type:** `(args: ActionEventArgs) => void`

Fired before action completes (before select, sort, filter, etc).

```tsx
interface ActionEventArgs {
  eventType: string;          // 'select', 'sorting', 'filtering', 'drop'
  cancel: boolean;            // Cancel action
  data?: any;                 // Action data
}

const handleActionBegin = (args: ActionEventArgs) => {
  if (args.eventType === 'select') {
    console.log('Before select');
  }
};

<ListViewComponent actionBegin={handleActionBegin} />
```

### actionComplete

**Type:** `(args: ActionEventArgs) => void`

Fired after action completes.

```tsx
const handleActionComplete = (args: ActionEventArgs) => {
  if (args.eventType === 'select') {
    console.log('After select');
  }
  if (args.eventType === 'drop') {
    console.log('Item dropped');
  }
};

<ListViewComponent actionComplete={handleActionComplete} />
```

### actionFailure

**Type:** `(args: any) => void`

Fired when action fails (e.g., data load error).

```tsx
const handleActionFailure = (args: any) => {
  console.error('Action failed:', args);
};

<ListViewComponent actionFailure={handleActionFailure} />
```

### scroll

**Type:** `(args: ScrollEventArgs) => void`

Fired when ListView is scrolled.

```tsx
interface ScrollEventArgs {
  isAtEnd: boolean;           // Scrolled to end
  isAtStart: boolean;         // Scrolled to start
  isInteracted: boolean;      // User interacted
  scrollTop: number;          // Vertical scroll position
  scrollLeft: number;         // Horizontal scroll position
}

const handleScroll = (args: ScrollEventArgs) => {
  if (args.isAtEnd) {
    console.log('Load more items');
  }
};

<ListViewComponent scroll={handleScroll} />
```

## Field Mapping

### Complete FieldSettingsModel

```tsx
interface FieldSettingsModel {
  // Unique identifier field
  id?: string;
  
  // Display text field
  text?: string;
  
  // Nested/child items field
  child?: string;
  
  // Grouping field
  groupBy?: string;
  
  // Image URL field
  image?: string;
  
  // Icon CSS class field
  iconCss?: string;
  
  // Sort field (if different from text)
  sortBy?: string;
  
  // Checkbox state field
  isChecked?: string;
  
  // Enable/disable state field
  enabled?: string;
}

// Usage example
<ListViewComponent
  fields={{
    id: 'itemId',
    text: 'itemName',
    child: 'subItems',
    groupBy: 'itemCategory',
    image: 'thumbnailUrl',
    iconCss: 'iconClass',
    sortBy: 'itemOrder',
    isChecked: 'isSelected',
    enabled: 'isActive'
  }}
/>
```

## Type Definitions

### AnimationSettings

```tsx
interface AnimationSettings {
  effect?: 'None' | 'FadeIn' | 'SlideUp' | 'SlideDown' | 'SlideLeft' | 'SlideRight' | 'Zoom' | 'Expand';
  duration?: number;
  easing?: 'ease' | 'ease-in' | 'ease-out' | 'ease-in-out' | 'linear';
}
```

### SelectedCollection

```tsx
interface SelectedCollection {
  text: string[];          // Text of selected items
  data: any[];             // Data of selected items
  index: number[];         // Index of selected items
}

// Usage
const selected = listViewRef.current?.getSelectedItems?.();
console.log(selected?.text);   // ['Item 1', 'Item 2']
console.log(selected?.data);   // [{ id: 1, text: 'Item 1' }, ...]
console.log(selected?.index);  // [0, 1]
```

### Query Filters

```tsx
import { Query, Predicate } from '@syncfusion/ej2-data';

// Equals
new Query().where('field', 'equal', 'value')

// Not equal
new Query().where('field', 'notequal', 'value')

// Contains
new Query().where('field', 'contains', 'value')

// Greater than
new Query().where('field', 'greaterthan', 10)

// Less than
new Query().where('field', 'lessthan', 10)

// Starts with
new Query().where('field', 'startswith', 'prefix')

// Ends with
new Query().where('field', 'endswith', 'suffix')

// AND condition
new Query().where('field1', 'equal', 'value1')
           .where('field2', 'equal', 'value2')

// OR condition
const predicate = new Predicate('field1', 'equal', 'value1')
                   .or('field2', 'equal', 'value2');
new Query().where(predicate)
```

