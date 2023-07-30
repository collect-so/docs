---
sidebar_position: 6
---

# Search 

## Documentation: Filtering and Searching Records in Our Database Solution

Collect offers powerful filtering and searching capabilities to help you retrieve the exact records you need based on specific criteria. \Below, we will explain the functionality and options available for filtering and searching records, along with multiple examples for each search operation.

### Supported Search Operations

The `parsePropertyParam` function supports various search operations, enabling you to apply filters based on different conditions. The following search operations are available:

1. `MATCH` and `EXACT_MATCH`: Perform exact or partial matching of property values with the given input.
2. **RANGE:** Filter property values that fall within a specified range.
3. **EXCLUDE:** Exclude records with property values matching the given input.
4. **EXACT_EXCLUDE:** Exclude records with exact property values matching the given input.
5. **EXCLUDE_RANGE:** Exclude records with property values falling within a specified range.
6. **LT, LE, GT, and GE:** Perform comparisons (less than, less than or equal to, greater than, greater than or equal to) with numeric property values.

### Request Body (POST Request)

To perform filtering and searching, include the `"properties"` object in the POST request body. Each property in the `"properties"` object corresponds to a specific search criterion.

### Examples for Each Search Operation:

#### 1. MATCH and EXACT_MATCH:

```json
{
    "properties": {
        "name": {
            "operation": "MATCH",
            "value": ["John", "Doe"],
            "strict": true
        },
        "city": {
            "operation": "EXACT_MATCH",
            "value": "New York"
        }
    }
}
```

In this example, we are searching for records where the "name" property matches either "John" or "Doe" partially (case-insensitive) and where the "city" property matches "New York" exactly (case-sensitive).

#### 2. RANGE:

```json
{
    "properties": {
        "age": {
            "operation": "RANGE",
            "min": 30,
            "max": 50,
            "strict": true
        }
    }
}
```

This example searches for records where the "age" property falls within the range of 30 to 50 (inclusive).

#### 3. EXCLUDE:

```json
{
    "properties": {
        "status": {
            "operation": "EXCLUDE",
            "value": ["archived", "inactive"],
            "strict": false
        }
    }
}
```

In this example, we are excluding records where the "status" property matches "archived" or "inactive" partially (case-insensitive).

#### 4. EXACT_EXCLUDE:

```json
{
    "properties": {
        "country": {
            "operation": "EXACT_EXCLUDE",
            "value": "United States"
        }
    }
}
```

This example excludes records where the "country" property matches "United States" exactly (case-sensitive).

#### 5. EXCLUDE_RANGE:

```json
{
    "properties": {
        "price": {
            "operation": "EXCLUDE_RANGE",
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

The `strict` option is available for various search operations, including `MATCH`, `EXACT_MATCH`, `EXCLUDE`, `EXACT_EXCLUDE`, `RANGE`, and `EXCLUDE_RANGE`. The `strict` option serves two different purposes, depending on the search operation being used. This documentation will explain the functionality of the `strict` option for each applicable search operation.

### Strict Option for MATCH and EXACT_MATCH

For `MATCH` and `EXACT_MATCH` operations, the `strict` option is used to filter records that have all of the desired criteria, combining them with an AND condition. When `strict` is set to `true`, all the search values provided in the `value` array must be present in the property's value for the record to match.

**Example:**

```json
{
    "properties": {
        "tags": {
            "operation": "MATCH",
            "value": ["apple", "banana"],
            "strict": true
        }
    }
}
```

In this example, the search is performed for records where the "tags" property contains both "apple" and "banana" as its values. Records with only one of these tags will not be included in the search results.

### Strict Option for RANGE and EXCLUDE_RANGE

For `RANGE` and `EXCLUDE_RANGE` operations, the `strict` option determines whether the equality comparison includes the desired bounds or not. When `strict` is set to `true`, the range comparison is strict, meaning that the values at the lower and upper bounds are not included in the search results. When `strict` is set to `false`, the range comparison is inclusive, and values at the lower and upper bounds are included in the search results.

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

In this example, the search will return records where the "price" property's value is greater than 100 and less than 200. Records with a price of exactly 100 or 200 will be excluded from the search results.

```json
{
    "properties": {
        "rating": {
            "operation": "EXCLUDE_RANGE",
            "min": 3.5,
            "max": 4.5,
            "strict": false
        }
    }
}
```

In this example, the search will exclude records where the "rating" property's value is between 3.5 and 4.5, but it will include records with a rating of exactly 3.5 or 4.5 in the search results.

### Conclusion

Collect's filtering and searching functionality empowers you to efficiently query records based on complex criteria. By specifying the desired property, search operation, and associated values, you can precisely tailor your search to meet your data retrieval needs. With this robust search feature, you can effortlessly locate and access the data that matters most to your applications. Happy searching!