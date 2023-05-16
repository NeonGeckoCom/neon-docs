---
description: >-
  Explore the fundamental building blocks of a Skill, and the knowledge required
  to create meaningful and engaging voice interactions.
---

# Development Setup

Neon Skills are the voice applications that provide different functionality for users. To create a Skill requires at least basic technical experience, a Neon installation or device, and an idea of what your Skill will do, and how people will use it.

## Technical Requirements

### Python programming language

Skills for Neon are written using the [Python programming language](https://www.python.org/). A simple Skill can be a great way for new developers to try Python out in a real project, whilst experienced programmers will quickly see the powerful possibilities available in a well crafted Skill.

If you aren't familiar with the basics of Python, check out our [list of Python tutorials and resources](python-resources.md) to get you started. If you've programmed in other object-oriented languages, like Javascript or C\#, then you'll be able to pick it up, but if you're totally new to programming, you might want to look at an [introductory programming course](https://www.edx.org/course/introduction-computer-science-mitx-6-00-1x-11).

### Github account

Skills are hosted on [Github](https://github.com), so you will need to create an account there if you don't already have one. It is good to have an understanding of the [Github basics](https://guides.github.com/activities/hello-world/), however the [Neon Skills Kit](../../Neon-technologies/Neon-skills-kit.md) also automates some of the more complex aspects of managing a Github repository \(or repo\).

### Running Neon

To test your Skill out, you will need to have a Neon environment in which to work. Neon [can be installed on any hardware with Debian/Ubuntu](https://neon.ai/NeonAISDKUbuntuInstallation) or a dedicated device like the Mycroft Mark 2. If you don't have dedicated hardware or a virtual machine with Ubuntu, [Neon can also run on Windows Subsystem for Linux](https://neon.ai/WindowsOptionSubsystemLinux), if necessary. Neon also comes with a number of helpful tools to create new Skills, host them on Github, run integration tests, and submit them to the Neon community (and eventually Neon Marketplace). If you aren't yet familiar with how Neon works, check out the [overview of Neon architecture](https://neon.ai/SystemsArchitectureDiagram) to understand the many technologies that come together to provide an intelligent voice assistant. Neon is built on top of [OpenVoice OS (OVOS)](https://openvoiceos.com), a fork of Mycroft with many enhancements and bug fixes. [Their developer documentation](https://openvoiceos.github.io/ovos-technical-manual/) will usually also apply to Neon systems.

## What makes a good Skill?

### Fulfilling a need the user has

Good Skills meet one or more of the user's needs. Popular Skills are popular because people use them frequently - for instance, to set alarms, reminders, or to identify the time in other time zones. On the other hand, a Skill that, say, recites π to 100 digits might be pretty cool, but when was the last time you needed to know π to 100 digits? Contrast that with the last time you set a reminder on your phone.

### Having an easy to use voice interface

Just like a web page with a thoughtfully-designed interface is much more pleasant to use, a Skill with a well designed voice interface is a delight, not a chore, to use. You should anticipate the task the user is trying to accomplish, and how to make that as straightforward as possible.

If you haven't already, be sure to read our [Voice User Interface Design Guidelines](../voice-user-interface-design-guidelines).

If you have an idea for a Skill, it's a great idea to join [Neon Chat](https://matrix.to/#/#NeonMycroft:matrix.org), and share what your plans are. You'll be able to get constructive and helpful feedback on your Skill from an experienced community.

## Skill terminology

You'll notice some new terms as you start to develop Skills.

- **utterance** - An utterance is a phrase spoken by the User, after the User says the Wake Word. `what's the weather like in Toronto?` is an utterance.
- **dialog** - A dialog is a phrase that is spoken by Neon. Different Skills will have different dialogs, depending on what the Skill does. For example, in a _weather_ Skill, a dialog might be `the.maximum.temperature.is.dialog`.
- **intent** - Neon matches utterances that a User speaks with a Skill by determining an intent from the utterance. For example, if a User speaks `Hey Neon, what's the weather like in Toronto?` then the intent will be identified as _weather_ and matched with the _Weather Skill_. When you develop new Skills, you need to define new intents.

If you encounter anything else you're not familiar with, checkout the [Neon Glossary](../overview/definitions.md).
