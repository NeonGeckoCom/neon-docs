# Standard Development Workflow
This document describes how to contribute to repositories under the Neon AI
GitHub organization. These best practices help to ensure contributions can be
efficiently reviewed and accepted by project maintainers while maintaining a
clear commit history and high standard of quality.

## Branch Naming
When creating a new branch to work on changes, it is recommended to use the
format `<type>_<description>`, where <type> is `feat`, `fix`, `doc`, or a 
similar descriptor of the nature of changes being contributed and <description>
is a brief description in kebab-case. For example, the branch that adds this
page to documentation is labeled `doc_developer-workflow`.
> This is similar to [Conventional Branch](https://conventionalbranch.org/), 
  though there are cases where `/` in a branch name can cause issues with
  git ref parsing that naively assumes it can segment the complete ref on `/`.

This naming convention is not a hard requirement for submitting changes, but
it is recommended to help quickly organize and manage branches. Note that 
branch names are case-sensitive, but it is recommended to use lowercase strings
for consistency and to avoid potential conflicts on file systems that are not
case-sensitive (i.e. APFS on macOS).

## Committing Changes
Since PRs will squash the commit history into a single commit on the base branch,
the feature branch commit history is unimportant. Developers are encouraged to
commit changes in whatever manner is most efficient for their workflow.
> [git-scm](https://git-scm.com/docs/git-commit) has a good description of how
  the `git commit` command works and its application.

## Pull Requests
PRs should always be opened with the repository default branch as the base
(`dev` in most cases). PR titles should be a short description of changes
in plain English; for clarity, they should not contain any reference to other
PR or Issue numbers.
> More information about Pull Requests can be found in the
  [GitHub Docs](https://docs.github.com/en/pull-requests).

### PR Description
PR descriptions should use the configured template if present so that changelogs
and release notes are consistent. Following the template, the Description section should
include a detailed description of changes; the organization is not specified, so
this may be a list of changes or a free-form description of the changes. The
description may include rationale for the changes and design decisions, though
this is not strictly required; the description should remain focused on the
changes contained in the PR.

The Issues section should contain a bulleted list of related issues and PRs,
both in the current repository and other repositories. Any issues that are
closed by the PR should use the "Closes" keyword so that they are automatically
closed when the PR is merged. There may be related issues that are not closed,
but they should still be included here for reference; this helps when reviewing
issues since related changes will be clearly visible in the issue history.
> A bulleted list is specified here because GitHub will render more detailed
  links in a bulleted list than it does in a regular text block.

The Other Notes section is available to include any additional information or
context that is relevant to the PR. This is a good place to include links to
references, justification of design decisions, edge cases that are handled, or
any other background information.

This format is not a hard requirement, but it is recommended to help PR 
reviewers quickly understand the changes and to maintain a consistent format
when reviewing previous changes/releases.

### PR Reviews
All PRs must receive an approving review before they may be merged with very
few exceptions. The review process is the most important control that ensures
we maintain a high standard of quality and that every change properly implements
its intent. The process for completing a review is described in [Reviewing a Pull Request](./review_pr.md).

When a PR is complete, it should not be marked as a draft and one or more 
reviewers should be requested. If the PR is not complete but a review is still
desired for feedback on WIP changes, or to discuss design decisions, it is best
to leave a comment with those specific questions, tagging the reviewer.

### GitHub Actions
Many repositories have configured automation that runs when a PR is opened or
commits are added to an open PR. Some of these will not run until the PR is
marked ready for review, while others will run even on draft PRs. In either 
case, a PR may not be merged until all checks have passed with no exceptions.

### Merging PRs
A PR is ready to be merged when at least one approving review has been left and
all checks have passed. If there are any reviews left requesting changes, those
changes must be addressed and that reviewer must leave an approving review. For
example, if there are two reviews requesting changes, then both reviewers must
approve the PR before it may be merged.

Once the PR is ready to be merged, it should be merged using the
"Squash and Merge" option. This ensures that the commit history of the base
branch remains clean and that the changes in the PR are represented as a
single commit with the PT title and description in the commit history.
> A PR should never be merged with a "Merge Commit" or "Rebase and Merge" as
  this will create a complex commit history on the base branch. With more 
  commits, it becomes difficult to parse the commit history and rebasing other
  feature branches is much more difficult.

## Rebasing Branches
Since multiple PRs may be open at the same time, and merge order is generally
unknown, it is likely that a PR will need to be rebased on the base branch at
some point. If a single author has multiple open PRs, they may choose to 
define the merge order by marking PRs as Drafts, or leaving comments in open
PRs to indicate an intended merge order.
> The [git-scm Cheat Sheet](https://git-scm.com/cheat-sheet#combine-diverged-branches)
  has a good visual representation of what rebasing does.

### Resolving Conflicts
When rebasing a branch, it is possible that merge conflicts will arise due to
changes in the base branch overlapping changes in the feature branch. When this
happens, the developer performing the rebase must make decisions about how to
resolve the conflicts.
> Since these conflicts can be complicated to resolve, it may be best to defer
  to or collaborate with the developer(s) who are responsible for the
  conflicting changes.

Since resolving conflicts is a potentially destructive operation, it is
recommended to create a backup of the feature branch being rebased; this may
be done locally or by creating a new branch on GitHub, based on the
feature branch. With a safe backup in place, resolve the conflicts and complete
the rebase.
> Note that because rebasing is basically re-playing each commit on the feature
  branch, there may be changes that need to be resolved where the result does
  not match either the base branch or the feature branch.

Once the rebase is complete, the local feature branch should be force pushed to
the remote (overwriting the previous branch that lacked recent commits on the
base branch). In nearly all cases, the PR diff should look identical to what it
was before the rebase.

### Rebase Tips
- If rebasing a complex feature branch, it may be easier to squash commits on
  the feature branch before rebasing. This reduces the number of commits that
  need to be rebased and avoids tricky merge conflicts that come up when the
  feature branch has multiple changes to the same chunk of code.
- If during a rebase you find something has gone wrong, you can always abort
  and try again using `git rebase --abort`. This will return the local branch
  to the state it was before the rebase started.
- If you find the PR diff has changed in a way you didn't expect after force
  pushing changes, you can look at the backup branch to compare the changes.
  To resolve, you can either start over by using `git reset --hard <backup-branch>`
  to return to the pre-rebase state, or you can create an additional commit to
  manually correct the rebase errors.
