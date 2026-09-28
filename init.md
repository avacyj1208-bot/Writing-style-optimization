# AI Natural-Language Writing Output Style Project — Initial Consolidation

## 0. Document status

This document consolidates the decisions and source-level findings from the preceding discussion so the project can continue in Codex. It is a handoff and analysis record, not a first draft of the eventual `SKILL.md` or plugin specification.

The distinctions below matter:

- Project scope, governance, and the `Structure -> Prose` architecture are agreed constraints.
- Source evaluations, proposed composition, and priorities are the current working assessment.
- Quoted text is retained for source verification. Before adoption, it should be checked against the corresponding upstream file and version.
- No candidate rule listed here becomes normative merely because it appears in this document.

## 1. Problem and project scope

### 1.1 Target problem

The project aims to improve AI **natural-language writing output style**, especially the failure mode seen in advanced models where:

- individual sentences may appear locally correct;
- global information order and claim dependency deteriorate;
- paragraphs become exchangeable or repetitious;
- the response accumulates local explanations, qualifications, and rhetorical moves without maintaining a cumulative argument;
- surface coherence remains, but the text does not make cumulative progress.

The problem is therefore broader than conventional “AI voice” or a blacklist of words such as `moreover`, `crucial`, or `not only X but Y`. The central issue is the combination of **discourse/argument structure failure** and **prose-level AI slop**.

### 1.2 Included

- Natural-language answers, explanations, analysis, advice, discussion, and long-form writing.
- Structure-level control of information order, claim dependency, paragraph function, inference boundaries, and cumulative progression.
- Prose-level control of redundant staging, repeated conclusions, pseudo-emphasis, fake transitions, vague claims, and other recurrent AI rhetorical patterns.
- Optional document-specific transforms when they are explicitly useful, such as compression or controlled language for README files, runbooks, and selected technical/model-architecture documents.
- Evaluation infrastructure needed to test whether a proposed rule actually improves output without losing content or technical precision.

### 1.3 Excluded

- Coding and other non-natural-language generation, including code, configuration, formulas, and structured data as output classes.
- A universal “make everything shorter” system.
- Treating compression as a mandatory top-level writing stage.
- Building a new writing theory from scratch when an existing source can be reviewed, decomposed, modified, or recombined.

The exclusion of coding is deliberate: its generation logic and quality criteria differ from those of natural-language prose.

## 2. Project-governance constraint

The project should primarily **review, decompose, combine, and lightly modify existing solutions**.

The governing rule is:

- ChatGPT/Codex must not inject substantial newly authored normative writing into the Skill or plugin without the user's explicit permission.
- Normative text should preferentially come from existing projects.
- If an existing source does not cover a necessary rule or principle, the user drafts the new substance first.
- ChatGPT/Codex may then review, compress, compare, identify conflicts, and help integrate that user-authored text.
- Without explicit permission, ChatGPT/Codex must not directly add large self-authored passages or independently rewrite the governing Skill documents.

This constraint exists to prevent the project from being contaminated by the same AI writing tendencies it is intended to correct. The present `init.md` is authorized consolidation, not authorization to draft the eventual normative Skill.

## 3. Corrected architecture

### 3.1 Main writing pipeline

`Structure` and `Prose` are sequential stages in the same general writing system:

```text
Natural-language request
        |
        v
Structure
information organization / argument order
        |
        v
Prose
syntax / rhetoric / redundancy
        |
        v
Final output
```

- **Structure** decides what to say and the dependency/order in which to say it.
- **Prose** realizes that structure in readable language and removes AI-specific rhetorical residue without destroying necessary complexity or technical precision.
- Structure planning and prose realization should not be collapsed into one uncontrolled generation process.

### 3.2 Compression is an optional special transform

Compression is not a third peer in the universal pipeline. It is a specialized transform for selected document classes:

```text
                         +-- README compression
Main writing pipeline ---+-- architecture-spec compression
                         +-- long technical note reduction
                         +-- other targeted transforms
```

`SimpleEnglish` / ASD-STE100 belongs here. It should not control ordinary discussion, complex analysis, or long sentences whose subordinate structure carries real logical relations.

### 3.3 Development/evaluation is external infrastructure

Evaluation is necessary, but it is not itself part of the final writing style:

```text
baseline failure
      |
source-rule intervention
      |
same pressure scenario
      |
content / clarity / drift evaluation
      |
retain, revise, or reject
```

## 4. Three evaluation questions

Every candidate project, file, or rule should be evaluated through these three questions:

1. **Structure:** Does it actually control claim dependency, information order, paragraph function, and argument progression, rather than merely instructing the model to “be clear” or “be concise”?
2. **Prose:** Can it remove AI-specific staging, repetition, pseudo-emphasis, false transitions, and similar residue without destroying necessary long sentences, complex clauses, or technical precision?
3. **Special transforms:** Does it contain rules that are useful only for particular document types and therefore must be isolated as an optional Skill/transform rather than allowed to contaminate the main writing style?

## 5. Source-level assessment summary

| Project / file | Structure | Prose | Special transform | Current use |
|---|---:|---:|---:|---|
| `academic-writing-skills/skills/academic-writing-skills/SKILL.md` | Very high | Medium-high | Low | Main Structure source |
| `academic-writing-skills/.../references/lifecycle-and-routing.md` | Very high | Low | Low | Priority extraction |
| `academic-writing-skills/.../references/universal-integrity.md` | Very high | High | Low | Best single source currently identified |
| `blader/humanizer/SKILL.md` | Low-medium | Very high | None | Main Prose / anti-slop source |
| `sciwrite/SKILL.md` | Low | High | Academic-text bias | Select rules; do not adopt wholesale |
| `sciwrite/HOW-TO-USE.md` | Explicitly excludes restructuring | — | — | Defines the project's boundary |
| `simple-output-styles/clarity-flow.md` | Medium | High | None | Extract flow principles only |
| `simple-output-styles/actionable-clarity.md` | Medium | Medium-high | None | Do not use as the base style |
| `simple-output-styles/evals/*` | — | — | — | Reuse evaluation methodology |
| `Superpowers/brainstorming/SKILL.md` | High as process analogy | None | None | Extract phase separation only |
| `Superpowers/writing-skills/SKILL.md` | — | — | — | Skill-development and regression method |
| `SimpleEnglish/skills/simple-english/SKILL.md` | Low | Specialized | Very high | Optional targeted transform |

## 6. Detailed findings

### 6.1 `academic-writing-skills`: primary Structure source

#### Useful files

- `academic-writing-skills/skills/academic-writing-skills/SKILL.md`
- `academic-writing-skills/skills/academic-writing-skills/references/lifecycle-and-routing.md`
- `academic-writing-skills/skills/academic-writing-skills/references/universal-integrity.md`

The two reference files are more important to this project than the repository README and, for the relevant mechanisms, more important than the main `SKILL.md` alone.

#### `SKILL.md`: paragraph/writing contract

The useful mechanism is to define a paragraph contract before drafting:

```text
function -> claim -> evidence -> development -> bridge
```

Quoted source snippet:

> “Draft only after the contract is coherent.”

The mechanism forces the model to know why a paragraph exists, what it claims, what supports it, how far that support permits inference, and why the next paragraph follows. `bridge` is not a transition word; it represents the logical relation from one paragraph to the next.

**Adoption caution:** the academic meaning of `evidence` cannot be copied unchanged into a universal AI-writing system. Ordinary answers may involve explanation, comparison, reasoning, or advice rather than paper-style evidence. Any generalization of that field requires user-authored normative wording or explicit permission.

#### `lifecycle-and-routing.md`: cumulative argument rather than heading list

Quoted source snippets:

> “Build the extended outline as an evidence plan, not a list of headings.”

> “its paragraphs must form one cumulative argument”

The planned-paragraph fields discussed were:

```text
reader function / central claim / authorized evidence / inference boundary / bridge
```

The file identifies three particularly useful structural failure types:

- `duplicated function`: multiple paragraphs perform the same argumentative job using different wording;
- `orphan evidence`: an example, datum, or concept appears without serving a claim;
- `unsupported transition`: a marker such as “therefore” or “more importantly” asserts a relation that the surrounding claims do not support.

These are stronger than a transition-word blacklist because they evaluate actual dependency.

#### `universal-integrity.md`: the strongest single document

This file is currently the closest single source to the project's combined needs because it joins Structure and Prose without confusing their roles.

Quoted source snippets:

> “Read only paragraph-opening sentences in sequence”

> “verify cumulative movement rather than repeated restatement”

The first is a top-down structural check: the paragraph-opening sentences, read alone, should form a coherent outline. The second distinguishes merely coherent text from progressive text. The target failure is often **coherent but non-progressive** writing.

The file's useful three-level flow model is:

```text
Sentence:
old -> new information
subject continuity

Paragraph:
claim -> evidence -> explanation
-> resolution / relation to next paragraph

Section:
topic sentences + closing sentences
-> cumulative movement
```

It also treats transitions as real relations—contrast, cause, condition, extension, qualification, or consequence—rather than decorative connectors. This belongs in Structure, not in a prose-level word detector.

### 6.2 `blader/humanizer`: primary Prose / anti-slop source

#### Useful file and role

- `blader/humanizer/SKILL.md`

Humanizer is a mature empirical catalog of AI-writing patterns and a strong post-draft prose-cleanup source. It is not a logic-restoration system and does not construct a claim-dependency graph. It can improve paragraphs individually while leaving their global order wrong.

Quoted source snippets discussed:

> “Preserve the information, not the shape.”

> “Every sentence you keep must add something the reader did not already have.”

> “Word habits change with every model release. The structural habits above persist.”

The useful shift in v3 is from word blacklists toward structural prose patterns and contextual judgments about strong signals, weak signals, and clusters.

#### Highest-value module: `Staging instead of stating`

The following patterns were judged strong candidates for direct extraction:

- redundant `not X but Y`, when the negative half answers no real misconception and adds no information;
- one-line closers such as “That is the key point” when they merely restate or inflate a completed claim;
- fake aphorisms or pseudo-profound abstractions that should be restored to a specific claim;
- staged run-ups such as “let's explore” or “here's what you need to know” that add no information;
- `arguing with no one`: inventing an objection or naive interpretation that the user never raised.

Other useful candidates discussed include stacked qualifiers, inflated significance, vague association, unsupported `-ing` riders, sales language, borrowed authority without a source, chatbot residue, and heading repetition.

#### Rules that require modification or downgrading

- **Em/en dash:** the useful diagnosis is that a dash can conceal an undefined clause relation; the blanket treatment “no dash” is too mechanical.
- **Rule of three:** retain only as a detector for a content-free third item added for rhythm. A meaningful three-part structure is valid.
- **Passive voice:** a weak signal only; revise when actor omission creates ambiguity or active voice materially clarifies actor/action.
- **AI vocabulary lists:** words such as `crucial`, `robust`, `intricate`, `landscape`, and `pivotal` are often legitimate in mathematical, quantitative, or model-architecture writing. Treat only as cluster evidence, not hard bans.
- **Repeated sentence openings and dash frequency:** warnings, not automatic rewrite triggers.

#### Rules/features currently considered irrelevant to the core goal

- curly-quote detection;
- hyphenated-compound detection by itself;
- other detector-oriented surface features with little relation to logical quality.

#### Overall boundary

Humanizer can remove **prose slop**, but not **argument slop**. It is a strong component/forking source and a poor choice as an unchanged always-on global system.

### 6.3 `SciWrite`: selective Prose reviewer

#### Useful files

- `sciwrite/SKILL.md`
- `sciwrite/HOW-TO-USE.md`

Quoted source boundary:

> “It does not restructure your argument or reorder sections.”

This explicitly removes SciWrite from consideration as the Structure layer. Its usefulness is in selective prose review.

#### Useful ideas

- **Clutter extraction:** catches constructions such as `It is worth noting that`, `It is important to note that`, and `in order to`. Humanizer is the preferred modern source; SciWrite provides cross-checking.
- **Verb vitality / buried action:** transformations such as `conducts an assessment -> assesses` and `performs an analysis -> analyzes` can reveal avoidable nominalization.
- **Sentence architecture:** tests whether long intervening material separates the grammatical subject from the main verb and buries the predicate. This is not a ban on long sentences; a 50-word sentence is acceptable when its dependency structure remains readable.
- **Terminology consistency:** protects technical concepts from synonym drift caused by naive anti-repetition rewriting.

Quoted source snippet:

> “terminological consistency is a virtue, not a defect.”

#### Adoption cautions

- Do not convert `noun + of` into a universal defect; constructions such as `distribution of parameter estimates` can be correct and necessary.
- Do not use SciWrite as a fixed academic template or assume Stanford-derived writing instruction is automatically universal.
- Its strongest contribution is sentence-internal architecture and terminology stability, not argument ordering.

### 6.4 `simple-output-styles`: extract Flow; reuse the evaluation harness

#### Useful files

- `simple-output-styles/clarity-flow.md`
- `simple-output-styles/actionable-clarity.md`
- `simple-output-styles/evals/*`

#### `clarity-flow.md`: valuable reader-information flow

Quoted source snippets:

> “Begin sentences with information the reader already has; end with the news.”

> “Use the end of one sentence to set up the beginning of the next.”

These provide the sentence-level complement to `academic-writing-skills` paragraph bridges:

```text
Clarity Flow: sentence N -> sentence N+1
Academic Writing Skills: paragraph N -> paragraph N+1
```

Maintaining a stable topic chain within a paragraph is also useful: grammatical subjects should not change arbitrarily when the topic has not changed.

**Adoption principle already reached:** take **Flow**, not blanket **Concision**. Subject-length heuristics, bans on long wind-up clauses, and generic instructions that chat answers must be short may overconstrain complex analysis.

#### `actionable-clarity.md`: do not adopt as the base style

Quoted source snippet:

> “Express one idea per sentence.”

Mechanically applying this would split relations that sometimes need to remain in one syntactic unit:

```text
condition + qualification + contrast + consequence
```

The project's normal work often requires complex sentences to make these dependencies explicit. Therefore `actionable-clarity.md` may yield individual candidates, but its whole style conflicts with the project's main use cases.

#### `evals/*`: possibly the repository's most valuable contribution

The evaluation harness measures or discusses:

- fidelity;
- token cost;
- reader value;
- content loss;
- clarity ranking;
- long-session drift;
- loss of facts;
- accidental conversion of hedged claims into certainty.

The prior analysis also recorded that `actionable-clarity` ranked highest under a model judge while human spot-check agreement was only `6/12 = 0.50`, leading to the explicit caveat:

> “actionable-clarity has no independent human validation.”

This weakens any claim that the style itself is proven, but strengthens the case for borrowing the project's evaluation discipline and transparency.

The reported comparison in which a concise style reached an output-token ratio of `0.580` yet ranked last on the clarity judge supports a limited conclusion: **shorter output is not necessarily clearer output**. Compression must not be substituted for the main writing objective.

### 6.5 `Superpowers`: process architecture and Skill-development method

#### Useful files

- `Superpowers/brainstorming/SKILL.md`
- `Superpowers/writing-skills/SKILL.md`

#### `brainstorming/SKILL.md`: phase separation only

The transferable mechanism is process separation:

```text
understand
-> compare approaches
-> freeze design
-> review design
-> implementation
```

Mapped narrowly to this project, the useful insight is:

> Structure planning and prose realization should not occur simultaneously.

The coding/system-design content itself should not enter the natural-language writing system. User-approval gates, codebase exploration, component architecture, and data-flow procedures are not writing-style rules.

#### `writing-skills/SKILL.md`: test from observed failure

Quoted source excerpt as recorded in the discussion:

> “If you didn't watch an agent fail without the skill...”

The useful method is:

```text
baseline without skill
-> record the failure mode
-> add the candidate source rule
-> repeat the same pressure scenario
-> observe whether it improves
-> modify only in response to demonstrated failures
```

This directly supports the governance constraint: do not add rules because they sound useful. First observe how a target model fails; then test whether an existing rule from Humanizer, Academic Writing Skills, Clarity Flow, or SciWrite repairs it. If existing sources fail, the user drafts any new normative rule.

### 6.6 `SimpleEnglish`: Special-only targeted transform

#### Useful file

- `SimpleEnglish/skills/simple-english/SKILL.md`

The discussed documentation constraints include:

- procedures: approximately 20 words per sentence;
- descriptions: approximately 25 words per sentence;
- active voice;
- simple tenses;
- one term per concept;
- condition before command;
- no semicolon or em dash.

These are suitable candidates for README files, runbooks, API guides, error messages, release notes, and selected technical-document compression.

#### Adoption cautions

- Do not install SimpleEnglish as the global writing style.
- Its discussed chat constraint—at most five sentences and no header/list/table—conflicts directly with the main project.
- Do not use it for philosophy, complex economics, research reasoning, or other prose whose precision depends on complex syntactic relations.
- For long model-architecture documents, strict STE is not automatically appropriate. A hard 25-word limit can split explicit dependencies such as conditions, exceptions, and override relations.

The previously proposed sequence for this special case remains experimental:

```text
original architecture
-> redundancy removal / terminology normalization
-> targeted SimpleEnglish compression
-> Structure recheck
```

SimpleEnglish must not determine the architecture itself. Whether and how it should be used on long architecture documents remains **Needs-test**, not a settled rule.

## 7. Current proposed composition

The current source composition is:

```text
MAIN WRITING PIPELINE

STRUCTURE
  academic-writing-skills
  - writing contract
  - paragraph function
  - bridge
  - cumulative argument
  - top-down / bottom-up checks
  - sentence / paragraph / section flow

  Superpowers (process mechanism only)
  - separate and freeze Structure before prose realization

        |
        v

PROSE
  blader/humanizer
  - staging and fake contrast
  - redundant closers and fake aphorisms
  - arguing with no one
  - inflated or unsupported rhetorical material

  simple-output-styles/clarity-flow.md
  - old -> new
  - sentence ending -> next sentence beginning
  - topic continuity

  SciWrite
  - buried predicates / actions
  - sentence-internal architecture
  - terminology consistency

        |
        v

FINAL PROSE
```

Independent optional path:

```text
SPECIAL TRANSFORM
  SimpleEnglish / STE
  - README
  - runbook
  - selected technical documentation
  - selected architecture compression, subject to testing
```

External development infrastructure:

```text
EVALUATION
  Superpowers/writing-skills
  - baseline -> intervention -> same-scenario regression

  simple-output-styles/evals
  - fidelity
  - information loss
  - hedge preservation
  - reader comprehension
  - verbosity
  - long-context drift
```

This is an analysis map, not authorized normative Skill text.

## 8. Current priorities

Priorities are based on likely contribution to the final project, not on which repository should be installed wholesale.

| Priority | Source | Focus |
|---|---|---|
| **A+** | `academic-writing-skills/.../references/universal-integrity.md` | Structure plus multi-level flow |
| **A+** | `academic-writing-skills/.../references/lifecycle-and-routing.md` | Paragraph contract and cumulative argument |
| **A+** | `blader/humanizer/SKILL.md` | Prose anti-slop rules |
| **A** | `simple-output-styles/evals/` | Regression and evaluation methodology |
| **A** | `Superpowers/writing-skills/SKILL.md` | Skill-development method |
| **B+** | `simple-output-styles/clarity-flow.md` | Old-to-new flow and topic chain |
| **B** | `SciWrite/SKILL.md` | Buried predicate and terminology consistency |
| **Special** | `SimpleEnglish/skills/simple-english/SKILL.md` | Targeted compression only |
| **C / partial extraction only** | `simple-output-styles/actionable-clarity.md` | Too dependent on short/simple-sentence assumptions |
| **Reject as global style** | `SimpleEnglish` | Conflicts with the main writing objective |

The present assessment is that additional broad searches for Humanizer-style projects have low marginal value. The next useful work is source-rule extraction, comparison, conflict analysis, and controlled testing.

## 9. Known conflicts and adoption cautions

These conflicts must be exposed before any Skill document is edited:

1. **Dash handling:** SciWrite may allow a dash to express a strong relation; Humanizer tends toward restricting it. Presence cannot be treated as a defect. The relevant question is whether punctuation hides or clarifies the clause relation.
2. **Sentence complexity:** Clarity Flow can support complex information progression; Actionable Clarity's “one idea per sentence” can fracture it. Long sentences are not themselves failures.
3. **Nominalization:** SciWrite's buried-action diagnostic can help, but `noun + of` is not a valid universal rewrite trigger.
4. **Terminology versus variation:** technical repetition may preserve meaning. Humanization must not produce synonym drift.
5. **Triads and parallelism:** rhythmic filler should be detected, but meaningful three-part structures, simultaneity, and rankings must be preserved.
6. **Active versus passive voice:** passive voice is legitimate when it controls information focus; only ambiguity or obscured agency justifies automatic concern.
7. **Compression versus clarity:** fewer tokens do not prove better structure or readability.
8. **Academic evidence model:** the academic paragraph contract is promising, but its `evidence` field cannot be generalized to every answer without deliberate user-controlled adaptation.
9. **Post-processing versus planning:** Humanizer and SciWrite cannot repair every global dependency error after drafting. Structure must be addressed before prose cleanup.
10. **Evaluation validity:** model-judge rankings require human checks; the current evidence does not independently validate Actionable Clarity.

## 10. Next step: rule-level extraction matrix

The next phase is to extract individual rules from these files:

- `academic-writing-skills/.../references/universal-integrity.md`
- `academic-writing-skills/.../references/lifecycle-and-routing.md`
- `academic-writing-skills/skills/academic-writing-skills/SKILL.md`
- `blader/humanizer/SKILL.md`
- `simple-output-styles/clarity-flow.md`
- `simple-output-styles/actionable-clarity.md`
- `sciwrite/SKILL.md`
- `Superpowers/writing-skills/SKILL.md`
- `SimpleEnglish/skills/simple-english/SKILL.md`

Each extracted rule should retain its source path and original wording, then receive exactly one working label:

| Label | Meaning in the next review |
|---|---|
| **Keep** | Candidate can be retained substantially as written, subject to source/version verification and final approval. |
| **Modify** | The underlying diagnosis or mechanism is useful, but its scope, trigger, or treatment is too narrow, broad, academic-specific, or mechanical. The user controls any new normative wording. |
| **Reject** | Rule conflicts with the project's scope or would predictably damage logic, technical meaning, or ordinary complex prose. |
| **Special-only** | Useful only for a named document class or optional transform; must not enter the universal pipeline. |
| **Needs-test** | Plausible but not adopted until baseline/intervention testing demonstrates benefit without unacceptable content or precision loss. |

For each row, the matrix should record:

```text
source project
source file and section
verbatim rule or short source excerpt
target layer: Structure / Prose / Special transform / Evaluation
label
reason for label
known conflict or semantic risk
test case or evidence status
```

The extraction workflow already agreed in principle is:

1. Observe and record a concrete baseline failure from the target model.
2. Extract a relevant existing rule without silently rewriting it.
3. Classify the rule by layer and assign a working label.
4. Expose conflicts with rules from other sources.
5. Re-run the same pressure scenario with the intervention.
6. Check structure, fidelity, information loss, hedge preservation, technical terminology, reader comprehension, verbosity, and long-context drift as applicable.
7. Retain, modify, reject, or isolate the rule based on evidence.
8. If no existing rule solves the failure, stop before adding normative prose; the user drafts the substantive addition first unless explicit permission is given otherwise.

No consolidated `SKILL.md` or plugin text should be drafted until this rule-level matrix has exposed the source conflicts and the user has approved the resulting selections or supplied the necessary new wording.
