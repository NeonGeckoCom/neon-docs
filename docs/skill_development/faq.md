# FAQ

## How do I disable a Skill?

During Skill development you may have reason to disable one or more Skills. Rather than constantly install or uninstall them via voice, or by adding and removing them from the skills folder, you can disable them in the `~/.config/neon/neon.yaml` file.

First, identify the name of the Skill. The name of the Skill is the `path` attribute in the [`.gitmodules`](https://github.com/MycroftAI/mycroft-skills/blob/master/.gitmodules) file.

To disable one or more Skills on a Mycroft Device, edit the `neon.yaml` file using an editor like `nano` or `vi`.

Search for the string `blacklisted` in the file. Then, edit the line below to include the Skill you wish to disable, and save the file. You will then need to reboot, or restart the `neon*` services on the Device.

```yaml
skills:
  blacklisted_skills:
    - skill-media
    - send_sms
    - skill-wolfram-alpha
    - YOUR_SKILL
```

## How do I find more information on Neon functions?

You can find documentation on Neon functions and helper methods at the [Mycroft Core API documentation](https://mycroft-core.readthedocs.io/en/master/)

## Need more help?

If something isn't working as expected, please join us in the [Neon Chat](https://matrix.to/#/#NeonMycroft:matrix.org).

It's also really helpful for us if you add an issue to our [documentation repo](https://github.com/NeonGeckoCom/neon-docs/issues). This means we can make sure it gets covered for all developers in the future.
