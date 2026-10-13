# EventBuffer

## Table of Contents

- [EventBuffer](#eventbuffer)
  - [Table of Contents](#table-of-contents)
  - [When to Use](#when-to-use)
  - [Required Configuration](#required-configuration)
  - [Buffer Template](#buffer-template)
  - [Views and Dependencies](#views-and-dependencies)
  - [Resource and Interaction Pattern](#resource-and-interaction-pattern)
  - [Minimal Working Sample](#minimal-working-sample)
  - [Validation Rules](#validation-rules)
  - [Behavior and Constraints](#behavior-and-constraints)

The Scheduler EventBuffer feature reserves configurable time before and after an event. Use it when an appointment needs preparation, setup, cleanup, recovery, or another operational interval that must participate in scheduling and conflict detection.

## When to Use

Suggest EventBuffer when the user needs to:
- Reserve preparation time before an appointment.
- Reserve cleanup, recovery, or follow-up time after an appointment.
- Display buffer intervals around events in vertical time-grid or timeline views.
- Include the reserved buffer range in overlap validation.
- Render custom buffer content such as an icon and duration.

Do not suggest EventBuffer for unrelated visual spacing, arbitrary padding around events, or a separate task that should be represented as its own appointment. Do not invent additional buffer properties or APIs.

## Required Configuration

Enable the feature through `eventSettings`:

```tsx
const eventSettings = {
  dataSource: events,
  enableBuffer: true,
  fields: {
    bufferBefore: { name: 'bufferBefore' },
    bufferAfter: { name: 'bufferAfter' }
  }
};
```

Each event can then provide buffer durations in minutes:

```tsx
const events = [
  {
    Id: 1,
    Subject: 'Procedure',
    StartTime: new Date(2026, 2, 25, 9, 0),
    EndTime: new Date(2026, 2, 25, 10, 30),
    bufferBefore: 30,
    bufferAfter: 45
  }
];
```

Generation rules:
- Keep `enableBuffer: true` under `eventSettings`.
- Map both `bufferBefore` and `bufferAfter` through `eventSettings.fields` when both sides are used.
- Store the mapped event values in minutes, as shown in the sample.
- Preserve the event's normal `StartTime` and `EndTime`; the buffer values describe reserved ranges around those event edges.
- Use `allowOverlap={false}` when buffered ranges must participate in conflict prevention, as demonstrated by the sample.

## Buffer Template

A custom template is assigned through `eventSettings.bufferTemplate`:

```tsx
const bufferTemplate = (args: any): JSX.Element => {
  const isBefore = args.bufferType === 'before';
  const duration = isBefore
    ? args.data.bufferBefore
    : args.data.bufferAfter;

  return (
    <div className="buffer-minimal">
      <span className="buffer-duration">{duration} min</span>
    </div>
  );
};

const eventSettings = {
  dataSource: events,
  enableBuffer: true,
  bufferTemplate,
  fields: {
    bufferBefore: { name: 'bufferBefore' },
    bufferAfter: { name: 'bufferAfter' }
  }
};
```

The supplied sample reads `args.bufferType` to distinguish `before` from `after`, then reads the corresponding value from `args.data.bufferBefore` or `args.data.bufferAfter`. A template may return different markup for each side, hide short durations, or render an icon and duration. The sample uses `scheduleObj.current?.currentView` to vary rendering between `Day` and timeline views.

When reproducing the sample's visual behavior, note these exact rules:
- Durations below 10 are returned as an empty fragment.
- In `Day`, the sample renders its image and duration only when the duration is greater than 15.
- In timeline views, the sample always renders its image for visible buffers and adds duration text only when the duration is greater than 20.
- The sample uses different image URLs for before and after buffers and CSS classes from `event-buffer.css`; these are presentation choices, not additional Scheduler APIs.

## Views and Dependencies

Buffers are described in the sample as supported in vertical time-grid and timeline views: `Day`, `Week`, `WorkWeek`, `TimelineDay`, `TimelineWeek`, and `TimelineWorkWeek`. Inject only the view services required by the generated sample. The supplied resource-grouped sample injects `Day`, `TimelineViews`, `DragAndDrop`, and `Resize` and declares `Day`, `TimelineDay`, and `TimelineWeek` with `ViewsDirective` and `ViewDirective`.

EventBuffer itself does not require resource grouping. Add `ResourcesDirective`, `ResourceDirective`, and `group` only when events must be scheduled against resources. In the supplied sample, each event uses `RoomId`, and the resource configuration maps `RoomId` to the `Rooms` resource collection.

## Resource and Interaction Pattern

For a resource-based workflow, the sample combines EventBuffer with:
- `group={{ resources: ['Rooms'] }}`.
- A `ResourceDirective` with `field='RoomId'`, `name='Rooms'`, `textField='RoomText'`, `idField='RoomId'`, and `colorField='RoomColor'`.
- `allowOverlap={false}` so buffered reserved ranges are considered during conflict detection.
- `dragStop` validation that cancels a drag when the target resource's specialty does not match the event's `RequiredSkill`.
- `actionBegin` handling for `eventCreate` to set `RequiredSkill` from the selected resource.

Do not add the specialty validation, dialog, or resource fields to a minimal EventBuffer sample unless the user requests resource-specific validation.

## Minimal Working Sample

```tsx
import * as React from 'react';
import {
  ScheduleComponent,
  ViewsDirective,
  ViewDirective,
  Day,
  TimelineViews,
  Inject
} from '@syncfusion/ej2-react-schedule';

const events = [
  {
    Id: 1,
    Subject: 'Procedure',
    StartTime: new Date(2026, 2, 25, 9, 0),
    EndTime: new Date(2026, 2, 25, 10, 30),
    bufferBefore: 30,
    bufferAfter: 45
  }
];

const bufferTemplate = (args: any): JSX.Element => {
  const duration = args.bufferType === 'before'
    ? args.data.bufferBefore
    : args.data.bufferAfter;

  return <span>{duration} min</span>;
};

const App = () => (
  <ScheduleComponent
    height="650px"
    selectedDate={new Date(2026, 2, 25)}
    currentView="TimelineDay"
    allowOverlap={false}
    eventSettings={{
      dataSource: events,
      enableBuffer: true,
      bufferTemplate,
      fields: {
        bufferBefore: { name: 'bufferBefore' },
        bufferAfter: { name: 'bufferAfter' }
      }
    }}
  >
    <ViewsDirective>
      <ViewDirective option="Day" />
      <ViewDirective option="TimelineDay" />
    </ViewsDirective>
    <Inject services={[Day, TimelineViews]} />
  </ScheduleComponent>
);

export default App;
```

## Validation Rules

Before presenting an EventBuffer sample, verify that:
- `enableBuffer` is inside `eventSettings`.
- The event data contains the mapped buffer fields with minute values.
- `eventSettings.fields` maps the exact data field names used by the events.
- `bufferTemplate` is assigned inside `eventSettings` when custom rendering is requested.
- The template distinguishes `args.bufferType === 'before'` from the after case and reads the matching event value.
- Any referenced view is declared and its required service is included in `Inject`.
- `allowOverlap={false}` is included when the requirement is to prevent conflicts with buffered ranges.
- Resource directives and drag/drop validation are added only for resource scheduling requirements.
- CSS files, image URLs, dialogs, and custom fields are treated as sample-specific additions rather than EventBuffer prerequisites.

## Behavior and Constraints

The sample states that buffers re-anchor to the event edges during drag and resize previews. Buffer rendering is therefore tied to the event rather than being an independent appointment. Keep buffer values separate from `StartTime` and `EndTime`, and do not convert the buffer into a second event unless the user explicitly requests that model.
