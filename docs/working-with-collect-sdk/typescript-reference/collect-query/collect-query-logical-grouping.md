---
sidebar_position: 3
---

# CollectQueryLogicalGrouping

The `CollectQueryLogicalGrouping` type is used to define logical groupings for combining multiple conditions in a query.

### Type Definition
```typescript
type CollectQueryLogicalGrouping<T extends CollectObject | CollectSchema = CollectSchema> =
  Record<'$AND' | '$NOT' | '$OR' | '$XOR', Enumerable<CollectQueryCondition<T>>>;
```

### Properties

#### $AND

- **Type:** `CollectQueryCondition[]`
- **Optional:** Yes

Combines multiple conditions using a logical AND.

#### $OR

- **Type:** `CollectQueryCondition[]`
- **Optional:** Yes

Combines multiple conditions using a logical OR.

#### $NOT

- **Type:** `CollectQueryCondition[]`
- **Optional:** Yes

Negates the combined conditions.

#### $XOR

- **Type:** `CollectQueryCondition[]`
- **Optional:** Yes

Combines multiple conditions using a logical XOR.

### Example Usage

Here is an example of how to use logical groupings in a query:
```typescript
const queryWhere: CollectQueryWhere<typeof AuthorSchema> = {
  $AND: [
    { age: { $gt: 25 } },
    { name: { $startsWith: 'A' } }
  ]
};
```

In this example:
- The query filters records where the `age` field is greater than 25 or the `name` field starts with 'A'.