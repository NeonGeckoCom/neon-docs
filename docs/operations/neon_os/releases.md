# Neon OS Releases
Neon OS releases can be primarily differentiated by a `core` version and
an `image` version, where `core` refers to the repository providing the primary
functionality (i.e. `neon-core` or `neon-nodes`) and `image` refers to code from
the `neon_debos` repository. A particular OS release  is identified by the 
timestamp at which it was created.

## Identifying Updates
Updates are identified as OS releases with the same `core` and a newer timestamp.

An OS release is identified as a beta if EITHER the `core` or the `image` ref
used is a beta. A device on a beta update track will only update to a newer
beta version; a device on a stable update track will only update to a newer
stable version. If a device changes tracks, it will update to a NEWER release on
the new track, but it will not install an older version by default.
> Note that in practice, any stable update will have first been released to the
> beta track.

## Version Management
Released images will be identified by GitHub releases in the 
[neon-os](https://github.com/neongeckocom/neon-os) repository.
This dedicated repository will use GHA to automate image generation when a 
relevant PR is merged in either the `core` of `image` repository according to
the Beta or Stable Release Workflow.

### Versioning Scheme
Releases will follow [CalVer](https://calver.org/), so a release version may be
`24.02.14` or `24.02.14b1`.