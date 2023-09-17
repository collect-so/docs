---
sidebar_position: 3
---
# Data Types

Collect supports a wide range of data types to accommodate diverse data needs and provide a flexible environment for your applications. Below is a comprehensive list of the supported data types along with their descriptions:

- `string` - any text data: `"Pat spit the pips in the tin"`
- `number` - both floats and integers: `-120.209817` or `42`
- `datetime` - ISO8601: `2012-12-21T18:29:37Z` with timezone support `2023-04-11T09:30:15+04:00`
- `boolean` - `true` or `false` (wow!)
- `null` - guess what? -aha: `null`

Basically it supports all data types as JSON does. But wait, what about arrays?
Collect supports arrays even for specific needs:

- `["apple", "banana", "carrot"]` - good
- `[null, null, null, null, null]` - wierd, but works fine 🤔
- `[4, 8, 15, 16, 23, 42]` - works as well
- `["2023-09-17T02:47:54+04:00", "1990-08-18T04:35:00+05:00"]` - also good
- `[true, false, true, false, true]` - love means everything (🌼)

This implies that an individual record can have an array as its properties:

```typescript
// Record:Coffee
const Coffee = {
    origin: "Guatemala", 
    process: "washed", 
    cupping: 86, 
    roasted: "2023-07-20T14:50:00Z",
    notes: ["Nuts", "Caramel", "Lime"] // ☕
}
```