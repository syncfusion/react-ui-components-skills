# Telemetry

## Table of Contents
- [Telemetry Overview](#telemetry-overview)
- [Getting Started with Telemetry](#getting-started-with-telemetry)
- [Configuring Metrics](#configuring-metrics)
- [Customizing the Report](#customizing-the-report)
- [Displaying AI Data](#displaying-ai-data)
- [Custom Attributes](#custom-attributes)
- [Telemetry Types Reference](#telemetry-types-reference)
- [Key Characteristics](#key-characteristics)

## Telemetry Overview

The **Telemetry** feature in AI AssistView tracks AI interaction metrics such as response duration, tool calls, and model information. When enabled, it renders a built-in telemetry button (displaying the response duration) in the toolbar of every response. Hovering over the button shows a tooltip with the full telemetry report.

Telemetry is fully controlled through the [`telemetrySettings`](../api/ai-assistview/telemetrySettings) option, which exposes two configuration points:

- **`metrics`** — Restrict the captured report to specific `TelemetryMetric` values and define the display order of rows in the tooltip.
- **`beforeReport`** — Event callback that intercepts the generated report before it is rendered, allowing you to modify it, add custom attributes, or return `null` to suppress the report entirely.

Automatically measured metrics:

- `duration` — Total time taken to generate the response
- `toolCalls` — Number of tool invocations performed while generating the response
- `streamingChunks` — Number of streamed content chunks received for the response

Metrics provided through the `TelemetryData` argument of [`addPromptResponse`](../api/ai-assistview#addpromptresponse):

- `model` — Name of the AI model used
- `inputTokens` — Number of tokens in the input prompt
- `outputTokens` — Number of tokens generated in the response
- `reasoningTokens` — Number of tokens consumed for reasoning, when available
- `cachedInputTokens` — Number of input tokens served from the model cache, when available

## Getting Started with Telemetry

Enable telemetry by setting `enable` to `true` in the `telemetrySettings` property. A telemetry button (showing the response duration) is rendered in every response toolbar, and hovering over it shows the telemetry report in a tooltip.

### Basic Telemetry Example

```tsx
import { AIAssistViewComponent, PromptRequestEventArgs, TelemetrySettingsModel } from '@syncfusion/ej2-react-interactive-chat';
import * as React from 'react';
import * as ReactDOM from "react-dom";

function App() {
    const assistInstance = React.useRef<AIAssistViewComponent>(null);
    const suggestions: string[] = [
        "How do I set achievable goals at work?",
        "Why do people fly in their dreams?"
    ];

    const telemetrySettings: TelemetrySettingsModel = {
        enable: true
    };

    const onPromptRequest = (args: PromptRequestEventArgs) => {
        // Your required action here
        setTimeout(() => {
            let defaultResponse = 'For real-time prompt processing, connect the AIAssistView component to your preferred AI service, such as OpenAI or Azure Cognitive Services. Ensure you obtain the necessary API credentials to authenticate and enable seamless integration.';
            assistInstance.current.addPromptResponse(defaultResponse);
        }, 1000);
    };

    return (
        // specifies the tag for render the AI AssistView component
        <AIAssistViewComponent
            id="gettingstarted"
            ref={assistInstance}
            promptRequest={onPromptRequest}
            promptSuggestions={suggestions}
            telemetrySettings={telemetrySettings}
        ></AIAssistViewComponent>
    );
}

ReactDOM.render(<App />, document.getElementById('container'));
```

## Configuring Metrics

Control which telemetry metrics are captured in the report using the `metrics` property of `telemetrySettings`. It accepts one or more values from the `TelemetryMetric` type. The order of values in the `metrics` array also determines the display order of rows in the tooltip.

### Available Metrics

| Metric | Description |
|---------|-------------|
| `status` | Indicates whether the response was completed or cancelled. |
| `duration` | Total time taken to generate the response. |
| `toolCalls` | Number of tool invocations performed while generating the response. |
| `streamingChunks` | Number of streamed content chunks received for the response. |
| `model` | Name of the AI model used to generate the response. |
| `inputTokens` | Number of tokens included in the input prompt. |
| `outputTokens` | Number of tokens generated in the response. |
| `totalTokens` | Total token count calculated as the sum of `inputTokens` and `outputTokens`. |
| `reasoningTokens` | Number of tokens consumed for reasoning by the model, when available. |
| `cachedInputTokens` | Number of input tokens served from the model cache, when available. |

### Metrics Filter Example

```tsx
import { AIAssistViewComponent, PromptRequestEventArgs, TelemetryData, TelemetryMetric, TelemetrySettingsModel } from '@syncfusion/ej2-react-interactive-chat';
import * as React from 'react';
import * as ReactDOM from "react-dom";

function App() {
    const assistInstance = React.useRef<AIAssistViewComponent>(null);
    const suggestions: string[] = [
        "How do I set achievable goals at work?",
        "Why do people fly in their dreams?"
    ];
    // Limits the report to the specified metrics and fixes their display order in the tooltip.
    const telemetrySettings: TelemetrySettingsModel = {
        enable: true,
        metrics: ['status', 'duration', 'model', 'inputTokens', 'outputTokens', 'totalTokens'] as TelemetryMetric[]
    };

    const onPromptRequest = (args: PromptRequestEventArgs) => {
        // Your required action here
        setTimeout(() => {
            let defaultResponse = 'For real-time prompt processing, connect the AIAssistView component to your preferred AI service, such as OpenAI or Azure Cognitive Services. Ensure you obtain the necessary API credentials to authenticate and enable seamless integration.';
            // Mock telemetry data with hard-coded usage values.
            let telemetryData: TelemetryData = {
                model: 'gpt-4o',
                inputTokens: 128,
                outputTokens: 512
            };
            assistInstance.current.addPromptResponse(defaultResponse, true, telemetryData);
        }, 1000);
    };

    return (
        // specifies the tag for render the AI AssistView component
        <AIAssistViewComponent
            id="metrics"
            ref={assistInstance}
            promptRequest={onPromptRequest}
            promptSuggestions={suggestions}
            telemetrySettings={telemetrySettings}
        ></AIAssistViewComponent>
    );
}

ReactDOM.render(<App />, document.getElementById('container'));
```

## Customizing the Report

Use the `beforeReport` event in `telemetrySettings` to intercept the generated report before it is rendered. It accepts the `TelemetryReport` object as an argument and allows you to:

- Modify the report (e.g., add `customAttributes`, transform values)
- Return `null` to suppress the report delivery entirely

Custom attributes added in this callback are displayed in the telemetry tooltip alongside the standard metrics.

### beforeReport Event Example

```tsx
import { AIAssistViewComponent, PromptRequestEventArgs, TelemetryReport, TelemetrySettingsModel } from '@syncfusion/ej2-react-interactive-chat';
import * as React from 'react';
import * as ReactDOM from "react-dom";

function App() {
    const assistInstance = React.useRef<AIAssistViewComponent>(null);
    const suggestions: string[] = [
        "How do I set achievable goals at work?",
        "Why do people fly in their dreams?"
    ];
    // Invoked before every report is delivered. Return the report (modified or not)
    // to deliver it, or null to suppress the report entirely.
    const beforeReport = (report: TelemetryReport): TelemetryReport | null => {
        report.customAttributes = { region: 'us-east-1', tenant: 'acme', beta: true };
        return report;
    };

    const telemetrySettings: TelemetrySettingsModel = {
        enable: true,
        beforeReport: beforeReport
    };

    const onPromptRequest = (args: PromptRequestEventArgs) => {
        // Your required action here
        setTimeout(() => {
            let defaultResponse = 'For real-time prompt processing, connect the AIAssistView component to your preferred AI service, such as OpenAI or Azure Cognitive Services. Ensure you obtain the necessary API credentials to authenticate and enable seamless integration.';
            assistInstance.current.addPromptResponse(defaultResponse);
        }, 1000);
    };

    return (
        // specifies the tag for render the AI AssistView component
        <AIAssistViewComponent
            id="beforereport"
            ref={assistInstance}
            promptRequest={onPromptRequest}
            promptSuggestions={suggestions}
            telemetrySettings={telemetrySettings}
        ></AIAssistViewComponent>
    );
}

ReactDOM.render(<App />, document.getElementById('container'));
```

## Displaying AI Data

The AI AssistView automatically measures `duration`, `toolCalls`, and `streamingChunks`. To include **model and token usage details**, pass a `TelemetryData` argument to the [`addPromptResponse`](../api/ai-assistview#addpromptresponse) method. The `model`, `inputTokens`, `outputTokens`, `reasoningTokens`, and `cachedInputTokens` values are merged into the final telemetry report.

### Pass TelemetryData to addPromptResponse

```tsx
import { AIAssistViewComponent, PromptRequestEventArgs, TelemetryData, TelemetrySettingsModel } from '@syncfusion/ej2-react-interactive-chat';
import * as React from 'react';
import * as ReactDOM from "react-dom";

function App() {
    const assistInstance = React.useRef<AIAssistViewComponent>(null);
    const suggestions: string[] = [
        "How do I set achievable goals at work?",
        "Why do people fly in their dreams?"
    ];

    const telemetrySettings: TelemetrySettingsModel = {
        enable: true
    };

    const onPromptRequest = (args: PromptRequestEventArgs) => {
        // Your required action here
        setTimeout(() => {
            let defaultResponse = 'For real-time prompt processing, connect the AIAssistView component to your preferred AI service, such as OpenAI or Azure Cognitive Services. Ensure you obtain the necessary API credentials to authenticate and enable seamless integration.';
            // Mock telemetry data with hard-coded usage values (model and token details).
            // In a real scenario, these values are obtained from the AI service response.
            let telemetryData: TelemetryData = {
                model: 'gpt-4o',
                inputTokens: 128,
                outputTokens: 512,
                reasoningTokens: 64,
                cachedInputTokens: 32
            };
            assistInstance.current.addPromptResponse(defaultResponse, true, telemetryData);
        }, 1000);
    };

    return (
        // specifies the tag for render the AI AssistView component
        <AIAssistViewComponent
            id="usagedata"
            ref={assistInstance}
            promptRequest={onPromptRequest}
            promptSuggestions={suggestions}
            telemetrySettings={telemetrySettings}
        ></AIAssistViewComponent>
    );
}

ReactDOM.render(<App />, document.getElementById('container'));
```

## Custom Attributes

Pass custom key/value pairs through the `customAttributes` field of the `TelemetryData` argument to surface **domain-specific metrics** (for example, `region`, `tenant`, `sessionId`, or `feature`) inside the telemetry report. Each entry is rendered as its own row in the tooltip and is included alongside the standard metrics **regardless of the `metrics` filter**.

### Custom Attributes Example

```tsx
import { AIAssistViewComponent, PromptRequestEventArgs, TelemetryData } from '@syncfusion/ej2-react-interactive-chat';
import * as React from 'react';
import * as ReactDOM from "react-dom";

function App() {
    const assistInstance = React.useRef<AIAssistViewComponent>(null);
    const suggestions: string[] = [
        "How do I set achievable goals at work?",
        "Why do people fly in their dreams?"
    ];

    const onPromptRequest = (args: PromptRequestEventArgs) => {
        // Your required action here
        setTimeout(() => {
            let defaultResponse = 'For real-time prompt processing, connect the AIAssistView component to your preferred AI service, such as OpenAI or Azure Cognitive Services. Ensure you obtain the necessary API credentials to authenticate and enable seamless integration.';
            // Mock telemetry data with domain-specific custom attributes.
            // Each key in customAttributes becomes its own row in the telemetry tooltip.
            let telemetryData: TelemetryData = {
                model: 'gpt-4o',
                inputTokens: 128,
                outputTokens: 512,
                customAttributes: {
                    region: 'us-east-1',
                    tenant: 'acme',
                    sessionId: 'sess-7841',
                    feature: 'goal-coach'
                }
            };
            assistInstance.current.addPromptResponse(defaultResponse, true, telemetryData);
        }, 1000);
    };

    return (
        // specifies the tag for render the AI AssistView component
        <AIAssistViewComponent
            id="customattributes"
            ref={assistInstance}
            promptRequest={onPromptRequest}
            promptSuggestions={suggestions}
            telemetrySettings={{ enable: true }}
        ></AIAssistViewComponent>
    );
}

ReactDOM.render(<App />, document.getElementById('container'));
```

## Telemetry Types Reference

### TelemetrySettingsModel

Configuration object passed to the `telemetrySettings` property.

| Property | Type | Description |
|----------|------|-------------|
| `enable` | `boolean` | Turns telemetry tracking on or off. |
| `metrics` | `TelemetryMetric[]` | Restricts the report to the specified metrics and fixes their display order. |
| `beforeReport` | `(report: TelemetryReport) => TelemetryReport \| null` | Invoked before every report is delivered. Return the report (modified or not) to deliver it, or `null` to suppress. |

### TelemetryData

Optional argument passed to `addPromptResponse` to attach model and token details to the response. These values are merged into the final telemetry report.

| Property | Type | Description |
|----------|------|-------------|
| `model` | `string` | Name of the AI model used. |
| `inputTokens` | `number` | Number of tokens in the input prompt. |
| `outputTokens` | `number` | Number of tokens generated in the response. |
| `reasoningTokens` | `number` | Number of tokens consumed for reasoning. |
| `cachedInputTokens` | `number` | Number of input tokens served from the cache. |
| `customAttributes` | `{ [key: string]: any }` | Domain-specific attributes rendered as their own rows. |

### TelemetryReport

Object passed to the `beforeReport` callback and bound to the response's telemetry tooltip.

| Property | Type | Description |
|----------|------|-------------|
| `status` | `'completed' \| 'canceled'` | Final status of the response turn. |
| `duration` | `number` | Total time taken in milliseconds. |
| `toolCalls` | `number` | Number of tool invocations. |
| `streamingChunks` | `number` | Number of streamed chunks received. |
| `model` | `string` | AI model used (from `TelemetryData`). |
| `inputTokens` | `number` | Input token count (from `TelemetryData`). |
| `outputTokens` | `number` | Output token count (from `TelemetryData`). |
| `totalTokens` | `number` | Auto-computed as `inputTokens + outputTokens`. |
| `reasoningTokens` | `number` | Reasoning token count (from `TelemetryData`). |
| `cachedInputTokens` | `number` | Cached input token count (from `TelemetryData`). |
| `customAttributes` | `{ [key: string]: any }` | Custom attributes added in `beforeReport` or via `TelemetryData`. |

## Key Characteristics

The following are the key behaviors and constraints of the telemetry support:

1. **Turn tracking** — A telemetry turn begins when a prompt is sent (or `executePrompt` is called) and ends when the response is completed or canceled (via the `Stop responding` button, `cancelPrompt`, or `failPrompt`). The status is recorded as `completed` or `canceled` respectively in the report.

2. **Telemetry button state** — The telemetry button in the response toolbar displays `— ms` (a disabled state) when no telemetry report is available for the completed or cancelled response, and then shows only the `duration` value once the report is generated.

3. **Tooltip data display** — The telemetry tooltip only displays rows for metrics that have valid values. Count-based metrics `toolCalls`, `inputTokens`, `outputTokens`, `totalTokens`, `reasoningTokens`, `cachedInputTokens`, and `streamingChunks` are displayed only when their values are positive numbers (greater than 0). Other metrics such as `model` and `status` are displayed whenever they are present.

4. **Duration formatting** — Duration values below 1 second are displayed in `ms`, and values of 1 second or above are displayed in `s` with two decimal places (for example, `500 ms`, `1.25 s`).

5. **Total tokens** — The `totalTokens` metric is automatically computed as the sum of `inputTokens` and `outputTokens` whenever either value is provided through `TelemetryData`.

6. **Metrics filter precedence** — When the `metrics` property is configured, only the specified metrics are retained in the report. However, `status` and `duration` are always retained, and `customAttributes` are always included in the tooltip.

7. **Regenerate flow** — Each regenerated response emits its own telemetry report. The report of the latest completion is bound to the prompt response.
