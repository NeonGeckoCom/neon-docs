## Undo a Stable PR Workflow
This describes the process for undoing stable release automation on the `dev`
branch of a repository.

### Application
This process applies in the following situations:
- It is dictated by a review workflow to make changes to a proposed stable release.
- A release workflow is run in error.
> This process must be completed by a repository administrator.

### Process
- Checkout the `dev` branch of the affected repository.
- Find the commit prior to the stable release automation.
  > This is usually an automatic commit `Update Changelog` or `Increment Version to <version>`.
- Force reset your local `dev` branch to the identified commit.
- Force push your local `dev` branch to the remote.
  > This will require administrator permissions
- Validate the remote repository is at the expected commit.

### Exceptions
- There should be no exceptions to this process.
