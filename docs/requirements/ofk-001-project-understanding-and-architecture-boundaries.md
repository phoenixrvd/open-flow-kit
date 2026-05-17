---
state: defined
---

# OFK-001: Project Understanding and Architecture Boundaries

## Context

OpenFlowKit provides instructions, agents, and workflow building blocks for AI-assisted automation of development and team processes. The project is designed for pragmatic use in real software projects and is intended to structure recurring engineering tasks without prescribing a closed automation system.

The goal is to make AI-assisted workflow automation easier for development teams to use. OpenFlowKit describes reusable workflows, roles, and operating instructions that are primarily formulated for OpenCode and can be transferred into other target environments during installation.

The focus is on supporting real development and team processes, for example documentation, reviews, project structuring, issue handling, release preparation, and recurring operational tasks. Full autonomy is not a goal; human control remains part of the working model.

OpenFlowKit is provided as a public source project hosted on GitHub. The documented project context distinguishes between the OpenFlowKit source and target projects into which agents and workflow building blocks are installed.

## Assumptions

- None

## Open Questions

- None

## Requirements

### Public GitHub Source Repository
**Type:** Constraint  
**Description:** OpenFlowKit must be maintained as a publicly accessible GitHub project.  
**Acceptance Criteria:**
- The OpenFlowKit source repository is hosted publicly on GitHub.
- GitHub serves as the central source for the provided instructions, agents, requirements, and workflow building blocks.
- The public GitHub repository describes the distribution of OpenFlowKit itself, not necessarily the initially supported installation sources.
  **References:** Project context

### Adaptability and Reuse
**Type:** Non-functional  
**Description:** OpenFlowKit must support functional reuse and local adaptation of agents, instructions, and workflows.  
**Acceptance Criteria:**
- The public GitHub structure remains traceable as a source for reuse.
- Content can be adapted to project context.
- Teams can modify agents, instructions, and workflows locally.
- The repository intentionally remains readable and free of unnecessary runtime dependencies.
  **References:** Project context

### Security Boundary
**Type:** Constraint  
**Description:** OpenFlowKit must treat agents, prompts, and instructions as potentially effective automation artifacts and leave responsibility for safe use with the user.  
**Acceptance Criteria:**
- Agents, prompts, and instructions are treated like executable code.
- Users are responsible for reviews, permissions, system access, secret handling, and their own sandbox strategies.
- OpenFlowKit does not guarantee that provided or merged agents are safe in every project context.
  **References:** Project context

### Non-Goals
**Type:** Constraint  
**Description:** OpenFlowKit must not be understood as a deterministic plugin system, security framework, sandbox system, central governance mechanism, or complete agent operating system.  
**Acceptance Criteria:**
- OpenFlowKit does not define a stable plugin runtime with guaranteed execution semantics.
- OpenFlowKit does not provide a security architecture, policy engine, or protection layer.
- OpenFlowKit does not isolate processes, tools, or agents.
- OpenFlowKit does not guarantee reproducible LLM results.
- OpenFlowKit does not replace a platform for scheduling, tool orchestration, permissions, or runtime management.
  **References:** Project context
