# 章节模板

本模板定义每章的固定五段式结构和代码引用规范。Skill 在 Phase 4 中使用此模板生成每章内容。

---

## 章节文件结构

每个章节文件使用以下格式：

```markdown
# 第N章: [章节标题]

> **上一章回顾**: [一句话总结上一章成果]
>
> **本章目标**: [一句话说明本章解决什么问题、学到什么]

---

## 问题引入

[描述在当前演进节点，项目面临的问题或需求]

[对于入门级用户（级别 1-2），此处包含基础概念解释]

[用 Mermaid 图或绘图描述展示问题的背景/现状]

---

## 设计决策

[列出可选方案，解释为什么选择了当前方案]

[信息来源标注示例:]
[- openspec proposal: 直接引用 proposal.md 中的 motivation]
[- 用户回答: 引用 context.md 中记录的用户说明]
[- AI 推理: 基于代码变化的推断，标注"[推断]"]

[对于进阶用户（级别 4-5），此处包含方案对比表和替代方案讨论]

---

## 核心实现

[贴关键代码片段，配注释解读]

[代码片段要求见下方"代码引用规范"]

[用 Mermaid 图展示实现的关键流程或结构]

---

## 演进复盘

[此步改了什么——Before/After 对比]

[带来什么改进]

[留了什么技术债或已知问题]

---

## 扩展思考

[还能怎么做——替代方案简述]

[业界其他项目是怎么处理的]

[读者可以尝试的练习或思考题]

---

> **下一章预告**: [一句话预告下一章要解决的问题]
```

---

## 文件命名规则

```
<NN>-<章节标题>.md
```

- `NN`: 两位数字编号，从 00 开始
- 章节标题: 使用中文，去掉标点符号中不适合做文件名的字符
- 示例：
  - `00-课程概述.md`
  - `01-从零搭建Gate-Game通信链路.md`
  - `02-为什么需要拆分出CenterService.md`
  - `03-认证体系从简单到JWT.md`

---

## 代码引用规范

### 规则一：关键片段 + 注释解读

只贴最能说明设计意图的代码。每个片段配解读。

````markdown
下面是 Gate 服务中处理玩家连接的核心逻辑：

```java
@Override
public void onOpen(WebSocketSession session) {
    // 为每个连接分配唯一 ID，用于后续路由
    String connectionId = IdGenerator.next();
    // 将连接注册到连接管理器，维护 connectionId -> session 的映射
    connectionManager.register(connectionId, session);
    // 通知 Game 服务有新玩家上线
    gameServiceClient.notifyPlayerOnline(connectionId);
}
```

这段代码做了三件事：分配连接 ID、注册到管理器、通知业务服务。
注意 `connectionManager` 是整个网关的核心组件——它维护所有活跃连接的映射，
后续的消息路由都依赖它。
````

### 规则二：Before/After 对比格式

用于展示演进前后的变化。

````markdown
### Before（简单认证）

```java
// V1: 直接校验用户名密码，无 token 机制
public boolean authenticate(String username, String password) {
    return userStore.verify(username, password);
}
```

### After（JWT 认证）

```java
// V2: 签发 JWT token，支持无状态鉴权
public String authenticate(String username, String password) {
    if (!userStore.verify(username, password)) {
        throw new AuthException("Invalid credentials");
    }
    // 签发包含用户 ID 和角色的 JWT
    return jwtProvider.generateToken(
        Map.of("userId", userStore.getId(username),
               "roles", userStore.getRoles(username))
    );
}
```

**变化要点：**
- 返回类型从 `boolean` 变为 `String`（JWT token）
- 新增 `jwtProvider` 依赖，负责 token 签发
- 认证成功后不再依赖 session，实现无状态鉴权
````

### 规则三：自包含

- 代码片段必须包含足够的上下文（import 不需要，但类名/方法签名要有）
- 不能写"完整代码见 xxx 文件"作为唯一说明
- 如果片段引用了前面章节已解释的组件，用一句话提醒即可

---

## 章节适配用户级别

### 入门级（级别 1-2）适配

- "问题引入"中增加概念解释段落
- "核心实现"中代码片段更少但注释更详细
- "设计决策"给出结论为主，少量对比
- "扩展思考"侧重基础练习

### 进阶级（级别 4-5）适配

- "问题引入"直入主题，跳过基础概念
- "核心实现"包含更多实现细节和边界处理代码
- "设计决策"深入对比，含性能数据和架构影响分析
- "扩展思考"讨论业界做法和高级优化方向
