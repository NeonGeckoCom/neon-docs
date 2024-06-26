## Skill Release Workflow
This describes the process for pushing a skill release.

### Application
This process applies when pushing a stable release for a skill.

### Process
- Verify that the current alpha version of the skill is specified in
  `NeonCore` dependencies (if applicable).
- Validate skill functionality in a Neon OS and/or Iris deployment (as applicable).
- Compare the latest alpha release to the latest stable release to determine the `release type`:

    - If any breaking changes are present, the next version will be a `major` release.
    - If any non-breaking functional changes are present, the next version will be a `minor` release.
    - If no functional changes are present (i.e. only bugfixes or optimizations), the next version
      will be a `patch` release.

- Look at package requirements to verify there are no alpha dependencies.
  > If there are any alpha dependencies, create an alpha release with no alpha dependencies and
    start this process over.
- Run the `Propose Stable Release` workflow with the determined `release type`.
- Upon workflow completion, review the generated PR.
  > The PR may need to be closed and re-opened to trigger automation.
- Review the PR as normal and request a review from another developer.
- Upon approval, `Create a Merge Commit` to create the stable release.
  > If changes are required, a maintaining developer is responsible for closing the PR
    and force-pushing the `dev` branch to the last commit prior to the release automation.
    After this is completed, a PR to `dev` should be created, referencing the PR where
    changes were requested.
- If the skill is a dependency of `NeonCore`, create a PR there to update dependencies
  to the new version. This will ensure new releases are created for Docker and NeonOS.

### Exceptions
- A maintaining developer may choose at their discretion to allow alpha dependencies in 
  optional extras, i.e. test dependencies.
- A maintaining developer may choose at their discretion to allow a pinned alpha
  dependency for a stable release if the pinned version has been adequately 
  tested, and it is unreasonable or impossible to use a stable release for the
  dependency.
- A maintaining developer may choose at their discretion to not request another review if
  tests are passing and there is not another developer qualified and available to review.