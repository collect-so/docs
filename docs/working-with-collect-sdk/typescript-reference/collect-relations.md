---
sidebar_position: 3
---

# CollectRelations

The `CollectRelations` type defines how models relate to each other within the Collect SDK. It specifies the relationships between different models, enabling you to establish connections and perform operations that involve multiple related records.

### Type Definition
```typescript
type CollectRelations = Record<
  string,
  {
    model: string;
  }
>;
```

### Properties

#### model

- **Type:** `string`
- **Required:** Yes
- **Description:** The name of the related model. This field is used to reference the target model in the relationship.

### Example Usage
```typescript
// Define a Post model with a relationship to Author and Blog
const Post = new CollectModel('post', {
  created: { type: 'datetime', default: () => new Date().toISOString() },
  title: { type: 'string' },
  content: { type: 'string' },
  rating: { type: 'number' }
}, {
  author: { model: 'author' },
  blog: { model: 'blog' }
});

// Attach a Post to an Author
Author.attach('author-id', { model: 'post', __id: 'post-id' }).then(response => {
  console.log(response.message);
  // Expected output: "Attached successfully" or similar message
});

// Detach a Post from an Author
Author.detach('author-id', { model: 'post', __id: 'post-id' }).then(response => {
  console.log(response.message);
  // Expected output: "Detached successfully" or similar message
});

// Attach a Post to a Blog
Blog.attach('blog-id', { model: 'post', __id: 'post-id' }).then(response => {
  console.log(response.message);
  // Expected output: "Attached successfully" or similar message
});

// Detach a Post from a Blog
Blog.detach('blog-id', { model: 'post', __id: 'post-id' }).then(response => {
  console.log(response.message);
  // Expected output: "Detached successfully" or similar message
});
```
