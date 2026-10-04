# Study Protocol — Draft v0.1

**Title:** Grounded Language Models for Routine Health Reporting: A Comparative
Evaluation of Factuality, Data-Quality Detection, and Abstention

**Status:** DRAFT (single author). Not frozen. Items marked `[DECIDE]`
need a decision by the study team; items marked `[PILOT]` are set after the
pilot. Once frozen, this document is registered (OSF) before any held-out run,
and every later change goes in the deviation log (Section 14).

**Reporting guideline:** TRIPOD-LLM (Gallifant et al., *Nature Medicine*, 2025).
Complete the checklist alongside the manuscript.

---

## 1. Background and rationale

LLMs are being attached to routine health information systems (for example
DHIS2) to summarise programme data. Routine data often contain incomplete
reporting, late reports, duplicates, implausible values and unstable rates.
A model that narrates these data directly may state wrong numbers, make
claims the data do not support, or miss data-quality problems. One proposed
safeguard is to compute indicators and data-quality checks in tested code and
let the model only narrate the results. There is no controlled evidence on
whether this works for routine reporting. See `docs/novelty/literature-gap.md`.

## 2. Objectives and hypotheses

**Primary objective:** compare numerical fidelity and unsupported-claim rate
across system designs that generate routine health-programme briefings.

- **H1:** Grounded designs (deterministic calculation + narration; tool-using
  agent) make fewer numerical errors and fewer unsupported claims than direct
  prompting.
- **H2:** Supplying explicit data-quality check results improves defect
  detection and appropriate abstention, independent of calculation grounding
  (tested by the 2×2 ablation, Section 4).
- **H3:** Grounding does not materially reduce expert-rated usefulness
  (non-inferiority margin `[DECIDE]`, proposed 0.5 points on a 5-point scale).

These are hypotheses to test. Results are reported whatever their direction.

## 3. Setting and task

A **scenario** is one routine reporting package for one programme, one
geographic unit (for example a district with its facilities) and a reporting
window (for example 12 months). The system must write a short programme
briefing (≤ 300 words) for a district health manager covering: main indicator
values and trends, data-quality problems that affect interpretation, and what
cannot be concluded from the data.

**Programme domains** `[DECIDE]`: start with malaria (case counts, testing
rate, test positivity, treatment) and immunisation or maternal health as a
second domain, so results are not specific to one programme.

## 4. System designs (arms)

All arms receive the same scenario data and the same task instruction, and
must return the same output format (Section 6).

| Arm | Calculation grounding | Data-quality check results supplied | Description |
|---|---|---|---|
| A1 Direct | No | No | Report tables + task instruction only |
| A1+DQ | No | Yes | As A1, plus the output of the data-quality checks |
| A2 Retrieval | Partial (source rows + indicator definitions) | No | Selected source rows and indicator definitions retrieved and added |
| A3−DQ | Yes | No | Tested indicator calculations supplied; model constrained to narrate them |
| A3 Deterministic + narration | Yes | Yes | Calculations + data-quality check results; model constrained to narrate them |
| A4 Tool-using agent | Yes, model-selected | Available as a tool | Fixed tool set (calculations, data-quality checks, row retrieval); validated tool schemas |

**The 2×2 ablation** is A1, A1+DQ, A3−DQ, A3 (grounding × DQ checks). It
separates the effect of grounding (H1) from the effect of data-quality checks
(H2). A2 and A4 are additional comparators.

**Shared components.** A3, A3−DQ, A1+DQ and A4 use the **same** calculation and
data-quality library (`src/` — to be built), which also produces the reference
answers. To avoid the reference simply equalling one arm's input, the
reference calculations are re-implemented independently (second
implementation or spreadsheet check, Section 8).

**Models** `[DECIDE]`: at least two model families, including one
open-weights model run locally (for reproducibility if APIs are retired).
Record exact model identifiers, API version, date, decoding settings.
Temperature `[DECIDE]` (proposed: 0 or lowest supported; same for all arms).
Retries: max 1 for malformed output, logged.

**Safety:** no arm has write access to any live DHIS2 instance. Only
aggregate, de-identified or synthetic data are sent to model APIs.

## 5. Scenarios

### 5.1 Data sources
1. **Synthetic generator** (primary, public release): DHIS2-shaped data —
   org-unit hierarchy, monthly periods, data elements with category options,
   reporting completeness and timeliness records.
2. **Real aggregate data** (validation subset, if approved) `[DECIDE]`:
   de-identified district/facility-month aggregates from a programme the team
   has permission to use, or the public DHIS2 demo database (check licence).
   Results on real data reported separately.

### 5.2 Defect taxonomy (aligned with WHO Data Quality Review domains)

| Code | Defect | WHO DQR domain | Injection rule (summary) |
|---|---|---|---|
| D0 | None (clean) | — | No change |
| D1 | Missing facility reports | 1 Completeness | Remove k facility-months |
| D2 | Late reports | 1 Timeliness | Mark reports as submitted after deadline |
| D3 | Duplicate records | 2 Internal consistency | Duplicate rows for a facility-month |
| D4 | Implausible / extreme / negative values | 2 Outliers | Replace value with negative, >x SD, or impossible value |
| D5 | Inconsistent totals or related indicators | 2 Consistency between indicators | e.g. positives > tested, sum of disaggregations ≠ total |
| D6 | Small denominator, unstable rate | 2 (analytic) | Denominator below threshold `[DECIDE]` |
| D7 | Insufficient evidence for trend/recommendation | — (abstention) | Too few periods, or change within noise |
| D8 | Denominator incompleteness (population/target issue) | 4 External consistency of population | Missing or implausible target population |

Each scenario carries: defect code(s), affected rows, an **abstention label**
(`must_abstain` / `may_answer` / `must_answer` for each required statement),
and a list of required findings.

Injection must change only the targeted values; a test checks that all other
values are unchanged.

### 5.3 Development vs held-out
- Development set: used for prompt and tool design. Size `[PILOT]` (proposed 40).
- Held-out set: generated with a different random seed and, where possible,
  different org-unit structures; hashed and frozen before any held-out run.
  Nobody tunes prompts on it.
- Target held-out size `[PILOT]`: proposed 120 scenarios (≈ 12–13 per defect
  code, clean included), finalised after the pilot (optional review by a statistician).

## 6. Output format (all arms)

Each output contains:
1. **Narrative briefing** (free text, ≤ 300 words).
2. **Key figures block** (structured JSON): indicator name, period, org unit,
   value. Used for automatic numerical scoring.
3. **Data-quality flags** (structured JSON list): defect type, location.
4. **Evidence references** (optional; arms that have row IDs may cite them).

For blinded human rating, outputs are normalised: evidence IDs removed,
formatting standardised, length checked. Traceability (Section 7.2) is scored
separately, by a rater who is not doing the usefulness rating for that output.

## 7. Outcomes

### 7.1 Primary outcomes (co-primary)
1. **Numerical fidelity** — share of stated figures that are correct.
   Counts and denominators: exact match. Rates and percentages: absolute error
   ≤ `[DECIDE]` (proposed 0.1 percentage point or rounding-equivalent).
   Scored automatically from the key figures block **and** checked by humans
   on the narrative (a narrative figure that contradicts the JSON counts as an
   error).
2. **Unsupported-claim rate** — unsupported claims ÷ total factual claims,
   per output, adjudicated by blinded human reviewers against the evidence
   rubric (Section 9).

**Co-primary rule** `[DECIDE]`: proposed — H1 is supported
only if both primary outcomes favour grounding; each tested at α = 0.025
(Bonferroni over two).

### 7.2 Secondary outcomes
- Data-quality defect detection: sensitivity and precision per defect code.
- Appropriate abstention: correct abstention on `must_abstain` items;
  unnecessary abstention on `must_answer` items.
- Evidence traceability (arms that cite): share of factual claims linked to a
  correct row or calculation.
- Expert-rated usefulness, clarity, actionability (1–5 scales, anchored rubric).
- Serious errors: errors that would change a programme decision (rubric
  definition), reported as counts and listed individually.
- Runtime, tokens, cost per scenario.
- Stability: agreement across repeated runs; differences across model families.

Secondary outcomes are adjusted for multiple comparisons (Holm) or labelled
exploratory.

## 8. Reference standard

1. Reference values computed by tested code (unit tests for every indicator
   and every data-quality check).
2. Independent check: a second implementation (or a team member re-computing
   a random 20% in a spreadsheet) must match exactly.
3. Each reference answer lists source-row IDs and calculation steps.
4. Required findings and abstention labels written by the domain lead **before
   any model is run on the held-out set**, and reviewed by one external rater.
5. The reference is never written by editing an LLM output.

## 9. Claim extraction and human adjudication

**Claim splitting.** Each narrative is split into atomic factual claims
(one checkable statement each) using a written guide `[to write]`.
Option `[DECIDE]`: humans split all narratives, or an LLM proposes the split
and humans correct it; if LLM-assisted, report human-vs-LLM agreement on a
sample of ≥ 50 narratives.

**Claim labels:** supported / unsupported / contradicted / not checkable
(opinion or recommendation; recommendations then judged for whether evidence
justifies them).

**Raters.** At least two external raters with public-health or M&E experience
who did not build the systems or the scenarios. Each rates independently,
blinded to arm and model. Disagreements are resolved by discussion; if still
unresolved, by a third rater. Raters are not told the hypotheses beyond what
is needed. Report Cohen's / Fleiss' κ (or Gwet's AC1 where prevalence is
extreme) before adjudication.

Raters are contributors acknowledged in the paper (Section 15), not
co-authors.

**Domain lead role.** The author (who builds the systems) writes
scenarios and the rubric, trains raters on development-set examples, and does
**not** rate held-out outputs.

**Rating load** `[PILOT]`: all arms × primary model × run 1 for the human-rated
outcomes; secondary model family human-rated on a stratified random 25%
sample. Estimate minutes per narrative in the pilot and adjust.

**LLM judges** may be used only as a secondary, exploratory check, reported
against human labels; never as the primary outcome.

## 10. Runs

- Repeats: 3 runs per scenario × arm × model `[DECIDE]`. Human rating on run 1;
  automatic outcomes on all runs (stability).
- Order of execution randomised; all prompts, tool definitions and configs
  version-controlled and hashed before the held-out run.
- Log per call: model ID, prompt version, input hash, output, latency, tokens,
  cost, retries, errors.
- Malformed outputs are kept and scored as failures (not dropped).

## 11. Sample size

Set after the pilot (≈ 20 development scenarios through all arms). Use pilot
variance in the paired difference of unsupported-claim rate (A1 vs A3) to
compute the number of scenarios for 80–90% power at a minimal important
difference `[DECIDE]`, accounting for clustering. Do not choose the sample
size to reach significance.

## 12. Statistical analysis

- Unit of analysis: scenario (outputs from the same scenario are paired).
- Primary: mixed-effects models with arm as fixed effect and scenario as
  random effect — binomial (claims supported / total; figures correct /
  total) or equivalent. Model family as fixed effect or analysed separately
  `[DECIDE]`.
- 95% CIs by scenario-level cluster bootstrap as a check.
- 2×2 ablation: test the grounding × DQ interaction for H2.
- H3: non-inferiority on usefulness with the prespecified margin.
- All results by defect code; serious errors listed individually.
- Sensitivity analyses: excluding malformed outputs; real-data subset alone;
  each model family alone.
- Analysis code written and tested on development/simulated data before
  unblinding.

## 13. Data governance and ethics

- No patient-level data. Only aggregate data are sent to external APIs.
- Real data used only with written data-owner permission; ethics review or a
  formal exemption obtained before analysis `[DECIDE: which IRB]`.
- Public release: synthetic data, generator, code, prompts, outputs, rater
  labels (anonymised). Real data shared only if permitted.
- Outputs are research artefacts, not operational advice or alerts.

## 14. Deviations log

| Date | Section | Change | Reason | Before/after held-out run |
|---|---|---|---|---|
| | | | | |

## 15. Authorship and roles

**Single-author study.** Khalilur Rahman Ridoy Khan: conceptualisation,
methodology, software, data curation, scenario design, formal analysis,
writing (all CRediT roles).

Contributors who do not meet ICMJE authorship criteria are named in the
Acknowledgements with what they did:

| Contributor | Contribution (acknowledged, not authors) |
|---|---|
| External raters (2+) | Blinded claim adjudication and usefulness rating (paid or volunteer) |
| Statistical reviewer (optional) | Review of analysis plan and code |
| Data owner contact (if real data) | Data access permission |

Raters must not draft or revise the manuscript or make design decisions; if
they do, they qualify for authorship and must be offered it.

### 15.1 Fallback if no external raters are available

If no external raters can be recruited, the study is re-framed as a
**benchmark/methods study** and:
- Numerical fidelity, defect detection and abstention (all scored
  automatically against code-generated references) become the primary
  outcomes.
- Unsupported-claim rate is rated by the author using a blinding tool
  (outputs shuffled, arm and model hidden, normalised format), with a
  repeat rating of a random 20% after ≥ 2 weeks to report intra-rater
  agreement; two LLM judges from different families are reported as
  secondary, with their agreement against the author's labels.
- Usefulness ratings (H3) are dropped or labelled exploratory.
- The limitations section states that there was no independent human
  review, and the paper makes no claim about operational safety.

## 16. Timeline (indicative)

| Month | Work |
|---|---|
| 1 | Finish novelty search; recruit raters (acknowledged); settle data route and ethics |
| 2 | Freeze defect taxonomy, rubric, output format; build library + generator with tests |
| 3 | Implement arms; pilot on development set; rater training; sample size |
| 4 | Freeze and register protocol; generate and hash held-out set; run |
| 5 | Blinded rating and adjudication |
| 6 | Analysis, figures, manuscript draft |
| 7 | Co-author revisions, TRIPOD-LLM checklist, preprint, journal submission |
