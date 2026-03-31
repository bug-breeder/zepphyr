---
name: review
description: Reviews a ZeppOS pull request using the zeppos-reviewer agent. Only activate when explicitly invoked.
argument-hint: [PR#]
disable-model-invocation: true
---

Review the pull request: $ARGUMENTS

Use the `zepphyr:zeppos-reviewer` agent to perform this review.
If `$ARGUMENTS` contains a PR number, pass it to the agent.
If `$ARGUMENTS` is empty, review the current branch's open PR.
