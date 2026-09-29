# Writing Style Selection

## 1. Role and Boundary

This file is loaded by `SKILL.md` after the Structure Gate passes. Writing Style Selection receives the Linearized Structure Draft and the projection of the Task Contract for this stage, and builds the Writing Style Map through the Default Style Selection Policy and the Active Transforms Index.

Writing Style Selection uses Model II. After determining the Default Language Profile, it uses Model I to read the Active Transforms Index, identify genre and function features, and build the Writing Style Map. After the Writing Style Map is formed, the Writing Style Gate checks it. Pass proceeds to Prose; Fail returns to the corresponding step within Writing Style Selection.

This stage only assigns realization to Semantic Units or Semantic Groups that already exist in the Linearized Structure Draft and, when applicable, records a selected transform and override scope. The Linearized Structure Draft remains unchanged during this stage. Writing Style Selection does not modify its text; change Semantic Units, Semantic Grouping, relations, Presentation Order, or hard presentation constraints; or read or execute the bodies of files in `transforms/`.

## 2. Input Contract

Writing Style Selection uses three types of input: the Linearized Structure Draft is the content input, the stage projection of the Task Contract is the control input, and the Active Transforms Index is the index used to select a Special Transform.

### 2.1 Linearized Structure Draft

The Linearized Structure Draft is linear text that has passed the Structure Gate. It already has a Presentation Order, Semantic Grouping, and Relation Realization, and consists of basically fluent, readable sentences.

Writing Style Selection refers directly to the Semantic Units and Semantic Groups already established in it. It does not split, merge, or establish units or groups again, or modify the text within the Draft.

### 2.2 Task Contract Projection

The projection of the Task Contract for Writing Style Selection is denoted by:

$$
T_W=\pi_{\mathrm{WritingStyleSelection}}(\text{Task Contract}).
$$

$T_W$ includes:

- requirements whose `target stages` include Writing Style Selection;
- Deliverable Contracts.

Writing Style Selection reads this content from the Task Contract, which remains continuously indexable throughout the workflow. Objective Links in requirements and Deliverable Contracts continue to identify attribution, but this stage does not reread or plan the content of Requested Objectives. The projection is not written into the Linearized Structure Draft or rewritten by Writing Style Selection.

### 2.3 Active Transforms Index

The Active Transforms Index is located in this file and records the categories, trigger features, applicable scopes, and file locations needed for transform selection. Writing Style Selection selects a transform from this index information but does not read the body of the corresponding file.

Each entry in the Index corresponds to an independent transform file. When no transform applies, the relevant content continues to use Default Style.

## 3. Output Contract

This stage forms one new artifact:

- **Writing Style Map**: records a Default Language Profile for each Deliverable and records Surface Realization for Semantic Units or Semantic Groups that already exist in the Linearized Structure Draft, together with a local Language Profile override, detected genre / function, selected transform, and override scope when applicable.

The Writing Style Map is a control artifact attached outside the Linearized Structure Draft and does not contain a rewritten Draft. After the Writing Style Gate passes, the unchanged Linearized Structure Draft and the checked Writing Style Map are passed together to Prose.

## 4. Artifact Definitions

### 4.1 Writing Style Selection Projection

The elements of the Task Contract projection serve the following functions within Writing Style Selection:

| Element | Content read by this stage |
|---|---|
| Writing Style Selection requirements | Requirements whose `target stages` include Writing Style Selection and that must be implemented and checked by this stage. |
| Deliverable Contracts | The deliverable boundaries and presentation conditions that the Writing Style Map must serve. |

The fields of a Deliverable Contract serve the following functions in selection:

| Field | Content read by this stage |
|---|---|
| `deliverable id` | The Deliverable served by the current style selection. |
| `objective links` | The attribution identifiers for the content carried by the Deliverable. |
| `artifact` | The functional category of the deliverable. |
| `audience` | The readers or users to whom the language profile, level of explanation, and terminology presentation must be adapted. |
| `carrier` | The surface presentation forms supported by the current delivery medium. |
| `granularity` | The scope, level of detail, and information density required of the content. |
| `explicit deliverable slots` | Visible components that the user explicitly requires in the Deliverable. |

These objects remain part of the Task Contract; Writing Style Selection reads only their projection.

### 4.2 Default Style Selection Policy

Default Style is the baseline writing approach for all text. Each Deliverable determines its Default Language Profile from applicable requirements and the Deliverable Contract. In the absence of special requirements, an ordinary response in the current conversation uses a direct, natural mode of communication suited to the current task and audience.

Default Style does not use a fixed template. Continuous argument can use paragraphs; existing genuine hierarchy, coordination, steps, or comparisons can be presented through sections, lists, tables, diagrams, or other forms supported by the carrier. Select the corresponding Surface Realization only when the structure already established in the Linearized Structure Draft requires that form. Necessary complex sentences may be retained.

This stage forms only realization directives. Prose implements the actual wording, syntax, sentence boundaries, and surface formatting.

### 4.3 Language Profile and Surface Realization

A Language Profile is an open description of how a Deliverable uses language for its audience, including the communicative relationship, formal conventions, domain conventions, terminology strategy, and assumptions about reader knowledge. It does not use fixed levels, closed categories, or numerical scales.

The Default Language Profile applies to one Deliverable. Only when a requirement explicitly requires local content to use a different language approach does the Writing Style Map record a local Language Profile override for the corresponding Semantic Unit or Semantic Group.

Surface Realization applies to an established Semantic Unit or Semantic Group. Based on the existing Semantic Grouping, relations, and carrier, it selects continuous text, paragraphs, sections, bullet lists, ordered lists, tables, diagrams, or another available form.

An explicit hard requirement directly constrains Language Profile and Surface Realization. Structure implements requirements involving grouping or Presentation Order in the Linearized Structure Draft; Writing Style Selection only selects the corresponding surface presentation for units or groups that have already been formed.

### 4.4 Active Transforms Index

Each entry in the Active Transforms Index records:

| Field | Content |
|---|---|
| `transform category` | The unique category identifier recorded by the Writing Style Map in `selected transform`. |
| `trigger features` | Applicable conditions observable from genre / function, requirements, or the Deliverable Contract. |
| `applicable scope` | Whether the Transform can cover an entire Deliverable or only specific types of Semantic Units or Groups. |
| `file path` | The body file read by Prose after the transform is selected. |

The current Index entries are:

| Transform category | Trigger features | Applicable scope | File path |
|---|---|---|---|
| `academic` | The reader needs to evaluate research claims, evidence, scope of inference, or scholarly attribution. | An entire academic Deliverable, or Semantic Groups whose primary reader function is evaluating research claims. | `transforms/academic.md` |
| `procedural` | The reader needs to perform actions under explicit conditions and recognize their results. | Semantic Groups whose primary reader function is recognizing conditions, actions, and results. | `transforms/procedural.md` |
| `lookup-reference` | The reader needs to retrieve locally interpretable content selectively and nonlinearly through a stable lookup key. | Semantic Groups that combine nonlinear access, a stable lookup key, and independently interpretable entries. | `transforms/lookup-reference.md` |
| `architecture` | The reader needs to understand components, dependencies, workflows, constraints, or design rationale. | Semantic Groups whose primary reader function is understanding system composition and its relations. | `transforms/architecture.md` |

Bibliographic references, citation lists, and literature reviews serve scholarly attribution or argument and do not belong to `lookup-reference`; when they satisfy the `academic` trigger, apply `academic`.

The Index serves only the indexing function needed for selection and contains no concrete writing rules from the transforms. To add a transform, first complete its Markdown file and then add a row to the Index. When a transform's trigger features, applicable scope, or file path changes, update the corresponding entry at the same time.

### 4.5 Genre and Function Features

Writing Style Selection identifies genre and function features of existing Semantic Units or Semantic Groups from the Linearized Structure Draft, applicable requirements, and the Deliverable Contract. These features support Surface Realization selection and are matched against the `trigger features` and `applicable scope` of the Active Transforms Index. The same unit or group may have multiple detected features; these features provide only candidate grounds for transform selection and do not constitute multiple transform assignments.

Genre and function features are identified from the current content. They do not change Semantic Units or Semantic Groups or constitute new content or structure.

### 4.6 Writing Style Map

The Writing Style Map first records Deliverable-level selection:

| Field | Content |
|---|---|
| `deliverable id` | The Deliverable served by the current Writing Style Map. |
| `default language profile` | The Language Profile used by the Deliverable by default. |

It then records entries using existing Semantic Units or Semantic Groups as the mapping unit:

| Field | Content |
|---|---|
| `semantic unit or semantic group` | A unit or group already established in the Linearized Structure Draft to which this entry assigns Surface Realization. |
| `surface realization` | The surface presentation used by the unit or group under Default Style. |
| `local language profile override, if any` | The local override used when a requirement explicitly requires the unit or group to depart from the Default Language Profile. |
| `detected genre / function` | The genre and function features that support the current Surface Realization or transform selection. |
| `selected transform, if any` | A Special Transform selected from the Active Transforms Index; omit when no transform applies. |
| `override scope` | The existing unit or group covered by the selected transform; omit when no transform is selected. |

Map entries may use a single unit or a semantic group already established by Structure, but do not establish new unit divisions or grouping. Each entry records at most one selected transform. A group-level entry means that every unit in the group receives the same transform, and no nested transform may be assigned to a unit within it. An entry with no local override inherits the Default Language Profile of its Deliverable. The Writing Style Map records only selection results and contains no actually rewritten text.

### 4.7 Special Transform

A Special Transform is a set of writing rules that overrides Default Style within a specific genre or functional scope. Writing Style Selection only selects a transform from the Active Transforms Index and records its label and override scope in the Writing Style Map.

The override scope of a selected transform must refer to units or groups that already exist in the Linearized Structure Draft. Within that scope, a Transform may override applicable rules of Default Style, but it must not require modification of Semantic Units, Semantic Grouping, relations, Presentation Order, hard presentation constraints, inference boundary, qualifications, hedges, or technical terms.

The effective style source for each Semantic Unit must be either Default Style or one selected transform. Different transforms may appear in the same Deliverable, but their override scopes must be disjoint. A selected transform becomes the effective style source within its scope; the other directives in the Writing Style Map and the general Prose constraints continue to apply.

Prose reads and executes the transform body from its `file path` after the Writing Style Gate passes. Content not covered by a selected transform continues to use Default Style, the Default Language Profile, and the corresponding Surface Realization.

## 5. Procedure

### 5.1 Determine the Default Language Profile

1. Read the applicable requirements and Deliverable Contracts in $T_W$.
2. Determine the Default Language Profile for each Deliverable from its artifact, audience, carrier, and granularity.
3. In the absence of special requirements, an ordinary response in the current conversation uses a direct, natural mode of communication suited to the current task and audience.
4. This step establishes only Deliverable-level selection and does not modify the Draft text.

### 5.2 Read the Active Transforms Index

Read the `transform category / trigger features / applicable scope / file path` from the Active Transforms Index. This step does not read the bodies of files in `transforms/`.

### 5.3 Detect Genre and Function Features

Using Semantic Units or Semantic Groups that already exist in the Linearized Structure Draft as the unit of analysis, identify genre and function features relevant to realization and transform selection from the applicable requirements and Deliverable Contract.

### 5.4 Select Surface Realization

1. Let each Semantic Unit or Semantic Group inherit the Default Language Profile of its Deliverable.
2. When a requirement explicitly requires local content to use a different language approach, record a local Language Profile override.
3. Select Surface Realization from the existing Semantic Grouping, relations, Presentation Order, and carrier.
4. Use the lowest-cost form that preserves the established structure and satisfies delivery conditions; do not establish new grouping or Presentation Order for the content.
5. When an explicit hard requirement specifies a presentation form, complete the selection within that constraint.

### 5.5 Select Special Transforms

1. Match detected genre / function against the trigger features and applicable scope in the Active Transforms Index.
2. Assign each Semantic Unit either Default Style or one selected transform.
3. When multiple transforms match the same range, first determine whether their reader functions belong to different existing units or groups. If so, create disjoint scopes along the existing boundaries and assign a transform to each.
4. When multiple transforms still match the same unit or group, select the one that best serves the primary reader function of that range. When no reader function dominates, retain Default Style.
5. When a semantic group is used as the scope, every unit within that group uses the same transform; do not assign a different transform to a unit within it.
6. The Writing Style Map does not record transform candidate sets or establish overlapping transform scopes.

### 5.6 Build the Writing Style Map

Record a Default Language Profile for each Deliverable and create complete, non-conflicting map entries for all content. Record the semantic unit or semantic group, Surface Realization, and detected genre / function, together with a local Language Profile override, selected transform, and override scope when applicable. Every unit that must appear in the output ultimately resolves to Default Style or one selected transform; the scopes of different transforms are disjoint. The Linearized Structure Draft remains unchanged throughout this process.

## 6. Writing Style Gate

The Writing Style Gate checks only whether Prose can execute the Writing Style Map directly, whether the Map is compatible with input constraints, and whether non-default selections are necessary. The Prose Gate checks the actually rewritten text and the realized effects of transforms.

### 6.1 Resolvability

- Each Deliverable has an explicit Default Language Profile;
- each unit that must appear in the output receives an explicit Surface Realization through an existing unit or group;
- map targets, local overrides, selected transforms, and override scopes all resolve to existing objects;
- no conflict among group-level selection, local overrides, and transform overlays leaves the result indeterminate;
- Prose can execute the Map without repeating Writing Style Selection.

### 6.2 Compatibility

- The Map satisfies hard requirements whose `target stages` include Writing Style Selection and implements soft requirements when they do not conflict with higher-priority constraints;
- the Default Language Profile, local overrides, and Surface Realization conform to the Deliverable Contract and the capabilities of the carrier;
- the Map does not require modification of the Linearized Structure Draft's text, Semantic Units, Semantic Grouping, relations, Presentation Order, or hard presentation constraints;
- Map directives do not require Prose to lose inference boundary, qualifications, hedges, attribution, or technical terms.

### 6.3 Selection Necessity

- Each local Language Profile override, non-default Surface Realization, and selected transform is supported by an applicable requirement, Deliverable condition, or detected genre / function;
- if removing a non-default selection would not affect requirement compliance, structural readability, or the efficiency of information presentation, remove that selection;
- do not add a transform or additional formatting when Default Style can complete the content.

## 7. Failure Routing

When the Map cannot form clear, valid directives, return to Writing Style Map construction. When a selection is incompatible with applicable requirements, the Deliverable Contract, or Structure, return to the corresponding Language Profile, Surface Realization, or transform selection. Remove or simplify an unnecessary non-default selection.

A Writing Style Gate Fail returns only to the corresponding step within Writing Style Selection. It does not return to Structure or enter Prose to modify content. All corrections modify only the Writing Style Map; the Linearized Structure Draft remains unchanged. This stage does not set an additional retry limit.

## 8. Handoff

After the Writing Style Gate passes, the unchanged Linearized Structure Draft and the checked Writing Style Map are passed together to Prose. The Writing Style Map is the control artifact for the Style-Guided Rewrite performed by Prose; the Linearized Structure Draft is its content artifact.

The Task Contract remains a continuously indexable, read-only control artifact throughout the workflow. The Task Contract projection used by Writing Style Selection serves only this stage and is not passed to the next stage as part of the Handoff; Prose reads its own projection from the Task Contract.

Prose first reads `references/prose.md`. When the Writing Style Map contains a selected transform, Prose then reads the corresponding transform body from the `file path` recorded in the Active Transforms Index and executes its rules only within the override scope. All other content is realized according to the Default Language Profile and the corresponding Surface Realization.
