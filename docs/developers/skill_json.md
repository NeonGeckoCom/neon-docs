# `skill.json` spec
This document specifies valid `skill.json` metadata.

### `title`
String skill title to be used in store listings and other user-facing places. This
is not guaranteed to be unique, so multiple skills may have the same `title`.

### `url`
String URL that a skill is hosted at (usually on github.com)

### `short_description`
String short description or by-line for a skill. This may be used in store listings
or other user-facing places.

### `description`
String description for a skill. This may be used in a 'details' page or similar.

### `examples`
List of string utterances that are handled by this skill. These should be formatted
as sentences, ready to be displayed as-is and should not include any wake words
or other extraneous information.

### `desktopFile` (optional)
Relative path to a `.desktop` file that can be used to launch a persistent skill
GUI.

### `systemDeps`
Boolean indicating if a skill includes system package dependencies.

### `requirements`
Dictionary with keys `python`, `system`, and `skill`, each with values indicating
requirements.

#### python
List of valid package specs, as would be found in `requirements.txt`.

#### system
Dictionary of package manager (i.e. `apt`) to a list of required packages.

#### skill
List of required skills, identified by skill_id.

### `incompatible_skills`
List of skills that can not be loaded at the same time as this one.

### `platforms`
List of string platforms this skill is compatible with.

Valid options: `['i386', 'x86_64', 'ia64', 'arm64', 'arm']`

### `license`
String licence identifier this skill is provided under.

### `icon`
String URI for an icon to use in user-facing UIs.

### `category`
Primary category this skill belongs to.

Valid options: `["Daily", "Configuration", "Entertainment", "Information", "IoT",
"Music & Audio", "Media", "Productivity", "Transport"]`

### `categories`
List of categories this skill belongs to.

Valid options: `["Daily", "Configuration", "Entertainment", "Information", "IoT",
"Music & Audio", "Media", "Productivity", "Transport"]`

### `tags`
List of string tags to apply to this skill. Tags can be any string and help with
searchability; some skill listings might require a skill contain a particular tag.

### `skillname`
String name of the skill, usually the Git repository name.

### `authorname`
String name of the skill's author, usually the Git user/organization name.

### `foldername`
Optional string base directory to install the skill to; default `null`.

### `lang`
String BCP-47 language code (i.e. `en-us`) the skill.json and README.md files are written in.

### `lang_support`
Dict of other supported languages (BCP-47 language codes) to dict with keys:
`skillname`, `description`, and `examples`. Missing fields will default to top-level fields.

### `credits` (optional)
List of string credits for a skill, usually GitHub usernames. 

### `branch` (optional)
Git branch this `skill.json` spec is associated with.

### `warning` (optional)
Optional string warning to display when installing a skill.

### `summary` (optional)
String short description or by-line for a skill. This may be used in store listings
or other user-facing places.