---
description: >-
  A User is a member of an Organization, who can be authenticated in that
  Organization and send Messages on its behalf.
---

# User

A User has the following attributes:

| Field Name | Type | Description |
| :--- | :--- | :--- |
| `id` | `string` | The GUID of the Contact. **Optional, should not be manually created** |
| `OrganizationId` | `string` | The GUID of the Organization that this User is a member of. **Required** |
| `auth0_id` | `string` | The ID used by Auth0 \(the authentication provider\) to identify this user. **Optional** |
| `createdAt` | `Date` | The date that the User was created. **Optional, should not be created manually.** |
| `updatedAt` | `Date` | The date that the User was last updated. **Optional, should not be created manually.** |

```typescript
interface User {
    id?: string,
    createdAt?: Date,
    updatedAt?: Date,
    OrganizationId?: string,
    auth0_id: string,
}
```

