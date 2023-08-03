---
description: >-
  Neon Skills provide a number of methods to perform actions at different
  points during the lifecycle of the Class instance.
---

# Lifecycle Methods

The NeonSkill class that all Skills inherit from contains a number of methods that can be overridden by an instance of the Class. This enables a Skill to execute code at specific points in the lifecycle of a Skill. Each of these is optional, meaning none are required to be defined in your Skill.

`NeonSkill` may be imported from `neon_utils.skills.neon_skill`.

## **\_\_init\_\_**

The `__init__` method is called when the Skill is first constructed. It is often used to declare variables or perform setup actions, however it cannot utilize other NeonSkill methods and properties as the class does not yet exist. This includes `self.bus`and `self.settings` which must instead be called from your Skill's `initialize` method.

Th `__init__` method is optional, but if used, the `__init__` method from the Super Class \(NeonSkill\) must be called.

In the following example we assign a variable `learning` to be `True`. The variable is appended to the instance using `self` so that we can access this variable in any part of our Skill.

```python
    def __init__(self):
        super().__init__()
        self.learning = True
```

## Initialize

The `initialize` method is called after the Skill is fully constructed and registered with the system. It is used to perform any final setup for the Skill including accessing Skill settings.

_This is considered deprecated in the underlying OVOS packages and will eventually be removed. It is recommended to use the `__init__` method or a custom method instead._

In the following example we access the `my_setting` value, that would have been defined in the Skill's [`settingsmeta.json`](skill-settings.md). We use the `get` method in case the variable `my_setting` is undefined.

```python
    def initialize(self):
        # Perform an action on the handler method when Mycroft is ready
        self.bus.once("mycroft.ready", self.handler)

    # Dynamically get settings
    @property
    def my_setting(self):
        return self.settings.get('my_setting', 'default_value')
```

## Converse

The `converse` method can be used to handle follow up utterances prior to the normal intent handling process. It can be useful for handling utterances from a User that do not make sense as a standalone [intent](../user-interaction/intents/).

The method receives one argument:

- `message` \(Message\): The message object containing the utterance(s), language, user, and other information.

Once the Skill has initially been triggered by the User, the `converse` method will be called each time an utterance is received. It is therefore important to check the contents of the utterance to ensure it matches what you expected.

If the utterance is handled by the converse method, we return `True` to indicate that the utterance should not be passed onto the normal intent matching service and no other action is required by the system. If the utterance was not handled, we return `False` and the utterance is passed on first to other `converse` methods, and then to the normal intent matching service.

In the following example, we check that utterances is not empty, and if the utterance matches vocabulary from `understood.voc`. If the user has understood we speak a line from `great.dialog` and return `True` to indicate the utterance has been handled. If the vocabulary does not match then we return `False` as the utterance should be passed to the normal intent matching service.

```python
    def converse(self, message):
        if message.get("utterances") and self.voc_match(utterances[0], 'understood'):
            self.speak_dialog('great')
            return True
        else:
            return False
```

## Stop

The `stop` method is called anytime a User says "Stop" or a similar command. It is useful for stopping any output or process that a User might want to end without needing to issue a Skill specific utterance such as media playback or an expired alarm notification.

In the following example, we call a method `stop_beeping` to end a notification that our Skill has created.

```python
    def stop(self):
        self.stop_beeping()
```

## Shutdown

The `shutdown` method is called during the Skill process termination. It is used to perform any final actions to ensure all processes and operations in execution are stopped safely. This might be particularly useful for Skills that have scheduled future events, may be writing to a file or database, or that have initiated new processes.

In the following example we cancel a scheduled event and call a method in our Skill to stop a subprocess we initiated.

```python
    def shutdown(self):
        self.cancel_scheduled_event('my_event')
        self.stop_my_subprocess()
```
