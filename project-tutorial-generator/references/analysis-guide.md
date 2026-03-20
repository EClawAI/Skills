# 项目分析指南

本文档定义了 project-tutorial-generator 从四个信息源提取教学素材的策略。Skill 在 Phase 2 中按需加载本指南。

---

## 1. 双模式分化逻辑

在开始分析前，检测项目是否有 openspec：

```bash
ls <project_path>/openspec/changes/ 2>/dev/null
ls <project_path>/openspec/changes/archive/ 2>/dev/null
```

**判断规则：**
- 存在 `openspec/` 目录且包含 changes（活跃或归档）→ **富信息模式**
- 不存在 `openspec/` 目录 → **交互探查模式**

| 维度 | 富信息模式 | 交互探查模式 |
|------|-----------|-------------|
| 迭代粒度 | openspec tasks 驱动 | AI 分析 git + code 推理 |
| 迭代原因 | proposal.md 直取 | 询问用户 → 跳过则 AI 推理 |
| 决策思考 | design.md 直取 | 询问用户 → 跳过则 AI 推理 |

---

## 2. 信息源一：Git History 分析

### 2.1 基础信息获取

```bash
# 获取完整提交历史（单行摘要）
git log --oneline --all

# 获取详细提交信息（含日期、作者、完整 message）
git log --format="%H|%ai|%an|%s" --all

# 获取文件变更统计
git log --stat --oneline

# 获取分支结构
git branch -a
```

### 2.2 Commit 模式识别

将 commit 分类为以下模式，用于识别演进节点：

| 模式 | 识别特征 | 教学价值 |
|------|---------|---------|
| **功能提交** | feat:, feature:, 添加, 实现 | 高 — 新能力引入 |
| **重构提交** | refactor:, 重构, 抽取, 拆分 | 高 — 架构决策点 |
| **修复提交** | fix:, bugfix:, 修复 | 中 — 可展示问题→解决 |
| **依赖变更** | deps:, 添加依赖, pom.xml/package.json 变化 | 中 — 技术选型点 |
| **配置变更** | config:, 配置, .yml/.yaml 变化 | 低 — 通常合并到功能讲解 |
| **文档提交** | docs:, 文档 | 低 — 辅助信息 |
| **构建/CI** | ci:, build:, 流水线 | 中 — 工程实践教学 |

### 2.3 演进节点提取

**策略：合并相关 commit 为一个演进节点。**

1. 按时间和主题聚类相邻 commit
2. 同一功能的多次 commit 合并为一个节点
3. 每个节点记录：
   - 名称（功能描述）
   - 涉及的 commit hash 列表
   - 变更的文件范围
   - 推断的意图分类

**对于大型项目（>100 commits）：** 优先识别里程碑式变更（大量文件变动、新模块引入、架构重构），跳过细碎修复。

### 2.4 变更内容获取

```bash
# 获取特定 commit 的变更详情
git show <commit_hash> --stat
git diff <commit_hash>~1 <commit_hash>

# 获取两个节点间的完整变更
git diff <start_hash> <end_hash>

# 获取特定文件的演进历史
git log --follow -p -- <file_path>
```

---

## 3. 信息源二：OpenSpec Proposals 分析

### 3.1 发现 openspec changes

```bash
# 查看归档的 changes（已完成的迭代）
ls <project_path>/openspec/changes/archive/

# 查看活跃的 changes（进行中的迭代）
ls <project_path>/openspec/changes/
```

### 3.2 提取教学素材

对每个 change（优先处理归档的），读取以下文件：

| 文件 | 提取内容 | 对应教学元素 |
|------|---------|-------------|
| `proposal.md` | Why 部分 — 为什么要做这个改动 | → 章节的"问题引入" |
| `proposal.md` | What Changes 部分 — 改了什么 | → 章节的"演进复盘" |
| `design.md` | Decisions 部分 — 技术选择和理由 | → 章节的"设计决策" |
| `design.md` | Risks/Trade-offs 部分 | → 章节的"扩展思考" |
| `tasks.md` | 任务列表 | → 决定迭代粒度和实现步骤 |
| `specs/*/spec.md` | 需求场景 | → 代码示例的上下文 |

### 3.3 变更排序

归档的 changes 通常有日期前缀（如 `2026-03-09-change-xxx`），按日期排序即为真实迭代顺序。

对于无日期前缀的 changes，尝试：
1. 从 git log 中匹配相关 commit 的时间
2. 从 tasks.md 中的依赖关系推断顺序
3. 如无法确定，在大纲阶段让用户调整顺序

---

## 4. 信息源三：已有设计文档

### 4.1 文档发现策略

按以下优先级扫描：

```
1. <project_path>/docs/           # 最常见的文档目录
2. <project_path>/doc/            # 替代命名
3. <project_path>/*.md            # 根目录的 markdown 文件
4. <project_path>/README.md       # 项目说明
5. <project_path>/CHANGELOG.md    # 变更记录
6. <project_path>/wiki/           # Wiki 目录
7. <project_path>/design/         # 设计文档目录
8. <project_path>/architecture/   # 架构文档目录
```

### 4.2 文档分类

扫描到的文档按内容分类：

| 类型 | 识别方式 | 教学用途 |
|------|---------|---------|
| 架构文档 | 包含"架构"、"architecture"、模块关系描述 | 全景理解、第0章素材 |
| 设计方案 | 包含"设计"、"方案"、"design"、决策讨论 | 迭代原因和设计决策 |
| API 文档 | 包含"API"、"接口"、endpoint 描述 | 核心实现参考 |
| 开发计划 | 包含"计划"、"任务"、"roadmap"、"TODO" | 迭代步骤参考 |
| 技术调研 | 包含"调研"、"对比"、"选型" | 设计决策的替代方案 |
| 变更记录 | CHANGELOG 格式、版本号 | 演进时间线 |

### 4.3 文档质量评估

对每个文档快速评估：
- **时效性**：是否有日期，距今多久
- **完整性**：是否有完整的结构，还是片段笔记
- **与代码一致性**：提到的模块/类是否在代码中存在

低质量文档标记为"仅供参考"，不作为主要素材。

---

## 5. 信息源四：代码结构分析

### 5.1 模块识别

**Java/Maven 项目：**
```bash
find <project_path> -name "pom.xml" -maxdepth 3  # 子模块
```

**Node.js 项目：**
```bash
find <project_path> -name "package.json" -maxdepth 3
```

**Go 项目：**
```bash
find <project_path> -name "go.mod" -maxdepth 3
```

**通用策略：** 识别顶层目录作为模块边界。

### 5.2 依赖关系分析

- **模块间依赖**：分析 import/require 语句，构建依赖图
- **外部依赖**：从依赖管理文件（pom.xml, package.json 等）提取
- **关键依赖**：识别核心框架和库（Spring Boot, React, gRPC 等）

### 5.3 关键类/接口识别

按照以下启发式规则识别教学价值高的代码：

| 特征 | 教学价值 |
|------|---------|
| 入口点（main, Application, index） | 高 — 项目起点 |
| 接口/抽象类 | 高 — 架构骨架 |
| 配置类 | 中 — 理解系统组装 |
| 被大量引用的类 | 高 — 核心组件 |
| 包含复杂逻辑的类（行数多、方法多） | 中 — 核心业务 |
| 测试类 | 中 — 理解预期行为 |

### 5.4 设计模式识别

扫描常见设计模式的代码特征：
- Factory/Builder 模式 → 说明对象创建策略
- Observer/Listener 模式 → 说明事件驱动架构
- Strategy 模式 → 说明可扩展设计
- Repository/DAO 模式 → 说明数据访问层设计

---

## 6. 无 OpenSpec 时的交互询问流程

当项目没有 openspec 时，Skill 需要通过与用户交互补充迭代原因和决策信息。

### 6.1 流程

```
识别演进节点（从 git + code）
        │
        ▼
  ┌─ 对每个节点 ─────────────────────┐
  │                                    │
  │  展示节点信息：                     │
  │  "我发现项目在某阶段做了 X 变更"     │
  │  "涉及文件: ..."                   │
  │  "代码变化: ..."                   │
  │                                    │
  │  询问用户：                        │
  │  "请问这次变更的主要原因是什么？"     │
  │  • 选项A（基于代码变化推测）         │
  │  • 选项B（基于代码变化推测）         │
  │  • 其他（用户自由描述）              │
  │  • 跳过（我不清楚 / 不重要）         │
  │                                    │
  │  记录到 context.md：               │
  │  - 用户回答 → 来源: "用户回答"      │
  │  - 用户跳过 → AI 推理并标注来源      │
  │                                    │
  └───────────────────────────────────┘
```

### 6.2 AI 推理策略

当用户跳过时，基于以下信息推理迭代原因：
1. commit message 中的关键词
2. 变更的文件类型和范围
3. 新增的依赖
4. 与前后节点的关系

推理结果格式：
```
来源: AI 推理
原因: [推断] 根据新增的 spring-security 依赖和 JWT 相关类，
      推测此次变更是为了将简单认证升级为 JWT 基于令牌的认证体系
```

### 6.3 减少交互疲劳

- 如果识别出的演进节点超过 10 个，先展示完整列表，让用户标记哪些是重点
- 对用户标记为"不重要"的节点，直接用 AI 推理
- 对相似类型的节点（如连续多个 bugfix），合并询问
