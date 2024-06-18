## Library Release Workflow
This describes the process for pushing a `library` module release.

### Application
This process applies when pushing a stable release for a `library`.

### Process
- Verify the current alpha version is used in any projects that depend on the
  proposed changes.
- Compare the latest alpha release to the latest stable release to determine the `release type`:

    - If any breaking changes are present, the next version will be a `major` release.
    - If any non-breaking functional changes are present, the next version will be a `minor` release.
    - If no functional changes are present (i.e. only bugfixes or optimizations), the next version
      will be a `patch` release.

- If this is a `major` release:

    - Ensure that any deprecated methods had deprecation warnings in the previous 
      stable release.
    > If they did not, create a PR to replace the removed methods and add a 
      deprecation warning and start this process over.
    - Ensure that anything marked for deprecation in this release has been removed.
    > If there is code marked for deprecation in this release, create a PR to 
        remove the relevant code and unit tests and start this process over.

- Look at package requirements to verify there are no alpha dependencies.
  > If there are any alpha dependencies, create an alpha release with no alpha dependencies and
    start this process over.
- Run the `Propose Stable Release` workflow with the determined `release type`.
- Upon workflow completion, review the generated PR.
  > The PR may need to be closed and re-opened to trigger automation.
- Review the PR as normal and request a review from another developer.
- Upon approval, `Create a Merge Commit` to create the stable release.
  > If changes are required, a maintaining developer is responsible for closing the PR
    and [reverting changes on the `dev` branch](https://neongeckocom.github.io/neon-docs/operations/git/undo_stable_pr/).
    After this is completed, a PR to `dev` should be created, referencing the PR where
    changes were requested.
### Exceptions
- A maintaining developer may choose at their discretion to allow alpha dependencies in 
  optional extras, i.e. test dependencies.
- A maintaining developer may choose at their discretion to allow a pinned alpha
  dependency for a stable release if the pinned version has been adequately 
  tested, and it is unreasonable or impossible to use a stable release for the
  dependency.
- A maintaining developer may choose at their discretion to not request another review if
  tests are passing and there is not another developer qualified and available to review.
