---
sidebar_position: 4
---

# Creating Records
:::note
Creating records is a fundamental operation when working with any data-driven application. The `CollectModel` class provides methods to create single or multiple records in the database. 
:::

## Table of Contents

- [Creating a Single Record](#create)
- [Creating Multiple Records](#createmany)

We will use the `Author` model defined earlier to demonstrate these operations.
```typescript
const Author = new CollectModel('author', {
  name: { type: 'string' },
  email: { type: 'string', uniq: true }
});
```

### `create`

The `create` method is used to create a single record.


**Signature:**
```typescript
create(
  record: InferSchemaTypesWrite<S>,
  transaction?: CollectTransaction | string,
  options?: { validate: boolean }
): Promise<CollectRecordInstance<InferSchemaTypesWrite<S>>>;
```

**Parameters:**

- `record`: An object that adheres to the schema defined for the model.
- `transaction` (optional): A transaction object or string to include the operation within a transaction.
- `options` (optional): An object to specify additional options, such as whether to validate the record before creating it.

**Returns:**

- A promise that resolves to a `CollectRecordInstance` containing the created record.

**Examples:**

*Basic Example:*
```typescript
const newAuthor = {
  name: 'John Doe',
  email: 'john.doe@example.com'
};

const createdAuthor = await Author.create(newAuthor);
console.log(createdAuthor);

/*
{
  data: {
    _collect_id: 'generated_id',
    _collect_label: 'author',
    name: 'John Doe',
    email: 'john.doe@example.com'
  }
}
*/
```

*Complex Example:*
```typescript
const newAuthor = {
  name: 'Jane Smith',
  email: 'jane.smith@example.com'
};

const transaction = await Collect.tx.begin();
try {
  const createdAuthor = await Author.create(newAuthor, transaction);
  await transaction.commit();
  console.log(createdAuthor);

  /*
  {
    data: {
      _collect_id: 'generated_id',
      _collect_label: 'author',
      name: 'Jane Smith',
      email: 'jane.smith@example.com'
    }
  }
  */
} catch (error) {
  await transaction.rollback();
  throw error;
}

```

### `createMany`

The `createMany` method is used to create multiple records in a single operation.

**Signature:**
```typescript
createMany(
  records: InferSchemaTypesWrite<S>[],
  transaction?: CollectTransaction | string,
  options?: { validate: boolean }
): Promise<CollectRecordsArrayInstance<S>>;
```

**Parameters:**

- `records`: An array of objects, each adhering to the schema defined for the model.
- `transaction` (optional): A transaction object or string to include the operation within a transaction.
- `options` (optional): An object to specify additional options, such as whether to validate the records before creating them.

**Returns:**

- A promise that resolves to a `CollectRecordsArrayInstance` containing the created records.

**Examples:**

*Basic Example:*
```typescript
const authors = [
  { name: 'Alice Johnson', email: 'alice.johnson@example.com' },
  { name: 'Bob Brown', email: 'bob.brown@example.com' }
];

const createdAuthors = await Author.createMany(authors);
console.log(createdAuthors);
/*
{
  data: [
    {
      _collect_id: 'generated_id_1',
      _collect_label: 'author',
      name: 'Alice Johnson',
      email: 'alice.johnson@example.com'
    },
    {
      _collect_id: 'generated_id_2',
      _collect_label: 'author',
      name: 'Bob Brown',
      email: 'bob.brown@example.com'
    }
  ],
  total: 2
}
*/
```

*Complex Example:*
```typescript
const authors = [
  { name: 'Charlie Green', email: 'charlie.green@example.com' },
  { name: 'David Blue', email: 'david.blue@example.com' }
];

const transaction = await Collect.tx.begin();
try {
  const createdAuthors = await Author.createMany(authors, transaction);
  await transaction.commit();
  console.log(createdAuthors);
  /*
  {
    data: [
      {
        _collect_id: 'generated_id_1',
        _collect_label: 'author',
        name: 'Charlie Green',
        email: 'charlie.green@example.com'
      },
      {
        _collect_id: 'generated_id_2',
        _collect_label: 'author',
        name: 'David Blue',
        email: 'david.blue@example.com'
      }
    ],
    total: 2
  }
  */
} catch (error) {
  await transaction.rollback();
  throw error;
}

```

