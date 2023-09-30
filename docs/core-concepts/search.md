---
sidebar_position: 7
---
# Search

Collect offers powerful filtering and searching capabilities to help you retrieve the exact records you need based on specific criteria. Below, we will explain the functionality and options available for filtering and searching records, along with multiple examples for each search operation.

## Search Architecture

In Collect, we love handy APIs. That's why its main API was carefully crafted for usefulness by design.

As you may already know, there are a few entities in Collect designed to hold and operate on data: [Records](/core-concepts/records), [Properties](/core-concepts/properties), and [Labels](/core-concepts/labels). Each of these entities relies on the same API design. This means that with just one payload, you can:

- Search for [Records](/core-concepts/records) based on provided criteria. 
- Search for [Properties](/core-concepts/properties) and its values based on provided criteria. 
- Search for related [Labels](/core-concepts/labels) based on provided criteria.

APIs that support search functionality based on this same DTO (Data Transfer Object) are marked as __`Search-Enabled`__. 
So, if you come across this mark in the documentation, rest assured that the results will be filtered based on the provided criteria.

Here is an example of a search payload. It can be seamlessly used with Search-enabled APIs.
```typescript
{
    sort: "lastActivity,desc",
    labels: [
        "CUSTOMER"
    ],
    properties: [
        {
            operation: "GE",
            name: "income",
            value: 5000
        },
        {
            operation: "CONTAINS",
            name: "name",
            value: "Alex"
        },
        {
            operation: "EQUALS",
            name: "eyeColor",
            value: ["black", "green"]
        },
        {
            name: "lastActivity",
            operation: "RANGE",
            min: {
                year: 2014,
                month: 1,
                day: 27
            },
            max: "2023-11-09T00:00:00Z"
        }
    ]
}
```

## Search Parameters



| Name           | Value                           | Defaults           | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
|----------------|---------------------------------|--------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| offset         | `0 .. +∞`                       | `0`                | It can be zero or any positive integer, with a maximum gap of 1000 between `offset` and `limit`.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| limit          | `1 .. +∞`                       | `1000`             | It can be any positive integer greater than `offset` (if provided). The gap between `offset` and `limit` is limited to 1000.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| depth          | `"full"` or `1 .. +∞`           | `"full"`           | The lookup depth from the current searching level, assuming you have [Nested Data](/core-concepts/nesting). The default is `"full"`, but it can also be any positive integer.                                                                                                                                                                                                                                                                                                                                                                                                                                |
| sort           | `string`                        | `"_,created,desc"` | Provides the ability to sort results based on Record properties. When using `_,` before the property name, sorting will be done on [internal property of Record](/core-concepts/records). When provided without `_,` results will be sorted based on [Property](/core-concepts/properties) `name` and in the direction specified after the last comma, for example, `"price,asc"`. Sorting direction can be either `asc` or `desc`.                                                                                                                                                                          |
| labels         | `Array<string>`                 | `[]`               | List of [Labels](/core-concepts/labels) to search for. An empty array or undefined value will result in searching for every label. To match only unlabeled [Record](/core-concepts/records) (and related) data, pass `["__COLLECT__UNLABELED__"]`.                                                                                                                                                                                                                                                                                                                                                           |
| compact        | `boolean`                       | `false`            | When provided as `true`, it will add `__` (double underscore) to every [internal property of Record](/core-concepts/records) and flattens [Properties](/core-concepts/properties) into a top-level dictionary (object).                                                                                                                                                                                                                                                                                                                                                                                      |
| onlyProperties | `boolean`                       | `false`            | In combination with `compact: true` and when provided as `true`, it removes [internal properties of Record](/core-concepts/records) and flattens [Properties](/core-concepts/properties) into a top-level dictionary (object).                                                                                                                                                                                                                                                                                                                                                                               |
| combineMode    | `"AND"`, `"OR"`,`"and"`, `"or"` | `"AND"`            | Affects how querying will combine the provided `properties` criteria. By default, it is set to `AND`, which means that every [Property](/core-concepts/properties) criteria should be present in the desired output. When set to `OR`, it will treat every property from `properties` as necessary to have but not all together.                                                                                                                                                                                                                                                                             |
| properties     | `Array<Property>`               | `[]`               | The centerpiece of Search. Holds array of criteria which hold `name` of searching `property`, searching `operation` to match values with, and actual `value` criteria. <br/>  For `ranges` needs to provide boundaries: `min` or/and `max`. <br/> If `value` holds and array you can also specify `combineMode` and control how querying mechanism will work: By default, it is set to `OR`, which means that desired [Record](/core-concepts/records) should hold any of provided in array values. When set to `AND`, it will search for [Record](/core-concepts/records) that holds all of desired values. |


### Types-first explanation
Let's dive into type-first explanation of Search feature of Collect. Here is the representation of those parameters in TypeScript:

```typescript
type SearchDTO = {
    offset?: number
    limit?: number
    depth?: number | "full"
    
    compact?: boolean
    onlyProperties?: boolean
    
    labels?: Array<string>
    properties?: Array<Property>
    combineMode?: "AND" | "OR" | "and" | "or"
    sort?: string
}

// ---------------------------------------------------------------------
// Property
// ---------------------------------------------------------------------

type Value = string | number | boolean | null | Datetime

type Property = {
    name: string
    operation: "EQUALS" | "NOT_EQUALS" | "CONTAINS" | "NOT_CONTAINS"
    value: Value | Array<Value>
    combineMode?: "AND" | "OR"
} | {
    name: string
    operation: "LT" | "LE" | "GT" | "GE"
    value: number | Datetime | Array<number | Datetime>
    combineMode?: "AND" | "OR"
} | {
    name: string
    operation: "RANGE" | "EXCLUDE_RANGE"
    min?: number | Datetime
    max?: number | Datetime
}

// ---------------------------------------------------------------------
// Datetime
// ---------------------------------------------------------------------

type DatetimeObject = {
    year: number
    month?: number
    day?: number
    hour?: number
    minute?: number
    second?: number
    millisecond?: number
    microsecond?: number
    nanosecond?: number
}

type Datetime = DatetimeObject | string // ISO8601

```

### `offset`
It can be zero or any positive integer, with a maximum gap of 1000 between `offset` and `limit`.
### `limit`
It can be any positive integer greater than `offset` (if provided). The gap between `offset` and `limit` is limited to 1000.
### `depth`
The lookup depth from the current searching level, assuming you have [Nested Data](/core-concepts/nesting). The default is `"full"`, but it can also be any positive integer.
### `sort`
Provides ability to sort results based on [Record](/core-concepts/records) properties. with `_,` specified before property name sort will be done on [internal property of Record](/core-concepts/records).
By providing it without `_,` results will be sorted based on [Property](/core-concepts/properties) `name` and by order specified after last comma: `"age,desc"`.
### `labels`
List of [Labels](/core-concepts/labels) to search for. An empty array or undefined value will result in searching for every label. To match only unlabeled [Record](/core-concepts/records) (and related) data, pass `["__COLLECT__UNLABELED__"]`.
### `compact`
When provided as `true`, it will add `__` (double underscore) to every [internal property of Record](/core-concepts/records) and flatten [Properties](/core-concepts/properties) into a top-level dictionary (object).
### `onlyProperties`
In combination with `compact: true` and when provided as `true`, it removes [internal properties of Record](/core-concepts/records) and flattens [Properties](/core-concepts/properties) into a top-level dictionary (object).
### `combineMode`
Affects how querying will combine the provided `properties` criteria. By default, it is set to `AND`, which means that every [Property](/core-concepts/properties) criteria should be present in the desired output. When set to `OR`, it will treat every property from `properties` as necessary to have but not all together.
### `properties`
The centerpiece of Search. Holds array of criteria which hold `name` of searching `property`, searching `operation` to match values with, and actual `value` criteria. For `ranges` needs to provide boundaries: `min` or/and `max`. 
If `value` holds and array you can also specify `combineMode` and control how querying mechanism will work: By default, it is set to `OR`, which means that desired [Record](/core-concepts/records) should hold any of provided in array values. When set to `AND`, it will search for [Record](/core-concepts/records) that holds all of desired values.




[//]: # (## Supported Search Operations)

[//]: # ()
[//]: # (Collect supports various search operations, enabling you to apply filters based on different conditions. The following search operations are available:)

[//]: # ()
[//]: # (### `EQUALS`)

[//]: # (Perform exact matching of records with property values with the given input.)

[//]: # (### `NOT_EQUALS`)

[//]: # (Filter records with exact property values matching the given input.)

[//]: # ()
[//]: # (### `CONTAINS`)

[//]: # (Perform partial matching of records with property values with the given input.)

[//]: # (### `NOT_CONTAINS`)

[//]: # (Filter records with property values matching the given input.)

[//]: # ()
[//]: # (### `LT`, `LE`, `GT`, `GE`)

[//]: # (Perform comparisons &#40;less than, less than or equal to, greater than, greater than or equal to&#41; with `number` or `datetime` property values.)

[//]: # ()
[//]: # (### `RANGE`)

[//]: # (Filter records with property values that fall within a specified range.)

[//]: # (### `EXCLUDE_RANGE`)

[//]: # (Filter records with property values falling within a specified range.)

[//]: # ()
[//]: # ()


[//]: # ()
[//]: # ()
[//]: # (## Request Body &#40;POST Request&#41;)

[//]: # ()
[//]: # (To perform filtering and searching, include the `"properties"` object in the POST request body. Each property in the `"properties"` object corresponds to a specific search criterion.)

[//]: # ()
[//]: # (### Examples for Each Search Operation:)

[//]: # ()
[//]: # (#### 1. CONTAINS and EQUALS:)

[//]: # ()
[//]: # (```json)

[//]: # ([)

[//]: # (    {)

[//]: # (        "name": "name",)

[//]: # (        "operation": "CONTAINS",)

[//]: # (        "value": ["John", "Galt"],)

[//]: # (        "combineMode": "AND")

[//]: # (    },)

[//]: # (    {)

[//]: # (        "name": "city",)

[//]: # (        "operation": "EQUALS",)

[//]: # (        "value": "New York")

[//]: # (    })

[//]: # (])

[//]: # (```)

[//]: # ()
[//]: # (In this example, we are searching for records where the "name" property matches either "John" or "Doe" partially &#40;case-insensitive&#41; and where the "city" property matches "New York" exactly &#40;case-sensitive&#41;.)

[//]: # ()
[//]: # (#### 2. RANGE:)

[//]: # ()
[//]: # (```json)

[//]: # ([{)

[//]: # (    "name": "age",)

[//]: # (    "operation": "RANGE",)

[//]: # (    "min": 30,)

[//]: # (    "max": 50)

[//]: # (}])

[//]: # (```)

[//]: # ()
[//]: # (This example searches for records where the "age" property falls within the range of 30 to 50 &#40;inclusive&#41;.)

[//]: # ()
[//]: # (#### 3. NOT_EQUALS:)

[//]: # ()
[//]: # (```json)

[//]: # ({)

[//]: # (    "properties": {)

[//]: # (        "status": {)

[//]: # (            "operation": "NOT_EQUALS",)

[//]: # (            "value": ["archived", "inactive"],)

[//]: # (            "strict": false)

[//]: # (        })

[//]: # (    })

[//]: # (})

[//]: # (```)

[//]: # ()
[//]: # (In this example, we are excluding records where the "status" property matches "archived" or "inactive" partially &#40;case-insensitive&#41;.)

[//]: # ()
[//]: # (#### 4. EXACT_NOT_EQUALS:)

[//]: # ()
[//]: # (```json)

[//]: # ({)

[//]: # (    "properties": {)

[//]: # (        "country": {)

[//]: # (            "operation": "EXACT_NOT_EQUALS",)

[//]: # (            "value": "United States")

[//]: # (        })

[//]: # (    })

[//]: # (})

[//]: # (```)

[//]: # ()
[//]: # (This example NOT_EQUALSs records where the "country" property matches "United States" exactly &#40;case-sensitive&#41;.)

[//]: # ()
[//]: # (#### 5. NOT_EQUALS_RANGE:)

[//]: # ()
[//]: # (```json)

[//]: # ({)

[//]: # (    "properties": {)

[//]: # (        "price": {)

[//]: # (            "operation": "NOT_EQUALS_RANGE",)

[//]: # (            "min": 100,)

[//]: # (            "max": 500,)

[//]: # (            "strict": true)

[//]: # (        })

[//]: # (    })

[//]: # (})

[//]: # (```)

[//]: # ()
[//]: # (In this example, we are excluding records where the "price" property falls within the range of 100 to 500 &#40;inclusive&#41;.)

[//]: # ()
[//]: # (#### 6. LT, LE, GT, and GE:)

[//]: # ()
[//]: # (```json)

[//]: # ({)

[//]: # (    "properties": {)

[//]: # (        "score": {)

[//]: # (            "operation": "GT",)

[//]: # (            "value": 90)

[//]: # (        },)

[//]: # (        "rating": {)

[//]: # (            "operation": "LE",)

[//]: # (            "value": 4.5)

[//]: # (        })

[//]: # (    })

[//]: # (})

[//]: # (```)

[//]: # ()
[//]: # (In this example, we are searching for records where the "score" property is greater than 90 and the "rating" property is less than or equal to 4.5.)

[//]: # ()
[//]: # ()
[//]: # (## Strict Option in Filtering Records)

[//]: # ()
[//]: # (The `strict` option is available for various search operations, including `CONTAINS`, `EQUALS`, `NOT_EQUALS`, `EXACT_NOT_EQUALS`, `RANGE`, and `NOT_EQUALS_RANGE`. The `strict` option serves two different purposes, depending on the search operation being used. This documentation will explain the functionality of the `strict` option for each applicable search operation.)

[//]: # ()
[//]: # (### Strict Option for CONTAINS and EQUALS)

[//]: # ()
[//]: # (For `CONTAINS` and `EQUALS` operations, the `strict` option is used to filter records that have all of the desired criteria, combining them with an AND condition. When `strict` is set to `true`, all the search values provided in the `value` array must be present in the property's value for the record to match.)

[//]: # ()
[//]: # (**Example:**)

[//]: # ()
[//]: # (```json)

[//]: # ({)

[//]: # (    "properties": {)

[//]: # (        "tags": {)

[//]: # (            "operation": "CONTAINS",)

[//]: # (            "value": ["apple", "banana"],)

[//]: # (            "strict": true)

[//]: # (        })

[//]: # (    })

[//]: # (})

[//]: # (```)

[//]: # ()
[//]: # (In this example, the search is performed for records where the "tags" property contains both "apple" and "banana" as its values. Records with only one of these tags will not be included in the search results.)

[//]: # ()
[//]: # (### Strict Option for RANGE and NOT_EQUALS_RANGE)

[//]: # ()
[//]: # (For `RANGE` and `NOT_EQUALS_RANGE` operations, the `strict` option determines whether the equality comparison includes the desired bounds or not. When `strict` is set to `true`, the range comparison is strict, meaning that the values at the lower and upper bounds are not included in the search results. When `strict` is set to `false`, the range comparison is inclusive, and values at the lower and upper bounds are included in the search results.)

[//]: # ()
[//]: # (**Example:**)

[//]: # ()
[//]: # (```json)

[//]: # ({)

[//]: # (    "properties": {)

[//]: # (        "price": {)

[//]: # (            "operation": "RANGE",)

[//]: # (            "min": 100,)

[//]: # (            "max": 200,)

[//]: # (            "strict": true)

[//]: # (        })

[//]: # (    })

[//]: # (})

[//]: # (```)

[//]: # ()
[//]: # (In this example, the search will return records where the "price" property's value is greater than 100 and less than 200. Records with a price of exactly 100 or 200 will be NOT_EQUALSd from the search results.)

[//]: # ()
[//]: # (```json)

[//]: # ({)

[//]: # (    "properties": {)

[//]: # (        "rating": {)

[//]: # (            "operation": "NOT_EQUALS_RANGE",)

[//]: # (            "min": 3.5,)

[//]: # (            "max": 4.5,)

[//]: # (            "strict": false)

[//]: # (        })

[//]: # (    })

[//]: # (})

[//]: # (```)

[//]: # ()
[//]: # (In this example, the search will NOT_EQUALS records where the "rating" property's value is between 3.5 and 4.5, but it will include records with a rating of exactly 3.5 or 4.5 in the search results.)

[//]: # ()
[//]: # (### Conclusion)

[//]: # ()
[//]: # (Collect's filtering and searching functionality empowers you to efficiently query records based on complex criteria. By specifying the desired property, search operation, and associated values, you can precisely tailor your search to meet your data retrieval needs. With this robust search feature, you can effortlessly locate and access the data that matters most to your applications. Happy searching!)