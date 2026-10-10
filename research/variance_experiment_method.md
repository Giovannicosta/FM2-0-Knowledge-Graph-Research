# Predicate-variance experiment — methodology and audit notes

## Research question
When an LLM is allowed to invent its own predicate instead of choosing from a fixed ontology vocabulary, how do predicate naming, diversity, and contextual explanations change for the same subject-object pairs?

## Input and inference
The input vocabulary consisted of 49 unique extracted FM 2-0 terms with their CSV definitions (50 source records). CSV term and definition strings were treated as immutable. Ollama ran the `llama3.2:3b` model in Google Colab with an NVIDIA Tesla T4 GPU.

### Stage A — fixed vocabulary
The model generated candidate subject-predicate-object relationships from definitions and model knowledge, using an allowed set of 19 predicate names. It initially retained 18 relationships and then performed a separate expansion using targeted small candidate sets to reach 70 relationships. This stage did not verify each triple directly against a PDF passage. The expansion used up to 250 tries and stopped with 70 valid, unique candidate pairs.

### Stage B — unconstrained predicates
For each fixed-stage subject-object pair, the model received the unmodified subject and object definitions. The fixed predicate **was not included** in the dynamic prompt. It returned a new lower_snake_case predicate, predicate meaning, operational context, rationale and a self-reported basis (definition or model knowledge). Each result was screened for JSON structure and minimum string lengths. The same endpoints remained fixed.

### Stage C — paired comparison
The study compared predicate names, number of unique predicates, and acceptance counts. The supplied experiment ZIP contains the complete checkpoint, paired comparison CSV/JSON, and summary JSON.

## Observations (from supplied logs)
| Quantity | Value |
|---|---:|
| Fixed relationships | 70 |
| Allowed fixed predicate types | 19 |
| Distinct fixed predicate types used | 15 |
| Dynamic evaluations | 70 |
| Dynamic accepted | 69 |
| Dynamic failed | 1 |
| Distinct dynamic predicate names | 39 |
| Accepted pairs with different predicate names | 62 |
| Fixed expansion attempts in checkpoint | 28 (additional expansion iterations; initial 18 were loaded) |

## Validity and limitations
- All relationships are **LLM-generated candidates**; structural acceptance does not entail domain correctness.
- Term pair selection was conditioned on fixed-stage outputs, so this design cannot measure independent recall/coverage of dynamic predicate discovery.
- Different predicate text is **not** a measure of semantic difference. Synonyms and reversed relation direction require expert evaluation.
- Model output is potentially stochastic. One run does not establish stable variance. Repeated seeds/runs and inter-rater review are needed.
- Dynamic context is a model-authored explanation rather than independently verified evidence.
- Some dynamic names (`has_definition`, `has_subject`, `operational_scope`) may fail a stricter relation-label ontology-quality rubric.
- Source FM 2-0 PDF and input terms CSV are not in the uploaded study ZIP; source evidence must not be inferred from their absence.

## Suggested next experiments
1. Normalize predicate aliases and evaluate semantic equivalence with blinded human annotations.
2. Score relationship validity and direction separately from predicate vocabulary.
3. Re-run inference 5+ times at explicit decoding settings and compare within-pair agreement.
4. Check candidate relationships against the full FM 2-0 source and record citations or unsupported labels.
5. Test independent pair discovery with dynamic predicates to assess coverage, not only naming.

## Audit status
This file summarizes the **real experiment reported in the uploaded artifacts**. No synthetic experiment logs have been invented. The full original artifacts were in `predicate_variance_study.zip` supplied in this conversation; they must be committed separately if not present under `data/predicate_variance_study/`.
