---
sidebar_position: 5
---

# CollectRecord | CollectRecordProps

The `CollectRecord` and `CollectRecordProps` types are used to define the structure and properties of records in the Collect SDK. These types provide a way to manage the data fields and internal properties of records.

### CollectRecord

The `CollectRecord` type represents a complete record, including both user-defined properties and internal metadata.

### Type Definition
```typescript
type CollectRecordInternalProps<T extends CollectObject | CollectSchema = CollectSchema> = {
  __id: string;
  __label?: string;
  __proptypes?: Record<keyof T, CollectPropertyType>;
};

type CollectRecordProps<T extends CollectObject | CollectSchema = CollectSchema> = {
  [K in keyof T]?: T extends CollectSchema ? InferTypesFromSchema<T>[K] : T[K];
};

type CollectRecord<T extends CollectObject | CollectSchema = CollectSchema> =
  CollectRecordInternalProps<T> & CollectRecordProps<T>;
```

### Properties

#### _collect_id

- **Type:** `string`
- **Required:** Yes

A unique identifier for the record.

#### _collect_label

- **Type:** `string`
- **Optional:** Yes

A label to categorize the record.

#### _collect_propsMetadata

- **Type:** `Record<string, CollectPropertyType>`
- **Optional:** Yes

Metadata about the properties of the record, including their types.

### Example Usage

Here is an example of how to define a `CollectRecord`:
```typescript
const authorRecord: CollectRecord<typeof AuthorSchema> = {
  __id: 'some-id',
  __label: 'author', 
  __proptypes: {
    name: 'string',
    email: 'string'
  },
  name: 'John Doe',
  email: 'john.doe@example.com'
};
```

### CollectRecordProps

The `CollectRecordProps` type represents the user-defined properties of a record, adhering to the schema defined for the model.

### Properties

The properties of `CollectRecordProps` depend on the schema defined for the model. Each field in the schema is represented as a key in the `CollectRecordProps` object.

### Example Usage

Here is an example of how to define a `CollectRecordProps`:
```typescript
const authorRecordProps: CollectRecordProps<typeof AuthorSchema> = {
  name: 'John Doe',
  email: 'john.doe@example.com'
};
```

In this example:
- The `CollectRecord` type includes both internal metadata fields (prefixed with `_collect_`) and user-defined fields (such as `name` and `email`).
- The `CollectRecordProps` type represents only the user-defined fields, adhering to the schema defined for the model.
