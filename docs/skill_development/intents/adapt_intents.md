# Adapt Intents

Adapt is an intent parser, developed by Mycroft AI.
There is [official documentation](https://mycroft-ai.gitbook.io/docs/mycroft-technologies/adapt)
published by Mycroft, but this document addresses Adapt in the context of
creating a skill for Neon/OVOS/Mycroft "Classic".

## Intent Example

Adapt intents use a collection of vocabulary files to build an intent. Vocabularies
are specified in `.voc` files. The Alerts skill uses an Adapt intent
[to create an alarm](https://github.com/NeonGeckoCom/skill-alerts/blob/dev/__init__.py#L203-L208).

### Vocabulary Files

Using the alarm example, the `alarm.voc` file looks like:

```
alarm
alarms
wake me up
```

Each line contains a word or phrase with the meaning "alarm".

## Regex Files

Using the [CaffeineWiz skill as an example](https://github.com/NeonGeckoCom/skill-caffeinewiz/blob/932ea389313066ca5a0a8325eb233495d4dbb450/__init__.py#L161-L162),
a `.rx` file is used to specify a regular expression to extract a `drink` entity.

The intent_handler looks like:

```python
@intent_handler(IntentBuilder("CaffeineContentIntent")
                .require("query_caffeine").require("drink"))
```

And `drink.rx` looks like:

```
(of|in) (?P<drink>.*)
```

`drink` in the `intent_handler` refers to the regex group (not the filename) in
this case. If the user said "how much caffeine is in coca cola", the regex would
match `coca cola` as `drink` and in the message passed to the intent handler,
`message.data['drink'] == 'coca cola`.

## Registering an Intent

In the alarm example, our intent is registered with the `intent_handler` decorator:

```python
from ovos_utils.intents import IntentBuilder
from ovos_workshop.decorators import intent_handler

    @intent_handler(IntentBuilder("CreateAlarm").require("set")
                    .require("alarm").optionally("playable")
                    .optionally("weekdays").optionally("weekends")
                    .optionally("everyday").optionally("repeat")
                    .optionally("until").optionally("script")
                    .optionally("priority"))
    def handle_create_alarm(self, message: Message):
        """
        Intent handler for creating an alarm
        :param message: Message associated with request
        """
        pass
```

This creates an intent named "CreateAlarm" and specifies several vocabulary files
that are either required for the intent to match, or optionally present in the
utterance. Matched vocabulary is added to Message data, so `message.data['alarm']`
will contain "alarm", "alarms", or "wake me up" based on our vocabulary file.
If the user said "set an alarm for weekends at 9 AM", `message.data['weekends']` would
contain "weekends".

### IntentBuilder methods

There are three methods to register vocabulary to an Adapt Intent. Any vocabulary
that is found in a user utterance will be added to `message.data`.

#### require

This specifies that an utterance must contain this vocabulary to be considered a match.

#### optionally

This specifies that an utterance might contain this vocabulary.

#### one_of

This specifies that an utterance must contain at least one of the passed vocabularies.
