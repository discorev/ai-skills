---
name: writing-agent-instructions
description: AGENTS.md, CLAUDE.md, agent instructions, project instructions, repo guidelines, write an AGENTS.md, update AGENTS.md, add to CLAUDE.md, add a rule, fork notes, /init, memory or CLAUDE.md
---

# Instructing agents

Modern agents are trained for human input. They do not need overly prescriptive prompts and context usage has a real cost in attention. High quality steering is important, growing rule dumps, repetition and things that can be worked out do not belong in them. A bad rule carries into every single session and wasted tokens impact every task.

## Ownership

In general, AGENTS.md and CLAUDE.md files are owned by the user. Do not create or alter one without their approval. A new project doesn't need one until it's clear what belongs in it for that project. When something feels like it should be added, suggest it, and propose the edit. Let the user decide whether they agree; they will likely have opinions.

Always show the user the exact change you're suggesting and align on it first. If the user says they've made a change in response, re-read the file and give feedback without changing it. Never restructure, condense or otherwise alter the file without the user's explicit approval.

## Content

AGENTS.md files should never repeat what can be easily worked out by reading the code or through other context. When reviewing issues, always prefer to fix the codebase first, then add linting rules or static validation, and write rules as prose only as a last resort.

What an agent can't work out by reading the code:

- Why the code is the way it is: constraints, past incidents, decisions and the alternatives that were rejected.
- Stable rules that aren't visible in the code: merge gates, release steps, what needs approval, where the project tracks its work (its Jira, Azure DevOps or GitHub Issues backlog), and long-standing ownership of an area. Anything that changes from week to week, such as who is working on what, doesn't belong.
- Commands and checks that are non-obvious or differ from the defaults.

BAD: an agent can find this by reading the code

> ❌ Config writes: `internal/config/writer.go`

BAD: the code should enforce this; make the atomic writer the only way to write config, or add a lint rule

> ❌ Never write the config in place; always call `writeConfigAtomic()`.

GOOD: nothing in the code can say this

> ✅ Upstream changes are merged, never rebased, because contributors base their PRs on our main and rewriting its history breaks every open PR.

## What doesn't belong

- Anything that changes often: project status, progress, TODOs, task tracking or who is working on what. It goes stale with the next commit. Track it where the project's instructions say work is tracked. If they don't say, use a local TODO.md listed in the repo's local exclude file (`git rev-parse --git-path info/exclude`), not `.gitignore`.
- Generic advice any competent agent already follows, such as "write tests".
- Instructions for one person or machine (local paths, ports, credentials, personal workflow) in a committed file. Put them in the user's own instructions or an untracked local file, and ask if it isn't clear which.
- README content. A README is for people using the project; agent instructions are for agents working on it. Don't duplicate between them.

## Where it goes

- User-level instructions hold preferences that apply in every repo.
- A project file holds that project's rules. If a rule only applies to one project, it goes there, not in the user-level file.
- Standing rules don't belong in memories.

## Style

- One rule per line where possible, with its reason.
- Be concrete: name the command, file or threshold.
- Match the file's existing headings and voice. Add to an existing section before creating a new one.
- After editing, re-read the whole file and remove anything your change made redundant or contradictory.
