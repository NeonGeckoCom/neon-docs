# Neon Core
Neon Core is a collection of services that comprise the Neon AI® Assistant.

## neon_messagebus
The messagebus is the service that enables communication among all the core modules.
The messagebus also serves as an entrypoint for MQ connections and provides the 
`SignalManager` for [IPC](https://en.wikipedia.org/wiki/Inter-process_communication).

## neon_speech
The speech service is where audio input is handled. Wake Word detection, Speech-to-Text (STT),
and input audio parsing all happens here.

## neon_audio
The audio service deals with audio outputs. Text-to-Speech (TTS) and audio playback
is managed in this module.

## neon_skills
The skills service is where user input is parsed and one or more responses are
generated. Text parsers may optionally modify input utterances before intent
parsing here, an intent is determined, and then the intent handler generates a
response.

## neon_gui
The GUI service handles interaction with a [GUI Application](https://github.com/MycroftAI/mycroft-gui).
This module is optional and may be omitted from voice-only configurations.

## neon_enclosure
The enclosure service implements a Platform/Hardware Abstraction Layer (PHAL).
This module is optional and handles things like volume controls, updates, power,
geolocation, and other platform-specific optional components.
