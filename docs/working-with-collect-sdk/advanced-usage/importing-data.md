---
sidebar_position: 2
---

# Importing Data Without Registering Models

When working with CollectSDK, creating models like Author, Post, and Blog repositories allows us to define clear TypeScript contracts, ensuring type safety and better development experience. However, in many scenarios, you might need to quickly import data from external sources, such as JSON files, without going through the process of registering models.

Using the `createMany` method, you can efficiently import large datasets into CollectSDK by directly specifying the labels and payloads. This approach is ideal for batch imports, data migrations, and integrating external data sources.

## Example: Importing Data from JSON
In this example, we will demonstrate how to import user, post, and blog data from a JSON file into CollectSDK using the `createMany` method:

```typescript
import CollectSDK from '@collect.so/javascript-sdk';
import fs from 'fs';

// Initialize the SDK
const Collect = new CollectSDK('your_api_token_here');

// Load data from a JSON file
const data = JSON.parse(fs.readFileSync('data.json', 'utf8'));

// Example JSON structure
/*
{
  "users": [
    { "name": "Alice Johnson", "email": "alice@example.com", "age": 30 },
    { "name": "Bob Smith", "email": "bob@example.com", "age": 25 }
  ],
  "posts": [
    { "title": "Introduction to Collect SDK", "content": "This is a post about Collect SDK...", "authorEmail": "alice@example.com" },
    { "title": "Advanced Collect SDK Usage", "content": "This post covers advanced usage of Collect SDK...", "authorEmail": "bob@example.com" }
  ],
  "blogs": [
    { "title": "Alice's Tech Blog", "description": "A blog about tech by Alice.", "ownerEmail": "alice@example.com" },
    { "title": "Bob's Coding Adventures", "description": "Bob shares his coding journey.", "ownerEmail": "bob@example.com" }
  ]
}
*/

// Convert JSON data to Collect records
const usersData = data.users.map(user => ({ label: 'user', payload: user }));
const postsData = data.posts.map(post => ({ label: 'post', payload: post }));
const blogsData = data.blogs.map(blog => ({ label: 'blog', payload: blog }));

// Function to import data
async function importData() {
  try {
    // Import users
    const importedUsers = await Collect.records.createMany(usersData);
    console.log('Imported Users:', importedUsers.data);

    // Import posts
    const importedPosts = await Collect.records.createMany(postsData);
    console.log('Imported Posts:', importedPosts.data);

    // Import blogs
    const importedBlogs = await Collect.records.createMany(blogsData);
    console.log('Imported Blogs:', importedBlogs.data);
  } catch (error) {
    console.error('Error importing data:', error);
  }
}

// Run the import function
importData();
```
