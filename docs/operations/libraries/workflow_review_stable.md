## Library Module PR Review Workflow
This describes the process for reviewing a PR to a `library` module.

### Application
This process applies when reviewing a PR to the `dev` branch of a `library` module.

### Process
- Review changes to imports and verify dependency minimum versions are accurate.
- Ensure there are no alpha dependencies
- Ensure code changes have adequate unit test coverage.
- Ensure any deprecated methods have adequate logs.
- Ensure there are no breaking changes.
  > If this is a major release, then there may be breaking changes. If there are,
    then ensure the previous releases had a deprecation warning with this
    proposed release version listed.
- If tests are modified, make sure newly deprecated methods are still tested and
  that changes do not remove any test cases/
- Ensure any added `TODO` items have not been completed and are related to an 
  open issue.
- Leave a review to `Approve` or `Request Changes`. If reviewing a draft or if 
  unable to test changes, leave a `Comment` review indicating why it isn't an 
  Approval, i.e. "Unable to test." or "Noted pending TODO"

### Exceptions
- A maintaining developer may choose at their discretion to allow a pinned alpha
  dependency for a stable release if the pinned version has been adequately 
  tested, and it is unreasonable or impossible to use a stable release for the
  dependency.