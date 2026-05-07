# Card Structure & Headers

## Table of Contents
- [Header Organization](#header-organization)
- [Title and Subtitle](#title-and-subtitle)
- [Header Images](#header-images)
- [Content Organization](#content-organization)
- [Complete Examples](#complete-examples)

## Header Organization

The card header provides a consistent area for displaying introductory information like titles, subtitles, and images.

### Header Container

The `e-card-header` class defines the header section. Inside it, use `e-card-header-caption` to group text content:

```jsx
<div className="e-card">
  <div className="e-card-header">
    <div className="e-card-header-caption">
      {/* Title and subtitle go here */}
    </div>
  </div>
</div>
```

**When to use headers:**
- User profile cards (name and title)
- Blog posts (article title and date)
- Product cards (product name and category)
- Team member listings
- Contact cards

## Title and Subtitle

### Header Title

The `e-card-header-title` class displays the main heading:

```jsx
<div className="e-card-header-caption">
  <div className="e-card-header-title">Main Title</div>
</div>
```

### Header Subtitle

The `e-card-sub-title` class displays secondary text below the title:

```jsx
<div className="e-card-header-caption">
  <div className="e-card-header-title">Primary Title</div>
  <div className="e-card-sub-title">Secondary information</div>
</div>
```

### Complete Title Block

```jsx
<div className="e-card">
  <div className="e-card-header">
    <div className="e-card-header-caption">
      <div className="e-card-header-title">Laura Callahan</div>
      <div className="e-card-sub-title">Sales Coordinator</div>
    </div>
  </div>
  <div className="e-card-content">
    Laura received a BA in psychology from the University of Washington.
  </div>
</div>
```

## Header Images

### Adding Header Images

Use the `e-card-header-image` class to include an image in the header. The image can be positioned before or after the caption text:

```jsx
<div className="e-card-header">
  <div className="e-card-header-image" />
  <div className="e-card-header-caption">
    <div className="e-card-header-title">Title</div>
  </div>
</div>
```

### Image Positioning

**Image before caption (left-aligned):**

```jsx
<div className="e-card">
  <div className="e-card-header">
    <div className="e-card-header-image" style={{ backgroundImage: 'url(./profile.jpg)' }} />
    <div className="e-card-header-caption">
      <div className="e-card-header-title">Name</div>
      <div className="e-card-sub-title">Role</div>
    </div>
  </div>
</div>
```

**Image after caption (right-aligned):**

```jsx
<div className="e-card">
  <div className="e-card-header">
    <div className="e-card-header-caption">
      <div className="e-card-header-title">Name</div>
      <div className="e-card-sub-title">Role</div>
    </div>
    <div className="e-card-header-image" style={{ backgroundImage: 'url(./profile.jpg)' }} />
  </div>
</div>
```

### Rounded Corner Images

Add the `e-card-corner` class to create rounded corner images:

```jsx
<div className="e-card-header-image e-card-corner" 
     style={{ backgroundImage: 'url(./avatar.jpg)' }} />
```

### Image Sizing

Style the header image with width and height:

```jsx
<div className="e-card-header-image e-card-corner" 
     style={{ 
       backgroundImage: 'url(./avatar.jpg)',
       width: '60px',
       height: '60px',
       backgroundSize: 'cover',
       backgroundPosition: 'center'
     }} />
```

## Content Organization

### Content Section

The `e-card-content` class holds the main body of the card:

```jsx
<div className="e-card-content">
  Main content goes here. This can include text, links, forms, or other HTML elements.
</div>
```

### Multiple Content Sections

You can add multiple content sections separated by dividers:

```jsx
<div className="e-card">
  <div className="e-card-title">Explore Cities</div>
  <div className="e-card-separator" />
  <div className="e-card-content">
    Sydney is a city on the east coast of Australia...
  </div>
  <div className="e-card-separator" />
  <div className="e-card-content">
    New York City has been described as the cultural capital...
  </div>
</div>
```

### Content Flexibility

Content sections accept any HTML:

```jsx
<div className="e-card-content">
  <h3>Section Title</h3>
  <p>Paragraph text</p>
  <ul>
    <li>List item 1</li>
    <li>List item 2</li>
  </ul>
  <a href="#">Link</a>
</div>
```

## Complete Examples

### Example 1: Professional Profile Card

```jsx
export default function ProfileCard() {
  return (
    <div className="e-card">
      <div className="e-card-header">
        <div className="e-card-header-image e-card-corner"
             style={{
               backgroundImage: 'url(./profile.jpg)',
               width: '80px',
               height: '80px'
             }} />
        <div className="e-card-header-caption">
          <div className="e-card-header-title">John Smith</div>
          <div className="e-card-sub-title">Senior Developer</div>
        </div>
      </div>
      <div className="e-card-content">
        <p>10+ years of experience in web development</p>
        <p>Specializing in React and modern JavaScript</p>
      </div>
    </div>
  );
}
```

### Example 2: Blog Article Card

```jsx
export default function BlogCard() {
  return (
    <div className="e-card">
      <div className="e-card-header">
        <div className="e-card-header-caption">
          <div className="e-card-header-title">
            Getting Started with React Hooks
          </div>
          <div className="e-card-sub-title">
            Published March 26, 2026 by John Doe
          </div>
        </div>
      </div>
      <div className="e-card-content">
        <p>React Hooks allow you to use state and other React features without writing a class component...</p>
      </div>
    </div>
  );
}
```

### Example 3: Product Header Card

```jsx
export default function ProductCard() {
  return (
    <div className="e-card">
      <div className="e-card-header">
        <div className="e-card-header-image"
             style={{
               backgroundImage: 'url(./product.jpg)',
               height: '120px'
             }} />
        <div className="e-card-header-caption">
          <div className="e-card-header-title">Premium Headphones</div>
          <div className="e-card-sub-title">Audio Excellence Series</div>
        </div>
      </div>
      <div className="e-card-content">
        <p><strong>$199.99</strong></p>
        <p>High-quality sound with noise cancellation</p>
      </div>
    </div>
  );
}
```

## Key CSS Classes

| Class | Purpose |
|-------|---------|
| `e-card-header` | Header container |
| `e-card-header-caption` | Text content wrapper |
| `e-card-header-title` | Main title |
| `e-card-header-image` | Header image |
| `e-card-sub-title` | Subtitle text |
| `e-card-corner` | Rounded corners for images |
| `e-card-content` | Content area |
| `e-card-separator` | Divider line |

## Tips & Best Practices

1. **Always wrap title/subtitle in caption**: The `e-card-header-caption` provides proper spacing and alignment

2. **Use semantic HTML**: Include proper heading levels (h1, h2, h3) within content sections for accessibility

3. **Image optimization**: Use appropriately sized images and set explicit width/height to prevent layout shift

4. **Responsive design**: Use percentage widths and max-widths for responsive cards

5. **Spacing**: Let CSS handle alignment; the framework provides default spacing

**Next:** Learn about [images, titles, and dividers](./images-titles-dividers.md) for full-width card images and advanced layouts.
