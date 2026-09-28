# Architecture

## 1. Purpose and Scope

Prose applies this transform within the `architecture` override scope assigned by the Writing Style Map. It expresses established system or model content so readers can understand its components, interactions, operating constraints, and design rationale. The Prose Rewrite Constraints and the Map's directives continue to apply.

Work within the existing grouping, Presentation Order, and assigned Surface Realization. Use the exposition locations and reference relationships established in the Draft; selecting or relocating them belongs to Structure.

## 2. Writing Rules

### 2.1 Mechanism Descriptions

**Components and interactions.** Describe what the relevant component does, what it acts on or produces, and how it interacts with other components, using the content supplied in the Draft. Use identifiable subjects and concrete relationships rather than announcing that a mechanism exists or is important. Preserve connected clauses where they make a dependency intelligible; do not force every description into a fixed set of fields or short sentences.

**Design status.** Keep current behavior, intended design, and provisional choices distinguishable through their wording and stated scope. Describe each mechanism directly rather than narrating the writer's successive corrections. Where the Draft includes substantive history, preserve the historical facts and their relevance without adding commentary about the editing process.

**Before:**

> The ventilation system has a controller, and the controller has an important role in deciding how the fan should operate. The room sensor provides a carbon-dioxide reading every minute, and this reading is what the controller uses. When the reading exceeds the configured threshold, the controller increases the fan speed. The part concerning the window needs to be made clear: if the window is open, the controller holds the fan at its minimum speed instead. Occupancy sensing is planned for a later version, but it does not currently influence the fan speed.

**After:**

> The room sensor sends a carbon-dioxide reading to the ventilation controller every minute. When the window is open, the controller holds the fan at its minimum speed; otherwise, it increases the speed when the reading exceeds the configured threshold. Occupancy sensing is planned for a later version and does not currently affect fan speed.

### 2.2 Constraints and Rationale

**Conditions and boundaries.** Attach each condition, exception, and restriction to the component, behavior, or relationship it governs. State the actual boundary instead of introducing it with a general warning. Retain negative distinctions when they determine meaning, such as an absent measurement versus a measured zero.

**Reasons and consequences.** Express an established rationale through the dependency, consequence, or trade-off that supports the design. When the Draft discusses an alternative, retain the relevant difference and the reason for the choice. Remove evaluations of the writer's correctness or the design's importance when they add no technical content. A mechanism does not need an additional rationale unless the Draft supplies one.

**Before:**

> The review arrangement is an important safeguard, and it is essential to understand exactly what it means. Each application is assessed independently by two reviewers. Their scores are averaged to determine its rank. There is one exception that must not be overlooked: when the scores differ by more than two points, a third reviewer assesses the application, and the median of all three scores determines its rank instead. This exception exists because averaging two sharply different assessments can conceal disagreement. Simply applying the average in every case would miss the point of the design.

**After:**

> Two reviewers independently assess each application, and the mean of their scores determines its rank. If the scores differ by more than two points, a third reviewer assesses the application, and the median of the three scores determines its rank instead. The third assessment addresses disagreement that averaging the first two scores could conceal.

### 2.3 Single-Point Exposition and Explicit References

**Complete exposition.** Explain each concept once, at its established exposition point. Express the supplied meaning, scope, conditions, and exceptions precisely enough to support the document's use of that concept. Completeness is relative to the document's purpose, not an invitation to add background or infer missing details.

**Subsequent references.** Refer to an explained concept by its stable name, identifier, or explicit link. Do not repeat its definition, paraphrase its explanation, or add a brief recall at later uses. Use an identifiable reference rather than a position-dependent phrase such as *the mechanism above*. Describing a new relationship involving the concept is not a second exposition of the concept itself.

**Integrated explanation.** Realize the explanation at its established location as a coherent account, not as a base definition followed by corrective asides or successive patches. This rule does not authorize moving content between groups or resolving conflicting definitions during Prose.

**Complementary representations.** Within the assigned Surface Realization, use prose to express the meaning, conditions, or consequences not already conveyed by a diagram, table, or formula. Do not mechanically restate every visible element. Retain distinct views when they express different relationships, even if they involve the same components. Reduce redundant expression, not necessary detail; no word limit or compression ratio determines completeness.

**Before:**

> **Reference period**
>
> The reference period is the twelve complete calendar months preceding the forecast date. Months with incomplete observations are excluded, and excluded months are not replaced by earlier months. A forecast is withheld if fewer than nine months remain.
>
> **Seasonal adjustment**
>
> The seasonal adjustment component estimates its baseline from the reference period. To recall what that means, this is the twelve complete calendar months before the forecast date, excluding months with incomplete observations without replacing them with earlier months. At least nine months must remain for a forecast to be issued. The component compares the current observation with the baseline to produce a seasonally adjusted value, which is passed to the forecasting component.

**After:**

> **Reference period**
>
> The reference period is the twelve complete calendar months preceding the forecast date. Months with incomplete observations are excluded and are not replaced by earlier months. A forecast is withheld if fewer than nine months remain.
>
> **Seasonal adjustment**
>
> The seasonal adjustment component estimates its baseline from the reference period (see **Reference period**). It compares the current observation with this baseline and passes the resulting seasonally adjusted value to the forecasting component.

---

**Temporary references for draft review**

- **Section 1:** `writing-style-optimization/references/prose.md`, Inputs and Style Application and Rewrite Constraints; `writing-style-optimization/references/writing-style-selection.md`, Active Transforms Index and Special Transform. These establish the application scope and inherited constraints.
- **Section 2.1:** [Diataxis — Explanation](https://diataxis.fr/explanation/), Make connections, Provide context, and Keep explanation closely bounded. Adapted to the expression of established mechanisms; no new analysis or document template is introduced.
- **Section 2.2:** [arc42 — Architecture Decisions](https://docs.arc42.org/section-9/), Content and Motivation. Adapted to the expression of supplied decisions, reasons, and trade-offs, without adopting an ADR workflow.
- **Section 2.3:** [arc42 — Refer to concepts, views or code](https://docs.arc42.org/tips/4-4/). The stricter single-point exposition and no-recall requirements follow the approved project discussion; arc42 supports the use of references to avoid redundant explanations. Exposition placement remains a Structure responsibility.
- **Examples:** The three Before/After pairs are illustrative, not source quotations. They retain the supplied mechanisms, conditions, rationale, and grouping while demonstrating the approved writing rules.
