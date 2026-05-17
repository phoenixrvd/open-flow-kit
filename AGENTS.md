# AGENTS.md

## Purpose

OpenFlowKit contains reusable OpenCode agents and instructions for recurring workflow automation.
It is not an installer, CLI, runtime system, security framework, or central manager for third-party projects.

## Structure

- `.opencode/agents/<group>/`: Source templates for OpenCode agents, grouped by category.
- `instructions/`: Reusable procedural instructions, currently `sync-agents.md`.
- `docs/guidelines/`: Project workflow rules that agents must consult before acting.
- `docs/requirements/`: Requirements and conceptual notes.
- `README.md`: Project overview and usage examples.

## Tooling

No project-level build, test, lint, or format commands are currently defined. Do not invent commands.

## Guidance

- Keep documentation concise and explicit.
- Create all project content exclusively in English, including comments, documentation, requirements, instructions, agent text, and examples.
- When agents formulate text, keep it as precise and low-verbosity as possible.
- Do not edit `.opencode/node_modules/` manually.
- When modifying agents, preserve trigger phrases and scope unless asked otherwise.

## Git Workflow

- Follow `docs/guidelines/git-workflow.md` for release branch, target branch, and finish rules.
- Release finish rules are defined in `.opencode/agents/release/finisher.md`.
- Do not push, tag, or create GitHub releases unless the user explicitly requests it.
