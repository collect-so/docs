---
sidebar_position: 6
---

# CollectRecordInstance | CollectRecordsArrayInstance
The `CollectRecordInstance` and `CollectRecordsArrayInstance` types are used to represent individual records and arrays of records in the Collect SDK. These instances provide methods to interact with the records, such as updating and deleting them.

## CollectRecordInstance

The `CollectRecordInstance` type represents a single record instance, providing methods for interacting with the record.

### Type Definition
```typescript
class CollectRecordInstance<T extends CollectObject | CollectSchema = CollectObject> {
  data: CollectRecord<T>;
  searchParams?: CollectQuery<T>;

  constructor(data: CollectRecord<T>, searchParams?: CollectQuery<T>) {
    this.data = data;
    this.searchParams = searchParams;
  }

  async update<T extends CollectObject = CollectObject>(
    data: CollectRecordObject | T,
    transaction?: CollectTransaction | string
  ): Promise<CollectRecordResult<T>> {
    // Implementation here...
  }

  async delete(transaction?: CollectTransaction | string): Promise<CollectApiResponse<{ message: string }>> {
    // Implementation here...
  }
}
```

### Properties

#### data

- **Type:** `CollectRecord<T>`
- **Required:** Yes

The data of the record.

#### searchParams

- **Type:** `CollectQuery<T>`
- **Optional:** Yes

The search parameters used to find the record.

### Example Usage
```typescript
const authorInstance = new CollectRecordInstance({
  __id: 'some-id',
  __label: 'author',
  name: 'John Doe',
  email: 'john.doe@example.com'
});

// Update the record
authorInstance.update({ name: 'Jane Doe' }).then(updatedRecord => {
  console.log(updatedRecord.data); // { __id: 'some-id', name: 'Jane Doe', email: 'john.doe@example.com' }
});

// Delete the record
authorInstance.delete().then(response => {
  console.log(response); // { message: 'Record deleted successfully' }
});
```

## CollectRecordsArrayInstance

The `CollectRecordsArrayInstance` type represents an array of record instances, providing methods for interacting with multiple records.

### Type Definition
```typescript
class CollectRecordsArrayInstance<T extends CollectObject | CollectSchema = CollectObject> {
  data: CollectRecord<T>[];
  total: number | undefined;
  searchParams?: CollectQuery<T>;

  constructor(data: CollectRecord<T>[], total?: number, searchParams?: CollectQuery<T>) {
    this.data = data;
    this.total = total;
    this.searchParams = searchParams;
  }

  async update<T extends CollectObject = CollectObject>(
    data: CollectRecordObject | T,
    transaction?: CollectTransaction | string
  ): Promise<CollectRecordsArrayResult<T>> {
    // Implementation here...
  }

  async delete(transaction?: CollectTransaction | string): Promise<CollectApiResponse<{ message: string }>> {
    // Implementation here...
  }
}
```

### Properties

#### data

- **Type:** `CollectRecord<T>[]`
- **Required:** Yes

The data of the records.

#### total

- **Type:** `number`
- **Optional:** Yes

The total number of records.

#### searchParams

- **Type:** `CollectQuery<T>`
- **Optional:** Yes

The search parameters used to find the records.

### Example Usage
```typescript
const authorsArrayInstance = new CollectRecordsArrayInstance([
  { __id: 'id-1', __label: 'author', name: 'John Doe', email: 'john.doe@example.com' },
  { __id: 'id-2', __label: 'author', name: 'Jane Doe', email: 'jane.doe@example.com' }
]);

// Update multiple records
authorsArrayInstance.update({ email: 'updated@example.com' }).then(updatedRecords => {
  console.log(updatedRecords.data); // [{ __id: 'id-1', name: 'John Doe', email: 'updated@example.com' }, { __id: 'id-2', name: 'Jane Doe', email: 'updated@example.com' }]
});

// Delete multiple records
authorsArrayInstance.delete().then(response => {
  console.log(response); // { message: 'Records deleted successfully' }
});
```

In this example:
- The `CollectRecordInstance` type represents a single record and provides methods for updating and deleting the record.
- The `CollectRecordsArrayInstance` type represents an array of records and provides methods for updating and deleting multiple records.
