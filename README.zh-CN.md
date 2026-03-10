# Ariadne Thread

> 指导创建 AI 友好的项目结构，通过渐进式索引（progressive disclosure）、模块化架构和意图导向的代码，让 AI 代理高效导航代码库。

## 功能概述

Ariadne Thread 帮助你将代码库改造成 AI 代理易于理解和安全修改的结构。核心思想如同神话中阿里阿德涅的线团——提供一条从项目概览到实现细节的清晰路径，使 AI 能够：

- **理解项目结构**
- **快速定位任意函数**
- **评估任何修改的影响范围**

而无需读取整个代码库。

## 核心目标

| 目标 | 说明 |
|-----|------|
| **导航效率** | 最小化查找正确代码所消耗的 token |
| **修改安全** | 通过显式契约和依赖可见性，提高正确修改的概率 |

## 四层索引架构

```
L0  项目根索引    AGENTS.md (80–150 行，优先简短)
│                  项目地图、导航表、构建命令、横切关注点模式、Agent Workflow
│
L1  模块索引      每个目录的 INDEX.md (~50 行)
│                  目的、公开 API、依赖关系、契约、任务路由、修改风险、测试映射
│
L2  文件意图      文件头部 (~5–10 行/文件)
│                  本文件职责、公开接口、契约、副作用
│
L3  行内说明      仅对非显而易见的逻辑添加注释
                  解释「为什么」，而非「做什么」
```

AI 代理从 L0 开始，按需逐层深入。

## 主要特性

- **语言无关**：适用于 TypeScript、C++、Python、Go、Rust、Java 等
- **泛化设计**：适配 Web 应用、库、monorepo — 原则优先于固定布局
- **渐进式披露**：只揭示当前任务所需的信息，减少 token 消耗
- **双轨文档**：Tier A（AI 运行时代码库索引）与 Tier B（人类文档）分离维护
- **模块设计原则**：单一职责、清晰的依赖方向、小而聚焦的文件

**何时不用**：极简单文件项目、快速原型或源文件少于 ~5 个时，仅保留轻量 AGENTS.md 即可。

## 适用场景

- 启动新项目并设计 AI 友好的结构
- 对现有代码库进行重构与索引
- 添加项目导航索引（AGENTS.md、INDEX.md、llms.txt 等）
- 设计模块边界与依赖关系
- 创建或维护 AI 代理可用的代码库说明

## 触发关键词

当提及以下概念时，可使用本 Skill 指导工作：

- "AI-friendly project" / AI 友好项目
- "progressive disclosure" / 渐进式披露
- "project index" / 项目索引
- "codebase navigation" / 代码库导航
- "ariadne"

## 文档结构

```
ariadne-thread/
├── SKILL.md                    # 主技能说明（四层索引、何时使用、工作流）
├── README.md                   # 英文版
├── README.zh-CN.md             # 本文件（中文版）
└── references/
    ├── index-templates.md      # L0/L1/L2 可复制模板
    ├── naming-api-conventions.md # 命名与 API 约定
    ├── doc-standards.md        # 文档标准与 ADR 格式
    ├── language-adaptation.md  # 各语言约定（C++、TS、Python、Go、Rust、Java）
    └── index-maintenance.md   # Tier A/B 维护触发表
```

## 快速开始

**新项目：**

1. 设计目录结构与模块边界
2. 创建 `AGENTS.md`（项目总览、导航表、横切模式）
3. 为各模块创建 `INDEX.md`
4. 为文件添加意图头部（L2），公开 API 文件需包含契约注释

**存量项目：**

1. 先建立索引，再考虑重构
2. 从 Fan-in 最高（被依赖最多）的模块开始
3. 创建 L0 → L1，再补充 L2/L3

详细步骤见 [SKILL.md](SKILL.md) 中的 Workflow 章节。

## 许可证

按项目仓库约定使用。
