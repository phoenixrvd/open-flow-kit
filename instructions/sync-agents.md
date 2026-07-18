# Sync OpenFlowKit Agents

Sync selected OpenFlowKit agents into a target project. Infer context when safe; ask only for ambiguous decisions.

## Inputs

Infer or ask for:

- `source_root`: local OpenFlowKit path, Git repository URL, or raw instruction URL.
- `target_root`: target project root.
- `agent_selection`: agent names, relative paths, or groups such as `code` or `release`.
- `target_tool`: `opencode`, `claude-code`, explicitly requested experimental `github-copilot`, or `other`.

## Source

Resolve `source_root` to a local directory containing `.opencode/agents/`.

- Use a local path only when that directory exists.
- Clone a repository URL into a temporary directory outside `target_root`.
- For a raw GitHub or GitLab URL, infer the repository only from a clear platform pattern.
- If resolution fails, ask for a local path or explicit repository URL.

Raw URL mappings:

- `https://raw.githubusercontent.com/owner/repo/branch/path` -> `https://github.com/owner/repo.git`
- `https://gitlab.example/group/project/-/raw/branch/path` -> `https://gitlab.example/group/project.git`

## Selection

Source agents are Markdown files in direct group subdirectories of:

```text
<source_root>/.opencode/agents/
```

Ignore temporary files, backups, and deeper nested directories. Match selection against the filename, relative path, or direct group name. If no selection is provided, show detected groups and ask; never copy all agents implicitly. Process selected agents alphabetically by relative path.

## Target

Use `target_tool` when provided. Otherwise inspect `target_root`:

- `.opencode/` -> OpenCode
- `.claude/agents/` or `.claudcode/agents/` -> Claude Code
- `.github/` alone is not enough to identify Copilot; require an explicit request or clear Copilot agent files

Ask when multiple tools match or none match.

OpenCode:

- Write to `<target_root>/.opencode/agents/`.
- Copy selected files unchanged and do not preserve source group directories.
- Before writing, detect duplicate destination filenames among selected agents. Stop and ask which agent or destination name to use for each collision.

Claude Code:

- Write to `.claude/agents/`, or to an existing `.claudcode/agents/` legacy directory.
- Follow existing local examples; otherwise use Markdown with frontmatter.
- Transfer purpose, scope, limits, allowed tools, safety rules, and workflow.

GitHub Copilot (explicit request only):

- This is optional and experimental, not part of the required installation path.
- Follow the existing `.github/` format or ask for the target path and format.

Other tools:

- Ask for the target path and format before writing.

For every conversion, preserve purpose, scope, safety rules, workflow, and permission intent. Adapt only the technical representation supported by the target tool. Do not add model pins or sampling parameters unless explicitly required by that format.

## Conflicts

Before writing, create missing target directories and stay inside `target_root`. Do not manage the target project beyond selected agent files and directories.

A conflict exists when a target file has the same filename, agent name, or purpose. Ask for exactly one action:

- `merge`: keep target-specific content and add compatible OpenFlowKit content.
- `overwrite`: replace it with the source or converted agent.
- `skip`: do not install it.

Merge semantically, keep local project rules, and mark unresolved decisions as `TODO: decision required`. Never overwrite without a decision. Do not create commits unless explicitly requested.

Before the final report, remove any temporary source clone created by this workflow. Report cleanup failures.

## Report

Report the detected target tool, target directory, installed agents, new files, changed files, skipped agents, and open decisions or review notes. If nothing was written, state why.
