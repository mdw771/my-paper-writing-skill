# Calibrated Style Profile

This profile was distilled from three papers by Ming Du:

1. *Demonstration of an AI-driven workflow for dynamic x-ray spectroscopy* (2025).
2. *Pty-Chi: A PyTorch-based modern ptychographic data analysis package* (2025).
3. *Fidelity-preserving enhancement of ptychography with foundational text-to-image models* (2026).

The guidance below captures patterns shared across the three papers. It is not a phrase bank for copying source prose.

## Cross-Paper Invariants

### The argument is built by technical necessity

The papers rarely introduce a method immediately after a generic statement of importance. They establish the mechanism of the scientific technique, identify operational criteria, compare existing method families, and expose a specific limitation. The new method then appears as the natural response to a set of requirements.

This produces a recurring introduction-level chain:

`scientific value -> operating principle -> present practice -> practical failure -> alternatives and tradeoffs -> unmet requirement -> enabling framework -> proposed design -> validation preview`

The amount of background varies by venue, but the chain remains recognizable in all three papers.

### Concepts are introduced through function

A technique, algorithm, or software component is normally introduced by saying what it measures, predicts, changes, or permits. Naming and citation follow or accompany the functional description. Mathematical concepts are translated into experiment-facing behavior, such as which point is measured next, which image feature is suppressed, or which reconstruction parameter becomes accessible.

### Comparisons are balanced but decisive

Alternatives receive real strengths before their limitations are discussed. Contrast frequently takes the form:

`Approach A has advantage X. However, under condition Y it produces limitation Z. Approach B provides property Q, which better matches the present requirement.`

The prose does not hide adverse findings. Unexpectedly high or low metrics are explained through scale, constraints, sampling, or optimization behavior.

### Results are narrated as experiments, not figure inventories

Subsections usually begin with the purpose of the case, then state the sample or data conditions, parameter settings, baseline behavior, proposed-method result, numerical or visual comparison, and mechanism. More difficult or realistic cases follow controlled demonstrations.

### Discussion is practical

Discussion sections connect algorithmic behavior to deployment constraints: instrumentation, computation time, memory, data requirements, transfer to unseen samples, software maintenance, or access to centralized resources. Future work is presented as an answer to a specific demonstrated limit.

## Quantitative Cadence

A lightweight sentence analysis of the main texts found mean sentence lengths of approximately 29-31 words and medians of approximately 23-27 words. Roughly one quarter to one third of sentences exceeded 35 words. This reflects a preference for explicit, logically connected sentences, not a requirement to maximize sentence length.

The most stable connectors across all papers include:

- `however`, `while`, `although`, and `on the other hand` for tradeoffs;
- `moreover`, `additionally`, and `we also` for extension;
- `therefore`, `thus`, `as such`, and `this allows` for consequence;
- `for example` and `in particular` for concrete cases;
- `first`, `second`, `we first`, and `we then` for ordered reasoning;
- `since` and `because` for causal explanation.

First-person plural is common for research actions and judgments. General scientific facts are normally stated directly, and procedures often use passive voice.

## Section-Specific Variants

### Workflow or adaptive-experiment paper

The domain introduction is expansive because the algorithm must be connected to measurement practice. Results emphasize acquisition sequence, measurement efficiency, error trajectories, and real-time instrument integration. Discussion compares algorithmic steering with hardware-based acceleration and considers movement overhead and dose.

### Software paper

The introduction is criteria-driven: algorithm coverage, runtime, accelerator portability, maintainability, accessibility, and extensibility each receive a paragraph. The contribution is then presented as a feature list. The design section moves layer by layer from backend to forward model, data structures, reconstructors, parallelization, and creative extension. Examples demonstrate capabilities rather than claiming a single universal performance win.

### Hybrid foundation-model paper

The introduction uses a deeper contrast structure: analytical versus deep-learning reconstruction, artifacts in analytical methods, limitations of artifact-specific remedies, benefits of a splitting framework, limitations of task-specific neural processors, and the resulting need for text-guided editing. Results foreground fidelity controls and explicitly explain why visually improved post-processing can still be scientifically worse.

Select the variant that matches the manuscript. Do not combine all three organizational schemes automatically.

## Synthetic Examples

These examples are newly written to demonstrate the style and are not sentences from the source papers.

### Introducing a concept

Weak:

> Foundation models are powerful and have recently attracted much attention for beamline automation.

Calibrated:

> Foundation models learn reusable representations from broad datasets and can therefore support tasks beyond the conditions used for a single experiment. When exposed to an agent through a tool interface, such a model becomes an analysis capability that can be selected during data collection rather than a fixed post-processing step.

The calibrated version defines the object through function, connects it to the experimental system, and avoids an attention-based opening.

### Moving from limitation to design

Weak:

> Existing beamline systems have network issues. We propose a distributed architecture.

Calibrated:

> Beamline control computers often operate inside restricted networks and depend on instrument libraries that are difficult to colocate with modern agent runtimes or GPU inference services. A useful architecture must therefore isolate these environments while preserving a reliable path from an agent decision to instrument action and reconstructed data. We address this requirement with a distributed set of tool servers and inference endpoints connected through explicit data-exchange interfaces.

The intermediate requirement makes the proposal feel derived rather than appended.

### Describing a method

Weak:

> The tool waits for an image and sends it to the agent.

Calibrated:

> After the acquisition command is issued, the tool monitors the exchange directory for the reconstruction produced by the inference pipeline. Once the image appears, the tool loads the array, renders a coordinate-aware preview, and returns the result to the agent, allowing the reconstructed field of view to inform the next action.

The calibrated version exposes sequence, implementation, and consequence.

### Reporting a result

Weak:

> The agent found the feature successfully, as shown in Fig. 4.

Calibrated:

> The agent first acquired a low-resolution survey image and identified two candidate regions from their morphological contrast (Fig. 4a). It then moved the sample to the higher-confidence region and collected a ptychographic image at the requested position (Fig. 4b). The target remained within the field of view after the coordinate transformation, showing that the image interpretation and motor action were consistent across the two acquisition stages.

The calibrated version states sequence, observation, and supported conclusion.

### Explaining a mixed metric

Weak:

> Model A had a better score, but its image looked worse.

Calibrated:

> Model A attained the lower pixel-wise error, but fine structures in its reconstruction were less distinct. This apparently inconsistent result arises because the global intensity scale, which contributes strongly to the metric, was better matched even though the local spatial resolution was reduced. The comparison therefore requires both the scalar error and the feature-level fidelity to be considered.

The calibrated version treats the apparent contradiction as a mechanism to explain.

### Stating a limitation

Weak:

> A limitation is that the system needs several computers.

Calibrated:

> The distributed deployment reduces dependency and network conflicts, but it also introduces operational dependence on several services and data-transfer paths. A failure in any intermediate endpoint can delay completion of the agent's action even when the instrument remains available. Service-level health checks and explicit timeout reporting are therefore important for routine beamline use.

The limitation is bounded, causal, and linked to a practical response.

## Sentence Templates

Use these as structural patterns, not fixed prose:

- `[Technique] measures/enables [capability], making it [scientific role].`
- `The time/cost/error is therefore proportional to [controlling factor].`
- `While [approach] provides [strength], it [limitation] when [condition].`
- `This is not to say that [qualified alternative] is immune to [problem].`
- `To address [requirement] while maintaining [constraint], we consider [framework].`
- `This design brings two desirable properties: first, [property]; second, [property].`
- `We first test [method] with [controlled case] to assess [claim].`
- `As shown in Fig. X, [observation], while [baseline contrast].`
- `This is expected because [mechanism].`
- `The result may appear counterintuitive, but it can be explained by [cause].`
- `A limitation of [method] is that [specific condition and consequence].`
- `One way to address this is to [concrete remedy].`
- `Beyond [current capability], [component] could also [broader application].`

## Common Failure Modes When Imitating the Style

### Excessive background

The source papers are explanatory, but their background is purposeful. Remove history or definitions that do not help establish the problem, method choice, or interpretation.

### Long sentences without logical architecture

Long source sentences normally encode a real relation: condition, cause, contrast, or consequence. Do not join independent facts merely to mimic surface length.

### Mechanical transition insertion

Do not begin every paragraph with “However” or “Moreover.” First determine the relation, then select the connector.

### Promotional claims

The style uses positive adjectives, but claims are anchored in measurements, comparisons, architecture, or demonstrated capabilities. Replace generalized praise with a mechanism and evidence.

### Copying grammatical slips

The papers contain occasional article, agreement, preposition, and number-agreement errors typical of working scientific prose. Correct these. The goal is a polished version of the author's stable voice.

### Repeating conclusions

Results explain observations; Discussion interprets boundaries and implications; Conclusion synthesizes. Do not restate the same claim with different adjectives in all three places.

## Close-Match Review Procedure

For a style diagnosis or high-fidelity revision:

1. Reverse-outline the target passage into rhetorical moves.
2. Compare the move sequence with the appropriate section recipe.
3. Mark each sentence as context, mechanism, limitation, design, evidence, interpretation, qualification, or implication.
4. Repair missing links, especially limitation -> requirement and observation -> explanation.
5. Check transition words against their actual relations.
6. Check sentence rhythm: retain useful compound reasoning, but split overloaded claims.
7. Replace copied or conspicuously distinctive source phrases with fresh wording.
8. Perform a final grammar and claim-evidence pass.
