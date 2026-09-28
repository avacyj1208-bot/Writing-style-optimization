---
name: writing-style-optimization
description: 用于任何以自然语言文本为主要交付物的任务，包括回答、解释、分析、讨论、建议、报告、文档、起草、重写和审校。通过任务识别与 External Work 交接、依赖结构重建、Writing Style Selection 和 Prose Rewrite 管理完整输出流程，并保留事实、scope、hedges、attribution、inference boundary 与技术术语。主要交付物为代码、配置、公式或结构化数据时不使用；其中附带的自然语言说明仍适用。
---
# Writing Style Optimization

## 范围

本 Skill 用于管理自然语言输出的完整流程，包括任务识别、信息交接、结构重建、写作方式选择和 prose realization。本 Skill 不直接执行检索、计算或推理，只在文档中规定这些工作的输入、输出与交接接口；具体工作交由 External Agents / Tools 执行。

## 核心约束

- 以 Requested Objectives 为全流程主线。Derived Work Items、Work Results、Semantic Units 和 Deliverables 均围绕 Objectives 重新组织；阶段之间传递内容及其必要 links，各类对象之间不要求固定的一一对应关系。
- 严格区分 Task Contract、Raw Material、Dependency Graph、Linearized Structure Draft、Writing Style Map 和 Final Output。每个阶段只修改自己负责的 artifact。
- External Agents / Tools 产生的 Work Results 经 External Work Gate 检查并整理为 Raw Material 后，后续阶段不得发明其中不存在的事实、主张或推导，不得丢失 scope、hedges、attribution、inference boundary、冲突状态或技术术语。
- Gate 属于其所检查阶段。Gate Fail 只返回该阶段，不建立跨阶段的常规回退。
- 严格遵循文档规定的处理顺序；除各阶段 Gate 规定的本地回退外，不跳过、倒置或跨阶段重做。

## 控制模型

使用两种基本处理模型：

- **Model I — Direct Pass**：步骤完成产物后直接交给下一步骤，用于不需要本地质量判定的阶段连接或内部转换。
- **Model II — Gate Check Loop**：步骤产生 artifact 后由本阶段 Gate 检查；Pass 后继续，Fail 则返回本阶段重做。

External Work、Structure、Writing Style Selection 和 Prose 使用 Model II；其余连接使用 Model I。

## 工作流与文件加载

所有任务都从 Analyze Prompt 开始，并按相同的阶段顺序处理。Supporting file 的加载条件只决定何时读取阶段细则，不决定是否执行该阶段。任务较小时，各阶段可以产生最小或退化形式的 artifact，但不得跳过阶段。

1. 开始 Analyze Prompt 时读取 `references/task-and-work.md`。
2. 得到 Raw Material 后读取 `references/structure.md`。
3. 建立包含 edges 的 Dependency Graph 时查询 `references/relation-inventory.md`；单一 unit、无 edge 的退化 Graph 不需要查询。
4. Structure Gate 通过后读取 `references/writing-style-selection.md`。
5. Writing Style Selection 通过 `references/writing-style-selection.md` 内的 Active Transforms Index 了解可用 transform 的类别、触发特征、适用范围和文件位置；Selection 只在 Writing Style Map 中记录 selected transform 和 override scope，不读取 `transforms/` 正文。
6. Writing Style Gate 通过后读取 `references/prose.md`；Prose 仅在 Writing Style Map 包含 selected transform 时读取对应的 `transforms/` 文件，并按其规则完成改写。

所有任务都从 Analyze Prompt 开始，只是中间对象可能很小。例如：

```text
一个 Objective
→ 一个 Derived Work Item
→ 一个 Work Result
→ 一个 Semantic Unit、零条 edge
→ 无 transform 的 Writing Style Map
→ 一段 Final Output
```

## 工作流

### 1. Analyze Prompt 与 External Work

Analyze Prompt 是整个流程的起始点。它对用户输入进行任务层分析与拆解，识别最终输出必须完成的内容，并形成 Task Contract。Task Contract 只定义任务及其输入，不定义段落或语义结构，包括：

- **Problem Specification**：Requested Objectives、Derived Work Items、Structure Readiness Conditions、Task Relations、Requirement Routing 与 Deliverables；
- **Context Specification**：区分任务与参考信息，并通过 Input Inventory 记录输入的 role、authority 和 objective links。

根据 Derived Work Items、required results、Task Relations、Structure Readiness Conditions 和 Input Inventory 生成 Derived Work Plan，交给 External Agents / Tools 执行。在进入 Structure 前，Work Results 必须经过 External Work Gate。

External Work Gate 检查 Coverage、Requirement Compliance、Traceability 和 Sufficiency。通过检查的 Work Results 与达到局部 retry 上限后生成的 Unresolved Work Results 一并整理为 Raw Material；未解决状态被保留，但不中止流程。具体定义、检查顺序和 retry 规则见 `references/task-and-work.md`。

### 2. Structure

Structure 接收 Raw Material，以及 Task Contract 中面向 Structure 的 Objectives、Task Relations、requirements 和 Deliverable Contracts。

首先依据开放的 Relation Inventory 识别语义关系及 endpoint roles，据此建立 Dependency Graph：

$$
D=(U,E,\lambda,H),
$$

其中 $U$ 为 Semantic Units，$E$ 为无类型 edge instances，$\lambda$ 为 edge-labeling function，$H$ 为 hard presentation constraints。找不到合适的既有 relation 时，可以建立只服务于当前 Graph 的 local relation；不得自动把它写回 Relation Inventory。

随后对 Dependency Graph 进行 Linearization，得到已经由基本通顺语句组成的 Linearized Structure Draft。Linearization 通过顺序、邻接、分组及必要的关系说明实现 Graph 中的重要 relations；Raw Material 的产生顺序不得直接成为 Presentation Order。

Structure Gate 分别检查 Graph construction 与 Linearization。Graph、unit、edge、label 或 hard constraint 的问题返回 Graph construction；顺序、分组或 Relation Realization 的问题返回 Linearization。具体过程与检查见 `references/structure.md`。

### 3. Writing Style Selection

Writing Style Selection 负责处理成功通过 Structure Gate 的 Linearized Structure Draft。所有文本本身设定为 Default Style。Writing Style Selection 通过 `references/writing-style-selection.md` 内的 Active Transforms Index 了解可用 transform 的类别、触发特征、适用范围和文件位置，但不读取 `transforms/` 正文。随后为每个 Deliverable 确定 Default Language Profile，并为现有 Unit 或 Unit Group 建立 Writing Style Map，记录 Surface Realization，以及适用时的 local Language Profile override、selected transform 和 override scope。每个 Semantic Unit 使用 Default Style 或恰好一个 selected transform；不同 transforms 只能分配给互不重叠的 scopes。

Special Transform 可以对 Default Style 的写作方式进行覆盖。Writing Style Selection 只分配 Special Transform 标签及其 override scope，不执行实际改写。Writing Style Gate 只检查 Style Map。Fail 时重新选择 Language Profile、Surface Realization、transform 或 override scope，不返回 Structure。具体过程、Active Transforms Index 和检查规则见 `references/writing-style-selection.md`。

### 4. Prose

Prose 按照通过检查的 Writing Style Map 重写 Linearized Structure Draft。进入本阶段后先读取 `references/prose.md`；仅当 Writing Style Map 包含 selected transform 时，才读取对应的 `transforms/` 文件，并在相应 override scope 内执行其规则。未被 transform 覆盖的部分按照 Default Language Profile 与相应的 Surface Realization 改写。本阶段可以调整措辞、句法、句子边界和表面格式，但不得新增 claim，也不得改变已经确定的 relations、Presentation Order、hard constraints、inference boundary、限定语、hedges 或技术术语。

Prose Gate 检查 Objectives 是否得到完整回应、结构关系是否被正确实现、信息与限定是否保留、术语是否稳定、selected transforms 是否在各自 override scope 内被正确实现，以及文本中是否仍存在无信息铺垫、重复结论、虚假 transition 或 chatbot residue。Transform 的实际文本效果由 Prose Gate 检查，不属于 Writing Style Gate。Fail 只返回 Style-Guided Prose Rewrite；Pass 后输出 Final Output。具体规则见 `references/prose.md`。

## 完成条件

只有同时满足以下条件，才交付 Final Output：

- Requested Objectives 已在相应 Deliverables 中得到回应；
- hard requirements 已满足，未满足的 soft requirements 已服从事实准确性、信息保真或更高优先级要求；
- Raw Material 中的 unresolved、conflict、scope、hedges 和 attribution 没有被无依据地消除；
- Dependency Graph 中的重要 relations 和 hard constraints 可以从最终文本恢复；
- Writing Style Map 没有覆盖 Structure 的决定；
- Prose Rewrite 没有新增内容主张或破坏术语与限定；
- 输出形式与真实逻辑复杂度相称，没有为遵循模板而增加不必要的章节、列表、表格或短句化。
