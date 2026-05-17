# 03 — User stories: new-customer onboarding
**Format:** Capability-level (not screen-level). No UI prescription.  
**Acceptance criteria:** Given/When/Then. Happy path, negative path, accessibility, performance (sourced only).  
**Invented figures flagged:** [NEEDS SOURCE]  
**Source:** Stakeholder quotes from discovery sessions (Liam, Aisha, Carlos, Tom, Reina) and baseline metrics (4.2-day median, 18% drop-off, 1.8 compliance days, 5 systems).

---

## US-01 — Compliance time decomposition
**Theme 1:** Compliance time is misread as regulatory floor when much of it is operational slack

### Story
**As** a compliance operations analyst responsible for routing KYC and AML review tasks across five systems,  
**I want** to see each onboarding case's time split into mandated review duration, queue wait time, and exception-handling rework — separately and at the individual case level,  
**so that** the business can distinguish regulatory constraints (which cannot be redesigned) from operational overhead (which can), and target improvement effort correctly.

### Business value
The current 1.8-day compliance attribution is undifferentiated. If even 0.4 days of that is queue latency (conservative estimate based on Liam's testimony), eliminating it reduces total cycle time by ~10% without touching any mandated control. At 18% drop-off, any reduction in cycle time that keeps applicants in-funnel has direct revenue impact [NEEDS SOURCE: conversion value per completed application].

### Source quote
> "The 1.8 compliance days — I've been saying for a year that at least 0.6 of that is just queue wait, not actual review. Nobody wanted to pull the data."  
> — *Liam, Head of Onboarding*

### Acceptance criteria

**AC-01 · Happy path — time split is available at case level**  
Given a completed or in-progress onboarding case with events logged across all five systems,  
When a compliance operations analyst queries the case timeline,  
Then the system returns three distinct duration values: (a) time spent in active review by a human or automated check, (b) time in queue awaiting assignment, and (c) time in exception or rework state — with each value traceable to a system event log entry.

**AC-02 · Happy path — aggregate view across cohorts**  
Given a date range and optional filter by product type or applicant segment,  
When the analyst generates a compliance time summary report,  
Then the report displays the median and P90 values for each of the three time categories across all cases in the cohort, with no category aggregated into another.

**AC-03 · Happy path — regulatory floor is flagged separately**  
Given a case where a mandated minimum review window applies (e.g., a defined AML hold period),  
When the time split is displayed,  
Then the mandated minimum duration is shown as a distinct sub-component of active review time, clearly labelled as non-discretionary, so that analysts do not conflate process overhead with regulatory requirement.

**AC-04 · Negative path — incomplete event log**  
Given a case where one or more system event logs are missing or have a gap greater than [NEEDS SOURCE: agreed threshold] between events,  
When the time split is requested,  
Then the system flags the case as having incomplete data, displays the available partial breakdown, and does not impute or estimate the missing duration — instead surfacing the data gap for remediation.

**AC-05 · Negative path — source system unavailable**  
Given that one of the five source systems is offline or unreachable at query time,  
When the analyst requests a compliance time report,  
Then the system returns a clear error identifying which source system is unavailable, presents any data it can retrieve from the remaining systems, and does not silently substitute zeroes or omit affected cases from the cohort without notification.

**AC-06 · Accessibility — WCAG 2.2 AA**  
Given a compliance analyst who relies on a screen reader or keyboard-only navigation,  
When they access the case-level time split view,  
Then all data values, labels, and status indicators are programmatically determinable (WCAG 2.2 SC 1.3.1 Info and Relationships), all interactive controls are operable via keyboard alone (SC 2.1.1), and no information is conveyed by colour alone (SC 1.4.1).

**AC-07 · Performance — [NO SOURCED TARGET — criterion omitted]**  
> No performance SLA for compliance reporting has been stated by any stakeholder. To be confirmed with Carlos or Aisha in week-2 briefing before this criterion is written. Placeholder: [NEEDS SOURCE].

---

## US-02 — Compliance gate as a deliverable
**Theme 2:** Trust in the process depends on written gates, not verbal assurances

### Story
**As** a compliance lead accountable for ECOA and Reg B obligations on all onboarding decisioning changes,  
**I want** a formally documented compliance gate — signed by counsel and the CCO — that defines the regulatory floor, the fair-lending review triggers, and the conditions under which any proposed process change must return for re-evaluation,  
**so that** I can contribute meaningfully to the redesign without carrying unquantified personal and institutional regulatory risk.

### Business value
Without a written gate, Aisha's only safe posture is to block all automation proposals. A signed gate memo converts her from a veto point to an active collaborator, unlocking the compliance workstream. It also provides the audit trail Carlos requires for the spring regulatory exam. The cost of producing the memo is one legal review cycle; the cost of not producing it is the entire project timeline.

### Source quotes
> "Show me the regulatory floor, in writing, from counsel — not from the project team."  
> — *Aisha, Compliance Lead*

> "I don't want him hearing about risks I haven't already signed off on."  
> — *Liam, Head of Onboarding (on briefing Carlos)*

### Acceptance criteria

**AC-01 · Happy path — gate memo is complete and signed**  
Given that legal counsel and the CCO have reviewed the proposed onboarding process changes,  
When the compliance gate memo is finalised,  
Then the document contains: (a) the minimum mandated review window for KYC and AML per applicable regulation, (b) the fair-lending and ECOA triggers that require a full model-risk review, (c) the conditions under which a previously approved change must return for re-evaluation, and (d) dated signatures from legal counsel and the CCO — and this document is stored in the project governance record before any automation is scoped.

**AC-02 · Happy path — gate is referenced in all subsequent scope decisions**  
Given the signed compliance gate memo exists,  
When any project team member proposes a change to the onboarding decisioning logic,  
Then a documented check against the gate criteria is recorded before the change enters solution design — and any change that triggers a fair-lending review criterion is escalated to Aisha before proceeding.

**AC-03 · Happy path — gate is version-controlled**  
Given the compliance landscape changes (e.g., a regulatory update or an internal policy revision),  
When the gate memo is updated,  
Then the prior version is retained in the governance record with a change log entry, so that the project team can demonstrate which gate criteria applied at each decision point during the spring regulatory exam.

**AC-04 · Negative path — scope change attempted before gate is signed**  
Given the compliance gate memo has not yet been signed,  
When a team member attempts to record a scoping decision that touches compliance-adjacent process steps,  
Then the governance workflow flags the decision as blocked, surfaces the unsigned gate as the dependency, and does not allow the scoping record to be marked complete until the gate is in place.

**AC-05 · Negative path — counsel review is inconclusive**  
Given legal counsel identifies a regulatory question that cannot be resolved within the 30-day discovery window,  
When the gate memo cannot be fully executed,  
Then the project scope is automatically constrained to process efficiency changes only (queue management, system consolidation, staffing), no automation of compliance steps is initiated, and this constraint is documented in the project record with the open legal question and its expected resolution date.

**AC-06 · Accessibility — WCAG 2.2 AA**  
Given a project team member who uses assistive technology accesses the governance workflow to check gate status,  
When they navigate the gate status indicator and any associated alerts,  
Then all status states (signed, pending, blocked, inconclusive) are conveyed via text or programmatic label — not colour alone (WCAG 2.2 SC 1.4.1) — and all form controls in the workflow are operable by keyboard (SC 2.1.1).

---

## US-03 — Shared measurement baseline
**Theme 3:** Absence of a shared measurement baseline means risk is felt but not quantified

### Story
**As** an SVP of Risk and Internal Controls preparing to authorise scope for an onboarding redesign,  
**I want** a pre-change baseline report that shows current KYC false-negative rate, per-step drop-off rate, per-step median dwell time, and SAR filing rate by applicant cohort — before any process change is designed,  
**so that** I can set a quantified risk appetite, evaluate any proposed change against it, and demonstrate to examiners that controls were measured before they were modified.

### Business value
Carlos's approval gates the entire project. His stated requirement — a false-negative rate baseline before automation is discussed — is a precondition, not a preference. Producing this report also converts Reina's drop-off data and Tom's system logs from siloed assets into a joined analytical foundation. Without it, every stakeholder applies a different implicit risk threshold and trade-off conversations are impossible.

### Source quote
> "What's the false-negative rate on KYC decisions today? If you can't tell me that number before we start, we shouldn't be talking about automation yet."  
> — *Carlos, SVP Risk & Internal Controls*

### Acceptance criteria

**AC-01 · Happy path — baseline report is complete**  
Given event log data has been successfully extracted from all five onboarding systems for a rolling 12-month period,  
When the baseline report is generated,  
Then it contains, for the full cohort and for each available segment: (a) KYC false-negative rate (cases cleared that subsequently triggered a SAR or fraud event within [NEEDS SOURCE: agreed lookback window]), (b) median and P90 dwell time per onboarding step, (c) drop-off rate per step, and (d) SAR filing rate by applicant cohort — with each metric traceable to its source system and extraction date.

**AC-02 · Happy path — risk appetite is formally recorded**  
Given the baseline report has been reviewed by Carlos,  
When he sets the risk appetite thresholds,  
Then his thresholds for acceptable false-negative rate, maximum acceptable drop-off rate change, and SAR filing rate tolerance are recorded in writing in the project governance record — and any proposed intervention is evaluated against these thresholds before design begins.

**AC-03 · Happy path — baseline is used as change measurement benchmark**  
Given the baseline report exists and risk appetite thresholds are recorded,  
When any post-implementation measurement is conducted,  
Then the report compares post-change values against the pre-change baseline using the same cohort definition, segmentation, and metric definitions — so that change impact is unambiguous and auditable.

**AC-04 · Negative path — KYC false-negative rate cannot be computed**  
Given that the data linkage between cleared KYC decisions and subsequent SAR or fraud events is not available within the 30-day window,  
When the baseline report is requested,  
Then the report clearly states that KYC false-negative rate is unavailable, documents the data gap and its owner, does not substitute a proxy metric without explicit sign-off from Carlos, and project scope is constrained to non-automated changes until the gap is resolved.

**AC-05 · Negative path — cohort data is incomplete for a segment**  
Given that drop-off data for a specific applicant segment (e.g., small-business applicants) is missing from Reina's dataset,  
When the baseline report is generated,  
Then the missing segment is flagged explicitly, the report is not averaged across segments to mask the gap, and the segment is excluded from intervention prioritisation until its data is available.

**AC-06 · Accessibility — WCAG 2.2 AA**  
Given a risk analyst who uses a screen reader accesses the baseline report,  
When they navigate data tables and metric summaries,  
Then all tables have programmatic row and column headers (WCAG 2.2 SC 1.3.1), all charts have text alternatives conveying the same data values, and the report can be fully navigated and comprehended without reference to visual formatting or colour.

**AC-07 · Performance — [NO SOURCED TARGET — criterion omitted]**  
> No query response SLA for the baseline report has been stated. To be confirmed with Tom (extraction cost) and Carlos (acceptable reporting lag) in week-2 briefing. Placeholder: [NEEDS SOURCE].

---

## US-04 — Early visible delivery
**Theme 4:** Past failed projects have created a prior that new projects will hand off without delivering

### Story
**As** a head of onboarding who has seen two prior transformation projects produce recommendations but no implemented change,  
**I want** to co-present a tangible findings output — containing a validated step-level time breakdown, a ranked list of intervention candidates, and a draft decision record — by the end of week three,  
**so that** my team sees credible early progress, I retain visible ownership of the redesign, and the project does not repeat the pattern of deferred delivery.

### Business value
Liam's co-presentation is not a communication preference — it is the intervention. If the project produces only internal analysis in its first 30 days, it will be perceived as another deck-and-handoff cycle, eroding the trust needed for Liam's team to participate in implementation. Early tangible output is the trust instrument. It also forces real analytical progress rather than indefinite discovery.

### Source quote
> "Both times, someone came in, mapped our flows, built a deck, and then handed it back to us to implement. I need to know this time is different before I open up my team's calendars."  
> — *Liam, Head of Onboarding*

### Acceptance criteria

**AC-01 · Happy path — week-3 findings readout is delivered on schedule**  
Given the discovery workstreams (system log extraction, drop-off cohort analysis, compliance interviews) have been completed by end of week two,  
When the week-3 findings readout is held,  
Then it contains: (a) a validated per-step time breakdown sourced from system event logs — not estimates, (b) a ranked shortlist of at least three intervention candidates with rationale, and (c) a draft decision record identifying which interventions require a compliance gate before progressing — and Liam is listed as a co-presenter on the session record.

**AC-02 · Happy path — Liam reviews and approves content before presentation**  
Given the findings document is drafted by end of week two,  
When Liam reviews it,  
Then he has at least 48 hours to review, amend, and approve the content before it is presented to the wider stakeholder group — and any section he disputes is either revised or clearly flagged as contested in the presentation materials.

**AC-03 · Happy path — decision record is actionable**  
Given the findings readout has been delivered,  
When the decision record is finalised,  
Then each intervention candidate has an assigned owner, a dependency list (including any compliance gate or data gap), and a binary proceed/park status agreed by Liam, Aisha, and Carlos — so that week four has an unambiguous workplan.

**AC-04 · Negative path — a discovery workstream is not complete by end of week two**  
Given one workstream (e.g., Tom's system log extraction) is delayed,  
When the week-3 findings readout date arrives,  
Then the readout proceeds with available data, the gap is explicitly named and owned in the session, affected intervention candidates are marked data-pending rather than removed, and a recovery date for the missing data is agreed in the session — the readout is not postponed in its entirety.

**AC-05 · Negative path — Liam is unavailable to co-present**  
Given Liam is unable to attend the week-3 session due to a scheduling conflict,  
When the session is held,  
Then a named delegate from Liam's team presents on his behalf, Liam's prior review and approval of the content is documented in the session record, and the session is not rescheduled beyond week four without Liam's written agreement.

**AC-06 · Accessibility — WCAG 2.2 AA**  
Given a meeting participant who relies on assistive technology joins the week-3 findings readout in digital or hybrid format,  
When presentation materials are shared,  
Then all shared documents meet WCAG 2.2 AA: all images of data have text alternatives, all slide content is readable by a screen reader in logical reading order (SC 1.3.2), and real-time captioning or a session transcript is available on request.

---

## US-05 — Joined customer-journey and operational dataset
**Theme 5:** Customer drop-off data exists but is disconnected from the operational and compliance view

### Story
**As** a customer-experience lead who holds step-level drop-off data but cannot currently connect it to the internal process steps customers were waiting on when they exited,  
**I want** a joined dataset that links each customer's exit event to the specific internal system state and dwell time that preceded it,  
**so that** the business can identify which internal process failures drive customer abandonment and prioritise interventions by actual customer impact rather than internal process intuition.

### Business value
The current 18% drop-off is measured but undiagnosed. Reina has the exit signal; Tom has the operational log. Unjoined, they support only intuition-based prioritisation. Joined, they make each intervention's business case quantifiable: a 1% reduction in drop-off at [NEEDS SOURCE: conversion value per completed application] produces a calculable revenue figure Tom can use to justify budget and Carlos can use to frame risk/return.

### Source quote (reconstructed — requires validation in week-1 interview with Reina)
> "We know where customers leave. We don't know which internal step they were waiting on when they decided to go."  
> — *Reina, Customer-Experience Lead (reconstructed)*

### Acceptance criteria

**AC-01 · Happy path — join is successful and complete**  
Given customer journey event data (from Reina's system) and internal onboarding system event logs (from Tom's five systems) share a common applicant identifier and overlapping timestamp range,  
When the joined dataset is produced,  
Then each customer exit event is matched to the internal system state at the time of exit — including the step the application was in, the dwell time elapsed at that step, and the system holding the application — with a match rate of at least [NEEDS SOURCE: agreed minimum match rate, e.g., 90%] of exit events.

**AC-02 · Happy path — drop-off is attributable to a step, not just a date**  
Given the joined dataset is available,  
When drop-off rate is calculated,  
Then it is expressed per onboarding step (not per calendar day), with a ranked output showing which steps account for the largest share of total drop-off volume — enabling direct prioritisation without further analytical interpretation.

**AC-03 · Happy path — dataset is reproducible with updated data**  
Given the join logic and data extraction scripts are documented,  
When a new cohort period is specified (e.g., the following quarter),  
Then the joined dataset can be reproduced by Tom's or Reina's team without re-engagement of the project team — confirming the output is an operational asset, not a one-time deliverable.

**AC-04 · Negative path — applicant identifier is inconsistent across systems**  
Given that one or more of the five systems uses a different applicant identifier format or has gaps in identifier population,  
When the join is attempted,  
Then the dataset documents the unmatched volume explicitly, does not silently exclude unmatched records from drop-off calculations, surfaces the identifier inconsistency as a remediation item for Tom's team, and marks affected steps' drop-off figures as partial until the gap is resolved.

**AC-05 · Negative path — timestamp precision is insufficient to determine step ordering**  
Given that event timestamps in one or more systems are recorded at day-level rather than time-level precision,  
When step ordering within a single day is required,  
Then the dataset flags steps where ordering cannot be determined with certainty, does not impute an assumed order, and excludes those cases from step-level dwell time calculations while retaining them in overall drop-off counts — so that partial data does not corrupt the ranking.

**AC-06 · Accessibility — WCAG 2.2 AA**  
Given an analyst with a visual impairment accesses the joined dataset via a reporting interface,  
When they navigate summary views and drill-down tables,  
Then all data tables include programmatic column and row headers (WCAG 2.2 SC 1.3.1), sort and filter controls are operable by keyboard alone (SC 2.1.1), and no data state (e.g., partial, unmatched, flagged) is conveyed by colour alone (SC 1.4.1).

**AC-07 · Performance — [NO SOURCED TARGET — criterion omitted]**  
> No refresh frequency or query latency target has been stated. To be confirmed with Tom (extraction cost) and Reina (reporting cadence need) in week-1 sessions. Placeholder: [NEEDS SOURCE].

---

## Traceability matrix

| Story | Theme | Persona | BABOK area | Key dependency | [NEEDS SOURCE] items |
|---|---|---|---|---|---|
| US-01 | Compliance time decomposition | Compliance operations analyst (KYC/AML routing) | Requirements Analysis & Design Definition | Event log access across all 5 systems | Performance SLA; conversion value per application; event gap threshold |
| US-02 | Compliance gate as deliverable | Compliance lead (ECOA / Reg B accountable) | Stakeholder Engagement | CCO and counsel availability in week 1 | None flagged |
| US-03 | Shared measurement baseline | SVP Risk & Internal Controls | BA Planning & Monitoring | KYC FN rate linkage; SAR data access; 12-month extract | Performance SLA; SAR lookback window |
| US-04 | Early visible delivery | Head of onboarding (post-failed-projects) | Elicitation & Collaboration | All discovery workstreams complete by week 2 | None flagged |
| US-05 | Joined journey + ops dataset | Customer-experience lead (drop-off data owner) | Solution Evaluation | Common applicant identifier; time-level timestamps | Match rate threshold; conversion value; refresh SLA |

---

## [NEEDS SOURCE] consolidated tracker

| ID | Item | Story | Who can resolve | When |
|---|---|---|---|---|
| NS-01 | Conversion value per completed application | US-01, US-05 | Liam / Finance | Week 1 |
| NS-02 | Event log gap threshold for incomplete-data flag | US-01 | Tom | Week 1 |
| NS-03 | Performance SLA for compliance time report | US-01, US-03 | Carlos / Aisha | Week 2 |
| NS-04 | SAR lookback window for KYC false-negative calculation | US-03 | Carlos / Aisha | Week 2 |
| NS-05 | Minimum acceptable join match rate (e.g., 90%) | US-05 | Reina / Tom | Week 1 |
| NS-06 | Dataset refresh frequency and query latency target | US-05 | Reina / Tom | Week 1 |

---

*Items marked [NEEDS SOURCE] are analytical assumptions that require stakeholder confirmation before acceptance criteria can be considered testable. All should be resolved in week-1 and week-2 sessions per the discovery plan (01_discovery_plan.md).*

*Generated: user story draft · pre-review · not yet accepted by product owner*
