# 04 — BPMN: new-customer onboarding process (validated)
**Process name:** New-customer onboarding — payments fintech  
**Source material:** Discovery plan (01), thematic synthesis (02), user stories (03), stakeholder quotes (Liam, Aisha, Carlos, Tom, Reina)  
**Baseline facts in scope:** 4.2-day median cycle, 18% drop-off, 1.8 compliance-attributed days, 5 systems, 5 named actors  
**Validation:** 6-error checklist applied post-draft. All findings and corrections recorded in Section 2.

---

## Section 1 — BPMN structured text

---

### START EVENT
**Trigger:** Prospective customer submits onboarding application  
**Type:** Message (inbound — application received by onboarding system)  
**Note:** The submission channel (web, mobile, branch, API) is not specified in source material. Channel is [NEEDS SOURCE] and does not affect the process structure below.

---

### SWIMLANE 1 — Customer (applicant)

**Activities:**
1. Submits onboarding application (start trigger)
2. Receives status notification at each step [NEEDS SOURCE: notification mechanism and frequency not named by any stakeholder]
3. Responds to any request for additional information (e.g., document upload, clarification)
4. Either completes the process (proceeds to account activation) or abandons (exit event)

**Note — drop-off:** 18% of applicants exit during the process. The step at which they exit is not currently known — this is the core gap described by Reina and the subject of US-05. Drop-off is modelled as an exception path (see EP-02).

---

### SWIMLANE 2 — Liam / Onboarding operations

**Activities:**
1. Receives submitted application from intake system
2. Conducts initial application review (completeness check)
3. Routes application to compliance queue (KYC/AML review)
4. Monitors application status across five systems [NEEDS SOURCE: specific system names not provided]
5. Manages queue assignments and routing between systems
6. Co-ordinates exception handling for stalled or flagged applications
7. Receives compliance decision and routes to final activation step
8. Confirms account activation and notifies customer

**Note — queue wait:** Liam states that at least 0.6 of the 1.8 compliance days is queue wait time attributable to this swimlane's routing activities, not to Aisha's review activities. This is unverified against system logs and marked [NEEDS SOURCE: extraction of per-step timestamps].

---

### SWIMLANE 3 — Aisha / Compliance (KYC, AML, ECOA, Reg B)

**Activities:**
1. Receives routed application from onboarding operations queue
2. Conducts KYC identity verification review
3. Conducts AML screening
4. Applies ECOA / Reg B fair-lending check to decisioning
5. Records review decision (clear / refer / decline)
6. Where applicable: raises exception for manual escalation
7. Returns decision to onboarding operations

**Note — mandated vs discretionary time:** The 1.8 compliance days includes both Aisha's active review time (mandated) and the queue wait before she receives the case (discretionary). These are not currently separated in any system. Separation is the subject of US-01.

---

### SWIMLANE 4 — Tom / Engineering & systems

**Activities:**
1. Maintains event logging across all five onboarding systems
2. Provides system log extracts for measurement and reporting [NEEDS SOURCE: extract cadence and format]
3. Manages inter-system handoffs and routing logic
4. Resolves system-level failures (e.g., a source system going offline)
5. Supports data join between Reina's customer journey data and internal event logs (US-05)

**Note:** Tom is not a process actor in the live onboarding flow — he is an infrastructure and measurement owner. His swimlane represents system-layer activities that underpin the process but are not visible to the customer. This distinction matters for gateway design.

---

### SWIMLANE 5 — Reina / Customer experience

**Activities:**
1. Monitors aggregate drop-off rate at funnel level (current: 18%)
2. Captures step-level exit events from customer-facing systems
3. Flags step-level drop-off signals to onboarding operations
4. [NEEDS SOURCE: Reina's role in active case intervention is not confirmed — she may be a monitoring actor only, not a process actor]

**Note:** Reina's swimlane is primarily a measurement and escalation lane, not a decisioning lane. She does not currently have a formal handoff point in the live process — this is the gap described in Theme 5.

---

### SWIMLANE 6 — Carlos / SVP Risk & Internal Controls

**Activities:**
1. Sets risk appetite thresholds (pre-process, governance layer)
2. Reviews aggregate risk metrics (SAR rate, KYC false-negative rate) on a periodic basis [NEEDS SOURCE: review cadence not stated]
3. Escalation recipient for exceptions that breach risk appetite thresholds
4. Signs off on any process change that affects compliance controls (governance gate)

**Note:** Carlos is not a transactional actor in individual onboarding cases. He operates at the governance and escalation layer. His swimlane contains no per-case activities in the current process.

---

### GATEWAY 1 — Application completeness check
**Type:** XOR (exclusive — one outcome per case)  
**Actor:** Onboarding operations (Liam)  
**Position:** After initial application review, before compliance queue routing  

**Outcomes:**
- Application complete → route to compliance queue (proceed to Gateway 2)
- Application incomplete → return to customer with request for additional information (loop back to Customer swimlane, Activity 3)
- Application cannot be assessed (system unavailable) → exception path EP-03

**Rationale for XOR:** Only one outcome is possible per case at this point. A case is either complete, incomplete, or blocked by a system failure — not a combination.

---

### GATEWAY 2 — Compliance review decision
**Type:** XOR (exclusive — one outcome per case)  
**Actor:** Compliance (Aisha)  
**Position:** After KYC, AML, and ECOA/Reg B review is complete  

**Outcomes:**
- Clear → route to onboarding operations for account activation
- Refer (manual escalation required) → exception path EP-01
- Decline → exception path EP-02 (regulatory decline, distinct from customer drop-off)

**Rationale for XOR:** Compliance produces one decision per case. An AND gateway would be wrong here — the case does not proceed on multiple simultaneous paths. An OR gateway would imply partial clearance is possible, which is not supported by any stakeholder statement.

**Correction applied:** Original draft used OR. Corrected to XOR. See validation checklist, Error 2.

---

### GATEWAY 3 — Customer response to additional information request
**Type:** XOR (exclusive — customer either responds or does not)  
**Actor:** Customer (with timeout managed by onboarding operations)  
**Position:** After Gateway 1 incomplete outcome, following request for additional information  

**Outcomes:**
- Customer provides required information within [NEEDS SOURCE: response window not stated by any stakeholder] → return to Gateway 1 completeness check
- Customer does not respond within the response window → exception path EP-02 (customer abandonment)

**Rationale for XOR:** Customer either responds or they do not — these are mutually exclusive at the point of timeout.

---

### EXCEPTION PATH 1 — Compliance referral / manual escalation
**Name:** EP-01 Compliance manual escalation  
**Trigger:** Aisha's review produces a "refer" decision at Gateway 2 — case cannot be cleared by standard automated or first-line review  

**Activities:**
1. Aisha flags case for manual escalation
2. Case enters a separate escalation queue [NEEDS SOURCE: queue owner and SLA not named]
3. Senior compliance reviewer (or external specialist) conducts enhanced due diligence [NEEDS SOURCE: EDD process steps not described in source material]
4. Second decision is produced: clear with conditions, decline, or hold pending further information
5. Decision is returned to Gateway 2 logic for onward routing

**End:** Either rejoins main path (clear with conditions → activation) or terminates as EP-02 (decline)  
**Risk flag:** This path is where Aisha's ECOA and Reg B concerns are most acute. Any automated routing into this path requires the compliance gate from US-02 before implementation.  
**Invented steps flag:** Step 3 (EDD process) is [NEEDS SOURCE] — no stakeholder described the escalation review steps in detail.

---

### EXCEPTION PATH 2 — Customer abandonment or regulatory decline
**Name:** EP-02 Application exit (customer-initiated or compliance decline)  
**Trigger:** (a) Customer fails to respond to information request at Gateway 3 timeout, OR (b) Compliance issues a decline decision at Gateway 2, OR (c) Customer voluntarily exits at any point in the Customer swimlane  

**Activities:**
1. Exit event is recorded in customer-facing system (Reina's data)
2. Exit event is timestamped against the internal system state at the point of exit [NEEDS SOURCE: this join does not currently exist — it is the gap described in US-05 and Theme 5]
3. Case is closed in onboarding operations system
4. For compliance declines: adverse action notice is issued to customer per ECOA / Reg B requirements [NEEDS SOURCE: adverse action process steps not described in detail]
5. Case is logged for Reina's drop-off cohort analysis

**End:** Terminal — case closed. No reactivation path is described in source material [NEEDS SOURCE: whether declined or abandoned applicants can re-apply and at what interval].  
**Note:** The 18% drop-off baseline is the aggregate of all exit sub-types in this path. The split between voluntary abandonment, timeout, and compliance decline is [NEEDS SOURCE].

---

### EXCEPTION PATH 3 — Source system unavailable
**Name:** EP-03 System failure during onboarding  
**Trigger:** One or more of the five onboarding systems is offline or returns an error during any active step  

**Activities:**
1. System failure is detected (by Tom's infrastructure monitoring or by an onboarding operations staff member encountering an error)
2. Affected case(s) are flagged as system-blocked in the onboarding queue
3. Tom's team investigates and resolves system availability [NEEDS SOURCE: resolution SLA not stated]
4. Once system is restored, case resumes from the point of interruption [NEEDS SOURCE: resume logic — whether cases auto-resume or require manual re-trigger — not described by any stakeholder]
5. If resolution exceeds [NEEDS SOURCE: threshold not stated], case is escalated to Liam for customer communication

**End:** Either rejoins main path at the point of interruption, or — if the customer has abandoned during the system outage — becomes EP-02.  
**Note:** This path was identified as a gap during validation (Error 1 in the checklist below). No stakeholder explicitly described it, but US-01 AC-05 (system unavailable negative path) implies it exists. Marked as [NEEDS SOURCE] for all SLA and resume-logic details.

---

### END EVENT
**Outcome:** Customer successfully onboarded — account activated  
**Type:** Terminal (message — activation confirmation sent to customer)  
**Preconditions:** Gateway 2 returned "clear"; onboarding operations has completed activation in the relevant system(s); customer has been notified.

**Note:** The end event for exception paths EP-01 (if cleared) rejoins this terminal. EP-02 and EP-03 (if unresolved) have their own terminal states (application closed / system-blocked case).

---

## Section 2 — 6-error validation checklist

---

### Error 1 — Missing exception paths

| Sub-check | Found? | Detail |
|---|---|---|
| What if compliance declines? | YES — found and fixed | EP-02 covers regulatory decline. Initially modelled only as a sub-type of customer abandonment. Separated into explicit Gateway 2 outcome and distinct branch within EP-02. |
| What if customer abandons mid-process? | YES — found and fixed | EP-02 covers voluntary abandonment. Initially modelled as a single exit event without step-level attribution. Noted that the step-level data gap (US-05) means the trigger point within the process is [NEEDS SOURCE]. |
| What if a system goes down? | YES — found and fixed | EP-03 added during validation. Not described by any stakeholder directly, but implied by US-01 AC-05 and Tom's system maintenance role. All SLA and resume-logic details marked [NEEDS SOURCE]. |
| What if a customer fails to respond to an information request? | YES — found and fixed | Gateway 3 added to cover the timeout case explicitly. Response window is [NEEDS SOURCE]. |

**Correction made:** Added EP-03 (system unavailable) and Gateway 3 (customer non-response timeout). Neither was present in the initial draft.

---

### Error 2 — Wrong gateway type

| Gateway | Initial type | Correct type | Found? | Evidence |
|---|---|---|---|---|
| Gateway 1 — Completeness check | XOR | XOR | No error — correct | A case is either complete or not. Mutually exclusive. No stakeholder evidence contradicts this. |
| Gateway 2 — Compliance decision | OR | XOR | YES — found and fixed | Aisha's review produces one outcome per case (clear, refer, or decline). An OR gateway implies a case could be partially cleared — not supported. Quote: *"Show me the regulatory floor, in writing"* (Aisha) — implies a binary gate, not a partial clearance model. Corrected to XOR. |
| Gateway 3 — Customer response timeout | XOR | XOR | No error — correct | Customer either responds or does not within the window. Mutually exclusive. |

**Evidence (Aisha):**
> "I'm not against improving this. I'm against improving it in a way that creates a disparate-impact pattern we can't explain to an examiner."  
This confirms that compliance produces a single, explainable decision per case — incompatible with an OR gateway that would allow partial clearance.

**Correction made:** Gateway 2 changed from OR to XOR. Rationale documented in gateway entry.

---

### Error 3 — Skipped handoffs / missing swimlanes

| Actor check | Swimlane present? | Handoff documented? | Finding |
|---|---|---|---|
| Customer | YES | YES — submits application; receives status; responds to RFI; exits or completes | No error |
| Liam / Onboarding operations | YES | YES — receives from customer; routes to compliance; receives decision; activates | No error |
| Aisha / Compliance | YES | YES — receives from onboarding queue; returns decision to onboarding | No error |
| Tom / Engineering | YES | YES — system layer; log extraction; inter-system routing | No error |
| Reina / Customer experience | YES | Partial — Reina receives exit events but has no formal handoff in the live process. Flagged as gap, not error. | Gap documented, not invented |
| Carlos / Risk & Controls | YES | Governance layer only — no per-case handoff. Correct for his role. | No error |
| **Handoff gap identified:** | — | Reina → Liam: no formal signal from drop-off monitoring to onboarding operations exists in current process. This is the Theme 5 gap. | [NEEDS SOURCE: formal escalation path] |

**Finding:** No swimlane was missing. One handoff gap (Reina → Liam) was identified and documented as a known process gap, not invented as an activity.

**Correction made:** Added explicit note to Reina's swimlane that her role is monitoring-only in the current process and that no formal handoff to operations exists. This is a gap, not an error in the BPMN.

---

### Error 4 — Invented activities

| Activity reviewed | In source material? | Action |
|---|---|---|
| Customer submits application | YES — implied by all source material (start event) | Retained |
| Completeness check by onboarding operations | YES — implied by Liam's routing description | Retained |
| KYC identity verification | YES — named by Aisha and in baseline metrics | Retained |
| AML screening | YES — named by Aisha and Carlos | Retained |
| ECOA / Reg B fair-lending check | YES — explicitly named by Aisha | Retained |
| Queue routing between systems | YES — Liam: *"at least 0.6 of that is just queue wait"* | Retained |
| Account activation | YES — implied as the terminal outcome | Retained |
| Enhanced due diligence steps in EP-01 | NO | Marked [NEEDS SOURCE] — not described by any stakeholder |
| Adverse action notice in EP-02 | Partially — ECOA/Reg B implies legal requirement, but process steps not described | Marked [NEEDS SOURCE] |
| System resume logic in EP-03 | NO — not described by any stakeholder | Marked [NEEDS SOURCE] |
| Customer notification mechanism | NO — not described by any stakeholder | Marked [NEEDS SOURCE] |
| Re-application eligibility after decline | NO | Marked [NEEDS SOURCE] |

**Evidence (Liam):**
> "The 1.8 compliance days — I've been saying for a year that at least 0.6 of that is just queue wait, not actual review."  
This supports queue routing as a real activity. It does not support inventing the specific steps within the escalation review.

**Correction made:** Six activities marked [NEEDS SOURCE]. No invented activities remain as stated facts.

---

### Error 5 — Missing event triggers

| Event | Trigger defined? | Finding |
|---|---|---|
| Start event | YES — customer submits application (message event) | No error |
| EP-01 trigger | YES — "refer" outcome from Gateway 2 | No error |
| EP-02 trigger | YES — three sub-triggers defined (timeout, decline, voluntary exit) | No error — improved during validation |
| EP-03 trigger | YES — system offline or error returned | Added during validation |
| End event (main path) | YES — account activated, customer notified | No error |
| End event (EP-02) | YES — application closed (terminal) | No error |
| End event (EP-03 unresolved) | YES — system-blocked case, escalated to Liam | No error |

**Finding identified during validation:** EP-02 originally had a single trigger (customer abandonment). The regulatory decline trigger and the Gateway 3 timeout trigger were added during error-checking. The original draft's single trigger was an oversimplification that would have caused EP-02 to conflate three distinct exit reasons — making the 18% drop-off figure undiagnosable.

**Correction made:** EP-02 now has three distinct triggers. Gateway 3 added as the timeout mechanism.

---

### Error 6 — SLA / time annotations made up

| Annotation | Present in source? | Action |
|---|---|---|
| 4.2-day median cycle time | YES — stated as baseline metric | Retained as context; not attached to any individual activity as an SLA |
| 1.8 days compliance-attributed | YES — stated as baseline metric | Retained as context; noted that it is undifferentiated (Theme 1) |
| 0.6 days queue wait (Liam's estimate) | YES — Liam's direct statement | Retained; flagged as unverified against system logs |
| Response window for customer information request (Gateway 3) | NO | Marked [NEEDS SOURCE] — no stakeholder named a timeout value |
| Escalation queue SLA in EP-01 | NO | Marked [NEEDS SOURCE] |
| System resolution SLA in EP-03 | NO | Marked [NEEDS SOURCE] |
| Review cadence for Carlos's governance layer | NO | Marked [NEEDS SOURCE] |
| Re-application interval after decline | NO | Marked [NEEDS SOURCE] |

**Evidence (Carlos):**
> "My concern is that we're running a regulatory exam next spring and I don't want any material process changes in-flight that I can't explain with a clean paper trail."  
This references a time horizon (spring exam) but does not define any process SLA. Not used as a time annotation.

**Correction made:** All seven SLA/time values without a source are marked [NEEDS SOURCE]. No invented numbers appear as stated facts in the BPMN.

---

## Section 3 — Corrections summary

| # | Error type | Found in draft? | Correction applied |
|---|---|---|---|
| 1a | Missing exception path — system unavailable | YES | EP-03 added; all SLA details marked [NEEDS SOURCE] |
| 1b | Missing exception path — customer non-response to RFI | YES | Gateway 3 added with timeout logic; response window marked [NEEDS SOURCE] |
| 2 | Wrong gateway type — Gateway 2 OR → XOR | YES | Gateway 2 corrected to XOR; rationale documented with Aisha quote |
| 3 | Handoff gap — Reina → Liam | YES (gap, not error) | Documented as a known process gap; not invented as an activity |
| 4 | Invented activities — EDD steps, adverse action steps, resume logic | YES | Six activities marked [NEEDS SOURCE]; none stated as fact |
| 5 | Missing event trigger — EP-02 had single trigger | YES | EP-02 now has three distinct triggers (timeout, decline, voluntary exit) |
| 6 | SLA annotations without source | YES | Seven time/SLA values marked [NEEDS SOURCE]; none appear as stated facts |

**Total errors found and fixed: 7 (across 6 error categories)**  
**[NEEDS SOURCE] items in this document: 14**  
**Invented activities remaining: 0**

---

## Section 4 — [NEEDS SOURCE] consolidated tracker (this document)

| ID | Item | Where | Who can resolve | When |
|---|---|---|---|---|
| NS-B01 | Submission channel (web / mobile / branch / API) | Start event | Liam / Tom | Week 1 |
| NS-B02 | Customer notification mechanism and frequency | Customer swimlane | Liam / Reina | Week 1 |
| NS-B03 | Five system names and boundaries | All swimlanes | Tom | Week 1 |
| NS-B04 | System log extract cadence and format | Tom swimlane | Tom | Week 1 |
| NS-B05 | Reina's formal role in live process vs monitoring only | Reina swimlane | Reina | Week 1 |
| NS-B06 | Carlos's governance review cadence | Carlos swimlane | Carlos | Week 2 |
| NS-B07 | Escalation queue owner and SLA in EP-01 | EP-01 | Aisha / Liam | Week 2 |
| NS-B08 | Enhanced due diligence process steps in EP-01 | EP-01 | Aisha | Week 2 |
| NS-B09 | Adverse action notice process steps in EP-02 | EP-02 | Aisha / Legal | Week 2 |
| NS-B10 | Split of 18% drop-off between voluntary, timeout, and decline | EP-02 | Reina / Aisha | Week 1 |
| NS-B11 | Re-application eligibility interval after decline | EP-02 | Aisha / Legal | Week 2 |
| NS-B12 | System resolution SLA in EP-03 | EP-03 | Tom | Week 1 |
| NS-B13 | Case resume logic after system restoration | EP-03 | Tom | Week 1 |
| NS-B14 | Customer response window for RFI timeout (Gateway 3) | Gateway 3 | Liam / Aisha | Week 1 |

---

*Generated: BPMN structured text with validation · pre-implementation · not yet reviewed by process owner (Liam) or compliance (Aisha)*  
*Next step: Liam and Aisha to review Section 1 for accuracy; Tom to confirm system names and log extract feasibility; Section 4 tracker to be worked through in week-1 sessions.*
