# Skill metadata

Custom frontmatter fields used by skills in this repo. They live under `metadata` so the top level stays within the Agent Skills spec and validators don't reject them. Any new field gets documented here before a skill uses it.

## `harness`
The harnesses a skill works in. Leave it out for universal skills, which is the default.

- **Type:** list of `skills` CLI agent ids, e.g. `claude-code`, `codex`
- **Default:** unset, meaning universal
- **Set it when:** the skill depends on a harness feature rather than a model capability, or I ask for it
- **Effect:** setup installs the skill only into the listed harnesses, never into `~/.agents/skills`

```yaml
metadata:
  harness: [claude-code]
```

### Usage

When installing a skill, if a list of harnesses is set then install with `npx skills add discorev/ai-skills -g -y -a <harness> [<harness>...] -s <skill>` to ensure that the skill is only added for the listed harnesses. If the skill is already in `~/.agents/skills` (e.g. it was universal before `harness` was set), remove that copy first with `npx skills remove -g -y <skill>`. `npx skills update` does not track these installs, so refresh them by re-running the `add`.