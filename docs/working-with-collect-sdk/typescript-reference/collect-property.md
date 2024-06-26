---
sidebar_position: 8
---

# CollectProperty

The `CollectProperty` type defines the structure of a property within a model's schema in the Collect SDK. It specifies the attributes of each property, including its name, type, and any metadata associated with it.

### Type Definition
```typescript
type CollectProperty = {
  id: string;
  metadata?: string;
  name: string;
  type: 'boolean' | 'datetime' | 'null' | 'number' | 'string';
};
```

### Properties

#### id

- **Type:** `string`
- **Required:** Yes
- **Description:** A unique identifier for the property.

#### name

- **Type:** `string`
- **Required:** Yes
- **Description:** The name of the property.

#### type

- **Type:** `'boolean' | 'datetime' | 'null' | 'number' | 'string'`
- **Required:** Yes
- **Description:** The data type of the property.

#### metadata

- **Type:** `string`
- **Required:** No
- **Description:** Optional metadata associated with the property.

### Example Usage
```typescript
// Define a property within a model's schema
const userSchema = {
  id: { type: 'string' },
  name: { type: 'string' },
  age: { type: 'number' },
  isActive: { type: 'boolean' },
  createdAt: { type: 'datetime', metadata: 'ISO 8601' }
};

// Example of a CollectProperty
const nameProperty: CollectProperty = {
  id: '1',
  name: 'name',
  type: 'string'
};

const createdAtProperty: CollectProperty = {
  id: '2',
  name: 'createdAt',
  type: 'datetime',
  metadata: 'ISO 8601'
}
```