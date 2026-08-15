# Sync OpenFlowKit Skills

Sync selected skills into a target project. Infer context when safe; ask only when a decision is ambiguous.

## Inputs

Infer or ask for `source_root`, `target_root`, `skill_selection` (names or relative paths), and `target_tool` (`opencode`, `claude-code`, explicitly requested experimental `github-copilot`, or `other`).

## Source and Selection

Resolve `source_root` to a local directory containing `.opencode/skills/`:

- Use an existing local path, or clone a repository URL outside `target_root`.
- Infer a repository only from a clear raw GitHub or GitLab URL; otherwise ask for a local path or repository URL.
- Raw mappings: `https://raw.githubusercontent.com/owner/repo/branch/path` -> `https://github.com/owner/repo.git`; `https://gitlab.example/group/project/-/raw/branch/path` -> `https://gitlab.example/group/project.git`.

Each selected direct child of `.opencode/skills/` must contain `SKILL.md` with a lowercase hyphen-separated `name` matching its directory and a non-empty `description`. Templates and supporting files must be inside the same skill directory and linked relatively. Ignore temporary files, backups, and nested skills. Match by directory name or relative path. When no selection is provided, show available skills and ask; never copy all skills implicitly. Process selections alphabetically.

## Target

Use `target_tool` when provided. Otherwise detect `.opencode/` as OpenCode and `.claude/skills/` or `.claudcode/skills/` as Claude Code. A `.github/` directory alone does not identify Copilot. Ask when multiple tools match or none match.

OpenCode:

- Create `<target_root>/.opencode/skills/` if needed.
- Copy each selected directory recursively and unchanged to `<target_root>/.opencode/skills/<skill-name>/`.

Claude Code:

- Create `<target_root>/.claude/skills/` if neither Claude Code skill directory exists.
- Copy each selected directory to `<target_root>/.claude/skills/<skill-name>/`, or to the existing `<target_root>/.claudcode/skills/<skill-name>/` legacy path.
- Follow local conventions; otherwise retain `SKILL.md` frontmatter, local templates, and referenced files.

GitHub Copilot is optional and experimental. Follow its existing format or ask for the target path and format. For other tools, ask for both before writing.

For conversions, preserve purpose, scope, safety rules, workflow, and template references. Adapt only the target-specific representation; do not add model pins or sampling parameters unless required.

## Conflicts and Validation

Stay inside `target_root`. A conflict exists when a target skill has the same directory name, frontmatter `name`, or purpose. Ask for exactly one action:

- `merge`: retain target-specific content and add compatible source content.
- `overwrite`: replace the target skill.
- `skip`: do not install it.

Merge semantically, preserve local rules and referenced templates, and mark unresolved decisions as `TODO: decision required`. Never overwrite without a decision or create commits unless requested.

After each write or merge, verify `SKILL.md`, matching `name`, non-empty `description`, and every local relative Markdown link. Templates must remain inside the skill directory. Before reporting, remove temporary clones and report cleanup failures. For OpenCode, tell the user to quit and restart OpenCode because skills load only at startup.

## Report

Report the detected tool and directory, installed, new, changed, and skipped skills, validation results, open decisions, and the OpenCode restart reminder when applicable. State why if nothing was written.
