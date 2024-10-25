---
sidebar_position: 5
---

# Reading Records
:::note
Reading records involves fetching data from the database. The `CollectModel` class provides several methods to retrieve records based on different criteria..
:::

## Table of Contents

- [Basic Querying](#find)
- [Complex Query Executions](#complex-example-with-related-models)


### `find`

The `find` method is used to retrieve multiple records based on specified criteria.

**Signature:**

```typescript
find(
  params?: CollectQuery<S> & { labels?: never },
  transaction?: CollectTransaction | string
): Promise<CollectRecordsArrayInstance<S>>;

```

**Parameters:**

- `params` (optional): An object specifying query parameters such as filters, sorting, and pagination.
- `transaction` (optional): A transaction object or string to include the operation within a transaction.

**Returns:**

- A promise that resolves to a `CollectRecordsArrayInstance` containing the retrieved records.

**Examples:**

```typescript
const authors = await Author.find({
  where: { name: { $contains: 'John' } },
  orderBy: { createdAt: 'desc' },
  limit: 10,
  skip: 5
});
console.log(authors);
/*
{
  data: [
    {
      __id: 'author_id_3',
      __label: 'author',
      name: 'John Brown',
      email: 'john.brown@example.com'
    },
    {
      __id: 'author_id_4',
      __label: 'author',
      name: 'John Green',
      email: 'john.green@example.com'
    }
  ],
  total: 2
}
*/

```

### `findOne`

The `findOne` method is used to retrieve a single record based on specified criteria.

**Signature:**
```typescript
findOne(
  params?: CollectQuery<S> & { labels?: never },
  transaction?: CollectTransaction | string
): Promise<CollectRecordInstance<S>>;

```

**Parameters:**

- `params` (optional): An object specifying query parameters.
- `transaction` (optional): A transaction object or string to include the operation within a transaction.

**Returns:**

- A promise that resolves to a `CollectRecordInstance` containing the retrieved record.

**Examples:**

```typescript
const author = await Author.findOne({
  where: {
    $and: [{ name: { $startsWith: 'Jane' } }, { email: { $contains: '@example.com' } }]
  },
  transaction
});
console.log(author);
/*
{
  data: {
    __id: 'author_id',
    __label: 'author',
    name: 'Jane Doe',
    email: 'jane.doe@example.com'
  }
}
*/

```

### `findById`

The `findById` method is used to retrieve a single record by its ID.

**Signature:**
```typescript
findById(
  id: string,
  transaction?: CollectTransaction | string
): Promise<CollectRecordInstance<S>>;

```

**Parameters:**

- `id`: The ID of the record to retrieve.
- `transaction` (optional): A transaction object or string to include the operation within a transaction.

**Returns:**

- A promise that resolves to a `CollectRecordInstance` containing the retrieved record.

**Examples:**

```typescript
const transaction = await Collect.tx.begin();
const author = await Author.findById('author_id', transaction);
await transaction.commit();
console.log(author);
/*
{
  data: {
    __id: 'author_id',
    __label: 'author',
    name: 'John Doe',
    email: 'john.doe@example.com'
  }
}
*/

```

## Conclusion

This section provided an in-depth look at the reading operations available through the `CollectModel` class. By understanding these methods and their parameters, you can effectively retrieve your application's data with the Collect SDK. Subsequent sections will delve into more advanced topics such as relationships between models and custom validations.
