# WebMCP Integration

## Table of Contents

- [WebMCP Integration](#webmcp-integration)
  - [Table of Contents](#table-of-contents)
  - [When to Use](#when-to-use)
  - [Required Configuration](#required-configuration)
  - [Property Documentation](#property-documentation)
  - [WebMcpScheduler Module](#webmcpschedulermodule)
  - [Built-in WebMCP Tools](#built-in-webmcp-tools)
  - [Tool Filtering and Custom Prefixes](#tool-filtering-and-custom-prefixes)
  - [Automatic Tool Registration](#automatic-tool-registration)
  - [Notification-Based Registration](#notification-based-registration)
  - [beforeWebMcpToolExecute Event](#beforewebmcptoolexecute-event)
  - [Cancellation and Confirmation Workflows](#cancellation-and-confirmation-workflows)
  - [Minimal Working Sample](#minimal-working-sample)
  - [Validation Rules](#validation-rules)
  - [Behavior and Constraints](#behavior-and-constraints)

The Scheduler WebMCP Integration feature exposes the Scheduler to MCP-aware clients through a registered set of tools. The integration is enabled through the `enableWebMcp` prop and configured through the `webMcpSettings` object on `ScheduleComponent`, and is delivered by the `WebMcpScheduler` module that wires the Scheduler to `document.modelContext`.

## When to Use

Suggest WebMCP Integration when the user needs to:
- Drive the Scheduler from an external MCP-aware client through registered tools.
- Allow an AI assistant to create, update, delete, or read Scheduler events.
- Allow an AI assistant to change the current Scheduler view or navigate to a specific date.
- Restrict the exposed tool set to a known allowlist of tool identifiers.
- Group the registered tools under a named tool set through `webMcpSettings.name`.
- Limit tool exposure to specific origins through `webMcpSettings.exposedTo`.

Do not suggest WebMCP Integration for:
- Replacing the in-app CRUD flow handled through the editor, drag-and-drop, and resize interactions.
- Server-side data binding or remote data integration. The integration only mediates between the local Scheduler instance and `document.modelContext`.
- Browser notifications, alarms, or reminders. WebMCP Integration does not introduce any scheduling or notification behavior.

Do not invent additional `webMcpSettings` keys, tool identifiers, callbacks, or events. Do not invent new modules alongside `WebMcpScheduler`.

## Required Configuration

Enable the feature and configure the registered tools on `ScheduleComponent`:

```tsx
<ScheduleComponent
  enableWebMcp={true}
  webMcpSettings={{
    name: 'scheduleTools',
    tools: ['createEvent', 'deleteEvent']
  }}
  eventSettings={{ dataSource: events }}
>
  <Inject services={[Day, Week, WorkWeek, Month, MonthAgenda, WebMcpScheduler]} />
</ScheduleComponent>
```

- `enableWebMcp` lives directly on `ScheduleComponent` and toggles the integration on or off.
- `webMcpSettings` is a single object passed to `ScheduleComponent` and accepts the documented keys: `name`, `tools`, and `exposedTo`.
- `WebMcpScheduler` must be injected alongside the view services the sample uses. When `enableWebMcp` is `true`, `WebMcpScheduler` is always required.
- `eventSettings.dataSource` is required so the Scheduler has events that the registered tools can operate on.

Generation rules:
- Place `enableWebMcp` on `ScheduleComponent` (or on the `Schedule` configuration object), not inside `webMcpSettings`.
- Pass `webMcpSettings` as one object; do not split it into separate props.
- Use only the documented keys (`name`, `tools`, `exposedTo`). Do not invent additional configuration keys.
- Inject `WebMcpScheduler` alongside the view services required by the sample. The supplied sample injects `Day`, `Week`, `WorkWeek`, `Month`, `MonthAgenda`, and `WebMcpScheduler`; inject only the view services the sample actually uses, but always include `WebMcpScheduler` when `enableWebMcp` is `true`.
- Provide an `eventSettings.dataSource` so the registered tools have events to operate on.

## Property Documentation

### `enableWebMcp` (on `ScheduleComponent`)

- **Type:** `boolean`
- **Default:** `false`
- **Description:** When `true`, the Scheduler registers its built-in WebMCP tools on `document.modelContext` so MCP-aware clients can drive the component. When `false`, no WebMCP tools are registered.

### `webMcpSettings` (on `ScheduleComponent`)

- **Type:** `object`
- **Default:** `{}`
- **Description:** A configuration object that controls how WebMCP tools are registered when `enableWebMcp` is `true`. Accepts the following keys:

| Key | Type | Default | Description |
| --- | --- | --- | --- |
| `name` | `string` | `null` | The prefix applied to every registered tool name. The default registration uses the Scheduler element's `id` when this key is `null` or `undefined`; an empty string registers tools without a prefix. Use a unique `name` when multiple Scheduler instances coexist on the same page. |
| `tools` | `string[] \| WebMcpTool[]` | `null` | The list of tools to register. Use an array of tool identifiers from the documented allowlist (`createEvent`, `deleteEvent`, and the read-only `changeView`/`navigateDate` commands) to expose a subset of tools. When omitted, all built-in tools are registered. |
| `exposedTo` | `string[]` | `null` | A list of allowed origins for cross-origin iframe access to the registered tools. The value is forwarded to `modelContext.registerTool` only when it is a non-empty array. |

Do not add additional keys to this object; the Scheduler only supports `name`, `tools`, and `exposedTo` here.

### `beforeWebMcpToolExecute` (on `ScheduleComponent`)

- **Type:** `event`
- **Description:** Fires before a registered WebMCP tool executes. Use the event to inspect the tool name and arguments, cancel the operation, request a confirmation dialog, or provide a custom cancellation response. The event arguments are described under [beforeWebMcpToolExecute Event](#beforewebmcptoolexecute-event).

## WebMcpScheduler Module

`WebMcpScheduler` is the module that integrates the Scheduler with the WebMCP protocol. It is exported from `@syncfusion/ej2-react-schedule` and must be injected when `enableWebMcp` is `true`.

```tsx
import {
  ScheduleComponent,
  Day,
  Week,
  WorkWeek,
  Month,
  MonthAgenda,
  WebMcpScheduler,
  Inject
} from '@syncfusion/ej2-react-schedule';
```

The module is responsible for:
- Wiring the `getWebMcpTools` and `registerWebMcpTools` notifications on the parent Scheduler.
- Looking up the built-in tool list and filtering it by name when `webMcpSettings.tools` is an array of identifiers.
- Registering the resolved tools on `document.modelContext.registerTool` with an `AbortController` signal and an optional `exposedTo` allowlist.
- Resolving the registered tool name (with its prefix stripped) to the matching built-in command handler.
- Triggering `beforeWebMcpToolExecute` before each command runs and honoring its `cancel`, `cancellationResponse`, and `showConfirmationDialog` flags.
- Destroying the abort controller and removing the listeners when the parent Scheduler is destroyed.

The previous module name `WebMcpAdapter` has been replaced by `WebMcpScheduler`. New code must inject `WebMcpScheduler`. Do not import or inject `WebMcpAdapter`.

## Built-in WebMCP Tools

The Scheduler registers the following built-in WebMCP tools when `enableWebMcp` is `true`. The tool names below are the unprefixed identifiers; the registered names on `modelContext` are `${prefix}_${toolName}`.

| Tool | Mutating | Description | Input |
| --- | --- | --- | --- |
| `createEvent` | Yes | Creates a new Scheduler event. | `eventObj` (`Subject`, `StartTime`, `EndTime`, `IsAllDay`, `Location`, `Description`) |
| `updateEvent` | Yes | Updates an existing Scheduler event. The event is located by `Subject` and can be narrowed further by `StartTime` and `EndTime`. | `eventObj` (`Subject`, optional `StartTime`/`EndTime`, then `NewSubject`/`NewStartTime`/`NewEndTime`/`IsAllDay`/`Location`/`Description`) |
| `deleteEvent` | Yes | Deletes an existing Scheduler event located by `Subject` and optionally narrowed by `StartTime`/`EndTime`. | `eventObj` (`Subject`, optional `StartTime`/`EndTime`) |
| `changeView` | No | Changes the current Scheduler view. | `view` (one of `Day`, `Week`, `WorkWeek`, `Month`, `Agenda`, `MonthAgenda`, `TimelineDay`, `TimelineWeek`, `TimelineWorkWeek`, `TimelineMonth`, `Year`, `TimelineYear`) |
| `navigateDate` | No | Navigates the Scheduler to a specific date. | `selectedDate` (ISO 8601 string or `Date`) |

The `createEvent` and `deleteEvent` identifiers are the documented allowlist for the typical WebMCP Integration sample. When `webMcpSettings.tools` is omitted, the integration also exposes the read-only commands `changeView` (changes the current Scheduler view through a `view` argument) and `navigateDate` (navigates the Scheduler to an ISO 8601 date through a `selectedDate` argument) so the AI client can drive the active view and selection. The `updateEvent` mutating command is available when explicitly listed in `webMcpSettings.tools`.

Generation rules:
- Set `webMcpSettings.tools` to an array of tool identifiers limited to the documented allowlist above. Do not invent additional tool identifiers.
- Treat `createEvent`, `updateEvent`, and `deleteEvent` as the mutating tools. The `beforeWebMcpToolExecute` confirmation flow is applied only to mutating commands.

### Tool Response Shape

Every built-in tool returns a response in the WebMCP standard shape:

```json
{
  "content": [
    { "type": "text", "text": "{\"success\": true, \"action\": \"createEvent\", \"data\": { ... }}" }
  ]
}
```

Failures set `isError: true` on the response. The text payload of an error response is a human-readable string. The `action` field in successful payloads reports the resolved command (`createEvent`, `deleteEvent`, or whichever mutating or read-only command was invoked).

## Tool Filtering and Custom Prefixes

When `webMcpSettings.tools` is an array of tool identifiers, only the listed tools are registered. Each registered tool's name is rewritten as `${prefix}_${toolName}` where `prefix` is resolved as follows:

- If `webMcpSettings.name` is a non-null, non-undefined string, that value is used.
- If `webMcpSettings.name` is `null` or `undefined`, the Scheduler element's `id` is used.
- If `webMcpSettings.name` is an empty string, the tools are registered without a prefix (the registered name starts with `_`).

The `exposedTo` array is forwarded to `modelContext.registerTool` only when it is a non-empty array. When `exposedTo` is `null` or an empty array, no `exposedTo` option is forwarded to `modelContext.registerTool`.

```tsx
<ScheduleComponent
  enableWebMcp={true}
  webMcpSettings={{
    name: 'scheduleTools',
    tools: ['createEvent', 'deleteEvent'],
    exposedTo: ['https://assistant.example.com']
  }}
>
  {/* ... */}
</ScheduleComponent>
```

Multi-instance guidance:
- Set `webMcpSettings.name` to a unique string for each Scheduler instance to avoid tool-name collisions on `document.modelContext`.
- Keep the `exposedTo` allowlist as narrow as possible. Restrict it to the origins that need to call the registered tools.

## Automatic Tool Registration

When `enableWebMcp` is `true`, the Scheduler automatically notifies the `registerWebMcpTools` notification during initialization, using the values from `webMcpSettings`. The resolved `prefix`, `tools`, and `exposedTo` values are forwarded to `WebMcpScheduler` so the tools are registered on `document.modelContext` without any additional imperative calls.

The automatic registration fires once during Scheduler initialization. Updating `webMcpSettings` after the Scheduler has rendered does not re-trigger registration in the current implementation. If you need to re-register tools at runtime, prefer toggling the `enableWebMcp` prop through a key-based remount of `ScheduleComponent` or destroy and recreate the Scheduler.

## Notification-Based Registration

`WebMcpScheduler` listens for two Scheduler notifications: `getWebMcpTools` (tool discovery) and `registerWebMcpTools` (registration). The automatic registration from `webMcpSettings` is the supported path; the underlying notifications exist so host applications can drive the same registration flow from a custom integration. The two notification names are:

- `getWebMcpTools` - returns the available tools, optionally filtered by `toolNames`.
- `registerWebMcpTools` - registers the resolved tools on `document.modelContext` using the supplied `prefix`, `tools`, and `exposedTo`.

Do not call the underlying `registerWebMcpTools` notification directly from application code. Use `webMcpSettings` to configure automatic registration.

## beforeWebMcpToolExecute Event

`beforeWebMcpToolExecute` fires on `ScheduleComponent` before every WebMCP tool execution. Use it to inspect, modify, or cancel a tool call. The event arguments expose the following fields:

| Field | Type | Description |
| --- | --- | --- |
| `toolName` | `string` | The fully prefixed tool name that the AI client invoked (for example, `scheduleTools_createEvent`). |
| `toolArgs` | `object` | The arguments the AI client passed to the tool. The shape matches the tool's `inputSchema`. |
| `cancel` | `boolean` | Set to `true` to prevent the tool from executing. |
| `cancellationResponse` | `string` | Optional custom message returned to the AI when the execution is cancelled. When omitted, a default cancellation message is returned. |
| `showConfirmationDialog` | `boolean` | Set to `true` to request a confirmation dialog before a mutating tool executes. The dialog is shown only for the mutating commands `createEvent` and `deleteEvent` (and for `updateEvent` when it is exposed through `webMcpSettings.tools`). Read-only commands (`changeView`, `navigateDate`) skip the dialog. |

```tsx
const onBeforeWebMcpToolExecute = (args: any): void => {
  // Cancel the tool when needed
  if (args.toolArgs && (args.toolArgs as any).eventObj && (args.toolArgs as any).eventObj.Subject === 'Block list') {
    args.cancel = true;
    args.cancellationResponse = 'This subject is on the block list.';
    return;
  }
};

<ScheduleComponent
  enableWebMcp={true}
  webMcpSettings={{ name: 'scheduleTools', tools: ['createEvent'] }}
  beforeWebMcpToolExecute={onBeforeWebMcpToolExecute}
>
  {/* ... */}
</ScheduleComponent>
```

Place `beforeWebMcpToolExecute` directly on `ScheduleComponent` (or on the `Schedule` configuration object) when tool argument customization is required. Do not nest the callback inside `webMcpSettings` and do not invent additional WebMCP callbacks or events.

## Cancellation and Confirmation Workflows

The Scheduler combines two opt-in controls on `beforeWebMcpToolExecute`:

- **Cancellation:** set `args.cancel = true` to stop a tool from running. The response includes `cancelled: true` and a `message` taken from `args.cancellationResponse` when provided, or a default message otherwise. The default message is `[USER_CANCELLED] Tool "${toolName}" was cancelled. This is final. Do NOT retry.`
- **Confirmation dialog:** set `args.showConfirmationDialog = true` to pop a modal confirmation dialog before a mutating tool (`createEvent`, `deleteEvent`, and `updateEvent` when it is exposed through `webMcpSettings.tools`) executes. The dialog message is generated from the command and the supplied event data (for example, `Create event "Standup" from Apr 8, 2026, 9:00 AM to Apr 8, 2026, 9:30 AM?`). The tool only runs when the user confirms.

Read-only commands (`changeView`, `navigateDate`) skip the confirmation dialog even when `showConfirmationDialog` is set.

The confirmation dialog is rendered through a Syncfusion `Dialog` from `@syncfusion/ej2-popups`. The host page must load the `Dialog` stylesheet and runtime to display the confirmation UI.

## Minimal Working Sample

```tsx
import * as React from 'react';
import {
  ScheduleComponent,
  ViewsDirective,
  ViewDirective,
  Day,
  Week,
  WorkWeek,
  Month,
  MonthAgenda,
  WebMcpScheduler,
  Inject
} from '@syncfusion/ej2-react-schedule';

const events = [
  {
    Id: 1,
    Subject: 'Project Review',
    StartTime: new Date(2026, 2, 25, 9, 0),
    EndTime: new Date(2026, 2, 25, 10, 30),
    Location: 'Conference Room A'
  },
  {
    Id: 2,
    Subject: 'Customer Meeting',
    StartTime: new Date(2026, 2, 26, 14, 0),
    EndTime: new Date(2026, 2, 26, 15, 30)
  }
];

const onBeforeWebMcpToolExecute = (args: any): void => {
  // Optional: cancel tools that do not pass our checks.
  // The supplied sample leaves cancellation and confirmation dialogs off.
};

const App = () => (
  <ScheduleComponent
    height="650px"
    selectedDate={new Date(2026, 2, 25)}
    eventSettings={{ dataSource: events }}
    enableWebMcp={true}
    webMcpSettings={{
      name: 'scheduleTools',
      tools: ['createEvent', 'deleteEvent']
    }}
    beforeWebMcpToolExecute={onBeforeWebMcpToolExecute}
  >
    <ViewsDirective>
      <ViewDirective option="Day" />
      <ViewDirective option="Week" />
      <ViewDirective option="WorkWeek" />
      <ViewDirective option="Month" />
      <ViewDirective option="MonthAgenda" />
    </ViewsDirective>
    <Inject services={[Day, Week, WorkWeek, Month, MonthAgenda, WebMcpScheduler]} />
  </ScheduleComponent>
);

export default App;
```

## Validation Rules

Before presenting a WebMCP Integration sample, verify that:
- `enableWebMcp` is passed as a prop on `ScheduleComponent` (or on the `Schedule` configuration object), not nested inside `webMcpSettings`.
- `webMcpSettings` is passed as a single object on `ScheduleComponent` (or on the `Schedule` configuration object).
- The only keys used inside `webMcpSettings` are `name`, `tools`, and `exposedTo`. Do not introduce additional configuration keys.
- `webMcpSettings.name` is a string when set, `webMcpSettings.tools` is an array of the documented tool identifiers (`createEvent`, `deleteEvent`, and the read-only `changeView`/`navigateDate` commands when explicitly listed), and `webMcpSettings.exposedTo` is an array of origin strings.
- `WebMcpScheduler` is injected alongside the view services the sample uses. When `enableWebMcp` is `true`, `WebMcpScheduler` must be present in the `Inject services` list.
- `beforeWebMcpToolExecute` is passed directly on `ScheduleComponent` (or on the `Schedule` configuration object) when tool argument customization is required. It is not nested inside `webMcpSettings`.
- The selected Scheduler view is declared with a `ViewDirective` and its required service is included in `Inject`.
- `eventSettings.dataSource` is provided so the Scheduler has events the registered tools can operate on.
- The host page loads the Syncfusion `Dialog` stylesheet and runtime when `args.showConfirmationDialog = true` is used.
- `document.modelContext` is expected to be supplied by the host page or the MCP-aware client. The Scheduler does not create `modelContext` itself; if it is missing, the tools are not registered and the integration is a no-op.

## Behavior and Constraints

- The `WebMcpScheduler` module name is `webMcpScheduler`. Earlier code referenced `webMcpAdapter` and `WebMcpAdapter`; both names have been removed from the documented API and must not be reintroduced.
- Tool registration is mediated through `document.modelContext.registerTool`. The integration does not work in hosts that do not expose `modelContext`. When `modelContext` is missing, the registration step is a no-op and no error is thrown.
- The tool name on `modelContext` is always `${prefix}_${toolName}`. The prefix is `webMcpSettings.name` when provided, otherwise the Scheduler element's `id`, otherwise an empty string. Update the AI client's tool allowlist to match the prefixed names.
- The `exposedTo` allowlist is forwarded to `modelContext.registerTool` only when it is a non-empty array. When omitted, no `exposedTo` option is forwarded.
- The `beforeWebMcpToolExecute` confirmation dialog applies only to mutating commands (`createEvent`, `updateEvent`, `deleteEvent`). The dialog is skipped for read-only commands even when `showConfirmationDialog` is set.
- `updateEvent` and `deleteEvent` locate the target event by `Subject`. When multiple events share the same `Subject`, narrow the match with `StartTime` and `EndTime`. The Scheduler reports a `No matching event found` error when no event matches the supplied identification fields.
- Create, update, and delete operations honor the Scheduler's permission settings. `eventSettings.allowAdding`, `eventSettings.allowEditing`, `eventSettings.allowDeleting`, and the active view's `readonly` flag gate the built-in tools. The Scheduler reports an error such as `Adding events is disabled` when a permission check blocks a tool call.
- The `WebMcpScheduler` module removes its listeners and aborts the registration's `AbortController` when the parent Scheduler is destroyed. The integration does not survive a Scheduler destroy/recreate cycle; the recreated Scheduler registers its tools again through the same automatic flow.
- The integration is purely additive. It does not change the in-app CRUD flow, drag-and-drop, resize, or any other Scheduler behavior. The standard editor and inline interactions continue to operate independently from the registered tools.
- The host page is responsible for security. Restrict `webMcpSettings.exposedTo` to the origins that legitimately need to invoke the registered tools and validate every argument that comes from an external MCP client.
