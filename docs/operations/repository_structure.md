# Repository Branches and Releases
This document describes how repositories are structured, including branch names,
versioning, and automation. This applies to Plugin, Skill, Service, and Library
repositories as [defined here](https://neongeckocom.github.io/neon-docs/overview/definitions/).

## Default Branches
Repositories always contain `dev` and `master` branches.

| Branch | Version | GitHub Release | Docker Image |
|--------|---------|----------------|--------------|
| dev    | alpha   | Pre-Release    | dev          |
| master | stable  | Release        | latest       |

New code is always PR'd into the `dev` branch as discrete changes. This 
[Alpha Release Workflow](https://neongeckocom.github.io/neon-docs/operations/libraries/workflow_alpha_release/)
describes the process of making changes to a repository. This allows the commit
history of the `dev` branch to be used as a changelog for the project, with each
change corresponding to a specific (alpha) version and a descriptive commit/PR.

When an alpha release is ready to be promoted to a stable release, a PR is created
to merge changes from `dev` into `master` with a version number that generally 
follows [semver](https://semver.org/) or [calver](https://calver.org/). This
[Stable Release Workflow](https://neongeckocom.github.io/neon-docs/operations/libraries/workflow_stable_release/)
describes the process of proposing and approving a stable release. A commit with
the specific version number is created on the `master` branch, along with a GitHub
Release so `master` always reflects the code from the latest release.

## Versioning
Repositories always publish alpha/beta/pre-release versions and latest/stable/normal 
versions. The terms are often used interchangeably, but this document will
follow SemVer's terminology of "pre-release" and "normal release".

Pull requests to `dev` and pre-releases map 1:1, so a merged PR to `dev` 
corresponds to a specific pre-release and vice versa. Pull requests to `master`
map to normal releases in the same way.

A pre-release should always have a corresponding GitHub Pre-release and a normal
release should have a GitHub Release.

For packages published to PyPI, the PyPI version will always match the repository
version. Docker images that correspond to a specific version will always have a
tag matching the repository version.

## GitHub Releases
GitHub pre-release tags will always correspond to a specific "pre-release" and 
releases tags will always correspond to a specific "normal release".
> An exception to this is the Neon OS repository which will list a normal release
  as a pre-release temporarily so the release may be manually validated before
  being made available to users tracking the stable update track.

## Docker Containers
For repositories that publish Docker images, the `dev` image tag will always
match the `dev` branch and the `latest` image will match the `master` branch.
A `stable` tag will always be equivalent to `latest`, tracking the `master`
branch. Images that correspond to any release (pre-release or normal release) 
will have an additional tag indicating the version.

A repository may optionally publish `alpha` tagged images that do not correspond
to a specific version or tag. These are used for development and testing and should
be considered unstable references. If `alpha` images are used in a repository,
they should track a branch named `alpha`.

## Kubernetes Deployments
In general, the `alpha` namespace will track `alpha` or `dev` images,
`beta` namespace will track `dev`, and `prod` will track specific stable versions.
`prod` will often specify versions equivalent to `latest` for each service, but
it should specify version tags to prevent accidental updates when a new `latest`
image is tagged and pods are re-created for some reason.
