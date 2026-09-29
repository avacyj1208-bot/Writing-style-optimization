# Architecture

## 1. Purpose and Scope

Prose applies this transform within the `architecture` override scope assigned by the Writing Style Map. It expresses established system or model content so readers can understand its components, interactions, operating constraints, and design rationale. The Prose Rewrite Constraints and the Map's directives continue to apply.

Work within the existing grouping, Presentation Order, and assigned Surface Realization. Use the exposition locations and reference relationships established in the Draft; selecting or relocating them belongs to Structure.

## 2. Writing Rules

### 2.1 Mechanism Descriptions

**Components and interactions.** Describe what the relevant component does, what it acts on or produces, and how it interacts with other components, using the content supplied in the Draft. Use identifiable subjects and concrete relationships rather than announcing that a mechanism exists or is important. Preserve connected clauses where they make a dependency intelligible; do not force every description into a fixed set of fields or short sentences.

**Design status.** Keep current behavior, intended design, and provisional choices distinguishable through their wording and stated scope. Describe each mechanism directly rather than narrating the writer's successive corrections. Where the Draft includes substantive history, preserve the historical facts and their relevance without adding commentary about the editing process.

**Before:**

> **Lifecycle.** Candidacy is now two clocks, not one age gate. A paper competes for admission for 72 hours from announcement; once admitted it has five days **from pool entry** to win a slot. `papers.status` materializes the resulting state machine (`scoring → rejected | active → expired | shown`), recomputed rather than advanced so that widening a window returns a lapsed paper to the queue instead of stranding it, and `db.advance_lifecycle` is the single definition both stages read.
>
> A placed paper is written `status = 'shown'` with its date, bucket, and $p_k$, **plus one `assignment_audit` row recording why it was placed** (which policy ordered the bucket, the `V` in force and its snapshot, the parameter fingerprint, `fallback_reason` when V was not used, and which pass took it). `assign --force` overwrites the current day's audit rows and clears any that the recomputed cohort no longer contains — the same decision, revised — and may never touch an earlier date, because no stage can reconstruct one. The placed paper is excluded from every later candidate set — this is what makes the feed turn over, since a 3-day half-life would otherwise re-elect the same papers for a week. Assignment is idempotent **per day**, not per row: re-running it no-ops rather than placing a second cohort, because the candidate filter is "never assigned".

**After:**

> **Lifecycle.** Admission eligibility lasts 72 hours from announcement. Admission starts a separate five-day pool-residency window in which the paper can win a display slot. Both stages use `db.advance_lifecycle` to recompute `papers.status` (`scoring → rejected | active → expired | shown`), rather than advance it irreversibly; widening a window can therefore return a lapsed paper to the queue.
>
> Placement sets `status = 'shown'` and records the date, bucket, and $p_k$. It also writes one `assignment_audit` row containing the bucket-ordering policy, the `V` value and snapshot, the parameter fingerprint, `fallback_reason` when V was not used, and the placement pass. `assign --force` revises only the current day's assignment: it overwrites that day's audit rows and removes rows for papers absent from the recomputed cohort. Earlier dates remain unchanged because no stage can reconstruct them. Placed papers are excluded from all later candidate sets, ensuring feed turnover instead of allowing the 3-day half-life to reselect the same papers for a week. Normal reruns are no-ops for the day: applying the "never assigned" filter again to place a second cohort would violate daily idempotence.

### 2.2 Constraints and Rationale

**Conditions and boundaries.** Attach each condition, exception, and restriction to the component, behavior, or relationship it governs. State the actual boundary instead of introducing it with a general warning. Retain negative distinctions when they determine meaning, such as an absent measurement versus a measured zero.

**Reasons and consequences.** Express an established rationale through the dependency, consequence, or trade-off that supports the design. When the Draft discusses an alternative, retain the relevant difference and the reason for the choice. Remove evaluations of the writer's correctness or the design's importance when they add no technical content. A mechanism does not need an additional rationale unless the Draft supplies one.

**Before:**

> **Seeds are deliberately absent from `papers`.**
>
> Keeping them out is structural rather than incidental: the assignment candidate filter is `assigned_date IS NULL`, so a seed inserted into `papers` would compete for a display slot, draw a paid summary, and be rendered. A seed is a direction anchor, not a candidate. The consequence is that seed vectors need their own embedding pass, which runs as a second phase of the `embed` stage **on the same encoder instance** — `SFF_revised.md` §2.1 fixes $E$ corpus-wide, and since `V` compares a candidate against a seed centroid, vectors from two encoder versions would be silently incomparable rather than merely imprecise.

**After:**

> **Seeds are deliberately absent from `papers`.**
>
> Seeds are direction anchors, not display candidates. Inserting them into `papers` would make them eligible under `assigned_date IS NULL`: they would compete for display slots, incur paid summaries, and be rendered. Their separate storage therefore requires a separate embedding pass, implemented as the second phase of `embed`. Both phases use the same encoder instance, preserving the corpus-wide $E$ required by `SFF_revised.md` §2.1. Since `V` compares candidate vectors with seed centroids, different encoder versions would make those vectors incomparable, not merely less precise.

### 2.3 Single-Point Exposition and Explicit References

**Complete exposition.** Explain each concept once, at its established exposition point. Express the supplied meaning, scope, conditions, and exceptions precisely enough to support the document's use of that concept. Completeness is relative to the document's purpose, not an invitation to add background or infer missing details.

**Subsequent references.** Refer to an explained concept by its stable name, identifier, or explicit link. Do not repeat its definition, paraphrase its explanation, or add a brief recall at later uses. Use an identifiable reference rather than a position-dependent phrase such as *the mechanism above*. Describing a new relationship involving the concept is not a second exposition of the concept itself.

**Integrated explanation.** Realize the explanation at its established location as a coherent account, not as a base definition followed by corrective asides or successive patches. This rule does not authorize moving content between groups or resolving conflicting definitions during Prose.

**Complementary representations.** Within the assigned Surface Realization, use prose to express the meaning, conditions, or consequences not already conveyed by a diagram, table, or formula. Do not mechanically restate every visible element. Retain distinct views when they express different relationships, even if they involve the same components. Reduce redundant expression, not necessary detail; no word limit or compression ratio determines completeness.

**Before:**

> **Opening terminology**
>
> **Terminology confirmed by the user (2026-09-10): field and arXiv category are synonyms.** `Λ` is the selected arXiv category set, `λ` is one such category, and `Λ(i)` contains the paper's arXiv-assigned categories within that set. There is no additional category-to-field taxonomy or rollup. Candidate category membership and editorial seed-library membership are different relations over the same category identifiers (§2.10.2). Preserve existing `math.NA` / `cs.NA` alias handling; it does not create a second taxonomy.
>
> **§2.7 — Assignment**
>
> Assignment was implementable before the rest of V2 because it needs neither Rating nor T3. Category keys already represent `λ`; they are not a substitute for a missing taxonomy.
>
> **§5 — Migration path and sequence**
>
> **Task 2 then took step 8's feedback half** (§2.10), leaving T4 rationale generation for later. It had to precede step 3 rather than follow it, because step 3's `V` upgrade consumes reference sets that did not exist: building `V` first would have meant building it against empty libraries. Its field keys are the selected arXiv categories; editorial seed assignments do not require a second taxonomy.

**After:**

> **Opening terminology**
>
> **Terminology confirmed by the user (2026-09-10): field and arXiv category are synonyms.** `Λ` is the selected arXiv category set, `λ` is one such category, and `Λ(i)` contains the paper's arXiv-assigned categories within that set. There is no additional category-to-field taxonomy or rollup. Candidate category membership and editorial seed-library membership are different relations over the same category identifiers (§2.10.2). Preserve existing `math.NA` / `cs.NA` alias handling; it does not create a second taxonomy.
>
> **§2.7 — Assignment**
>
> Assignment was implementable before the rest of V2 because it needs neither Rating nor T3. It uses the category keys defined in **Opening terminology**.
>
> **§5 — Migration path and sequence**
>
> Task 2 implemented the feedback half of step 8 (§2.10), leaving T4 rationale generation for later. It had to precede step 3 because that step's `V` upgrade requires reference sets; building `V` first would have meant using empty libraries. Field keys follow **Opening terminology**, and editorial seed memberships follow §2.10.2.
