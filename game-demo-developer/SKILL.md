---
name: game-demo-developer
description: 根据游戏策划描述或现有游戏玩法快速生成可演示的游戏demo
license: MIT
metadata:
  author: Game Demo Developer Team
  version: "1.0.0"
  domain: game-development
  role: creator
  scope: rapid-prototyping
  output-format: game-project
  related-skills: skill-planner, project-document-organizer
---

# Game Demo Developer

## Overview

使用此 skill 根据游戏策划描述或参考现有游戏玩法，快速生成可运行的游戏 demo 原型。支持多种技术栈（Unity、Cocos、WebGL），帮助游戏策划和开发者快速验证创意。

**Announce at start:** "我正在使用 game-demo-developer skill 来帮你快速生成游戏demo！"

## 核心功能

### 1. 游戏策划解析
- 理解自然语言描述的游戏玩法
- 支持参考现有游戏名称进行复刻
- 提取核心玩法、游戏类型、关键机制

### 2. 技术栈选择
- **Unity (C#)**: 适合2D/3D复杂游戏
- **Cocos Creator (TypeScript)**: 适合轻量级2D游戏
- **WebGL (JavaScript/HTML5)**: 适合快速Web原型（默认推荐）

### 3. 基础架构生成
- 自动创建项目目录结构
- 生成基础配置文件
- 设置构建系统

### 4. 核心玩法实现
- 实现主要游戏循环
- 处理输入控制（键盘/触摸）
- 实现碰撞检测、物理等基础系统
- 使用网络免费素材资源

### 5. Demo 测试与预览
- 自动构建项目
- 提供本地预览服务器
- 生成运行说明文档

### 6. 代码注释与文档
- 关键代码添加中文注释
- 生成 README.md 说明文档
- 提供架构说明

## 使用流程

### 步骤 1: 需求收集
1. 询问游戏描述或参考游戏
2. 确认游戏类型和维度（2D/3D）
3. 选择技术栈

### 步骤 2: 项目生成
1. 解析需求，确认理解
2. 创建项目结构
3. 生成基础代码

### 步骤 3: 玩法实现
1. 实现核心游戏机制
2. 集成资源素材
3. 添加基本UI

### 步骤 4: 测试交付
1. 构建项目
2. 启动预览服务器
3. 提供使用说明

## 配置选项

| 参数 | 类型 | 默认值 | 说明 |
|------|------|--------|------|
| tech_stack | enum | webgl | 技术栈选项: unity, cocos, webgl |
| game_dimension | enum | 2d | 游戏维度: 2d, 3d |
| complexity | enum | simple | 复杂度: simple, medium, full |
| resource_source | enum | web | 资源来源: web, placeholder |

## 使用示例

### 示例 1: 创建横版射击游戏
**输入**: "我想做一个简单的2D横版射击游戏，玩家控制飞船射击敌人"

**输出**:
- WebGL项目结构
- 玩家飞船控制（左右移动、射击）
- 敌人生成和移动
- 碰撞检测系统
- 简单的计分UI

### 示例 2: 复刻贪吃蛇
**输入**: "复刻贪吃蛇游戏的核心玩法"

**输出**:
- 完整的贪吃蛇游戏
- 方向控制
- 食物生成
- 碰撞检测和游戏结束逻辑

### 示例 3: 平台跳跃游戏
**输入**: "一个简单的平台跳跃游戏demo"

**输出**:
- 玩家角色控制（左右移动、跳跃）
- 平台系统
- 重力和物理模拟
- 简单关卡设计

## 最佳实践

1. **优先使用 WebGL**: 对于快速原型，WebGL 是最佳选择，无需额外安装引擎
2. **保持简单**: MVP 版本专注核心玩法，避免过度设计
3. **资源版权**: 确保使用的网络素材有明确的免费使用许可
4. **代码可读性**: 生成的代码应包含清晰的中文注释，便于后续修改
5. **可扩展性**: 代码结构应设计合理，便于功能扩展

## 错误处理

- **需求解析失败**: 提供游戏类型模板供用户选择
- **构建失败**: 给出详细的错误信息和修复建议
- **资源加载失败**: 自动降级使用占位符素材
- **预览服务器启动失败**: 提供手动运行说明

## 相关 Skill

- **skill-planner**: 用于规划更复杂的 skill
- **project-document-organizer**: 用于整理和文档化生成的游戏项目
