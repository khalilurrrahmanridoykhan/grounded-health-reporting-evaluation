# LLM Public Health Reporting Trustworthiness — Paper Plan

## Working title

**Grounded Language Models for Routine Health Reporting: A Comparative Evaluation of Factuality, Data-Quality Detection, and Abstention**

## Recommendation

Study whether grounding a language model in deterministic indicator calculations and source records makes routine public-health reporting safer and more useful than asking a model to interpret a report directly.

This is an empirical evaluation paper, not a software-description paper. The contribution must be a reproducible evaluation with held-out reporting scenarios, defensible reference answers, and independent domain-expert review. A Q1 outcome is not guaranteed; journal quartiles vary by year and category.

## Why this fits

The work builds on experience with DHIS2-style reporting, health surveillance, structured LLM output, AI guardrails, and the public-health reporting evaluation contribution. The AMR Stewardship Briefing API provides a practical example of separating deterministic statistics from LLM narration.

The paper must go beyond the existing demonstration benchmark: create a new, held-out evaluation set and answer a research question across multiple system designs. Do not describe the earlier benchmark contribution itself as a completed comparative study.

## Research question

Among systems that generate routine health-programme briefings, does deterministic analysis followed by constrained LLM narration reduce numerical errors and unsupported claims, while preserving useful data-quality detection, compared with direct LLM interpretation?

### Hypotheses

- **H1:** Systems grounded in verified calculations and source evidence produce fewer numerical errors and unsupported claims than direct-prompt systems.
- **H2:** Explicit missing-data and data-quality checks improve detection of reporting defects and increase appropriate abstention when evidence is insufficient.
- **H3:** Grounding reduces factual errors without materially reducing expert-rated usefulness.

Treat these as hypotheses to test, not expected results to assume.

## Study design

Build a task-based benchmark of routine aggregate health reports. Each scenario has input data, a data-quality condition, reference calculations, evidence rows, and a rubric for acceptable conclusions. Use de-identified aggregate programme data if access, permissions, and governance allow. Otherwise use realistic synthetic data, validate its scenarios with domain experts, and state clearly that this limits external validity.

### System arms

1. **Direct prompt:** LLM receives the report tables and is asked to summarize them.
2. **Retrieval-grounded:** LLM receives selected source rows and indicator definitions alongside the report.
3. **Deterministic plus narration:** tested calculations and data-quality checks are supplied to an LLM constrained to narrate those results.
4. **Tool-using agent:** LLM selects from a fixed set of calculation and evidence-retrieval tools; tool inputs and output schemas are validated.

Keep model versions, prompts, decoding settings, tool definitions, and retries fixed and recorded for each experiment. Include at least two model families if access permits. No system may write to a live DHIS2 instance.

### Scenario conditions

Include clean reports and controlled defects such as:

- Missing facilities or reporting periods.
- Late reports and incomplete denominators.
- Duplicate records.
- Negative, extreme, or implausible values.
- Inconsistent totals or conflicting indicators.
- Small denominators where rates are unstable.
- Insufficient evidence to support a trend or recommendation.

Create development scenarios for prompt and tool refinement, then freeze a separate held-out test set before final comparisons. Avoid reusing public benchmark answers in prompts or development examples.

## Outcomes

### Primary outcomes

- **Numerical fidelity:** exact match for counts and denominators; prespecified tolerance for derived rates.
- **Unsupported-claim rate:** claims not entailed by the supplied records, definitions, or calculations, adjudicated against an evidence rubric.

### Secondary outcomes

- Data-quality defect detection: sensitivity and precision by defect type.
- Evidence traceability: proportion of factual claims linked to a valid supporting row or calculation.
- Appropriate abstention: abstention when evidence is insufficient, and unnecessary abstention when it is sufficient.
- Expert-rated usefulness, clarity, and actionability using a predefined rating scale.
- Runtime, token usage, and cost per scenario.
- Stability across repeated runs and model families.

Do not collapse all outcomes into one accuracy score. Report error types and confidence intervals, including failures that could change programme interpretation.

## Reference standard and review

1. Generate expected statistics with independently tested deterministic code.
2. Attach source-row identifiers and calculation steps to each reference answer.
3. Have at least two public-health or monitoring-and-evaluation reviewers independently assess narrative claims using a blinded rubric.
4. Resolve disagreements through adjudication and report inter-rater agreement.
5. Keep reviewers blinded to system arm where the output format makes that feasible.

Document reviewer qualifications and conflicts of interest. The reference standard must not be written by simply copying an LLM response.

## Analysis plan

- Compare paired system outputs on the same held-out scenarios.
- Report estimates with 95% confidence intervals; resample or cluster at the scenario level so multiple outputs from one scenario are not treated as independent data.
- Use a prespecified paired test or regression model appropriate to each outcome and distribution.
- Correct for multiple comparisons on secondary outcomes, or label them exploratory.
- Report results by defect type, not just pooled results.
- Publish all exclusions, failed runs, malformed outputs, and prompt/model changes.

Finalize the sample size and analysis method with a statistician or methods collaborator after piloting scenario variability. Do not choose the sample size solely to reach a desired significance result.

## Data governance and safety

- Prefer public, synthetic, or properly de-identified aggregate data; do not send patient-level data or identifiers to an external model API.
- Obtain institutional and data-owner approval before using non-public programme data; determine whether ethics review or an exemption is required before analysis.
- Use synthetic data for the public release unless sharing the real data is explicitly permitted.
- Make clear that outputs are research artifacts, not clinical advice, official surveillance decisions, or an autonomous alerting service.
- Do not evaluate clinical decision-making or claim that a model detects real outbreaks unless the study is designed and validated for that purpose.

## Minimum credible contribution

- A new held-out benchmark with documented scenario generation and reference calculations.
- A controlled comparison of the four system designs, with reproducible prompts and software.
- Expert-reviewed claim-support and usefulness ratings.
- Error analysis showing when systems fail, including abstention and data-quality edge cases.
- An openly available protocol, code, synthetic data, and model/version details where licensing permits.

If the study has only synthetic scenarios and no independent expert review, frame it as a methods/benchmark study and do not imply real-world operational validity. A stronger applied informatics paper needs realistic external data and domain-expert validation.

## Execution phases

### Phase 0 — Novelty and feasibility check

- Search recent systematic reviews and primary studies on LLM factuality in health reporting, DHIS2 analytics, data-quality detection, and abstention.
- Identify the closest existing benchmarks and write a one-page gap statement explaining what this study adds.
- Confirm a domain-expert collaborator and determine whether de-identified aggregate data can be accessed and shared.
- Stop or revise the paper question if a close benchmark already answers it or data governance cannot be resolved.

**Exit criterion:** a documented novelty gap, feasible data route, and agreed expert-review plan.

### Phase 1 — Protocol and benchmark specification

- Define the unit of analysis, scenario taxonomy, inclusion criteria, primary outcomes, adjudication rubric, and statistical plan.
- Predefine which scenarios are development versus held out.
- Obtain required ethics/data approvals before accessing non-public data.

**Exit criterion:** a frozen protocol and scenario specification reviewed by the collaborator.

### Phase 2 — Build and validate scenarios

- Create scenarios from permitted aggregate data or a documented synthetic generator.
- Add controlled defects without changing unrelated values accidentally.
- Verify all reference calculations independently and attach evidence identifiers.
- Pilot the rubric with reviewers and revise it before the final evaluation.

**Exit criterion:** every held-out scenario has a validated reference answer and review rubric.

### Phase 3 — Implement evaluation arms

- Implement the four approaches with shared input data and consistent output requirements.
- Validate tools and schemas with unit tests; keep calculations outside the LLM.
- Log model identifier, prompt version, inputs, outputs, latency, retries, and cost.

**Exit criterion:** a reproducible run can execute all arms without changing the held-out answers or prompts mid-test.

### Phase 4 — Run and adjudicate

- Freeze configurations, then run the held-out test set.
- Blind outputs for expert review where feasible.
- Adjudicate disagreements before unblinding system arms.

**Exit criterion:** locked results table, adjudication log, and analysis scripts reproduce the reported outcomes.

### Phase 5 — Write and submit

- Report using a suitable health-informatics or clinical-AI reporting guideline and disclose model and API versions.
- Select a journal only after confirming current scope, indexing, article type, and quartile in the relevant year and category.
- Potential scope examples to investigate: *Journal of Biomedical Informatics*, *International Journal of Medical Informatics*, and *Artificial Intelligence in Medicine*. These are candidates, not a claim that any specific journal will be Q1 at submission time.

## First concrete actions

1. Ask one public-health/M&E expert to co-design the scenario taxonomy and review rubric.
2. Identify one permitted aggregate dataset and one synthetic fallback; settle data governance before model experiments.
3. Write the novelty search table: closest paper/benchmark, task, data, systems compared, outcomes, and the gap this study addresses.
4. Draft the protocol and analysis plan before tuning prompts on the held-out scenarios.

## Main risks

| Risk | Mitigation |
|---|---|
| The benchmark is too similar to prior LLM evaluation work | Do the novelty check first; narrow the contribution to routine health-reporting defects, evidence traceability, and abstention only if the literature supports that gap. |
| Synthetic data do not reflect actual reporting practice | Co-design scenarios with programme experts and add permitted external aggregate data for stronger validation. |
| Expert ratings are subjective | Use a prespecified rubric, blinded independent reviewers, agreement reporting, and adjudication. |
| LLM APIs change during evaluation | Pin versions where possible; log dates/configurations; rerun arms under a frozen protocol. |
| Results look good only on aggregate averages | Report defect-specific performance, uncertainty, and serious failure cases. |
| A software demo is mistaken for evidence of safety | Keep research evaluation separate from deployment claims; no autonomous DHIS2 writes or operational recommendations. |