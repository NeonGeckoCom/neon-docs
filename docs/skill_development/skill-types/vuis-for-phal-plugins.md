# Creating Voice User Interfaces for PHAL Plugins

Neon's enclosure module implements the Platform and Hardware Abstraction Layer (PHAL) from [OpenVoiceOS](https://github.com/OpenVoiceOS/ovos-PHAL). This service loads PHAL plugins that provide different functionality to the core; plugins primarily differ from skills in that they do not have any intents and that they may only be valid in certain core environments (i.e. only for particular hardware or operating system environments).

In order to implement a Voice User Interface (VUI) and provide intents to PHAL plugins, a separate skill must be created that integrates with the Message Bus events the PHAL plugin implements.

## Planning

PHAL plugins will have already implemented many Message Bus events and sometimes a GUI. In many ways this makes the developer's job simpler for the VUI, but planning ahead is still beneficial.

Ask yourself:

1. Which events are worth adding to the VUI?
   - Take note of these specific events in a file with your skill
   - Example: `ovos.phal.plugin.homeassistant.call.supported.function` or `ovos-PHAL-plugin-homeassistant.close`
2. Are there aspects of the GUI (if one exists) that could/should be voice controlled?
3. Is there new functionality that a VUI could contribute? Sometimes a PHAL plugin is created without thinking about how a VUI might work.
   - If so, the PHAL plugin will need a pull request to add the functionality.

Start designing the VUI per [this documentation](../introduction/your-first-skill.md).

## Implementing message bus events

Once you have a list of events to implement and a VUI design it's time to start implementing them in your skill. VUIs are just like any other skill but must take advantage of the message bus. They will only handle intents, localization, and emitting/receiving messages from the bus.

For illustration purposes, we will use the `ovos.phal.plugin.homeassistant.call.supported.function` event from [ovos-PHAL-plugin-homeassistant](https://github.com/OpenVoiceOS/ovos-PHAL-plugin-homeassistant/blob/dev/ovos_PHAL_plugin_homeassistant/__init__.py#L63).

Looking at the PHAL plugin, when this event is received, it calls `self.handle_call_supported_function()`.

```python
self.bus.on("ovos.phal.plugin.homeassistant.call.supported.function",
            self.handle_call_supported_function)
```

This `bus.on()` method tells the plugin that when the event in the first parameter is received, execute the method in the second parameter. This is important to understand not just for reading from the PHAL plugin but also to use later in the VUI.

The plugin handler method looks like this:

```python
def handle_call_supported_function(self, message):
    """ Handle the call supported function message

    Args:
        message (Message): The message object
    """
    device_id = message.data.get("device_id", None)
    function_name = message.data.get("function_name", None)
    function_args = message.data.get("function_args", None)
    if device_id is not None and function_name is not None:
        for device in self.registered_devices:
            if device.device_id == device_id:
                if function_args is not None:
                    response = device.call_function(
                        function_name, function_args)
                else:
                    response = device.call_function(function_name)
                self.bus.emit(message.response(data=response))
                return
    else:
        LOG.error("Device id or function name not provided")
```

We can see that the message should have a data section something like this:

```python
{
    "data": {
        "device_id": "",
        "function_name": "",
        "function_args": ""
    },
    ...
}
```

That means that our VUI must implement an intent handler that gathers that information and passes it to the message bus, like so:

```python
@intent_handler("lights.set.brightness.intent")
def handle_set_brightness_intent(self, message: Message):
    device, device_id = self._get_device_from_message(message)
    if device and device_id:  # If the intent doesn't understand the device, you'll get a device_id but no device
        brightness = message.data.get("brightness")
        call_data = {
            "device_id": device_id,
            "function_name": "turn_on",
            "function_args": {"brightness": self._get_ha_value_from_percentage_brightness(brightness)},
        }
        LOG.info(call_data)
        self.bus.emit(
            Message("ovos.phal.plugin.homeassistant.call.supported.function", call_data, message.context)
        )
        self.speak_dialog("acknowledge")
    else:
        self.speak_dialog("device.not.found", data={"device": device})
```

The key portion here is `self.bus.emit()`. This method expects the following parameters:

1. The name of the event
2. The data portion of the event. Whatever you pass in here will be the value of the `data` key in the event dictionary
3. The context. Usually optional, but depending on the complexity of your skill, it may be essential to know

## Avoiding race conditions

When developing the VUI, it's important to avoid any race conditions with the PHAL plugin. In most cases this won't be an issue, but sometimes you will want to only perform certain initialization features once you know the PHAL plugin is ready, or even load the plugin in a thread from the VUI if it's not loaded in the Neon system.

[The OCP plugin implements a ping-pong pattern](https://github.com/OpenVoiceOS/ovos-ocp-audio-plugin/blob/dev/ovos_plugin_common_play/__init__.py#L38) to wait for both sides of the service to be ready. Something similar could be implemented in your own VUI.

## Need more help?

If something isn't working as expected, please join us in the [Neon Chat](https://matrix.to/#/#NeonMycroft:matrix.org).

It's also really helpful for us if you add an issue to our [documentation repo](https://github.com/NeonGeckoCom/neon-docs/issues). This means we can make sure it gets covered for all developers in the future.
