---
name: review-loop
description: review loop, sol loop, council, independent review, adversarial review, second opinion, cross-model review, review with another model, review until approved, fresh review, review before merge
license: MIT. Adapted from t3dotgg/tstack; LICENSE.txt has the terms.
---

# Review loop

Get an independent model to review a change, fix what is real, and repeat until the reviewer approves or the budget runs out. Make the smallest changes that satisfy the review. Fight scope creep, and push back when a finding is wrong.

## Reviewers

- Use the reviewers the user names. Otherwise use the review defaults in the user's instructions. Failing that, use a strong model from a different provider than the one that wrote the change if possible.
- For several reviewers (a council), run them in parallel each round on the same brief and merge their findings into one ledger, prefixing ids per reviewer (A1, S1).
- Launch reviewers with the harness's native Agent or Workflow tools, following the `using-workflows` skill where it is installed. If a named reviewer can't be launched, report the blocker; never substitute another model.
- Reviewers are read-only: no edits, commits, pushes, PR comments or subagents.

## Budget

- 3 rounds by default. Round 1 is a full review; later rounds check only changed code and open findings.
- Extend to 5 only for security, auth, data loss or concurrency while a blocker or should stays open with evidence, or when the user asks.
- Stop early when a round returns only nits. Declined nits and comment, doc or format-only fixes don't need a new round.

## 1. Scope

- Run `git status --short`. Don't stash, discard or include unrelated changes. If the change's paths don't separate cleanly from other work, stop and ask.
- Run `git fetch origin`, then find the base. For a PR, use `git merge-base HEAD origin/<baseRefName>`. Otherwise use `git merge-base HEAD origin/<default-branch>`. With no remote, use a base the user names, or `HEAD` for uncommitted changes only.
- The change is base to working tree in those paths, including untracked files. Use the same diff command every round.
- State the intent and what is out of scope in one or two sentences. Run the checks that cover the change and don't send code that breaks them. Note missing or already-failing checks in the brief.

## 2. Brief

Each round starts a fresh reviewer with no history. Never message or resume an earlier one. Don't edit the scoped files while a review runs, and wait for its completion notice rather than polling.

```
Review only. Do not edit, commit, push, comment on PRs, or start subagents.
Repo: <abs path>  Base: <sha>
Intent: <intent>  Out of scope: <scope>  Checks: <results>
Change: git diff <base> -- <paths>; git status --short (read untracked files).
Review only this change. Read surrounding code only to judge it; do not review the rest of the project.
Find bugs, broken callers and mismatches with the intent. Skip style unless it hides a bug.
Hold to the smallest change: flag code, tests or guards the intent doesn't need.
Severity: blocker = the intended case is wrong. should = a real bug callers will hit, within the intent. nit = everything else (other paths, rare cases, small perf, extra compat).
Reply:
VERDICT: APPROVE or CHANGES
<id> [blocker|should|nit] <file>:<line> <claim>. Fix: <suggestion>
```

From round 2, append the ledger and the change since the last round. Ask the reviewer to mark each open id `resolved` or `still open: <reason>`, and to review only the changed code.

## 3. Answer findings

Check each finding and its severity yourself. Another path, a rare case or a small cost is a nit, whatever it was labelled.

- **Agree:** make the smallest fix. Prefer deleting or narrowing code over a new lock, cache, helper or option. Add one focused test that fails without the fix, only if the intent needs it.
- **Disagree:** say why, with evidence (file:line, a test, output).
- **Out of scope or nit:** decline in one line and list it as a follow-up.

Keep a ledger of id, severity and outcome (fixed, disagreed or declined) with evidence. Re-run the checks and `git diff --shortstat <base>`. If review grew the diff, cut it back before the next round. Don't agree just to end the loop.

## 4. Stop

- **Agreed:** every reviewer approves the current diff, no blocker or should is open, and you have no concern of your own.
- **Stuck:** an item stays disputed after two replies, or the budget runs out. Show both sides in two or three lines each and let the user decide. Running out of budget is not approval.
- A timeout, error or empty reply is not approval either.

Don't push during the loop. Push afterwards only if the user asked to push or ship.

## Report

Give the rounds, reviewers (model and effort), verdict, what changed because of review, diff size before and after, and fixed and declined findings with reasons. If the work is in a PR, add one line to its description:

`Reviewed with review-loop: <N> rounds, <reviewers>, <approved | stopped: reason>`
