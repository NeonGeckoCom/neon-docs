# Adding Support for Other Languages
Generally, skill development starts with support for one language (whatever the
skill author speaks). After the skill is behaving as expected, 
[skill tests](https://neongeckocom.github.io/neon-docs/skill_development/skill_testing/)
should be added for resources and intents in the initial supported language.

## Translating Resources
First, the skill resources in the `locale` directory will need to be translated.
A good way to start is to copy the directory for a fully supported language and
then go through each resource file and translate the contents. Directories within
`locale` should be named as a `BCP-47` language code, More information
about how these various files are used can be found in the 
[Adapt](https://neongeckocom.github.io/neon-docs/skill_development/adapt_intents/)
and [Padatious](https://neongeckocom.github.io/neon-docs/skill_development/padatious_intents/)
documentation.

## Updating Tests
To ensure translations are complete and functional, unit tests are used to check
resources and expected intent matches.

### Resource Tests
After resources are translated, the new language code should be added to `languages`
in the `test/test_resources.yaml` file for the skill. These tests will ensure that
all expected resources are translated for the skill and that future changes to
the skill maintain language support.

### Intent Tests
Once initial translation is completed, `test/test_intents.yaml` should be updated
to include tests for the added language. Copying tests from an existing language
and then translating them can be a helpful starting point; more detailed information
is available in the 
[skill testing documentation](https://neongeckocom.github.io/neon-docs/skill_development/skill_testing/).

## Updating Documentation
Documentation should be updated to include translated strings and include intent
examples for the newly supported language.

### README.md
`# TODO: Link to doc`

### skill.json
No manual changes are necessary to `skill.json` if using the Neon skill
automation. More information about the `skill.json` specification can be found
in the [skill.json docs](https://neongeckocom.github.io/neon-docs/developers/skill_json/).
> *Note*: The infrastructure for this is still under construction, but the 
> supporting files can be added so `skill.json` is updated in the future.