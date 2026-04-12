---
name: skill-dual-optimizer
description: 优化和重构现有 skill。用于检查目标 skill 的触发描述、SKILL.md 工作流、确认门槛、渐进式披露，以及 references/scripts/assets 的组织方式。当用户提到"优化 skill""检查 skill 质量""改进某个 skill""重构技能说明"，或直接说明要优化哪些方面时使用。默认先审查、再出计划、等待用户确认后再修改目标 skill。不适用于从零创建新 skill（请使用 skill-creator）。
---

# Skill Dual Optimizer

先审查，再规划，最后在确认后修改。

## Gotchas

- 先输出完整审查结论，等用户确认后再开始改文件（审查与修改严格分步）
- 改动面与用户确认的计划一致，额外发现单独列为"附加建议"
- 只读取当前审查步骤需要的 reference，按 Step 2 的触发条件逐步加载
- 用户指定了优化方向时，先围绕该方向完成，再补充少量高价值建议
- 不要在没确认前修改目标 skill
- 审查 skill-dual-optimizer 自身时，只做一轮审查，不要递归应用自己的优化流程

## 设计模式

本 skill 主要采用：
- **Reviewer**：先判断目标 skill 的问题，再给诊断
- **Inversion**：先出计划并等待确认，不直接改文件
- **Generator（轻度）**：在用户确认后，输出更清晰的 skill 结构或工作流

## 工作流

复制此清单并跟踪进度：

```text
优化进度：
- [ ] 步骤 1：Scope（确定范围）
- [ ] 步骤 2：Review（审查目标 skill）
- [ ] 步骤 3：Plan（输出优化计划并等待确认）
- [ ] 步骤 4：Implement（确认后实施）
- [ ] 步骤 5：Verify（校验结果）
```

### Step 1: Scope（确定范围）

先确认目标 skill 和本次优化范围。

- 优先采用用户明确提出的优化方向，例如触发词、description、结构拆分、脚本化、确认流程。
- 如果用户只说"优化这个 skill"，先做完整审查，再给出分优先级计划。
- 如果用户明确只要快速诊断（不要修改），跳过 Step 4-5，只输出 Step 3 的审查结论部分。
- 如果目标 skill 不明确，只问一个最短问题，不要一次追问很多细节。
- 如果目标 skill 路径不存在或 SKILL.md 为空，立即告知用户并终止，不要猜测内容。

### Step 2: Review（审查目标 skill）

先读目标 skill 的 `SKILL.md`，再按需读取它直接链接的 `references/`、`scripts/`、`assets/`。

- 默认使用 [references/review-checklist.md](references/review-checklist.md) 做审查基线。
- 默认再结合 [references/skill-design-review-framework.md](references/skill-design-review-framework.md) 判断模式是否匹配、设计是否合理。
- 需要判断最佳实践取舍时，再读取 [references/技能创作最佳实践 - Claude API Docs.md](<references/技能创作最佳实践 - Claude API Docs.md>)。
- 当 checklist 发现 ≥2 个〔A〕类问题时，再读取 [references/prompt-engineering-principles.md](references/prompt-engineering-principles.md) 做深度 PE 审查。
- 当用户要求打分或量化评估时，再读取 [references/quality-rubric.md](references/quality-rubric.md)。
- 不要为了"审查完整"而把无关 reference 全部读入上下文。

快速判断（先于详细检查）：

- 当前 skill 属于哪种主模式（Tool Wrapper / Generator / Reviewer / Inversion / Pipeline），模式是否匹配任务
- description 是否能独立触发，无需阅读 SKILL.md 正文
- 工作流是否有硬性确认门槛，是否存在必须确认却没卡住的步骤

详细检查项见 [references/review-checklist.md](references/review-checklist.md)。

如果用户只要求微调某一部分（例如只改 description、只补 references、只修确认门槛），优先做局部审查，不要擅自把任务升级成整 skill 重构。

### Step 3: Plan（输出优化计划并等待确认）

先给诊断，再给计划，不要直接改文件。

问题排序规则：
1. 先修触发失败、确认缺失、流程不可执行的问题
2. 再修结构臃肿、重复内容、资源组织混乱的问题
3. 最后处理措辞润色、示例补充、可读性增强

输出格式参考 [references/sample-review-output.md](references/sample-review-output.md)。输出必须包含两个部分：

```markdown
# Skill 审查结论

## 模式判断
- 主模式：...
- 次模式：...
- 当前判断：模式匹配 / 模式错位 / 模式不清

## 高优先级
- [问题] 影响触发、正确性或执行稳定性

## 中优先级
- [问题] 影响可维护性、可复用性或上下文成本

## 低优先级
- [问题] 体验提升项，可选

# 优化计划
1. 修改 [目标文件路径]
   - 变更内容：
   - 原因：
   - 影响范围：仅本文件 / 需同步修改 [其他文件]
   - 是否受用户指定方向驱动：是/否

请确认是否按以上计划执行。
```

规则：

- 如果用户明确给了优化方向，先围绕这些方向出计划，再补充少量高价值附加建议。
- 如果发现超出本次范围的大问题，单独列为"额外建议"，不要偷偷扩大改动面。
- 未获得确认前，不要修改目标 skill。

### Step 4: Implement（确认后实施）

在用户确认后，再修改目标 skill。

- 优先改动计划中已确认的文件。
- 尽量小步重构；仅在确有收益时拆分新的 `references/`、`scripts/`、`assets/`。
- 如果新增引用文件，确保它们都直接从目标 `SKILL.md` 链接，避免深层跳转。
- 保留用户原有有效内容；只删除重复、失效或与新流程冲突的部分。

### Step 5: Verify（校验结果）

改完后至少完成这些校验：

- frontmatter 只包含 `name` 和 `description`
- `name`、目录名、触发语义一致
- `description` 能独立表达触发条件
- `SKILL.md` 主体比改动前更短、更清晰，且工作流可执行
- 新增 `references/` 是否只承载细节，没有和 `SKILL.md` 重复大段内容
- 如有"先审查后确认再修改"的门槛，是否在说明里写清楚

最后向用户汇报：

```markdown
## 优化完成
- 已修改：[文件列表]
- 已落实：[优化方向列表]
- 遗留风险：[风险列表，无则写"无"]
```
