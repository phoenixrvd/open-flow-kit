# OpenFlowKit

OpenFlowKit is a set of OpenCode agents and instructions for recurring workflow automation across multiple projects.

The target audience is solo developers and small teams that work across multiple projects and want to use similar agents, instructions, and workflows in those projects.

The problem: agent templates often need project-specific adjustments. This quickly creates different local versions that are hard to keep in sync. OpenFlowKit helps maintain those agents centrally as a starting point and selectively adopt them into local projects.

The agents are functional, but intentionally workflow-specific. They are not a universal standard solution, but templates that should be reviewed and adapted per project.

## Contents

- `.opencode/agents/<group>/`: agents for code, reviews, documentation, and releases, grouped by category
- `instructions/sync-agents.md`: instructions for syncing agents into a target project
- `docs/requirements/`: project requirements and conceptual notes

## Usage

Agents are installed locally in each project. This allows them to be versioned in the target project and adapted to its rules.

With OpenCode, synchronization can be started directly through the existing instruction. OpenCode should first read `instructions/sync-agents.md` and then execute the steps described there.

Example prompt for OpenCode:

```text
Read https://raw.githubusercontent.com/phoenixrvd/open-flow-kit/main/instructions/sync-agents.md and then run the synchronization.

Infer source, target project, target tool, and agent selection automatically from the project context.
Ask only when a decision is ambiguous.
```

## Note

LLM agents are not deterministic. Review new or changed agents like regular project code before using them in production.
