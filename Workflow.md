This document explains the workflow of developing the API, how it's tracked, and what stages a change needs to go through before it can be considered done.

We use a [Github project](https://github.com/orgs/gpuweb/projects/1) for tracking the status of features and fixes. Every non-trivial change should have a single tracking issue in the [gpuweb](https://github.com/gpuweb/gpuweb) repository, which can link to other issues and PRs. Tracking has to go through the following steps:

  1. _Design_ - The change is on our radar. If work has been started, the tracking issue gets assigned to a "champion", who will drive the discussion. We may be investigating the differences between the native APIs, evaluating the portability and security constraints as well as the value of a change. An [investigation](https://github.com/gpuweb/gpuweb/labels/investigation)-labelled issue can be linked with the main one.
  2. _Specification_ - We have accepted one of the proposals and have a rough idea of how the change would be specified and tested. When an issue enters this stage, the assignee is potentially cleared, unless the same person wants to drive it through. Once it's picked up, the assignee works on formally describing the change in the specification. A [gpuweb](https://github.com/gpuweb/gpuweb) project PR may be linked with the main issue.
  6. _Testing_ - We have landed the specification changes, and the assignee gets cleared again. Once it's picked up, the new assignee works on adding tests to support the specified change. A [cts](https://github.com/gpuweb/cts) project PR may be linked with the main issue at this stage.
  7. _Done_ - A change has been either rejected, or fully implemented in the specification and the test suite.

## Linking PRs to the project

Pull requests shouldn't be added as cards to the project. Instead, they should link back to gpuweb issues which are tracked in the project. **Note:** Don't use "closes #xxx" or "fixes #xxx" in the PR if "#xxx" is a tracking issue. Instead, move the tracking issue manually between columns of the tracker. (This may be automated in the future.)

## Reading the [project tracker](https://github.com/orgs/gpuweb/projects/1)

A cursory look at the project can give us a rough idea on what the issues are currently in the works, and which ones need attention.

On each card on the tracker, an icon appears on the bottom-right of the card if it is assigned. If no icon appears, the task (of completing the current column for this card) is currently unassigned. These issues are up for grabs.

In order to get a list of all project issues that are waiting to be picked up, use [this issue query](https://github.com/gpuweb/gpuweb/issues?q=is%3Aopen+is%3Aissue+project%3Agpuweb%2F1+no%3Aassignee). It shows all unassigned tracking issues (regardless of column).