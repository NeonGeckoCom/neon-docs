# Project Quality Assurance Standards
This document describes the common practices used to ensure code repositories
maintain a reasonable standard of quality. This applies to any projects under
the Neon AI GitHub organization.
> At the time of writing, this is comprised almost entirely of Python projects,
  but this document should apply to all projects as much as possible.

## Standard Automated Tests
All projects should implement the following tests unless there is need for a
specific exception.
- `license_tests`: This checks for any incompatibly licensed dependencies. A
  failure here can be resolved by removing the offending package, allowing an
  exception for the offending package, or allowing the offending license.
- `python_build_tests`: This ensures a package will build, including checks for
  relative path errors in `setup.py`.
- `docker_build_tests`: For packages that include a `Dockerfile`, this ensures
  containers will build for the specified platforms.

### Skills
For skills, there are some additional standard tests.
- `skill_tests`: This shared automation runs tests defined in the `test` directory.
  The shared automation includes boilerplate code for installing/loading the skill.
- `skill_intent_tests`: This tests that intents defined in `test/test_intents.yaml`
  match as expected in different conditions.
- `skill_resource_tests`: This tests that resource files defined in `test/test_resources.yaml`
  are defined in all configured languages.
- `skill_install_tests`: This tests that a skill may be installed and is compatible
  with different common core configurations.
  > At the time of writing, this automation is in need of updates to properly
    test installation since OSM has been deprecated.

## Unit Tests
In addition to the standard test automation, unit tests should also be defined
for any projects that are not in an alpha state. There are currently no specific
guidelines for test coverage, but generally the expectation is that tests are 
added with code changes, such that coverage only increases with each change.

## Contributions and Changes
Any code contribution or change *must not* be accepted with failing tests. There
are a few options for how a failing test may be resolved.

### Newly added test is failing
If a test that was added with other changes is failing, the simplest solution is
to update the new test or code. Consider what the expected behavior is or should
be and update the test or code as necessary.

### Breaking change in new code
If code contributions create a breaking change, consider how to maintain backwards-compatibility
so that existing tests pass. If this is not possible, update the tests to properly
test the new expected behavior.
> Note that a code review may ask that breaking changes be reverted, so check with
  the repository maintainer before updating tests.

### Test failure unrelated to changes
Sometimes a test failure occurs and is unrelated to the changes that triggered
the automation run. In these cases, *tests still must pass before the PR is
merged*. If you are able to diagnose and resolve the failure, it is best to open
a new PR with just those changes and then rebase your other changes after the
test fixes are merged. If you are unsure why tests are failing, ask the repository
maintainer to look into it as they are responsible for fixing that issue.
