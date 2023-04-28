# Padadious Intents
Padatious is an intent parser, developed by Mycroft AI. 
There is [official documentation](https://mycroft-ai.gitbook.io/docs/mycroft-technologies/padatious)
published by Mycroft, but this document addresses Padatious in the context of
creating a skill for Neon/OVOS/Mycroft "Classic".

## Intents
Unlike Adapt intents, Padatious intents are specified as a list of potential user
utterances. These examples are specified in an `.intent` file. For example, the
Update Skill uses a Padatious intent [update_device.intent](https://github.com/NeonGeckoCom/skill-update/blob/ae8ac710bb133aa2b9805319e3d513bfb934e6c7/locale/en-us/intent/update_device.intent).

### Simple Intents
Below are some simple intents in our Update Skill example:
```
update my device
check for updates
are there any updates
i want you to update
```

Each line specifies something a user might say if their `intent` is to update
their device.

### Parentheses Expansion
Padatious intents can be specified with parenthetical values that are expanded.

From our Update Skill example:
```
update my( core| skills|)( software|)
```

The above line could be expanded to:
```
update my core software
update my core
update my skills software
update my skills
update my software
update my
```

Note that spaces are treated like any other character, so it's important to pay
attention to parentheses that contain a `None` option and where that might create
a missing or additional space.

### Extra Unknown Words
Sometimes you might want to account for an extra filler word in an intent. For
example, a user might say "um, check for updates" or "you update my device"; if
they used a button instead of a wake word to wake the device, they might say
"neon, check for updates. These unknown words can be accounted for with `:0` 
which specifies one word.

From our skill example:
```
:0 update my device
```

## Entities
It might be useful to extract a word or phrase from an utterance. Below is an
example from the [stock skill](https://github.com/NeonGeckoCom/skill-stock/blob/4f4b7c526994060c88c5e5031d8d4a6245f61acc/locale/en-us/intent/stock_price.intent):

```
(what is|tell me|i want to know) (the |) (share|stock|trade|trading) (price|value) for {company}
(tell me|i want to know) what {company} is trading at
what is {company} (trading at|stock price|share price)
```

In the above intent file, `company` is an entity, so the utterance "what is microsoft trading at"
would match the intent and include: `{"company": "microsoft"}` in the `Message` data.
> Note that entity names should always be specified as lowercase alpha characters.

### .entity files
If you wanted to limit valid values for `company` in the above example, you could
create a `company.entity` file that looks like:
```
microsoft
amazon
netflix
google
apple
```

This would limit `company` to only values in the `company.entity` file, so the
utterance "what is tesla trading at" would NOT match in this case.

## Registering an Intent
Once you have specified an `.intent` file, you can register an intent handler
method in the skill.

Using the [update skill example](https://github.com/NeonGeckoCom/skill-update/blob/ae8ac710bb133aa2b9805319e3d513bfb934e6c7/__init__.py#L130):
```python
from mycroft.skills import intent_file_handler
class UpdateSkill:
    @intent_file_handler("update_device.intent")
    def handle_update_device(self, message):
        """
        Handle a user request to check for updates.
        :param message: message object associated with request
        """
        pass
```

Note that the intent file basename, including the file extension, is passed to the decorator.

## Advantages over Adapt
The above intents could just as easily be added to a `.voc` file and then a user
request "update my device" would behave exactly the same. One of the advantages
of Padatious is that an intent is trained on examples, so where Adapt would match
"are there any updates in my inbox" because it contains "are there any updates",
Padatious is less likely to match that utterance.
