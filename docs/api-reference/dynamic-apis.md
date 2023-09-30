---
sidebar_position: 7
---
# Dynamic APIs

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