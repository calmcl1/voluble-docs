---
description: >-
  A Contact represents a person in the Organization's database, to whom a
  message might be sent.
---

# Contact

A Contact has the following attributes:

| Field Name | Type | Description |
| :--- | :--- | :--- |
| `id` | `string` | The GUID of the Contact. **Optional, should not be manually created** |
| `first_name` | `string` | The first name of the contact. **Required** |
| `surname` | `string` | The last name of the contact. **Required** |
| `email_address` | `string` | The contact's valid email address. **Required** |
| `phone_number` | `string` | The contact's E164-formatted phone number. **Required** |
| `ServicechainId` | `string` | The GUID of the default [Servicechain](servicechain.md) of the Contact. **Optional** |
| `OrganizationId` | `string` | The GUID of the Organization that the Contact belongs to. **Optional** |
| `createdAt` | `Date` | The date that the Contact was created. **Optional, should not be manually created** |
| `updatedAt` | `Date` | The date that the Contact was last updated. **Optional, should not be manually created** |

{% tabs %}
{% tab title="index.ts" %}
```typescript
interface Contact {
    first_name: string,
    surname: string,
    email_address: string,
    phone_number: string,
    ServicechainId?: string,
    OrganizationId?: string
    id?: string,
    createdAt?: Date,
    updatedAt?: Date
}
```
{% endtab %}
{% endtabs %}

