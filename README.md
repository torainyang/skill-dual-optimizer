# Skill Dual Optimizer

A CodeFuse skill for reviewing and optimizing existing skills, using a **dual-perspective audit framework**.

## What It Does

审查并优化 CodeFuse skill 的触发描述、工作流设计、确认门槛、渐进式披露和资源组织。

核心流程：**审查 → 计划 → 确认 → 实施 → 校验**。

## Dual-Perspective Framework

与通用 skill optimizer 的关键区别：每个检查项同时从两个视角评估——

| 视角 | 关注点 |
|---|---|
| **〔A〕LLM 执行质量** | 指令是否可执行、无歧义、token 高效 |
| **〔B〕用户价值质量** | 输出是否解决用户问题、体验是否流畅 |

## Features

- **5 种设计模式识别**：Tool Wrapper / Generator / Reviewer / Inversion / Pipeline
- **8 维双视角 Checklist**：触发、工作流、确认门槛、渐进式披露、Gotchas、资源组织、PE 质量、输出规范
- **5 维量化评分体系**：触发准确性 25% / 工作流可执行性 25% / 结构效率 20% / 护栏完整性 15% / 输出质量 15%
- **PE 原则库**：H1-H5 硬性原则 + S1-S5 软性指南
- **正向示例驱动**：Generator 和 Tool Wrapper 两种模式的完整审查示例

## File Structure

```
skill-dual-optimizer/
├── SKILL.md                          # 主指令文件（LLM 读取入口）
├── version.json                      # 版本与元数据
├── README.md                         # 本文件（人类阅读）
└── references/
    ├── review-checklist.md           # 8 维双视角审查清单
    ├── skill-design-review-framework.md  # 5 种设计模式框架
    ├── prompt-engineering-principles.md  # PE 原则库（H1-H5 + S1-S5）
    ├── quality-rubric.md             # 5 维量化评分体系
    ├── sample-review-output.md       # 审查输出示例（Generator + Tool Wrapper）
    └── 技能创作最佳实践 - Claude API Docs.md  # Anthropic 官方最佳实践参考
```

## Quality Score

经过 3 轮自审迭代：

| 轮次 | 版本 | 得分 | 等级 |
|---|---|---|---|
| R1 | v1.2 → v1.3 | 3.85 | B+ |
| R2 | v1.3 → v1.5 | 4.425 | A |
| R3 | v1.5 (final) | 4.725 | A |

## Usage

在 CodeFuse 中安装后，对话中说：

- "优化这个 skill"
- "检查 skill 质量"
- "帮我改进 XXX skill"
- "重构技能说明"

即可触发。

## Attribution

本 skill 基于 [ant_skill_center/skill-optimizer v1.2](https://code.alipay.com/ant_skill_center/skill-optimizer)（作者：悟鸣）重构而来。

v1.3–v1.6 的双视角审查框架、6 个 reference 文件、SKILL.md 完全重写及 README 由 **Torain** 完成。

## License

Internal use. Derivative of `ant_skill_center/skill-optimizer`.
