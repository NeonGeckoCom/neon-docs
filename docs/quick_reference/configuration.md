# Configuration

Neon Core configuration follows the same general structure as
[Mycroft](https://mycroft-ai.gitbook.io/docs/using-mycroft-ai/customizations/mycroft-conf)
and [OVOS](https://openvoiceos.github.io/community-docs/060-config/),
with the main difference being that Neon uses [YAML](https://yaml.org/) formatting.

## Core Configuration Files

The full documentation for configuration files is available in the
[OVOS Documentation](https://openvoiceos.github.io/community-docs/config/), but
this document describes the default configuration for NeonOS.

### Default Configuration

A default configuration comes included with the [neon-core](https://github.com/NeonGeckoCom/NeonCore/blob/master/neon_core/configuration/neon.yaml)
package. This configuration file contains all possible configuration parameters
with default values. The configuration is updated with the `neon-core` package
and is not intended to be modified by anyone other than neon-core contributors.

### System Configuration

A system configuration is included in [Neon OS](https://neongeckocom.github.io/neon-docs/neon_os/)
images at `/etc/neon/neon.yaml` to override default values and add more
configuration that is specific to those installations. For example,
[PHAL Plugins](https://openvoiceos.github.io/community-docs/110-ht_phal/) that are
included with NeonOS have their configuration set in `/etc/neon/neon.yaml`. This
configuration is intended to me modified by image builders and not end-users.

### User Configuration

A user configuration file exists in `~/.config/neon/neon.yaml` and values here
generally override any other configuration files. This file is where an end-user
would set configuration, for example to enable debug logging or to prevent certain
skills from loading.

## User Profile Configuration

Neon Core adds support for multiple users which is not currently implemented in
Mycroft or OVOS. Part of this support includes separating user configuration from
core configuration, since there might be multiple users connected to one core.

### Core Configuration Overrides

The core configuration values for location, units, and language are still specified
and are treated as default values. If a user location is not set, then
core configuration values are used. Some configuration references are also not tied
to a particular user, i.e. the Home Screen display; these references will also use
core configuration.

Any time a skill knows what user made a request, that user's specific configuration
will take priority over core configuration. For voice inputs, the local user configuration
saved at `~/.config/neon/ngi_user_info.yml` will be used.

## Common Configuration Tasks

There are a few common configuration changes a user might want to make to their
Neon installation.

### Change STT/TTS engines

To override default STT/TTS engines, a user can add or change the following configuration
in `~/.config/neon/neon.yaml`:

```yaml
stt:
  module: <stt plugin entrypoint name>
  fallback_module: <fallback stt plugin entrypoint name>
tts:
  module: <tts plugin entrypoint name>
  fallback_module: <fallback tts plugin entrypoint name>
```

> More information about plugins can be found [in the OVOS docs](https://openvoiceos.github.io/community-docs/300-glossary/#opm).

### Blacklist a skill

If you want to disable a skill without uninstalling it, you can use configuration
to prevent that skill from loading. This may be useful for troubleshooting intent
conflicts or for making sure a skill that is known to cause conflicts never loads.

```yaml
skills:
  blacklisted_skills:
    - <blacklisted skill id>
    - <other blacklisted skill id>
```

> The `skill_id` here is usually something like `skill_name.author`. See also [definitions](../overview/definitions.md#skill-id)

> More information about skills can be found [in the OVOS docs](https://openvoiceos.github.io/community-docs/080-ht_skills/).

### Change the Wakeword

Information on changing the wakeword (ex: Hey Neon, Hey Mycroft, etc.) can be 
found [in the OVOS docs](https://openvoiceos.github.io/community-docs//104-ht_ww/#ovos-listener-wakewords-hotwords).
