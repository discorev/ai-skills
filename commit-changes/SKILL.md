---
name: commit-changes
description: Create git commits with conventional commit messages and a descriptive body. Use when the user asks to commit staged changes, commit current work, write a conventional commit subject/body, or derive a commit message from `git status` and `git diff`.
---

# Commit Changes

Review the repository state, derive the commit scope from the actual diff, and create a conventional commit with a useful body by default.

## Commit Workflow

1. Inspect `git status --short` before deciding what belongs in the commit.
2. Prefer staged changes when any files are already staged.
3. If nothing is staged, inspect the working tree and infer the intended commit scope from the user's request. Do not stage unrelated files.
4. Read the relevant diff before writing the message:
   - staged changes: `git diff --cached --stat` and `git diff --cached`
   - unstaged changes: `git diff --stat` and `git diff`
5. If the user asked to commit, perform the commit. If they asked only for wording, draft the message without running `git commit`.
6. Choose the conventional commit type from the change itself: `fix`, `feat`, `refactor`, `test`, `docs`, `chore`, and similar.
7. Add a scope only when one module, package, or feature area is clearly dominant.
8. Write the subject in imperative form and keep it specific.
9. Add a descriptive body unless the change is trivial and self-explanatory.
10. Use multiple `-m` flags for non-interactive commits.

## Message Rules

- Format the subject as `type(scope): summary`.
- Omit the scope when no single scope is defensible.
- Avoid vague subjects such as `update stuff` or `fix issues`.
- Summarize the behavior change, bug fix, or feature outcome instead of listing raw implementation detail.
- Use the body to explain what changed, why it changed, and any meaningful side effects or follow-up context.
- Keep the body concise but concrete.

## Safety Checks

- Do not silently include unrelated files in the commit.
- If the diff contains separate concerns, split the work into multiple commits or ask the user.
- Do not amend, rebase, or rewrite history unless the user asks.
- If there are no relevant changes, say so instead of creating an empty commit.
- If the repository has an existing commit convention beyond Conventional Commits, follow it.

## Branch Creation

- When creating a branch, preserve the intended branch name exactly, including slash prefixes such as `feat/`, `fix/`, or `chore/`.
- If branch creation fails in the sandbox, treat that as a sandbox or permission issue unless the git error explicitly says the branch name is invalid or already exists.
- Do not infer that a slash-prefixed branch name is unavailable merely because sandboxed branch creation failed.
- If escalation is needed after a sandbox failure, request escalation for the same branch name you originally intended to create.
- Do not retry with a degraded branch name, such as dropping `feat/`, unless the user explicitly requests that name change or git explicitly reports the original branch name is invalid.

## Example Requests

- `Commit this`
- `Use $commit-changes to commit my staged changes`
- `Write a conventional commit with a descriptive body for the current diff`
