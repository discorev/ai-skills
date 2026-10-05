---
name: review-pr
description: Use when asked to review a pull request or leave review comments on one, including a GitHub or Azure DevOps PR link (github.com/.../pull/N, dev.azure.com/.../pullrequest/N, *.visualstudio.com/.../pullrequest/N), "review <link>", "review PR 123", "review this PR" or "review the PR on this branch".
compatibility: Requires gh for GitHub or the `ado` CLI (https://github.com/discorev/ado-helper, macOS) for Azure DevOps, and the responding-to-others skill for posting comments.
---

# Review PR

Review the requested changes and produce useful, actionable findings.

## 1. Decode the user's request

Identify the PR from the request:

| Input | Resolve it automatically |
| --- | --- |
| A PR URL | Read the provider, host, repository and PR identity from the URL, then retrieve the PR. The explicit URL takes precedence over the current checkout. |
| A PR number, such as `review PR 123` | Infer the provider and repository from the current workspace's Git remotes and conversation context. For ADO, also resolve the organisation and project. Retrieve and verify the matching PR. |
| `Review this PR` or `review the PR on this branch` | Inspect the current repository, branch and upstream remote, then find its open PR. Account for forks and multiple remotes; match the source repository as well as the branch name. |

Use the explicit repository or branch when one is named. Confirm the resolved PR with a brief linked progress update and begin; this is not an approval checkpoint. If discovery leaves multiple plausible PRs, ask one concise question with the candidates. If none exists, say so; do not create one. Ask for a repository or URL only when context cannot resolve the identity.

Establish the requested scope, delivery format and whether posting to the PR is authorised.

A request to review a PR authorises the review, not automatically publishing comments. When the user explicitly asks to leave comments, post within that scope without asking again. Posting comments does not itself authorise changing the review vote, resolving others' threads, modifying code or merging the PR.

## 2. Access GitHub or Azure DevOps

Use `gh` for GitHub and `ado` for Azure DevOps. `ado` is the local helper for ADO Git and PR management, used in place of `az` for this workflow. `az` is reserved for Azure cloud management, much like `aws` for AWS; leave its login and configuration alone.

Use the CLIs, checking access to the target repository and the identity that would author comments. When a CLI is unavailable or lacks a capability, such as ADO build logs and policy checks, use an authenticated connector or open the resolved PR URL in the signed-in browser UI. If access still requires login or permissions, ask only for that missing access and continue any review possible from available code.

- **GitHub:** `gh pr view` accepts a number, URL or branch and, in a repository with no argument, resolves the current branch's PR. Retrieve the PR metadata, diff and checks; defer reading review commentary until stage 4. See the [GitHub CLI manual](https://cli.github.com/manual/gh_pr_view).
- **Azure DevOps:** run `ado help` to discover its commands for accessing PRs, obtaining local source and posting inline comments. Defer reading threads until stage 4.

### Discover local ADO profiles

Run `ado auth status` to list locally configured profile names, organisations and identities, and match the organisation from the PR URL or repository remote. Treat `https://<org>.visualstudio.com` and `https://dev.azure.com/<org>` as the same organisation; a difference in URL format does not indicate missing or expired authentication. Use `ado auth status --check` when live verification is needed. Successful authentication does not prove permission to post comments; never post a test comment to check permissions.

A full PR URL, or a bare number in a local checkout with an ADO upstream, automatically selects the organisation. Otherwise supply `--profile NAME` from established context; ask only if it remains ambiguous.

If a sandboxed invocation reports `Could not secure the local profile store`, that is a local filesystem restriction, not an expired PAT: the helper secures `~/.config/ado` even during reads and needs native Keychain access. Use the environment's execution approval mechanism to retry the same command with the required access; do not reset credentials or change Azure accounts to work around the sandbox.

If a profile is missing or expired, relay the helper's `ado auth add NAME --org URL` or `ado auth update NAME` command for the user to run interactively; it opens the PAT settings, explains the scopes and stores the token in Keychain. Do not broaden token permissions or fall back to an unrelated Azure login.

### Prepare the review

Follow pagination and check for truncated diffs or omitted threads. Fetch source into an isolated checkout when needed for investigation or tests (`ado pr clone` for ADO); do not switch a working tree with unrelated work merely to review a linked PR. Do not claim a complete review when existing discussions or changes could not be read.

Confirm the target branch and current source commit, and review the actual PR changes rather than assuming the local checkout matches. Read applicable repository instructions and the PR description.

## 3. Perform an independent code review

Review the code within the requested scope and record your findings before reading existing PR commentary or reviews.

## 4. Enrich feedback with existing PR commentary and reviews

After the independent review, read existing PR commentary and reviews, including replies and resolution state, and compare them with your findings. Use them to enrich the review feedback: distinguish newly identified issues from those already raised, and include the reviewer and discussion link where useful.

Check existing findings against the current code before treating them as still applicable. Explain when an issue has been fixed, reintroduced, or appears mistaken. Existing coverage does not remove a relevant finding from the report to the user; deduplication applies only to comments posted to the PR.

## 5. Deliver as instructed

Deliver in the format and destination the user requested. For a report, include the findings enriched by existing discussions, relevant validation results and material limits. If there are no findings, say so. Apply the following commenting instructions only when posting to the PR was requested.

When comments were authorised, report what was actually posted and anything deliberately omitted or still blocked. Link to the PR or individual discussions where available. Do not claim actions or results that did not happen.

### When asked to comment: communication

Before drafting or posting PR comments, use the `responding-to-others` skill. If it is unavailable, tell the user and ask how to proceed before posting on their behalf.

Do not reply to an automated reviewer's thread, such as CodeRabbit's, to add context or results. That feeds back to the bot, not the team, and they already have the issue. If an automated finding misses part of a problem, post a new comment on the same code covering what it missed. Interact with reviewer bots only when the user specifically asks. If an existing finding appears wrong, explain that to the user.

### When asked to comment: draft and place

Post only new, actionable findings. Before posting, reconcile each finding against existing PR discussions. Do not duplicate an issue already covered by a human or automated reviewer, including confirming replies or restatements with a different severity or more reproduction detail. Check resolved or outdated threads against the current code; explain what changed if reporting a reintroduced defect. Keep each comment focused on one issue: explain the problem, a concrete trigger and consequence, and the necessary correction without prescribing unrelated implementation detail.

Attach code-specific findings to the smallest relevant line range in the PR diff. For a defect spanning files, anchor at the primary cause and link other relevant code. Use remote PR or repository links, not local filesystem links.

Apart from the short body a batched review requires, reserve top-level comments for an explicitly requested overall summary or a genuinely PR-wide concern without a meaningful code anchor. If a finding cannot be attached correctly, retain it in the report to the user and explain the limitation.

Where the provider supports it, batch inline findings into a single review so the author receives one notification. On GitHub, create one review with `event: COMMENT` against the reviewed commit, with each finding in `comments` carrying its file path, diff side and line/range. GitHub requires a review body for `COMMENT`; keep it to a short summary that points to the inline comments rather than repeating them. `APPROVE` and `REQUEST_CHANGES` are review votes and need separate authorisation. `gh pr comment` creates a general conversation comment and is not the inline route. See the [GitHub review API](https://docs.github.com/en/rest/pulls/reviews#create-a-review-for-a-pull-request). For ADO, use `ado help` for inline posting.

### When asked to comment: submit and verify

Before each submission, confirm the reviewed revision is current, the finding is still valid and not already covered by a newly added comment, and the draft preserves the intended wording, disclosure and user edits. Check that the destination is a new inline thread on the intended file and lines, not a reply to a bot.

After posting, verify the saved comment appears on the intended code. If submission is uncertain, inspect the thread before retrying to avoid duplicates.
