# Co-author Guide — Who to Ask, What to Ask For, What to Send

## The team you need

| Role | How many | Who fits | Why the paper needs them |
|---|---|---|---|
| **External raters** | 2 (3 is safer) | M&E officers, data officers, district/programme surveillance staff, public-health graduates working with routine data (e.g. NMEP, DGHS, MORU, BRAC, GMGI colleagues) | Blinded, independent judgment of claims and usefulness. **Required** — you built the systems, so you cannot rate. |
| **Statistician / methods co-author** | 1 | Biostatistician or epidemiologist with peer-reviewed papers (university faculty, icddr,b, research institute) | Sample size, mixed models, co-primary rule. Reviewers trust a named statistician. |
| **Senior co-author** | 1 | Health-informatics or public-health researcher with Q1/Q2 publications | Supervision, writing quality, credibility, journal choice. Can be the same person as the statistician. |
| **Data owner contact** | 0–1 | Programme manager or data lead who can approve de-identified aggregate data | Only if you use real data (strongly recommended for Q1). |

Ask in this order: **senior co-author/statistician first** (they shape the
protocol), then raters, then the data owner.

## What to ask each person for

### Senior co-author / statistician
- Review and improve `protocol/protocol-draft.md`, especially the items marked `[DECIDE]`.
- Decide the co-primary outcome rule, non-inferiority margin and sample size after the pilot.
- Check or write the analysis model; review the results section.
- Review the full manuscript; advise on journal choice.
- Time: about 15–25 hours over 6 months, mostly in months 1, 3 and 6.
- Offer: co-authorship (usually last or second-to-last author for the senior), CRediT roles listed.

### External raters
- 2-hour training session on the rubric using development examples.
- Rate a pilot set (about 20 narratives) to test the rubric.
- Rate held-out narratives independently, without discussing with each other, and without knowing which system produced each one.
- Join one adjudication meeting to resolve disagreements.
- Do **not** help write scenarios or build systems (that would break independence).
- Time: depends on the pilot; estimate 20–40 hours each, spread over 3–4 weeks.
- Offer: co-authorship (meets ICMJE criteria if they also review and approve the manuscript), CRediT roles "Investigation, Validation, Writing — review & editing".

### Data owner (if real data)
- Written permission to use de-identified aggregate data (district or facility × month counts only, no patient data).
- Confirm what can be published or shared.
- Help get an ethics exemption letter if needed.
- Review the manuscript description of their data.
- Offer: co-authorship if they contribute to the work and manuscript, otherwise acknowledgement — agree this in writing at the start.

## Agree these points in writing before starting

1. **Author order** (proposed: you first; raters and data contact in the middle; senior author last).
2. **CRediT roles** for each person (see protocol Section 15).
3. **ICMJE criteria:** every author contributes, reviews the draft, approves the final version, and is accountable.
4. **Independence rule:** raters do not see which system produced an output and do not help build scenarios.
5. **Data rules:** what data may be used, stored, shared and published.
6. **Timeline** and what happens if someone cannot continue (they move to acknowledgements).
7. **Conflicts of interest** (any work for LLM vendors, DHIS2 vendors, etc.) — each person declares.
8. **Preprint and open-code policy:** protocol registered on OSF, preprint on medRxiv, code and synthetic data public.

## Message templates

Edit names and details; keep them short.

### To a senior co-author / statistician

> Subject: Invitation to co-author a study on LLMs for routine health reporting
>
> Dear Dr. [Name],
>
> I lead IT and MIS work on public-health surveillance in Bangladesh (malaria
> surveillance and NMEP/Global Fund reporting systems, ODK/KoBo field data,
> DHIS2-style aggregate reporting). I am preparing a preregistered study that
> tests whether grounding language models in tested indicator calculations
> and data-quality checks reduces numerical errors and unsupported claims in
> routine health-programme briefings, compared with prompting a model
> directly. The design compares six system variants on a held-out benchmark
> with injected data-quality defects (WHO DQR-aligned), blinded expert
> rating, and analysis at the scenario level.
>
> I would value your input as [senior co-author / methods co-author],
> mainly on the analysis plan, sample size and manuscript review — roughly
> 15–25 hours over six months. We are targeting a health-informatics journal
> such as JBI, IJMI or AIIM, following TRIPOD-LLM.
>
> The draft protocol (6 pages) is attached. Could we have a 30-minute call in
> the next two weeks?
>
> Kind regards,
> Khalilur Rahman Ridoy Khan
> ORCID 0009-0000-6742-6207

### To a rater

> Subject: Request: expert rater (co-author) for a public-health reporting study
>
> Dear [Name],
>
> I am running a research study on whether AI systems write accurate and
> useful briefings from routine health reporting data (district/facility
> monthly reports). I need two or three people with M&E or surveillance
> experience to rate the AI-written briefings independently.
>
> What it involves: a 2-hour training session, a short pilot, then rating
> briefings over 3–4 weeks (estimated 20–40 hours total, flexible), and one
> meeting to resolve disagreements. You would not see which system wrote
> each briefing. You would be a co-author on the paper, credited for
> investigation and validation.
>
> Would you be interested? I can share the rating guide and a sample briefing.
>
> Thank you,
> Ridoy

### To a data owner

> Subject: Request to use de-identified aggregate [programme] data for a research study
>
> Dear [Name],
>
> I am requesting permission to use de-identified, aggregate [programme] data
> (district/facility × month counts only — no patient-level data or
> identifiers) for a research study evaluating AI-generated programme
> briefings. The data would be used only for research, would not be sent
> anywhere at patient level, and would be published only in summary form,
> unless you permit more. No AI system would have access to any live
> system. Your team would review how the data are described before
> publication, and we would discuss authorship or acknowledgement as you
> prefer.
>
> Could I share the protocol and a one-page data request for your review?
>
> Regards,
> Ridoy

## What to bring to the first call
- `protocol/protocol-draft.md` (export to PDF).
- `docs/novelty/literature-gap.md` gap statement.
- One sample scenario and one sample model output (from the earlier `ai-agent-book` benchmark) so they see the task.
- A list of the `[DECIDE]` items you want them to own.
