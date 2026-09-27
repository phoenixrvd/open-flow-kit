---
state: implemented
---

# OFK-004: GitHub Deployment SSH Key Skill

## Context

OpenFlowKit provides reusable skills for recurring engineering tasks (OFK-001). Creating a dedicated SSH key for a project's GitHub deployment access is such a recurring task and currently has no skill support. A skill should guide the agent through creating the key, configuring local SSH access, and preparing the key for GitHub deployment use. Skills are maintained in the OpenCode skill source structure (OFK-002) and distributed to target projects with `instructions/sync-skills.md`.

## Assumptions

- The project name is derived from the current working directory path at skill invocation.
- GitHub is the hosting platform the skill targets.

## Open Questions

- None

## Requirements

### SSH Key Skill
**Type:** Functional  
**Description:** OpenFlowKit must provide a skill that supports creating an SSH key for a project's GitHub deployment access.  
**Acceptance Criteria:**
- A skill exists whose purpose is creating an SSH key for GitHub deployment use.
- The skill is named `release-ssh-keygen`.
- The skill includes a plain POSIX shell script as a local artifact that performs the key generation.
- The skill is maintained in the OpenCode skill source structure.
  **References:** OFK-001, OFK-002

### Manual Invocation Only
**Type:** Constraint  
**Description:** The skill must never be invoked automatically by the model; only explicit user invocation may load it.  
**Acceptance Criteria:**
- The skill frontmatter sets `slash: true` so it stays available in the interactive command catalog.
- The skill frontmatter sets `metadata.opencode/autoinvoke: false` so the skill is omitted from the model's available skill list.
- The skill description states the purpose and contains no trigger phrases.
- The skill remains registered and loadable explicitly by ID.
  **References:** SSH Key Skill

### Project Name Confirmation
**Type:** Functional  
**Description:** On invocation, the skill must determine the project name from the current path and have the user confirm it or provide an alternative.  
**Acceptance Criteria:**
- The project name is determined automatically from the current working directory on invocation.
- The user is asked to confirm whether the determined project name fits.
- The user can specify a different configuration (project name) instead of the determined one.
  **References:** None

### Existing Key Check
**Type:** Functional  
**Description:** Before generating a key, the skill must check whether the key already exists and, if it does, skip generation and show the existing public key with the GitHub guide.  
**Acceptance Criteria:**
- Before key generation, the target key file is checked for existence.
- If the key exists, no new key is generated and a message reports the existing key.
- If the key exists, the public key is displayed together with the short GitHub storage instruction.
  **References:** Non-interactive key generation, Public key output and GitHub deployment guide

### Non-Interactive Key Generation
**Type:** Functional  
**Description:** The skill must generate the SSH key with the Ed25519 algorithm, without a passphrase, and without interactive prompts.  
**Acceptance Criteria:**
- The key is generated with the Ed25519 algorithm.
- The generated key has no passphrase.
- Key generation runs without interactive requests.
- The key file is stored as `~/.ssh/gh_<project_name_in_snake_case>` with the snake_case `gh_` prefix.
  **References:** Project name confirmation

### Public Key Output and GitHub Deployment Guide
**Type:** Functional  
**Description:** After key generation, the skill must output the public key and a short guide on where to store the key in GitHub, noting that the key must be stored as a read-only deployment key.  
**Acceptance Criteria:**
- The public key is displayed after generation.
- A short instruction describes where to add the key in GitHub.
- The output states that the key must be added in GitHub as a read-only deployment key.
  **References:** Non-interactive key generation

### SSH Config Entry
**Type:** Functional  
**Description:** The skill must provide an SSH config entry for `~/.ssh/config`, display it, and offer to write it into the file.  
**Acceptance Criteria:**
- The entry uses `gh-<project-name-in-kebab-case>` as `Host`.
- The entry sets `HostName github.com`, `User git`, and `IdentitiesOnly yes`.
- The entry references the generated key as `IdentityFile ~/.ssh/gh_<project_name_in_snake_case>`.
- The entry is displayed for manual entry and writing it to `~/.ssh/config` is offered with user confirmation.
- Before writing, `~/.ssh/config` is checked for an existing entry for this host.
- If an entry already exists, no changes are made; the entry is only displayed and the user is informed that it already exists.
  **References:** Project name confirmation, Non-interactive key generation

### Additional Git Remote
**Type:** Functional  
**Description:** The skill must configure an additional git remote named `deploy` in the project, in addition to origin, pointing to the SSH config host entry.  
**Acceptance Criteria:**
- The project receives an additional git remote named `deploy` besides `origin`.
- The remote URL reuses the origin repository path with the host replaced by the SSH config host (`Host gh-<project-name-in-kebab-case>`).
- If the project has no `origin` remote, the user is asked for the repository URL, and both `origin` and `deploy` are created.
- If a remote named `deploy` already exists, nothing is changed; the summary reports that it already exists and shows its URL.
- The remote is added only after the user confirms.
  **References:** SSH Config Entry

### Project-Language Output
**Type:** Constraint  
**Description:** The skill must produce all user-facing output in the language of the target project.  
**Acceptance Criteria:**
- All user-facing output is written in the project's language.
  **References:** Public key output and GitHub deployment guide
