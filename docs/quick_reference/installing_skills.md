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

## Installing Skills from GitHub
There are many community skills published to GitHub that may be searched for and
installed using [OSM](https://openvoiceos.github.io/community-docs/osm/). Hints
are available from the CLI via `osm --help` or `osm install --help`. Beware that
not all skills are tested and skills installed from GitHub may not work, may be
malicious, or may no longer be maintained. The [skill lists below](#other-skill-indices)
have generally been evaluated to some degree, but still be careful about running
any code from the internet.

### Other Considerations
Skill installation from git has the possibility of breaking Python dependencies.
If you run into trouble after installing a skill, you may want to perform a
factory reset to get back to a set of working Python packages.

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
