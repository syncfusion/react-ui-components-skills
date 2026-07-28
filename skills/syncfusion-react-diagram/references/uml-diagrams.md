# UML Diagrams in Syncfusion React Diagram

## Table of Contents
- [UML Class Diagrams](#uml-class-diagrams)
- [Class Shape](#class-shape)
- [Interface Shape](#interface-shape)
- [Enumeration Shape](#enumeration-shape)
- [UML Class Relationships](#uml-class-relationships)
- [UML Sequence Diagrams](#uml-sequence-diagrams)
- [Sequence Diagram Participants](#sequence-diagram-participants)
- [Sequence Diagram Messages](#sequence-diagram-messages)
- [Activation Boxes](#activation-boxes)
- [Fragments](#fragments)
- [Advanced Properties](#advanced-properties)
- [Troubleshooting](#troubleshooting)

---

## UML Class Diagrams

UML Class Diagrams model the static structure of a system using classes, interfaces, enumerations, and their relationships. Set `shape.type: 'UmlClassifier'` and configure the `classifier` property to select the element type.

No additional module injection is required for UML Class Diagrams — they work out of the box.

---

## Class Shape

A class has a name, attributes (fields), and methods.

```tsx
import { DiagramComponent, NodeModel, UmlClassifierShapeModel } from '@syncfusion/ej2-react-diagrams';

const nodes: NodeModel[] = [
  {
    id: 'OrderClass',
    offsetX: 200, offsetY: 250,
    style: { fill: '#26A0DA', strokeColor: '#1a6fa0' },
    shape: {
      type: 'UmlClassifier',
      classifier: 'Class',
      classShape: {
        name: 'Order',
        attributes: [
          { name: 'id',          type: 'int',    scope: 'Private' },
          { name: 'customerName', type: 'string', scope: 'Public' },
          { name: 'total',       type: 'double', scope: 'Protected' }
        ],
        methods: [
          {
            name: 'placeOrder',
            type: 'void',
            scope: 'Public',
            parameters: [
              { name: 'customerId', type: 'int' }
            ]
          },
          { name: 'cancel', type: 'bool', scope: 'Public' }
        ]
      }
    } as UmlClassifierShapeModel
  }
];

export default function App() {
  return (
    <DiagramComponent id="diagram" width={'100%'} height={'600px'} nodes={nodes} />
  );
}
```

### Scope (visibility) values

`'Public'` (+) | `'Private'` (-) | `'Protected'` (#) | `'Package'` (~)

---

## Interface Shape

An interface declares a contract: method signatures without implementation details.

```tsx
const nodes: NodeModel[] = [
  {
    id: 'PaymentInterface',
    offsetX: 450, offsetY: 250,
    style: { fill: '#26A0DA' },
    shape: {
      type: 'UmlClassifier',
      classifier: 'Interface',
      interfaceShape: {
        name: 'IPayment',
        attributes: [
          { name: 'amount', type: 'double', scope: 'Public' }
        ],
        methods: [
          {
            name: 'process',
            type: 'bool',
            scope: 'Public',
            parameters: [{ name: 'amount', type: 'double' }]
          },
          { name: 'refund', type: 'void', scope: 'Public' }
        ]
      }
    } as UmlClassifierShapeModel
  }
];
```

---

## Enumeration Shape

An enumeration defines a fixed set of named constants.

```tsx
const nodes: NodeModel[] = [
  {
    id: 'StatusEnum',
    offsetX: 300, offsetY: 450,
    style: { fill: '#26A0DA' },
    shape: {
      type: 'UmlClassifier',
      classifier: 'Enumeration',
      enumerationShape: {
        name: 'OrderStatus',
        members: [
          { name: 'Pending' },
          { name: 'Processing' },
          { name: 'Shipped' },
          { name: 'Delivered' },
          { name: 'Cancelled' }
        ]
      }
    } as UmlClassifierShapeModel
  }
];
```

---

## UML Class Relationships

Relationships between classes are modeled as connectors with `shape.type: 'UmlClassifier'` and a `relationship` property.

### Relationship types and connector decorators

| `relationship` | Decorator | Description |
|---------------|-----------|-------------|
| `'Association'` | Arrow | General relationship |
| `'Aggregation'` | Hollow diamond at source | Has-A (weak ownership) |
| `'Composition'` | Filled diamond at source | Has-A (strong ownership) |
| `'Inheritance'` | Open triangle at target | Is-A (extends) |
| `'Dependency'` | Dashed arrow | Uses-A |
| `'Realization'` | Dashed open triangle | Implements |

```tsx
import { ConnectorModel } from '@syncfusion/ej2-react-diagrams';

const connectors: ConnectorModel[] = [
  // Inheritance: OrderClass extends BaseEntity
  {
    id: 'inherit1',
    sourceID: 'OrderClass', targetID: 'BaseEntity',
    type: 'Straight',
    shape: {
      type: 'UmlClassifier',
      relationship: 'Inheritance'
    }
  },
  // Aggregation: Order has many Items
  {
    id: 'agg1',
    sourceID: 'OrderClass', targetID: 'ItemClass',
    type: 'Straight',
    shape: {
      type: 'UmlClassifier',
      relationship: 'Aggregation'
    }
  },
  // Realization: PaymentService implements IPayment
  {
    id: 'real1',
    sourceID: 'PaymentService', targetID: 'PaymentInterface',
    type: 'Straight',
    shape: {
      type: 'UmlClassifier',
      relationship: 'Realization'
    }
  },
  // Directional Association
  {
    id: 'assoc1',
    sourceID: 'OrderClass', targetID: 'CustomerClass',
    type: 'Straight',
    shape: {
      type: 'UmlClassifier',
      relationship: 'Association',
      association: 'Directional'   // 'Directional' | 'BiDirectional'
    }
  }
];
```

---

## UML Sequence Diagrams

UML Sequence Diagrams model how objects interact over time through a series of messages. Use the `model` prop on `DiagramComponent` with a `UmlSequenceDiagramModel` object.

```tsx
import { DiagramComponent, SnapConstraints } from '@syncfusion/ej2-react-diagrams';
import { UmlSequenceDiagramModel } from '@syncfusion/ej2-diagrams';

const umlSequenceDiagramModel: UmlSequenceDiagramModel = {
  participants: [...],
  messages: [...]
};

export default function App() {
  return (
    <DiagramComponent
      id="seqDiagram"
      width={'100%'} height={'600px'}
      model={umlSequenceDiagramModel}
      snapSettings={{ constraints: SnapConstraints.None }}
    />
  );
}
```

---

## Sequence Diagram Participants

Participants appear at the top with vertical lifelines. Use the `stereotype` property (`UmlSequenceParticipantStereotype`) to control the visual style of each participant header.

### Participant Stereotype Values

| Stereotype | Description |
|-----------|-------------|
| `Default` | Standard labeled rectangle (default) |
| `Actor` | External person/system — stick figure |
| `Boundary` | UI or API gateway interface |
| `Control` | Controller/coordinator |
| `Entity` | Domain object or stored data |
| `Database` | Persistent storage — cylindrical shape |

```tsx
import { DiagramComponent, SnapConstraints, UmlSequenceDiagramModel, UmlSequenceParticipantStereotype } from '@syncfusion/ej2-react-diagrams';

const umlSequenceDiagramModel: UmlSequenceDiagramModel = {
  participants: [
    {
      id: 'Customer',
      content: 'Customer',
      stereotype: UmlSequenceParticipantStereotype.Actor     // stick figure
    },
    {
      id: 'WebServer',
      content: 'Web Server',
      stereotype: UmlSequenceParticipantStereotype.Control,
      showDestructionMarker: false                           // X at lifeline end
    },
    {
      id: 'Database',
      content: 'Database',
      stereotype: UmlSequenceParticipantStereotype.Database,
      showDestructionMarker: true
    }
  ]
};
```

---

## Sequence Diagram Messages

Messages are arrows between lifelines showing communication. Configure in the `messages` array.

### Message Types

| Type | Description |
|------|-------------|
| `Synchronous` | Sender waits for a response |
| `Asynchronous` | Sender continues without waiting |
| `Reply` | Response to a previous message |
| `Create` | Creates a new participant |
| `Delete` | Terminates a participant |
| `Self` | Message from a participant to itself |

```tsx
import { DiagramComponent, SnapConstraints, UmlSequenceDiagramModel, UmlSequenceMessageType, UmlSequenceParticipantStereotype } from '@syncfusion/ej2-react-diagrams';

const umlSequenceDiagramModel: UmlSequenceDiagramModel = {
  participants: [
    { id: 'Customer', content: 'Customer', stereotype: UmlSequenceParticipantStereotype.Actor },
    { id: 'Server',   content: 'Server' },
    { id: 'DB',       content: 'Database', stereotype: UmlSequenceParticipantStereotype.Database }
  ],
  messages: [
    {
      id: 'msg1',
      fromParticipantID: 'Customer',
      toParticipantID: 'Server',
      content: 'POST /orders',
      type: UmlSequenceMessageType.Synchronous
    },
    {
      id: 'msg2',
      fromParticipantID: 'Server',
      toParticipantID: 'DB',
      content: 'INSERT order',
      type: UmlSequenceMessageType.Synchronous
    },
    {
      id: 'msg3',
      fromParticipantID: 'DB',
      toParticipantID: 'Server',
      content: 'OK',
      type: UmlSequenceMessageType.Reply
    },
    {
      id: 'msg4',
      fromParticipantID: 'Server',
      toParticipantID: 'Customer',
      content: '201 Created',
      type: UmlSequenceMessageType.Reply
    }
  ]
};
```

---

## Activation Boxes

`UmlSequenceActivationBoxModel` represents periods when a participant is actively processing. Activation boxes render as thin rectangles on the lifeline, spanning between a start message and an end message.

| Property | Type | Description |
|----------|------|-------------|
| `id` | `string \| number` | Unique identifier |
| `startMessageID` | `string \| number` | Message that starts the activation |
| `endMessageID` | `string \| number` | Message that ends the activation |

Define activation boxes inside each participant:

```tsx
import { DiagramComponent, SnapConstraints, UmlSequenceDiagramModel, UmlSequenceMessageType, UmlSequenceParticipantStereotype } from '@syncfusion/ej2-react-diagrams';

const umlSequenceDiagramModel: UmlSequenceDiagramModel = {
  participants: [
    {
      id: 'Client',
      content: 'Client',
      stereotype: UmlSequenceParticipantStereotype.Actor,
      activationBoxes: [
        { id: 'act1', startMessageID: 'req1', endMessageID: 'res1' }
      ]
    },
    {
      id: 'Server',
      content: 'Server',
      activationBoxes: [
        { id: 'act2', startMessageID: 'req1', endMessageID: 'res1' }
      ]
    }
  ],
  messages: [
    { id: 'req1', fromParticipantID: 'Client', toParticipantID: 'Server', content: 'Request', type: UmlSequenceMessageType.Synchronous },
    { id: 'res1', fromParticipantID: 'Server', toParticipantID: 'Client', content: 'Response', type: UmlSequenceMessageType.Reply }
  ]
};
```

---

## Fragments

`UmlSequenceFragmentModel` groups messages inside a labeled rectangle representing conditional logic, loops, or alternatives.

### Fragment Types

| Type | Description |
|------|-------------|
| `Optional` | Executes only if a condition is met |
| `Alternative` | Multiple if-else paths; one branch executes |
| `Loop` | Repeating sequence based on a loop condition |

### Fragment Model Properties

| Property | Type | Description |
|----------|------|-------------|
| `id` | `string \| number` | Unique identifier |
| `type` | `UmlSequenceFragmentType` | `Optional` \| `Alternative` \| `Loop` |
| `conditions` | `UmlSequenceFragmentConditionModel[]` | One per branch |

### Condition Model Properties

| Property | Type | Description |
|----------|------|-------------|
| `content` | `string` | Condition label text |
| `messageIds` | `(string \| number)[]` | Messages inside this branch |
| `fragmentIds` | `string[]` | Nested fragment IDs (for nesting fragments) |

```tsx
import * as React from "react";
import * as ReactDOM from "react-dom/client";
import { DiagramComponent, SnapConstraints } from '@syncfusion/ej2-react-diagrams';
import {
  UmlSequenceDiagramModel, UmlSequenceMessageType,
  UmlSequenceFragmentType, UmlSequenceParticipantStereotype
} from '@syncfusion/ej2-diagrams';

const model = {
  spaceBetweenParticipants: 300,
  participants: [
    { id: 'Customer',       content: 'Customer',        stereotype: UmlSequenceParticipantStereotype.Actor },
    { id: 'OrderSystem',    content: 'Order System' },
    { id: 'PaymentGateway', content: 'Payment Gateway' }
  ],
  messages: [
    { id: 'MSG1', content: 'Place Order',            fromParticipantID: 'Customer',       toParticipantID: 'OrderSystem',    type: UmlSequenceMessageType.Synchronous },
    { id: 'MSG2', content: 'Check Stock',            fromParticipantID: 'OrderSystem',    toParticipantID: 'OrderSystem',    type: UmlSequenceMessageType.Synchronous },
    { id: 'MSG3', content: 'Stock Available',        fromParticipantID: 'OrderSystem',    toParticipantID: 'Customer',       type: UmlSequenceMessageType.Reply },
    { id: 'MSG4', content: 'Process Payment',        fromParticipantID: 'OrderSystem',    toParticipantID: 'PaymentGateway', type: UmlSequenceMessageType.Synchronous },
    { id: 'MSG5', content: 'Payment Successful',     fromParticipantID: 'PaymentGateway', toParticipantID: 'OrderSystem',    type: UmlSequenceMessageType.Reply },
    { id: 'MSG6', content: 'Order Confirmed',        fromParticipantID: 'OrderSystem',    toParticipantID: 'Customer',       type: UmlSequenceMessageType.Reply },
    { id: 'MSG7', content: 'Payment Failed',         fromParticipantID: 'PaymentGateway', toParticipantID: 'OrderSystem',    type: UmlSequenceMessageType.Reply },
    { id: 'MSG8', content: 'Retry Payment',          fromParticipantID: 'OrderSystem',    toParticipantID: 'Customer',       type: UmlSequenceMessageType.Reply }
  ],
  fragments: [
    // Optional: only if item is in stock
    { id: 1, type: UmlSequenceFragmentType.Optional,
      conditions: [{ content: 'if item is in stock', messageIds: ['MSG4'] }] },
    // Alternative: payment success vs failure
    { id: 2, type: UmlSequenceFragmentType.Alternative,
      conditions: [
        { content: 'if payment is successful', messageIds: ['MSG5', 'MSG6'] },
        { content: 'if payment fails',         messageIds: ['MSG7', 'MSG8'] }
      ]
    },
    // Loop wraps both child fragments
    { id: 3, type: UmlSequenceFragmentType.Loop,
      conditions: [{ content: 'while attempts < 3', fragmentIds: ['1', '2'] }] }
  ]
};

export default function App() {
  return (
    <DiagramComponent id="container" width={'100%'} height={'700px'}
      model={model}
      snapSettings={{ constraints: SnapConstraints.None }}
    />
  );
}

const root = ReactDOM.createRoot(document.getElementById('diagram'));
root.render(<App />);
```

> Use `spaceBetweenParticipants` on the model to increase horizontal spacing when message labels are long.

---

## Advanced Properties

### UML Class node properties

| Property | Type | Description |
|----------|------|-------------|
| `classifier` | `'Class' \| 'Interface' \| 'Enumeration'` | Shape type |
| `classShape.name` | `string` | Class name (header) |
| `classShape.attributes` | `UmlClassAttributeModel[]` | Field list |
| `classShape.methods` | `UmlClassMethodModel[]` | Method list |
| `interfaceShape` | `UmlInterfaceModel` | Interface config |
| `enumerationShape.members` | `UmlEnumerationMemberModel[]` | Enum values |

### UML Attribute / Method properties

| Property | Type | Description |
|----------|------|-------------|
| `name` | `string` | Field/method name |
| `type` | `string` | Data type or return type |
| `scope` | `'Public' \| 'Private' \| 'Protected' \| 'Package'` | Visibility |
| `parameters` | `UmlClassMethodParametersModel[]` | Method parameters (methods only) |

### UML Class relationship values

`'Association'` | `'Aggregation'` | `'Composition'` | `'Inheritance'` | `'Dependency'` | `'Realization'`

### UML Sequence participant properties

| Property | Type | Description |
|----------|------|-------------|
| `id` | `string \| number` | Unique identifier |
| `content` | `string` | Display label |
| `stereotype` | `UmlSequenceParticipantStereotype` | Visual style: `Default` \| `Actor` \| `Boundary` \| `Control` \| `Entity` \| `Database` |
| `showDestructionMarker` | `boolean` | Show X at end of lifeline |
| `activationBoxes` | `UmlSequenceActivationBoxModel[]` | Active-processing periods |

### UML Sequence message properties

| Property | Type | Description |
|----------|------|-------------|
| `id` | `string \| number` | Unique identifier |
| `content` | `string` | Display label |
| `fromParticipantID` | `string \| number` | Sender participant |
| `toParticipantID` | `string \| number` | Receiver participant |
| `type` | `UmlSequenceMessageType` | Message type (see below) |

### UML Sequence message types

`Synchronous` | `Asynchronous` | `Reply` | `Create` | `Delete` | `Self`

### UML Sequence fragment types

`Optional` | `Alternative` | `Loop`

### `UmlSequenceDiagramModel` top-level properties

| Property | Description |
|----------|-------------|
| `participants` | Array of `UmlSequenceParticipantModel` |
| `messages` | Array of `UmlSequenceMessageModel` |
| `fragments` | Array of `UmlSequenceFragmentModel` |
| `spaceBetweenParticipants` | Horizontal spacing (number, e.g. `300`) |

---

## Troubleshooting

**Class shape not displaying attributes/methods**
- Confirm `classifier` matches the shape config key: `'Class'` → `classShape`, `'Interface'` → `interfaceShape`, `'Enumeration'` → `enumerationShape`

**Relationship decorator not showing**
- Ensure connector `shape.type: 'UmlClassifier'` and `relationship` is set to a valid value

**Sequence diagram not rendering**
- Use the `model` prop (not `nodes`/`connectors`) to pass `UmlSequenceDiagramModel`
- Import `UmlSequenceDiagramModel`, `UmlSequenceMessageType`, `UmlSequenceParticipantStereotype`, `UmlSequenceFragmentType` from `'@syncfusion/ej2-diagrams'` (not the react package)

**Participant showing wrong visual style**
- Use `stereotype: UmlSequenceParticipantStereotype.Actor` (not `isActor: true`) — the `stereotype` enum is the current API

**Activation boxes not appearing**
- Define `activationBoxes` inside the participant object, not at the top-level model
- Ensure `startMessageID` and `endMessageID` match valid message IDs

**Fragment not grouping messages**
- For `Alternative`, define multiple objects in `conditions[]` — one per branch
- For nested fragments, use `fragmentIds` in the condition (not `messageIds`)

**Scope symbols not appearing**
- Check `scope` is capitalized exactly: `'Public'`, `'Private'`, `'Protected'`, `'Package'`

**Related docs:**
- Shapes & Styles → [shapes-and-styles.md](shapes-and-styles.md)
- BPMN Diagrams → [bpmn-diagrams.md](bpmn-diagrams.md)
- Data Binding → [data-binding.md](data-binding.md)
