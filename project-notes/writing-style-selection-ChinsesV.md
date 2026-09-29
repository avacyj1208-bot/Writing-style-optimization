# Writing Style Selection

## 1. Role and Boundary

本文件由 `SKILL.md` 在 Structure Gate 通过后加载。Writing Style Selection 接收 Linearized Structure Draft 以及 Task Contract 面向本阶段的 projection，通过 Default Style Selection Policy 和 Active Transforms Index 建立 Writing Style Map。

Writing Style Selection 使用 Model II。确定 Default Language Profile 后，通过 Model I 读取 Active Transforms Index、识别文类与功能特征并建立 Writing Style Map；Writing Style Map 形成后，由 Writing Style Gate 检查。Pass 后进入 Prose，Fail 则返回 Writing Style Selection 内部对应步骤。

本阶段只为 Linearized Structure Draft 中已经存在的 Semantic Units 或 Semantic Groups 分配 realization，并在适用时记录 selected transform 与 override scope。Linearized Structure Draft 在本阶段保持不变；Writing Style Selection 不修改其文本，不改变 Semantic Units、Semantic Grouping、relations、Presentation Order 或 hard presentation constraints，也不读取或执行 `transforms/` 正文。

## 2. Input Contract

Writing Style Selection 使用三类输入：Linearized Structure Draft 是内容输入，Task Contract 的阶段 projection 是控制输入，Active Transforms Index 是选择 Special Transform 时使用的索引。

### 2.1 Linearized Structure Draft

Linearized Structure Draft 是已经通过 Structure Gate 的线性文本。它已经形成 Presentation Order、Semantic Grouping 和 Relation Realization，并由基本通顺、可以阅读的语句组成。

Writing Style Selection 直接引用其中已经建立的 Semantic Units 与 Semantic Groups。它不重新拆分、合并或建立 units 或 groups，也不修改 Draft 内部文本。

### 2.2 Task Contract Projection

Task Contract 面向 Writing Style Selection 的 projection 记为：

$$
T_W=\pi_{\mathrm{WritingStyleSelection}}(\text{Task Contract}).
$$

$T_W$ 包括：

- `target stages` 包含 Writing Style Selection 的 requirements；
- Deliverable Contracts。

Writing Style Selection 从全流程持续可索引的 Task Contract 读取这些内容。Requirement 与 Deliverable Contract 中的 Objective Links 继续承担归属标识，但本阶段不重新读取或规划 Requested Objectives 的内容。该 projection 不写入 Linearized Structure Draft，也不由 Writing Style Selection 重写。

### 2.3 Active Transforms Index

Active Transforms Index 位于本文件中，记录 transform selection 所需的类别、触发特征、适用范围和文件位置。Writing Style Selection 根据这些索引信息选择 transform，但不读取对应文件的正文。

Index 中的每个条目对应一个独立的 transform 文件。没有适用 transform 时，相关内容继续使用 Default Style。

## 3. Output Contract

本阶段形成一个新的 artifact：

- **Writing Style Map**：为每个 Deliverable 记录 Default Language Profile，并为 Linearized Structure Draft 中已经存在的 Semantic Units 或 Semantic Groups 记录 Surface Realization，以及适用时的 local Language Profile override、detected genre / function、selected transform 和 override scope。

Writing Style Map 是附加在 Linearized Structure Draft 之外的控制 artifact，不包含对 Draft 的改写结果。Writing Style Gate Pass 后，未经修改的 Linearized Structure Draft 与通过检查的 Writing Style Map 一并交给 Prose。

## 4. Artifact Definitions

### 4.1 Writing Style Selection Projection

Task Contract projection 中各项内容在 Writing Style Selection 内承担以下作用：


| 内容                                 | 本阶段读取的内容                                                                      |
| ------------------------------------ | ------------------------------------------------------------------------------------- |
| Writing Style Selection requirements | `target stages` 包含 Writing Style Selection、需要由本阶段落实并检查的 requirements。 |
| Deliverable Contracts                | Writing Style Map 需要服务的交付物边界及其呈现条件。                                  |

Deliverable Contract 中各字段对 selection 的作用如下：


| 字段                         | 本阶段读取的内容                                             |
| ---------------------------- | ------------------------------------------------------------ |
| `deliverable id`             | 当前 style selection 所服务的 Deliverable。                  |
| `objective links`            | 该 Deliverable 所承载内容的归属标识。                        |
| `artifact`                   | 交付物的功能类别。                                           |
| `audience`                   | language profile、解释程度与术语呈现需要适配的读者或使用者。 |
| `carrier`                    | 当前交付载体支持的表面呈现形式。                             |
| `granularity`                | 内容需要达到的展开范围、细化程度与信息密度。                 |
| `explicit deliverable slots` | 用户明确要求在交付物中出现的可见组成部分。                   |

这些对象仍属于 Task Contract；Writing Style Selection 只读取其 projection。

### 4.2 Default Style Selection Policy

Default Style 是所有文本的基础写作方式。每个 Deliverable 根据 applicable requirements 与 Deliverable Contract 确定 Default Language Profile；当前对话中的普通 response 在没有特殊要求时采用直接、自然、适合当前任务与 audience 的交流方式。

Default Style 不使用固定模板。连续论证可以使用自然段；已经存在的真实层级、并列、步骤或比较可以通过 section、list、table、diagram 或其他 carrier 支持的形式呈现。只有 Linearized Structure Draft 中已经建立的结构需要相应形式时才选择对应的 Surface Realization，必要的复杂长句仍可保留。

本阶段只形成 realization directive。措辞、句法、句子边界和表面格式的实际实现由 Prose 完成。

### 4.3 Language Profile and Surface Realization

Language Profile 是对 Deliverable 如何面向其 audience 使用语言的开放描述，包括交流关系、形式规范、领域惯例、术语策略和读者知识假设。它不使用固定等级、封闭分类或数值尺度。

Default Language Profile 作用于一个 Deliverable。只有 requirement 明确要求局部内容采用不同语言方式时，Writing Style Map 才为相应 Semantic Unit 或 Semantic Group 记录 local Language Profile override。

Surface Realization 作用于已经形成的 Semantic Unit 或 Semantic Group，依据现有 Semantic Grouping、relations 与 carrier，在连续文本、段落、section、bullet list、ordered list、table、diagram 或其他可用形式中作出选择。

显式 hard requirement 直接约束 Language Profile 与 Surface Realization。涉及 grouping 或 Presentation Order 的要求由 Structure 在 Linearized Structure Draft 中落实；Writing Style Selection 只为已经形成的 units 或 groups 选择相应的表面呈现方式。

### 4.4 Active Transforms Index

Active Transforms Index 中每个条目记录：


| 字段                 | 写入内容                                                                             |
| -------------------- | ------------------------------------------------------------------------------------ |
| `transform category` | Writing Style Map 在 `selected transform` 中记录的唯一类别标识。                      |
| `trigger features`   | 可以从 genre / function、requirements 或 Deliverable Contract 中观察到的适用条件。    |
| `applicable scope`   | Transform 可以覆盖整个 Deliverable，还是只能覆盖特定类型的 Semantic Units 或 Groups。 |
| `file path`          | Prose 在 transform 被选中后读取的正文位置。                                           |

Index 的实际条目如下：

| Transform category | Trigger features | Applicable scope | File path |
|---|---|---|---|
| `academic` | 读者需要评估研究主张、证据、推理范围或 scholarly attribution。 | 整个学术 Deliverable，或以研究主张评估为主要读者功能的 Semantic Groups。 | `transforms/academic.md` |
| `procedural` | 读者需要在明确条件下执行动作，并识别执行结果。 | 以条件、动作与结果识别为主要读者功能的 Semantic Groups。 | `transforms/procedural.md` |
| `lookup-reference` | 读者需要通过稳定的 lookup key 选择性、非线性地检索可局部理解的内容。 | 同时具有非线性访问方式、稳定 lookup key 和可独立解释 entry 的 Semantic Groups。 | `transforms/lookup-reference.md` |
| `architecture` | 读者需要理解组件、依赖、流程、约束或设计 rationale。 | 以系统组成及其关系为主要读者功能的 Semantic Groups。 | `transforms/architecture.md` |

Bibliographic references、citation lists 和 literature review 用于学术 attribution 或论证，不属于 `lookup-reference`；满足 `academic` trigger 时按 `academic` 处理。

Index 只承担 selection 所需的索引功能，不包含 transform 的具体写作规则。新增 transform 时，先完成对应的 Markdown 文件，再向 Index 增加一行；transform 的 trigger features、applicable scope 或 file path 发生变化时，同步更正相应条目。

### 4.5 Genre and Function Features

Writing Style Selection 根据 Linearized Structure Draft、applicable requirements 和 Deliverable Contract，识别现有 Semantic Unit 或 Semantic Group 的文类与功能特征。这些特征用于选择 Surface Realization，并与 Active Transforms Index 的 `trigger features` 和 `applicable scope` 匹配。同一 unit 或 group 可以具有多个 detected features；这些 features 只构成 transform selection 的候选依据，不等于多个 transform assignments。

文类与功能特征根据当前内容识别，不改变 Semantic Units 或 Semantic Groups，也不构成新的内容或结构。

### 4.6 Writing Style Map

Writing Style Map 先记录 Deliverable-level selection：


| 字段                       | 写入内容                                      |
| -------------------------- | --------------------------------------------- |
| `deliverable id`           | 当前 Writing Style Map 所服务的 Deliverable。 |
| `default language profile` | 该 Deliverable 默认采用的 Language Profile。  |

随后以现有 Semantic Unit 或 Semantic Group 为映射单位记录 entries：

| 字段                                      | 写入内容                                                                                         |
| ----------------------------------------- | ------------------------------------------------------------------------------------------------ |
| `semantic unit or semantic group`         | Linearized Structure Draft 中已经建立、由本条 entry 分配 Surface Realization 的 unit 或 group。 |
| `surface realization`                     | 该 unit 或 group 在 Default Style 下采用的表面呈现方式。                                        |
| `local language profile override, if any` | Requirement 明确要求该 unit 或 group 偏离 Default Language Profile 时采用的局部覆盖。           |
| `detected genre / function`               | 支持当前 Surface Realization 或 transform selection 的文类与功能特征。                           |
| `selected transform, if any`              | 从 Active Transforms Index 中选择的 Special Transform；没有适用 transform 时省略。               |
| `override scope`                          | selected transform 覆盖的现有 unit 或 group；未选择 transform 时省略。                           |

Map entries 可以以单个 unit 或 Structure 已经建立的 semantic group 为单位，但不建立新的 unit 划分或 grouping。每个 entry 至多记录一个 selected transform；group-level entry 表示该 group 内全部 units 接受同一 transform，不允许再为其中的 unit 嵌套分配其他 transform。没有 local override 的 entry 继承所属 Deliverable 的 Default Language Profile。Writing Style Map 只记录 selection 结果，不包含实际改写后的文本。

### 4.7 Special Transform

Special Transform 是在特定文类或功能范围内覆盖 Default Style 的写作规则。Writing Style Selection 只从 Active Transforms Index 选择 transform，并在 Writing Style Map 中记录其标签与 override scope。

Selected transform 的 override scope 必须引用 Linearized Structure Draft 中已经存在的 units 或 groups。Transform 可以在该 scope 内覆盖 Default Style 中适用的规则，但不得要求修改 Semantic Units、Semantic Grouping、relations、Presentation Order、hard presentation constraints、inference boundary、限定语、hedges 或技术术语。

每个 Semantic Unit 的 effective style source 必须是 Default Style 或一个 selected transform。不同 transforms 可以出现在同一 Deliverable 中，但其 override scopes 必须互不重叠。Selected transform 在其 scope 内成为 effective style source；Writing Style Map 的其他 directives 与通用 Prose constraints 继续适用。

Transform 正文由 Prose 在 Writing Style Gate Pass 后按 `file path` 读取并执行。未被 selected transform 覆盖的内容继续使用 Default Style、Default Language Profile 与相应的 Surface Realization。

## 5. Procedure

### 5.1 Determine the Default Language Profile

1. 读取 $T_W$ 中 applicable requirements 与 Deliverable Contracts。
2. 根据 artifact、audience、carrier 与 granularity，为每个 Deliverable 确定 Default Language Profile。
3. 当前对话中的普通 response 在没有特殊要求时采用直接、自然、适合当前任务与 audience 的交流方式。
4. 本步骤只建立 Deliverable-level selection，不修改 Draft 文本。

### 5.2 Read the Active Transforms Index

读取 Active Transforms Index 中的 `transform category / trigger features / applicable scope / file path`。本步骤不读取 `transforms/` 正文。

### 5.3 Detect Genre and Function Features

以 Linearized Structure Draft 中已经存在的 Semantic Unit 或 Semantic Group 为单位，结合 applicable requirements 与 Deliverable Contract，识别与 realization 及 transform selection 有关的文类和功能特征。

### 5.4 Select Surface Realization

1. 让每个 Semantic Unit 或 Semantic Group 继承所属 Deliverable 的 Default Language Profile。
2. Requirement 明确要求局部内容采用不同语言方式时，记录 local Language Profile override。
3. 根据现有 Semantic Grouping、relations、Presentation Order 与 carrier 选择 Surface Realization。
4. 使用能够保存既有结构并满足交付条件的最低形式成本，不为内容建立新的 grouping 或 Presentation Order。
5. 显式 hard requirement 已经指定呈现形式时，在其约束内完成 selection。

### 5.5 Select Special Transforms

1. 将 detected genre / function 与 Active Transforms Index 的 trigger features 和 applicable scope 匹配。
2. 对每个 Semantic Unit 只分配 Default Style 或一个 selected transform。
3. 多个 transforms 匹配同一范围时，先判断其 reader functions 是否分别属于现有的不同 units 或 groups；若是，则沿既有边界建立互不重叠的 scopes，并分别分配 transform。
4. 多个 transforms 仍匹配同一 unit 或 group 时，选择最能服务该范围主要 reader function 的一个 transform；没有占主导地位的 reader function 时，保留 Default Style。
5. 以 semantic group 为 scope 时，该 group 内全部 units 使用同一 transform，不再为其中的 unit 分配不同 transform。
6. Writing Style Map 不记录 transform candidate sets，也不建立相互重叠的 transform scopes。

### 5.6 Build the Writing Style Map

为每个 Deliverable 记录 Default Language Profile，并为全部内容建立无遗漏、无冲突的 map entries，记录 semantic unit or semantic group、Surface Realization、detected genre / function，以及适用时的 local Language Profile override、selected transform 和 override scope。每个需要输出的 unit 最终解析为 Default Style 或一个 selected transform；不同 transform 的 scopes 互不重叠。Linearized Structure Draft 在该过程中保持不变。

## 6. Writing Style Gate

Writing Style Gate 只检查 Writing Style Map 是否可以由 Prose 直接执行、是否与输入约束兼容，以及非默认 selection 是否必要。实际改写后的文本及 transform 的实际效果由 Prose Gate 检查。

### 6.1 Resolvability

- 每个 Deliverable 均有明确的 Default Language Profile；
- 每个需要输出的 unit 均能通过现有 unit 或 group 得到明确的 Surface Realization；
- map targets、local overrides、selected transforms 和 override scopes 均能解析到既有对象；
- group-level selection、local override 与 transform overlay 之间不存在无法确定结果的冲突；
- Prose 可以执行 Map，而不需要重新进行 Writing Style Selection。

### 6.2 Compatibility

- Map 满足 `target stages` 包含 Writing Style Selection 的 hard requirements，并在不与更高优先级约束冲突时落实 soft requirements；
- Default Language Profile、local overrides 与 Surface Realization 符合 Deliverable Contract 和 carrier 能力；
- Map 不要求修改 Linearized Structure Draft 的文本、Semantic Units、Semantic Grouping、relations、Presentation Order 或 hard presentation constraints；
- Map directives 不要求 Prose 丢失 inference boundary、限定语、hedges、attribution 或技术术语。

### 6.3 Selection Necessity

- 每个 local Language Profile override、非默认 Surface Realization 和 selected transform 均由 applicable requirement、Deliverable condition 或 detected genre / function 支持；
- 移除某项非默认 selection 后，如果 requirement compliance、结构可读性和信息呈现效率均不受影响，则该 selection 应被移除；
- Default Style 能够完成的内容不增加 transform 或额外格式。

## 7. Failure Routing

Map 无法形成明确有效指令时，返回 Writing Style Map construction。Selection 与 applicable requirements、Deliverable Contract 或 Structure 不兼容时，返回对应的 Language Profile、Surface Realization 或 transform selection。非默认 selection 没有必要时，删除或简化该 selection。

Writing Style Gate Fail 只返回 Writing Style Selection 内部对应步骤，不返回 Structure，也不进入 Prose 修改内容。所有修复只修改 Writing Style Map，Linearized Structure Draft 保持不变。本阶段不另行设置 retry 次数。

## 8. Handoff

Writing Style Gate Pass 后，未经修改的 Linearized Structure Draft 与通过检查的 Writing Style Map 一并交给 Prose。Writing Style Map 是 Prose 执行 Style-Guided Rewrite 的控制 artifact；Linearized Structure Draft 是其内容 artifact。

Task Contract 继续作为全流程持续可索引的只读控制 artifact 保留。Writing Style Selection 使用的 Task Contract projection 只服务本阶段，不作为 Handoff 传给下一阶段；Prose 从 Task Contract 读取与自身有关的 projection。

Prose 首先读取 `references/prose.md`。Writing Style Map 包含 selected transform 时，Prose 再按 Active Transforms Index 记录的 `file path` 读取相应 `transforms/` 正文，并只在 override scope 内执行其规则；其余内容按 Default Language Profile 与相应的 Surface Realization 实现。
