## 1. Skill 骨架搭建

- [x] 1.1 创建 `project-tutorial-generator/` 目录结构（SKILL.md、templates/、references/）
- [x] 1.2 编写 SKILL.md 框架：description、触发条件、配置参数、六阶段工作流概览（Phase 0-5 骨架）
- [x] 1.3 编写 SKILL.md Phase 0（初始化）：接收项目路径、确认输出目录、快速扫描项目基本信息、创建 context.md

## 2. 项目分析能力（project-analysis）

- [x] 2.1 编写 `references/analysis-guide.md`：四个信息源的分析策略（git history、openspec proposals、existing docs、code structure）
- [x] 2.2 在 analysis-guide.md 中定义 git history 分析方法：git log 解析、commit 模式识别、演进节点提取
- [x] 2.3 在 analysis-guide.md 中定义 openspec 分析方法：读取 archived changes 的 proposal.md/design.md/tasks.md，提取迭代动机和设计决策
- [x] 2.4 在 analysis-guide.md 中定义设计文档发现策略：扫描 docs/、doc/、项目根目录 .md 文件、README、CHANGELOG
- [x] 2.5 在 analysis-guide.md 中定义代码结构分析方法：模块识别、依赖关系、关键类/接口、设计模式
- [x] 2.6 在 analysis-guide.md 中定义有/无 openspec 的双模式分化逻辑
- [x] 2.7 在 analysis-guide.md 中定义无 openspec 时的交互询问流程：识别演进节点 → 逐节点询问用户 → 跳过则 AI 推理
- [x] 2.8 编写 SKILL.md Phase 2（项目深度分析）：引用 analysis-guide.md，编排分析流程
- [x] 2.9 定义 context.md 的完整格式：项目信息、目标用户、演进节点与决策（含来源标注）、大纲

## 3. 目标用户定级（audience-profiling）

- [x] 3.1 编写 SKILL.md Phase 1（目标用户定级）：基于技术栈动态生成 3-5 档、展示给用户选择、记录到 context.md
- [x] 3.2 在 `references/pedagogy-guide.md` 中定义用户级别对教程深度的影响矩阵（概念解释、代码量、架构讨论、图表复杂度等维度）

## 4. 课程大纲生成（outline-generation）

- [x] 4.1 编写 `templates/course-outline.md`：大纲预览模板，包含章节名+简介、小节名+简介的展示格式
- [x] 4.2 编写 SKILL.md Phase 3（大纲生成与确认）：生成预览 → 用户反馈循环（不限轮次）→ 用户确认
- [x] 4.3 在 Phase 3 中定义六种修改操作的处理逻辑：重排、拆分、合并、增删、调深度、调视角
- [x] 4.4 在 Phase 3 确认后增加生成模式选择：一次性生成 vs 逐章确认

## 5. 章节内容生成（chapter-writing）

- [x] 5.1 编写 `templates/chapter-template.md`：五段式章节模板（问题引入→设计决策→核心实现→演进复盘→扩展思考）
- [x] 5.2 在 chapter-template.md 中定义代码引用规范：关键片段 + 注释解读、Before/After 对比格式
- [x] 5.3 编写 `templates/diagram-patterns.md`：Mermaid 图表模式库（flowchart、sequenceDiagram、classDiagram、stateDiagram、gitgraph 示例）
- [x] 5.4 在 diagram-patterns.md 中定义绘图描述格式规范：图表类型、标题、布局、元素、连接、标注
- [x] 5.5 编写 SKILL.md Phase 4（内容生成）：生成第0章（课程概述）→ 逐章生成 → 两种模式处理
- [x] 5.6 在 Phase 4 中定义逐章确认模式的交互：每章后暂停，支持继续/重写/调深/调浅
- [x] 5.7 在 Phase 4 中定义章节文件命名规则：`00-课程概述.md`、`01-xxx.md` 等

## 6. 质量检查（quality-review）

- [x] 6.1 在 `references/pedagogy-guide.md` 中编写章节自检清单：内容完整性、代码可读性、图表正确性、用户级别对齐
- [x] 6.2 在 pedagogy-guide.md 中编写章节间一致性检查标准：术语一致性、引用完整性、渐进复杂度、无重复内容
- [x] 6.3 在 pedagogy-guide.md 中编写教学效果评估标准：每章不超过 2-3 个新概念、实例配合抽象概念、清晰的 before/after
- [x] 6.4 编写 SKILL.md Phase 5（质量检查）：执行自检 → 一致性检查 → 输出完成摘要

## 7. 集成与验证

- [x] 7.1 审查 SKILL.md 总行数，确保 < 500 行；超出部分移到 references/ 或 templates/
- [x] 7.2 安装 skill 到 `~/.agents/skills/project-tutorial-generator/`
- [ ] 7.3 使用 GateDemo 项目（~/Work/Lzw/GateDemo/）作为测试素材，验证完整工作流
- [x] 7.4 编写 evals/evals.json 测试用例
