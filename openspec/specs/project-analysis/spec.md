## ADDED Requirements

### Requirement: Skill SHALL support four information sources for project analysis

The system SHALL analyze projects by extracting teaching material from four information sources:
1. **git history** — commit logs, diffs, branch structure to reconstruct evolution timeline
2. **openspec proposals** — proposal.md, design.md, tasks.md to extract iteration motivations and design decisions
3. **existing design docs** — any `.md` files in the project that contain design decisions, technical plans, architecture descriptions
4. **code structure** — module organization, dependency relationships, key classes/interfaces, design patterns

#### Scenario: Project with all four sources available
- **WHEN** the project has git history, openspec directory, design docs in `docs/`, and source code
- **THEN** the system SHALL extract and correlate information from all four sources

#### Scenario: Project with only git and code (no openspec, no docs)
- **WHEN** the project has no `openspec/` directory and no design documents
- **THEN** the system SHALL analyze git history and code structure only, and enter interactive inquiry mode for missing context

#### Scenario: Project with openspec but no git history
- **WHEN** the project has openspec proposals but no meaningful git history (e.g., single commit or squashed)
- **THEN** the system SHALL primarily use openspec proposals for evolution narrative and code structure for implementation details

### Requirement: Skill SHALL detect openspec presence and switch behavior mode

The system SHALL check for the existence of an `openspec/` directory in the project root and automatically select the appropriate working mode.

#### Scenario: OpenSpec detected — rich information mode
- **WHEN** `openspec/` directory exists with archived or active changes
- **THEN** the system SHALL use openspec tasks as iteration granularity, extract iteration reasons from proposal.md, and extract design decisions from design.md

#### Scenario: No OpenSpec — interactive inquiry mode
- **WHEN** no `openspec/` directory exists
- **THEN** the system SHALL identify key evolution points from git history and code analysis, then interactively ask the user about iteration reasons for each point

#### Scenario: User skips iteration reason inquiry
- **WHEN** the system asks the user about an evolution point's motivation and the user chooses "skip" or states they don't know
- **THEN** the system SHALL use AI inference to generate a plausible iteration reason based on code changes, and record the source as "AI 推理"

### Requirement: Skill SHALL identify key evolution nodes

The system SHALL analyze the project to identify significant evolution points that represent meaningful iteration steps.

#### Scenario: Evolution nodes from openspec
- **WHEN** the project has openspec with archived changes
- **THEN** the system SHALL treat each archived change as a potential evolution node, using the change's proposal for motivation

#### Scenario: Evolution nodes from git history
- **WHEN** the project has no openspec but has git history
- **THEN** the system SHALL analyze commit patterns (feature commits, major refactors, dependency additions) to identify evolution nodes

### Requirement: Skill SHALL discover design documents automatically

The system SHALL scan the project for existing design documents by checking common locations: `docs/`, `doc/`, project root `.md` files, `README.md`, `CHANGELOG.md`, and any markdown files containing architecture/design content.

#### Scenario: Documents found in standard locations
- **WHEN** the project has `.md` files in `docs/` directory
- **THEN** the system SHALL read and index these documents as potential teaching material

#### Scenario: No design documents found
- **WHEN** no design documents are found in any standard location
- **THEN** the system SHALL proceed with git history and code analysis only, without error

### Requirement: Skill SHALL record all analysis results to context.md

All analysis results, user responses, and AI inferences SHALL be recorded to `context.md` in the tutorial output directory. Each entry SHALL include its source: "openspec", "design doc", "user response", or "AI inference".

#### Scenario: Recording user response
- **WHEN** the user provides a reason for an evolution point
- **THEN** the system SHALL record it in context.md with source marked as "用户回答"

#### Scenario: Recording AI inference
- **WHEN** the user skips a question and AI generates an inference
- **THEN** the system SHALL record it in context.md with source marked as "AI 推理"
