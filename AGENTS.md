# AGENTS.md

## Purpose

OpenFlowKit contains reusable OpenCode skills and instructions for recurring workflow automation.
It is not an installer, CLI, runtime system, security framework, or central manager for third-party projects.

## Structure

- `.opencode/skills/<skill-name>/`: Source templates for OpenCode skills.
- `instructions/`: Reusable procedural instructions, including `sync-skills.md` and `migrate-agents.md`.
- `docs/guidelines/`: Project workflow rules that skills must consult before acting.
- `docs/requirements/`: Requirements and conceptual notes.
- `README.md`: Project overview and usage examples.

## Tooling

No project-level build, test, lint, or format commands are currently defined. Do not invent commands.

## Guidance

- Keep documentation concise and explicit.
- Create all project content exclusively in English, including comments, documentation, requirements, instructions, skill text, and examples.
- When skills formulate text, keep it as precise and low-verbosity as possible.
- Do not edit `.opencode/node_modules/` manually.
- When modifying skills, preserve trigger phrases and scope unless asked otherwise.

## Git Workflow

- Follow `docs/guidelines/git-workflow.md` for release branch, target branch, and finish rules.
- Release finish rules are defined in `.opencode/skills/release-finish/SKILL.md`.
- Do not push, tag, or create GitHub releases unless the user explicitly requests it.
