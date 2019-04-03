---
description: >-
  A ServicesInSC object is the entry in a given Servicechain that specifies
  which Services exist in the Servicechain, and what their priority is for
  sending a message.
---

# ServicesInSC

This represents a single entry in a Servicechain, detailing which Service is in the Servicechain, and it's priority in the Servicechain. Therefore, if a Message is to be sent using a Servicechain that has an `id` of `ABCDEFG-ABCD-ABCD-ABCD-ABCDEFGHIJKL`, then by finding the ServicesInSCs with this value as the `servicechainId`, and sorting them by `priority`, then we can construct the list of Services that we should use to send the Message.

A Servicechain has the following attributes:

| Field Name | Type | Description |
| :--- | :--- | :--- |
| `id` | `string` | The GUID of the ServicesInSC object. **Optional, should not be manually created** |
| `servicechainId` | `string` | The GUID of the Servicechain that this ServicesInSC entry relates to. **Required** |
| `serviceId` | `string` | The GUID of the Service that this ServicesInSC entry relates to. **Required** |
| `priority` | `number` | The position in the Servicechain \(starting at 1 and working upwards\) of this Service in the Servicechain. |
| `createdAt` | `Date` | The date that the ServicesInSC was created. **Optional, should not be created manually.** |
| `updatedAt` | `Date` | The date that the ServicesInSC was last updated. **Optional, should not be created manually.** |

```typescript
interface ServicesInSC {
    id?: string,
    createdAt?: Date,
    updatedAt?: Date,
    servicechainId: string,
    serviceId: string,
    priority: number
}
```

