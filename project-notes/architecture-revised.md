# Writing Style Optimization：精简架构

## 0. 文档性质

本文只规定系统边界、处理阶段、阶段间接口和候选来源，不是最终 `SKILL.md`。任何新增规范必须经过人工审核；开源规则也只有在核对原文、适用范围和失败案例后才能进入 Skill。

参考：本项目用户确认的治理约束；`Superpowers/skills/writing-skills/SKILL.md` 的规则发现与测试方法。

## 1. 总体架构

```text
User message + relevant context
              |
              v
        Analyze Prompt
              |
        internal Task Contract
              |
              v
External task work -> Raw Information
                         |
                         v
              Form Output Structure
                         |
                  Semantic Plan
                         |
                         v
                       Prose
                         |
                         v
                    Final output

Special Transform: 按文类选择性覆盖 Structure / Prose，随后返回主流程检查
Evaluation: 位于运行时流程之外，用于决定候选规则是否进入 Skill
```

主流程是 `Analyze Prompt -> Form Output Structure -> Prose`。检索、计算、推理和 agent 协作不属于本 Skill；Special Transform 不是第四个必经阶段。

参考：本项目用户确认的架构；`Superpowers/skills/brainstorming/SKILL.md` 的阶段分离思路。只借用 phase separation，不继承其软件设计流程。
*这是brainstorming时使用的内容，主要的借鉴我觉得是他的文档写作格式。*

## 2. Analyze Prompt

### 2.1 目标

分析当前消息及与其相关的上下文，确定最终输出必须回应什么。当前消息可能延续、修正或替换上文，也可能把上文当作待分析材料；因此上下文是证据和约束来源，不是自动服从对象。

参考：本项目用户确认的修正；无外部 Skill 直接提供这一通用对话边界。

### 2.2 Task Contract

在开始组织输出前，静默形成最小内部契约：

```text
primary task
secondary tasks
central question or required outcome
relevant context and its role
locked decisions and constraints
output genre
relations among tasks
```

任务关系至少检查：顺序依赖、共享信息、包含关系和冲突。多任务不能退化成互不相干的清单；发生冲突时，优先保留用户当前明确修正和已锁定约束。

参考：`academic-writing-skills/skills/academic-writing-skills/SKILL.md` 的 writing contract 与 locked decisions；本项目将其缩减为通用 Task Contract。通用化措辞需人工批准后才能进入最终 Skill。

*不错，这个contract的思路很值得参考，不过我们需要思考一个task contract需要变成什么样，目前这个contract还是太像是structure部分的东西了。*

### 2.3 Skill 内的体现

Skill 只要求模型在 output shaping 前完成 Task Contract，并据此选择默认流程或 Special Transform。Task Contract 默认不输出，也不记录检索、agent 调度或 hidden reasoning；外部工作结束后，只把可用于回答的结果作为 `Raw Information` 交给 Structure。

参考：`academic-writing-skills/skills/academic-writing-skills/SKILL.md` 的 contract-before-drafting 与不暴露内部映射；本项目的运行时边界判断。

*我们需要明确给定一个Raw Information的定义，或者说需要摸清楚这个阶段AI到底是怎么实现的。不然没法准确的支配让他从这个skill跳出去 让后保存这个contract，最后搜索完信息之后再回到步骤3去做整合。*

## 3. Form Output Structure

### 3.1 Semantic Plan

Structure 不直接写成章节清单，而先形成语义计划。每个语义单位暂用以下字段：

```text
reader function: 该单位为读者完成什么
central claim/question: 它表达或解决什么
support: 使用 Raw Information 中的何种依据、解释或推导
inference boundary: 最多能推出什么，哪些不能推出
bridge: 与前后单位的真实关系
```

这些字段来自学术写作的 paragraph contract，但 `evidence` 被暂时扩展为 `support`，以容纳普通解释、比较、建议和推导。这一扩展是候选修改，不是已批准规则。

参考：`academic-writing-skills/skills/academic-writing-skills/SKILL.md` 的 “Draft from Explicit Writing Contracts”；`academic-writing-skills/skills/academic-writing-skills/references/lifecycle-and-routing.md` 的 planned-paragraph fields。

*Draft from Explicit Writing Contracts的复用太严重了，可以参考点别的东西。而且我们需要给出一个semantic unit的基本定义，是由这些字段定义unit还是定义完unit的具体实例后说明内部包含以下字段。而且本身要联系到2.2提出的那些contract，做不同skill页的关系勾连。
lifecycle-and-routing.md 是值得参考的，尤其是他Stage Gate的方案是很好的结构定义。我们也可以通过类似的gate结构来确保ai遵从我们的指令去得到具体的semantic字段。此外他对模型的规定也给我了启发，我们需要在写skill使用根据任务强度来分配sub-agents的模型水平。
*

### 3.2 关系与线性化

先确定语义关系，再决定线性呈现顺序：

- 层级：总括—展开、整体—部分；
- 协调：并列、对比、选择；
- 依赖：因果、条件—结果、主张—支持、解释、举例、限定或例外、时间或操作顺序。

标题、列表和段落只是这些关系的呈现方式，不能代替关系本身。复杂的条件、让步、交叉依赖或非线性关系必须由语言或图明确表达。

参考：`academic-writing-skills/skills/academic-writing-skills/references/universal-integrity.md` 的 transition relations 与 sentence/paragraph/section flow；`academic-writing-skills/skills/academic-writing-skills/references/lifecycle-and-routing.md` 的 cumulative argument。关系分类是本项目的归并，需在原文抽取阶段逐项核查。

*我觉得universal-integrity 用在这里或许不太合适。当然他提到的transition relation是本项目3.2的重要展开内容。但是universal-integrity这类文本本身存在的意义就值得我们参考。我们在跑完3.1-3.4后是否也需要一个universal-integrity类似的文本去审查当前structure文档是否交付完整，亦或者是在最开始的3.1利用universal-integrity这个思想去奠定整个步骤3的基调。*

*我们其实是对cumulative argument 进行更抽象层面的细化。我们提出的语义关系一定要覆盖的足够全，不要忽略任何一种语义关系以及基本关系外或许可以衍生出的复杂关系。此外three timelines也提醒我们一定要让ai区分，思考时的顺序和写作顺序不一定一样。我们就是要从他raw info的思考结果中抽离出这些语义关系，然后重构一段文字。*


### 3.3 可证伪检查

交换两个本应存在依赖关系的相邻语义单位：若逻辑、指代或推导几乎不受影响，应重新检查该依赖是否真实建立。并列单位允许互换，因此“可移动”本身不是错误；测试针对的是被声明为依赖关系的单位。

同时检查段落开头能否单独构成递进的结构摘要，以及全文是否在累计推进而非换词复述。

参考：`academic-writing-skills/skills/academic-writing-skills/references/universal-integrity.md` 的 paragraph-opening-sentence 与 cumulative-movement checks；交换测试是本项目提出的可证伪检验，尚待测试。

*我认为3.2的架构进一步保障了3.3可证伪检验的可行性。我们的思路要比universal-integrity的思路更为严谨，且更可证。*
### 3.4 Presentation Selection：默认项目对话

默认项目对话属于核心 Structure，而非 Special Transform。只有存在真实层级才使用标题；只有存在并列、步骤或比较关系才使用列表或表格。简单回答不建 section，一个自然段足够时不建 subsection；结构复杂度随逻辑复杂度增长，不随字数增长。

这一规则应放在核心 Skill 的 `Form Output Structure / Presentation Selection`，因为所有输出都需要判断是否采用格式；Special Transform 只能在其后增加文类特有约束。

参考：本项目用户确认的默认 profile 与 complexity gate；`simple-output-styles/plugin/output-styles/clarity-flow.md` 只用于检验信息流，不作为格式模板。

*我觉得这一部分应该独立出来，作为中间过渡阶段衔接Structure和Prose。街道上文给出的Semantic Plan和Logical Relation后，与其说叫Presentation Selection不如说是Writing Style Selection。这一部分应该描述default模式的基本结果/定义，并且对上文的structure进行style分类，确认哪个部分需要去引用Special Transformation。此外，还需要明确说出Special Transformation是可以对Defualt style进行覆盖的。*
## 4. Prose

Prose 接收已经冻结的 Semantic Plan，把各语义单位实现为句子和段落。它不得重新决定全局结构，不得新增 Raw Information 中不存在的 claim，并必须保留逻辑关系、限定语、hedges 和技术术语。若计划无法清楚实现，应退回 Structure，而不是用过渡词掩盖问题。

参考：`academic-writing-skills/skills/academic-writing-skills/references/universal-integrity.md` 的 integrity/flow checks；`SciWrite/SKILL.md` 的 content-preservation 与 terminology consistency。

*也就是说Prose要有一个check用的文档。使用这个文档检查当前的计划是否可以实现，if pass 则继续，fail就退回structure做修改。*

Prose 的主候选来源是 Humanizer：删除无信息的铺垫、虚假对照、重复收尾、伪格言、凭空树立的反方、夸张意义和 chatbot residue。判断单位是“该句是否增加信息或建立真实关系”，而不是命中某个词。

参考：`blader/humanizer/SKILL.md` 的 “Why AI text sounds the way it does”“How to work”及 patterns 1–5；采用其 structural anti-slop 诊断，不整体照搬全部 pattern。

*Humanizer大部分写的都很好，基本都可以采用，细节上可以在确定架构后讨论。*

句内信息流可补充 old-to-new、topic continuity 和 sentence-end-to-next-beginning；长句只在主谓关系、修饰范围或依赖变得难以解析时拆分。技术术语不得为了避免重复而更换同义词。

参考：`simple-output-styles/plugin/output-styles/clarity-flow.md`；`SciWrite/SKILL.md` 的 “Sentence Architecture”与“Keyword Consistency and Terminology”。

*clarity-flow 的old to new很重要。对技术术语的要求也很重要。*

不得采用全局短句化、one-idea-per-sentence、固定句长、全面主动语态、禁用破折号或词汇黑名单。它们只能作为 warnings，并依据语境决定是否修改。

参考：`simple-output-styles/plugin/output-styles/actionable-clarity.md` 作为反例/局部候选；`SciWrite/SKILL.md` 对 passive voice 与 sentence architecture 的例外；`blader/humanizer/SKILL.md` 的 contextual pattern judgment。

*这一类过于绝对的规则也需要逐个审核后再加入我们的skill中。*

## 5. Special Transforms

Special Transform 由 Task Contract 中的文类和用途触发，只覆盖必要的 Structure/Prose 规则，完成后仍须通过主流程的依赖、信息保真和术语检查。

参考：本项目用户确认的 routing 设计；`academic-writing-skills/skills/academic-writing-skills/SKILL.md` 的 lifecycle routing 可作为实现参考。

| 文类                           | 判定依据                                         | 候选来源与用法                                                                                                                                                                                                                                                   | 边界                                     |
| ---------------------------- | -------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------- |
| 科研写作                         | 需要 claim–evidence、inference boundary、论文级累计论证 | `academic-writing-skills/skills/academic-writing-skills/SKILL.md`；`academic-writing-skills/skills/academic-writing-skills/references/lifecycle-and-routing.md`；`academic-writing-skills/skills/academic-writing-skills/references/universal-integrity.md` | 架构可行但尚未完成人工逐条审查，不直接定稿                  |
| 数学写作                         | 定义、命题、证明、符号一致性构成主要表达约束                       | `Math-writing-skill/SKILL.md`                                                                                                                                                                                                                             | 由用户现有 Skill 管理；本地精确路径待确认，本项目不重写        |
| Procedure / runbook          | 读者按条件执行有序动作，并需识别成功或失败状态                      | `SimpleEnglish/skills/simple-english/SKILL.md` 的 procedural classification、condition-before-command、one-term-one-concept                                                                                                                                  | 可较直接采用 pragmatic mode；strict mode 仍需测试 |
| Reference / overview         | 读者主要查找概念、接口、配置或系统入口                          | `SimpleEnglish/skills/simple-english/SKILL.md` 的 descriptive mode 与 terminology rules；`simple-output-styles/plugin/output-styles/clarity-flow.md`                                                                                                         | 选择性简化，不强制 20/25 词限制                    |
| Architecture / specification | 核心信息是组件依赖、约束、exception、override 和 rationale  | `academic-writing-skills/skills/academic-writing-skills/references/universal-integrity.md` 的 dependency/flow；`SciWrite/SKILL.md` 的 terminology；SimpleEnglish 仅作局部压缩                                                                                       | 不允许短句规则切断依赖；现有来源不足以直接形成完整 transform    |

README 不能按文件名整体归类，应逐 section 判断其交际功能：安装步骤可走 Procedure，概念索引可走 Reference，设计原理与模块依赖应走 Architecture。DualTau 类长架构文档的暂定顺序是：恢复结构关系、删除结构性重复、统一术语、局部压缩、重新检查依赖；任何条件、例外、override 或模块关系受损都应撤回压缩。

参考：`SimpleEnglish/skills/simple-english/SKILL.md` 的 procedural/descriptive 区分；`academic-writing-skills/skills/academic-writing-skills/references/universal-integrity.md`；`SciWrite/SKILL.md`。README 三分法与 DualTau 顺序是本项目候选设计，需人工审核或继续检索来源。

`claude-plain-english` 不再进入候选集；`SimpleEnglish` 不作为 global style；`actionable-clarity` 不作为主流程基底。

参考：本项目已确认的来源筛选结论；`SimpleEnglish/skills/simple-english/SKILL.md` 与 `simple-output-styles/plugin/output-styles/actionable-clarity.md` 仅用于界定不采用范围。

## 6. 最小化与规则准入

核心 Skill 只保留跨文类成立的少量 invariants；文类规则进入 Special Transforms；统计特征和表面特征默认是 warnings，不是 bans。不得规定固定章节、段落数、句长、列表数或统一回答模板。

参考：本项目用户确认的最小化原则；`blader/humanizer/SKILL.md` 的 strong/weak signals；`Superpowers/skills/writing-skills/SKILL.md` 的 symptom-based、technology-agnostic rule design。

规则准入必须由人工控制：

1. 保存观察到的失败案例和候选来源原文；
2. AI 只做来源对照、冲突检查、测试与候选矩阵；
3. 人工标记 `Keep / Modify / Reject / Special-only / Needs-test`；
4. 新规范由用户起草或明确批准；
5. 只将批准文本写入最终 Skill，并用原场景和 held-out 场景复测。

同一失败已有规则覆盖时不增加同义规则。AI 不得把自己生成的大段规范直接写入最终 Skill；本架构文档也不构成这种授权。

参考：`Superpowers/skills/writing-skills/testing-skills-with-subagents.md` 的 baseline failure → intervention → retest；`simple-output-styles/evals/ARCHITECTURE.md` 与 `simple-output-styles/evals/RESULTS.md` 的 fidelity、hedge survival、held-out prompts 与 long-session drift；本项目用户确认的人工审批门。

*规则准入必须由人工控制这一部分可以直接删掉。整个skill开发近乎就是一次性的。我定稿之后不需要频繁更改。如果我发现问题我就会直接回来跟你讨论，然后作出适当修改。不需要你搞这些形式主义的审批流程，这不是一个大型的项目。*

## 7. 最小运行时与外部评测

运行时只保留四项：Task Contract、Semantic Plan、Presentation Selection、Prose invariants；Special Transform 按需加载。失败案例收集、规则比较、model/human judging 和回归测试全部放在 Skill 外，避免把开发方法变成每次回答的负担。

参考：`Superpowers/skills/writing-skills/SKILL.md` 的 skill discovery/渐进加载；`Superpowers/skills/writing-skills/testing-skills-with-subagents.md`；`simple-output-styles/evals/ARCHITECTURE.md`。
