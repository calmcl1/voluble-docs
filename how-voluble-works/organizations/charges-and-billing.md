# Charges and Billing

## Plan Charges

Each `Organization` will be charged a flat, recurring fee based on the plan that they have subscribed to. Plan details are not yet finalized, but they will vary depending on the amount of `Contacts` that are able to be stored, the amount of `Users` that are available, and perhaps the amount of `Messages` per month that can be sent.

All plans include a set quota of free `Messages` per month that don't count against an `Organizations`' credit balance.

## Per-Message Charges

Voluble uses a 'credit' system to keep track of each [`Organizations`](./)' messaging. For every [`Message`](../messages.md) that is sent successfully, one credit is billed against the `Organization`. It doesn't matter how many attempts are made to send the `Message`, or how many steps through the `Servicechain` are required, a `Message` will only cost one credit. If the `Message` cannot be sent, then no credits will be counted against the `Organization`.

### Purchasing Credits

Credits can be purchased in two ways - either via our '**Light**' or '**Full**' methods.

#### Light Credit Purchasing

This is effectively a 'pay-as-you-go' method of purchasing credits.

#### Full Credit Purchasing

This is a 'pay-in-advance' method.

