---
description: >-
  A Message represents a message in the Organization's database, that has been
  either sent or received.
---

# Message

### Message Object

A Message has the following attributes:

| Field Name | Type | Description |
| :--- | :--- | :--- |
| `id` | `string` | The GUID of the Message object. **Optional, should not be manually created** |
| `body` | `string` | The content of the Message. **Required** |
| `ServicechainId` | `string` | The GUID of the Servicechain that the Message has been sent using. **Required** |
| `contact` | `string` | The GUID of the Contact who is the recipient for an outbound message, or the author of an inbound message. **Required** |
| `is_reply_to` | `string | null` | The GUID of the Message that this Message is a response to, if it is part of a conversation. **Optional** |
| `direction` | `string` | Whether this message is inbound \(being received by Voluble\) or outbound \(being sent by Voluble\). **Must** be one of the valid [`MessageDirections`](message.md#messagedirections). **Required** |
| `sent_time` | `Date` | The date that the message was initially sent. **Required** |
| `message_state` | `string` | The sending-state of the message. Must be one of the valid [`MessageStates`](message.md#messagestates). **Required** |

```typescript
interface Message {
    body: string
    ServicechainId: string,
    contact: string
    is_reply_to?: string | null | undefined
    direction: MessageDirections,
    sent_time?: Date,
    message_state: MessageStates
}
```

### MessageDirections

`MessageDirections`is an `enum` containing the only allowed values for `Message.direction`. It signifies whether a message is inbound \(being received\) or outbound \(being sent\). 

| Value | Meaning |
| :--- | :--- |
| `INBOUND` | Signifies that the message has been sent by a Contact to Voluble. |
| `OUTBOUND` | Signifies that the message is being sent by Voluble to a Contact. |

```typescript
enum MessageDirections {
    INBOUND = "INBOUND",
    OUTBOUND = "OUTBOUND"
}
```

### MessageStates

`MessageStates` is an enum containing the only allowed values for `Message.message_state`. It signifies whether the message is in the process of being sent, has been sent successfully, has been read, or has failed.

| Value | Meaning |
| :--- | :--- |
| `MSG_PENDING` | Signifies that the Message is in the process of being created by Voluble and handed over to a worker for sending. |
| `MSG_SENDING` | Signifies that the Message has been handed over to a worker and being sent by a Service. |
| `MSG_DELIVERED_SERVICE` | Signifies that the Message has been successfully delivered to a Service for delivery to the Contact. |
| `MSG_DELIVERED_USER` | Signifies that the Message has been successfully delivered to the Contact |
| `MSG_READ` | Signifies that the Contact has read the Message. |
| `MSG_REPLIED` | Signifies that the Contact has replied to the Message. If this state is used, there should be another Message with the opposite `MessageDirection`, that specifies the ID of this Message in its `is_reply_to` field. |
| `MSG_FAILED` | Signifies that the Message could not be sent with the given Servicechain. |
| `MSG_ARRIVED` | Signifies that an inbound Message has been successfully parsed and received. |

```typescript
enum MessageStates {
    MSG_PENDING = "MSG_PENDING",
    MSG_SENDING = "MSG_SENDING",
    MSG_DELIVERED_SERVICE = "MSG_DELIVERED_SERVICE",
    MSG_DELIVERED_USER = "MSG_DELIVERED_USER",
    MSG_READ = "MSG_READ",
    MSG_REPLIED = "MSG_REPLIED",
    MSG_FAILED = "MSG_FAILED",
    MSG_ARRIVED = "MSG_ARRIVED"
}
```

