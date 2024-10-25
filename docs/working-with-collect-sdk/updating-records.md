---
sidebar_position: 6
---

# Updating Records
:::note
Updating records is a crucial operation for maintaining and modifying data within your application. The `CollectModel` class provides methods to update single or multiple records. We will use the `Author` and `Post` models defined earlier to demonstrate these operations.
:::

## Table of Contents

- [Updating a Single Record](#updatebyid)
- [Updating Multiple Records](#updating-multiple-records)
- [Updating Records in a Transaction](#complex-example-with-transactions)

### `updateById`

The `updateById` method is used to update a single record by its ID.

**Signature:**
```typescript
updateById(
  id: string,
  record: InferSchemaTypesWrite<S>,
  transaction?: CollectTransaction | string,
  options?: { validate: boolean }
): Promise<CollectRecordInstance<S>>;
```

**Parameters:**

- `id`: The ID of the record to update.
- `record`: An object containing the updated data that adheres to the schema defined for the model.
- `transaction` (optional): A transaction object or string to include the operation within a transaction.
- `options` (optional): An object to specify additional options, such as whether to validate the record before updating it.

**Returns:**

- A promise that resolves to a `CollectRecordInstance` containing the updated record.

**Examples:**

*Example with Author:*
```typescript
const transaction = await Collect.tx.begin();
try {
  const updatedAuthor = await Author.updateById('author_id', {
    name: 'Jane Doe Updated'
  }, transaction);
  await transaction.commit();
  console.log(updatedAuthor);
  /*
  {
    data: {
      __id: 'author_id',
      __label: 'author',
      name: 'Jane Doe Updated',
      email: 'jane.doe@example.com'
    }
  }
  */
} catch (error) {
  await transaction.rollback();
  throw error;
}
```

*Example with Post:*
```typescript
const transaction = await Collect.tx.begin();
try {
  const updatedPost = await PostRepo.updateById('post_id', {
    title: 'Updated Title in Transaction',
    rating: 5
  }, transaction);
  await transaction.commit();
  console.log(updatedPost);
  /*
  {
    data: {
      __id: 'post_id',
      __label: 'post',
      created: '2023-01-02T00:00:00Z',
      title: 'Updated Title in Transaction',
      content: 'This is a new blog post content.',
      rating: 5
    }
  }
  */
} catch (error) {
  await transaction.rollback();
  throw error;
}
```

### Updating Multiple Records

To update multiple records, you can use a combination of `find` and `updateById`. This involves retrieving the records you want to update, modifying them, and then saving the changes.

**Examples:**

*Example with Post:*
```typescript
const postsToUpdate = await PostRepo.find({ where: { rating: { $lt: 5 } } });
const transaction = await Collect.tx.begin();
try {
  for (const post of postsToUpdate.data) {
    await PostRepo.updateById(post.__id, { rating: 5 }, transaction);
  }
  await transaction.commit();
  console.log(postsToUpdate);
  /*
  {
    data: [
      {
        __id: 'post_id_1',
        __label: 'post',
        created: '2023-01-02T00:00:00Z',
        title: 'Blog Post Title 1',
        content: 'This is a blog post content.',
        rating: 5
      },
      {
        __id: 'post_id_2',
        __label: 'post',
        created: '2023-01-03T00:00:00Z',
        title: 'Blog Post Title 2',
        content: 'This is another blog post content.',
        rating: 5
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

### Conclusion

This section covered how to update records using the `CollectModel` class. By understanding these methods and their parameters, you can effectively modify your application's data with the Collect SDK. The next sections will explore deleting records and other advanced operations.
