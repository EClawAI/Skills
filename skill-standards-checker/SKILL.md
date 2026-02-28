---
name: "skill-standards-checker"
description: "检查和规范新 skill 的开发，确保所有 skill 符合团队标准。调用此 skill 在创建新 skill 后、更新现有 skill 时或定期检查所有 skill 的规范性。支持动态添加新的规范规则。"
license: MIT
metadata:
  author: Skill Standards Team
  version: "1.0.0"
  domain: skills
  role: quality-assurance
  scope: standards-enforcement
  output-format: report
  related-skills: skill-creator, skill-planner
---

# Skill Standards Checker

## Overview

使用此 skill 确保所有新创建或更新的 skill 都符合团队的开发规范和标准。提供可扩展的规范规则系统，可以随时添加新的检查规则。

**Announce at start:** "我正在使用 skill-standards-checker skill 来帮你检查 skill 规范！"

## 核心功能

### 1. 规范检查引擎
- 验证 SKILL.md 的基本结构
- 检查 Announce at start 提示是否存在
- 验证 metadata 配置完整性
- 可扩展的规则系统

### 2. 规范模板生成
- 提供标准的 SKILL.md 模板
- 包含所有必需的字段
- 支持自定义模板

### 3. 自动修复建议
- 发现问题时提供修复建议
- 自动生成缺失的部分
- 交互式修复流程

### 4. 规范管理
- 可以添加新的规范规则
- 支持规则的启用/禁用
- 规则优先级管理

### 5. 报告生成
- 生成规范检查报告
- 高亮显示问题和建议
- 导出检查结果

## 配置选项

| 参数 | 类型 | 默认值 | 说明 |
|------|------|--------|------|
| action | enum | check | 操作类型: check, create-template, add-rule, list-rules |
| target_skill | string | - | 要检查的 skill 名称（留空则检查所有）|
| auto_fix | boolean | false | 是否自动修复问题 |
| strict_mode | boolean | true | 严格模式（警告视为错误）|

## 使用流程

### 操作类型 1: 检查规范 (check)

1. 选择要检查的 skill（或检查所有）
2. 运行所有规范规则检查
3. 显示检查结果报告
4. 提供修复建议
5. 可选：自动修复问题

### 操作类型 2: 创建模板 (create-template)

1. 询问新 skill 的基本信息
2. 生成符合规范的 SKILL.md 模板
3. 包含所有必需的字段和结构
4. 提供 Announce at start 提示占位符

### 操作类型 3: 添加规则 (add-rule)

1. 询问新规则的名称和描述
2. 定义规则的检查逻辑
3. 设置规则的优先级和严重程度
4. 保存规则到配置文件

### 操作类型 4: 列出规则 (list-rules)

1. 显示所有当前启用的规则
2. 显示规则的描述和状态
3. 允许启用/禁用特定规则

## 规范规则列表

### 当前内置规则

| 规则 ID | 名称 | 严重程度 | 描述 |
|---------|------|----------|------|
| RULE-001 | Announce at Start | 必须 | 检查是否有 Announce at start 提示 |
| RULE-002 | Metadata 完整性 | 必须 | 检查 metadata 配置是否完整（license, author, version 等）|
| RULE-003 | Overview 部分 | 必须 | 检查是否有 Overview 部分 |
| RULE-004 | Description 完整性 | 必须 | 检查 description 是否包含功能和触发条件 |
| RULE-005 | 文档结构 | 建议 | 检查文档结构是否完整 |
| RULE-006 | 配置选项表格 | 建议 | 检查是否有配置选项表格 |
| RULE-007 | 使用示例 | 建议 | 检查是否有使用示例 |
| RULE-008 | 错误处理 | 建议 | 检查是否有错误处理部分 |
| RULE-009 | 相关 Skill | 建议 | 检查是否有相关 Skill 引用 |

### 添加新规则

要添加新的规范规则，请使用 `add-rule` 操作，规则将保存在：
`.trae/skills/skill-standards-checker/config/rules.json`

## 使用示例

### 示例 1: 检查单个 skill

**输入**:
```
action: check
target_skill: game-demo-developer
auto_fix: false
```

**输出**:
```
=== Skill 规范检查报告 ===
Skill: game-demo-developer

✅ 通过: RULE-001 (Announce at Start)
✅ 通过: RULE-002 (Metadata 完整性)
✅ 通过: RULE-003 (Overview 部分)
✅ 通过: RULE-004 (Description 完整性)
✅ 通过: RULE-005 (文档结构)
✅ 通过: RULE-006 (配置选项表格)
✅ 通过: RULE-007 (使用示例)
✅ 通过: RULE-008 (错误处理)
✅ 通过: RULE-009 (相关 Skill)

🎉 所有检查通过！
```

### 示例 2: 检查有问题的 skill

**输入**:
```
action: check
target_skill: my-new-skill
auto_fix: true
```

**输出**:
```
=== Skill 规范检查报告 ===
Skill: my-new-skill

❌ 失败: RULE-001 (Announce at Start)
   - 问题: 缺少 Announce at start 提示
   - 建议: 添加 "**Announce at start:** \"我正在使用 xxx skill 来帮你...\""

❌ 失败: RULE-002 (Metadata 完整性)
   - 问题: 缺少 license, author, version metadata
   - 建议: 添加完整的 metadata 配置

⚠️ 警告: RULE-005 (文档结构)
   - 问题: 缺少错误处理部分
   - 建议: 添加错误处理章节

是否自动修复？(y/n): y

🔧 正在自动修复...
✅ 已添加 Announce at start 提示
✅ 已添加 metadata 配置
✅ 已生成错误处理部分模板

修复完成！请检查并完善内容。
```

### 示例 3: 创建新 skill 模板

**输入**:
```
action: create-template
skill_name: my-awesome-skill
skill_description: "做一些很棒的事情"
```

**输出**:
```
已生成 SKILL.md 模板！

模板特点:
✅ 包含完整的 metadata
✅ 包含 Announce at start 提示
✅ 包含 Overview 部分
✅ 包含配置选项表格
✅ 包含使用示例
✅ 包含错误处理部分
✅ 包含相关 Skill 引用

请编辑模板并填写具体内容。
```

### 示例 4: 添加新规则

**输入**:
```
action: add-rule
rule_id: RULE-010
rule_name: 中文注释要求
severity: 建议
description: "检查关键代码是否有中文注释"
```

**输出**:
```
✅ 规则已添加！
规则 ID: RULE-010
名称: 中文注释要求
严重程度: 建议
描述: 检查关键代码是否有中文注释

规则已保存到配置文件，下次检查时生效。
```

## 何时使用此技能

### 1. 创建新 skill 后
- **skill-creator 完成后** - 立即检查新创建的 skill 是否符合规范
- **手动创建 skill 后** - 验证手工创建的 skill 规范性

### 2. 更新现有 skill 时
- **修改 SKILL.md 后** - 确保更新后仍符合规范
- **添加新功能后** - 检查文档是否同步更新

### 3. 定期检查
- **每周/每月** - 批量检查所有 skill 的规范性
- **新规范发布后** - 使用新规则重新检查所有 skill

### 4. 团队协作
- **代码审查前** - 确保提交的 skill 符合规范
- **新人培训** - 使用此 skill 作为教学工具

## 最佳实践

1. **在 skill-creator 后立即调用** - 确保新 skill 从一开始就符合规范
2. **使用自动修复功能** - 节省时间，但要检查自动生成的内容
3. **定期添加新规则** - 根据团队需求持续完善规范
4. **保持规则平衡** - 必须规则和建议规则合理搭配
5. **文档与规则同步** - 更新规则时同步更新此文档

## 错误处理

- **Skill 不存在**: 提示用户检查 skill 名称
- **规则加载失败**: 使用默认规则继续检查
- **自动修复失败**: 提供手动修复指导
- **权限不足**: 提示用户检查目录读写权限
- **配置文件损坏**: 恢复默认配置

## 相关 Skill

- **skill-creator**: 用于创建新 skill，创建后建议调用此 skill 检查
- **skill-planner**: 用于规划 skill 设计，规划完成后可使用此 skill 的模板功能
