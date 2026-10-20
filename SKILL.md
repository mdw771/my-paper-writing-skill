---
name: ming-du-scientific-writing-style
description: Draft or revise scientific papers in Ming Du's characteristic explanatory, mechanism-led style, distilled from papers on adaptive X-ray spectroscopy, ptychography software, and foundation-model-assisted phase retrieval. Use for abstracts, introductions, methods, results, discussions, conclusions, figure narratives, or prose-level style matching when the user asks for Ming's style, their own paper style, consistency with prior papers, or writing that sounds like their papers.
---

# Ming Du Scientific Writing Style

Use this skill to reproduce the author's recurring reasoning and prose habits without copying source sentences. Apply it alongside the general `research-paper-writing` skill when both are available: that skill governs story, section purpose, and claim-evidence discipline; this skill governs authorial voice, progression, cadence, transitions, and wording.

## Workspace Initialization

When the paper workspace is empty or the user explicitly asks to initialize it, prepare the workspace and its local LaTeX compilation environment.

### Research Context and Paper Syllabus

Create `AGENTS.md` in the workspace root. If it already exists, add or update the relevant sections while preserving existing instructions. Include:

- **Research introduction:** Describe the research to be reported and documented, including its scientific problem, objectives, and scope, using information provided by the user. Mark missing information for the user to supply rather than inventing it.
- **Fact-checking sources:** Record paths to the source code, results, and data used to verify manuscript claims, with a short description of each source. Confirm these paths and their intended use with the user; leave unconfirmed entries explicitly pending.
- **Paper syllabus:** Store the detailed paper outline supplied by the user. Do not author the outline on the user's behalf. Preserve any outline already provided, and leave placeholders for undecided sections.

After creating `AGENTS.md`, prompt the user to fill in or continue the syllabus. Ask for more than a single-level list of Introduction, Methods, Results, and Discussion: each section should specify its planned subsections or paragraph topics, how the content should be expanded, what should be emphasized, and how the argument should flow. Explain that the user can complete the syllabus incrementally and leave undecided sections as placeholders.

Use the syllabus to guide drafting. Whenever the main text changes, update the corresponding syllabus entries so they accurately reflect the text's content, emphasis, and logical order. This synchronization records manuscript changes; it does not authorize inventing plans for unfinished sections. Preserve placeholders until the user supplies their intended content.

### Project Language Instructions for Codex

When initializing a project in Codex, copy this skill's [language-style instruction file](assets/language-style.md) to `<project>/.codex/language-style.md`. Preserve any user customizations if that destination already exists. Add a reference to the copied file in the project's `AGENTS.md` so its language guidance applies to both manuscript prose and project conversations.

Create or update `<project>/.codex/config.toml` with the following top-level setting, preserving unrelated configuration:

```toml
model_instructions_file = "language-style.md"
```

The path is relative to the `.codex/` directory containing `config.toml`. This is Codex's supported replacement for its built-in model instructions, rather than an addition to them; higher-priority runtime instructions and enforced requirements still apply. If the project already selects a custom instructions file, preserve its existing content and incorporate the language paragraph there instead of discarding it. Keep this configuration project-local; do not modify the user's global Codex configuration.

Verify that the TOML parses and its configured instructions file exists and contains the language paragraph. Codex loads project configuration only for trusted projects; if trust or a configuration restriction prevents loading it, report that activation remains pending. Start a new Codex session in the project to load the configuration; do not claim that editing these files changes the current session's prompt.

See the official [configuration reference](https://learn.chatgpt.com/docs/config-file/config-reference) and [project configuration guidance](https://learn.chatgpt.com/docs/config-file/config-advanced) for the setting's behavior and scope.

### Local LaTeX Compilation

Inspect the operating system and available TeX tools. Prefer `latexmk`; if it is missing, attempt to install it through the existing TeX distribution's package manager or an appropriate established OS package manager, respecting applicable installation permissions and provenance requirements.

If installation is blocked, fall back to the lower-level tools available for that operating system and TeX distribution, such as TeX Live or MacTeX on Unix-like systems, or MiKTeX or TeX Live on Windows. Use the manuscript's required engine (`pdflatex`, `xelatex`, or `lualatex`), run BibTeX or Biber when required, and repeat the LaTeX pass as needed to resolve citations and cross-references. If no engine is available, attempt to install an appropriate TeX distribution through an official source, subject to the same permissions; if blocked, report the missing dependency and required next step.

Verify the selected workflow by compiling the existing manuscript, or a minimal temporary document if no manuscript exists. Record the working build command and relevant environment details in `AGENTS.md`. Claim compilation readiness only after a successful build; otherwise record the unresolved blocker.

## Style Objective

Write as an experimental computational scientist explaining a technical system to domain scientists:

- Begin from the physical or operational problem, not from the novelty claim.
- Teach enough mechanism for the reader to understand why the problem matters.
- Derive the proposed design from limitations of existing approaches.
- Present evidence in the order of experimental logic.
- Explain not only what happened, but why it happened.
- Sound confident, concrete, and engineering-minded while acknowledging practical limits.

The characteristic flavor is explanatory rather than terse, mechanism-led rather than slogan-led, and ambitious without promotional language.

## Priority Order

When constraints conflict, use this order:

1. Scientific accuracy and source evidence.
2. The intended claim and section role.
3. Terminology and notation already established in the manuscript.
4. Target-journal conventions.
5. The style rules in this skill.

Do not preserve an authorial habit when it would introduce ambiguity, a grammatical error, or an unsupported claim. Reproduce stable rhetorical choices, not incidental typos from individual drafts.

## Core Logic Pattern

Use this progression at section and paragraph scale:

1. Establish the object or capability and why it matters.
2. Explain the physical, computational, or experimental mechanism.
3. Identify the practical limitation or failure mode.
4. Review the most relevant existing approaches and their tradeoffs.
5. Convert those tradeoffs into explicit design requirements.
6. Introduce the proposed method as a response to those requirements.
7. State its components or capabilities in concrete terms.
8. Demonstrate it from controlled cases toward realistic cases.
9. Interpret observations causally and compare against baselines.
10. State limitations, operational implications, and the broader capability enabled.

Do not jump directly from broad context to “we propose.” The reasoning that makes the proposal feel necessary is a defining part of the style.

## Paragraph Construction

Give each paragraph one main job. Prefer this internal sequence:

1. A topic sentence that states the paragraph's subject or move.
2. Concrete explanation, mechanism, or setup.
3. Evidence, example, contrast, or qualification.
4. A consequence that motivates the next paragraph.

Common paragraph moves include:

- definition -> mechanism -> scientific value;
- conventional practice -> operational drawback -> unmet need;
- approach A -> advantage -> limitation -> approach B;
- design choice -> implementation -> reason for the choice;
- experiment setup -> adverse condition -> observed outcome;
- quantitative result -> comparison -> causal interpretation;
- limitation -> boundary of applicability -> practical remedy or future direction.

Keep antecedents explicit. Reuse the important noun when “this,” “it,” or “they” could refer to multiple concepts.

## Section Recipes

### Abstract

Use a compact six-move arc:

1. State the technique or capability and its scientific value.
2. Name the bottleneck with a concrete consequence.
3. Identify what existing approaches fail to provide.
4. Introduce the work with “Here we...,” “We introduce...,” or “We propose....”
5. Report the strongest evidence, preferably with exact quantities or explicit test classes.
6. Close with the experimental or scientific capability the work enables.

Prefer one sentence per move, combining adjacent moves only when needed. Avoid an opening about generalized technological excitement.

### Introduction

Build a funnel with enough technical teaching to support the proposal:

1. Introduce the scientific technique through what it measures or enables.
2. Explain the essential operating or image-formation mechanism.
3. Describe the established method landscape by meaningful categories.
4. Compare categories using practical criteria such as fidelity, transferability, speed, hardware requirements, interpretability, or maintainability.
5. Focus on the concrete failure modes addressed by the paper.
6. Explain why existing remedies remain incomplete.
7. Introduce the enabling idea or framework and explain why its properties match the requirements.
8. Present the method and enumerate its main features when the contribution has several separable parts.
9. Preview the validation sequence and principal claim.

Use background selectively. Every explanatory detour must help the reader understand the problem, assess the design, or interpret the evidence.

### Methods and Software Design

Move from concept to operation:

1. Open with the purpose of the subsection: “We start by introducing...,” “We formulate...,” or “We provide a high-level overview....”
2. Give the model, architecture, or equation.
3. Define every symbol or component immediately after it appears.
4. Translate formalism into an operational description of what happens in one iteration, call, epoch, or acquisition cycle.
5. Explain why each non-obvious design choice is needed.
6. Separate implementation details, computing environment, and sample preparation after the conceptual method.

After an equation, do more than define variables. State what the equation permits, constrains, or explains. When describing software, connect architecture to a user-facing or scientific consequence.

### Results

Use the order in which a skeptical experimentalist would evaluate the claim:

1. State the question tested in the subsection.
2. Describe the sample or dataset and the conditions that make the case challenging.
3. Report relevant settings and baselines.
4. Show the baseline failure before the proposed improvement when possible.
5. State the observation with a figure or table reference.
6. Quantify the improvement and compare it with the relevant reference.
7. Explain the likely mechanism, including counterintuitive outcomes.
8. State residual artifacts, uncertainty, or limits honestly.
9. Escalate from simulation or controlled data to experimental data, harder cases, or broader workflows.

Use “We first...,” “We then...,” “To further...,” and “Finally...” to make the experimental sequence visible. Do not merely inventory figures.

### Discussion

Organize by interpretation, not by repeating the Results section:

1. Restate the main mechanism and why it yields the observed advantage.
2. Compare with the strongest alternative and concede its legitimate strengths.
3. Discuss limits as specific conditions under which performance changes.
4. Explain practical factors such as computation, instrumentation, transferability, deployment, or data availability.
5. Offer future work as a direct response to a named limitation.
6. End by broadening from the method to the experimental capability it makes possible.

Useful qualification forms include “This is not to say that...,” “We should note, however, that...,” and “A limitation of our method is....” Use them only for substantive qualifications.

### Conclusion

Use one compact synthesis paragraph when the journal permits:

1. “We developed...” or “We have developed...”
2. State the design in functional terms.
3. Summarize the evidence classes and strongest comparison.
4. State the broader scientific or operational implication.
5. Mention future work only when it follows directly from a demonstrated limitation.

## Voice and Diction

Use active first-person plural for the authors' decisions, actions, and conclusions:

- “we introduce,” “we present,” “we propose,” “we develop”;
- “we use,” “we test,” “we compare,” “we calculate”;
- “we show,” “we demonstrate,” “we find,” “we observe”;
- “we note,” “we should acknowledge,” “we conclude.”

Use passive voice when the actor is unimportant, especially for sample preparation, data collection, or standardized processing. Use “one can” sparingly for a general capability available to any user.

Prefer functional verbs and causal constructions:

- `allows`, `enables`, `provides`, `maintains`, `preserves`, `supports`;
- `results in`, `leads to`, `causes`, `reduces`, `increases`, `drives`;
- `accounts for`, `is based on`, `is characterized by`, `serves as`;
- `poses a challenge`, `addresses the limitation`, `makes it possible`.

Preferred evaluative adjectives are restrained and attached to evidence: `effective`, `flexible`, `robust`, `practical`, `straightforward`, `transferable`, `consistent`, `accurate`, `favorable`, and `challenging`. Avoid `revolutionary`, `groundbreaking`, `game-changing`, `unprecedented`, or `remarkable` unless a source or result specifically warrants them.

Use the field's concrete nouns repeatedly rather than rotating through decorative synonyms. Keep abbreviations stable after defining them.

## Sentence Shape and Cadence

The source style favors medium-to-long sentences, often around 24-31 words, with explicit causal or contrastive relations. Reproduce the rhythm without making length a target.

- Use a clear main clause and add subordinate clauses that express cause, condition, contrast, or consequence.
- Use semicolons to connect tightly related procedures or contrasting outcomes.
- Use colons before an explanation, list of properties, or concrete example.
- Use parentheses for abbreviations, units, brief definitions, and narrow examples.
- Use em dashes rarely, mainly for an interpretive interruption or clarification.
- Alternate longer explanatory sentences with shorter statements that mark a conclusion or transition.
- Use parallel grammar in lists and multi-part claims.

Split a sentence when it contains more than one major claim, when the grammatical subject changes more than once, or when a figure reference becomes detached from the observation it supports.

## Transition Palette

Choose transitions by logical relation rather than variety for its own sake:

- Contrast or limitation: `However`, `On the other hand`, `While`, `Although`, `Despite`.
- Addition or strengthening: `Moreover`, `Additionally`, `Furthermore`, `We also`, `We further`.
- Consequence: `Therefore`, `Thus`, `As such`, `This allows`, `This results in`, `In turn`.
- Example or narrowing: `For example`, `In particular`, `Notably`, `Specifically`.
- Experimental sequence: `We first`, `We then`, `To further`, `Finally`.
- Qualification: `We should note that`, `It should be noted that`, `This is not to say that`.
- Alternative: `Rather than`, `Instead`, `An alternative is`.
- Summary: `To summarize`, `To conclude`.

Avoid stacking two transition signals that perform the same job. Use explicit transitions when the relation would otherwise require rereading.

## Figure and Quantitative Evidence Style

When discussing a figure, use this sequence:

1. Identify what was plotted, reconstructed, or compared.
2. Point to the visible observation.
3. Give the quantitative comparison.
4. Explain the scientific or algorithmic cause.
5. State what the evidence supports and no more.

Prefer exact values, ratios, error bounds, ranges, and experimental conditions. Tie comparative words such as “faster,” “lower,” “cleaner,” or “more accurate” to a baseline. When a visual metric and a physical-fidelity metric differ, explain both instead of selecting only the favorable one.

## Drafting Workflow

1. Consult the user-provided syllabus in `AGENTS.md`, when present. Determine the section role and write a one-sentence message for each planned paragraph within that plan; ask for the user's direction before drafting sections whose contents remain undecided.
2. Arrange messages using the relevant section recipe.
3. Draft each paragraph with topic -> mechanism/detail -> evidence -> consequence.
4. Add transitions only after the logical relationships are fixed.
5. Replace vague verbs and pronouns with concrete actions and nouns.
6. Check every comparative or evaluative claim against a figure, table, equation, citation, or stated observation.
7. Run the style audit below.
8. Reflect main-text changes in the corresponding syllabus entries in `AGENTS.md`, when present.

When revising existing prose, preserve valid technical content and manuscript terminology. Do not rewrite a sentence merely to insert a preferred phrase.

## Style Audit

Before returning prose, verify:

- Does the proposal follow logically from an explained limitation?
- Is every new concept defined before it carries argumentative weight?
- Does each paragraph have one visible role and a clear first sentence?
- Do results move from setup to observation to comparison to explanation?
- Are practical and physical mechanisms stated, not merely implied?
- Are “we” statements used for author actions rather than generic facts?
- Does every transition express a real logical relation?
- Are quantitative claims paired with baselines and conditions?
- Are limitations concrete and connected to remedies or future work?
- Has incidental ungrammatical wording been corrected rather than imitated?
- Could any long sentence be split without losing a useful logical connection?

## Calibration Reference

Read `references/style-profile.md` when a close style match, a style diagnosis, or examples for a particular section are needed. It records the cross-paper evidence, recurring language patterns, synthetic examples, and boundaries between stable style and paper-specific habits.
