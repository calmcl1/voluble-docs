---
description: An introduction to the basic entities that Voluble uses
---

# How Voluble Works

## Voluble's Structure

The structure of Voluble is fairly straightforward: an `Organization` \(`org` for short\) is an entity that represents a company or group that intends to send messages to its members.

The `Organization` can have a number of `Contacts` to whom various messages can be sent, either in bulk or individually. If messages are sent in bulk, the recipient `Contact` isn't made aware of any messages being sent to any other `Contact` - it's the same message sent individually to each `Contact`.

On the other side, the `Organization` can have a number of `Users` - individual people who are authorized to send messages on the `Organizations`' behalf. These might be staff members or team managers who need to communicate to the various `Contacts`. There can be as many `Users` as you like - dozens, or just one.

{% hint style="info" %}
For more information on the technical details of each entity - `Contacts`, `Organizations`, and so on, check out the [Main Objects](voluble-common-elements/main-objects/) page.
{% endhint %}

## Sending Messages

Now that we have an `Organization` with `Users` and `Contacts` set up, we need a way of sending a `Message`.

### Services

The most straightforward way of sending a message with Voluble is an SMS. This is why Voluble [requires a phone number](voluble-common-elements/main-objects/contact.md) when adding a new `Contact`. Voluble doesn't connect to the phone network itself - it's not another version of Twilio or Esendex or Sinch, etc. Instead, Voluble uses providers like these \(Esendex by default\) to transfer your `Message` to it's intended recipient.

These third-party providers are called `Services` in Voluble. As there are many third-party messaging providers \(Telegram, WhatsApp, Facebook Messenger, and so on\), Voluble can interface with most of them to deliver a message. It doesn't have to be just SMS! This is at the heart of Voluble's flexibility.

There's one `Service` set up in Voluble by default - `esendex`. This interfaces with the SMS provider Esendex, in order to be able to send SMS messages to your `Contacts`.

### Servicechains

Having an SMS provider by default is all well and good, but what about the other third-party plugins that Voluble can interface with? It might well be that some of your `Contacts` would prefer their message to be sent as an email, or as a WhatsApp or Telegram message.

That's straightforward enough, but mixing-and-matching delivery services for different `Contacts` is where it gets complicated. Some of your `Contacts` might prefer different delivery systems to some of your other `Contacts`, and some might just prefer straight-up SMS. Setting up each of these for every `Contact` isn't straightforward - especially when they change their mind, or delete an messaging account without telling you!

Therefore, we created a `Servicechain`. A`Servicechain` is a priority-ordered list of `Services` that Voluble can use to attempt to deliver a message. Let's say that you created a `Servicechain` that looks like the following:

```text
1. Telegram
2. Esendex
3. Email
```

This means, that when you try and send a `Message` to your `Contacts`, it'll first try and use Telegram. If that's successful, then the `Message` is delivered. However, if \(say\) you don't have a Telegram handle for your `Contact`, or if the message couldn't be delivered, then Voluble will try again by sending the same `Message` as an SMS \(via Esendex\). If that doesn't work \(it bounces or their number has changed\), Voluble will try one last time by sending the `Message` as an email.

You can have different `Servicechains` for different `Contacts` or groups of `Contacts` if you wish, or you can keep everyone on the same `Servicechain`. This flexibility is Voluble's way of trying to ensure that everyone receives your `Message`, one way or another - and you can control how, while offering your `Contacts` the option of having their `Messages`  delivered how they prefer.

