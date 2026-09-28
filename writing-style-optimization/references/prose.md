# Prose

## 1. Role and Boundary

This file is loaded by `SKILL.md` after the Writing Style Gate passes. Prose performs a Style-Guided Rewrite of the Linearized Structure Draft using the approved Writing Style Map and the Task Contract projection for this stage.

Prose may adjust wording, syntax, sentence boundaries, and the surface presentation specified by the Map. It improves how established content is delivered.

Prose uses Model II: rewrite the text, check the rewritten candidate with the Prose Gate, and revise on Fail. Pass permits delivery as Final Output. The Writing Rules guide the rewrite; they are not a sequence of mandatory audit passes.

## 2. Inputs and Style Application

| Input | Use in Prose |
|---|---|
| Linearized Structure Draft | The text to rewrite, including its established content, semantic relations, grouping, Presentation Order, and hard presentation constraints. |
| Writing Style Map | The Default Language Profile, local Language Profile overrides, Surface Realization, selected transforms, and override scopes to implement. |
| Task Contract projection | The relevant Requested Objectives and Deliverable Contracts, and requirements whose `target stages` include Prose. Objectives provide the reference for response completeness. |
| Selected transform files | The writing rules to apply within the override scopes recorded in the Map. |

Read the stage projection from the Task Contract, which remains a continuously indexable, read-only control artifact. Use existing links to consult upstream material when checking preservation.

Apply the Map's Default Language Profile, local overrides, and Surface Realization to their assigned units or groups. Where the Map selects a transform, read its body at the `file path` recorded in the Active Transforms Index in `references/writing-style-selection.md`. Apply its rules only within the assigned override scope. Outside that scope, continue with the applicable default rules and local overrides.

When the Map calls for a supplied writing sample, match its sentence length, word choice, punctuation, openings, and transitions within the Rewrite Constraints. Prose implements the recorded style choices; it does not rebuild the Map.

## 3. Rewrite Constraints

- **Preserve established meaning.** Rewriting must preserve the content and semantic relations established in the Linearized Structure Draft, within its grouping and presentation constraints.
- **Preserve referential identity.** Keep established names, technical terms, and other identifying expressions stable so that rewriting does not change or obscure what they refer to.

These constraints apply to every writing rule and selected transform.

## 4. Writing Rules

Use the following rules where they improve the text. Replace a flagged pattern only when the revision improves precision, evidence linkage, or information flow. Identify observable features; never infer or allege AI authorship from these features.

### 4.1 Sentence Construction

**Actors and actions.** Use active voice when it makes the actor and action clearer. Put the action in the verb, not in an abstract noun. A verb buried in a noun makes the reader dig:

| Smothered form | Direct verb |
|---|---|
| Conducts an assessment of | Assesses |
| Performs an analysis of | Analyzes |
| Provides a description of | Describes |

Keep an abstract noun when it names a settled concept the reader knows ("the migration", "the review"); the rule targets buried actions, not technical nouns.

**Passive voice.** Do NOT mechanically convert every passive sentence. Flag the ones where the passive obscures accountability or the actor. Passive voice is acceptable when:

- The actor is genuinely unknown or irrelevant.
- Disciplinary or genre conventions require it.
- The sentence deliberately emphasizes the object over the actor.

**Buried predicates.** Check whether intervening material makes the connection between the subject and main verb difficult to follow. Restructure the sentence when it does, preserving the intervening content and its scope.

**Punctuation for efficiency.** A colon can set up a list or specific explanation; a dash can mark a parenthetical; semicolons can link closely related independent clauses. Choose punctuation that makes the existing relation clear and fits the assigned style.

**Simple verbs.** When longer phrases replace simple verbs without changing the meaning, use *is*, *are*, and *has*.

### 4.2 Local Information Flow

- Place familiar information before new information when it helps comprehension; keep the grammatical subject aligned with the entity being discussed.
- Put the stress at the end when it supports the intended emphasis. The last words of a sentence carry the most weight, so land on the point, not on an afterthought.
- Keep a consistent string of topics through a paragraph. If consecutive sentences switch subjects with every line, the reader loses the thread.
- Use the end of one sentence to set up the beginning of the next when that expresses their existing relation.
- Use transitions that name the actual relation—contrast, cause, condition, extension, qualification, or consequence. Do not insert a transition merely to make adjacent paragraphs sound connected.
- Read the preceding and following paragraphs before accepting a local rewrite.

These choices operate within the established Presentation Order and semantic relations.

### 4.3 Terminology and Repetition

Use one stable term per concept. Resolve vague referents locally. Synonym variation for technical terms forces the reader to wonder whether a new category has been introduced. Check key terms against the Linearized Structure Draft and flag substitutions that change or obscure a defined term.

Do not rotate technical terms merely for variety. Separate four cases:

1. Required repetition of a defined construct, model, population, or outcome.
2. Useful repetition that preserves a section's subject or comparison.
3. Avoidable repetition of a nontechnical word, phrase, or sentence opening.
4. Exact or near-duplicate prose that adds no new evidence or reasoning.

Preserve necessary technical repetition and do not use synonym rotation as a cosmetic fix. Remove redundant wording while retaining the information and established structure it carries.

**Repeated sentence openings.** Do not ban the repeated word. Writers also repeat an opening on purpose for rhythm, as in "She came. She saw. She conquered." Revise mechanical repetition only when the change preserves topic continuity and the intended emphasis.

### 4.4 Redundancy and Rhetorical Inflation

Judge each pattern in context. Leave a watched phrase alone inside a quotation, a title, a proper name, or a passage that discusses the phrase rather than uses it. Act on a weak signal only when several signals share a passage and the revision improves the expression. Keep the details that carry the writer's voice unless they hurt the meaning.

Apply the Before/After pairs within the Rewrite Constraints: retain supported details, and use existing material for any clarification added by a rewrite.

**Not X but Y.** The negative half names something no one claimed, so the positive half sounds larger. It adds weight without adding a claim. State the point directly. Keep a contrast only when the negative half corrects a belief the reader actually holds, or when both halves carry information.

**Before:**

> It's not just about the beat riding under the vocals; it's part of the aggression and atmosphere. It's not merely a song, it's a statement.

**After:**

> The heavy beat adds to the aggressive tone.

**Before (split across sentences):**

> This does not mean every choice is equal. It means there is no external system that confirms which choice is right.

**After:**

> No external system confirms which choice is right, although the choices still have different consequences.

**Before (clipped tail):**

> The options come from the selected item, no guessing.

**After:**

> The options come from the selected item without forcing the user to guess.

**One-line closers and dramatic fragments.** The line asks the reader to pause on a claim instead of adding to it. One short sentence can carry emphasis when it carries a new fact. Cut a closer that repeats. Merge a row of fragments into a sentence with a specific claim, within the established grouping.

**Before:**

> Then AlphaEvolve arrived. It had no preference for symmetry. No aesthetic prior. No nostalgia for human taste. The old rules were gone.

**After:**

> AlphaEvolve changed the search because it did not favor symmetry or human-looking designs. That made some of the older assumptions less useful.

**Before (repeated closer):**

> Caching cuts repeat work.
>
> That is the real win.
>
> Retries hide brief outages.
>
> That is the real win.

**After:**

> Caching cuts repeat work.
>
> Retries hide brief outages.

**Sayings that sound deep.** An ordinary point is dressed as a hidden truth or an aphorism, and the dressing adds no detail. Replace the saying with the specific claim.

**Before:**

> The real question is whether teams can adapt. At its core, what really matters is organizational readiness.

**After:**

> The question is whether teams can adapt. That mostly depends on whether the organization is ready to change its habits.

**Before (aphorism):**

> Symmetry is the language of trust. Efficiency becomes a trap when teams forget the human layer.

**After:**

> Symmetric layouts often feel more predictable to users. Teams can over-optimize workflows and miss how people actually use them.

**Staged run-up before the point.** The writer announces the point or stages a moment of candor instead of making the point. Remove the run-up, not just its tone. "Honestly" or "look" inside a casual sentence is ordinary; the tell is the standalone opener before a routine claim.

**Before:**

> Let's dive into how caching works in Next.js. Here's what you need to know.

**After:**

> Next.js caches data at multiple layers, including request memoization, the data cache, and the router cache.

**Before (staged candor):**

> Is it worth the price? Honestly? It depends on how often you'll use it.

**After:**

> Whether it's worth the price depends on how often you'll use it.

**Arguing with no one.** The text answers an objection or rejects an option that appears nowhere else, usually a leftover from an earlier draft. Remove the defense; if it holds a real claim, state the claim. Keep an objection the text attributes or answers in full, and keep an option a reader would actually weigh.

**Before:**

> This isn't mainly about prompt length, and I'm not arguing that documentation doesn't matter. You could categorize the problem another way, but the issue is whether the agent can use the instruction when it acts.

**After:**

> The issue is whether the agent can use the instruction when it acts.

**Before (fake alternative):**

> Session tokens are rotated every 24 hours. A tempting approach would be to rotate them by restarting the auth service on a cron job, but that would drop every active session. Rotation happens in place, and clients refresh transparently.

**After:**

> Session tokens are rotated every 24 hours, in place, and clients refresh transparently.

**Forced triads.** Ideas arrive in threes to sound complete, whether the meaning has three parts or not. Check that each item adds a distinct idea. Remove redundant wording without dropping distinct content. Keep three real items when the meaning needs three.

**Before:**

> The event features keynote sessions, panel discussions, and networking opportunities. Attendees can expect innovation, inspiration, and industry insights.

**After:**

> The event includes talks and panels. There's also time for informal networking between sessions.

**Before (paragraph scale):**

> A career can look promising and fail. A relationship can feel important and end. A skill can take years and remain useless. These decisions rarely explain themselves.

**After:**

> A career can look promising and fail. So can a relationship that felt important and ended, or a skill that took years and remained useless. These decisions rarely explain themselves.

**Stacked qualifiers.** Repeated editing adds one qualifier after another until every claim sounds uncertain, usually to repair an earlier overstatement rather than to report real doubt. Consolidate redundant qualifiers while preserving the established uncertainty and scope. Keep scope statements, legal and safety notices, and real corrections. Ordinary hedges such as *perhaps* or *tends to* are human habits and not tells. Weak alone.

**Before:**

> It could potentially possibly be argued that the policy might have some effect on outcomes.

**After:**

> The policy may affect outcomes.

**Inflated significance.** An ordinary detail is said to mark a change, prove a legacy, or promise a future. Keep the fact and drop unsupported rhetorical inflation. Preserve significance, implications, and plans that the established content supports.

**Before:**

> The Statistical Institute of Catalonia was officially established in 1989, marking a pivotal moment in the evolution of regional statistics in Spain. This initiative was part of a broader movement across Spain to decentralize administrative functions and enhance regional governance.

**After:**

> The Statistical Institute of Catalonia was established in 1989, part of a wider decentralization of administrative functions in Spain.

**Before (stock section):**

> Despite its industrial prosperity, Korattur faces challenges typical of urban areas, including traffic congestion and water scarcity. Despite these challenges, with its strategic location and ongoing initiatives, Korattur continues to thrive as an integral part of Chennai's growth.

**After:**

> Korattur has recurring traffic congestion and water shortages.

**Before (send-off):**

> The future looks bright for the company. Exciting times lie ahead as they continue their journey toward excellence.

**After:**

> (Cut the paragraph. End on the last concrete fact.)

**Vague connection or association.** The text says two things are connected without saying how. "He was associated with the leadership of ExampleCorp" hides whether he was the CEO, a board member, or a consultant. Name the relationship the source gives. If the source does not say, keep the vague wording rather than inventing a role.

**Before:**

> He is associated with the Rajhans Orchestra, which he founded and conducts. The concerts were organised in connection with the celebrations of Pakistan's 50th anniversary.

**After:**

> He founded and conducts the Rajhans Orchestra. The concerts were part of the celebrations of Pakistan's 50th anniversary.

**Shallow -ing riders.** An -ing phrase is bolted onto a simple fact to make it sound deeper. Attaching it to a named source does not make it true. Keep the fact; keep the rider only when the source supports what it claims.

**Before:**

> The temple's color palette of blue, green, and gold resonates with the region's natural beauty, symbolizing Texas bluebonnets, the Gulf of Mexico, and the diverse Texan landscapes, reflecting the community's deep connection to the land.

**After:**

> The temple is painted blue, green, and gold, colors meant to evoke Texas bluebonnets and the Gulf of Mexico.

**Sales language.** When promotional language conflicts with the assigned Language Profile, state what the thing is and remove the promotional dressing.

**Before:**

> Nestled within the breathtaking region of Gonder in Ethiopia, Alamata Raya Kobo stands as a vibrant town with a rich cultural heritage and stunning natural beauty.

**After:**

> Alamata Raya Kobo is a town in the Gonder region of Ethiopia.

**Borrowed authority.** A name or an unnamed authority stands in for what was said. Unnamed experts prop up a claim; a list of prestige outlets props up a person. When the source text names the real source and what it said, use that. Never invent a source. A missing citation alone is not a tell; most writing is unsourced. Remove unsupported appeals added during rewriting while preserving established attribution and unresolved source limitations.

**Before (unnamed authority):**

> Due to its unique characteristics, the Haolai River is of interest to researchers and conservationists. Experts believe it plays a crucial role in the regional ecosystem.

**After:**

> Researchers and conservationists study the Haolai River for its unusual characteristics.

**Before (prestige list):**

> Her views have been cited in The New York Times, BBC, Financial Times, and The Hindu. She maintains an active social media presence with over 500,000 followers.

**After:**

> Her views have been cited in The New York Times and the BBC.

**Chatbot residue.** A chatbot's greeting, praise, offer, or closing remains in text that should stand on its own. Remove the wrapper and keep the content. Retain salutations or sign-offs when they belong to the requested Deliverable.

**Before:**

> Great question! Here is an overview of the French Revolution. It began in 1789 when a financial crisis and food shortages led to widespread unrest. I hope this helps! Let me know if you'd like me to expand on any section.

**After:**

> The French Revolution began in 1789 when a financial crisis and food shortages led to widespread unrest.

## 5. Prose Gate and Revision

Audit the exact text that will be delivered after the final rewrite, not the source paragraph or an earlier candidate. The Prose Gate checks the result in three dimensions.

### 5.1 Meaning and Structure Preservation

Verify that the rewritten text preserves established meaning and referential identity. Check whether the rewrite added or dropped any fact, name, number, date, quote, citation, ranking, or claim that things happen at once. Preserve qualifications, uncertainty, attribution, inference boundaries, and unresolved or conflicting information.

Verify that the established semantic relations, grouping, Presentation Order, and hard constraints remain intact, and that the relevant Requested Objectives remain addressed.

### 5.2 Style and Requirement Compliance

Verify that the Default Language Profile, local overrides, Surface Realization, and selected transforms are correctly realized within their assigned scopes. Check compliance with the applicable Prose requirements and Deliverable Contracts.

### 5.3 Readability and Economy

Check referents, sentence construction, and flow with the preceding and following text. Review exact and semantic repetition, stock transitions, vague synthesis, excessive summary, rhetorical inflation, and chatbot residue. Necessary explanation and technical repetition must survive the cleanup.

### 5.4 Revision

On Fail, revise the Prose candidate. State each point naturally instead of patching flagged phrases one at a time. If a sentence stays awkward, rewrite the surrounding text within its established semantic grouping and constraints.

Recheck the affected content and its context. Findings from the pre-edit text do not certify the revision. If any word changes after this gate, repeat the affected checks on the new exact candidate.

Fail returns only to Style-Guided Rewrite. Task Contract, Raw Material, Dependency Graph, Linearized Structure Draft, and Writing Style Map remain unchanged. This stage sets no separate retry count.

## 6. Final Output

The exact text that passes the Prose Gate becomes Final Output. Deliver it according to the Deliverable Contract and the completion conditions in `SKILL.md`. Read requirements routed to `final delivery check` from the Task Contract and verify them before delivery.

Return the requested Deliverables. Include drafts, critiques, or review findings only when they are part of the requested output.
