---
description: >-
  Confirmations are used to verify that the input from the User was understood
  correctly. These may be verbal or non-verbal.
---

# Confirmations

**Note:** The links to the various NeonSkill methods in this documentation point to the `mycroft-core` documentation. There is no material difference between them for the methods referenced and, since there is not publicly available `neon_core` documentation, we will reference `mycroft-core` instead.

May be verbal or non-verbal. See the [Voice User Interface Design Guidelines section on Confirmations](../voice-user-interface-design-guidelines/interactions-and-guidelines/confirmations)

Note that Neon has a common practice of enabling confirmation that an intent handler is doing something when the action might take some time.

```python
if get_user_prefs(message)['response_mode'].get('hesitation'):
    self.speak_dialog("check_updates")
```

## Non-verbal Confirmation

See [`acknowledge()`](https://mycroft-core.readthedocs.io/en/latest/source/mycroft.html#mycroft.MycroftSkill.acknowledge)
