## Alpha Release Workflow
This describes the process for pushing a skill alpha release.

### Application
This process applies when making any change to a skill.

### Process
- Create a branch from the `dev` branch of a skill.
- Complete any changes, including test coverage, documentation updates, and
  dependency changes.
- Create a Pull Request to the `dev` branch.
- If any tests fail, mark the Pull Request as a Draft and make necessary changes.
- After tests are passing, request a review from a maintainer.
  > If the PR was marked as a draft, mark it ready for review here
- Upon approving review, `Squash and Merge` changes to the `dev` branch
- If the skill is a dependency of `NeonCore`, create a PR there to update dependencies
  to the new version. This will ensure new releases are created for Docker and NeonOS.

### Exceptions
- A maintaining developer may choose at their discretion to skip the review process if
  tests are passing and there is not another developer qualified and available to review.