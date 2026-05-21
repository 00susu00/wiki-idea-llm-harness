# 编译工作流

## thinking/ 管理

### 写入规则

- 会话中产生有价值思考 → 在 `thinking/` 下建或追加子目录
- 子目录名使用 kebab-case，代表主题
- 内容随意写：草稿、推理链、试错记录、对比分析均可
- 无来源要求，无格式要求

### thinking/ → projects/ 编译

当对话或思考在某个主题上彻底结束：

#### 阶段一：价值判断

**任务**：判断 thinking/ 中的思考内容是否合理、有价值，值得进入佐证核查。

**步骤**：
1. 读取 `thinking/<主题>/` 下的所有文件
2. 评估每条核心结论：
   - 逻辑是否自洽？
   - 是否对项目推进有增量贡献，而非简单重复 wiki/ 或 ideas/ 中已有内容？
   - 是否具有项目指导价值？
3. 判定结果：
   - **有价值** → 进入阶段二
   - **价值不足** → 留在 thinking/ 继续打磨，或提示用户是否删除
   - **明显错误** → 丢弃

#### 阶段二：佐证核查

**任务**：验证有价值的结论能否被 wiki/ 知识或 ideas/ 支撑。

**步骤**：
1. 提取通过阶段一的核心结论和推理链
2. 在 `wiki/` 和 `ideas/` 中查找佐证来源
3. 对每条结论判定：
   - **有佐证**（wiki/ 知识或 ideas/verified/ 支撑）→ 可编译
   - **部分佐证**（ideas/referenced/ 或 hypothesized/ 支撑）→ 可编译，但需标明佐证层级
   - **无佐证** → 留在 thinking/，不编译
4. 编译有佐证的结论为项目文章，放入 `projects/<project>/` 对应阶段目录
5. 新文章的来源标注包含引用的 wiki 文档和 ideas 文件
6. 删除 `thinking/<主题>/` 子目录

**关键约束**：`thinking/` 内容本身不作为可信来源。只有通过价值判断 + 佐证核查双重验证的内容才能进 projects/。

## projects/ 编译流程

### 从 ideas + wiki 综合

当用户要求综合某个项目的规律：

1. 读取 `ideas/<project>/` 下所有置信度目录的文件
2. 读取 `wiki/` 中与该项目主题相关的文档
3. 结合两个来源，综合出项目规律和思考
4. 按项目当前阶段写入 `projects/<project>/<phase>/`
5. 更新项目状态文件

### 元数据格式

```yaml
---
title: "标题（用中文）"
project: <项目名>
phase: <当前阶段>
date: YYYY-MM-DD
last_updated: YYYY-MM-DD
tags:
  - "标签"
wiki_refs:
  - "[[Wiki-Article|wiki 文章]]"
idea_refs:
  - path: ideas/<project>/verified/notes.md
  - path: ideas/<project>/hypothesized/idea.md
---
```

## 健康检查

- **thinking/ 停留检查**：扫描 thinking/ 下存在超过 30 天的子目录，提示用户是否编译或清理
- **thinking/ 编译残留**：检查已编译的主题是否仍保留在 thinking/ 中
- **ideas/ 哈希变化**：检测 ideas/ 文件的哈希变化，提示哪些 projects 文章可能需要更新
- **项目阶段停滞**：项目在 exploring 或 planning 阶段停留超过 60 天时提示
