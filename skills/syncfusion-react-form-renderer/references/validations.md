# Validations

## Overview

Validation in the Form Renderer is declared directly on each field. The renderer checks rules on submit and, for common rules, also on change as the user types.

## Built-in validation rules

Common rules include:

- `required`
- `minLength`
- `maxLength`
- `pattern`
- `min`
- `max`

Example:

```json
{
  "properties": {
    "emailAddress": {
      "name": "emailAddress",
      "type": "string",
      "label": "Email Address",
      "widget": "textbox",
      "required": true,
      "textboxType": "email",
      "maxLength": 254
    },
    "age": {
      "name": "age",
      "type": "number",
      "label": "Age",
      "widget": "number",
      "required": true,
      "min": 18,
      "max": 120
    }
  }
}
```

## Custom validation

When built-in rules are not enough, use `customValidation`. Each entry is a rule object with an `expression`, and the expression must assign `valid` to either `true` or an error string.

Example:

```json
{
  "properties": {
    "confirmPassword": {
      "name": "confirmPassword",
      "type": "string",
      "label": "Confirm Password",
      "widget": "textbox",
      "customValidation": [
        {
          "expression": "valid = (input === {password}) ? true : 'Confirm password should match password'"
        }
      ]
    }
  }
}
```

## `customValidation` structure

The `customValidation` array contains one or more rule objects. Each rule can read:

- `input`: the current value of the field being validated
- `{name}`: any other field value in the form
- expression operators and functions, including math and dates

The field is only valid when every custom rule passes.

## Behavior on submit and change

The renderer runs validation in two ways:

- on submit: all required and visible fields are checked
- on change: common validation rules re-run as the user edits the form

A submit action raises a `submit` event with `args.data` and `args.isValid`.

## When to use built-in vs custom rules

Use built-in rules for straightforward field-level constraints such as required fields or text lengths. Use custom validation whenever the rule depends on another form value or requires a custom message.

## Key guidance

Combine built-in rules and custom validation when needed. They are additive, and the field must satisfy every rule before the form is valid.
