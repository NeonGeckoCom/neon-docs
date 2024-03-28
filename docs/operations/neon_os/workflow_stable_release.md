## Stable Release Workflow
This describes the process for pushing a stable Neon OS release.

### Application
This process applies in the following situations:
- A new `stable` version of a `core` package is released.
- A new `stable` version of `neon_debos` is released.
- A major fix is applied to a `core` package dependency.

### Process
- Upon a new release in the `core` or `neon_debos` repository, relevant editions
  of Neon OS are eligible for release.
- If a Neon OS release has already been pushed on this calendar day, another 
  release will have to wait until the next day for consistent versioning.
  > For this reason, a stable release should be pushed after all relevant `core`
    and `neon-debos` stable releases for the day are completed.
- The developer approving release will run the workflow to trigger a release in
  the [neon-os repository](https://github.com/NeonGeckoCom/neon-os/actions/workflows/manual_release.yaml)
  and verify the automation has started.
  > The developer approving release is responsible for selecting the `master` 
    branch and the appropriate repo (`neon-debos`, `neon-nodes`, or `neon-core`).
- Upon automation completion, the developer approving release will validate the
  update applies properly from a previous version.
  > If the update does not properly apply, follow the procedure for removing a 
    release.
- The developer approving release will validate the GitHub release and changelog
  for completeness and accuracy, including any expected artifacts
- The developer approving release will wait a minimum of 24 hours to receive
  feedback from users on the `beta` track in case there are new issues to 
  address.
- If there is no feedback to be addressed, the developer approving release will 
  mark the release as `latest` and announce the release in the relevant thread 
  on [the forums](https://community.openconversational.ai).

### Exceptions
- A maintaining developer may choose at their discretion to push a new stable
  release at any time, provided (1) they validate the release functionality
  and (2) stable versions of the `core` and `neon_debos` packages are used.
