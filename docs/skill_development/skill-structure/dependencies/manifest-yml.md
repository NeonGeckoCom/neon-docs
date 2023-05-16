---
description: >-
  A Skill's `manifest.yml` file is the default method for defining the
  dependencies of a Neon Skill.
---

# Manifest.yml

**NOTE:** Manifest.yml is not supported fully by pip-installed skills. [It is recommended to use the `skill.json` file instead](../../development-setup/skill_json.md).

In this file we can include Python packages, Linux applications or other Neon skills that are required for our own Skill to function properly.

[YAML](https://en.wikipedia.org/wiki/YAML) is a language commonly used for configuration files. It uses indentation rather than brackets or parentheses to define the structure or hierarchy of its contents.

## File contents

We start the `manifest.yml` by defining a top-level key of `dependencies` and the type of dependency we want to include.

```yaml
dependencies:
  python:
```

### Python Packages

Here we can see a simple example that defines the `requests` and `gensim` Python packages as required dependencies.

```yaml
dependencies:
  python:
    - requests
    - gensim
```

When a Skill with this `manifest.yml` file is being installed, Neon would check for, and if required install, both packages from [PyPI](https://pypi.org/) using the PIP installer.

There is no limit to the number of packages you can install, however these are reviewed during the [Skills Acceptance Process](../../marketplace-submission/skills-acceptance-process/) to ensure they are appropriate for the Skill being installed.

### Linux System Packages

Linux packages are defined under the `system` key. As Neon can be installed on many different Linux distributions, support is provided for a range of package managers.

For packages that have consistent naming across package managers, we can use `all`.

```yaml
dependencies:
  system:
    all: pianobar piano-dev
```

If the package has a different name on specific platforms, we can define that using the name of the package manager as a key. In the following example, we want to use the `libpiano-dev` package when using the `APT` package manager on Debian, Ubuntu and other related distributions.

```yaml
dependencies:
  system:
    all: pianobar piano-dev
    apt-get: pianobar libpiano-dev
```

Finally we can check that certain executables are available for the install to succeed. This is done by checking the [PATH environment variable](http://www.linfo.org/path_env_var.html).

```yaml
dependencies:
  system:
    all: pianobar piano-dev
    apt-get: pianobar libpiano-dev

  exes:
    - pianobar
```

Here we have installed a number of `pianobar` packages, and then verify that the `pianobar` executable is available for our Skill.

### Other Neon Skills

A Skill may even require that other Neon Skills are installed rather than duplicate functionality. Here we can see that the Cocktails Skill and the Mozilla Webthings Gateway are listed as dependencies.

```yaml
dependencies:
  skill:
    - cocktails
    - webthings-gateway
```

Anything listed in this section will be passed to Neon for installation.

## Example files

A complete `manifest.yml` example can be found in the [official Mycroft Template Skill on Github](https://github.com/MycroftAI/mycroft-skills/blob/19.08/00__skill_template/manifest.yml).

A simple example from a real Skill can be found in the [Desktop Launcher Skill](https://github.com/MycroftAI/skill-desktop-launcher/blob/19.08/manifest.yml).
