---
sidebar_position: 10
---

# Querying Data
:::note
In previous sections, you have encountered the `where` condition and various logical operators like `$and` and `$xor`. This section provides a comprehensive guide on querying data using the `CollectQuery` type, covering all available logical and comparison operators.
:::

## Table of Contents

- [Logical Operators](#logical-operators)
    - [$and](#and)
    - [$or](#or)
    - [$not](#not)
    - [$nor](#nor)
    - [$xor](#xor)
- [Comparison Operators](#comparison-operators)
    - [Boolean Operators](#boolean-operators)
        - [$not](#not-1)
    - [Datetime Operators](#datetime-operators)
        - [$gt](#gt)
        - [$gte](#gte)
        - [$lt](#lt)
        - [$lte](#lte)
        - [$in](#in)
        - [$nin](#nin)
    - [Number Operators](#number-operators)
        - [$gt](#gt-1)
        - [$gte](#gte-1)
        - [$lt](#lt-1)
        - [$lte](#lte-1)
        - [$in](#in-1)
        - [$nin](#nin-1)
    - [String Operators](#string-operators)
        - [$contains](#contains)
        - [$endsWith](#endswith)
        - [$startsWith](#startswith)
        - [$in](#in-2)
        - [$nin](#nin-1)

## Logical Operators

Logical operators allow you to build complex queries by combining multiple conditions.

### $and

The `$and` operator combines multiple conditions and returns results that match all the conditions.

Alternatively, you can omit `$and` and directly list the conditions if there are no other logical operators at the same level.

##### Examples:
```typescript
// Basic example with $and
const queryWithAnd = await Collect.records.find('AUTHOR', {
  where: {
    $and: [
      { name: { $startsWith: 'Jane' } },
      { email: { $contains: '@example.com' } }
    ]
  }
});
```

```typescript
// Basic example without $and
const queryWithAnd = await Collect.records.find('AUTHOR', {
  where: {
    name: { $startsWith: 'Jane' },
    email: { $contains: '@example.com' }
  }
});
```

### $or

The `$or` operator combines multiple conditions and returns results that match any of the conditions. This is useful for querying records that meet at least one of several criteria.

##### Examples:
```typescript
// Complex example with $or
const queryWithOr = await Collect.records.find('POST', {
  where: {
    $or: [
      { rating: { $gte: 4 } },
      { title: { $contains: 'Guide' } }
    ]
  }
});
```

```typescript
// Complex example with $and and $or for numbers
const queryComplexNumber = await Collect.records.find('POST', {
  where: {
    $and: [
      { rating: { $gte: 3, $lte: 5 } },
      { views: { $gt: 1000 } }
    ],
    $or: [
      { comments: { $lt: 50 } },
      { shares: { $gte: 100 } }
    ]
  }
});
```

### $not

The `$not` operator inverts the condition it applies to, returning results that do not match the specified condition.

##### Examples:
```typescript
// Example using $not
const queryWithNot = await Collect.records.find('AUTHOR', {
  where: {
    $not: [
      { email: { $contains: '@example.com' } }
    ]
  }
});
```

### $nor

The `$nor` operator returns true when all values are false

##### Examples:
```typescript
// Example using $nor
const queryWithNot = await Collect.records.find('AUTHOR', {
  where: {
    $nor: [
      { email: { $contains: '@gmail.com' } },
      { email: { $contains: '@proton.me' } }
    ]
  }
});
```

### $xor
```typescript
// Example using $xor
const queryWithXor = await Collect.records.find('AUTHOR', {
  where: {
    $xor: [
      { name: { $startsWith: 'Jane' } },
      { email: { $contains: '@example.com' } }
    ]
  }
});
```

The `$xor` operator (exclusive OR) combines multiple conditions and returns results that match one and only one of the conditions.

##### Examples:

## Comparison Operators

Comparison operators are used to filter records based on specific field values.

### Boolean Operators

#### $not

The `$not` operator checks if a field is not equal to a specified value. This operator is implicitly used when specifying field values directly.

##### Examples:
```typescript
const queryNotFalse = await Collect.records.find('AUTHOR', {
  where: {
    email: { $startsWith: '' },
    married: { $not: false }
  }
});
```

### Datetime Operators

#### $gt

The `$gt` (greater than) operator checks if a field's value is greater than the specified datetime value.

##### Examples:
```typescript
const queryGreaterDatetime = await Collect.records.find('POST', {
  where: {
    created: { $gt: { $year: 2023, $month: 1, $day: 1 } }
  }
});
// Finds posts created after January 1, 2023
```

#### $gte

The `$gte` (greater than or equal to) operator checks if a field's value is greater than or equal to the specified datetime value.

##### Examples:
```typescript
const queryGreaterOrEqualDatetime = await Collect.records.find('POST', {
  where: {
    created: { $gte: '2023-01-01T00:00:00Z' }
  }
});
// Finds posts created on or after January 1, 2023, 00:00:00 UTC
```

#### $lt

The `$lt` (less than) operator checks if a field's value is less than the specified value.

##### Examples:
```typescript
const queryLesserDatetime = await Collect.records.find('POST', {
  where: {
    created: { $lt: { $year: 2024, $month: 1, $day: 1 } }
  }
});
// Finds posts created before January 1, 2024
```

#### $lte

The `$lte` (less than or equal to) operator checks if a field's value is less than or equal to the specified value.

##### Examples:
```typescript
// Complex example with $gte and $lte for datetime
const queryWithDatetime = await Collect.records.find('POST', {
  where: {
    created: { $gte: '2023-01-01T00:00:00Z', $lte: '2023-12-31T23:59:59Z' }
  }
});
```
```typescript
// Example using $lte for datetime as object
const queryWithLteDatetimeObject = await Collect.records.find('POST', {
  where: {
    created: { $lte: { $year: 2024, $month: 1, $day: 1 } }
  }
});
```

#### $not

The `$not` operator is used to find records where the datetime field does not match the specified value.

##### Examples:
```typescript
const queryNotDatetime = await Collect.records.find('POST', {
  where: {
    created: { $not: '2023-01-01T00:00:00Z' }
  }
});
// Finds posts not created on January 1, 2023, 00:00:00 UTC
```

#### $in

The `$in` operator is used to find records where the datetime field matches any value in the specified array.

##### Examples:
```typescript
const queryInDatetime = await Collect.records.find('POST', {
  where: {
    created: { $in: [
      '2023-01-01T00:00:00Z',
      '2023-02-01T00:00:00Z'
    ]}
  }
});
// Finds posts created on January 1, 2023 or February 1, 2023
```

#### $nin

The `$nin` operator is used to find records where the datetime field doesn't match any value in the specified array.

##### Examples:
```typescript
const queryInDatetime = await Collect.records.find('POST', {
  where: {
    created: { $nin: [
      '2023-01-01T00:00:00Z',
      '2023-02-01T00:00:00Z'
    ]}
  }
});

// Finds posts that were not created on January 1, 2023, or February 1, 2023
```

### Number Operators

#### $gt

The `$gt` (greater than) operator checks if a field's value is greater than the specified value.

##### Examples:
```typescript
// Example using $gt
const queryWithGt = await Collect.records.find('POST', {
  where: {
    rating: { $gt: 4 }
  }
});
```

#### $gte

The `$gte` (greater than or equal to) operator checks if a field's value is greater than or equal to the specified value.

##### Examples:
```typescript
// Example using $gte
const queryWithGte = await Collect.records.find('POST', {
  where: {
    rating: { $gte: 4 }
  }
});
```

#### $lt

The `$lt` (less than) operator checks if a field's value is less than the specified value.

##### Examples:
```typescript
// Example using $lt
const queryWithLt = await Collect.records.find('POST', {
  where: {
    rating: { $lt: 4 }
  }
});
```

#### $lte

The `$lte` (less than or equal to) operator checks if a field's value is less than the specified value.

##### Examples:
```typescript
// Example using $lte
const queryWithLte = await Collect.records.find('POST', {
  where: {
    rating: { $lte: 4 }
  }
});

```

#### $in

The `$in` operator checks if a field's value is within a specified array of values.

##### Examples:
```typescript
// Example using $in (numbers)
const queryWithInNumbers = await Collect.records.find('AUTHOR', {
  where: {
    age: { $in: [25, 30, 35] }
  }
});
```

#### $nin

The `$nin` operator checks if a field's value is not within a specified array of values.

##### Examples:
```typescript
// Example using $nin (numbers)
const queryWithNotInNumbers = await Collect.records.find('AUTHOR', {
  where: {
    age: { $nin: [25, 30, 35] }
  }
});
```

### String Operators

#### $contains

The `$contains` operator checks if a string field contains the specified substring.

##### Examples:
```typescript
// Example using $contains
const queryWithContains = await Collect.records.find('POST', {
  where: {
    content: { $contains: 'Graph' }
  }
});
```

#### $endsWith

The `$endsWith` operator checks if a string field ends with the specified substring.

##### Examples:
```typescript
// Example using $endsWith
const queryWithEndsWith = await Collect.records.find('POST', {
  where: {
    title: { $endsWith: 'Databases' }
  }
});
```

#### $startsWith

The `$startsWith` operator checks if a string field starts with the specified substring.

##### Examples:
```typescript
// Example using $startsWith
const queryWithStartsWith = await Collect.records.find('POST', {
  where: {
    title: { $startsWith: 'Understanding' }
  }
});
```

```typescript
// Complex example with multiple string operators
const queryWithStringOperators = await Collect.records.find('POST', {
  where: {
    $or: [
      { title: { $startsWith: 'Understanding' } },
      { title: { $contains: 'Graph' } },
      { title: { $endsWith: 'Databases' } }
    ]
  }
});
```

#### $in

The `$in` operator checks if a field's value is within a specified array of values.

##### Examples:
```typescript
// Example using $in (strings)
const queryWithInStrings = await Collect.records.find('AUTHOR', {
  where: {
    name: { $in: ['Jane Doe', 'John Smith'] }
  }
});
```

#### $nin

The `$nin` operator checks if a field's value is not within a specified array of values.

##### Examples:
```typescript
// Example using $nin (strings)
const queryWithNotInStrings = await Collect.records.find('AUTHOR', {
  where: {
    name: { $nin: ['Jane Doe', 'John Smith'] }
  }
});
```

## Notes

- You can use both `CollectModel` and `Collect` class instances to perform queries.
- Using logical operators allows building complex and precise queries.
- The examples provided showcase how to leverage these operators for querying data effectively.
