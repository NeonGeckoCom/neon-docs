# Working With Git(Hub)
This overview is meant to define some commonly used git and GitHub concepts as 
well as document how Neon code is managed.

## Definitions
### Git
- [Repository](https://git-scm.com/book/en/v2/Git-Basics-Getting-a-Git-Repository) -
  A directory with changes tracked by git
- [Refs](https://git-scm.com/book/en/v2/Git-Internals-Git-References) -
  Human-readable names given to a particular `commit`
- [Commit](https://git-scm.com/docs/git-commit) - A unit of code changes as determined by the code author
- [Branch](https://git-scm.com/docs/git-branch) - A version of code, built off of some specified `ref`
- [HEAD](https://git-scm.com/book/en/v2/Git-Internals-Git-References) A reference to the current `branch`
- [Remote](https://git-scm.com/docs/git-remote) - A reference to a remote code `repository`
- [Push](https://git-scm.com/docs/git-push) - Copy new `commits` from a local 
  `ref` to a remote `ref`. These `refs` usually have the same names on the local and remote, but not necessarily
  - Force Push - Copy the `commit` history from the local `ref` to the remote `ref`. 
    Note that this can result in losing `commits` that exist on the `remote` but not the `local` `branch`. 
    You usually do this after rebasing changes or dropping `commits`.
- [Tag](https://git-scm.com/book/en/v2/Git-Basics-Tagging) - A `ref` that is tied
  to a static `commit` and is not mutable; the `tag` will always reference the same `commit`

### GitHub
- [Fork](https://docs.github.com/en/get-started/quickstart/fork-a-repo) - 
  A copy of a `repository` at a particular point in time/`commit` history
- [Pull Request](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/about-pull-requests) - 
  A request to include `commits` from some `head` branch into a `base` branch 
- [Release](https://docs.github.com/en/repositories/releasing-projects-on-github/about-releases) - 
  Basically a git `tag` but specific to GH with some additional UI/Notifications 
- [Upstream](https://docs.github.com/en/get-started/quickstart/fork-a-repo) - 
  The parent repository of a `fork`
- Organization (Org) - An entity that has `repositories` (like a user) and that is managed by several users with various permissions

## Common Workflows
### Pull Requests
Pull Requests have three options for resolution (exluding closing the PR):

- Create a merge commit - Creates a new commit to the base branch AND applies all commits from the head branch to the base branch
- [Squash and merge]((https://docs.github.com/en/desktop/contributing-and-collaborating-using-github-desktop/managing-commits/squashing-commits)) - 
  A shortcut to rewriting Git history. Takes multiple commits and translates all changes into a single commit on the base branch
- Rebase and merge - Rebase all commits from the head to the base

#### Squash and Merge
This is generally desired for merging feature/bugfix `branches` into a shared development `branch`. 
This means 1 `commit` == 1 logical change and makes commit history more readable.

#### Create a Merge Commit
This is generally used when merging changes from a development `branch` into a 
master or release `branch`. This preserves the `commit history` and adds `commits` 
that indicate when/where the release `branch` was updated. In most cases, a `commit` 
to the release `branch` corresponds to a `tag` and `release` that will reference the `merge commit`.

#### Rebase and merge
This could be useful if contributing to a feature `branch`. This would maintain `commit history` without generating an extra merge `commit`.

### [Rebase](https://git-scm.com/docs/git-rebase)
When working on a `branch`, it is often necessary to `rebase` your changes on other 
changes merged into the `base` while you are working on a feature. Rebasing is 
the process of taking the `commits` from your `branch` and applying them after the 
`commits` on the `base` `branch`. If there are conflicts, you will have to resolve 
them, which modifies the `commits` on your `branch`. After resolving conflicts, 
you should Force Push your `branch` to the `remote`.

### [Cherry Pick](https://git-scm.com/docs/git-cherry-pick)
When working with a `fork`, it is often helpful to `cherry-pick` changes from the `upstream`. 
There are a couple common situations where `cherry-picking` is useful:

- Projects forked from an open source repository where the fork is diverged from the original repository
- `Branches` that include both fixes and features may be split into 2 `branches` with 2 `PR`’s.
  The “FIX” `PR` could `cherry-pick` the relevant commits from the “FEAT” `PR` 
  and the “FEAT” `PR` would later be `rebased` after the “FIX” PR is `merged`

### Drop
Sometimes a `commit` is accidentally added or was added for testing and the 
changes were not successful. In these instances, `dropping` the `commit` is 
generally easier and cleaner than creating an additional `commit` to roll back 
the changes. It may also be useful to drop `commits` that were `cherry-picked` 
to a different `fork` as described [above](#cherry-pick).

## Resources/References
[GitHub quick reference](https://training.github.com/downloads/github-git-cheat-sheet.pdf)

[Official Git documentation]( https://git-scm.com/docs)

