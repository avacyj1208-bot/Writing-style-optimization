---
name: writing-style-optimization
description: Applies to any task whose primary deliverable is natural-language text, including answers, explanations, analyses, discussions, recommendations, reports, documentation, drafting, rewriting, and editing. Manages the complete output workflow through task identification and External Work handoff, dependency-structure reconstruction, Writing Style Selection, and Prose Rewrite, while preserving facts, scope, hedges, attribution, inference boundaries, and technical terminology. Do not use when the primary deliverable is code, configuration, formulas, or structured data; accompanying natural-language explanations remain in scope.
---
# Writing Style Optimization

## Scope

This Skill manages the complete workflow for natural-language output, including task identification, information handoff, structure reconstruction, writing approach selection, and prose realization. This Skill does not directly perform retrieval, calculation, or reasoning; it only specifies the input, output, and handoff interfaces for this work in its documentation. The actual work is performed by External Agents / Tools.

## Core Constraints

- Use Requested Objectives as the organizing axis of the entire workflow. Derived Work Items, Work Results, Semantic Units, and Deliverables are all reorganized around Objectives; stages pass content and its necessary links forward, and no fixed one-to-one correspondence is required among object types.
- Strictly distinguish the Task Contract, Raw Material, Dependency Graph, Linearized Structure Draft, Writing Style Map, and Final Output. Each stage modifies only the artifact for which it is responsible.
- After Work Results produced by External Agents / Tools have passed the External Work Gate and been organized into Raw Material, subsequent stages must not invent facts, claims, or inferences absent from it, or lose scope, hedges, attribution, inference boundaries, conflict states, or technical terminology.
- A Gate belongs to the stage it checks. A Gate Fail returns only to that stage and does not establish routine cross-stage rollback.
- Strictly follow the processing order specified in the documentation. Apart from the local rollback defined by each stage's Gate, do not skip or invert stages or redo work across stages.

## Control Models

Use two basic processing models:

- **Model I — Direct Pass**: after a step produces an artifact, it passes the artifact directly to the next step. This model is used for stage connections or internal transformations that do not require a local quality decision.
- **Model II — Gate Check Loop**: after a step produces an artifact, the Gate for that stage checks it. Pass continues the workflow; Fail returns to that stage for another attempt.

External Work, Structure, Writing Style Selection, and Prose use Model II. All other connections use Model I.

## Workflow and File Loading

Every task begins with Analyze Prompt and follows the same stage sequence. The loading condition for a supporting file determines only when the details of a stage are read, not whether the stage is executed. For a small task, each stage may produce a minimal or degenerate artifact, but no stage may be skipped.

1. Read `references/task-and-work.md` when Analyze Prompt begins.
2. Read `references/structure.md` after Raw Material has been formed.
3. Consult `references/relation-inventory.md` when building a Dependency Graph that contains edges. A degenerate Graph with a single unit and no edges does not require the Relation Inventory.
4. Read `references/writing-style-selection.md` after the Structure Gate passes.
5. Writing Style Selection uses the Active Transforms Index in `references/writing-style-selection.md` to learn the available transform categories, trigger features, applicable scopes, and file locations. Selection records only the selected transform and override scope in the Writing Style Map; it does not read the bodies of files in `transforms/`.
6. Read `references/prose.md` after the Writing Style Gate passes. Prose reads the corresponding file in `transforms/` only when the Writing Style Map contains a selected transform, and completes the rewrite according to its rules.

Every task begins with Analyze Prompt; only the intermediate objects may be small. For example:

```text
one Objective
→ one Derived Work Item
→ one Work Result
→ one Semantic Unit, zero edges
→ a Writing Style Map with no transform
→ one-paragraph Final Output
```

## Workflow

### 1. Analyze Prompt and External Work

Analyze Prompt is the starting point of the entire workflow. It analyzes and decomposes the user input at the task level, identifies what the Final Output must accomplish, and forms the Task Contract. The Task Contract defines only the task and its inputs; it does not define paragraph or semantic structure. It includes:

- **Problem Specification**: Requested Objectives, Derived Work Items, Structure Readiness Conditions, Task Relations, Requirement Routing, and Deliverables;
- **Context Specification**: distinguishes tasks from reference information and records the role, authority, and objective links of inputs through the Input Inventory.

Build the Derived Work Plan from the Derived Work Items, required results, Task Relations, Structure Readiness Conditions, and Input Inventory, and give it to External Agents / Tools for execution. Before entering Structure, Work Results must pass through the External Work Gate.

The External Work Gate checks Coverage, Requirement Compliance, Traceability, and Sufficiency. Work Results that pass the Gate and Unresolved Work Results produced after the local retry limit is reached are organized together into Raw Material. Unresolved states are preserved but do not stop the workflow. See `references/task-and-work.md` for the detailed definitions, check order, and retry rules.

### 2. Structure

Structure receives Raw Material and the Objectives, Task Relations, requirements, and Deliverable Contracts in the Task Contract that apply to Structure.

First, identify semantic relations and endpoint roles using the open Relation Inventory, and build the Dependency Graph from them:

$$
D=(U,E,\lambda,H),
$$

where $U$ is the set of Semantic Units, $E$ is the set of untyped edge instances, $\lambda$ is the edge-labeling function, and $H$ is the set of hard presentation constraints. When no suitable existing relation can be found, create a local relation that serves only the current Graph; do not automatically write it back to the Relation Inventory.

Then linearize the Dependency Graph to obtain a Linearized Structure Draft composed of basically fluent sentences. Linearization realizes important relations in the Graph through order, adjacency, grouping, and necessary relation statements. The production order of Raw Material must not become the Presentation Order directly.

The Structure Gate checks Graph construction and Linearization separately. Problems with the Graph, units, edges, labels, or hard constraints return to Graph construction; problems with order, grouping, or Relation Realization return to Linearization. See `references/structure.md` for the detailed procedure and checks.

### 3. Writing Style Selection

Writing Style Selection processes the Linearized Structure Draft that has passed the Structure Gate. All text is assigned Default Style. Writing Style Selection uses the Active Transforms Index in `references/writing-style-selection.md` to learn the available transform categories, trigger features, applicable scopes, and file locations, but does not read the bodies of files in `transforms/`. It then determines a Default Language Profile for each Deliverable and builds a Writing Style Map for existing Units or Unit Groups, recording Surface Realization and, when applicable, a local Language Profile override, selected transform, and override scope. Each Semantic Unit uses Default Style or exactly one selected transform; different transforms may be assigned only to disjoint scopes.

A Special Transform may override the writing approach of Default Style. Writing Style Selection only assigns the Special Transform label and its override scope; it does not perform the actual rewrite. The Writing Style Gate checks only the Style Map. On Fail, reselect the Language Profile, Surface Realization, transform, or override scope without returning to Structure. See `references/writing-style-selection.md` for the detailed procedure, Active Transforms Index, and check rules.

### 4. Prose

Prose rewrites the Linearized Structure Draft according to the checked Writing Style Map. Read `references/prose.md` when entering this stage. Only when the Writing Style Map contains a selected transform, read the corresponding file in `transforms/` and execute its rules within the corresponding override scope. Rewrite content not covered by a transform according to the Default Language Profile and corresponding Surface Realization. This stage may adjust wording, syntax, sentence boundaries, and surface formatting, but it must not add claims or change established relations, Presentation Order, hard constraints, inference boundaries, qualifiers, hedges, or technical terminology.

The Prose Gate checks whether Objectives are answered completely, structural relations are realized correctly, information and qualifications are preserved, terminology remains stable, selected transforms are implemented correctly within their respective override scopes, and the text still contains uninformative staging, repeated conclusions, false transitions, or chatbot residue. The actual textual effects of a Transform are checked by the Prose Gate, not the Writing Style Gate. Fail returns only to the Style-Guided Prose Rewrite; Pass produces the Final Output. See `references/prose.md` for the detailed rules.

## Completion Conditions

Deliver the Final Output only when all of the following conditions are satisfied:

- Requested Objectives have been addressed in the corresponding Deliverables;
- hard requirements have been satisfied, and any unsatisfied soft requirements have yielded to factual accuracy, information fidelity, or higher-priority requirements;
- unresolved, conflict, scope, hedges, and attribution in Raw Material have not been eliminated without basis;
- important relations and hard constraints in the Dependency Graph can be recovered from the final text;
- the Writing Style Map has not overridden decisions made by Structure;
- the Prose Rewrite has not introduced new content claims or damaged terminology or qualifications;
- the output form is proportionate to the actual logical complexity and does not add unnecessary sections, lists, tables, or short-sentence fragmentation merely to comply with a template.
