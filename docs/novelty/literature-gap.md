# Phase 0 — Novelty Search and Gap Statement (v0.1)

Status: **first pass, 2026-10-04.** Built from web search results and abstracts.
Full texts on arXiv and medRxiv could not be opened from the build environment,
so every row below must be checked against the full paper before the protocol
is frozen. Add rows as new papers are found; do not delete rows that turn out
to be irrelevant — mark them instead, so the search is auditable.

## Search log

| Date | Source | Query | Notes |
|---|---|---|---|
| 2026-10-04 | Web search | large language model DHIS2 health data analysis evaluation | No controlled DHIS2 evaluation found |
| 2026-10-04 | Web search | LLM factuality summarizing public health surveillance data tables hallucination | General medical summarisation work only |
| 2026-10-04 | Web search | LLM data quality anomaly detection routine health information system HMIS | Nothing on HMIS reporting defects |
| 2026-10-04 | Web search | LLM table-to-text numerical hallucination grounded computation vs direct prompting | General-domain only |
| 2026-10-04 | Web search | LLM abstention benchmark insufficient evidence healthcare | Clinical / RAG abstention, not routine reporting |
| 2026-10-04 | Web search | DHIS2 AI chatbot LLM natural language analytics | Tools and a symposium paper, no comparative evaluation |
| 2026-10-04 | Web search | LLM generate epidemiological surveillance report narrative from aggregate data | Found EpiNarrate (closest work) |

**Still to do before freezing the protocol:** structured searches in PubMed,
Scopus or Web of Science, IEEE Xplore and ACL Anthology, using a fixed search
string and date range, recorded in this log. Check the reference lists of the
closest papers (backward search) and papers citing them (forward search).

## Closest existing work

| # | Paper | Task | Data | Systems compared | Outcomes | What it does **not** cover |
|---|---|---|---|---|---|---|
| 1 | Datta et al., *EpiNarrate: Agentic Generation of Grounded Narratives from Epidemiological Scenario Projections*, arXiv 2607.15544 (Jul 2026) | Narratives from scenario-modelling outputs | COVID-19 Scenario Modeling Hub projections | Agentic framework separating numerical reasoning from generation vs. baselines | Factual grounding, coverage of salient patterns, style | Model projections, not routine facility reporting; no injected data-quality defects; no abstention outcome (verify); routine-programme M&E setting absent. **Closest paper — must be cited and contrasted.** |
| 2 | *Evaluating LLMs for natural-language-to-code generation on aggregate Czech public health data analysis*, medRxiv 10.64898/2025.12.05.25341697 (Dec 2025) | NL query → Python code over aggregate incidence/prevalence data | Czech NZIP datasets | 11 LLMs | Code executability, result correctness (none consistently >70%) | Code generation, not narrative briefings; no data-quality defects, unsupported claims or abstention |
| 3 | *Integrating NLP and LLMs into DHIS2 to improve health data utilization*, ICSE 2025 SEiGS symposium | NL querying of DHIS2 (LangChain + Ollama) | DHIS2 instance | Single system | Analysis time, accessibility (reported) | System description; no controlled comparison, no held-out reference answers, no expert blinded rating |
| 4 | `jmesplana/dhis2_ai_insights` (GitHub) | NL analytics app for DHIS2 | Live DHIS2 | Single tool | None (software) | Software only — shows practitioner demand, not evidence |
| 5 | Zhang & Wu, *Do LLMs Know When Evidence is Insufficient? An Evidence Sufficiency Benchmark…* (2026, CMC) | Answer vs. abstain under graded evidence | General RAG | Multiple LLMs, prompting strategies | Over-answer rate by evidence level (65–91% under conflicting evidence) | General domain; gives an abstention design to borrow (graded evidence levels) |
| 6 | *Judge-dependent safety gains and model-specific helpfulness costs of evidence-sufficiency prompting in clinical LLMs*, arXiv 2607.18086 (Jul 2026) | Clinical answers under incomplete evidence | Clinical vignettes | Evidence-sufficiency prompt vs. baseline, several LLMs, two LLM judges + blinded clinicians | Unsafe overconfidence, correct diagnosis | Clinical, not reporting. **Methodological lesson:** LLM-judge effect sizes changed by judge; safety gains carried model-specific helpfulness costs → our design must anchor on human raters and measure usefulness jointly (H3). |
| 7 | AbstentionBench, arXiv 2506.09038 | Abstention across 20 datasets | General | 20 LLMs | Abstention recall | General domain; scaling does not fix abstention — supports H2 relevance |
| 8 | FACTS Grounding leaderboard, arXiv 2501.03200 | Grounded long-form answers | General documents | Many LLMs | Grounding/factuality via LLM judges | General domain; no numeric derivation or data-quality defects |
| 9 | Synthetic table-to-text benchmarking (VLDB TaDA workshop 2025) | Table-to-text factuality | Synthetic tables | LLMs | Factuality, numeric/temporal accuracy, coherence | General domain; notes numbers embedded in narrative resist automatic extraction → supports human claim adjudication |
| 10 | TRIPOD-LLM (Gallifant et al., *Nature Medicine* 2025) | Reporting guideline | — | — | — | Not a competitor: **the reporting guideline to follow** |
| 11 | WHO Data Quality Review (DQR) toolkit / DHIS2 WHO Data Quality Tool | Data-quality metrics for routine data | — | — | Completeness, timeliness, outliers, internal/external consistency | Not a competitor: **source for our defect taxonomy and reference calculations** |
| 12 | Own prior work: Khan, public-health reporting agent evaluation in `bojieli/ai-agent-book` (merged PR 72, Jul 2026) | Demo benchmark, synthetic DHIS2-style data | Synthetic | Reference agent | Tool selection, calculations, evidence, DQ detection | Demonstration only, one system, no held-out set, no expert review. Cite as prior work; do not present as the study. |

## Draft gap statement (one page, for the introduction)

LLMs are already being connected to routine health information systems such
as DHIS2 to answer questions and draft summaries for programme managers
(rows 3–4). Published evaluations of LLMs on aggregate public-health data have
focused on code generation (row 2) or on narratives from modelled projections
(row 1). Separately, abstention studies show that LLMs answer when evidence is
insufficient or conflicting (rows 5–7), and that safety gains measured by LLM
judges can be unreliable without human review (row 6).

What is missing, to our current knowledge, is a controlled, preregistered
comparison of **system designs** — direct prompting, retrieval grounding,
deterministic calculation with constrained narration, and tool-using agents —
on **routine facility reporting data containing realistic data-quality defects**
(incomplete reporting, late reports, duplicates, implausible values,
inconsistent totals, unstable small-denominator rates), measuring together:
(a) numerical fidelity, (b) unsupported claims judged by blinded public-health
reviewers, (c) detection of data-quality defects using WHO DQR-aligned
definitions, (d) appropriate abstention, and (e) whether grounding costs
usefulness.

This study fills that gap with a held-out benchmark whose reference answers
are produced by independently tested code, an ablation that separates the
effect of grounding from the effect of data-quality checks, and blinded expert
adjudication.

**Stop/revise rule (from the plan):** if the full-text check or structured
search finds a study that already compares grounded vs. direct LLM designs on
routine HMIS data with injected defects and abstention, narrow the
contribution (for example to defect-specific failure modes or LMIC malaria
programme data) before continuing.

## Sources

- https://arxiv.org/abs/2607.15544
- https://www.medrxiv.org/content/10.64898/2025.12.05.25341697v1
- https://conf.researchr.org/details/icse-2025/icse-2025-symposium-on-software-engineering-in-the-global-south/2/INTEGRATING-NATURAL-LANGUAGE-PROCESSING-AND-LARGE-LANGUAGE-MODELS-INTO-DHIS2-TO-IMPRO
- https://github.com/jmesplana/dhis2_ai_insights
- https://www.sciencedirect.com/org/science/article/pii/S1546221826007526
- https://arxiv.org/abs/2607.18086
- https://arxiv.org/pdf/2506.09038
- https://arxiv.org/pdf/2501.03200
- https://www.vldb.org/2025/Workshops/VLDB-Workshops-2025/TaDA/TaDA25_13.pdf
- https://www.nature.com/articles/s41591-024-03425-5
- https://docs.dhis2.org/en/full/use/optional-apps/who-data-quality-tool-manual.html
- https://github.com/bojieli/ai-agent-book/pull/72
