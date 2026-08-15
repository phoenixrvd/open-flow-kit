# Migrate Project Agents to Skills

Convert only the current project's OpenCode agents into OpenCode skills. OpenFlowKit provides this instruction only; never migrate agents from the OpenFlowKit source. Preserve purpose, trigger phrases, rules, workflow, and local templates. Delete each source only after its destination passes validation.

## Scope

Use the directory from which the migration was requested as `target_root`. Migrate only agents stored inside `target_root`:

- Markdown files in `.opencode/agent/` and `.opencode/agents/`, including grouped subdirectories.
- Inline `agent` entries with a `prompt` in `opencode.json`, `opencode.jsonc`, or `.opencode/opencode.json`.

Ignore disabled entries and built-in-agent overrides without a `prompt`; report them as skipped. Do not read or migrate agents from OpenFlowKit, global configuration, or directories outside `target_root`. Do not modify `.opencode/node_modules/`.

## Discovery

1. Read project instructions and inspect all source locations before editing.
2. List every candidate with its source, description, and intended skill name.
3. Derive file-agent names by joining relative path components with hyphens: `<group>/<agent>.md` becomes `<group>-<agent>`; a direct `<agent>.md` keeps `<agent>`. Use the inline entry key for inline agents.
4. Require lowercase hyphen-separated target names. Stop and ask when a derived name is invalid or two sources derive the same name.

## Conversion

For each agent, create `<target_root>/.opencode/skills/<skill-name>/SKILL.md`:

- Use frontmatter with the derived `name` and the source description. When it is absent, derive a concise description only from the source content. Preserve trigger phrases.
- Transfer the agent body semantically without dropping rules, safety limits, workflow, output requirements, or template references.
- Remove agent-only frontmatter such as `mode`, `permission`, model settings, and sampling settings.
- Translate restrictive permission intent into explicit instructions when it is not already present. A skill cannot enforce tool permissions; its caller's permissions apply.
- Copy each referenced local template or supporting file into the skill directory and update the skill to use a relative link.
- A reference such as `docs/requirements/TEMPLATE.md` becomes `<skill-name>/TEMPLATE.md` and `Use [TEMPLATE.md](TEMPLATE.md)`.
- Do not add model pins, permissions, or new behavior.

For inline agents, use `prompt` as the source body. Keep unrelated configuration fields unchanged.

## Conflicts

Before writing, check for an existing target skill with the same directory name, frontmatter `name`, or purpose. Ask for exactly one action:

- `merge`: retain target-specific content and add compatible agent content.
- `overwrite`: replace the target skill.
- `skip`: leave the agent unchanged.

Merge semantically and preserve project-specific rules. Mark unresolved choices as `TODO: decision required`. Never overwrite without a decision.

## Validation and Cleanup

After each converted skill, verify:

- `SKILL.md` exists and has a matching `name` and non-empty `description`.
- Every local relative Markdown link resolves.
- The source purpose, rules, workflow, and template content are represented in the skill.
- The skill has no dependency on templates outside its own directory.

Only after validation succeeds:

- Delete the migrated Markdown agent file.
- For an inline agent, remove only its migrated `agent` entry and keep the remaining configuration valid.
- Delete a former document-directory template only when every reference was migrated and no project file still uses it.
- Remove empty agent directories.

Keep every skipped, invalid, or failed source unchanged. Do not create commits unless explicitly requested. Tell the user to quit and restart OpenCode after the migration because skills load only at startup.

## Report

Report migrated skills, deleted sources, skipped sources and reasons, conflicts, validation results, inline configuration changes, and the restart reminder.
