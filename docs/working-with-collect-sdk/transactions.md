---
sidebar_position: 3
---

# Transactions
:::note
Transactions are a powerful feature in the Collect SDK that allow you to bundle multiple operations into a single, atomic unit. This ensures that all operations within the transaction either succeed or fail together, maintaining data integrity and consistency.
:::

## Table of Contents

- [Overview](#why-use-transactions)
- [Using Transactions](#creating-a-transaction)
- [Advanced Transaction Management](#example-parallel-transactions)


### Why Use Transactions?

While it's not mandatory to use transactions, they can significantly enhance the developer experience (DX) by providing a robust mechanism for handling complex operations. Transactions are particularly useful when you need to perform a series of operations that depend on each other. If any operation in the transaction fails, all preceding operations are rolled back, preventing partial updates that could lead to data inconsistencies.

### Key Benefits of Transactions

1. **Atomicity**: Ensures that all operations within the transaction are completed successfully. If any operation fails, the entire transaction is rolled back.
2. **Consistency**: Maintains data integrity by ensuring that the database is always in a valid state, even in the event of an error.
3. **Isolation**: Transactions are isolated from each other, meaning changes made in one transaction are not visible to other transactions until they are committed.
4. **Durability**: Once a transaction is committed, the changes are permanent, even in the case of a system failure.

### Working with Transactions

To demonstrate how to work with transactions in the Collect SDK, we will use the `Author` and `Post` models defined earlier.

**Note:** In the following examples, we will use methods that have not yet been explained in detail. These methods are essential for demonstrating transaction functionality, and we will cover them in the next sections.

### Creating a Transaction

To begin a transaction, use the `Collect.tx.begin` method. This method returns a transaction object that you can use to include subsequent operations within the transaction.
```typescript
const transaction = await Collect.tx.begin();
```

### Committing a Transaction

Once all operations are successfully completed, you can commit the transaction using the `commit` method. This makes all changes within the transaction permanent.
```typescript
await transaction.commit();
```

### Rolling Back a Transaction

If any operation within the transaction fails, you can roll back the transaction using the `rollback` method. This undoes all changes made within the transaction, ensuring that no partial updates are applied.
```typescript
await transaction.rollback();
```

### Example: Basic Transaction

Let's start with a simple example to illustrate the basic usage of transactions. In this example, we will create an `Author` within a transaction. If the operation fails, the transaction will be rolled back.

**Steps:**

1. Begin a transaction.
2. Create an `Author`.
3. Commit the transaction if the operation succeeds.
4. Roll back the transaction if the operation fails.
```typescript
const transaction = await Collect.tx.begin();

try {
  // Step 1: Create an Author
  const author = await Author.create(
    {
      name: 'John Doe',
      email: 'john.doe@example.com'
    },
    transaction
  );

  // Step 2: Commit the transaction if the operation succeeds
  await transaction.commit();

  console.log('Transaction committed successfully');
  console.log('Author:', author);
} catch (error) {
  // Roll back the transaction if any operation fails
  await transaction.rollback();
  console.error('Transaction rolled back due to an error:', error);
}
```

### Example: Using Transactions

Let's see an example of how to use transactions with the `Author` and `Post` models. In this example, we will create an `Author` and a `Post` within a transaction. If any operation fails, the transaction will be rolled back.

**Steps:**

1. Begin a transaction.
2. Create an `Author`.
3. Create a `Post` and attach it to the `Author`.
4. Commit the transaction if all operations succeed.
5. Roll back the transaction if any operation fails.

```typescript
const transaction = await Collect.tx.begin();

try {
  // Step 1: Create an Author
  const author = await Author.create(
    {
      name: 'John Doe',
      email: 'john.doe@example.com'
    },
    transaction
  );

  // Step 2: Create a Post and attach it to the Author
  const post = await Post.create(
    {
      title: 'New Blog Post',
      content: 'This is the content of the blog post.',
      rating: 5
    },
    transaction
  );

  await Author.attach(author._collect_id, { model: 'post', _collect_id: post._collect_id }, transaction);

  // Step 3: Commit the transaction if all operations succeed
  await transaction.commit();

  console.log('Transaction committed successfully');
  console.log('Author:', author);
  console.log('Post:', post);
} catch (error) {
  // Roll back the transaction if any operation fails
  await transaction.rollback();
  console.error('Transaction rolled back due to an error:', error);
}
```

### Example: Parallel Transactions

In some cases, you might need to manage multiple transactions simultaneously. This example demonstrates how to handle parallel transactions to ensure data consistency across different operations.

**Steps:**

1. Begin two separate transactions.
2. Perform operations within each transaction independently.
3. Commit or roll back each transaction based on the outcome of its operations.
```typescript
const transaction1 = await Collect.tx.begin();
const transaction2 = await Collect.tx.begin();

try {
  // Perform operations within transaction1
  const author1 = await Author.create(
    {
      name: 'Alice Smith',
      email: 'alice.smith@example.com'
    },
    transaction1
  );

  // Perform operations within transaction2
  const author2 = await Author.create(
    {
      name: 'Bob Johnson',
      email: 'bob.johnson@example.com'
    },
    transaction2
  );

  // Commit both transactions if all operations succeed
  await transaction1.commit();
  await transaction2.commit();

  console.log('Transaction1 committed successfully');
  console.log('Author1:', author1);
  console.log('Transaction2 committed successfully');
  console.log('Author2:', author2);
} catch (error) {
  // Roll back both transactions if any operation fails
  await transaction1.rollback();
  await transaction2.rollback();
  console.error('Transactions rolled back due to an error:', error);
}
```

### Example: Handling Transaction Failures

This example demonstrates how to handle scenarios where one transaction fails while another succeeds. The operations within each transaction are independent, ensuring that a failure in one does not affect the other.

**Steps:**

1. Begin two separate transactions.
2. Perform operations within each transaction independently.
3. Attempt to commit both transactions.
4. Roll back the failed transaction if any operation within it fails.
```typescript
const transaction1 = await Collect.tx.begin();
const transaction2 = await Collect.tx.begin();

try {
  // Perform operations within transaction1
  const author1 = await Author.create(
    {
      name: 'Alice Smith',
      email: 'alice.smith@example.com'
    },
    transaction1
  );

  // Attempt to commit transaction1
  await transaction1.commit();
  console.log('Transaction1 committed successfully');
  console.log('Author1:', author1);

  // Perform operations within transaction2
  const author2 = await Author.create(
    {
      name: 'Bob Johnson',
      email: 'bob.johnson@example.com'
    },
    transaction2
  );

  // Intentionally cause an error in transaction2
  await Author.create(
    {
      name: 'Invalid',
      email: 'invalid@example.com'
    },
    transaction2
  );

  await transaction2.commit();
  console.log('Transaction2 committed successfully');
  console.log('Author2:', author2);
} catch (error) {
  // Roll back the failed transaction2
  await transaction2.rollback();
  console.error('Transaction2 rolled back due to an error:', error);
}
```

### Example: Independent Transactions with Failure Handling

In this example, we will create an `Author` and a `Blog` with a `Post` in parallel transactions. If the transaction creating the blog and post fails, the transaction creating the author will still be committed.

**Steps:**

1. Begin two separate transactions.
2. Create an `Author` in the first transaction.
3. Create a `Blog` and a `Post` in the second transaction.
4. Commit the first transaction.
5. Roll back the second transaction if any operation fails.
```typescript
const transaction1 = await Collect.tx.begin();
const transaction2 = await Collect.tx.begin();

try {
  // Perform operations within transaction1
  const author1 = await Author.create(
    {
      name: 'Alice Smith',
      email: 'alice.smith@example.com'
    },
    transaction1
  );

  // Attempt to commit transaction1
  await transaction1.commit();
  console.log('Transaction1 committed successfully');
  console.log('Author1:', author1);

  // Perform operations within transaction2
  const author2 = await Author.create(
    {
      name: 'Bob Johnson',
      email: 'bob.johnson@example.com'
    },
    transaction2
  );

  // Intentionally cause an error in transaction2
  await Author.create(
    {
      name: 'Invalid',
      email: 'invalid@example.com'
    },
    transaction2
  );

  await transaction2.commit();
  console.log('Transaction2 committed successfully');
  console.log('Author2:', author2);
} catch (error) {
  // Roll back the failed transaction2
  await transaction2.rollback();
  console.error('Transaction2 rolled back due to an error:', error);
}
```

### Conclusion

Transactions are a powerful tool for managing complex operations in your application. By using transactions, you can ensure data integrity and consistency, making your application more robust and reliable. In the next sections, we will delve into performing CRUD operations, which can also be managed within transactions.
