# Validation in React Grid

## Table of Contents
- [Overview](#overview)
- [Built-in Validators](#built-in-validators)
- [Validation Rule Definition](#validation-rule-definition)
- [Custom Validation](#custom-validation)
- [Async Validation](#async-validation)
- [Error Display](#error-display)
- [Validation Events](#validation-events)
- [Server-Side Validation](#server-side-validation)

## Overview

Validation ensures data quality and consistency before saving to the database. Syncfusion Grid supports:
- **Built-in validators** (required, min, max, pattern)
- **Custom validation functions**
- **Async validation** (server-side checks)
- **Error messages and styling**
- **Validation events** for hooks

---

## Built-in Validators

### Required Field Validation

```tsx
import { GridComponent, ColumnsDirective, ColumnDirective, Inject, Edit, Toolbar } from '@syncfusion/ej2-react-grids';

<GridComponent
  dataSource={data}
  editSettings={{ mode: 'Dialog', allowEditing: true }}
  toolbar={['Edit', 'Update', 'Cancel']}
>
  <ColumnsDirective>
    <ColumnDirective
      field='OrderID'
      headerText='Order ID'
      width='100'
      isPrimaryKey={true}
      editParams={{ params: { required: true } }}
    />
    <ColumnDirective
      field='CustomerID'
      headerText='Customer ID'
      width='120'
      editParams={{ params: { required: true } }}
    />
  </ColumnsDirective>
  <Inject services={[Edit, Toolbar]} />
</GridComponent>
```

### Number Range Validation

```tsx
<ColumnDirective
  field='Freight'
  headerText='Freight'
  width='100'
  format='C2'
  editParams={{
    params: {
      required: true,
      min: 0,
      max: 1000
    }
  }}
/>
```

### Date Validation

```tsx
<ColumnDirective
  field='OrderDate'
  headerText='Order Date'
  type='date'
  format='yMd'
  editParams={{
    params: {
      required: true,
      min: new Date(2020, 0, 1),
      max: new Date()
    }
  }}
/>
```

### Pattern/Regex Validation

```tsx
<ColumnDirective
  field='Email'
  headerText='Email'
  editParams={{
    params: {
      required: true,
      pattern: /[A-Z0-9a-z._%+-]+@[A-Za-z0-9.-]+.[A-Za-z]{2,4}/
    }
  }}
/>
```

---

## Validation Rule Definition

### Define Validation Rules on Column

```tsx
const orderIDRules = {
  required: { message: 'Order ID is required' },
  min: { value: 1, message: 'Order ID must be at least 1' }
};

const freightRules = {
  required: { message: 'Freight is required' },
  min: { value: 0, message: 'Freight cannot be negative' },
  max: { value: 5000, message: 'Freight cannot exceed 5000' }
};

<GridComponent dataSource={data}>
  <ColumnsDirective>
    <ColumnDirective
      field='OrderID'
      headerText='Order ID'
      editParams={{ params: orderIDRules }}
    />
    <ColumnDirective
      field='Freight'
      headerText='Freight'
      editParams={{ params: freightRules }}
    />
  </ColumnsDirective>
  <Inject services={[Edit]} />
</GridComponent>
```

### Validation Object Structure

```tsx
const validationRules = {
  required: true | { message: 'Custom message' },
  min: number | { value: number, message: 'Custom message' },
  max: number | { value: number, message: 'Custom message' },
  minLength: number | { value: number, message: 'Custom message' },
  maxLength: number | { value: number, message: 'Custom message' },
  pattern: RegExp | { pattern: RegExp, message: 'Custom message' },
  custom: validationFunction
};
```

---

## Custom Validation

### Simple Custom Validation

```tsx
const validateEmail = (value) => {
  const regex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
  return regex.test(value);
};

<ColumnDirective
  field='Email'
  headerText='Email'
  editParams={{
    params: {
      required: true,
      custom: {
        validator: validateEmail,
        message: 'Invalid email format'
      }
    }
  }}
/>
```

### Custom Validation with Context

```tsx
const validateFreightMinimum = (args) => {
  if (args.value < 10) {
    args.status = false;
    args.statusMessage = 'Freight must be at least $10';
  } else {
    args.status = true;
  }
};

<GridComponent
  dataSource={data}
  editSettings={{ mode: 'Dialog' }}
  actionBegin={(args) => {
    if (args.requestType === 'save') {
      validateFreightMinimum(args);
    }
  }}
>
  {/* columns */}
  <Inject services={[Edit, Toolbar]} />
</GridComponent>
```

### Multi-Field Validation

```tsx
const validateDateRange = (args) => {
  if (args.data.EndDate < args.data.StartDate) {
    args.status = false;
    args.statusMessage = 'End date must be after start date';
  }
};

<GridComponent
  dataSource={data}
  editSettings={{ mode: 'Dialog' }}
  actionBegin={(args) => {
    if (args.requestType === 'save') {
      validateDateRange(args);
    }
  }}
/>
```

### Validation with Rules State

```tsx
import React, { useState } from 'react';

function GridWithValidation() {
  const [validationRules, setValidationRules] = useState({
    minFreight: 10,
    maxFreight: 5000
  });

  const validateFreight = (value) => {
    return value >= validationRules.minFreight && value <= validationRules.maxFreight;
  };

  const updateRules = (newRules) => {
    setValidationRules({ ...validationRules, ...newRules });
  };

  return (
    <div>
      <button onClick={() => updateRules({ minFreight: 50 })}>
        Set minimum freight to $50
      </button>

      <GridComponent dataSource={data}>
        <ColumnsDirective>
          <ColumnDirective
            field='Freight'
            editParams={{
              params: {
                custom: {
                  validator: validateFreight,
                  message: `Freight must be between $${validationRules.minFreight} and $${validationRules.maxFreight}`
                }
              }
            }}
          />
        </ColumnsDirective>
        <Inject services={[Edit]} />
      </GridComponent>
    </div>
  );
}

export default GridWithValidation;
```

---

## Async Validation

### Server-Side Unique Check

```tsx
const validateUniqueCustomerID = async (value) => {
  const response = await fetch(`/api/customers/exists/${value}`);
  const data = await response.json();
  return !data.exists;  // true if unique
};

<ColumnDirective
  field='CustomerID'
  headerText='Customer ID'
  editParams={{
    params: {
      required: true,
      custom: {
        validator: validateUniqueCustomerID,
        message: 'Customer ID already exists'
      }
    }
  }}
/>
```

### Async Validation with Promise

```tsx
const validateOrderNumber = (value) => {
  return new Promise((resolve) => {
    setTimeout(() => {
      // Simulate server check
      const isValid = value.length >= 3;
      resolve(isValid);
    }, 500);
  });
};

<GridComponent
  dataSource={data}
  editSettings={{ mode: 'Dialog' }}
  actionBegin={async (args) => {
    if (args.requestType === 'save') {
      const isValid = await validateOrderNumber(args.data.OrderNumber);
      if (!isValid) {
        args.status = false;
        args.statusMessage = 'Order number must be at least 3 characters';
      }
    }
  }}
/>
```

### Debounced Async Validation

```tsx
import { useCallback, useRef } from 'react';

const createDebouncedValidator = (validationFn, delay = 500) => {
  const timeoutRef = useRef(null);

  return (value) => {
    return new Promise((resolve) => {
      clearTimeout(timeoutRef.current);
      timeoutRef.current = setTimeout(async () => {
        const result = await validationFn(value);
        resolve(result);
      }, delay);
    });
  };
};

const validateEmail = async (email) => {
  const response = await fetch(`/api/email/validate?email=${email}`);
  const data = await response.json();
  return data.isValid;
};

const debouncedValidation = createDebouncedValidator(validateEmail);

<ColumnDirective
  field='Email'
  editParams={{
    params: {
      custom: {
        validator: debouncedValidation,
        message: 'Email is invalid or already registered'
      }
    }
  }}
/>
```

---

## Error Display

### Style Validation Errors

```tsx
const validationErrorStyles = `
  .e-grid .e-gridcontent .e-invalid {
    border: 2px solid #FF6B6B !important;
    background-color: #FFE5E5;
  }

  .e-grid .e-form-group.e-error .e-float-text {
    color: #FF6B6B;
    font-weight: bold;
  }

  .e-grid .e-errorinline {
    color: #FF6B6B;
    font-size: 12px;
    margin-top: 4px;
  }
`;

<style>{validationErrorStyles}</style>
<GridComponent dataSource={data}>
  {/* columns */}
</GridComponent>
```

### Custom Error Message Display

```tsx
const gridRef = useRef(null);

const handleValidationError = (args) => {
  if (!args.status) {
    console.error(`Validation Error: ${args.statusMessage}`);
    
    // Show toast notification
    showNotification({
      type: 'error',
      message: args.statusMessage,
      duration: 5000
    });
  }
};

<GridComponent
  ref={gridRef}
  dataSource={data}
  editSettings={{ mode: 'Dialog' }}
  actionFailure={handleValidationError}
>
  {/* columns */}
  <Inject services={[Edit, Toolbar]} />
</GridComponent>
```

### Tooltip Error Messages

```tsx
const validationErrorTooltip = `
  .e-grid .e-invalid::after {
    content: attr(title);
    position: absolute;
    background-color: #FF6B6B;
    color: white;
    padding: 4px 8px;
    border-radius: 4px;
    font-size: 12px;
    white-space: nowrap;
    z-index: 1000;
  }
`;

<GridComponent dataSource={data}>
  <ColumnsDirective>
    <ColumnDirective
      field='Email'
      title='Must be valid email'
      editParams={{
        params: {
          pattern: /^[^\s@]+@[^\s@]+\.[^\s@]+$/
        }
      }}
    />
  </ColumnsDirective>
</GridComponent>
```

---

## Validation Events

### Validate Before Save

```tsx
const gridRef = useRef(null);

const handleActionBegin = (args) => {
  if (args.requestType === 'save') {
    if (!args.data.OrderID) {
      args.cancel = true;
      showError('Order ID is required');
    }
    
    if (args.data.Freight < 0) {
      args.cancel = true;
      showError('Freight cannot be negative');
    }
    
    if (args.data.CustomerID.length < 2) {
      args.cancel = true;
      showError('Customer ID must be at least 2 characters');
    }
  }
};

<GridComponent
  ref={gridRef}
  dataSource={data}
  actionBegin={handleActionBegin}
>
  {/* columns */}
  <Inject services={[Edit, Toolbar]} />
</GridComponent>
```

### Validate After Edit

```tsx
const handleActionComplete = (args) => {
  if (args.requestType === 'save') {
    console.log('Record saved successfully');
  }
  
  if (args.requestType === 'cancel') {
    console.log('Edit cancelled');
  }
};

<GridComponent
  dataSource={data}
  actionComplete={handleActionComplete}
>
  {/* columns */}
</GridComponent>
```

### Real-time Field Validation

```tsx
const handleEdit = (args) => {
  if (args.requestType === 'beginEdit') {
    // Get input elements
    const inputs = document.querySelectorAll('.e-gridcontent input');
    
    inputs.forEach(input => {
      input.addEventListener('change', (e) => {
        validateField(e.target.value, e.target.name);
      });
    });
  }
};

const validateField = (value, fieldName) => {
  let isValid = true;
  let message = '';

  if (fieldName === 'OrderID' && !value) {
    isValid = false;
    message = 'Order ID is required';
  } else if (fieldName === 'Freight' && value < 0) {
    isValid = false;
    message = 'Freight must be positive';
  }

  updateValidationStatus(fieldName, isValid, message);
};

<GridComponent actionBegin={handleEdit}>
  {/* columns */}
</GridComponent>
```

---

## Server-Side Validation

### Send Data for Server Validation

```tsx
const validateAndSaveToServer = async (data) => {
  try {
    const response = await fetch('/api/orders/validate', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify(data)
    });

    const result = await response.json();
    
    if (!result.isValid) {
      return {
        isValid: false,
        errors: result.errors
      };
    }

    // If valid, save
    const saveResponse = await fetch('/api/orders', {
      method: 'POST',
      body: JSON.stringify(data)
    });

    return { isValid: true };
  } catch (error) {
    return {
      isValid: false,
      error: error.message
    };
  }
};

const handleSave = async (args) => {
  if (args.requestType === 'save') {
    const validation = await validateAndSaveToServer(args.data);
    
    if (!validation.isValid) {
      args.cancel = true;
      showErrors(validation.errors || validation.error);
    }
  }
};

<GridComponent actionBegin={handleSave}>
  {/* columns */}
</GridComponent>
```

### ASP.NET Validation Endpoint

```csharp
[HttpPost("validate")]
public IActionResult ValidateOrder([FromBody] OrderRequest request)
{
    var errors = new List<string>();

    if (string.IsNullOrEmpty(request.OrderID))
        errors.Add("Order ID is required");

    if (request.Freight < 0 || request.Freight > 5000)
        errors.Add("Freight must be between 0 and 5000");

    if (string.IsNullOrEmpty(request.CustomerID))
        errors.Add("Customer ID is required");

    // Check for duplicates
    if (OrderExists(request.OrderID))
        errors.Add("Order ID already exists");

    return Ok(new {
        isValid = errors.Count == 0,
        errors = errors
    });
}
```

---

## Validation Checklist

- [ ] Mark required fields clearly
- [ ] Validate data type (number, date, etc.)
- [ ] Set min/max ranges for numeric fields
- [ ] Validate email format
- [ ] Check for unique values (customer ID, email)
- [ ] Validate date logic (start < end)
- [ ] Show clear error messages
- [ ] Style invalid fields distinctly
- [ ] Prevent save if validation fails
- [ ] Test with edge cases
- [ ] Test with special characters
- [ ] Test async validators
- [ ] Provide user feedback on progress
- [ ] Implement server-side validation
- [ ] Log validation errors for debugging
