---
name: wikillm
description: Compiles raw documents into a structured, cross-linked Chinese Wiki knowledge base. Use when ingesting raw materials, answering questions about the wiki, or maintaining the wiki structure.
license: MIT
metadata:
  version: "3.0"
  author: WikiLLM Project
---

# WikiLLM Skill

基于可信来源（raw/）编译事实知识库。

## 系统架构

```text
wikillm/
├── raw/        # 【输入层】事实来源（不可变）
├── ideas/      # 人的动态输入（按项目/置信度分，可变）
├── thinking/   # 【草稿层】模型思考（按主题分，临时，完成后编译并删除）
├── wiki/       # 【输出层】编译后的知识库（可检索）
│   ├── concepts/   # 从 raw 编译 → "是什么"
│   ├── practices/  # 从 raw 编译 → "怎么做"
│   ├── queries/    # 会话研究 + 来源核查 → "事实"
│   ├── visual/     # 可视化内容
│   └── assets/     # 图像和资源文件
└── projects/   # 从 ideas + wiki 综合的项目规律（ideallm skill 处理）
```

### 三层隔离

| 层 | 目录 | 可检索 | 来源要求 | 生命周期 |
|---|---|---|---|---|
| 输入层 | `raw/` | 编译时读取 | 外部素材，只读 | 永久 |
| 草稿层 | `thinking/` | 不检索 | 无要求 | 主题完成后编译进 wiki 并删除 |
| 输出层 | `wiki/` | Q&A 和 Linting 时检索 | 必须有来源支撑 | 永久 |

## 功能一览

### 1. 增量编译（raw/ → wiki/）

- 扫描 raw/ 目录，哈希对比检测新增/修改文件
- 多模态解构：大文档完整阅读、图片 OCR 转 Mermaid
- 非线性重构重写为中文化 wiki 文章
- 按内容性质分目录：concepts/（是什么）、practices/（怎么做）
- 资源同步：raw/images/ → wiki/assets/

### 2. thinking/ 管理（对话 → 草稿 → wiki）

- 对话中产生有价值思考 → 写入 thinking/ 按主题分的子目录
- 话题结束后进入两阶段编译：
  - **阶段一：价值判断** — 逻辑是否自洽？是否有增量贡献？值得保留？
  - **阶段二：佐证核查** — 回 raw/ 查找佐证，有佐证才编译进 wiki
- 佐证通过 → 编译进 wiki/（concepts/practices/queries/）
- 无法佐证 → 留在 thinking/，不编译
- 编译完成后删除 thinking/ 子目录

### 3. Q&A（基于 wiki 的知识查询）

- 检索范围：raw/ + wiki/，不含 thinking/
- 回答后提供"参考依据"，列出支撑结论的 wiki 文档

### 4. queries/ 会话沉淀

- Q&A 中产生有价值且经来源核查的结论 → 写入 wiki/queries/
- 必须有 wiki/ 或 raw/ 中的具体来源支撑，模型凭记忆的不算
- 视同事实，与 concepts/practices 平级

### 5. 网络化链接

- Wikilink 格式规范（kebab-case 文件名）
- 双链注入（Glossary 术语自动包裹）
- 反向链接（文末"相关研究"模块）
- 动态索引更新

### 6. Linting（健康检查）

- 术语一致性检查
- 孤岛页面扫描
- thinking/ 停留检查（超过 30 天提示）
- thinking/ 编译残留检查
- queries/ 来源核查
- raw/ 更新时发布补丁

## 任务路由（首先阅读本节！）

在执行任何操作前，先判断当前任务属于以下哪个场景：

| 场景 | 判断标准 | 跳转至 |
|------|----------|--------|
| **增量编译** | `raw/` 目录有新增或修改的文件需要编译到 `wiki/` | [references/workflows.md](references/workflows.md) |
| **thinking 编译** | `thinking/` 下某主题完成，需要编译进 `wiki/` | [references/workflows.md](references/workflows.md) 阶段 3.5 |
| **Q&A** | 用户针对 Wiki 内容提出问题（询问、咨询、探讨） | [references/qa.md](references/qa.md) |
| **Linting** | 需要检查 Wiki 的一致性、修复孤岛页面等 | [references/errors.md](references/errors.md) |

> **注意**：`ideas/` 和 `projects/` 的管理由 ideallm skill 处理，不属于本 skill 范围。

## 快速开始

### 核心逻辑

- 人工不直接编写 Wiki，仅负责投放素材和发起查询
- LLM 负责理解、重写、链接与维护
- `thinking/` 是模型的草稿纸，Q&A 时不检索
- `wiki/queries/` 的内容必须有来源核查，视同事实
- 可信度由目录位置决定，不使用额外元数据标注
- 适配工具：Obsidian（IDE 前端）

### 详细文档

- **编译工作流**：见 [references/workflows.md](references/workflows.md)
- **Q&A 与会话沉淀**：见 [references/qa.md](references/qa.md)
- **质量标准**：见 [references/standards.md](references/standards.md)
- **常见错误**：见 [references/errors.md](references/errors.md)
