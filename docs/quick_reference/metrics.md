# Neon Metrics
Neon collects and exposes a number of metrics that can be used to monitor changes
in key performance areas like speech-to-text, text-to-speech, and intent handling.
Some of these metrics are contained in `Message.context` to measure complex
interactions, and others are sent as `neon.metric` Messages to report specific
measured metrics.

## Timing Context
Neon uses a combination of timestamps and directly measured durations to record
how long certain user interactions take to complete. Both timestamps and measured 
durations are added to `Message.context["timing"]` and can be used for manual or 
automated evaluation.

### Timestamps
Some known timestamps include the following. Note that any module that handles a
Message may add to or overwrite these metrics. These timestamps should always be
epoch seconds to avoid complexity.

- `audio_begin` - Time when audio playback began
- `audio_end` - Time when audio playback ended
- `client_sent` - Time when a client module sent an input
- `gradio_sent` - For gradio web clients only, the time when GradIO handled a user submission
- `handle_utterance` - Time when a `Message` object reaches `handle_utterance` in the skills service
- `response_sent` - Time when a core module emits a response to some input 
  (i.e. `neon.get_stt.response`, `klat.response`)
- `speech_start` - Time when a `speak` Message is emitted by a skill (DEPRECATED)
- `transcribed` - DEPRECATED in favor of `handle_utterance`

### Measured Durations
Some specific processes are timed with measured timings added to timing context.
Note that any module that handles a Message my add to or overwrite these metrics.
All durations are in seconds.

- `get_stt`: Time for STT plugin to evaluate audio and return transcript(s)
- `get_tts`: Time for TTS plugin to evaluate a string and return audio
- `iris_input_handling`: Time spent in Iris between user submission and MQ emit (can be affected by MQ re-connection on emit failure)
- `mq_response_handler`: Time for messagebus-mq connector to translate a Message into serialized data
- `mq_from_core`: Time from the core response Message to reach the MQ connector module (Messagebus transit time)
- `mq_from_client`: Time for the client message to reach the MQ connector module (MQ Bus transit time)
- `mq_input_handler`: Time for MQ connector module to process MQ input into a valid Message
- `client_to_core`: Time from the client emit event to the core Messagebus event handler
- `client_from_core`: Time from the core `klat.response` event emit to the client handler
- `save_transcript`: Time to save transcripts of input
- `text_parsers`: DEPRECATED in favor of `transform_utterance`
- `transform_audio`: Time for Speech Transformers service to evaluate input
- `transform_utterance`: Time for Utterance Transformer service to execute
- `wait_in_queue`: Time spent in gradio queue, waiting to be processed
