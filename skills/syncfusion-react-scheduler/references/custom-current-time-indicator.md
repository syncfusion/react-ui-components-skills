# CustomCurrentTimeIndicator

## Table of Contents

- [CustomCurrentTimeIndicator](#customcurrenttimeindicator)
  - [Table of Contents](#table-of-contents)
  - [When to Use](#when-to-use)
  - [Required Configuration](#required-configuration)
  - [Property Documentation](#property-documentation)
  - [Supported Views](#supported-views)
  - [Runtime Updates](#runtime-updates)
  - [Minimal Working Sample](#minimal-working-sample)
  - [Validation Rules](#validation-rules)
  - [Behavior and Constraints](#behavior-and-constraints)

CustomCurrentTimeIndicator highlights the current moment in the Scheduler and lets you control how the indicator line is drawn, where it is placed, and whether it extends to previously visible dates. The Scheduler exposes the indicator through the `showTimeIndicator` prop on `ScheduleComponent` and the `currentTimeIndicatorSettings` configuration object.

## When to Use

Suggest CustomCurrentTimeIndicator when the user needs to:
- Highlight the present time on a Day, Week, Work Week, or Timeline view.
- Toggle the indicator on or off at runtime.
- Show or hide the time label that accompanies the indicator line.
- Show the indicator on previous dates that are still within the current visible range.
- Place the indicator above (on top of) the appointment content rather than beneath it.

Do not suggest CustomCurrentTimeIndicator for:
- Reminders, alarms, notifications, or any behavior that depends on a specific event's time.
- Recurring scheduling logic, working-hours computation, or resource availability checks.
- Replacing the time-axis (`timeScale`) configuration. The indicator is rendered against the existing time axis and `timeScale` is a separate concern.

Do not invent additional keys for `currentTimeIndicatorSettings` beyond `showTime`, `showPreviousDates`, and `onTop`, and do not introduce alternate APIs to control the indicator.

## Required Configuration

Enable and customize the indicator through two properties on `ScheduleComponent`:

```tsx
<ScheduleComponent
  showTimeIndicator={true}
  currentTimeIndicatorSettings={{
    showTime: true,
    showPreviousDates: true,
    onTop: true
  }}
>
  {/* views and inject */}
</ScheduleComponent>
```

- `showTimeIndicator` lives directly on `ScheduleComponent` and toggles the indicator line on or off.
- `currentTimeIndicatorSettings` is a single object passed to `ScheduleComponent` and accepts the three documented keys: `showTime`, `showPreviousDates`, and `onTop`.
- All three `currentTimeIndicatorSettings` keys are booleans.

Generation rules:
- Place `showTimeIndicator` on `ScheduleComponent`, not inside `currentTimeIndicatorSettings`.
- Pass `currentTimeIndicatorSettings` as one object; do not split it into separate props.
- Use only the documented keys (`showTime`, `showPreviousDates`, `onTop`). Do not invent additional configuration keys.
- Provide a `currentView` that the indicator can be drawn against, and inject the matching view service.

## Property Documentation

### `showTimeIndicator` (on `ScheduleComponent`)

- **Type:** `boolean`
- **Default:** `true`
- **Description:** When `true`, the Scheduler renders the current time indicator line in supported views. When `false`, the indicator is hidden.

### `currentTimeIndicatorSettings` (on `ScheduleComponent`)

- **Type:** `object`
- **Description:** A configuration object for the current time indicator. Accepts the following boolean keys:

| Key | Type | Default | Description |
| --- | --- | --- | --- |
| `showTime` | `boolean` | `true` | When `true`, displays the time label that accompanies the indicator line. When `false`, only the line is drawn. |
| `showPreviousDates` | `boolean` | `false` | When `true`, the indicator is drawn on the prior dates that are still within the visible range, in addition to the current date. When `false`, the indicator only appears on the current date. |
| `onTop` | `boolean` | `false` | When `true`, the indicator line is rendered above the appointment content so it remains visible. When `false`, the indicator is drawn beneath appointment content. |

Do not add additional keys to this object; the Scheduler only supports `showTime`, `showPreviousDates`, and `onTop` here.

## Supported Views

The indicator is meaningful in views that display a time axis:

- Day
- Week
- Work Week
- Timeline Day
- Timeline Week
- Timeline Work Week

When generating a sample:
- Declare the selected view with `currentView` and add the matching `ViewDirective`.
- Inject the corresponding view service(s). The supplied sample injects `Day`, `Week`, `WorkWeek`, and `TimelineViews` and declares `Day`, `Week`, `WorkWeek`, `TimelineDay`, `TimelineWeek`, and `TimelineWorkWeek`.
- Provide `timeScale={{ enable: true }}` so the indicator can be drawn against the visible time axis.

## Runtime Updates

When the user asks for an interactive example, drive the indicator's configuration from React state and pass the values to the Scheduler:

```tsx
const [showTimeIndicator, setShowTimeIndicator] = React.useState(true);
const [showTime, setShowTime] = React.useState(true);
const [showPreviousDates, setShowPreviousDates] = React.useState(true);
const [onTop, setOnTop] = React.useState(true);

<ScheduleComponent
  showTimeIndicator={showTimeIndicator}
  currentTimeIndicatorSettings={{ showTime, showPreviousDates, onTop }}
  timeScale={{ enable: true }}
  // ...
/>
```

Each checkbox in the supplied sample is bound to its own `useState` value, and toggling a checkbox immediately updates the Scheduler because React re-renders with the new prop value. Do not call imperative Scheduler methods to flip these toggles.

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
  TimelineViews,
  Inject
} from '@syncfusion/ej2-react-schedule';

const currentDate = new Date();

const indicatorData = [
  {
    Id: 1,
    Subject: 'Travel Preparation',
    StartTime: new Date(currentDate.getFullYear(), currentDate.getMonth(), currentDate.getDate(), 8, 30),
    EndTime: new Date(currentDate.getFullYear(), currentDate.getMonth(), currentDate.getDate(), 9, 30)
  },
  {
    Id: 2,
    Subject: 'Customer Meeting',
    StartTime: new Date(currentDate.getFullYear(), currentDate.getMonth(), currentDate.getDate(), 10, 0),
    EndTime: new Date(currentDate.getFullYear(), currentDate.getMonth(), currentDate.getDate(), 12, 30)
  },
  {
    Id: 3,
    Subject: 'Lab Review',
    StartTime: new Date(currentDate.getFullYear(), currentDate.getMonth(), currentDate.getDate(), 14, 0),
    EndTime: new Date(currentDate.getFullYear(), currentDate.getMonth(), currentDate.getDate(), 15, 0)
  }
];

export default function App() {
  const [showTimeIndicator, setShowTimeIndicator] = React.useState(true);
  const [showTime, setShowTime] = React.useState(true);
  const [showPreviousDates, setShowPreviousDates] = React.useState(true);
  const [onTop, setOnTop] = React.useState(true);

  return (
    <div>
      <div
        style={{
          display: 'flex',
          flexWrap: 'wrap',
          gap: '12px',
          marginBottom: '16px',
          alignItems: 'center'
        }}
      >
        <label>
          <input
            type="checkbox"
            checked={showTimeIndicator}
            onChange={(e) => setShowTimeIndicator(e.target.checked)}
          />{' '}
          showTimeIndicator
        </label>

        <label>
          <input
            type="checkbox"
            checked={showTime}
            onChange={(e) => setShowTime(e.target.checked)}
          />{' '}
          showTime
        </label>

        <label>
          <input
            type="checkbox"
            checked={showPreviousDates}
            onChange={(e) => setShowPreviousDates(e.target.checked)}
          />{' '}
          showPreviousDates
        </label>

        <label>
          <input
            type="checkbox"
            checked={onTop}
            onChange={(e) => setOnTop(e.target.checked)}
          />{' '}
          onTop
        </label>
      </div>

      <ScheduleComponent
        width="100%"
        height="550px"
        selectedDate={currentDate}
        currentView="TimelineWeek"
        showTimeIndicator={showTimeIndicator}
        currentTimeIndicatorSettings={{
          showTime,
          showPreviousDates,
          onTop
        }}
        timeScale={{
          enable: true
        }}
        eventSettings={{
          dataSource: indicatorData
        }}
      >
        <ViewsDirective>
          <ViewDirective option="Day" />
          <ViewDirective option="Week" />
          <ViewDirective option="WorkWeek" />
          <ViewDirective option="TimelineDay" />
          <ViewDirective option="TimelineWeek" />
          <ViewDirective option="TimelineWorkWeek" />
        </ViewsDirective>

        <Inject services={[Day, Week, WorkWeek, TimelineViews]} />
      </ScheduleComponent>
    </div>
  );
}
```

## Validation Rules

Before presenting a CustomCurrentTimeIndicator sample, verify that:
- `showTimeIndicator` is passed as a prop on `ScheduleComponent`, not nested inside `currentTimeIndicatorSettings`.
- `currentTimeIndicatorSettings` is passed as a single object on `ScheduleComponent`.
- The only keys used inside `currentTimeIndicatorSettings` are `showTime`, `showPreviousDates`, and `onTop`.
- All values inside `currentTimeIndicatorSettings` are booleans.
- A `currentView` is set on `ScheduleComponent` and a matching `ViewDirective` plus the corresponding service are included in `Inject`.
- `timeScale.enable` is `true` so the indicator is rendered against the visible time axis.
- Runtime updates are driven by React state (`useState`) and passed to the Scheduler via props, not via imperative methods.
- Event data is provided through `eventSettings.dataSource` and uses valid `StartTime` and `EndTime` `Date` values.

## Behavior and Constraints

- The indicator is rendered against the time axis, so the relevant `timeScale` configuration must be enabled.
- `showTime` controls only the time label; the indicator line itself is controlled by `showTimeIndicator`.
- `showPreviousDates` extends the indicator to prior dates inside the visible range; it does not change the current-date indicator.
- `onTop` controls stacking order versus appointment content; it does not affect where the indicator is drawn within the time axis.
- The feature is purely presentational. It does not change event data, scheduling, or any CRUD behavior.
- The supplied sample uses checkbox controls bound to `useState`. Treat the checkbox labels (`showTimeIndicator`, `showTime`, `showPreviousDates`, `onTop`) as presentation choices, not as APIs.
