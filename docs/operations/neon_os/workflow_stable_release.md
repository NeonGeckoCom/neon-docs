## Stable Release Workflow
This describes the process for pushing a stable Neon OS release

### Application
This process applies in the following situations
- A new `stable` version of a `core` package is released
- A new `stable` version of `neon_debos` is released
- A major fix is applied to a `core` package dependency

### Process
- Upon a new release in the `core` or `neon_debos` repository, automation will
  trigger an image build for the affected edition(s) of Neon OS.
- The developer approving release will verify automation has started.
- Upon automation completion, the developer approving release will validate the
  update applies properly from a previous version.
  > If the update does not properly apply, follow the procedure for removing a release
- The developer approving release will validate the GitHub release and changelog
  for completeness and accuracy, including any expected artifacts
- The developer approving release will wait a minimum of 24 hours to receive
  feedback from users on the `beta` track in case there are new issues to 
  address.
- If there is no feedback to be addressed, the developer approving release will 
  mark the release as `latest` and announce the release in the relevant thread on
  [the forums](https://community.openconversational.ai).

### Exceptions
- A maintaining developer may choose at their discretion to push a new stable
  release at any time, provided they validate the release functions as expected
  and stable versions of the `core` and `neon_debos` packages are used

## Removing a Release Workflow
This describes the process for removing a beta or stable Neon OS release

### Application
This process applies in the following situations
- It is dictated by a release workflow to remove a release
- A major issue is found and a maintainer deems it necessary to remove a release

### Process
- Delete the release from GitHub
- If release files have been uploaded to any public servers for download, remove
  them
- If an announcement has been made in the forums, edit the announcement to 
  indicate the release has been removed, including a reason for its removal

### Exceptions
- There should be no exceptions to this workflow.