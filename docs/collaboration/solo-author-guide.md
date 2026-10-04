# Solo-Author Guide

You are the only author. That is allowed in Q1 journals: they judge the
study, not the number of authors. What the study still needs is
**independent human checking of the outputs**, because you build the systems
and write the scenarios. Helpers who do that work are **acknowledged, not
authors**, provided they do not write or revise the paper or make design
decisions (ICMJE rules).

## Two routes

| | Route A — solo author + acknowledged raters (recommended) | Route B — fully solo (fallback) |
|---|---|---|
| Who rates outputs | 2+ external raters, blinded | You, blinded by a tool, plus two LLM judges as a secondary check |
| Primary outcomes | Numerical fidelity + unsupported-claim rate | Numerical fidelity, defect detection, abstention (all scored automatically by code) |
| Usefulness (H3) | Tested | Dropped or exploratory |
| Paper type | Comparative evaluation | Benchmark / methods paper |
| Q1 chance | Realistic | Lower; reviewers will name the lack of independent review |

Protocol Section 15 and 15.1 describe both routes.

## Route A — who to ask and what to ask for

### Raters (2–3)
- **Who:** M&E officers, data officers or surveillance staff who work with
  routine reports (NMEP, DGHS, MORU, BRAC, GMGI colleagues, or public-health
  graduates).
- **Ask them to:** attend a 2-hour training, rate a pilot of about 20
  briefings, then rate briefings blind over 3–4 weeks (estimate 20–40 hours;
  confirmed in the pilot), and join one meeting to settle disagreements.
- **They must not:** help write scenarios, build systems, or write or edit
  the paper. (If they do, they become eligible for authorship.)
- **Offer:** named acknowledgement with their contribution, a certificate or
  letter of contribution, and payment if you can afford it (state any
  payment in the funding/acknowledgement section).

### Optional: a statistical reviewer
- Asks to check the analysis plan and code once before you freeze the protocol.
- Acknowledged, not an author, as long as they only review.

### Optional: data owner (strongly helps Q1)
- Written permission to use de-identified aggregate counts (district or
  facility × month, no patient data); help with an ethics exemption letter.
- Acknowledged as the data provider.

## Agree in writing (email is enough)
1. They will be acknowledged, not authors, and what the acknowledgement says.
2. Payment, if any.
3. They rate independently and do not see which system wrote each briefing.
4. They do not share the scenarios or outputs.
5. Their time commitment and dates.

## Message templates

### To a rater

> Subject: Request: paid/volunteer expert rater for a public-health reporting study
>
> Dear [Name],
>
> I am running a research study on whether AI systems write accurate and
> useful briefings from routine health reporting data (district/facility
> monthly reports). I need two or three people with M&E or surveillance
> experience to rate AI-written briefings independently.
>
> It involves a 2-hour training session, a short pilot, then rating
> briefings over 3–4 weeks (about 20–40 hours in total, flexible), and one
> meeting to resolve disagreements. You will not see which system wrote each
> briefing. Your contribution will be named in the paper's acknowledgements
> [and paid at ___ per hour / as a fixed fee of ___].
>
> Would you be interested? I can share the rating guide and a sample briefing.
>
> Thank you,
> Khalilur Rahman Ridoy Khan

### To a statistical reviewer (optional)

> Subject: Request for a review of a statistical analysis plan
>
> Dear Dr. [Name],
>
> I am preparing a preregistered single-author study comparing AI system
> designs for writing routine health-programme briefings. Before I register
> the protocol, I would be grateful if you could review the 2-page analysis
> plan (mixed-effects models clustered by scenario, two co-primary
> outcomes, a 2×2 ablation). It should take about two hours. You would be
> acknowledged in the paper.
>
> Kind regards,
> Khalilur Rahman Ridoy Khan

### To a data owner

> Subject: Request to use de-identified aggregate [programme] data for a research study
>
> Dear [Name],
>
> I am requesting permission to use de-identified, aggregate [programme] data
> (district/facility × month counts only — no patient-level data or
> identifiers) for a research study evaluating AI-generated programme
> briefings. The data will be used only for research, will be published only
> in summary form unless you permit more, and no AI system will have access to
> any live system. Your team can review how the data are described before
> publication, and your organisation will be acknowledged as the data
> provider.
>
> Could I share the protocol and a one-page data request?
>
> Regards,
> Khalilur Rahman Ridoy Khan

## Solo-author checklist for submission
- Describe your background accurately (e.g. "health information systems
  lead, national malaria surveillance reporting").
- Declare that you designed the systems and the scenarios, and explain how
  rating was kept independent.
- Register the protocol on OSF before the held-out run.
- Publish code, synthetic data, prompts and rating labels.
- Complete the TRIPOD-LLM checklist.
- Use acknowledgements for every helper, with their contribution.
