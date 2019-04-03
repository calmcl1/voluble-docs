---
description: >-
  A Service represents a third-party platform that can deliver a message by its
  own mechanism.
---

# Service

A Service has the following attributes:

| Field Name | Type | Description |
| :--- | :--- | :--- |
| `id` | `string` | The GUID of the Service object. **Optional, should not be manually created** |
| `name` | `string` | The user-friendly name of the Service. **Required** |
| `directory_name` | `string` | The name of the directory under the plugins directory that contains the plugin that interfaces with this Service. **Required** |
| `createdAt` | `Date` | The date that the Service was created. **Optional, should not be created manually.** |
| `updatedAt` | `Date` | The date that the Service was last updated. **Optional, should not be created manually.** |

```typescript
interface Service {
    id?: string,
    createdAt?: Date,
    updatedAt?: Date,

    name: string,
    directory_name: string,
}
```

