---
description: You can think of skills as the apps of the Neon system.
---

# What can a Skill do?

Skills give Neon the ability to perform a variety of functions. They can be installed or removed by the user, and can be easily updated to expand functionality. To get a good idea of what skills to build, let’s talk about the best use cases for a voice assistant, and what types of things Neon can do.

Neon can run on a variety of platforms from the Linux Desktop to [Mycroft.AI](https://mycroft.ai)'s dedicated Smart Speakers, the Mark I and Mark II. Different devices will have slightly different use cases. Devices in the home are generally located in the living room or kitchen and are ideal for listening to the news, playing music, general information, using timers while cooking, checking the weather, and other similar activities that are easily accomplished hands free.

## Basic functions

We cover a lot of the basics with our Default Skills, things like Timers, Alarms, Weather, Time and Date, and more.

## Information

We also call this General Question and Answer, and it covers all of those factual questions someone might think to ask a voice assistant. Questions like “who was the 32nd President of the United States?”, or “how tall is Eiffel Tower?” Although the Default Skills cover a great deal of questions there is room for more. There are many topics that could use a specific skill such as Science, Academics, Movie Info, TV info, and Music info, etc...

Generally, an Information/Question and Answer skill should be a Common Query skill. This skill type has not changed much from Mycroft so any existing skills should still work. For more information on Common Query skills, please see the [Common Query framework](../skill-types/common-query-framework.md).

## Media

One of the biggest use cases for Smart Speakers is playing media. The reason media playback is so popular is because it makes playing a song so easy, all you have to do is say “Hey Neon play the Beatles,” and you can be enjoying music without having to reach for a phone or remote.

Neon differs notably from Mycroft in that it is built on top of [Open Voice Operating System (OVOS)](https://openvoiceos.com), which means it uses OVOS Common Play (OCP) as its general-purpose media player. Most classic Mycroft skills are compatible with Neon but media skills are a notable difference.

For a list of current OCP skills, please see the following repository: [awesome-ocp-skills](https://github.com/OpenVoiceOS/awesome-ocp-skills)
For a quick reference guide to writing OCP skills, [please see the OVOS documentation](https://openvoiceos.github.io/community-docs/dev_ocp_skill/).

### Music

Most people will use their smart speaker to play music. OCP allows you to pass a media URL, either streaming or to download, and it will play the media. This means that you can play music from a variety of sources, including YouTube, Plex, and more. You can also play local music files.

### News

Much like listening to music, getting the latest news with a simple voice interaction is extremely convenient. Neon supports multiple news feeds, and has the ability to support multiple news skills.

Note that classic Mycroft news skills are still compatible with Neon, but newer skills can more easily be developed as part of the OCP framework.

### Radio

Radio is another popular media type. Neon can play radio stations from around the world, and can also play local radio stations from streaming websites. If a skill were written for it and a radio tuner were plugged into your smart speaker, that could also be used to play radio stations.

## Smart Home

Another popular use case for Voice Assistants is to control Smart Home and IoT products. Within our community there are skills for Home Assistant, Wink IoT, Lifx and more, but there are many products that we do not have skill for yet. The open source community has been enthusiastically expanding Neon's ability to voice control all kinds of smart home products.

## Games

Voice games are becoming more and more popular, especially those that allow multiple users to play together. Trivia games are some of the most popular types of games to develop for voice assistants. There are several games already available in the old Mycroft Marketplace. There is a port of the popular text adventure game Zork, a Crystal Ball game, and a Number Guessing game. When working on a game skill it might be worth creating a new Persona to be the host of your game.
