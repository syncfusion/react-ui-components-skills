# Editor Events in React Modern Rich Text Editor

## Table of Contents
- [change](#change)
- [created](#created)
- [destroyed](#destroyed)
- [focus](#focus)
- [blur](#blur)
- [actionBegin](#actionbegin)
- [actionComplete](#actioncomplete)
- [Event Summary](#event-summary)

The Modern Rich Text Editor component emits various events to notify your application about user interactions, state changes, and content modifications. This guide covers all available events and how to handle them.

---

- [API Reference](#api-reference)
## change

The `change` event is raised whenever the editor content changes, such as when text is inserted, deleted, moved, replaced, or formatted. Its event arguments provide the updated document and selection states, the type of action performed, and the nodes affected by the change.

The following example demonstrates how to handle the `change` event and display the action performed along with the primary affected element.

```tsx
import * as React from 'react';
import { RichTextEditorUIComponent } from '@syncfusion/ej2-react-richtexteditor-ui';

const toolbarSettings = {
    items: [
        'Bold', 'Italic', 'Underline', '|',
        'BulletList', 'NumberedList', '|',
        'Undo', 'Redo'
    ]
};

function App() {
    const [eventLog, setEventLog] = React.useState<string>('');

    const handleChange = (args: any) => {
        const action = args.action || 'Unknown';
        const affectedNodes = args.affectedNodes || [];
        const nodeType = affectedNodes.length > 0 && affectedNodes[0].type
            ? affectedNodes[0].type
            : 'None';

        const logEntry = `Action: ${action} | Affected Node: ${nodeType}`;
        setEventLog(logEntry);
    };

    return (
        <div className="container">
            <RichTextEditorUIComponent
                toolbarSettings={toolbarSettings}
                placeholder='Type to track content changes...'
                change={handleChange}
            />
            <div className="event-log">
                <div>{eventLog || 'No changes detected yet'}</div>
            </div>
        </div>
    );
}

export default App;
```

---

## created

The `created` event is raised after the Rich Text Editor UI component has been successfully initialized and rendered. Use this event to perform setup tasks that depend on the editor instance or its rendered DOM elements.

The following example demonstrates how to handle the `created` event and display a notification confirming that the editor is ready for interaction.

```tsx
import * as React from 'react';
import { RichTextEditorUIComponent } from '@syncfusion/ej2-react-richtexteditor-ui';

const toolbarSettings = {
    items: [
        'Bold', 'Italic', 'Underline', '|',
        'BulletList', 'NumberedList', '|',
        'Undo', 'Redo'
    ]
};

function App() {
    const [eventStatus, setEventStatus] = React.useState('');

    const handleCreated = () => {
        setEventStatus(`Editor component created at ${new Date().toLocaleTimeString()}`);
    };

    return (
        <div className="container">
            <RichTextEditorUIComponent
                toolbarSettings={toolbarSettings}
                placeholder='Editor initialized...'
                created={handleCreated}
            />
            <div className="event-status">
                {eventStatus || 'Waiting for created event...'}
            </div>
        </div>
    );
}

export default App;
```

---

## destroyed

The `destroyed` event is raised when the Rich Text Editor component is destroyed and removed from the application. It can be used to perform cleanup tasks, such as releasing resources, removing event handlers, or resetting related state.

The following example demonstrates how to handle the `destroyed` event and log the destruction notification to confirm cleanup and resource release.

```tsx
import * as React from 'react';
import { RichTextEditorUIComponent } from '@syncfusion/ej2-react-richtexteditor-ui';

const toolbarSettings = {
    items: [
        'Bold', 'Italic', 'Underline', '|',
        'BulletList', 'NumberedList', '|',
        'Undo', 'Redo'
    ]
};

function App() {
    const [eventStatus, setEventStatus] = React.useState('');

    const handleDestroyed = () => {
        setEventStatus(`Editor component destroyed at ${new Date().toLocaleTimeString()}`);
    };

    return (
        <div className="container">
            <RichTextEditorUIComponent
                toolbarSettings={toolbarSettings}
                placeholder='Type something...'
                destroyed={handleDestroyed}
            />
            <div className="event-status">
                {eventStatus || 'Editor component active'}
            </div>
        </div>
    );
}

export default App;
```

---

## focus

The `focus` event is raised when the editor receives focus, either through user interaction or a method call. Its event arguments identify the event name, indicate whether the focus was user-initiated, and specify the focus source.

The following example demonstrates how to handle the `focus` event and log a notification indicating that the editor has received focus and is ready for user input.

```tsx
import * as React from 'react';
import { RichTextEditorUIComponent } from '@syncfusion/ej2-react-richtexteditor-ui';

const toolbarSettings = {
    items: [
        'Bold', 'Italic', 'Underline', '|',
        'BulletList', 'NumberedList', '|',
        'Undo', 'Redo'
    ]
};

function App() {
    const [eventStatus, setEventStatus] = React.useState('');

    const handleFocus = () => {
        setEventStatus(`Focus event triggered at ${new Date().toLocaleTimeString()}`);
    };

    return (
        <div className="container">
            <RichTextEditorUIComponent
                toolbarSettings={toolbarSettings}
                placeholder='Click in the editor to trigger focus event...'
                focus={handleFocus}
            />
            <div className="event-status">
                {eventStatus || 'No focus event yet'}
            </div>
        </div>
    );
}

export default App;
```

---

## blur

The `blur` event is raised when the editor loses focus, either through user interaction or a method call. Its event arguments identify the event name, indicate whether the blur was caused by user interaction, provide the focus event, and specify the source.

The following example demonstrates how to handle the `blur` event and log a notification indicating that the editor has lost focus and user input has ceased.

```tsx
import * as React from 'react';
import { RichTextEditorUIComponent } from '@syncfusion/ej2-react-richtexteditor-ui';

const toolbarSettings = {
    items: [
        'Bold', 'Italic', 'Underline', '|',
        'BulletList', 'NumberedList', '|',
        'Undo', 'Redo'
    ]
};

function App() {
    const [eventStatus, setEventStatus] = React.useState('');

    const handleBlur = () => {
        setEventStatus('Editor lost focus (blur event triggered)');
    };

    const handleFocus = () => {
        setEventStatus('Editor received focus (focus event triggered)');
    };

    return (
        <div className="container">
            <RichTextEditorUIComponent
                toolbarSettings={toolbarSettings}
                placeholder='Click to focus or click outside to blur...'
                blur={handleBlur}
                focus={handleFocus}
            />
            <div className="event-status">
                {eventStatus || 'No event triggered yet'}
            </div>
        </div>
    );
}

export default App;
```

---

## actionBegin

The `actionBegin` event is raised before an editor action is executed, such as inserting, deleting, formatting, or modifying content. It provides details about the pending action and allows applications to monitor or customize the editor behavior before the operation begins.

The following example demonstrates how to handle the `actionBegin` event and log pre-action events with timestamps to track when operations are initiated before they complete.

```tsx
import * as React from 'react';
import { RichTextEditorUIComponent } from '@syncfusion/ej2-react-richtexteditor-ui';

const toolbarSettings = {
    items: [
        'Bold', 'Italic', 'Underline', 'Strikethrough', '|',
        'BulletList', 'NumberedList', '|',
        'Link', 'Image', 'Table', '|',
        'Undo', 'Redo'
    ]
};

function App() {
    const [logs, setLogs] = React.useState<string[]>([]);

    const handleActionBegin = (args: any) => {
        const timestamp = new Date().toLocaleTimeString();
        const logEntry = `[${timestamp}] Action: ${args.action || 'Unknown'}`;
        setLogs([logEntry, ...logs.slice(0, 49)]);
    };

    const clearLogs = () => {
        setLogs([]);
    };

    return (
        <div className="event-container">
            <div>
                <RichTextEditorUIComponent
                    toolbarSettings={toolbarSettings}
                    placeholder='Type something to trigger actionBegin events...'
                    actionBegin={handleActionBegin}
                />
            </div>
            <div>
                <button onClick={clearLogs}>Clear Logs</button>
                <div className="event-log">
                    {logs.map((log, index) => (
                        <div key={index} className="event-row">
                            {log}
                        </div>
                    ))}
                </div>
            </div>
        </div>
    );
}

export default App;
```

---

## actionComplete

The `actionComplete` event is raised after an editor action, such as inserting, deleting, formatting, or modifying content, has been completed. It provides details about the completed action so applications can respond, update related state, or perform follow-up processing.

The following example demonstrates how to handle the `actionComplete` event and log post-action events with timestamps to track when operations have successfully finished and are ready for follow-up processing.

```tsx
import * as React from 'react';
import { RichTextEditorUIComponent } from '@syncfusion/ej2-react-richtexteditor-ui';

const toolbarSettings = {
    items: [
        'Bold', 'Italic', 'Underline', 'Strikethrough', '|',
        'BulletList', 'NumberedList', '|',
        'Undo', 'Redo'
    ]
};

function App() {
    const [eventLog, setEventLog] = React.useState('');

    const handleActionComplete = (args: any) => {
        const timestamp = new Date().toLocaleTimeString();
        const logEntry = `[${timestamp}] Action Complete: ${args.action || 'Unknown'}`;
        setEventLog(logEntry);
    };

    return (
        <div className="container">
            <RichTextEditorUIComponent
                toolbarSettings={toolbarSettings}
                placeholder='Perform actions to trigger actionComplete events...'
                actionComplete={handleActionComplete}
            />
            <div className="event-log">
                {eventLog || 'No action complete event yet'}
            </div>
        </div>
    );
}

export default App;
```

---

## Event Summary

| Event | Raised when | Passed as prop |
| --- | --- | --- |
| `change` | Editor content changes (insert, delete, move, replace, format) | `change` |
| `created` | The component is initialized and rendered | `created` |
| `destroyed` | The component is destroyed and removed from the application | `destroyed` |
| `focus` | The editor receives focus | `focus` |
| `blur` | The editor loses focus | `blur` |
| `actionBegin` | Before an editor action executes | `actionBegin` |
| `actionComplete` | After an editor action completes | `actionComplete` |

## Settings-Level Events

These events are declared inside a settings object rather than passed as a top-level prop:

| Settings | Event | Purpose |
| --- | --- | --- |
| `toolbarSettings` | `itemClicked` | Fires before any toolbar button's command runs. Branch on `args.item.actionId` to handle custom items. See [toolbar.md](toolbar.md#itemclicked-event). |
| `toolbarSettings` | `updatedToolbarStatus` | Fires after the toolbar syncs its visual state with the current selection. See [toolbar.md](toolbar.md#updatedtoolbarstatus-event). |
| `slashCommandSettings` | `itemSelect` | Fires when the user picks an item from the slash-command popup. See [features.md](features.md#use-the-itemselect-event). |

## Image Upload Events

These events are declared on the top-level component in the image upload scenarios:

| Event | Purpose |
| --- | --- |
| `onFileUploading` | Access the current request and set a request header before the upload is sent. See [image.md](image.md#secure-image-upload-with-authentication). |
| `onFileUploadSuccess` | Handle the server response after a successful upload. See [image.md](image.md#rename-images-before-inserting). |

## API Reference


## ChangeEventArgs

**Event Arguments** — Arguments passed to the `change` event.

### Properties

#### `action` (DocumentChangeAction)
Semantic action describing the nature of the change:
- `'Insertion'` — Content was added (typing, paste, insert command)
- `'Deletion'` — Content was removed (backspace, delete, remove command)
- `'Moved'` — Content was relocated (drag-drop, reorder, move command)
- `'Replaced'` — Content was swapped (find-replace, transform)
- `'Update'` — Attributes or formatting changed without structural change (heading level, text color)
- `'Unknown'` — Transaction too complex to categorize; examine `affectedNodes` for details

#### `affectedNodes` (EditorNode[])
All Headless Editor nodes touched by this transaction. Populated based on action:
- `'Insertion'` — Newly inserted nodes and their ancestors
- `'Deletion'` — Nodes that existed before deletion (for archival/undo preview)
- `'Moved'` — Relocated nodes
- `'Replaced'` — Both old and new nodes
- `'Update'` — Mutated nodes
- `'Unknown'` — All nodes with changes (best effort)

#### `document` (EditorDocument)
The document state after the change.

#### `selection` (Selection)
The selection state after the change.

---

## BeforePopupOpenEventArgs

**Event Arguments** — Event args for `beforePopupOpen`. Extends the ej2 `PopupEventArgs` with the `source` field that identifies the editor feature (e.g., `'Link'`, `'Image'`, `'Table'`).

### Properties

#### `animation` (AnimationModel)
Specifies the animation Object.

#### `cancel` (boolean)
Illustrates whether the current action needs to be prevented or not.

#### `event` (MouseEvent | KeyboardEventArgs | TouchEvent | Object)
Specifies the event.

#### `popup` (Popup)
Specifies the popup Object.

#### `source` (string)
The editor feature the sub-component belongs to.

---

## BeforePopupCloseEventArgs

**Event Arguments** — Event args for `beforePopupClose`. Extends the ej2 `BeforeCloseEventArgs` with the `source` field.

### Properties

#### `cancel` (boolean)
Defines whether the current action can be prevented.

#### `closedBy` (string)
Returns how the dialog was closed: "close icon", "overlayClick", "escape", or "user action"

#### `container` (HTMLElement)
Root container element of the dialog.

#### `element` (Element)
Element of the dialog.

#### `event` (Event)
Original event arguments.

#### `isInteracted` (boolean)
Determines whether the event is triggered by interaction.

#### `preventFocus` (boolean)
Whether to prevent focus from returning to the previously active element.

#### `source` (string)
The editor feature the sub-component belongs to.

#### `target` (HTMLElement | string)
Target element of the dialog.

---

## BeforeDialogOpenEventArgs

**Event Arguments** — Event args for `beforeDialogOpen`. Extends the ej2 `BeforeOpenEventArgs` with the `source` field.

### Properties

#### `cancel` (boolean)
Defines whether the current action can be prevented.

#### `container` (HTMLElement)
Root container element of the dialog.

#### `element` (Element)
Element of the dialog.

#### `maxHeight` (string)
Value to override max-height of dialog.

#### `source` (string)
The editor feature the dialog belongs to (e.g., `'Link'`, `'Image'`, `'Table'`).

#### `target` (HTMLElement | string)
Target element of the dialog.

---

## BeforeDialogCloseEventArgs

**Event Arguments** — Event args for `beforeDialogClose`. Extends the ej2 `BeforeCloseEventArgs` with the `source` field.

### Properties

#### `cancel` (boolean)
Defines whether the current action can be prevented.

#### `closedBy` (string)
Returns how the dialog was closed.

#### `container` (HTMLElement)
Root container element of the dialog.

#### `element` (Element)
Element of the dialog.

#### `event` (Event)
Original event arguments.

#### `isInteracted` (boolean)
Determines whether the event is triggered by interaction.

#### `preventFocus` (boolean)
Whether to prevent focus from returning to the previously active element.

#### `source` (string)
The editor feature the dialog belongs to.

#### `target` (HTMLElement | string)
Target element of the dialog.

---

## EditorComponentEventArgs

**Base Event Arguments** — Editor feature (`source`) carried by every sub-component event argument raised on the editor. The `source` is a plain string set at sub-component initialization (e.g. `'Link'`, `'Image'`, `'Table'`) identifying which feature the dialog, popup, or upload belongs to. A single handler can branch on `args.source` rather than wiring one callback per feature.

### Properties

#### `source` (string)
The editor feature the sub-component belongs to. Supplied by the caller when the sub-component is created. Undefined for generic sub-components that are not tied to a specific feature.

---

## API Reference

## ChangeEventArgs

**Event Arguments** — Arguments passed to the `change` event.

### Properties

#### `action` (DocumentChangeAction)
Semantic action describing the nature of the change:
- `'Insertion'` — Content was added (typing, paste, insert command)
- `'Deletion'` — Content was removed (backspace, delete, remove command)
- `'Moved'` — Content was relocated (drag-drop, reorder, move command)
- `'Replaced'` — Content was swapped (find-replace, transform)
- `'Update'` — Attributes or formatting changed without structural change (heading level, text color)
- `'Unknown'` — Transaction too complex to categorize; examine `affectedNodes` for details

#### `affectedNodes` (EditorNode[])
All Headless Editor nodes touched by this transaction. Populated based on action:
- `'Insertion'` — Newly inserted nodes and their ancestors
- `'Deletion'` — Nodes that existed before deletion (for archival/undo preview)
- `'Moved'` — Relocated nodes
- `'Replaced'` — Both old and new nodes
- `'Update'` — Mutated nodes
- `'Unknown'` — All nodes with changes (best effort)

#### `document` (EditorDocument)
The document state after the change.

#### `selection` (Selection)
The selection state after the change.

---
## BeforePopupOpenEventArgs

**Event Arguments** — Event args for `beforePopupOpen`. Extends the ej2 `PopupEventArgs` with the `source` field that identifies the editor feature (e.g., `'Link'`, `'Image'`, `'Table'`).

### Properties

#### `animation` (AnimationModel)
Specifies the animation Object.

#### `cancel` (boolean)
Illustrates whether the current action needs to be prevented or not.

#### `event` (MouseEvent | KeyboardEventArgs | TouchEvent | Object)
Specifies the event.

#### `popup` (Popup)
Specifies the popup Object.

#### `source` (string)
The editor feature the sub-component belongs to.

---
## BeforePopupCloseEventArgs

**Event Arguments** — Event args for `beforePopupClose`. Extends the ej2 `BeforeCloseEventArgs` with the `source` field.

### Properties

#### `cancel` (boolean)
Defines whether the current action can be prevented.

#### `closedBy` (string)
Returns how the dialog was closed: "close icon", "overlayClick", "escape", or "user action"

#### `container` (HTMLElement)
Root container element of the dialog.

#### `element` (Element)
Element of the dialog.

#### `event` (Event)
Original event arguments.

#### `isInteracted` (boolean)
Determines whether the event is triggered by interaction.

#### `preventFocus` (boolean)
Whether to prevent focus from returning to the previously active element.

#### `source` (string)
The editor feature the sub-component belongs to.

#### `target` (HTMLElement | string)
Target element of the dialog.

---
## BeforeDialogOpenEventArgs

**Event Arguments** — Event args for `beforeDialogOpen`. Extends the ej2 `BeforeOpenEventArgs` with the `source` field.

### Properties

#### `cancel` (boolean)
Defines whether the current action can be prevented.

#### `container` (HTMLElement)
Root container element of the dialog.

#### `element` (Element)
Element of the dialog.

#### `maxHeight` (string)
Value to override max-height of dialog.

#### `source` (string)
The editor feature the dialog belongs to (e.g., `'Link'`, `'Image'`, `'Table'`).

#### `target` (HTMLElement | string)
Target element of the dialog.

---
## BeforeDialogCloseEventArgs

**Event Arguments** — Event args for `beforeDialogClose`. Extends the ej2 `BeforeCloseEventArgs` with the `source` field.

### Properties

#### `cancel` (boolean)
Defines whether the current action can be prevented.

#### `closedBy` (string)
Returns how the dialog was closed.

#### `container` (HTMLElement)
Root container element of the dialog.

#### `element` (Element)
Element of the dialog.

#### `event` (Event)
Original event arguments.

#### `isInteracted` (boolean)
Determines whether the event is triggered by interaction.

#### `preventFocus` (boolean)
Whether to prevent focus from returning to the previously active element.

#### `source` (string)
The editor feature the dialog belongs to.

#### `target` (HTMLElement | string)
Target element of the dialog.

---
## EditorComponentEventArgs

**Base Event Arguments** — Editor feature (`source`) carried by every sub-component event argument raised on the editor. The `source` is a plain string set at sub-component initialization (e.g. `'Link'`, `'Image'`, `'Table'`) identifying which feature the dialog, popup, or upload belongs to. A single handler can branch on `args.source` rather than wiring one callback per feature.

### Properties

#### `source` (string)
The editor feature the sub-component belongs to. Supplied by the caller when the sub-component is created. Undefined for generic sub-components that are not tied to a specific feature.

---
