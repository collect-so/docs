---
sidebar_position: 4
---
# Ridiculously Quick Start Guide

1. Get API Token at [app.collect.so](https://app.collect.so/)
2. Install `@collect.so/javascript-sdk`
3. Build something ~~people~~ you want:

```typescript
import Collect, { CollectModel } from '@collect.so/javascript-sdk';

const CollectInstance = new Collect("API_TOKEN")

// Push any data to Collect the way you perceive it
await CollectInstance.records.createMany({
  label: "PRODUCT",
  payload: [
      { 
         title: 'T-Shirt', 
         price: 50,
      },
      {
         title: 'Sneakers',
         price: 135,
         SIZE: [
            {
              uk: 8.5,
              qty: 5 
            }
         ]
      }
   ]
})
  
// Find it with granular precision and without any query language
await CollectInstance.records.find("PRODUCT", {
    where: {
        title: { $contains: "Sneakers" },
        SIZE: {
            uk: { $gte: 8, $lte: 9 },
            qty: { $gt: 0 } 
        } 
    }
})
```

--- 
## Example with data modeling

```typescript
import Collect, { CollectModel } from '@collect.so/javascript-sdk';

const CollectInstance = new Collect("API_TOKEN")

const UserRepo = new CollectModel(
  'USER', 
   {
      name: { type: 'string' },
      rating: { type: 'number' },
   },
  CollectInstance
);

await UserRepo.create({
   name: "John Galt", 
   rating: 100
})

await UserRepo.find({
   where: {
     rating: { $gte: 50 }
   }
})
```
