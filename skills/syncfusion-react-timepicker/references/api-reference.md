# TimePicker API Reference

## Table of Contents
- [Properties](#properties)
- [Methods](#methods)
- [Events](#events)
- [Interfaces](#interfaces)
- [Type Definitions](#type-definitions)

---

## Properties

Complete list of all TimePicker properties with descriptions, types, defaults, and use cases.

### allowEdit

| Property | Value |
|----------|-------|
| **Type** | boolean |
| **Default** | true |
| **Description** | Specifies whether the input textbox is editable or not. When false, users can only select from popup, not type. |

**Use Case:** Create read-only time selection (users click icon only, no keyboard input)

```tsx
<TimePickerComponent allowEdit={false} />
```

---

### cssClass

| Property | Value |
|----------|-------|
| **Type** | string |
| **Default** | null |
| **Description** | Specifies root CSS class for customizing appearance by overriding styles. |

**Use Case:** Apply custom styling via CSS classes

```tsx
<TimePickerComponent cssClass="custom-timepicker primary-theme" />
```

---

### enableMask

| Property | Value |
|----------|-------|
| **Type** | boolean |
| **Default** | false |
| **Description** | Enables masked input mode with format guidance. |

**Use Case:** Guide users with visual format hints (HH:MM AM/PM)

```tsx
<TimePickerComponent enableMask={true} format="hh:mm a" />
```

---

### enablePersistence

| Property | Value |
|----------|-------|
| **Type** | boolean |
| **Default** | false |
| **Description** | Enable persisting component's state between page reloads. Persists: Value |

**Use Case:** Remember selected time across browser sessions

```tsx
<TimePickerComponent enablePersistence={true} />
```

---

### enableRtl

| Property | Value |
|----------|-------|
| **Type** | boolean |
| **Default** | false |
| **Description** | Enable rendering component in right-to-left direction. |

**Use Case:** Support Arabic, Hebrew, Persian languages

```tsx
<TimePickerComponent enableRtl={true} locale="ar-SA" />
```

---

### enabled

| Property | Value |
|----------|-------|
| **Type** | boolean |
| **Default** | true |
| **Description** | Specifies whether component is enabled or disabled. |

**Use Case:** Disable time picker conditionally (e.g., when form is readonly)

```tsx
<TimePickerComponent enabled={!isFormLocked} />
```

---

### floatLabelType

| Property | Value |
|----------|-------|
| **Type** | 'Never' \| 'Always' \| 'Auto' |
| **Default** | 'Never' |
| **Description** | Specifies how placeholder floats in the input. |

**Values:**
- **Never:** Label stays inline with placeholder
- **Always:** Label always floats above input
- **Auto:** Label floats on focus or value entry

**Use Case:** Modern form styling with floating labels

```tsx
<TimePickerComponent 
  floatLabelType="Auto" 
  placeholder="Select time"
/>
```

---

### format

| Property | Value |
|----------|-------|
| **Type** | string \| TimeFormatObject |
| **Default** | Based on culture |
| **Description** | Time display format. Defaults to culture-specific format. |

**Common Formats:**
- `"HH:mm"` - 24-hour (14:30)
- `"hh:mm a"` - 12-hour (2:30 PM)
- `"HH:mm:ss"` - With seconds (14:30:45)
- `{ skeleton: 'Hm' }` - Locale-aware format

**Use Case:** Display time in specific format

```tsx
<TimePickerComponent format="hh:mm a" />
```

---

### fullScreenMode

| Property | Value |
|----------|-------|
| **Type** | boolean |
| **Default** | false |
| **Description** | Show time picker popup in full-screen mode on mobile devices. |

**Use Case:** Better UX on small screens

```tsx
<TimePickerComponent fullScreenMode={true} />
```

---

### htmlAttributes

| Property | Value |
|----------|-------|
| **Type** | { [key: string]: string } |
| **Default** | {} |
| **Description** | Additional HTML attributes for input element (name, placeholder, aria-*, etc.) |

**Use Case:** Add form field attributes and accessibility labels

```tsx
<TimePickerComponent 
  htmlAttributes={{
    name: 'appointment_time',
    'aria-label': 'Select appointment time',
    'data-testid': 'time-picker'
  }}
/>
```

---

### keyConfigs

| Property | Value |
|----------|-------|
| **Type** | { [key: string]: string } |
| **Default** | null |
| **Description** | Customize keyboard shortcuts for different key actions. |

**Default Mappings:**
- `enter` - Select time
- `escape` - Close popup
- `up/down` - Navigate times
- `home/end` - First/last time
- `alt+down` - Open popup
- `alt+up` - Close popup

**Use Case:** Support alternative keyboard layouts (German, French, etc.)

```tsx
<TimePickerComponent 
  keyConfigs={{
    enter: 'space',
    home: 'ctrl+home',
    end: 'ctrl+end'
  }}
/>
```

---

### locale

| Property | Value |
|----------|-------|
| **Type** | string |
| **Default** | 'en-US' |
| **Description** | Override global culture and localization value. |

**Supported Locales:** en-US, de-DE, fr-FR, ja-JP, ar-SA, zh-CN, etc.

**Use Case:** Display time in specific language/region format

```tsx
<TimePickerComponent locale="de-DE" />
```

---

### maskPlaceholder

| Property | Value |
|----------|-------|
| **Type** | TimeMaskPlaceholderModel |
| **Default** | `{ day: 'day', month: 'month', year: 'year', hour: 'hour', minute: 'minute', second: 'second', dayOfTheWeek: 'day of the week' }` |
| **Description** | Placeholder text shown in masked input mode. |

```typescript
interface TimeMaskPlaceholderModel {
  day?: string;
  month?: string;
  year?: string;
  hour?: string;
  minute?: string;
  second?: string;
  dayOfTheWeek?: string;
}
```

**Use Case:** Custom placeholder for masked input

```tsx
<TimePickerComponent 
  enableMask={true}
  maskPlaceholder={{ hour: 'HH', minute: 'MM', second: 'SS' }}
/>
```

---

### max

| Property | Value |
|----------|-------|
| **Type** | Date |
| **Default** | 00:00 |
| **Description** | Maximum selectable time value. |

**Use Case:** Restrict time selection to business hours (9 AM - 5 PM)

```tsx
<TimePickerComponent 
  min={new Date('1/1/2018 9:00 AM')}
  max={new Date('1/1/2018 5:00 PM')}
/>
```

---

### min

| Property | Value |
|----------|-------|
| **Type** | Date |
| **Default** | 00:00 |
| **Description** | Minimum selectable time value. |

**Use Case:** Set earliest available appointment time

```tsx
<TimePickerComponent 
  min={new Date('1/1/2018 9:00 AM')}
/>
```

---

### openOnFocus

| Property | Value |
|----------|-------|
| **Type** | boolean |
| **Default** | false |
| **Description** | Open popup when input receives focus (not just icon click). |

**Use Case:** Improve mobile UX by opening on tap/focus

```tsx
<TimePickerComponent openOnFocus={true} />
```

---

### placeholder

| Property | Value |
|----------|-------|
| **Type** | string |
| **Default** | null |
| **Description** | Hint text displayed in empty input. |

**Use Case:** Show expected format or instruction

```tsx
<TimePickerComponent placeholder="Select appointment time" />
```

---

### readonly

| Property | Value |
|----------|-------|
| **Type** | boolean |
| **Default** | false |
| **Description** | Set component to read-only state (no editing). |

**Use Case:** Display selected time but prevent changes

```tsx
<TimePickerComponent readonly={true} />
```

---

### scrollTo

| Property | Value |
|----------|-------|
| **Type** | Date |
| **Default** | null |
| **Description** | Initial scroll position in popup list when opened. |

**Use Case:** Scroll to typical time when popup opens (e.g., 12 PM for lunch)

```tsx
<TimePickerComponent scrollTo={new Date('1/1/2018 12:00 PM')} />
```

---

### serverTimezoneOffset

| Property | Value |
|----------|-------|
| **Type** | number |
| **Default** | null |
| **Description** | Server timezone offset for processing initial time value. |

**Use Case:** Handle server-side timezone conversion

```tsx
<TimePickerComponent serverTimezoneOffset={-300} /> // EST: UTC-5
```

---

### showClearButton

| Property | Value |
|----------|-------|
| **Type** | boolean |
| **Default** | true |
| **Description** | Show or hide the clear button in the input. |

**Use Case:** Allow users to clear selection or prevent clearing

```tsx
<TimePickerComponent showClearButton={true} />
```

---

### step

| Property | Value |
|----------|-------|
| **Type** | number |
| **Default** | 30 |
| **Description** | Time interval in minutes between popup list items. |

**Common Values:**
- 5 - Precise timing (medical, scientific)
- 15 - Appointments, quick booking
- 30 - General purposes (default)
- 60 - Hourly slots

**Use Case:** Define time slot granularity

```tsx
<TimePickerComponent step={15} />
```

---

### strictMode

| Property | Value |
|----------|-------|
| **Type** | boolean |
| **Default** | false |
| **Description** | Validate input and restrict to valid times only. Resets to previous value if invalid. |

**Use Case:** Prevent invalid time entry in business-critical apps

```tsx
<TimePickerComponent 
  strictMode={true}
  min={new Date('1/1/2018 9:00 AM')}
  max={new Date('1/1/2018 5:00 PM')}
/>
```

---

### value

| Property | Value |
|----------|-------|
| **Type** | Date |
| **Default** | null |
| **Description** | Current selected time value. Parsed based on culture format. |

**Use Case:** Set initial time and two-way binding

```tsx
<TimePickerComponent 
  value={new Date('1/1/2018 2:30 PM')}
  change={(e: any) => setTime(e.value)}
/>
```

---

### width

| Property | Value |
|----------|-------|
| **Type** | string \| number |
| **Default** | null |
| **Description** | Width of TimePicker component. Popup width matches component width. |

**Use Case:** Custom sizing for responsive layouts

```tsx
<TimePickerComponent width="300px" />
<TimePickerComponent width="100%" />
```

---

### zIndex

| Property | Value |
|----------|-------|
| **Type** | number |
| **Default** | 1000 |
| **Description** | Z-index value of the TimePicker popup element. |

**Use Case:** Ensure popup appears above other elements (modals, overlays)

```tsx
<TimePickerComponent zIndex={9999} />
```

---

## Methods

TimePicker provides **5 methods** for programmatic control via useRef.

### focusIn()

**Description:** Focus the TimePicker input element.

**Returns:** void

**Example:**
```tsx
const timePickerRef = useRef<TimePickerComponent>(null);

const handleFocus = () => {
  timePickerRef.current?.focusIn();
};
```

---

### focusOut()

**Description:** Remove focus from TimePicker input element.

**Returns:** void

**Example:**
```tsx
const timePickerRef = useRef<TimePickerComponent>(null);

const handleBlur = () => {
  timePickerRef.current?.focusOut();
};
```

---

### show()

**Description:** Open the TimePicker popup.

**Returns:** void

**Example:**
```tsx
const timePickerRef = useRef<TimePickerComponent>(null);

const handleOpenPopup = () => {
  timePickerRef.current?.show();
};
```

---

### hide()

**Description:** Close the TimePicker popup.

**Returns:** void

**Example:**
```tsx
const timePickerRef = useRef<TimePickerComponent>(null);

const handleClosePopup = () => {
  timePickerRef.current?.hide();
};
```

---

### getPersistData()

**Description:** Get properties that persist across page reloads when enablePersistence is true.

**Returns:** string (JSON serialized persistence data)

**Example:**
```tsx
const timePickerRef = useRef<TimePickerComponent>(null);

const handleGetPersistData = () => {
  const persistData = timePickerRef.current?.getPersistData();
  console.log('Persisted data:', persistData);
};
```

---

## Events

Complete list of **9 TimePicker events** with event arguments.

### blur

**Description:** Triggers when control loses focus.

**Event Args:** BlurEventArgs

```typescript
interface BlurEventArgs {
  value: Date;
  e: FocusEvent;
}
```

**Example:**
```tsx
const handleBlur = (args: any) => {
  console.log('TimePicker blurred, value:', args.value);
};

<TimePickerComponent blur={handleBlur} />
```

---

### change

**Description:** Triggers when selected time value changes.

**Event Args:** ChangeEventArgs

```typescript
interface ChangeEventArgs {
  value: Date;
  previousValue: Date;
  type: string;  // 'set' or 'change'
  e: Event;
}
```

**Example:**
```tsx
const handleChange = (args: any) => {
  console.log('New time:', args.value);
  console.log('Previous time:', args.previousValue);
};

<TimePickerComponent change={handleChange} />
```

---

### cleared

**Description:** Triggers when time value is cleared via clear button.

**Event Args:** ClearedEventArgs

**Example:**
```tsx
const handleCleared = (args: any) => {
  console.log('Time cleared');
};

<TimePickerComponent cleared={handleCleared} showClearButton={true} />
```

---

### close

**Description:** Triggers when popup closes.

**Event Args:** PopupEventArgs

```typescript
interface PopupEventArgs {
  popup: Popup;
  preventDefault(): void;
}
```

**Example:**
```tsx
const handleClose = (args: any) => {
  console.log('Popup closed');
};

<TimePickerComponent close={handleClose} />
```

---

### created

**Description:** Triggers when component is created and ready.

**Event Args:** Object

**Example:**
```tsx
const handleCreated = () => {
  console.log('TimePicker created');
};

<TimePickerComponent created={handleCreated} />
```

---

### destroyed

**Description:** Triggers when component is destroyed.

**Event Args:** Object

**Example:**
```tsx
const handleDestroyed = () => {
  console.log('TimePicker destroyed');
};

<TimePickerComponent destroyed={handleDestroyed} />
```

---

### focus

**Description:** Triggers when control gets focus.

**Event Args:** FocusEventArgs

```typescript
interface FocusEventArgs {
  value: Date;
  e: FocusEvent;
}
```

**Example:**
```tsx
const handleFocus = (args: any) => {
  console.log('TimePicker focused');
};

<TimePickerComponent focus={handleFocus} />
```

---

### itemRender

**Description:** Triggers while rendering each popup list item. Allows custom item formatting.

**Event Args:** ItemEventArgs

```typescript
interface ItemEventArgs {
  value: Date;
  element: HTMLElement;
  isSelected: boolean;
}
```

**Example:**
```tsx
const handleItemRender = (args: any) => {
  if (args.value.getHours() >= 9 && args.value.getHours() < 17) {
    args.element.style.backgroundColor = '#e8f5e9';
  }
};

<TimePickerComponent itemRender={handleItemRender} />
```

---

### open

**Description:** Triggers when popup opens.

**Event Args:** PopupEventArgs

```typescript
interface PopupEventArgs {
  popup: Popup;
  preventDefault(): void;
}
```

**Example:**
```tsx
const handleOpen = (args: any) => {
  console.log('Popup opened');
};

<TimePickerComponent open={handleOpen} />
```

---

## Interfaces

### TimeFormatObject

Used for specifying time format using skeleton property:

```typescript
interface TimeFormatObject {
  skeleton?: 'hm' | 'Hm' | 'hms' | 'Hms';
  // hm: 2:30 PM
  // Hm: 14:30
  // hms: 2:30:45 PM
  // Hms: 14:30:45
}
```

### TimeMaskPlaceholderModel

Used for configuring masked input placeholders:

```typescript
interface TimeMaskPlaceholderModel {
  day?: string;
  month?: string;
  year?: string;
  hour?: string;
  minute?: string;
  second?: string;
  dayOfTheWeek?: string;
}
```

---

## Type Definitions

### FloatLabelType

```typescript
type FloatLabelType = 'Never' | 'Always' | 'Auto';
```

### TimeFormat

```typescript
type TimeFormat = string | TimeFormatObject;
```

---

## Complete Property Table (26 Properties)

| Property | Type | Default | Accepts | Use Case |
|----------|------|---------|---------|----------|
| allowEdit | boolean | true | true/false | Control keyboard input |
| cssClass | string | null | Any string | Custom styling |
| enableMask | boolean | false | true/false | Guided format input |
| enablePersistence | boolean | false | true/false | Remember across reloads |
| enableRtl | boolean | false | true/false | RTL languages |
| enabled | boolean | true | true/false | Enable/disable |
| floatLabelType | string | Never | Never/Always/Auto | Label behavior |
| format | string/object | Culture-based | 'HH:mm', { skeleton: 'Hm' } | Time display format |
| fullScreenMode | boolean | false | true/false | Mobile full-screen |
| htmlAttributes | object | {} | { [key]: value } | DOM attributes |
| keyConfigs | object | null | { [action]: key } | Keyboard shortcuts |
| locale | string | en-US | en-US, de-DE, etc. | Language/region |
| maskPlaceholder | object | See default | { hour, minute, second } | Mask placeholders |
| max | Date | 00:00 | Any Date | Max selectable time |
| min | Date | 00:00 | Any Date | Min selectable time |
| openOnFocus | boolean | false | true/false | Open on focus |
| placeholder | string | null | Any string | Hint text |
| readonly | boolean | false | true/false | Read-only state |
| scrollTo | Date | null | Any Date | Scroll position |
| serverTimezoneOffset | number | null | Any number | Timezone offset |
| showClearButton | boolean | true | true/false | Show clear button |
| step | number | 30 | 5/15/30/60 | Time intervals |
| strictMode | boolean | false | true/false | Validation |
| value | Date | null | Any Date | Selected time |
| width | string/number | null | '300px', '100%' | Component width |
| zIndex | number | 1000 | Any number | Popup z-index |

## Related Documentation

- [Getting Started](getting-started.md)
- [Time Format and Display](time-format-and-display.md)
- [Time Range and Selection](time-range-and-selection.md)
- [Events and Methods](events-and-methods.md)
- [Customization and Styling](customization-and-styling.md)
