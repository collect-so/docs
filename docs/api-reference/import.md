---
sidebar_position: 5
---

# Import API

## Overview
The Import API allows you to push data and get it automatically normalized and labeled. This API is designed to receive 
`JSON`, `CSV`, `YML` and `XML` payloads and normalize the incoming data while optionally generating labels and suggesting
[data types](/core-concepts/data-types) for each field within the payload.

### Endpoint URLs
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

## Request

### Request Headers
- Content-Type: application/json

### Request Body
- The request body must be a JSON object conforming to the following Data Transfer Object (DTO):

```json
{
  "parentId": "string (optional)",
  "payload": {
    "field1": "value1",
    "field2": "value2"
  },
  "label": "string (optional)",
  "options": {
    "generateLabels": true/false,
    "suggestTypes": true/false
  }
}
```

#### Request Parameters
- `parentId` (optional): A string identifier for grouping related payloads together. Useful for organizing data hierarchically.
- `payload`: The JSON data that you want to collect, normalize, and label.
- `label` (optional): A custom label for the payload. If not provided, labels can be generated automatically based on the keys within the payload.
- `options`: An object containing options for data processing.
    - `generateLabels`: A boolean flag indicating whether to generate labels automatically based on keys that hold nested objects within the payload.
    - `suggestTypes`: A boolean flag indicating whether to suggest data types (e.g., boolean, number, string, datetime, null) based on the values within the payload.

## Response

### Success Response (200 OK)
- The API will respond with a JSON object containing the normalized and labeled data, along with a unique identifier for the collected data.

```json
{
  "dataId": "string (unique identifier)",
  "normalizedPayload": {
    "field1": "normalized_value1",
    "field2": "normalized_value2",
    ...
  },
  "label": "string (provided or generated label)",
  "dataTypes": {
    "field1": "string (suggested data type)",
    "field2": "string (suggested data type)",
    ...
  }
}
```

### Error Responses
- If the request is invalid or encounters an error, the API will respond with an appropriate error message along with the corresponding HTTP status code.

```json
{
  "error": "string (error message)"
}
```

## Example

### Request Example
```http
POST /api/v1/collect/json
Content-Type: application/json

{
  "parentId": "parent_id_123",
  "payload": {
    "name": "John Doe",
    "age": 30,
    "email": "johndoe@example.com"
  },
  "label": "User Data",
  "options": {
    "generateLabels": true,
    "suggestTypes": true
  }
}
```

### Response Example (200 OK)
```json
{
  "dataId": "data_id_456",
  "normalizedPayload": {
    "name": "John Doe",
    "age": 30,
    "email": "johndoe@example.com"
  },
  "label": "User Data",
  "dataTypes": {
    "name": "string",
    "age": "number",
    "email": "string"
  }
}
```

## Notes
- The API provides flexibility by allowing you to customize labels and enable or disable automatic label generation and data type suggestions as needed.
- The `dataId` returned in the response can be used to reference or retrieve the collected data in the future.
- Ensure that the request payload conforms to the specified JSON format to ensure successful data processing.