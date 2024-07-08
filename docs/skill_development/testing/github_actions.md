# GitHub Actions automation
[This automation](https://github.com/NeonGeckoCom/skill-about/blob/533ab60f7b635918b951ce3b1f17c8e7fee25a5f/.github/workflows/skill_tests.yml)
can be included with a skill to enable automatic tests any time a PR is opened
with changes. This is the best way to ensure changes do not break any skill
behavior.

## Unit Testing with GitHub Actions
The simplest method for implementing unit tests is to extend the `SkillTestCase`
from Minerva, like the [About Skill does](https://github.com/NeonGeckoCom/skill-about/blob/533ab60f7b635918b951ce3b1f17c8e7fee25a5f/test/test_skill.py#L26).
The base class implements the skill loading logic and some basic mocking to simplify
unit tests.

## Intent Tests
Intent tests make sure a user input matches (or doesn't match) an expected skill
intent. The [Intent Test specification](https://github.com/NeonGeckoCom/skill-about/blob/533ab60f7b635918b951ce3b1f17c8e7fee25a5f/.github/workflows/skill_tests.yml)
describes how to write intent tests.

### GitHub Action configuration
The GitHub Action for running intent tests accepts the following inputs:
- `intent_file`: Path to yaml test spec, defaults to `test/test_intents.yaml`
- `skill_entrypoint`: Test skill entrypoint (file path or skill entrypoint), 
  defaults to skill repository root `action/skill`.
- `timeout`: Maximum time in minutes the test may run
- `test_padatious`: Test using the Padatious intent engine
- `test_padacioso`: Test using the Padacioso intent engine
- `neon_versions`: Python versions to test with Neon Core
- `ovos_versions`: Python versions to tests with OVOS Core

## Resource Tests
Resource tests make sure skill resources exist and are properly translated for
all supported languages. The [Resource Test specification](https://github.com/neongeckocom/neon-minerva?tab=readme-ov-file#resource-tests)
describes how to write resource tests.

### GitHub Action configuration
The GitHub Action for running resource tests accepts the following inputs:
- `resource_file`: Path to yaml test spec, defaults to `test/test_resources.yaml`
- `skill_entrypoint`: Test skill entrypoint (file path or skill entrypoint), 
  defaults to skill repository root `action/skill`.