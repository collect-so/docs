---
sidebar_position: 3
---
# Records

In Collect, you have the capability to store meaningful data within **Records**. These **Records** consist of individual data 
pieces, each containing keys and their corresponding values (**Properties**):

```typescript
// Record:User
const user = {
    id: "si310jfpiej20i9h",
    name: "John Galt",
    emailConfirmed: true,
    registeredAt: "2022-07-19T08:30:28.000Z",
    rating: 4.98,
    currency: "USD",
    email: "john.galt@example.com"
}
```

```typescript
// Record:Coffee
const coffee = {
    origin: "Guatemala", 
    process: "washed", 
    cupping: 86, 
    roasted: "2023-07-20T14:50:00Z",
    notes: ["Nuts", "Caramel", "Lime"]
}
```

As Collect cares about data-consistency and - it automates type suggestions and utilized data modeling and scheming instantly.

