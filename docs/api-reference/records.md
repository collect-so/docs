---
sidebar_position: 2
---

# Records API

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