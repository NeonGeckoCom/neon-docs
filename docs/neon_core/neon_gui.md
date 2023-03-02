# Neon GUI
GUI Module for Neon Core. This module extracts the GUI components from the 
[Mycroft Mark 2 Skill](https://github.com/MycroftAI/skill-mark-2) and the `enclosure` module
from [mycroft-core](https://github.com/MycroftAI/mycroft-core/tree/dev/mycroft/enclosure). 

## Neon Enhancements
Isolating the GUI to a standalone module allows for standardized implementation of common 
components.

## Compatibility
This package can be treated as a drop-in replacement for the GUI components in `skill-mark-2`.
`RestingScreen`, and gui bus events are handled here.

## Running in Docker
The included `Dockerfile` may be used to build a docker container for the neon_messagebus module. The below command may be used
to start the container.

```shell
docker run -d \
--name=neon_gui \
--network=host \
neon_gui
```
