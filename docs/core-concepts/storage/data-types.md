---
sidebar_position: 3
---
# Data Types

Collect supports a wide range of data types to accommodate diverse data needs and provide a flexible environment for your applications. Below is a comprehensive list of the supported data types along with their descriptions:

[//]: # (## String)

[//]: # (   Textual data consisting of characters and words.)

[//]: # ()
[//]: # (   Example: `"John Doe"`, `"Hello, World!"`)

[//]: # (## Number)

[//]: # (   Numeric data representing integers or floating-point numbers.)

[//]: # ()
[//]: # (   Example: `42`, `3.14`)

[//]: # (## Datetime)

[//]: # (   Data representing date and time information in ISO8601 format.)

[//]: # ()
[//]: # (   Example: `"2023-07-27T12:34:56Z"`)

[//]: # (## Point &#40;2D/3D&#41;)

[//]: # (   Geospatial data representing coordinates in either 2-dimensional or 3-dimensional space.)

[//]: # ()
[//]: # (   Example &#40;2D&#41;: `"37.7749,-122.4194"`)

[//]: # ()
[//]: # (   Example &#40;3D&#41;: `"37.7749,-122.4194,10.0"`)

[//]: # (## Boolean)

[//]: # (   Description: Logical data representing true or false values.)

[//]: # ()
[//]: # (   Example: `true`, `false`)

[//]: # (## String[])

[//]: # (   An array of strings, allowing multiple values to be stored.)

[//]: # ()
[//]: # (   Example: `["apple", "banana", "orange"]`)

[//]: # (## Number[])

[//]: # (   An array of numbers, allowing multiple numeric values to be stored.)

[//]: # ()
[//]: # (   Example: `[1, 2, 3, 4, 5]`)

[//]: # (## Datetime[])

[//]: # (   An array of datetime values, allowing multiple date and time entries to be stored.)

[//]: # ()
[//]: # (   Example: `["2023-07-27T12:00:00Z", "2023-07-28T10:30:00Z", "2023-07-29T15:45:00Z"]`)

[//]: # (## Point[])

[//]: # (   Description: An array of 2D or 3D points, allowing multiple geospatial coordinates to be stored.)

[//]: # ()
[//]: # (   Example &#40;2D&#41;: `["40.7128,-74.0060", "37.7749,-122.4194"]`)

[//]: # ()
[//]: # (   Example &#40;3D&#41;: `["40.7128,-74.0060,10.0", "37.7749,-122.4194,15.0"]`)

[//]: # ()
[//]: # (These supported data types empower your applications to handle various types of data, ranging from simple text and numbers to complex geospatial coordinates and arrays. With this diversity, you can build robust and dynamic solutions that cater to the specific needs of your apps.)

[//]: # ()
[//]: # ()
[//]: # (## Records)

[//]: # ()
[//]: # (## Magic Fields)

[//]: # ()
[//]: # (## Labels)

[//]: # ()
[//]: # (## )

[//]: # (## Type suggestions)

[//]: # (Collect automatically suggests data types depending on input:)

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