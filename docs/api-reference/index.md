---
sidebar_position: 1
---

# API Overview

Collect API provides reach options to work with data through the API.

There are different APIs to accomplish different things:

## Records API

```bash
POST /api/v1/record
```
```bash
GET|POST|PATCH|DELETE /api/v1/record/{recordId}
```
```bash
POST /api/v1/record/{recordId}/link
```
```bash
POST /api/v1/record/{recordId}/unlink
```
```bash
POST /api/v1/record/{recordId}/search
```
```bash
POST /api/v1/record/{recordId}/fields
```
```bash
POST /api/v1/record/fields
```
```bash
POST /api/v1/record/{recordId}/traverse-parents
```

## Properties API

```bash
GET /api/v1/property
```
```bash
POST /api/v1/record/properties
```
```bash
POST /api/v1/record/{entityId}/properties
```

## Labels API
```bash
POST /api/v1/label
```
```bash
POST /api/v1/label/{labelName}
```

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

## Export API
```bash
POST /api/v1/export/csv
```

## Dynamic API
```bash
GET|POST /api/v1/property/{name}/equal/{value}
```
```bash
GET|POST /api/v1/property/{name}/not-equal/{value}
```
```bash
GET|POST /api/v1/property/{name}/contains/{value}
```
```bash
GET|POST /api/v1/property/{name}/not-contain/{value}
```
```bash
GET|POST /api/v1/property/{name}/lt/{value}
```
```bash
GET|POST /api/v1/property/{name}/le/{value}
```
```bash
GET|POST /api/v1/property/{name}/gt/{value}
```
```bash
GET|POST /api/v1/property/{name}/ge/{value}
```
```bash
GET|POST /api/v1/property/{name}/range/{min}/{max}
```
```bash
GET|POST /api/v1/property/{name}/exclude-range/{min}/{max}
```