# Neon OS Developer Documentation
[Neon OS](./index.md) is a customized
Debian distribution used to deploy Neon to edge devices. Developers may wish to
change some default behaviors of Neon OS to aid in debugging or development.

## Enable debug logging
By default, `debug` level logs are excluded from Neon module log files; to enable
debug logging, add the below line to `~/.config/neon/neon.yaml`:
```yaml
log_level: DEBUG
```
More information about configuration can be found [here](../quick_reference/configuration.md).

## Disable `log2ram`
By default [`log2ram`](https://github.com/azlux/log2ram) is used to minimize writing to persistent storage.
Behavior can be configured in `/etc/log2ram.conf` or the service may be disabled entirely with
`sudo systemctl stop log2ram`.

## Getting Build Information
For most situations, the information provided under `Settings` -> `About` is
enough to determine what version of Neon Core is running and what version of
Neon OS it came with. More complete details of what is included in a particular
`.squashfs` release can be found at `/opt/neon/build_info.json`. An annotated 
example file is included below:

```json
{
  // neon_core package information (last commit, time, and version.py contents)
  "core": {
    "sha": "c061a7c51a1c835cded5e57de54c113364520b25",
    "time": 1692929705.0,
    "version": "23.8.25a12"
  },
  // neon_debos package information (last commit, time, and version.py contents)
  "image": {
    "sha": "b2791e63d31bb6205f1db6c7bae9c4109f1ddd56",
    "time": 1692927223.0,
    "version": "23.8.25a13"
  },
  // SHIPPED initramfs information (path and MD5 hash). This does not account for updated hashes
  "initramfs": {
    "path": "/boot/firmware/initramfs",
    "md5": "9ed30557e09de796e4788b1be3f98f4e"
  },
  // Name of debos recipe, architecture, and time of build
  "base_os": {
    "name": "debian-neon-image-rpi4",
    "time": 1692940440.0,
    "arch": "arm64"
  }
}
```
