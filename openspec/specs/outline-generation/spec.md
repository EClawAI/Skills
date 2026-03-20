## ADDED Requirements

### Requirement: Skill SHALL generate preview outline with four-level detail

The preview outline SHALL contain four levels of information for each chapter:
1. Chapter name (章节名称)
2. Chapter content summary (章节内容简介 — what problem this chapter solves, what the reader learns)
3. Section names (小节名称)
4. Section content summaries (小节内容简介 — what each section specifically covers)

#### Scenario: Preview outline generated
- **WHEN** project analysis is complete and the system generates a preview outline
- **THEN** every chapter SHALL have a name, content summary, and at least one section with name and summary

#### Scenario: Preview outline displayed to user
- **WHEN** the preview outline is presented
- **THEN** the user SHALL be able to see all four levels of detail for every chapter

### Requirement: Skill SHALL support unlimited interaction rounds for outline revision

The system SHALL present the outline for user review and support unlimited rounds of revision. The system SHALL NOT impose a maximum number of interaction rounds. Content generation SHALL only begin after explicit user confirmation.

#### Scenario: User requests modification
- **WHEN** the user requests a change to the outline (reorder, split, merge, add, remove, adjust depth, adjust perspective)
- **THEN** the system SHALL apply the modification and present the updated outline for further review

#### Scenario: User confirms outline
- **WHEN** the user explicitly confirms the outline (e.g., "可以了", "确认", "开始生成")
- **THEN** the system SHALL record the final outline to context.md and proceed to content generation

#### Scenario: Multiple rounds of revision
- **WHEN** the user has made 3+ rounds of modifications
- **THEN** the system SHALL continue to accept modifications without suggesting the user should stop

### Requirement: Skill SHALL support six types of outline modifications

The system SHALL support the following types of user modifications to the outline:

1. **Reorder** — change chapter sequence
2. **Split** — break one chapter into multiple
3. **Merge** — combine multiple chapters into one
4. **Add/Remove** — add new chapters or remove existing ones
5. **Adjust depth** — make a chapter deeper or shallower
6. **Adjust perspective** — change the narrative angle of a chapter

#### Scenario: Split a chapter
- **WHEN** the user says "第3章太大了，认证和TLS分开讲"
- **THEN** the system SHALL split chapter 3 into two chapters, each with appropriate sections, and renumber subsequent chapters

#### Scenario: Merge chapters
- **WHEN** the user says "限流和熔断放一起讲就行"
- **THEN** the system SHALL merge the two chapters into one with combined sections

#### Scenario: Reorder chapters
- **WHEN** the user says "安全部分放到通信后面"
- **THEN** the system SHALL move the security chapter after the communication chapter and renumber

### Requirement: Skill SHALL construct teaching path (pedagogical reordering)

The outline's chapter order SHALL follow optimal learning sequence, which may differ from the actual git commit chronology. The system SHALL reorder evolution nodes into a path that builds knowledge incrementally.

#### Scenario: Teaching order differs from git history
- **WHEN** git history shows security features were added before some core features
- **THEN** the system MAY reorder to teach core features first if that produces a better learning progression

### Requirement: Skill SHALL ask user to choose generation mode before starting

After outline confirmation, the system SHALL present two generation mode options:
1. **Batch mode (一次性生成)** — generate all chapters without interruption
2. **Chapter-by-chapter mode (逐章确认)** — pause after each chapter for user feedback (rewrite/deeper/shallower/continue)

#### Scenario: User selects batch mode
- **WHEN** the user chooses batch mode
- **THEN** the system SHALL generate all chapters sequentially without pausing

#### Scenario: User selects chapter-by-chapter mode
- **WHEN** the user chooses chapter-by-chapter mode
- **THEN** the system SHALL pause after each chapter and ask: "继续 / 重写 / 调深 / 调浅"
