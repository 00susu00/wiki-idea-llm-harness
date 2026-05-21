---
name: ideallm
description: Advances personal ideas and project insights by synthesizing ideas/ and wiki/ knowledge. Use when working on projects, classifying ideas, managing thinking drafts, or asking project-level questions.
license: MIT
metadata:
  version: "1.0"
  author: WikiLLM Project
---

# IdeaLLM Skill

基于可信知识（wiki/）和个人想法（ideas/），推进项目思考和规律综合。

## 系统架构

```text
ideas/                          # 人的动态输入
└── <project>/
    ├── verified/               # 已验证：你确认过的经验
    ├── referenced/             # 引述：他人观点
    ├── hypothesized/           # 猜想：直觉推断
    ├── inspired/               # 灵感：碎片化想法
    └── *.md                    # 未分类

thinking/                       # 模型草稿（按主题分，临时）
└── <topic>/                   # 完成后编译进 projects/

projects/                       # 项目规律综合
└── <project>/
    ├── exploring/              # 探索：方向未定
    ├── planning/               # 规划：方案设计中
    ├── advancing/              # 推进：执行中
    ├── validating/             # 验证：结论待确认
    └── concluded/              # 定论：已确定
```

### ideas/ 置信度与引用

| 子目录 | 含义 | 编译进 projects 时 | 引用方式 |
|--------|------|-------------------|----------|
| `verified/` | 你确认过的经验 | 直接作为依据 | "已验证：……" |
| `referenced/` | 他人观点 | 可引用，需注明来源 | "据 [[引述来源]] 观点：……" |
| `hypothesized/` | 直觉推断 | 可引用，需标明猜想 | "当前猜想认为：……" |
| `inspired/` | 碎片化灵感 | 仅作思路参考 | "探索方向：……" |
| 未分类 | 待归类 | 先询问用户归类，再决定 | 按归类结果处理 |

### projects/ 阶段流转

```text
exploring ⇄ planning ⇄ advancing ⇄ validating ⇄ concluded
```

## 功能一览

### 1. ideas/ 管理（人的动态输入）

- 按项目建子目录，每个项目下四个置信度分类：verified/referenced/hypothesized/inspired
- 未分类文件：Linting 时扫描，向用户引导归类（"你验证过吗？""别人说的？"）
- ideas/ 文件是动态的，用户会持续修改
- ideas/ 哈希变化时提示相关 projects 文章可能需要更新

### 2. thinking/ 管理（对话 → 草稿 → projects）

- 对话中产生有价值思考 → 写入 thinking/ 按主题分的子目录
- 话题结束后进入两阶段编译：
  - **阶段一：价值判断** — 逻辑是否自洽？对项目推进有增量贡献？值得保留？
  - **阶段二：佐证核查** — 在 wiki/ 和 ideas/ 中查找佐证，有佐证才编译进 projects
- 佐证来源分级：wiki/ 或 ideas/verified/ → 可编译；ideas/referenced/ 或 hypothesized/ → 需标明层级；无佐证 → 留在 thinking/
- 编译完成后删除 thinking/ 子目录

### 3. 项目 Q&A

- 检索范围：wiki/ + ideas/，不含 thinking/
- 回答后提供"参考依据"，区分来源层级（知识库/已验证/引述/猜想）
- 引用规则：wiki 作事实依据，verified 作经验依据，referenced 需注明来源，hypothesized 标明猜想，inspired 仅作方向参考

### 4. projects/ 项目综合（ideas + wiki → 项目规律）

- 五个阶段目录：exploring → planning → advancing → validating → concluded
- 阶段可前进也可回退，回退时记录原因
- 每个项目有 STATUS.md 记录当前阶段和阶段历史
- exploring 引用灵感/猜想，planning 引用已验证/引述，advancing 引用已验证+wiki，validating 需佐证，concluded 确定结论
- concluded/ 中的结论可被其他项目引用

### 5. Linting（健康检查）

- ideas/ 未分类文件扫描 + 归类引导
- ideas/ 哈希变化 → 提示相关 projects 文章可能需要更新
- thinking/ 停留检查（超过 30 天提示）
- thinking/ 编译残留检查
- 项目阶段完整性检查（五个目录 + STATUS.md）
- 项目阶段停滞检查（exploring/planning 超 60 天提示）
- 低置信度引用检查（advancing/validating/concluded 中引用 hypothesized/inspired 时需限定语）

## 任务路由（首先阅读本节！）

在执行任何操作前，先判断当前任务属于以下哪个场景：

| 场景 | 判断标准 | 跳转至 |
|------|----------|--------|
| **想法归类** | ideas/ 中有未分类文件需要归类 | [references/ideas.md](references/ideas.md) |
| **项目 Q&A** | 用户针对项目提问，需要结合知识和想法推理 | [references/ideas.md](references/ideas.md) Q&A 部分 |
| **thinking 管理** | 对话中产出有价值思考，或话题结束需要编译 | [references/workflows.md](references/workflows.md) |
| **项目综合** | 需要将 ideas + wiki 综合为项目规律 | [references/projects.md](references/projects.md) |

## 快速开始

### 核心逻辑

- 用户在 ideas/ 下写笔记和想法，模型负责归类和推进
- Q&A 检索 wiki/ + ideas/，不含 thinking/
- thinking/ 是模型的草稿纸，完成后编译进 projects/
- projects/ 按阶段组织，阶段可前进也可回退
- 适配工具：Obsidian（IDE 前端）

### 与 wikillm 的关系

- **wikillm**：处理纯可信知识（raw → wiki），Q&A 只查 wiki/
- **ideallm**：推进个人想法（ideas + wiki → projects），Q&A 查 wiki/ + ideas/
- thinking/ 共用：wikillm 上下文编译进 wiki/，ideallm 上下文编译进 projects/

### 详细文档

- **ideas 管理**：见 [references/ideas.md](references/ideas.md)
- **编译工作流**：见 [references/workflows.md](references/workflows.md)
- **项目综合**：见 [references/projects.md](references/projects.md)
- **常见错误**：见 [references/errors.md](references/errors.md)
