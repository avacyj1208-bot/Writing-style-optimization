# Relation Inventory

## 1. Role and Boundary

本文件由 `references/structure.md` 在 Relation-guided Graph Construction 需要建立 edges 时查询。它为 edge-labeling function $\lambda$ 提供 reference relations、endpoint roles、directionality、可能的 hard-order implications 和最小示例。

Relation Inventory 是开放的判断参考，不是封闭 taxonomy。Structure 应使用能够保存当前含义的既有 relation；没有合适定义时，建立只服务当前 Dependency Graph 的 local relation。单一 Semantic Unit、无 edge 的退化 Graph 不需要查询本文件。

本文件只帮助识别 Semantic Units 之间的关系，不决定 Semantic Unit 的固定粒度，不直接生成 Presentation Order，也不规定 Prose 的连接词或句式。

## 2. Relation Representation

每条 reference relation 由以下内容定义：

| 内容 | 定义 |
|---|---|
| Relation label | $\lambda$ 写入 edge 的稳定 relation type。 |
| Endpoint roles | Relation 两端 Semantic Units 各自承担的语义角色。 |
| Core test | 判断该 relation 是否成立的最低语义条件。 |
| Directionality | Relation 是否因交换 endpoint roles 而改变含义。 |
| Possible $H$ | 该 relation 在特定任务中可能产生的 hard presentation constraint。 |
| Example | 只用于说明关系判定，不规定输出措辞或 Presentation Order。 |

**Symmetric** relation 在交换 endpoints 后保持同一种语义关系；**asymmetric** relation 必须通过 endpoint roles 保存方向。Directionality 属于 relation semantics，不自动等于 Presentation Order。

`Possible H` 中的“无内在顺序”表示该 relation 本身不产生 hard presentation constraint。只有交换顺序会破坏理解、scope 或任务完成时，Structure 才把相应约束写入 $H$。

## 3. Open-set Principles

### 3.1 Use Semantic Tests, Not Surface Cues

Relation 根据两个内容单元之间可恢复的含义判断，不根据单个 connective、标点、原始相邻关系或 Raw Material 的产生顺序判断。同一 connective 可以表达不同 relations；没有显式 connective 也可以存在 relation。

### 3.2 Keep Only Structural Distinctions

只有当一个语义差异会改变 endpoint roles、Graph interpretation、scope preservation 或可能的 $H$ 时，才需要不同 relation label。

Negation、modality、quantifier、hedge、certainty 和 attribution scope 优先保留在 Semantic Unit content 及其 links 中，不为其机械派生新的 relation subtype。Relation 的方向通过 endpoint roles 表达，不为正反方向建立两套 labels。

### 3.3 Allow Multiple Relations When They Matter

同一对 Semantic Units 可以存在多条 edge instances。只有每条 relation 都保存独立且影响 Graph 的含义时才并存；能够由一个 relation 完整恢复的含义不重复标注。

### 3.4 Preserve Ambiguity When Evidence Is Insufficient

Relation 无法可靠区分时，不强行选择更细标签。可以采用能够保存共同含义的较粗 reference relation；如果较粗 relation 仍会损失当前任务需要的语义，则建立 local relation，并记录 `preserved meaning`。

## 4. Core Reference Relations

下列 family 只用于检索，不写入 $\lambda$。实际 edge label 使用表中的 relation label。

### 4.1 Coordination and Logical Status

| Relation | Endpoint roles | Core test | Directionality | Possible $H$ | Example |
|---|---|---|---|---|---|
| `conjunction` | conjunct / conjunct | 两项对同一上位内容作并列贡献，并且均被断言、要求或保留。 | Symmetric | 无内在顺序；存在递进或依赖时应另标 relation。 | “模型精度提高”与“推理延迟下降”共同构成结果。 |
| `alternative` | option / option | 两项被呈现为可选路径、可能情况或替代方案；不自动表示互斥。 | Symmetric | 无内在顺序；用户指定优先级时由其他 constraint 保存。 | “使用表格”或“使用列表”。 |
| `entailment` | premise / conclusion | 在当前接受的定义与前提下，接受 premise 后必须接受 conclusion；概率性支持不足以成立。 | Asymmetric | 读者必须跟随推导时可设 premise $<$ conclusion；结论先行仍可通过明确回溯实现。 | “所有通过 Gate 的结果都有 lineage；$r$ 通过 Gate”蕴含“$r$ 有 lineage”。 |
| `equivalence` | expression / equivalent | 两项在当前 scope 与 definitions 下表达同一内容，可以相互替换而不改变真值或任务含义。 | Symmetric | 无内在顺序。 | “$x$ 是偶数”与“$x\bmod 2=0$”。 |
| `incompatibility` | proposition / proposition | 两项在同一对象、时间与条件下不能同时成立。 | Symmetric | 无内在顺序；需要作选择时可同时标 `alternative`。 | “同一 run 已收敛”与“同一 run 未收敛”。 |

### 4.2 Contingency and Intention

| Relation | Endpoint roles | Core test | Directionality | Possible $H$ | Example |
|---|---|---|---|---|---|
| `condition` | condition / consequence | consequence 的成立、适用或执行依赖 condition；condition 可以是现实、假设或反事实条件。 | Asymmetric | 条件 scope 若可能误读，可设 condition $<$ consequence 或要求显式标记。 | “样本量足够大”是“正态近似有效”的条件。 |
| `cause-effect` | cause / effect | cause 在所述世界或模型中产生、促成或改变 effect，而不只是提高对 effect 的相信程度。 | Asymmetric | 无内在顺序；cause-first 与 effect-first 均可。 | “训练数据泄露”导致“评估结果被抬高”。 |
| `means-purpose` | means / purpose | means 是主体为实现 purpose 而采用的行动、机制或资源；关系包含目标导向。 | Asymmetric | 操作指令需要先执行 means 时可形成相应 $H$；说明文本无内在顺序。 | “缓存中间结果”用于“减少重复计算”。 |

### 4.3 Argumentative Support and Limits

| Relation | Endpoint roles | Core test | Directionality | Possible $H$ | Example |
|---|---|---|---|---|---|
| `evidence` | evidence / claim | evidence 提高 claim 的可信度，但不使 claim 成为逻辑必然。 | Asymmetric | 无内在顺序；claim-first 与 evidence-first 均可。 | “三次独立复现实验得到一致结果”支持“效果具有稳定性”。 |
| `justification` | reason / decision | reason 支持一个判断、建议、选择或行动的合理性，而不是直接证明描述性 claim 为真。 | Asymmetric | 无内在顺序。 | “修改会破坏兼容性”支持“保留现有接口”的决定。 |
| `qualification` | qualifier / claim | qualifier 缩小 claim 的适用范围、强度或确定性，但不建立独立的条件后果关系。 | Asymmetric | qualifier 与 claim 的 scope 可能混淆时，二者必须邻接或显式关联。 | “仅对单中心数据”限定“该结论成立”。 |
| `exception` | exception / generalization | exception 指出 generalization 所覆盖集合中不适用的局部成员或情形。 | Asymmetric | exception 必须与其 generalization 保持可恢复的 scope；顺序可变。 | “除正则化模型外”限定“所有模型均过拟合”。 |

### 4.4 Comparison and Opposition

| Relation | Endpoint roles | Core test | Directionality | Possible $H$ | Example |
|---|---|---|---|---|---|
| `similarity` | counterpart / counterpart | 两项在明确或可恢复的共同维度上具有被强调的相似性。 | Symmetric | 无内在顺序；比较维度必须可恢复。 | 方法 A 与方法 B 都使用相同训练目标。 |
| `contrast` | counterpart / counterpart | 两项在共同维度上存在被强调的差异；差异本身不要求二者互斥。 | Symmetric | 无内在顺序；比较维度必须可恢复。 | 方法 A 低偏差高方差，方法 B 高偏差低方差。 |
| `violated-expectation` | expectation basis / unexpected result | expectation basis 通常引出某一预期，但 unexpected result 否定该预期；两项本身仍可同时为真。 | Asymmetric | 无内在顺序；预期来源和被否定结果必须可恢复。 | “训练损失持续下降”，但“验证误差反而上升”。 |

### 4.5 Temporal and Operational Order

| Relation | Endpoint roles | Core test | Directionality | Possible $H$ | Example |
|---|---|---|---|---|---|
| `temporal-sequence` | earlier / later | 两项在时间、操作或状态迁移上具有先后关系。 | Asymmetric | 当先后本身决定可执行性或理解时设 earlier $<$ later；回顾性叙述可通过明确标记采用相反顺序。 | “先标准化数据”，随后“拟合模型”。 |

### 4.6 Expansion and Interpretation

| Relation | Endpoint roles | Core test | Directionality | Possible $H$ | Example |
|---|---|---|---|---|---|
| `elaboration` | detail / topic | detail 为同一 topic、事件、对象或过程补充组成、属性、步骤或说明，但不只是给出实例或同义改写。 | Asymmetric | 无内在顺序；detail 必须保持对 topic 的可恢复依附。 | “系统使用两级缓存”；其中“L1 位于内存，L2 位于磁盘”。 |
| `instantiation` | instance / general claim | instance 是 general claim 所述类别、规律或情形的具体成员。Presentation Order 不改变两端角色。 | Asymmetric | 无内在顺序；多个 instances 可共同连接同一 general claim。 | “BERT”是“Transformer 模型”的实例。 |
| `definition` | definiens / term | definiens 规定 term 在当前文本中的含义、判定条件或等价表达。 | Asymmetric | term 在未定义时不可理解，或后续推理依赖该定义时，可设 definition $<$ dependent use。 | “预测值与真实值之差”定义术语“residual”。 |
| `background` | context / focal content | context 提供理解 focal content 所需的情境、先验状态或解释框架，但不直接补充 focal content 的内部细节。 | Asymmetric | focal content 离开 context 会被误解时，可设 context $<$ focal content；否则顺序可变。 | “数据采集跨越三个版本”构成“指标采用版本内标准化”的背景。 |

### 4.7 Source and Discourse Organization

| Relation | Endpoint roles | Core test | Directionality | Possible $H$ | Example |
|---|---|---|---|---|---|
| `attribution` | source / attributed content | source 是 attributed content 的提出者、记录载体或责任归属。Relation 不表示 source 赞同之外的证据强度。 | Asymmetric | source 与 attributed content 必须保持无歧义的 scope；先后顺序可变。 | “实验日志”记录“训练在第 20 轮终止”。 |
| `question-answer` | question / answer | answer 直接满足或部分满足 question 提出的信息需求。 | Asymmetric | 两端均显式以 Q/A 形式呈现时通常设 question $<$ answer。 | “哪个阶段负责改写？”—“Prose。” |
| `problem-solution` | problem / solution | solution 直接处理、缓解或解决 problem，而不只与其发生因果联系。 | Asymmetric | solution 离开 problem 无法解释时可设 problem $<$ solution；结论先行时可显式回指。 | “长文档难以维护”—“使用分层索引”。 |

## 5. Relation Selection

对每个待判断的 semantic connection：

1. 先确定两端内容在当前 Requested Objectives 下各自承担的角色，不沿用文本出现顺序作为 Arg1 / Arg2 语义。
2. 使用 inventory 的 Core test 判断 relation 是否成立；connective 只作为证据之一。
3. 选择能够完整保存当前含义且不引入额外假设的 relation label。
4. 通过 endpoint roles 写入语义方向；对 symmetric relation 使用同类 roles。
5. 独立判断该 relation 在当前 Graph 中是否产生 $H$，不把 directionality 自动转换为 Presentation Order。
6. 一个 label 无法保存全部结构性含义时，可以为同一对 units 建立多条 edge instances；无法由 inventory 准确表达时建立 local relation。

Relation 判断完成后，Structure 才依据 relation 涉及的内容和 endpoint roles 确定 Semantic Unit boundaries。

## 6. Disambiguation Rules

### 6.1 `entailment` / `evidence` / `justification` / `cause-effect`

- 接受前项后后项逻辑上必须成立：`entailment`；
- 前项只提高后项 claim 的可信度：`evidence`；
- 前项使决定、建议或行动更合理：`justification`；
- 前项在所述世界中产生或促成后项：`cause-effect`。

同一内容可能同时构成世界中的 cause 和说理中的 evidence；只有两种含义都影响当前 Graph 时才建立两条 edges。

### 6.2 `condition` / `qualification`

`condition` 建立 condition 与 consequence 的依赖；`qualification` 只调整 claim 的 scope、strength 或 certainty。把 qualifier 删除会使 claim 过强，但不一定产生一个反事实 consequence。

### 6.3 `contrast` / `incompatibility` / `violated-expectation`

- 共同维度上的差异：`contrast`；
- 同一条件下不能同时成立：`incompatibility`；
- 一项否定由另一项通常引出的预期：`violated-expectation`。

### 6.4 `elaboration` / `instantiation` / `equivalence` / `definition`

- 为同一 topic 增加内部细节：`elaboration`；
- 具体成员与一般类别或规律相连：`instantiation`；
- 两项在当前 scope 内表达同一内容：`equivalence`；
- 一项规定另一术语的文本内含义：`definition`。

`example` 与 `generalization` 不建立为两个独立 labels；二者均使用 `instantiation`，由 `instance / general claim` roles 保存方向，并由 Presentation Order 决定一般内容或实例先出现。

### 6.5 `means-purpose` / `cause-effect` / `problem-solution`

- 行动由目标导向：`means-purpose`；
- 一项实际产生或促成另一项：`cause-effect`；
- 一项被提出用于处理已经识别的问题：`problem-solution`。

### 6.6 `background` / `elaboration`

`background` 提供解释 focal content 所需的外部框架；`elaboration` 展开 topic 本身。Context 与 focal content 可以涉及不同事件，detail 与 topic 则描述同一对象、事件或过程的不同粒度。

### 6.7 `alternative` / `incompatibility`

`alternative` 表示文本把两项作为选项或可能性；它不自动表示互斥。只有两项在相同条件下不能同时成立时，才另有 `incompatibility`。

## 7. Local Relations

当 Core Reference Relations 均不能保存当前 Graph 需要的含义时，按 `references/structure.md` 建立 local relation：

| 字段 | 写入内容 |
|---|---|
| `descriptive name` | 能够区分该 relation 的任务内名称。 |
| `endpoint roles` | 两端 Semantic Units 在该 relation 中的角色。 |
| `directionality` | Relation 的语义方向。 |
| `preserved meaning` | 既有 inventory 无法保存、但当前 Graph 必须保留的含义。 |

Local relation 只服务当前 Dependency Graph。它不因一次使用自动进入本 inventory；新增 reference relation 需要人工确认其跨任务复用价值，并检查是否能够由现有 label、endpoint roles 或 unit scope 表达。

## 8. Source Basis

本 inventory 综合以下来源，但不复制其中任何一套完整 taxonomy：

- William C. Mann 与 Sandra A. Thompson，[*Rhetorical Structure Theory: Toward a Functional Theory of Text Organization*](https://doi.org/10.1515/text.1.1988.8.3.243)（1988），以及 RST 官方 [Introduction](https://www.sfu.ca/rst/01intro/intro.html) 与 [Relation Definitions](https://www.sfu.ca/rst/01intro/definitions.html)：relation 的功能判定、endpoint roles、Evidence、Background、Condition、Elaboration、Concession、Sequence 与 open-set 原则。
- Ted Sanders、Wilbert Spooren 与 Leo Noordman，[*Toward a Taxonomy of Coherence Relations*](https://doi.org/10.1080/01638539209544800)（1992）：以少量认知上可判定的 primitives 组织 coherence relations，而不是依赖不断扩张的表面标签。
- Bonnie Webber、Rashmi Prasad、Alan Lee 与 Aravind Joshi，[*The Penn Discourse Treebank 3.0 Annotation Manual*](https://catalog.ldc.upenn.edu/docs/LDC2019T05/PDTB3-Annotation-Manual.pdf)（2019）：Temporal、Contingency、Comparison、Expansion 的上层划分，argument directionality，以及对稀有或难以一致标注的细粒度 senses 的简化。
- Frank Schilder，[*An Underspecified Segmented Discourse Representation Theory*](https://aclanthology.org/P98-2194/)（ACL 1998）：以 discourse relations 连接 discourse units 并形成 graph representation。
- Amir Zeldes 等，[*eRST: A Signaled Graph Theory of Discourse Relations and Organization*](https://aclanthology.org/2025.cl-1.3/)（*Computational Linguistics*, 2025）：graph、non-projective relations 与 concurrent relations 的现代实现依据。
