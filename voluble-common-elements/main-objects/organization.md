---
description: >-
  An Organization represents a company or group that operates its own
  communications with its own Contacts and Users with its own phone number and
  other contact details.
---

# Organization

Note, the term has been shortened to `Org` in the interface declaration.

An Org has the following attributes:

| Field Name | Type | Description |
| :--- | :--- | :--- |
| `id` | `string` | The GUID of the Organization. **Optional, should not be created manually** |
| `name` | `string` | The user-friendly name of the Organization. **Required** |
| `phone_number` | `string` | The E164-formatted phone number of the Organization. **Required** |
| `createdAt` | `Date` | The date that the Org was created. **Optional, should not be created manually.** |
| `updatedAt` | `Date` | The date that the Org was last updated. **Optional, should not be created manually.** |

```typescript
interface Org {
    id?: string,
    createdAt?: Date,
    updatedAt?: Date,

    name: string,
    phone_number: string
}
```

