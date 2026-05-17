---
state: defined
---

# OFK-003: Installation and Target Project Integration

## Context

OpenFlowKit building blocks are adopted into target projects so teams can use and adapt them there in a project-specific way. Installation is performed by an LLM agent following OpenFlowKit instructions and places artifacts in the target project without imposing requirements on the target project's own versioning or governance.

The initial installation supports OpenCode as a direct target format and Claude Code as a converted target format. GitHub Copilot target support is optional, experimental, and outside the initial scope.

## Assumptions

- None

## Open Questions

- None

## Requirements

### Project-Local Agent Storage
**Type:** Functional  
**Description:** OpenFlowKit must provide agents so they can be stored locally in target projects and bound to the respective codebase.  
**Acceptance Criteria:**
- Agents are stored in the target project.
- The target project decides whether and how stored agents are versioned.
- OpenFlowKit does not take over version control for third-party target project artifacts.
  **References:** OFK-002, installation concept

### Target Tool Conversion
**Type:** Functional  
**Description:** The installing LLM agent must transfer OpenCode agents into the target project's target format when needed.  
**Acceptance Criteria:**
- For OpenCode target projects, agents are copied into the OpenCode-compliant target structure.
- For Claude Code, role description, semantic intent, and work instructions are transferred into the target format.
- The technical representation may be adapted to the target tool.
- OpenFlowKit does not provide a standalone installer or CLI for this transfer.
  **References:** OFK-002

### Initial Target Format Scope
**Type:** Constraint  
**Description:** The initial version does not need to support GitHub Copilot as a target format. Any Copilot handling is optional, experimental, and outside the initial scope.  
**Acceptance Criteria:**
- GitHub Copilot is not a mandatory supported target format in the initial version.
- Later support for GitHub Copilot remains possible, but is not part of this requirement.
- Copilot-specific implementation logic must not be presented as a mandatory installation or update path.
  **References:** Target tool conversion

### Conflict Handling During Installation
**Type:** Functional  
**Description:** The installing agent must check existing agents in the target project before copying and explicitly handle name conflicts.  
**Acceptance Criteria:**
- Before copying, the target directory is checked for agents with identical names.
- Existing agents are not overwritten silently.
- For name conflicts, the user decides between replacement and merging.
  **References:** Project-local agent storage

### Semantic Merging
**Type:** Functional  
**Description:** When merging existing agents, the installing agent must merge semantically instead of only combining line by line.  
**Acceptance Criteria:**
- Purpose, rules, role description, and concrete work instructions are evaluated.
- The existing target version has priority when it contains project-specific adaptations.
- Content from OpenFlowKit is integrated only as an addition when it is compatible with the target version.
  **References:** Conflict handling during installation
