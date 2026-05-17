# 05 — BRD-ready opportunities: new-customer onboarding
**Method:** Process mining path — one bottleneck, one rework loop, three opportunities  
**Format:** Metric / baseline / target / intervention / evidence  
**Source:** Stakeholder quotes (Liam, Aisha, Carlos, Reina), baseline metrics (4.2-day median, 18% drop-off, 1.8 compliance days), BPMN structured text (04_bpmn_validated.md)  
**Invented figures:** Marked [NEEDS SOURCE]

---

## Identified bottleneck

**Compliance queue routing latency** — the handoff gap between Swimlane 2 (Onboarding operations, Liam) and Swimlane 3 (Compliance, Aisha). Cases sit in a routing queue before any compliance analyst touches them. Liam estimates 0.6 of the 1.8 compliance-attributed days is this wait, not active review. Unverified against system logs; US-01 instrumentation produces the verified figure.

---

## Identified rework loop

**Gateway 1 → Customer RFI → Gateway 1.** Applications that fail the completeness check are returned to the applicant, who must supply missing information before the case re-enters the queue. Each cycle consumes dwell time in both the customer and onboarding operations swimlanes without advancing the case toward a compliance decision. Rework rate is currently unmeasured — requires Gateway 1 event log extraction from Tom in week 1.

---

## OPP-01 — Compliance queue routing latency

| Field | Content |
|---|---|
| **Metric** | Median compliance queue-to-touch time (time between case entering the compliance queue and first active review action by a compliance analyst) |
| **Baseline** | 0.6 days [NEEDS SOURCE: Liam estimate — unverified against system logs; US-01 instrumentation produces the verified figure] |
| **Target** | ≤ 0.1 days [NEEDS SOURCE: requires Tom's system capacity confirmation and Carlos's risk appetite sign-off before committing] |
| **Intervention** | Automated case assignment routing cases directly from the compliance intake queue to an available analyst, removing the manual handoff step currently performed by onboarding operations |
| **Evidence** | Liam: *"at least 0.6 of that is just queue wait, not actual review. Nobody wanted to pull the data"* — BPMN Swimlane 2 → Swimlane 3 handoff gap |

**Refined statement:**
Reduce median compliance queue-to-touch time from **0.6 days** [NEEDS SOURCE] to **≤ 0.1 days** [NEEDS SOURCE] by implementing automated case assignment from the compliance intake queue directly to an available compliance analyst — eliminating the manual routing step currently performed by onboarding operations — evidenced by Liam's direct statement that *"at least 0.6 of that is just queue wait, not actual review"* and the BPMN handoff gap between Swimlane 2 and Swimlane 3.

**Gate before scoping:** Aisha's compliance gate (US-02) must confirm that automated routing does not constitute a decisioning change under ECOA or Reg B before implementation is scoped.

**BRD priority:** High — directly addresses the largest reducible component of the 4.2-day cycle without touching mandated review steps.

---

## OPP-02 — Intake rework loop

| Field | Content |
|---|---|
| **Metric** | Percentage of submitted applications cycling through the completeness check (Gateway 1) more than once before routing to compliance |
| **Baseline** | [NEEDS SOURCE: Gateway 1 rework rate requires event log extraction — Tom, week 1] |
| **Target** | [NEEDS SOURCE: requires measured baseline first; directional goal is to halve the rework rate, but no number can be committed without the baseline] |
| **Intervention** | Pre-submission validation surfacing missing fields and required documents to the applicant at the point of intake, before the application is submitted to onboarding operations |
| **Evidence** | Gateway 1 rework loop identified in BPMN (04_bpmn_validated.md); Reina's 18% aggregate drop-off — US-05 joined dataset will confirm whether rework loop cycling is a drop-off driver |

**Refined statement:**
Reduce the proportion of applications cycling through the completeness check more than once from **[NEEDS SOURCE]** to **[NEEDS SOURCE]** by introducing a pre-submission validation step that surfaces missing fields or documents to the applicant at intake before the application is submitted — evidenced by the Gateway 1 rework loop identified in the BPMN and Reina's 18% aggregate drop-off figure, which the US-05 joined dataset will confirm or deny as partially rework-driven.

**Gate before scoping:** None required from compliance — pre-submission validation is a customer-facing quality check, not a decisioning change, and does not trigger ECOA or Reg B review. Tom must confirm which intake system is in scope (NS-B03).

**BRD priority:** Medium-High — cannot be fully sized until the rework rate baseline exists; however, low compliance risk means design can begin in parallel with data collection.

---

## OPP-03 — Total onboarding cycle time

| Field | Content |
|---|---|
| **Metric** | Median new-customer onboarding cycle time (end-to-end: application submission to account activation) |
| **Baseline** | 4.2 days (stated baseline metric) |
| **Target** | ≤ 2.5 days [NEEDS SOURCE: directional placeholder only — cannot be committed until US-01 time decomposition, US-02 compliance gate, and US-03 risk appetite sign-off are all complete] |
| **Intervention** | Sequential delivery of OPP-01 (automated queue routing, recovers ~0.6 days) and OPP-02 (intake pre-validation, rework-day reduction unconfirmed), while holding active compliance review time constant and at or above the regulatory floor confirmed by Aisha and counsel |
| **Evidence** | 4.2-day baseline (stated); Liam: *"at least 0.6 of that is just queue wait, not actual review"*; Carlos: *"speed is fine — speed without a measurement framework is a risk I won't carry"*; Aisha: *"I'm not against improving this"* |

**Refined statement:**
Reduce median new-customer onboarding cycle time from **4.2 days** to **≤ 2.5 days** [NEEDS SOURCE: directional placeholder] by sequentially eliminating compliance queue routing latency (OPP-01, ~0.6 days recoverable) and rework loop overhead (OPP-02, quantum unconfirmed) while holding active compliance review time constant — evidenced by the 4.2-day baseline, Liam's 0.6-day queue wait testimony, Carlos's confirmation that speed requires a measurement framework, and Aisha's confirmation that cycle time reduction is not opposed in principle.

**Gate before scoping:** OPP-01 and OPP-02 must both be underway; US-01 decomposition, US-02 compliance gate, and US-03 risk appetite sign-off are all required before the 2.5-day target can be stated as a commitment rather than a direction.

**BRD priority:** High — headline business case metric for Carlos and the steering committee. Present as directional, not committed, until gates are cleared.

---

## Traceability matrix

| Opportunity | Bottleneck / loop | User story | Theme | Gate required | [NEEDS SOURCE] items |
|---|---|---|---|---|---|
| OPP-01 | Bottleneck — queue routing latency | US-01, US-02 | Theme 1 | US-02 compliance gate (Aisha) | Baseline verified figure; target confirmed by Tom + Carlos |
| OPP-02 | Rework loop — Gateway 1 RFI cycle | US-05 | Theme 5 | None (not a decisioning change) | Baseline rework rate; target reduction; rework-drop-off causal link |
| OPP-03 | Both (composite) | US-01, US-02, US-03 | Themes 1, 3 | US-01 + US-02 + US-03 all required | 2.5-day target; rework-day recovery quantum |

---

## [NEEDS SOURCE] tracker — this document

| ID | Item | Who resolves | When |
|---|---|---|---|
| NS-O01 | OPP-01 baseline: verified queue-to-touch time from system logs | Tom | Week 1 (US-01 instrumentation) |
| NS-O02 | OPP-01 target: ≤ 0.1 days confirmed by Tom (capacity) and Carlos (risk appetite) | Tom / Carlos | Week 2 |
| NS-O03 | OPP-02 baseline: Gateway 1 rework rate from event log extraction | Tom | Week 1 |
| NS-O04 | OPP-02 target: reduction goal once baseline is measured | Liam / Tom | Week 2 |
| NS-O05 | OPP-02 causal link: rework loop as drop-off driver confirmed by US-05 joined dataset | Reina / Tom | Week 2 |
| NS-O06 | OPP-03 target: 2.5-day figure confirmed as commitment (requires US-01 + US-02 + US-03) | Liam / Aisha / Carlos | Week 3 |

---

*Generated: BRD-ready opportunities · pre-commitment · targets marked [NEEDS SOURCE] are directional placeholders*
*Presentation guidance: OPP-01 and OPP-03 are ready to present to Liam as directional statements. OPP-02 requires Tom's week-1 extract before it can be sized. Carlos should receive OPP-03 framing as a goal, not a commitment, until all three gates are cleared.*
