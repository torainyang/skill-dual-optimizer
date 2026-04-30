# Skill Dual Optimizer

审查并优化日常 Skill 的上下文执行可靠性和认知决策质量。

核心流程：**审查 → 计划 → 确认 → 实施 → 校验**。

## Features

- **5 种设计模式识别**：Tool Wrapper / Generator / Reviewer / Inversion / Pipeline
- **2 维度审查框架**：上下文与执行可靠性 / 认知与决策质量
- **3 档评分体系**：优秀 / 合格 / 需修复 + 综合定性（生产级/原型级/玩具级）
- **快速失败门控**：Quick-Fail Flags 命中时跳过深度审查
- **PE 原则库**：H1-H4 硬性原则 + S1-S4 软性指引
- **正向示例驱动**：Generator 完整示例 + Tool Wrapper 差异要点

## File Structure

```
skill-dual-optimizer/
├── SKILL.md                          # 主指令文件（LLM 读取入口）
├── version.json                      # 版本与元数据
├── README.md                         # 本文件（人类阅读）
└── references/
    ├── prompt-engineering-principles.md  # PE 原则库（H1-H4 + S1-S4）
    ├── quality-rubric.md             # 3 档评分体系 + Quick-Fail Flags
    ├── sample-review-output.md       # 审查输出示例
    └── skill-design-patterns.md      # 增量设计模式参考
```

## Usage

在 CodeFuse 中安装后，对话中说：

- "优化这个 skill"
- "检查 skill 质量"
- "帮我改进 XXX skill"
- "重构技能说明"

即可触发。

## Attribution

本 skill 基于 [ant_skill_center/skill-optimizer v1.2](https://code.alipay.com/ant_skill_center/skill-optimizer)（作者：悟鸣）重构而来。

v1.3–v2.0 的审查框架、reference 文件、SKILL.md 完全重写及 README 由 **Torain** 完成。

## License

Internal use. Derivative of `ant_skill_center/skill-optimizer`.

---

## Changelog

### v2.0 (2026-04-25) — 定位回归：日常Skill，聚焦执行可靠性

**框架精简**：
- 四维度砍为两维度：D1 上下文与执行可靠性 + D2 认知与决策质量
- 砍掉 D1 架构与业务（原子性/领域耦合/可组合性）—— 日常Skill不需要审"该不该存在"
- 砍掉 D4 工程交付（Token经济学/安全边界/可观测性）—— 个人Skill不关心这些
- 有用的检查点（引用文件健康）保留在 D1，异常兜底降级为 Gotchas 按需触发

**输出格式简化**：
- 砍掉评分矩阵（/10分制与3档制自相矛盾）
- 保留核心定性 + 问题清单〔执行问题/输出问题〕 + 优化计划
- Quick-Fail 删除"薄壳套娃"和"无兜底策略"（不适用日常场景）

### v1.9 (2026-04-25) — 架构级重构：四维度审查体系

**审查框架升级**：
- 三原则（先全局/精简优先/先找病）替换为四维度框架：
  - D1 架构与业务：原子性、领域知识耦合、可组合性
  - D2 上下文与感知：注意力稀疏、信噪比耐受、中间遗忘
  - D3 认知与决策：推理衰减、业务纠错、幻觉与遵从
  - D4 工程交付：Token 经济学、异常兜底、可观测性、安全边界

**quality-rubric 同步升级**：
- 评分维度从 5 维（触发/工作流/结构/防护/输出）改为 4 维匹配新框架
- 综合判定术语从 A/B/C/D 改为生产级/原型级/需修复/玩具级
- Quick-Fail Flags 新增 2 条：薄壳套娃、无异常兜底策略

**审查输出格式升级**：
- 新增"核心定性"（一句话定性可用性级别 + 最大硬伤）
- 新增 4 维度 10 分制评分矩阵
- 新增"致命风险"章节（高并发/真实业务流中会崩溃的节点）

### v1.8 (2026-04-25) — 自审第二轮：注意力/幻觉/效率/遵从性

**SKILL.md 结构重排**：
- 三原则移到文件最前（高注意力区），输出模板紧随 Gotchas
- 五原则精简为三原则：P3（去重）+ P4（原则优于穷举）合并为 P2（精简优先）；P2（LLM vs 脚本）降级为 Gotchas 条目
- 总规则数从 10+ 降至 ≤7

**幻觉风险修复**：
- P1 "先算总 token" 改为 "先估算总字数"（LLM 无法可靠计算 token）
- quality-rubric 0-5 分制改为 3 档等级制（优秀/合格/需修复），取消加权计算公式

**执行效率提升**：
- Step 2 新增门控：Quick-Fail 命中 → 标记"快速失败"→ 跳过深度审查直接进 Step 3
- 按需加载触发条件从模糊描述改为明确行为触发

**遵从性改善**：
- prompt-engineering-principles.md 删除 H4/S1 死条目（"已合并"标注改为直接删除）
- H4→H5 重编号，S1→S2 重编号，保持编号连续

### v1.7 (2026-04-25) — 自审第一轮：负担过重/走过程/遵从性

**最大改动——692行文档蒸馏**：
- 删除 `references/技能创作最佳实践 - Claude API Docs.md`（692行 API 文档转储）
- 新建 `references/skill-design-patterns.md`（41行增量蒸馏版）

**SKILL.md 去重**：
- 新增模式分类定义（5 种模式各 1 行判断标准）
- 确认门槛从 3 处重复减为 Gotchas 1 处定义，Step 3/4 改为引用
- 删除"30 秒内完成"幻影指令
- 按需加载路径更新（旧文件名 → 新文件名）

**原则重叠修复**：
- P3/P4 补充深层原理（原 prompt-engineering-principles H4/S1 的内容合并进来）

**引用文件精简**：
- quality-rubric.md：Token 阈值改为引用 SKILL.md P1；Quick-Fail 删除 2 条与评分维度重复的条目
- sample-review-output.md：示例 2 从完整填充压缩为差异摘要
- prompt-engineering-principles.md：H4/S1 标注"已合并到 SKILL.md"

**version.json**：
- description 改为简短一句 + 指向 SKILL.md frontmatter，不再逐字重复