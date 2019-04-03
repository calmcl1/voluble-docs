---
description: >-
  A Servicechain is a list of Services that should be used to deliver a Message,
  in priority order.
---

# Servicechain

A Servicechain has the following attributes:

| Field Name | Type | Description |
| :--- | :--- | :--- |
| `id` | `string` | The GUID of the Servicechain object. **Optional, should not be manually created** |
| `name` | `string` | The user-friendly name of the Servicechain. **Required** |
| `OrganizationId` | `string` | The GUID of the Organization that this Servicechain belongs to. **Optional** |
| `createdAt` | `Date` | The date that the Servicechain was created. **Optional, should not be created manually.** |
| `updatedAt` | `Date` | The date that the Servicechain was last updated. **Optional, should not be created manually.** |

```typescript
interface Servicechain {
    id?: string,
    createdAt?: Date,
    updatedAt?: Date,
    name: string,
    OrganizationId?: string
}
```

