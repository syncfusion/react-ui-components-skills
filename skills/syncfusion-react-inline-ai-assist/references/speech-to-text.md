# Speech-to-Text

## Table of Contents
- [Speech-to-Text Overview](#speech-to-text-overview)
- [Prerequisites](#prerequisites)
- [Enable Built-in Speech-to-Text](#enable-built-in-speech-to-text)
- [Configure Speech Recognition Language](#configure-speech-recognition-language)
- [Configure Microphone Button Settings](#configure-microphone-button-settings)
- [Enable Interim Results](#enable-interim-results)
- [Configure Tooltip Settings](#configure-tooltip-settings)
- [Speech-to-Text Events](#speech-to-text-events)
- [Browser Compatibility](#browser-compatibility)

## Speech-to-Text Overview

The Syncfusion React Inline AI Assist component integrates `Speech-to-Text` functionality through the browser's [Web Speech API](https://developer.mozilla.org/en-US/docs/Web/API/Web_Speech_API). This enables the conversion of spoken words into text using the device's microphone, allowing users to provide voice input directly in the Inline AI Assist prompt area.

When content is selected, the recognized speech can be used as a prompt along with the selected content to provide contextual AI assistance. The generated response is displayed inline, where users can review and either accept or discard the suggestion.

The feature is configured through the [`speechToTextSettings`](../api/inline-ai-assist#speechtotextsettings) property (`SpeechToTextSettingsModel`), which provides a single place to control language, interim results, microphone button labels/icons, tooltips, and lifecycle event handlers.

> Note: this is the **built-in** Speech-to-Text feature powered by the Syncfusion settings object — distinct from any custom voice input you might build inside an `editorTemplate`. The built-in option is the recommended path because it handles browser compatibility, prompt injection, and event lifecycles for you.

## Prerequisites

Before integrating Speech-to-Text, ensure the following:

1. The Syncfusion Inline AI Assist component is properly set up in your React application.
2. The Inline AI Assist component is integrated with an AI service (typically [Azure OpenAI](https://azure.microsoft.com/en-us/products/ai-foundry/models/openai)) so that the recognized speech can be turned into an actual AI response.

## Enable Built-in Speech-to-Text

Enable Speech-to-Text by setting the `enable` flag to `true` inside `speechToTextSettings`. Once enabled, a microphone button appears in the Inline AI Assist prompt area, and the recognized speech is converted into prompt text that is passed through the `promptRequest` event when the prompt is submitted.

```tsx
import { InlineAIAssistComponent, InlinePromptRequestEventArgs, ResponseItemSelectEventArgs, SpeechToTextSettingsModel } from '@syncfusion/ej2-react-interactive-chat';
import { enableRipple } from '@syncfusion/ej2-base';
import * as React from 'react';
import * as ReactDOM from 'react-dom';

enableRipple(true);

const App: React.FC = () => {
    const assistRef = React.useRef<InlineAIAssistComponent>(null);
    const editableRef = React.useRef<HTMLDivElement>(null);
    const isAcceptedRef = React.useRef<boolean>(false);

    // Enables the built-in speech-to-text functionality using Web Speech API
    const speechToTextSettings: SpeechToTextSettingsModel = {
        enable: true
    };

    // Opens the Inline AI Assist popup when the Summarize button is clicked
    const handleSummarizeBtnClick = (): void => {
        if (assistRef.current && typeof assistRef.current.showPopup === 'function') {
            assistRef.current.showPopup();
        }
    };

    // Triggered when a prompt is submitted from voice input
    const handlePromptRequest = (args: InlinePromptRequestEventArgs): void => {
        setTimeout(() => {
            // Default response displayed in the AI Assist popup before Accept and Discard
            const defaultResponse: string = 'For real-time prompt processing, connect the Inline AI Assist component to your preferred AI service, such as OpenAI or Azure Cognitive Services. Ensure you obtain the necessary API credentials to authenticate and enable seamless integration.';
            if (assistRef.current && typeof assistRef.current.addResponse === 'function') {
                assistRef.current.addResponse(defaultResponse, true);
            }
        }, 1000);
    };

    // Handles Accept and Discard buttons on the inline response
    const handleResponseItemSelect = (args: ResponseItemSelectEventArgs): void => {
        if (args.command.label === 'Accept') {
            isAcceptedRef.current = true;
            if (editableRef.current) {
                editableRef.current.innerHTML = 'For real-time prompt processing, connect the Inline AI Assist component to your preferred AI service, such as OpenAI or Azure Cognitive Services. Ensure you obtain the necessary API credentials to authenticate and enable seamless integration.';
            }
            if (assistRef.current && typeof assistRef.current.hidePopup === 'function') {
                assistRef.current.hidePopup();
            }
        } else if (args.command.label === 'Discard') {
            isAcceptedRef.current = false;
            if (assistRef.current && typeof assistRef.current.hidePopup === 'function') {
                assistRef.current.hidePopup();
            }
        }
    };

    return (
        <div>
            <button
                id="summarizeBtn"
                className="e-btn e-primary"
                style={{ marginBottom: '10px' }}
                onClick={handleSummarizeBtnClick}
            >
                Content Summarize
            </button>
            <div id="editableText" contentEditable="true" ref={editableRef}>
                <p>Inline AI Assist component provides intelligent text processing capabilities that enhance user productivity. It leverages advanced natural language processing to understand context and deliver precise suggestions. Users can seamlessly integrate AI-powered features into their applications.</p>
                <p>With real-time response streaming and customizable prompts, developers can create interactive experiences. The component supports multiple response modes including inline editing and popup-based interactions.</p>
            </div>
            <InlineAIAssistComponent
                id="stt"
                ref={assistRef}
                relateTo="#summarizeBtn"
                responseMode="Inline"
                promptRequest={handlePromptRequest}
                speechToTextSettings={speechToTextSettings}
                responseSettings={{
                    itemSelect: handleResponseItemSelect
                }}
            />
        </div>
    );
};

ReactDOM.render(<App />, document.getElementById('container'));
```

**Companion CSS** (used by all the variants below):

```css
#container {
    margin: 20px auto;
    width: 650px;
    visibility: hidden;
}

#loader {
    color: #008cff;
    height: 40px;
    left: 45%;
    position: absolute;
    top: 45%;
    width: 30%;
}

#editableText {
    width: 100%;
    min-height: 120px;
    max-height: 300px;
    overflow-y: auto;
    font-size: 16px;
    padding: 12px;
    border-radius: 4px;
    border: 1px solid;
}
```

## Configure Speech Recognition Language

The `lang` property of `speechToTextSettings` sets the language code for speech recognition. By default, it uses the browser's language settings, but you can specify a custom language code, such as `en-US`, `es-ES`, or `fr-FR`. This ensures that the speech recognition engine recognizes and transcribes speech in the specified language accurately.

Common language codes include:

| Code | Language |
|------|----------|
| `en-US` | English — United States |
| `en-GB` | English — Great Britain |
| `es-ES` | Spanish — Spain |
| `es-MX` | Spanish — Mexico |
| `fr-FR` | French — France |
| `de-DE` | German — Germany |
| `zh-CN` | Mandarin Chinese (Simplified) |
| `ja-JP` | Japanese |

## Configure Microphone Button Settings

The `buttonSettings` property of `speechToTextSettings` lets you customize the microphone button's appearance and text content. Available sub-properties:

| Property | Description |
|----------|-------------|
| `content` | Text displayed when the button is idle. |
| `stopContent` | Text displayed while recording. |
| `iconCss` | CSS class for the icon shown when idle. |
| `stopIconCss` | CSS class for the icon shown while recording. |

## Enable Interim Results

The `allowInterimResults` property of `speechToTextSettings` enables real-time transcription while the user is speaking. When set to `true`, the speech recognition engine returns interim transcripts as it processes the audio. This provides immediate feedback by displaying partially recognized speech before the final transcript is generated.

The example below demonstrates how to configure Inline AI Assist with the speech recognition language, microphone button settings, and interim results together.

```tsx
import { InlineAIAssistComponent, InlinePromptRequestEventArgs, ResponseItemSelectEventArgs, SpeechToTextSettingsModel } from '@syncfusion/ej2-react-interactive-chat';
import { enableRipple } from '@syncfusion/ej2-base';
import * as React from 'react';
import * as ReactDOM from 'react-dom';

enableRipple(true);

const App: React.FC = () => {
    const assistRef = React.useRef<InlineAIAssistComponent>(null);
    const editableRef = React.useRef<HTMLDivElement>(null);
    const isAcceptedRef = React.useRef<boolean>(false);

    // Configures language, interim results, and custom microphone button labels and icons
    const speechToTextSettings: SpeechToTextSettingsModel = {
        enable: true,
        lang: 'en-US',
        allowInterimResults: true,
        buttonSettings: {
            content: 'Start Recording',
            stopContent: 'Stop Recording',
            iconCss: 'e-icons e-microphone',
            stopIconCss: 'e-icons e-microphone-off'
        }
    };

    // Opens the Inline AI Assist popup on Summarize button click
    const handleSummarizeBtnClick = (): void => {
        if (assistRef.current && typeof assistRef.current.showPopup === 'function') {
            assistRef.current.showPopup();
        }
    };

    // Triggered once the recognized voice prompt is submitted
    const handlePromptRequest = (args: InlinePromptRequestEventArgs): void => {
        setTimeout(() => {
            // Default response displayed in the AI Assist popup before Accept and Discard
            const defaultResponse: string = 'For real-time prompt processing, connect the Inline AI Assist component to your preferred AI service, such as OpenAI or Azure Cognitive Services. Ensure you obtain the necessary API credentials to authenticate and enable seamless integration.';
            if (assistRef.current && typeof assistRef.current.addResponse === 'function') {
                assistRef.current.addResponse(defaultResponse, true);
            }
        }, 1000);
    };

    // Handles Accept and Discard buttons on the inline response
    const handleResponseItemSelect = (args: ResponseItemSelectEventArgs): void => {
        if (args.command.label === 'Accept') {
            isAcceptedRef.current = true;
            if (editableRef.current) {
                editableRef.current.innerHTML = 'For real-time prompt processing, connect the Inline AI Assist component to your preferred AI service, such as OpenAI or Azure Cognitive Services. Ensure you obtain the necessary API credentials to authenticate and enable seamless integration.';
            }
            if (assistRef.current && typeof assistRef.current.hidePopup === 'function') {
                assistRef.current.hidePopup();
            }
        } else if (args.command.label === 'Discard') {
            isAcceptedRef.current = false;
            if (assistRef.current && typeof assistRef.current.hidePopup === 'function') {
                assistRef.current.hidePopup();
            }
        }
    };

    return (
        <div>
            <button
                id="summarizeBtn"
                className="e-btn e-primary"
                style={{ marginBottom: '10px' }}
                onClick={handleSummarizeBtnClick}
            >
                Content Summarize
            </button>
            <div id="editableText" contentEditable="true" ref={editableRef}>
                <p>Inline AI Assist component provides intelligent text processing capabilities that enhance user productivity. It leverages advanced natural language processing to understand context and deliver precise suggestions. Users can seamlessly integrate AI-powered features into their applications.</p>
                <p>With real-time response streaming and customizable prompts, developers can create interactive experiences. The component supports multiple response modes including inline editing and popup-based interactions.</p>
            </div>
            <InlineAIAssistComponent
                id="speech-configuration"
                ref={assistRef}
                relateTo="#summarizeBtn"
                responseMode="Inline"
                promptRequest={handlePromptRequest}
                speechToTextSettings={speechToTextSettings}
                responseSettings={{
                    itemSelect: handleResponseItemSelect
                }}
            />
        </div>
    );
};

ReactDOM.render(<App />, document.getElementById('container'));
```

## Configure Tooltip Settings

You can customize the tooltips displayed for the microphone button using the `tooltipSettings` property of `speechToTextSettings`. The tooltip can provide different information based on whether speech recognition is idle or active.

```tsx
import { InlineAIAssistComponent, InlinePromptRequestEventArgs, ResponseItemSelectEventArgs, SpeechToTextSettingsModel } from '@syncfusion/ej2-react-interactive-chat';
import { enableRipple } from '@syncfusion/ej2-base';
import * as React from 'react';
import * as ReactDOM from 'react-dom';

enableRipple(true);

const App: React.FC = () => {
    const assistRef = React.useRef<InlineAIAssistComponent>(null);
    const editableRef = React.useRef<HTMLDivElement>(null);

    // Custom tooltips shown for the microphone button while idle and recording
    const speechToTextSettings: SpeechToTextSettingsModel = {
        enable: true,
        tooltipSettings: {
            content: 'Click to start recording',
            stopContent: 'Click to stop recording'
        }
    };

    // Opens the popup on Summarize button click
    const handleSummarizeBtnClick = (): void => {
        if (assistRef.current && typeof assistRef.current.showPopup === 'function') {
            assistRef.current.showPopup();
        }
    };

    // Triggered when the voice prompt is submitted
    const handlePromptRequest = (args: InlinePromptRequestEventArgs): void => {
        setTimeout(() => {
            const defaultResponse: string = 'For real-time prompt processing, connect the Inline AI Assist component to your preferred AI service, such as OpenAI or Azure Cognitive Services. Ensure you obtain the necessary API credentials to authenticate and enable seamless integration.';
            if (assistRef.current && typeof assistRef.current.addResponse === 'function') {
                assistRef.current.addResponse(defaultResponse, true);
            }
        }, 1000);
    };

    // Handles Accept and Discard buttons on the inline response
    const handleResponseItemSelect = (args: ResponseItemSelectEventArgs): void => {
        if (args.command.label === 'Accept') {
            if (editableRef.current) {
                editableRef.current.innerHTML = 'For real-time prompt processing, connect the Inline AI Assist component to your preferred AI service, such as OpenAI or Azure Cognitive Services. Ensure you obtain the necessary API credentials to authenticate and enable seamless integration.';
            }
            if (assistRef.current && typeof assistRef.current.hidePopup === 'function') {
                assistRef.current.hidePopup();
            }
        } else if (args.command.label === 'Discard') {
            if (assistRef.current && typeof assistRef.current.hidePopup === 'function') {
                assistRef.current.hidePopup();
            }
        }
    };

    return (
        <div>
            <button
                id="summarizeBtn"
                className="e-btn e-primary"
                style={{ marginBottom: '10px' }}
                onClick={handleSummarizeBtnClick}
            >
                Content Summarize
            </button>
            <div id="editableText" contentEditable="true" ref={editableRef}>
                <p>Inline AI Assist component provides intelligent text processing capabilities that enhance user productivity. It leverages advanced natural language processing to understand context and deliver precise suggestions. Users can seamlessly integrate AI-powered features into their applications.</p>
                <p>With real-time response streaming and customizable prompts, developers can create interactive experiences. The component supports multiple response modes including inline editing and popup-based interactions.</p>
            </div>
            <InlineAIAssistComponent
                id="tooltipSettings"
                ref={assistRef}
                relateTo="#summarizeBtn"
                responseMode="Inline"
                placeholder="Type or speak a prompt"
                promptRequest={handlePromptRequest}
                speechToTextSettings={speechToTextSettings}
                responseSettings={{
                    itemSelect: handleResponseItemSelect
                }}
            />
        </div>
    );
};

ReactDOM.render(<App />, document.getElementById('container'));
```

## Speech-to-Text Events

The Speech-to-Text functionality provides the `onStart`, `onStop`, `transcriptChanged`, and `onError` events. The recognized transcript is updated in the Inline AI Assist prompt area. When the prompt is submitted, both typed and speech-recognized input are available through the `prompt` property of the `promptRequest` event arguments.

| Event | Fires when... |
|-------|---------------|
| `onStart` | Speech recognition starts. |
| `onStop` | Recognition stops. |
| `transcriptChanged` | The recognized transcript is updated (interim or final). |
| `onError` | An error occurs during speech recognition. |

The example below wires all four events into a status panel that surfaces the recording state, live transcript, and any error to the user.

```tsx
import { InlineAIAssistComponent, InlinePromptRequestEventArgs, ResponseItemSelectEventArgs, SpeechToTextSettingsModel } from '@syncfusion/ej2-react-interactive-chat';
import { enableRipple } from '@syncfusion/ej2-base';
import * as React from 'react';
import * as ReactDOM from 'react-dom';

enableRipple(true);

const App: React.FC = () => {
    const assistRef = React.useRef<InlineAIAssistComponent>(null);
    const editableRef = React.useRef<HTMLDivElement>(null);
    const recordingStatusRef = React.useRef<HTMLSpanElement>(null);
    const transcriptDisplayRef = React.useRef<HTMLDivElement>(null);
    const errorMessageRef = React.useRef<HTMLDivElement>(null);
    const transcriptTimerRef = React.useRef<ReturnType<typeof setTimeout> | null>(null);
    const lastTranscriptRef = React.useRef<string>('');

    // Updates the status panel when speech recognition begins
    const onSpeechStart = (args: any): void => {
        if (recordingStatusRef.current) {
            recordingStatusRef.current.textContent = 'Recording...';
            recordingStatusRef.current.className = 'status-indicator recording';
        }
        if (errorMessageRef.current) {
            errorMessageRef.current.style.display = 'none';
        }
    };

    // Resets the status and displays the captured transcript after recognition stops
    const onSpeechStop = (args: any): void => {
        if (recordingStatusRef.current) {
            recordingStatusRef.current.textContent = 'Ready to record';
            recordingStatusRef.current.className = 'status-indicator ready';
        }
        if (transcriptDisplayRef.current && lastTranscriptRef.current) {
            transcriptDisplayRef.current.textContent = lastTranscriptRef.current;
            transcriptDisplayRef.current.style.fontStyle = 'normal';
        }
        if (transcriptTimerRef.current) {
            clearTimeout(transcriptTimerRef.current);
        }
        transcriptTimerRef.current = setTimeout(() => {
            lastTranscriptRef.current = '';
            if (transcriptDisplayRef.current) {
                transcriptDisplayRef.current.textContent = 'Waiting for speech input...';
            }
        }, 2000);
    };

    // Streams interim and final transcripts to the status panel during recognition
    const onTranscriptChanged = (args: any): void => {
        const currentTranscript: string = args.text || args.value || args.transcript || (args.result && args.result.transcript) || '';
        const isFinal: boolean = args.isFinal || args.final || (args.result && args.result.isFinal) || false;

        if (currentTranscript) {
            lastTranscriptRef.current = currentTranscript;
        }

        if (transcriptDisplayRef.current) {
            transcriptDisplayRef.current.textContent = currentTranscript;
            transcriptDisplayRef.current.style.fontStyle = isFinal ? 'normal' : 'italic';
        }
    };

    // Surfaces recognition errors in the status panel and resets the recording state
    const onSpeechError = (args: any): void => {
        if (errorMessageRef.current) {
            const errorText: HTMLElement | null = errorMessageRef.current.querySelector('.error-text');
            if (errorText) {
                errorText.textContent = 'Error: ' + (args.error || 'Speech recognition error occurred');
            }
            errorMessageRef.current.style.display = 'block';
        }
        if (recordingStatusRef.current) {
            recordingStatusRef.current.textContent = 'Ready to record';
            recordingStatusRef.current.className = 'status-indicator ready';
        }
    };

    // Wires up all speech recognition lifecycle event handlers
    const speechToTextSettings: SpeechToTextSettingsModel = {
        enable: true,
        onStart: onSpeechStart,
        onStop: onSpeechStop,
        transcriptChanged: onTranscriptChanged,
        onError: onSpeechError
    };

    // Opens the popup on Summarize button click
    const handleSummarizeBtnClick = (): void => {
        if (assistRef.current && typeof assistRef.current.showPopup === 'function') {
            assistRef.current.showPopup();
        }
    };

    // Triggered when the recognized voice prompt is submitted
    const handlePromptRequest = (args: InlinePromptRequestEventArgs): void => {
        setTimeout(() => {
            // Default placeholder response shown after the thinking delay
            const defaultResponse: string = 'For real-time prompt processing, connect the Inline AI Assist component to your preferred AI service, such as OpenAI or Azure Cognitive Services. Ensure you obtain the necessary API credentials to authenticate and enable seamless integration.';
            if (assistRef.current && typeof assistRef.current.addResponse === 'function') {
                assistRef.current.addResponse(defaultResponse, true);
            }
        }, 1000);
    };

    // Handles Accept and Discard buttons on the inline response
    const handleResponseItemSelect = (args: ResponseItemSelectEventArgs): void => {
        if (args.command.label === 'Accept') {
            if (editableRef.current) {
                editableRef.current.innerHTML = 'For real-time prompt processing, connect the Inline AI Assist component to your preferred AI service, such as OpenAI or Azure Cognitive Services. Ensure you obtain the necessary API credentials to authenticate and enable seamless integration.';
            }
            if (assistRef.current && typeof assistRef.current.hidePopup === 'function') {
                assistRef.current.hidePopup();
            }
        } else if (args.command.label === 'Discard') {
            if (assistRef.current && typeof assistRef.current.hidePopup === 'function') {
                assistRef.current.hidePopup();
            }
        }
    };

    return (
        <div>
            <div className="integration-speechtotext-section">
                <div className="speech-events-container">
                    <div className="speech-info-panel">
                        <div className="speech-status">
                            <label>Recording Status:</label>
                            <span id="recordingStatus" className="status-indicator ready" ref={recordingStatusRef}>Ready to record</span>
                        </div>
                        <div className="transcript-section">
                            <label>Live Transcript:</label>
                            <div id="transcriptDisplay" className="transcript-display" ref={transcriptDisplayRef}></div>
                        </div>
                        <div className="error-section" id="errorMessage" style={{ display: 'none' }} ref={errorMessageRef}>
                            <span className="error-text"></span>
                        </div>
                    </div>
                </div>
                <button
                    id="summarizeBtn"
                    className="e-btn e-primary"
                    style={{ marginBottom: '10px' }}
                    onClick={handleSummarizeBtnClick}
                >
                    Content Summarize
                </button>
                <div id="editableText" contentEditable="true" ref={editableRef}>
                    <p>Inline AI Assist component provides intelligent text processing capabilities that enhance user productivity. It leverages advanced natural language processing to understand context and deliver precise suggestions. Users can seamlessly integrate AI-powered features into their applications.</p>
                    <p>With real-time response streaming and customizable prompts, developers can create interactive experiences. The component supports multiple response modes including inline editing and popup-based interactions.</p>
                </div>
                <InlineAIAssistComponent
                    id="speechEvents"
                    ref={assistRef}
                    relateTo="#summarizeBtn"
                    responseMode="Inline"
                    placeholder="Type or speak a prompt"
                    promptRequest={handlePromptRequest}
                    speechToTextSettings={speechToTextSettings}
                    responseSettings={{
                        itemSelect: handleResponseItemSelect
                    }}
                />
            </div>
        </div>
    );
};

ReactDOM.render(<App />, document.getElementById('container'));
```

**Companion CSS** (status panel and responsive layout):

```css
#container {
    visibility: hidden;
    margin: 20px auto;
}
#loader {
    color: #008cff;
    height: 40px;
    left: 45%;
    position: absolute;
    top: 45%;
    width: 30%;
}

.integration-speechtotext-section {
    width: 650px;
    margin: 0 auto;
}

.speech-events-container {
    margin-bottom: 10px;
}

.speech-info-panel {
    margin-bottom: 10px;
}

.speech-status {
    margin-bottom: 4px;
}

.speech-status label,
.transcript-section label {
    font-weight: normal;
}

.status-indicator {
    display: inline;
}

.transcript-section label {
    display: block;
}

.transcript-display {
    min-height: 22px;
}

.transcript-display:empty::before {
    content: "Waiting for speech input...";
}

.error-section {
    margin-top: 4px;
}

.error-text {
    color: #c62828;
}

#editableText {
    width: 100%;
    min-height: 120px;
    max-height: 300px;
    overflow-y: auto;
    font-size: 16px;
    padding: 12px;
    border-radius: 4px;
    border: 1px solid;
}

@media only screen and (max-width: 700px) {
    .integration-speechtotext-section {
        width: 100%;
    }
}
```

## Browser Compatibility

The Speech-to-Text functionality in Inline AI Assist relies on the browser's [Speech Recognition API](https://developer.mozilla.org/en-US/docs/Web/API/SpeechRecognition), which has limited browser support. Refer to the [Browser Compatibility](https://ej2.syncfusion.com/react/documentation/speech-to-text/speech-recognition#browser-support) section for detailed information.

The browser may request permission to access the device's microphone when speech recognition starts. Speech-to-Text functionality is available only when microphone permission is granted and the browser supports the Speech Recognition API.
