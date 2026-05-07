# Action Buttons

## Adding Action Buttons

Action buttons allow users to interact with card content. They are organized in an action container at the bottom of the card.

### Basic Button Structure

Action buttons are contained within a `div` with the `e-card-actions` class:

```jsx
<div className="e-card">
  <div className="e-card-content">
    Card content
  </div>
  <div className="e-card-actions">
    <button className="e-card-btn">Button 1</button>
    <button className="e-card-btn">Button 2</button>
  </div>
</div>
```

### Button and Link Elements

Use both `button` and `anchor` elements with the `e-card-btn` class:

```jsx
<div className="e-card-actions">
  <button className="e-card-btn">Action Button</button>
  <a href="#" className="e-card-btn">Link Button</a>
</div>
```

### Multiple Action Buttons

```jsx
<div className="e-card-actions">
  <button className="e-card-btn">View</button>
  <button className="e-card-btn">Edit</button>
  <button className="e-card-btn">Delete</button>
</div>
```

## Horizontal Layout (Default)

By default, action buttons are positioned horizontally from left to right:

```jsx
export default function HorizontalButtonsCard() {
  return (
    <div className="e-card" style={{ maxWidth: '300px' }}>
      <div className="e-card-header-title">Product Name</div>
      <div className="e-card-content">
        High-quality product with excellent features.
      </div>
      <div className="e-card-actions">
        <button className="e-card-btn">View</button>
        <button className="e-card-btn">Add to Cart</button>
        <button className="e-card-btn">Share</button>
      </div>
    </div>
  );
}
```

**Output:** Buttons displayed side-by-side from left to right.

## Vertical Layout

Add the `e-card-vertical` class to the actions container to stack buttons vertically:

```jsx
<div className="e-card-actions e-card-vertical">
  <button className="e-card-btn">More</button>
  <a href="#" className="e-card-btn">Share</a>
  <button className="e-card-btn">Archive</button>
</div>
```

### Complete Vertical Buttons Card

```jsx
export default function VerticalButtonsCard() {
  return (
    <div className="e-card" style={{ maxWidth: '300px' }}>
      <div className="e-card-header-title">Eiffel Tower</div>
      <div className="e-card-content">
        The Eiffel Tower is acknowledged as the universal symbol of Paris and France.
      </div>
      <div className="e-card-actions e-card-vertical">
        <button className="e-card-btn">More</button>
        <a href="#" className="e-card-btn">Share</a>
        <button className="e-card-btn">Save</button>
      </div>
    </div>
  );
}
```

## Icon Buttons

### Icon with Text

```jsx
<div className="e-card-actions">
  <button className="e-card-btn">
    <span>📌</span> Bookmark
  </button>
  <button className="e-card-btn">
    <span>❤️</span> Like
  </button>
  <button className="e-card-btn">
    <span>🔗</span> Share
  </button>
</div>
```

### Icon Only Buttons

```jsx
<div className="e-card-actions">
  <button className="e-card-btn" title="Bookmark">📌</button>
  <button className="e-card-btn" title="Like">❤️</button>
  <button className="e-card-btn" title="Share">🔗</button>
</div>
```

### Using Image Icons

```jsx
<div className="e-card-actions">
  <button className="e-card-btn">
    <img src='./bookmark.png' alt="Bookmark" style={{ width: '20px' }} />
  </button>
  <button className="e-card-btn">
    <img src='./like.png' alt="Like" style={{ width: '20px' }} />
  </button>
  <button className="e-card-btn">
    <img src='./share.png' alt="Share" style={{ width: '20px' }} />
  </button>
</div>
```

## Button Styling

### Custom Button Styling

Style buttons with inline CSS or CSS classes:

```jsx
<button className="e-card-btn"
        style={{
          padding: '10px 20px',
          fontSize: '14px',
          fontWeight: 'bold',
          borderRadius: '4px'
        }}>
  Custom Button
</button>
```

### Primary and Secondary Buttons

```jsx
<div className="e-card-actions">
  <button className="e-card-btn"
          style={{
            background: '#007bff',
            color: 'white',
            border: 'none',
            padding: '8px 16px'
          }}>
    Primary
  </button>
  <button className="e-card-btn"
          style={{
            background: '#e9ecef',
            color: '#333',
            border: 'none',
            padding: '8px 16px'
          }}>
    Secondary
  </button>
</div>
```

### Disabled Buttons

```jsx
<button className="e-card-btn"
        disabled
        style={{
          opacity: 0.5,
          cursor: 'not-allowed'
        }}>
  Disabled Button
</button>
```

## Interactive Button Examples

### Example 1: E-Commerce Card

```jsx
export default function ProductCard() {
  const [inCart, setInCart] = React.useState(false);

  return (
    <div className="e-card" style={{ maxWidth: '280px' }}>
      <div className="e-card-image"
           style={{
             backgroundImage: 'url(./product.jpg)',
             height: '200px'
           }}>
        <div className="e-card-title e-card-top-right">
          Sale
        </div>
      </div>
      <div className="e-card-header-title">Wireless Headphones</div>
      <div className="e-card-sub-title">$99.99</div>
      <div className="e-card-content">
        <p>Premium sound quality with noise cancellation</p>
      </div>
      <div className="e-card-actions">
        <button className="e-card-btn"
                onClick={() => setInCart(!inCart)}
                style={{
                  background: inCart ? '#dc3545' : '#28a745',
                  color: 'white',
                  flex: 1
                }}>
          {inCart ? 'Remove' : 'Add to Cart'}
        </button>
        <button className="e-card-btn"
                style={{
                  background: '#6c757d',
                  color: 'white'
                }}>
          ❤️
        </button>
      </div>
    </div>
  );
}
```

### Example 2: Team Member Card

```jsx
export default function TeamMemberCard() {
  const [expanded, setExpanded] = React.useState(false);

  return (
    <div className="e-card">
      <div className="e-card-header">
        <div className="e-card-header-image e-card-corner"
             style={{
               backgroundImage: 'url(./avatar.jpg)',
               width: '80px',
               height: '80px'
             }} />
        <div className="e-card-header-caption">
          <div className="e-card-header-title">Sarah Johnson</div>
          <div className="e-card-sub-title">Product Designer</div>
        </div>
      </div>
      <div className="e-card-content">
        <p>10+ years of experience in UX/UI design</p>
        {expanded && <p>Email: sarah@company.com | Phone: +1-234-567-8900</p>}
      </div>
      <div className="e-card-actions">
        <button className="e-card-btn"
                onClick={() => setExpanded(!expanded)}>
          {expanded ? 'Less Info' : 'More Info'}
        </button>
        <a href="#" className="e-card-btn">Message</a>
        <a href="#" className="e-card-btn">Connect</a>
      </div>
    </div>
  );
}
```

### Example 3: Settings Card

```jsx
export default function SettingsCard() {
  const [notifications, setNotifications] = React.useState(true);

  return (
    <div className="e-card">
      <div className="e-card-header-title">Notification Settings</div>
      <div className="e-card-separator" />
      <div className="e-card-content">
        <label>
          <input type="checkbox"
                 checked={notifications}
                 onChange={(e) => setNotifications(e.target.checked)} />
          Enable notifications
        </label>
      </div>
      <div className="e-card-separator" />
      <div className="e-card-actions e-card-vertical">
        <button className="e-card-btn"
                style={{
                  background: '#007bff',
                  color: 'white',
                  padding: '10px'
                }}>
          Save Settings
        </button>
        <button className="e-card-btn"
                style={{
                  background: '#e9ecef',
                  padding: '10px'
                }}>
          Reset
        </button>
      </div>
    </div>
  );
}
```

## Best Practices

1. **Limit buttons**: Keep 2-4 action buttons per card for clarity

2. **Clear labels**: Use descriptive button text (avoid "Click here")

3. **Consistent styling**: Maintain consistent button appearance across cards

4. **Logical order**: Place primary actions (e.g., "Buy Now") first

5. **Visual hierarchy**: Use color and styling to highlight important actions

6. **Responsive spacing**: Buttons should have adequate spacing on mobile devices

7. **Accessibility**: Use semantic HTML (`<button>` or `<a>`) with descriptive labels

## CSS Classes Reference

| Class | Purpose |
|-------|---------|
| `e-card-actions` | Action button container |
| `e-card-btn` | Individual button styling |
| `e-card-vertical` | Vertical button alignment |

**Next:** Learn about [layouts and customization](./layouts-customization.md) for horizontal card layouts and advanced positioning.
