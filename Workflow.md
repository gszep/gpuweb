## Workflow

This document explains the workflow of developing the API, how it's tracked, and what stages a feature needs to go through before it can be considered done.

We use [Github project](https://github.com/orgs/gpuweb/projects/1) for tracking the status of features and fixes. Every non-trivial change should have a single tracking issue in the "gpuweb" repository, which can link to other issues and PRs. Tracking has to go through the following steps:
  1. _Not started_ - the change is on our radar. We may not even have an issue created at this point.
  2. _In discussion_ - we are investigating the differences between the native APIs, evaluating the portability and security constraints as well as the value of a change. An "investigation"-labelled issue can be linked with the main one.
  4. _Needs specification_ - we have accepted one of the proposals and have a rough idea on how this would be specified and tested.
  5. _In specification_ - there is ongoing work to formally describe the change in the specification. A "gpuweb" project PR may be linked with the main issue.
  6. _In testing_ - there is ongoing work to add tests supporting the specified change. A "cts" project PR may be linked with the main issue at this stage.
  7. _Done_ - a change has been either rejected, or fully implemented in the specification and the test suite.

The main tracking issue needs to be manually advanced through tabs as we progress. No "closes Xxx" or "fixes Xxx" is expected to be seen in the PR description, if "Xxx" is a main tracking issue for a change. This may be automated in the future.