# Structure

## 1. Role and Boundary

This file is loaded by `SKILL.md` after Raw Material has been formed. Structure receives Raw Material and the projection of the Task Contract for this stage, builds the Dependency Graph, and produces the Linearized Structure Draft through Linearization.

Structure uses Model II. Relation-guided Graph Construction produces the Dependency Graph and passes it to Linearization through Model I. After Linearization produces the Linearized Structure Draft, the Structure Gate checks it. Pass proceeds to Writing Style Selection; Fail returns to the corresponding step within Structure.

This stage only identifies and realizes structural relations between content. It does not modify the Task Contract or Raw Material, add facts, claims, or inferences that do not exist in Raw Material, or perform Writing Style Selection or Prose Rewrite.

## 2. Input Contract

Structure uses two types of input: Raw Material is the content input, and the stage projection of the Task Contract is the control input.

### 2.1 Raw Material

Raw Material is a discrete, unordered set of results that has gone through the External Work Gate. It preserves Generation Lineage, Work Item Links, and Objective Links, and may contain existing states such as unresolved, conflict, scope, hedges, attribution, inference boundary, and technical terms.

Here, “unordered” means only that Raw Material has not yet formed a Dependency Graph or Presentation Order. The production order of Work Results does not constitute the order of the written output.

### 2.2 Task Contract Projection

The projection of the Task Contract for Structure is denoted by:

$$
T_S=\pi_{\mathrm{Structure}}(\text{Task Contract}).
$$

$T_S$ includes:

- Requested Objectives;
- Task Relations;
- requirements whose `target stages` include Structure;
- Deliverable Contracts.

Structure reads this content from the Task Contract, which remains continuously indexable throughout the workflow. The projection is not written into Raw Material or rewritten by Structure.

### 2.3 Relation Inventory

The Relation Inventory is an open-ended reference for judgments during Graph Construction. Structure consults `references/relation-inventory.md` only when building a Dependency Graph that contains edges. A degenerate Graph with a single Semantic Unit and no edge does not require the inventory.

Relation definitions, endpoint roles, directionality, possible hard-order implications, and examples are defined in `references/relation-inventory.md`; this file does not repeat the relation taxonomy.

## 3. Output Contract

Structure produces two distinct artifacts:

- **Dependency Graph**: the output of Relation-guided Graph Construction and the input to Linearization;
- **Linearized Structure Draft**: the output of Linearization and the content artifact passed to Writing Style Selection after the Structure Gate passes.

The Dependency Graph records Semantic Units, relations, and hard presentation constraints. The Linearized Structure Draft realizes these structural constraints as basically fluent, readable linear text, but does not perform Writing Style Selection or Prose Rewrite.

## 4. Artifact Definitions

### 4.1 Structure Projection

The elements of the Task Contract projection serve the following functions within Structure:

| Element | Content read by this stage |
|---|---|
| Requested Objectives | The user-visible content objectives that Structure must organize and cover in the structure. |
| Task Relations | The established execution and organizational relations among Derived Work Items. |
| Structure requirements | Requirements whose `target stages` include Structure and that must be implemented by this stage. |
| Deliverable Contracts | The boundaries of the deliverables that Structure must ultimately serve. |

These objects remain part of the Task Contract; Structure reads only their projection.

### 4.2 Dependency Graph

Relation-guided Graph Construction is denoted by:

$$
S_1:(M,T_S;\mathcal R)\mapsto D=(U,E,\lambda,H),
$$

where $M$ is Raw Material, $T_S$ is the projection of the Task Contract for Structure, and $\mathcal R$ is the open Relation Inventory.

The Dependency Graph consists of:

| Object | Definition |
|---|---|
| $U$ | The set of Semantic Units. |
| $E$ | Untyped edge instances that connect Semantic Units; multiple edges are allowed between the same pair of units. |
| $\lambda$ | The edge-labeling function, which assigns each edge a relation type and the endpoint roles at both ends. |
| $H$ | Hard presentation constraints derived from some labeled relations and the current task. |

### 4.3 Semantic Units

A Semantic Unit is content involved in an identified reference relation or local relation under the current Requested Objectives. Its boundary must allow the unit and labeled relations to preserve the original meaning together.

Each Semantic Unit preserves:

| Element | Function |
|---|---|
| Unit content | The content through which the unit participates in the current relations. |
| Objective Links | The Requested Objectives served by the unit. |
| Material Links | The Raw Material on which the unit is based. |

Raw Material and Semantic Units do not maintain a fixed numerical correspondence. One Raw Material item may form multiple units, and multiple Raw Material items may jointly form one unit. One unit may participate in multiple relations; multiple independent Objectives may form disconnected components; a simple task may form a degenerate Graph with a single unit and no edge.

### 4.4 Edges and Edge Labels

An edge instance in $E$ indicates only that a connection exists between two Semantic Units; it carries no relation type. $\lambda$ assigns each edge a relation type and roles for both endpoints. Different relations may produce multiple edges between the same pair of units.

The semantic direction of a relation does not automatically determine Presentation Order. Endpoint roles can determine the semantic status of each unit in the relation, but they do not automatically prescribe the units' order in linear text.

### 4.5 Reference Relations and Local Relations

The label of each edge belongs to:

$$
\lambda(e)\in
\mathcal R_{\mathrm{reference}}
\cup
\mathcal R_{\mathrm{local}}.
$$

S1 should first use a relation from `references/relation-inventory.md` that preserves the current meaning. When no suitable definition exists, it may create a local relation that serves only the current Dependency Graph. Each local relation records:

| Field | Content |
|---|---|
| `descriptive name` | A task-local name that distinguishes the relation. |
| `endpoint roles` | The roles carried by the Semantic Units at both endpoints of the relation. |
| `directionality` | The semantic direction of the relation. |
| `preserved meaning` | The meaning that the relation must preserve in the current Graph. |

A local relation is not automatically written back to the Relation Inventory; later human review determines whether it should be included as a general relation.

### 4.6 Hard Presentation Constraints

$H$ stores only presentation constraints whose violation would damage comprehension or task completion. The semantic direction of a relation does not automatically become a hard presentation constraint; the same relation may use different Presentation Orders according to the current Objectives and Graph.

### 4.7 Linearized Structure Draft

Linearization is denoted by:

$$
S_2:D\mapsto L,
$$

where $L$ is the Linearized Structure Draft. It already consists of basically fluent, readable sentences rather than a mechanical sequence of unit IDs.

The Linearized Structure Draft contains:

| Structural property | Definition |
|---|---|
| Presentation Order | The order of Semantic Units in the linear text. |
| Semantic Grouping | How units are organized into sections, paragraphs, or other semantic groups. |
| Relation Realization | How Graph relations are realized through order, adjacency, grouping, or explicit language. |

## 5. Procedure

### 5.1 Build the Dependency Graph

1. Follow Objective Links to review the Raw Material related to each Requested Objective.
2. Examine semantic connections among Raw Material content that may affect splitting, connection, or ordering. At this point, only locate content connections that require judgment; do not yet determine relation types. Task Relations remain execution and organizational relations among Derived Work Items.
3. Consult the Relation Inventory and identify a reference relation that preserves the current meaning for each semantic connection under consideration.
4. When no suitable reference relation exists, create a local relation that serves only the current Graph, and specify its descriptive name, endpoint roles, directionality, and preserved meaning.
5. Determine Semantic Units from the content and endpoint roles involved in the identified reference relation or local relation, and preserve Objective Links and Material Links.
6. Create untyped edge instances between the units that correspond to the relation endpoints, and use $\lambda$ to assign the relation type and endpoint roles.
7. Determine $H$ from the labeled relations and the current task.

Graph Construction follows the Requested Objectives Oriented principle. The reconstruction from Raw Material into Semantic Units is organized around Objectives and does not inherit the number or partitioning of Raw Material items.

### 5.2 Linearize the Dependency Graph

1. Select a Presentation Order for the Semantic Units that satisfies $H$.
2. For relations that allow multiple orders, select the direction of expression used in the current text.
3. Organize related units into sections, paragraphs, or other semantic groups.
4. Realize important relations in the Graph through order, adjacency, grouping, connectives, or necessary complete explanations.
5. Render the structural result as a basically fluent, readable Linearized Structure Draft.

Linearization determines written order from Requested Objectives, Graph relations, and hard presentation constraints; it does not directly inherit the production order of Raw Material. The Dependency Graph may be represented through text, labels, and local structure and need not maintain a purely symbolic state detached from language.

## 6. Structure Gate

The Structure Gate separately checks the Dependency Graph produced by S1 and the Linearized Structure Draft produced by S2.

### 6.1 Graph Construction Checks

- Each Requested Objective has corresponding Semantic Units that address it;
- each unit preserves Objective Links and Material Links;
- each edge has a relation type and endpoint roles assigned by $\lambda$;
- the meaning of each local relation is sufficiently identifiable;
- the units and labeled relations can jointly recover the original scope, hedges, attribution, inference boundary, and conflict state.

### 6.2 Linearization Checks

- The Linearized Structure Draft covers the units that must appear in the output;
- Presentation Order satisfies $H$;
- Graph relations can be recovered through order, adjacency, grouping, or explicit language;
- the production order of Raw Material has not been mistaken for Presentation Order;
- the structure forms cumulative progression, with complexity proportional to the actual relations.

### 6.3 Falsifiable Checks

Perform two types of checks against the Dependency Graph:

- Exchange units governed by a hard constraint, or delete or reverse the related edge. If logic, reference, derivation, scope, and completion conditions all remain unaffected, the corresponding constraint or relation may not have been genuinely established;
- exchange units considered independent. If this produces a clear loss, the Graph may have omitted a relation or hard constraint.

## 7. Failure Routing

Problems with the Graph, Semantic Units, edges, $\lambda$, local relations, or $H$ return to Relation-guided Graph Construction. Problems with Presentation Order, Semantic Grouping, or Relation Realization return to Linearization.

A Structure Gate Fail returns only to the corresponding step within Structure. It does not return to Analyze Prompt or External Work or enter Writing Style Selection to modify content. This stage does not set an additional retry limit.

## 8. Handoff

After the Structure Gate passes, the Linearized Structure Draft becomes the content artifact passed directly from Structure to Writing Style Selection.

The Task Contract remains a continuously indexable, read-only control artifact throughout the workflow. The Task Contract projection used by Structure serves only this stage and is not passed to the next stage as part of the Handoff; Writing Style Selection reads its own projection from the Task Contract.

Writing Style Selection may build a Writing Style Map from the Linearized Structure Draft, but it must not change the Semantic Units, relations, Presentation Order, or hard presentation constraints that have passed the Structure Gate.
