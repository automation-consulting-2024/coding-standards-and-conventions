# Typescript Conventions

## UPPER_CASE_FOR_CONSTANTS

const CONSTANT = 'this-value-doesn't-change'

## No single line return

Do **NOT** do this

```typescript
if (condition) return true;
```

Do this instead

```typescript
if (condition) {
  return true;
}
```

## Error logging

When logging errors, make sure that the following are logged:

1. A hard-coded message, i.e., no string interpolation or dynamic message
2. A reason, usually this is the message from the error object
3. All non-sensitive arguments that are passed into the function
4. A unique identifier such as user UUID (if available)
5. Trace log of original error

For example:

```typescript
console.error({
  message: 'A hard-coded message that should be unique',
  reason: (error as Error).message,
  stack: (error as Error).stack,
  ...nonSensitiveFunctionArguments,
  ...otherImportantNonSensitiveArguments,
});
```

Do **NOT**:

1. have dynamic message
2. logs sensitive arguments or data

## Prefering Pure-Functions

- Pure Functions are functions that have no side effects and always produce the same output for the same input, without modifying any external state

```js
// This is a pure function
const add = (a, b) => {
  return a + b;
};

// This is an un-pure function
const MULTIPLY_BY_DEFAULT = 2

const multiplyBy (a, b) => {
  return (a+b) * MULTIPLY_BY_DEFAULT
}

```
## Use named constants instead of hardcoded numbers:

```typescript
// Bad ❌
if (lastPage.data.length < 10) {
  return undefined;
}

// Good ✅
if (lastPage.data.length < APP_CONFIG.DEFAULT_PAGE_SIZE) {
  return undefined;
}

```
## Strict Null Checks 

```typescript
// Good ✅
const getName = (user: User | null): string => {
  return user?.name ?? 'Anonymous';
};

// Bad ❌
const getName = (user: User): string => {
  return user!.name; // Dangerous!
}
```

## Use declarative approach for the following conditions 
- Complex data transformations
- Business calculations
- Data filtering and mapping
- State updates
- Data aggregation

```typescript 
// Declarative approach✅
const activeUsers = users
  .filter(user => user.isActive)
  .map(user => ({
    name: user.name,
    email: user.email
  }));

// Imperative Approach ❌
const activeUsers = [];
for (let i = 0; i < users.length; i++) {
  if (users[i].isActive) {
    activeUsers.push({
      name: users[i].name,
      email: users[i].email
    });
  }
}

```

```typescript
// Declarative ✅ Good Example
const calculateOrderTotal = (order: Order): number => {
  const subtotal = order.items
    .map(item => item.price * item.quantity)
    .reduce((sum, price) => sum + price, 0);
    
  const withTax = subtotal * (1 + TAX_RATE);
  const withDiscount = applyDiscount(withTax, order.discountCode);
  
  return withDiscount;
};

// Imperative Approach ❌ Bad Example
const calculateOrderTotal = (order: Order): number => {
  let subtotal = 0;
  for (let i = 0; i < order.items.length; i++) {
    subtotal += order.items[i].price * order.items[i].quantity;
  }
  
  let withTax = subtotal + (subtotal * TAX_RATE);
  let total = withTax;
  
  if (order.discountCode) {
    total = applyDiscount(total, order.discountCode);
  }
  
  return total;
};
```

