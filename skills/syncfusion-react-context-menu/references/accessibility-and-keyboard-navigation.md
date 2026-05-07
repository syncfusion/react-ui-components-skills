# Accessibility and Keyboard Navigation

## Table of Contents
- [WCAG Compliance](#wcag-compliance)
- [Keyboard Shortcuts](#keyboard-shortcuts)
- [Screen Reader Support](#screen-reader-support)
- [ARIA Attributes](#aria-attributes)
- [Right-to-Left Support](#right-to-left-support)
- [Focus Management](#focus-management)
- [Testing Accessibility](#testing-accessibility)

## WCAG Compliance

The Syncfusion React ContextMenu component meets accessibility standards:

| Standard | Support | Details |
|----------|---------|---------|
| WCAG 2.2 | ✓ Full | Levels A, AA, AAA compliance |
| Section 508 | ✓ Full | US Federal accessibility requirements |
| ADA | ✓ Full | Americans with Disabilities Act compliance |
| Screen Reader Support | ✓ Full | JAWS, NVDA, VoiceOver compatible |
| Color Contrast | ✓ Full | Meets WCAG AA color contrast ratios |
| Keyboard Navigation | ✓ Full | Complete keyboard accessibility |
| Mobile Device Support | ✓ Full | Touch and mobile interactions |

## Keyboard Shortcuts

The ContextMenu supports standard keyboard navigation following WAI-ARIA patterns:

| Key | Action | Purpose |
|-----|--------|---------|
| `Esc` | Close menu | Close the open context menu or submenu |
| `Enter` | Select item | Activate the focused menu item |
| `Space` | Select item | Activate the focused menu item (alternative) |
| `↑ Arrow Up` | Navigate up | Move focus to previous menu item |
| `↓ Arrow Down` | Navigate down | Move focus to next menu item |
| `← Arrow Left` | Close submenu | Close open submenu and focus parent |
| `→ Arrow Right` | Open submenu | Open submenu of focused item |

### Example: Keyboard Navigation

```ts
import { ContextMenuComponent, MenuItemModel } from '@syncfusion/ej2-react-navigations';
import * as React from 'react';

function App() {
  const menuItems: MenuItemModel[] = [
    { text: 'Cut', iconCss: 'e-icons e-cut' },
    { text: 'Copy', iconCss: 'e-icons e-copy' },
    { text: 'Paste', iconCss: 'e-icons e-paste' },
    { separator: true },
    {
      text: 'More Options',
      items: [
        { text: 'Delete' },
        { text: 'Rename' },
        { text: 'Properties' }
      ]
    }
  ];

  return (
    <div>
      <div id='target' role='main'>
        <h2>Example: Press Ctrl+Shift+X to open context menu</h2>
        <p>Right click or use keyboard navigation</p>
      </div>
      <ContextMenuComponent target='#target' items={menuItems} />
    </div>
  );
}

export default App;
```

## Screen Reader Support

The ContextMenu is fully compatible with assistive technologies:

**Tested Screen Readers:**
- JAWS (Windows)
- NVDA (Windows, Linux)
- VoiceOver (macOS, iOS)
- TalkBack (Android)

### Screen Reader Announcements

When users interact with the context menu:
1. Menu opening is announced: "Context Menu, popup"
2. Menu items are read individually
3. Item state (disabled) is announced
4. Submenu availability is indicated
5. Keyboard shortcuts are announced if provided

### Example: Keyboard Shortcut Announcements

```ts
import { ContextMenuComponent, MenuItemModel } from '@syncfusion/ej2-react-navigations';
import * as React from 'react';

function App() {
  const menuItems: MenuItemModel[] = [
    { 
      text: 'Cut',
      iconCss: 'e-icons e-cut',
      // Screen readers will announce: "Cut, keyboard shortcut Ctrl+X"
    },
    { 
      text: 'Copy',
      iconCss: 'e-icons e-copy',
      // Screen readers will announce: "Copy, keyboard shortcut Ctrl+C"
    },
    { 
      text: 'Paste',
      iconCss: 'e-icons e-paste',
      // Screen readers will announce: "Paste, keyboard shortcut Ctrl+V"
    }
  ];

  return (
    <div>
      <div id='target'>Right click to open menu</div>
      <ContextMenuComponent target='#target' items={menuItems} />
    </div>
  );
}

export default App;
```

## ARIA Attributes

The ContextMenu automatically includes WAI-ARIA attributes for accessibility:

| ARIA Attribute | Value | Purpose |
|----------------|-------|---------|
| `role` | `menu` | Identifies the element as a menu widget |
| `role` | `menuitem` | Identifies each item as a menu item |
| `aria-haspopup` | `true` | Indicates menu has a popup submenu |
| `aria-expanded` | `true/false` | Indicates if submenu is expanded |
| `aria-label` | Item text | Provides accessible label for items |
| `aria-disabled` | `true` | Indicates item is disabled |

### Example: ARIA in Action

```ts
// Generated HTML (automatically by Syncfusion)
// <ul role="menu" class="e-contextmenu">
//   <li role="menuitem" aria-label="Cut">
//     <a href="#">Cut</a>
//   </li>
//   <li role="menuitem" aria-label="Copy">
//     <a href="#">Copy</a>
//   </li>
//   <li role="menuitem" aria-haspopup="true" aria-expanded="false">
//     <a href="#">Edit</a>
//     <ul role="menu" aria-hidden="true">...</ul>
//   </li>
// </ul>
```

## Right-to-Left Support

The ContextMenu supports RTL languages (Arabic, Hebrew, etc.):

```ts
import { ContextMenuComponent, MenuItemModel } from '@syncfusion/ej2-react-navigations';
import * as React from 'react';

function App() {
  const menuItems: MenuItemModel[] = [
    { text: 'قص' },      // Arabic: Cut
    { text: 'نسخ' },     // Arabic: Copy
    { text: 'لصق' }      // Arabic: Paste
  ];

  return (
    <div dir='rtl'>
      <div id='target'>انقر بزر الماوس الأيمن لفتح القائمة</div>
      <ContextMenuComponent target='#target' items={menuItems} />
    </div>
  );
}

export default App;
```

**Enable RTL:**
- Add `dir='rtl'` to parent container
- Context menu automatically adjusts layout
- Icons and arrows reverse direction

## Focus Management

Proper focus management ensures keyboard navigation works correctly:

```ts
import { ContextMenuComponent, MenuEventArgs } from '@syncfusion/ej2-react-navigations';
import * as React from 'react';

function App() {
  const menuRef = React.useRef<ContextMenuComponent>(null);

  const handleBeforeOpen = (args: MenuEventArgs) => {
    // Focus management automatically handled by component
    // First menu item receives focus when menu opens
    // Focus is maintained during navigation
  };

  const handleBeforeClose = (args: MenuEventArgs) => {
    // Focus returns to trigger element when menu closes
  };

  return (
    <div>
      <div id='target' tabIndex={0}>
        Right click to open menu
      </div>
      <ContextMenuComponent
        ref={menuRef}
        target='#target'
        beforeOpen={handleBeforeOpen}
        beforeClose={handleBeforeClose}
      />
    </div>
  );
}

export default App;
```

## Testing Accessibility

### Automated Accessibility Validation

The ContextMenu is tested with:
- **Accessibility Checker:** Validates WCAG compliance
- **Axe Core:** Detects accessibility violations
- **Jest-Axe:** Integration testing with React

### Manual Testing Checklist

- [ ] Navigate entire menu using keyboard only
- [ ] All items are reachable via Tab/Arrow keys
- [ ] Screen reader announces all menu items correctly
- [ ] Submenu navigation works with arrow keys
- [ ] Escape key closes menu and returns focus
- [ ] Disabled items are skipped during navigation
- [ ] Menu works in different browsers
- [ ] Touch devices can access context menu
- [ ] High contrast mode displays correctly

### Example: Accessibility Testing

```ts
// Test keyboard navigation
describe('ContextMenu Accessibility', () => {
  it('should navigate menu items with arrow keys', () => {
    // Simulate arrow key navigation
    const menuItem = document.querySelector('.e-menu-item');
    const downArrowEvent = new KeyboardEvent('keydown', {
      key: 'ArrowDown',
      code: 'ArrowDown'
    });
    menuItem?.dispatchEvent(downArrowEvent);
    
    // Verify next item is focused
    expect(document.activeElement?.textContent).toBe('Copy');
  });

  it('should close menu with Escape key', () => {
    // Simulate Escape key
    const escapeEvent = new KeyboardEvent('keydown', {
      key: 'Escape',
      code: 'Escape'
    });
    document.dispatchEvent(escapeEvent);
    
    // Verify menu is closed
    const contextMenu = document.querySelector('.e-contextmenu-wrapper');
    expect(contextMenu?.style.display).toBe('none');
  });
});
```

## Best Practices

1. **Always include text labels:** Never rely on icons alone; include descriptive text
2. **Use semantic HTML:** Leverage the component's built-in ARIA attributes
3. **Keyboard-first design:** Test all functionality with keyboard navigation
4. **Color accessibility:** Don't convey meaning through color alone
5. **Test with real assistive tech:** Use actual screen readers for testing
6. **Provide alternatives:** Include keyboard shortcuts in documentation
7. **Consistent patterns:** Follow standard menu interaction patterns
8. **Performance:** Ensure animations don't interfere with screen readers

## Accessibility Resources

- [WAI-ARIA Menu Pattern](https://www.w3.org/WAI/ARIA/apg/patterns/menubar/)
- [WCAG 2.2 Guidelines](https://www.w3.org/TR/WCAG22/)
- [Section 508 Standards](https://www.section508.gov/)
- [Syncfusion Accessibility Documentation](../common/accessibility)
