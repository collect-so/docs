---
sidebar_position: 2
---
# CollectModel

The `CollectModel` class represents a data model in the Collect SDK. It provides methods for performing CRUD operations, managing relationships, and validating records according to a defined schema.

### Type Definition
```typescript
class CollectModel<S extends CollectSchema = CollectSchema, R extends CollectRelations = CollectRelations> extends CollectRestApiProxy {
  private readonly label: string;
  public schema: S;
  public relationships: R;
  private validator?: Validator;

  constructor(modelName: string, schema: S, relationships: R = {} as R) {
    super();
    this.label = modelName;
    this.schema = schema;
    this.relationships = relationships;
  }
}
```

### Constructor Parameters

#### modelName

- **Type:** `string`
- **Required:** Yes

A unique string identifier for the model. It's used to reference the model within the SDK and to associate records with their corresponding model type in the database.

#### schema

- **Type:** `CollectSchema`
- **Required:** Yes

The schema definition based on `CollectSchema`, which dictates the structure and rules of the data stored.

#### relationships

- **Type:** `CollectRelations`
- **Optional:** Yes

Defines how this model relates to other models, which is essential for establishing connections between different data types.

### Methods

#### find

Finds multiple records based on specified query parameters.

**Signature:**
```typescript
find(params?: CollectQuery<S> & { labels?: never }, transaction?: CollectTransaction | string): Promise<CollectRecordsArrayInstance<S>>;
```

#### findOne

Finds a single record based on specified query parameters.

**Signature:**
```typescript
findOne(params?: CollectQuery<S> & { labels?: never }, transaction?: CollectTransaction | string): Promise<CollectRecordInstance<S>>;
```

#### findById

Finds a single record by its ID.

**Signature:**
```typescript
findById(id: string, transaction?: CollectTransaction | string): Promise<CollectRecordInstance<S>>;
```

#### create

Creates a single record.

**Signature:**
```typescript
create(record: InferSchemaTypesWrite<S>, transaction?: CollectTransaction | string, options?: { validate: boolean }): Promise<CollectRecordInstance<InferSchemaTypesWrite<S>>>;
```

#### attach

Attaches a target record to the source record.

**Signature:**
```typescript
attach(sourceId: string, target: CollectRelationTarget, transaction?: CollectTransaction | string): Promise<CollectApiResponse<{ message: string }>>;
```

#### detach

Detaches a target record from the source record.

**Signature:**
```typescript
detach(sourceId: string, target: CollectRelationTarget, transaction?: CollectTransaction | string): Promise<CollectApiResponse<{ message: string }>>;
```

#### updateById

Updates a single record by its ID.

**Signature:**
```typescript
updateById(id: string, record: InferSchemaTypesWrite<S>, transaction?: CollectTransaction | string, options?: { validate: boolean }): Promise<CollectRecordInstance<S>>;
```

#### createMany

Creates multiple records in a single operation.

**Signature:**
```typescript
createMany(records: InferSchemaTypesWrite<S>[], transaction?: CollectTransaction | string, options?: { validate: boolean }): Promise<CollectRecordsArrayInstance<S>>;
```

#### delete

Deletes multiple records based on specified query parameters.

**Signature:**
```typescript
delete<T extends InferSchemaTypesWrite<S> = InferSchemaTypesWrite<S>>(params?: Omit<CollectQuery<T>, 'labels'>, transaction?: CollectTransaction | string): Promise<CollectApiResponse<{ message: string }>>;
```

#### validate

Validates a record according to the schema rules.

**Signature:**
```typescript
validate(data: InferSchemaTypesWrite<S>): Promise<unknown>;
```

### Example Usage
```typescript
const Author = new CollectModel('author', {
  name: { type: 'string' },
  email: { type: 'string', uniq: true }
});

// Find multiple records
Author.find({ where: { name: { $contains: 'Doe' } } }).then(records => {
  console.log(records.data);
  // Expected output: Array of authors with name containing 'Doe'
});

// Find one record
Author.findOne({ where: { email: 'john.doe@example.com' } }).then(record => {
  console.log(record.data);
  // Expected output: Single author record with email 'john.doe@example.com'
});

// Find by ID
Author.findById('some-id').then(record => {
  console.log(record.data);
  // Expected output: Author record with the specified ID
});

// Create a record
Author.create({ name: 'John Doe', email: 'john.doe@example.com' }).then(record => {
  console.log(record.data);
  // Expected output: Newly created author record
});

// Attach a related record
const Blog = new CollectModel('blog', {
  title: { type: 'string' },
  description: { type: 'string' }
});

Author.attach('author-id', { model: 'blog', __id: 'blog-id' }).then(response => {
  console.log(response.message);
  // Expected output: "Attached successfully" or similar message
});

// Detach a related record
Author.detach('author-id', { model: 'blog', __id: 'blog-id' }).then(response => {
  console.log(response.message);
  // Expected output: "Detached successfully" or similar message
});

// Update a record by ID
Author.updateById('some-id', { name: 'John Smith' }).then(record => {
  console.log(record.data);
  // Expected output: Updated author record with name 'John Smith'
});

// Create multiple records
Author.createMany([
  { name: 'John Doe', email: 'john.doe@example.com' },
  { name: 'Jane Doe', email: 'jane.doe@example.com' }
]).then(records => {
  console.log(records.data);
  // Expected output: Array of newly created author records
});

// Delete records
Author.delete({ where: { name: { $contains: 'Doe' } } }).then(response => {
  console.log(response.message);
  // Expected output: "Deleted successfully" or similar message
});
```
