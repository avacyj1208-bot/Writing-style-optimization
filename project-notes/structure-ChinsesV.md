# Structure

## 1. Role and Boundary

本文件由 `SKILL.md` 在 Raw Material 形成后加载。Structure 接收 Raw Material 以及 Task Contract 面向本阶段的 projection，建立 Dependency Graph，并通过 Linearization 形成 Linearized Structure Draft。

Structure 使用 Model II。Relation-guided Graph Construction 生成 Dependency Graph，并通过 Model I 进入 Linearization；Linearization 生成 Linearized Structure Draft 后，由 Structure Gate 检查。Pass 后进入 Writing Style Selection，Fail 则返回 Structure 内部对应步骤。

本阶段只负责识别和实现内容之间的结构关系，不修改 Task Contract 或 Raw Material，不新增 Raw Material 中不存在的事实、主张或推导，也不执行 Writing Style Selection 或 Prose Rewrite。

## 2. Input Contract

Structure 使用两类输入：Raw Material 是内容输入，Task Contract 的阶段 projection 是控制输入。

### 2.1 Raw Material

Raw Material 是经过 External Work Gate 的离散无序结果集合。它保留 Generation Lineage、Work Item Links 和 Objective Links，并可能包含 unresolved、conflict、scope、hedges、attribution、inference boundary 和技术术语等已有状态。

这里的“无序”只表示 Raw Material 尚未形成 Dependency Graph 或 Presentation Order。Work Results 的产生顺序不构成成文顺序。

### 2.2 Task Contract Projection

Task Contract 面向 Structure 的 projection 记为：

$$
T_S=\pi_{\mathrm{Structure}}(\text{Task Contract}).
$$

$T_S$ 包括：

- Requested Objectives；
- Task Relations；
- `target stages` 包含 Structure 的 requirements；
- Deliverable Contracts。

Structure 从全流程持续可索引的 Task Contract 读取这些内容。该 projection 不写入 Raw Material，也不由 Structure 重写。

### 2.3 Relation Inventory

Relation Inventory 是 Graph Construction 使用的开放判断参考。Structure 只在建立包含 edges 的 Dependency Graph 时查询 `references/relation-inventory.md`；单一 Semantic Unit、无 edge 的退化 Graph 不需要查询。

Relation definitions、endpoint roles、directionality、可能的 hard-order implications 和示例由 `references/relation-inventory.md` 定义，本文件不重复列举 relation taxonomy。

## 3. Output Contract

Structure 形成两个相互区分的 artifact：

- **Dependency Graph**：Relation-guided Graph Construction 的输出，也是 Linearization 的输入；
- **Linearized Structure Draft**：Linearization 的输出，也是 Structure Gate Pass 后交给 Writing Style Selection 的内容 artifact。

Dependency Graph 记录 Semantic Units、relations 及 hard presentation constraints。Linearized Structure Draft 将这些结构约束落实为基本通顺、可以阅读的线性文本，但不执行 Writing Style Selection 或 Prose Rewrite。

## 4. Artifact Definitions

### 4.1 Structure Projection

Task Contract projection 中各项内容在 Structure 内承担以下作用：

| 内容 | 本阶段读取的内容 |
|---|---|
| Requested Objectives | Structure 必须组织并在结构中覆盖的用户可见内容目标。 |
| Task Relations | Derived Work Items 之间已经确定的执行与组织关系。 |
| Structure requirements | `target stages` 包含 Structure、需要由本阶段落实的 requirements。 |
| Deliverable Contracts | Structure 最终需要服务的交付物边界。 |

这些对象仍属于 Task Contract；Structure 只读取其 projection。

### 4.2 Dependency Graph

Relation-guided Graph Construction 记为：

$$
S_1:(M,T_S;\mathcal R)\mapsto D=(U,E,\lambda,H),
$$

其中 $M$ 是 Raw Material，$T_S$ 是 Task Contract 面向 Structure 的 projection，$\mathcal R$ 是开放的 Relation Inventory。

Dependency Graph 的组成如下：

| 对象 | 定义 |
|---|---|
| $U$ | Semantic Units 的集合。 |
| $E$ | 连接 Semantic Units 的无类型 edge instances；同一对 units 之间允许存在多条 edges。 |
| $\lambda$ | Edge-labeling function，为每条 edge 指定 relation type 及两端的 endpoint roles。 |
| $H$ | 根据部分 labeled relations 与当前任务得到的 hard presentation constraints。 |

### 4.3 Semantic Units

Semantic Unit 是已经识别的 reference relation 或 local relation 在当前 Requested Objectives 下涉及的内容。其边界必须使 unit 与 labeled relations 共同保留原义。

每个 Semantic Unit 保留：

| 内容 | 作用 |
|---|---|
| Unit content | 该 unit 参与当前 relations 的内容。 |
| Objective Links | 该 unit 所服务的 Requested Objectives。 |
| Material Links | 该 unit 所依据的 Raw Material。 |

Raw Material 与 Semantic Units 不保持固定数量关系。一个 Raw Material item 可以形成多个 units，多个 Raw Material items 也可以共同形成一个 unit。一个 unit 可以参与多条 relations；多个独立 Objectives 可以形成 disconnected components；简单任务可以形成只有一个 unit、没有 edge 的退化 Graph。

### 4.4 Edges 与 Edge Labels

$E$ 中的 edge instance 只表示两个 Semantic Units 之间存在联系，不携带 relation type。$\lambda$ 为每条 edge 指定 relation type 和两端 roles；同一对 units 可以因不同 relations 形成多条 edges。

Relation 的语义方向不自动等于 Presentation Order。Endpoint roles 可以确定 relation 中各 unit 的语义地位，但不自动规定它们在线性文本中的先后顺序。

### 4.5 Reference Relations 与 Local Relations

每条 edge 的 label 属于：

$$
\lambda(e)\in
\mathcal R_{\mathrm{reference}}
\cup
\mathcal R_{\mathrm{local}}.
$$

S1 优先使用 `references/relation-inventory.md` 中能够保存当前含义的 relation。没有合适定义时，可以建立只服务当前 Dependency Graph 的 local relation。每个 local relation 记录：

| 字段 | 写入内容 |
|---|---|
| `descriptive name` | 能够区分该 relation 的任务内名称。 |
| `endpoint roles` | Relation 两端 Semantic Units 各自承担的角色。 |
| `directionality` | Relation 的语义方向。 |
| `preserved meaning` | 该 relation 在当前 Graph 中需要保存的含义。 |

Local relation 不自动写回 Relation Inventory；是否收录为通用 relation 由后续人工审核决定。

### 4.6 Hard Presentation Constraints

$H$ 只保存违反后会破坏理解或任务完成的 presentation constraints。Relation 的语义方向不自动成为 hard presentation constraint；同一 relation 可以根据当前 Objectives 和 Graph 采用不同的 Presentation Order。

### 4.7 Linearized Structure Draft

Linearization 记为：

$$
S_2:D\mapsto L,
$$

其中 $L$ 是 Linearized Structure Draft。它已经由基本通顺、可以阅读的语句组成，而不是 unit IDs 的机械排列。

Linearized Structure Draft 包含：

| 结构属性 | 定义 |
|---|---|
| Presentation Order | Semantic Units 在线性文本中的先后顺序。 |
| Semantic Grouping | Units 被组织为章节、段落或其他 semantic groups 的方式。 |
| Relation Realization | Graph relations 通过顺序、邻接、分组或明确语言得到实现的方式。 |

## 5. Procedure

### 5.1 Build the Dependency Graph

1. 沿 Objective Links 审核与各 Requested Objectives 相关的 Raw Material。
2. 审查 Raw Material 内容之间可能影响拆分、连接或排列的 semantic connections。此时只定位待判断的内容联系，尚未确定 relation type；Task Relations 仍是 Derived Work Items 之间的执行与组织关系。
3. 查询 Relation Inventory，为每个待判断的 semantic connection 识别能够保存当前含义的 reference relation。
4. 没有合适 reference relation 时，建立仅服务当前 Graph 的 local relation，并明确 descriptive name、endpoint roles、directionality 与 preserved meaning。
5. 根据已经识别的 reference relation 或 local relation 所涉及的内容和 endpoint roles 确定 Semantic Units，并保留 Objective Links 与 Material Links。
6. 在 relation endpoints 对应的 units 之间建立无类型 edge instances，并通过 $\lambda$ 指定 relation type 与 endpoint roles。
7. 根据 labeled relations 与当前任务确定 $H$。

Graph Construction 遵循 Requested Objectives Oriented 原则。Raw Material 到 Semantic Units 的重构围绕 Objectives 进行，不继承 Raw Material items 的数量或划分方式。

### 5.2 Linearize the Dependency Graph

1. 为 Semantic Units 选择满足 $H$ 的 Presentation Order。
2. 为允许多种顺序的 relations 选择当前文本采用的表达方向。
3. 将相关 units 组织为章节、段落或其他 semantic groups。
4. 通过顺序、邻接、分组、连接语或必要的完整说明实现 Graph 中的重要 relations。
5. 将结构结果落实为基本通顺、可以阅读的 Linearized Structure Draft。

Linearization 根据 Requested Objectives、Graph relations 与 hard presentation constraints 决定成文顺序，不直接沿用 Raw Material 的产生顺序。Dependency Graph 可以通过文字、标签和局部结构表示，不要求维持脱离文字的纯符号状态。

## 6. Structure Gate

Structure Gate 分别检查 S1 生成的 Dependency Graph 和 S2 生成的 Linearized Structure Draft。

### 6.1 Graph Construction Checks

- 每个 Requested Objective 均有相应 Semantic Units 回应；
- 每个 unit 均保留 Objective Links 与 Material Links；
- 每条 edge 均由 $\lambda$ 指定 relation type 与 endpoint roles；
- local relation 的含义足以被识别；
- units 与 labeled relations 能够共同恢复原有 scope、hedges、attribution、inference boundary 与冲突状态。

### 6.2 Linearization Checks

- Linearized Structure Draft 覆盖需要输出的 units；
- Presentation Order 满足 $H$；
- Graph relations 能够通过顺序、邻接、分组或明确语言恢复；
- Raw Material 的产生顺序没有被误当作 Presentation Order；
- 结构形成累计推进，复杂度与真实关系相称。

### 6.3 Falsifiable Checks

以 Dependency Graph 为基准执行两类检查：

- 交换受 hard constraint 约束的 units，或删除、反转相关 edge；如果逻辑、指代、推导、scope 或完成条件均不受影响，对应 constraint 或 relation 可能没有真实建立；
- 交换被认为独立的 units；如果产生明显损失，Graph 可能遗漏 relation 或 hard constraint。

## 7. Failure Routing

Graph、Semantic Unit、edge、$\lambda$、local relation 或 $H$ 的问题返回 Relation-guided Graph Construction。Presentation Order、Semantic Grouping 或 Relation Realization 的问题返回 Linearization。

Structure Gate Fail 只返回 Structure 内部对应步骤，不返回 Analyze Prompt 或 External Work，也不进入 Writing Style Selection 修改内容。本阶段不另行设置 retry 次数。

## 8. Handoff

Structure Gate Pass 后，Linearized Structure Draft 成为 Structure 直接交给 Writing Style Selection 的内容 artifact。

Task Contract 继续作为全流程持续可索引的只读控制 artifact 保留。Structure 使用的 Task Contract projection 只服务本阶段，不作为 Handoff 传给下一阶段；Writing Style Selection 从 Task Contract 读取与自身有关的 projection。

Writing Style Selection 可以依据 Linearized Structure Draft 建立 Writing Style Map，但不得改变已经通过 Structure Gate 的 Semantic Units、relations、Presentation Order 或 hard presentation constraints。
