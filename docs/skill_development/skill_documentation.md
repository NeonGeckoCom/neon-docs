# Skill Documentation
Some basic documentation about a skill helps user discovery and is necessary for
a skill to be included in some lists/stores.

## skill.json
The [`skill.json`](https://neongeckocom.github.io/neon-docs/developers/skill_json/) 
file contains parsed information about a skill, such as examples,
a description, known incompatible skills, and dependent skills. The link above
contains details about the `skill.json` spec, but this document will discuss the
existing tools and automations to generate that `json` file for a skill.

### Automating `skill.json` Updates
The following automation can be added to a skill repository as 
`.github/workflows/update_skill_json.yml` to enable automated updates to `skill.json`
when committing a change to GitHub:

```yaml
name: Update skill.json
on:
  push:

jobs:
  update_skill_json:
    uses: neongeckocom/.github/.github/workflows/skill_update_json_spec.yml@master
```

The above automation assumes `README.md`, `LICENSE.md`, and either 
`requirements.txt` or `manifest.yml` are present in the skill repository.

## README.md
All skills should contain a `README.md` file which is shown by default when viewing
the skill repository on GitHub. Using [skill-alerts](https://github.com/NeonGeckoCom/skill-alerts/blob/1.4.0/README.md?plain=1)
as an example, this document will go through some of the expected `README.md` sections.

The first line of the file specifies a skill icon (in this case, a file in the
git repository `logo.svg`) and a title for the skill (`Alerts`)
```markdown
# <img src='./logo.svg' card_color="#FF8600" width="50" style="vertical-align:bottom" style="vertical-align:bottom">Alerts
```
---
The `Summary` section contains a brief description of the skill.
```markdown
## Summary  
  
A skill to schedule alarms, timers, and reminders
```
---
The `Description` contains a much longer description of the skill, including some
information about how the skill works.
```markdown
## Description  
  
The skill provides functionality to create alarms, timers and reminders, remove them by name, time, or type, and ask for
what is active. You may also silence all alerts and ask for a summary of what was missed if you were away, your device
was off, or you had quiet hours enabled.

Alarms and reminders may be set to recur daily or weekly. An active alert may be snoozed for a specified amount of time
while it is active. Any alerts that are not acknowledged will be added to a list of missed alerts that may be read and
cleared when requested.
```
---
The `Examples` section contains some example utterances and in this case, some
descriptions about what the examples do. Each example is formatted as a sentence
that is ready to be shown to the user as-is; the `"` are optional. Note that the
text that is not part of an unordered list (no leading `-` or `*`) is excluded 
from the parsed list of examples.
```markdown
## Examples  
  
If you are skipping wake words, say `Neon` followed by any of the following, otherwise say your `Wake Word`:

- "Set an alarm for 8 AM."
- "When is my next alarm?"
- "Cancel my 8 AM alarm."

- "Set a 5 minute timer."
- "How much time is left?"

- "Remind me to go home at 6."
- "Remind me to take out the trash every thursday at 7 PM."
- "What are my reminders?"

- "Cancel all alarms."
- "Cancel all timers."
- "Cancel all reminders."

- "Go to sleep."
- "Start quiet hours."

If there is an active alert (expired and currently speaking or playing), you may snooze or dismiss it:

- "Stop."

- "Snooze."
- "Snooze for 1 minute."
  
If you had quiet hours enabled, your device was off, or you were away and missed an alert, you may ask for a summary:

- "Wake up."
- "What did I miss?"
- "Did I miss anything?"
```
---
`Incompatible Skills`, like `Examples` contains a list of skills that conflict with
the `skill-alerts.neongeckocom` skill. This example uses skill ID's with links to
the skill URLs, but only the URLs are required
```markdown
## Incompatible Skills
This skill has known intent collisions with the following skills:
- [skill-reminder.mycroftAI](https://github.com/mycroftai/skill-reminder)
- [skill-alarm.mycroftAI](https://github.com/mycroftai/skill-alarm)
- [mycroft-timer.mycroftAI](https://github.com/mycroftai/mycroft-timer)
```
---
`Credits` contains some users/organizations who contributed to the skill. This
section is optional, but it is best practice to specify user/org names with links
to their GitHub account. Names without account links are also supported
```markdown
## Credits
[NeonGeckoCom](https://github.com/NeonGeckoCom)
[NeonDaniel](https://github.com/NeonDaniel)
```
---
`Category` contains some categories this skill fits under, with the "primary" category
bolded (`**`Category`**`). Valid categories are:
- Daily
- Configuration
- Entertainment
- Information
- IoT
- Music & Audio
- Media
- Productivity
- Transport
```markdown
## Category
**Productivity**
Daily
```
---
`Tags` contain some tags that apply to the skill. There are no hard rules about
what may be in a tag, but `NeonGecko Original` is reserved for use with skills
owned by the `NeonGeckoCom` organization.

## LICENSE.md
Every project on GitHub should contain a `LICENSE.md` file with a known license.
This file will be parsed to try and determine the license applied to the skill,
otherwise it will be listed as 'unknown'

## requirements.txt
This is a standard Python requirements file with one package per line. Lines
with a leading `#` are ignored as comments.

## manifest.yml
This is a specification [from MycroftAI](https://github.com/MycroftAI/documentation/blob/master/docs/skill-development/skill-structure/dependencies/manifest-yml.md)
to specify requirements. A skill will generally contain either `requirements.txt`
OR `manifest.yml`.