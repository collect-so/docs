---
sidebar_position: 9
---

# Linking Records
:::note
In most cases, our records do not exist independently and separately but are interconnected. The Collect SDK provides a way to define relationships between records and perform powerful queries that take these relationships into account. Linking records in Collect SDK allows you to establish these relationships, creating complex data structures where entities are interconnected. The methods `.attach()` and `.detach()` are used to manage these relationships.
:::

## Table of Contents

- [Attaching Records](#attaching-records)
- [Detaching Records](#detaching-records)

### Understanding Relationships

In Collect, relationships are established between records of different models. For example, an `Author` can be linked to multiple `Posts` through a `Blog`. This hierarchical structure can be managed using the `.attach()` and `.detach()` methods.

### Creating Models

Let's start by defining our models: `Author`, `Post`, and `Blog`.
```typescript
const Author = new CollectModel('author', {
  name: { type: 'string' },
  email: { type: 'string', uniq: true }
});

const Post = new CollectModel('post', {
  created: { type: 'datetime', default: () => new Date().toISOString() },
  title: { type: 'string' },
  content: { type: 'string' },
  rating: { type: 'number' }
});

const Blog = new CollectModel('blog', {
  title: { type: 'string' },
  description: { type: 'string' }
});
```

### Attaching Records

The `.attach()` method is used to establish a relationship between two records. Here’s how to use it.
```typescript
async attach(sourceId: string, target: CollectRelationTarget, transaction?: CollectTransaction | string): Promise<CollectApiResponse<{ message: string }>>;
```

#### Method Signature

**Parameters:**
- `sourceId`: The ID of the source record.
- `target`: The target record to attach, which includes the model and the target record ID.
- `transaction` (optional): The transaction context.

#### Example: Attaching a Post to a Blog

Let's attach a `Post` to a `Blog`.
```typescript
const transaction = await Collect.tx.begin();

try {
  const blog = await Blog.create(
    {
      title: 'Tech Blog',
      description: 'A blog about tech'
    },
    transaction
  );

  const post = await Post.create(
    {
      title: 'First Post',
      content: 'This is the first post',
      rating: 5
    },
    transaction
  );

  await Blog.attach(blog._collect_id, { model: 'post', _collect_id: post._collect_id }, transaction);

  await transaction.commit();

  console.log('Transaction committed successfully');
  // Expected output:
  // Transaction committed successfully
  // Blog: {
  //   _collect_id: 'some-id',
  //   title: 'Tech Blog',
  //   description: 'A blog about tech'
  // }
  // Post: {
  //   _collect_id: 'some-id',
  //   created: '2024-01-01T00:00:00Z',
  //   title: 'First Post',
  //   content: 'This is the first post',
  //   rating: 5
  // }
} catch (error) {
  await transaction.rollback();
  console.error('Transaction rolled back due to an error:', error);
}
```

### Detaching Records

The `.detach()` method is used to remove a relationship between two records.

#### Method Signature
```typescript
async detach(sourceId: string, target: CollectRelationTarget, transaction?: CollectTransaction | string): Promise<CollectApiResponse<{ message: string }>>;
```

**Parameters:**
- `sourceId`: The ID of the source record.
- `target`: The target record to detach, which includes the model and the target record ID.
- `transaction` (optional): The transaction context.

#### Example: Detaching a Post from a Blog

Let's detach a `Post` from a `Blog`.
```typescript
const transaction = await Collect.tx.begin();

try {
  // Assuming we have existing blog and post
  const existingBlog = await Blog.findOne({ where: { title: 'Tech Blog' } });
  const existingPost = await Post.findOne({ where: { title: 'First Post' } });

  await Blog.detach(existingBlog.data._collect_id, { model: 'post', _collect_id: existingPost.data._collect_id }, transaction);

  await transaction.commit();

  console.log('Transaction committed successfully');
  // Expected output:
  // Transaction committed successfully
  // Blog: {
  //   _collect_id: 'some-id',
  //   title: 'Tech Blog',
  //   description: 'A blog about tech'
  // }
  // Post: {
  //   _collect_id: 'some-id',
  //   created: '2024-01-01T00:00:00Z',
  //   title: 'First Post',
  //   content: 'This is the first post',
  //   rating: 5
  // }
} catch (error) {
  await transaction.rollback();
  console.error('Transaction rolled back due to an error:', error);
}
```

### Complex Example: Attaching and Detaching with Nested Relations

Here's a more complex example that demonstrates attaching and detaching with nested relations.

**Steps:**

1. Create an `Author`.
2. Create a `Blog` and a `Post`.
3. Attach the `Post` to the `Blog` and the `Blog` to the `Author`.
4. Perform nested searches using the established relationships.
```typescript
const transaction = await Collect.tx.begin();

try {
  // Create Author
  const author = await Author.create(
    {
      name: 'Jane Doe',
      email: 'jane.doe@example.com'
    },
    transaction
  );

  // Create Blog
  const blog = await Blog.create(
    {
      title: 'Jane’s Tech Blog',
      description: 'Exploring the latest in tech'
    },
    transaction
  );

  // Create Post
  const post = await Post.create(
    {
      title: 'Understanding Graph Databases',
      content: 'This post dives into the features of graph databases...',
      rating: 5
    },
    transaction
  );

  // Attach Post to Blog
  await Blog.attach(blog._collect_id, { model: 'post', _collect_id: post._collect_id }, transaction);

  // Attach Blog to Author
  await Author.attach(author._collect_id, { model: 'blog', _collect_id: blog._collect_id }, transaction);

  await transaction.commit();

  console.log('Transaction committed successfully');
  // Expected output:
  // Transaction committed successfully
  // Author: {
  //   _collect_id: 'some-id',
  //   name: 'Jane Doe',
  //   email: 'jane.doe@example.com'
  // }
  // Blog: {
  //   _collect_id: 'some-id',
  //   title: 'Jane’s Tech Blog',
  //   description: 'Exploring the latest in tech'
  // }
  // Post: {
  //   _collect_id: 'some-id',
  //   created: '2024-01-01T00:00:00Z',
  //   title: 'Understanding Graph Databases',
  //   content: 'This post dives into the features of graph databases...',
  //   rating: 5
  // }
} catch (error) {
  await transaction.rollback();
  console.error('Transaction rolled back due to an error:', error);
}
```

### Example: Nested Query

Now that we've set up the relationships, let's perform a nested query to find an author by the content of their post.
```typescript
const nestedQuery = await Author.find({
    where: {
        name: 'Jane Doe',
        blog: {
            post: {
                content: { $startsWith: 'This post dives into' }
            }
        }
    }
});

console.log(nestedQuery.data);
// Expected output:
// [
//   {
//     _collect_id: 'some-id',
//     name: 'Jane Doe',
//     email: 'jane.doe@example.com',
//   }
// ]
```

### Example: Advanced Nested Query with Multiple Conditions

Here's an example of an advanced nested query that searches for an author based on multiple fields in each model.
```typescript
const advancedNestedQuery = await Author.find({
    where: {
        name: { $startsWith: 'Jane' },
        email: { $contains: '@example.com' },
        blog: {
            title: { $contains: 'Tech' },
            post: {
                $or: [
                    { title: { $startsWith: 'Understanding' } },
                    { rating: { $gte: 4 } }
                ]
            }
        }
    }
});

console.log(advancedNestedQuery.data);
// Expected output:
// [
//   {
//     _collect_id: 'some-id',
//     name: 'Jane Doe',
//     email: 'jane.doe@example.com',
//   }
// ]
```

### Example: Searching Posts in a Specific Blog

Now, let's perform a query to find posts that belong to a specific blog and match certain criteria.
```typescript
const blogPostsQuery = await Post.find({
    where: {
        blog: { title: 'Jane’s Tech Blog' },
        created: { $gte: '2024-01-01T00:00:00Z' },
        rating: { $gte: 4 }
    }
});

console.log(blogPostsQuery.data);
// Expected output:
// [
//   {
//     _collect_id: 'some-id',
//     created: '2024-01-01T00:00:00Z',
//     title: 'Understanding Graph Databases',
//     content: 'This post dives into the features of graph databases...',
//     rating: 5
//   },
//   {
//     _collect_id: 'some-id',
//     created: '2024-01-01T00:00:00Z',
//     title: 'Another Post',
//     content: 'Another post content',
//     rating: 4
//   }
// ]
```

### Using Different Types for `CollectRelationTarget`

The `target` parameter in the `.attach()` and `.detach()` methods can take various forms, including instances of `CollectRecordsArrayInstance`, `CollectRecord`, `CollectRecordInstance`, or simply an array of IDs. Here are examples demonstrating each type.
```typescript
// Using CollectRecordsArrayInstance
const postsArray = await Post.find();
await Blog.attach(blog._collect_id, postsArray.data, transaction);

// Using CollectRecord
const postRecord = await Post.findOne({ where: { title: 'Understanding Graph Databases' } });
await Blog.attach(blog._collect_id, postRecord.data, transaction);

// Using CollectRecordInstance
await Blog.attach(blog._collect_id, postRecord.data, transaction);

// Using array of IDs
const postIds = postsArray.data.map(post => post._collect_id);
await Blog.attach(blog._collect_id, postIds, transaction);
```
