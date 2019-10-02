---
description: >-
  This section explains how Voluble receives messages from third-party Services,
  and how plugins should help to make them Voluble-friendly.
---

# Receiving Messages

## API Endpoint

Voluble can receive messages from the Service that a given plugin interacts with by calling the endpoint `https://<VOLUBLE-URL>/services/endpoint/<YOUR-PLUGIN-NAME>`with the data that the plugin expects to be able to handle, in order to extract the relevant information to provide to Voluble.

Let's say we've created a plugin, `my-sms-handler`, to interact with a Service that provides SMS messaging. We might expect that when a message is sent to the Voluble Organization's phone number, that we can configure the SMS provider to contact the endpoint `https://<VOLUBLE-URL>/services/endpoint/my-sms-handler` with the following request body:

```markup
<?xml version="1.0">
    <message>
        <from>"+447836820573"</from>
        <to>"+44029376927"</from>
        <body>"Hi, just received your message - I'm afraid I can't make the meeting on Thursday, sorry!</body>
        <timestamp>1570022793</timestamp>
    </message>
<?xml>
```

This tells us everything we need to know about the message, but Voluble can't handle it in this state. The plugin is required to handle incoming messages. To achieve this, the plugin **must** specify a method, `handle_incoming_message`.

```typescript
async handle_incoming_message(message_data: any): Promise<InterpretedIncomingMessage | null>
```

The method **must** accept only one argument, `message_data`, which will be the content of the request body verbatim as received by the API Endpoint for the plugin. For more information, check out the [handle\_incoming\_message documentation](plugin-apis.md#handle_incoming_message) of the [Plugin APIs](plugin-apis.md).

