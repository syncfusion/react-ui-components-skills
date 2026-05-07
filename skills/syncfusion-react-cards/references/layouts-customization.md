# Layouts & Customization

## Table of Contents
- [Horizontal Card Layout](#horizontal-card-layout)
- [Stacked Sections](#stacked-sections)
- [Image Title Positioning](#image-title-positioning)
- [Advanced Customization](#advanced-customization)
- [Responsive Patterns](#responsive-patterns)

## Horizontal Card Layout

By default, card elements are stacked vertically following the natural DOM flow. The horizontal layout provides an alternative arrangement where elements are positioned side-by-side.

### Basic Horizontal Layout

Add the `e-card-horizontal` class to the root card element:

```jsx
<div className="e-card e-card-horizontal">
  <img src="./image.png" alt="Sample" style={{ height: '180px' }} />
  <div className="e-card-stacked">
    <div className="e-card-header">
      <div className="e-card-header-caption">
        <div className="e-card-header-title">Title</div>
      </div>
    </div>
    <div className="e-card-content">
      Content arranged vertically within horizontal layout
    </div>
  </div>
</div>
```

### When to Use Horizontal Layout

- Product listings with image and details side-by-side
- Team member cards with photo and information
- Article previews with thumbnail and summary
- Blog post cards with featured image
- Settings or preference cards with icon and options

### Complete Horizontal Layout Example

```jsx
export default function HorizontalCard() {
  return (
    <div className="e-card e-card-horizontal" style={{ width: '400px', margin: '20px' }}>
      <img src="./product.jpg" alt="Product"
           style={{
             width: '150px',
             height: '150px',
             objectFit: 'cover'
           }} />
      <div className="e-card-stacked">
        <div className="e-card-header">
          <div className="e-card-header-caption">
            <div className="e-card-header-title">Philips Trimmer</div>
            <div className="e-card-sub-title">Premium Quality</div>
          </div>
        </div>
        <div className="e-card-content">
          <p>Powered by innovative DuraPower Technology for long-lasting performance.</p>
          <p style={{ fontSize: '14px', color: '#666' }}>4.5 ⭐ (320 reviews)</p>
        </div>
        <div className="e-card-actions">
          <button className="e-card-btn" style={{ flex: 1 }}>View</button>
          <button className="e-card-btn" style={{ flex: 1 }}>Buy Now</button>
        </div>
      </div>
    </div>
  );
}
```

## Stacked Sections

### About Stacked Sections

The `e-card-stacked` class forces vertical alignment for specific sections within a horizontal layout. This is useful when you want part of your card to maintain the default vertical stacking while other parts are arranged horizontally.

### Basic Stacked Example

```jsx
<div className="e-card e-card-horizontal">
  {/* First element - positioned horizontally */}
  <img src="./image.jpg" alt="Featured" style={{ width: '200px', height: '150px' }} />

  {/* Stacked section - content flows vertically */}
  <div className="e-card-stacked">
    <div className="e-card-header">
      <div className="e-card-header-caption">
        <div className="e-card-header-title">Feature Title</div>
      </div>
    </div>
    <div className="e-card-content">First content block</div>
    <div className="e-card-separator" />
    <div className="e-card-content">Second content block</div>
  </div>
</div>
```

### Multiple Content Sections in Stacked

```jsx
export default function StackedSectionsCard() {
  return (
    <div className="e-card e-card-horizontal" style={{ width: '500px' }}>
      <div style={{
        background: 'linear-gradient(135deg, #667eea 0%, #764ba2 100%)',
        width: '150px',
        display: 'flex',
        alignItems: 'center',
        justifyContent: 'center',
        color: 'white',
        fontSize: '48px'
      }}>
        📊
      </div>
      <div className="e-card-stacked" style={{ flex: 1 }}>
        <div className="e-card-header-title">Analytics Report</div>
        <div className="e-card-separator" />
        <div className="e-card-content">
          <h4>Performance Metrics</h4>
          <p>Total Views: 12,450</p>
          <p>Engagement Rate: 8.5%</p>
        </div>
        <div className="e-card-separator" />
        <div className="e-card-content">
          <h4>Key Insights</h4>
          <ul style={{ margin: '8px 0', paddingLeft: '20px' }}>
            <li>Peak hours: 2-4 PM</li>
            <li>Top source: Organic search</li>
            <li>Device: Mobile (65%)</li>
          </ul>
        </div>
      </div>
    </div>
  );
}
```

## Image Title Positioning

### Default Title Position

By default, image titles appear in the **bottom-left corner** with an overlay effect:

```jsx
<div className="e-card-image" style={{ backgroundImage: 'url(./image.jpg)', height: '200px' }}>
  <div className="e-card-title">Image Title</div>
</div>
```

### Position Classes

Use positioning classes to customize title location:

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

### Complete Position Example

```jsx
export default function TitlePositionCard() {
  const [position, setPosition] = React.useState('e-card-bottom-left');

  const positions = [
    { value: 'e-card-bottom-left', label: 'Bottom-Left' },
    { value: 'e-card-bottom-right', label: 'Bottom-Right' },
    { value: 'e-card-top-left', label: 'Top-Left' },
    { value: 'e-card-top-right', label: 'Top-Right' }
  ];

  return (
    <div>
      <div style={{ marginBottom: '20px', display: 'flex', gap: '10px' }}>
        {positions.map(pos => (
          <button
            key={pos.value}
            onClick={() => setPosition(pos.value)}
            style={{
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

      <div className="e-card" style={{ maxWidth: '300px' }}>
        <div className="e-card-image"
             style={{
               backgroundImage: 'url(./scenic.jpg)',
               backgroundSize: 'cover',
               backgroundPosition: 'center',
               height: '250px'
             }}>
          <div className={`e-card-title ${position}`}
               style={{
                 fontSize: '18px',
                 fontWeight: 'bold',
                 padding: '10px'
               }}>
            Mountain View
          </div>
        </div>
      </div>
    </div>
  );
}
```

### Customizing Position Styles

Override default positioning with CSS:

```jsx
<style>{`
  .e-card-title.custom-position {
    top: 50% !important;
    left: 50% !important;
    transform: translate(-50%, -50%) !important;
    background: rgba(0,0,0,0.7) !important;
  }
`}</style>

<div className="e-card-title custom-position">Centered Title</div>
```

## Advanced Customization

### Custom Card Styling

```jsx
const cardStyle = {
  borderRadius: '12px',
  boxShadow: '0 4px 12px rgba(0,0,0,0.1)',
  overflow: 'hidden',
  transition: 'transform 0.3s ease, box-shadow 0.3s ease'
};

const hoverStyle = {
  transform: 'translateY(-4px)',
  boxShadow: '0 8px 20px rgba(0,0,0,0.15)'
};

export default function CustomStyledCard() {
  const [isHovered, setIsHovered] = React.useState(false);

  return (
    <div className="e-card"
         onMouseEnter={() => setIsHovered(true)}
         onMouseLeave={() => setIsHovered(false)}
         style={{
           ...cardStyle,
           ...(isHovered ? hoverStyle : {})
         }}>
      <div className="e-card-image"
           style={{
             backgroundImage: 'url(./image.jpg)',
             height: '200px'
           }} />
      <div className="e-card-content">
        <h3>Elevated Card</h3>
        <p>Hover to see the elevation effect</p>
      </div>
    </div>
  );
}
```

### Card Grid Layout

```jsx
export default function CardGrid() {
  const cards = [
    { id: 1, title: 'Card 1', image: './image1.jpg' },
    { id: 2, title: 'Card 2', image: './image2.jpg' },
    { id: 3, title: 'Card 3', image: './image3.jpg' },
    { id: 4, title: 'Card 4', image: './image4.jpg' }
  ];

  return (
    <div style={{
      display: 'grid',
      gridTemplateColumns: 'repeat(auto-fill, minmax(250px, 1fr))',
      gap: '20px',
      padding: '20px'
    }}>
      {cards.map(card => (
        <div key={card.id} className="e-card">
          <div className="e-card-image"
               style={{
                 backgroundImage: `url(${card.image})`,
                 height: '200px'
               }}>
            <div className="e-card-title">{card.title}</div>
          </div>
          <div className="e-card-content">
            <p>Card description</p>
          </div>
        </div>
      ))}
    </div>
  );
}
```

## Responsive Patterns

### Mobile-First Responsive Card

```jsx
export default function ResponsiveCard() {
  const [isMobile, setIsMobile] = React.useState(window.innerWidth < 768);

  React.useEffect(() => {
    const handleResize = () => setIsMobile(window.innerWidth < 768);
    window.addEventListener('resize', handleResize);
    return () => window.removeEventListener('resize', handleResize);
  }, []);

  return (
    <div className={`e-card ${isMobile ? '' : 'e-card-horizontal'}`}
         style={{ maxWidth: isMobile ? '100%' : '500px' }}>
      <img src="./product.jpg" alt="Product"
           style={{
             width: isMobile ? '100%' : '150px',
             height: isMobile ? '200px' : '150px',
             objectFit: 'cover'
           }} />
      <div className={isMobile ? '' : 'e-card-stacked'}>
        <div className="e-card-header">
          <div className="e-card-header-caption">
            <div className="e-card-header-title">Product Title</div>
          </div>
        </div>
        <div className="e-card-content">Product description</div>
      </div>
    </div>
  );
}
```

### CSS Media Query Approach

```jsx
const styles = `
  @media (max-width: 768px) {
    .e-card.responsive-card {
      flex-direction: column !important;
    }
    
    .e-card.responsive-card img {
      width: 100% !important;
    }
  }

  @media (min-width: 768px) {
    .e-card.responsive-card {
      flex-direction: row !important;
    }
  }
`;

export default function ResponsiveMediaCard() {
  return (
    <>
      <style>{styles}</style>
      <div className="e-card responsive-card e-card-horizontal">
        <img src="./image.jpg" alt="Sample" style={{ height: '150px' }} />
        <div className="e-card-stacked">
          <div className="e-card-content">Content here</div>
        </div>
      </div>
    </>
  );
}
```

## Position Classes Reference

| Class | Position |
|-------|----------|
| `e-card-bottom-left` | Bottom-left corner (default) |
| `e-card-bottom-right` | Bottom-right corner |
| `e-card-top-left` | Top-left corner |
| `e-card-top-right` | Top-right corner |

## Layout Classes Reference

| Class | Purpose |
|-------|---------|
| `e-card-horizontal` | Horizontal layout |
| `e-card-stacked` | Vertical stack within horizontal |

## Tips & Best Practices

1. **Container width**: Set explicit width for horizontal cards to control proportions

2. **Image dimensions**: Use consistent image sizes for aligned grids

3. **Flex layout**: Use flexbox for responsive horizontal layouts

4. **Mobile consideration**: Test horizontal layouts on mobile—may need to stack vertically

5. **Spacing**: Use padding and margins for consistent internal spacing

**Next:** Learn about [embedding components](./embedding-components.md) to integrate other Syncfusion components within cards.
