# Skill Testing
Testing is an important aspect to skill development to ensure that a skill acts 
as expected. Neon provides several automations to make basic testing trivial to
implement and more advanced testing easier.

## Automating Tests
To enable automated tests with GitHub actions, create 
`.github/workflows/skill_tests.yml` in your skill repository with the below snippet:

```yaml
# This workflow will run unit tests

name: Test Skill
on:
  pull_request:
  workflow_dispatch:

jobs:
  # TODO: Add any jobs here
```

The above automation means that build tests will be run any time a pull_request 
is created/updated, or `Test Skill` is manually run from GitHub.

## Build Tests
For skills that include a `setup.py` file for installation, enabling build tests
can help catch typos or errors. To enable this automation, add the below snippet
to the GitHub automation [specified above](#automating-tests)

```yaml
  py_build_tests:
    uses: neongeckocom/.github/.github/workflows/python_build_tests.yml@master
```

## Install Tests
For any skill (even those without `setup.py`), installation tests can be used to
make sure the skill can be installed via [`osm`](https://github.com/OpenVoiceOS/ovos_skill_manager).
To enable this automation, add the below snippet to the GitHub automation 
[specified above](#automating-tests):

```yaml
  skill_install_tests:
    uses: neongeckocom/.github/.github/workflows/skill_test_installation.yml@master
```

## Unit Tests
Unit tests are generally a good way to make sure components are working as expected
while building a skill. It is generally recommended to write unit tests while writing
a skill.

To enable this automation, specify `test/test_skill.py` and add the below 
snippet to the GitHub automation [specified above](#automating-tests):

```yaml
  skill_unit_tests:
    uses: neongeckocom/.github/.github/workflows/skill_tests.yml@master
```

### Example
Using [skill-about](https://github.com/NeonGeckoCom/skill-about/blob/0.2.1/test/test_skill.py) as an example,
unit tests are fairly simple to implement.

The snippet below shows all of the common test code that can simply be copy/pasted
to a new skill test:

```python
import json
import os
import shutil
import unittest
import pytest

from os import mkdir
from os.path import dirname, join, exists
from mock import Mock
from mycroft_bus_client import Message
from ovos_utils.messagebus import FakeBus


class TestSkill(unittest.TestCase):

    @classmethod
    def setUpClass(cls) -> None:
        from mycroft.skills.skill_loader import SkillLoader

        bus = FakeBus()
        bus.run_in_thread()
        skill_loader = SkillLoader(bus, dirname(dirname(__file__)))
        skill_loader.load()
        cls.skill = skill_loader.instance
        cls.test_fs = join(dirname(__file__), "skill_fs")
        if not exists(cls.test_fs):
            mkdir(cls.test_fs)
        cls.skill.settings_write_path = cls.test_fs
        cls.skill.file_system.path = cls.test_fs

        # Override speak and speak_dialog to test passed arguments
        cls.skill.speak = Mock()
        cls.skill.speak_dialog = Mock()

    @classmethod
    def tearDownClass(cls) -> None:
        shutil.rmtree(cls.test_fs)

    def tearDown(self) -> None:
        self.skill.speak.reset_mock()
        self.skill.speak_dialog.reset_mock()

    def test_00_skill_init(self):
        # Test any parameters expected to be set in init or initialize methods
        pass
```

The setup methods here create an instance of the skill class and mock the settings
and file system parameters to prevent interfering with any installations on the
system running tests. This also mocks `speak` and `speak_dialog` so test methods
can check what is passed to those methods.

Continuing with that example, a test for an intent handler would look like:
```python
def test_read_license(self):
    valid_message = Message("test_message",
                            {"tell": "tell", "license": "license"},
                            {"neon_should_respond": True})
    valid_message_long = Message("test_message",
                                 {"tell": "tell", "license": "license",
                                  "long": "long"},
                                 {"neon_should_respond": True})

    self.skill.read_license(valid_message)
    self.skill.speak_dialog.assert_called_with("license_short")

    self.skill.read_license(valid_message_long)
    self.skill.speak_dialog.assert_called_with("license_long")
```

The `message` passed to the intent handler is mocked here with some expected
`data` and `context`; note that the `msg_type` is unimportant in this case. The
skill handler method `read_license` is called directly with the test `message`
and calls to `speak_dialog` are tested to make sure the correct dialog was
selected.

The [unittest documentation](https://docs.python.org/3/library/unittest.html) is
another helpful resource for testing other methods and there are various guides
for writing generic unit tests, such as [this one from RealPython](https://realpython.com/python-testing/).

## Resource Tests
Resource tests ensure that any expected resource files are present and not empty.
This is particularly useful for making sure supported languages are maintained
and changes to the skill don't accidentally break language support.

To enable this automation, specify `test/test_resources.yaml` and add the below 
snippet to the GitHub automation [specified above](#automating-tests):

```yaml
  skill_resource_tests:
    uses: neongeckocom/.github/.github/workflows/skill_test_resources.yml@master
```

### Example
Using [skill-about](https://github.com/NeonGeckoCom/skill-about/blob/0.2.1/test/test_resources.yaml) 
as an example, resource tests look like:

```yaml
# Specify resources to test here.

# Specify languages to be tested
languages:
  - "en-us"
  - "uk-ua"

# vocab is lowercase .voc file basenames
vocab:
  - license
  - long
  - skills
  - tell

# dialog is .dialog file basenames (case-sensitive)
dialog:
  - license_long
  - license_short
  - skills_list
# regex entities, not necessarily filenames
regex: []
intents:
  # Padatious intents are the `.intent` file names
  padatious: []
  # Adapt intents are the name passed to the constructor
  adapt:
    - LicenseIntent
    - ListSkillsIntent
```

Comments in the above file describe how to fill out each section; this file generally
shouldn't change unless a new intent is added to a skill. Also note that both missing
AND extraneous files will result in a test failure.

## Intent Tests
Intent tests make sure that a skill matches user utterances to intents as expected.
Intent tests are very useful in making sure a skill handler handles all the user
utterance it should and not the ones it shouldn't.

It is also helpful to take reported intent failures or invalid matches and add
them to the tests here and then adjust intent resources to achieve the desired
behavior.

To enable this automation, specify `test/test_intents.yaml` and add the below 
snippet to the GitHub automation [specified above](#automating-tests):

```yaml
  skill_resource_tests:
    uses: neongeckocom/.github/.github/workflows/skill_test_intents.yml@master
```

### Example
Using [skill-about](https://github.com/NeonGeckoCom/skill-about/blob/0.2.1/test/test_intents.yaml) 
as an example, resource tests look like:

```yaml
# Specify intents to test here. Valid test cases are as follows:

# Basic intent match tests only:
#lang:
#  intent_name:
#    - example utterance
#    - other example utterance

# Intent tests with expected vocab/entity matches:
#lang:
#  intent_name:
#    - example_utterance:
#        - expected vocab name
#        - other expected vocab name

# Intent tests with specific vocab/entity extraction tests:
#lang:
#  intent_name:
#    - example_utterance:
#        - expected_vocab_key: expected_vocab_value
#        - expected_entity_key: expected_entity_value


en-us:
  LicenseIntent:
  - what is your license
  - what is my license
  - tell me the license
  - tell me the full license:
      - long: full
  - tell me my complete license:
      - long: complete
  ListSkillsIntent:
    - tell me my skills
    - tell me installed skills
    - what skills are installed
    - what can you do

unmatched intents:
  en-us:
    - what is your name
    - what is my name
    - tell me what a skill is
#    - tell me what skills are
    - what can you tell me about life
    - tell me what a license is
```

The comments in the above file note how to specify valid intent matches, including
optional checks for extracted intent data (vocab/entity/regex).

The `unmatched intents` specifies utterances that should match no intent in this
skill and are useful for making sure a skill will work well with other skills.
