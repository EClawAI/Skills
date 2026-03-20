## ADDED Requirements

### Requirement: Skill SHALL dynamically generate 3-5 target audience levels based on project tech stack

The system SHALL NOT use generic "beginner/intermediate/advanced" levels. Instead, it SHALL analyze the project's technology stack and domain to generate 3-5 audience levels that are specific to this project.

#### Scenario: Java microservices project (e.g., Spring Boot + gRPC + Redis)
- **WHEN** the project uses Java, Spring Boot, gRPC, and Redis
- **THEN** the system SHALL generate levels such as:
  1. Java beginner (needs basic concept explanations)
  2. Java developer without microservices experience (focus on service decomposition motivation)
  3. Backend developer with microservices experience (focus on domain-specific patterns)
  4. Domain-specific developer (direct architecture comparison and deep implementation)
  5. Project maintainer/inheritor (project-specific designs, pitfalls, extension points)

#### Scenario: Frontend project (e.g., React + TypeScript + GraphQL)
- **WHEN** the project uses React, TypeScript, and GraphQL
- **THEN** the system SHALL generate levels relevant to frontend ecosystem knowledge progression, NOT Java-specific levels

### Requirement: Skill SHALL present audience levels for user selection

The system SHALL present the generated audience levels with descriptions explaining what each level means for tutorial content depth.

#### Scenario: User selects audience level
- **WHEN** the audience levels are presented
- **THEN** each level SHALL include a description of what the tutorial will emphasize and what will be skipped

#### Scenario: Audience level selection recorded
- **WHEN** the user selects a level
- **THEN** the selection SHALL be recorded in context.md under "目标用户" section

### Requirement: Audience level SHALL control tutorial depth dimensions

The selected audience level SHALL influence the following dimensions of generated content:

| Dimension | Lower levels | Higher levels |
|-----------|-------------|---------------|
| Basic concept explanations | Detailed | Skipped |
| Code snippet volume | Less, key fragments | More, with implementation details |
| Architecture decision discussion | Simplified, give conclusions | Deep comparison of alternatives |
| Diagram complexity | Simplified overviews | Detailed interaction/sequence diagrams |
| "Why not X" discussions | Occasional | Frequent |
| Performance/operations considerations | Mentioned | Expanded |

#### Scenario: Low-level audience — concept explanation
- **WHEN** the selected level is 1 or 2 (entry-level)
- **THEN** each chapter SHALL include explanations of fundamental concepts used in that chapter

#### Scenario: High-level audience — skip basics
- **WHEN** the selected level is 4 or 5 (advanced)
- **THEN** the system SHALL skip basic concept explanations and focus on architecture decisions and project-specific implementation details
