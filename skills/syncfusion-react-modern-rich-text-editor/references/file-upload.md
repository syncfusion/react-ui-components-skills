# File Upload

File upload functionality in the Rich Text Editor allows users to upload images and files directly to the editor. Configure file upload behavior, validate files, and handle upload events.

## Table of Contents

- [Overview](#overview)
- [Basic Setup](#basic-setup)
- [API Reference](#api-reference)

## Overview

The Rich Text Editor provides comprehensive file upload support through:
- Image upload with validation
- Event handlers for upload lifecycle
- File removal and cleanup
- Error handling and failure scenarios

## Basic Setup

```jsx
import { RichTextEditorComponent } from @syncfusion/ej2-react-richtexteditor;

function App() {
  return (
    <RichTextEditorComponent>
      <Inject services={[Image, Link, HtmlEditor, Toolbar]} />
    </RichTextEditorComponent>
  );
}
```

## API Reference

## FileUploadingEventArgs

**Event Arguments** — Event args for `RichTextEditor.fileUploading`. Extends the ej2 `UploadingEventArgs` with the `source` field that identifies the editor feature the upload is for.

### Properties

#### `cancel` (boolean)
Defines whether the current action can be prevented.

#### `chunkSize` (number)
Returns the chunk size in bytes if the chunk upload is enabled.

#### `currentChunkIndex` (number)
Returns the index of current chunk if the chunk upload is enabled.

#### `currentRequest` (XMLHttpRequest)
Returns the XMLHttpRequest instance that is associated with upload action.

#### `customFormData` ({ [key: string]: Object }[])
Defines the additional data in key and value pair format that will be submitted to the upload action.

#### `fileData` (FileInfo)
Returns the list of files that will be uploaded.

#### `source` (string)
The editor feature the sub-component belongs to. Supplied by the caller when the sub-component is created. Undefined for generic sub-components that are not tied to a specific feature.

---
## FileUploadSuccessEventArgs

**Event Arguments** — Event args for `RichTextEditor.fileUploadSuccess`. Extends the ej2 `SuccessEventArgs` with the `source` field that identifies the editor feature the upload is for.

### Properties

#### `chunkIndex` (number)
Returns the upload chunk index.

#### `chunkSize` (number)
Returns the upload chunk size.

#### `e` (object)
Returns the original event arguments.

#### `event` (object)
Returns the original event arguments.

#### `file` (FileInfo)
Returns the details about upload file.

#### `operation` (string)
Returns the upload event operation.

#### `response` (ResponseEventArgs)
Returns the upload event operation.

#### `source` (string)
The editor feature the sub-component belongs to. Supplied by the caller when the sub-component is created. Undefined for generic sub-components that are not tied to a specific feature.

#### `statusText` (string)
Returns the upload status.

#### `totalChunk` (number)
Returns the total chunk size.

---
## FileUploadFailedEventArgs

**Event Arguments** — Event args for `RichTextEditor.fileUploadFailed`. Extends the ej2 `FailureEventArgs` with the `source` field that identifies the editor feature the upload is for.

### Properties

#### `chunkIndex` (number)
Returns the upload chunk index.

#### `chunkSize` (number)
Returns the upload chunk size.

#### `e` (object)
Returns the original event arguments.

#### `event` (object)
Returns the original event arguments.

#### `file` (FileInfo)
Returns the details about upload file.

#### `operation` (string)
Returns the upload event operation.

#### `response` (ResponseEventArgs)
Returns the upload event operation.

#### `source` (string)
The editor feature the sub-component belongs to. Supplied by the caller when the sub-component is created. Undefined for generic sub-components that are not tied to a specific feature.

#### `statusText` (string)
Returns the upload status.

#### `totalChunk` (number)
Returns the total chunk size.

---
## FileSelectedEventArgs

**Event Arguments** — Event args for `RichTextEditor.fileSelected`. Extends the ej2 `SelectedEventArgs` with the `source` field that identifies the editor feature the upload is for.

### Properties

#### `cancel` (boolean)
Defines whether the current action can be prevented.

#### `currentRequest` ({ [key: string]: string }[])
Set the current request header to the XMLHttpRequest instance.

#### `customFormData` ({ [key: string]: Object }[])
Defines the additional data in key and value pair format that will be submitted to the upload action.

#### `event` (MouseEvent|TouchEvent|DragEvent|ClipboardEvent)
Returns the original event arguments.

#### `filesData` (FileInfo[])
Returns the list of selected files.

#### `isCanceled` (boolean)
Specifies whether the file selection has been canceled

#### `isModified` (boolean)
Determines whether the file list generates based on the modified data.

#### `modifiedFilesData` (FileInfo[])
Specifies the modified files data to generate the file items. The argument depends on `isModified` argument.

#### `progressInterval` (string)
Specifies the step value to the progress bar.

#### `source` (string)
The editor feature the sub-component belongs to. Supplied by the caller when the sub-component is created. Undefined for generic sub-components that are not tied to a specific feature.

---
## FileRemovingEventArgs

**Event Arguments** — Event args for `RichTextEditor.fileRemoving`. Raised before a previously-uploaded image is removed (client-side `URL.revokeObjectURL` for `saveFormat: 'Blob'`, or before the `removeUrl` POST is sent for server-side removal). Set `args.cancel = true` to keep the image in place; both the `Blob` revocation and the `removeUrl` POST will be skipped.

### Properties

#### `cancel` (boolean)
Cancels the removal. Both local `URL.revokeObjectURL` and the `removeUrl` POST will be skipped.

#### `source` (string)
The editor feature the sub-component belongs to. Supplied by the caller when the sub-component is created. Undefined for generic sub-components that are not tied to a specific feature.

#### `src` (string)
Source URL of the image being removed.

---
## BeforeFileUploadEventArgs

**Event Arguments** — Event args for `beforeFileUpload`. Extends the ej2 `BeforeUploadEventArgs` with the `source` field.

### Properties

#### `cancel` (boolean)
Defines whether the current action can be prevented.

#### `currentRequest` ({ [key: string]: string }[])
Returns the XMLHttpRequest instance headers.

#### `customFormData` ({ [key: string]: Object }[])
Additional data in key/value pairs to submit to the upload action.

#### `source` (string)
The editor feature the upload is for (e.g., `'Image'`).

---
## BeforeFileDropEventArgs

**Event Arguments** — Event args for `beforeFileDrop`. Raised when files are dropped (or pasted) onto the editable area, before the image module routes them through the typed command pipeline.

### Properties

#### `cancel` (boolean)
Cancels the drop pipeline.

#### `files` (FileList)
Files dropped onto or pasted into the editor.

#### `source` (string)
The editor feature the sub-component belongs to.

---
