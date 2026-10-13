# Mentions

## Table of Contents
- [Mentions Overview](#mentions-overview)
- [Getting Started with Mentions](#getting-started-with-mentions)
- [MentionSettings Properties](#mentionsettings-properties)
- [Configure the Mention Trigger](#configure-the-mention-trigger)
- [Configure the Mention Data Source](#configure-the-mention-data-source)
- [Configure Mention Fields](#configure-mention-fields)
- [Configure the Query](#configure-the-query)
- [Configure the Filter Type](#configure-the-filter-type)
- [Configure Highlighting](#configure-highlighting)
- [Configure the Mention Character in Selected Mentions](#configure-the-mention-character-in-selected-mentions)
- [Configure the Display Template](#configure-the-display-template)
- [Configure the Popup Size](#configure-the-popup-size)
- [Configure the Item Template](#configure-the-item-template)
- [Configure the No Records Template](#configure-the-no-records-template)
- [Handle Mention Selection](#handle-mention-selection)

## Mentions Overview

The AI AssistView supports **mentions** in the prompt editor, allowing users to select and reference items from a configured data source. When users type the configured mention character, a suggestion popup displays matching items. Users can then select an item to insert it as a mention in the prompt.

Use the [`mentions`](../api/ai-assistview#mentions) property to configure the mention trigger character, data source, field mappings, filtering behavior, popup dimensions, and templates. The `mentions` property accepts an **array** of mention settings so you can support multiple trigger characters (e.g. `@` for agents and `/` for slash commands) in the same prompt editor.

## Getting Started with Mentions

The following example demonstrates a basic mention configuration using the `@` character as the trigger and a local data source.

```tsx
import * as React from "react";
import * as ReactDOM from "react-dom";
import { AIAssistViewComponent, PromptRequestEventArgs } from "@syncfusion/ej2-react-interactive-chat";


function App() {
    const agents = [
      {
        id: "TechSupport",
        name: "TechSupport",
        placeholder: "Ask about VPN, network, or device issues",
      },
      {
        id: "HRAssistant",
        name: "HRAssistant",
        placeholder: "Ask about leave, benefits, and HR policies",
      },
      {
        id: "KnowledgeBase",
        name: "KnowledgeBase",
        placeholder: "Search the internal knowledge base",
      },
    ];

    const assistInstance = React.useRef<AIAssistViewComponent>(null);
    const promptRequest = (args: PromptRequestEventArgs) => {
        window.setTimeout(() => {
            assistInstance.current.addPromptResponse(
                "For real-time prompt processing, connect the AIAssistView component to your preferred AI service, such as OpenAI or Azure Cognitive Services. Ensure you obtain the necessary API credentials to authenticate and enable seamless integration.",
            );
        }, 500);
    };

    return (
        <AIAssistViewComponent
            id="basic"
            ref={assistInstance}
            promptPlaceholder="Type '@' to mention an agent..."
            mentions={[
                {
                    mentionChar: "@",
                    dataSource: agents,
                    fields: { text: "name", value: "id" },
                },
            ]}
            promptRequest={promptRequest}
        />
    );
}

ReactDOM.render(<App />, document.getElementById("container"));
```

## MentionSettings Properties

The `mentions` property accepts an array of mention settings. Each mention setting supports the following properties.

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `mentionChar` | `string` | `''` | Specifies the trigger character that opens the mention suggestion popup. |
| `dataSource` | `string[] \| DataManager \| { [key: string]: Object; }[] \| number[] \| boolean[]` | `[]` | Specifies the data source used to populate the mention suggestion popup. You can provide a local collection or a `DataManager` instance for local or remote data. |
| `fields` | `FieldSettingsModel` | `{ text: 'text', value: 'id' }` | Specifies the field mappings used to display and identify mention items. |
| `query` | `Query` | `null` | Specifies the `Query` used to retrieve and process data from the configured data source. |
| `filterType` | `FilterType` | `'Contains'` | Specifies how the typed text is matched against mention items. |
| `highlight` | `boolean` | `false` | Specifies whether the matching text is highlighted in the mention suggestion popup. |
| `showMentionChar` | `boolean` | `true` | Specifies whether the mention trigger character is displayed with the selected mention in the prompt editor. |
| `popupWidth` | `string \| number` | `'auto'` | Specifies the width of the mention suggestion popup. |
| `popupHeight` | `string \| number` | `'300px'` | Specifies the height of the mention suggestion popup. |
| `displayTemplate` | `string \| Function` | `''` | Specifies the template used to customize the appearance of a selected mention in the prompt editor. |
| `itemTemplate` | `string` | `''` | Specifies the template used to customize the appearance of items in the mention suggestion popup. |
| `noRecordsTemplate` | `string` | `'No records found'` | Specifies the template displayed in the mention suggestion popup when no matching items are available. |

## Configure the Mention Trigger

Use the `mentionChar` property inside [`mentions`](../api/ai-assistview#mentions) to configure characters such as `@` or `/`. When the configured character is typed in the prompt editor, the mention suggestion popup is displayed.

You can configure multiple mention settings on the same component to support different trigger characters for different data sources (for example, `@` for agents and `/` for slash commands).

```tsx
import * as React from "react";
import * as ReactDOM from "react-dom";
import { AIAssistViewComponent, PromptRequestEventArgs } from "@syncfusion/ej2-react-interactive-chat";


function App() {
    const agents = [
      {
        id: "TechSupport",
        name: "TechSupport",
        placeholder: "Ask about VPN, network, or device issues",
      },
      {
        id: "HRAssistant",
        name: "HRAssistant",
        placeholder: "Ask about leave, benefits, and HR policies",
      },
      {
        id: "KnowledgeBase",
        name: "KnowledgeBase",
        placeholder: "Search the internal knowledge base",
      },
    ];
    const commands = [
      {
        id: "table",
        name: "/table",
        placeholder: "Format the response as a table",
      },
      {
        id: "rewrite",
        name: "/rewrite",
        placeholder: "Improve clarity and professional tone",
      },
      {
        id: "checklist",
        name: "/checklist",
        placeholder: "Convert the response into a checklist",
      },
    ];
    const assistInstance = React.useRef<AIAssistViewComponent>(null);
    const promptRequest = (args: PromptRequestEventArgs) => {
        window.setTimeout(
            () =>
                assistInstance.current.addPromptResponse(
                    "For real-time prompt processing, connect the AIAssistView component to your preferred AI service, such as OpenAI or Azure Cognitive Services. Ensure you obtain the necessary API credentials to authenticate and enable seamless integration.",
                ),
            500,
        );
    };
    return (
        <AIAssistViewComponent
            id="mention-char"
            ref={assistInstance}
            promptPlaceholder="Type '@' for agents or '/' for commands..."
            mentions={[
                {
                    mentionChar: "@",
                    dataSource: agents,
                    fields: { text: "name", value: "id" },
                },
                {
                    mentionChar: "/",
                    dataSource: commands,
                    showMentionChar: false,
                    fields: { text: "name", value: "id" },
                },
            ]}
            promptRequest={promptRequest}
        />
    );
}

ReactDOM.render(<App />, document.getElementById("container"));
```

## Configure the Mention Data Source

Use the `dataSource` property inside [`mentions`](../api/ai-assistview#mentions) to provide a local collection, such as a string array or an object array, or use a `DataManager` instance to retrieve data locally or from a remote data source.

```tsx
import * as React from "react";
import * as ReactDOM from "react-dom";
import { AIAssistViewComponent,PromptRequestEventArgs } from "@syncfusion/ej2-react-interactive-chat";

function App() {
    const agents = [
      {
        id: "TechSupport",
        name: "TechSupport",
        placeholder: "Ask about VPN, network, or device issues",
      },
      {
        id: "HRAssistant",
        name: "HRAssistant",
        placeholder: "Ask about leave, benefits, and HR policies",
      },
      {
        id: "KnowledgeBase",
        name: "KnowledgeBase",
        placeholder: "Search the internal knowledge base",
      },
    ];
    const commands = [
      {
        id: "table",
        name: "/table",
        placeholder: "Format the response as a table",
      },
      {
        id: "rewrite",
        name: "/rewrite",
        placeholder: "Improve clarity and professional tone",
      },
      {
        id: "checklist",
        name: "/checklist",
        placeholder: "Convert the response into a checklist",
      },
    ];
    const assistInstance = React.useRef<AIAssistViewComponent>(null);
    const promptRequest = (args: PromptRequestEventArgs) =>
        window.setTimeout(
            () =>
                assistInstance.current.addPromptResponse(
                    "For real-time prompt processing, connect the AIAssistView component to your preferred AI service, such as OpenAI or Azure Cognitive Services. Ensure you obtain the necessary API credentials to authenticate and enable seamless integration.",
                ),500,);
    return (
        <AIAssistViewComponent
            id="data-source"
            ref={assistInstance}
            promptPlaceholder="Type '@' for agents or '/' for commands..."
            mentions={[
                {
                    mentionChar: "@",
                    dataSource: agents,
                    fields: { text: "name", value: "id" },
                },
                {
                    mentionChar: "/",
                    dataSource: commands,
                    showMentionChar: false,
                    fields: { text: "name", value: "id" },
                },
            ]}
            promptRequest={promptRequest}
        />
    );
}
ReactDOM.render(<App />, document.getElementById("container"));
```

## Configure Mention Fields

Use the `fields` property inside [`mentions`](../api/ai-assistview#mentions) to map object-based data sources, specifying the property names that contain the display text and value. The default field mapping is `{ text: 'text', value: 'id' }`.

```tsx
import * as React from "react";
import * as ReactDOM from "react-dom";
import { AIAssistViewComponent, PromptRequestEventArgs } from "@syncfusion/ej2-react-interactive-chat";

function App() {
    const agents = [
      {
        agentId: "TechSupport",
        agentName: "TechSupport",
        placeholder: "Ask about VPN, network, or device issues",
      },
      {
        agentId: "HRAssistant",
        agentName: "HRAssistant",
        placeholder: "Ask about leave, benefits, and HR policies",
      },
      {
        agentId: "KnowledgeBase",
        agentName: "KnowledgeBase",
        placeholder: "Search the internal knowledge base",
      },
    ];
    const commands = [
      {
        commandId: "table",
        commandName: "/table",
        placeholder: "Format the response as a table",
      },
      {
        commandId: "rewrite",
        commandName: "/rewrite",
        placeholder: "Improve clarity and professional tone",
      },
      {
        commandId: "checklist",
        commandName: "/checklist",
        placeholder: "Convert the response into a checklist",
      },
    ];
    const assistInstance = React.useRef<AIAssistViewComponent>(null);
    const promptRequest = (args: PromptRequestEventArgs) =>
        window.setTimeout(
            () =>
                assistInstance.current.addPromptResponse(
                    "For real-time prompt processing, connect the AIAssistView component to your preferred AI service, such as OpenAI or Azure Cognitive Services. Ensure you obtain the necessary API credentials to authenticate and enable seamless integration.",
                ),500,);
    return (
        <AIAssistViewComponent
            id="fields"
            ref={assistInstance}
            promptPlaceholder="Type '@' for agents or '/' for commands..."
            mentions={[
                {
                    mentionChar: "@",
                    dataSource: agents,
                    fields: { text: "agentName", value: "agentId" },
                },
                {
                    mentionChar: "/",
                    dataSource: commands,
                    showMentionChar: false,
                    fields: { text: "commandName", value: "commandId" },
                },
            ]}
            promptRequest={promptRequest}
        />
    );
}
ReactDOM.render(<App />, document.getElementById("container"));
```

## Configure the Query

Use the `query` property to apply additional data operations to the configured mention data source. It accepts a Syncfusion `Query` object. The default value is `null`.

```tsx
import * as React from "react";
import * as ReactDOM from "react-dom";
import { Query } from "@syncfusion/ej2-data";
import { AIAssistViewComponent, PromptRequestEventArgs } from "@syncfusion/ej2-react-interactive-chat";

function App() {
    const agents = [
      {
        id: "TechSupport",
        name: "TechSupport",
        placeholder: "Ask about VPN, network, or device issues",
      },
      {
        id: "HRAssistant",
        name: "HRAssistant",
        placeholder: "Ask about leave, benefits, and HR policies",
      },
      {
        id: "KnowledgeBase",
        name: "KnowledgeBase",
        placeholder: "Search the internal knowledge base",
      },
    ];
    const commands = [
      {
        id: "table",
        name: "/table",
        placeholder: "Format the response as a table",
      },
      {
        id: "rewrite",
        name: "/rewrite",
        placeholder: "Improve clarity and professional tone",
      },
      {
        id: "checklist",
        name: "/checklist",
        placeholder: "Convert the response into a checklist",
      },
    ];
    const assistInstance = React.useRef<AIAssistViewComponent>(null);
    const promptRequest = (args: PromptRequestEventArgs) =>
        window.setTimeout(
            () =>
                assistInstance.current.addPromptResponse(
                    "For real-time prompt processing, connect the AIAssistView component to your preferred AI service, such as OpenAI or Azure Cognitive Services. Ensure you obtain the necessary API credentials to authenticate and enable seamless integration.",
                ),500,);
    return (
        <AIAssistViewComponent
            id="query"
            ref={assistInstance}
            promptPlaceholder="Type '@' for agents or '/' for commands..."
            mentions={[
                {
                    mentionChar: "@",
                    dataSource: agents,
                    fields: { text: "name", value: "id" },
                    query: new Query().take(2),
                },
                {
                    mentionChar: "/",
                    dataSource: commands,
                    showMentionChar: false,
                    fields: { text: "name", value: "id" },
                    query: new Query().take(2),
                },
            ]}
            promptRequest={promptRequest}
        />
    );
}
ReactDOM.render(<App />, document.getElementById("container"));
```

## Configure the Filter Type

Use the `filterType` property to specify how the text typed after the mention trigger character is matched against the items in the data source. The default value is `Contains`.

### Supported Filter Types

| Filter type | Description |
|-------------|-------------|
| `Contains` | Matches items whose display text contains the typed text. |
| `StartsWith` | Matches items whose display text starts with the typed text. |
| `EndsWith` | Matches items whose display text ends with the typed text. |

```tsx
import * as React from "react";
import * as ReactDOM from "react-dom";
import { AIAssistViewComponent, PromptRequestEventArgs } from "@syncfusion/ej2-react-interactive-chat";

function App() {
    const agents = [
      {
        id: "TechSupport",
        name: "TechSupport",
        placeholder: "Ask about VPN, network, or device issues",
      },
      {
        id: "HRAssistant",
        name: "HRAssistant",
        placeholder: "Ask about leave, benefits, and HR policies",
      },
      {
        id: "KnowledgeBase",
        name: "KnowledgeBase",
        placeholder: "Search the internal knowledge base",
      },
    ];
    const commands = [
      {
        id: "table",
        name: "/table",
        placeholder: "Format the response as a table",
      },
      {
        id: "rewrite",
        name: "/rewrite",
        placeholder: "Improve clarity and professional tone",
      },
      {
        id: "checklist",
        name: "/checklist",
        placeholder: "Convert the response into a checklist",
      },
    ];
    const assistInstance = React.useRef<AIAssistViewComponent>(null);
    const promptRequest = (args: PromptRequestEventArgs) =>
        window.setTimeout(
            () =>
                assistInstance.current.addPromptResponse(
                    "For real-time prompt processing, connect the AIAssistView component to your preferred AI service, such as OpenAI or Azure Cognitive Services. Ensure you obtain the necessary API credentials to authenticate and enable seamless integration.",
                ),500,);
    return (
        <AIAssistViewComponent
            id="filter-type"
            ref={assistInstance}
            promptPlaceholder="Type '@' for agents or '/' for commands..."
            mentions={[
                {
                    mentionChar: "@",
                    dataSource: agents,
                    fields: { text: "name", value: "id" },
                    filterType: "StartsWith",
                },
                {
                    mentionChar: "/",
                    dataSource: commands,
                    showMentionChar: false,
                    fields: { text: "name", value: "id" },
                    filterType: "StartsWith",
                },
            ]}
            promptRequest={promptRequest}
        />
    );
}
ReactDOM.render(<App />, document.getElementById("container"));
```

## Configure Highlighting

Use the `highlight` property to specify whether the matching text is highlighted in the mention suggestion popup. The default value is `false`. When set to `true`, the portion of the suggestion item that matches the typed text is highlighted.

```tsx
import * as React from "react";
import * as ReactDOM from "react-dom";
import { AIAssistViewComponent, PromptRequestEventArgs } from "@syncfusion/ej2-react-interactive-chat";

function App() {
    const agents = [
      {
        id: "TechSupport",
        name: "TechSupport",
        placeholder: "Ask about VPN, network, or device issues",
      },
      {
        id: "HRAssistant",
        name: "HRAssistant",
        placeholder: "Ask about leave, benefits, and HR policies",
      },
      {
        id: "KnowledgeBase",
        name: "KnowledgeBase",
        placeholder: "Search the internal knowledge base",
      },
    ];
    const commands = [
      {
        id: "table",
        name: "/table",
        placeholder: "Format the response as a table",
      },
      {
        id: "rewrite",
        name: "/rewrite",
        placeholder: "Improve clarity and professional tone",
      },
      {
        id: "checklist",
        name: "/checklist",
        placeholder: "Convert the response into a checklist",
      },
    ];
    const assistInstance = React.useRef<AIAssistViewComponent>(null);
    const promptRequest = (args: PromptRequestEventArgs) =>
        window.setTimeout(
            () =>
                assistInstance.current.addPromptResponse(
                    "For real-time prompt processing, connect the AIAssistView component to your preferred AI service, such as OpenAI or Azure Cognitive Services. Ensure you obtain the necessary API credentials to authenticate and enable seamless integration.",
                ),500,);
    return (
        <AIAssistViewComponent
            id="highlight"
            ref={assistInstance}
            promptPlaceholder="Type '@' for agents or '/' for commands..."
            mentions={[
                {
                    mentionChar: "@",
                    dataSource: agents,
                    fields: { text: "name", value: "id" },
                    highlight: true,
                },
                {
                    mentionChar: "/",
                    dataSource: commands,
                    showMentionChar: false,
                    fields: { text: "name", value: "id" },
                    highlight: true,
                },
            ]}
            promptRequest={promptRequest}
        />
    );
}
ReactDOM.render(<App />, document.getElementById("container"));
```

## Configure the Mention Character in Selected Mentions

Use the `showMentionChar` property to specify whether the configured mention trigger character is displayed with the selected mention. The default value is `true`. When set to `false`, the trigger character is omitted from the displayed mention.

```tsx
import * as React from "react";
import * as ReactDOM from "react-dom";
import { AIAssistViewComponent, PromptRequestEventArgs } from "@syncfusion/ej2-react-interactive-chat";

function App() {
    const agents = [
      {
        id: "TechSupport",
        name: "TechSupport",
        placeholder: "Ask about VPN, network, or device issues",
      },
      {
        id: "HRAssistant",
        name: "HRAssistant",
        placeholder: "Ask about leave, benefits, and HR policies",
      },
      {
        id: "KnowledgeBase",
        name: "KnowledgeBase",
        placeholder: "Search the internal knowledge base",
      },
    ];
    const commands = [
      {
        id: "table",
        name: "/table",
        placeholder: "Format the response as a table",
      },
      {
        id: "rewrite",
        name: "/rewrite",
        placeholder: "Improve clarity and professional tone",
      },
      {
        id: "checklist",
        name: "/checklist",
        placeholder: "Convert the response into a checklist",
      },
    ];
    const assistInstance = React.useRef<AIAssistViewComponent>(null);
    const promptRequest = (args: PromptRequestEventArgs) =>
        window.setTimeout(
            () =>
                assistInstance.current.addPromptResponse(
                    "For real-time prompt processing, connect the AIAssistView component to your preferred AI service, such as OpenAI or Azure Cognitive Services. Ensure you obtain the necessary API credentials to authenticate and enable seamless integration.",
                ),500,);
    return (
        <AIAssistViewComponent
            id="show-mention-char"
            ref={assistInstance}
            promptPlaceholder="Type '@' for agents or '/' for commands..."
            mentions={[
                {
                    mentionChar: "@",
                    dataSource: agents,
                    fields: { text: "name", value: "id" },
                    showMentionChar: false,
                },
                {
                    mentionChar: "/",
                    dataSource: commands,
                    showMentionChar: false,
                    fields: { text: "name", value: "id" },
                },
            ]}
            promptRequest={promptRequest}
        />
    );
}
ReactDOM.render(<App />, document.getElementById("container"));
```

## Configure the Display Template

Use the `displayTemplate` property to customize the appearance of a selected mention in the prompt editor. The template receives the selected item's data, allowing you to customize the rendered mention content. For example, you can display an icon, avatar, or additional information along with the item's display text.

```tsx
import * as React from "react";
import * as ReactDOM from "react-dom";
import {
    AIAssistViewComponent,
    PromptRequestEventArgs,
} from "@syncfusion/ej2-react-interactive-chat";

const agents = [
  {
    id: "TechSupport",
    name: "TechSupport",
    placeholder: "Ask about VPN, network, or device issues",
  },
  {
    id: "HRAssistant",
    name: "HRAssistant",
    placeholder: "Ask about leave, benefits, and HR policies",
  },
  {
    id: "KnowledgeBase",
    name: "KnowledgeBase",
    placeholder: "Search the internal knowledge base",
  },
];
const commands = [
  {
    id: "table",
    name: "/table",
    placeholder: "Format the response as a table",
  },
  {
    id: "rewrite",
    name: "/rewrite",
    placeholder: "Improve clarity and professional tone",
  },
  {
    id: "checklist",
    name: "/checklist",
    placeholder: "Convert the response into a checklist",
  },
];
const displayTemplate =
    '<span class="e-aiassist-mention-item-chip">${name}</span>';

function App() {
    const assistInstance = React.useRef<AIAssistViewComponent>(null);
    const promptRequest = (args: PromptRequestEventArgs) =>
        window.setTimeout(
            () =>
                assistInstance.current.addPromptResponse(
                    "For real-time prompt processing, connect the AIAssistView component to your preferred AI service, such as OpenAI or Azure Cognitive Services. Ensure you obtain the necessary API credentials to authenticate and enable seamless integration.",
                ),
            500,
        );
    return (
        <AIAssistViewComponent
            id="display-template"
            ref={assistInstance}
            promptPlaceholder="Type '@' for agents or '/' for commands..."
            mentions={[
                {
                    mentionChar: "@",
                    dataSource: agents,
                    fields: { text: "name", value: "id" },
                    displayTemplate,
                },
                {
                    mentionChar: "/",
                    dataSource: commands,
                    showMentionChar: false,
                    fields: { text: "name", value: "id" },
                    displayTemplate,
                },
            ]}
            promptRequest={promptRequest}
        />
    );
}
ReactDOM.render(<App />, document.getElementById("container"));
```

## Configure the Popup Size

Use the `popupWidth` and `popupHeight` properties to customize the dimensions of the mention suggestion popup.

- **`popupWidth`** — Specifies the popup width. It accepts CSS width values such as `'400px'` or `'50%'`, or a numeric value representing pixels. The default value is `'auto'`.
- **`popupHeight`** — Specifies the popup height. It accepts CSS height values such as `'300px'` or `'50%'`, or a numeric value representing pixels. The default value is `'300px'`.

```tsx
import * as React from "react";
import * as ReactDOM from "react-dom";
import { AIAssistViewComponent, PromptRequestEventArgs } from "@syncfusion/ej2-react-interactive-chat";

function App() {
    const agents = [
      {
        id: "TechSupport",
        name: "TechSupport",
        placeholder: "Ask about VPN, network, or device issues",
      },
      {
        id: "HRAssistant",
        name: "HRAssistant",
        placeholder: "Ask about leave, benefits, and HR policies",
      },
      {
        id: "KnowledgeBase",
        name: "KnowledgeBase",
        placeholder: "Search the internal knowledge base",
      },
    ];
    const commands = [
      {
        id: "table",
        name: "/table",
        placeholder: "Format the response as a table",
      },
      {
        id: "rewrite",
        name: "/rewrite",
        placeholder: "Improve clarity and professional tone",
      },
      {
        id: "checklist",
        name: "/checklist",
        placeholder: "Convert the response into a checklist",
      },
    ];
    const assistInstance = React.useRef<AIAssistViewComponent>(null);
    const promptRequest = (args: PromptRequestEventArgs) =>
        window.setTimeout(
            () =>
                assistInstance.current.addPromptResponse(
                    "For real-time prompt processing, connect the AIAssistView component to your preferred AI service, such as OpenAI or Azure Cognitive Services. Ensure you obtain the necessary API credentials to authenticate and enable seamless integration.",
                ),500,);
    return (
        <AIAssistViewComponent
            id="display-template"
            ref={assistInstance}
            promptPlaceholder="Type '@' for agents or '/' for commands..."
            mentions={[
                {
                    mentionChar: "@",
                    dataSource: agents,
                    fields: { text: "name", value: "id" },
                    popupWidth: '300px',
                    popupHeight: '200px',
                },
                {
                    mentionChar: "/",
                    dataSource: commands,
                    showMentionChar: false,
                    fields: { text: "name", value: "id" },
                    popupWidth: '300px',
                    popupHeight: '200px',
                },
            ]}
            promptRequest={promptRequest}
        />
    );
}
ReactDOM.render(<App />, document.getElementById("container"));
```

## Configure the Item Template

Use the `itemTemplate` property to customize the appearance of items in the mention suggestion popup. You can use it to display additional information or visual elements, such as an avatar, icon, or description, along with the mention item's display text.

```tsx
import * as React from "react";
import * as ReactDOM from "react-dom";
import { AIAssistViewComponent, PromptRequestEventArgs } from "@syncfusion/ej2-react-interactive-chat";

function App() {
    const agents = [
      {
        id: "TechSupport",
        name: "TechSupport",
        description: "Troubleshoot technical issues",
        placeholder: "Ask about VPN, network, or device issues",
      },
      {
        id: "HRAssistant",
        name: "HRAssistant",
        description: "Get assistance with HR policies",
        placeholder: "Ask about leave, benefits, and HR policies",
      },
      {
        id: "KnowledgeBase",
        name: "KnowledgeBase",
        description: "Search internal documentation",
        placeholder: "Search the internal knowledge base",
      },
    ];
    const commands = [
      {
        id: "table",
        name: "/table",
        description: "Answer as a markdown table",
        placeholder: "Format the response as a table",
      },
      {
        id: "rewrite",
        name: "/rewrite",
        description: "Rewrite content for clarity",
        placeholder: "Improve clarity and professional tone",
      },
      {
        id: "checklist",
        name: "/checklist",
        description: "Create a step-by-step checklist",
        placeholder: "Convert the response into a checklist",
      },
    ];
    const itemTemplate = '<div class="mention-item"><span class="mention-name">${name}</span><span class="mention-description">${description}</span></div>';
    const assistInstance = React.useRef<AIAssistViewComponent>(null);
    const promptRequest = (args: PromptRequestEventArgs) =>
        window.setTimeout(
            () =>
                assistInstance.current.addPromptResponse(
                    "For real-time prompt processing, connect the AIAssistView component to your preferred AI service, such as OpenAI or Azure Cognitive Services. Ensure you obtain the necessary API credentials to authenticate and enable seamless integration.",
                ),500,);
    return (
        <AIAssistViewComponent
            id="item-template"
            ref={assistInstance}
            promptPlaceholder="Type '@' for agents or '/' for commands..."
            mentions={[
                {
                    mentionChar: "@",
                    dataSource: agents,
                    fields: { text: "name", value: "id" },
                    itemTemplate,
                },
                {
                    mentionChar: "/",
                    dataSource: commands,
                    showMentionChar: false,
                    fields: { text: "name", value: "id" },
                    itemTemplate,
                },
            ]}
            promptRequest={promptRequest}
        />
    );
}
ReactDOM.render(<App />, document.getElementById("container"));
```

## Configure the No Records Template

Use the `noRecordsTemplate` property to customize the content displayed when no mention items match the text entered by the user. The default value is `'No records found'`.

```tsx
import * as React from "react";
import * as ReactDOM from "react-dom";
import { AIAssistViewComponent, PromptRequestEventArgs } from "@syncfusion/ej2-react-interactive-chat";

function App() {
    const agents = [
      {
        id: "TechSupport",
        name: "TechSupport",
        placeholder: "Ask about VPN, network, or device issues",
      },
      {
        id: "HRAssistant",
        name: "HRAssistant",
        placeholder: "Ask about leave, benefits, and HR policies",
      },
      {
        id: "KnowledgeBase",
        name: "KnowledgeBase",
        placeholder: "Search the internal knowledge base",
      },
    ];
    const commands = [
      {
        id: "table",
        name: "/table",
        placeholder: "Format the response as a table",
      },
      {
        id: "rewrite",
        name: "/rewrite",
        placeholder: "Improve clarity and professional tone",
      },
      {
        id: "checklist",
        name: "/checklist",
        placeholder: "Convert the response into a checklist",
      },
    ];
    const assistInstance = React.useRef<AIAssistViewComponent>(null);
    const promptRequest = (args: PromptRequestEventArgs) =>
        window.setTimeout(
            () =>
                assistInstance.current.addPromptResponse(
                    "For real-time prompt processing, connect the AIAssistView component to your preferred AI service, such as OpenAI or Azure Cognitive Services. Ensure you obtain the necessary API credentials to authenticate and enable seamless integration.",
                ),500,);
    return (
        <AIAssistViewComponent
            id="no-records-template"
            ref={assistInstance}
            promptPlaceholder="Type '@' for agents or '/' for commands..."
            mentions={[
                {
                    mentionChar: "@",
                    dataSource: agents,
                    fields: { text: "name", value: "id" },
                    noRecordsTemplate: "No matching agents found. Try a different search.",
                },
                {
                    mentionChar: "/",
                    dataSource: commands,
                    showMentionChar: false,
                    fields: { text: "name", value: "id" },
                    noRecordsTemplate: "No matching commands found. Try a different search.",
                },
            ]}
            promptRequest={promptRequest}
        />
    );
}
ReactDOM.render(<App />, document.getElementById("container"));
```

## Handle Mention Selection

The `mentionSelect` event is triggered when a user selects an item from the mention suggestion popup. Use this event to access the selected mention data and perform custom actions.

```tsx
import * as React from "react";
import * as ReactDOM from "react-dom";
import { AIAssistViewComponent, PromptRequestEventArgs } from "@syncfusion/ej2-react-interactive-chat";

function App() {
    const agents = [
      {
        id: "TechSupport",
        name: "TechSupport",
        placeholder: "Ask about VPN, network, or device issues",
      },
      {
        id: "HRAssistant",
        name: "HRAssistant",
        placeholder: "Ask about leave, benefits, and HR policies",
      },
      {
        id: "KnowledgeBase",
        name: "KnowledgeBase",
        placeholder: "Search the internal knowledge base",
      },
    ];
    const commands = [
      {
        id: "table",
        name: "/table",
        placeholder: "Format the response as a table",
      },
      {
        id: "rewrite",
        name: "/rewrite",
        placeholder: "Improve clarity and professional tone",
      },
      {
        id: "checklist",
        name: "/checklist",
        placeholder: "Convert the response into a checklist",
      },
    ];
    const assistInstance = React.useRef<AIAssistViewComponent>(null);
    const [selectedMention, setSelectedMention] = React.useState("");
    const promptRequest = (args: PromptRequestEventArgs) =>
        window.setTimeout(
            () =>
                assistInstance.current.addPromptResponse(
                    "For real-time prompt processing, connect the AIAssistView component to your preferred AI service, such as OpenAI or Azure Cognitive Services. Ensure you obtain the necessary API credentials to authenticate and enable seamless integration.",
                ),500,);
    const mentionSelect = (args: any) =>
        setSelectedMention(
            "Selected mention: " + (args.itemData.name || args.itemData.id),
        );
    return (
        <>
            <AIAssistViewComponent
                id="mention-select"
                ref={assistInstance}
                promptPlaceholder="Type '@' for agents or '/' for commands..."
                mentions={[
                    {
                        mentionChar: "@",
                        dataSource: agents,
                        fields: { text: "name", value: "id" },
                        mentionSelect,
                    },
                    {
                        mentionChar: "/",
                        dataSource: commands,
                        showMentionChar: false,
                        fields: { text: "name", value: "id" },
                        mentionSelect,
                    },
                ]}
                promptRequest={promptRequest}
            />
            <div id="mention-status">{selectedMention}</div>
        </>
    );
}
ReactDOM.render(<App />, document.getElementById("container"));
```

## mentionSelect Event Args

The `mentionSelect` event handler receives a `MentionSelectEventArgs` object that exposes details about the selected mention item. The following table lists the available properties.

| Property | Type | Description |
| --- | --- | --- |
| `cancel` | `boolean` | Specifies whether the mention selection should be canceled. Set to `true` to prevent the default selection action. |
| `event` | `Event` | Specifies the original event that triggered the mention selection. Provides access to the underlying browser event details. |
| `isInteracted` | `boolean` | Specifies whether the selection was initiated through user interaction. Returns `true` when the action is performed directly by the user. |
| `itemData` | `FieldSettingsModel` | Specifies the data associated with the selected mention item. Contains the field settings information for the selected mention. |
| `name` | `string` | Specifies the name of the event. |

```tsx
import * as React from "react";
import * as ReactDOM from "react-dom";
import { AIAssistViewComponent, PromptRequestEventArgs } from "@syncfusion/ej2-react-interactive-chat";

function App() {
    const agents = [
      {
        id: "TechSupport",
        name: "TechSupport",
        placeholder: "Ask about VPN, network, or device issues",
      },
      {
        id: "HRAssistant",
        name: "HRAssistant",
        placeholder: "Ask about leave, benefits, and HR policies",
      },
      {
        id: "KnowledgeBase",
        name: "KnowledgeBase",
        placeholder: "Search the internal knowledge base",
      },
    ];
    const assistInstance = React.useRef<AIAssistViewComponent>(null);
    const promptRequest = (args: PromptRequestEventArgs) =>
        window.setTimeout(
            () =>
                assistInstance.current.addPromptResponse(
                    "For real-time prompt processing, connect the AIAssistView component to your preferred AI service, such as OpenAI or Azure Cognitive Services. Ensure you obtain the necessary API credentials to authenticate and enable seamless integration.",
                ),500,);
    const mentionSelect = (args: any) => {
        // Cancel the default mention selection when needed
        // args.cancel = true;

        // Inspect the underlying browser event
        // console.log('Event type:', args.event?.type);

        // Determine whether the user performed the action
        // console.log('Is user interaction:', args.isInteracted);

        // Access the selected mention data
        // console.log('Selected item:', args.itemData);

        // Inspect the event name
        // console.log('Event name:', args.name);
    };
    return (
        <AIAssistViewComponent
            id="mention-select-args"
            ref={assistInstance}
            promptPlaceholder="Type '@' to mention an agent..."
            mentions={[
                {
                    mentionChar: "@",
                    dataSource: agents,
                    fields: { text: "name", value: "id" },
                    mentionSelect,
                },
            ]}
            promptRequest={promptRequest}
        />
    );
}
ReactDOM.render(<App />, document.getElementById("container"));
```
