# Installing Skills
Most distributions of Neon Core will include some installed skills. 
[OVOS Skills Manager](https://openvoiceos.github.io/community-docs/osm/) (OSM) 
can be used to install new skills, including Mycroft and OVOS community skills.
The simple answer to "how do I install a skill?" is: `osm install <skill_url>`.

## Default Neon Skills
Many [Neon skills](https://github.com/NeonGeckoCom/neon_skills) are included by
default and all of them should be installable via `pip`. Many of the skills can
be installed [from PyPI](https://pypi.org/search/?q=neon-skill) (i.e. `pip install
neon-skill-translation`), and any containing `setup.py` can be installed from git
(i.e. `pip install git+https://github.com/NeonGeckoCom/skill-translation`).

## Configuring Extra Skills
The recommended method for adding non-default skills is to add them to your
configuration. This will ensure that skills are updated and that they are 
re-installed after system updates. Extra skills can be added to configuration:

```yaml
skills:
  default_skills:
    - git+https://github.com/NeonGeckoCom/skill-translation
    - neon-skill-translation~=1.0 
```
> Note that the skills here are [Python packages](https://packaging.python.org/en/latest/).
> This is the same kind of string you would pass to 
> [`pip install`](https://pip.pypa.io/en/stable/cli/pip_install/), including any (optional)
> version specs.

## Installing Skills from GitHub
> OSM installation is documented here, but not recommended. Skills should be 
> updated to use pip installation, as described above.

There are many community skills published to GitHub that may be searched for and
installed using [OSM](https://github.com/openvoiceos/ovos_skill_manager). Hints
are available from the CLI via `osm --help` or `osm install --help`. Beware that
not all skills are tested and skills installed from GitHub may not work, may be
malicious, or may no longer be maintained. The [skill lists below](#other-skill-indices)
have generally been evaluated to some degree, but still be careful about running
any code from the internet.

### Other Considerations
Skill installation from git has the possibility of breaking Python dependencies.
If you run into trouble after installing a skill, you may want to perform a
factory reset to get back to a set of working Python packages.

## Locating Installed Skills
Skills are generally installed to one of two locations, depending on whether they
were installed as Python packages or cloned from Git.

### Python Packages
For Python packaged skills, a skill is installed like any other Python package.
In most cases, you should have core modules and skills installed to a `venv`. On
Neon OS devices, this environment is located at `/home/neon/venv` and skills can
be found at `/home/neon/venv/lib/python3.7/site-packages/skill_*`.

### Git Cloned Skills
For skills installed from Git (usually using OSM), the default install location
is `~/.local/share/neon/skills`, but this may be configured in `neon.yaml` to
any location. More detailed configuration documentation is available 
[in the OVOS Docs](https://openvoiceos.github.io/community-docs/config/).

## Other Skill Indices
### OpenVoiceOS Organization
There are several skills in the [OpenVoiceOS organization](https://github.com/OpenVoiceOS?q=skill-ovos&type=all&language=&sort=)
that may be `pip` installed.

### Mycroft Marketplace
There are also skills in the [Mycroft Marketplace](https://github.com/MycroftAI/mycroft-skills)
(note that there are more open Pull Requests there too).

### Andlo's List
There is a list of Mycroft skills maintained by community member
[andlo on GitHub](https://github.com/andlo/mycroft-skills-list-gitbook/tree/master/skills).
