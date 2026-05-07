# Images, Titles & Dividers

## Card Images

### Full-Width Card Images

The `e-card-image` class creates a full-width image section that spans the entire width of the card:

```jsx
<div className="e-card">
  <div className="e-card-image" 
       style={{ backgroundImage: 'url(./image.jpg)', height: '200px' }} />
  <div className="e-card-content">
    Content below the image
  </div>
</div>
```

**Key points:**
- By default, card images occupy 100% of their parent element's width
- Set explicit height to control image display area
- Use CSS `backgroundImage` property with background sizing

### Image Styling

```jsx
<div className="e-card-image"
     style={{
       backgroundImage: 'url(./photo.jpg)',
       backgroundSize: 'cover',
       backgroundPosition: 'center',
       height: '250px'
     }} />
```

### Image with Content Overlay

Position content over the image using absolute positioning:

```jsx
<div style={{ position: 'relative' }}>
  <div className="e-card-image"
       style={{
         backgroundImage: 'url(./background.jpg)',
         height: '300px'
       }} />
  <div style={{
    position: 'absolute',
    bottom: '0',
    left: '0',
    right: '0',
    padding: '20px',
    background: 'rgba(0,0,0,0.5)',
    color: 'white'
  }}>
    Overlay content here
  </div>
</div>
```

## Image Titles

### Basic Image Title

The `e-card-title` class displays a title overlay on top of card images. By default, titles appear in the bottom-left corner with an overlay effect:

```jsx
<div className="e-card">
  <div className="e-card-image" 
       style={{ backgroundImage: 'url(./image.jpg)', height: '200px' }}>
    <div className="e-card-title">Image Title</div>
  </div>
  <div className="e-card-content">
    Card content
  </div>
</div>
```

### Title Positioning

By default, titles appear in the bottom-left position. Use positioning classes to change location:

```jsx
{/* Bottom-left (default) */}
<div className="e-card-title e-card-bottom-left">Title</div>

{/* Bottom-right */}
<div className="e-card-title e-card-bottom-right">Title</div>

{/* Top-left */}
<div className="e-card-title e-card-top-left">Title</div>

{/* Top-right */}
<div className="e-card-title e-card-top-right">Title</div>
```

### Complete Image Title Example

```jsx
export default function ImageTitleCard() {
  return (
    <div className="e-card">
      <div className="e-card-image"
           style={{
             backgroundImage: 'url(./city.jpg)',
             backgroundSize: 'cover',
             backgroundPosition: 'center',
             height: '250px'
           }}>
        <div className="e-card-title e-card-bottom-left">
          Beautiful City
        </div>
      </div>
      <div className="e-card-content">
        <p>Stunning urban landscape photography</p>
      </div>
    </div>
  );
}
```

### Customizing Title Appearance

Style the title with CSS:

```jsx
<div className="e-card-image"
     style={{ backgroundImage: 'url(./image.jpg)', height: '200px' }}>
  <div className="e-card-title e-card-top-right"
       style={{
         fontSize: '24px',
         fontWeight: 'bold',
         padding: '15px',
         textShadow: '2px 2px 4px rgba(0,0,0,0.7)'
       }}>
    Custom Title
  </div>
</div>
```

### Interactive Title Position Selection

```jsx
import React, { useState } from 'react';

export default function InteractiveTitleCard() {
  const [position, setPosition] = useState('e-card-bottom-left');

  const positions = [
    { value: 'e-card-bottom-left', label: 'Bottom-Left' },
    { value: 'e-card-bottom-right', label: 'Bottom-Right' },
    { value: 'e-card-top-left', label: 'Top-Left' },
    { value: 'e-card-top-right', label: 'Top-Right' }
  ];

  return (
    <div>
      <div style={{ marginBottom: '20px' }}>
        {positions.map(pos => (
          <button
            key={pos.value}
            onClick={() => setPosition(pos.value)}
            style={{
              marginRight: '10px',
              padding: '8px 12px',
              cursor: 'pointer',
              background: position === pos.value ? '#007bff' : '#e9ecef',
              color: position === pos.value ? 'white' : 'black',
              border: 'none',
              borderRadius: '4px'
            }}>
            {pos.label}
          </button>
        ))}
      </div>

      <div className="e-card">
        <div className="e-card-image"
             style={{
               backgroundImage: 'url(./sample.jpg)',
               height: '300px'
             }}>
          <div className={`e-card-title ${position}`}>
            Image Title
          </div>
        </div>
      </div>
    </div>
  );
}
```

## Dividers

### Card Separator Element

The `e-card-separator` class creates a horizontal divider line between card sections:

```jsx
<div className="e-card">
  <div className="e-card-header-title">Section 1</div>
  <div className="e-card-separator" />
  <div className="e-card-content">
    First content section
  </div>
</div>
```

### Multiple Dividers

Use multiple dividers to create distinct content sections:

```jsx
export default function MultiSectionCard() {
  return (
    <div className="e-card">
      <div className="e-card-header-title">City Guide</div>
      <div className="e-card-separator" />

      <div className="e-card-content">
        <h3>Sydney</h3>
        <p>Sydney is a city on the east coast of Australia...</p>
      </div>
      <div className="e-card-separator" />

      <div className="e-card-content">
        <h3>New York</h3>
        <p>New York City has been described as the cultural capital...</p>
      </div>
      <div className="e-card-separator" />

      <div className="e-card-content">
        <h3>London</h3>
        <p>London is the capital and most populous city of England...</p>
      </div>
    </div>
  );
}
```

### Divider with Images and Content

```jsx
export default function RichDividedCard() {
  return (
    <div className="e-card">
      <div className="e-card-image"
           style={{
             backgroundImage: 'url(./header.jpg)',
             height: '150px'
           }} />
      <div className="e-card-separator" />

      <div className="e-card-content">
        <h2>Main Heading</h2>
        <p>Important content goes here</p>
      </div>
      <div className="e-card-separator" />

      <div className="e-card-content">
        <h3>Additional Details</h3>
        <ul>
          <li>Detail point 1</li>
          <li>Detail point 2</li>
          <li>Detail point 3</li>
        </ul>
      </div>
    </div>
  );
}
```

## Title Position Classes Reference

| Class | Position |
|-------|----------|
| `e-card-bottom-left` | Bottom-left corner (default) |
| `e-card-bottom-right` | Bottom-right corner |
| `e-card-top-left` | Top-left corner |
| `e-card-top-right` | Top-right corner |

## Common Patterns

### Pattern 1: Gallery Card

```jsx
<div className="e-card" style={{ maxWidth: '300px' }}>
  <div className="e-card-image"
       style={{
         backgroundImage: 'url(./gallery.jpg)',
         height: '250px'
       }}>
    <div className="e-card-title e-card-top-right">
      Featured
    </div>
  </div>
  <div className="e-card-separator" />
  <div className="e-card-content">
    <p>Beautiful sunset over the mountains</p>
  </div>
</div>
```

### Pattern 2: Article Preview

```jsx
<div className="e-card">
  <div className="e-card-image"
       style={{
         backgroundImage: 'url(./article-header.jpg)',
         height: '200px'
       }}>
    <div className="e-card-title e-card-bottom-left">
      Technology
    </div>
  </div>
  <div className="e-card-separator" />
  <div className="e-card-content">
    <h2>Article Title</h2>
    <p>Article summary and excerpt...</p>
  </div>
</div>
```

### Pattern 3: Feature Showcase

```jsx
<div className="e-card">
  <div className="e-card-header-title">Feature Name</div>
  <div className="e-card-separator" />
  <div className="e-card-image"
       style={{
         backgroundImage: 'url(./feature.jpg)',
         height: '150px'
       }} />
  <div className="e-card-separator" />
  <div className="e-card-content">
    Feature description and details
  </div>
</div>
```

## Tips & Best Practices

1. **Use dividers sparingly**: They organize content but too many can make a card feel cluttered

2. **Consistent image heights**: Keep image heights consistent across a grid of cards for better alignment

3. **Title contrast**: Ensure title text has sufficient contrast against background images (use `textShadow` if needed)

4. **Image optimization**: Compress images for better performance and faster loading

5. **Responsive heights**: Use percentage heights or viewport units for responsive layouts

**Next:** Learn about [action buttons](./action-buttons.md) to add interactive elements to your cards.
