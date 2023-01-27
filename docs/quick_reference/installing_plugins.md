# Installing Plugins
Many components in Neon Core can be replaced with alternatives or added as plugins.
This guide provides a brief overview of how to replace some common components with
alternate plugins. Detailed documentation about available plugins can be found in the 
[OVOS Plugin Manager Docs](https://openvoiceos.github.io/community-docs/OPM/).
More information about configuration is available 
[in the Neon Docs](https://neongeckocom.github.io/neon-docs/quick_reference/configuration/).

## STT
STT plugins convert your recorded voice into text. More information about 
STT plugins and some known plugins can be found 
[in the OVOS Docs](https://openvoiceos.github.io/community-docs/stt_plugins/).

### Example
This example shows the steps you would take to install and configure the
[Deepspeech STT Plugin](https://github.com/NeonGeckoCom/neon-stt-plugin-deepspeech_stream_local).

```shell
# Install the plugin package
pip install neon-stt-plugin-deepspeech-stream-local
# Configure Neon to use the new plugin
nano ~/.config/neon/neon.yaml
```

Add or update the user configuration to use the new plugin and configure the plugin.

```yaml
stt:
  # Set the module to the Plugin's entrypoint.
  # This is usually in the plugin's `README.md` file, or it can be found in `setup.py`
  module: deepspeech_stream_local
  deepspeech_stream_local:
    model_path: ~/.local/share/neon/deepspeech-0.8.1-models.pbmm
    scorer_path: ~/.local/share/neon/deepspeech-0.8.1-models.scorer
```

Restart core services after making this change to reload with these changes.

## TTS
TTS plugins convert skill responses into audio. More information about TTS plugins
and some known plugins can be found
[in the OVOS Docs](https://openvoiceos.github.io/community-docs/tts_plugins/).

### Example
This example shows the steps you would take to install and configure the
[Mozilla Remote TTS Plugin](https://github.com/NeonGeckoCom/neon-tts-plugin-mozilla_remote).

```shell
# Install the plugin package
pip install neon-tts-plugin-mozilla-remote
# Configure Neon to use the new plugin
nano ~/.config/neon/neon.yaml
```

Add or update the user configuration to use the new plugin and configure the plugin.

```yaml
tts:
  # Set the module to the Plugin's entrypoint.
  # This is usually in the plugin's `README.md` file, or it can be found in `setup.py`
  module: mozilla_remote
  mozilla_remote:
    api_url: https://mtts.2022.us/api/tts
```

Restart core services after making this change to reload with these changes.