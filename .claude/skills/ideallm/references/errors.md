# 常见错误与避免方法

## 错误 1：thinking/ 内容当作可信来源引用

**表现**：编译 projects 文章时，把 `thinking/` 中的结论直接作为事实依据，未经 wiki/ 或 ideas/verified/ 支撑

**避免**：`thinking/` 内容不作为可信来源。编译时必须有 `wiki/` 知识或 `ideas/verified/` 支撑，否则保留在 thinking/ 中不编译。

## 错误 2：thinking/ 子目录编译后未删除

**表现**：主题已编译进 projects，但 `thinking/` 下的子目录仍然存在

**避免**：编译完成后必须删除对应的 `thinking/<主题>/` 子目录

## 错误 3：未分类 ideas 文件直接使用

**表现**：编译 projects 时引用了 ideas/ 项目根目录下的未分类文件，未先引导用户归类

**避免**：遇到未分类文件时，先向用户确认归类，再决定如何使用

## 错误 4：低置信度 ideas 当作事实引用

**表现**：在 projects/advancing/ 或 projects/validating/ 中，把 ideas/hypothesized/ 或 ideas/inspired/ 的内容当事实引用

**避免**：hypothesized 内容必须标明"当前猜想认为"，inspired 内容只能作为"探索方向"，不能作为结论依据

## 错误 5：阶段回退时不记录原因

**表现**：项目阶段从 validating 回退到 advancing，但没有记录为什么回退

**避免**：每次阶段回退时，必须在 STATUS.md 的 phase_history 中记录回退原因

## 错误 6：混淆 wikillm 和 ideallm 的 Q&A

**表现**：用户问项目相关问题，却只检索 wiki/，忽略了 ideas/ 中的个人经验；或者用户问纯知识问题，却检索了 ideas/ 引入主观内容

**避免**：判断问题的性质——纯知识问题用 wikillm（只查 wiki/），项目相关问题用 ideallm（查 wiki/ + ideas/）

## 错误 7：projects/ 跨阶段引用混乱

**表现**：advancing/ 阶段的文章大量引用 exploring/ 阶段的早期草稿，而 exploring/ 中的内容已被后续阶段推翻

**避免**：优先引用当前或后续阶段的内容。引用早期阶段内容时，确认其是否仍成立

## 总体执行清单

* [ ] **来源层级确认**：引用的 ideas/ 文件是否在正确的置信度子目录？
* [ ] **未分类检查**：是否有未分类的 ideas/ 文件需要归类？
* [ ] **thinking/ 清理**：是否有已编译但未删除的 thinking/ 子目录？
* [ ] **项目状态**：projects/ 中的项目 STATUS.md 是否与实际阶段匹配？
* [ ] **Q&A 范围**：当前问题是纯知识问题还是项目问题？

## Linting 检查清单

- [ ] **thinking/ 停留检查**：存在超过 30 天的子目录，提示编译或清理
- [ ] **thinking/ 编译残留**：已编译的主题是否仍保留在 thinking/ 中
- [ ] **ideas/ 未分类扫描**：项目根目录下是否有未分类 .md 文件
- [ ] **ideas/ 哈希变化**：ideas/ 文件哈希变化时，提示相关 projects 文章可能需要更新
- [ ] **阶段完整性**：每个项目是否包含五个阶段子目录和 STATUS.md
- [ ] **项目阶段停滞**：exploring 或 planning 停留超过 60 天时提示
- [ ] **低置信度引用检查**：advancing/validating/concluded 中引用 hypothesized/inspired 时是否有适当限定语
