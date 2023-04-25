# Test Runner

Voight Kampff is designed to test not only your Skill in isolation, but also ensuring Skills operate as expected when co-existing with different sets of Skills from the Marketplace.

Parameters for this testing can be set using commandline flags or through a YAML configuration file.

## Running the tests

### With helper commands

If the Mycroft helper commands \(located in `mycroft-core/bin/`\) have been included on your `$PATH` you can initiate the test runner using either:

```bash
mycroft-start vktest -t skill-to-test
```

or

```bash
mycroft-skill-testrunner vktest -t skill-to-test
```

So to test the Mycroft Weather Skill we can run:

```bash
mycroft-skill-testrunner vktest -t mycroft-weather
```

### Without helper commands

If the helper commands are unavailable you can run this directly from your mycroft-core installation:

```bash
cd ~/mycroft-core
./start-mycroft.sh vktest -t skill-to-test
```

### Commandline options

Commandline flags can be used to set or override configuration and runtime options.

```text
  -p PLATFORM, --platform PLATFORM            
        Set the PLATFORM or device type, must be one of:[default, kde,
        mycroft_mark_1, mycroft_mark_2, mycroft_mark_2pi, picroft, respeaker].
  -t SKILL,SKILL, --test-skills SKILL,SKILL
        Test the provided comma-separated list of SKILLS.
  -e PATTERN, --exclude PATTERN
        Don't run feature files matching regular expression PATTERN.
  -i PATTERN, --include PATTERN
        Only run feature files matching regular expression PATTERN.
  -s SKILL,SKILL, --extra-skills SKILL,SKILL
        Ensure the provided comma-separated list of SKILLS are installed.
  -r INT, --random-skills INT
        Install INT random Skills before the test is run.
        WARNING: These Skills will be installed and not automatically removed.
  -d DIR, --skills-dir
        Set the directory for Skills on this system.
  -c FILE, --config FILE
        Use the provided YAML configuration file for the test parameters.
  -o FILE, --outfile FILE
        Write to specified file instead of stdout.
  -f FORMAT, --format FORMAT
        Specify a formatter. If none is specified the default formatter is used.
        Pass "--format help" to get a list of available formatters.
  --stop
        Stop running tests at the first failure.
  --tags TAG_EXPRESSION
        Only execute features or scenarios with tags matching TAG_EXPRESSION.
        Pass "--tags-help" for more information.
  -w, --wip
        Only run scenarios tagged with "wip". Additionally: use the "plain"
        formatter, do not capture stdout or logging output and stop at the
        first failure.
  -h, --help
        Display this help message
```

## What Skill name to use?

{% hint style="warning" %}
There are currently a few ways that Mycroft refers to Skills. This is currently under review so that we can have a single universal Skill naming/reference convention, but in the meantime...
{% endhint %}

MSM does fuzzy matching on Skill names to determine which Skill you intended. So the majority of the time, writing something that reflects the name of your Skill will work. Using the Hello World Skill as an example any of the following should work:

* hello-world
* skill-hello-world
* mycroft-hello-world
* hey-world

The last one might be a surprise, but remember we are fuzzy matching. 

Super simple right? Well not quite. You might run into cases where your Skill does not match, or where two very closely named Skills cause issues. In that case we need to get more specific with our naming.

### Skills in the Marketplace

If the Skill you are testing has a version in the Marketplace, then [MSM's](../mycroft-skills-manager/) primary way of referencing it is the submodule path. You can find the submodule path for any Skill listed in the [`.gitmodules`](https://github.com/MycroftAI/mycroft-skills/blob/21.02/.gitmodules) file in the mycroft-skills repository. In the case of the Hello World Skill, we would find the following submodule definition:

```text
[submodule "mycroft-hello-world"]
	path = skill-hello-world
	url = https://github.com/mycroftai/skill-hello-world.git
```

From this we can see that the submodule path is `skill-hello-world` and so for this Skill, the best way to call the test runner is using:

```text
mycroft-start vktest -t skill-hello-world
```

### Skills not yet in the Marketplace

If a Skill has not previously been added to the official Skills Marketplace, then the directory name will be used. 

Imagine I create a "Hey World" Skill at `/opt/mycroft/skills/hey-world-skill`

To guarantee that this Skill is chosen, I would then use:

```text
mycroft-start vktest -t hey-world-skill
```

## YAML Configuration

For more complex or repeatable testing configurations we can use a YAML file to define our test parameters. The configuration of these tests consists of three main settings:

* Platform \(string\) - the platform or device type that the tests are being run on. This must be one of \[default, mycroft\_mark\_1, mycroft\_mark\_2, mycroft\_mark\_2pi, picroft, kde, respeaker\]
* Test Skills \(list\) - the testrunner will execute the tests for these Skills
* Extra Skills \(list\) - additional Skills that will be installed prior to the test. Tests from these Skills will not be executed.

```yaml
platform: mycroft_mark_1
tested_skills:
- mycroft-hello-world
- mycroft-personal
extra_skills:
- cocktails
```

The example above shows that this tests suite:

* is being run on a Mark 1 device
* the tests from the Hello World and Personal Skills will be included
* before running the tests, the Cocktails Skill be installed if it isn't already on the system.

By default the test runner will use the configuration stored at: `mycroft-core/test/integrationtests/voight_kampff/default.yml`.

You can specify an alternate file location with the `-c` flag:

```bash
mycroft-skill-testrunner vktests -c /path/to/your/configuration.yml
```

## Stacking test runs

Voight Kampff has been designed to allow test runs to be stacked.

When you execute a test run the files for that test will be copied into `mycroft-core/test/integrationtests/voight_kampff/features/`

The test files will remain there until cleared, or overwritten. As such each test you run will also execute all previous tests. For example running:

```bash
mycroft-start vktest -t mycroft-weather
```

Will test only the Weather Skill. However if we then run:

```bash
mycroft-start vktest -t mycroft-timer
```

Then both the Weather and Timer Skill will be tested.

## Clearing the test files

To avoid this, or to clean up your system after running tests, we can clear all existing test files:

```bash
mycroft-start vktest clear
```

This will remove all of the Feature and custom Step files that have been transferred during our previous test runs.

## Disabling audio output during testing

If you don't want audio output whilst the tests are running, you can switch this off temporarily through your device configuration.

This change also has the benefit of speeding up testing, as the framework doesn't have to wait for Mycroft to stop talking. Note, that to have audio output during normal debugging the config change should be reverted.

### Using `mycroft-config`

To enable the dummy tts setting and disable audio for tests run the following command:

`mycroft-config set tts.module "dummy"`

To reset the tts settings to default run:

`mycroft-config set tts.module "mimic2"`

### Editing `mycroft.conf`

You can also make the adjustment by editing the config file directly `~/.mycroft/mycroft.conf`.

```javascript
{
  "tts": {
    "module": "dummy"
  }
}
```

To edit the file we recommend using the Mycroft Configuration tool as it will validate your changes on save, warning you about any errors you may have made.

```text
mycroft-config edit user
```

