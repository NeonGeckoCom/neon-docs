# Neon Enclosure
The enclosure module implements the Platform and Hardware Abstraction Layer (PHAL)
from [OpenVoiceOS](https://github.com/OpenVoiceOS/ovos-PHAL). This service loads
PHAL plugins that provide different functionality to the core; plugins primarily 
differ from skills in that they do not have any intents and that they may only
be valid in certain core environments (i.e. only for particular hardware or
operating system environments).

## Admin Services
`neon_enclosure.admin` contains a service much like `neon_enclosure`, but plugins
it loads will have `root` privileges. This service is intended for handling any
OS-level interactions requiring escalation of privileges and is excluded from Docker support.
Because this service runs as root, it also requires configuration be initialized
prior to its initialization; user-level configurations will be placed in the `/root`
directory per XDG, so any configuration should be done at the system-level.

## Running in Docker
The included `Dockerfile` may be used to build a docker container for the neon_audio module. The below command may be used
to start the container.

```shell
docker run -d \
--network=host \
--name=neon_enclosure \
-v ~/.config/pulse/cookie:/home/neon/.config/pulse/cookie:ro \
-v ${XDG_RUNTIME_DIR}/pulse:${XDG_RUNTIME_DIR}/pulse:ro \
--device=/dev/snd:/dev/snd \
-e PULSE_SERVER=unix:${XDG_RUNTIME_DIR}/pulse/native \
-e PULSE_COOKIE=/home/neon/.config/pulse/cookie \
neon_enclosure
```

>*Note:* The above example assumes Docker data is stored in the standard user locations `~/.local/share` and `~/.config`.
> You may want to change these values to some other path to separate container and host system data.
