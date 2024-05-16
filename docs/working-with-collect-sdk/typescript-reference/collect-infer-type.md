---
sidebar_position: 7
---

# CollectInferType

The `CollectInferType` type is used to infer the types of schema fields for a given `CollectModel`. This utility type helps in deriving the correct types for fields defined in the schema, ensuring type safety and proper validation.

### Type Definition
```typescript
type CollectInferType<T extends CollectModel<any> = CollectModel<any>> = FlattenTypes<
  InferTypesFromSchema<T['schema']>
>;
```

### Properties

The `CollectInferType` does not have explicit properties since it is used to infer types from a schema.

### Example Usage
```typescript
// Define a model with specific schema
const Author = new CollectModel('author', {
  name: { type: 'string' },
  email: { type: 'string', uniq: true }
});

// Use CollectInferType to infer the type of the model's schema
type AuthorType = CollectInferType<typeof Author>;

// Example usage of inferred type
const newAuthor: AuthorType = {
  name: 'John Doe',
  email: 'john.doe@example.com'
};
```