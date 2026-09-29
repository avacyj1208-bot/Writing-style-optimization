# Writing Style Optimization

Writing Style Optimization 是一个面向自然语言交付物的 Codex Skill。它处理的不是孤立的“润色”步骤，而是从任务识别到最终成文的完整输出流程：识别用户真正要求的结果，组织检索、计算、分析等外部工作，将结果重建为可解释的语义结构，选择适合当前读者与交付形式的写作方式，最后完成受约束的 Prose Rewrite。

本 README 用于说明项目定位、运行流程和文件组织。正式运行规则以 [`writing-style-optimization/SKILL.md`](writing-style-optimization/SKILL.md) 及其直接加载的 `references/`、`transforms/` 文件为准；如 README 与运行时文件出现不一致，应以运行时文件为准。

## 适用范围

本 Skill 适用于主要交付物为自然语言文本的任务，包括：

- 回答、解释、分析与讨论；
- 建议、评估与报告；
- 文档起草、重写与审校；
- 代码、配置、公式或结构化数据任务中附带的自然语言说明。

当主要交付物是代码、配置、公式或结构化数据时，不使用本 Skill 管理这些非自然语言产物本身。

本 Skill 也不直接执行检索、计算或领域推理。它定义这些工作的任务接口、结果要求、质量检查和阶段交接；具体工作由 External Agents / Tools 完成。

## 核心原则

整个流程以 Requested Objectives 为组织主线。内部 Work Items、外部 Work Results、Semantic Units 和最终 Deliverables 都服务于这些 Objectives，但它们之间不要求一一对应。

流程严格区分六类核心 artifact：

| Artifact | 作用 | 负责阶段 |
|---|---|---|
| Task Contract | 定义用户要求完成什么、需要哪些内部工作、输入如何使用，以及最终交付物的边界 | Analyze Prompt |
| Raw Material | 保存通过 External Work Gate 的结果及其来源、链接、限定和未解决状态 | External Work |
| Dependency Graph | 表示 Semantic Units 之间的语义关系及 hard presentation constraints | Structure：Graph Construction |
| Linearized Structure Draft | 将 Graph 转换为具有 Presentation Order、Semantic Grouping 和 Relation Realization 的基本通顺文本 | Structure：Linearization |
| Writing Style Map | 为 Deliverable 和既有 Semantic Units / Groups 指定 Language Profile、Surface Realization 与可选 transform | Writing Style Selection |
| Final Output | 通过 Prose Gate 和最终交付检查的实际文本 | Prose |

每个阶段只修改自己负责的 artifact。后续阶段不得用“改善表达”为理由重做上游决策，也不得丢失已经建立的事实、scope、hedges、attribution、inference boundary、conflict states 或技术术语。

Task Contract 在整个流程中保持可索引、只读，不会在某次 handoff 后失效。各阶段按职责读取自己的 Task Contract projection；该 projection 不会被复制进内容 artifact，也不会作为前一阶段的附加输出传递。

## 完整流程

```text
User Input
  ↓
Analyze Prompt
  ├─ Task Contract
  └─ Derived Work Plan
       ↓
External Agents / Tools
       ↓
Work Results
       ↓
External Work Gate
       ↓
Raw Material
       ↓
Relation-guided Graph Construction
       ↓
Dependency Graph
       ↓
Linearization
       ↓
Linearized Structure Draft
       ↓
Structure Gate
       ↓
Writing Style Selection
       ↓
Writing Style Map
       ↓
Writing Style Gate
       ↓
Style-Guided Prose Rewrite
       ↓
Prose Gate
       ↓
Final Output
```

所有任务都执行相同的阶段顺序。简单任务可以生成最小或退化形式的 artifact，例如单一 Objective、单一 Semantic Unit、零条 edge 和不选择 transform 的 Writing Style Map，但不能跳过阶段。

流程使用两种控制模型：

- **Model I — Direct Pass**：步骤生成 artifact 后直接交给下一步骤，用于不需要本地质量判定的连接或转换。
- **Model II — Gate Check Loop**：阶段生成 artifact 后由本阶段 Gate 检查；Pass 后继续，Fail 只返回当前阶段的对应步骤。

External Work、Structure、Writing Style Selection 和 Prose 使用 Model II。Gate 属于它所检查的阶段，不建立跨阶段的常规回退。

## 1. Analyze Prompt 与 External Work

Analyze Prompt 从用户输入及有效上下文中识别 Requested Objectives，并建立 Task Contract。Task Contract 包含：

```text
Task Contract
├─ Problem Specification
│  ├─ Requested Objectives
│  ├─ Derived Work Items
│  ├─ Structure Readiness Conditions
│  ├─ Task Relations
│  ├─ Requirement Routing
│  └─ Deliverables
│     └─ Deliverable Contract
└─ Context Specification
   └─ Input Inventory
```

Problem Specification 区分用户可见的内容目标、内部执行工作、工作间关系、要求所约束的阶段，以及实际承载结果的 Deliverables。Task Relations 只表示 Derived Work Items 之间的执行与组织关系；内容内部的语义关系由 Structure 建立。

每项 requirement 记录约束强度、target stages 和 objective links。Hard requirement 必须满足；soft requirement 在与 hard requirement、事实准确性或信息保真冲突时可以让步。每个 Deliverable Contract 记录 `artifact / audience / carrier / granularity`，并在用户明确要求可见组成部分时记录 `explicit deliverable slots`。Context Specification 则通过 Input Inventory 区分任务与参考信息，并记录每项输入的 role、authority 和 objective links。

Derived Work Plan 根据 Work Items、required results、Task Relations、Structure Readiness Conditions 和 Input Inventory 组织 External Agents / Tools 的工作。返回的 Work Results 必须保留：

- result payload；
- generation lineage；
- work item links；
- objective links。

External Work Gate 依次检查：

1. **Coverage**：每个 Objective 下已经建立的 Work Item 是否都有对应结果；
2. **Requirement Compliance**：External Work 阶段负责的 requirements 是否满足；
3. **Traceability**：结果能否追溯到输入、来源、工具输出、计算过程或上游结果，并保留 attribution；
4. **Sufficiency**：所有结果合在一起，是否足以让 Structure 在不发明新事实或推导的前提下完成 Objectives。

Gate Fail 只重做未通过检查的 Work Items。每个失败项最多经历一次初始尝试和四次 retry；达到上限后形成保留原有 attribution 的 Unresolved Work Result，记录未完成状态并继续形成 Raw Material，而不是让整个流程报错停止。

Raw Material 是离散、无 Presentation Order 的结果集合。“无序”只表示尚未建立 Dependency Graph 和书写顺序；其中的 lineage、links、scope、hedges、attribution、conflict 和 unresolved 状态仍须完整保留。这些 links 只用于 traceability 与 attribution，本身不构成 semantic graph。形成 Raw Material 后，Work Results 不再作为另一条平行内容输入交给 Structure。

## 2. Structure

Structure 接收 Raw Material 作为内容输入，并读取 Task Contract 中适用于本阶段的 Requested Objectives、Task Relations、requirements 和 Deliverable Contracts。

### Dependency Graph

Relation-guided Graph Construction 建立：

$$
D=(U,E,\lambda,H),
$$

其中：

- $U$ 是 Semantic Units；
- $E$ 是连接 Semantic Units 的无类型 edge instances；
- $\lambda$ 为每条 edge 指定 relation type 与 endpoint roles；
- $H$ 保存违反后会损害理解或任务完成的 hard presentation constraints。

Graph Construction 在需要建立 edge 时查询开放的 Relation Inventory。它优先使用能够保存当前含义的 reference relation；现有 relation 无法准确表达时，可以建立仅服务于当前 Graph 的 local relation。Local relation 不会自动写回 Relation Inventory。

Relation 的语义方向不自动决定 Presentation Order。一个 relation 是否形成 $H$，需要根据当前 Objectives、scope 和理解要求独立判断。

### Linearization

Linearization 将 Dependency Graph 转换为 Linearized Structure Draft：

$$
S_2:D\mapsto L.
$$

该 Draft 已经是基本通顺、可读的线性文本，并明确体现：

- Presentation Order；
- Semantic Grouping；
- Relation Realization。

Raw Material 的产生顺序不能直接成为书写顺序。Linearization 根据 Objectives、Graph relations 和 $H$ 选择顺序，并通过邻接、分组、connectives 或必要的完整说明实现重要 relations。

### Structure Gate

Structure Gate 分别检查 Graph Construction 与 Linearization：

- Graph、Semantic Units、edges、labels、local relations 或 $H$ 的问题返回 Graph Construction；
- Presentation Order、Semantic Grouping 或 Relation Realization 的问题返回 Linearization。

Gate 还通过交换受 hard constraint 约束的 units，以及交换被视为独立的 units，检验现有 relation 和 constraint 是否真实成立、是否存在遗漏。Fail 不返回 Analyze Prompt 或 External Work，也不进入 Writing Style Selection 修补结构问题。

## 3. Writing Style Selection

Writing Style Selection 接收已经通过 Structure Gate 的 Linearized Structure Draft。此阶段保持 Draft 完全不变，只建立附着在其外部的 Writing Style Map。

每个 Deliverable 首先根据 applicable requirements 和 Deliverable Contract 获得一个 Default Language Profile。Language Profile 是开放描述，包括沟通关系、正式程度、领域惯例、术语策略和对读者知识的假设；它不是固定等级或封闭类别。

随后为既有 Semantic Units 或 Semantic Groups 选择 Surface Realization，例如连续文本、段落、章节、项目列表、有序步骤、表格或图示。Surface Realization 只能表达 Structure 已经建立的 grouping、relations 和 Presentation Order，不能重新划分内容。

### Special Transforms

Active Transforms Index 当前包含四类 transform：

| Transform | 触发条件 | 适用范围 |
|---|---|---|
| `academic` | 读者需要评估研究 claims、evidence、inference scope 或 scholarly attribution | 整个 academic Deliverable，或主要功能为评估研究 claims 的 Semantic Groups |
| `procedural` | 读者需要在明确条件下执行动作并识别结果 | 主要功能为呈现 conditions、actions 和 results 的 Semantic Groups |
| `lookup-reference` | 读者需要借助稳定 lookup key 进行选择性、非线性检索，并能局部理解各 entry | 同时具有 nonlinear access、stable lookup key 和 independently interpretable entries 的 Semantic Groups |
| `architecture` | 读者需要理解 components、dependencies、workflows、constraints 或 design rationale | 主要功能为解释系统组成及其关系的 Semantic Groups |

Writing Style Selection 只读取 Index 中的 category、trigger features、applicable scope 和 file path，不读取 transform 正文。每个 Semantic Unit 最终使用 Default Style 或恰好一个 selected transform；不同 transforms 的 override scopes 必须互不重叠。无法确定主要 reader function 时，保留 Default Style。

Writing Style Map 记录：

- Deliverable 级的 Default Language Profile；
- unit 或 group 级的 Surface Realization；
- 必要时的 local Language Profile override；
- detected genre / function；
- 必要时的 selected transform 与 override scope。

### Writing Style Gate

Writing Style Gate 只检查 Map，不检查改写后的文本：

- **Resolvability**：每项 directive 能否解析到现有 Deliverable、unit 或 group，Prose 是否可以直接执行；
- **Compatibility**：Map 是否符合 requirements、Deliverable Contract、carrier 能力和 Structure 已确定的内容；
- **Selection Necessity**：每个非默认选择是否有实际依据，Default Style 足够时是否移除了多余 transform 或格式。

Fail 只修改 Writing Style Map，不返回 Structure，也不通过提前改写 Draft 来规避 selection 问题。

## 4. Prose

Prose 接收未改动的 Linearized Structure Draft 和通过检查的 Writing Style Map，并读取适用于本阶段的 Task Contract projection。进入本阶段后先加载通用的 Prose rules；只有 Map 实际选择了 transform，才读取对应 transform 文件，并且只在指定 override scope 内执行。

Style-Guided Rewrite 可以调整：

- wording；
- syntax；
- sentence boundaries；
- Map 指定的 surface formatting。

它不得新增 claims，也不得改变已经确定的 Semantic Units、relations、Presentation Order、hard constraints、inference boundaries、qualifications、hedges 或技术术语。

所有通用规则与 transforms 都受两个 Rewrite Constraints 约束：

1. **Preserve established meaning**：保存 Draft 中已经建立的内容和语义关系；
2. **Preserve referential identity**：保持名称、技术术语和其他识别性表达稳定。

通用 Prose rules 处理 sentence construction、local information flow、terminology and repetition，以及 redundancy and rhetorical inflation。它们是并行适用的写作指导，不是必须逐项执行的审计流水线，也不能被用作判断文本是否由 AI 生成的检测标准。

### Prose Gate

Prose Gate 审查将要实际交付的精确文本，而不是源文本或较早候选稿：

- **Meaning and Structure Preservation**：事实、数值、限定、归属、关系、grouping 和 Objectives 是否保留；
- **Style and Requirement Compliance**：Language Profile、Surface Realization、transforms、Prose requirements 和 Deliverable Contracts 是否正确实现；
- **Readability and Economy**：指代、句法和局部信息流是否清楚，是否仍有无效重复、虚假 transition、空泛综合、修辞膨胀或 chatbot residue。

Fail 只返回 Style-Guided Prose Rewrite。若 Gate 之后又修改任何文字，必须重新检查受影响的内容及上下文。通过检查的精确文本才成为 Final Output。

## 文件加载与 Progressive Disclosure

运行时文件按照阶段按需加载：

1. Analyze Prompt 开始时读取 [`references/task-and-work.md`](writing-style-optimization/references/task-and-work.md)；
2. Raw Material 形成后读取 [`references/structure.md`](writing-style-optimization/references/structure.md)；
3. 构建包含 edges 的 Graph 时查询 [`references/relation-inventory.md`](writing-style-optimization/references/relation-inventory.md)；
4. Structure Gate 通过后读取 [`references/writing-style-selection.md`](writing-style-optimization/references/writing-style-selection.md)；
5. Writing Style Selection 只读取 Active Transforms Index，不读取 transform 正文；
6. Writing Style Gate 通过后读取 [`references/prose.md`](writing-style-optimization/references/prose.md)；
7. Prose 只读取 Writing Style Map 实际选择的 transform 文件。

这种加载方式让核心流程始终可见，同时避免在不适用的任务中加载完整 relation inventory、全部 transform 或大量例子。

## 项目结构

```text
.
├─ README.md
├─ writing-style-optimization/
│  ├─ SKILL.md
│  ├─ references/
│  │  ├─ task-and-work.md
│  │  ├─ structure.md
│  │  ├─ relation-inventory.md
│  │  ├─ writing-style-selection.md
│  │  └─ prose.md
│  └─ transforms/
│     ├─ academic.md
│     ├─ procedural.md
│     ├─ lookup-reference.md
│     └─ architecture.md
└─ project-notes/
   └─ 非运行时的设计记录、历史版本与开发参考
```

`writing-style-optimization/` 是可运行、唯一权威的 Skill 目录。`project-notes/` 不属于 Skill 的 Progressive Disclosure 路径，也不是运行时依赖。

## 扩展规则

新增 transform 时，先完成独立的 Markdown 文件，再把 category、trigger features、applicable scope 和 file path 添加到 Active Transforms Index。修改 transform 的触发条件、范围或路径时，必须同步更新对应 Index entry。

Relation Inventory 是开放集合，但任务中建立的 local relation 只服务当前 Dependency Graph。只有经过人工确认其跨任务复用价值，并确认现有 labels、endpoint roles 或 unit scope 无法表达其含义后，才应将其加入正式 inventory。

## 完成条件

只有同时满足以下条件，流程才交付 Final Output：

- Requested Objectives 已在相应 Deliverables 中得到回应；
- hard requirements 已满足，soft requirements 只在与事实准确性、信息保真或更高优先级要求冲突时让步；
- Raw Material 中的 unresolved、conflict、scope、hedges 和 attribution 未被无依据地删除；
- Dependency Graph 的重要 relations 和 hard constraints 可以从最终文本中恢复；
- Writing Style Map 没有覆盖 Structure 的决定；
- Prose Rewrite 没有新增内容主张，也没有破坏术语、限定或 referential identity；
- 输出形式与真实逻辑复杂度相称，没有为了套用模板而增加不必要的章节、列表、表格或短句化。
