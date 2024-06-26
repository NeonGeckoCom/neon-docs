# Neon OS
Neon OS is an operating system for using Neon AI® on a dedicated device. 

## Releases
Neon OS releases can be primarily differentiated by a `core` version and
an `image` version, where `core` refers to the repository providing the primary
functionality (i.e. `neon-core` or `neon-nodes`) and `image` refers to code from
the `neon_debos` repository. A particular OS release is identified by a
`build version` string based on the time at which the release was compiled.

## Identifying Updates
Updates are identified as OS releases with the same `core` and a newer 
`build version`.

A `build version` is marked as a beta if either the `core` or the `image` ref
used is a beta (these will always be the same ref in automated builds). 
A device on a beta update track will only update to a newer
beta version; a device on a stable update track will only update to a newer
stable version. If a device changes tracks, it will update to a NEWER release on
the new track, but it will not install an older version by default.
> Note that in practice, any stable update will have first been released to the
> beta track.

## Version Management
Released images are identified in GitHub releases in the 
[neon-os repository](https://github.com/NeonGeckoCom/neon-os). The `yaml`
index files may also be used to view release history per-image. Each yaml index
entry has a `version` key that corresponds to a unique build; two builds may have
the same `core` and `image` versions but different build versions based on when
they were created.

### Versioning Scheme
Releases will follow [CalVer](https://calver.org/), so a release version may be
`24.02.14` or `24.02.14b1`. Note that the GitHub beta tags will *not* match the 
associated images' versions for beta versions since each release may relate to a
different `core`.
> i.e. Neon OS tag `24.02.27b1` may contain `debian-neon-image-24.02.27b1` 
> and Neon OS tag `24.02.27b2` may contain `debian-node-image-24.02.27b4`.

## Glossary
- `core`: the module/repository providing the primary functionality.
- `image`: the framework/repository building the OS image (`neon-debos`).
- `build id`: `recipe`-`platform` string identifier (i.e. `debain-neon-image-rpi4`).
- `build version`: the version identifier for a specific release image (i.e. `24.03.01b1`).
- `release`: a GitHub release in the Neon OS repository, identified by the `build version`.

## Build System
[Neon OS](https://github.com/neongeckocom/neon-os) images are built using recipes 
in [neon-debos](https://github.com/NeonGeckoCom/neon_debos).
OS images and updates are served via HTTP server with a predictable directory
structure. The build scripts are located in the [neon-os repository](https://github.com/neongeckocom/neon-os).

## Build Image Action
An OS build is triggered via GitHub actions and generally runs on a self-hosted
runner. This action may build for multiple platforms (i.e. rpi4, opi5) and may
build for multiple [cores](../neon_os/index.md#glossary). The following inputs 
are accepted:
- REF: `beta` or `stable` to indicate if the build is a beta or stable release.
  > This was previously labelled as `dev` and `master` as it was tied to the
    `core` branch used in the build.
- DEBOS_REF: branch of `neon_debos` to check out, generally `dev` or `master`
- CORE_REF: branch of the [core repository](../neon_os/index.md#glossary) to
  check out, generally `dev` or `master`
- PLATFORMS: space-delimited list of platforms to build for (i.e. `"rpi4 opi5"`)
- UPLOAD_URL: base URL outputs may be downloaded from (i.e. `"https://2222.us"`)
- OUTPUT_DIR: directory to move outputs to, may be a local or a remote directory
  spec (i.e. `/var/www/html/neon_images`, `<remote>:/<path_to_directory>`)
- DO_CORE: boolean value, if true then build a release with the `debian-neon-image` recipe
- DO_NODE: boolean value, if true then build a release with the `debian-node-image` recipe

## Build Outputs
Output images are served via HTTP server, based on the inputs to the 
[build image action](#build-image-action).

An output image looks like:
```text
<core>/<platform>/<ref>/<build_id>_<timestamp>.img.xz
 node /   opi5   / dev / debian-node-image-opi5_2024-06-06_16_25.img.xz
```

Update files include a `squashfs` update and `json` metadata:
```text
<core>/<platform>/updates/<ref>/<build_id>_<timestamp>.<ext>
 node /   opi5   /updates/ dev / debian-node-image-opi5_2024-06-06_16_25.squashfs
```