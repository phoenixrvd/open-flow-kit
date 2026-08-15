---
state: defined
---

# OFK-002: Source Structure and OpenCode Primary Skill Format

## Context

OpenFlowKit provides reusable skills, instructions, and workflow building blocks from a user-specified source. This source must have a traceable structure so installation, updates, and conversion can operate with functional clarity.

OpenCode is the primary format for skills in OpenFlowKit. Other target formats can be derived from this source, but they are not the leading representation in the source project.

## Assumptions

- None

## Open Questions

- None

## Requirements

### User-Defined Installation Source
**Type:** Functional  
**Description:** Installation and updates must obtain content from the source entered by the user.  
**Acceptance Criteria:**
- The user specifies the source from which skills, instructions, and workflow building blocks are read.
- Installation does not implicitly use a local working copy as a functional prerequisite.
- The source is used as the starting point for placement in the target project.
  **References:** OFK-001, installation concept

### OpenCode as Primary Skill Format
**Type:** Constraint  
**Description:** OpenFlowKit must use OpenCode as the primary format for skills.
**Acceptance Criteria:**
- Skills are maintained under the OpenCode-compliant `.opencode/skills/<skill-name>/` structure.
- No obsolete agent source structure is part of the target structure.
- Derived target formats may adapt the technical representation, but must originate from the OpenCode source.
  **References:** OFK-001

### Source Structure
**Type:** Constraint  
**Description:** The entered OpenFlowKit source must contain the expected project structure.  
**Acceptance Criteria:**
- Requirements are located under `docs/requirements/`.
- The synchronization instruction is located under `instructions/sync-skills.md`.
- OpenCode skill sources are direct child directories of `.opencode/skills/`, each containing `SKILL.md`.
- Exported OpenCode target skills are written to `.opencode/skills/<skill-name>/`.
- No project-specific OpenCode plugin dependencies are required.
- Skill sources contain `SKILL.md` files with OpenCode skill frontmatter.
  **References:** Project structure of the entered source
