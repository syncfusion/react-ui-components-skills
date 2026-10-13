# Expressions

## Table of Contents
- [Overview](#overview)
- [Basic Example](#basic-example)
- [Expression Syntax](#expression-syntax)
- [Operators](#operators)
- [Constants](#constants)
- [Available Functions](#available-functions)
- [Common Patterns](#common-patterns)
- [Key Guidance](#key-guidance)

## Overview

Expressions let a field derive its value from other fields instead of requiring manual entry. Add an `expressionValue` string to the target field in `properties`. Whenever a referenced field changes, the renderer reevaluates the expression and updates the target field, avoiding hand-written change handlers.

## Basic example

```json
{
  "properties": {
    "firstName": {
      "name": "firstName",
      "type": "string",
      "label": "First Name",
      "widget": "textbox"
    },
    "lastName": {
      "name": "lastName",
      "type": "string",
      "label": "Last Name",
      "widget": "textbox"
    },
    "name": {
      "name": "name",
      "type": "string",
      "label": "Name",
      "widget": "textbox",
      "expressionValue": "{firstName} + \" \" + {lastName}"
    }
  }
}
```

When `firstName` or `lastName` changes, the `name` field is recomputed. String `+` concatenates the values with a space.

## Expression syntax

Expressions support field references, array indexing, grouping, ternary expressions, and boolean combinations:

| Form | Syntax | Example |
| --- | --- | --- |
| Field reference | `{fieldName}` | `{firstName}` |
| Array index | `{fieldName}[index]` | `{multiselect_1}[0]` |
| Grouping | `(expression)` | `({qty} * {price}) + {tax}` |
| Ternary | `condition ? valueTrue : valueFalse` | `{age} >= 18 ? "Adult" : "Minor"` |
| Boolean combination | `condition1 && condition2` | `{age} >= 18 && {age} <= 60` |

Use field `name` values inside braces, not display labels.

## Operators

### Arithmetic operators

When either operand of `+` is a string, it concatenates rather than adding numerically.

| Operator | Operation | Example | Result |
| --- | --- | --- | --- |
| `+` | Addition or concatenation | `{subtotal} + {tax}`; `{firstName} + " " + {lastName}` | number or string |
| `-` | Subtraction | `{total} - {discount}` | number |
| `*` | Multiplication | `{quantity} * {price}` | number |
| `/` | Division | `{total} / {count}` | number |
| `%` | Modulo | `{value} % 2` | number |
| `^` | Exponent | `{base} ^ {exponent}` | number |

### Comparison operators

Comparison expressions return a boolean. `===` and `!==` compare both value and type; `==` and `!=` compare value only.

| Operator | Meaning | Example |
| --- | --- | --- |
| `==` | Equals | `{value} == 10` |
| `!=` | Not equals | `{value} != 10` |
| `===` | Strict equals | `{value} === 10` |
| `!==` | Strict not equals | `{value} !== "10"` |
| `<` | Less than | `{age} < 18` |
| `>` | Greater than | `{age} > 18` |
| `<=` | Less than or equal | `{value} <= 100` |
| `>=` | Greater than or equal | `{value} >= 50` |

### Logical operators

| Operator | Meaning | Example |
| --- | --- | --- |
| `&&` | AND: both conditions must be true | `{age} >= 18 && {age} <= 60` |
| `||` | OR: either condition can be true | `{isMember} == TRUE || {isGuest} == TRUE` |
| `!` | NOT: invert a condition | `!{isDeleted}` |

## Constants

Constants are reserved tokens recognized by the expression engine. The `Math` object is also exposed, including methods such as `Math.sqrt`, `Math.round`, and `Math.max`.

| Constant | Type | Meaning | Example |
| --- | --- | --- | --- |
| `PI` | number | Pi | `{radius} * PI` |
| `TRUE` | boolean | `true` | `{enabled} == TRUE` |
| `FALSE` | boolean | `false` | `{disabled} === FALSE` |
| `NULL` | null | `null` | `{value} == NULL` |
| `UNDEFINED` | undefined | `undefined` | `{value} != UNDEFINED` |
| `Math` | object | JavaScript Math object | `Math.sqrt({number})` |

## Available Functions

Function names are case-sensitive and must be uppercase as shown. Use aggregate functions for arrays, math functions for numeric values, string functions for text, and date functions for date calculations or formatting.

### Aggregate functions

Aggregate functions accept an array and return one value.

| Function | Syntax | Parameters | Returns | Example |
| --- | --- | --- | --- | --- |
| `SUM` | `SUM(array)` | `number[]` | number | `SUM([{numA}, {numB}])` |
| `AVG` | `AVG(array)` | `number[]` | number | `AVG([{numA}, {numB}])` |
| `MIN` | `MIN(array)` | `number[]` | number | `MIN([{numA}, {numB}])` |
| `MAX` | `MAX(array)` | `number[]` | number | `MAX([{numA}, {numB}])` |
| `COUNT` | `COUNT(array)` | `any[]` | number | `COUNT([{itemA}, {itemB}])` |

### Math functions

`ROUND` accepts an optional number of decimal places. The other listed functions take one numeric value.

| Function | Syntax | Returns | Example |
| --- | --- | --- | --- |
| `ROUND` | `ROUND(value, decimals?)` | number | `ROUND({price}, 2)` |
| `FLOOR` | `FLOOR(value)` | number | `FLOOR({value})` |
| `CEIL` | `CEIL(value)` | number | `CEIL({value})` |
| `ABS` | `ABS(value)` | number | `ABS({difference})` |
| `SQRT` | `SQRT(value)` | number | `SQRT({area})` |

### String functions

`CONCAT` accepts two or more strings. `SUBSTRING` takes a starting index and an optional length.

| Function | Syntax | Returns | Example |
| --- | --- | --- | --- |
| `UPPER` | `UPPER(text)` | string | `UPPER({name})` |
| `LOWER` | `LOWER(text)` | string | `LOWER({name})` |
| `CONCAT` | `CONCAT(str1, str2, ...)` | string | `CONCAT({firstName}, " ", {lastName})` |
| `LENGTH` | `LENGTH(text)` | number | `LENGTH({name})` |
| `TRIM` | `TRIM(text)` | string | `TRIM({name})` |
| `SUBSTRING` | `SUBSTRING(text, start, length?)` | string | `SUBSTRING({name}, 0, 3)` |

### Date functions

`TODAY` takes no arguments. `AGE` returns years, `DATEDIFF` returns days, and the formatting/addition/subtraction functions return date strings.

| Function | Syntax | Returns | Example |
| --- | --- | --- | --- |
| `TODAY` | `TODAY()` | string (date) | `TODAY()` |
| `AGE` | `AGE(birthDate)` | number (years) | `AGE({birthDate})` |
| `DATEFORMAT` | `DATEFORMAT(date, format)` | string | `DATEFORMAT({birthDate}, "yyyy-MM-dd")` |
| `DATEDIFF` | `DATEDIFF(date1, date2)` | number (days) | `DATEDIFF({startDate}, {endDate})` |
| `ADDDAYS` | `ADDDAYS(date, days)` | string (date) | `ADDDAYS({startDate}, 5)` |
| `SUBDAYS` | `SUBDAYS(date, days)` | string (date) | `SUBDAYS({endDate}, 5)` |

## Common patterns

```json
{
  "properties": {
    "quantity": { "name": "quantity", "type": "number", "widget": "number" },
    "unitPrice": { "name": "unitPrice", "type": "number", "widget": "number" },
    "total": {
      "name": "total",
      "type": "number",
      "widget": "number",
      "expressionValue": "{quantity} * {unitPrice}"
    },
    "displayName": {
      "name": "displayName",
      "type": "string",
      "widget": "textbox",
      "expressionValue": "CONCAT(TRIM({firstName}), \" \", UPPER({lastName}))"
    }
  }
}
```

Use aggregation for totals, ternaries for derived labels, and date helpers for date calculations. Ensure every `{name}` reference matches a field name in the same form schema.
