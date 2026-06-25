# Reviewing a Pull Request
This document describes the process for reviewing a pull request (PR). Specific
review criteria may apply to different projects, but this document aims to
cover the general process that applies to Neon AI repositories.
> See also [Project QA Standards](../qa_standards.md) for
  more information about expectations for testing and
  automation.

## When to Review
A PR should generally be reviewed by requested reviewers when it is marked 
"Ready for Review". If a review is requested on a Draft PR, the reviewer should
read the PR description and comments to understand if the author is looking for
specific feedback or input.
> Note that a PR may be marked "Ready for Review", but pending or failing 
  checks may result in further changes before the PR is actually ready. In
  these cases, it is best to notify the PR author of the issues and hold a
  full review until they are resolved.

## Review Process
The purpose of PR reviews is to ensure that changes are complete, described
accurately and completely in the PR description, adequately tested, and that
any relevant issues are linked and resolved or otherwise partially addressed.

### 1. Read the PR Title, Description, and Comments
The PR title should be a succinct and understandable statement of the changes
in the PR. If unclear, edit or ask the author to do so. The PR description
should provide an understandable summary of the changes and highlight any
specific trade-offs or design decisions. If any of those design decisions don't
make sense or there is a better alternative, quote the relevant line in a 
comment to discuss with the PR author. Be sure to read the comment history as
well in case the decision has already been discussed.

Once there is clear alignment on the goal of the PR and design discussions are
resolved, the code is ready for review.

### 2. Review the Code Changes
Code changes should be reviewed for correctness, readability, consistency, and
scope. Much of the review process is subjective, so this document will only aim
to provide guidance for things to look for, rather than a rubric for evaluation.

#### Correctness
Changes should obviously be free of syntax errors and other bugs. Ensure that
any changes come with test coverage or that they can be manually validated if
adding tests is not feasible. Consider any edge cases or potential exceptions
and confirm they are handled or at least acknowledged. If there are undefined
behaviors that are not explicitly handled or discussed in the PR, flag them as
issues to be resolved.

#### Readability
Review new methods, functions, and variables for clear names and type annotations
as appropriate. New methods and functions should have docstrings that match the
format of other docstrings in the repository. If there is complex logic that
isn't clear, make sure it has a comment explaining its purpose. Ensure that
any exception handling includes clear logs and that any raised exceptions use
an appropriate exception type and message.

#### Consistency
Ensure that changes are consistent with the existing codebase. New functions and
classes should integrate with existing modules where it makes sense and
signatures should follow the same format as existing code. Suggest changes as
appropriate to help the author address any issues.

#### Scope
All changes in the PR should be constrained in scope to one specific feature,
fix, documentation task, or other discrete change. If a PR includes multiple
unrelated changes, flag the changes that are out of scope to request they be
separated into their own PR(s). This can be particularly helpful if some
changes are complete and ready to merge, while others have extensive feedback
to address.
> Note that this may lead to PRs that are dependent on each other. Use your
  judgement to determine if this is appropriate, or if separating changes
  will create more work for limited benefit.

### 3. Complete the Review
Once changes have been reviewed and comments left, a review must be completed.
A review can be completed to Approve, Request Changes, or Comment on the PR.

#### Approving Review
If there are no changes to be made and the PR is ready to be merged, leave an
approving review. This may be done with or without comment, but it is best
practice to leave a comment indicating what you did (i.e. tested changes,
reviewed code, reviewed tests, etc.) in case something comes up in the future
and the merged PR needs to be referenced.
> Note that an approving review may include comments that suggest a future
  enhancement or consideration to be moved to a new Issue. These should be
  addressed and resolved before merge, even though the PR is already approved.

#### Requesting Changes
If there are issues noted in the review that must be addressed before the PR
is ready to merge, leave a review requesting changes. It is helpful to highlight
in the review comment what comments in particular need to be addressed and if
any of the comments are optional suggestions or open for discussion.

#### Comment Only
If there is feedback that is not a specific request for changes and the PR is
not yet ready to be approved, a Comment review may be left. This will most 
often be done when a PR is still a Draft and comments don't necessarily relate
to a specific change. This kind of review can also be helpful when changes are
reviewed, but further testing will be done before approval.
> If the PR author is reviewing changes, they are only allowed to leave a 
  Comment, since it would not make sense to approve or request changes on their
  own PR. This can be a useful practice for reviewing changes before requesting
  reviews from others.

