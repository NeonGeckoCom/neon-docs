## Skill PR Review Workflow
This describes the process for reviewing a skill PR.

### Application
This process applies when reviewing a PR to the `dev` or `master` branch of any skill.
> Note that part of the process only applies to stable releases.

### Process
- Review changes to imports and verify dependency minimum versions are accurate.
- Ensure code changes have adequate unit test coverage.
- Ensure resource changes have adequate resource and intent test coverage.
- Highlight any breaking changes and consider how they could be made backwards-compatible.
  > Note that breaking changes will not necessarily block approval, but it is up to the
    PR author and reviewer to determine if the breaking change is appropriate.
- Highlight any added `TODO` or existing `TODO` comments related to the changes.
  > It is up to the PR author and reviewer to decide if a `TODO` must be completed
    before merging OR if an issue can be created to address it in a later PR.
- If reviewing a `Stable Release`:
  - Validate there are no alpha dependencies
  - Validate that the skill works as expected in an alpha deployment (i.e. Iris, HANA, or Neon OS).
    If the skill is not deployed to any of those, test agaomst a local installation.
  - Ensure the skill is not generating any `ERROR` or `WARNING` logs.
- Leave a review to `Approve` or `Request Changes`. If reviewing a draft or if unable to test 
  changes, leave a `Comment` review indicating why it isn't an Approval, 
  i.e. "Unable to test." or "Noted pending TODO"

### Exceptions
- A maintaining developer may choose at their discretion to allow a pinned alpha
  dependency for a stable release if the pinned version has been adequately 
  tested, and it is unreasonable or impossible to use a stable release for the
  dependency.