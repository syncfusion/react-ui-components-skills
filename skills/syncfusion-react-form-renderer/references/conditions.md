# Conditional Rules

## Table of Contents
- [Overview](#overview)
- [Rule Structure](#rule-structure)
- [Conditional Types](#conditional-types)
- [Operators](#operators)
- [Visibility and Hide](#visibility-and-hide)
- [Required, Read-Only, and Disabled](#required-read-only-and-disabled)
- [Set Value When](#set-value-when)
- [Choice-Based Field](#choice-based-field)
- [Conditional Support by Component](#conditional-support-by-component)
- [Key Guidance](#key-guidance)

## Overview

Conditional rules let a form respond to user input without hand-written change handlers. Put rules in the target field's `conditions` object. Rules compare the current value of a source field, identified by its `id`, then control visibility, validation requirement, read-only/disabled state, assigned values, or dropdown options.

## Rule structure

Each rule is declared under one of the supported keys in a field's `conditions` object. A condition can be a single leaf rule or a logical group. Groups use `condition` with `and` or `or` and contain a `rules` array; nested groups may be mixed with leaf rules.

| Member | Purpose |
| --- | --- |
| `condition` | For a group, choose `and` (every rule must match) or `or` (at least one rule must match). Required when providing a group. |
| `rules` | Leaf rules or nested condition groups to evaluate. |
| `label` | Display name of the source field for the rule. |
| `field` | The `id` of the source field whose value is checked. |
| `operator` | Comparison applied to the source value. |
| `type` | Source value type: `string`, `number`, `date`, or `boolean`. |
| `value` | Comparison value. Use `null` for `isempty` and `isnotempty`, which do not need a comparison value. |

The following shape combines two checks. Nested groups can express more complex logic such as `(A and B) or (C and D)`:

```json
{
  "properties": {
    "password": {
      "name": "password",
      "type": "string",
      "label": "Password",
      "widget": "textbox"
    },
    "confirmPassword": {
      "name": "confirmPassword",
      "type": "string",
      "label": "Confirm Password",
      "widget": "textbox",
      "conditions": {
        "visibleWhen": {
          "condition": "and",
          "rules": [
            {
              "label": "Password",
              "field": "textbox_1",
              "operator": "isnotempty",
              "type": "string",
              "value": null
            }
          ]
        }
      }
    }
  }
}
```

## Conditional types

The Form Renderer supports these condition keys. Support depends on the field widget; consult the matrix below before applying a rule.

- `visibleWhen`: render the field only while the condition matches.
- `hideWhen`: render the field by default and hide it while the condition matches.
- `requiredWhen`: make the field required only while the condition matches.
- `readOnlyWhen`: keep the value visible and included in submitted data, but prevent editing.
- `disabledWhen`: render the field as non-interactive and visually disabled while the condition matches.
- `setValueWhen`: assign a configured value while the condition matches; the value is reapplied as source fields change while the condition remains true.
- `choiceBasedField`: filter a dropdown's options based on another field's value.

## Operators

Operators compare the source field's current value with the rule's `value`. Date comparisons use parsed `Date` values.

| Operator | Meaning | Needs `value` | Supported data |
| --- | --- | :---: | --- |
| `isempty` | Source value is empty. | No | All |
| `isnotempty` | Source value is not empty. | No | All |
| `equal` | Source value equals the comparison value. | Yes | All |
| `notequal` | Source value differs from the comparison value. | Yes | All |
| `contains` | Source string contains the comparison substring. | Yes | String |
| `notcontains` | Source string does not contain the comparison substring. | Yes | String |
| `startswith` | Source string begins with the comparison string. | Yes | String |
| `doesnotstartswith` | Source string does not begin with the comparison string. | Yes | String |
| `greaterthan` | Source value is greater than the comparison value. | Yes | Number, Date |
| `lessthan` | Source value is less than the comparison value. | Yes | Number, Date |
| `greaterthanorequal` | Source value is greater than or equal to the comparison value. | Yes | Number, Date |
| `lessthanorequal` | Source value is less than or equal to the comparison value. | Yes | Number, Date |

## Visibility and hide

`visibleWhen` and `hideWhen` are complementary ways to express visibility. When a field is not rendered because of these rules, its value is excluded from the submitted data.

- Choose `visibleWhen` when the field should appear only after a condition becomes true.
- Choose `hideWhen` when the field should appear by default and disappear in a specific case.

Example: show a confirmation field only after the password field is non-empty.

```json
{
  "confirmPassword": {
    "id": "textbox_confirm",
    "name": "confirmPassword",
    "type": "string",
    "label": "Confirm Password",
    "widget": "textbox",
    "conditions": {
      "visibleWhen": {
        "condition": "and",
        "rules": [
          {
            "label": "Password",
            "field": "textbox_password",
            "operator": "isnotempty",
            "type": "string",
            "value": null
          }
        ]
      }
    }
  }
}
```

To express the inverse, use `hideWhen` with `isempty` on the password field. Select whichever form reads more clearly for the business rule.

## Required, read-only, and disabled

`requiredWhen` changes validation without removing the field from the layout. The field is required only when its condition matches; when it does not match, the conditional requirement no longer applies.

```json
{
  "lastName": {
    "id": "textbox_lastName",
    "name": "lastName",
    "type": "string",
    "label": "Last Name",
    "widget": "textbox",
    "conditions": {
      "requiredWhen": {
        "condition": "and",
        "rules": [
          {
            "label": "First Name",
            "field": "textbox_firstName",
            "operator": "startswith",
            "type": "string",
            "value": "A"
          }
        ]
      }
    }
  }
}
```

`readOnlyWhen` lets the user see the value and keeps it in submitted data while blocking edits. `disabledWhen` instead places the control in a disabled, non-interactive state.

```json
{
  "lastName": {
    "id": "textbox_lastName",
    "name": "lastName",
    "type": "string",
    "label": "Last Name",
    "widget": "textbox",
    "conditions": {
      "readOnlyWhen": {
        "condition": "and",
        "rules": [
          {
            "label": "First Name",
            "field": "textbox_firstName",
            "operator": "contains",
            "type": "string",
            "value": "Guest"
          }
        ]
      }
    }
  }
}
```

Replace `readOnlyWhen` with `disabledWhen` when the desired behavior is a disabled control rather than a read-only value. Check component support: some widgets do not support both behaviors.

## Set Value When

`setValueWhen` extends the condition with a `value` to assign. The renderer reapplies the value when source fields change for as long as the condition remains true.

```json
{
  "lastName": {
    "id": "textbox_lastName",
    "name": "lastName",
    "type": "string",
    "label": "Last Name",
    "widget": "textbox",
    "conditions": {
      "setValueWhen": {
        "condition": {
          "condition": "and",
          "rules": [
            {
              "label": "First Name",
              "field": "textbox_firstName",
              "operator": "doesnotstartswith",
              "type": "string",
              "value": "K"
            },
            {
              "label": "First Name",
              "field": "textbox_firstName",
              "operator": "doesnotstartswith",
              "type": "string",
              "value": "B"
            }
          ]
        },
        "value": "Guest"
      }
    }
  }
}
```

Here `lastName` is set to `Guest` when both checks pass. The assigned value is not an expression; use `expressionValue` for computed values.

## Choice-Based Field

`choiceBasedField` is for dependent dropdown options. Its configuration identifies the parent with `primaryFieldId`, maps parent values to child choices with `choiceMapping`, and controls unmapped values with `showAllWhenNotMapped`. The parent reference uses its field `id`. See the dependent dropdown reference for a full cascading schema.

## Practical use cases

Typical examples include:

- show a confirmation field only after a password is entered
- require a shipping address only when the user picks a delivery option
- lock a calculated field so the user cannot edit it manually
- populate a discount field when a product category is selected

## Conditional support by component

Use this matrix before assigning a condition. A `-` means that condition is unsupported for the component; the renderer ignores it. `No` is retained where the source explicitly reports that value.

| Component | Visibility | Hide | Required | Read-only | Disable | Set value | Choice-based field |
| --- | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| Textbox | Yes | Yes | Yes | Yes | Yes | Yes | - |
| Text Area | Yes | Yes | Yes | Yes | Yes | Yes | - |
| Numeric Textbox | Yes | Yes | Yes | Yes | Yes | Yes | - |
| Masked Input | Yes | Yes | Yes | Yes | Yes | Yes | - |
| Multiselect | Yes | Yes | Yes | Yes | Yes | Yes | - |
| Date Picker | Yes | Yes | Yes | Yes | Yes | Yes | - |
| DateTime Picker | Yes | Yes | Yes | Yes | Yes | Yes | - |
| Time Picker | Yes | Yes | Yes | Yes | Yes | Yes | - |
| Date Range Picker | Yes | Yes | Yes | Yes | Yes | Yes | - |
| Checkbox | Yes | Yes | - | - | Yes | - | - |
| Button | Yes | Yes | - | - | Yes | - | - |
| Switch | Yes | Yes | - | - | Yes | - | - |
| Split button | Yes | Yes | - | - | Yes | - | - |
| Image Editor | Yes | Yes | - | - | Yes | - | - |
| File Uploader | Yes | Yes | - | - | Yes | - | - |
| Color Picker | Yes | Yes | - | - | Yes | - | - |
| Radio button | Yes | Yes | Yes | No | Yes | Yes | - |
| Checkbox group | Yes | Yes | Yes | No | Yes | Yes | - |
| Dropdown list | Yes | Yes | Yes | Yes | Yes | Yes | Yes |
| Rating | Yes | Yes | Yes | Yes | No | Yes | - |
| Range Slider | Yes | Yes | No | Yes | Yes | Yes | - |
| Signature | Yes | Yes | - | - | - | - | - |
| Message | Yes | Yes | - | - | - | - | - |
| Panel | Yes | Yes | - | - | - | - | - |
| Table | Yes | Yes | - | - | - | - | - |
| Tabs | Yes | Yes | - | - | - | - | - |
| Card | Yes | Yes | - | - | - | - | - |
| HTML | Yes | Yes | - | - | - | - | - |
| Rich Text Editor | Yes | Yes | - | Yes | Yes | - | - |
| Data Grid | Yes | Yes | - | - | - | - | - |

## Key guidance

Use conditional rules to keep form behavior declarative and close to the field it affects. Check the support matrix before applying a condition, reference source fields by `id`, and use nested `and` / `or` groups when a rule depends on multiple comparisons.
