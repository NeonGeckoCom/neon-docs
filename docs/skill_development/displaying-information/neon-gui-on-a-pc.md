---
description: >-
  During development, it can be useful to emulate the Mark II display on your
  primary machine.
---

# Neon-GUI on a PC

## Installation

**NOTE:** `neon-gui` will soon be deprecated; use [`ovos-shell`](https://github.com/OpenVoiceOS/ovos-shell) instead where possible.

The [neon-gui](https://github.com/NeonGeckoCom/neon-gui) package requires some newer packages, so a newer distribution such as KDE Neon or K/Ubuntu 20.04 is recommended.

On supported systems, the installation is equivalent to a development installation of mycroft-core:

```bash
git clone https://github.com/NeonGeckoCom/neon-gui
cd neon-gui
./dev_setup.sh
```

For more detail see the [Getting Started](https://github.com/NeonGeckoCom/neon-gui#getting-started) section of the neon-gui repository.

## Running the application

Running the Neon GUI is as simple as launching:

```bash
mycroft-gui-app
```

If neon-core is not already running, all Neon services will be started.

By default the GUI window includes a text entry box to send utterances to Neon.

![Neon-GUI application](../../.gitbook/assets/mycroft-gui-screenshot.jpg)

In the bottom-left corner of the window is a hamburger-style menu. Here you can find hints for using Neon, settings for the Neon-gui application, and licensing information.

### Emulating a Mark II

In order to emulate the display of a Mycroft Mark II, or any other device, you can set the dimensions of the screen and hide the text input bar when launching the application.

```bash
mycroft-gui-app --width=800 --height=480 --hideTextInput
```
