# Neon OS Developer Documentation
[Neon OS](https://neongeckocom.github.io/neon-docs/neon_os/) is a customized
Debian distribution used to deploy Neon to edge devices. Developers may wish to
change some default behaviors of Neon OS to aid in debugging or development.

## Enable debug logging
By default, `debug` level logs are excluded from Neon module log files; to enable
debug logging, add the below line to `~/.config/neon/neon.yaml`:
```yaml
log_level: DEBUG
```
More information about configuration can be found [here](https://neongeckocom.github.io/neon-docs/quick_reference/configuration/).

## Disable `log2ram`
By default [`log2ram`](https://github.com/azlux/log2ram) is used to minimize writing to persistent storage.
Behavior can be configured in `/etc/log2ram.conf` or the service may be disabled entirely with
`sudo systemctl stop log2ram`.
