# 项目综合

## 目录结构

```text
projects/
└── <project>/
    ├── exploring/              # 探索：方向未定
    ├── planning/               # 规划：方案设计中
    ├── advancing/              # 推进：执行中
    ├── validating/             # 验证：结论待确认
    └── concluded/              # 定论：已确定
```

每个项目目录下必须包含五个阶段子目录。项目目录名使用 kebab-case。

## 阶段定义

| 阶段 | 含义 | 内容特征 | 主要引用 |
|------|------|----------|----------|
| `exploring/` | 探索：调研中，方向未定 | 问题、可能性、初步观察 | ideas/inspired/、ideas/hypothesized/、wiki/ |
| `planning/` | 规划：方向已定，方案设计中 | 方案设计、决策记录、可行性分析 | ideas/verified/、ideas/referenced/、wiki/ |
| `advancing/` | 推进：执行中，持续产出 | 执行记录、阶段性发现、遇到的问题 | ideas/verified/、wiki/ |
| `validating/` | 验证：结论待确认 | 待确认结论、验证方案、需要补充的佐证 | wiki/、ideas/verified/ |
| `concluded/` | 定论：项目结论已确定 | 确定的项目规律和结论 | 全部来源 |

## 阶段流转

```text
exploring ⇄ planning ⇄ advancing ⇄ validating ⇄ concluded
```

阶段可以前进，也可以回退。回退的典型场景：

| 回退 | 触发条件 |
|------|----------|
| planning → exploring | 规划中发现方向不可行，需要重新探索 |
| advancing → planning | 执行中发现方案需要重大调整 |
| validating → advancing | 验证中发现结论不成立，需要继续推进 |
| concluded → validating | 新信息出现，原有结论需要重新验证 |

### 阶段变更规则

- 阶段变更时，不删除已有阶段目录中的文件
- 新内容写入当前阶段目录
- 项目元数据中的 `phase` 字段更新为当前阶段
- 如果阶段回退，在对应文件中记录回退原因

## 项目元数据

每个项目有一个状态文件 `projects/<project>/STATUS.md`：

```yaml
---
project: <项目名>
phase: advancing
created: YYYY-MM-DD
last_updated: YYYY-MM-DD
phase_history:
  - phase: exploring
    date: YYYY-MM-DD
  - phase: planning
    date: YYYY-MM-DD
  - phase: advancing
    date: YYYY-MM-DD
---
```

## concluded 阶段的内容引用

`concluded/` 中的项目结论可以被其他项目引用：
- 其他项目在 exploring 阶段可以引用已定论的项目结论
- 引用格式：`据 [[project/concluded/xxx|项目名：结论]] ……`

## Linting 检查

- [ ] **阶段完整性**：每个项目是否包含五个阶段子目录
- [ ] **项目状态文件**：每个项目是否有 STATUS.md，phase 是否与内容匹配
- [ ] **阶段停滞**：项目在 exploring 或 planning 停留超过 60 天时提示
- [ ] **concluded 可引用性**：concluded/ 中的结论是否被其他项目正确引用
