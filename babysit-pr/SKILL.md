---
name: babysit-pr
description: babysit, babysit the PR, watch the PR, monitor the PR, wait for CI, CI failed, greptile, review bot, bot review comments, fix review findings, get the PR ready to merge, merge when green
license: MIT. Adapted from t3dotgg/tstack; LICENSE.txt has the terms.
---

# Babysit PR

Take a PR through CI and its review bots to ready-to-merge. Fix what is real, answer what isn't, and report its state honestly.

## Ownership

Only the agent that owns the PR watches or merges it. Subagents and workflow agents never do either; they return their results to the agent that launched them. Watch only PRs you are working on.

## Waiting

Never write your own polling: no `sleep` loops and no foreground `gh pr checks --watch`. Use one watcher per PR.

- **In T3 Code:** handle the existing checks and comments first, then call `watch_pull_request` and end your turn. T3 wakes you when a check finishes, someone comments or reviews, or the branch conflicts. Call `unwatch_pull_request` before handing back to the user (ready, blocked or asking a question). A merge or close ends the watch on its own.
- **Elsewhere:** run `gh pr checks <pr> --watch --fail-fast` as a background task and end your turn. It only tracks checks, so when it finishes, read new reviews and comments yourself.

A wake is news, not a merge decision; check the gate yourself. When a wake needs no action, end your turn without a message. Never post or say "still waiting".

"Stopped watching, could not read it" usually means the GitHub GraphQL rate limit is spent. Check `gh api rate_limit --jq .resources.graphql`, and if `remaining` is near 0, wait for `reset` with one background command before watching again. Never re-watch in a tight loop.

## GitHub rate limits

`gh pr create`, `gh pr view` and `gh pr checks` use GraphQL. The REST limit is separate and usually still has room: read a PR with `gh api repos/<owner>/<repo>/pulls/<n>` and its checks with `gh api repos/<owner>/<repo>/commits/<sha>/check-runs`.

## Reviews

- Act only on checks and comments newer than the latest push.
- Verify every bot finding against the source before changing code. Fix real findings and CI failures, and tell repository failures from infrastructure flakes.
- A finding the bot marks as blocking (P1, "not safe to merge", changes requested) blocks the merge until it is fixed or the user accepts it. Never downgrade one yourself.
- When a finding isn't worth fixing, reply with the reason and resolve the thread. Use the `responding-to-others` skill for every comment or reply, and resolve a thread only after the reply's URL comes back.
- If the repo has rule files for a review bot, read them before replying to that bot.

## Keep the PR small

Review loops grow a PR one cheap-looking fix at a time. Fix real bugs in the intended case. Decline the rest with a written reason and list them as follow-ups: unneeded tests, guards for edge cases that don't matter, extra compatibility and small performance costs. Prefer the fix that deletes or narrows code.

## Merge gate

The PR is ready when all of these hold on the latest head:

- Required checks are green.
- The review bots the repo uses have reviewed this head, and every finding is fixed or answered with a reason.
- No blocking finding is open, and the bot's score meets any threshold in the repo's instructions, such as a minimum Greptile confidence.
- No human review requests changes.
- The branch is up to date with its base and GitHub reports it mergeable. `watch_pull_request` reports conflicts but not BEHIND, so check that yourself.

## Reporting

Every status update and the final report gives the bot's latest score or verdict and the number of open findings, saying which block the merge. Never call the PR resolved, ready or done while a blocking finding or failing check remains; say what is left instead.

## Base branch and merging

- Rebase when new base commits overlap this PR. If an overlapping PR makes this one obsolete, stop monitoring, report it and ask before closing.
- Merge only when the user explicitly asked. Otherwise report that the PR is ready, with its link.
- Right before merging, `git fetch` and check again. Never merge when the PR is BEHIND or DIRTY, or when base commits that landed after the last green run touch its files: rebase, push and wait for green again.
- Merge with the repo's usual method. Never use `--admin` or another bypass.
