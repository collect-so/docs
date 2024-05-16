---
sidebar_position: 2
---

# CollectQueryWhere

The `CollectQueryWhere` type is used to define the filtering conditions for a query. It supports logical grouping and field-specific conditions.

### Type Definition
```typescript
type CollectQueryWhereClause<T extends CollectObject | CollectSchema = CollectSchema> = {
    where?: CollectQueryWhere<T>;
};

type CollectQueryWhere<T extends CollectObject | CollectSchema = CollectSchema> =
    | CollectQueryCondition<T>
    | RequireAtLeastOne<CollectQueryLogicalGrouping<T>>;

type CollectQueryCondition<T extends CollectObject | CollectSchema = CollectSchema> = {
    [K in keyof T]?: T extends CollectSchema ? CollectWhereValueByType[T[K]['type']]
        : T[K] extends number ? NumberValue
            : T[K] extends boolean ? BooleanValue
                : T[K] extends string ? DatetimeValue | StringValue
                    : T[K] extends null ? NullValue
                        : CollectWhereValue
};

type CollectWhereValueByType = {
    boolean: BooleanValue;
    datetime: DatetimeValue;
    null: NullValue;
    number: NumberValue;
    string: StringValue;
};


type CollectWhereValue = BooleanValue | DatetimeValue | NullValue | NumberValue | StringValue;
```

### Properties

#### Logical Grouping

- **Type:** `CollectQueryLogicalGrouping`
- **Optional:** Yes

Defines logical groupings (`$AND`, `$OR`, `$NOT`, `$XOR`) for combining multiple conditions.

#### Field Conditions

- **Type:** `CollectQueryCondition`
- **Optional:** Yes

Defines the conditions for individual fields based on their types.

### Example Usage

Here is an example of how to define filtering conditions using `CollectQueryWhere`:
```typescript
const queryWhere: CollectQueryWhere<typeof AuthorSchema> = {
  $AND: [
    { age: { $gt: 25 } },
    { name: { $startsWith: 'A' } }
  ]
};
```

In this example:
- The query filters records where the `age` field is greater than 25 and the `name` field starts with 'A'.