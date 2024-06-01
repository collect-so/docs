---
sidebar_position: 1
---

# Enhanced TypeScript Support
:::note
When working with CollectSDK, achieving perfect TypeScript contracts ensures a seamless development experience. TypeScript's strong typing system allows for precise autocomplete suggestions and error checking, particularly when dealing with complex queries and nested models. This section will guide you on how to enhance TypeScript support by defining comprehensive type definitions for your models.
:::

## Defining Comprehensive TypeScript Types

To fully leverage TypeScript's capabilities, you can define types that include all schemas you've registered with `CollectModel`. This will allow you to perform complex queries with nested model fields, ensuring type safety and better autocompletion.

### Step 1: Create Models with `CollectModel`

First, define your models using `CollectModel`:
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

### Step 2: Create an Exportable Type for All Schemas

Next, create an exportable type that includes all the schemas defined in your application:
```typescript
export type Models = {
  author: typeof Author.schema
  post: typeof Post.schema
  blog: typeof Blog.schema
}
```

### Step 3: Extend the CollectModels Interface

Add this type definition to the existing `index.d.ts` file in the root of your project. This ensures that CollectSDK is aware of your models:
```typescript
declare module '@collect.so/javascript-sdk' {
  export interface CollectModels extends Models {}
}
```

### Example Usage

By following these steps, you can now write complex queries with confidence, knowing that TypeScript will help you avoid errors and provide accurate autocomplete suggestions. Here's an example demonstrating how you can leverage this setup:

#### Finding Posts Rated by a Specific Author with a Rating Above 5
```typescript
const query = await Collect.records.find('post', {
  where: {
    author: {
      name: { $contains: 'John' }, // Checking if the author's name contains 'John'
      post: {
        rating: { $gt: 5 } // Posts with rating greater than 5
      }
    }
  }
});
```
In this example, the `Collect.records.find` method allows you to use nested fields in the `where` condition, thanks to the enhanced TypeScript definitions. This ensures that you can easily and accurately query your data, leveraging the full power of TypeScript.

### Conclusion

By defining comprehensive type definitions for your models and extending the `CollectModels` interface, you can significantly enhance your TypeScript support when working with CollectSDK. This approach ensures type safety, better autocompletion, and a more efficient development experience.
