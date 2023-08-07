---
description: >-
  An intent is the task the user intends to accomplish when they say something.
  The role of the intent parser is to extract from the user's speech key data
  elements that specify their intent.
---

# Intents

A user can accomplish the same task by expressing their intent in multiple ways. The role of the intent parser is to extract from the user's speech key data elements that specify their intent in more detail. This data can then be passed to other services, such as Skills to help the user accomplish their intended task.

_Example_: Julie wants to know about today's weather in her current location, which is Melbourne, Australia.

> "hey Neon, what's today's weather like?"
>
> "hey Neon, what's the weather like in Melbourne?"
>
> "hey Neon, weather"

Even though these are three different expressions, for most of us they probably have roughly the same meaning. In each case we would assume the user expects Neon to respond with today's weather for their current location. The role of an intent parser is to determine what this intent is.

In the example above, we might extract data elements like:

- **weather** - we know that Julie wants to know about the weather, but she has not been specific about the type of weather, such as _wind_, _precipitation_, _snowfall_ or the risk of _fire danger_ from bushfires. Melbourne, Australia rarely experiences snowfall, but falls under bushfire risk every summer.
- **location** - Julie has stipulated her location as Melbourne, but she does not state that she means Melbourne, Australia. How do we distinguish this from Melbourne, Florida, United States?
- **date** - Julie has been specific about the _timeframe_ she wants weather data for - today. But how do we know what today means in Julie's timezone. Melbourne, Australia is between 14-18 hours ahead of the United States. We don't want to give Julie yesterday's weather, particularly as Melbourne is renowned for having changeable weather.

It is up us as Skill creators to teach Neon the variety of ways that a user might express the same intent. This is a key part of the design process. It is the key difference between a Skill that kind of works if you know what to say, and a Skill that feels intuitive and natural to talk to.

This is handled by an intent parser whose job it is to learn from your Skill what intents it can handle, and extract from the user's speech any key information that might be useful for your Skill. In this case it might include the specified date and location.

## Neon's Intent Parsing Engines

Neon has three separate Intent parsing engines each with their own strengths. Each of these can be used in most situations, however they will process the utterance in different ways.

[**Padatious**](https://github.com/MycroftAI/padatious) is a light-weight neural network that is trained on whole phrases. Padatious intents are generally more accurate however require you to include sample phrases that cover the breadth of ways that a User may ask about something.

[**Padacioso**](https://github.com/OpenVoiceOS/padacioso) is a drop-in replacement for Padatious, built by the OVOS team to resolve several bugs and enhance features of Padatious. The API is the same as Padatious.

[**Adapt**](https://github.com/MycroftAI/adapt) is a keyword based parser. It is more flexible, as it detects the presence of one or more keywords in an utterance, however this can result in false matches.

We will now look at each in more detail, including how to use them in a Neon Skill.
