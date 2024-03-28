## Beta Release Workflow
This describes the process for pushing a release to the beta track.

### Application
This process applies in the following situations:
- A new `beta` version of a `core` package is released.
- A new `beta` version of `neon_debos` is released.

### Process
- Upon a new release in the `core` or `neon_debos` repository, automation will
  trigger an image build for the affected edition(s) of Neon OS.
- The developer approving release will verify automation has started.
- Upon automation completion, the developer approving release will validate the
  update applies properly from a previous version.
  > If the update does not properly apply, follow the procedure for removing a release.
- The developer approving release will validate the GitHub release and changelog
  for completeness and accuracy, including any expected artifacts.

### Exceptions
- The developer approving release may opt to test only one target platform, rather
  than all supported platforms.
- A maintaining developer may choose at their discretion to push a new beta
  release at any time.