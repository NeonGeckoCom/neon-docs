## Library Module Alpha PR Review Workflow
This describes the process for reviewing a PR to a `library` module `dev` branch.

### Application
This process applies when reviewing a PR to the `dev` branch of a `library` module.

### Process
- Complete the following. Note any issues in Review Comments before leaving a 
  review to `Request Changes`. If there are no comments or the comments do not
  require code changes, then you *may* `Approve` the PR.
  - Review changes to imports and verify dependency minimum versions are accurate.
  - Ensure code changes have adequate unit test coverage.
  - Ensure there are no breaking changes.
  - If changes replace or could replace an existing method, ensure a deprecation 
    warning is logged in the replaced method (either `deprecated` decorator or
    `log_deprecation` call).
  - If tests are modified, make sure newly deprecated methods are still tested and
    that changes do not remove any test cases.
  - Highlight any added `TODO` or existing `TODO` comments related to the changes.
    >   It is up to the PR author and reviewer to decide if a `TODO` must be completed
      before merging OR if an issue can be created to address it in a later PR.

- Ensure the PR title and body are completed. Make any revisions necessary or
  work with the PR author to make those changes.
- Leave a review to `Approve` or `Request Changes`. If reviewing a draft or if 
  unable to test changes, leave a `Comment` review indicating why it isn't an 
  Approval, i.e. "Unable to test." or "Noted pending TODO"

### Exceptions
- There should be no exceptions to this review process.