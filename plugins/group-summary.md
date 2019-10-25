# Plugins Introduction

## What's A Plugin?

A plugin in CrewText exists to interface between Voluble engine and a third-party message provider `Service`, such as Esendex or Telegram - these are the platforms that actually handle message delivery. This way, in order to be able to send messages for new platforms, all we have to do is write a plugin that interfaces with that platform.

Bear in mind, that the plugins are not running all of the time like separate processes - they are only instantiated when a message needs to be sent. For more information on how to handle inbound messages from users via the platform that your plugin interfaces with, see [Receiving Messages](receiving-messages.md).

Let's say a new messaging app is released, called **PidgeonChat**, and our users want to be able to send messages on it using CrewText. We'd have to write a plugin that exposes a set of methods that Voluble expects to be available, and calls the PidgeonChat APIs to send and receive messages on Voluble's behalf.

## What's In A Plugin?

Just one file: `plugin.js`, in its own subdirectory under the `/plugins`directory in the Voluble root. When it starts up, Voluble scans the `/plugins` directory for subdirectories and tries to import each `plugin.js` file it finds.

So, our **PidgeonChat** plugin would be found at `<voluble_root>/plugins/pidgeonchat/plugin.js`. That's not to say, of course, that the plugin can't have more code in other files alongside it - plugins can `require()` other files, so you can break up your codebase - but the entry point always has to be in `plugin.js`.

## What Does A Plugin Have To Do?

All plugins **must** inherit from the `plugin_base` abstract class provided by Voluble. This can be found in the `/plugins` folder. The constructor call requires two parameters: `plugin_name` and `plugin_description`. These are the user-friendly name and short description of what the plugin does.

All plugins **must** override the `send_message()` and `handle_incoming_message()`, and the module must return a function, which when called with no arguments, must return an instance of the plugin.

{% hint style="info" %}
For more information on the technical implementation of a plugin, check out the [Plugin APIs](plugin-apis.md) page.
{% endhint %}

