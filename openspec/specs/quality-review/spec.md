## ADDED Requirements

### Requirement: Skill SHALL perform chapter self-check after generation

After each chapter is generated, the system SHALL run a self-check against a checklist covering:
- Content completeness (all five sections present)
- Code snippet readability (annotations present, self-contained)
- Diagram correctness (Mermaid syntax valid, descriptions complete)
- Audience level alignment (depth matches selected level)

#### Scenario: Self-check passes
- **WHEN** a chapter passes all self-check items
- **THEN** the system SHALL proceed to the next chapter (batch mode) or present for user review (chapter-by-chapter mode)

#### Scenario: Self-check finds issue
- **WHEN** a chapter fails a self-check item (e.g., missing "演进复盘" section)
- **THEN** the system SHALL auto-fix the issue before presenting the chapter

### Requirement: Skill SHALL check cross-chapter consistency

After all chapters are generated, the system SHALL verify consistency across chapters:
- Terminology consistency (same concept uses same term throughout)
- Reference integrity (chapter cross-references point to correct chapters)
- Progressive complexity (later chapters do not assume concepts not introduced in earlier ones)
- No content duplication (same explanation not repeated across chapters)

#### Scenario: Terminology inconsistency detected
- **WHEN** chapter 2 calls a component "网关服务" but chapter 5 calls it "Gate 服务"
- **THEN** the system SHALL normalize to the term used in the earliest chapter or the most common usage

#### Scenario: Forward reference detected
- **WHEN** chapter 3 references a concept first introduced in chapter 5
- **THEN** the system SHALL add a brief inline explanation or rearrange the reference

### Requirement: Skill SHALL generate completion summary

After all chapters pass quality checks, the system SHALL output a completion summary containing:
- Total number of chapters generated
- Total estimated reading time
- List of chapters with brief descriptions
- Any quality notes or caveats

#### Scenario: Tutorial complete
- **WHEN** all chapters have been generated and pass quality checks
- **THEN** the system SHALL display a summary with chapter list, estimated reading time, and the output directory path

### Requirement: Skill SHALL support pedagogy evaluation criteria

The system SHALL evaluate generated content against teaching effectiveness standards:
- Each chapter introduces no more than 2-3 new major concepts
- Practical examples accompany every abstract concept
- Each chapter has a clear "before and after" showing what changed
- The tutorial as a whole tells a coherent story from simple to complex

#### Scenario: Concept overload detected
- **WHEN** a chapter introduces more than 3 major new concepts
- **THEN** the system SHALL suggest splitting the chapter or simplifying content in the quality report
