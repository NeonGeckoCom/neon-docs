# Logging
There are a couple of ways that Neon components generate logs. For core modules,
logs are generally available in `~/.local/state/neon/`. Most installations will
have the following logs: `admin.log`, `audio.log`, `bus.log`, `enclosure.log`,
`gui.log`, `skills.log`, `voice.log`.

## Neon OS
For Neon OS installations, core service logs are written to the default 
`/home/neon/.local/state/neon` paths. Logs are also written to the system 
journal since the services are started via systemD. You can see service logs via
`journalctl -u <service_name>`. You could copy logs off of your Mark2 from a 
computer on the same network with:
```
scp neon@<device_ip>:/home/neon/.local/state/neon/logs ./
```

## Docker
For running docker containers, you can follow logs via `docker logs -f <container_name>`.
If you bound a path to `/xdg` in the container, persistent log files are also written
to `/xdg/state/neon/`.