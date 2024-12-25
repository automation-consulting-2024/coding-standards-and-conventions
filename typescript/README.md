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
