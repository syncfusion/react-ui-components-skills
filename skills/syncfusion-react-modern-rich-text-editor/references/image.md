# Images in React Modern Rich Text Editor

## Table of Contents
- [Insert Images](#insert-images)
- [Local Storage](#local-storage)
- [Web URLs](#web-urls)
- [Allowed Image Formats](#allowed-image-formats)
- [Image Size Restrictions](#image-size-restrictions)
- [Image Save Formats](#image-save-formats)
- [Blob Format (Recommended - Default)](#blob-format-recommended---default)
- [Base64 Format](#base64-format)
- [Limitations of Base64 and Blob](#limitations-of-base64-and-blob)
- [Base64 Limitations](#base64-limitations)
- [Blob Limitations](#blob-limitations)
- [Dimension](#dimension)
- [Image Display Options](#image-display-options)
- [Image Resizing](#image-resizing)
- [Image Quick Toolbar](#image-quick-toolbar)
- [Available Quick Toolbar Items](#available-quick-toolbar-items)
- [Caption](#caption)
- [Alt Text](#alt-text)
- [Replace](#replace)
- [Text Wrap](#text-wrap)
- [Alignment](#alignment)
- [Remove](#remove)
- [Customizing Quick Toolbar Items](#customizing-quick-toolbar-items)
- [Remote Image Upload](#remote-image-upload)
- [Writing an Endpoint for Image Upload](#writing-an-endpoint-for-image-upload)
- [Rename Images Before Inserting](#rename-images-before-inserting)
- [Secure Image Upload with Authentication](#secure-image-upload-with-authentication)

Image insertion in the Modern Rich Text Editor supports multiple methods to accommodate different workflows and use cases. The editor provides flexible options for inserting images from local storage and web URLs, along with detailed configuration for storage, display, sizing, and a contextual image quick toolbar.

- [API Reference](#api-reference)
## Insert Images

### Local Storage

To upload images from your local machine, click the `Image` tool in the toolbar. By default, this tool opens a dialog box where you can browse and select an image to insert from your local machine.

```tsx
<RichTextEditorUIComponent
  imageSettings={{
    uploadUrl: 'https://api.example.com/upload',
    imageUrl: '/uploads/'
  }}
  toolbarSettings={{
    items: ['Image']
  }}
/>
```

### Web URLs

To insert an image from an online source, click the `Image` tool in the toolbar. By default, this tool opens a dialog box with an input field where you can provide the image URL from the web to insert the image.

## Allowed Image Formats

The `allowedTypes` property specifies the image file extensions that can be selected, dropped, pasted, or uploaded.

**Default Supported Formats:**
- JPEG (`.jpg`, `.jpeg`)
- PNG (`.png`)
- GIF (`.gif`)
- BMP (`.bmp`)
- WebP (`.webp`)

```tsx
<RichTextEditorUIComponent
  imageSettings={{
    allowedTypes: ['.jpg', '.jpeg', '.png', '.gif', '.bmp', '.svg', '.webp']
  }}
/>
```

## Image Size Restrictions

The `maxFileSize` property specifies the maximum permitted image file size in bytes.

```tsx
<RichTextEditorUIComponent
  imageSettings={{
    // File size restriction (in bytes)
    maxFileSize: 5242880           // 5MB in bytes
  }}
/>
```

## Image Save Formats

The RichTextEditor supports two primary formats for saving images: Blob (server-based) and Base64 (embedded). Each format offers distinct advantages and trade-offs based on your use case.

### Blob Format (Recommended - Default)

```tsx
<RichTextEditorUIComponent
  imageSettings={{
    saveFormat: 'Blob'
  }}
/>
```

### Base64 Format

```tsx
<RichTextEditorUIComponent
  imageSettings={{
    saveFormat: 'Base64'
  }}
/>
```

## Limitations of Base64 and Blob

Understanding the limitations of each format helps you choose the right approach for your specific requirements.

### Base64 Limitations

| Issue | Impact | Solution |
|-------|--------|----------|
| File size increase | 33% larger than original | Compress images first |
| Email compatibility | Large Base64 breaks email clients | Use URLs for emails |
| Performance | Slower parsing with many images | Limit to < 5 images per document |
| Mobile concerns | Large payload on mobile devices | Use Blob format for mobile apps |
| Debugging | Hard to inspect in dev tools | Use Blob for development |

### Blob Limitations

| Issue | Impact | Solution |
|-------|--------|----------|
| Server storage required | Additional infrastructure | Plan storage capacity |
| Cross-domain issues | CORS restrictions | Configure CORS headers |
| Temporary files | Need cleanup mechanism | Implement file lifecycle management |
| Session dependency | Files tied to session | Persist files properly |

## Dimension

Configure custom dimensions or preset sizes for images with min/max constraints. The `dimension` property allows you to set default and constraint values for image sizing. You can specify width and height as CSS strings (e.g., `'300px'`, `'auto'`) or numeric values (interpreted as pixels).

```tsx
import * as React from 'react';
import { RichTextEditorUIComponent } from '@syncfusion/ej2-react-richtexteditor-ui';

const toolbarSettings = {
    items: [
        'Undo', 'Redo', '|',
        'Bold', 'Italic', 'Underline', '|',
        'Image', 'Link', 'Table'
    ]
};

const imageSettings = {
    saveUrl: 'https://services.syncfusion.com/react/uploader/Save',
    path: '/Images/',
    dimension: {
        width: '300px',
        height: 'auto',
        minWidth: '50px',
        maxWidth: '1000px',
        minHeight: '50px',
        maxHeight: '800px'
    }
};

const quickToolbarSettings = {
    image: [
        'AltText', 'Caption', '|',
        'Align', 'Display', 'WrapText', '|',
        'Dimension', 'Replace', 'Remove'
    ]
};

function App() {
    return (
        <RichTextEditorUIComponent
            toolbarSettings={toolbarSettings}
            imageSettings={imageSettings}
            quickToolbarSettings={quickToolbarSettings}
            placeholder='Type something...'
        />
    );
}

export default App;
```

## Image Display Options

Control how images are rendered and positioned within the editor content through display modes and wrapping options.

### Display

Configure how images are rendered in the document - either flowing with text or on a separate line.

**Display Modes:**
- `inline` (default) - Image flows within the current text line
- `break` (block) - Image placed on a separate line

```tsx
import * as React from 'react';
import { RichTextEditorUIComponent } from '@syncfusion/ej2-react-richtexteditor-ui';

const toolbarSettings = {
    items: [
        'Undo', 'Redo', '|',
        'Bold', 'Italic', 'Underline', '|',
        'Image', 'Link', 'Table'
    ]
};

const imageSettings = {
    saveUrl: 'https://services.syncfusion.com/react/uploader/Save',
    path: '/Images/'
};

const quickToolbarSettings = {
    image: [
        'AltText', 'Caption', '|',
        'Align', 'Display', 'WrapText', '|',
        'Dimension', 'Replace', 'Remove'
    ]
};

function App() {
    return (
        <RichTextEditorUIComponent
            toolbarSettings={toolbarSettings}
            imageSettings={imageSettings}
            quickToolbarSettings={quickToolbarSettings}
            placeholder='Type something...'
        />
    );
}

export default App;
```

## Image Resizing

Enable and configure image resizing with constraints and event tracking.

### Enable Image Resize

The `resize` property controls whether images can be resized by users. By default, image resizing is enabled. Use the `dimension` property to set minimum and maximum constraints for resizable images.

```tsx
import * as React from 'react';
import { RichTextEditorUIComponent } from '@syncfusion/ej2-react-richtexteditor-ui';

const toolbarSettings = {
    items: [
        'Undo', 'Redo', '|',
        'Bold', 'Italic', 'Underline', '|',
        'Image', 'Link', 'Table'
    ]
};

const imageSettings = {
    saveUrl: 'https://services.syncfusion.com/react/uploader/Save',
    path: '/Images/',
    resize: true,
    dimension: {
        minWidth: '50px',
        maxWidth: '800px',
        minHeight: '50px',
        maxHeight: '600px'
    }
};

const quickToolbarSettings = {
    image: [
        'AltText', 'Caption', '|',
        'Align', 'Display', 'WrapText', '|',
        'Dimension', 'Replace', 'Remove'
    ]
};

function App() {
    return (
        <RichTextEditorUIComponent
            value='<p>The image in this example can be resized using the quick toolbar.</p>'
            valueFormat='html'
            placeholder='Type something...'
            toolbarSettings={toolbarSettings}
            imageSettings={imageSettings}
            quickToolbarSettings={quickToolbarSettings}
        />
    );
}

export default App;
```

## Image Quick Toolbar

The Image Quick Toolbar provides instant access to common image operations when an image is selected in the editor. It offers a streamlined interface for managing image properties, alignment, sizing, and other aspects without requiring modal dialogs.

### Available Quick Toolbar Items

The Image Quick Toolbar appears automatically when an image is selected. It provides quick access to common image operations through dedicated toolbar buttons.

### Caption

Captions are rendered as `<figcaption>` elements inside a `<figure>` container.

**HTML Output:**
```html
<figure>
    <img src="image.png" alt="Sample" class="e-img-inline" />
    <figcaption>Image Caption Text</figcaption>
</figure>
```

```tsx
// Enable Caption in quick toolbar
<RichTextEditorUIComponent
  quickToolbarSettings={{
    image: ['Caption']
  }}
/>
```

### Alt Text

Use the Alt Text quick toolbar item to add descriptive alternative text for the image.

```tsx
// Enable Alt Text in quick toolbar (default)
<RichTextEditorUIComponent
  quickToolbarSettings={{
    image: ['AltText']
  }}
/>
```

### Replace

Use the Replace quick toolbar item to replace the currently selected image with a new image source.

```tsx
// Enable Replace in quick toolbar
<RichTextEditorUIComponent
  quickToolbarSettings={{
    image: ['Replace']
  }}
/>
```

### Text Wrap

Control how text flows around the selected image using the Text Wrap quick toolbar item. Configure text wrapping to position images inline with text or allow text to wrap around them.

```tsx
// Enable WrapText in quick toolbar
<RichTextEditorUIComponent
  quickToolbarSettings={{
    image: ['WrapText']
  }}
/>
```

**Text Wrap Modes:**
- `'left'` - Wraps text to the right of the image
- `'right'` - Wraps text to the left of the image
- `'none'` - No text wrapping (default)

### Alignment

Set horizontal alignment of the selected image using the Alignment quick toolbar item. You can align images to the left, center, or right within the editor.

```tsx
// Enable Align in quick toolbar
<RichTextEditorUIComponent
  quickToolbarSettings={{
    image: ['Align']
  }}
/>
```

**Available Alignment Options:**
- `'left'` - Align image to the left
- `'center'` - Center the image
- `'right'` - Align image to the right
- `'none'` - Remove alignment (default)

### Remove

Delete the selected image from the editor using the Remove quick toolbar item. This provides a quick way to remove images without using the delete key.

```tsx
// Enable Remove in quick toolbar (recommended)
<RichTextEditorUIComponent
  quickToolbarSettings={{
    image: ['Remove']
  }}
/>
```

### Customizing Quick Toolbar Items

The Modern Rich Text Editor provides comprehensive customization options for the image quick toolbar, offering a rich set of tools including 'AltText', 'Caption', 'Align', 'Display', 'WrapText', 'Dimension', 'Replace', and 'Remove'. By configuring these toolbar items through the `quickToolbarSettings` property, you can create a tailored editing experience that streamlines image operations.

**Default Quick Toolbar Items for Images:**
The default image quick toolbar includes:
- `AltText` - Edit alternative text
- `Caption` - Add/edit image caption
- `Align` - Align image (left, center, right, none)
- `Display` - Change display mode (inline, block)
- `WrapText` - Set text wrapping (left, right, none)
- `Dimension` - Adjust width and height
- `Replace` - Replace with a different image
- `Remove` - Delete the image

```tsx
import * as React from 'react';
import { RichTextEditorUIComponent } from '@syncfusion/ej2-react-richtexteditor-ui';

const toolbarSettings = {
    items: [
        'Undo', 'Redo', '|',
        'Bold', 'Italic', 'Underline', '|',
        'Image', 'Link', 'Table'
    ]
};

const imageSettings = {
    saveUrl: 'https://services.syncfusion.com/react/uploader/Save',
    path: '/Images/'
};

const quickToolbarSettings = {
    image: [
        'AltText', 'Caption', '|',
        'Align', 'Display', 'WrapText', '|',
        'Dimension', 'Replace', 'Remove'
    ]
};

function App() {
    return (
        <RichTextEditorUIComponent
            toolbarSettings={toolbarSettings}
            imageSettings={imageSettings}
            quickToolbarSettings={quickToolbarSettings}
            placeholder='Type something...'
        />
    );
}

export default App;
```

See [toolbar.md](toolbar.md#image-quick-toolbar-items) for the `ImageQuickToolbarItem` union and custom quick-toolbar items.

---

## Remote Image Upload

Remote image upload enables centralized image management on your server, providing better control over storage, performance, and security. This section covers implementing a complete server-side upload pipeline with authentication and validation.

### Writing an Endpoint for Image Upload

When a user uploads an image through the RichTextEditor, the component sends the file to your server using the form field name `UploadFiles`. Your server processes the file and returns a JSON response containing the filename, which the editor combines with the `imageUrl` setting to create the final image source.

#### Client-Side Configuration

Configure the Modern Rich Text Editor component with the upload endpoint and base URL:

```tsx
// ============================================
// CLIENT-SIDE: Configure the RichTextEditor
// ============================================
<RichTextEditorUIComponent
  imageSettings={{
    uploadUrl: 'https://api.example.com/images/upload',  // POST endpoint for uploads
    imageUrl: '/uploads/',                               // Base URL to resolve uploaded filenames
    removeUrl: 'https://api.example.com/images/remove',  // DELETE endpoint for removal
    allowedTypes: ['.jpg', '.jpeg', '.png', '.gif'],     // Allowed file types
    maxFileSize: 5 * 1024 * 1024                         // 5MB limit
  }}
/>
```

#### Server-Side Configuration

```cs
// ============================================
// SERVER-SIDE: ASP.NET Core Host Configuration (program.cs)
// ============================================
// Wire up the services and middleware required for the
// Modern Rich Text Editor image upload endpoints to work.
using Microsoft.Extensions.FileProviders;

var builder = WebApplication.CreateBuilder(args);

// --- Services ---------------------------------------------------------

// Add framework services (controllers, model binding, etc.).
builder.Services.AddControllers();

// OpenAPI helper used in development to keep the API discoverable.
builder.Services.AddOpenApi();

// Optional: allow directory browsing for diagnostic purposes.
builder.Services.AddDirectoryBrowser();

// CORS: allow the editor's client during development.
// Restrict origins in production.
builder.Services.AddCors(options =>
{
    options.AddPolicy("AllowAllOrigins", policy =>
    {
        policy.AllowAnyOrigin()
              .AllowAnyHeader()
              .AllowAnyMethod();
    });
});

// Increase multipart body length limit (10 MB) for image uploads.
builder.Services.Configure<Microsoft.AspNetCore.Http.Features.FormOptions>(options =>
{
    options.MultipartBodyLengthLimit = 10 * 1024 * 1024; // 10 MB
});

var app = builder.Build();

// --- Middleware pipeline ---------------------------------------------

// Development helpers.
if (app.Environment.IsDevelopment())
{
    app.MapOpenApi();
}

// Force HTTPS for every request.
app.UseHttpsRedirection();

// 1. Serve default wwwroot static files.
app.UseStaticFiles();

// 2. Set up a File Provider for the custom RichTextEditor folder
//    that the controller writes uploaded images to.
var richTextFolderProvider = new PhysicalFileProvider(
    Path.Combine(app.Environment.ContentRootPath, "wwwroot", "RichTextEditor")
);

// Actually serve the files from the RichTextEditor folder.
app.UseStaticFiles(new StaticFileOptions
{
    FileProvider = richTextFolderProvider,
    RequestPath  = "/public"
});

// Browse the directory index (HTML list) of the folder.
app.UseDirectoryBrowser(new DirectoryBrowserOptions
{
    FileProvider = richTextFolderProvider,
    RequestPath  = "/public"
});

// Routing must come before CORS/Authorization for endpoint routing to work correctly.
app.UseRouting();

// Enable the CORS policy defined above.
app.UseCors("AllowAllOrigins");

app.UseAuthorization();

app.MapControllers();

app.Run();
```

### Rename Images Before Inserting

You can implement server-side renaming to ensure all uploaded images follow your naming standards. The client receives the renamed filename and automatically inserts it using the `imageUrl` configuration.

#### Server-Side Configuration

```cs
// ============================================
// SERVER-SIDE: Rename Images During Upload (ASP.NET Core)
// ============================================
// Generates a deterministic, user-scoped filename on the server so the
// client never has to invent a name. The new filename is returned in the
// response and the editor resolves it through `imageSettings.imageUrl`.
[HttpPost("SaveFile")]
[Authorize]  // Authenticate user via the configured auth scheme.
public IActionResult SaveFile([FromForm] IList<IFormFile> UploadFiles)
{
    try
    {
        // Read the user id from the authentication claims.
        var userId = User.FindFirst(ClaimTypes.NameIdentifier)?.Value;

        if (UploadFiles == null || UploadFiles.Count == 0)
        {
            return BadRequest(new { error = "No file provided" });
        }

        var file = UploadFiles[0];

        // Preserve the original file extension.
        var originalName  = file.FileName;
        var fileExtension = Path.GetExtension(originalName);

        // Build a standardized, collision-resistant filename with user context.
        var timestamp   = DateTime.UtcNow.ToString("yyyyMMddHHmmssfff");
        var newFilename = $"img-user{userId}-{timestamp}{fileExtension}";

        // Resolve the uploads directory under wwwroot (fall back to
        // a synthesized wwwroot if WebRootPath is not configured).
        var webRoot     = _webHostEnvironment.WebRootPath
                          ?? Path.Combine(Directory.GetCurrentDirectory(), "wwwroot");
        var uploadsPath = Path.Combine(webRoot, "uploads");
        Directory.CreateDirectory(uploadsPath);

        var savePath = Path.Combine(uploadsPath, newFilename);

        using (var fs = System.IO.File.Create(savePath))
        {
            file.CopyTo(fs);
            fs.Flush();
        }

        // Return the renamed filename so the editor can resolve it
        // against the configured `imageUrl` base path.
        return Ok(new { name = newFilename });
    }
    catch (Exception)
    {
        return StatusCode(500, new { error = "Upload failed" });
    }
}
```

#### Client-Side Configuration

```tsx
// ============================================
// CLIENT-SIDE: Track Upload Success
// ============================================
<RichTextEditorUIComponent
  imageSettings={{
    uploadUrl: 'https://api.example.com/images/upload',
    imageUrl: '/uploads/'
  }}
  onFileUploadSuccess={(args: any) => {
    if (args.source === 'Image') {
      console.log('Image uploaded successfully');
      console.log('Renamed filename:', args.response);
    }
  }}
/>
```

### Secure Image Upload with Authentication

You can add additional data with the image uploaded from the Modern Rich Text Editor on the client side, which can even be received on the server side. By using the `fileUploading` event and its arguments you can access the current request and set the request header within this event. On the server side, you can fetch the custom headers by accessing the form collection from the current request, which retrieves the values sent using the POST method.

#### Client-Side Configuration

```tsx
// CLIENT-SIDE: Add authentication token before upload
<RichTextEditorUIComponent
  imageSettings={{
    uploadUrl: 'https://api.example.com/images/upload',
    imageUrl: '/uploads/'
  }}
  onFileUploading={(args: any) => {
    args.currentRequest.setRequestHeader('Authorization', 'Syncfusion');
  }}
/>
```

#### Server-Side Configuration

```cs
// ============================================
// SERVER-SIDE: Secure Image Upload with Authentication (ASP.NET Core)
// ============================================
// Same upload endpoint as the basic version, but with a custom
// `Authorization` header check. The client sets this header inside
// the editor's `fileUploading` event using
// `args.currentRequest.setRequestHeader('Authorization', 'Syncfusion')`.
[HttpPost("SaveFile")]
[EnableCors("AllowAllOrigins")]
public IActionResult SaveFile(IList<IFormFile> UploadFiles)
{
    try
    {
        // Fetch the custom authentication header that the client attached
        // before the request was sent.
        string? authorizationHeader = Request.Headers["Authorization"].FirstOrDefault();

        if (string.IsNullOrEmpty(authorizationHeader))
        {
            return StatusCode(401, new { error = "Authorization header missing" });
        }

        // Optional: validate the token here (e.g. JWT, API key, session id).
        // if (!IsValidToken(authorizationHeader)) { return Unauthorized(); }

        if (UploadFiles == null || UploadFiles.Count == 0)
        {
            return BadRequest("No files provided.");
        }

        if (UploadFiles.Count > 1)
        {
            return BadRequest("Too many files. Maximum 1 file allowed per request.");
        }

        foreach (IFormFile uploadFile in UploadFiles)
        {
            var fileNameSegment = ContentDispositionHeaderValue
                .Parse(uploadFile.ContentDisposition).FileName;
            string? fileName = fileNameSegment.HasValue
                ? fileNameSegment.Value.Trim('"')
                : null;

            // DOS PREVENTION - Filename length limit.
            if (fileName?.Length > 255)
            {
                return BadRequest("Filename too long. Maximum 255 characters allowed.");
            }

            // PATH TRAVERSAL PREVENTION - Block dangerous characters.
            if (fileName != null && (fileName.Contains("..") ||
                                     fileName.Contains("/")  ||
                                     fileName.Contains("\\")))
            {
                return BadRequest("Invalid filename - path traversal detected.");
            }

            string filePath = Path.Combine(
                _webHostEnvironment.WebRootPath,
                "RichTextEditor",
                fileName!);

            if (System.IO.File.Exists(filePath))
            {
                System.IO.File.Delete(filePath);
            }

            using (FileStream fs = System.IO.File.Create(filePath))
            {
                uploadFile.CopyTo(fs);
                fs.Flush();
            }
        }

        return Ok("Files saved successfully.");
    }
    catch (Exception ex)
    {
        return StatusCode(500, $"An error occurred: {ex.Message}");
    }
}
```

> **Note:** the server snippets above are labelled `cs` because they are C# controller and host code, not TypeScript.

## API Reference


## ImageSettings

**Configuration Class** — Configures the image settings of the RichTextEditor.

### Properties

#### `allowedTypes` (string[])
Specifies the image file extensions that can be selected, dropped, pasted, or uploaded. API names use TypeScript camel casing.  
**Default:** `['.jpg', '.jpeg', '.png', '.gif', '.bmp', '.webp']`

#### `dimension` (ImageDimensionOptions)
Specifies the default and permitted dimensions of an image.  
**Default:** `{ width: 'auto', height: 'auto', minWidth: 0, maxWidth: null, minHeight: 0, maxHeight: null }`

#### `display` (ImageDisplayMode)
Specifies how an image is positioned in the editor content. Possible values are:
- `inline`: Places the image within the current text flow.
- `break`: Places the image on a separate line.  
**Default:** `'inline'`

#### `imageUrl` (string | null)
Specifies the public base URL used to resolve uploaded image names returned by the server. For example, when `imageUrl` is `/uploads/images/` and the server returns `sample.png`, the resulting source is `/uploads/images/sample.png`.  
**Default:** `null`

#### `maxFileSize` (number)
Specifies the maximum permitted image file size, in bytes.  
**Default:** `30000000`

#### `removeUrl` (string | null)
Specifies the server endpoint used to remove an uploaded image.  
**Default:** `null`

#### `resize` (boolean)
Specifies whether users can resize images in the editor.  
**Default:** `true`

#### `saveFormat` (ImageSaveFormat)
Specifies how local images are represented when an upload endpoint is not configured. Possible values are:
- `Blob`: Creates a temporary object URL for the image.
- `Base64`: Embeds the image as a Base64 data URL.  
**Default:** `'Blob'`

#### `uploadUrl` (string | null)
Specifies the server endpoint used to upload image files. When this property is not configured, the editor uses `saveFormat` to create a local image source.  
**Default:** `null`

---

## ImageSettingsModel

**Interface** — Model for image settings configuration

### Properties

#### `allowedTypes` (string[])
Specifies the image file extensions that can be selected, dropped, pasted, or uploaded. API names use TypeScript camel casing.

#### `dimension` (ImageDimensionOptions)
Specifies the default and permitted dimensions of an image.

#### `display` (ImageDisplayMode)
Specifies how an image is positioned in the editor content. Possible values are:
- `inline`: Places the image within the current text flow.
- `break`: Places the image on a separate line.

#### `imageUrl` (string | null)
Specifies the public base URL used to resolve uploaded image names returned by the server. For example, when `imageUrl` is `/uploads/images/` and the server returns `sample.png`, the resulting source is `/uploads/images/sample.png`.

#### `maxFileSize` (number)
Specifies the maximum permitted image file size, in bytes.

#### `removeUrl` (string | null)
Specifies the server endpoint used to remove an uploaded image.

#### `resize` (boolean)
Specifies whether users can resize images in the editor.

#### `saveFormat` (ImageSaveFormat)
Specifies how local images are represented when an upload endpoint is not configured. Possible values are:
- `Blob`: Creates a temporary object URL for the image.
- `Base64`: Embeds the image as a Base64 data URL.

#### `uploadUrl` (string | null)
Specifies the server endpoint used to upload image files. When this property is not configured, the editor uses `saveFormat` to create a local image source.

---

## ImageSaveFormat

**Enumeration** — Local representation chosen for the next image insertion when the editor uses a local proxy source (no `uploadUrl`).

**Values:**
- `'Blob'` — Creates a temporary object URL for the image.
- `'Base64'` — Embeds the image as a Base64 data URL.

---

## ImageDisplayMode

**Enumeration** — Default display mode for inserted images.

**Values:**
- `'inline'` — Places the image within the current text flow.
- `'break'` — Places the image on a separate line.

---

## ImageQuickToolbarItem

**Union Type** — Built-in items exposed by the Image Quick Toolbar.

**Built-in values:**
- `'AltText'`
- `'Caption'`
- `'Align'`
- `'Display'`
- `'WrapText'`
- `'Dimension'`
- `'Replace'`
- `'Remove'`
- Separator (`''`)
- `CustomToolbarItem`

---

## API Reference

## ImageSettings

**Configuration Class** — Configures the image settings of the RichTextEditor.

### Properties

#### `allowedTypes` (string[])
Specifies the image file extensions that can be selected, dropped, pasted, or uploaded. API names use TypeScript camel casing.  
**Default:** `['.jpg', '.jpeg', '.png', '.gif', '.bmp', '.webp']`

#### `dimension` (ImageDimensionOptions)
Specifies the default and permitted dimensions of an image.  
**Default:** `{ width: 'auto', height: 'auto', minWidth: 0, maxWidth: null, minHeight: 0, maxHeight: null }`

#### `display` (ImageDisplayMode)
Specifies how an image is positioned in the editor content. Possible values are:
- `inline`: Places the image within the current text flow.
- `break`: Places the image on a separate line.  
**Default:** `'inline'`

#### `imageUrl` (string | null)
Specifies the public base URL used to resolve uploaded image names returned by the server. For example, when `imageUrl` is `/uploads/images/` and the server returns `sample.png`, the resulting source is `/uploads/images/sample.png`.  
**Default:** `null`

#### `maxFileSize` (number)
Specifies the maximum permitted image file size, in bytes.  
**Default:** `30000000`

#### `removeUrl` (string | null)
Specifies the server endpoint used to remove an uploaded image.  
**Default:** `null`

#### `resize` (boolean)
Specifies whether users can resize images in the editor.  
**Default:** `true`

#### `saveFormat` (ImageSaveFormat)
Specifies how local images are represented when an upload endpoint is not configured. Possible values are:
- `Blob`: Creates a temporary object URL for the image.
- `Base64`: Embeds the image as a Base64 data URL.  
**Default:** `'Blob'`

#### `uploadUrl` (string | null)
Specifies the server endpoint used to upload image files. When this property is not configured, the editor uses `saveFormat` to create a local image source.  
**Default:** `null`

---
## ImageSettingsModel

**Interface** — Model for image settings configuration

### Properties

#### `allowedTypes` (string[])
Specifies the image file extensions that can be selected, dropped, pasted, or uploaded. API names use TypeScript camel casing.

#### `dimension` (ImageDimensionOptions)
Specifies the default and permitted dimensions of an image.

#### `display` (ImageDisplayMode)
Specifies how an image is positioned in the editor content. Possible values are:
- `inline`: Places the image within the current text flow.
- `break`: Places the image on a separate line.

#### `imageUrl` (string | null)
Specifies the public base URL used to resolve uploaded image names returned by the server. For example, when `imageUrl` is `/uploads/images/` and the server returns `sample.png`, the resulting source is `/uploads/images/sample.png`.

#### `maxFileSize` (number)
Specifies the maximum permitted image file size, in bytes.

#### `removeUrl` (string | null)
Specifies the server endpoint used to remove an uploaded image.

#### `resize` (boolean)
Specifies whether users can resize images in the editor.

#### `saveFormat` (ImageSaveFormat)
Specifies how local images are represented when an upload endpoint is not configured. Possible values are:
- `Blob`: Creates a temporary object URL for the image.
- `Base64`: Embeds the image as a Base64 data URL.

#### `uploadUrl` (string | null)
Specifies the server endpoint used to upload image files. When this property is not configured, the editor uses `saveFormat` to create a local image source.

---
## ImageSaveFormat

**Enumeration** — Local representation chosen for the next image insertion when the editor uses a local proxy source (no `uploadUrl`).

**Values:**
- `'Blob'` — Creates a temporary object URL for the image.
- `'Base64'` — Embeds the image as a Base64 data URL.

---
## ImageDisplayMode

**Enumeration** — Default display mode for inserted images.

**Values:**
- `'inline'` — Places the image within the current text flow.
- `'break'` — Places the image on a separate line.

---
## ImageQuickToolbarItem

**Union Type** — Built-in items exposed by the Image Quick Toolbar.

**Built-in values:**
- `'AltText'`
- `'Caption'`
- `'Align'`
- `'Display'`
- `'WrapText'`
- `'Dimension'`
- `'Replace'`
- `'Remove'`
- Separator (`''`)
- `CustomToolbarItem`

---
