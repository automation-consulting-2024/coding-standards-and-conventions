# Typescript Conventions

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

For example:

```typescript
console.error({
  message: "A hard-coded message that should be unique",
  reason: (error as Error).message,
  ...nonSensitiveFunctionArguments,
  ...otherImportantNonSensitiveArguments,
});
```

Do **NOT**:

1. have dynamic message
2. logs sensitive arguments or data
