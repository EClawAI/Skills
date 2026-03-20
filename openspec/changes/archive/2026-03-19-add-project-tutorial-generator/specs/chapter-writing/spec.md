## ADDED Requirements

### Requirement: Each chapter SHALL follow the five-section structure

Every chapter (except chapter 0) SHALL use the following fixed structure:

1. **问题引入** — What problem or need exists at this evolution point
2. **设计决策** — What options were considered, why the chosen approach was selected
3. **核心实现** — Key code snippets with annotation-style explanations
4. **演进复盘** — What changed, what improved, what technical debt remains
5. **扩展思考** — Alternative approaches, industry practices, exercises for the reader

#### Scenario: Standard chapter generated
- **WHEN** a chapter is generated for a normal evolution step
- **THEN** the chapter SHALL contain all five sections in order

#### Scenario: Chapter adapts to audience level
- **WHEN** the audience level is entry-level (1-2)
- **THEN** "问题引入" SHALL include basic concept explanations, and "核心实现" SHALL have fewer but more thoroughly annotated code snippets
- **WHEN** the audience level is advanced (4-5)
- **THEN** "设计决策" SHALL include deep comparison of alternatives, and "扩展思考" SHALL reference industry best practices

### Requirement: Chapter 0 SHALL be a course overview

Chapter 0 (00-课程概述.md) SHALL contain:
1. Project panoramic architecture diagram (全景图)
2. Technology stack description with version info
3. Tutorial roadmap showing all chapters and their relationships
4. Prerequisites for the reader based on audience level

#### Scenario: Course overview generated
- **WHEN** the system starts content generation
- **THEN** chapter 0 SHALL be generated first, before any other chapter

### Requirement: Code snippets SHALL be self-contained with annotations

Code references in chapters SHALL be self-contained — a reader SHALL be able to understand the code without opening the source project. Each snippet SHALL include inline comments or paragraph explanations.

#### Scenario: Code snippet with annotation
- **WHEN** a code snippet is included in a chapter
- **THEN** the snippet SHALL include either inline comments explaining key lines or a paragraph explanation immediately following the code block

#### Scenario: Cross-version comparison
- **WHEN** a chapter discusses an evolution from version A to version B
- **THEN** the system SHALL use a "Before / After" format showing both versions side by side with explanations of what changed and why

### Requirement: Chapters SHALL maximize use of diagrams

The system SHALL use diagrams wherever they would aid understanding. Diagrams SHALL be generated in two formats:

1. **Mermaid code blocks** — for flowcharts, sequence diagrams, class diagrams, state diagrams, and gitgraph
2. **Structured drawing descriptions** — for diagrams that Mermaid cannot express (deployment topology, complex UI interactions, performance comparison charts)

#### Scenario: Mermaid diagram generated
- **WHEN** a flow, sequence, class relationship, or state machine needs illustration
- **THEN** the system SHALL generate a Mermaid code block that renders correctly in standard Markdown viewers

#### Scenario: Drawing description generated
- **WHEN** a diagram type cannot be expressed in Mermaid (e.g., deployment topology with icons)
- **THEN** the system SHALL generate a structured drawing description with: diagram type, title, layout, elements, connections, and annotations — precise enough for manual drawing or AI illustration tools

#### Scenario: Diagram density
- **WHEN** a chapter covers architectural changes
- **THEN** the chapter SHALL contain at least one architectural diagram (Mermaid or description)

### Requirement: Each chapter SHALL be an independent Markdown file

Each chapter SHALL be output as a separate `.md` file in the tutorial output directory, named with a numeric prefix for ordering: `00-课程概述.md`, `01-xxx.md`, `02-xxx.md`, etc.

#### Scenario: File naming
- **WHEN** a chapter with index N and title "从零搭建通信链路" is generated
- **THEN** the file SHALL be named `01-从零搭建通信链路.md` (with zero-padded index)

#### Scenario: Independent readability
- **WHEN** a reader opens a single chapter file
- **THEN** the chapter SHALL be understandable on its own, with brief references to prior chapters where needed (not requiring the reader to have read them)
