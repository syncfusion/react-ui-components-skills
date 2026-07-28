# ER Diagrams

## Table of Contents
- [Overview](#overview)
- [Creating ER Entity Nodes](#creating-er-entity-nodes)
- [Configure the Entity Header](#configure-the-entity-header)
- [Define Entity Fields](#define-entity-fields)
- [Field Properties Reference](#field-properties-reference)
- [Add or Remove Fields at Runtime](#add-or-remove-fields-at-runtime)
- [Configure Default Field Appearance](#configure-default-field-appearance)
- [Style ER Entities and Fields](#style-er-entities-and-fields)
- [Track Entity Field Changes](#track-entity-field-changes)
- [Creating ER Relationships](#creating-er-relationships)
- [Relationship Multiplicity](#relationship-multiplicity)
- [Complete Multi-Table Example](#complete-multi-table-example)

---

## Overview

An Entity Relationship (ER) diagram is a visual representation of a database structure. It displays entities (such as tables), their attributes (columns), and the relationships between entities.

In Syncfusion React Diagram:
- **ER entity nodes** are configured using `ErShapeModel` with `shape.type = 'Er'`
- **ER relationships** are connectors configured using `ErConnectorShapeModel` with `shape.type = 'Er'`
- Entity nodes are added to the `nodes` property; relationships are added to the `connectors` property

---

## Creating ER Entity Nodes

An ER entity node represents a database table. It renders as a box with a header row (entity name) and field rows (columns). Set `shape.type` to `'Er'` to activate the ER shape.

```tsx
import * as React from "react";
import * as ReactDOM from "react-dom/client";
import { DiagramComponent, Diagram, ErDiagrams } from "@syncfusion/ej2-react-diagrams";
import { NodeModel, ErShapeModel } from "@syncfusion/ej2-diagrams";

Diagram.Inject(ErDiagrams);

const customer: NodeModel = {
  id: 'Customer',
  offsetX: 300,
  offsetY: 200,
  shape: {
    type: 'Er',
    header: {
      annotation: { content: 'Customer' }
    },
    fields: [
      {
        id: 'cust_id',
        name: 'CustomerID',
        dataType: 'INT',
        isPrimaryKey: true,
        constraints: ['NotNull']
      },
      {
        id: 'cust_firstname',
        name: 'FirstName',
        dataType: 'VARCHAR(50)',
        constraints: ['NotNull']
      },
      {
        id: 'cust_email',
        name: 'Email',
        dataType: 'VARCHAR(100)',
        constraints: ['Unique']
      }
    ]
  } as ErShapeModel
};

function App() {
  return (
    <DiagramComponent id="container" width={'100%'} height={'600px'} nodes={[customer]} />
  );
}
const root = ReactDOM.createRoot(document.getElementById('diagram'));
root.render(<App />);
```

> If no `fields` are specified, a single default field is automatically added. If no `header` is specified, a default header is added with default style and height.

---

## Configure the Entity Header

The header is the top section of an ER entity node that displays the entity name.

| Property | Description |
|---|---|
| `annotation` | Text content displayed in the header. |
| `height` | Height of the header area in pixels. |
| `style` | Fill color, text color, font settings for the header. |

```tsx
shape: {
  type: 'Er',
  header: {
    annotation: {
      content: 'CUSTOMER TABLE',
      style: { color: 'white', fontSize: 13, bold: true, fontFamily: 'Arial' }
    },
    height: 35,
    style: { fill: '#2E75B6' }
  },
  fields: [ /* ... */ ]
} as ErShapeModel
```

---

## Define Entity Fields

Fields represent columns or attributes of an entity. Define them using the `fields` property on the `ErShapeModel`.

```tsx
import { NodeModel, ErShapeModel, ErFieldModel } from "@syncfusion/ej2-diagrams";

Diagram.Inject(ErDiagrams);

const product: NodeModel = {
  id: 'Product',
  offsetX: 250,
  offsetY: 200,
  shape: {
    type: 'Er',
    header: { annotation: { content: 'Product' } },
    fields: [
      {
        id: 'prod_id',
        name: 'ProductID',
        dataType: 'INT',
        isPrimaryKey: true,
        constraints: ['NotNull']
      },
      {
        id: 'prod_code',
        name: 'ProductCode',
        dataType: 'VARCHAR(50)',
        constraints: ['NotNull', 'Unique']
      },
      {
        id: 'prod_price',
        name: 'Price',
        dataType: 'DECIMAL(10,2)',
        constraints: ['NotNull']
      },
      {
        id: 'prod_catid',
        name: 'CategoryID',
        dataType: 'INT',
        isForeignKey: true
      }
    ] as ErFieldModel[]
  } as ErShapeModel
};
```

---

## Field Properties Reference

| Property | Type | Description |
|---|---|---|
| `id` | `string` | Unique identifier for the field within the entity. |
| `name` | `string` | Display name of the field (shown in the row). |
| `dataType` | `string` | Data type such as `INT`, `VARCHAR(255)`, `BOOLEAN`, `DECIMAL(10,2)`. |
| `isPrimaryKey` | `boolean` | Marks the field as the primary key. Renders a PK indicator. |
| `isForeignKey` | `boolean` | Marks the field as a foreign key referencing another entity. |
| `constraints` | `ErFieldConstraint[]` | Additional constraints: `'NotNull'`, `'Unique'`. Accepts an array. |
| `style` | `ShapeStyleModel` | Visual style for the field row (fill, stroke, opacity). Overrides node-level and fieldDefaults styles. |
| `annotation` | `ShapeAnnotationModel` | Only `annotation.style` is applicable. The `annotation.content` property is ignored. |

---

## Add or Remove Fields at Runtime

Use `addErField` and `removeErField` diagram methods to modify entity fields after initial render.

### Add a field

```tsx
let entityNode = diagramInstance.nodes[0];
let newField = {
  id: 'customer_phone',
  name: 'Phone',
  dataType: 'VARCHAR(20)',
};
// Append to the end
diagramInstance.addErField(entityNode, newField);

// Insert at a specific index (e.g., position 2)
diagramInstance.addErField(entityNode, newField, 2);
```

### Remove a field

```tsx
let fieldToRemove = entityNode.shape.fields.find(
  field => field.id === 'customer_phone'
);

if (fieldToRemove) {
  diagramInstance.removeErField(entityNode, fieldToRemove);
}
```

---

## Configure Default Field Appearance

The `fieldDefaults` property sets the default visual appearance for all fields in an ER entity node. Individual field-level styles override these defaults.

| Property | Description |
|---|---|
| `alternateRowColors` | Array of exactly two colors cycled across rows. Row 0 → `[0]`, Row 1 → `[1]`, Row 2 → `[0]`, etc. |
| `height` | Default height of each field row in pixels. |

```tsx
shape: {
  type: 'Er',
  header: { annotation: { content: 'Customer' } },
  fields: [ /* ... */ ],
  fieldDefaults: {
    alternateRowColors: ['#ffffff', '#E7F0F7'],
    height: 30
  }
} as ErShapeModel
```

---

## Style ER Entities and Fields

- **Node-level style** (`node.style`) controls the overall entity border and background.
- **Field-level style** (`field.style`) overrides node-level and `fieldDefaults` styles for individual rows.
- **Header style** (`shape.header.style`) controls the header fill color separately.

```tsx
const customer: NodeModel = {
  id: 'Customer',
  offsetX: 500,
  offsetY: 200,
  shape: {
    type: 'Er',
    header: {
      annotation: { content: 'CUSTOMER TABLE', style: { bold: true, color: 'white' } },
      height: 35,
      style: { fill: '#2E75B6' }
    },
    fields: [
      {
        id: 'cust_id',
        name: 'CustomerID',
        dataType: 'INT',
        isPrimaryKey: true
      },
      {
        id: 'cust_email',
        name: 'Email',
        dataType: 'VARCHAR(100)',
        style: { fill: '#FFE699' }   // field-level style override
      }
    ] as ErFieldModel[],
    fieldDefaults: {
      alternateRowColors: ['#ffffff', '#E7F0F7']
    }
  } as ErShapeModel,
  style: {
    fill: '#ffffff',
    strokeColor: '#2E75B6',
    strokeWidth: 1
  }
};
```

> Field-level styles override applicable node-level and field default styles.

---

## Track Entity Field Changes

The `erEntityChanged` event fires when ER entity fields are added, removed, or reordered. Use it to track modifications, validate changes, or sync with an external data source.

```tsx
import { IErEntityChangedEventArgs } from '@syncfusion/ej2-diagrams';

<DiagramComponent
  id="container"
  width={'100%'}
  height={'600px'}
  nodes={[customer]}
  erEntityChanged={(args: IErEntityChangedEventArgs) => {
    // ER fields can be reordered using drag-and-drop within the entity.
    if (args.cause === 'FieldsReorder' && args.state === 'Completed') {
        console.log('ER fields reordered successfully.');
    }
    if (args.cause === 'FieldsAdd') {
        console.log('Field Added');
    }
    if (args.cause === 'FieldsRemove') {
        console.log('Field Removed');
    }
  }}
/>
```

---

## Creating ER Relationships

Relationships are ER connectors that link two entity nodes. Set connector `shape.type` to `'Er'` and use `ErConnectorShapeModel` properties to configure multiplicity.

| Property | Description |
|---|---|
| `type` | Set to `'Er'` to activate the ER connector shape. |
| `relationship` | Whether the relationship is identifying or non-identifying. |
| `sourceMultiplicity` | Crow's Foot symbol rendered at the source end. |
| `targetMultiplicity` | Crow's Foot symbol rendered at the target end. |

```tsx
import { ConnectorModel, ErConnectorShapeModel } from "@syncfusion/ej2-diagrams";

Diagram.Inject(ErDiagrams);

const relationship: ConnectorModel = {
  id: 'customer_order',
  sourceID: 'Customer',
  targetID: 'Order',
  shape: {
    type: 'Er',
    relationship: 'NonIdentifying',
    sourceMultiplicity: { type: 'One' },
    targetMultiplicity: { type: 'OneOrMany' }
  } as ErConnectorShapeModel,
  style: { strokeColor: '#7c3aed', strokeWidth: 1.5 },
  sourceDecorator: { style: { strokeColor: '#7c3aed', strokeWidth: 1.5 } },
  targetDecorator: { style: { strokeColor: '#7c3aed', strokeWidth: 1.5 } }
};
```

---

## Relationship Multiplicity

Multiplicity is represented using Crow's Foot notation at each end of an ER connector.

| Multiplicity Type | Meaning | Example Use Case |
|---|---|---|
| `One` | Single participation marker | A customer has one primary account |
| `OneAndOnlyOne` | Exactly one mandatory instance | A user must have exactly one profile |
| `Many` | Multiple instances | A customer can have many orders |
| `ZeroOrOne` | Zero or one instance | An employee may have zero or one manager badge |
| `OneOrMany` | One or more instances | A department must have one or more employees |
| `ZeroOrMany` | Zero or more instances | A customer may have zero or more wish list items |

---

## Complete Multi-Table Example

A full ER diagram showing Customer, Order, and Product entities with relationships:

```tsx
import * as React from "react";
import * as ReactDOM from "react-dom/client";
import { DiagramComponent, Diagram, ErDiagrams } from "@syncfusion/ej2-react-diagrams";
import { NodeModel, ConnectorModel, ErShapeModel, ErFieldModel, ErConnectorShapeModel } from "@syncfusion/ej2-diagrams";

Diagram.Inject(ErDiagrams);

const customer: NodeModel = {
  id: 'Customer',
  offsetX: 250,
  offsetY: 200,
  shape: {
    type: 'Er',
    header: {
      annotation: { content: 'Customer', style: { bold: true, color: 'white' } },
      height: 35,
      style: { fill: '#2E75B6' }
    },
    fields: [
      { id: 'cust_id', name: 'CustomerID', dataType: 'INT', isPrimaryKey: true, constraints: ['NotNull'] },
      { id: 'cust_name', name: 'FirstName', dataType: 'VARCHAR(50)', constraints: ['NotNull'] },
      { id: 'cust_email', name: 'Email', dataType: 'VARCHAR(100)', constraints: ['Unique'] }
    ] as ErFieldModel[],
    fieldDefaults: { alternateRowColors: ['#ffffff', '#E7F0F7'] }
  } as ErShapeModel,
  style: { fill: '#ffffff', strokeColor: '#2E75B6', strokeWidth: 1 }
};

const order: NodeModel = {
  id: 'Order',
  offsetX: 750,
  offsetY: 200,
  shape: {
    type: 'Er',
    header: {
      annotation: { content: 'Order', style: { bold: true, color: 'white' } },
      height: 35,
      style: { fill: '#7c3aed' }
    },
    fields: [
      { id: 'order_id', name: 'OrderID', dataType: 'INT', isPrimaryKey: true, constraints: ['NotNull'] },
      { id: 'order_cust_id', name: 'CustomerID', dataType: 'INT', isForeignKey: true },
      { id: 'order_date', name: 'OrderDate', dataType: 'DATE', constraints: ['NotNull'] }
    ] as ErFieldModel[],
    fieldDefaults: { alternateRowColors: ['#ffffff', '#F3E8FF'] }
  } as ErShapeModel,
  style: { fill: '#ffffff', strokeColor: '#7c3aed', strokeWidth: 1 }
};

const product: NodeModel = {
  id: 'Product',
  offsetX: 750,
  offsetY: 500,
  shape: {
    type: 'Er',
    header: {
      annotation: { content: 'Product', style: { bold: true, color: 'white' } },
      height: 35,
      style: { fill: '#70AD47' }
    },
    fields: [
      { id: 'prod_id', name: 'ProductID', dataType: 'INT', isPrimaryKey: true, constraints: ['NotNull'] },
      { id: 'prod_name', name: 'ProductName', dataType: 'VARCHAR(150)', constraints: ['NotNull'] },
      { id: 'prod_price', name: 'Price', dataType: 'DECIMAL(10,2)', constraints: ['NotNull'] }
    ] as ErFieldModel[],
    fieldDefaults: { alternateRowColors: ['#ffffff', '#F2F2F2'] }
  } as ErShapeModel,
  style: { fill: '#ffffff', strokeColor: '#70AD47', strokeWidth: 1 }
};

const connectors: ConnectorModel[] = [
  {
    id: 'cust_order',
    sourceID: 'Customer',
    targetID: 'Order',
    shape: {
      type: 'Er',
      relationship: 'NonIdentifying',
      sourceMultiplicity: { type: 'One' },
      targetMultiplicity: { type: 'ZeroOrMany' }
    } as ErConnectorShapeModel,
    style: { strokeColor: '#7c3aed', strokeWidth: 1.5 }
  },
  {
    id: 'order_product',
    sourceID: 'Order',
    targetID: 'Product',
    shape: {
      type: 'Er',
      relationship: 'Identifying',
      sourceMultiplicity: { type: 'OneOrMany' },
      targetMultiplicity: { type: 'One' }
    } as ErConnectorShapeModel,
    style: { strokeColor: '#70AD47', strokeWidth: 1.5 }
  }
];

function App() {
  return (
    <DiagramComponent
      id="container"
      width={'100%'}
      height={'700px'}
      nodes={[customer, order, product]}
      connectors={connectors}
    />
  );
}
const root = ReactDOM.createRoot(document.getElementById('diagram'));
root.render(<App />);
```
