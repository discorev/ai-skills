---
name: using-workflows
description: Use whenever you are about to call the Workflow tool, orchestrate multi-agent work, launch gpt models as subagents, or steer/resume/debug a running workflow. Also invoke when an unexpected agent appears mid-session or a workflow seems stuck. Invoking this skill on my request counts as my explicit opt-in to run a workflow.
---

# Using workflows

How I want workflows run. The model table and Main Loop rule live in the "My calibration of models" section of `~/.claude/CLAUDE.md` and still govern every model pick here — this skill is the operational layer on top of them.

## How to interpret the model table
The model table governs models for agents and workflows that you launch. It does NOT apply to model defaults baked into tools, scripts or dependencies in my projects: those pins are part of that code's calibration and should not be changed without my agreement. That includes upgrading model generation or effort. If a pin conflicts with the model table, surface it and ask before changing anything - including dependency bumps needed to make a newer model work.

For bulk/mechanical work (clear-spec implementation, data analysis, migrations) prefer gpt models - they are effectively free when compared to claude models. An approved design or plan makes the implementation clear-spec by definition — hand it over, then review the result inline (review IS expensive-loop work).

The table expresses defaults not limits. You have standing permission to override them: if a cheaper model's output doesn't meet the bar, rerun or redo the work with a smarter model without asking. Judge the output, not the price tag. Escalating costs less than shipping mediocre work. Overrides point toward escalating quality — never toward the expensive loop absorbing delegable work.

Scope the model to the work, e.g. high cost is justified when judgement and taste are critical. Take advantage of cheaper options to get more information and try things before moving the work to a more expensive option.

Never use Haiku.

## Opt-in and permissions

- You have my standing permission to use workflows to access gpt models.
- For larger workflows/higher investment, check with me (per alignment in `~/.claude/CLAUDE.md`) before launching it, unless it is already clear that is what I asked for. (e.g. If I've asked you to implement a complex, multi-step plan I've already approved investment based on the decisions in the plan).
- Automode may still block the `Workflow` call if I haven't allowed it this session. If blocked, ask me to allow it — as the only question in that turn.
- A harness constraint never justifies the wrong model. If the classifier or opt-in state blocks launching the right one (e.g. a gpt workflow), stop and ask — do not silently substitute whatever is launchable.

## Picking models inside workflows

- The `model` parameter takes both claude and gpt models. Don't route through a codex wrapper.
- I use short-names for gpt models: sol, luna and terra, the full model name for the `model` parameter is prefixed with `gpt-5.6-` e.g. `gpt-5.6-sol`
- ALWAYS pass `effort` explicitly — never rely on defaults.
- Reviews of plans/implementations: opus or fable, optionally sol @ high/xhigh as an extra independent perspective.
- Picking a gpt effort level: medium for clear-spec mechanical work, high as the default for implementation and review, xhigh when handing over a hard problem unsupervised (deep debugging, design with unknowns). Higher effort costs wall-clock time (roughly 1.5-3x per step up), so don't reach for xhigh on work medium handles.
- Label every agent with a `<model>-<effort>:` prefix, e.g. `{label: 'sol-high:review-auth'}`, so I can see at a glance who is running. Use short model names `fable`, `opus`, `sol` etc. over full model ids.

## Security related work and cyber refusals
If we are working in a security sensitive area it is likely we will touch on dual-use activities as part of verifying or hardening security. This can result in refusals or model switching. It's important to ensure that the requested model actually completed the work through checking transcripts. Fable is the most likely model to be refused and switch back to Opus-4.8.

I am verified in the Claude Cyber Verification Program and gpt Trusted Access for Cyber. As part of this Sol and Opus cyber refusals should be rare so prefer these models for adversarial / attack-simulation phases. If they occur, report them to me and request my preference on which model we use to proceed.

## The forking rule (the one that has burned us most)

- NEVER SendMessage a workflow-spawned agent — any model. It fails with "No transcript found" at best; historically it forked a competing editor with the workflow agent prompt based on the main thread model/effort. This agent then starts to overlap with the workflow agent and causes issues around delegation of responsibility.
- The rule must bind sub-agents too: end EVERY agent prompt inside a workflow script with — "Never use SendMessage or spawn agents that report back by message; return everything as your final output." A nested agent replying outward forks the workflow agent exactly the same way. I don't mind workflow agents spawning sub-agents, but they must pick an external communication channel (e.g. a temp file) rather than using `SendMessage`.
- If an unexplained agent appears ("resumed", "general-purpose", anything I didn't launch), it is ALWAYS yours — a fork — never me. The main thread is the only channel I use. Find it, stop it, and resolve the overlap without asking me whose it is. I will be explicit if I am launching another agent you cannot see to work within a directory I've given you authority over.

## Steering, liveness, and resuming

- Steer a running workflow by editing its script file and re-invoking with `{scriptPath, resumeFromRunId}` — completed agents replay from cache. Never steer by messaging agents.
- If you change models/reviewers mid-run, resume so completed steps stay cached — never relaunch from step 1 and re-run work that was already approved. Re-reviewing finished slices is wasted usage.
- Don't judge liveness by the token display or file mtimes — sol shows 0 tokens until it completes. That does NOT mean it is stuck or failed. Inspect the transcript JSONL under `subagents/workflows/<runId>/` instead.
- Monitor in the background; don't block the main thread polling.

## Git cadence around workflows

- Git is cheap; history brings recovery. Uncommitted work is lost work. Commit a checkpoint after each workflow/agent round completes, before launching the next. A large multi-round diff sitting uncommitted is a defect — flag it and checkpoint.

## Scope discipline for workflow launches

- A workflow launch is a "significant investment" under my alignment rules: confirm the scope and model mix with me before the first launch of a workstream, unless I've already approved the plan it implements.
- Keep review agents on-task: scope their prompts to the changed surface. Reviewers have previously drifted into node_modules and dead code — that wastes expensive tokens.
