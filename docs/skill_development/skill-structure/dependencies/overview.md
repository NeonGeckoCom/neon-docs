---
description: >-
  Neon Skills are powerful because we can make use of external packages and
  applications, or add voice interfaces to existing tools.
---

# Dependencies

There are three main categories of dependencies:

- Python packages sourced from [PyPI](https://pypi.org/).
- Linux system packages sourced from the repositories available on the Neon device.
- Skills sourced from the [Mycroft Skills Marketplace](https://market.mycroft.ai/) or other [Marketplaces](https://openvoiceos.github.io/community-docs/install_skills/#supported-stores).

Some of these may already be installed on a User's device, however some may not. To make sure a system has everything that your Skill needs, we can define the dependencies or requirements of the Skill. During installation Neon will then check that they are installed, and if not attempt to do so.

For more information on Python package management and Python virtual environments, see our [general Python Resources.](../../introduction/python-resources.md)

There are three files that we can use to define these dependencies.

## Recommended

[`manifest.yml` is the default method](manifest-yml.md). This can include all three types of dependencies including variations for different operating systems if required.

## Alternatives

`requirements.txt` can be used only for Python packages.

`requirements.sh` is used to run a custom script during installation.

[More details on each are available in this documentation](requirements-files.md).

Which ever file you choose to use, it must be located in the root directory of your Skill.

There is no limit to the number of packages you can install, however these are reviewed for all official Neon skills to ensure they are appropriate for the Skill being installed and do not pose a security concern for Users. _For this reason and many others, install non-official skills at your own risk, and be sure to review the code for questionable sources if you can._

## Manual installation

The files outlined above ensure that dependencies are available on devices when a Skill is being installed by Neon. If you are developing the Skill on your own machine, you may need to install these dependencies manually.

System packages can be installed using your standard package manager, for example:

```bash
apt install system-package-name
```

Neon Skills should ideally be installed using pip:

```bash
pip install required-skill-name
pip install git+https://github.com/neongeckocom/required-skill-name
```

Neon Skills can be installed using the OVOS Skills Manager (may behave unexpectedly):

```bash
osm install required-skill-name
```

Python packages must be installed in the Neon virtual environment. When logging into any official Neon image, this environment shell will automatically be loaded, so you can just use the `pip` command.
