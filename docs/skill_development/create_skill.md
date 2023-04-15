# Create a Skill
This document summarizes how to create a skill, following current best practices.

## Create a Repository
First, create a GitHub repository to start working on your new skill. Start with
the [template-skill](https://github.com/NeonGeckoCom/template-skill) to get all
of the boilerplate code in place, including all required files.

Make sure your new repository is able to run all of the GitHub actions by going
to `Settings` -> `Actions` -> `General` and making sure both 
"reusable workflows" are allowed and "Read and write permissions" is checked.

## Update the Template
The next step is to clone your new repository locally and start making changes.
To start, update the boilerplate files to include information for your new skill
Using the [Holiday skill as an example](https://github.com/NeonGeckoCom/skill-holidays/pull/1/commits/efd662d538b84f4278e124e57b5255578739ee8d),
you will want to update `README.md` with details for your specific skill, optionally
including an icon (`logo.svg` in our example).

`__init__.py` should be updated so the Skill object has a meaningful name and
to pass a string "name" that can be used to identify the skill.

`setup.py` should be updated to specify a `SKILL_NAME` that uniquely describes
the skill and also update the `PLUGIN_ENTRY_POINT` to reflect the changes made 
in `__init__.py`. In the `setup` call, make sure all passed arguments are valid.

You may also want to update "LICENSE.md" to use a different license than the
default BSD-3 used in the template repository.

## Getting Started on Skill Logic
With the boilerplate changes out of the way, the next step is to work on the
actual skill. Using our [Holiday skill example](https://github.com/NeonGeckoCom/skill-holidays/pull/1/commits/9fcb26769347e8bd6c37f20f66230f5eb445a9e6),
we start by adding some helper methods and defining some variables in our skill.
At the same time, we update `requirements.txt` to include any dependencies.

We also update `test/test_skill` to test our added methods to make sure everything
is working as expected. It helps to write these tests as you go so you catch errors
early and don't have to debug an unexpected behavior after adding all of the layers
of complexity.

For this example, we extended the `CommonQuerySkill`, because we will want to handle
generic "W" questions (who/what/when/where/why). More on that later.

### Bonus: Caching content for offline usage
Since our skill uses an online resource for relatively static information, we can
include a local version of that data in case a user skips internet setup or the
online data provider goes down for some reason. In [this commit](https://github.com/NeonGeckoCom/skill-holidays/pull/1/commits/cc8750f86d98c70bb6edd711cb58557de6a60758),
`holidays.json` is created with a current version of what the skill stores
internally. Note that this file is added to `package_data` in `setup.py` to be
included when a user installs the skill.

## Common Query Handling
Since this skill is intended to handle requests like "when is Valentine's Day",
we extend "CommonQuerySkill" rather than making a specific intent. To do this,
we implement `CQS_match_query_phrase` [in this commit](https://github.com/NeonGeckoCom/skill-holidays/pull/1/commits/ca8ec99a4a8594416237b694b4a373607078f5dd0).
A `match_level` is specified, indicating how sure we are that this skill's response
is relevant to the user's request and if we don't find a holiday in the request,
we return `None`.

Our skill uses a method `_format_response` that generates a
string to be spoken if the skill response is the best answer to the user's question.

We also add some `vocab` and `dialog` resources here which are added to `test_resources.yaml`.
`common query` intent tests specify some example utterances and some expected
confidence levels.
More information about tests can be found in the [Skill Testing Docs](https://neongeckocom.github.io/neon-docs/skill_development/skill_testing/).

## Adding an Intent Handler
Common Query will handle some requests, but we also want to reply if a user asks
"what holiday is on June 19?". To do this, we add a 
[Padatious Intent](https://neongeckocom.github.io/neon-docs/skill_development/padatious_intents/).

In [our skill](https://github.com/NeonGeckoCom/skill-holidays/pull/1/commits/0f774aee802dcb593db0fb56886c2099dfedc4dd),
we define `holiday_on_date.intent` and the handler `handle_holiday_on_day`. If
a date is found in the user's request, then the skill checks for a holiday on that
day and speaks a response.

We also add [intent tests](https://neongeckocom.github.io/neon-docs/skill_development/skill_testing/) to go with these changes, as well as a test for the
handler.

## Addressing Test Results
In our Common Query tests, the skill was responding to "what is Memorial Day" with
a higher confidence than is desired, since another skill could provide a better
reply than just the next date for Memorial Day. To get the confidence below the
maximum we defined in the test, we simply [reduce the confidence level for
"general" queries](https://github.com/NeonGeckoCom/skill-holidays/pull/1/commits/46c8d1d35578b9756758934fa7437f73767332a2).

We don't want to omit a response completely since giving the date for Memorial
Day is better than saying "I don't know" if no other skill responds (i.e. if
offline).

## Getting Ready to Release
Unit tests are a great way to make sure things work as expected, but you should
still test your skill out before the first release. To install your skill, you
can run `pip install git+<github_url>@<branch>`. For our example, this would be:
`pip install git+https://github.com/neongeckocom/skill-holidays@FEAT_InitialImplementation`.

Make sure to look for any `pip` dependency resolution errors and try to address
them; unit tests should have caught any incompatibilities with core packages,
but another skill or plugin might have a conflict.

Our example skill had a version conflict with another skill during testing, so
we simply [loosened our dependencies](https://github.com/NeonGeckoCom/skill-holidays/pull/1/commits/02d372dda27928ca47c76eca8fba4f54ab1f6338).


