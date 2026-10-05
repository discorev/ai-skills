# AI Skills

This is a public repo of AI skills that I use and share.

## Writing a new skill

When writing a new skill, ensure that it is written appropriately to be commited and made public. It should be general (`the user`) unless I explicitly say otherwise. Paths should only be included when generic and it should be assumed that installed skills do not have access to the repo so should not have relative paths etc. Prefer "use the `skill-name` skill" to "use the [skill-name](../skill-name/SKILLmd)".

### Skill metadata

This repo uses custom frontmatter fields. These are documented in `docs/metadata.md`. Include any appropriate for the new skill being written.

## Setting up or refreshing my environment
Install every skill globally with the `skills` CLI, from GitHub rather than a local checkout. Check each skill's metadata and follow the usage in `docs/metadata.md` for any fields it sets. Otherwise install with:

`npx skills add discorev/ai-skills -g -y -s <skill> [<skill>...]`

Finish by checking `npx skills list -g` and fixing anything that ended up in the wrong place.
