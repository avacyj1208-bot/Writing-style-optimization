# Relation Inventory

## 1. Role and Boundary

This file is consulted by `references/structure.md` when Relation-guided Graph Construction needs to create edges. It provides reference relations, endpoint roles, directionality, possible hard-order implications, and minimal examples for the edge-labeling function $\lambda$.

The Relation Inventory is an open-ended reference for judgment, not a closed taxonomy. Structure should use an existing relation that preserves the current meaning; when no suitable definition exists, it should create a local relation that serves only the current Dependency Graph. A degenerate Graph with a single Semantic Unit and no edge does not require this file.

This file only supports the identification of relations between Semantic Units. It does not determine a fixed Semantic Unit granularity, directly generate Presentation Order, or prescribe connectives or sentence forms for Prose.

## 2. Relation Representation

Each reference relation is defined by the following elements:

| Element | Definition |
|---|---|
| Relation label | The stable relation type that $\lambda$ assigns to an edge. |
| Endpoint roles | The semantic roles carried by the Semantic Units at the two endpoints of the relation. |
| Core test | The minimum semantic condition for determining that the relation holds. |
| Directionality | Whether exchanging the endpoint roles changes the meaning of the relation. |
| Possible $H$ | A hard presentation constraint that the relation may produce in a particular task. |
| Example | Illustrates relation identification only; it does not prescribe output wording or Presentation Order. |

A **symmetric** relation retains the same semantic relation when its endpoints are exchanged. An **asymmetric** relation must preserve direction through its endpoint roles. Directionality belongs to relation semantics and does not automatically determine Presentation Order.

In `Possible H`, “no inherent order” means that the relation itself does not produce a hard presentation constraint. Structure writes the corresponding constraint into $H$ only when exchanging the order would damage comprehension, scope, or task completion.

## 3. Open-set Principles

### 3.1 Use Semantic Tests, Not Surface Cues

Determine a relation from the recoverable meaning between two content units, not from a single connective, punctuation mark, original adjacency, or the production order of Raw Material. The same connective can express different relations, and a relation can exist without an explicit connective.

### 3.2 Keep Only Structural Distinctions

A semantic distinction requires a separate relation label only when it changes endpoint roles, Graph interpretation, scope preservation, or a possible $H$.

Preserve negation, modality, quantifiers, hedges, certainty, and attribution scope primarily in Semantic Unit content and its links; do not mechanically derive new relation subtypes from them. Express relation direction through endpoint roles rather than creating separate labels for opposite directions.

### 3.3 Allow Multiple Relations When They Matter

The same pair of Semantic Units can have multiple edge instances. Retain multiple relations only when each preserves an independent meaning that affects the Graph; do not annotate the same meaning repeatedly when one relation can recover it completely.

### 3.4 Preserve Ambiguity When Evidence Is Insufficient

When relations cannot be distinguished reliably, do not force a finer label. Use a broader reference relation that preserves their shared meaning. If the broader relation would still lose meaning required by the current task, create a local relation and record its `preserved meaning`.

## 4. Core Reference Relations

The following families are used only for lookup and are not written into $\lambda$. The actual edge label is the relation label shown in each table.

### 4.1 Coordination and Logical Status

| Relation | Endpoint roles | Core test | Directionality | Possible $H$ | Example |
|---|---|---|---|---|---|
| `conjunction` | conjunct / conjunct | Both items contribute in parallel to the same superordinate content, and both are asserted, required, or retained. | Symmetric | No inherent order; add another relation when progression or dependency exists. | “Model accuracy increases” and “inference latency decreases” jointly constitute the result. |
| `alternative` | option / option | The two items are presented as optional paths, possible cases, or alternatives; mutual exclusion is not implied automatically. | Symmetric | No inherent order; preserve a user-specified priority through another constraint. | “Use a table” or “use a list.” |
| `entailment` | premise / conclusion | Under the currently accepted definitions and premises, accepting the premise requires accepting the conclusion; probabilistic support is insufficient. | Asymmetric | Set premise $<$ conclusion when the reader must follow the derivation; conclusion-first presentation remains possible through explicit back-reference. | “Every result that passes the Gate has lineage; $r$ passes the Gate” entails “$r$ has lineage.” |
| `equivalence` | expression / equivalent | Under the current scope and definitions, the two items express the same content and can replace each other without changing truth value or task meaning. | Symmetric | No inherent order. | “$x$ is even” and “$x\bmod 2=0$.” |
| `incompatibility` | proposition / proposition | The two items cannot both hold for the same object, time, and conditions. | Symmetric | No inherent order; when a choice is required, `alternative` may also be labeled. | “The same run has converged” and “the same run has not converged.” |

### 4.2 Contingency and Intention

| Relation | Endpoint roles | Core test | Directionality | Possible $H$ | Example |
|---|---|---|---|---|---|
| `condition` | condition / consequence | The truth, applicability, or execution of the consequence depends on the condition; the condition may be actual, hypothetical, or counterfactual. | Asymmetric | When the condition scope could be misread, set condition $<$ consequence or require an explicit marker. | “The sample size is sufficiently large” is a condition for “the normal approximation is valid.” |
| `cause-effect` | cause / effect | In the described world or model, the cause produces, contributes to, or changes the effect, rather than merely increasing belief in the effect. | Asymmetric | No inherent order; both cause-first and effect-first are possible. | “Training-data leakage” causes “inflated evaluation results.” |
| `means-purpose` | means / purpose | The means is an action, mechanism, or resource adopted by an agent to achieve the purpose; the relation is goal-directed. | Asymmetric | Operational instructions may produce a corresponding $H$ when the means must be executed first; explanatory text has no inherent order. | “Caching intermediate results” serves to “reduce repeated computation.” |

### 4.3 Argumentative Support and Limits

| Relation | Endpoint roles | Core test | Directionality | Possible $H$ | Example |
|---|---|---|---|---|---|
| `evidence` | evidence / claim | The evidence increases the credibility of the claim but does not make the claim logically necessary. | Asymmetric | No inherent order; both claim-first and evidence-first are possible. | “Three independent replications produced consistent results” supports “the effect is stable.” |
| `justification` | reason / decision | The reason supports the rationality of a judgment, recommendation, choice, or action rather than directly proving a descriptive claim true. | Asymmetric | No inherent order. | “The change would break compatibility” supports the decision to “retain the current interface.” |
| `qualification` | qualifier / claim | The qualifier narrows the claim's applicability, strength, or certainty without establishing a separate condition-consequence relation. | Asymmetric | When the scope of the qualifier and claim could be confused, they must remain adjacent or explicitly linked. | “Only for single-center data” qualifies “the conclusion holds.” |
| `exception` | exception / generalization | The exception identifies a local member or case within the set covered by the generalization to which the generalization does not apply. | Asymmetric | The exception must retain a recoverable scope over its generalization; their order may vary. | “Except for regularized models” qualifies “all models overfit.” |

### 4.4 Comparison and Opposition

| Relation | Endpoint roles | Core test | Directionality | Possible $H$ | Example |
|---|---|---|---|---|---|
| `similarity` | counterpart / counterpart | The two items share an emphasized similarity along an explicit or recoverable common dimension. | Symmetric | No inherent order; the comparison dimension must be recoverable. | Method A and Method B use the same training objective. |
| `contrast` | counterpart / counterpart | The two items have an emphasized difference along a common dimension; the difference does not require them to be mutually exclusive. | Symmetric | No inherent order; the comparison dimension must be recoverable. | Method A has low bias and high variance, whereas Method B has high bias and low variance. |
| `violated-expectation` | expectation basis / unexpected result | The expectation basis normally gives rise to an expectation, but the unexpected result contradicts that expectation; both items can still be true. | Asymmetric | No inherent order; the source of the expectation and the contradicted result must be recoverable. | “Training loss continues to decrease,” but “validation error increases instead.” |

### 4.5 Temporal and Operational Order

| Relation | Endpoint roles | Core test | Directionality | Possible $H$ | Example |
|---|---|---|---|---|---|
| `temporal-sequence` | earlier / later | The two items have an order in time, operation, or state transition. | Asymmetric | Set earlier $<$ later when the order itself determines executability or comprehension; retrospective narration may use the reverse order with explicit marking. | “First normalize the data,” then “fit the model.” |

### 4.6 Expansion and Interpretation

| Relation | Endpoint roles | Core test | Directionality | Possible $H$ | Example |
|---|---|---|---|---|---|
| `elaboration` | detail / topic | The detail adds a component, property, step, or explanation about the same topic, event, object, or process, rather than merely giving an instance or synonymous restatement. | Asymmetric | No inherent order; the detail must remain recoverably attached to the topic. | “The system uses a two-level cache”; specifically, “L1 is in memory, and L2 is on disk.” |
| `instantiation` | instance / general claim | The instance is a concrete member of the category, regularity, or case described by the general claim. Presentation Order does not change the endpoint roles. | Asymmetric | No inherent order; multiple instances may connect to the same general claim. | “BERT” is an instance of a “Transformer model.” |
| `definition` | definiens / term | The definiens specifies the meaning, decision criteria, or equivalent expression of the term in the current text. | Asymmetric | Set definition $<$ dependent use when the term would be incomprehensible before definition or later reasoning depends on it. | “The difference between the predicted and true values” defines the term “residual.” |
| `background` | context / focal content | The context provides a situation, prior state, or interpretive framework needed to understand the focal content but does not directly add internal details to that content. | Asymmetric | Set context $<$ focal content when the focal content would be misread without the context; otherwise, the order may vary. | “Data collection spans three versions” provides background for “the metric uses within-version normalization.” |

### 4.7 Source and Discourse Organization

| Relation | Endpoint roles | Core test | Directionality | Possible $H$ | Example |
|---|---|---|---|---|---|
| `attribution` | source / attributed content | The source is the originator, author, recording medium, or responsible party associated with the attributed content. The relation records attribution or provenance only; it does not express agreement, truth, proof, justification, or evidential support. | Asymmetric | The source and attributed content must retain an unambiguous scope; either may appear first. | “The experiment log” records that “training terminated at epoch 20.” |
| `question-answer` | question / answer | The answer directly satisfies or partially satisfies the information need posed by the question. | Asymmetric | When both endpoints are explicitly presented in Q/A form, normally set question $<$ answer. | “Which stage performs the rewrite?”—“Prose.” |
| `problem-solution` | problem / solution | The solution directly addresses, mitigates, or resolves the problem rather than merely standing in a causal relation to it. | Asymmetric | Set problem $<$ solution when the solution cannot be interpreted without the problem; a conclusion-first presentation may use an explicit back-reference. | “Long documents are difficult to maintain”—“use a layered index.” |

## 5. Relation Selection

For each semantic connection under consideration:

1. First determine the roles of the content at both endpoints under the current Requested Objectives; do not use textual order as the semantics of Arg1 / Arg2.
2. Use the inventory's Core test to determine whether the relation holds; treat a connective as only one piece of evidence.
3. Select a relation label that completely preserves the current meaning without introducing additional assumptions.
4. Record semantic direction through endpoint roles; use roles of the same type for a symmetric relation.
5. Independently determine whether the relation produces $H$ in the current Graph; do not automatically convert directionality into Presentation Order.
6. When one label cannot preserve all structural meaning, multiple edge instances may be created for the same pair of units; when the inventory cannot express the meaning accurately, create a local relation.

Only after relation identification does Structure determine Semantic Unit boundaries from the content involved in the relation and its endpoint roles.

## 6. Disambiguation Rules

### 6.1 `entailment` / `evidence` / `justification` / `cause-effect`

- If accepting the first item makes the second logically necessary: `entailment`;
- if the first item only increases the credibility of the second item's claim: `evidence`;
- if the first item makes a decision, recommendation, or action more reasonable: `justification`;
- if the first item produces or contributes to the second item in the described world: `cause-effect`.

The same content may be both a cause in the world and evidence in an argument. Create two edges only when both meanings affect the current Graph.

### 6.2 `condition` / `qualification`

`condition` establishes a dependency between a condition and a consequence. `qualification` only adjusts a claim's scope, strength, or certainty. Removing a qualifier makes the claim too strong but does not necessarily produce a counterfactual consequence.

### 6.3 `contrast` / `incompatibility` / `violated-expectation`

- A difference along a common dimension: `contrast`;
- inability to hold simultaneously under the same conditions: `incompatibility`;
- one item contradicts an expectation normally produced by the other: `violated-expectation`.

### 6.4 `elaboration` / `instantiation` / `equivalence` / `definition`

- Adds internal detail to the same topic: `elaboration`;
- connects a concrete member to a general category or regularity: `instantiation`;
- expresses the same content under the current scope: `equivalence`;
- specifies the in-text meaning of another term: `definition`.

`example` and `generalization` are not established as separate labels. Both use `instantiation`: the `instance / general claim` roles preserve direction, and Presentation Order determines whether the general content or the instance appears first.

### 6.5 `means-purpose` / `cause-effect` / `problem-solution`

- An action is directed toward a goal: `means-purpose`;
- one item actually produces or contributes to another: `cause-effect`;
- one item is proposed to address an identified problem: `problem-solution`.

### 6.6 `background` / `elaboration`

`background` provides the external framework needed to interpret the focal content. `elaboration` expands the topic itself. Context and focal content may involve different events, whereas detail and topic describe the same object, event, or process at different levels of granularity.

### 6.7 `alternative` / `incompatibility`

`alternative` indicates that the text presents two items as options or possibilities; it does not automatically indicate mutual exclusion. Add `incompatibility` only when the two items cannot both hold under the same conditions.

## 7. Local Relations

When none of the Core Reference Relations can preserve the meaning required by the current Graph, create a local relation according to `references/structure.md`:

| Field | Content |
|---|---|
| `descriptive name` | A task-local name that distinguishes the relation. |
| `endpoint roles` | The roles of the two Semantic Units in the relation. |
| `directionality` | The semantic direction of the relation. |
| `preserved meaning` | The meaning that the existing inventory cannot preserve but the current Graph must retain. |

A local relation serves only the current Dependency Graph. A single use does not automatically add it to this inventory. Adding a reference relation requires human confirmation of its reuse value across tasks and verification that the meaning cannot be expressed by an existing label, endpoint roles, or unit scope.

## 8. Source Basis

This inventory synthesizes the following sources without reproducing any of their complete taxonomies:

- William C. Mann and Sandra A. Thompson, [*Rhetorical Structure Theory: Toward a Functional Theory of Text Organization*](https://doi.org/10.1515/text.1.1988.8.3.243) (1988), together with the official RST [Introduction](https://www.sfu.ca/rst/01intro/intro.html) and [Relation Definitions](https://www.sfu.ca/rst/01intro/definitions.html): functional identification of relations, endpoint roles, Evidence, Background, Condition, Elaboration, Concession, Sequence, and the open-set principle.
- Ted Sanders, Wilbert Spooren, and Leo Noordman, [*Toward a Taxonomy of Coherence Relations*](https://doi.org/10.1080/01638539209544800) (1992): organizing coherence relations through a small number of cognitively identifiable primitives rather than an ever-expanding set of surface labels.
- Bonnie Webber, Rashmi Prasad, Alan Lee, and Aravind Joshi, [*The Penn Discourse Treebank 3.0 Annotation Manual*](https://catalog.ldc.upenn.edu/docs/LDC2019T05/PDTB3-Annotation-Manual.pdf) (2019): the high-level division into Temporal, Contingency, Comparison, and Expansion, argument directionality, and simplification of fine-grained senses that are rare or difficult to annotate consistently.
- Frank Schilder, [*An Underspecified Segmented Discourse Representation Theory*](https://aclanthology.org/P98-2194/) (ACL 1998): connecting discourse units with discourse relations to form a graph representation.
- Amir Zeldes et al., [*eRST: A Signaled Graph Theory of Discourse Relations and Organization*](https://aclanthology.org/2025.cl-1.3/) (*Computational Linguistics*, 2025): a modern implementation basis for graphs, non-projective relations, and concurrent relations.
