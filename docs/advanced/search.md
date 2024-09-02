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

### `offset`
It can be zero or any positive integer, with a maximum gap of 1000 between `offset` and `limit`.
### `limit`
It can be any positive integer greater than `offset` (if provided). The gap between `offset` and `limit` is limited to 1000.
### `sort`
Provides ability to sort results based on [Record](/core-concepts/records) properties. with `_,` specified before property name sort will be done on [internal property of Record](/core-concepts/records).
By providing it without `_,` results will be sorted based on [Property](/core-concepts/properties) `name` and by order specified after last comma: `"age,desc"`.
### `labels`
List of [Labels](/core-concepts/labels) to search for. An empty array or undefined value will result in searching for every label. To match only unlabeled [Record](/core-concepts/records) (and related) data, pass `["__COLLECT__UNLABELED__"]`.
### `where`
The centerpiece of Search. Holds array of criteria which hold `name` of searching `property`, searching `operation` to match values with, and actual `value` criteria. For `ranges` needs to provide boundaries: `min` or/and `max`. 
If `value` holds and array you can also specify `combineMode` and control how querying mechanism will work: By default, it is set to `OR`, which means that desired [Record](/core-concepts/records) should hold any of provided in array values. When set to `AND`, it will search for [Record](/core-concepts/records) that holds all of desired values.

