---
state: defined
---

# OFK-002: Source Structure and OpenCode Primary Format

## Context

OpenFlowKit provides reusable agents, instructions, and workflow building blocks from a user-specified source. This source must have a traceable structure so installation, updates, and conversion can operate with functional clarity.

OpenCode is the primary format for agents in OpenFlowKit. Other target formats can be derived from this source, but they are not the leading representation in the source project.

## Assumptions

- None

## Open Questions

- None

## Requirements

### User-Defined Installation Source
**Type:** Functional  
**Description:** Installation and updates must obtain content from the source entered by the user.  
**Acceptance Criteria:**
- The user specifies the source from which agents, instructions, and workflow building blocks are read.
- Installation does not implicitly use a local working copy as a functional prerequisite.
- The source is used as the starting point for placement in the target project.
  **References:** OFK-001, installation concept

### OpenCode as Primary Format
**Type:** Constraint  
**Description:** OpenFlowKit must use OpenCode as the primary format for agents.  
**Acceptance Criteria:**
- Agents are maintained under the OpenCode-compliant `.opencode/agents/` structure.
- The earlier generic top-level `agents/` directory is not part of the target structure.
- Derived target formats may adapt the technical representation, but must originate from the OpenCode source.
  **References:** OFK-001

### Source Structure
**Type:** Constraint  
**Description:** The entered OpenFlowKit source must contain the expected project structure.  
**Acceptance Criteria:**
- Requirements are located under `docs/requirements/`.
- The synchronization instruction is located under `instructions/sync-agents.md`.
- OpenCode agent sources are grouped in direct category subdirectories under `.opencode/agents/`.
- Exported OpenCode target agents are written directly under `.opencode/agents/`.
- No project-specific OpenCode plugin dependencies are required.
- Agent sources are Markdown files with OpenCode frontmatter.
  **References:** Project structure of the entered source
