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
