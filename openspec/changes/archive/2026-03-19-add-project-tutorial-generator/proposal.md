## Why

现有的 `project-document-organizer` skill 只能生成项目最终态的参考文档（架构图、模块说明、API 文档），但无法展现项目是如何一步步迭代演进到当前状态的。开发者学习一个项目时，最有价值的不是"它现在长什么样"，而是"它是怎么一步步长成这样的"——每一步迭代的原因、设计决策的权衡、从简单到复杂的演进脉络。

需要一个新的 skill，能够分析项目的演进历史，将其重构为一条清晰的教学路径，生成可独立阅读的、按迭代步骤组织的系列教程文档。

## What Changes

- 新增 `project-tutorial-generator` skill，安装到 `~/.agents/skills/` 全局可用
- Skill 主文件 `SKILL.md`（<500 行），复杂逻辑拆分到 `templates/` 和 `references/` 按需加载
- 支持接收任意项目路径作为输入，不绑定特定项目
- 支持四个信息源分析：git history、openspec proposals、existing design docs、code structure
- 支持有/无 openspec 的项目，行为自动分化
- 交互式工作流：目标用户定级 → 项目分析 → 大纲预览与确认 → 内容生成
- 输出为 Markdown 系列文档，每章独立文件，含 Mermaid 图表和绘图描述
- context.md 永久保留在教程输出目录，记录用户画像、演进决策、大纲等元数据

## Capabilities

### New Capabilities

- `project-analysis`: 项目分析引擎——从 git history、openspec proposals、existing design docs、code structure 四个信息源提取教学素材，识别关键演进节点，生成中间分析结果
- `audience-profiling`: 目标用户定级——基于项目技术栈动态生成 3-5 档目标用户级别，用户选择后决定教程深度、代码量、概念解释详细程度
- `outline-generation`: 课程大纲生成与交互确认——将分析结果映射为教学路径，生成含章节名+简介、小节名+简介的预览大纲，支持不限轮次的交互修订直到用户确认
- `chapter-writing`: 章节内容生成——按"问题引入→设计决策→核心实现→演进复盘→扩展思考"固定结构生成每章内容，包含关键代码片段+注释解读、Mermaid 图表、绘图描述
- `quality-review`: 质量检查——章节自检清单（内容完整性、代码可读性、图表正确性）、章节间一致性检查、教学效果评估标准

### Modified Capabilities

（无，本变更不修改任何现有能力）

## Impact

- **新增文件**：
  - `project-tutorial-generator/SKILL.md` — 主入口文件
  - `project-tutorial-generator/templates/course-outline.md` — 课程大纲模板
  - `project-tutorial-generator/templates/chapter-template.md` — 章节模板
  - `project-tutorial-generator/templates/diagram-patterns.md` — Mermaid 图表模式库
  - `project-tutorial-generator/references/analysis-guide.md` — 项目分析指南
  - `project-tutorial-generator/references/pedagogy-guide.md` — 教学设计原则
- **依赖**：无外部代码依赖，纯 Skill 指令文件
- **安装**：最终产物复制到 `~/.agents/skills/project-tutorial-generator/`
- **测试**：使用 GateDemo 项目（`~/Work/Lzw/GateDemo/`）作为验证素材
