# 02 — Thematic synthesis: new-customer onboarding discovery
**Source:** Stakeholder map and imagined-but-realistic quote sets (Liam, Aisha, Carlos, Tom, Reina)  
**BABOK v3 reference:** Business Analysis Body of Knowledge, 3rd edition  

---

## Theme 1 — Compliance time is misread as regulatory floor when much of it is operational slack

**What the data says:** 1.8 of 4.2 days are attributed to compliance. Stakeholder testimony suggests a meaningful share is queue wait and manual routing, not mandated review time.

**Representative quote:**
> "The 1.8 compliance days — I've been saying for a year that at least 0.6 of that is just queue wait, not actual review. Nobody wanted to pull the data."
> — *Liam, Head of Onboarding*

**Persona most affected:** Liam (process owner); Aisha (sets the boundary between what is mandated and what is discretionary).

**BABOK area:** *Requirements Analysis and Design Definition* — specifically, distinguishing business rules (regulatory constraints) from business processes (operational steps that happen to occur within a compliance context). Misclassifying process overhead as a regulatory constraint prevents teams from redesigning steps they actually own.

**Implication:** The first analytical task is to decompose the 1.8 days into (a) mandated minimum review window, (b) queue and routing latency, and (c) exception-handling rework. Only (b) and (c) are candidates for redesign without a compliance gate.

---

## Theme 2 — Trust in the process depends on written gates, not verbal assurances

**What the data says:** 18% drop-off and stakeholder resistance both point to a process that feels uncertain — to customers mid-journey and to internal reviewers asked to approve changes.

**Representative quote:**
> "Show me the regulatory floor, in writing, from counsel — not from the project team."
> — *Aisha, Compliance Lead*

**Persona most affected:** Aisha (veto authority); Carlos (risk accountability at SVP level).

**BABOK area:** *Stakeholder Engagement* — specifically, managing stakeholder concerns and building the conditions for elicitation to succeed. Aisha's posture is not obstruction; it is a legitimate requirement for governance artefacts before she can contribute. The engagement strategy must produce written outputs (compliance gate memo, regulatory floor sign-off) as preconditions for later elicitation, not as afterthoughts.

**Implication:** No automation scope should be agreed before a written compliance gate exists. The gate is itself a deliverable, not a meeting.

---

## Theme 3 — Absence of a shared measurement baseline means risk is felt but not quantified

**What the data says:** Drop-off rate (18%) is known at aggregate level. Per-step dwell time, per-step drop-off, and KYC false-negative rate are not currently surfaced to decision-makers.

**Representative quote:**
> "What's the false-negative rate on KYC decisions today? If you can't tell me that number before we start, we shouldn't be talking about automation yet."
> — *Carlos, SVP Risk & Internal Controls*

**Persona most affected:** Carlos (risk framing); Reina (holds the drop-off data but it is not connected to operational metrics).

**BABOK area:** *Business Analysis Planning and Monitoring* — specifically, defining the metrics and measurement approach before elicitation begins. Without a shared baseline, each stakeholder applies their own implicit risk threshold, making trade-off conversations impossible. Establishing the measurement framework is a prerequisite, not a phase-2 task.

**Implication:** Week 1 instrumentation (per-step timestamps, drop-off cohort, KYC false-negative rate) must be completed before any intervention is sized. Carlos's question is the acceptance criterion for that instrumentation workstream.

---

## Theme 4 — Past failed projects have created a prior that new projects will hand off without delivering

**What the data says:** Stakeholder language around ownership, control, and presence in decision rooms reflects institutional memory of projects that produced recommendations but not change.

**Representative quote:**
> "I've had three 'transformation' projects run through my team in the last two years. Both times, someone came in, mapped our flows, built a deck, and then handed it back to us to implement."
> — *Liam, Head of Onboarding*

**Persona most affected:** Liam (directly burned); Tom (budget-holder who has seen spend without outcome).

**BABOK area:** *Elicitation and Collaboration* — specifically, establishing a collaborative environment and managing the impact of historical context on current engagement. Resistance rooted in prior experience is not irrational; it is data. The elicitation approach must be structured to produce visible, tangible progress within the first 30 days to interrupt the prior pattern.

**Implication:** The discovery plan must include a week-3 findings readout — not a draft, a real output — that Liam co-presents. Early visible delivery is the intervention, not a communication strategy.

---

## Theme 5 — Customer drop-off data exists but is disconnected from the operational and compliance view

**What the data says:** Reina holds step-level drop-off data. Compliance owns review SLAs. Engineering owns system logs. No single view connects customer exit behaviour to the internal process steps that precede it.

**Representative quote (inferred from Reina's posture):**
> "We know where customers leave. We don't know which internal step they were waiting on when they decided to go."
> — *Reina, Customer-Experience Lead (reconstructed)*

**Persona most affected:** Reina (data owner without operational counterpart); Liam (process owner without customer signal); Carlos (risk owner who needs to understand whether drop-off correlates with high-risk applicant profiles).

**BABOK area:** *Solution Evaluation* — specifically, assessing the performance of the current solution against business goals. The 18% drop-off is a performance metric for the current onboarding solution. Connecting it to internal process latency data transforms it from a symptom into a diagnostic instrument. Without this linkage, interventions are selected by intuition rather than evidence.

**Implication:** A joined dataset — customer journey timestamp × internal system event log × drop-off indicator — is the analytical foundation for all subsequent prioritisation. Reina and Tom must be jointly commissioned to produce this in week 1.

---

## Cross-theme summary

| Theme | Primary persona | BABOK area | 30-day action |
|---|---|---|---|
| Compliance time = operational slack + mandated floor | Liam / Aisha | Requirements Analysis & Design Definition | Decompose 1.8-day attribution |
| Trust requires written gates | Aisha / Carlos | Stakeholder Engagement | Compliance gate memo signed by CCO |
| No shared measurement baseline | Carlos / Reina | BA Planning & Monitoring | Instrument per-step metrics; surface KYC FN rate |
| Prior project failure creates resistance | Liam / Tom | Elicitation & Collaboration | Week-3 co-presented findings readout |
| Drop-off data disconnected from operations | Reina / Tom | Solution Evaluation | Joined dataset: journey × event log × drop-off |

---

*Generated: thematic synthesis · pre-solution · feeds directly into solution options and business case*
