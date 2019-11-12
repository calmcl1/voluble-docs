---
description: >-
  This page explains what a Voluble plugin is required to do, and the methods it
  must override.
---

# Plugin APIs

{% hint style="info" %}
All of the code samples on this page will be in TypeScript, so as to better explain the parameters and return types of each method.
{% endhint %}

## Initialization

To start with, we need to export a function that returns an instance of our plugin class. This plugin class **must** extend from the `voluble_plugin` class found in the `plugin_base` file in `/plugins`.

The `super()` call expects two parameters:

| Parameter Name | Type | Description |
| :--- | :--- | :--- |
| `plugin_name` | `string` | The user-friendly name of the plugin. **Required** |
| `plugin_description` | `string` | A short description of what the plugin does. |

{% tabs %}
{% tab title="plugin.ts" %}
```typescript
import * as plugin_base from '../plugin_base'
class ExamplePlugin extends plugin_base.voluble_plugin{
    constructor(){
        super('ExamplePlugin', 'Interfaces with the Example Messaging Platform')
    }
}

module.exports = ()=>{ return new ExamplePlugin() }
```
{% endtab %}
{% endtabs %}

## Methods

A plugin also needs to expose various methods, in order to send messages and handle incoming ones.

### `send_message`

In order to send a message, the plugin class must expose the method `send_message`.

#### Parameters

| Parameter Name | Type | Description |
| :--- | :--- | :--- |
| `message` | `plugin_base.messageInstance` | An object that represents the message to be sent. Functionally equivalent to the [Message](../voluble-common-elements/main-objects/message.md) object, which `plugin_base` effectively re-exports. **Required** |
| `contact` | `plugin_base.contactInstance` | An object that represents the recipient of the message, including their relevant contact details. Functionally equivalent to the [Contact](../voluble-common-elements/main-objects/contact.md) object. **Required** |

#### Returns

<table>
  <thead>
    <tr>
      <th style="text-align:left">Type</th>
      <th style="text-align:left">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"><code>Promise&lt;boolean&gt; | boolean</code>
      </td>
      <td style="text-align:left">
        <p>Represents whether or not the message has successfully been sent.</p>
        <p><code>true</code> if successful, <code>false</code> otherwise.</p>
      </td>
    </tr>
  </tbody>
</table>{% tabs %}
{% tab title="plugin.ts" %}
```typescript
send_message(message: plugin_base.messageInstance, contact: plugin_base.contactInstance): boolean | Promise<boolean> {
    // Do something here that sends messages
    // This must either return a boolean or a Promise of a boolean.
    // The boolean represents whether or not a message has successfully been sent - true if it has, or false if there was an error.
}
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
For more info about the types of the `message` and `contact` parameters, see the [Common Elements](../voluble-common-elements/introduction.md).
{% endhint %}

### `handle_incoming_message`

Voluble exposes a REST API endpoint for every plugin, so that platforms can use this to provide push notifications, usually for notifications of inbound messages. Whenever this occurs, Voluble will call the `handle_incoming_message` method of the relevant plugin and pass on any data that has been transferred to Voluble, so that the plugin can interpret it.

For more detail on how this method is used, see the documentation on [receiving messages](receiving-messages.md).

#### Parameters

| Parameter Name | Type | Description |
| :--- | :--- | :--- |
| `message_data` | `any` | An object that has been passed to Voluble by the platform, for consumption by the plugin. Could potentially be null. **Required** |

#### Returns

| Type | Description |
| :--- | :--- |
| `Promise<InterpretedIncomingMessage> | InterpretedIncomingMessage | null` | If the notification from the platform represents an inbound message, it is the plugin's responsibility to parse the data and attempt to identify the message's author and the message content. Otherwise \(for example, if the notification was simply a service message,\) return `null`. |

{% tabs %}
{% tab title="plugin.ts" %}
```typescript
handle_incoming_message(message_data: any): Promise<InterpretedIncomingMessage> | InterpretedIncomingMessage | null{
    // This is called when a platform contacts the API endpoint that Voluble exposes for the plugin.
    // `message_data` is an object representing the data that has been POSTed to the endpoint.
    
    // This should either return null, or an InterpretedIncomingMessage (or a Promise representing one)
}
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
For more info about the `InterpretedIncomingMessage` type, check out the [documentation](plugin-apis.md#interpretedincomingmessage).
{% endhint %}

## Plugin Base Types

`plugin_base` exports various helpful interfaces for non-standard or Voluble-specific objects that may be used as parameters or return values for plugin methods. These are detailed below.

### `InterpretedIncomingMessage`

An `InterpretedIncomingMessage` is what a plugin should return when it receives an inbound message from a platform.

An `InterpretedIncomingMessage` should contain either`contact_id` or at least one of `phone_number` or `email_address`, so that if the plugin cannot determine the `contact_id`, then Voluble should be able to identify the `Contact` based upon the contacts database.

#### Attributes

| Field Name | Type | Description |
| :--- | :--- | :--- |
| `message_body` | `string` | The content of the message. **Required** |
| `contact_id` | `string` | The GUID of a Voluble `Contact` representing the author of the message. **Optional, but preferred** |
| `is_reply_to` | `string | null` | If the plugin can identify that this message is a reply to a previous outbound message, then this should contain the GUID of the `Message` that it is a reply to. If the plugin can identify that it is definitely not a reply to a previous message, then this should be `null`. If unsure, leave `undefined`. **Optional**. |
| `phone_number` | `string` | The E164-formatted phone number of the message author, if available. **Optional, unless `contact_id` is not specified.** |
| `email_address` | `string` | The valid email address of the message author, if available. **Optional, unless `contact_id` is not specified.** |

{% tabs %}
{% tab title="plugin\_base.ts" %}
```typescript
interface InterpretedIncomingMessage {
    message_body: string,
    contact_id?: string,
    is_reply_to?: string | null,
    phone_number?: string,
    email_address?: string
}
```
{% endtab %}
{% endtabs %}

