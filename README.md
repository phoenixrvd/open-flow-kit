# OpenFlowKit

OpenFlowKit is a set of OpenCode skills and instructions for recurring workflow automation across multiple projects.

The target audience is solo developers and small teams that work across multiple projects and want to use similar skills, instructions, and workflows in those projects.

The problem: skill templates often need project-specific adjustments. This quickly creates different local versions that are hard to keep in sync. OpenFlowKit helps maintain those skills centrally as a starting point and selectively adopt them into local projects.

The skills are functional, but intentionally workflow-specific. They are not a universal standard solution, but templates that should be reviewed and adapted per project.

## Contents

- `.opencode/skills/<skill-name>/`: skills for code, reviews, documentation, and releases
- `instructions/sync-skills.md`: instructions for syncing skills into a target project
- `instructions/migrate-agents.md`: instructions for migrating project-local OpenCode agents to skills
- `docs/requirements/`: project requirements and conceptual notes

## Usage

Skills are installed locally in each project. This allows them to be versioned in the target project and adapted to its rules.

OpenFlowKit is not an installer, CLI, runtime system, security framework, or central manager for third-party projects. Installation and updates are performed by the LLM agent that reads and follows the provided instructions.

## Sync Skills

To install skills into a target project, read and execute `instructions/sync-skills.md`.

Example prompt for OpenCode:

```text
Read https://raw.githubusercontent.com/phoenixrvd/open-flow-kit/main/instructions/sync-skills.md and then run the synchronization.

Infer source, target project, target tool, and skill selection automatically from the project context.
Ask only when a decision is ambiguous.
```

After an OpenCode skill is installed or updated, quit and restart OpenCode so it loads the changed skill files.

## Migrate Project Agents

To convert existing project-local OpenCode agents into skills, read and execute `instructions/migrate-agents.md`. The migration preserves agent content semantically, validates each destination skill, then deletes only successfully migrated agent sources.

Example prompt for OpenCode:

```text
Read https://raw.githubusercontent.com/phoenixrvd/open-flow-kit/main/instructions/migrate-agents.md and migrate this project's OpenCode agents to skills.
```

## Note

LLM agents are not deterministic. Review new or changed skills like regular project code before using them in production.
