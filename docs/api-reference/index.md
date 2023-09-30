---
sidebar_position: 1
---

# API Overview

Collect API provides reach options to work with data through the API.

There are different APIs to accomplish different things:

## Records API
### Create
Create single [Record](/core-concepts/records). Learn more at [Records API](/api-reference/records#create).
```bash
POST /api/v1/record
```
Create single [Record](/core-concepts/records) under specific Record as child. Learn more at [Records API](/api-reference/records#create).
```bash
POST /api/v1/record/:id
```
### Read
Get single [Record](/core-concepts/records). Learn more at [Records API](/api-reference/records#read).
```bash
GET /api/v1/record/:id
```
Get list of all parents of [Record](/core-concepts/records). Learn more at [Records API](/api-reference/records#read).
```bash
GET /api/v1/record/:id/traverse-parents
```
### Update
Update single [Record](/core-concepts/records). Learn more at [Records API](/api-reference/records#update).
```bash
PATCH /api/v1/record/:id
```
### Delete
Delete single [Record](/core-concepts/records). Learn more at [Records API](/api-reference/records#delete).
```bash
DELETE /api/v1/record/:id
```
### Search
Retrieve [Record](/core-concepts/records) in the way you need them, followed by flexible criteria. Learn more at [Records API](/api-reference/records#search).
From `root` (all data in project will be searched according to provided parameters):
```bash
POST /api/v1/record/search
```
Search from a specific [Record](/core-concepts/records) to all its nested children (assuming you have [Nested Data](/core-concepts/nesting)):
```bash
POST /api/v1/record/:id/search
```
---

## Properties API
```bash
POST /api/v1/record/properties
```
```bash
POST /api/v1/record/:id/properties
```
```bash
POST /api/v1/property
```
---

## Labels API
```bash
POST /api/v1/label
```
```bash
POST /api/v1/label/:name
```
---

## Import API
```bash
POST /api/v1/collect/json
```
```bash
POST /api/v1/import/json
```
```bash
POST /api/v1/collect/csv
```
```bash
POST /api/v1/import/csv
```
---

## Export API
```bash
POST /api/v1/export/csv
```
---

## Dynamic API
### Equals
Find [Records](/core-concepts/records) that have a [Property](/core-concepts/properties) with the name `:name` and a value __equal__ to `:value`.
```bash
GET /api/v1/property/:name/equals/:value
```
```bash
POST /api/v1/property/:name/equals/:value
```

### Not Equals
Find [Records](/core-concepts/records) that have a [Property](/core-concepts/properties) with the name `:name` and a value __not equal__ to `:value`.
```bash
GET /api/v1/property/:name/not-equals/:value
```
```bash
POST /api/v1/property/:name/not-equals/:value
```
### Contains
Find [Records](/core-concepts/records) that have a [Property](/core-concepts/properties) with the name `:name` and a value __containing__ the substring `:value`.
```bash
GET /api/v1/property/:name/contains/:value
```
```bash
POST /api/v1/property/:name/contains/:value
```
### Not Contains
Find [Records](/core-concepts/records) that have a [Property](/core-concepts/properties) with the name `:name` and a value __not containing__ the substring `:value`.
```bash
GET /api/v1/property/:name/not-contains/:value
```
```bash
POST /api/v1/property/:name/not-contains/:value
```
### LT (Less Than)
Find [Records](/core-concepts/records) that have a [Property](/core-concepts/properties) with the name `:name` and a value (of type `number` or `datetime`) __less than__ `:value`.
```bash
GET /api/v1/property/:name/lt/:value
```
```bash
POST /api/v1/property/:name/lt/:value
```
### LE (Less Than or Equal)
Find [Records](/core-concepts/records) that have a [Property](/core-concepts/properties) with the name `:name` and a value (of type `number` or `datetime`) __less than or equal__ to `:value`.
```bash
GET /api/v1/property/:name/le/:value
```
```bash
POST /api/v1/property/:name/le/:value
```
### GT (Greater Than)
Find [Records](/core-concepts/records) that have a [Property](/core-concepts/properties) with the name `:name` and a value (of type `number` or `datetime`) __greater than__ `:value`.
```bash
GET /api/v1/property/:name/gt/:value
```
```bash
POST /api/v1/property/:name/gt/:value
```
### GE (Greater Than or Equal)
Find [Records](/core-concepts/records) that have a [Property](/core-concepts/properties) with the name `:name` and a value (of type `number` or `datetime`) __greater than or equal__ to `:value`.
```bash
GET /api/v1/property/:name/ge/:value
```
```bash
POST /api/v1/property/:name/ge/:value
```
### Range
Find [Records](/core-concepts/records) that have a [Property](/core-concepts/properties) with the name `:name` and a values (of type `number` or `datetime`) within the __range__ of `:value`.
```bash
GET /api/v1/property/:name/range/:min/:max
```
```bash
POST /api/v1/property/:name/range/:min/:max
```
### Exclude Range
Find [Records](/core-concepts/records) that have a [Property](/core-concepts/properties) with the name `:name` and a values (of type `number` or `datetime`) __outside the range__ of `:value`.
```bash
GET /api/v1/property/:name/exclude-range/:min/:max
```
```bash
POST /api/v1/property/:name/exclude-range/:min/:max
```
---

## Relations API
### Link 
Create relations between [Records](/core-concepts/records). Learn more at [Relations API](/core-concepts/records)
```bash
POST /api/v1/record/:id/link
```
### Unlink
Delete relations between [Records](/core-concepts/records). Learn more at [Relations API](/core-concepts/records)
```bash
POST /api/v1/record/:id/unlink
```