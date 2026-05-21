# 会话沉淀与 thinking 工作区

## 两个目录的分工

| | `thinking/` | `wiki/queries/` |
|---|---|---|
| 位置 | 项目根目录，与 raw/、wiki/ 平级 | wiki/ 内，与 concepts/、practices/ 平级 |
| 性质 | 模型草稿纸，活跃主题的工作区 | 会话中有价值的、经来源核查的事实性结论 |
| 检索 | 不可检索 | 可检索，作为 wiki 知识库的一部分 |
| 来源要求 | 无，随意写 | 必须引用 wiki/ 或 raw/ 中的具体来源，且来源必须核查 |
| 可信度 | 不标注 | 视同事实（经来源核查） |
| 生命周期 | 主题结束后编译进 wiki，子目录删除 | 长期保留 |
| 与 concepts/practices 关系 | 编译后按性质进入对应目录 | 平级关系，不升级 |

## thinking/ 工作区

### 目录结构

`thinking/` 按主题建子目录，每个子目录是一个活跃的思考工作区：

```text
thinking/
├── harness-vs-finetuning/     # 活跃主题
│   ├── draft-analysis.md      # 基于对话和 raw 的分析草稿
│   ├── comparison-table.md    # 对比笔记
│   └── ...
├── agent-memory-architecture/ # 活跃主题
│   ├── design-notes.md
│   └── ...
└── (已完成的主题编译进 wiki 后删除)
```

### 写入规则

- 会话中产生有价值思考 → 在 `thinking/` 下建或追加子目录
- 子目录名使用 kebab-case，代表主题
- 内容随意写：草稿、推理链、试错记录、对比分析均可
- 无来源要求，无格式要求

### 主题完成 → 编译进 wiki

当对话或思考在某个主题上彻底结束：

1. 以 `thinking/<主题>/` 下的内容为素材
2. 回 `raw/` 中查找佐证来源
3. 编译为 wiki 文章，按内容性质选择目标目录：
   - 理论框架、概念定义 → `wiki/concepts/`
   - 实践指南、部署经验 → `wiki/practices/`
   - 会话研究结论（有来源核查）→ `wiki/queries/`
4. 新文章的 `raw_sources` 指向佐证的 raw 文件
5. 删除 `thinking/<主题>/` 子目录

编译时 `thinking/` 内容本身不作为可信来源，其结论必须被 `raw/` 素材支撑才能进 wiki。

## wiki/queries/ 会话沉淀

### 定位

`queries/` 与 `concepts/`、`practices/` 是平级关系，不是升级关系。区别只在产出路径：

- `concepts/`：从 raw 编译 → "是什么"
- `practices/`：从 raw 编译 → "怎么做"
- `queries/`：从会话研究+来源核查 → "事实"

### 写入清单（严格按顺序执行）

- [ ] **步骤 1**：判断是否产生值得沉淀的内容（满足任一）：
  - 跨文档综合推理（引用 2+ 篇 wiki 文档）
  - 设计决策或方案对比，有来源支撑
  - 回答了具有通用价值的问题，有来源支撑
  - 产生了独立于对话上下文的数据洞察，有来源支撑
- [ ] **步骤 2**：来源核查 — 每条核心结论必须指向 wiki/ 或 raw/ 中的具体文档，模型凭记忆得出的结论不算
- [ ] **步骤 3**：写入文件：
  - 存入 `wiki/queries/` 目录
  - 文件名使用 kebab-case，如 `Harness-vs-Fine-tuning.md`
  - 元数据使用下方规范
  - 在 `INDEX.md` 的"会话沉淀"部分创建入口
  - 包含至少 2 个指向 wiki 其他文章的 Wikilink

### 元数据格式

```yaml
---
title: "标题（用中文）"
source: "WikiLLM 会话沉淀"
date: YYYY-MM-DD
tags:
  - "标签"
sources:
  - "[[Source-Article|来源文章]]"
session_trigger: |
  触发本次沉淀的原始问题或讨论话题
---
```

### Q&A 回答格式

当用户针对 Wiki 发起查询时：

1. **检索范围**：`raw/` + `wiki/`（concepts、practices、queries），不含 `thinking/`
2. **回答格式**：回答后提供"参考依据"，列出支撑结论的 wiki 文档：
   ```markdown
   ## 参考依据
   - [[Article-Title|文章标题]]
   - [[Another-Article|另一篇文章]]
   ```
