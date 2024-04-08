## Alpha Release Workflow
This describes the process for pushing an alpha release for a `library` module.
A `library` module is a module or repository that is a dependency of one or more
Neon `services` or `plugins`.

### Application
This process applies when making any change to a `library` module.

### Process
- Create a branch from the `dev` branch of the module.
- Complete any changes, including test coverage, documentation updates, and
  dependency changes.
- Create a Pull Request to the `dev` branch.
  > Ensure the PR title is descriptive and the template is completed. A description
    is required and relevant issues must be noted as either related to the PR or
    closed by the PR.
- If any tests fail, mark the Pull Request as a Draft and make necessary changes.
- After tests are passing, request a review from a maintainer.
  > If the PR was marked as a draft, mark it ready for review here.
- Upon approving review, `Squash and Merge` changes to the `dev` branch

### Exceptions
- A maintaining developer may choose at their discretion to skip the review process if
  tests are passing and there is not another developer qualified and available to review.
  > If there is no code coverage automation, the maintaining developer is responsible
    for ensuring coverage for any changes.