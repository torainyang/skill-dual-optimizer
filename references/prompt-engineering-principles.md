# Prompt Engineering Principles for Skill Design

审查 skill 时可参考的核心原则。分为硬性原则（每次审查都应检查）和软性指引（按需参考）。

---

## 硬性原则（Hard Principles）

### H1: 注意力经济学 (Attention Economics)

LLM 对 system prompt 的注意力分布不均匀：前 ~20% 和后 ~20% 是高注意力区，中间 ~60% 是低注意力区（首因-近因效应）。

检查点：
- 最关键的约束是否放在 SKILL.md 的开头
- 重要规则是否被埋在中间区域
- Gotchas 是否放在高注意力位置

### H2: 正向示例 > 负向禁止 (Positive Examples > Negative Prohibitions)

一个好示例的效果是五条"不要做X"规则的 3-5 倍。具体的坏示例会触发负向引导（Negative Priming）——LLM 的注意力被吸引到坏示例上，反而增加生成类似内容的概率。

检查点：
- 好示例与坏示例的比例是否 ≥ 3:1
- 是否存在可能触发负向引导的具体坏示例
- 能否用更多好示例替代"不要做X"的规则

### H3: 仅给可执行指令 (Executable Instructions Only)

自回归模型逐 token 生成，无法"先想好再写"或"输出前验证"。如果需要预计算影响输出，必须让 LLM 显式输出分析过程（而非"默默验证"）。

检查点：
- 是否存在幻影指令（如"默默检查后再输出"）
- 是否存在互相矛盾的指令
- 需要判断力的规则是否提供了明确标准

### H4: 语义去重 (Semantic Deduplication)

每条规则只在最相关的位置说一次。重复浪费 token 且将注意力分散到同一约束的多个实例上。

检查点：
- 同一规则是否在多处重复表述
- 能否合并分散的相关规则
- 重复是否导致了不一致

### H5: 运行时 / 元数据分离 (Runtime / Meta Separation)

- SKILL.md = 运行时指令（精简、精确、给 LLM 执行用）
- 评估标准、方法论、历史记录 = 元数据（给人或 LLM-judge 用，放在独立文件）

混合这两层会让 LLM 在"生成"和"评估"之间混淆。

检查点：
- SKILL.md 是否混入了审计方法论或评分标准
- 是否把应该拆到 references/ 的内容堆在主文件里
- 运行时指令和参考知识的边界是否清晰

---

## 软性指引（Soft Guidelines）

### S1: 规则数量纪律 (Rule Count Discipline)

规则超过 5-7 条时，每条的遵从率显著下降。核心规则压缩到 ≤7 条，次要检查内联到相关段落。

### S2: 长度锚定 (Length Anchoring)

没有显式长度指引时，LLM 倾向过度生成，后半段质量下降。为每个输出组件提供软性长度目标。中英双语 skill 同时提供字数和字符数。

### S3: 输出分段 (Output Segmentation)

单次输出超过 ~4000 token 时质量下降（重复增加、锐度降低）。甜区：2000-4000 token/次。更长输出设计文件交付或多轮策略。

### S4: 格式效率 (Format Efficiency)

System prompt 中列表比表格更省 token。表格适合输出（给人看），但在 prompt 中浪费——Markdown 表格分隔符和对齐空格无语义价值。

### S5: 双语术语锚定 (Bilingual Term Anchoring)

当 prompt 语言和输出语言不同时，关键结构性术语要双语提供，防止语言切换时语义扁平化。
