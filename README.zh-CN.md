# Logic Architect

[English](README.md) | 简体中文

一个**执行前推理 Skill**。它会在真正动手之前，先挑战隐藏假设、校准复杂需求，并通过 **M.E.N. 框架**把需求整理成可执行的结构：

- **M — Metadata（元数据 / 结构元信息）**
- **E — Essence（本质 / 不可违背的核心规则）**
- **N — Non-linear（非线性 / 异常、回退与并发行为）**

Logic Architect 适合用在这样一类任务上：**如果过早开始执行，就很容易把错误假设直接固化进后续实现。**

---

## 这个项目是为了解决什么问题

很多复杂需求，表面看起来已经说清楚了，但底层逻辑其实并没有闭合。

常见失败模式包括：

- 隐含假设从未被显式指出
- 实体被提到了，但结构关系没有建模
- invariant（不变量）似乎存在，但没有被明确写出来
- 规则冲突时谁优先并不清楚
- rollback、打断、部分完成这些真实世界问题被忽略
- 还没完成逻辑对齐，就直接进入执行

Logic Architect 就是为这个问题而设计的。

它不是直接帮你写最终产物，而是作为一个**执行前的推理层**存在：
在写 prompt、代码、workflow 或 Skill 之前，先把逻辑结构显式化。

---

## 它和普通 Prompt / 助手有什么区别

Logic Architect **不是**被动总结器，也**不是**顺从型起草助手。

它的职责是：

- 在实现之前暴露逻辑真空
- 用高杠杆问题挑战高风险假设
- 用 M.E.N. 建模需求的底层结构
- 通过显式的 execution handover，把分析桥接到执行

它的核心原则是：

> **Preserve epistemic sharpness; reduce interaction friction.**
>
> 保留认知上的锋利，降低交互上的阻力。

这意味着：

- 不为了“好说话”而回避关键逻辑问题
- 不为了显得聪明而刻意抬杠
- 宁可提出 1 个致命 challenge，也不堆 5 个浅层问题
- 保留严谨性，但输出必须真的可用

---

## M.E.N. 框架

### M — Metadata
用于建模系统里“有什么东西存在”。

重点关注：

- 实体（entities）
- 持久状态 vs 运行时状态
- source of truth（真值来源）
- topology（结构拓扑）
- ownership / gravity（控制权与依附关系）
- 实体之间的结构关系

### E — Essence
用于定义系统中不可违背的逻辑核心。

重点关注：

- invariants（不变量）
- success definition（成功定义）
- conflict hierarchy（冲突优先级）
- tradeoff priority（取舍顺序）

### N — Non-linear
用于建模系统在异常、打断、变化和混乱条件下会怎样表现。

重点关注：

- rollback / backtrack（回滚与回退）
- partial completion（部分完成）
- minimum viable valid state（最小有效状态）
- concurrency 与冲突处理
- recovery logic（恢复逻辑）

---

## 什么时候适合使用 Logic Architect

当需求涉及以下情况时，Logic Architect 很有价值：

- 多个实体相互作用，而且存在跨实体约束
- 存在状态迁移、回滚、继承或部分完成
- 存在并发、优先级冲突或权限层级
- 需要设计 workflow / protocol / orchestration logic
- 要设计 Skill、Agent 或业务规则框架
- 一旦早期假设出错，就会污染后续实现

典型使用场景包括：

- 设计一个 agent skill
- 规划多步骤 workflow
- 梳理业务规则系统
- 在编码前对齐需求逻辑
- 把一个 prompt 打磨成更稳健的执行框架
- 在实现前先定义系统行为

---

## 什么时候不需要它

以下场景通常不需要 Logic Architect：

- 简单起草
- 低风险内容生成
- 直接总结现有内容
- 试错成本比前置分析更低的任务

---

## 模式

### Sharp Mode
默认模式。

适合：

- 任务已经复杂到需要先做逻辑对齐
- 但风险还没高到必须进入严格确认流程

行为特点：

- 1–2 个 precision challenge
- 紧凑版 M.E.N. 输出
- 直接给出 execution mapping
- 在必要时带着显式假设继续推进

### Hardcore Mode
适合：

- 任务会定义架构方向
- 歧义密集
- 下游出错成本很高
- 用户明确要求更强的挑战密度

行为特点：

- 更密集的 challenge 层
- 更完整的 M.E.N. 分析
- 显式 assumptions 与 gaps
- 在正式执行前增加确认门

---

## 工作流

Logic Architect 的整体流程如下：

```text
[Request]
→ [Evidence-First Reset]
→ [Precision Challenge]
→ [M.E.N. Analysis]
→ [Execution Mapping]
→ [Confirmation / Direct Continuation depending on risk]
```

### 1. Evidence-First Reset
不要不加判断地套用旧模式。

更好的做法是：

- 默认暂停复用既有模式
- 只有当前证据支持时，才复用已有抽象
- 比起熟悉的类比，更优先追求结构清晰

### 2. Precision Challenge
找出那 1–2 个如果不被检视，就最可能毁掉整个任务的关键假设。

例如：

- scope 边界不清
- 把“第一种实现形状”误当成真实需求
- 冲突优先级没有定义
- rollback 行为缺失
- success criteria 没有闭合

### 3. M.E.N. Analysis
搭建逻辑框架：

- 系统里有什么
- 哪些东西必须始终成立
- 现实变复杂时会发生什么

### 4. Execution Mapping
把分析结果翻译成可执行结构。

通常包括：

- implementation goal
- ordered TODOs
- key assumptions
- failure points to guard
- minimal shippable version

---

## 输出形态

一个典型输出大致会长这样：

```md
## Logic Alignment Draft

### Precision Challenges
1. [High-leverage challenge]
2. [High-leverage challenge]

### M — Metadata
[working model, entities, source of truth, topology]

### E — Essence
[invariants, success definition, conflict hierarchy]

### N — Non-linear
[rollback, partial completion, minimum viable state, recovery]

### Assumptions / Gaps
[what is assumed, what remains unresolved]

### Execution Mapping
[implementation goal, TODOs, guardrails, minimal valid next step]
```

---

## 示例

### 输入

> 设计一个工作流：一个 agent 写代码，一个 agent 审查代码，另一个 agent 自动合并。

### Logic Architect 会优先思考什么

- 最终谁拥有最高决策权？
- 如果 review 无限循环，谁来打破死锁？
- 自动 merge 的最低合法状态是什么？
- review feedback 是建议性的还是阻断性的？
- merge 成功后，如果 post-merge validation 失败，该如何处理？

### 可能的对齐结果

- **M：** writer、reviewer、merger、human override；source of truth 是 review state 加 branch status
- **E：** 未验证代码绝不能自动合并；人工 override 保留最终裁决权
- **N：** 连续 3 轮 review 失败后，不再无限循环，而是升级为人工处理
- **Execution Mapping：** 先定义 review states，再加 max-iteration guard，最后在 merge 前增加 final validation gate

---

## 仓库结构

```text
.
├── SKILL.md
├── README.md
├── README.zh-CN.md
├── LICENSE
└── examples/
    ├── example-agent-workflow.md
    └── example-business-logic.md
```

---

## 如何使用

你可以这样调用它：

```text
Use Logic Architect on this requirement before execution.
```

```text
Run a M.E.N. analysis on this workflow.
```

```text
Use Hardcore Logic Architect mode for this system design.
```

---

## 设计哲学

Logic Architect 建立在一个很简单的判断上：

> 很多糟糕的执行，并不是从执行本身开始失败的，
> 而是在更早的时候，就把未经挑战的结构假设当成了事实。

所以它在第一步并不追求速度，而是优先追求：

**在形成惯性之前，先保证结构正确。**

它不是为了“强硬”而强硬，
而是为了在真正开始生产之前，让逻辑缺口无法继续伪装成“已经清楚的需求”。

---

## 当前状态

当前版本：**v2 draft**

这个仓库正在向一个更稳定的生产级 Skill 规范演进，目标是：

- 推理足够锋利
- 结构足够明确
- 交互足够可用
- 分析能自然桥接到执行

---

## 贡献方式

欢迎提出建议、批评和结构性改进。

高价值的贡献包括：

- 更合理的 trigger logic
- 更强的 M.E.N. 定义
- 更稳的 execution handover 模式
- 更好的示例
- 更锋利但更干净的 challenge 表达

欢迎提交 issue 或 pull request。

---

## License

本项目使用 [MIT License](LICENSE) 开源。
