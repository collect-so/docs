---
sidebar_position: 7
---
# Search

:::tip
If you're seeking documentation for the Search API, you can find it by following the link to [Search API](/api-reference/search).
:::

Collect offers powerful filtering and searching capabilities to help you retrieve the exact records you need based on specific criteria. Below, we will explain the functionality and options available for filtering and searching records, along with multiple examples for each search operation.

## Search Architecture



## Supported Search Operations

Collect supports various search operations, enabling you to apply filters based on different conditions. The following search operations are available:

### `CONTAINS`
Perform partial matching of records with property values with the given input.
### `EQUALS`
Perform exact matching of records with property values with the given input.
### `NOT_CONTAINS`
Filter records with property values matching the given input.
### `NOT_EQUALS`
Filter records with exact property values matching the given input.
### `RANGE`
Filter records with property values that fall within a specified range.
### `EXCLUDE_RANGE`
Filter records with property values falling within a specified range.
### `LT`, `LE`, `GT`, `GE`
Perform comparisons (less than, less than or equal to, greater than, greater than or equal to) with `number` or `datetime` property values.




----



## Request Body (POST Request)

To perform filtering and searching, include the `"properties"` object in the POST request body. Each property in the `"properties"` object corresponds to a specific search criterion.

### Examples for Each Search Operation:

#### 1. CONTAINS and EQUALS:

```json
[
    {
        "name": "name",
        "operation": "CONTAINS",
        "value": ["John", "Galt"],
        "combineMode": "AND"
    },
    {
        "name": "city",
        "operation": "EQUALS",
        "value": "New York"
    }
]
```

In this example, we are searching for records where the "name" property matches either "John" or "Doe" partially (case-insensitive) and where the "city" property matches "New York" exactly (case-sensitive).

#### 2. RANGE:

```json
[{
    "name": "age",
    "operation": "RANGE",
    "min": 30,
    "max": 50
}]
```

This example searches for records where the "age" property falls within the range of 30 to 50 (inclusive).

#### 3. NOT_EQUALS:

```json
{
    "properties": {
        "status": {
            "operation": "NOT_EQUALS",
            "value": ["archived", "inactive"],
            "strict": false
        }
    }
}
```

In this example, we are excluding records where the "status" property matches "archived" or "inactive" partially (case-insensitive).

#### 4. EXACT_NOT_EQUALS:

```json
{
    "properties": {
        "country": {
            "operation": "EXACT_NOT_EQUALS",
            "value": "United States"
        }
    }
}
```

This example NOT_EQUALSs records where the "country" property matches "United States" exactly (case-sensitive).

#### 5. NOT_EQUALS_RANGE:

```json
{
    "properties": {
        "price": {
            "operation": "NOT_EQUALS_RANGE",
            "min": 100,
            "max": 500,
            "strict": true
        }
    }
}
```

In this example, we are excluding records where the "price" property falls within the range of 100 to 500 (inclusive).

#### 6. LT, LE, GT, and GE:

```json
{
    "properties": {
        "score": {
            "operation": "GT",
            "value": 90
        },
        "rating": {
            "operation": "LE",
            "value": 4.5
        }
    }
}
```

In this example, we are searching for records where the "score" property is greater than 90 and the "rating" property is less than or equal to 4.5.


## Strict Option in Filtering Records

The `strict` option is available for various search operations, including `CONTAINS`, `EQUALS`, `NOT_EQUALS`, `EXACT_NOT_EQUALS`, `RANGE`, and `NOT_EQUALS_RANGE`. The `strict` option serves two different purposes, depending on the search operation being used. This documentation will explain the functionality of the `strict` option for each applicable search operation.

### Strict Option for CONTAINS and EQUALS

For `CONTAINS` and `EQUALS` operations, the `strict` option is used to filter records that have all of the desired criteria, combining them with an AND condition. When `strict` is set to `true`, all the search values provided in the `value` array must be present in the property's value for the record to match.

**Example:**

```json
{
    "properties": {
        "tags": {
            "operation": "CONTAINS",
            "value": ["apple", "banana"],
            "strict": true
        }
    }
}
```

In this example, the search is performed for records where the "tags" property contains both "apple" and "banana" as its values. Records with only one of these tags will not be included in the search results.

### Strict Option for RANGE and NOT_EQUALS_RANGE

For `RANGE` and `NOT_EQUALS_RANGE` operations, the `strict` option determines whether the equality comparison includes the desired bounds or not. When `strict` is set to `true`, the range comparison is strict, meaning that the values at the lower and upper bounds are not included in the search results. When `strict` is set to `false`, the range comparison is inclusive, and values at the lower and upper bounds are included in the search results.

**Example:**

```json
{
    "properties": {
        "price": {
            "operation": "RANGE",
            "min": 100,
            "max": 200,
            "strict": true
        }
    }
}
```

In this example, the search will return records where the "price" property's value is greater than 100 and less than 200. Records with a price of exactly 100 or 200 will be NOT_EQUALSd from the search results.

```json
{
    "properties": {
        "rating": {
            "operation": "NOT_EQUALS_RANGE",
            "min": 3.5,
            "max": 4.5,
            "strict": false
        }
    }
}
```

In this example, the search will NOT_EQUALS records where the "rating" property's value is between 3.5 and 4.5, but it will include records with a rating of exactly 3.5 or 4.5 in the search results.

### Conclusion

Collect's filtering and searching functionality empowers you to efficiently query records based on complex criteria. By specifying the desired property, search operation, and associated values, you can precisely tailor your search to meet your data retrieval needs. With this robust search feature, you can effortlessly locate and access the data that matters most to your applications. Happy searching!