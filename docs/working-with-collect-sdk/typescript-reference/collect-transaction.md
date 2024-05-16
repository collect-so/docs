---
sidebar_position: 9
---

# CollectTransaction

The `CollectTransaction` class is used to manage transactions within the Collect SDK. Transactions allow you to perform multiple operations in a way that they either all succeed or none of them are applied, ensuring data integrity.

### Type Definition
```typescript
declare class CollectTransaction extends CollectRestApiProxy {
  readonly id: string;

  constructor(id: string);

  commit(): Promise<void>;
  rollback(): Promise<void>;
}
```

### Properties

#### id

- **Type:** `string`
- **Required:** Yes
- **Description:** A unique identifier for the transaction.

### Methods

#### commit

- **Description:** Commits the transaction, applying all operations performed within it.
- **Returns:** A promise that resolves once the transaction is successfully committed.

#### rollback

- **Description:** Rolls back the transaction, undoing all operations performed within it.
- **Returns:** A promise that resolves once the transaction is successfully rolled back.

### Example Usage
```typescript
// Begin a new transaction
const transaction = await Collect.tx.begin();
try {
  // Perform operations within the transaction
  await UserRepo.create({ name: 'Jane Doe', email: 'jane.doe@example.com' }, transaction2);
  await PostRepo.create({ title: 'Another Post', content: 'This is another post' }, transaction2);
  // Commit the transaction
  await transaction2.commit();
} catch (error) {
  // Rollback the transaction in case of an error
  await transaction2.rollback();
}
```