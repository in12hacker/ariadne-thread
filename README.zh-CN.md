# Ariadne Thread

> 让 AI 代理能够导航、定位并安全修改代码——而无需通读整个项目。

[English](README.md)

---

## 它做什么

Ariadne Thread 为代码库建立一套分层索引，为 AI 代理提供一条从项目总览到实现细节的清晰路径。就像神话中阿里阿德涅的线团指引出迷宫，它让 AI 代理理解结构、找到任意函数、评估修改影响——所用 token 远少于直接读取源代码。

**量化效果**：在结构化评估中，使用本 skill 生成的输出通过了 **100% 的 AI 导航检查**，而不使用时仅为 59%。主要差距集中在：缺少修改风险分析、任务路由映射、横切关注点模式文档，以及 Agent Workflow 检查清单——这些恰恰是 token 节省效益最高的部分。

---

## 四层索引架构

```
L0  AGENTS.md          项目根部——地图、导航表、构建命令、
│                       架构约束、横切关注点模式、
│                       Agent Workflow 检查清单（80–150 行）
│
L1  INDEX.md           每个模块目录一份——公开 API、依赖关系、
│                       被依赖方（Fan-in）、修改风险、任务路由、
│                       接口契约、测试位置（约 50 行）
│
L2  文件头部           每个源文件 5–10 行——意图、公开接口、
│                       契约（@pre/@post/@throws）、副作用
│
L3  行内注释           仅用于非显而易见的逻辑——解释「为什么」，而非「做什么」
```

AI 代理从 L0 开始，按需逐层深入。

---

## 相比基础文档的核心差异

| 章节 | AI 解答的问题 | token 节省 |
|------|-------------|-----------|
| **Dependents（Fan-in）** | 谁调用了我？修改后什么会崩？ | 避免全项目 grep 来发现调用方 |
| **Modification Risk** | 这个改动安全吗？需要更新多少文件？ | 避免低估影响范围、遗漏被依赖方 |
| **Task Routing** | 任务 X 应该改哪个文件？ | 避免读错文件再回头找正确的 |
| **Interface Contract** | 我必须保持哪些不变量？ | 避免重读调用方来推断约束条件 |
| **Cross-Cutting Patterns** | 这个项目如何处理错误/日志/并发？ | 避免读 3-4 个源文件来推断全局模式 |

---

## 何时使用

**适用场景：**
- 启动新项目，希望从第一天就具备 AI 友好结构
- 对现有代码库进行 AI 可读性改造
- 创建或更新 `AGENTS.md`、`INDEX.md` 或 `llms.txt`
- 设计模块边界，并明确记录依赖关系和修改风险
- 让代码库对 AI 代理可导航，而无需通读整个项目

**跳过的情况：**
- 项目源文件少于约 5 个（一份轻量 `AGENTS.md` 即可）
- 一次性快速原型
- 单层代码库，无跨模块调用

---

## 如何触发

> **重要提示**：本 skill 覆盖的任务 Claude 通常能独立完成，因此可能不会自动触发。建议明确指定 skill 名称以获得最佳效果。

**单次使用：**
```
使用 ariadne-thread skill 为这个项目创建索引
```

**持久生效（推荐）**——在项目的 `CLAUDE.md` 中添加：
```markdown
## AI 导航
创建或更新 AGENTS.md、INDEX.md 或任何项目导航索引时，
请使用 ariadne-thread skill。
```

这样无论用户如何描述需求，AI 在处理索引工作时都会自动参考本 skill。

---

## 支持的语言

C++、TypeScript/JavaScript、Python、Go、Rust、Java/Kotlin——每种语言都有对应的目录布局、文件命名、L2 契约注释和构建命令约定。详见 [`references/language-adaptation.md`](references/language-adaptation.md)。

支持编译库、仅头文件库、Web 应用、monorepo 和子项目。

---

## 快速开始

**新项目：**
1. 确认语言生态 → 查阅 [`references/language-adaptation.md`](references/language-adaptation.md)
2. 设计目录结构与模块边界
3. 创建 `AGENTS.md`（全部 9 个章节，均不可省略）
4. 随创建各目录同步创建对应的 `INDEX.md`
5. 为文件添加意图头部（L2），公开 API 文件需包含契约注释

**现有项目（改造）：**
1. 先建立索引，再考虑重构——按现状文档化，用 ⚠ 标记问题
2. 从 Fan-in 最高的模块开始（被依赖最多 = 导航收益最大）
3. 创建 L0（`AGENTS.md`） → L1（各模块 `INDEX.md`） → L2/L3
4. 对禁用命名的文件（`utils.*`、`helpers.*`、`types.*`）标记 `⚠ needs split`

详细步骤见 [SKILL.md](SKILL.md) 中的 Workflow 章节。

---

## 文件结构

```
ariadne-thread/
├── SKILL.md                      # 主技能说明
├── README.md                     # 英文版
├── README.zh-CN.md               # 本文件（中文版）
└── references/
    ├── index-templates.md        # L0/L1/L2 可复制模板
    ├── index-maintenance.md      # Tier A/B 维护触发表
    ├── language-adaptation.md    # 各语言约定
    ├── naming-api-conventions.md # 命名规则与 API 设计模式
    └── doc-standards.md          # 人类文档标准与 ADR 格式
```

---

## 文档双轨制

| 层级 | 文件 | 更新时机 | 过时影响 |
|------|------|---------|---------|
| **A — AI 运行时索引** | `AGENTS.md`、`INDEX.md`、文件头部 | 实时，与代码变更原子同步 | 严重——过时即导航错误 |
| **B — 人类文档** | `README.md`、`architecture.md`、ADR、API 文档 | 批量，在迭代结束时同步 | 适中——人类会注意到，AI 不受影响 |

两轨分离可降低每次代码变更的 AI 额外开销（具体降幅因项目规模而异），同时提升 Tier B 文档质量（批量更新比逐次修补更连贯）。
