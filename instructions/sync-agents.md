# Sync OpenFlowKit Agents

Sync selected OpenFlowKit agents into a target project. Work deterministically and ask only when no safe single choice exists.

These instructions are executed by the LLM agent. OpenFlowKit itself is not an installer, CLI, runtime system, security framework, or central manager for third-party projects.

## Inputs

Infer these from context when possible; otherwise ask:

- `source_root`: local OpenFlowKit path, Git repository URL, or raw instruction URL.
- `target_root`: target project root.
- `agent_selection`: agent names, relative paths, or groups, for example `reviewer`, `doc/adr-writer`, `code`, `doc`, `release`.
- `target_tool`: `opencode`, `claude-code`, optional experimental `github-copilot`, or `other`.

## Resolve Source

Resolve `source_root` to a local directory containing `.opencode/agents/`.

- Local path: use it if `.opencode/agents/` exists; otherwise ask.
- Git repository URL: clone it into a temporary directory outside `target_root` and use that checkout.
- Raw instruction URL: infer the repository URL only from clear platform patterns, then clone it.

Known raw URL mappings:

- GitHub: `https://raw.githubusercontent.com/owner/repo/branch/path` -> `https://github.com/owner/repo.git`
- GitLab: `https://gitlab.example/group/project/-/raw/branch/path` -> `https://gitlab.example/group/project.git`

If source resolution or cloning fails, ask for a local `source_root` or explicit repository URL.

## Select Agents

Source agents are Markdown files in direct group subdirectories of:

```text
<source_root>/.opencode/agents/
```

Ignore temporary files, backups, and deeper nested directories.

If `agent_selection` is provided, match entries against:

- filename without extension
- relative path without extension, for example `doc/adr-writer`
- direct group directory name

If no selection is provided, ask and offer detected groups and agents. Never copy all agents without explicit selection. Process selected agents alphabetically by relative path.

## Detect Target Tool

Use `target_tool` when provided. Otherwise inspect `target_root`:

- `.opencode/` -> OpenCode
- `.claude/agents/` -> Claude Code
- `.claudcode/agents/` -> Claude Code legacy
- `.github/` -> GitHub Copilot only when explicitly requested; this target is optional, experimental, and outside the initial scope

Ask when multiple tools match or none match.

## Target Formats

OpenCode:

- Target: `<target_root>/.opencode/agents/`
- Copy selected files unchanged.
- Do not preserve source group directories.

Claude Code:

- Target: `<target_root>/.claude/agents/`, or existing `<target_root>/.claudcode/agents/` for legacy projects.
- Convert to the existing local Claude agent format when examples exist.
- If no examples exist, use Markdown with YAML frontmatter and transfer name, purpose, scope, limits, allowed tools, and work instructions.

GitHub Copilot:

GitHub Copilot target support is optional, experimental, and outside the initial OpenFlowKit scope. Do not present Copilot-specific conversion as a required part of installation.

- Use the existing Copilot structure under `<target_root>/.github/`.
- Convert to the existing local Copilot format when examples exist.
- Ask for target path or format when unclear.

Other:

- Ask for target path and format before writing.

## Conflicts And Writing

Before writing, create missing target directories and stay inside `target_root`.
Do not manage the target project beyond the selected agent files and directories.

A conflict exists when a target file has the same filename, same agent name, or clearly the same purpose. For each conflict, ask for exactly one action:

- `merge`: keep the target as base and add compatible OpenFlowKit content.
- `overwrite`: replace with the OpenFlowKit file or converted version.
- `skip`: do not install the agent.

Merge semantically. Keep local target rules, tooling assumptions, and project conventions. Mark unresolved merge decisions with `TODO: decision required`.

Do not overwrite without a conflict decision. Do not create Git commits unless explicitly requested.

## Final Message

Report briefly:

- detected target tool
- target directory
- installed agents
- new files
- changed files
- skipped agents
- open decisions or review notes

If nothing was written, state why.
